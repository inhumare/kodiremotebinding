## Bind unused remote buttons in Kodi:
This guide is intended for using a MCE remote with Kodi. In [the official Kodi source](https://github.com/xbmc/xbmc/blob/master/xbmc/input/IRTranslator.cpp), a list of buttons are defined that can be rebound easily with the normal keymap.xml file. If your remote has other buttons such as "slideshow", "aspect ratio", "Windows button" that you want to control Kodi with then you've found the right guide! 

# Find out the code of the buttons you want to use
We need to find out the key information that Kodi receives when the button is pressed.

- Enable debug logging and press keys you want to bind.
- Disable debug logging and grep with 'LIRC\|HandleKey' to filter out what we're interested in.
'''
cat kodi.log | grep 'LIRC\|HandleKey'
'''
You'll be able to see the keys you want to use that haven't been bound:
>DEBUG: LIRC: - NEW 1a9 0 KEY_PRESENTATION devinput (KEY_PRESENTATION)
>DEBUG: HandleKey: 0 (0x00, obc255) pressed, action is
Notice the lack of any associated action - this is obviously what we want to add.

- You can also use the command irw then pressed keys and device name will be listed in console.
You should see something like:
>174 0 KEY_FULL_SCREEN devinput
>1a9 0 KEY_PRESENTATION devinput

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/inhumare/kodiremotebinding/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
