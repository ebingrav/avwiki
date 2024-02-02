The scripts listed in this category are written to work in the
[TinTin++](http://tintin.sourceforge.net/) client.

If you would like to request a script, please do so on the
[Requests](Requests "wikilink") page.

If you would like to report a bug or request a feature for a specific
script, please do so on that script's talk page.

## Basic Info

TinTin++ is a bit different than you may be used to in a mud client. It
doesn't have a character selection screen and it doesn't persistently
remember your scripts and variables. Instead, you keep your scripts
saved as plain-text files and load them up using the **\#read** command.
Personally, I keep each script as a separate file, and each of my
characters as a separate file. The character file then is full of a
bunch of \#reads which load all the scripts that I want for that
character like so:

-   JoeBlow.char

`#var player JoeBlow`  
`#read RunCount.tin`  
`#read WikiSearch.tin`  
`#read JoeBlow.gear`  
`#read PsiTriggers.tin`  
`#session JoeBlow avatar.outland.org 3000`  
`$player`  
<password>

Then when I open JoeBlow.char with tt++, it'll automatically log JoeBlow
in, and load up all of the scripts that I mentioned. It has a lot of
really cool potential when you get used to it.

There is a lot of great information on the tintin++ website
tintin.sourceforge.net/starting.php

**tip:** It helps to know some basic unix commands to navigate around in
Terminal in OS X. When you start Terminal it puts you into your home
directory. So if you type 'ls' you should see the files and folders
there. You can change directory one by one and navigate to where tintin
and your character files are. For example: 'cd directoryName', then 'ls'
to see where you are, repeat until you see tintin and your char files
and then type './tt++ JoeBob.char' to load JoeBob's char file. Or you
can keep a string somewhere that will do it in one stroke, something
like 'cd firstDirectory/secondDirectory;cd tintin;./tt++ JoeBob.char'
where you need to change firstDirectory and secondDirectory to match the
names of your folders that contain tintin - and maybe add more or
whatever depending on how far it's buried in your home directory. also,
in the menu beneath Terminal is Preferences, where you can change the
window background color and text etc.

-   as a second opinion: i love tintin++ (as an OS X user) and have
    really come to appreciate it's flexibility and performance even
    though i've just been using it a short while. once you make a few
    character files you'll have your basics set to copy for the next.
    for example i have my full-sized keyboard use its keypad for
    directions and other functions. many characters might share certain
    things, like a trig to pick up a disarmed weapon or a piece of eq
    that zaps from alignment change. you can copy these into just the
    character files that need them, in the format as shown above. i'll
    put a longer one below:

<!-- -->

    #var player joebob
    #read AutoRescue.tin
    #action {You feel a slight headache} {c 'shocking grasp'=c 'magic missile'}
    #action {You feel less fatigued} {snk=snk}
    #action {disarms you and sends your weapon flying} {get antenna=wield antenna}
    #action {You failed your %w} {c %1}
    #action {You failed your %w%s} {c %1}
    #action {The safety of shade disappears} {t joebob.md|R|Your |C|Dark Embrace|R| has fallen!|N|}
    #action {Your mystical barrier shimmers and is gone} {t joebob |R|Your |P|Mystical Barrier|R| has fallen!|N|}
    #macro {\eOw} {#send aff}
    #macro {\eOx} {#send north}
    #macro {\eOy} {#send aff}
    #macro {\eOt} {#send west}
    #macro {\eOu} {#send look}
    #macro {\eOv} {#send east}
    #macro {\eOq} {#send mae}
    #macro {\eOr} {#send south}
    #macro {\eOs} {#send mtr}
    #macro {\eOm} {#send down}
    #macro {\eOl} {#send up}
    #macro {\e[15~} {#send config +savespell}
    #macro {\e[17~} {#send config -savespell}
    #split
    #ses joebob avatar.outland.org 3000;joebob;myPassWord

## Scripts

**[Run Stats Counter](TinTin++_Run_Stats_Counter "wikilink"):** *Counts
numerous things during your runs. Mostly xp gained and lost, and certain
actions performed during runs (bash, trip, rescue, flee, death, etc) and
shows them only when appropriate.*  
**[WikiSearch](TinTin++_WikiSearch.md "wikilink"):** *Searches
AvatarWiki for whatever you'd like.*  
**Before using actions, make sure you are familiar with Avatar's
[Trigger Policy](Trigger-Using_Policy.md "wikilink").**

[Category: Scripting](Category:_Scripting "wikilink")
