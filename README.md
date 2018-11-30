# WIX
[1. Window Installer XML](#1.windowinstallerxml)

[2. Custom Action in WIX](#2.customactioninwix)
* [C++](#c++)


<!-- toc -->
1. Window Installer XML

2. Custom Action in WIX
* How to create Custom Action in C++ 

  Custom Action written in c++ is unmanaged code, which is best for custom actions. To create custom actions using Visual studio open File->New->Project. A Dialog will pop up with all the templates and types of application. Select Installed->Templates->WIX Toolset->v3 and C++ Custom Actions Project for WiX v3.
  Once the application gets created CustomAction.cpp and CustomAction.def file gets generated. Since the projects generates .dll file so you can not run the application directly instead you should only compile and build this application.

Note:- In solution Explorer You may see that Application shows as the (Visual Studio 2010) If you have visual studio version 2013 or above. If you compile this application you will get the error as "error MSB8020: The build tools for Visual Studio 2010 (Platform Toolset = 'v100') cannot be found". To resolve this issue open Solution Explorer, right click on the project and select to "Upgrade VC++ Compiler and Library". This upgrades your project to current version and now you will be able to compile and build the application.

