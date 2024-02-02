This is the Affect List color set of triggers from the amezyarak site,
added as the site is now defunct.

Type affect then afflist and it will tell you which important spells you
are missing needs updating for aegis and protection good/evil and the
like for separate awen.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {AffectList}
    #ALIAS afflist {emote is missing: @afflist}
    #VAR afflist {|bb|foci, |bp|fortitudes, |w|awen, |bk|iron skin, |p|steel skeleton, |bw|invincibility, |y|barkskin, |bc|concentrate, |bw|sanctuary, |br|frenzy, |bg|endurance, |b|water breathing}
    #TRIGGER {^Spell: 'fortitudes'} {#var afflist %remove( "|bp|fortitudes, ", @afflist)}
    #TRIGGER {^Spell: 'foci'} {#var afflist %remove( "foci, ", @afflist)}
    #TRIGGER {^Spell: 'awen'} {#var afflist %remove( "|w|awen, ", @afflist)}
    #TRIGGER {^Spell: 'invincibility'} {#var afflist %remove( "|bw|invincibility, ", @afflist)}
    #TRIGGER {^Spell: 'steel skeleton'} {#var afflist %remove( "|p|steel skeleton, ", @afflist)}
    #TRIGGER {^Spell: 'iron skin'} {#var afflist %remove( "|bk|iron skin, ", @afflist)}
    #TRIGGER {^Spell: 'barkskin'} {#var afflist %remove( "|y|barkskin, ", @afflist)}
    #TRIGGER {^Spell: 'concentrate'} {#var afflist %remove( "|bc|concentrate, ", @afflist)}
    #TRIGGER {^Spell: 'sanctuary'} {#var afflist %remove( "|bw|sanctuary, ", @afflist)}
    #TRIGGER {^Spell: 'frenzy'} {#var afflist %remove( "|br|frenzy, ", @afflist)}
    #TRIGGER {^You are affected by:} {#var afflist "|bb|foci, |bp|fortitudes, |w|awen, |bk|iron skin, |p|steel skeleton, |bw|invincibility, |y|barkskin, |bc|concentrate, |bw|sanctuary, |br|frenzy, |bg|endurance, |b|water breathing"}
    #TRIGGER {^Spell: 'water breathing'} {#var afflist %remove( "|b|water breathing", @afflist)}
    #TRIGGER {^Spell: 'endurance'} {#var afflist %remove( "|bg|endurance, ", @afflist)}
    #TRIGGER {^You are not under the affects of any spells or skills.} {#var afflist "|bb|foci, |bp|fortitudes, |w|awen, |bk|iron skin, |p|steel skeleton, |bw|invincibility, |y|barkskin, |bc|concentrate, |bw|sanctuary, |br|frenzy, |bg|endurance, |b|water breathing"}
    #CLASS 0

## Designer comments

All the credits for this trigger set go to amezyarak.

Like I said previously it needs updating for Aegis and for people that
use separate awen.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
