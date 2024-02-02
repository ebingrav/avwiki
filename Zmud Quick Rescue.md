This is the Quick rescue set of triggers from the amezyarak site, added
as the site is now defunct.

Type Group and it will add all groupies to a list. When a groupie gets
hit a message will appear saying they need need rescuing nand you press
F12 to rescue them.

Limitations:

-   It assumes whoever you are following is the tank and will not
    highlight him for rescuing, a problem when someone is holding the
    group or for lord groups.
-   When you press F12 it will rescue whoever got hit last, so if the
    5000hp min bzk got hit last you will rescue them rather than the
    500hp spr sor.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {quickrescue}
    #VAR groupmem {}
    #TRIGGER {(%*) attack[s ]strike[s ]({@groupmem})} {
      #if %ismember( %2, %char|@tank) {#no} {
        #var quick_rescue %2
        #echo %2 needs a rescue! ~<F-12> to rescue.
        }
      }
    #TRIGGER {(%*) attacks haven't hurt ({@groupmem})} {
      #if %ismember( %2, %char|@tank) {#no} {
        #var quick_rescue %2
        #echo %2 needs a rescue! ~<F-12> to rescue.
        }
      }
    #TRIGGER {You join (%w)'s group.} {#var tank %1}
    #TRIGGER {'s group:} {#var groupmem ""}
    #TRIGGER {[[ ](%d) (%w)[ ~]] (%w)(%s)(%d)[/ ](%d) hp(%s)(%d)[/ ](%d) mana(%s)(%d)[/ ](%d) mv} {#var groupmem %additem( %lower( %3), @groupmem)}
    #TRIGGER {[[ ](%d) (%w)](%w)(%s)(%w)(%s)(%d)/(%d)(%s)(%d)/(%d)(%s)(%d)/(%d)} {#var groupmem %additem( %3, @groupmem)}
    #TRIGGER {(%d)[| ](%d)(%s)(%w)(%s)(%w)(%s)(%w)(%s)(%d)/(%d)(%s)(%d)/(%d)(%s)(%d)/(%d)(%s)(%d) } {#var groupmem %additem( %lower( %6), @groupmem)}
    #KEY F12 {rescue @quick_rescue}
    #CLASS 0

## Designer comments

All the credits for this trigger set go to amezyarak.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
