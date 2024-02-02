This is the tell capture set of triggers from the amezyarak site, added
as the site is now defunct.

Captures tells and group tells and puts them in a window above the main
mud window. Two other triggers are included that go with the [Zmud
Affects Drop](Zmud_Affects_Drop "wikilink") and the [Zmud Level
Gear](Zmud_Level_Gear "wikilink") set of triggers.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {tell}
    #TRIGGER {^(%w) tells you} {#cap tell}
    #TRIGGER {^(%w) is asleep, but you tell} {#cap tell}
    #TRIGGER {^You tell (%w) '} {#cap tell}
    #TRIGGER {^You dream of (%*) coming to you and trying to tell you} {#cap tell}
    #TRIGGER {You tell the group} {#cap tell}
    #TRIGGER {SPELL DROP: } {#cap tell}
    #TRIGGER {LEVEL INFO:} {#cap tell}
    #TRIGGER {^*(%w)* tells the group '} {#cap tell}
    #CLASS 0

## Designer comments

All the credits for this trigger set go to amezyarak.

The SPELL DROP and LEVEL INFO triggers, are there for [Zmud Affects
Drop](Zmud_Affects_Drop "wikilink") and [Zmud Level
Gear](Zmud_Level_Gear "wikilink") triggers which \#Echo information and
this places it in the tell capture window too. This was added by me when
the no trigger rule was in affect to avoid outputting to the mud.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
