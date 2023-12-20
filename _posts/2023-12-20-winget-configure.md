---
categories: [winget]
tags: [winget, devenv]
title: Using winget configure to setup dev environment
---

After working a lot of different projects, my Windows 11 dev environment eventually gets a mess. I was looking for ways to speed up the process of setting up a new dev environment and stumbled upon the Winget configure functionality. Which proved to be a big help!


## What is WinGet?
The WinGet command line tool enables users to discover, install, upgrade, remove and configure applications on Windows 10 and Windows 11 computers.

WinGet has been around for some time but the WinGet configure option has only recently been introduced. WinGet configure can be used to automatically handle the setup and configuration requirements for an ideal development environment on your Windows machine. In the background it's just desired state configuration (DSC).

Applying a WinGet configuration file helps with installing and managing software packages, applications, programming languages, frameworks, tools, or settings necessary for a project.

Let's try this one out!

## Checking prerequisites
To run the WinGet configure command you need at least Windows 10 RS5 or Windows 11, and WinGet version 1.6.2631 or later.

Run the following Powershell command to verify your current WinGet version:

```Powershell
PS >winget -v
v1.6.3133
```

If your version of Winget is too low, refer to the [documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/) for instructions on how to update your Winget version.

## Winget configuration file
When running Winget configure, you need to specify a configuration file. This file needs to written in YAML-formatting. The convention for naming a WinGet Configuration file is "configuration.dsc.yaml"

### Configuration file components
The configuration file consists of multiple components which need (or should) be present in the file.

#### Schema 

Schema: The first line in your configuration file should contain the following comment:

```YAML
# yaml-language-server: $schema=https://aka.ms/configuration-dsc-schema/<most recent schema version #>
```

The most recent schema number at the time of writing this pose is 0.2.

#### Properties
Properties: The root node for a configuration file is “properties” which must contain a configuration version (configurationVersion: 0.2.0 in this example). In addition, the properties node should contain an assertions node and a resources node.

#### Assertions
The assertions contain the preconditions which must be met befor applying the configutation. In this example, a minimum OS version is required.

#### Resources
The resources node containts the individual resources which need to be conifugured. The resource should be given the name of the PowerShell module followed by the name of the module's DSC resource that will be invoked to apply your desired state: {ModuleName}/{DscResource}. When applying a configuration, WinGet will know to install the module from the PowerShell Gallery and invoke the specified DSC resource.

#### Directives
The directives section provides information about the module and the resource. This section should include a description value to describe the configuration task being accomplished by the module. The allowPrerelease value enables you to choose whether or not the configuration will be allowed (true) to use "Prerelease" modules from the PowerShell Gallery.

#### Settings
The settings value of a resource represents the collection of name-value pairs being passed to the PowerShell DSC Resource. Settings could represent anything from whether Developer Mode is enabled, to applying a reg key, or to establishing a particular network setting.

#### Dependencies
The dependsOn value of a resource determines whether any other assertion or resource must be complete prior to beginning this task. If the dependency failed, this resource will also automatically fail.

#### Identifier
A unique identifier for the particular resource instance. The id value can be used if another resource has a dependency on this resource being applied first.

### Example configuration file
In the example configuration file below (configuration.dsc.yaml), after checking the minimum OS version, the following software gets installed. 

- Azure CLI
- Powershell 7
- Visual Studio Code
- Visual Studio Code - Bicep Extension
- Visual Studio Code - PowerShell Extension
- Visual Studio Code - Azure Account Extension
- Visual Studio Code - Python extenson
- Git

