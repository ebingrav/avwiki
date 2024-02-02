This is the color set of triggers from the amezyarak site, added as the
site is now defunct. Highlights several things to add a bit more color
to your mudding experience.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {color}
    #ALIAS low_stat_checker {
      #if (@hps_percent<50) {#temp { @hps_string} {#cw 14}}
      #if (@mana_percent<50) {#temp { @mana_string} {#cw 14}}
      #if (@moves_percent<50) {#temp { @moves_string} {#cw 14}}
      #if (@hps_percent<20) {#temp { @hps_string} {#cw 12}}
      #if (@mana_percent<20) {#temp { @mana_string} {#cw 12}}
      #if (@moves_percent<20) {#temp { @moves_string} {#cw 12}}
      }

    #TRIGGER {~(Black Aura~)} {#cw 8}
    #TRIGGER {~(flying~)} {#cw 11}
    #TRIGGER {~(Glowing~)} {#cw 12}
    #TRIGGER {~(Green Aura~)} {#cw 10}
    #TRIGGER {~(hide~)} {#cw 8}
    #TRIGGER {~(humming~)} {#cw 9}
    #TRIGGER {~(pink aura~)} {#cw 13}
    #TRIGGER {~(invis~)} {#cw 8}
    #TRIGGER {~(sneak~)} {#cw 8}
    #TRIGGER {~(white aura~)} {#cw 15}
    #TRIGGER {Pristine} {#cw 15}
    #TRIGGER {~(magical~)} {#cw 13}
    #TRIGGER {~(demonic~)} {#cw 8}
    #TRIGGER {~(sharp~)} {#cw 15}
    #TRIGGER {^Darii deems you worthy, and protects you} {#hi}
    #TRIGGER {^Darii surrounds} {#hi}
    #TRIGGER {~(translucent~)} {#cw 3}
    #TRIGGER {^You are surrounded by a white aura} {#cw 15}
    #TRIGGER {white aura} {#cw 15}
    #TRIGGER {^You are filled with rage.} {#cw 12}
    #TRIGGER {arrives from} {#hi}
    #TRIGGER {screams and attacks!} {#co 12}
    #TRIGGER {^Tul-Sith deems you worthy, and protects you!} {#cw 15}
    #TRIGGER {Spell: '([a-z ])'(%*)for ({0|1|2} hours).} {
      #TEMP {%1} {#cw 524}
      #TEMP {%3} {#cw 524}
      }
    #TRIGGER {Your attack[s ]strike[s ](%*) (%d) time[s, ]with[* ](%w)[* ](%w)[!.]} {
      #TEMP {%2} {#cw 14}
      #TEMP {%3} {#cw 14}
      }
    #TRIGGER {(%d)[| ](%d)(%s)(%w)(%s)(%w)(%s)(%w)(%s)(%d)/(%d)(%s)(%d)/(%d)(%s)(%d)/(%d)(%s)(%d) } {
      #var hps_string %10
      #math hps_percent (%10*100)/%11
      #var mana_string %13
      #math mana_percent (%13*100)/%14
      #var moves_string %16
      #math moves_percent (%16*100)/%17
      low_stat_checker
      }
    #CLASS 0

## Designer comments

All the credits for this trigger set go to Amezyarak, some of the
triggers might be defunct now as the mud might do this for you.
Currently on updating this for Cmud as the \#temp \#cw method does not
work in Cmud therefore working it all out with \#pcol.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
