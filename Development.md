**Contents**


# Introduction #

Goals, current direction
[Installation](Installation.md)

# Details #

Add your content here.  Format your content with:
  * Text in **bold** or _italic_
  * Headings, paragraphs, and lists
  * Automatic links to other wiki pages

# Code Structure #
## front end ##

### index.php ###
Main page of the WebAnywhere system. Holds two frames containing browser.php and content.php .

### browser.php ###
Main controllor of WA. Loaded in the upper browser frame of browser. It will load javascripts, handle requests from user.

### content.php ###
Default content page in content frame. Show introduction, keyboard commands and news. Default content page can be changed in config.php.

## main scripts ##

### startup/start.js ###
It calls WA.start.

For standalone mode, it will add event listeners to the navigation window. Set the soundManager onload function. Then call WA.Sound.initSound() to initialize sounds.

When soundmanager loaded, it will call newPage if top.pageLoaded (What if pageLoaded == false at this point? newPage will never be called again?)

### config.php ###
Config TTS, default content page, trafic control, extensions.

### scripts/wa.js ###
This script provides the main functionality for reading through a web page.

Call setBrowseMode(WA.READ) will trigger WA to read text.

### scripts/vars.js ###
Contains global namespace declarations, including configuration directives.

### scripts/nodes.js ###
Contains functions for handling DOM elements.

## proxy ##
wp folder.

## playing sounds ##

### sound ###
Sound folder. Store some mp3 files.

### scripts/sound ###
Following two files have no much use because of soundmanager2.js
  * flash.js
  * sound\_emed.js

### scripts/sound/soundmanager2.js ###
This is third party script. For detail, please refer http://www.schillmania.com/projects/soundmanager2/

### scripts/sound/sounds.js ###
Contains functions for retrieving, prefetching, and playing sounds. Following are frequently used functions:
  * WA.Sound.initSound()
  * WA.Sound.addSound("text to say")
  * WA.SOund.resetSounds()

playWaiting will run as a deamon:
setInterval(function(){self.playWaiting()}, this.playWaitingInterval);

## minify ##
Minify is a library for combining, minifying, and caching JavaScript and CSS files on demand before sending them to a web browser.

## capturing keys ##

### scripts/input/keyboard.js ###
Contains functions for handling keyboard events.

Most key press will trigger WA.Sound.resetSounds() to clear sound queue.

## tracking location of reader and states ##

## scripts yet to be placed under a catagory ##
  * recorder.php
  * sounds3.js
  * survey.php
  * ustils.js