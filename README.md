The method with the file selector requires the tkinter module to be accessible from within blender. This should work out of the box on linux, but for Windows (and maybe MacOS) the legacy method of file selection will have to be used, until a work around is implemented in the upcoming days. 

# Single Frame (Non-animated 3D models)

3D models should ideally be exported in the .3DS Autodesk format. If you for whatever reason cannot export in that format, but can in any of the following:
 * fbx, obj, x3d, ply, stl, dae, abc

Have a talk to @michaelruigrok, it shouldn't be too hard to put together a fix for at least some of these.

There are several old methods to run the script, however the easiest way it to simply load the script up in blender and let it run, following each of the prompts. Unlike the old methods, this let's you choose your output. The other option is to run from a command line, which is useful for implementing it in a script.

Even after you've converted your model to sprites, it's a good idea to add it to the resources/model folder.

**Note: Exporting animations uses a different script, as explained at the bottom.**

## Running from blender
### Using an exported .3DS file

First, change the screen layout to Scripting

[[/Images/blender/scripting-screen-mode.png | alt="On the top bar, select the four-rectangles dropdown with 'Default' likely next to it"]]

Select 'Open', and find the scripting file within your file system

[[/Images/blender/open-script.png | alt=image]]

Click 'Run Script'. You may need to extend the size of the Text Editor window for it to show up on the menu.

[[/Images/blender/run-script.png | alt=image]]

### Legacy Method - Specify model by path

After loading the script in blender, you can specify the absolute file path in the MODEL_FILE variable declaration. 

[[/Images/blender/add-model-file.png | alt=image]]

With this method, files are either outputted in C:\blender-output\ on windows, or ~/blender-output/ on MacOS or other POSIX systems.

### Using the model already in Blender
Instead of specifying the file name, simply select all the objects that the model is comprised of, and then run the script. Make sure you don't have the camera or anything selected when you do so. When the file selector pops up for the 3D model, just press 'cancel'.

If tkinter is working, you will still be prompted where to put the output files. Otherwise, they will be put 

## Run from the command line.

This script can be called from the command line
like so:
```
blender -b -P sprit_batcher.py -- input_model.3ds output
```

Note that no matter the file extension given in 'output name', the output will always be a png.

If you're using a POSIX machine (e.g Mac, Linux) you can also call the helper script 3ds_to_sheet.sh to automatically combine all the sprites onto one sheet, and rename the sprites to compass directions. 
```
./3ds_to_sheet.sh input_model.3ds output.png
```

However, you can also run it from within blender, which may be the easiest option for anyone who's unsure about running from the command line.

# Converting animations to Sprites
This section is still being written, and as such is under-developed. If you need help, contact @michaelruigrok on slack.

We'll use the raccoon walking animation as an example.

Open blender with the file containing the animation. First, select the mesh you want the model to rotate around. For the raccoon, we'll pick its stomach block. Since it's right between each of the raccoon's legs, it makes sense as the place the raccoon would turn around.

Find the selected mesh in the Outliner (It'll be the one with white text) and rename it to 'Model'. This will be the parent object for the model.

Next, select all the other meshes that make up your model. If all your mesh objects are named similarly, e.g. Cube.00X, you can use the select pattern tool to select multiple at once: ` Select > Select Pattern > type in 'Cube*' (Or whatever is equivalent) > hit enter`. Otherwise, manually select the objects with shift+left click in the Outliner view. Make sure you select the parent object 'Model' last, such that it has white text (which indicates it is the 'active object'. 


Then, with the mouse in the 3D view, press Ctrl+P and Set Parent to `Object (Keep Transform)`. 

You should be all good to load and run the script.