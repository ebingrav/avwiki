The following code will send a command in every 190 seconds, and due to
random nature of commands, it will stay alive forever. 190 sec is
sufficient to not AFK (or if you do go AFK you'll stay there for 5
seconds tops) and is long enough to avoid large queues when sharpening
for example.

## Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
them to autoload with your character.*

*If autoloading, add a command to start the anti-idler into your main
script by simply putting **idleron** after the **\#read** command.*

    #variable antiidlers[1] affects
    #variable antiidlers[2] who 1 50
    #variable antiidlers[3] who angel
    #variable antiidlers[4] time
    #variable antiidlers[5] look
    #variable antiidlers[6] attr
    #variable antiidlers[7] exits

    #alias {idleroff} {#variable idler 0; #unticker stillalive;}
    #alias {idleron} {#variable idler 1; #ticker {stillalive} {#math var_idle 1d7; $antiidlers[$var_idle];} 190};

## Usage

The script should be autoloaded and enabled by default when logging in.
should you (for some odd reason) not wish to have this on, use
**idleroff** and **idleron** to toggle.

[Category: TinTin++ Scripting](Category:_TinTin++_Scripting "wikilink")
