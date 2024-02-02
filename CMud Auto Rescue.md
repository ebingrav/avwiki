Your basic Auto Rescue trigger, it will check if someone is getting hit,
compare their name to your rescue list and rescue if you have the
trigger turned on, which is done by a button at the top of your main
window.

This script has been updated for CMud v3.32.

## How to Use It

**addrescue <name>** - Adds <name> to the rescue list. Can also be
accessed by a drop down menu when the Auto Rescue button is enabled.  
**removerescue <name>** - Removes <name> from the rescue list. Also
available in the drop down.  
**clearrescue** - Clears the rescue list.  
**reportrescue** - Reports the rescue list to grouptell.  
**showrescue** - Reports the rescue list to local echo.  

## The Script

Save the following code as an .xml file, and import it into Cmud:  

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="Rescue">
        
          <value>lollipot|atreyu|renculos|mactabion|seperia|styrr|murugan|druidess|daripper</value>
          <json>["lollipot","atreyu","renculos","mactabion","seperia","styrr","murugan","druidess","daripper"]</json>
        
        <alias name="addrescue">
          <value>#ec adding %1 to autorescue;
            #var rescuelist %additem(%lower(%1),@rescuelist)
            #ec
            #ec ---  CHARACTERS IN THE RESCUE LIST  ---
            #ec
            #fo @rescuelist {#ec %i}</value>
        </alias>
        <alias name="clearrescue">
          <value>#var rescuelist ""
            #var rescwho ""
            #echo ---- RESCUE LIST CLEARED ----</value>
        </alias>
        <alias name="removerescue">
          <value>#ec Removing %1 from rescue list
            #var rescuelist %delitem(%lower(%1),@rescuelist)
            #ec
            #ec ---  CHARACTERS IN THE RESCUE LIST  ---
            #ec
            #fo @Rescuelist {#ec %i}</value>
        </alias>
        <alias name="showrescue">
          <value>#ec
            #ec ---  CHARACTERS IN THE RESCUE LIST  ---
            #ec
            #fo @rescuelist {#ec %i}</value>
        </alias>
        <alias name="reportresc">
          <value>gt my rescue list is: @rescuelist</value>
        </alias>
        <menu priority="5530">
          addrescue
          <value>addrescue %selword</value>
        </menu>
        <menu priority="5540">
          remove from rescue
          <value>removerescue %selword</value>
        </menu>
        <menu priority="5550">
          show rescue
          <value>showresc</value>
        </menu>
        <menu priority="5560">
          clear rescue
          <value>clearrescue</value>
        </menu>
        <class name="Resc" initenable="true">
          <trigger priority="5470">
            <pattern>* attack{s|} strike{s|} (%w)[1234567890 times,]with *</pattern>
            <value>#if %ismember(%1,@rescuelist) {rescue %1;#var rescwho %1;#class Resc 0} {}</value>
          </trigger>
          <trigger priority="5570">
            <pattern>* attack{s|} haven't hurt (%w)</pattern>
            <value>#if %ismember(%1,@rescuelist) {rescue %1;#var rescwho %1;#class Resc 0} {}</value>
          </trigger>
        </class>
        Seperia
        <trigger priority="6040">
          <pattern>^@rescwho doesn't NEED rescuing!$</pattern>
          <value>#class Resc 1</value>
        </trigger>
        <trigger priority="6050">
          <pattern>^You successfully rescue @rescwho from (%*)</pattern>
          <value>#class Resc 1</value>
        </trigger>
        <trigger priority="6060">
          <pattern>^You fail to rescue @rescwho from (%*)</pattern>
          <value>#class Resc 1</value>
        </trigger>
      </class>
    </cmud>

## The Button

You'll also need to include this button to toggle the Auto Rescue
trigger on and off.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <button type="Toggle" autosize="false" width="72" height="23" autopos="false" left="401" color="#F0F0F0" priority="5619">
        <caption>Rescue Off</caption>
        <value>#class Rescue 1
    #echo ---- RESCUE MODE ON ----</value>
        <button>
          <caption>Rescue On</caption>
          <value>#Class Rescue 0
    #echo ---- RESCUE MODE OFF ----</value>
        </button>
      </button>
    </cmud>

## Designer comments

Feel free to note me [here](User:Shalineth "wikilink") or on board 2 to
Shalineth with any feedback or suggestions.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
