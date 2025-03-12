autotype(1) -- Automate typing with xdotool or ydotool
=============================================

## SYNOPSIS

`autotype [options] [text]`

## DESCRIPTION

autotype is a command-line tool that automates text input using
either `xdotool` or `ydotool`. It can send keystrokes as regular text input or
simulate modifier key combinations.

Default tool is `xdotool`, but `ydotool` is chosen if the active window name
ends in 'on QEMU/KVM'. This is because `xdotool` does not work well with
virt-manager or virt-viewer.

Autotype uses the same syntax for both `ydotool` and `ydotool`. The syntax is
based on /usr/include/linux/input-event-codes.h key variables.

Autotype can as-is copy-type text from a file or from `stdin`.

## OPTIONS

- `text`  
  Text to write or modifier keys to press. The input can be provided as an
  argument, read from a file, or taken from `stdin`.

- `-d`, `--delay MS | ACTOR`  
  Delay (in milliseconds) between each key press.  
  Default is 120 milliseconds.  
  Maximum delay is 1300 milliseconds, else xdotool will bug out.  
  Instead of MS, you can use ACTOR to automatically choose a delay based on a
  name. Here is the list:

      child: 500
      generic: 250
      clerk: 180
      programmer: 120
      hacker: 90

- `-m`, `--mod`  
  Parse the input as a modifier key combination instead of regular text. Does
  not work when reading from a file or `stdin`.  
  The input text must be a key code variables, for the 'Input event codes' file
  found in /usr/include/linux/input-event-codes.h . Key code variables start
  with the KEY_ or BTN_ prefix.  
  Does not need to start with KEY_ or BTN_ prefix. You can even skip the prefix
  and even use lowercase variables.

## ENVIRONMENT

* `AUTOTYPE_DEBUG` :
  Enable debug output with AUTOTYPE_DEBUG=debug. Will write debug information
  to the file ~/.autotype.log .

* `AUTOTYPE_LAYOUT` :
  For ydotool only. Override systems keyboard layout and force a specific
  layout. Currently, AUTOTYPE_LAYOUT=us or AUTOTYPE_LAYOUT=se can be used.

* `AUTOTYPE_TOOL` :
  Override the typing tool and force a specific one.  
  Can be AUTOTYPE_LAYOUT=xdotool or AUTOTYPE_LAYOUT=ydotool.

## EXAMPLES

Read text from positional argument:

    $ autotype 'Hello, world!'

Read text from file and type it:

    $ autotype < file.txt

Using a 300 milliscond delay between each character:

    $ autotype -d 300 'This text appears slowly.'

Type text much faster:

    $ autotype -d hacker 'This text appears fast.'

Modifier Key Combination.  
This opens "Run as" on Windows or usually "Application Finder" on Linux:

    $ autotype -m 'KEY_LEFTMETA r'

The same, with lowercase and no prefix variables:

    $ autotype -m 'leftmeta r'

This simulates pressing `Ctrl+C`:

    $ autotype -m 'KEY_LEFTCTRL c'

## KEY2CODE

Extra program to help find key variable names for modifier keys.  
This is also used by autotype internally to find key codes for modifier keys.  
Parses /usr/include/linux/input-event-codes.h for key codes.

`key2code [key_name] | [-l|--list]`

Find keycode for a specific key:

    $ key2code KEY_LEFTCTRL

For example find all Windows keys:

    $ key2code -l | grep -i meta

Or find all Ctrl keys:

    $ key2code -l | grep -i ctr

Results in:

    $ KEY_LEFTCTRL 29
    $ KEY_RIGHTCTRL 97

## KNOWN BUGS

If the text starts with a minux sign (-), autotype will interpret it as an
option. To avoid this, use the `--` option to signal the end of options:

    $ autotype -d 10 -- '-hello'

## DEPENDENCIES

- `xdotool`
- `ydotool` (if using `ydotool` mode)

## COPYRIGHT

See license file

## SEE ALSO

xdotool(1), ydotool(1), libinput(1)
