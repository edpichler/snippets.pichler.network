---
title: "macOS X"
weight: 5
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

## Showing hidden files in Mac OS

Just do  `'Command' + 'Shift' + '.'` 

Or, to make it permantently:

```
defaults write com.apple.Finder AppleShowAllFiles true
killall Finder # restart Finder
```

## Switching between different installed Java versions in macOS X
Once you have several java versions installed, you can configure your `.bash_profile` to have the following lines:

``` bash
alias j19="export JAVA_HOME=`/usr/libexec/java_home -v 19`;  java -version"
alias j17="export JAVA_HOME=`/usr/libexec/java_home -v 17`;  java -version"
alias j16="export JAVA_HOME=`/usr/libexec/java_home -v 16`;  java -version"
alias j12="export JAVA_HOME=`/usr/libexec/java_home -v 12`;  java -version"
alias j11="export JAVA_HOME=`/usr/libexec/java_home -v 11`;  java -version"
alias j10="export JAVA_HOME=`/usr/libexec/java_home -v 10`;  java -version"
alias j9="export JAVA_HOME=`/usr/libexec/java_home  -v 9`;   java -version"
alias j8="export JAVA_HOME=`/usr/libexec/java_home  -v 1.8`; java -version"
alias j7="export JAVA_HOME=`/usr/libexec/java_home  -v 1.7`; java -version"
```

Or, in case your user does not have many permissions or/and have used homebrew:

``` bash
alias j8='export  JAVA_HOME="/opt/homebrew/opt/openjdk@8/libexec/openjdk.jdk/Contents/Home"; java -version'
alias j11='export JAVA_HOME="/opt/homebrew/opt/openjdk@11/libexec/openjdk.jdk/Contents/Home"; java --version'
alias j17='export JAVA_HOME="/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"; java --version'
alias j18='export JAVA_HOME="/opt/homebrew/opt/openjdk@18/libexec/openjdk.jdk/Contents/Home"; java --version'
```

## Generating self-signed SSL certificates to be used in Nginx
The same command works on [Linux](../linux/).

``` bash
openssl req -x509 -nodes -days 36500 -newkey rsa:2048  \ 
  -keyout private-selfsigned.key -out public-selfsigned.crt
```

## Discover the IPs that are being resolved for a domain
``` bash
nslookup google.se
```

