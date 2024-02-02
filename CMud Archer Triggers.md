There are a couple useful parts to this script.

The first part is an auto-[fletch](Fletch "wikilink") trigger. Type
**autofletch** to scroll through the options to automatically create
hero or lord level [arrows](:Category:Arrows "wikilink") or
[bolts](:Category:Bolts "wikilink"). You can specify type (including
poison) and quantity you wish to make. The script will continue
fletching until you make or exceed your goal.

The second part is an [autoheld](Held_Shot "wikilink") trigger. Type
**autoheld** to turn the feature on or off. An echo will let you know
your autoheld status.

The third part is a [longshot](Longshot "wikilink") script. <u>This is
**not** an automatic feature</u>. When a mob flees, it will create a
hotkey with what it thinks is the best chance at a longshot and echo it.
Hit the **F8** key to fire the longshot. If the script is not right,
don't fire it. This does not violate the longshot trigger policy, as it
does not automatically fire longshot, the user has to make the conscious
decision to fire.  
<img src="Longshot.jpg" title=" | |Longshot Echo Example" width="600"
alt=" | |Longshot Echo Example" />

This script will automatically pick up any arrows and bolts on the floor
after a fight. Be sure to have [Autopull](Autopull "wikilink") turned
on!

I use this ArcherClass script in conjunction with my
[AmmoCheck](CMud_Ammocheck "wikilink") script to keep my ammo counts up
to date.

## The Script

Save the following code as an .xml file, and import it into Cmud:  

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="ArcherClass">
        <alias name="autoheld">
          <value>#if (@autoheld = "On") {#var autoheld "Off"} {#var autoheld "On"}
    #echo %concat("-- Autoheld ",@autoheld," --")</value>
        </alias>
        <trigger priority="11440">
          <pattern>^Your shot hits</pattern>
          <value>#if (@autoheld = "On") {held}</value>
        </trigger>
        <var name="autoheld" type="Literal">Off</var>
        <trigger priority="11480">
          <pattern>^A moment's distraction and your shot is ruined!</pattern>
          <value>#if (@autoheld = "On") {held}</value>
        </trigger>
        <trigger priority="11490">
          <pattern>^Critical hit!</pattern>
          <value>#if (@autoheld = "On") {held}</value>
        </trigger>
        <trigger priority="11500">
          <pattern>^You fire at * and miss!</pattern>
          <value>get all.arrow</value>
        </trigger>
        <trigger priority="11520">
          <pattern>^You receive (%d) experience points.</pattern>
          <value>get all.arrow
    get all.bolt
    ammocheck</value>
        </trigger>
        <trigger priority="11550">
          <pattern>(*) has fled (%w)</pattern>
          <value><![CDATA[#var lstarget %1
    #var lsdir %2
    #IF (%ismember(%word(@lsTarget,1),"A|a|An|an|The|the")) {#var lstarget %word(@lstarget,2)} {#var lstarget %word(@lstarget,1)}
    #KEY F8 {ls @lsdir @lstarget}
    #echo >>---> Hit F8 to Longshot @lsdir @lstarget <---<<]]></value>
        </trigger>
        <alias name="autofletch">
          <value><![CDATA[#var ammoclass %pick("p:Select one:","o:1","Arrow","Bolt")
    #var ammotype %pick("p:Select type:","o:1","Standard","Steel","Barbed","Poison","Flaming","Piercing","Splinter","Explosive","Sableroix","Mithril (Lord level):Mithril","Lightning (Lord level):Lightning","Ice (Lord level):Ice","Ebony (Lord Level): Ebony")
    #prompt ammogoal "How many are you looking to make?"
    #if (@ammotype="Poison") {#prompt ammopoison "Enter the keyword for the poison type"}
    #echo >>---> Fletching @ammotype %concat(@ammoclass,"s") <---<<
    #if (@ammotype="Poison") {fletch @ammoclass poison @ammopoison} {fletch @ammoclass @ammotype}]]></value>
        </alias>
        <trigger priority="11570">
          <pattern>Your efforts produced (%d) (%w) ({bolts|arrows})</pattern>
          <value>#math ammofletched (@ammofletched+%1)
    #if (@ammofletched&gt;=@ammogoal) {#wait 29000;#echo You have fletched @ammofletched @ammotype %concat(@ammoclass,"s");autofletchclear} {#if (@ammotype="Poison") {fletch @ammoclass poison @ammopoison} {fletch @ammoclass @ammotype}}
    </value>
        </trigger>
        <var name="ammotype"/>
        <var name="ammofletched">31</var>
        <var name="ammoclass"/>
        <var name="ammopoison"/>
        <var name="ammogoal">20</var>
        <alias name="autofletchclear">
          <value>#var ammoclass ""
    #var ammofletched ""
    #var ammogoal ""
    #var ammopoison ""
    #var ammotype ""</value>
        </alias>
        <var name="lstarget">peddler</var>
        <trigger priority="11690">
          <pattern>You fire a long shot *!</pattern>
          <value>#unkey F8</value>
        </trigger>
        <trigger priority="11710">
          <pattern>* tells the group 'ls (%w) (%w)'</pattern>
          <value><![CDATA[#var lstarget %2
    #var lsdir %1
    #KEY F8 {ls @lsdir @lstarget}
    #echo >>---> Hit F8 to Longshot @lsdir @lstarget <---<<]]></value>
        </trigger>
        <var name="lsdir">south</var>
      </class>
    </cmud>

## Notes

You may want to create a trigger that matches on your archer's name on
login to enable the ArcherClass class, as well as disable it on logout.

This script has been updated for CMud v 3.32.

## Designer comments

Feel free to note me [here](User:Shalineth "wikilink") or on board 2 to
Shalineth with any feedback or suggestions.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
