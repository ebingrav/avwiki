This is a script for stabbing at lord.

Usage:

Using the right click menu add what mobs you are stabbing and which
number(s).

Then click on the button or press F12 to activate the Auto stab trigger.
When the Auto stab trigger is on you will try to stab whatever mobs you
have added to the target list, for whatever number you have added,
whenever you follow someone into a room.

An option for custom targets is included in the menus for new runs or
runs that are not in the menu system, to add multiple targets this way
use \| to separate eg mob1\|mob2\|mob3, using this method adds to the
current target list and does not replace it.

F10 will stab manually, useful when your full and group is resting in a
non safe area to catch aggies.

## Code

*Copy this section and paste it into a \*.txt file. Then, in zMUD, go to
"Settings -\> File -\> Import Text" and select the file you saved to:*

    #CLASS {Stabbing}
    #ALIAS stab {#for @StabTargList {assas %1%i}}
    #ALIAS stabbing {#if (@StabNumbList==%null) {stab} {#if (%numitems( @StabNumbList)==1) {stab @StabNumbList.} {#for @StabNumbList {stab %i.}}}}
    #VAR StabTargList {}
    #VAR StabNumbList {}
    #KEY F12 {#bu stabtrigbut}
    #KEY F10 {stabbing}
    #BUTTON 4 {stab-off} {#t+ stabtrig} {Stab-on} {#t- stabtrig} {} {1} {} {Size} {70} {20} {} {} {} {31} {30} {} {} "" {} {} {stabtrigbut}
    #MENU {Stab Targets} {} "" {StabTargMenu}
    #MENU {Stab Number} {} "" {StabNumberMenu}
    #CLASS 0
    #CLASS {Stabbing|StabTargMenu} {menu}
    #VAR CustomTarg {}
    #MENU {Clear Targets} {#var StabTargList %null} ""
    #MENU {Custom targets} {
      #PR CustomTarg "Enter targets separated with |"
      #if (%begins( @customtarg, "|")) {#var CustomTarg %delete( @customtarg, 1, 1)}
      #if (%ends( @customtarg, "|")) {#var CustomTarg %delete( @customtarg, %len( @customtarg), 1)}
      #if ((@StabTargList!=%null)AND(@CustomTarg!=%null)) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, @CustomTarg)
      } ""
    #MENU {gi} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "gish|gith|female")
      } ""
    #MENU {water} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "water|wyrm")
      } ""
    #MENU {Chim} {#var StabTargList %additem( chim, @StabTargList)} ""
    #MENU {bale} {#var StabTargList %additem( bale, @StabTargList)} ""
    #MENU {fae} {#var StabTargList %additem( fae, @StabTargList)} ""
    #MENU {stat} {#var StabTargList %additem( stat, @StabTargList)} ""
    #MENU {dem} {#var StabTargList %additem( dem, @StabTargList)} ""
    #MENU {jungle} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "pan|ana|sus|yor|kzi")
      } ""
    #MENU {volcano} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "pyro|darken|mephit|salam|lava|fire")
      } ""
    #MENU {karn proper} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "akuma|man|cutthroat|crazed|humanoid|scourge|merm|reaper")
      } ""
    #MENU {memlane astral} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "gith|soul|memory|pain|death|tiam|fae|shadow|queen|giant")
      } ""
    #MENU {rohp} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "delus|phant|pred|obli|denog|dog")
      } ""
    #MENU {earth} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "elem|wyrm|merc|slave|serv")
      } ""
    #MENU {fire} {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "elem|fire|knight|ember|troll|lava|mephit|sala")
      } ""
    #MENU {cinderheim} {
      #var StabTargList %additem( sent, @StabTargList)
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, "imp|flame|sala|smoke|tar|fire|lava|dweller|wind|spirit|giant|body")
      } ""
    #CLASS 0
    #CLASS {Stabbing|StabNumberMenu} {menu}
    #MENU {3.} {#var StabNumbList %additem( 3 , @StabNumbList)} ""
    #MENU {2.} {#var StabNumbList %additem( 2 , @StabNumbList)} ""
    #MENU {1.} {#var StabNumbList %additem( 1 , @StabNumbList)} ""
    #MENU {Clear Numbers} {#var StabNumbList %null} ""
    #CLASS 0
    #CLASS {Stabbing|stabtrig}
    #TRIGGER {You follow} {
      stabbing
      }
    #CLASS 0

## Designer comments

The menu system for mob targets is a bit jumbled and could be tidied up
for all the specific runs (Gear or XP) also I don't have the targets for
all the specific runs so they would need adding. Its VERY SPAMMY to the
user so the use of \#gag might be helpful.

Also if people have names that match the stab target you will try to
stab the player rather than the mob, eg: imp for cinderheim would stab
anyone whose name begins with imp.

There is a cleaner way of doing this using database record variables and
%pick (Credits to Tabion one night over chat for explaining this method)
which I have used for my cMUD trigger set.

[Category:Zmud Scripting](Category:Zmud_Scripting "wikilink")
