A very simple script to colorize each auction post for easier viewing.

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="1" copy="yes">
        <pattern>^ (???) |%s(%d)%s|%s(%d)%s|%s(%d)%s|%s(%d)</pattern>
        <value>#if @bidline=1 {#var bidline 0
    #CO cyan} {
    #var bidline 1
    #CO green}</value>
      </trigger>
    </cmud>

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <trigger priority="3910" copy="yes">
        <pattern>^%s&gt; %1</pattern>
        <value>#if @bidline=0 {
    #CO cyan} {
    #CO green}</value>
      </trigger>
    </cmud>

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
