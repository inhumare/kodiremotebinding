## Bind unused remote buttons in Kodi:
This guide is intended for using a MCE remote with Kodi. In [the official Kodi source](https://github.com/xbmc/xbmc/blob/master/xbmc/input/IRTranslator.cpp), a list of buttons are defined that can be rebound easily with the normal keymap.xml file. If your remote has other buttons such as "slideshow", "aspect ratio", "Windows button" that you want to control Kodi with then you've found the right guide! 

### Get access to Kodi:
It's handy to enable ssh to log in and run commands (can use Putty for Windows).
Enable SMB if it isn't already and map to remote drive so config files can be copied over easily.

### Find out the code of the buttons you want to use
We need to find out the key information that Kodi receives when the button is pressed.

- Enable debug logging and press keys you want to bind.
This makes a debug log file in `/storage/.kodi/temp/kodi.log` (hidden folder)
- Disable debug logging and grep with 'LIRC\|HandleKey' to filter out what we're interested in.
```
cat kodi.log | grep 'LIRC\|HandleKey'
```
You'll be able to see the keys you want to use that haven't been bound:
>DEBUG: LIRC: - NEW 1a9 0 KEY_PRESENTATION devinput (KEY_PRESENTATION)
>DEBUG: HandleKey: 0 (0x00, obc255) pressed, action is

Notice the lack of any associated action - this is obviously what we want to add.

- You can also use the command irw then pressed keys and device name will be listed in console.
You should see something like:
>174 0 KEY_FULL_SCREEN devinput
>1a9 0 KEY_PRESENTATION devinput

### Link the key codes to variables we can bind to actions
Make a new file in the userdata folder 
>Lircmap.xml

The format should be like the following:
Note that the device name is taken from the debug log above.
```xml
<lircmap>
	<remote device="devinput">
		<obc1>KEY_FULL_SCREEN</obc1>
		<obc2>KEY_PRESENTATION</obc2>
		<obc3>KEY_MEDIA</obc3>
		<obc4>KEY_EPG</obc4>
	</remote>
</lircmap>
```
The obc tags are like variables we can bind the key codes to as Kodi doesn't specify all the buttons we need.

### Actually bind your buttons to Kodi actions
Now edit existing xml keymap file or add a new one in `/userdata/keymaps/`

Choose an [action](https://kodi.wiki/view/Action_IDs) for Kodi to perform when key is pressed.

**The important bit is that the obc binds have to be in a separate tag <universalremote>.**
Other more basic remote buttons that have been already defined by Kodi can be rebound normally in the <remote> tag.
```xml
  <keymap>
	<global>
		<remote>
			<recordedtv>AudioNextLanguage</recordedtv>
			<mypictures>Screenshot</mypictures>
			<channelplus>PageUp</channelplus>
			<channelminus>PageDown</channelminus>
			<livetv>ShowSubtitles</livetv>
			<record>ShowSubtitles</record>
			<eject>NextSubtitle</eject>
		</remote>
		<universalremote>
			<obc1>NextSubtitle</obc1>
			<obc2>ToggleWatched</obc2>
			<obc3>Fullscreen</obc3>
			<obc4>CodecInfo</obc4>
		</universalremote>
	</global>
</keymap>
```
	
It's very convenient to use the command `kodi-send --action=reloadkeymaps` after modifying the keymap file to test changes when developing your binds instead of rebooting!

