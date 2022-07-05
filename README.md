XFCE is one of the best Desktop Environments out there, it’s one of the oldest, most stable, fast, light and customizable. You can have it looking like Mac or even Windows.

One of the few things that annoys me, is the XFCE default panel, that, when you click an item, it will always launch a new instance of that program, and that behaviour, is just, well, stupid.

There is however a way to fix this using wmctrl and a custom script (thanks ToZ), that will be used to launch our applications, and every time we click an icon on the panel, our script will search for a running instance, and if it’s found, it will simply maximize the already open application.

# Installation

install xmctrl: `$ sudo dnf install wmctrl`
Make the script executable: `$ chmod +x launcer.sh`

# Usage

Where ever you can execute a script, you can use this as a app-launcher. Two possible scenarios are described here as examples:

## Create a new panel launcher
* Right-click Panel->Panel->Add New Items,
* select “Launcher” and click “Add” (new launcher appears on panel), click “Close”
* Right-click the new launcher icon->Properties
* click on “Add new EMPTY item” button (!not "add item"!)
* set:
    - Name = <app name> (same name you would use to launch it from a terminal)
    - Comment = <whatever_you_want>
    - Command = `/path-to-a-folder/launcer.sh/launcher.sh <app name>`
* Select an icon
* Check “Use startup notification”
* Click “Create”

## Use as keyboard shortcut
* Go to Keyboard-settings
* open the "Application Shortcuts" tab
* click the "+ Add"-button
* set:
    - Command = `/path-to-a-folder/launcer.sh/launcher.sh <app name>`
    - check "Use startup notidication"
* click "okay"
* press your button-combo
* enjoy
