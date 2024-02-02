Save the following code as an .xml and import it into CMud: See below
for an explanation of how it works. It's messy. I'm not an incredible
coder, so if anyone sees any room for improvement, please let me know on
my [discussion](User_talk:Shalineth.md "wikilink") page.

Updated for CMud v3.32. Thanks to Cesroc for pointing out the
correction.

## The Code

    <?xml version="1.0" encoding="ISO-8859-1" ?>
    <cmud>
      <class name="dcounter">
        <alias name="dcadd">
          <value>#additem /dcounter/DCtrack %1
    #additem /dcounter/DCnames %1
    #echo %proper(%1) added to the damage counting list.
    </value>
        </alias>
        <alias name="dcrep">
          <value>#YESNO "What report would you like to show?" {Short report (total damage done and taken) :dcrepA} {Breakdown of damage verbs:dcrepB}
    </value>
        </alias>
        <var name="dverbshort" type="StringList">
          <value>nil|pathetic|weak|punishing|surprising|amazing|astonishing|mauling|decimating|devastating|pulverizing|maiming|eviscerating|mutilating|disemboweling|dismembering|massacring|mangling|demolishing|obliterating|annihilating|eradicating|vaporizing|destructive|extreme|porcine|divine|daunting|terminal</value>
          <json>["nil","pathetic","weak","punishing","surprising","amazing","astonishing","mauling","decimating","devastating","pulverizing","maiming","eviscerating","mutilating","disemboweling","dismembering","massacring","mangling","demolishing","obliterating","annihilating","eradicating","vaporizing","destructive","extreme","porcine","divine","daunting","terminal"]</json>
        </var>
        <var name="dvalues" type="StringList">
          <value>0|2|4|8|10|14|18|22|26|30|34|38|42|46|49|55|60|65|70|75|80|85|90|95|100|110|120|130|140|150|160|170|180|190|200|225|250|275|300|325|350|375|400|425|450|475|500|540|574|606|675|730|769|810|884|915|1000|1100|1200|1300|1400|1500|1600|1700|1800|1900|2000|2200|2400|2600|2800|3000|3200|3400|3600|3800|4100|4500|5007|5901|5902|6200|6500|7000|7500|7800|8200|8500|9000|9500|10000|11000|12000|13000|14000|15000|16500|18000|19000|20000|21000|22000|23000|24000|25000|26000|27000|28000|29000|30000|31000|32000|33000|34000|35000|36000|37000|38000|39000|40000|41000|42000|43000|44500|47000|48000|50000|51000|53000|55000|57000|59000|61000|65000|70000|75000|80000|100000|0</value>
          <json>[0,2,4,8,10,14,18,22,26,30,34,38,42,46,49,55,60,65,70,75,80,85,90,95,100,110,120,130,140,150,160,170,180,190,200,225,250,275,300,325,350,375,400,425,450,475,500,540,574,606,675,730,769,810,884,915,1000,1100,1200,1300,1400,1500,1600,1700,1800,1900,2000,2200,2400,2600,2800,3000,3200,3400,3600,3800,4100,4500,5007,5901,5902,6200,6500,7000,7500,7800,8200,8500,9000,9500,10000,11000,12000,13000,14000,15000,16500,18000,19000,20000,21000,22000,23000,24000,25000,26000,27000,28000,29000,30000,31000,32000,33000,34000,35000,36000,37000,38000,39000,40000,41000,42000,43000,44500,47000,48000,50000,51000,53000,55000,57000,59000,61000,65000,70000,75000,80000,100000,0]</json>
        </var>
        <var name="dverbs" type="StringList">
          <value><![CDATA[nil|pathetic|weak|punishing|surprising|amazing|astonishing|mauling|MAULING|MAULING*|MAULING**|MAULING***|decimating|DECIMATING|DECIMATING*|DECIMATING**|DECIMATING***|devastating|DEVASTATING|DEVASTATING*|DEVASTATING**|DEVASTATING***|pulverizing|PULVERIZING|PULVERIZING*|PULVERIZING**|PULVERIZING***|maiming|MAIMING|MAIMING*|MAIMING**|MAIMING***|eviscerating|EVISCERATING|EVISCERATING*|EVISCERATING**|EVISCERATING***|mutilating|MUTILATING|MUTILATING*|MUTILATING**|MUTILATING***|disemboweling|DISEMBOWELING|DISEMBOWELING*|DISEMBOWELING**|DISEMBOWELING***|dismembering|DISMEMBERING|DISMEMBERING*|DISMEMBERING**|DISMEMBERING***|massacring|MASSACRING|MASSACRING*|MASSACRING**|MASSACRING***|mangling|MANGLING|MANGLING*|MANGLING**|MANGLING***|demolishing|DEMOLISHING|DEMOLISHING*|DEMOLISHING**|DEMOLISHING***|obliterating|OBLITERATING|OBLITERATING*|OBLITERATING**|OBLITERATING***|annihilating|ANNIHILATING|ANNIHILATING*|ANNIHILATING**|ANNIHILATING***|ANNIHILATING***<|ANNIHILATING***<<|ANNIHILATING***<<<|ANNIHILATING***<<<<|eradicating|ERADICATING|ERADICATING*|ERADICATING**|ERADICATING***|ERADICATING***<|ERADICATING***<<|ERADICATING***<<<|ERADICATING***<<<<|vaporizing|VAPORIZING|VAPORIZING*|VAPORIZING**|VAPORIZING***|VAPORIZING***<|VAPORIZING***<<|VAPORIZING***<<<|VAPORIZING***<<<<|destructive|DESTRUCTIVE|DESTRUCTIVE*|DESTRUCTIVE**|DESTRUCTIVE***|DESTRUCTIVE****|DESTRUCTIVE****<|DESTRUCTIVE****<<|DESTRUCTIVE****<<<|DESTRUCTIVE****<<<<|DESTRUCTIVE***<<<<|DESTRUCTIVE**<<<<="="|DESTRUCTIVE*<<<<="=="|DESTRUCTIVE<<<<="==="|extreme|EXTREME|EXTREME*|EXTREME**|EXTREME***|EXTREME****|EXTREME****<|EXTREME****<<|EXTREME****<<<|EXTREME****<<<<|EXTREME***<<<<|EXTREME**<<<<="="|EXTREME*<<<<="=="|EXTREME<<<<="==="|porcine|PORCINE|PORCINE*|PORCINE**|PORCINE***|PORCINE***<|PORCINE***<<|PORCINE***<<<|PORCINE***<<<<|divine|daunting|terminal]]></value>
          <json><![CDATA[["nil","pathetic","weak","punishing","surprising","amazing","astonishing","mauling","MAULING","MAULING*","MAULING**","MAULING***","decimating","DECIMATING","DECIMATING*","DECIMATING**","DECIMATING***","devastating","DEVASTATING","DEVASTATING*","DEVASTATING**","DEVASTATING***","pulverizing","PULVERIZING","PULVERIZING*","PULVERIZING**","PULVERIZING***","maiming","MAIMING","MAIMING*","MAIMING**","MAIMING***","eviscerating","EVISCERATING","EVISCERATING*","EVISCERATING**","EVISCERATING***","mutilating","MUTILATING","MUTILATING*","MUTILATING**","MUTILATING***","disemboweling","DISEMBOWELING","DISEMBOWELING*","DISEMBOWELING**","DISEMBOWELING***","dismembering","DISMEMBERING","DISMEMBERING*","DISMEMBERING**","DISMEMBERING***","massacring","MASSACRING","MASSACRING*","MASSACRING**","MASSACRING***","mangling","MANGLING","MANGLING*","MANGLING**","MANGLING***","demolishing","DEMOLISHING","DEMOLISHING*","DEMOLISHING**","DEMOLISHING***","obliterating","OBLITERATING","OBLITERATING*","OBLITERATING**","OBLITERATING***","annihilating","ANNIHILATING","ANNIHILATING*","ANNIHILATING**","ANNIHILATING***","ANNIHILATING***<","ANNIHILATING***<<","ANNIHILATING***<<<","ANNIHILATING***<<<<","eradicating","ERADICATING","ERADICATING*","ERADICATING**","ERADICATING***","ERADICATING***<","ERADICATING***<<","ERADICATING***<<<","ERADICATING***<<<<","vaporizing","VAPORIZING","VAPORIZING*","VAPORIZING**","VAPORIZING***","VAPORIZING***<","VAPORIZING***<<","VAPORIZING***<<<","VAPORIZING***<<<<","destructive","DESTRUCTIVE","DESTRUCTIVE*","DESTRUCTIVE**","DESTRUCTIVE***","DESTRUCTIVE****","DESTRUCTIVE****<","DESTRUCTIVE****<<","DESTRUCTIVE****<<<","DESTRUCTIVE****<<<<",{"DESTRUCTIVE***<<<<":""},{"DESTRUCTIVE**<<<<":"="},{"DESTRUCTIVE*<<<<":"=="},{"DESTRUCTIVE<<<<":"==="},"extreme","EXTREME","EXTREME*","EXTREME**","EXTREME***","EXTREME****","EXTREME****<","EXTREME****<<","EXTREME****<<<","EXTREME****<<<<",{"EXTREME***<<<<":""},{"EXTREME**<<<<":"="},{"EXTREME*<<<<":"=="},{"EXTREME<<<<":"==="},"porcine","PORCINE","PORCINE*","PORCINE**","PORCINE***","PORCINE***<","PORCINE***<<","PORCINE***<<<","PORCINE***<<<<","divine","daunting","terminal"]]]></json>
        </var>
        <trigger priority="2980">
          <pattern>^Welcome back to the AVATAR System, {lord|lady|hero} (%w).</pattern>
          <value>#var dcCurChar %lower( %1)

    </value>
        </trigger>
        <trigger priority="2990">
          <pattern>^Welcome back to the AVATAR System (%w),</pattern>
          <value>#var dcCurChar %lower( %1)

    </value>
        </trigger>
        <menu priority="3020">
          <caption>DC Add</caption>
          <value>dcadd %lower( %selword)
    </value>
        </menu>
        <menu priority="3030">
          <caption>DC Clear</caption>
          <value>dcclear
    </value>
        </menu>
        <menu priority="3040">
          <caption>DC List</caption>
          <value>#echo -
    #echo --- CHARACTERS ON THE DAMAGE COUNTER LIST ---
    #forall @DCnames {#echo %proper(%i)}
    </value>
        </menu>
        <alias name="dcclear">
          <value>#FO @DCtrack {#UNVAR %i}
    #VAR /dcounter/DCtrack "you"
    #VAR /dcounter/DCnames @DCCurChar
    #ECHO Damage counter reset. Please re-add groupies.
    </value>
        </alias>
        <alias name="dcs">
          <value>dcclear
    #if (!DCreport) {DCreport = gt} {}
    #t+ "%w is leading (%d) player[ s]with"
    groupstat
    </value>
        </alias>
        <trigger priority="3100" enabled="false">
          <pattern>%w is leading (%d) player[ s]with</pattern>
          <value>#t+ "%d~|*%d %w%s(&amp;%w{DCtemp1})%s[Sleep|Stand|Fight|Rest|]"
    #var /dcounter/groupcounter %1
    group
    #t- "%w is leading (%d) player[ s]with"


    </value>
        </trigger>
        <var name="dcCurChar">antiopeia</var>
        <trigger priority="5870">
          <pattern>* ({@DCtrack})* with[*&gt;= ]({@dverbshort})([*&lt;= ])%w[!.]</pattern>
          <value>#addkey %1 taken %eval(%db(@{%1}, taken) + %item( @dvalues, %ismember( %concat( %2, %trim(%3)), @dverbs)))

    // tracks mobs damage done to you and groupmates.</value>
        </trigger>
        <trigger priority="5880" trigontrig="false">
          <pattern>^({@DCtrack})~'s[^&gt;;] * [^&gt;;] with[*&gt;= ]({@dverbshort})([*&lt;= ])%w[!.]</pattern>
          <value>#addkey %1 %2 (%eval(%db(@{%1}, %2)+1))
    #addkey %1 dealt (%eval(%db(@{%1}, dealt) + %item( @dvalues, %ismember(%concat(%2,%trim(%3)), @dverbs))))
    #addkey %1 attacks (%eval(%db(@{%1}, attacks)+1))

    // Tracks groupmates damage done to mobs.</value>
        </trigger>
        <trigger priority="3000" enabled="false">
          <pattern>%d~|*%d %w%s(&amp;%w{DCtemp1})%s[Sleep|Stand|Fight|Rest|]</pattern>
          <value>#if (%lower(@DCtemp1) != @dcCurChar) {dcadd %lower(@DCtemp1)}
    #var groupcounter @groupcounter-1
    #if (@groupcounter &lt;= 0) {#t- "%d~|*%d %w%s(%w{@DCtemp1})%s[Sleep|Stand|Fight|Rest|]"} {}
    </value>
        </trigger>
        <var name="DCnames">athalos</var>
        <var name="DCtemp1">Athalos</var>
        <var name="DCrepto">gt</var>
        <var name="DC" type="Record">
          <value>totalcount=5|takencount=6|reportfor=you|total=8|taken=7</value>
          <json>{"totalcount":5,"takencount":6,"reportfor":"you","total":8,"taken":7}</json>
        </var>
        <trigger priority="5880" trigontrig="false">
          <pattern>^([you|your]) * with[*&gt;= ]({@dverbshort})([*&lt;= ])%w[!.]</pattern>
          <value>#addkey You %2 (%eval(%db(@You, %2)+1))
    #addkey You dealt (%eval(%db(@You, dealt) + %item( @dvalues, %ismember( %concat( %2, %trim(%3)), @dverbs))))
    #addkey You attacks (%eval(%db(@You, attacks)+1))
    // Tracks damage you do to mobs.

    </value>
        </trigger>
        <alias name="DCrepto">
          <value>#YESNO "What channel would you like to output to?" {*Grouptell: #var DCrepto gt} {Say: #var DCrepto say} {Echo (local): #var DCrepto #ECHO}
    </value>
        </alias>
        <var name="Groupcounter">0</var>
        <var name="DCtrack" type="Literal">you</var>
        <alias name="DCrep1">
          <value>#if (%db(@you, terminal)="") {#addkey you terminal 0} {}
    #var avgdamage %eval((%db(@you, dealt))/(%db(@you, attacks)))
    $msg1 = "Damage done by "%proper(@dcCurChar)": |r|" %db(@you, attacks) "|n| attacks for an average of |br|" %format("&amp;2.0n",@avgdamage) "|n| hps or " 
    #if (%ismember(@avgdamage,@dvalues)) {} {#until (%ismember(@avgdamage,@dvalues)) {#var avgdamage %eval(@avgdamage+1)}} 
    $msg2 = "|br|"%item(@fulldverbs,%ismember(@avgdamage,@dvalues)) "|n| damage, "
    $msg3 = "with |bb|" %db(@you, terminal) "|n| terminal hits. " 
    $msg4 = "I dished out |bg|" %format("&amp;2.0n",%db(@you, dealt)) "|n| damage while taking |bb|" %format("&amp;2.0n",%db(@you, taken)) "|n| hps myself."
    @dcrepto %concat(" ",$msg1, $msg2, $msg3, $msg4)

    // Short report format showing damage you've done and taken. </value>
        </alias>
        <var name="avgdamage">180</var>
        <var name="fulldverbs" type="StringList">
          <value><![CDATA[nil|pathetic|weak|punishing|surprising|amazing|astonishing|mauling|MAULING|*MAULING*|**MAULING**|***MAULING***|decimating|DECIMATING|*DECIMATING*|**DECIMATING**|***DECIMATING***|devastating|DEVASTATING|*DEVASTATING*|**DEVASTATING**|***DEVASTATING***|pulverizing|PULVERIZING|*PULVERIZING*|**PULVERIZING**|***PULVERIZING***|maiming|MAIMING|*MAIMING*|**MAIMING**|***MAIMING***|eviscerating|EVISCERATING|*EVISCERATING*|**EVISCERATING**|***EVISCERATING***|mutilating|MUTILATING|*MUTILATING*|**MUTILATING**|***MUTILATING***|disemboweling|DISEMBOWELING|*DISEMBOWELING*|**DISEMBOWELING**|***DISEMBOWELING***|dismembering|DISMEMBERING|*DISMEMBERING*|**DISMEMBERING**|***DISMEMBERING***|massacring|MASSACRING|*MASSACRING*|**MASSACRING**|***MASSACRING***|mangling|MANGLING|*MANGLING*|**MANGLING**|***MANGLING***|demolishing|DEMOLISHING|*DEMOLISHING*|**DEMOLISHING**|***DEMOLISHING***|obliterating|OBLITERATING|*OBLITERATING*|**OBLITERATING**|***OBLITERATING***|annihilating|ANNIHILATING|*ANNIHILATING*|**ANNIHILATING**|***ANNIHILATING***|>***ANNIHILATING***<|>>***ANNIHILATING***<<|>>>***ANNIHILATING***<<<|>>>>***ANNIHILATING***<<<<|eradicating|ERADICATING|*ERADICATING*|**ERADICATING**|***ERADICATING***|>***ERADICATING***<|>>***ERADICATING***<<|>>>***ERADICATING***<<<|>>>>***ERADICATING***<<<<|vaporizing|VAPORIZING|*VAPORIZING*|**VAPORIZING**|***VAPORIZING***|>***VAPORIZING***<|>>***VAPORIZING***<<|>>>***VAPORIZING***<<<|>>>>***VAPORIZING***<<<<|destructive|DESTRUCTIVE|*DESTRUCTIVE*|**DESTRUCTIVE**|***DESTRUCTIVE***|****DESTRUCTIVE****|>****DESTRUCTIVE****<|>>****DESTRUCTIVE****<<|>>>****DESTRUCTIVE****<<<|>>>>****DESTRUCTIVE****<<<<|=>>>>***DESTRUCTIVE***<<<<=|==>>>>**DESTRUCTIVE**<<<<==|===>>>>*DESTRUCTIVE*<<<<===|====>>>>DESTRUCTIVE<<<<====|extreme|EXTREME|*EXTREME*|**EXTREME**|***EXTREME***|****EXTREME****|>****EXTREME****<|>>****EXTREME****<<|>>>****EXTREME****<<<|>>>>****EXTREME****<<<<|=>>>>***EXTREME***<<<<=|==>>>>**EXTREME**<<<<==|===>>>>*EXTREME*<<<<===|====>>>>EXTREME<<<<====|porcine|PORCINE|*PORCINE*|**PORCINE**|***PORCINE***|>***PORCINE***<|>>***PORCINE***<<|>>>***PORCINE***<<<|>>>>***PORCINE***<<<<|divine|daunting|terminal]]></value>
          <json><![CDATA[["nil","pathetic","weak","punishing","surprising","amazing","astonishing","mauling","MAULING","*MAULING*","**MAULING**","***MAULING***","decimating","DECIMATING","*DECIMATING*","**DECIMATING**","***DECIMATING***","devastating","DEVASTATING","*DEVASTATING*","**DEVASTATING**","***DEVASTATING***","pulverizing","PULVERIZING","*PULVERIZING*","**PULVERIZING**","***PULVERIZING***","maiming","MAIMING","*MAIMING*","**MAIMING**","***MAIMING***","eviscerating","EVISCERATING","*EVISCERATING*","**EVISCERATING**","***EVISCERATING***","mutilating","MUTILATING","*MUTILATING*","**MUTILATING**","***MUTILATING***","disemboweling","DISEMBOWELING","*DISEMBOWELING*","**DISEMBOWELING**","***DISEMBOWELING***","dismembering","DISMEMBERING","*DISMEMBERING*","**DISMEMBERING**","***DISMEMBERING***","massacring","MASSACRING","*MASSACRING*","**MASSACRING**","***MASSACRING***","mangling","MANGLING","*MANGLING*","**MANGLING**","***MANGLING***","demolishing","DEMOLISHING","*DEMOLISHING*","**DEMOLISHING**","***DEMOLISHING***","obliterating","OBLITERATING","*OBLITERATING*","**OBLITERATING**","***OBLITERATING***","annihilating","ANNIHILATING","*ANNIHILATING*","**ANNIHILATING**","***ANNIHILATING***",">***ANNIHILATING***<",">>***ANNIHILATING***<<",">>>***ANNIHILATING***<<<",">>>>***ANNIHILATING***<<<<","eradicating","ERADICATING","*ERADICATING*","**ERADICATING**","***ERADICATING***",">***ERADICATING***<",">>***ERADICATING***<<",">>>***ERADICATING***<<<",">>>>***ERADICATING***<<<<","vaporizing","VAPORIZING","*VAPORIZING*","**VAPORIZING**","***VAPORIZING***",">***VAPORIZING***<",">>***VAPORIZING***<<",">>>***VAPORIZING***<<<",">>>>***VAPORIZING***<<<<","destructive","DESTRUCTIVE","*DESTRUCTIVE*","**DESTRUCTIVE**","***DESTRUCTIVE***","****DESTRUCTIVE****",">****DESTRUCTIVE****<",">>****DESTRUCTIVE****<<",">>>****DESTRUCTIVE****<<<",">>>>****DESTRUCTIVE****<<<<","=>>>>***DESTRUCTIVE***<<<<=","==>>>>**DESTRUCTIVE**<<<<==","===>>>>*DESTRUCTIVE*<<<<===","====>>>>DESTRUCTIVE<<<<====","extreme","EXTREME","*EXTREME*","**EXTREME**","***EXTREME***","****EXTREME****",">****EXTREME****<",">>****EXTREME****<<",">>>****EXTREME****<<<",">>>>****EXTREME****<<<<","=>>>>***EXTREME***<<<<=","==>>>>**EXTREME**<<<<==","===>>>>*EXTREME*<<<<===","====>>>>EXTREME<<<<====","porcine","PORCINE","*PORCINE*","**PORCINE**","***PORCINE***",">***PORCINE***<",">>***PORCINE***<<",">>>***PORCINE***<<<",">>>>***PORCINE***<<<<","divine","daunting","terminal"]]]></json>
        </var>
        <alias name="DcrepA">
          <value>#if (!DCrepto) {dcrepto} {}
    #YESNO "Who would you like to report for?" {Yourself: DCrep1} {Everyone: DCrep2} {Partial:#NEWVAR DCreportfor %pick( "p:Who would you like to display reports for?:", "o:1",@DCnames); DCrep3}</value>
        </alias>
        <alias name="DCrep3">
          <value>#if (@dcCurChar = @DCreportfor) {DCrep1} {
    #if (%db(@{@DCReportfor}, terminal)="") {#addkey @DCreportfor terminal 0} {}
    #var avgdamage %eval((%db(@{@DCReportfor}, dealt))/(%db(@{@DCReportfor}, attacks)))
    $msg1 = "Damage done by "%proper(@DCReportfor)":.md|r|" %db(@{@DCReportfor}, attacks) "|n| attacks for an average of |br|" %format("&amp;2.0n",@avgdamage) "|n| hps or " 
    #if (%ismember(@avgdamage,@dvalues)) {} {#until (%ismember(@avgdamage,@dvalues)) {#var avgdamage %eval(@avgdamage+1)}} 
    $msg2 = "|br|"%item(@fulldverbs,%ismember(@avgdamage,@dvalues)) "|n| damage, "
    $msg3 = "with |bb|" %db(@{@DCReportfor}, terminal) "|n| terminal hits. " 
    $msg4 = @DCReportfor" dished out |bg|" %format("&amp;2.0n",%db(@{@DCReportfor}, dealt)) "|n| damage while taking |bb|" %format("&amp;2.0n",%db(@{@DCReportfor}, taken)) "|n| hps."
    @dcrepto %concat(" ",$msg1, $msg2, $msg3, $msg4)}

    // Short report format showing damage a selected groupie (@DCReportfor) has done and taken.</value>
        </alias>
        <var name="DCreportfor">boaz</var>
        <alias name="DcrepB">
          <value>#if (!DCrepto) {dcrepto} {}
    #YESNO "Who would you like to report for?" {Yourself: DCrep4} {Everyone: DCrep5} {Partial:#NEWVAR DCreportfor %pick( "p:Who would you like to display reports for?:", "o:1",@DCnames); DCrep6}</value>
        </alias>
        <alias name="DCrep2">
          <value>// DCrep1
    #forall @DCnames {#if (%i="your") {} {#var DCreportfor %i; DCrep3}}

    // Short report format for all members of group, excluding "your".</value>
        </alias>
        <alias name="DCrep4">
          <value>#VAR youline ""
    #LOOPDB @you {#if (%pos(%key,taken|dealt|attacks|terminal)&gt;0) {} {youline = %concat(@youline,", ",%key,":",%val)}}
    @dcrepto " " |bc|%proper(@DCCurChar)"'s |n|damage report: "%db(@you, attacks)" attacks," %lower(%right(@youline,1))", "%db(@you, terminal)" terminal hits."

    // long report format showing damage for @you.
    </value>
        </alias>
        <var name="youline">, dismembering:7, pathetic:22, DISEMBOWELING:2, MUTILATING:20, weak:7, MAIMING:5, EVISCERATING:17</var>
        <alias name="DCrep6">
          <value>#if (@dcCurChar = @DCreportfor) {DCrep4} {
    #VAR youline ""
    #LOOPDB @{@dcreportfor} {#if (%pos(%key,taken|dealt|attacks|terminal)&gt;0) {} {youline = %concat(@youline,", ",%key,":",%val)}}
    @dcrepto " " |bc|%proper(@DCreportfor)"'s |n|damage report: "%db(@{@DCreportfor}, attacks)" attacks," %lower(%right(@youline,1))", "%db(@@{@dcreportfor}, terminal)" terminal hits."}

    // long report format showing damage for a selected groupie (@DCreportfor).
    </value>
        </alias>
        <alias name="DCrep5">
          <value>// DCrep4
    #forall @DCnames {#if (%i="your") {} {#var DCreportfor %i; DCrep6}}

    // Short report format for all members of group, excluding "your".</value>
        </alias>
        <menu priority="10840">
          <caption>DC Report</caption>
          <value>dcrep</value>
        </menu>
      </class>
    </cmud>

