This is a basic spellbot script with a manually-updated "banned" list.
It has options for full macros, split awen, divinity with augment
levels, portals with a check to see if there are existing portals, and
sanctuary. There is a trigger to keep heighten up. The script will check
for a reasonable amount of mana before proceeding with a spellup.

Use the "spellban name" alias to add someone to the "banned" list. (Be
sure to read the Trigger Using
[Policy](Trigger-Using_Policy.md "wikilink") about this.)

*Important Note:* I have another script that captures my current mana
from my prompt and places it in @currmana . This is a very important
step that I intentionally left out of here because everyone configures
their prompt differently. If you use my prompt script here: [CMud Prompt
& Mob Cond](CMud_Prompt_%26_Mob_Cond.md "wikilink") script, it should
work fine. (Thanks to [Scrape](User:Scrape.md "wikilink") for pointing
this out.)

## The Script

Save the following code as an .xml file, and import it into Cmud:  

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="Spellbot">
        <var name="spelltarget"/>
        <var name="spelllist"/>
        <var name="bannedlist" type="StringList"/>
        <var name="spellcount"/>
        <trigger priority="1000">
          <pattern>^(%w) tells you ~'full~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {fullspell %1}</value>
        </trigger>
        <trigger priority="1000">
          <pattern>^(%w) tells you ~'split~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {split %1}</value>
        </trigger>
        <trigger priority="11120">
          <pattern>^(%w) tells you ~'sanc~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {sanct %1}</value>
        </trigger>
        <trigger priority="11130">
          <pattern>^(%w) tells you ~'pp (*)~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {wake;#TEMP {You look at a portal here...} {c seal portal};look portal;{say Portal to %2!;c portal %2;sleep;#UNTRIGGER {You look at a portal here...}}</value>
        </trigger>
        <trigger priority="11140">
          <pattern>^You dream of (%w) telling you ~'div~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {#if (@currmana&lt;100) {tell @spelltarget I don't have enough mana right now. Please wait a few minutes and request again.} {wake;c div %1;sleep}}</value>
        </trigger>
        <trigger priority="11150">
          <pattern>^You dream of (%w) telling you ~'div(%d)~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {#if (@currmana&lt;300) {tell @spelltarget I don't have enough mana right now. Please wait a few minutes and request again.} {wake;augment %2;c div %1;augment off;sleep}}</value>
        </trigger>
        <trigger priority="11180">
          <pattern>^(%w) tells you ~'rc~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {c 'remove curse' %1}</value>
        </trigger>
        <trigger priority="11190">
          <pattern>^You dream of (%w) telling you ~'help~'</pattern>
          <value>#IF (%ismember(%1, @bannedlist)) {tell %1 "You are on the banned list. Send me a note on board 2 to appeal. Sorry."} {tell %1 "Tell me the following: full, split, pp target (for portal), div|div2|div3|div4|div5 (for divinity),rc (remove curse)."}</value>
        </trigger>
        <trigger priority="11200">
          <pattern>^Your senses return to normal</pattern>
          <value>wake
    heighten
    sleep</value>
        </trigger>
        <trigger priority="11210">
          <pattern>^You fail to heighten your senses.</pattern>
          <value>wake
    heighten
    sleep</value>
        </trigger>
        <alias name="split">
          <value>#if (@currmana&lt;1300) {tell @spelltarget I don't have enough mana right now. Please wait a few minutes and request again.} {tell @spelltarget I am now going to be giving you spells with split Awen. Sanctuary is the last spell I will cast. Check your affects at the end, I'm not perfect!;wake;c 'water breathing' @spelltarget;c 'holy sight' @spelltarget;c armor @spelltarget;c 'holy armor' @spelltarget;c 'holy aura' @spelltarget;c bless @spelltarget;c invinc @spelltarget;c barkskin @spelltarget;c 'iron skin' @spelltarget;c 'steel skeleton' @spelltarget;c foci @spelltarget;c fort @spelltarget;c sanc %1;tell @spelltarget I'm done. Be sure to cast Concentrate and Protection spells on yourself.;sleep}</value>
        </alias>
        <alias name="fullspell">
          <value>#if (@currmana&lt;1500) {tell @spelltarget I don't have enough mana right now. Please wait a few minutes and request again.} {tell @spelltarget I am now going to be giving you full spells. Awen is the last spell I will cast. Check your affects at the end, I'm not perfect!;wake;c 'water breathing' @spelltarget;c 'holy sight' @spelltarget;c invinc @spelltarget;c barkskin @spelltarget;c 'iron skin' @spelltarget;c 'steel skeleton' @spelltarget;c foci @spelltarget;c fort @spelltarget;c awen %1;tell @spelltarget Spells complete. Be sure to cast Concentrate on yourself.;sleep}</value>
        </alias>
        <alias name="sanct">
          <value>#if (@currmana&lt;50) {tell @spelltarget I don't have enough mana right now. Please wait a moment and request again.} {wake;c sanct @spelltarget;sleep}</value>
        </alias>
        <trigger priority="11190">
          <pattern>^(%w) tells you ~'help~'</pattern>
          <value>#IF (%ismember(%1, @bannedlist)) {tell %1 "You are on the banned list. Send me a note on board 2 to appeal. Sorry."} {tell %1 "Tell me the following: full, split, pp target (for portal), div|div2|div3|div4|div5 (for divinity),rc (remove curse)."}</value>
        </trigger>
        <trigger priority="11150">
          <pattern>^(%w) tells you ~'div(%d)~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {#if (@currmana&lt;300) {tell @spelltarget I don't have enough mana right now. Please wait a few minutes and request again.} {wake;augment %2;c div %1;augment off;sleep}}</value>
        </trigger>
        <trigger priority="11140">
          <pattern>^(%w) tells you ~'div~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {#if (@currmana&lt;100) {tell @spelltarget I don't have enough mana right now. Please wait a few minutes and request again.} {wake;c div %1;sleep}}</value>
        </trigger>
        <trigger priority="1000">
          <pattern>^You dream of (%w) telling you ~'split~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {split %1}</value>
        </trigger>
        <trigger priority="11120">
          <pattern>^You dream of (%w) telling you ~'sanc~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {sanct %1}</value>
        </trigger>
        <trigger priority="11180">
          <pattern>^You dream of (%w) telling you ~'rc~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {c 'remove curse' %1}</value>
        </trigger>
        <trigger priority="11130">
          <pattern>^You dream of (%w) telling you ~'pp (*)~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {wake;#TEMP {You look at a portal here...} {c seal portal};look portal;{say Portal to %2!;c portal %2;sleep;#UNTRIGGER {You look at a portal here...}}</value>
        </trigger>
        <trigger priority="1000">
          <pattern>^You dream of (%w) telling you ~'full~'</pattern>
          <value>#VAR spelltarget %1
    #IF (%ismember(%1, @bannedlist)) {tell %1 You are on the banned list. Send me a note on board 2 to appeal. Sorry.} {fullspell %1}</value>
        </trigger>
        <alias name="spellban">
          <value>#additem bannedlist %1
    #Echo -- %proper(%1) has been added to the Banned List. --

    // Used to add characters to the banned list. </value>
        </alias>
      </class>
    </cmud>

## Notes

I created a trigger that fires on login and asks if I want to run in
Spellbot mode (using \#YESNO to activate the \#CLASS), and another
trigger that fires on logout and disables this class:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="11370">
        <pattern>^Welcome back to the AVATAR System, ([Hero|Lord]) (%w).</pattern>
        <value>#yesno "Do you wish to run in Spellbot mode?" {#class spellbot 1} {#class spellbot 0}
            </value>
      </trigger>
    </cmud>

## Designer comments

Feel free to note me [here](User:Shalineth.md "wikilink") or on board 2
to Shalineth with any feedback or suggestions.

Updated for CMud v3.32.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