## Creating a brew PR to upgrade a version
Let's take Opera as an example. The Opera's [tap is here](https://github.com/Homebrew/homebrew-cask/blob/HEAD/Casks/opera.rb).

To upgrade brew casks:
``` bash
brew bump-cask-pr opera --version 77.0.4054.276
```

And for regular formulas:
``` bash
brew bump-formula-pr  ...
```

## Creating permanent aliases
Create a file like ~/.zsh_aliases
``` bash
cd; touch .zsh_aliases
```
Add alias on it;
``` bash
echo "myalias=\"ls\"" >> .zsh_aliases
```

Reference the alias in your bash profile by adding the follow lines.
``` bash
if [ -f ~/.zsh_aliases ]; then
. ~/.zsh_aliases
else 
  touch .zsh_aliases;
  echo echo "myalias=\"ls\"" >> .zsh_aliases;
fi
```

## Oh My Zsh
Show date time in the right side of the window

``` bash
RPROMPT='%{$fg_bold[blue]%} %D %T % %{$reset_color%}'
```

## Advanced macOS Command-Line Tools
### caffeinate - set Mac sleep behavior
Running caffeinate with no flags or arguments prevents your Mac from going to sleep as long as the command continues to run.

caffeinate -u -t <seconds> prevents sleep for the specified number of seconds.

Adding the -d flag also prevents the display from going to sleep.

Specifying an existing process with -w <pid> automatically exits the caffeinate command once the specified process exits.

Passing a command with caffeinate <command> starts the given command in a new process and prevents sleep until that process exits.

### networkQuality
Run networkQuality to run an Internet speed test from your Mac.

Add the -v flag to view more detailed information.

Use the -I flag to run the network test on a specific network interface.


### open - open files and applications
open -a <app> <file> opens the given file with the specified application.

Add the -g flag to open the file in the background, without losing focus from the current application.

open . opens the current directory in a new Finder window.

open -R <file> reveals the given file in a new Finder window.

### textutil - document file converter
textutil can convert files to and from Microsoft Word, plain text, rich text, and HTML formats.

textutil -convert html journal.doc converts journal.doc into journal.html.

The possible values for -convert are: txt, html, rtf, rtfd, doc, docx.

### say - text-to-speech engine
say <message> announces the given message.

say -f input.txt -o output.aiff creates an audiobook from the given text file.

### softwareupdate - manage OS updates
softwareupdate --list prints out available software updates.

sudo softwareupdate -ia installs all available updates.

softwareupdate --fetch-full-installer --full-installer-version <version> tries to download the full installer of the specified macOS version to /Applications.

### system_profiler - view system information
system_profiler by default prints all available system information, which is usually overwhelming.

system_profiler <datatype> only prints information about the given sub-system.

system_profiler -listDataTypes lists all available sub-systems to get information from.

Some particularly useful ones:

system_profiler SPHardwareDataType prints an overview of the hardware of the current machine, including its model name and serial number.

system_profiler SPSoftwareDataType prints an overview of the software of the current machine, including the exact macOS version number.

system_profiler SPPowerDataType prints power and battery information, including the current AC wattage and battery cycle count.

system_profiler SPDeveloperToolsDataType prints the currently active version of the Xcode developer tools and SDK.

## macOS Tricks
### General
#### UI conventions
 - Hold the `Option` key while expanding an outline view to recursively expand all children. (The easiest place to test this is in Finder's List view.)
 - By default, clicking inside a scroll bar will scroll partially towards the clicked location. Hold `Option` while clicking in the scroll bar to jump directly to the clicked location.
 - In a scroll view, use the `Up/Down` keys to scroll in small increments. Hold `Option` to scroll in larger increments, and hold Command to scroll to the beginning or end.
#### Screenshoots
 - After pressing `Shift + Command + 4`, hold `Control` while taking the screenshot to copy to the clipboard instead of saving to file.
 - After pressing `Shift + Command + 4`, press the `Space bar` to select a window to screenshot. Hold down `Option` while taking the screenshot to remove the window's shadow.
 - Right click on the floating screenshot preview to access additional actions.
 #### Open/Save dialogs
  - Press `/` to open it prefilled with the root directory.
  - Press `⌘ + R` to reveal the selected item in Finder.
#### Mission Control / Window Management
 - When a window is inactive, use the `Command` key to interact with it without making it active.
 https://saurabhs.org/macos-tips
#### Menu Bar / Notification Center
 - Hold `Option` while opening the Wi-Fi and Bluetooth menus to access extra options.
 - Hold `Command` while dragging a menu bar icon to move it to a new position.
 - Add new menu bar items by dragging icons from Control Center to the menu bar.
 - `Option-click` the date/time in the menu bar to toggle Do Not Disturb.
 - On a trackpad, swipe horizontally with two fingers over a notification to dismiss that notification.
#### Finder
 - After copying a file, press `⌥⌘V` to move the file instead of pasting a copy of it.
 - Press `⌃⌘N` with multiple files selected to create a new folder with those items.
 - Press `Tab` and `Shift-Tab` to navigate through files alphabetically, regardless of the current sort ordering (only in Icons and List view).
 - Hold `Option` while activating Quick Look to immediately launch into full-screen view.
 - After opening Quick Look with multiple files selected, press `⌘⏎` to display a grid view of all items. Use the arrow keys to navigate and press Return to select an item to focus on.
 - In Columns view, hold `Option` while resizing a column to simultaneously resize all columns.
 - In Columns view, double-click a column separator to auto-resize that column. Hold `Option` while double-clicking on any separator to auto-resize all columns.
 - In Columns view, click the empty space at the bottom of a folder to go to the parent folder.
 - In Columns view, and when in a deeply nested file, press `Shift-Tab` and `Tab` to navigate through the parent directories without losing the path to the file.
 - Hold `Option` while dragging a file to make a new copy instead of moving the original. Hold Command and Option to create an alias to the file.
 - Press `⌘I` to show the inspector for the current file.
 - Press `⌥⌘I` to show a floating inspector that updates with the selected file.
 - Press `⌥⌘C` to copy the full pathname of the currently selected file.
 - Press `⇧⌘.` to toggle showing hidden files.
 - Merge folders by holding Option while dragging one folder on top of another folder.
 - Set a custom icon for a folder by copying the new icon, inspecting the folder (`⌘I`), and pasting the icon by selecting the folder icon in the upper-left of the inspector window and pressing `⌘V`.
 - Drag selected text into a Finder window to quickly create and save a text clipping. (Text clippings are text files that can't be edited and don't require a filename to be saved.)
 - Press `⌥⌘O` to open the selected file and automatically close the Finder window.
 - Press `⌥⇧⌘V` to paste an item while preserving the file permission flags.
 - Hold Command while dragging an icon in Icon view to align it to a grid.
 - Restart Finder by holding Option while right-clicking the Finder dock icon and selecting Relaunch.
 - Drag a folder to the new tab button (only visible if multiple tabs are already open) to open the folder in a new tab.
 - Press `⌃⌘↑` to open the parent folder in a new window.
 - If the toolbar is hidden (`⌥⌘T`), Finder will open folders in a new window.
 - Press `⌘R` with an image selected to rotate it clockwise, and `⌘L` to rotate it counter-clockwise.
#### Dock
 - Press `⌥⌘D` to hide and show the dock.
 - Right-click the Launchpad dock icon to open an app from an inline menu.
 - Add shortcuts to System Settings to the Dock by navigating to /System/Library/PreferencePanes and dragging preference panes to the Dock.
 - To add AirDrop to the Dock, navigate to /System/Library/CoreServices/Finder.app/Contents/Applications in Finder and drag the AirDrop icon to the Dock.
#### Spotlight
 - Press `⌘C` to copy the full path to the selected file, or to copy the result of the current calculation.
 - Press `⌘⏎` or `⌘R` to reveal the selected file in Finder.
 - Use the `name:` filter to only search in the filename.
 - Add `kind:folder` to only search for folder names.
 - Hold `Command` to show the path to the currently selected file.
#### Preview
 - Press ` to bring up a magnifier, and then press + and - to resize it.
 - In a PDF document, re-order the pages in the document by re-ordering the pages in the sidebar.
 - Merge two PDF documents by dragging pages from one document's sidebar to the other document's sidebar.
 - In the save dialog for an image, hold Option while opening the Format menu to access an extended list of formats.
 - Hold `Option` and the Space bar to activate the pan tool.
 - Hold `Option` while in text selection mode to switch to rectangular text selection.
#### QuickTime Player
 - Grab a single frame from a video by pausing on the desired frame (using the Left and Right arrow keys to navigate individual frames) and pressing `⌘C`.
#### Photo Booth
 - Hold `Option` while taking a picture to skip the countdown.



