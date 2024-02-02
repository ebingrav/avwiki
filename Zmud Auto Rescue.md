This is the Auto rescue set of triggers from the amezyarak site, added
as the site is now defunct.

Syntax: addrescue <charname>, adds character to the rescue list.  
Syntax: remrescue <charname>, remove a character from the rescue list.  
Syntax: clearrescue, clears the rescue list.  
Syntax: showrescue, shows the rescue list.  
A right click menu also lets you right click on a char name and it will
either add or remove depending on what you select.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {autorescue}
    #ALIAS addrescue {#ec autorescue - %1;#var rescuelist %additem( %lower( %1), @rescuelist);#ec %null;#ec --- CHARACTERS IN THE RESCUE LIST ---;#ec %null;#fo @rescuelist {#ec %i}}
    #ALIAS remrescue {#ec don't rescue - %1;#var rescuelist %delitem( %lower( %1), @rescuelist);#ec %null;#ec --- CHARACTERS IN THE RESCUE LIST ---;#ec %null;#fo @rescuelist {#ec %i}}
    #ALIAS clearrescue {#ye {clear the rescue list?} {yes:rescuelist="";#ec rescue buffer - cleared} {no:}}
    #ALIAS showrescue {#ec %null;#ec --- CHARACTERS IN THE RESCUE LIST ---;#ec %null;#fo @rescuelist {#ec %i}}
    #VAR rescuelist {test|stest}
    #MENU {Rescue - Add} {addrescue %selword} ""
    #MENU {Rescue - Remove} {remrescue %selword} ""
    #MENU {Rescue - Show} {showrescue} ""
    #MENU {Rescue - Clear} {clearrescue} ""
    #CLASS 0
    #CLASS {autorescue|rescue}
    #TRIGGER {(%*) attacks strike (%w)} {#if %ismember( %lower( %2), @rescuelist) {rescue %2;#t- rescue}}
    #TRIGGER {(%*) attacks haven't hurt (%w)} {#if %ismember( %lower( %2), @rescuelist) {rescue %2;#t- rescue}}
    #TRIGGER {(%*) attack strikes (%w)} {#if %ismember( %lower( %2), @rescuelist) {rescue %2;#t- rescue}}
    #CLASS 0
    #CLASS {autorescue|rescue back on}
    #TRIGGER {^(%*) is DEAD!!} {#T+ rescue}
    #TRIGGER {You successfully rescue} {#T+ rescue}
    #TRIGGER {doesn't need your help.} {#T+ rescue}
    #TRIGGER {You fail to rescue} {#T+ rescue}
    #TRIGGER {doesn't NEED rescuing!} {#T+ rescue}
    #CLASS 0

## Designer comments

All the credits for the original trigger set go to amezyarak.

When archers came in I altered the original trigger line to count for
archer mobs, I failed as it would not rescue people if their name began
with a S, so split it back into two lines. Also the original trigger
would only do one rescue per mob/player killed this was to prevent the
trigger spamming rescue, so I have added a couple more trigger lines to
turn it back on for player already rescued, failed rescue, a successful
rescue and try to rescue when mob is dead.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
