---
layout: post
title: 'How to replace gksu (in i3)'
---

## Scenario

You're running i3wm. You have a script that requires root access and you want to bind this script to a key, or perhaps run it when you log in. Usually gksu would solve this problem for you by launching a GUI window in which you can type the password required by sudo. But, if you're on Debian/Ubuntu et al. you'll notice that gksu is now deprecated and no longer available. So what do you do now?

## Enter urxvt

Instead of trying to find a replacement clone for gksu, you can solve this problem on your own! With a terminal like urxvt, that supports window size and executables as arguments, you can do something like this in your i3 config:

```
for_window [class="URxvt" instance="sudo_prompt"] floating toggle

mode "script" {
        bindsym m exec urxvt -geometry 30x5 -name "sudo_prompt" -e my-super-cool-sudo-script; mode "default"
        # ... more script
        # back to normal: Enter or Escape
        bindsym Return mode "default"
        bindsym Escape mode "default"
}

bindsym $mod+Shift+Return mode "script"
```

Some notes on the above configuration:

- The first line configures all urxvt windows with the name "sudo_prompt" to be floating.
- The `-geometry` flag sets the size of the urxvt window
- The `-name` flag sets the name of the urxvt instance (needed for line 1 to work)
- The `-e` flag specifies an executable that urxvt should launch when started

Now instead of gksu, you'll have a small terminal window pop up in the middle of the screen prompting you for your password. :)

A real world example where I need to use this is for my fan speed script which looks like this:

```
#!/bin/bash
x=$(echo -e "0.1\n0.25\n0.5\n0.75\n1" | dmenu -p "Enter a speed (0-1)")

echo 1 | sudo tee /sys/class/drm/card0/device/hwmon/hwmon0/pwm1_enable
MAX=80
SPEED=`echo "$x*$MAX" | bc`
SPEED=${SPEED%.*}
echo $SPEED
if [ "$SPEED" -gt "$MAX" ]; then
    echo "Speed set to above maximum. Aborting."
    exit 1
fi
echo $SPEED | sudo tee /sys/class/drm/card0/device/hwmon/hwmon0/pwm1
```

I hope that you found this useful, cheers!
