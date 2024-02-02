This is a relatively simple auto-rescue script. It doesn't have advanced
mechanisms for tracking other people's rescues (which happen often in
EHA, and may cause you undue lag as you try to rescue the already
rescued people).

## Code

*Put the following into a file in your tintin++ folder and use **\#read
<filename>** while in tintin to load them up. Though you probably want
them to autoload with your character.*

`#var resc 0;`  
  
`#alias {ron} {`  
`  #var resc 1;`  
`  gt |BK|AUTO RESCUE: |BG|on|BK|, |BW|gt add me|BK| for rescue, |BW|gt remove me|BK| to get off.|N|;`  
`} `  
  
`#alias {roff} {`  
`  #var resc 0;`  
`  gt |BK|AUTO RESCUE: |R|off|N|;`  
`}`  
  
`#action {%1 tells the group 'add me'}{`  
`  #if {$resc == 1} {`  
`    #var input %1;`  
`    #if {"$input" == "*%**"} {gt |BK|Acknowledged, our illustrious leader, you are added to rescue list.;};`  
`    #else {gt |BK|Acknowledged, |BG|%1|BK|, you are added to rescue list.;};`  
`    #replace input {*}{};`  
`    #list RescueList ins 1 $input;`  
`  }`  
`}`  
  
`#action {%1 tells the group 'remove me'}{`  
`  #if {$resc == 1} {`  
`    #var input %1;`  
`    #if {"$input" == "*%**"} {gt |BK|Acknowledged, our illustrious leader, you are removed from the chicken list.;};`  
`    #else { gt |BK|Acknowledged, |BR|%1|BK|, you are removed from the rescue list.;};`  
`    #replace input {*}{};`  
`    #list RescueList fnd {$input} rdelindex;#list RescueList del {$rdelindex};`  
`  }`  
`}`  
  
`#alias {radd} {`  
`  #list RescueList ins 1 %1;`  
`  gt |BG|%1|BK| has been added to the rescue list.;`  
`}`  
  
`#alias {rdel} {`  
`  #list RescueList fnd {%1} rdelindex;`  
`  #list RescueList del {$rdelindex};`  
`  gt |R|%1|BK| has been removed from the rescue list.;`  
`}`  
  
`#alias {rblank} {`  
`  #list RescueList clr;`  
`  #var rindex 0;`  
`  gt |BK|Autorescue list has been |BW|blanked|BK|. Readd yourselves.;`  
`}`  
  
`#alias {rlist} {`  
`  #list RescueList size rindex;`  
`  #if {$rindex == 0} {gt |BK|Autorescue list is |BW|blank|BK|.|N|;};`  
`  #else {`  
`    #var totallist |BK|Autorescue list contains: |N|;`  
`    #loop 1 $rindex pvar {`  
`      #var totallist $totallist |BG|$RescueList[$pvar]|BK|,|N|;`  
`    };`  
`    gt $totallist;`  
`  };`  
`}`  
  
`#action {%1 attacks strike %2 %3 times, %4} {`  
`  #if {$resc == 1} {`  
`    #foreach $RescueList[%*] person #if {"$person" == "%2"} {rescue %2;}`  
`  }`  
`}`  
  
`#action {%1 attack strikes %2 %3 time, %4} {`  
`  #if {$resc == 1} {`  
`    #foreach $RescueList[%*] person #if {"$person" == "%2"} {rescue %2;}`  
`  }`  
`}`  
  
`#action {%1 attacks haven't hurt %2!} {`  
`  #if {$resc == 1} {`  
`    #foreach $RescueList[%*] person #if {"$person" == "%2"} {rescue %2;}`  
`  }`  
`}`

## Usage

Use **ron** and **roff** to toggle, **rblank** to clear, **radd** and
**rdell** to manually add or remove people, and **rlist** to display the
rescue list.

Please note that the script is not case-sensitive, so when manually
adding or removing people, use proper capitalization.

[Category: TinTin++ Scripting](Category:_TinTin++_Scripting "wikilink")
