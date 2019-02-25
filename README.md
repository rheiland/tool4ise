# tool4ise
Auto-create a Jupyter tool for PhysiCell-related models and output. The directory structure of this repository is intended to match that required for a [nanoHUB](https://nanohub.org/) tool installation. However, creating an actual nanoHUB tool is optional; the GUI created here should also work (with fewer bells & whistles, perhaps) on your personal computer, assuming you have the required Python modules and are able to run a Jupyter notebook server.


## Dependencies
* We highly recommend installing the Anaconda Python 3.x distribution. This will contain Python and various 3rd party modules needed to run the PhysiCell Jupyter GUI. It will also contain the Jupyter notebook server.
* If you are not on Windows, it's possible to install a Python module (hublib) that will provide customized widgets for the GUI. (The make_my_tool.py script will attempt to install this)

## Steps to follow
<!--
```
Copy the relevant files from your PhysiCell model into this repo, specifically:
Your .xml config file into data/PhysiCell_settings.xml (overwrite the one there)
Edit data/PhysiCell_settings.xml so that:
<folder>.</folder>
Your main.cpp into /src
Your /custom_modules/*{.h,.cpp} into /src/custom_modules/*
Your Makefile into src/Makefile, however, you need to edit it there so that:
PROGRAM_NAME := myproj
Build using the Makefile - it will build a ‘myproj’ executable. Copy ‘myproj’ to /bin in your repo.

From the root dir of your repo, run ```python make_my_tool.py <your tool name>``` 
e.g., “python make_my_tool.py iu399sp19p001”
This script will do a number of things:
Replace the name of the tool in various places to be your tool’s name, e.g., in the /middleware/invoke bash script, and the names of the notebook (.ipynb) and the primary Python module (both in /bin).
Run the xml2jupyter.py script on data/PhysiCell_settings.xml and copy the resulting user_params.py into the /bin directory.
```
python xml2jupyter.py PhysiCell_settings.xml
cp user_params.py ../bin
```
Attempt to install the “hublib” Python module that provides the fancy “Run” button widget (with Output and Cancel features).

Copy the “initial.xml”, from the output you generated when you ran your project in another directory, into the /data directory of your repo.
Test your notebook locally. From the root directory:
```$ jupyter notebook <toolname>.ipynb```

Select ‘Cell’ → ‘Run All’ menu item to display the notebook.
Click ‘Run’ button to see if it works. Output files should appear in the /tmpdir directory.
```
-->

Copy the relevant files from your PhysiCell model into this repo, specifically:
* copy your .xml config file into data/PhysiCell_settings.xml (overwrite the one there)
* edit data/PhysiCell_settings.xml so that:
```<folder>.</folder>```
* copy your main.cpp into /src
* copy your /custom_modules/*{.h,.cpp} into /src/custom_modules/*
* copy your Makefile into src/Makefile and edit it there so that:
```
PROGRAM_NAME := myproj
```
Build using the Makefile - it will build a ‘myproj’ executable. 
* copy ‘myproj’ to /bin in your repo.

From the root dir of your repo:
* run “python make_my_tool.py <your tool name>”, 
e.g., “python make_my_tool.py iu399sp19p001”
  
This script will do a number of things: 
* replace the name of the tool in various places to be your tool’s name, e.g., in the /middleware/invoke bash script, and the names of the notebook (.ipynb) and the primary Python module (both in /bin).
* run the xml2jupyter.py script on data/PhysiCell_settings.xml and copy the resulting user_params.py into the /bin directory.
* attempt to install the “hublib” Python module that provides the fancy “Run” button widget (with Output and Cancel features).

Copy the “initial.xml”, from the output you generated when you ran your project in another directory, into the /data directory of your repo.

Test your notebook locally. From the root directory:
```
$ jupyter notebook <toolname>.ipynb
```
Select ‘Cell’ → ‘Run All’ menu item to display the notebook.
Click ‘Run’ button to see if it works. Output files should appear in the /tmpdir directory.

If everything appears to be correct and you want to test and possibly publish your tool on nanoHUB:

* Delete tool4ise.zip in your repo. Optionally, clean up (delete) /src/*.o
* Commit files to your GitHub repo.
* From the status page of your new tool on nanoHUB, click the link to have it installed for testing. (You must be logged in to nanoHUB). Wait 1-3 days for that to happen. You will receive an email from nanoHUB when the tool is installed.
* After your tool in installed and you have tested it and feel like it’s ready to publish, click the link on your tool’s status page that you approve it (for publishing). You will (I think) then be asked to provide the license for your tool and check a box to verify the license is indeed correct. You will receive an email from nanoHUB when the tool is published.


<!--
You will need to provide the following files in the `data` subdirectory:
```
PhysiCell_settings.xml 
initial.xml 
```
-->

<!--
In the `data` directory, you will run the `xml2jupyter.py` script on the .xml file to 
generate `user_params.py`.
-->


