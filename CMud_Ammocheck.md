This script will keep a counter of archery ammunition, updating as
arrows are held, removed or the "ammocheck" command is used. Will track
the information in a small window to the side of your CMud screen. This
script is customized for Hero archers only, and may need some tweaking
to be used for Lords or bullet-using classes (ie. Fusilier).

This script has been updated for CMud v3.32.

## The Script

Save the following code as an .xml file, and import it into Cmud:  
Then scroll down and find the reference to "Your_Name" and replace it
with your hero's name.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="AmmoCount">
        <var name="splinter">278</var>
        <trigger priority="1840">
          <pattern>You hold (%d) (%w) arrows in your hands.$</pattern>
          <value>#VAR %2 %1
    Ammowindow</value>
        </trigger>
        <trigger priority="1850">
          <pattern>&lt;held&gt; %s (%d) (%w) arrows$</pattern>
          <value>#VAR %2 %1
    Ammowindow</value>
        </trigger>
        <var name="flaming">122</var>
        <trigger priority="1890">
          <pattern>You stop using (%d) (%w) arrows.$</pattern>
          <value>put arrow quiver
    #var %2 %1
    Ammowindow</value>
        </trigger>
        <alias name="splinter">
          <value>get splinter quiver
    hold splinter</value>
        </alias>
        <alias name="flaming">
          <value>get flaming quiver
    hold flaming</value>
        </alias>
        <alias name="standard">
          <value>get standard quiver
    hold standard
    </value>
        </alias>
        <alias name="steel">
          <value>get steel quiver
    hold steel
    </value>
        </alias>
        <alias name="poison">
          <value>get poison quiver
    hold poison</value>
        </alias>
        <alias name="piercing">
          <value>get piercing quiver
    hold piercing</value>
        </alias>
        <alias name="barbed">
          <value>get barbed quiver
    hold barbed</value>
        </alias>
        <var name="piercing">115</var>
        <var name="barbed">113</var>
        <var name="steel">120</var>
        <var name="poison">153</var>
        <var name="standard"/>
        <trigger priority="2020">
          <pattern>You have (%d) (%w) arrows remaining.$</pattern>
          <value>#VAR %2 %1
    Ammowindow</value>
        </trigger>
        <alias name="Ammowindow">
          <value>#EXECWIN AmmoCount {#CLR}
    #WIN AmmoCount "Barbed"   @barbed
    #WIN AmmoCount "Flaming"  @flaming
    #WIN AmmoCount "Piercing" @piercing
    #WIN AmmoCount "Poison"   @poison
    #WIN AmmoCount "Splinter" @splinter
    #WIN AmmoCount "Standard" @standard
    #WIN AmmoCount "Steel"    @steel</value>
        </alias>
        <alias name="clearammo">
          <value>#clr</value>
        </alias>
      </class>
    </cmud>

## Notes

Be sure to add a trigger that matches on logout and disables the
Ammocount class, unless you plan on doing it manually. The following
will work:

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="2880">
        <pattern>May your stay in reality be worthwhile.</pattern>
        <value>#class AmmoCount 0</value>
      </trigger>
    </cmud>

## Designer comments

Feel free to note me [here](User:Shalineth "wikilink") or on board 2 to
Shalineth with any feedback or suggestions.

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
