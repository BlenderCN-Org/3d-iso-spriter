# Introduction
Hiya! If you're brand new to this page, probably check out the [Running Scripts in Blender](#running-scripts-in-blender) before reading the rest of the intro.

If you have any more than one frame, it is highly recommended that you use the `anim_batcher.py`, see [Converting Animations to Sprites](#converting-animations-to-sprites). It's much faster than running the other script on each frame, and will make sure that the sprite maintains correct position and scaling throughout the animation. 

If you have an unanimated model, for instance producing an entity's idle sprites, you can use `sprite_batcher.py`, see [Single Frame (Non-animated 3D models)](#single-frame-non-animated-3d-models)

### File Selectors
If you want to use a file selector, you have two options. First, you can copy the tkinter python 3 package into the directory where the scripts preside. 

Secondly, you can run the `get_export_name.py` script and then run the batcher script afterwards, as well as the `get_import_name.py` script if you're using `script_batcher.py` with a 3DS file. These scripts simply bring up a file dialog and saves the chosen path somewhere the other script can access it. You can check if the script worked by calling `bpy.context.scene['(export/import)_name']` in blender's python console. If you use this method, make sure you DO NOT save the blender file after running them. Otherwise you may cause some headaches for people running the script in the future. Calling `bpy.context.scene['(export/import)_name']` on a freshly loaded blender file should result in an exception being thrown.

# Running Scripts in Blender
First, change the screen layout to Scripting

![On the top bar, select the four-rectangles dropdown with 'Default' likely next to it](https://raw.githubusercontent.com/wiki/michaelruigrok/3d-iso-spriter/Images/blender/scripting-screen-mode.png)

Select 'Open', and find the scripting file within your file system

![The bottom of the left, light grey window, should have a toolbar with the word 'Open' on it](https://raw.githubusercontent.com/wiki/michaelruigrok/3d-iso-spriter/Images/blender/open-script.png)

Click 'Run Script'. You may need to extend the size of the Text Editor window for it to show up on the menu.

![Run script appears on the same toolbar, to the right](https://raw.githubusercontent.com/wiki/michaelruigrok/3d-iso-spriter/Images/blender/run-script.png)


# Converting Animations to Sprites

## Setup
We'll use the raccoon walking animation as an example.

Open blender with the file containing the animation. First, select the mesh you want the model to rotate around. For the raccoon, we'll pick its stomach block. Since it's right between each of the raccoon's legs, it makes sense as the place the raccoon would turn around.

[[/Images/blender/select-rotation-mesh-raccoon.png | alt=image]]

Find the selected mesh in the Outliner (It'll be the one with white text) and rename it to 'Model'. This will be the parent object for the model.

[[/Images/blender/rename-selected-mesh.png | alt=image]]

Next, select all the other meshes that make up your model. If all your mesh objects are named similarly, e.g. Cube.00X, you can use the select pattern tool to select multiple at once: ` Select > Select Pattern > type in 'Cube*' (Or whatever is equivalent) > hit enter`. Otherwise, manually select the objects with shift+left click in the Outliner view. Make sure you select the parent object 'Model' last, such that it has white text (which indicates it is the 'active object'). 

[[/Images/blender/select-pattern.png | alt=image]]

[[/Images/blender/select-pattern-2.png | alt=image]]

Then, with the mouse in the 3D view, press Ctrl+P and Set Parent to `Object (Keep Transform)`. 

[[/Images/blender/set-parent.png | alt=image]]

You should be all good to load and run the script as described [above](#Running-Scripts-in-Blender).

## Run from the command line
Once you have prepared the blender file as above and saved it, you can run the script on the saved blender file like so:
```
blender blender_file.blend -b -P anim_batcher.py -- output
```
Note that no matter the file extension given in 'output name', the output will always be a png.

# Single Frame (Non-animated 3D models)
3D models should ideally be exported in the .3DS Autodesk format. If you for whatever reason cannot export in that format, but can in any of the following:
 * fbx, obj, x3d, ply, stl, dae, abc

Have a talk to @michaelruigrok, it shouldn't be too hard to put together a fix for at least some of these.

There are several old methods to run the script, however the easiest way it to simply load the script up in blender and let it run, following each of the prompts. Unlike the old methods, this let's you choose your output. The other option is to run from a command line, which is useful for implementing it in a script.

Even after you've converted your model to sprites, it's a good idea to add it to the resources/model folder.

## Running from blender
### Using an exported .3DS file
The easiest way to run the script is to use one of the file dialog options, as described in the [Introduction](#Introduction)

### Legacy Method - Specify model by path

After loading the script in blender, you can specify the absolute file path in the MODEL_FILE variable declaration. 

![MODEL_FILE appears at the top of the script, under the imports, bundled with all the other global variabls](https://raw.githubusercontent.com/wiki/michaelruigrok/3d-iso-spriter/Images/blender/add-model-file.png)

With this method, files are either outputted in C:\blender-output\ on windows, or ~/blender-output/ on MacOS or other POSIX systems.

### Using the model already in Blender
Instead of specifying the file name, simply select all the objects that the model is comprised of, and then run the script. Make sure you don't have the camera or anything selected when you do so. When the file selector pops up for the 3D model, just press 'cancel'.

If tkinter is working, you will still be prompted where to put the output files. Otherwise, they will be put 

## Run from the command line.

This script can be called from the command line
like so:
```
blender -b -P sprite_batcher.py -- input_model.3ds output
```

Note that no matter the file extension given in 'output name', the output will always be a png.

If you're using a POSIX machine (e.g Mac, Linux) you used to be able to use the helper script `3ds_to_sheet.sh` to automatically combine all the sprites onto one sheet, and rename the sprites to compass directions. This is now obselete, as the python scripts do the compass directions for you, and it is not likely that we will use full sprite sheets.
