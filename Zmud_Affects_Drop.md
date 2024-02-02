This is the Affect Drop set of triggers from the amezyarak site, added
as the site is now defunct.

Echo's to the user when spells have dropped.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {affectdrop}
    #TRIGGER {^The pink aura around you fades away.} {#echo SPELL DROP: Faerie Fire.}
    #TRIGGER {^Your mental barrier breaks down.} {#echo SPELL DROP: Fortitudes}
    #TRIGGER {^Your Aura of Holiness fades...} {#echo SPELL DROP: Awen}
    #TRIGGER {^You feel less focused.} {#ec SPELL DROP: concentrate.}
    #TRIGGER {Your skin returns to normal.} {#echo SPELL DROP: Barkskin.}
    #TRIGGER {^You slowly come out of your rage} {#echo SPELL DROP: Frenzy.}
    #TRIGGER {^The protective aura fades from around your body.} {#echo SPELL DROP: Sanctuary.}
    #TRIGGER {^Your force shield shimmers then fades away.} {#echo SPELL DROP: Foci.}
    #TRIGGER {^You no longer feel invincible!} {#echo SPELL DROP: invincible.}
    #TRIGGER {^The safety of shade disappears.} {#echo SPELL DROP: Dark Embrace.}
    #CLASS 0

## Designer comments

All the credits for this trigger set go to amezyarak.

Originally emoted to the mud when spells had dropped changed by me to
this method when the no trigger rule was in affect. Other spells could
be added to this trigger set if needed. Works well with the [Zmud Tell
Window](Zmud_Tell_Window "wikilink") script.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
