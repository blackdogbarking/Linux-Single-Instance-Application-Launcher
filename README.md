XFCE is one of the best Desktop Environments out there, it’s one of the oldest, most stable, fast, light and customizable. You can have it looking like Mac or even Windows.

One of the few things that annoys me, is the XFCE default panel, that, when you click an item, it will always launch a new instance of that program, and that behaviour, is just, well, stupid.

There is however a way to fix this using wmctrl and a custom script (thanks ToZ), that will be used to launch our applications, and every time we click an icon on the panel, our script will search for a running instance, and if it’s found, it will simply maximize the already open application.

#### 1. Install xmctrl
`sudo dnf install wmctrl`
#### 2. Create the launcher file:
`touch /path-to-a-suitable-location/launcer.sh`
`nano /path-to-a-suitable-location/launcer.sh`
```
#!/bin/bash
# This script acts as a launcher for apps that observes the following rules:
#   1. If the app is not running, then start it up
#   2. If the app is running, don't start a second instance, instead:
#     2a. If the app does not have focus, give it focus
#     2b. If the app has focus, minimize it
# Reference link: http://forum.xfce.org/viewtopic.php?id=6168&p=1

# there has to be at least one parameter, the name of the file to execute
if [ $# -lt 1 ]
then
  echo "Usage: `basename $0` {executable_name parameters}"
  exit 1
fi

BNAME=`basename $1`

# test to see if program is already running
if [ "`wmctrl -lx | tr -s ' ' | cut -d' ' -f1-3 | grep -i $BNAME`" ]; then
    # means it must already be running
    ACTIV_WIN=$(xdotool getactivewindow getwindowpid)
    LAUNCH_WIN=$(ps -ef | grep "$BNAME" | grep -v grep | tr -s ' ' | cut -d' ' -f2 | head -n 1)

    if [ "$ACTIV_WIN" == "$LAUNCH_WIN" ]; then
        # launched app is currently in focus, so minimize
        xdotool getactivewindow windowminimize
    else
        # launched app is not in focus, so raise and bring to focus
        for win in `wmctrl -lx | tr -s ' ' | cut -d' ' -f1-3 | grep -i $BNAME | cut -d' ' -f1`
        do
            wmctrl -i -a $win
        done
    fi
    exit

else
    # start it up
    $*&
fi

exit 0
```
Make the script executable: `chmod +x /path-to-a-suitable-location/launcer.sh/launcher.sh`

#### 3. Create a new launcher (pcmanfm for demo purposes):
* Right-click Panel->Panel->Add New Items,
* select “Launcher” and click “Add” (new launcher appears on panel), click “Close”
* Right-click the new launcher icon->Properties
* click on “Add new EMPTY item” button (!not "add item"!)
* set:
    - Name = <appname> (same name you would use to launch it from a terminal)
    - Comment = <whatever_you_want>
    - Command = `/path-to-a-suitable-location/launcer.sh/launcher.sh pcmanfm`
* Select an icon
* Check “Use startup notification”
* Click “Create”
