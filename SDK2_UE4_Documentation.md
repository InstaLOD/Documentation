<img src="http://files.InstaLOD.io/Web/1280x720_InstaLODUE4Header.png">

This page will help you to get up to speed with InstaLOD for Unreal Engine 4 in minutes.

[[MORE]]

## Table of Contents

<!-- MarkdownTOC depth=3 autoanchor=true autolink=true bracket='round' -->

- [What is InstaLOD for Unreal Engine 4](#what-is-instalod-for-unreal-engine-4)
- [Prerequisites](#prerequisites)
- [Installing InstaLOD for Unreal Engine 4](#installing-instalod-for-unreal-engine-4)
- [Updating from a previous version of InstaLOD for Unreal Engine 4](#updating-from-a-previous-version-of-instalod-for-unreal-engine-4)
- [Machine Authorization](#machine-authorization)
- [Machine Deauthorization](#machine-deauthorization)
- [Setting up Unreal Engine for InstaLOD](#setting-up-unreal-engine-for-instalod)
	- [Preparing Unreal Engine 4.14 and later for InstaLOD](#preparing-unreal-engine-414-and-later-for-instalod)
	- [Preparing Unreal Engine 4.9 - 4.13 for InstaLOD](#preparing-unreal-engine-49---413-for-instalod)
- [Using InstaLOD for Unreal Engine 4](#using-instalod-for-unreal-engine-4)
- [Vertex based optimization weights](#vertex-based-optimization-weights)
	- [Painting vertex colors in Unreal Engine 4](#painting-vertex-colors-in-unreal-engine-4)
- [Contributing to this document](#contributing-to-this-document)
- [Website](#website)

<!-- /MarkdownTOC -->

<a name="what-is-instalod-for-unreal-engine-4"></a>
## What is InstaLOD for Unreal Engine 4
InstaLOD for Unreal Engine 4 enables you to optimize 3D meshes and scenes from within Unreal Engine 4. 
Full support for both static and skeletal meshes as well as draw call reduction make InstaLOD the best choice for 
optimization in Unreal Engine 4.

<a name="prerequisites"></a>
## Prerequisites
InstaLOD for Unreal Engine currently supports all engine versions from 4.9 to the latest stable version on both Windows PC and MacOS.
InstaLOD has been built on Windows 10 using Visual Studio 2015. If you are using an earlier version of Windows, the installation of the [Visual Studio 2015 redistributables](https://www.microsoft.com/en-us/download/details.aspx?id=48145) is required.

> If you are not using C++ in your UE4 project you will have to create an empty C++ class inside UE4 first.
> To create an empty C++ class inside UE4 open your project and click the green `Add New` button inside the `Content Browser` and select `New C++ Class`. Next UE4 will prompt you to select a parent class, select the `None` or `Empty` to proceed. 
> InstaLOD for Unreal Engine ships with source code so this step is required in order for UE4 to compile and use the InstaLOD plugin.

<a name="installing-instalod-for-unreal-engine-4"></a>
## Installing InstaLOD for Unreal Engine 4
To get started copy the `InstaLODMeshReduction` folder to your project's `Plugins` folder.
If you cannot find the `Plugins` folder in the root of your Unreal Engine project simply create it using either Windows Explorer or MacOS Finder.
Finally, right click on your project file `projectname.uproject` and depending on your operating system select either `Generate Xcode project` (MacOS) or `Generate Visual Studio project files` (Windows).
The next time you will open your UE4 project the InstaLOD plugin will be compiled.

> If you are not using a custom fork of Unreal Engine 4, it is recommended to install InstaLOD to your project's plugins folder, 
> not into the engine's plugins folder.

### Installing InstaLOD for Unreal Engine 4.17
Due to a bug in Unreal Engine 4.17 a prompt appears when starting the engine for the first time after installing InstaLOD for Unreal Engine.
The prompt asks the user to disable the plugin due to incompatibilities, this is not correct and the dialog is the result of a bug within the engine. To proceed, simply click 'No' when asked to disable the `InstaLODMeshReduction` plugin.
More information on bug [UE-49007](https://issues.unrealengine.com/issue/UE-49007) is available [here](https://issues.unrealengine.com/issue/UE-49007).

<a name="updating-from-a-previous-version-of-instalod-for-unreal-engine-4"></a>
## Updating from a previous version of InstaLOD for Unreal Engine 4
Before installing the latest version of InstaLOD for Unreal Engine, delete the `InstaLODMeshReduction` folder from your project's `Plugins` folder. 
Install the latest version of InstaLOD for Unreal Engine as described in the previous chapter.

<a name="machine-authorization"></a>
## Machine Authorization
Your workstation needs to be authorized before InstaLOD for Unreal Engine can be used. If you are starting a project with InstaLOD installed but you have not authorized your machine a dialog will appear asking you to authorize this workstation. Enter your license information and press `Authorize` to request a license for your workstation. Once a license has been installed Unreal Engine needs to be restarted. Your workstation is now authorized for use with InstaLOD. Make sure to deauthorize your workstation before uninstalling InstaLOD or you will not be able to authorize another workstation.

> If you proceed without authorizing your machine. All optimization operations will result in the generation
> of a model that represents a key with the InstaLOD logo.
> To replace these keys with the proper optimized geometry after authorizing your machine,
> you can rebuild the mesh manually - or delete the `Intermediate` folder to force Unreal Engine to
> rebuild all LOD models.

<a name="machine-deauthorization"></a>
## Machine Deauthorization
To deauthorize your workstation before uninstalling InstaLOD for Unreal Engine, open the InstaLOD help dialog by clicking the InstaLOD icon in the toolbar of either the static mesh editor or the skeletal mesh editor (Persona).
A window will appear that contains information about InstaLOD, scroll down to the bottom and click the `DEAUTHORIZE MACHINE AND REMOVE LICENSE` link. The authorization dialog will appear asking you to enter your license information. Press `Deauthorize` to remove deauthorize your workstation. Unreal Engine will shut down after the workstation has been deauthorized. You can now uninstall InstaLOD for Unreal Engine.

<img src="http://files.InstaLOD.io/Web/instalod_ue4_deauthorize.png">

<a name="setting-up-unreal-engine-for-instalod"></a>
## Setting up Unreal Engine for InstaLOD
Before InstaLOD can be fully used with Unreal Engine 4 version specific configuration needs to be setup.

<a name="preparing-unreal-engine-414-and-later-for-instalod"></a>
### Preparing Unreal Engine 4.14 and later for InstaLOD
Users of Unreal Engine 4.14 and later must manually select the `InstaLODMeshReduction` plugin to be used for optimization.
Open the Main Window of Unreal Engine 4 and click the `Settings` button in the main toolbar.
Select `Project Settings...` to open the project specific settings. 
In the left side of the project settings dialog, scroll down until the `Editor` category becomes fully visible and select `Mesh Simplification`.

<img src="http://files.InstaLOD.io/Web/1000x584_ue4_plugin_setup.png">

<a name="preparing-unreal-engine-49---413-for-instalod"></a>
### Preparing Unreal Engine 4.9 - 4.13 for InstaLOD
Users of Unreal Engine 4.13 and earlier versions must manually enable the `Merge Actor` feature. 
Open the Unreal Engine 4 Editor preferences by clicking the `File` menu and selecting `Editor Preferences`. 
The `Merge Actor` feature can be enabled under the `Experimental`-Settings in the `General` category.

<a name="using-instalod-for-unreal-engine-4"></a>
## Using InstaLOD for Unreal Engine 4
InstaLOD is fully integrated into Unreal Engine 4 and all LOD features are now available to you.
This includes hierarchical LOD clusters, remeshing and the generation of static mesh and skeletal mesh LOD.
To perform remeshing outside of the HLOD feature, select the `Merge Actor` from the `Developer Tools` menu.

To get more information on how to use Unreal Engine's user interface to create LOD, please refer to the UE4 online documentation available at: http://www.InstaLOD.io/ue4documentation.

<img src="http://files.InstaLOD.io/Web/instalod_ue4_staticmesh_lod.png" >


<a name="vertex-based-optimization-weights"></a>
## Vertex based optimization weights
InstaLOD supports vertex color based optimization weights. This is a great way to give artists full control over the optimization.

  - Vertex Pinning, use the blue vertex color channel to mark vertices as important. Values between [0...1] will increase the weight of the vertex. A value of 1 marks the vertex as pinned and therefore not as collapsible.
  - Vertex Cullable Detail, use the red vertex color channel to mark vertices as cullable detail. Values between [0...1] will decrease the weight of the vertex. A value of 0 means the vertex will be culled as early as possible.

InstaLOD will only accept vertex color channels that have an empty green channel.
Avoid using both RED and BLUE for a single vertex. If both RED and BLUE colors are used on the same vertex the high color value will determine the weighting type. It is recommended to use pinning instead of culling, to avoid interfering with the optimization strategy of the optimizer.
To enable vertex based optimization weights, set `Silhouette` to `High` or enable the `InstaLOD.ForceOptimizerWeights` cvar.

> InstaLOD normally supports up to four vertex color channels and a dedicated mesh attribute for optimization weights.
> If you're running your project on a custom engine fork and you need to store other data in the vertex color channel, 
> you can make use of the additional fields provided by InstaLOD by modifing the `FRawMesh` conversion methods found in the `FInstaLOD` class.  

<a name="painting-vertex-colors-in-unreal-engine-4"></a>
### Painting vertex colors in Unreal Engine 4
Vertex colors are typically painted onto vertices during the model's creation in a DCC tool like Autodesk Maya.
However, vertex colors can also be painted directly on a mesh from inside Unreal Engine 4.
To paint vertex colors from inside UE4 drag a model into the viewport of your level.
Switch into paint mode by pressing `Shift+F2` and paint your desired vertex colors.
Once you are finished painting press the `Copy instance colors to source mesh` button to commit the vertex colors to your mesh.
Open the mesh in the static mesh editor, verify that the vertex colors have been updated using the `Vert Colors` view mode and press `Save.

<a name="contributing-to-this-document"></a>
## Contributing to this document
The InstaLOD documentation efforts are open-sourced and we welcome third-party contributions. 
Feel free to contribute to our documentation by submitting pull-requests to our [documentation repository on GitHub](https://github.com/InstaLOD/Documentation).

<a name="website"></a>
## Website

Please visit http://www.InstaLOD.io to stay up to date!

Thank you for using InstaLOD.
