This is a relatively simple leveling gear swapping script that tracks
TNL, and will wear preset items when approaching level (and will also
swap back after leveling).

Assumptions:

-   You have TNL in you prompt. If not, simply enable prompt2, and put
    *(TNL:%T/<whatever>* in it, or change the prompt tracking line to
    match your current setup.
-   You will be wearing a [Wide Brimmed Black
    Hat](Wide_Brimmed_Black_Hat "wikilink") as well as a [Silver
    Crucifix](Silver_Crucifix "wikilink"), both etched *lvlgear*. This
    is a common combo of items for those characters
    [devoting](Devotion.md "wikilink")
    [Quixoltan](Quixoltan "wikilink"). If you do not need both items,
    simply remove the line in question below.
-   Similarly, replace the line wearing items back with the correct etch
    for your running items, or directly put in item names if you use
    unetched gear.
-   You are ok with the threshold being set to 250 TNL. If you do not
    think gains this high are likely (lord gameplay), adjust the
    treshold variable.

## Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
them to autoload with your character, as well as autostart by putting
**aon** command after the **\#read** line.*

    #variable autolvl 0;
    #variable treshold 250;
    #variable shouldbeon 0;
    #variable gearon 0;

    #alias {aon}{#showme Starting auto-lvl/gear tracker;#var autolvl 1;#var gearon 0;equipment;};
    #alias {aoff}{#showme Disabling auto-lvl/gear tracker;#var autolvl 0;};

    #nop Determining status - is the gearon or off and what's the state of the tnl;

    #action {^<worn around neck> %1 silver crucifix$}{
            #if {"$autolvl" == "1"}{#showme Leveling gear located;#var gearon 1;};
    };

    #nop Tracking TNL / initiating swaps;

    #action {^(TNL:%1/%2$} {
            #if {"$autolvl" == "1"}{
                    #var currenttnl %1;
                    #if {$currenttnl < $treshold} {#var shouldbeon 1;};
                    #else {#var shouldbeon 0;};
                    #if {("$shouldbeon" == "1") && ("$gearon" == "0")}{
                            #showme Treshold reached! Wearing lvl gear.;
                            wear 'crucifix lvlgear';
                            wear 'widebrimmed lvlgear';
                            #var gearon 1;
                    };
            };
    };

    #action {^Your powers increase!!!$}{
            #if {"$autolvl" == "1"}{
                    #nop Need to wear standard gear.;
                    rem all.lvlgear;
                    wear all.ivoherohit;
                    #var shouldbeon 0;
                    #var gearon 0;
            };
    };

## Usage

Type **aon** to enable, and **aoff** to disable, though it is
recommended to enable this automatically when logging in.

[Category:TinTin++ Scripting](Category:TinTin++_Scripting "wikilink")
