## cMUD Fast alt switch script

command to change alt "log altname" It will disconnect you with the
command quit and log you back in with whatever alt you've substituted
for "altname"

## CLIENT CONFIGURATION

First of all. You will have to go into your settings. File menu --\>
(options, general.) Once you've opened it. On the left hand side click
"network" and then on top tab, click on "connection"... Take the check
mark out of the box that says.. "auto reconnect"

## EXPLANATION

If your wondering about the "lskdfjlsdjf" and "lskdfjlskdj" in the text.
it's just so that cMUD on connection and login will hit enter twice. you
can change that text to whatever you want. {l\_\_j\_\_j \\\_/
1\_\_j\_\_j 1\_\_j 1\_\_j\_\_j1\_\_j\\\_j} \<-- that line is what fires
the login' trigger. it's a line in the motd.

## INSTALLATION

Open notepad! copy the code in from below. click "file" then "save as"
on the bottom where it says "save as type" select "all files" then where
it says "file name" type in "autolog.xml"....

## IMPORTANT READ

In the below where it says. "YOURPASSWORDHERE" You have to change it to
your password or this will not work. \<--------READ-------\> Also where
it says "ALTNAMEHERE" you can put in one of your nicks. The first time
you use the command "log altname" it will reset to that name. Your
password must be the same for all alts. if not you will have to create
another variable for it.

## CODE

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="login">
        <alias name="log">
          <value>charname=%1
    quit</value>
        </alias>
        <trigger priority="20">
          <pattern>^May your stay in reality be worthwhile.</pattern>
          <value>#connect</value>
        </trigger>
        <trigger priority="30">
          <pattern>l__j__j  \_/  1__j__j  1__j  1__j__j1__j\_j</pattern>
          <value>@charname
    YOURPASSWORDHERE
    lskjdfj
    lskjdflsd</value>
        </trigger>
        <var name="charname">ALTNAMEHERE</var>
      </class>
    </cmud>

## CONTACT

I hope you all like this and have no trouble with it. It's much easier
then manually logging a char each time. If you need help with it. You
can msg me (Scrape) or note me on the greatest mud of all.
avatar.outland.org
