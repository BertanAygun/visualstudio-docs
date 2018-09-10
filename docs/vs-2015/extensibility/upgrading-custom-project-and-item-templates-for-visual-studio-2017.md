---
title: "Upgrading Custom Project and Item Templates for Visual Studio “15” | Microsoft Docs"
ms.custom: ""
ms.date: "2018-06-30"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: ad02477b-e101-4f32-aeb7-292bf95d5c2f
caps.latest.revision: 4
ms.author: "gregvanl"
manager: "ghogen"
---
# Upgrading Custom Project and Item Templates for Visual Studio “15”
[!INCLUDE[vs2017banner](../includes/vs2017banner.md)]

The latest version of this topic can be found at [Upgrading Custom Project and Item Templates for Visual Studio ](https://docs.microsoft.com/visualstudio/extensibility/upgrading-custom-project-and-item-templates-for-visual-studio-2017).  
  
Starting in Visual Studio "15" Preview 4, Visual Studio is changing the way it discovers project and item templates that have been installed by a .vsix or an .msi. If you own extensions that use custom project or item templates, you need to update your extensions. This topic explains what you must do.  
  
 This change affects only Visual Studio "15". It does not affect previous versions of Visual Studio.  
  
 If you want to create a project or item template as part of a VSIX extension, see [Creating Custom Project and Item Templates](../extensibility/creating-custom-project-and-item-templates.md).  
  
## Template Scanning  
 Previously, **devenv /setup** or **devenv /installvstemplates** scanned the local disk to find project and item templates. Starting in Preview 4, scanning will be performed only for the user-level location (**%USERPROFILE%\Documents\\<Visual Studio version\>\My Exported Templates\\**) that is used for templates generated by the **File / Export Templates** command.  
  
 For other (non-user) locations, you must include a manifest(.vstman) file that specifies the location and other characteristics of the template. The .vstman file is generated along with the .vstemplate file used for templates. If you install your extension using a .vsix, you can accomplish this by recompiling the extension in Visual Studio "15" Preview 2. But if you use an .msi, you need to make the changes manually. For a list of what you need to do to make these changes, see  **Upgrades for Extensions Installed with an .MSI** later in this topic.  
  
## How to Update a VSIX Extension with Project or Item Templates  
 This procedure explains how to have Visual Studio "15" Preview 2 update your existing VSIX extension.  
  
1.  Open the solution in Visual Studio "15" Preview 2. You will be asked to upgrade the code. Click **OK**.  
  
2.  After the upgrade completes, you may need to change the version of the install target. In the VSIX project, open the source.extension.vsixmanifest file and select the **Install Targets** tab. If the **Version Range** field is **[14.0]**, click **Edit** and change it to include Visual Studio "15". For example, you can set it to **[14.0,15.0]** to install the extension to either Visual Studio 2015 or Visual Studio "15", or to **[15.0]** to install it to just Visual Studio "15".  
  
3.  Recompile the code.  
  
4.  Close Visual Studio.  
  
5.  Install the VSIX.  
  
6.  You can test the update by doing the following:  
  
    1.  The file scanning change is activated by the following registry key:  
  
         **reg add hklm\software\microsoft\visualstudio\15.0\VSTemplate /v DisableTemplateScanning /t REG_DWORD /d 1 /reg:32**  
  
    2.  After you have added the key, run **devenv /installvstemplates**.  
  
    3.  Reopen Visual Studio. You should find your template in the expected location.  
  
    > [!NOTE]
    >  The Visual Studio Extensibility project templates are not available when the registry key is present. You must delete the registry key (and rerun **devenv /installvstemplates**) to use them.  
  
## Other Recommendations for Deploying Project and Item Templates  
  
-   Avoid using zipped template files. Zipped template files need to be uncompressed in order to retrieve resources and content, so they will be costlier to use. Instead, you should deploy project and item templates as individual files under their own directory to speed up template initialization. For VSIX extensions, SDK build tasks will automatically unzip any zipped template while creating the VSIX file.  
  
-   Avoid using package/resource ID entries for the template name, description, icon or preview in order to avoid unnecessary resource assembly loads during template discovery. Instead, you can use localized manifests to create a template entry for each locale, which uses localized names or properties.  
  
-   If you are including templates as file items, manifest generation might not give you the expected results. In that case, you will have to add a manually-generated manifest to the VSIX project.  
  
## File Changes in Project and Item Templates  
 We will show the points of difference between the Visual Studio 2015 version and the Visual Studio “15” version of the template files, so that you can create the new files correctly.  
  
 Here is the default project .vstemplate file created by Visual Studio 2015:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<VSTemplate Version="3.0.0" Type="Project" xmlns="http://schemas.microsoft.com/developer/vstemplate/2005" xmlns:sdk="http://schemas.microsoft.com/developer/vstemplate-sdkextension/2010">  
  <TemplateData>  
    <Name>ProjectTemplate1</Name>  
    <Description>ProjectTemplate1</Description>  
    <Icon>ProjectTemplate1.ico</Icon>  
    <ProjectType>CSharp</ProjectType>  
    <RequiredFrameworkVersion>2.0</RequiredFrameworkVersion>  
    <SortOrder>1000</SortOrder>  
    <TemplateID>05b79cc9-2146-4716-a8e5-7e085cdd2221</TemplateID>  
    <CreateNewFolder>true</CreateNewFolder>  
    <DefaultName>ProjectTemplate1</DefaultName>  
    <ProvideDefaultName>true</ProvideDefaultName>  
  </TemplateData>  
  <TemplateContent>  
    <Project File="ProjectTemplate.csproj" ReplaceParameters="true">  
      <ProjectItem ReplaceParameters="true" TargetFileName="Properties\AssemblyInfo.cs">AssemblyInfo.cs</ProjectItem>  
      <ProjectItem ReplaceParameters="true" OpenInEditor="true">Class1.cs</ProjectItem>  
    </Project>  
  </TemplateContent>  
</VSTemplate>  
  
```  
  
 Here is the .vstman file (you can find it in the VSIX project’s manifest directory) that resulted from the rebuilding of the VSIX project:  
  
```  
VSTemplateManifest Version="1.0" Locale="1033" xmlns="http://schemas.microsoft.com/developer/vstemplatemanifest/2015">  
  <VSTemplateContainer TemplateType="Project">  
    <RelativePathOnDisk>CSharp\1033\ProjectTemplate1</RelativePathOnDisk>  
    <TemplateFileName>ProjectTemplate1.vstemplate</TemplateFileName>  
    <VSTemplateHeader>  
      <TemplateData xmlns="http://schemas.microsoft.com/developer/vstemplate/2005">  
        <Name>ProjectTemplate1</Name>  
        <Description>ProjectTemplate1</Description>  
        <Icon>ProjectTemplate1.ico</Icon>  
        <ProjectType>CSharp</ProjectType>  
        <RequiredFrameworkVersion>2.0</RequiredFrameworkVersion>  
        <SortOrder>1000</SortOrder>  
        <TemplateID>05b79cc9-2146-4716-a8e5-7e085cdd2221</TemplateID>  
        <CreateNewFolder>true</CreateNewFolder>  
        <DefaultName>ProjectTemplate1</DefaultName>  
        <ProvideDefaultName>true</ProvideDefaultName>  
      </TemplateData>  
    </VSTemplateHeader>  
  </VSTemplateContainer>  
</VSTemplateManifest>  
  
```  
  
 The information provided by the [TemplateData](../extensibility/templatedata-element-visual-studio-templates.md) element remains the same. The **\<VSTemplateContainer>** element points to the .vstemplate file for the associated template.  
  
 Here is the default item .vstemplate file created by Visual Studio 2015:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<VSTemplate Version="3.0.0" Type="Item" xmlns="http://schemas.microsoft.com/developer/vstemplate/2005" xmlns:sdk="http://schemas.microsoft.com/developer/vstemplate-sdkextension/2010">  
  <TemplateData>  
    <Name>ItemTemplate1</Name>  
    <Description>ItemTemplate1</Description>  
    <Icon>ItemTemplate1.ico</Icon>  
    <TemplateID>bfeadf8e-a251-4109-b605-516b88e38c8d</TemplateID>  
    <ProjectType>CSharp</ProjectType>  
    <RequiredFrameworkVersion>2.0</RequiredFrameworkVersion>  
    <NumberOfParentCategoriesToRollUp>1</NumberOfParentCategoriesToRollUp>  
    <DefaultName>Class.cs</DefaultName>  
  </TemplateData>  
  <TemplateContent>  
    <References>  
      <Reference>  
        <Assembly>System</Assembly>  
      </Reference>  
    </References>  
    <ProjectItem ReplaceParameters="true">Class.cs</ProjectItem>  
  </TemplateContent>  
</VSTemplate>  
  
```  
  
 Here is the .vstman file (you can find it in the VSIX project’s manifest directory) that resulted from the rebuilding of the VSIX project:  
  
```  
VSTemplateManifest Version="1.0" Locale="1033" xmlns="http://schemas.microsoft.com/developer/vstemplatemanifest/2015">  
  <VSTemplateContainer TemplateType="Item">  
    <RelativePathOnDisk>CSharp\1033\ItemTemplate1</RelativePathOnDisk>  
    <TemplateFileName>ItemTemplate1.vstemplate</TemplateFileName>  
    <VSTemplateHeader>  
      <TemplateData xmlns="http://schemas.microsoft.com/developer/vstemplate/2005">  
        <Name>ItemTemplate1</Name>  
        <Description>ItemTemplate1</Description>  
        <Icon>ItemTemplate1.ico</Icon>  
        <TemplateID>bfeadf8e-a251-4109-b605-516b88e38c8d</TemplateID>  
        <ProjectType>CSharp</ProjectType>  
        <RequiredFrameworkVersion>2.0</RequiredFrameworkVersion>  
        <NumberOfParentCategoriesToRollUp>1</NumberOfParentCategoriesToRollUp>  
        <DefaultName>Class.cs</DefaultName>  
      </TemplateData>  
    </VSTemplateHeader>  
  </VSTemplateContainer>  
</VSTemplateManifest>  
  
```  
  
 The information provided by the **\<TemplateData>** element remains the same. The **\<VSTemplateContainer>** element points to the .vstemplate file for the associated template  
  
 For more information about the different elements of the .vstman file, see [Visual Studio Template Manifest Schema Reference](../extensibility/visual-studio-template-manifest-schema-reference.md).  
  
## Upgrades for Extensions Installed with an .MSI  
 Some MSI-based extensions deploy templates to common template locations such as the following:  
  
-   **\<Visual Studio installation directory>\Common7\IDE\\<ProjectTemplates/ItemTemplates>**  
  
-   **\<Visual Studio installation directory>\Common7\IDE\Extensions\\<ExtensionName\>\\<Project/ItemTemplates>**  
  
 If your extension performs an MSI-based deployment, you need to generate the template manifest manually and ensure that it is included in the extension setup. You should compare the .vstman examples listed above and the [Visual Studio Template Manifest Schema Reference](../extensibility/visual-studio-template-manifest-schema-reference.md). to see what you need to include  
  
 You should create separate manifests for project and item templates, and they should point to root template directory as specified above. You should create one manifest per extension and locale.  
  
## Troubleshooting Template Installation  
 If you run into problems deploying your project or item templates, you can enable diagnostic logging.  
  
1.  Run the following command to set the registry key to enable logging:  
  
     **reg add HKCU\software\microsoft\visualstudio\15.0_Config\VSTemplate /v EnableTemplateDiscoveryLog /t REG_DWORD /d 1**  
  
2.  Start Visual Studio and launch the New Project and New Item dialogs to initialize both template trees. The template log now appears in **%LOCALAPPDATA%\Microsoft\VisualStudio\15.0\VsTemplateDiagnosticsList.csv**. Each template tree initialization appends entries to this log.  
  
 The log file contains the following columns:  
  
-   **FullPathToTemplate**, which has the following values:  
  
    -   1 for manifest-based deployment  
  
    -   0 for disk-based deployment  
  
-   **TemplateFileName**  
  
-   Other template properties
