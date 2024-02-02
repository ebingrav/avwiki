Here is an alarm that will open your default browser and bring you to
the voting page. Then you just have to click the link on the page to
place your vote. This alarm fires every 12hrs as that is how often you
can vote on topmudsites.com

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger name="vote" type="Alarm" priority="3090">
        <pattern>43200</pattern>
        <value>#url topmudsites.com/vote-snikt.html</value>
      </trigger>
    </cmud>

If you don't want it to do it automatically here is a simple alias. Just
type vote into your command line and your default browser will open and
load the voting page.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <alias name="vote">
        <value>#URL topmudsites.com/vote-snikt.html</value>
      </alias>
    </cmud>

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
