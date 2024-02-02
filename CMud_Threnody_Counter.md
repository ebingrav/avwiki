This counter will count how many people are in a
[Threnody](Threnody "wikilink") ritual when you join it, and will
increment every time another [Lord](:Category:Lord.md "wikilink") joins
in. Requires a window to display the values of the variables
"thren_count" and "lblthren". (I use my Status window).

## How to Use It

The script does all the work, no aliases or commands necessary.

## The Script

Save the following code as an .xml file, and import it into Cmud:  

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="Lord_Events">
        <trigger priority="2080">
          <pattern>~[LORD INFO~]: %w finishes Threnody, moving corpse of %w to safety.</pattern>
          <value>thren_count = ""
    lblthren = ""
    #echo -- Threnody ritual complete --
    </value>
        </trigger>
        <trigger priority="2090">
          <pattern>~[LORD INFO~]: %w initiates a Threnody dirge for corpse of (%w) in*</pattern>
          <value>lblthren = "Threnody Starting"
    #echo -- Threnody ritual starting --</value>
        </trigger>
        <trigger priority="2100">
          <pattern>You feel * power mingle with yours as * joins the ritual!</pattern>
          <value>#MATH thren_count (@thren_count+1)</value>
        </trigger>
        <trigger priority="2110">
          <pattern>You join (*) in performing the threnody ritual!</pattern>
          <value>thren_count = %eval(%numwords(%1, " and ")+1)
    lblthren = "Threnody Count: "</value>
          <notes>Not Working right now.</notes>
        </trigger>
        <var name="thren_count" type="Integer"/>
        <trigger priority="2140">
          <pattern>You begin a dirge for corpse of *...</pattern>
          <value>#var thren_count 1
    #var lblthren "Threnody Count: "</value>
        </trigger>
        <var name="lblthren" type="Literal"/>
      </class>
    </cmud>

## Designer comments

You can also add an \#ECHO line after each of the triggers to report the
current counter to the screen, rather than a Window if you prefer.

Updated for CMud v3.32.

[Category:Cmud_Scripting](Category:Cmud_Scripting "wikilink")
