<img src="http://files.InstaLOD.io/Web/1280x720_InstaLODUnityHeader.png" width="100%">

This page will help you to get up to speed with InstaLOD for Unity in minutes.

[[MORE]]

## Table of Contents

<!-- MarkdownTOC depth=3 autoanchor=true autolink=true bracket='round' -->

- [What is InstaLOD for Unity](#what-is-instalod-for-unity)
- [Prerequisites](#prerequisites)
- [Installing InstaLOD for Unity](#installing-instalod-for-unity)
- [Updating from a previous version of InstaLOD for Unity](#updating-from-a-previous-version-of-instalod-for-unity)
- [Machine Authorization](#machine-authorization)
- [Machine Deauthorization](#machine-deauthorization)
- [Using InstaLOD for Unity](#using-instalod-for-unity)
	- [Create a game object LOD chain](#create-a-game-object-lod-chain)
	- [Using the InstaLOD Toolkit](#using-the-instalod-toolkit)
	- [Vertex based optimization weights](#vertex-based-optimization-weights)
	- [InstaLOD Data Cache](#instalod-data-cache)
- [Contributing to this document](#contributing-to-this-document)
- [Website](#website)

<!-- /MarkdownTOC -->

<a name="what-is-instalod-for-unity"></a>
## What is InstaLOD for Unity
InstaLOD for Unity enables you to optimize 3D meshes and scenes from within Unity. 
Full support for both static and skeletal meshes optimizations as well as complex mesh operations make InstaLOD the ultimate solution for 3d optimization in Unity.

<a name="prerequisites"></a>
## Prerequisites
InstaLOD for Unity currently supports all engine versions from 2017 to the latest stable version on both Windows PC and MacOS.
InstaLOD has been built on Windows 10 using Visual Studio 2015. If you are using an earlier version of Windows, the installation of the [Visual Studio 2015 redistributables](https://www.microsoft.com/en-us/download/details.aspx?id=48145) is required.

<a name="installing-instalod-for-unity"></a>
## Installing InstaLOD for Unity
To get started, copy the `InstaLOD` folder to your project's `Assets` folder.
The next time you will open your Unity project the InstaLOD plugin will be available.

<a name="updating-from-a-previous-version-of-instalod-for-unity"></a>
## Updating from a previous version of InstaLOD for Unity
Before installing the latest version of InstaLOD for Unity, delete the `InstaLOD` folder from your project's `Assets` folder. 
Install the latest version of InstaLOD for Unity as described in the previous chapter.

<a name="machine-authorization"></a>
## Machine Authorization
Your workstation needs to be authorized before InstaLOD for Unity can be used. 
Spawn either the `InstaLOD` or the `InstaLOD Toolkit` window by selecting it from the `Window` menu.
Enter your license information and press `Authorize` to request a license for your workstation. Once a license has been installed InstaLOD for Unity is fully operational. Your workstation is now authorized for use with InstaLOD. Make sure to deauthorize your workstation before uninstalling InstaLOD or you will not be able to authorize another workstation.

<a name="machine-deauthorization"></a>
## Machine Deauthorization
It is important to deauthorize your machine before uninstalling InstaLOD for Unity or your seat/node will remain locked.
To deauthorize your workstation, spawn either the `InstaLOD` or the `InstaLOD Toolkit` window by selecting it from the `Window` menu. 
Click on the `?` tab and select the `Deauthorize` to display the deauthorization UI elements.
Enter your license information and press `Deauthorize Workstation` to deauthorize your workstation.

> Deauthorization takes 24 hours to complete. Until the deauthorization cycle is fully complete the seat/node will remain locked and cannot be used to activate another machine.

<a name="using-instalod-for-unity"></a>
## Using InstaLOD for Unity
InstaLOD is fully integrated into Unity and all features are now available to you.
All operations run offline and asynchronous inside Unity, so you can keep on working on your scene while your data is being processed.

<a name="create-a-game-object-lod-chain"></a>
### Create a game object LOD chain 
InstaLOD supports automatic LOD generation for both static and skinned meshes. 
To create a LOD chain for your game object, select the game object in the viewport or hierarchy and spawn the `InstaLOD` window by selecting it from the `Window` menu.
If the selected game object has either a `Mesh Renderer` or a `Skinned Mesh Renderer` assigned the InstaLOD window will update to prompt you to create a `LOD Group` component for the select game object. Once the selected game object has a `LOD Group` component assigned, the InstaLOD window will update to display the LOD setup of the game object. By default, a Unity LOD group contains three entries and only the first entry is properly setup with the base mesh.
Press `Create missing LODGroup entry` and InstaLOD will automatically create the missing mesh for the selected LOD group entry.
By default, newly created LOD group entries are setup to use 50% triangles of the previous LOD group entry.
Therefore, pressing the `Create missing LODGroup entry` again to create `LOD2` will create another optimization at 25% triangles in relation to the base mesh. 
To create additional LOD group entries or modify when individual LOD group entry transition occurs, simply modify the `LOD Group` assign to the game object. 
Right-clicking on the `LOD Group` component UI allows you to insert or delete entries, dragging the separator allows you to change when the transition occurs.
All per game object InstaLOD settings will be persisted in the `InstaLODSettingsComponent` that is automatically assigned to your game object. 

> By default, InstaLOD will perform a polygon optimization when creating new LOD chain entries. 
> However, InstaLOD for Unity also supports the creation of imposters and remeshed geometry as LOD chain entry by changing the `Mesh Operation` value.

<a name="using-the-instalod-toolkit"></a>
### Using the InstaLOD Toolkit
The InstaLOD Toolkit enables Unity developers to perform complex mesh operations inside Unity that are normally only available in DCC tools.
Easily remesh large scenes or 3d scanned data, create high-quality imposters, merge all your scene materials onto a single texture or remove hidden faces from your geometry. Nearly the entire suite of InstaLOD's powerful mesh operations is available, right inside Unity.
To get started, spawn the `InstaLOD Toolkit` window by selecting it from the `Window` menu.
Select the mesh operation you want to perform and select all meshes that you want to process in the viewport or the hierarchy.
All selected materials and the number of meshes will be displayed in the toolbox.
Configure the settings for the mesh operation and press the confirmation button at the bottom of the window to start the mesh operation.

> Please refer to the [InstaLODCmd SDK2 documentation](http://www.InstaLOD.io/GettingStartedWithCmd) for an overview of all features.
> Teams using InstaLOD SDK 2 have the option of getting a private introduction session via Skype video. Please [get in touch with us](http://www.instalod.io/Contact) to schedule your session.

<a name="vertex-based-optimization-weights"></a>
### Vertex based optimization weights
InstaLOD supports vertex color based optimization weights when performing polygon optimizations. 
This is a great way to give artists full control over the optimization.

  - Vertex Pinning, use the blue vertex color channel to mark vertices as important. Values between [0...1] will increase the weight of the vertex. A value of 1 marks the vertex as pinned and therefore not as collapsible.
  - Vertex Cullable Detail, use the red vertex color channel to mark vertices as cullable detail. Values between [0...1] will decrease the weight of the vertex. A value of 0 means the vertex will be culled as early as possible.

InstaLOD will only accept vertex color channels that have an empty green channel.
Avoid using both RED and BLUE for a single vertex. If both RED and BLUE colors are used on the same vertex the high color value will determine the weighting type. It is recommended to use pinning instead of culling, to avoid interfering with the optimization strategy of the optimizer.
To enable vertex based optimization weights, check the `Vertex Colors As Weights` checkbox under the `Advanced` settings of the `Optimize` operation.

<a name="instalod-data-cache"></a>
### InstaLOD Data Cache
InstaLOD automatically creates a folder called `InstaLOD Data` inside your projects `Assets folder`. 
This folder contains all mesh, material and texture data that was created by InstaLOD.
Persisting this data to the disk is an important step when creating data for Unity, in order to facilitate the creation of prefabs that contains data generated by InstaLOD. It is recommended to purge unused data from the `InstaLOD Data` folder from time to time.

<a name="contributing-to-this-document"></a>
## Contributing to this document
The InstaLOD documentation efforts are open-sourced and we welcome third-party contributions. 
Feel free to contribute to our documentation by submitting pull-requests to our [documentation repository on GitHub](https://github.com/InstaLOD/Documentation).

<a name="website"></a>
## Website

Please visit http://www.InstaLOD.io to stay up to date!

Thank you for using InstaLOD.
