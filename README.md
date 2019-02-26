# tool4ise
This repository helps auto-generate a Jupyter notebook GUI for PhysiCell-related models and output. The directory structure and content of the repository matches a template required for a [nanoHUB](https://nanohub.org/) tool installation. However, creating an actual nanoHUB tool is optional; the GUI created here should also work (with fewer bells & whistles, perhaps) on your personal computer, assuming you have the required Python modules and are able to run a Jupyter notebook server.


## Dependencies
* We highly recommend installing the Anaconda Python 3.x distribution. This will contain Python and various 3rd party modules needed to run the PhysiCell Jupyter GUI. It will also contain the Jupyter notebook server.
* If you are not on Windows, it's possible to install a Python module (hublib) that will provide customized widgets for the GUI. (The make_my_tool.py script will attempt to install this)

## Steps to follow

* Create a new, public repository on github.com (not the IU github) and clone it to your computer. Call it whatever you want (it doesn't have to match the name of your eventual nanoHUB tool). Don't bother to create a default README.md as it would be overwritten below. For the example steps below, we choose the name "ise_proj1".
* Clone this tool4ise repo to your computer. (If you made a default README.md and want to save it, make a copy).
* Copy the contents of the tool4ise repo to your newly created repo (but NOT the hidden ```.git``` directory!)

Copy the relevant files from your PhysiCell model into the relevant subdirectories of this repo. Essentially, you need to copy all of your code (and directory structure) so that when you type ```make``` here in your new repo, it will build your project. For example, one would typically do the following (there's a Python script in /src called ```copy_myproj.py``` that should perform these copies - see Example Steps below):

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
Build using the Makefile (run ```make```) - it should build a ‘myproj’ executable. 
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
Select ‘Cell’ → ‘Run All’ menu item to display the notebook (or, if necessary, select the 'Kernel' → ‘Restart & Run All’ menu item).
Click the ‘Run’ button in the GUI to see if it works. Output files should appear in the /tmpdir directory.

If everything appears to be correct and you want to test and possibly publish your tool on nanoHUB:

* Delete tool4ise.zip in your repo. Optionally, clean up (delete) /src/*.o
* Commit files to your GitHub repo.


## Follow-on Steps (for a nanoHUB tool)

* If you do not have a nanoHUB account, register for one at https://nanohub.org/register/
* On https://nanohub.org/tools/create, fill out the basic information for creating your nanoHUB tool. Tool Name should be 3-15 alphanumeric characters, including at least one non numeric character (e.g., ```iu399sp19p042```). Although not required, it’s probably wise to also use only lowercase characters. You’ll also be warned if a tool name has already been taken, however, it may take a few seconds for that to appear. Select the bullet to: Publish as a Jupyter notebook.

* in a browser, go to your newly created repository and edit the ```middleware/invoke``` script *in-place*. You want the name of the .ipynb to be your newly created notebook (=repo) name and the name following the ```-t``` to be the name of your nanoHUB tool. For example:
```
/usr/bin/invoke_app "$@" -C "start_jupyter -A -T @tool ise_proj1.ipynb" -t iu399sp19p042 \
```
If you happened to create your repo to be the same name as your nanoHUB tool, then it would be:
```
/usr/bin/invoke_app "$@" -C "start_jupyter -A -T @tool iu399sp19p001.ipynb" -t iu399sp19p042 \
```

* From the status page of your new tool on nanoHUB, click the link to have it installed for testing. (You must be logged in to nanoHUB). Wait 1-3 days for that to happen. You will receive an email from nanoHUB when the tool is installed.
* After your tool in installed and you have tested it and feel like it’s ready to publish, click the link on your tool’s status page that you approve it (for publishing). You will (I think) then be asked to provide the license for your tool and check a box to verify the license is indeed correct. You will receive an email from nanoHUB when the tool is published.


## Example Steps (on Unix-like shell)

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

~/git/ise_proj1/src$ python copy_myproj.py ~/dev/PhysiCell_heterogeneity
num_args= 2
path_to_proj= /Users/heiland/dev/PhysiCell_heterogeneity
/Users/heiland/dev/PhysiCell_heterogeneity/core  -->  ./core
/Users/heiland/dev/PhysiCell_heterogeneity/BioFVM  -->  ./BioFVM
/Users/heiland/dev/PhysiCell_heterogeneity/modules  -->  ./modules
/Users/heiland/dev/PhysiCell_heterogeneity/custom_modules  -->  ./custom_modules
/Users/heiland/dev/PhysiCell_heterogeneity/Makefile  -->  ./Makefile
/Users/heiland/dev/PhysiCell_heterogeneity/main.cpp  -->  ./main.cpp
/Users/heiland/dev/PhysiCell_heterogeneity/VERSION.txt  -->  ./VERSION.txt


# edit the Makefile:  PROGRAM_NAME := myproj

~/git/ise_proj1/src$ 
~/git/ise_proj1/src$ ls
BioFVM/		copy_myproj.py	custom_modules/	modules/
Makefile	core/		main.cpp

~/git/ise_proj1/src$ make
 ... (will hopefully build and generate myproj)
 
 ~/git/ise_proj1/src$ cp myproj ../bin

~/git/ise_proj1/src$ cd ../data
~/git/ise_proj1/data$ ls
mygui.py	xml2jupyter.py
~/git/ise_proj1/data$ cp ~/dev/PhysiCell_heterogeneity/config/PhysiCell_settings.xml .

# edit this PhysiCell_settings.xml so that: <folder>.</folder>

~/git/ise_proj1$ python make_my_tool.py 
num_args= 1
Usage: %s <your repo name>
 
~/git/ise_proj1$ python make_my_tool.py ise_proj1
num_args= 2
toolname= ise_proj1
Renaming  bin/tool4ise.py  to  bin/ise_proj1.py
Replacing toolname in  bin/ise_proj1.py
Renaming  tool4ise.ipynb  to  ise_proj1.ipynb
Replacing toolname in  ise_proj1.ipynb
Trying to run xml2jupyter.py on your .xml file in /data
num_args= 2
tumor_radius {'type': 'double', 'units': 'micron'}
double:  250.0 , delta_val= 10
oncoprotein_mean {'type': 'double', 'units': 'dimensionless'}
double:  1.0 , delta_val= 0.1
oncoprotein_sd {'type': 'double', 'units': 'dimensionless'}
double:  0.25 , delta_val= 0.01
oncoprotein_min {'type': 'double', 'units': 'dimensionless'}
double:  0.0 , delta_val= 0.01
oncoprotein_max {'type': 'double', 'units': 'dimensionless'}
double:  2.0 , delta_val= 0.1
random_seed {'type': 'int', 'units': 'dimensionless'}
int:  0 , delta_val= 1

 --------------------------------- 
Generated a new:  user_params.py

Trying to import hublib.ui
<IPython.core.display.Javascript object>
```

Finally, try to run the notebook to display the GUI:
```
~/git/ise_proj1$ jupyter notebook ise_proj1.ipynb 
...


If your notebook tells you a file is not found, but it's really there, e.g.:
FileNotFoundError: [Errno 2] No such file or directory: '/Users/heiland/git/ise_proj1/data/PhysiCell_settings.xml'

try: Kernel --> Restart & Run All
```

<!--
In the `data` directory, you will run the `xml2jupyter.py` script on the .xml file to 
generate `user_params.py`.
-->


