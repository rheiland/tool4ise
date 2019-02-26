# tool4ise
This repository helps auto-generate a Jupyter notebook GUI for PhysiCell-related models and output. The directory structure and content of the repository matches a template required for a [nanoHUB](https://nanohub.org/) tool installation. However, creating an actual nanoHUB tool is optional; the GUI created here should also work (with fewer bells & whistles, perhaps) on your personal computer, assuming you have the required Python modules and are able to run a Jupyter notebook server.


## Dependencies
* We highly recommend installing the Anaconda Python 3.x distribution. This will contain Python and various 3rd party modules needed to run the PhysiCell Jupyter GUI. It will also contain the Jupyter notebook server.
* If you are not on Windows, it's possible to install a Python module (hublib) that will provide customized widgets for the GUI. (The make_my_tool.py script will attempt to install this)

## Steps to follow

* Create a new, public repository on github.com (not the IU github) and clone it to your computer. Call it whatever you want (it doesn't have to match the name of your eventual nanoHUB tool). Don't bother to create a default README.md as it would be overwritten below. For the example steps below, we choose the name "ise_proj1".
* Clone this tool4ise repo to your computer. (If you made a default README.md and want to save it, make a copy).
* Copy the contents of the tool4ise repo to your newly created repo (but NOT the hidden ```.git``` directory!)

Copy the relevant files from your PhysiCell model into the relevant subdirectories of this repo. Essentially, you need to copy all of your code (and directory structure) so that when you type ```make``` here in your new repo, it will build your project. For example, one would typically do the following (and there's a Python script in /src which should perform these copies - see Example Steps below):

* copy all /core/ files into the /src/core/ directory
* copy all /BioFVM/ files into the /src/BioFVM/ directory
* copy all /modules/ files into the /src/modules/ directory
* copy all /custom_modules/ files into the /src/custom_modules/ directory
* copy your main.cpp into /src
* copy your Makefile into src/Makefile and 
* edit your newly copied Makefile so that:
```
PROGRAM_NAME := myproj
```
Build using the Makefile - it will build a ‘myproj’ executable. 
* copy ‘myproj’ to /bin in your repo.

* copy your .xml configuration file into data/PhysiCell_settings.xml (overwrite the one there)
* edit data/PhysiCell_settings.xml so that:
```<folder>.</folder>```

From the root dir of your repo:
* run “python make_my_tool.py <your repo name>”, 
e.g., “python make_my_tool.py ise_proj1"    <!-- iu399sp19p001” -->
  
This script will do a number of things: 
* rename two files: <!-- in the /middleware/invoke bash script, and --> the name of the notebook (```.ipynb```, in the root directory) and the primary Python module (in /bin).
* run the xml2jupyter.py script on data/PhysiCell_settings.xml and copy the resulting user_params.py into the /bin directory.
* attempt to install the “hublib” Python module that provides the fancy “Run” button widget (with Output and Cancel features).

Copy the “initial.xml”, from the output you generated when you ran your project in its original location, into this repo's /data directory.

Test your notebook locally. From the root directory:
```
$ jupyter notebook <your-repo>.ipynb
```
Select ‘Cell’ → ‘Run All’ menu item to display the notebook.
Click ‘Run’ button to see if it works. Output files should appear in the /tmpdir directory.

If everything appears to be correct and you want to test and possibly publish your tool on nanoHUB:

* Delete tool4ise.zip in your repo. Optionally, clean up (delete) /src/*.o
* Commit files to your GitHub repo.
* From the status page of your new tool on nanoHUB, click the link to have it installed for testing. (You must be logged in to nanoHUB). Wait 1-3 days for that to happen. You will receive an email from nanoHUB when the tool is installed.
* After your tool in installed and you have tested it and feel like it’s ready to publish, click the link on your tool’s status page that you approve it (for publishing). You will (I think) then be asked to provide the license for your tool and check a box to verify the license is indeed correct. You will receive an email from nanoHUB when the tool is published.


## Example Steps

Here is an example walk-through using commands in a (Unix-like) shell
```
~/git$ git clone git@github.com:rheiland/ise_proj1.git
Cloning into 'ise_proj1'...
warning: You appear to have cloned an empty repository.
~/git$ cd ise_proj1/

~/git/ise_proj1$ cp -R ~/git/tool4ise/* .

~/git/ise_proj1$ ls
LICENSE.txt		doc/			rappture/
README.md		examples/		src/
bin/			make_my_tool.py		tmpdir/
data/			middleware/		tool4ise.ipynb

~/git/ise_proj1$ cd src
~/git/ise_proj1/src$ ls
copy_myproj.py
~/git/ise_proj1/src$ python copy_myproj.py 
num_args= 1
Usage: %s </full/path/to/PhysiCell-proj>

~/git/ise_proj1/src$ python copy_myproj.py ~/dev/PhysiCell-biorobots
num_args= 2
path_to_proj= /Users/heiland/dev/PhysiCell-biorobots
/Users/heiland/dev/PhysiCell-biorobots/core  -->  ./core
/Users/heiland/dev/PhysiCell-biorobots/BioFVM  -->  ./BioFVM
/Users/heiland/dev/PhysiCell-biorobots/modules  -->  ./modules
/Users/heiland/dev/PhysiCell-biorobots/custom_modules  -->  ./custom_modules
/Users/heiland/dev/PhysiCell-biorobots/Makefile  -->  ./Makefile
~/git/ise_proj1/src$ 
~/git/ise_proj1/src$ ls
BioFVM/		copy_myproj.py	custom_modules/
Makefile	core/		modules/

~/git/ise_proj1/src$ 
 


```


<!--
In the `data` directory, you will run the `xml2jupyter.py` script on the .xml file to 
generate `user_params.py`.
-->


