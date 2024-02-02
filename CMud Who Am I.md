This very simple script has a variety of uses from customizing kill
triggers, to the way you react to bot commands, to even simplifying
commands through single aliasing:

All it does is it simply creates the variables @me, @myclass, and
@myrace

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="login" copy="yes">
        <trigger priority="3040" copy="yes">
          <pattern>^Welcome back to the AVATAR System &amp;me, hope you get beyond level %d today!</pattern>
          <value>#echo @me
    #send whois @me</value>
        </trigger>
        <trigger priority="3050" copy="yes">
          <pattern>^Welcome back to the AVATAR System, %w &amp;me%p</pattern>
          <value>#echo @me
    #send whois @me</value>
        </trigger>
      <trigger name="MyRaceClass" priority="2490" copy="yes">
        <pattern>^~[??? ???? (%w) (%w) ??? ? ~] {~(OUTLAW~) |~(INVIS~) |~(AFK~) |~(LINKDEAD~) |~(SHADOW~) |}@me%*</pattern>
        <value>#var myrace %2
    #var myclass %1</value>
      </trigger>
      </class>
    </cmud>

    <trigger name="MyRaceClass" priority="2490" copy="yes">
        <pattern>^~[??? ???? (%w) (%w) ??? ? ~] {~(OUTLAW~) |~(INVIS~) |~(AFK~) |~(LINKDEAD~) |~(SHADOW~) |}@me%*</pattern>
        <value>#var myrace %2
    #var myclass %1</value>
      </trigger>

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
