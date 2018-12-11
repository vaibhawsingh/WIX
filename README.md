# WIX
This Documents will explains alomst all the WiX concepts from zero level to advanced level. Every Chapter in this tutorials explains the concepts in very easy way and also provide the coding.

-------------------------------------------------------------------------------------------------------------------------------
[1. Windows Installer XML](#1-windows-installer-xml)

[2. How To Install WiX](#2-how-to-install-wix)

[3. Creating Application using WiX](#3-creating-application-using-wix)

  - [3.1 Creating Shortcut on Desktop and Program Menu Folder](#3.1-creating-shortcut-on-desktop-and-program-menu-folder)
  - [3.2 Check for Prerequisite](#3.2-check-for-prerequisite)
  - [3.3 Version Upgradation](#3.3-version-upgradation)
  - [3.4 Changing Banner](#3.4-changing-banner)
  - [3.5 Adding License](#3.5-adding-license)
  - [3.6 Icon for Application](#3.6-icon-for-application)
  - [3.7 Changing Instalation Directory](#3.7-changing-instalation-directory)
  
[4. Extracting Files and folders using Heat](#4-extracting-files-and-folders-using-heat)

[5. Customizing Dialog in WIX](#5-customizing-dialog-in-wix)

  - [5.1 Customizing predefined dialog](#5.1-customizing-predefined-dialog)

[20. Custom Action in WIX](#20-custom-action-in-wix)
* [C++](#c++)

--------------------------------------------------------------------------------------------------------------------------------

<!-- toc -->
## 1. Windows Installer XML ##
  - Windows Installer XML also known as WiX, is used for creating Installer which generates .msi or .exe application. This can be used for creating installer which bundles all kinds of file.
  - Current Version of WiX  is **v3.11.1** released on **2017/12/31**
  - Its source files extension is .wxs and .wxl.
## 2. How To Install WiX ##
  - Download latest version of WiX from http://wixtoolset.org/ and install it to your PC. 
  - If you want to install an interactive WiX setup editor then go to TOOLS and open Extensions and Updates...
    * Search for **WAX** and then install it.
  - Restart the Visual Studio to patch with WiX.
  
  **Note:-** WAX can be only used if the project contains the WiX as one of the project in Solution Explorer. In my opinion WAX is not useful if you are making some complex application.
## 3. Creating Application using WiX ##
  - Create New Project by selecting WiX Toolset v3 Setup Project for **WiX v3** from the Templates with proper name and directory.
  - Once the project created open Product.wxs and add Manufacturer name to compile the project.
  - Add Directory structure for the application to install in **C Drive**.
  - Here **ProgramFilesFolder** denotes **Program Files (x86)**. If you want to change the folder to 64 bit then you need to use **ProgramFiles64Folder**. 
  
  **Note:-** To create 64 bit application you need to add "**Platform="x64"**" in Package Tag and all the Component tag should be added by "**Win64="yes"**".
  <pre><code>
  &lt;Fragment&gt;
	&lt;Directory Id="TARGETDIR" Name="SourceDir"&gt;
		&lt;Directory Id="ProgramFilesFolder"&gt;
			&lt;Directory Id="INSTALLFOLDER" Name="SetupProject" /&gt;
		&lt;/Directory&gt;
	&lt;/Directory&gt;
  &lt;/Fragment&gt;
  </code></pre>
  - If you want the cab file to be embeded inside the installer then add "**EmbedCab = "yes"**" in MediaTemplate Tag.
  <pre><code>
  &lt;MediaTemplate EmbedCab="yes"/&gt;
  </code></pre>
  - Feature tag should contain those components and files which needs to be installed as one feature. All feature needs to be inside the Product Tag.
  - You need to refer the WiX UI for dialog to be added by default to your application. This sholud be closed between Product Tag
  <pre><code>
  &lt;UIRef Id="Customize_Mondo"/&gt;
  </code></pre>
### 3.1 Creating Shortcut on Desktop and Program Menu Folder ###
  - To create the shortcut first you need to add the directry structure as below.
  <pre><code>
  &lt;Directory Id="TARGETDIR" Name="SourceDir"&gt;
    &lt;Directory Id="ProgramMenuFolder"&gt;
      &lt;Directory Id="MenuShortCut" Name="App"&gt; //App is the folder name which gets created in program menu 
      &lt;/Directory&gt;
      &lt;Directory Id="DesktopFolder" Name="Desktop" /&gt; //creates desktop folder for shortcut on desktop
    &lt;/Directory&gt;
  &lt;/Directory&gt;
  </code></pre>
   - Now you need to add the code for creating shortcut. Here we also need to add the **RemoveFolder** Tag for deleting the shortcut at the time of Uninstalling the application. Also don't forget to add this component reference in feature tag.
  you also need to add the registry value so that it can be located easily at the time of instalation and product can be checked at the time of instalation weather it is available or not.
  <pre><code>
  //Program menu shortcut
  &lt;Fragment&gt;
    &lt;DirectoryRef Id="MenuShortCut"&gt;
      &lt;Component Id="ProgramMenuShortcut" Guid="9bd13330-6540-406f-a3a8-d7f7c69ae7f9"&gt;
        //DirectoryName is the directory where the exe is available in C Drive
        &lt;Shortcut Id="ShortcutProgramMenu" Name="XYZ" Description="App" Target="[DirectoryName]App.exe" WorkingDirectory="DirectoryName" /&gt;
        &lt;RemoveFolder Id="RemoveProgramMenuFolder" Directory="ProgramMenuFolder" On="uninstall" /&gt;
        &lt;RemoveFolder Id="RemoveProgramMenuFolder3" Directory="MenuShortCut" On="uninstall" /&gt;
        &lt;RegistryValue Root="HKCU" Key="Software\Setup\App" Name="installed" Type="integer" Value="1" KeyPath="yes" /&gt;
      &lt/Component&gt;
    &lt/DirectoryRef&gt;
   &lt;/Fragment&gt;
   </code></pre>
### 3.2 Check for Prerequisite ###
   - To check for pre-requisite add the condition with RegistrySearch tag. For any kind of pre-requisite you need to check the registry entry.
   <pre><code>
   //registry search
   &lt;Property Id="CHECKVER10"&gt;
      &lt;RegistrySearch Id="VERSION_10_REG_SEARCH" Root="HKLM" Key="SOFTWARE\XYZ\Application" Name="Version" Type="raw" Win64="yes" /&gt;
    &lt;/Property&gt;
    &lt;Condition Message="Version 10 is Installed"&gt;&lt;![CDATA[Installed OR (CHECKVER10 &ge; "10.00")]]&gt;&lt;/Condition&gt;
    </code></pre>
   - If the condition is satisfied installer will pops up a window which shows the condition message which is available in Condition tag and the instalation gets aborted.
### 3.3 Version Upgradation ###
   - To create a new release of installer either a new version or the same, you need to change the Product ID.
   - Also you need to add **MajorUpgrade** elememt to your application.
   <pre><code>
   &lt;MajorUpgrade AllowSameVersionUpgrades="yes" DowngradeErrorMessage="A newer version of [ProductName] is already installed." /&gt;
   </code></pre>
   - you can use the same element for different types of upgradation and degradation.
### 3.4 Changing Banner ###
   - To change the banner you need to override the predefined/Default *WixVariable* **WixUIBannerBmp**
   <pre><code>
   &lt;WixVariable Id="WixUIBannerBmp"  Value="newBaner.bmp" /&gt;
   </code></pre>
### 3.5 Adding License ###
   - To change the License you need to override the predefined/Default *WixVariable* **WixUILicenseRtf**
   - License must be saved in *&#42;.rtf* extension and it should be edited in wordpad only not with the micrsoft word.
   <pre><code>
   &lt;WixVariable Id="WixUILicenseRtf" Value="..\..\License\License.rtf"/&gt;
   </code></pre>
### 3.6 Icon for Application ###
   - Icons need to be added so that it can be distinguished between different application.
   - To add icons in *program and features* you need to override the Property **ARPPRODUCTICON**. It is also called as app Icon.
   <pre><code>
   &lt;Property Id="ARPPRODUCTICON"&gt;Icon&lt/Property&gt;
   </code></pre>
   - To add icons for shortcut either on desktop or program menu you need to add **Icon** tag in **Shortcut** element, and provide the Icon value. 
   - Now you need to add/provide the value to the Icon.
   <pre><code>
   &lt;Icon Id="Icon" SourceFile="Abc.ico"/&gt;
   </code></pre>
### 3.7 Changing Instalation Directory ###
   - If you are providing an option to the user for selecting the directory for instalation, you need to override the Default Install Directory. you need to update the Property **WIXUI_INSTALLDIR** value with new directory.
   <pre><code>
   &lt;Property Id="WIXUI_INSTALLDIR" Value="APPLICATIONFOLDER"/&gt;
   </code></pre>
## 4. Extracting Files and folders using Heat ##
   - **Heat.exe** is used to extract the files from the folder structure.
   - If you have application which have dll and images which are structured under the folder and it should be installed in the same way, you can use **Heat.exe** for extracting those files.
   - Open Properties from the solution explorer and select Build Events.
   - Add the below command in **Pre-build Event Command Line**.
   <pre><code>
   "C:\Program Files (x86)\WiX Toolset v3.11\bin\heat.exe" dir "D:\Working\Heat\XYZ" -cg Component -gg -scom -sreg -sfrag -srd -out "D:\Working\XYZ_WixSetup\XYZ\FilesFragment.wxs"
   </code></pre>
   - Here the folder used for extracting the files is **D:\Working\Heat\XYZ**.
   - **Component** is used for putting the files under one component.
   - And **D:\Working\XYZ_WixSetup\XYZ\FilesFragment.wxs** is used for creating the file once the extraction is completed.
## 5. Customizing Dialog in WIX ##
   - When you are building application using Wix, you may need to change some buuton and may provide some custom button, Radio button, CheckBox or ComboBox.
   - These changes you can do either on the predefined dialog by customizing it or you can add your new customize dialog.
### 5.1 Customizing predefined dialog ###
   - For customizing dialog you need to download the source code of **WIX** and then the dialog which you want to customize that dialog file needs to be added to your application.
   - Since you are customizing the dialog you need to change the file name which you are adding into your application, and also you need to change the base file which contains the control of all the dialogs. 
   
   
   
  
  
## 20. Custom Action in WIX ##
* How to create Custom Action in C++ 

  Custom Action written in c++ is unmanaged code, which is best for custom actions. To create custom actions using Visual studio open File->New->Project. A Dialog will pop up with all the templates and types of application. Select Installed->Templates->WIX Toolset->v3 and C++ Custom Actions Project for WiX v3.
  Once the application gets created CustomAction.cpp and CustomAction.def file gets generated. Since the projects generates .dll file so you can not run the application directly instead you should only compile and build this application.

Note:- In solution Explorer You may see that Application shows as the (Visual Studio 2010) If you have visual studio version 2013 or above. If you compile this application you will get the error as "error MSB8020: The build tools for Visual Studio 2010 (Platform Toolset = 'v100') cannot be found". To resolve this issue open Solution Explorer, right click on the project and select to "Upgrade VC++ Compiler and Library". This upgrades your project to current version and now you will be able to compile and build the application.

Once you are able to build the application, now the time is for calling custom action from your WiX Application.

Now we will see how to call custom action from WiX appliaction with example to add and delete Items in ComboBox depending upon the user selection of checkBox. Complete Source code is available in Repository.

Here I am going to call two custom Action One to fill the ComboBox (FillingCombBox) and another is to delete the item from ComboBox(DletingFromComboBox). Function FillingComboBox() will get called when the user selects CheckBox and click on Next Button on that Dialog, While the function DletingFromComboBox() will get called when user click back button from the ComboBoxDlg. Here the ComboBoxDlg contains the ComboBox. 
