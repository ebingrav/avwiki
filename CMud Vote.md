Save the following code as an .xml file and import it into the CMud
editor.

<?xml version="1.0" encoding="ISO-8859-1" ?>

<cmud>  
`  `<class name="Vote">  
`    `<class name="TMS">  
`      `<alias name="tms">  
`        `<value>`#url "www.topmudsites.com/vote-snikt.html"`  
`#var tmstime @newtime`</value>  
`      `</alias>  
`      `<var name="tmstime">`0`</var>  
`    `</class>  
`    `<class name="TMC">  
`      `<alias name="tmc">  
`        `<value>`#url "www.mudconnect.com/cgi-bin/vote_rank.cgi?mud=AVATAR+Mud"`  
`#var tmctime @newtime`</value>  
`      `</alias>  
`      `<var name="tmctime">`0`</var>  
`    `</class>  
`    `<trigger priority="7440">  
`      `<pattern>`^Welcome back to the AVATAR System,*.$`</pattern>  
`      `<value><![CDATA[#var newtime (%time(y)*1440*(365+%if((%time(y)/4)=(%float(%time(y))/4),1,0))+(%db(@cdays,%time(m))+%time(d)+%if((%time(y)/4)=(%float(%time(y))/4) && %time(m)>`2,1,0))*1440+%time(h)*60+%time(n))`  
`#if (@newtime>(@tmctime+1440)) {tmc}`  
`#if (@newtime>(@tmstime+720)) {tms}]]>`</value>  
`    `</trigger>  
`    `<var name="cdays" type="Record">  
`      `<value>`9=243|8=212|7=182|6=151|5=120|4=90|3=59|2=31|1=0|12=334|11=304|10=273`</value>  
`      `<json>`{"9":243,"8":212,"7":182,"6":151,"5":120,"4":90,"3":59,"2":31,"1":0,"12":334,"11":304,"10":273}`</json>  
`    `</var>  
`    `<var name="newtime">`5838298`</var>  
`  `</class>  
</cmud>

This script checks if you've voted in the past 12 or 24 hours whenever
you log onto Avatar. If you haven't, your browser will open so you can
vote.

[Category:Cmud Scripting](Category:Cmud_Scripting "wikilink")