```yaml
# yaml-language-server: $schema=https://aka.ms/configuration-dsc-schema/0.2
properties:
  assertions:
    - resource: Microsoft.Windows.Developer/OsVersion
      directives:
        description: Verify min OS version requirement
        allowPrerelease: true
      settings:
        MinVersion: '10.0.22000'
  resources:
    - resource: Microsoft.WinGet.DSC/WinGetPackage
      directives:
        module: Microsoft.WinGet.DSC
        description: 'Install Microsoft Visual Studio Code'
        allowPrerelease: true
      settings:
        id: Microsoft.VisualStudioCode
        source: winget
    - resource: Microsoft.WinGet.DSC/WingetPackage
      directives:
        module: Microsoft.WinGet.DSC
        description: 'Install Git'
        allowPrerelease: true
      settings:
        id: Git.Git
        source: winget
    - resource: Microsoft.WinGet.DSC/WingetPackage
      directives:
        module: Microsoft.WinGet.DSC
        description: 'Install Powershell 7*'
        allowPrerelease: true
      settings:
        id: Microsoft.Powershell
        source: winget
    - resource: PSDscResources/Script
      id: Install VScode Extensions
      directives:
        description: Script to install Powershell extensions
        allowPrerelease: true
      settings:
        GetScript: |
          # Not using this at the moment.
        TestScript: |
          return $false
        SetScript: |
          # Ignore deprecation warnings & reload path
          $env:NODE_OPTIONS="--no-deprecation"
          $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User") 
          # Extensions to install
          code --install-extension ms-vscode.azure-account
          code --install-extension ms-vscode.PowerShell
          code --install-extension ms-azuretools.vscode-bicep
          code --install-extension ms-python.python
  configurationVersion: 0.2.0 
```

## Running winget configure
Now we just need a single command to setup our environment:

```powershell
winget configure configuration.dsc.yaml
```

After verifying the results, hit 'Y' and enter to continue.

```Powershell
PS > winget configure .\configuration.dsc.yaml
Assert :: OsVersion
  Verify min OS version requirement
  Module: Microsoft.Windows.Developer by Microsoft Corporation [Local]
    DSC Resource for Windows
  Settings:
    MinVersion: 10.0.22000
Apply :: WinGetPackage
  Install Microsoft Visual Studio Code
  Module: Microsoft.WinGet.DSC
  Settings:
    id: Microsoft.VisualStudioCode
    source: winget
Apply :: PowershellModule
  Install Powershell module
  Module: PSModulesDsc
  Settings:
    Module_Name: az
Apply :: WingetPackage
  Install Git
  Module: Microsoft.WinGet.DSC
  Settings:
    id: Git.Git
    source: winget
Apply :: vscodeextension
  Install VSCode Bicep Extension
  Module: vscode by Ravikanth Chaganti [Local]
    https://github.com/rchaganti/DSCResources/tree/master/vscode
    DSC resources for installing and managing Microsoft Visual Studio Code editor
  Settings:
    Ensure: Present
    Name: ms-azuretools.vscode-bicep
Apply :: vscodeextension
  Install VSCode PowerShell Extension
  Module: vscode by Ravikanth Chaganti [Local]
    https://github.com/rchaganti/DSCResources/tree/master/vscode
    DSC resources for installing and managing Microsoft Visual Studio Code editor
  Settings:
    Ensure: Present
    Name: ms-vscode.PowerShell
Apply :: vscodeextension
  Install VSCode Azure Account Extension
  Module: vscode by Ravikanth Chaganti [Local]
    https://github.com/rchaganti/DSCResources/tree/master/vscode
    DSC resources for installing and managing Microsoft Visual Studio Code editor
  Settings:
    Ensure: Present
    Name: ms-vscode.azure-account
You are responsible for understanding the configuration settings you are choosing to execute. Microsoft is not responsible for the configuration file you have authored or imported. This configuration may change settings in Windows, install software, change software settings (including security settings), and accept user agreements to third-party packages and services on your behalf.  By running this configuration file, you acknowledge that you understand and agree to these resources and settings. Any applications installed are licensed to you by their owners. Microsoft is not responsible for, nor does it grant any licenses to, third-party packages or services.
Have you reviewed the configuration and would you like to proceed applying it to the system?
[Y] Yes  [N] No:
```

Changes to your system are now being made. If all goes well, the output should look like this:

```Powershell
Assert :: OsVersion
  Configuration successfully applied.
Apply :: WinGetPackage
  Configuration successfully applied.
Apply :: PowershellModule
  Configuration successfully applied.
Apply :: WingetPackage
  Configuration successfully applied.
Apply :: vscodeextension
  Configuration successfully applied.
Apply :: vscodeextension
  Configuration successfully applied.
Apply :: vscodeextension
  Configuration successfully applied.
  Apply :: vscodeextension
  Configuration successfully applied.
```

## references
- [Winget documentation](https://learn.microsoft.com/en-us/windows/package-manager/configuration/)
- [Github repository](https://github.com/microsoft/winget-cli)
- [Powershell Gallery DSC packages](https://www.powershellgallery.com/packages)

