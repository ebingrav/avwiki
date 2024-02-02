This is a pretty complex script that allows you to manage your alts.

Every time you log in, it will begin counting how often you've logged in
on that alt. Then it will recompile the list each time you log in sorted
based on which was logged in more and pop up list to choose and renames
your window to that name for easier tracking on multiday.

It also pops up a password box that's masked for a more secure login
with gagging of your password entry.

------------------------------------------------------------------------

**USAGE**

Go into the list variable "Charnames" and add in all your characters
into the list. The sorting will begin as you log in characters and you
will notice the most logged in characters at the top.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="Altlist" copy="yes">
        <var name="charnames" type="StringList" copy="yes">
          <json>[]</json>
        </var>
        <var name="thelog" type="Record" copy="yes">
          <json>{}</json>
        </var>
        <trigger priority="3740" newline="false" prompt="true" copy="yes">
          <pattern>^What name shall you be known by, adventurer?</pattern>
          <value>#var logging 1 0
    #var meverified 0
    #var spamlist "o:1"
    #var thelog2 @thelog
    #var highestalt 0
    #var highalt ""
    #loop %numwords( @charnames, "|") {
      #var highestalt 0
      #loopdb @thelog2 {#if %val&gt;@highestalt {
          #var highestalt %val
          #var highalt %key
          } {}}
      #additem spamlist @highalt
      #delkey thelog2 @highalt
      }
    #loop %numwords( @charnames, "|") {#if %ismember( %word( @charnames, %i, "|")) {} {#additem spamlist %word( @charnames, %i, "|")}}
    #var thelog2 @thelog
    #loop %numwords( @spamlist, "|") {#if %word( @spamlist, %i, "|")=%lower( @me) {#var spamlist %lower( %replaceitem( *@me, %i, @spamlist))} {}}
    #var logchar %pick( %replaceitem( %word( @spamlist, 2, "|"), 2, @spamlist))
    #if %iskey( @thelog, @logchar) {#addkey thelog @logchar %eval( %db( @thelog, @logchar)+1)} {#addkey thelog @logchar 1}
    @logchar
    #var logged 2 0
    #name %proper( @logchar)</value>
        </trigger>
        <var name="spamlist" type="StringList" copy="yes">
          <json>[]</json>
        </var>
        <var name="thelog2" type="Record" copy="yes"/>
        <var name="highestalt" copy="yes"/>
        <var name="highalt" copy="yes"/>
        <var name="logchar" copy="yes"/>
        <var name="logged" usedef="true" copy="yes">
          <default>0</default>
        </var>
        <trigger priority="2980" newline="false" prompt="true" copy="yes">
          <pattern>^Your Password:</pattern>
          <value>#send %prompt("","Password:","*")
    #gag
    #var logging 0 0
    #send " "
    #send " "
    #send " "
    #send " "</value>
        </trigger>
      </class>
    </cmud>

[Category: Scripting](Category:_Scripting "wikilink")
