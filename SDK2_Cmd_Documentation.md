
<img src="http://files.InstaLOD.io/Web/1280x720_InstaLODCmdSDK2Header.png" width="100%">

This page will help you to get up to speed with InstaLODCmd SDK2 in just a few minutes.

[[MORE]]

## What is InstaLODCmd SDK 2
InstaLODCmd is a cross-platform console application that enables you to perform mesh operations on a batch of 3D models or scenes without having to use a graphical user interface. Complex batch processes can be set up within minutes by specifying optimization profiles and batches using JSON.

> InstaLODCmd also forms the optimization backend for InstaLOD for Autodesk Maya and InstaLOD for Autodesk 3ds Max.

<img src="http://files.InstaLOD.io/Web/1280x720_SDK2.png" width="100%">

## Table of Contents
<!-- MarkdownTOC depth=3 autoanchor=true autolink=true bracket='round' -->

- [Getting Started](#getting-started)
- [Prerequisites](#prerequisites)
- [License](#license)
  - [Machine Authorization](#machine-authorization)
  - [Machine Deauthorization](#machine-deauthorization)
  - [Offline Authorization \(Enterprise\)](#offline-authorization-enterprise)
- [Profiles and Batches](#profiles-and-batches)
- [Polygon Optimization](#polygon-optimization)
  - [Optimizing without a Profile](#optimizing-without-a-profile)
- [Remeshing](#remeshing)
  - ["Game-Ready" Remeshing](#game-ready-remeshing)
- [Imposters](#imposters)
  - [Hybrid Billboard Cloud Imposters](#hybrid-billboard-cloud-imposters)
    - [Rendering Hybrid Billboard Cloud Imposters](#rendering-hybrid-billboard-cloud-imposters)
- [Merging Mesh Materials](#merging-mesh-materials)
- [Occlusion Culling](#occlusion-culling)
  - [Automatic Interior Culling](#automatic-interior-culling)
  - [Camera Based Occlusion Culling](#camera-based-occlusion-culling)
  - [Visibility Based Mesh Optimization](#visibility-based-mesh-optimization)
- [UV operations](#uv-operations)
  - [UV Unwrap](#uv-unwrap)
  - [UV Repack](#uv-repack)
- [Baking](#baking)
  - [Targeting Meshes By Name](#targeting-meshes-by-name)
  - [Internally Generated Texture Pages](#internally-generated-texture-pages)
  - [Baking Vertex Colors](#baking-vertex-colors)
  - [Your First Bake with InstaLODCmd](#your-first-bake-with-instalodcmd)
  - [Baking Cages](#baking-cages)
  - [Advanced Baking](#advanced-baking)
- [Mesh Operation Chains](#mesh-operation-chains)
- [Materials](#materials)
  - [Automatic Texture Page Configurations](#automatic-texture-page-configurations)
  - [Custom Texture Page Configurations](#custom-texture-page-configurations)
    - [Example 1: Color maps](#example-1-color-maps)
    - [Example 2: Alpha masks](#example-2-alpha-masks)
    - [Example 3: Input normal maps](#example-3-input-normal-maps)
    - [Example 4: Customizing internal textures](#example-4-customizing-internal-textures)
  - [Rendering Normal Maps](#rendering-normal-maps)
- [Format strings with InstaLOD Script variables](#format-strings-with-instalod-script-variables)
- [InstaLOD Script](#instalod-script)
- [Program Arguments](#program-arguments)
- [Available import/export file formats](#available-importexport-file-formats)
- [Adding custom file format support via plug-ins](#adding-custom-file-format-support-via-plug-ins)
- [Website](#website)

<!-- /MarkdownTOC -->
  
<a name="getting-started"></a>
## Getting Started
To get started quickly with InstaLODCmd it is recommended to inspect the sample files that are included in the InstaLODCmd distribution.

> InstaLODCmd works by executing *mesh operations* as specified by JSON *profiles* on input files
> queued either via JSON *batch files* or by manually queuing them by specifying file or
> folder input via *program arguments*.
> Multiple profiles and batches can be loaded and executed in parallel with a single invocation
> of InstaLODCmd.

<a name="prerequisites"></a>
## Prerequisites

InstaLODCmd for Windows has been compiled using Visual Studio 2015 Update 3 on Windows 10.
If you are not using the latest version of Windows the installation of the [Visual C++ Redistributables for Visual Studio 2015 Service Pack 3](http://bit.ly/2nDdBwU) is required.
If you have already installed Visual Studio 2015 or later on your workstation, the installation of the
redistributables is not necessary. InstaLODCmd for Windows is compatible with Windows XP Service Pack 3 and later.
IntaLODCmd for MacOS requires MacOS Sierra 10.12 or later.

<a name="license"></a>
## License

<a name="machine-authorization"></a>
### Machine Authorization

Before InstaLODCmd can be used your workstation needs to be authorized. To authorize your workstation `cd` into the InstaLODCmd directory and execute the following command and replace the placeholders with your actual license information:

	InstaLODCmd -authorize <username> <password>

Your workstation is now authorized for use with InstaLOD. Make sure to deauthorize your workstation before uninstalling InstaLOD or you will not be able to authorize another workstation.

> Machine authorization for InstaLOD is system wide. If you've already been using InstaLOD on your computer,
> there is no need to authorize your machine for InstaLODCmd.

<a name="machine-deauthorization"></a>
### Machine Deauthorization

To deauthorize your workstation prior to uninstalling InstaLODCmd, use the `-deauthorize` argument:

	InstaLODCmd -deauthorize <username> <password>

> Deauthorization takes 24 hours to complete. Until the deauthorization cycle is fully complete the seat/node will remain locked and cannot be 
> used to activate another machine.

<a name="offline-authorization-enterprise"></a>
### Offline Authorization (Enterprise)

Enterprise licensees can make use of offline authorization in situations where a machine that needs to be activated has no access to the internet.
In these cases, a machine that has an active internet connection can be used to create a license file on behalf of another machine.
First, it is necessary to acquire the machine key of the target machine by specifying the `-machineKey` argument:

    InstaLODCmd -machineKey

InstaLODCmd will now output the machine key to the console. Copy and paste the key to the machine that will fulfill the authorization request
and specify the machine key together with the `authorizeKey` argument:

    InstaLODCmd -authorizeKey <key> <filename> <username> <password>

InstaLODCmd will now request a license file for the specified `<key>` and write it to the specified `<filename>`.
Copy the generated license file to the target machine and ingest it into the InstaLOD licensing system by specifying the `-ingestLicense` argument:

    InstaLODCmd -ingestLicense <filename>

The target machine will now be fully activated and ready for use with any of the tools that are part of the InstaLOD SDK.

<a name="profiles-and-batches"></a>
## Profiles and Batches
InstaLOD ships with profile templates for each operation supported by InstaLOD SDK 2.
It is recommended to use these bundled profiles as a starting point when creating new profiles.
Each field or setting in a profile template is fully documented, so they serve both as template and as documentation at the same time.

In order to queue files for optimization a valid profile needs to be loaded. 
Profiles can be loaded by specifying the `-profile` argument followed by a filename:

	-profile <filename>

Once a profile has been loaded, files can be queued by either loading a batch file, specifying a file or searching through a folder.
To load a batch file specify the `-batch` argument.

	-batch <filename>

Files are queued directly using the `-file` argument.
The `-file` argument accepts the following arguments in fixed order:

    -file <input filename> <output filename> <profile name>

By passing the `-folder` argument to InstaLODCmd entire folders can be queued at once by specifying a search folder and a file-mask.
The `-folder` argument requires the following arguments in a fixed order.

	-folder <search path> <file mask> <output format> <profile name>

If the `search path` argument ends with `/**`, InstaLODCmd will recursively search for all files in all sub-folders of the specified folder.

When queuing files using `-folder`, a format string needs to be used to avoid overwriting a single output file name. Format strings can be used anywhere an output path is specified.
For more information on built-in variables and format strings, please refer to the chapter "Format strings with InstaLOD Script variables".

The following command searches recursively through the `Data/` folder and queues all files that match the file mask `SK_*.fbx` using the `OptimizeDemo` profile.

	InstaLODCmd -profile "Profiles/Optimize.json" -folder "Data/**" "SK_*.fbx" "Build/{YEAR}{MONTH}{DAY}_{MESH.FILENAME}_{INDEX}.fbx" OptimizeDemo

> Multiple profiles, batches, files and folders can be specified in a single InstaLODCmd invocation.
> However, profiles need to be loaded prior to queuing any batches, files or folders that are referencing a particular profile.

InstaLODCmd makes use of all available CPU cores, therefore, it is recommended to batch as many operations as possible in a single invocation.

Profiles can be created and edited manually using any text editor. Another excellent way to create InstaLODCmd profiles is by using InstaLOD for Autodesk Maya or InstaLOD for Autodesk 3ds max. Both integrations can export operation based profiles and profiles that create sophisticated LOD chains using the `Batch` functionality of the integration.

Working with the InstaLOD DCC integrations to configure and export profiles is our preferred way of creating new profiles.

> InstaLODCmd ships with well-documented template profiles for all operation types supported by InstaLOD.
> It is highly recommended to use these bundled profiles as a starting point when creating new profiles for InstaLODCmd.


<a name="polygon-optimization"></a>
## Polygon Optimization
Polygon Optimization is the process of removing polygons until the polygon count of the mesh matches the target value. InstaLOD is able to optimize meshes with multiple millions of polygons while maintaining vertex attributes like normals, texture coordinates and colors. Artists can influence the optimization by using vertex colors to mark vertices for preservation or for early removal. When optimizing skinned meshes, InstaLOD will automatically try to preserve polygons around areas critical for the animation. Additionally, InstaLOD features built-in skeleton rig optimization to reduce the amount of bones or vertex weights required to animate the model.
InstaLOD's Smart Optimizer mode aims to produce the best result on a variety of mesh topologies without requiring any additional configuration. 

<img src="http://files.InstaLOD.io/Web/1280x720_Optimize.png" width="100%">

The polygon optimization profile template can be found at `Profiles/Optimize.json`. The bundled optimization profile creates a LOD chain with 4 entries at 100%, 50%, 25% and 10% triangles of the input mesh.

Execute the following command in a shell to load the template profile and execute a sample batch. The batch file consists of two meshes, a static couch model and an animated and skinned parasite zombie: 

    InstaLODCmd -profile Profiles/Optimize.json -batch Batch/Optimize.json

Remember, it is not necessary to use batch files to optimize your data.
To optimize the contents of a single model file, specify it using the `-file` argument:

    InstaLODCmd -profile Profiles/Optimize.json -file Data/SM_Zetsuda_130k.fbx Build/SM_Zetsuda_Optimize.fbx OptimizeDemo

> The `SM_Zetsuda_130k` model is a AAA quality game character that is comprised of more than 35 high resolution textures (up to 4k).
> It is making use of the Stingray PBS-material found in 2016 and later versions of Autodesk Maya, Autodesk Maya LT and Autodesk 3ds Max.
> Earlier versions will not be able to render the character with texture, unless the material setup is recreated manually.
> The character is also being used to demonstrate the powerful material merging capabilities of InstaLOD SDK 2.
> See the chapter '[Merging Mesh Materials](#merging-mesh-materials)' for more information.

Make sure to review the bundled profile template located at `Profiles/Optimize.json` to get a better understanding of all settings related to the polygon optimization operation.

By combining multiple operations InstaLOD is able to perform complex optimization operations like [Visibility Based Mesh Optimization](#visibility-based-mesh-optimization) or custom polygon remeshing as described in the chapter [Mesh Operation Chains](#mesh-operation-chains). 


<a name="optimizing-without-a-profile"></a>
### Optimizing without a Profile
Optimization can be performed without creating an optimization profile. To optimize without a profile specify the `-instaOptimize` argument followed by the target amount of polygons in percent between 1 and 100. InstaLOD will automatically create a default profile with the specified optimization target in percent. It is worth mentioning that if the `instaOptimize` argument has been specified, all queued files and batches will use the automatically created optimization profile.

    InstaLODCmd -instaOptimize 50 -file Data/SM_Couch_10k.fbx Build/Opt_{MESH.FILENAME}{MESH.EXTENSION} InstaOptimize


<a name="remeshing"></a>
## Remeshing
Remeshing is the process of using input mesh data to construct a new mesh from scratch. A normal map is generated in the process that is used to authentically replicate the shading of the input mesh, even when going from multiple millions of polygons down to a few thousand.
Full control over the remeshing resolution and the target face count enables users to create the ideal output mesh for every specific situation.
With features like `Automatic Occlusion Geometry` even non-manifold input meshes can be remeshed without generating interior faces.

> InstaLOD's remeshing is both powerful and fast. Therefore, it is highly recommended to watch the tutorial videos that cover
> the remeshing specific features and setting in detail. 

<img src="http://files.InstaLOD.io/Web/1280x720_Remeshing.png" width="100%">

The remeshing profile template can be found at `Profiles/Remesh.json`. The bundled remeshing profile remeshes the input file at `Normal` resolution using a fuzzy face count target of `Normal`. Using the fuzzy face count target is a great way to create real-time ready assets without specifying a target polygon count that needs to be updated for each mesh. When using a fuzzy face count target, InstaLOD chooses an appropriate target face count based on the remeshing resolution, the input mesh face count, the input mesh complexity and it's bounding box size.

The following command uses the remeshing profile template to remesh the 2 million polygon mesh. The resulting surface will have approximately 5000 polygons and a normal map texture to reproduce the fine details of the high poly input mesh.  

    InstaLODCmd -profile Profiles/Remesh.json -file Data/SM_Scan_Nefertiti_2mio.fbx Build/SM_Nefertiti_Remesh.fbx RemeshDemo

> _Maya Specific_: if color management is enabled, make sure to set the color space of the normal map texture to `Raw` or severe rendering
> artifacts will be present when rendering as `sRGB`.
> 
> _Normal Map Tangent Space_: the template profile outputs a tangent space for `OpenGL` by default.
> The tangent space can be set to `DirectX` by changing the `TangentSpaceFormat` field in the profile.
> 
> _Game Engine Specific_: If your game engine computes the binormal/bitangent per fragment set the `ComputeBinormalPerFragment` field to true in the profile.
> (Unreal Engine 4: `true`, Unity: `false`)
> 
>  Please see the chapter [Rendering Normal Maps](#rendering-normal-maps) for more information.

<a name="game-ready-remeshing"></a>
### "Game-Ready" Remeshing
This workflow is intended to be used for the rapid creation of "Game-Ready" prop assets, where a small trade-off in quality
can be made for a game-changing boost in efficiency and thus saving huge amounts of development time.

  - Take existing high poly sources from generic 3D object repositories and get them game-ready in minutes.
  - Create environment props by quickly throwing together high poly props and let InstaLOD turn the scene into a game-ready prop.
  - Turn high-poly subdivision surfaces or sculpted objects into game ready props without retopologizing the asset.

> The possibilities are endless and with some basic understanding of the system, you will be creating incredible assets with minimum effort in no time.
> Check out our tutorial videos for some great examples on how this workflow can revolutionize your production.

<center><iframe width="640" height="360" src="https://www.youtube.com/embed/z7Q7XwZR-UE?rel=0&amp;controls=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe></center>

-------------------------------------------------------------------------------------------------------------------------------

InstaLODCmd ships with a profile template that produces "Game-Ready" assets. The profile can be found at `Profiles/RemeshGameReady.json`.
This profile remeshes the input mesh using a `High` Resolution and a `Normal` fuzzy face count target.
Important for this particular workflow are the output texture maps.
The "Game-Ready" profile will automatically generate all texture maps necessary to texture the asset
generated by InstaLOD in tools like "Substance Painter" or "Substance Designer":

  - Tangent Space Normals
  - Object Space Normals
  - Position
  - Thickness
  - Curvature
  - Ambient Occlusion
  - Mesh ID

Textures will be generated with a 2048x2048 resolution using x2 super sampling. Existing texture maps of the input mesh will be automatically transferred to the mesh constructed by InstaLOD.

    InstaLODCmd -profile Profiles/RemeshGameReady.json -file Data/SM_Game_MilitaryCrate_1mio.fbx Build/SM_Game_MilitaryCrate_GameReady.fbx RemeshGameReadyDemo

> When importing the geometry created with this profile in Substance Painter or Substance Designer, set the Tangent Space to `OpenGL` and disable `Compute Tangent per Fragment`.
> Additionally, set the texture resolution to 2048x2048.
> It is highly recommended to use 'Tri-planar projection' when texturing with Substance tools to minimize visible seams where possible.
> 
> For more information on using InstaLOD with Substance tools check out our tutorial videos.


<a name="imposters"></a>
## Imposters
Imposters are stand-in meshes typically used for foliage type meshes that are difficult to optimize otherwise. 
InstaLOD is capable of creating a wide range of different imposter types: 

  - AABB
  - Flip-book
  - Billboard
  - (Hybrid) Billboard Cloud 
  - Imposters using custom geometry

Imposter creation with InstaLOD is a one-click solution with full support for per-pixel light and the creation of advanced maps like curvature, AO and more.

The imposterize profile template can be found at `Profiles/Imposterize.json`. The bundled imposterize profile creates a regular billboard imposter with two-sided quads spawned along the XY-axis and YZ-axis.

    InstaLODCmd -profile Profiles/Imposterize.json -file Data/SM_Tree_Birch.fbx Build/{MESH.FILENAME}_Imposter.fbx ImposterizeDemo

<a name="hybrid-billboard-cloud-imposters"></a>
### Hybrid Billboard Cloud Imposters
Hybrid billboard cloud imposters are a new method of creating highly optimized representations of foliage meshes. 
One of the great benefits of using hybrid billboard cloud imposters is the preservation of volume and depth, which allows for excellent self shadowing.
By mixing billboard abstractions for leaves and regular polygon meshes for trunks and branches this kind of hybrid imposter can hold up even on close inspection.
However, using poly meshes for certain parts of the mesh is not a strict requirement. 
If no meshes are found in the scene where the name matches the `CloudPolygonalGeometryMeshSuffix` as configured in the profile, the entire mesh will be built using billboards.
The imposterize profile template can be found at `Profiles/ImposterizeCloud.json`. 
The following example uses a batch file to create a hybrid billboard cloud imposter each of the two input meshes in the batch:

    InstaLODCmd -profile Profiles/ImposterizeCloud.json -batch Batch/Imposterize.json

<center><iframe width="640" height="360" src="https://www.youtube.com/embed/JZctnk-VS1s?rel=0&amp;controls=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe></center>
The video above shows Unreal Engine 4 rendering a hybrid billboard cloud imposter next to the respective input mesh that was used to create the imposter. 
Even though imposters are designed to be viewed at a distance, when used properly they can create a result that holds up well when viewed up close.

<a name="rendering-hybrid-billboard-cloud-imposters"></a>
#### Rendering Hybrid Billboard Cloud Imposters
The result is best rendered using a two-sided masked material for the entire mesh. It is highly recommended to utilize a material that supports two-sided foliage rendering so leaves are properly lit. Alternatively, a `MeshID` map can be generated by InstaLOD to determine when to discard fragments during runtime. 
If the material is properly lit hybrid billboard cloud imposters can produce stunning results at a very low rendering cost.

<a name="merging-mesh-materials"></a>
## Merging Mesh Materials
Both characters and environment props are often constructed using modular pieces or a process called "kit-bashing".
These techniques are a great way to create a wide variety of assets while keeping production time down.
One major draw back of these techniques is that it often requires multiple material textures for such a composed mesh - resulting in a high per object draw-call count.
Therefore, the improvements in production time take a toll on rendering performance.

With InstaLOD such cases can be beautifully optimized by merging all material textures onto a single output texture.
Material size on the resulting texture can be configured by adjusting importance weights:
 
 - Visual weight, fast ray-tracing against the scene at a low resolution to determine visible material to pixel coverage.
 - Texture weight, weights generated via texture dimensions
 - World-space geometry weight, weights generated by determining material coverage in world-space units.
 - UV weight, weights generated by determining material coverage in UV space.

High quality solidification, super-sampling and full control over UV-shell rotation constraints ensure best-mode quality in all cases.
InstaLOD supports merging materials of both static and skinned models or a combination of both.
Handling large scenes with multiple millions of polygons and over-sized 30k textures is no problem for InstaLOD. 

<img src="http://files.InstaLOD.io/Web/1280x720_MaterialMerge.png" width="100%">

The mesh material merge profile template can be found at `Profiles/MaterialMerge.json`. 
The bundled mesh material merge profile creates a 2048x2048 output texture using x2 super-sampling.

    InstaLODCmd -profile Profiles/MaterialMerge.json -file Data/SM_Zetsuda_130k.fbx Build/SM_Zetsuda_MaterialMerge.fbx MeshMaterialMergeDemo


<a name="occlusion-culling"></a>
## Occlusion Culling
InstaLOD features high precision occlusion culling that uses an in-house developed multi-threaded ray tracer.
Visible faces are determined by ray-tracing the scene to a preconfigured resolution using one or multiple cameras stored in the input file.
Occluded geometry can be either directly removed by InstaLOD or vertex colors can be written that can be used to further process the mesh.
In order to avoid removing single faces of large geometry InstaLOD supports removing geometry on a per sub-mesh basis. 
When culling on a per sub-mesh basis the entire sub-mesh will be removed if less than a specified number of faces is visible.
Additionally, InstaLOD supports automatically removing interior faces as a zero-configuration solution. 

<a name="automatic-interior-culling"></a>
### Automatic Interior Culling
As a zero-configuration solution to removing interior geometry the automatic interior culling mode can be used.
Depending on the complexity and density of the input mesh the fields `Resolution`, `AutomaticPrecision` and `AdjacencyDepth` can be increased at the cost of processing time.

<img src="http://files.InstaLOD.io/Web/1280x720_OcclusionCulling_Automatic.png" width="100%">

The occlusion culling profile template can be found at `Profiles/OcclusionCullAutomatic.json`.

    InstaLODCmd -profile Profiles/OcclusionCullAutomatic.json -file Data/SM_OC_House.fbx Build/SM_House_CullAutomatic.fbx OcclusionCullDemo

<a name="camera-based-occlusion-culling"></a>
### Camera Based Occlusion Culling
Camera based occlusion computes occlusion data based on one or multiple cameras.
Both perspective and orthogonal cameras found in the input file will be used for occlusion culling.
In order to better comprehend the view of the camera used for the operation InstaLOD supports writing ray traced image output to disk.
To enabled image rendering simply set `RenderOutputPath` to a valid path.

<img src="http://files.InstaLOD.io/Web/1280x720_OcclusionCulling_Camera.png" width="100%">

The occlusion culling profile template can be found at `Profiles/OcclusionCullCamera.json`. 
The bundled occlusion culling profile uses a custom camera to remove occluded faces and writes camera output to the `Build/` folder.  

    InstaLODCmd -profile Profiles/OcclusionCullCamera.json -file Data/SM_OC_House.fbx Build/SM_House_CullCamera.fbx OcclusionCullDemo

<a name="visibility-based-mesh-optimization"></a>
### Visibility Based Mesh Optimization
Combining occlusion culling and mesh optimization allows for visibility based mesh optimization.
This is a great way to ensure that the optimizer chooses to remove occluded faces before removing visible faces.
By setting the field `DataUsage` to `WriteOptimizerWeightsToWedgeColors` InstaLOD is able to use the visibility data to write optimizer weights into the vertex color channel of the output mesh.
In order to use the output of one operation as input for another operation in the same profile set the `UsePreviousOutputAsInput` field to `true`.
This functionality enables the mesh optimization operation to use the optimizer weights generated by the occlusion cull operation to further optimize the mesh.

<img src="http://files.InstaLOD.io/Web/1280x720_VisibilityOptimize.png" width="100%">
In the image above, a character model has been reduced to 25% of the initial face count using visibility based mesh optimization.
A camera has been placed that captures only the shoulders and the head of the model (bright edges).
The optimized mesh topology that is visible by the camera that was used to perform the occlusion culling remains unchanged.
However, occluded polygons - or polygons outside the camera's frustum (dark edges) have been heavily optimized by InstaLOD. 

The visibility based mesh optimization profile template can be found at `Profiles/VisbilityOptimize.json`. 
The bundled profile uses a custom camera to generate optimizer weights for the input mesh and optimizes the mesh to 25% while making use of the generated optimizer weights.  

    InstaLODCmd -profile Profiles/VisbilityOptimize.json -file Data/SM_OC_House.fbx Build/SM_House_VisOptimize.fbx OcclusionCullDemo

> For more information on how to create complex chained operations please see the chapter 'Mesh Operation Chains'.

<a name="uv-operations"></a>
## UV operations
InstaLOD's powerful UV functionality can also be used outside of any optimization functionality. This can be useful to automatically UV parameterize surfaces (UV Unwrap) or to improve the UV packing of existing meshes.

<a name="uv-unwrap"></a>
### UV Unwrap
The UV Unwrap template can be found at `Profiles/UVUnwrap.json`.
InstaLOD UV unwrapper features three distinct algorithms that each have their strengths on different kinds of input meshes.
`StretchBased` works great for organic meshes while `BestPlane` and `AngleBased` are better suited for hard-surface/constructed type meshes. 

The bundled UV Unwrap profile is configured to use the `BestPlane` algorithm as the couch model used in this example
is a hard-surface type
 mesh.

    InstaLODCmd -profile Profiles/UVUnwrap.json -file Data/SM_Couch_10k.fbx Build/SM_Couch_Unwrap.fbx UVUnwrapDemo

<a name="uv-repack"></a>
### UV Repack
The UV Pack template can be found at `Profiles/UVPack.json`.
The UV packer will create a new UV layout for the input meshes. It is able to stack duplicate shells and create layouts for non-square textures.
In order to preserve axis aligned shells, constraints can be configured that determine how the packer can rotate shells: 
`Allow90` (90° rotations only), `None` (no rotations) or `Arbitrary` (arbitrary rotations).

The bundled UV Pack profile repacks the input mesh UV channel 0 to a 1024x1024 texture with a 2px gutter size and writes the result into UV channel 1.

    InstaLODCmd -profile Profiles/UVPack.json -file Data/SK_ParasiteZombie_10k.fbx Build/SK_ParasiteZombie_Repack.fbx UVPackDemo


<a name="baking"></a>
## Baking
InstaLOD features a sophisticated baker that is fully batch-able and scalable in terms of both output texture dimensions and polygon counts.
Source meshes can be targeted by name using regular expressions to avoid baking onto the wrong target mesh.
Even the most trickiest bakes can easily be solved without inserting additional edges, by utilizing baking cages that have a different topology than the target mesh. A wide range of internally generated texture maps and the capability to transfer existing normal and texture maps make InstaLOD's baker the go-to solution for even the most challenging bake.
High quality dilation and post-process solidification ensure that you will always get best-mode quality for textures generated by InstaLOD.

<a name="targeting-meshes-by-name"></a>
### Targeting Meshes By Name
InstaLOD's baker renders high quality bakes while keeping iteration speed high and setup friction low.
By default, InstaLOD will bake all source meshes onto all target meshes. However, in some cases, it is preferable to perform targeted bakes.

Enable `MatchMeshByName` to match source meshes to target meshes by name. This avoids baking wrong source geometry to target meshes.
Meshes matching the suffix configured in the field `MatchMeshByNameTargetSuffix` will be considered as target mesh.
The suffix will be truncated and the resulting string will form the mesh name that is used to identify matching meshes.
Meshes matching the regular expression (ECMA, ignore-case) configured in the field `MatchMeshByNameSourceRegExFormat` will be used as source mesh.

> Example 1: `hood_rail_low` -> matches suffix `_low` as configured in the field `MatchMeshByNameTargetSuffix` ->
> resulting mesh name `hood_rail` ->
> mesh `hood_rail_high` matches the format string regex `{MESH.NAME}_high` as configured in the field `MatchMeshByNameSourceRegExFormat`.
> 
> Example 2: `pilot_target` -> matches suffix `_target` as configured in the field `MatchMeshByNameTargetSuffix` ->
> resulting mesh name `pilot` ->
> mesh `pilot_source_A` matches the format string regex `{MESH.NAME}_source(.*)` as configured in the field `MatchMeshByNameSourceRegExFormat` ->
> mesh `pilot_source_B` also matches the regex as configured in the field `MatchMeshByNameSourceRegExFormat` as the source mesh name
> regular expression is configured in a way that allows additional characters after the `_source` string.

<a name="internally-generated-texture-pages"></a>
### Internally Generated Texture Pages
In addition to transferring existing normal and texture maps,
InstaLOD supports the creation of texture maps that are directly or indirectly derived from the source mesh.
The following texture pages can be automatically generated by InstaLOD.

| Name                  | Field                         | Pixel Format      |
|-----------------------|-------------------------------|-------------------|
| NormalTangentSpace    | TexturePageNormalTangentSpace | 16bpp RGB         |
| NormalObjectSpace     | TexturePageNormalObjectSpace  | 16bpp RGB         |
| MeshID                | TexturePageMeshID             | 8bpp RGB          |
| VertexColor           | TexturePageVertexColor        | 16bpp RGBA        |
| AmbientOcclusion      | TexturePageAmbientOcclusion   | 8bpp LUMINANCE    |
| Thickness             | TexturePageThickness          | 16bpp LUMINANCE   |
| Displacement          | TexturePageDisplacement       | 16bpp LUMINANCE   |
| Position              | TexturePagePosition           | 16bpp RGB         |
| Curvature             | TexturePageCurvature          | 16bpp LUMINANCE   |
| Transfer              | TexturePageTransfer           | 16bpp RGB         |
| Opacity               | TexturePageOpacity            | 8bpp LUMINANCE    |

<a name="baking-vertex-colors"></a>
### Baking Vertex Colors
By default, existing vertex colors of the source mesh will be transferred.
However, both internally and externally sampled texture pages can be baked into vertex colors instead.
When `AutoMatchTexturePages` is enabled, external texture page names are generated by their type followed by their index ex. `Color_0`, `Color_1`.
Internal texture pages like Ambient Occlusion are referenced by their name ex. `AmbientOcclusion`.

<a name="your-first-bake-with-instalodcmd"></a>
### Your First Bake with InstaLODCmd
The bake template can be found at `Profiles/Bake.json`. InstaLOD's baker is highly configurable and supports many features that ensure you get the possible result. Using the bundled bake template profile, we can easily perform a bake in the same way other mesh operations are executed:

    InstaLODCmd -profile Profiles/Bake.json -file `Source Mesh File` `Target Mesh File` BakeDemo

When baking, the `input filename` will be used as source mesh file and the `output filename` as target mesh file.
Baking supports queuing folders using the `-folder` command and batch files using the `-batch` command in the exact same manner as every other mesh operation.
Output textures will be written to the `OutputPathFormat` as configured in the `Materials` section of the bake profile.
Besides textures, InstaLODCmd will also generate a `output filename_bake.fbx` file that contains the target meshes with materials that reference all generated materials.

If you are in a situation where a single file contains both the source and the target mesh or meshes. You can specify the same file for both input and output. However, this requires the bake profile to target meshes by name.

    InstaLODCmd -profile Profiles/Bake.json -file Data/SM_Game_MilitaryCrate_1mio.fbx Data/SM_Game_MilitaryCrate_Target.fbx BakeDemo

<a name="baking-cages"></a>
### Baking Cages
InstaLOD's baker supports using baking cages with an entirely different topology than the target mesh.
This allows you to insert edge loops to bake beautiful bevels, cut out pieces to perfectly bake decals or build your geometry around specific objects.
When using a cage mesh, InstaLOD matches cage and target meshes via the UV-space, therefore it is important that the cage mesh UVs match those of the target mesh.

> Ensuring that UVs stay in place when creating the baking cage for your target mesh can be a trivial task.
> When using Maya simply enable the move-tool setting 'Preserve UV' prior to moving components -
> or use the 'Multi-Cut' when inserting additional helper edges or edge-guards.

Meshes matching the suffix configured in the field `CageMeshSuffix` will be used as a cage mesh.
There is no limitation on the amount of cage meshes used in a bake.

By default the cage mesh will dictate the origin of the ray that is cast during the bake. 
This gives artists precise control over the bake and helps to resolve bakes that would otherwise be wrong.
However, baking cages become especially helpful once `CageMeshNormalsAsRayDirections` is enabled.
Using the cage mesh's normals as ray directions enables artists to bake beautiful bevels and orthogonal decals onto a fully averaged surface (or bent normals on a surface with face normals). Therefore it is not necessary to insert edges and polygons in the target mesh, that would normally only serve the purpose to force a specific vertex normal orientation.

<img src="http://files.InstaLOD.io/Web/1280x720_BakingCages.png" width="100%">

The following example helps to better illustrate the benefits of using cages. The target mesh `cube_low` is a simple cube, stretched along one axis.
In order to avoid the "puffy surfaces" issue, the normals of the target have been averaged. 
The source mesh `cube_high` is the same cube but with smooth bevels and additional details added.
The following example bakes the mesh without using a cage:

    InstaLODCmd -profile Profiles/Bake.json -file Data/SM_Bake_CageDemo_Source.fbx Data/SM_Bake_CageDemo_NoCage_Target.fbx BakeDemo

The bake will generate the output file `Data/SM_Bake_CageDemo_NoCage_Target_bake.fbx` with the normal map texture assigned.
Upon reviewing the normal map that has been written to the `Build/`-folder it becomes apparent that the additional details appear distorted. 
This is typical when using averaged normals, as baking rays are projected along the normals of the mesh. 

> One of the typical methods of solving this issue is to bake using a target mesh that has additional helper edges inserted.
> This ensures that the normals are orthogonal on flat surfaces, but bent along the edges for smooth bevels. 
> However, the resulting tangent space normal map cannot directly be used on the final mesh as the topology is different.
> Another intermediate step is required as an object space normal map needs to be used and converted into a tangent space normal map usable on the final mesh.
> With InstaLOD, this is not necessary as baking cages can beautifully solve this issue to generate a tangent space normal map that is directly usable.

The following example bakes the mesh using a cage mesh `cube_cage` for the target mesh `cube_low` to solve the issue:

    InstaLODCmd -profile Profiles/Bake.json -file Data/SM_Bake_CageDemo_Source.fbx Data/SM_Bake_CageDemo_Target.fbx BakeDemo

<a name="advanced-baking"></a>
### Advanced Baking
InstaLOD's baker is highly configurable. Please refer to the baking template profile at `Profiles/Bake.json` to get an overview of all available settings and their meaning.

> An excellent way to creating InstaLODCmd baking profiles is by using InstaLOD for Autodesk Maya or InstaLOD for Autodesk 3ds max.
> Both integrations can export bake profiles by right clicking on the `Bake`-button and selecting `Export as InstaLODCmd Profile...`.

<a name="mesh-operation-chains"></a>
## Mesh Operation Chains
InstaLODCmd supports creating complex operations by chaining together multiple mesh operations.
This allows for the creation of unique and highly customized operations that can be used in the same way other profiles are used.

Mesh operation chains function by using the output of one operation as input for the next operation in the chain.
In order to use the output of one operation as input for another operation in the same profile set the `UsePreviousOutputAsInput` field to `true`. When using mesh operation chains it can be desirable to disable the export of intermediate mesh operations, as only the final mesh operation's output is the intended result. To disable the export for specific mesh operations, simply set the `Export` field of the mesh operation entry to `false`.

InstaLOD ships with two template profiles that make use of mesh operation chains. The first example performs visibility based mesh optimization using two chained mesh operations. The profile can be found at `Profiles/VisbilityOptimize.json`.

    InstaLODCmd -profile Profiles/VisbilityOptimize.json -file Data/SM_OC_House.fbx Build/SM_House_VisOptimize.fbx OcclusionCullDemo

The visibility based mesh optimization is described in detail in the dedicated [Visibility Based Mesh Optimization](#visibility-based-mesh-optimization) chapter.

<img src="http://files.InstaLOD.io/Web/1280x720_ComplexMeshOperation.png" width="100%">

The second and more complex example can be found at `Profiles/ComplexPolyRemesh.json`.
It performs a custom polygon-based remeshing using four different mesh operations:

  1. Optimize the input mesh to 10%, but discard all mesh attributes. This allows InstaLOD to optimize with the least amount of constraints and therefore it can generate best-mode for the raw geometry.
  2. Perform occlusion culling in automatic interior mode to remove all occluded faces.
  3. UV unwrap the surface and generate tangents.
  4. Bake the input mesh onto the unwrapped mesh.

To execute the profile invoke the following command:

    InstaLODCmd -profile Profiles/ComplexPolyRemesh.json -file Data/SM_Zetsuda_130k.fbx Build/SM_Zetsuda_PolyRemesh.fbx ComplexPolyRemeshDemo

<a name="materials"></a>
## Materials
The material configuration in InstaLOD is tied to a profile but common accross all operations that make use of materials or textures.
This means that the same configuration can be used in all profiles.

InstaLOD uses `Texture Pages` to configure output texture specifications like texture dimensions and pixel format.
At the same time texture pages are used to determine the input and output type for individual input textures and 
to which output texture it's contents belong.

<img src="http://files.InstaLOD.io/Web/1280x720_MaterialTexturePages.png" width="100%">

> Texture page rules have to be established in order for InstaLOD to understand the semantic of an input texture.
> Texture pages with identical names will be written to a single output texture. 
> Additionally, certain texture types like tangent space normal map or alpha masks can be used by InstaLOD to perform various operations.
> When InstaLOD is generating output materials, the texture page's type determines to which material channel it is assigned.

The `Materials` section of a profile contains all settings relevant to the global material system as used by InstaLODCmd.


| Field                                           | Description                                                       |
|-------------------------------------------------|-------------------------------------------------------------------|
| OutputPathFormat                                | `(string)` The output format string for generated texture files.  |
| DefaultWidth                                    | `(int)` The default output size for texture pages (internal).     |
| DefaultHeight                                   | `(int)` The default output size for texture pages (internal).     |
| AutoMatchTexturePages                           | `(bool)` Enables to use automatic texture page configurations.    |
| AutoMatchTexturePagesForceDefaultOutputSize     | `(bool)` Enables to use default output size for all textures.     |
| TexturePageExportFormat8Bit                     | `(string)` Output format for 8bit textures (PNG/HDR/EXR16/EXR32). |
| TexturePageExportFormat16Bit                    | `(string)` Output format for 16bit textures (PNG/HDR/EXR16/EXR32). |
| TexturePageExportFormat32Bit                    | `(string)` Output format for 32bit textures (PNG/HDR/EXR16/EXR32). |
| TexturePages                                    | `(array)` Array of TexturePage entries.                           |

Example `Materials` configuration:

    "Materials" : {
        "OutputPathFormat" : "Build/{MESH.NAME}_{TEXTURE.TYPE}_{INDEX}_{RANDOM}",
        "DefaultWidth" : 2048,
        "DefaultHeight" : 2048,
        "AutoMatchTexturePages" : true,
        "AutoMatchTexturePagesForceDefaultOutputSize" : true,
        "TexturePageExportFormat8Bit" : "PNG",
        "TexturePageExportFormat16Bit" : "PNG",
        "TexturePageExportFormat32Bit" : "PNG",
        "TexturePages" : [ ]
    }

All textures generated by InstaLOD will be written to the file path as configured in the field `OutputPathFormat`.
In order to avoid overwriting the same file for each texture InstaLOD built-in script variables can be used to construct a unique filename for each texture.
Refer to the chapter [Format strings with InstaLOD Script variables](#format-strings-with-instalod-script-variables) for an overview over all available variables. 
By default, internally generated textures will be created using the size as configured in the fields `DefaultWidth` and `DefaultHeight`.
Externally loaded textures will use the input texture size as output texture size. However, the output size for both internal and external textures can be manually configured using texture page configurations.

<a name="automatic-texture-page-configurations"></a>
### Automatic Texture Page Configurations
Using custom texture page configurations is an excellent way to setup complex rules that integrate perfectly into custom pipelines.
However, InstaLODCmd also supports the automatic creation of rules based on the input file when `AutoMatchTexturePages` is set to `true`.
InstaLOD will then automatically create texture pages for all materials that are attached to a specific material channel (e.g. Color, Normal, ...).
InstaLOD supports using "Layered Textures" for all material channels to handle even the most complex material setups. When using layered textures, InstaLOD will automatically create a texture page with a generated name based on the material channel type followed by the layer index ex. `Color_0`, `Color_1`. 
When loading Stingray-PBS materials, InstaLOD will automatically generate texture pages for each texture assigned to a PBR texture slot.
To force a common output size for all output textures when using `AutoMatchTexturePages` simply set `AutoMatchTexturePagesForceDefaultOutputSize` to `true` and the `DefaultWidth` and `DefaultHeight` will be used for both internal and external textures.

> Automatic texture page configuration is both powerful and flexible and therefore well suited to handle most situations.
> By default, InstaLOD for Autodesk Maya and InstaLOD for Autodesk 3ds max use automatic texture configurations to handle mesh materials.

<a name="custom-texture-page-configurations"></a>
### Custom Texture Page Configurations
Custom texture page configurations are a great way to make sure that your file naming conventions are fully recognized by InstaLOD.
Texture page configurations work by assigning a texture page to input files that match the regular expression of the texture page. 
Texture pages control the type, the pixel specification and dimensions of the texture and whether it is generated or not.
By default, the output dimensions matches the input dimensions and generation is disabled. 
If no texture page configuration matches a texture's filename, a default texture page configuration will be created.

> Custom texture page configurations can also be used to control the output size of internally generated texture pages.
> However, the pixel specification of internal texture cannot be modified.

Texture page configurations can be prioritized using the `Priority` field. A configuration with a high priority overwrites a configuration with a lower priority.
InstaLOD regular expressions are case-insensitive and based on the ECMAScript derivate.

<a name="example-1-color-maps"></a>
#### Example 1: Color maps
The following configuration matches all filenames that contain one of the following strings: _diffuse, _color or _albedo, -diffuse, -color, -albedo.
If a texture page matches this rule, it will be exported as 8bpp RGB texture.

    {
        "MatchRegExFormat" : "(_|-)(diffuse|color|albedo)",
        "MatchWholeWord" : false,
        "EnableOutput" : true,
        "TexturePageName" : "Color",
        "TexturePageType" : "Color",
        "TexturePageBitDepth" : 8,
        "TexturePagePixelType" : "RGB"
    }

<a name="example-2-alpha-masks"></a>
#### Example 2: Alpha masks
Alpha masks can be used by ray casting operations to allow rays to be cast through a surface.
In general, all textures that have an alpha channel can be used as alpha mask.
Greyscale textures can only be used as alpha mask if the `TexturePageType` is set to `Opacity`.
The following configuration matches all filenames that contain the string: _colormask.
If a texture page matches this rule it will be used as alpha mask during casting operations and exported as 8bpp RGBA texture.

    {
        "MatchRegExFormat" : "-colormask.png",
        "MatchWholeWord" : false,
        "EnableOutput" : true,
        "TexturePageName" : "Color",
        "TexturePageType" : "Color",
        "UseAsAlphaMask" : true,
        "TexturePagePixelType" : "RGBA",
        "TexturePageBitDepth" : 8,
        "Priority" : 2
    }

> If the configurations from Example 1 and Example 2 are used in the same profile both rules would apply for a filename
> that ends with the word '-colormask.png' as it would match Example 1's regex rule of '-color' and Example 2's regex rule of '-colormask.png'.
> However, due to the higher priority of Example 2 it would override Example 1.

<a name="example-3-input-normal-maps"></a>
#### Example 3: Input normal maps
A rule needs to be setup for InstaLOD to use normal maps during operations bake operations.
The following configuration matches all filenames that contain the string: _normal.

    {
        "MatchRegExFormat" : "_normal",
        "MatchWholeWord" : false,
        "TexturePageName" : "InputNormalMap",
        "TexturePageType" : "NormalMapTangentSpace",
        "EnableOutput" : false
    }

> In most cases `EnableOutput` should be disabled for input normal maps (default) as the baker will export a properly transformed tangent space normal map if
> `TexturePageNormalTangentSpace` has been enabled in the `BakeOutput` settings of the profile.
> If the output is not disabled, InstaLOD will export the tangent space untransformed.

<a name="example-4-customizing-internal-textures"></a>
#### Example 4: Customizing internal textures
Texture pages can also be used to configure internally generated textures.
The following configuration sets the size of the ambient occlusion map to 1024x1024.

    {
        "MatchRegExFormat" : "AmbientOcclusion",
        "MatchWholeWord" : true,
        "EnableOutput" : true,
        "TexturePageType" : "AmbientOcclusion",
        "TexturePageName" : "AmbientOcclusion",
        "OutputWidth" : 1024,
        "OutputHeight" : 1024
    }

> Whether InstaLOD generates an internal texture is not determined by texture page rules, but by modifying the `BakeOutput` settings
> of the mesh operation. However, for the size rule to be active, `EnableOutput` must be set to `true`.

<a name="rendering-normal-maps"></a>
### Rendering Normal Maps
InstaLOD is able to generate tangent space normal maps that are compatible with either OpenGL or DirectX. 
If normal mapped surface details appear inverted change the value of the `TangentSpaceFormat` field to either `OpenGL` or `DirectX`.
Depending on how your renderer draws normal mapped surfaces, it might be necessary to change `ComputeBinormalPerFragment` to either `true` or `false`. 
If this setting does not match, surfaces can appear slightly perturbed. (Unreal Engine 4: `true`, Unity: `false`)
 
> _Maya Specific_: if color management is enabled, make sure to set the color space of the normal map texture to `Raw` or severe rendering
> artifacts will be present when rendering as `sRGB`.
> This issue is unrelated to InstaLOD and applies to all versions of Maya that support color management.
> Textures assigned to the normal map material channels are imported as `sRGB` instead of `Raw`.
> More information can be found at [Autodesk Maya: Troubleshoot Normal Mapping](http://archive.is/Oxhti).

<a name="format-strings-with-instalod-script-variables"></a>
## Format strings with InstaLOD Script variables
Format strings can be formatted using built-in InstaLOD Script variables. The same placeholders can be used when scripting with InstaLOD Script.

| Token          	  | Description       												                        | Example				        |
|-------------------|-------------------------------------------------------------------|-----------------------|
|{YEAR}        		  |The current year. 													                        | 2017					        |
|{MONTH}        	  |The current month.													                        | 06	   				        |
|{DAY}        		  |The current day of the month.										                  | 12	   				        |
|{INDEX}       		  |The input file index when queuing folders and batch files. 		    | 3						          |
|{RANDOM}      		  |Generates a random integer number. 								                | 1315341				        |
|{MESH.PATH}       	|The file path of the input file.									                  | /Users/InstaLOD/Cmd/	|
|{MESH.FILENAME}   	|The file name of the input file without file extension.			      | SM_Sphere 			      |
|{MESH.NAME}		    |The name of the current mesh in the scene.							            | Sphere_001			      |
|{MESH.EXTENSION}	  |The input file extension including separator.						          | .obj					        |
|{PROFILE.NAME}     |The profile used for the current task.								              | Default				        |
|{TEXTURE.PATH}     |The file path of the texture file.									                | /Users/InstaLOD/Cmd/	|
|{TEXTURE.FILENAME} |The file name of the texture file without file extension.			    | T_NormalTangent		    |
|{TEXTURE.EXTENSION}|The texture file extension including separator.					          | .png 					        |
|{TEXTURE.TYPE}		  |The type of the current texture.									                  | Normal				        |
|{MATERIAL.NAME}	  |The name of the current material.									                | M_MetalRust			      |

The availability and contents of built-in variables is context sensitive.
Mesh related variables are typically available when referencing meshes (eg. when formatting output paths or name matching).
Material and texture related variables are typically available when referencing textures and materials (eg. when formatting output paths or name matching).

> Profile settings that contain the word `Format` denote a format string.
> Format strings enables the use of built-in variables.
> 
> Example: Both `OutputPathFormat` and `MatchMeshByNameSourceRegExFormat` are format strings and therefore allow
> using built-in variables to construct a string that will be evaluated during runtime.

<a name="instalod-script"></a>
## InstaLOD Script
InstaLOD Script (IS) is an interpreted scripting language designed for InstaLOD. 
It enables developers to easily create complex optimization processes that perfectly integrate existing asset pipelines. 
Perform web requests, execute shell commands, write files and read user input. There are nearly no limits to what is possible with InstaLOD Script.

Until the technical documentation for InstaLOD Script is available online, we encourage you to take a look at the sample code that ships with InstaLODCmd. Simply open `Scripts/Example.is` with a text editor of your choice and take a look around. It is fully documented and contains a compact reference and syntax guide.
To execute the script example that ships with InstaLODCmd  enter the following command:

	InstaLODCmd -script Scripts/Example.is

Play around and break things!


<a name="program-arguments"></a>
## Program Arguments
The following arguments can be specified when starting InstaLODCmd. It is necessary that at least one profile is specified and one file or batch operation. If no file or batch is specified, a help will be output to the terminal.

    -profile <filename>
    Loads the profile with the specified filename.
    NOTE: Profiles must be specified prior to files or batches.

    -file <input filename> <output filename> <profile name>
    Queues the specified input file with the specified profile.
    The optimized file is written to the specified output file path.
    InstaLOD will select the importer/exporter based on the file extension.

    -batch <filename>
    Loads the specified batch file.

    -folder <search path> <file mask> <output format> <profile name>
    Searchs the specified folder for files matching the specified file mask.
    To search in sub directories append two asterix to the path e.g. "/models/**"
    To avoid overriding the same file, the output format should contain format tokens.

    -script <filename>
    Queues the specified InstaLODScript file for execution.
    NOTE: If script files are executed the job queue must be manually started in the script.

    -listenScript <filename>
    Listens for the specified InstaLODScript file.
    If the file is found, it is executed. Afterwards it will be deleted and a <filename>.tick file is generated.
    If the file is not found, InstaLODCmd will sleep until the file is available.
    NOTE: To abort execution InstaLODCmd has to be forced to quit (CTRL+C)

    -instaOptimize <percentage>
    Creates a default profile with the specified reduction in percent [0-100]
    and assigns it too all queues files. WARNING: This setting will override any previously set profile.

    -threads <count>
    Specifies the amount of threads available for optimization operations.

    -fbxversion <2010/2011/2012/2013/2014/2016/2018>
    Specifies the FBX file format version used for FBX exports. Default: 2013

    -authorize <email/username> <license/password>
    Authorizes this machine for use with InstaLODCmd.
    NOTE: Please deauthorize your computer before uninstalling to avoid a locked seat.

    -deauthorize <email/username> <license/password>
    Deauthorizes this machine.
    NOTE: Deauthorization takes 24 hours to complete.

    -machineKey
    Retrieves the machine specific authorization key.

    -authorizeKey <key> <filename> <email/username> <license/password>
    Authorizes the machine with the specified key and writes the licensing file to the specified file.

    -ingestLicense <filename>
    Authorizes this machine using the specified license file.

    -plugins <path>
    Loads custom FBX SDK file-format plugins from the specified path.


<a name="available-importexport-file-formats"></a>
## Available import/export file formats
The following file formats can be loaded by InstaLODCmd.
InstaLOD will automatically detect the file format based on the file extensions and file header:

- FBX (*.fbx)
- AutoCAD DXF (*.dxf)
- Alias OBJ (*.obj)
- 3D Studio 3DS (*.3ds) (import only)
- Collada DAE (*.dae)
- (*.zip)

> It is highly recommended to use FBX for both import and export as it is the only file format
> for 3D data that supports storing important data points such as animations, skeleton, normal, binormals, tangents, 
> smoothing groups as well as embedding of texture data.

<a name="adding-custom-file-format-support-via-plug-ins"></a>
## Adding custom file format support via plug-ins
InstaLODCmd allows developers to extend the file format import/export capabilities via plug-ins loaded at runtime.
Plug-ins can implement both custom readers and writers.
To load plug-ins specify the `-plugins <path>` argument when invoking InstaLODCmd from the command line.
More information on [customizing file formats with FBX SDK I/O plug-ins](http://help.autodesk.com/view/FBX/2018/ENU/?guid=__files_GUID_75CD0DC4_05C8_4497_AC6E_EA11406EAE26_htm) is available online.

> It is important to build and link your plug-in to the corresponding FBX SDK version used by InstaLODCmd.
> The current FBX SDK version is output to the console on the 4th line upon launching InstaLODCmd.

<a name="website"></a>
## Website
Please visit http://www.InstaLOD.io to stay up to date!

Thank you for using InstaLOD.
