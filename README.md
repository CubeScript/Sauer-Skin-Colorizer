# <span style='color: rgb(236, 178, 59)'>Cube 2 Sauerbraten:</span> <span style='color: rgb(88, 221, 235)'>Skin Colorizer</span> <strong style='color: white'>Lite</strong>
**SkinColorizer Lite** adds a menu to [Sauerbraten](http://sauerbraten.org) that allows you to colorize individual parts of models without needing a modified client or image editor.

![](https://github.com/SalatielSauer/misc/blob/master/skincolorizerlite_demo.gif?raw=true)

## Installation
1. Download the [skincolorizer.zip](https://github.com/SalatielSauer/Sauer-Skin-Colorizer/releases/latest) (without extracting it).
2. Move it to the root folder of your Sauerbraten (either the home folder or the main installation folder).
3. Find your autoexec.cfg file or create a new one (also in one of the root folders), open it in a text editor, and add the following two commands:<br>
  `addzip skincolorizer.zip; exec skincolorizer.cfg`<br>
    This will load the zip file and execute its configuration file (skincolorizer.cfg) whenever you start the game.

To open the menu, type `/skincolorizer` or `/skincolorizerlite` in the game's chat console (T key by default).

Alternatively, in step 3, you can type the command `/notepad autoexec.cfg` during the game to open the built-in text editor.

## Tools

Unlike previous ~~Snout~~Colorizer, this version allows you to color hands, weapons, projectiles, pickups, vweps and all playermodels.

![](https://github.com/SalatielSauer/misc/blob/master/skincolorizerlite_demo3.gif?raw=true)

- ### R G B
  These sliders change the color of the selected parts. When you click on a part, the value ​​of each slider is updated with the stored values ​​of that part. You can reset these values to default by clicking on the square showing the current color.

- ### Fullbright
  If enabled, this checkbox displays a slider that lets you control how bright the selected part is.

- ### Good/Neutral/Evil
  These buttons allow you to preview each team's model and move team-related parts to the top of the list. Clicking on the playermodel icons will have the same effect.

- ### Filter
  The filter field is used to move parts that match the input to the top of the list.

- ### Select <part/filter>
  If the filter is empty, the Select button will target all parts of the selected playermodel.

![](https://github.com/SalatielSauer/misc/blob/master/skincolorizerlite_demo2.gif?raw=true)

## Adding new parts

  **SkinColorizer Lite** works by intercepting skin commands (iqmskin, md3skin, md5skin...) directly in the configuration files of each model. To do this, simply add the `skincolorizer` command before the skin command.

  In some cases, the `setskincolorizerdir` command must also be added before the *dir commands (iqmdir, md3dir, md5dir...) to display texture previews correctly in the menu.

In the available skincolorizer.zip, some models already have the necessary settings, so everything should work when you open the menu. However, if you want to add new models/parts:

  For example, in `packages/models/ammo/rockets/md3.cfg`:
  ```plaintext
  md3load "../tris.md3"
  md3skin * "skin.jpg" "masks.jpg"
  mdlambient 50
  mdlspec 150
  mdlglare 0.1 1.0
  ```
  
  becomes:

  ```plaintext
  md3load "../tris.md3"
  skincolorizer md3skin * "skin.jpg" "masks.jpg"
  mdlambient 50
  mdlspec 150
  mdlglare 0.1 1.0
  ```
  This addition allows the menu to get the information it needs to handle the colorization of each part.


## How it works
- **SkinColorizer Lite** uses dynamic variable identifiers to store and access data for each part. These identifiers follow the format:<br>
  `$_skincolorizer_path:packages/models/snoutx10k|part:Upper|`<br>
  When accessed, this variable returns the value:<br>
  `[[R G B] texture is_selected brightness]`<br>
  Storing information directly in the identifier makes filtering and displaying the data much easier.

- To apply the colors, it uses the `<mad>` prefix, which is part of a list of texture processing commands available in vanilla Sauerbraten.

- To display the changes without having to reload the client, it uses the `clearmodel` command. `Clearmodel` only works for models that are currently loaded (with the exception of mapmodels). To work around this, each part is drawn individually in the menu using the `guimodelpreview` command and remains there throughout the session, though kept out of view. This allows selected but undisplayed parts to still be affected by color changes.

- Not all textures for each part are located in the same directory as the .cfg file (accessible via `$mdlname`), so it first tries to use the path defined by `setskincolorizerdir`. If no valid texture is found, it will navigate backward in the file structure from the current `$mdlname` using the `findfile` command until a match is found. If none is found, the small texture thumbnail will be replaced by a white texture, and an error message will be displayed in the console. This is usually fixed by revising the `setskincolorizerdir` command.

- Text colors in Sauerbraten are usually limited to the special characters `^f1` through `^f8`, but this limitation does not apply to the `guitextbox` command. Its fourth argument accepts any custom color value in the format 0xRRGGBB.

- Sauerbraten has a system for determining whether variables should be kept permanently (by writing them to the config.cfg file) or discarded when the client exits. By default, variables defined within model configuration files are not persistent.<br>Since the registration of parts happens through the `skincolorizer` command, which is executed in the context of the model loader, it would normally be impossible to maintain changes between sessions. To work around this, **SkinColorizer Lite** uses two variables containing the same data: <strong>(A)</strong> `$_skcl_all_model_aliases_temp` and <strong>(B)</strong> `$_skcl_all_model_aliases`.  
Variable <strong>A</strong> exists only during the session, while <strong>B</strong>  is defined in a different scope and copies the values from <strong>A</strong> whenever there is a change to the model. If <strong>A</strong> exists, <strong>B</strong> copies its value when restarting the client.