## Here's how it works

**Menu Options**  
There are four menu options, accessed by right clicking in the main
window:  
**DC Add** will add the player's name you click on.  
**DC Clear** will reset the entire damage counter list.  
**DC List** will show the current list of players (in group) being
tracked.  
**DC Report** will display two menus prompting you which report you wish
to view.  

**Aliases**  
**dcadd** does the same thing as DC Add above.  
**dcclear** does the same thing as DC Clear above.  
**dcrep** will display two menus. One to select which report format you
want (see below) the second to select who you want to report for.  
**dcrepto** lets you select which channel you output to: say, grouptell
or echo within CMud main window.

**Report Format**  
Short Format: Reports number of hits, average and total damage done and
taken. Example:  

    You tell the group 'Damage done by Athalos: 435 attacks for an average of 349 hps or **MUTILATING** damage, with 40 terminal hits.
    I dished out 152,196 damage while taking 5,417 hps myself.'

Breakdown Format: Breaks down each player selected by damage verb and
count. Example:  

    You tell the group 'Athalos's damage report: 435 attacks,  maiming:2, pathetic:166, eviscerating:1, weak:1, disemboweling:11,
    mangling:9, mauling:4, dismembering:134, massacring:49, demolishing:6, mutilating:9, amazing:2, astonishing:1, 40 terminal hits.'

[Category: Cmud Scripting](Category:_Cmud_Scripting "wikilink")
