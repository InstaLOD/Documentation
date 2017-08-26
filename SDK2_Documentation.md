<img src="http://files.InstaLOD.io/Web/1280x720_InstaLODSDK2Header.png" width="100%">

This page will help you to get up to speed with InstaLOD SDK 2.


## Welcome to InstaLOD SDK2
We are thrilled that you want to integrate our SDK into your product. If you have any questions or need code level support, please don't
hesitate to get in touch with us at hello@InstaLOD.io.

Integrating our SDK is a straight forward and fun task. There are no external dependencies on heavyweight runtimes like .NET and
great care has been taken to create a modern and clean API with little in your way to creating complex optimizations.


<img src="http://files.InstaLOD.io/Web/1280x720_SDK2.png" width="100%">

## Table of Contents

<!-- MarkdownTOC depth=3 autoanchor=true autolink=true bracket='round' -->

- [Getting Started with InstaLOD SDK2](#getting-started-with-instalod-sdk2)
	- [Step 1: Building the Sample Code](#step-1-building-the-sample-code)
	- [Step 2: Loading the Library](#step-2-loading-the-library)
	- [Step 3: Initializing and Authorizing InstaLOD](#step-3-initializing-and-authorizing-instalod)
	- [Step 4: Converting Mesh Data](#step-4-converting-mesh-data)
	- [Step 5: Mesh Optimization](#step-5-mesh-optimization)
	- [Step 6: Creating a renderable IInstaLODMesh representation](#step-6-creating-a-renderable-iinstalodmesh-representation)
	- [Step 7: Multithreading](#step-7-multithreading)
- [Explore InstaLOD](#explore-instalod)

<!-- /MarkdownTOC -->

<a name="getting-started-with-instalod-sdk2"></a>
## Getting Started with InstaLOD SDK2

InstaLOD SDK for Windows has been built using Visual Studio 2015 Update 3 on Windows 10 and requires
the installation of the [Visual C++ Redistributables for Visual Studio 2015 Update 3](http://bit.ly/2nDdBwU).
If you already have installed Visual Studio 2015 or later on your workstation, the installation of the
redistributables is not necessary. The InstaLOD SDK is compatible with Windows XP Service Pack 3 and later.

InstaLOD SDK for MacOS has been built using Xcode 8 on MacOS Sierra.

> The InstaLOD SDK is a 64-bit dynamically linked library that is loaded and linked into your application during runtime.
> 32-bit builds, statically linked libraries and exotic platform binaries can be provided upon request.

<a name="step-1-building-the-sample-code"></a>
### Step 1: Building the Sample Code
Before integrating the InstaLOD SDK into your technology, it is important to ensure that your workstation is setup to run
the InstaLOD SDK. The best way to do this is by building and running one of the bundled sample projects.

InstaLOD SDK2 Update 2 ships with limited sample code. However, a fully documented sample command-line OBJ optimizer is included
in the distribution that demonstrates several tasks:

	- Initializing the InstaLOD SDK
	- Load OBJs from disk
	- Convert OBJ data into an IInstaLODMesh instance using vertex arrays, similar to how GPUs handle data.
	- Optimize a mesh to 50%
	- Mesh operation progress callbacks
	- Export an InstaLODMesh as OBJ
	- Basic Multithreading
	- Tangent and binormal calculation using MikkTSpace
	- Improving InstaLODMesh data by removing duplicate vertex positions.
	- Basic time profiling using C++ 11

Please review the tutorial specific configuration at the top of the `main.cpp` file.
The working directory of the solution should be set to the location of the `main.cpp` file or
the application will not be able to locate the bundled OBJ files.

> If you want to review the capabilities and optimization performance of InstaLOD, check out our
> InstaLOD for Autodesk Maya integration or optimize your FBX files directly with InstaLODCmd.
> Due to the limitations of the OBJ file format and the OBJ importer the results are not in best-mode.


<a name="step-2-loading-the-library"></a>
### Step 2: Loading the Library
The first thing you have to do is to install our header files in a location that is accessible by your project.
Once that is done you can include our C++ header files in your C++ source file.
Add the following lines to the top of your C++ source file:

	#define INSTALOD_LIB_DYNAMIC
	#include "InstaLODAPI.h"
	#include "InstaLODMeshExtended.h"

Once you have included our header you will be able to load the InstaLOD SDK's dynamic library.

> The `INSTALOD_LIB_DYNAMIC` define can also be placed among your project's other pre-processor defines in your solution setup.

If your project does not have it's own DLL loading functions you can use the platform agnostic macro the InstaLOD SDK provides.
However, for these to work you will have to `#include <windows.h>` on Windows platforms or `#include <dlfcn.h>` on MacOS platforms.
To load the InstaLOD SDK with the InstaLOD DLL macros, add the following lines to the initialization of your app:

	INSTALOD_DLHANDLE dllHandle = INSTALOD_DLOPEN(INSTALOD_LIBRARY_NAME);
	InstaLOD::IInstaLOD *instaLOD = NULL;

	if (dllHandle != NULL) {
		fprintf(stdout, "Loaded InstaLOD Library\n");
		pfnGetInstaLOD pGetInstaLOD = (pfnGetInstaLOD)INSTALOD_DLSYM(dllHandle, "GetInstaLOD");

		if (!pGetInstaLOD(INSTALOD_API_VERSION, &instaLOD)) {
			fprintf(stderr, "Failed to get InstaLOD SDK API. Invalid SDK version?\n");
			return;
		}
	}


<a name="step-3-initializing-and-authorizing-instalod"></a>
### Step 3: Initializing and Authorizing InstaLOD
> If you are using a custom build of the SDK with customized licensing you can skip this step!

Before using any public methods of the InstaLOD API you will have to initialize the licensing subsystems.
Using the following code, you can initialize your SDK build:

	if (!instaLOD->InitializeAuthorization("InstaLOD_SDK", NULL)) {
		fprintf(stderr, "Failed to initialize authorization module:\n%s\n", instaLOD->GetAuthorizationInformation());
	}

If you have already authorized your workstation, InstaLOD will automatically load the license from the OS shared preferences path.
On Windows the shared license path is `C:\ProgramData\InstaLOD\` on MacOS the path is `/Users/Shared/InstaLOD/`
Otherwise you can authorize your workstation using your license information from within your app:

	if (!instaLOD->AuthorizeMachine("username", "password")) {
		fprintf(stderr, "Failed to acquire license:\n%s\n", instaLOD->GetAuthorizationInformation());
		return;
	}
	// output current license information to console
	fprintf(stdout, "%s\n", instaLOD->GetAuthorizationInformation());
	fprintf(stdout, "Is host authorized: %s\n", instaLOD->IsHostAuthorized() ? "YES" : "NO");


<a name="step-4-converting-mesh-data"></a>
### Step 4: Converting Mesh Data
Before you can optimize or bake your meshes you need to convert your geometry into a `IInstaLODMesh` class.
The InstaLODMesh is a wedge-based mesh structure. This means that there is no need to manage duplicate vertex positions due to splits
as every wedge has all attributes: Tangent-Basis (Normal, Binormal and Tangent), Colors, TexCoords etc.
The InstaLODMesh is easy to work with as faces can be removed or inserted, wedge attributes can be changed without
having to worry about splitting vertices.
Any kind of mesh buffer - including familiar vertex split-based mesh representations - can be easily converted into
an InstaLOD mesh using the `IInstaLODMesh::SetAttributeArray` method. Alternatively, you can directly access the InstaLODMesh buffers.

Converting an InstaLODMesh back into a vertex split-based mesh representation can be done using the `IInstaLODRenderMesh` class
(see Step 6 for more information).

All interaction with InstaLOD is done via the API handle you've allocated in step 2.
To allocate an IInstaLODMesh instance, simply invoke `IInstaLOD::AllocMesh`:

	InstaLOD::IInstaLODMesh *const mesh = instaLOD->AllocMesh();

To deallocate an IInstaLODMesh, simply invoke `IInstaLOD::DeallocMesh`:

	instaLOD->DeallocMesh(mesh);

To initialize the mesh buffers of your mesh, use the available APIs of the InstaLODMesh class. 
At a bare minimum your mesh needs to have the following mesh attributes properly setup:

  - Front face winding order (Default: counter-clockwise)
  - Mesh format type (Default: OpenGL)
  - Vertex positions
  - Wedge indices (array elements must point to valid elements in the vertex position array)
  - Wedge normals (array size must be equal to size of wedge indices array)
  - Wedge tangents and binormals (array sizes must be equal to size of wedge indices array)
  - Wedge texture coordinate set 0 (array size must be equal to size of wedge indices array)
  - Face smoothing groups (array size must be equal to size of wedge indices / 3)
  - Face material indices (array size must be wedge indices / 3)

> Wedge normals, tangents and binormals can be automatically computed by InstaLOD. To calculate mesh normals, 
> simply cast your mesh into the `IInstaLODMeshExtended` type and invoke `CalculateNormals`. 
> To calculate mesh tangents, invoke `CalculateTangentsWithMikkTSpace`. Calculating tangents requires valid texture coordinates to be present.
> If your input mesh has no texture coordinates, simply fill the array with `InstaVec2F(0, 0)` and the tangents with `InstaVec3F(1, 0, 0)` and binormals with `InstaVec3F(0, 1, 0)`.
> Alternatively, create valid texture coordinates by unwrapping your mesh using the `IUnwrapOperation` class and calculate tangents by invoking `CalculateTangentsWithMikkTSpace`.
> If your input mesh data structure does not contain smoothing groups or materials, simply fill the corresponding IInstaLODMesh array with 0.

Once you have converted your input mesh into an InstaLODMesh representation you can use the `IInstaLODMesh::IsValid` method to
check if the InstaLODMesh is in a healthy state. 
If valid wedge indices and vertex positions are available a call to `IInstaLODMesh::SanitizeMesh`
will either calculate all missing data or fill it with default values to ensure the mesh is considered valid.

Skeletal mesh bone weights can be specified via the `IInstaLODSkinnedVertexData` class which can be obtained by
invoking `IInstaLODMesh::GetSkinnedVertexData` on your InstaLOD mesh instance.
Specifying the skeleton joint hierarchy by constructing an `IInstaLODSkeleton` is only necessary for operations requiring an InstaLOD skeleton instance.


<a name="step-5-mesh-optimization"></a>
### Step 5: Mesh Optimization
To perform mesh optimization, simply allocate an instance of the operation you want to perform using the `IInstaLOD` handle you have allocated in step 2.
In the following example, a basic polygon optimization is performed. The output mesh will have reduced the input mesh triangle count by 50%.

	InstaLOD::OptimizeSettings settings;
	settings.PercentTriangles = 0.5f;

	InstaLOD::IInstaLODMesh *const outputMesh = instaLOD->AllocMesh();
	InstaLOD::IOptimizeOperation *const optimize = instaLOD->AllocOptimizeOperation();

	const InstaLOD::OptimizeResult result = optimize->Execute(mesh, outputMesh, settings);
	if (result.Success == true)
		outputMesh->WriteLightwaveOBJ("Optimized.obj");

	instaLOD->DeallocOptimizeOperation(optimize);
	instaLOD->DeallocMesh(outputMesh);

To reduce memory pressure, most optimization operations can use the same mesh as both input and output mesh.
If the input mesh has vertex skinning data setup, the vertex skinning data will automatically be optimized.

> Optimizing multiple meshes at once can be done by appending all InstaLOD mesh instances to a single InstaLOD mesh using
> `IInstaLODMeshExtended::AppendMesh`. Once the optimization has been completed successfully, simply use `IInstaLODMeshExtended::ExtractSubmesh`
> to extract the optimized meshes.

The next example performs remeshing on an input mesh with two materials. Input tangent space normal maps will also be transferred to the
output mesh.

	using namespace InstaLOD;

	// setup material data, this is consistent for all material based operations
	IInstaLODMaterialData *const materialData = instaLOD->AllocMaterialData();
	IInstaLODMaterial *const material0 = materialData->AddMaterialWithID("Material 0", 0);
	material0->AddTexturePageFromDisk("Color", "color0.png", IInstaLODTexturePage::TypeColor);
	material0->AddTexturePageFromDisk("Normal", "normal0.png", IInstaLODTexturePage::TypeNormalMapTangentSpace);

	IInstaLODMaterial *const material1 = materialData->AddMaterialWithID("Material 1", 1);
	material1->AddTexturePageFromDisk("Color", "color1.png", IInstaLODTexturePage::TypeColor);
	material1->AddTexturePageFromDisk("Roughness", "roughness1.png", IInstaLODTexturePage::TypeMask);
	material1->AddTexturePageFromDisk("Normal", "normal1.png", IInstaLODTexturePage::TypeNormalMapTangentSpace);

	// configure outputs, this is the same for all material based operations that generate textures
	materialData->SetDefaultOutputTextureSize(1024, 1024);
	materialData->SetOutputTextureSizeForPage("Color", 512, 512);
	materialData->EnableOutputForTexturePage("Color", IInstaLODTexturePage::TypeColor, IInstaLODTexturePage::ComponentTypeUInt16, IInstaLODTexturePage::PixelTypeRGB);
	materialData->EnableOutputForTexturePage("Roughness", IInstaLODTexturePage::TypeMask, IInstaLODTexturePage::ComponentTypeUInt8, IInstaLODTexturePage::PixelTypeLuminance);

	// setup remeshing with 3000 triangles as output
	RemeshingSettings settings;
	settings.BakeOutput.TexturePageNormalTangentSpace = true;
	settings.BakeOutput.TexturePageCustom = true;
	settings.AbsoluteTriangles = 3000;

	IRemeshingOperation *const remesh = instaLOD->AllocRemeshingOperation();
	remesh->SetMaterialData(materialData);
	// add all input meshes
	remesh->AddMesh(mesh);

	// start the remesh, optionally set a callback prior to Execute()
	IInstaLODMesh *const outputMesh = instaLOD->AllocMesh();
	const RemeshingResult result = remesh->Execute(outputMesh, settings);
	if (result.Success) {
		outputMesh->WriteLightwaveOBJ("Remesh.obj");
		// retrieve output textures and write to disk
		for (int i=0; i<result.BakeMaterial->GetTexturePageCount(); i++) {
			IInstaLODTexturePage *const texturePage = result.BakeMaterial->GetTexturePageAtIndex(i);
			char filename[256];
			snprintf(filename, sizeof(filename), "Remesh_%s.png", texturePage->GetName());
			texturePage->WritePNG(filename);
		}
	}

	instaLOD->DeallocRemeshingOperation(remesh);
	instaLOD->DeallocMaterialData(materialData);
	instaLOD->DeallocMesh(outputMesh);

This example will write four files to the current working directory:

  - `Remesh.obj`, the output mesh
  - `Remesh_Color.png`, the combined texture pages named "Color"
  - `Remesh_Roughness.png`, the texture page named "Roughness" - as only a single material references this texture it contains blank parts for those input materials that do not reference the texture page.
  - `Remesh_NormalTangentSpace.png`, the output tangent-space normal map with the input mesh normal maps transferred to the remesh surface.

By default, custom input texture pages will not be output by operations that generate textures.
Texture page output must be enabled on a per-texture page basis by invoking `IInstaLODMaterialData::EnableOutputForTexturePage`.
It is possible to specify a different output pixel specification from the input texture pages, and input texture page pixel specifications can be mixed and matched.
If no output size for a texture page has been specified using `IInstaLODMaterialData::SetOutputTextureSizeForPage` the default output size as specified by invoking
`IInstaLODMaterialData::SetDefaultOutputTextureSize` will be used.
Internally generated texture pages can be output by modifying the members of the `BakeOutputSettings` structure.
The `IInstaLODTexturePage` features convenience methods to directly write texture page contents to disk. Alternatively, the underlying data arrays can be
accessed by invoking `IInstaLODTexturePage::GetData`. The `IInstaLODTexturePage` is a fairly complex image representation, it features resizing and blitting operations as well as
sampling methods via pixel and UV coordinates.


<a name="step-6-creating-a-renderable-iinstalodmesh-representation"></a>
### Step 6: Creating a renderable IInstaLODMesh representation
Converting a regular vertex-split mesh into a wedge-based IInstaLODMesh is a trivial task. However, converting a wedge-based mesh representation back to
a vertex-split mesh can be a bit trickier due to the need to determine when to insert vertex splits.
InstaLOD SDK2 ships with a new class that will help you to quickly build renderable mesh representations from your InstaLOD mesh instances.
To build a GPU compatible mesh, cast your InstaLOD mesh into the `IInstaLODMeshExtended` type to allocate an `IInstaLODRenderMesh` and construct the buffers.

	IInstaLODRenderMesh *const renderMesh = outputMeshExtended->AllocRenderMesh();
	InstaLODRenderMeshConstructionSettings constructionSettings;
	renderMesh->Construct(constructionSettings);
	const InstaLODRenderMeshVertex *vertices = renderMesh->GetVertices(&renderMeshVertexCount);
	const uint32 *indices = renderMesh->GetIndices(&renderMeshIndexCount);
	// feed vertices and indices into your app's native mesh structure
	...
	outputMeshExtended->DeallocRenderMesh(renderMesh);

Due to the insertion of vertex splits the indices of the render mesh do not line up with the input InstaLOD mesh instance.
However, the InstaLOD render mesh class provides an array that maps InstaLOD mesh wedge indices to InstaLOD render mesh vertex indices.
To access the wedge index map array, simply invoke `IInstaLODRenderMesh::GetWedgeIndexToRenderVertexMap`.
Skinning data can be obtained by invoking `IInstaLODRenderMesh::GetSkinnedVertexData`.


<a name="step-7-multithreading"></a>
### Step 7: Multithreading
InstaLOD supports multi-threading in various ways. Certain operations, like remeshing and baking automatically spawn
child threads to accelerate certain computations. The amount of child threads spawned is automatically determined according to the
amount of cores available to your operating system.
Additionally, all operations can be allocated and executed in parallel on a child thread.


<a name="explore-instalod"></a>
## Explore InstaLOD
You're now ready to explore the vast amount of features present in InstaLOD SDK2. Once you have implemented the conversion between meshes and materials
from your app to InstaLOD you will be able to execute most of the operations available in SDK2 with just a few lines of code.
The InstaLOD SDK API is very consistent and has been designed with the developer in mind.

If you have any questions, please don't hesitate to get in touch with us at hello@InstaLOD.io. We can provide code level assistance via eMail and Skype.