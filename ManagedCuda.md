# Installation von Cuda und ManagedCuda


- Nvidia Toolkit 10 installieren (cuda_10.0.130_411.31_win10 (2GB))
- Prüfen ob nvrtc64_100.dll Datei angelegt wurde, gegebenenfalls umbennenen
- Visual Studio Installation (Reihenfolge eventuell egal)
- ManagedCuda Installation aus dem VS Projekt heraus über den Package Manager (siehe unten)



# Projekte Aufsetzen

### ManagedCuda

Installation über Nuget Termin pro Projekt

Reihenfolge wichtig: 

PM> Install-Package ManagedCuda-100 -Version 10.0.31

PM> Install-Package ManagedCuda-NVRTC -Version 10.0.31  <- includes lots of libraries

PM> Install-Package ManagedCuda-CUBLAS -Version 10.0.31

PM> Install-Package ManagedCuda-NPP -Version 10.0.31

PM> Install-Package ManagedCuda-Nvml.NETStandard -Version 9.1.300

Konfigurationsmanager auf 64x

Pfad zu nvrtc64_100.dll anpassen oder file in Debugordner kopieren 

### Kompilieren

1. C++ Project : Properties --> CUDA C/C++ --> Keep Preprocessed Files --> Yes (--keep)

--> erzeugt .ptx file

2. C++ Properties Device > Code Generation: compute_70,sm_70
  Generate GPU Debug Information: No

(Zuause muss noch vcvars64.bat angepasst werden
@call "%~dp0vcvarsall.bat" x64 vcvars_ver=14.11 %*
https://devblogs.microsoft.com/cppblog/side-by-side-minor-version-msvc-toolsets-in-visual-studio-2017/)

Jedes mal Rebuild notwendig

CudaContext ctx = new CudaContext(0);

CudaKernel matMulGPUDouble = ctx.LoadKernel(@"C:\Users\s224549\source\repos\ConsoleApp15\CudaRuntime\x64\Debug\kernel.ptx", "matMulGPUDouble");

https://github.com/kunzmi/managedCuda/wiki/Setup-a-managedCuda-project noch aktuell?


https://stackoverflow.com/questions/31658832/cant-find-neither-cubin-nor-ptx-file-compiling-cuda

https://devblogs.nvidia.com/programming-tensor-cores-cuda-9/



nvcc -arch=sm_70 C:\Users\s224549\source\repos\ConsoleApp15\CudaRuntime\kernel2.cu

nvcc -arch=sm_70 C:\Users\s224549\source\repos\ConsoleApp15\CudaRuntime\simpleTensorCoreGEMM.cu -lcurand -lcublas

(default ist sm_30)

https://arnon.dk/matching-sm-architectures-arch-and-gencode-for-various-nvidia-cards/

https://docs.nvidia.com/nsight-visual-studio-edition/3.2/Content/CUDA_Properties_Config.htm

### TensorCores

link against libs (either in VS or in Command Line)

https://stackoverflow.com/questions/13570285/how-to-link-library-e-g-cublas-cusparse-for-cuda-on-windows
