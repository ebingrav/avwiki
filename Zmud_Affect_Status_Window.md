This is the affect window set of triggers from the amezyarak site, added
as the site is now defunct. When you type affect it grabs the duration
and will display them in the status window.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {AffectStatWind}
    #ALIAS affreset {
      #var sanc "-"
      #var fren "-"
      #var foci "-"
      #var fort "-"
      #var awen "-"
      #var invinc "-"
      #var steel "-"
      #var iron "-"
      #var conc "-"
      #var bark "-"
      #var sneak "-"
      #var mvhid "-"
      #var water "-"
      #var endur "-"
      #var faerie "-"
      #var prayer "-"
      #var prayermod "Nothing"
      #var prayertype "None"
      }
    #VAR sanc {-}
    #VAR fren {-}
    #VAR foci {-}
    #VAR fort {-}
    #VAR awen {-}
    #VAR invinc {-}
    #VAR steel {-}
    #VAR iron {-}
    #VAR conc {-}
    #VAR bark {-}
    #VAR sneak {-}
    #VAR mvhid {-}
    #VAR water {-}
    #VAR endur {-}
    #VAR faerie {-}
    #VAR prayer {-}
    #VAR prayermod {Nothing}
    #VAR prayertype {None}
    #TRIGGER {Spell: 'fortitudes'  for (%d) hours.} {#cw 13;#var fort %1}
    #TRIGGER {Spell: 'frenzy'  modifies damage roll by (%d) for (%d) hours.} {#co 12;#var fren %2}
    #TRIGGER {Spell: 'sanctuary'  for (%d) hours.} {#cw 15;#var sanc %1}
    #TRIGGER {Spell: 'invincibility'  modifies armor class by -(%d) for (%d) hours.} {#cw 15;#var invinc %2}
    #TRIGGER {Spell: 'iron skin'  modifies armor class by -(%d) for (%d) hours.} {#co 7;#var iron %2}
    #TRIGGER {Spell: 'steel skeleton'  modifies armor class by -(%d) for (%d) hours.} {#co 13;#var steel %2}
    #TRIGGER {Spell: 'foci'  for (%x) hours.} {#cw 9;#var foci %1}
    #TRIGGER {Spell: 'barkskin'  modifies armor class by -(%d) for (%d) hours.} {#var bark %2;#cw 6}
    #TRIGGER {Spell: 'concentrate'  modifies armor class by -(%d) for (%d) hours.} {#var conc %2}
    #TRIGGER {Spell: 'sneak'  for (%d) hours.} {#var sneak %1}
    #TRIGGER {Spell: 'shadow form'  for (%d) hours.} {#var sneak %1}
    #TRIGGER {Spell: 'move hidden'  for (%d) hours.} {#var mvhid %1}
    #TRIGGER {Spell: 'faerie fire'  modifies armor class by (%d) for (%d) hours.} {#cw 13;#var faerie %2}
    #TRIGGER {Spell: 'awen'  for (%d) hours.} {#cw 7;#var awen %1}
    #TRIGGER {Spell: 'water breathing'  for (%d) hours.} {#var water %1}
    #TRIGGER {You are affected by:} {affreset}
    #TRIGGER {^Spell: 'endurance'  modifies moves by (%d) for (%d) hours.} {#var endur %2;#cw 10}
    #TRIGGER {^You are not under the affects of any spells or skills.} {affreset}
    #TRIGGER {Spell: 'prayer'  modifies (%*) by (%n) for (%d) hours.} {#var prayer %3;#var prayermod %2;#var prayertype {%1}}
    #TRIGGER {Spell: 'prayer'  for (%d) hours.} {#var prayer %1}
    #STW {awen @awen%{cr}%ansi( 13)forti @fort%{cr}%ansi( 9)foci @foci%{cr}%ansi( 15)invinc @invinc%{cr}%ansi( 8)iron @iron%{cr}%ansi( 5)steel @steel%{cr}%ansi( 11)conc @conc%{cr}%ansi( 6)bark @bark%{cr}%ansi( 1)water @water %cr%{cr}%ansi( 13)prayer @prayer%{cr}@prayermod - @prayertype%{cr}%ansi( 15)sanc @sanc%{cr}%ansi( 12)frenzy @fren%{cr}%ansi( 8)mv hid @mvhid%{cr}sneak @sneak%{cr}%ansi( 10)endur @endur%{cr}%ansi( 13)faerie @faerie%{cr}#st ""} "" "1"
    #CLASS 0

## Designer comments

All the credits for this trigger set go to Amezyarak been tweaked a
little for import.

I personally do not use this trigger set as for some reason my status
window does not display anymore, also if it doesnt display for you see
if Windows-\>Status is checked.

Look at the [Discussion
Page](Talk:Zmud_Affect_Status_Window.md "wikilink") for other things
that could be added to the script.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
