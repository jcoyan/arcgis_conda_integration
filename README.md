# arcgis_conda_integration
This arcgis_conda_integration repo contains batch files and a Python script to link parallel user-space Anaconda Python installations with either 32-bit ArcMap 10.5 or 64-bit ArcGIS Pro.

Modification of the Python installations bundled with ArcGIS is potentially dangerous, in that updates to module versions could break ArcPy, sometimes requiring a full rebuild of the user environment. For this reason, the Python bundled with a packaged installation of ArcGIS  is usually locked-down and not able to be be extended or modified by users. The solution is to create a parallel installation of Anaconda Python in user-space, with package versions the same or close to the versions shipped with ArcGIS, and manage the search lists to allow Conda to access ArcGIS libraries and vice versa.

The usercustomise.py script will append the Anaconda Python libraries from a Conda virtual environment to the search paths for ArcGIS Python when Python is invoked from ArcGIS, or, conversely, it will append the ArcGIS Python libraries from ArcGIS Python to the search paths for Anaconda when Python is invoked from Anaconda. This allows users to install and update packages more-or-less at will, and provides a safe way to test new packages in Conda virtual environments.

The versions of packages installed by the batch files in this repo most closely match the ArcGIS versions as at July 16, 2018.

# Installation
The following instructions assume that there are no instances of Python installed other than the ones bundled with ArcGIS. Please uninstall PythonXY or any other distributions you may have installed before commencing the Anaconda installation.

Please perform the required installation(s) for your version(s) of ArcGIS, selecting the "Just Me" option to install in user-space (i.e. without requiring administrative privileges), and accept all other default options except the "Set as default Python 2/3", which will make it easier for you to install and use other IDEs such as Eclipse or PyCharm.

ArcMap 10.5 requires a user-space installation of the 32-bit version of Anaconda Python 2 (https://repo.anaconda.com/archive/Anaconda2-5.2.0-Windows-x86.exe). N.B: DO NOT INSTALL THE 64-BIT VERSION OF PYTHON 2 UNLESS ABSOLUTELY REQUIRED, AND THEN ONLY AFTER READING THE NOTE BELOW.

ArcGIS Pro  requires a user-space installation of the 64-bit version of Anaconda Python 3 (https://repo.anaconda.com/archive/Anaconda3-5.2.0-Windows-x86_64.exe). N.B: DO NOT INSTALL THE 32-BIT VERSION OF PYTHON 3 UNLESS ABSOLUTELY REQUIRED.

Note that you will need to separate the 32-bit and 64-bit versions of Python 2 if you require both on your system, since both installers will attempt to write to the one directory. Recommended practice in this case would be to add a "_32" or "_64" suffix to each default istallation directory in order to explicitly identify which is which. You will also need to adjust the CONDA_CONFIG section of usercustomize.py to be able to find the "Anaconda2_32" directory, since the default settings will look for the default "Anaconda2" directory.

After you have installed the required version(s) of Anaconda, download or clone this repo (https://github.com/GeoscienceAustralia/arcgis_conda_integration.git) to a known directory (e.g. C:\Temp\arcgis_conda_integration), then open the "Anaconda Prompt" shortcut under the relevant version of Anaconda. At the resultant command line, change directory to the one containing these files, and then run the relevant batch file for the Anaconda version, i.e. prepare_anaconda2.bat for Anaconda2, or prepare_anaconda3.bat for Anaconda3. Each batch file will:
- update the base environment of Anaconda
- install some generally useful packages not included in the initial Anaconda installation to the base environment
- create a virtual environment with specific package versions for ArcGIS compatibility
- copy the usercustomize.py script to C:\Users\%username%\AppData\Roaming\Python\Python36\site-packages

Note that if you are prompted for administrative access, You can either accept this if you have access to an administrator account, or just click "Cancel" - this is caused by some packages trying (erroneously) to write to the shared startup menu, and will not affect the actual installations or updates. Once the script has completed, you should be able to start the Anaconda Python, and enter "import arcpy" without error. We may include a work-around to create the missing user start-menu icons at a later date.

To use the ArcGIS-compatible virtual environment, you can use the versions of Spyder and/or Jupyter installed in the virtual environment. Alternatively, you can start the "Anaconda Prompt" for the relevant Python, and then type "activate arc105_32bit" for ArcMap 105 / Python 2.7, or "activate arcgispro-py3" for ArcGIS Pro / Python 3.6. You can then start Python, Jupyter or anything else in the virtual environment from that command prompt.

# Creating your own Virtual Environments / Recovery
Should you wish to test different versions of packages, it is possible to clone the virtual environment as follows:

For ArcMap 105 / Python 2.7:
	conda create --name myclone --clone arc105_32bit 
	
For ArcGIS Pro / Python 3.6:
	conda create --name myclone --clone arcgispro-py3  

Should you corrupt a Conda virtual environment, it is possible to delete it and recreate it from a known starting point. Please refer to https://conda.io/docs/user-guide/tasks/manage-environments.html for more information. In a worst-case scenario, it is also possible to manually delete all files and shortcuts created by the user-space installation, and start the installation procedure completely afresh.

# Contacts and Acknowledgment
To provide feedback or obtain further information on this repo, please contact Alex Ip of Geoscience Australia (alex.ip@ga.gov.au / +61 2 6249 9517).

The version of usercustomize.py in this repo was functionally based on a script written by Curtis Price of USGS (cprice@usgs.gov).
This usercustomize.py script was written by Alex Ip of Geoscience Australia, based on an augmented version by Duncan Moore, also of GA.