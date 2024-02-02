This script will utilize a prompt formatted in the following manner:

    <1216/1216hp 2576/2576ma 1229v 41> 0 lag - - surge off

Where the two '-' symbols are monitor target's hp and hp as a % of max,
respectively. Use the following Prompt script in-game:  
*Note: There has to be a space after the word "lag" in the first prompt,
or it won't space the prompt2 properly.*

prompt \<\|w\|%h\|n\|/%Hhp \|w\|%m\|n\|/%Mma %vv \|y\|%T\|n\|\> %s lag  
prompt2 %w %P surge %S %n  

This script also creates three gauges at the top of your screen (or
wherever you choose to place them) showing your current hit points, mana
points, and the condition of the mob you are fighting. (This condition
is an estimate based upon damage statements.)  
![](CMud_Gauges.jpg "CMud_Gauges.jpg")

## The Script

Save the following code as an .xml file, and import it into Cmud:


    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="Prompt">
        <trigger priority="2180">
          <pattern>~&lt;(%d)/(%d)hp (%d)/(%d)ma (%d)v (%d)&gt; (%d) lag * * surge *</pattern>
          <value>#var currhp %1
    #var maxhp %2
    #var currmana %3
    #var maxmana %4
    #var currmoves %5
    #var currtnl %6
    #var currlag %7
    #if (@currtnl &lt; 200) {#var message %ansi(high,yellow,blink)"Put on Level Gear Now" %ansi(default)}</value>
        </trigger>
        <stat name="StatBar" priority="2190"/>
        <button name="hp" type="Gauge" autosize="false" width="120" height="23" inset="true" toolstyle="true" color="#0080FF" gaugelowcol="red" gaugebackcol="silver" priority="19">
          <caption>Hit Points</caption>
          <value>@currhp</value>
          <expr>@currhp</expr>
          <gaugemax>@maxhp</gaugemax>
          <gaugelow>@maxhp/4</gaugelow>
        </button>
        <button name="mp" type="Gauge" autosize="false" width="120" height="23" autopos="false" left="121" inset="true" toolstyle="true" color="lime" gaugelowcol="red" gaugebackcol="silver" priority="25">
          <caption>Mana Points</caption>
          <value>@currmana</value>
          <expr>@currmana</expr>
          <gaugemax>@maxmana</gaugemax>
          <gaugelow>@maxmana/4</gaugelow>
        </button>
        <var name="currtnl">560</var>
        <var name="currhp">650</var>
        <var name="maxhp">650</var>
        <var name="currmana">2137</var>
        <var name="maxmana">2137</var>
        <var name="currmoves">791</var>
        <var name="currlag">0</var>
        <alias name="setprompt">
          <value>#verbatim
    prompt &lt;|w|%h|n|/%Hhp |w|%m|n|/%Mma %vv |y|%T|n|&gt; %s lag 
    prompt2 %w %P surge %S %n
    prompt2
    #verbatim</value>
        </alias>
        <button name="mobcondition" type="Gauge" autosize="false" width="120" height="23" autopos="false" left="241" inset="true" toolstyle="true" color="#FF8000" gaugelowcol="red" gaugebackcol="silver" priority="29">
          <caption>@mobname</caption>
          <value>@mobcond</value>
          <expr>@mobcond</expr>
          <gaugemax>@mobmax</gaugemax>
          <gaugelow>@mobmax/3</gaugelow>
        </button>
      </class>

      <class name="MobCondition">
        <trigger priority="4830">
          <pattern>(*) is in excellent condition.</pattern>
          <value>#var mobname %1
    #var mobcond 100</value>
        </trigger>
        <trigger priority="4840">
          <pattern>(*) has a few scratches.</pattern>
          <value>#var mobname %1
    #var mobcond 90</value>
        </trigger>
        <trigger priority="4850">
          <pattern>(*) has some small wounds and bruises.</pattern>
          <value>#var mobname %1
    #var mobcond 80</value>
        </trigger>
        <trigger priority="4860">
          <pattern>(*) has quite a few wounds.</pattern>
          <value>#var mobname %1
    #var mobcond 70</value>
        </trigger>
        <trigger priority="4870">
          <pattern>(*) has some big nasty wounds and scratches.</pattern>
          <value>#var mobname %1
    #var mobcond 50</value>
        </trigger>
        <trigger priority="4880">
          <pattern>(*) looks pretty hurt.</pattern>
          <value>#var mobname %1
    #var mobcond 30</value>
        </trigger>
        <trigger priority="4890">
          <pattern>(*) is in awful condition.</pattern>
          <value>#var mobname %1
    #var mobcond 20</value>
        </trigger>
        <var name="mobname" type="Literal">No mob</var>
        <var name="mobcond">100</var>
        <var name="mobmax">100</var>
      </class>

    </cmud>

## Notes

Used in conjunction with the [zAffects](CMud_ZAffects "wikilink")
script, this will populate all of this information into a Status Window.
I place mine in the bottom right hand corner of my CMud window, where it
is convenient, but non-intrusive.  

## Designer comments

Feel free to note me [here](User:Shalineth "wikilink") or on board 2 to
Shalineth with any feedback or suggestions.

Updated for CMud v3.32.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
