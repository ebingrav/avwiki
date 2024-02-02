This is a script for stabbing at lord.

Usage:

Using the right click menu add what mobs you are stabbing, which
number(s) and the option to reverse the numbers.

Then click on the button or press F12 to activate the Auto stab trigger.
When the Auto stab trigger is on you will try to stab whatever list of
mobs you have added to the target list, for whatever number you have
added, whenever you follow someone into a room.

An option for adding custom targets is included in the Right click menus
for new runs or runs that are not in the menu system, to add multiple
targets this way use \| to separate eg mob1\|mob2\|mob3, using this
method adds to the current target list and does not replace it.

There is also an option to add New Target Lists to the Database or
Update Target Lists.

F10 will stab manually, useful when your full and group is resting in a
non safe area to catch aggies.

## Code

*Copy this section and paste it into a \*.xml file. Then import it into
cMUD*

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
    <module name="new stab trigger" global="true">
      <uid>{DC8B893F-1ED5-4FCB-9B62-3B12A6F00D80}</uid>
      <var name="NewTargList">test|test|test</var>
      <var name="TargetListDB" type="Record" sorted="1">
        <value>Arcadia, Chimeras and Baleflames="chim|bale"|Astral, Mem Lane="gith|soul|memory|pain|death|tiam|fae|shadow|queen|giant"|Earth="elem|wyrm|merc|slave|serv"|Fae=fae|Fire, Cinderheim="imp|flame|sala|smoke|tar|fire|lava|dweller|wind|spirit|giant|body"|Fire, Proper="elem|fire|knight|ember|troll|lava|mephit|sala"|Karn, Proper="akuma|man|cutthroat|crazed|humanoid|scourge|merm|reaper"|Kzin, Jungle="pan|ana|sus|yor|kzi|ant"|Kzin, Volcano="pyro|darken|mephit|salam|lava|fire"|Outland, Gith="gish|gith|female"|Rohp="delus|phant|pred|obli|denog|dog"|Tarterus, Garden="demon|statue"|Water="water|wyrm"</value>
        <json>{"Arcadia, Chimeras and Baleflames":["chim","bale"],"Astral, Mem Lane":["gith","soul","memory","pain","death","tiam","fae","shadow","queen","giant"],"Earth":["elem","wyrm","merc","slave","serv"],"Fae":"fae","Fire, Cinderheim":["imp","flame","sala","smoke","tar","fire","lava","dweller","wind","spirit","giant","body"],"Fire, Proper":["elem","fire","knight","ember","troll","lava","mephit","sala"],"Karn, Proper":["akuma","man","cutthroat","crazed","humanoid","scourge","merm","reaper"],"Kzin, Jungle":["pan","ana","sus","yor","kzi","ant"],"Kzin, Volcano":["pyro","darken","mephit","salam","lava","fire"],"Outland, Gith":["gish","gith","female"],"Rohp":["delus","phant","pred","obli","denog","dog"],"Tarterus, Garden":["demon","statue"],"Water":["water","wyrm"]}</json>
      </var>
      <var name="NewTargetKey">test2</var>
      <menu priority="3450">
        <caption>Add New Stab List</caption>
        <value>#var NewTargetKey ""
    #var NewTargList ""
    #PR NewTargetKey "List Name"
    #if (%iskey(@TargetListDB, @NewTargetKey)) {
    #var NewTargList %db(@TargetListDB,@NewTargetKey)
    }
    #PR NewTargList "Enter targets separated with |"
    #addkey TargetListDB @NewTargetKey @NewTargList
    #showdb @TargetListDB</value>
      </menu>
      <menu priority="3450">
        <caption>Stabbing Targets</caption>
        <value>#var TargetKeys ""
    #var StabTargList ""
    #loopdb @TargetListDB {#additem TargetKeys %key}
    #var TargetKeys %pick( "p:Targets to stab", @TargetKeys)
    #forall @TargetKeys {
      #if (@StabTargList!=%null) {#var StabTargList %concat( @StabTargList, "|")}
      #var StabTargList %concat( @StabTargList, %db( @TargetListDB, %i))
      }
    #var StabNumList %pick( "p:Numbers to Stab", "1", "2", "3")

    #if (%yesno("Do you want the numbers reversing? EG. 3. 2. 1.")) {
    #var tempnumlist @StabNumList
    #var StabNumList ""
    #forall @tempnumlist {#var StabNumList %push(%i, @StabNumList)}
    }
    #show Stabbing Folling numbers in this order: @StabNumList
    #show Stabbing Following Targets: @StabTargList
      
    </value>
      </menu>
      <var name="TargetKeys"/>
      <var name="StabTargList"/>
      <var name="StabNumList"/>
      <var name="tempnumlist"/>
      <menu priority="3450">
        <caption>Add Custom targets</caption>
        <value>#PR CustomTarg "Enter targets separated with |"
    #if (%begins( @customtarg, "|")) {#var CustomTarg %delete( @customtarg, 1, 1)}
    #if (%ends( @customtarg, "|")) {#var CustomTarg %delete( @customtarg, %len( @customtarg), 1)}
     #if ((@StabTargList!=%null)AND(@CustomTarg!=%null)) {#var StabTargList %concat( @StabTargList, "|")}
     #var StabTargList %concat( @StabTargList, @CustomTarg)</value>
      </menu>
      <var name="CustomTarg"/>
      <macro key="F12" send="false">
        <value>#bu stabbut</value>
      </macro>
      <macro key="F10" send="false">
        <value>stabbing</value>
      </macro>
      <alias name="stabbing" autoappend="true">
        <value>#if (@StabNumList==%null) {stab} {#if (%numitems( @StabNumlist)==1) {stab @StabNumList.} {#for @StabNumList {stab %i.}}}</value>
      </alias>
      <alias name="stab" autoappend="true">
        <value>#for @StabTargList {#sendraw assas %1%i}</value>
      </alias>
      <class name="stabtrig" initdisable="true">
        <trigger priority="3040">
          <pattern>You follow</pattern>
          <value>stabbing</value>
        </trigger>
      </class>
      <button name="stabbut" type="Toggle" autosize="false" width="80" height="20" color="navy" textcolor="white" priority="12">
        <caption>Stab-Off</caption>
        <value>#T+ stabtrig</value>
        <button color="navy" textcolor="white">
          <caption>Stab-On</caption>
          <value>#t- stabtrig</value>
        </button>
      </button>
    </module>
    </cmud>

## Designer comments

Reversing the numbers why? Well say there are 2 stabbers, you and
someone that stabs harder than you, you select "1.", "2." Reversing the
numbers means you will stab 2. first then 1. This gives the harder
stabber a chance at hitting first and doing more damage also when there
are 2 mobs in a room they do not always have the same keyword so "2.mob"
would miss the second mob.

I used Right Click Menus as I did not want buttons clogging up the bar
however you may set this up with buttons if you wish.

Only thing that is missing is a way to delete target lists from the
database at the moment. I don't have the targets for all the specific
runs so they would need adding.

Its VERY SPAMMY to the user on certain runs due to a) not finding
multiple targets and b) when the mob is already fighting so the use of
\#gag might be helpful for the corresponding lines. The use of \#sendraw
stops the echo spam of the assassinate loop however.

Also if people have names that match the stab target you will try to
stab the player rather than the mob, eg: imp for cinderheim would stab
anyone whose name begins with imp.

(Credits to Tabion one night over chat for giving me the ideas for this
method compared to my Zmud trigger).

[Category:Cmud Scripting](Category:Cmud_Scripting "wikilink")
