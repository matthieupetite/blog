---
layout: post
comments: true
title: Helm, Kubernetes, and multi-level file systems
type: post
tags: [kubernetes, docker]
lang: en
image: /images//post/2020/03/2020-03-18-helm-filesystem-multin-niveau.png
---
In this article we will discuss setting up a multi-tier file system within a docker image in a Kubernetes context, with a kind deployment.

# Problem

Very often, when deploying an application under Kubernetes, it is advisable to ensure that the application configuration files are present on the image. Admittedly, it is not recommended to inject configuration by file system (we often prefer environment variables), but when we port an application under k8s it is unfortunately often a problem we are confronted with. Especially since the said files must evolve according to the deployment environments.

A simple approach would be to set up one config map per file, but the work can be long and tedious when you are in the case of several dozen files.

I therefore present here the solution that I have implemented in multiple projects to simplify my task.

# Prerequisites

Here is the list of prerequisites for this tutorial:

1. have a Kubernetes environment, we will use [k3d](https://k3d.io/) here
2. have knowledge of the HELM deployment solution
3. master the Kubernetes concepts of configuration map / deployment / ...

# Workaround

We're going to set up a mechanism for dynamically building a configuration map which we'll then mount into an image. The files presented in the map configuration will be text files in which it will be necessary to change the content according to the value defined in the values.yaml file of the chart.

# Definition of the expected

To illustrate our multilevel map configuration we will build the following filesystem in a container:

```bash
App
 ┣ subfolder
 ┃ ┗ file3.cfg
 ┣ file1.cfg
 ┗ file2.cfg
```

In the file1.cfg we will make sure to inject configuration data from the chart configuration file: the values.yaml file

# Tutorial step

## Installing k3d

After installing the k3d tool by following the [installation procedure](https://k3d.io), check the correct installation of the tool by running the following command:

```bash
k3d --version
```

You should get the following result:

```bash
k3d version v3.2.0
k3s version v1.18.9-k3s1 (default)
```

Create your first cluster by running the command

```bash
k3d cluster create mycluster
```

which gives you the following result:

![Creating k3d cluster](/blog/images/post/2020/03/2020-03-18-helm-filesystem-multin-level-1.png)

you should be able to see the list of pods in your brand new cluster by running the following command:

```bash
kubectl get pods --all-namespaces
```

which gives you this result:

![List of cluster pods](/blog/images/post/2020/03/2020-03-18-helm-filesystem-multin-level-2.png)

## HELM3 installation

The tool installation procedure is present here [https://helm.sh/docs/intro/install/](https://helm.sh/docs/intro/install/).

In my case I am on an Ubuntu machine so I simply run the following script as indicated in the documentation:

```bash
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

you can verify that helm3 is correctly installed by running the following command:

```bash
helm version
```

Which should return the following information to you:

```bash
version.BuildInfo{Version:"v3.5.1", GitCommit:"32c22239423b3b4ba6706d450bd044baffdcf9e6", GitTreeState:"clean", GoVersion:"go1.15.7"}
```

## Creating the chart from the template

Now let's move on to setting up the helm chart itself. Create a project folder, in which we will work.

```bash
mkdir ~/home/multilevel && cd ~/home/multilevel
```

et lancer la commande de création du chart Helm:

```bash
helm create multilevelcm
```

This command creates the chart skeleton in the `~/home/multilevel/` folder.

you get the following file structure:

```bash
📦multilevelcm
 ┣ 📂charts
 ┣ 📂templates
 ┃ ┣ 📂tests
 ┃ ┃ ┗ 📜test-connection.yaml
 ┃ ┣ 📜NOTES.txt
 ┃ ┣ 📜_helpers.tpl
 ┃ ┣ 📜deployment.yaml
 ┃ ┣ 📜hpa.yaml
 ┃ ┣ 📜ingress.yaml
 ┃ ┣ 📜service.yaml
 ┃ ┗ 📜serviceaccount.yaml
 ┣ 📜.helmignore
 ┣ 📜Chart.yaml
 ┗ 📜values.yaml
```

This is a basic chart that creates and deploys an nginx server.

For more information on chart anatomy the documentation is here [https://helm.sh/docs/helm/helm_create/](https://helm.sh/docs/helm/helm_create/).

## Creation of the dynamic map configuration

### creating the file structure in the chart

In the chart folder, create a files folder that contains our folder structure. You should get the following file structure:

```bash
📦multilevelcm
 ┣ 📂charts
 ┣ 📂files
 ┃ ┣ 📂subfolder
 ┃ ┃ ┗ 📜file3.cfg
 ┃ ┣ 📜file1.cfg
 ┃ ┗ 📜file2.cfg
 ┣ 📂templates
 ┃ ┣ 📂tests
 ┃ ┃ ┗ 📜test-connection.yaml
 ┃ ┣ 📜NOTES.txt
 ┃ ┣ 📜_helpers.tpl
 ┃ ┣ 📜deployment.yaml
 ┃ ┣ 📜hpa.yaml
 ┃ ┣ 📜ingress.yaml
 ┃ ┣ 📜service.yaml
 ┃ ┗ 📜serviceaccount.yaml
 ┣ 📜.helmignore
 ┣ 📜Chart.yaml
 ┗ 📜values.yaml
```

In the files `file2.cfg` and `file3.cfg` put the content you want. In the `file1.cfg`, position the following content:

```bash
{% raw %}
{{ .Values.configuration.file1 }}
{% endraw %}
```

This will allow the template engine to fetch the value from the chart file. For this at the end of the values.yaml file, add the following content:

```yaml
configuration:
  file1: |-
    contenu du fichier 1
```

### creating the configmap

In the Chart folder, create a configmap folder to house the definition file of our object.

in this file, position the following content:

```yaml
{% raw %}
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ include "multilevelcm.fullname" . }}-config
  labels:
    {{- include "multilevelcm.labels" . | nindent 4 }}
data:
  {{ $root := . }}
  {{- range $path, $bytes := $root.Files.Glob (printf "files/**") -}}
  {{ $name := base $path }}
  {{- sha256sum (printf "%s" (index (regexSplit "files/" ($path) -1 ) 1))}}{{ print ": |- " }}
    {{- tpl ($root.Files.Get ($path)) $ | nindent 4 }}
  {{end }}
{% endraw %}
```

the use of the sha256sum function makes it possible to avoid any special characters in the file name. The tpl function allows files to be considered as helm templates and therefore to inject data from the helm chart.

## mounting the map configuration in the pod

It remains to mount the configuration map as a volume in the container. To do this, edit the deployment file to add the following:

In the container part, add the module declaration:

```yaml
containers:
  - name: {{ .Chart.Name }}
      securityContext:
      {{- toYaml .Values.securityContext | nindent 12 }}
      image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart  AppVersion }}"
      imagePullPolicy: {{ .Values.image.pullPolicy }}
      ports:
      - name: http
        containerPort: 80
        protocol: TCP
      livenessProbe:
      httpGet:
        path: /
        port: http
      readinessProbe:
      httpGet:
        path: /
        port: http
      resources:
      {{- toYaml .Values.resources | nindent 12 }}
      volumeMounts:
      - name: config
        mountPath: "/app"
```

then at the end of the file add the volume declaration:

```yaml
{% raw %}
volumes:
- name: config
  configMap:
    name: {{ include "multilevelcm.fullname" . }}-config
    items:
    {{- range $path, $bytes := .Files.Glob (printf "files/**") -}}
    {{ $name := base $path }}
        - key: {{ sha256sum (printf "%s" (index (regexSplit "files/" ($path) -1) 1)) }}
        path: {{ printf "%s" (index (regexSplit "files/" ($path) -1) 1) }}
    {{- end}}
{% endraw %}
```

## Results

Our chart is ready, to deploy it on the k3d cluster. position yourself in your ~/multilevel folder and run the following command:

```bash
helm install -name myrelease -n myrelease --create-namespace ./multilevelcm/
```

With the tool [lens](https://k8slens.dev/) connect to your cluster, then enter your pod. By browsing in the /app folder, you find that all your files are there and that the content of your file1.cfg corresponds to what you put in the values ​​file.

![result](/blog/images/post/2020/03/2020-03-18-helm-filesystem-multin-level-3.png)

the chart code is available on my [github](https://github.com/matthieupetite/kubernetes-multilevel-configurationmap)