# Compiling-Matconvnet-for-GPU
Configuring Matconvnet for GPU

The following detailed how I successfully compiled matconvnet for GPU on Windows 10 after a lot of failures. I hope this helps someone.

## To compile matconvnet, I used the following:

1. MATLAB R2019a
2. CUDA 10.2
3. Visual Studio Community 2017 (VS2017) (Make sure you install command line tool when you are installing VS2017)
4. matconvnet-1.0-beta25

## IMPORTANT NOTES: 
1. It is relatively easier to use matconvnet on windows than Linux.
2. It is not advisable to compile matconvnet with the latest versions of matlab, cuda, and visual studio. I had to downgrade to the above versions for a successful compilation.


That being said... Let's go.

### Step 0: 
Make sure you have installed matlab, cuda, and VS2017. Also, download matconvnet from the official website (https://www.vlfeat.org/matconvnet/).

### Step 1:
Open matlab and run (mex -setup) to configure matlab's c++ compiler to VS2017.

### Step 2: 
Edit matlab's nvcc path to ensure that matlab is able to 'see' your cuda installation. Do this by navigating to where you have installed matlab. Typical location is as below:

C:\Program Files\MATLAB\toolbox\distcomp\gpu\extern\src\mex\win64

Open nvcc_msvcpp2017.xml and nvcc_msvcpp2017_dynamic.xml and change the version number to 10.2 or whatever your cuda version is. If you cannot see these two files in the folder, then you are in the wrong matlab folder.

More details can be found here: https://blog.csdn.net/qq_17783559/article/details/105474663

### Step 3:
In vl_compile of matconvnet, comment lines 340 and 341

	%flags.base{end+1} = '-O3' ;
	%flags.base{end+1} = '-DNDEBUG' ;
	
### Step 4: 
In vl_compile of matconvnet, edit line 621 as below:

	flags.base, flags.mexlink{2:end}, '-R2018a', ...

More details can be found here: https://github.com/vlfeat/matconvnet/issues/1200

### Step 5: 
In vl_compile of matconvnet, comment line 646 and put the path to your cl.exe. Usually, the path will be

cl_path = 'C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\Hostx64\x64';

Ensure this path is correct for your VS2017.

### Step 6: 
Compile matcomvnet with the following:

vl_compile('enableGpu;, true, 'cudaMethod', 'nvcc')

Best of luck.
