This trigger has been made a bit obsolete by the Roster command, but has
the additional feature of grouping characters into one of four
categories: Healer, Hitter, Brute or Firepower.

## The Script

Save the following code as an .xml file, and import it into Cmud:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="GroupReport">
        <class name="GroupVars">
          <var name="CntHealer">
            <value>0</value>
            <default>0</default>
          </var>
          <var name="CntBrute">
            <value>0</value>
            <default>0</default>
          </var>
          <var name="CntHitter">
            <value>0</value>
            <default>0</default>
          </var>
          <var name="NmsHitter" type="Literal">
            <default>None</default>
          </var>
          <var name="CntTotal">1</var>
          <var name="groupleader">Athalos</var>
          <var name="CntFirePwr">1</var>
          <var name="NmsBrute" type="Literal"/>
          <var name="NmsFirePwr">, Athalos</var>
          <var name="NmsHealer" type="Literal"/>
        </class>
        <trigger priority="440" case="true">
          <pattern>(%w) the (*) is a (%d)[st|nd|rd|th] level {Hero |Lord |""}(*), was born</pattern>
          <value>#if %ismember(%4, "Rogue|Assassin|Archer|Fusilier|Bladedancer|Black Circle Initiate") {#var NmsHitter %concat(@NmsHitter,", ",%1)}
    #if %ismember(%4, "Rogue|Assassin|Archer|Fusilier|Bladedancer|Black Circle Initiate") {#var CntHitter (@CntHitter + 1)}
    #if %ismember(%4, "Berserker|Warrior|Bodyguard|Paladin|Ranger|Monk|Shadowfist") {#var NmsBrute %concat(@NmsBrute,", ",%1)}
    #if %ismember(%4, "Berserker|Warrior|Bodyguard|Paladin|Ranger|Monk|Shadowfist") {#var CntBrute (@CntBrute + 1)}
    #if %ismember(%4, "Mage|Sorcerer|Wizard|Psionicist|Mindbender") {#var NmsFirePwr %concat(@NmsFirePwr,", ",%1)}
    #if %ismember(%4, "Mage|Sorcerer|Wizard|Psionicist|Mindbender") {#var CntFirePwr (@CntFirePwr + 1)}
    #if %ismember(%4, "Cleric|Priest|Druid") {#var NmsHealer %concat(@NmsHealer,", ",%1)}
    #if %ismember(%4, "Cleric|Priest|Druid") {#var CntHealer (@CntHealer + 1)}
    #var CntTotal (@CntTotal + 1)</value>
        </trigger>
        <trigger priority="450">
          <pattern>(%d)~|(%s)(%d) [Hero|Lord] (%w) (*)  [Stand|Rest|Sleep|Fighting] *</pattern>
          <value>last %4</value>
        </trigger>
        <alias name="GroupRepto">
          <value>#prompt grouprepto "What channel do you want to report to? (gt, say or buddy)"
    #if %ismember(@grouprepto, "chat|nchat|hero|lord|pray|auction|grtz|joke|ask|music|yell|shout|joke|esl|eslchat|quest|answer|ch|cha|nc|nch|ncha|her|lo|lor") {#say Invalid Channel. Sending to public channels is not permitted.}
    #if %ismember(@grouprepto,"gt|say|buddy") {} {#say "You need to specify gt, say or buddy"}
    #if %ismember(@grouprepto,"gt|say|buddy") {} {#exe grouprepto}
    #if %ismember(@grouprepto,"gt|say|buddy") {} {#var grouprepto ""}</value>
        </alias>
        <trigger priority="630">
          <pattern>(%w)'s group:</pattern>
          <value>#var groupleader %1 _nodef GroupVars</value>
        </trigger>
        <var name="grouprepto">
          <value>gt</value>
          <default>GroupRepTo</default>
        </var>
        <trigger priority="5820">
          <pattern>(%d)~|(%d) [Hero|Lord] (%w) (*)  [Stand|Rest|Sleep|Fighting] *</pattern>
          <value>last %3</value>
        </trigger>
      </class>



      <alias name="grouplist">
        <value>#class GroupReport 1
    #exec GroupClear
    #exec GroupRepto
    group</value>
      </alias>
      <alias name="groupclear">
        <value>#var NmsHitter "" _nodef GroupVars
    #var CntHitter 0 _nodef GroupVars
    #var NmsHealer "" _nodef GroupVars
    #var CntHealer 0 _nodef GroupVars
    #var NmsBrute "" _nodef GroupVars
    #var CntBrute 0 _nodef GroupVars
    #var NmsFirePwr "" _nodef GroupVars
    #var CntFirePwr 0 _nodef GroupVars
    #var CntTotal 0 _nodef GroupVars
    #say "Group Counter Cleared"</value>
      </alias>
      <alias name="GroupReport">
        <value>%concat (@grouprepto," The group that ",@groupleader, " is leading has |br|"@cnttotal, " |n|total members: ")
    #if (@CntHitter = 1) {%concat(@grouprepto," |br|",@CntHitter, " hitter: ",%delete(@NmsHitter,1,2))}
    #if (@CntBrute = 1) {%concat(@grouprepto," |bb|",@CntBrute, " brute: ",%delete(@NmsBrute,1,2))}
    #if (@CntFirepwr = 1) {%concat(@grouprepto," |bg|",@CntFirepwr, " mana-flinger: ",%delete(@NmsFirepwr,1,2))}
    #if (@CntHealer = 1) {%concat(@grouprepto," |by|",@CntHealer, " healer: ",%delete(@NmsHealer,1,2))}
    #if (@CntHitter &gt; 1) {%concat(@grouprepto," |br|",@CntHitter, " hitters: ",%delete(@NmsHitter,1,2))}
    #if (@CntBrute &gt; 1) {%concat(@grouprepto," |bb|",@CntBrute, " brutes: ",%delete(@NmsBrute,1,2))}
    #if (@CntFirepwr &gt; 1) {%concat(@grouprepto," |bg|",@CntFirepwr, " mana-flingers: ",%delete(@NmsFirepwr,1,2))}
    #if (@CntHealer &gt; 1) {%concat(@grouprepto," |by|",@CntHealer, " healers: ",%delete(@NmsHealer,1,2))}
    #class GroupReport 0</value>
      </alias>


    </cmud>

## Usage

Running the **grouplist** alias will add all members of your current
group to lists. Also prompts you to input which channel you wish to
output to.

The **groupreport** alias will output to your selected channel (say, gt
or buddy).

The **groupclear** alias will clear the entire list.

## Designer comments

Feel free to note me [here](User:Shalineth.md "wikilink") or on board 2
to Shalineth with any feedback or suggestions.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
