---
layout: post
comments: true
title: Powershell DSC Presentation
type: post
tags: [powershell, devOps]
lang: en
image: /images/post/2019/01/presentation-powershell-dsc.png
---
There you go, I **fell in love**, as our French-speaking friends across the Atlantic say... And when you "fall in love" well, you just want to talk about it.

My love of the day is called **DSC**, for **Desired State Configuration**.

I will present in this article, how DSC will save your life and I hope to make you want to take the plunge by showing a practical example of implementation.

This article is the first in a series that I hope will introduce you to:

- How to make your first DSC configuration
- The operation of DSC modules and resources
- The creation of your first custom modules, because yes, everything has not yet been done on GitHub
- Testing your configurations
- How to distribute your configurations on your infrastructure
- Supervise the correct application of your configurations

# Desired State Configuration, quick overview

Over the past few years, we have seen an acceleration in our small IT world.
We must deliver faster, quickly put applications online and therefore platforms in the hands of users to quickly get their feedback.

This acceleration is a real challenge for developers who no longer have the time to test, validate, and even specify.
**Agility**, the practice of **testing**, and automation were the response to these new constraints.

If in the field of application development, the question is old, the technical answers relatively mature, and the tools present with tools of the [Application Lifecycle Management](https://azure.microsoft.com/fr-fr /services/devops/), in the area of ​​infrastructure, answers were a little slower in coming.

It is with the emergence of cloud platforms such as [Azure](https://azure.microsoft.com) or [Amazon](https://aws.amazon.com) that things have become possible.
And with **Desired State Configuration**, Microsoft gives us the tool that allows us to meet these challenges at the OS level.

Deploying infrastructure in a testable, automated, predictable way while finally offering a solution to the OPS nightmare: "configuration drift", this is DSC's promise in the face of **Dev/OPS** challenges .

The objective of **DSC** is therefore to allow you to describe your infrastructures using a **scripting syntax** that you can apply to your servers periodically to ensure that what is actually deployed is what you have registered with the repository.

And since the description of a configuration is code, you have the possibility of:

- **Manage** your infrastructure in a source code repository and thus make the link with the applications,
- **Test** your infrastructure,
- **Automate** deployments of configuration changes
- **Audit** the correct application of the configurations

# Linux, Windows: DSC is available

Incidentally, the use of DSC is not limited to the Windows platform alone.
Indeed, it is possible to use this technology on a wide range of [Linux distribution](https://docs.microsoft.com/fr-fr/powershell/dsc/getting-started/lnxgettingstarted).

# General operation

As mentioned above, a DSC configuration is above all a powershell script (extension .ps1) in which we find the keyword "Configuration".
This file relies on resources that we can import in order to manipulate the state of the target server.

A configuration can describe the desired state of a set of servers. This approach makes it possible to pool configurations, which is very useful when deploying clusters or for "**mastering**" a large volume of servers.

The ```ps1``` configuration file is not directly used by the DSC agent deployed on the server that applies the DSC configurations. These should be "**compiled**" in order to produce one Mof file per server.

The production scheme of a DSC configuration is therefore as follows:

![presentation-powershell-dsc-03](/blog/images/post/2019/01/presentation-powershell-dsc-03.png)

Once the configuration has been compiled, the server's **local configuration manager** will be able to apply the ```MOF``` file resulting from the compilation of our configuration.

Any Windows server since version 2012 runs the LCM agent, and this is the strength of DSC technology since no addition is necessary to interpret our configurations.
This functionality is native, which frees us from any additional configuration.

# Presentation of the structure of a configuration

Now let's go into a little more detail about what our DSC configuration is.
As mentioned above, a configuration is a simple powershell file.

```powershell
Configuration MaPremiereConfiguration {

  Import-Module -ModuleName xTimeZone

  Node MonPremierServeur {
    xTimeZone 'FrenchTimeZone' {
      IsSingleInstance = 'Yes'
      TimeZone = 'Romance Standard Time'
    }
  }

}
```

Let's see in detail, on this small example, the objects that are manipulated.

| items                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Configuration                                               | As you can see, a DSC configuration always starts with the Powershell language keyword **Configuration**. This keyword is, underlying the language, a powershell function that can be called to produce the Mof files .                                                                                                                                                                                                                                                                                            |
| MaPremiereConfiguration                                     | Give the name of the DSC configuration                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Import-Module -ModuleName xTimeZone | This powershell directive is used to load the DSC resources on which our configuration depends. A DSC resource is a powershell module that must be installed on the server on which we want to apply our configuration. It is in these modules that we will find all the logic for installing and configuring the server that we wish to apply. The community provides a [large panel of open source modules](https://github.com/PowerShell/DscResources) that will allow you to configure your servers. |
| Node MyFirstServer | Here we declare the server for which the configuration must be applied. This directive should be read as follows: if the server has a hostname equal to "MyFirstConfiguration" then the configuration will be applied. Otherwise, it will simply be ignored. This approach therefore allows you to create a single configuration for a set of servers and apply or not apply DSC resources according to your needs. |
| xTimeZone "FrenchTimeZone" | This configuration line specifies that in our configuration we are using the **xTimeZone** resource which, as its name suggests, allows us to configure the time location of our server in order to manage the time properly. If you want to see the code for this resource, just refer to the module content we declared above. |
| IsSingleInstance = "Yes" TimeZone = "Romance Standard Time" | Finally, these last two elements constitute the parameters necessary for the DSC xTimeZone module                                                                                                                                                                                                                                                                                                                                                                                                                                           |

#How to apply a DSC configuration

The call kinematics of a DSC configuration is as follows. For each resource present in the configuration, the local configuration manager will call three powershell methods in sequence:

1. Get-TargetResource
2. Test-TargetResource
3. Set-TargetResource

As you will understand, all DSC resources must implement these functions so that they can be applied by the local configuration manager.

## Get-TargetResource

This first method is used to retrieve the component that we want to configure.
Thus the xTimeZone module retrieves the current configuration of the TimeZone of the server on which we want to apply our configuration.

## Test-TargetResource

This method tests the state of the current server against the expected configuration.
Two cases can then arise:

1. If the current state of the server conforms to that requested in the configuration, the Test-TargetResource function then returns the value ```$true```
2. If the current state is not in the desired state, then it returns ```$false```.

Depending on the return of this method, the Local Configuration Manager executes or not the ```Set-TargetResource``` function.

## Set-TargetResource

This last method actually performs the configuration of the server. In our case, it configures the server's TimeZone to "**Romance Standard Time**".

At the end of the cycle, the server is therefore configured as described in the repository.

# And now let's talk about deploying configurations

Now that you've mastered the overall logic of a DSC setup, let's get down to business.

## La compilation

First, you need to create and **compile** the DSC configuration. To do this, we will start from the following example:

```powershell

  Configuration ConfigurationTest {
    node Localhost {
      File DossierExemple {
        Ensure = 'Present'
        Type = 'Directory'
        DestinationPath = 'C:\temp\test'
      }
    }
  }

```
Copy the contents of this file into your favorite Powershell editor and save it in the ```c:\temp``` folder as ```ConfigurationTest.ps1```

![presentation-powershell-dsc-04](/blog/images/post/2019/01/presentation-powershell-dsc-04.png)

Then, you have to "**dotsourciez**" the file in order to load the DSC configuration you just created in your powershell session.

![presentation-powershell-dsc-05](/blog/images/post/2019/01/presentation-powershell-dsc-05.png)

You now have in your Shell the possibility to call the ```ConfigurationTest``` function to compile your configuration.

![presentation-powershell-dsc-06](/blog/images/post/2019/01/presentation-powershell-dsc-06.png)

And that's it, the configuration is now ready to be applied by the local configuration manager.
As you can see, the configuration action created a subfolder in the ```c:\temp``` directory named ConfigurationTest and containing the Mof file, the result of the configuration.

![presentation-powershell-dsc-07](/blog/images/post/2019/01/presentation-powershell-dsc-07.png)

## The deployment

Now, let's move on to the actual deployment of our configuration on a server.
There are two ways of applying configurations:

1. Push Mode
2. Sweater Mode

### Push mode

As part of deployment in **Push** mode, it is the system administrator who locally applies the configuration on the server.
This is the simplest approach.
Each time the configuration changes, the system administrator must manually reapply the configuration.

The command used to perform this operation is:

```powershell
$spats = @{
  ComputerName = '<nom du serveur cible>'
  Path         = '<Chemin du dossier contenant le fichier Mof'
  Wait         = $true
  Force        = $true
  Verbose      = $true
}
Start-DscConfiguration @spats
```

The application of the "**ConfigurationTest**" that we created above is therefore triggered as follows:

![presentation-powershell-dsc-08](//dev-portfolio-blogassets/img/post/2019/01/presentation-powershell-dsc-08.png)

As shown in the image above, which presents the traces of the actions of the local configuration manager, we can observe that the latter first called the **Test** method of the **File** resource.

The latter checked the presence on the disk of a folder named ```c:\temp\test```.
The latter did not exist, therefore the Set directive was called to create the folder.

On the second call to the DSC configuration, we can see that the Set method was "**Skip**"

![presentation-powershell-dsc-09](/blog/images/post/2019/01/presentation-powershell-dsc-09.png)

### Sweater mode

If the Push mode offers a first level of response, when using DSC on an infrastructure comprising a large number of servers you will quickly be confronted with certain **limits**:

- Applying configurations in push mode requires an **administrator action** to apply new configuration versions
- You must **deploy by hand** the DSC resource modules that your configuration needs on all the targeted servers
- You do not have access to reporting on the status of the application of your configurations in a centralized way.

Pull mode is for you.
Indeed in this configuration mode, the local configuration manager of your servers points to a central configuration server named **pull server**.
On a configurable time base, it interrogates the latter to retrieve the latest DSC configuration version assigned to it and possibly download the necessary modules before the configuration is applied.

![presentation-powershell-dsc-10](/blog/images/post/2019/01/presentation-powershell-dsc-10.png)

We will see in a future article how to implement a server pull within your infrastructure.