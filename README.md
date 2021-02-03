# Cube 2 Sauerbraten Skin Colorizer
Skin Colorizer adds a menu to [Sauerbraten](http://sauerbraten.org) that allows the colorization of individual parts of a playermodel.

![](https://raw.githubusercontent.com/SalatielSauer/misc/master/skincolorizer_demo2.gif)

### Installation
1. Download the [skincolorizer.zip](https://github.com/SalatielSauer/Sauer-Skin-Colorizer/releases/latest) (without extracting it).
2. Move it to the root folder of your Sauerbraten (the home folder or the main installation folder).
3. Find your autoexec.cfg file or create a new one (also in one of the root folders), open it in a text editor and add the two commands:<br>
  `addzip skincolorizer.zip; exec skincolorizer.cfg`<br>
    It will extract (virtually) the zip and run its configuration file (skincolorizer.cfg) whenever you start the game.

To open the menu type `/skincolorizer` in the game's chat console (T key by default).

Alternatively to step 3 you can type the command `/notepad autoexec.cfg` during the game to open the built-in text editor.

### Tools

<img src="gui/filler.png" width="32px"/> Paint Bucket: Apply the color and texture of the selected part to all parts.<br>
![](https://raw.githubusercontent.com/SalatielSauer/misc/master/skincolorizer_colorpicker.gif)<br>

<img src="gui/dropper.png" width="32px"/> Color Picker: Apply the color and texture of the selected part to the target.
![](https://raw.githubusercontent.com/SalatielSauer/misc/master/skincolorizer_paintbucket.gif)<br>

The four circles are base textures in which the colors will be applied, the first three represent the original skins of each team, the last one is a completely white skin.<br>

The radio buttons in the center determine the team to which the skin will be applied.

The colors of the labels represent their state:<br>
Blue: not selected<br>
Orange: Selected<br>
Gray: Source<br>
Green: Target<br>

### Reverting changes
The original files are kept intact in the installation folder, however the client prioritizes files from the home folder, so to recover the original model/skins just remove its folder located at:<br>
`mygames/packages/models/ <playermodel name>` (Windows)<br>
`.sauerbraten/packages/models/ <playermodel name>` (Linux)
