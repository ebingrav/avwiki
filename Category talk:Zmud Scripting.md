## Zmud Fast alt switch script

command to change alt "log altname" It will disconnect you with the
command quit and log you back in with whatever alt you've substituted
for "altname"

## CLIENT CONFIGURATION

First of all. You will have to go into your settings. (view,
preferences, general.) \<-- file menu. Once you've opened it. On the
left hand side click the + next to "connection" and in that sub-menu,
click on "reconnection"... Take the check mark out of the box that
says.. "auto reconnect"

## EXPLANATION

If your wondering about the "lskdfjlsdjf" and "lskdfjlskdj" in the text.
it's just so that zmud on connection and login will hit enter twice. you
can change that text to whatever you want. {l\_\_j\_\_j \\\_/
1\_\_j\_\_j 1\_\_j 1\_\_j\_\_j1\_\_j\\\_j} \<-- that line is what fires
the login' trigger. it's a line in the motd.

## INSTALLATION

Now you can either copy the below and paste it into a .txt file then
import it through the file menu or copy and paste each line one at a
time into your command line and hit enter after each line. if you don't
know what i mean by make a text file. open notepad, copy every line that
starts with a pound sign and paste it in. save it as auto-log.txt

## IMPORTANT READ

In the below where it says. "PASSWORDHERE" You have to change it to your
password or this will not work. \<--------READ-------\> Also where it
says "SCRAPE" you can put in one of your nicks. The first time you use
the command "log altname" it will reset to that name.

    #CLASS {fast-alt-switch} {enable}
    #ALIAS log {charname=%1;quit}
    #VAR charname {SCRAPE}
    #TRIGGER {May your stay in reality be worthwhile.} {#connect}
    #TRIGGER {l__j__j  \_/  1__j__j  1__j  1__j__j1__j\_j} {@charname;PASSWORDHERE;lskdfjlsdjf;lskdfjlskdj}
    #CLASS 0

## CONTACT

I hope you all like this and have no trouble with it. It's much easier
then manually logging a char each time. If you need help with it. You
can msg me (Scrape) or note me on the greatest mud of all.
avatar.outland.org
