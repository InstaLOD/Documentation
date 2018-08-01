<img src="http://files.InstaLOD.io/Web/1280x720_InstaLOD3dsMaxSDK2Header.png" alt="image">

This page will help you to get up to speed with InstaLOD for Autodesk 3ds Max SDK 2.

[[MORE]]

## What is InstaLOD for Autodesk 3ds Max SDK 2

InstaLOD for Autodesk 3ds Max enables you to optimize 3D meshes without having to leave your favorite DCC tool. 
The integration features a graphical user interface, supports complex batch operations and can be scripted with its MAXScript scripting based API.
Great care has been taken to ensure a high degree of usability and productivity even when using InstaLOD for Autodesk 3ds Max for the first time.

> Please refer to the [InstaLODCmd SDK2 documentation](http://www.InstaLOD.io/GettingStartedWithCmd) for an overview of all features.
> 
> Teams using InstaLOD SDK 2 Update 2 have the option of getting a private instruction session via Skype video.
> Please [get in touch with us](http://www.InstaLOD.io/Contact) to schedule your session. 

<img src="http://files.InstaLOD.io/Web/1280x720_SDK2.png" alt="image">

## Table of Contents



- [Compatibility](#compatibility)
- [Prerequisites](#prerequisites)
- [Installing InstaLOD for 3ds Max](#installing-instalod-for-3ds-max)
- [Getting started with Autodesk 3ds Max](#getting-started-with-autodesk-3ds-max)
- [Optimizing a single mesh](#optimizing-a-single-mesh)
- [Optimizing multiple meshes](#optimizing-multiple-meshes)
- [Creating batch operations](#creating-batch-operations)
	- [Deleting batch profiles](#deleting-batch-profiles)
- [Exporting settings and batch profiles for InstaLODCmd](#exporting-settings-and-batch-profiles-for-instalodcmd)
- [Scripting InstaLOD for Autodesk 3ds Max via Python or MAXScript](#scripting-instalod-for-autodesk-3ds-max-via-python-or-maxscript)
	- [Pre- and post optimization callbacks](#pre--and-post-optimization-callbacks)
	- [Optimizing via script](#optimizing-via-script)
	- [Scripting with Python](#scripting-with-python)
- [Known Limitations](#known-limitations)
- [Website](#website)



<a name="compatibility" id="compatibility"></a>
## Compatibility

The following versions of Autodesk 3ds Max are supported: 2014, 2015, 2016 and 2017.

<a name="prerequisites" id="prerequisites"></a>
## Prerequisites

InstaLOD for Autodesk 3ds Max uses InstaLODCmd as backend for optimization operations. 
InstaLODCmd must be installed before you can start using InstaLOD for Autodesk 3ds Max.
Please refer to the [InstaLODCmd documentation](http://www.InstaLOD.io/GettingStartedWithCmd) on how to install InstaLODCmd on your workstation.

<a name="installing-instalod-for-3ds-max"></a>
## Installing InstaLOD for 3ds Max

Once you have installed and authorized InstaLODCmd on your workstation unzip the InstaLOD for 3ds Max file.

**Installation**

Copy the `InstaLOD_3dsMaxIntegration.ms` file into your 3ds Max startup scripts folder `\scripts\startup`, normally found within your 3ds Max user folder at `\Users\\AppData\Local\Autodesk\3dsMax\<3dsmax-version>\<3dsmax-language>`.
More information on how to install startup scripts can be found in the [Autodesk 3ds Max Documentation for Startup Scripts](https://knowledge.autodesk.com/support/3ds-max/learn-explore/caas/CloudHelp/cloudhelp/2015/ENU/3DSMax/files/GUID-C372E124-2B7E-41BB-94B0-5FF945D51095-htm.html).

<a name="getting-started-with-autodesk-3ds-max"></a>
## Getting started with Autodesk 3ds Max

Once the integration's script file is copied to your 3ds Max's scripts folder you can start 3ds Max. 
The script will automatically create the InstaLOD menu item, that can be used to spawn the InstaLOD settings window.

If the InstaLOD window has been closed, it can be spawned again by selecting `InstaLOD->Open Window...` from 3ds Max's main menu.

<img src="http://files.InstaLOD.io/Web/1280x720_3dsMaxSetup.png" alt="image">

On the first start of InstaLOD, it is necessary to point InstaLOD for 3ds Max to the installation directory of InstaLODCmd. 
Click the `Browse...`-button and browse to the InstaLODCmd installation directory on your workstation.

If InstaLODCmd was found in the specified directory the window contents will change.
If your machine has already been authorized for InstaLOD you can start using InstaLOD for Autodesk 3ds Max now.
If your machine has not been authorized for InstaLOD yet you can enter your license information in the dialog to authorize your workstation.

> Please refer to the [InstaLODCmd SDK2 documentation](http://www.InstaLOD.io/GettingStartedWithCmd) for an overview of all features.
> 
> Teams using InstaLOD SDK 2 Update 2 have the option of getting a private instruction session via Skype video.
> Please [get in touch with us](http://www.InstaLOD.io/Contact) to schedule your session. 

<a name="optimizing-a-single-mesh"></a>
## Optimizing a single mesh

To optimize a mesh select the mesh in the viewport or hierarchy. Select the `Optimize`-tab in the InstaLOD window and enter the percentage of triangles for the output mesh in the `Percent Triangles` text field.
Click the `Optimize Selected Meshes` button to optimize the mesh. 3ds Max will export the geometry for InstaLOD and start the optimization.
Once the geometry has been exported InstaLOD will execute the mesh operation asynchronously.

Both skeletal and static meshes are supported by InstaLOD for 3ds Max. 
<img src="http://files.InstaLOD.io/Web/1280x720_3dsMax.png" alt="image">
The image above shows the UI of InstaLOD for Autodesk 3ds Max docked to the right side of the window. The InstaLOD window can be docked and undocked like other native 3ds Max windows. 

<a name="optimizing-multiple-meshes"></a>
## Optimizing multiple meshes

InstaLOD SDK 2 introduces a new feature called 'Global Optimization'. 
Optimizing multiple meshes in a single operation with global optimization enabled allows the optimizer to consider all input
meshes when executing the operation. This results in the lowest visual deviation for the input meshes as a whole.
Global optimization is enabled by default, but can be disabled by unchecking the checkbox in the `Advanced Settings` found on the `Optimize`-tab.

Optimizing multiple meshes works identical to the optimization of a single mesh. 
Select all meshes to be optimized in the viewport or hierarchy, enter the optimization target value and click the `Optimize Selected Meshes` button to optimize all currently selected and visible meshes.

<img src="http://files.InstaLOD.io/Web/1280x720_GlobalOptimization.png" alt="image"><a name="creating-batch-operations"></a>
## Creating batch operations

By right-clicking on the mesh operation execution button and selecting `Save as Batch-Profile...` the current mesh operation settings will be saved as batch profile inside the directory of your InstaLODCmd installation. On the `Batch`-tab one saved profiles can be selected and executed on the current mesh selection in parallel. This is a great way to create a complete LOD chain with a single click.

<a name="deleting-batch-profiles"></a>
### Deleting batch profiles

To delete a saved batch profile open the `Batch`-tab. Right click on the `Execute Batch` button and select `Delete selected Batch Profiles` to delete all selected profiles.

<a name="exporting-settings-and-batch-profiles-for-instalodcmd"></a>
## Exporting settings and batch profiles for InstaLODCmd

To export a profile that can be used with InstaLODCmd without modification. Configure your mesh operation and right click the mesh operation execution button and select `Export as InstaLODCmd Profile...`.
Another great feature of InstaLOD for Autodesk 3ds Max is the ability to create and export multi operation batch profiles for InstaLODCmd.
To export a multi operation batch profile open then `Batch`-tab and select all saved batch profiles that will be included in the multi operation.
Right click the `Execute Batch` button and select `Export as InstaLODCmd Profile...` to export the profile for InstaLODCmd.

Profiles exported with InstaLOD for Autodesk 3ds Max have a hard-coded profile name of `3dsMax`. When queuing files with InstaLODCmd the profile needs to be specified:

    InstaLODCmd -profile ExportedWithMax.json -file Data/SM_Zetsuda_130k.fbx Build/SM_Zetsuda_Optimize.fbx 3dsMax 

<a name="scripting-instalod-for-autodesk-3ds-max-via-python-or-maxscript"></a>
## Scripting InstaLOD for Autodesk 3ds Max via Python or MAXScript
InstaLOD for Autodesk 3ds Max can be scripted using both Python and MAXScript. 
The integration's callback system enables developers to prepare texture and geometry data before submitting it to InstaLOD for optimization.
One such example would be custom materials that are flattened and converted to layered standard materials during the pre-optimization callback
and converted back into a custom material in the post-optimization callback.


<a name="pre--and-post-optimization-callbacks"></a>
### Pre- and post optimization callbacks
InstaLOD for Autodesk 3ds Max provides two functions that are invoked pre and post optimization for each object used in the operation.
To hook into these callbacks, simply implement the MAXScript functions listed in the table below in the global namespace.

| Callback Name                   | Argument 1            | Argument 2         |
|---------------------------------|-----------------------|--------------------|
| `InstaLOD_Event_WillExecute`    |(string) operation type|(node) object       |
| `InstaLOD_Event_DidExecute`     |(string) operation type|(node) object       |
|                                 |                       |                    |

> The `InstaLOD_Event_WillExecute` callback will be invoked during an undo chunk, so it is recommended to only perform operations
> on the object that fully support undo. 
> Once control returns back to 3ds Max the undo chunk will be rolled back and the object will be reverted to it's original state before running the optimization.

<a name="optimizing-via-script"></a>
### Optimizing via script
When using InstaLOD for 3ds Max via Script (MAXScript or Python) it is necessary to invoke the MAXScript function `InstaLOD_Initialize()` prior to using 
any of the optimization functionality.

InstaLOD for Autodesk 3ds Max saves all settings that are relevant to building optimization profiles in nested structures.
The naming convention for the settings is `INSTALOD_SETTINGS_INSTANCE.InstaLOD_[type]Settings.[field]` where `[type]` is the mesh operation type and field is a corresponding settings field e.g. `INSTALOD_SETTINGS_INSTANCE.InstaLOD_OptimizeSettings.PercentTriangles`. The `[field]` names are matching the names of variables defined in the InstaLOD C++ SDK.

`function InstaLOD_Initialize()`
Initializes the integration.

`function InstaLOD_IsCurrentInstaLODCmdPathValid()`
Determines if the current InstaLODCmdPath is valid.

`function InstaLOD_GetLicenseInfo()` 
Returns a string containing InstaLOD license information. This method requires the InstaLODCmd path to be setup.

`function InstaLOD_ResetSettings respawnUI:true` 
Resets all settings to default values. Set respawnUI to true to automatically recreate the InstaLOD user interface, true by default.

`function InstaLOD_OptimizeMesh meshObject optimizeType &outErrorLog externalProfilePath: allowAsync:true` 
Optimizes the specified `meshObject` using the specified `optimizeType`. `outErrorLog` is a string reference that will contain error information. 
Optionally `externalProfilePath` can be specified to load a json profile from the disk. If no `externalProfilePath` is specified, a profile will be built from the settings structs.

`function  InstaLOD_OptimizeMeshes objectArray optimizeType &outErrorLog externalProfilePath: allowAsync:true`
Optimizes the meshes specified in the `objectArray` using the specified `optimizeType`. `outErrorLog` is a string reference that will contain error information. 
Optionally `externalProfilePath` can be specified to load a json profile from the disk. If no `externalProfilePath` is specified, a profile will be built from the settings structs.

Before an optimization can be started via script. The InstaLOD settings structs need to be setup to match the desired operation. 
The following MAXScript example sets the mesh operation type to `Optimize` and optimizes a mesh with the name `Sphere` to 50% triangles.

	::InstaLOD_ResetSettings respawnUi:false;
	INSTALOD_SETTINGS_INSTANCE.InstaLOD_OptimizeSettings.PercentTriangles = 50.0;
	errorLog = "";
	meshObject = getNodeByName "Sphere";
	::InstaLOD_OptimizeMesh meshObject "Optimize" &errorLog;

<a name="scripting-with-python"></a>
### Scripting with Python
InstaLOD for Autodesk 3ds Max does not provide a native Python API. 
However, invoking the MAXScript API from Python works as a quality workaround.
The following Python-example sets the mesh operation type to `Optimize` and optimizes two meshes with the name `Sphere001` and `Sphere002` to 50% triangles.
  
	import MaxPlus
	import pymxs

	# querying a struct value as FPValue type
	currentPercentTriangles = MaxPlus.Core.EvalMAXScript("INSTALOD_SETTINGS_INSTANCE.InstaLOD_OptimizeSettings.PercentTriangles");
	print("Percent Triangle is set to: " + str(currentPercentTriangles.GetFloat()));

	# reset all settings to their defaults
	pymxs.runtime.execute("::InstaLOD_ResetSettings respawnUI:false;");

	# set Optimize settings
	pymxs.runtime.execute("INSTALOD_SETTINGS_INSTANCE.InstaLOD_OptimizeSettings.PercentTriangles = 50.0;");
	# create errorLog string
	pymxs.runtime.execute("errorLog = \"\";");
	# execute optimization on object (without async)
	# pymxs.runtime.execute("::InstaLOD_OptimizeMesh (getNodeByName \"Sphere001\") \"Optimize\" &errorLog allowAsync:false;");

	# execute optimization on multiple objects (without async)
	pymxs.runtime.execute("::InstaLOD_OptimizeMeshes #((getNodeByName \"Sphere001\"), (getNodeByName \"Sphere002\")) \"Optimize\" &errorLog allowAsync:false;");

<a name="known-limitations"></a>
## Known Limitations

There are currently no known limitations.

<a name="website" id="website"></a>
## Website

Please visit http://www.InstaLOD.io to stay up to date!

Thank you for using InstaLOD.
