# WIX
This Documents will explains alomst all the WiX concepts from zero level to advanced level. Every Chapter in this tutorials explains the concepts in very easy way and also provide the coding.

-------------------------------------------------------------------------------------------------------------------------------
[1. Windows Installer XML](#1-windows-installer-xml)

[2. How To Install WiX](#2-how-to-install-wix)

[3. Creating Application using WiX](#3-creating-application-using-wix)

  - [3.1 Creating Shortcut on Desktop and Program Menu Folder](#3.1-creating-shortcut-on-desktop-and-program-menu-folder)

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
### - 3.1 Creating Shortcut on Desktop and Program Menu Folder ###
  
  
  
  
  
## 20. Custom Action in WIX ##
* How to create Custom Action in C++ 

  Custom Action written in c++ is unmanaged code, which is best for custom actions. To create custom actions using Visual studio open File->New->Project. A Dialog will pop up with all the templates and types of application. Select Installed->Templates->WIX Toolset->v3 and C++ Custom Actions Project for WiX v3.
  Once the application gets created CustomAction.cpp and CustomAction.def file gets generated. Since the projects generates .dll file so you can not run the application directly instead you should only compile and build this application.

Note:- In solution Explorer You may see that Application shows as the (Visual Studio 2010) If you have visual studio version 2013 or above. If you compile this application you will get the error as "error MSB8020: The build tools for Visual Studio 2010 (Platform Toolset = 'v100') cannot be found". To resolve this issue open Solution Explorer, right click on the project and select to "Upgrade VC++ Compiler and Library". This upgrades your project to current version and now you will be able to compile and build the application.

Once you are able to build the application, now the time is for calling custom action from your WiX Application.

Now we will see how to call custom action from WiX appliaction with example to add and delete Items in ComboBox depending upon the user selection of checkBox. Complete Source code is available in Repository.

Here I am going to call two custom Action One to fill the ComboBox (FillingCombBox) and another is to delete the item from ComboBox(DletingFromComboBox). Function FillingComboBox() will get called when the user selects CheckBox and click on Next Button on that Dialog, While the function DletingFromComboBox() will get called when user click back button from the ComboBoxDlg. Here the ComboBoxDlg contains the ComboBox. 
