This is a relatively complex auto-picker script with some fault-recovery
logic in it. It could have been more dumb and work better due to a lack
of complexity, but it's functional the way it is now.

Assumptions:

-   You have a hero level picker - if false, change where the recall
    point goes, substitute with teleport.
-   Your picker does **not** fail dismantling. Races such as halflings
    will not fail even at lord traps - this script does not have a
    dismantle recovery trigger since I have never failed!
-   Your location in Sanctuary is e, n - change this in the header.
-   Overflow mechanic works - I have never tested it as it's hard to get
    in that state. A naked picker with nothing but a load of lockboxes
    and some picks will in principle lose weight as goes goes through
    the boxes, not gain it. You'd need to work over a ton of lord
    lockboxes in order to gain weight and approach buffer.
-   You will be using chest-picks and trap kits - those are simple and
    purchasable at rog guildmaster - if not, change that for something
    else.
-   The picker is atheist and can sacrifice without penalty to health.
    If untrue, leave out the sac part.

## Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
them to autoload with your character.*

    #nop Just change these - location is from sanc, and buffer is the space you don't want to overfill.

    #var mylocation e=n;
    #var carrybuffer 30;

    #nop These are just initializers.

    #var autopick 0;
    #var number 0;
    #var tempcount 0;
    #var tempinput 0;
    #var trapattempt 0;
    #var trap1 none;
    #var trap2 none;
    #var trap3 none;
    #var trap4 none;
    #var trapattempt 1;
    #var carrydif 0
    #var deposited 0;

    #alias {autopickoff} {#showme Stopping.;#var autopick 0;};

    #alias {autopick} {

            #nop We start by determining the amount of lockboxes, irrelevant of condition.;
            #nop Have them in inventory, of course.;

            #var autopick 1;
            #var number 0;
            #var tempcount 0;
            stand;
            i;
            emote ends counting;
    };

    #action {%1 [%2] a small wooden lockbox}{
            #if {"$autopick" == "1"}{
                    #var tempinput %1;
                    #if {"$tempinput" == "(%*)"} {
                            #replace tempinput {(}{};
                            #replace tempinput {)}{};
                            #replace tempinput { }{};
                            #math tempcount {$tempcount + $tempinput};
                    };
                    #else {#math tempcount {$tempcount + 1};};
            };
    };


    #action {^%1 ends counting.$}{
            #if {"$autopick" == "1"}{
                    #if {"$tempcount" > "0"}{

                            #nop we reached the end and determined we have something to do.;

                            #var number $tempcount;
                            recall reset;
                            recall;
                            n;n;n;n;open e;e;recall set;deposit all;sanc;$mylocation;
                            rem trap;
                            hold trap;
                    };
                    #else {
                            #showme Nothing to do!;
                            #var autopick 0;
                    };
            };
    };

    #action {^You are not carrying a trap.$}{
            #if {"$autopick" == "1"}{
                    #showme You have no trapkits. Turning script off.;
                    #var autopick 0;
            };
    };

    #action {^You hold a trap-disarming kit in your hands.$}{
            #if {"$autopick" == "1"}{
                    drop all.lockbox;
                    #var currentbox 1;
                    #var trapattempt 1;
                    inspect lockbox;
            };
    };

    #nop To be absolutely certain, we will require that all 4 attempts end in same result, and if there is
    #nop a difference, we restart the check.

    #action {^The a small wooden lockbox looks like it is armed with a %1 trap.$}{
            #if {"$autopick" == "1"}{
                    #if {"$trapattempt" == "1"}{#var trap1 %1;#var trapattempt 2;inspect $currentbox.lockbox;};
                    #elseif {"$trapattempt" == "2"}{#var trap2 %1;#var trapattempt 3;inspect $currentbox.lockbox;};
                    #elseif {"$trapattempt" == "3"}{#var trap3 %1;#var trapattempt 4;inspect $currentbox.lockbox;};
                    #elseif {"$trapattempt" == "4"}{#var trap4 %1;#var trapattempt 1;dodismantle;};
            };
    };

    #action {^The a small wooden lockbox is not trapped.$}{
            #if {"$autopick" == "1"}{
                    #if {"$trapattempt" == "1"}{#var trap1 none;#var trapattempt 2;inspect $currentbox.lockbox;};
                    #elseif {"$trapattempt" == "2"}{#var trap2 none;#var trapattempt 3;inspect $currentbox.lockbox;};
                    #elseif {"$trapattempt" == "3"}{#var trap3 none;#var trapattempt 4;inspect $currentbox.lockbox;};
                    #elseif {"$trapattempt" == "4"}{#var trap4 none;#var trapattempt 1;dodismantle;};
            };
    };

    #alias {dodismantle}{
            #if {"$autopick" == "1"}{
                    #if {("$trap1" == "$trap2") && ("$trap1" == "$trap3") && ("$trap1" == "$trap4")} {
                            #showme All are the same! Good to proceed.;
                            #if {"$trap1" == "none"}{
                                    #if {$currentbox < $number} {
                                            #math currentbox {$currentbox + 1};
                                            #var trapattempt 1;
                                            #showme inspecting $currentbox.lockbox;
                                            inspect $currentbox.lockbox;
                                    };
                                    #else {
                                            #showme Done with dismantling, onto picking!;
                                            #var currentbox 1;
                                            hold chest-pick;
                                            pick lockbox;
                                    };
                            };
                            #else {dismantle $trap1 $currentbox.lockbox;};
                    };
                    #else {
                            #showme A different trap? Rechecking.;
                            #var trapattempt 1;
                            inspect $currentbox.lockbox;
                    };
            };
    };

    #action {^Wrong trap! You set it off!$}{
            #if {"$autopick" == "1"}{
                    #showme Oops. Assuming I survived, rechecking this one.;
                    #var trapattempt 1;
                    inspect $currentbox.lockbox;
            };
    };

    #action {^You stand up and wipe the sweat from your brow.$}{
            #if {"$autopick" == "1"}{
                    #if {$currentbox < $number} {
                            #math currentbox {$currentbox + 1};
                            #showme inspecting $currentbox.lockbox;
                            inspect $currentbox.lockbox;
                    };
                    #else {
                            #showme Done with dismantling, onto picking!;
                            #var currentbox 1;
                            hold chest-pick;
                            pick lockbox;
                    };
            };
    };

    #action {^The a small wooden lockbox was not trapped.$}{
            #if {"$autopick" == "1"}{
                    #showme some error perhaps. Let's retest.;
                    #var trapattempt 1;
                    inspect $currentbox.lockbox;
            };
    };

    #nop once done we'll lockpick the bunch, which is a much simpler process.;

    #action {^You are not carrying a chest-pick.$}{
            #if {"$autopick" == "1"}{
                    #Showme Out of chest-picks. Get some and retry.;
                    #var autopick 0;
            };
    };

    #action {^*Click*$}{
            #if {"$autopick" == "1"}{
                    #if {$currentbox < $number} {
                            #math currentbox {$currentbox + 1};
                            #showme picking $currentbox.lockbox;
                            pick $currentbox.lockbox;
                    };
                    #else {attr};
            };
    };

    #action {^You couldn't make the lock turn on a small wooden lockbox.$}{
            #if {"$autopick" == "1"}{
                    pick $currentbox.lockbox;
            };
    };

    #action {^A small wooden lockbox is not locked.$}{
            #if {"$autopick" == "1"}{
                    #if {$currentbox < $number} {
                            #math currentbox {$currentbox + 1};
                            pick $currentbox.lockbox;
                    };
                    #else {attr};
            };
    };

    #action {^A simple chest-pick breaks off in the lock!$}{
            #if {"$autopick" == "1"}{
                    hold chest-pick;
            };
    };

    #nop Once all are lockpicked, we open and sac them till fullish, deposit, rinse and repeat.;

    #action {You are carrying %1/%2 lbs.}{
            #if {"$autopick" == "1"}{
                    #math carrydif {%2 - %1};
                    #if {$carrydif < $carrybuffer}{
                            config -demonbank;
                            #if {"$deposited" == "1"}{
                                    #showme Aborting, full! Dump crap!;
                                    #var autopick 0;
                            };
                            #else {
                                    recall;
                                    deposit all;
                                    sanc;
                                    $mylocation;
                                    #var deposited 1;
                                    attr;
                            };
                    };
                    #else {
                            #var deposited 0;
                            open lockbox;
                            get all lockbox;
                            sac lockbox;
                            attr;
                    };
            };
    };

    #action {^A small wooden lockbox is locked.$}{
            #if {"$autopick" == "1"}{
                    #showme Aborting, error!;
                    #var autopick 0;
            };
    };

    #action {^You do not see a lockbox here.$}{
            #if {"$autopick" == "1"}{
                    recall;
                    deposit all;
                    sanc;
                    $mylocation;
                    #showme All done.;
                    #var autopick 0;
                    s;
                    drop all.brown;
                    n;
                    drop all.orange;
                    get all.red;
                    s;
                    drop all.orange;
                    n;
                    get all.heal;
            };
    };

## Usage

Type **autopick** and cross fingers. For best results, have a picker
that is well rested (there will be moving required), and with a ton
(30+) of lockboxes, as well as a reasonable supply (10+) of chest-picks.

[Category:TinTin++ Scripting](Category:TinTin++_Scripting "wikilink")
