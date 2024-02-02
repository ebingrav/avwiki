This is the Auto rescue set of triggers from the Avatar Wikipedia
Zmud_Auto_Rescue with multiple tweaks.

addrescue or addr adds someone to Rescue List

clearrescue clears Rescue List

showrescue shows Rescue List

dradd adds someone to the Don't Rescue List

autoaddr \# adds everyone in group that is \# HP or lower and will not
add anyone on the Don't Rescue List

pres augments then rescues. There is a check in the rescue trigger where
it checks if your name in variable @lastlogin is Zuno and rescues you

rescue checks if you are below 30% hp and if you are it tries to revive
you. It also checks if you are above your @rescuehp threshold to rescue.
For example if your @rescuehp is 13000 and you are at 12000 hp you will
not rescue

## Code

    #CLASS {autorescue|rescue}
    #CLASS {autorescue|rescue back on}
    #CLASS {autorescue|autoadd}
    #ALIAS addrescue {#ec autorescue - %1;#var rescuelist %additem( %lower( %1), @rescuelist)} "autorescue"
    #ALIAS clearrescue {#ye {clear the rescue list?} {yes:rescuelist="";#ec rescue buffer - cleared} {no:}} "autorescue"
    #ALIAS showrescue {#ec %null;#ec --- CHARACTERS IN THE RESCUE LIST ---;#ec %null;#fo @rescuelist {#ec %i}} "autorescue"
    #ALIAS ar {#var ar %1;#var report 1;#t+ report;#var frenzy 1;#if (@ar) {#t+ autorescue;#say Autorescue On} {#t- autorescue;#say Autorescue Off}}
    #ALIAS aprompt {prompt |BR|<%h/%Hhp>|BW|<%m/%Mm>|BY|<%v/%Vmv><%T><Lag:%s>%n;prompt2 <Mon:%u:%w/%W>%n;//prompt2 <Mon:%u:%w/%W><%jIQI/%J %kOQI/%K>%n}
    #ALIAS addr {addrescue %1}
    #ALIAS autoaddr {#var autoaddrhp %1;#t+ autorescue|autoadd;gr} "autorescue"
    #ALIAS remrescue {#ec don't rescue - %1;#var rescuelist %delitem( %lower( %1), @rescuelist);//#fo @rescuelist {#ec %i}}
    #ALIAS dradd {#var dontrescuelist %additem( %lower( %1), @dontrescuelist);#echo Added %1 to Don't Rescue list}
    #ALIAS pres {aug 3;rescue %1;aug off}
    #ALIAS rescue {#math hps_percent {@HP*100/@maxhp};#if (@hps_percent < 30) {rev};#if (@hp > @rescuehp) {#if (@rescuedelay > 0) {#untrig "+@rescuedelay";#alarm +@rescuedelay {#if (@lastlogin = Zuno) {aug 3};rescu %1;#if (@lastlogin = Zuno) {aug off}}} {#if (@lastlogin = Zuno) {aug 3};rescu %1;#if (@lastlogin = Zuno) {aug off}}}}
    #TRIGGER {(%*) attacks strike (%w)} {#if %ismember( %lower( %2), @rescuelist) {rescue %2;#t- rescue}} "autorescue|rescue"
    #TRIGGER {(%*) attacks haven't hurt (%w)} {#if %ismember( %lower( %2), @rescuelist) {rescue %2;#t- rescue}} "autorescue|rescue"
    #TRIGGER {(%*) {pierce|attack} strikes (%w)} {#if %ismember( %lower( %2), @rescuelist) {rescue %2;#t- rescue}} "autorescue|rescue"
    #TRIGGER {^(%*) is DEAD!!} {#T+ rescue} "autorescue|rescue back on"
    #TRIGGER {You successfully rescue} {#T+ rescue} "autorescue|rescue back on"
    #TRIGGER {doesn't need your help.} {#T+ rescue} "autorescue|rescue back on"
    #TRIGGER {You fail to rescue} {stance protect;#T+ rescue} "autorescue|rescue back on"
    #TRIGGER {doesn't NEED rescuing!} {#T+ rescue;look} "autorescue|rescue back on"
    #TRIGGER {(*) pokes you in the ribs} {rescue %1} "autorescue"
    #TRIGGER {(*) tells the group 'add me'} {addrescue %1} "autorescue"
    #TRIGGER {(*) tells the group 'rem me'} {remrescue %1} "autorescue"
    #TRIGGER {(*)'s backstab} {#if %ismember( %lower( %1), @rescuelist) {rescue %1;#t- rescue}} "autorescue|rescue"
    #TRIGGER {^{*}(*){*} tells the group 'get me'} {rescue %1} "autorescue"
    #TRIGGER {(*) tells the group 'remove me'} {remrescue %1} "autorescue"
    #TRIGGER {^*@leader* tells the group 'get (%1)'} {#var rescuetarget {%1};#if (@rescuetarget != me) {rescue %1} {rescue @leader}} "autorescue"
    #TRIGGER {^{*}(*){*} tells the group 'remove me'} {remrescue %1} "autorescue"
    #TRIGGER {^{*}(*){*} tells the group 'add me'} {addrescue %1} "autorescue"
    #TRIGGER {(*) tells the group 'get me'} {rescue %1} "autorescue"
    #TRIGGER {(*) {Lord|Hero} (*) {Stand|Fight|Busy|Sleep|Rest} (*)/(*) (*)/(*) (*)/(*)} {#var groupmemberhp %4;#if (@autoaddrhp > @groupmemberhp) {#if %ismember( %lower( %2), @dontrescuelist) {} {addr %2}}} "autorescue|autoadd"
    #TRIGGER {(*) joins @leader's group.} {autoaddr @autoaddrhp} "autorescue"
    #TRIGGER {You pale as you see death before you.} {gt I am feared! Please emotive drain me for more rescues} "autorescue"
    #TRIGGER {You consider rescuing (*), but chicken out!} {gt Feared, can't rescue :(} "autorescue"
    #TRIGGER {You finally get your nerves under control.} {gt I am no longer afraid! (Fear expired)} "autorescue"
    #TRIGGER {strikes down (%*)'s illusion!} {#if %ismember( %lower( %1), @rescuelist) {rescue %1;#t- rescue}} "autorescue|rescue"
    #TRIGGER {is here, fighting (*).} {#if %ismember( %lower( %1), @rescuelist) {rescue %1;#t- rescue}} "autorescue|rescue"
    #TRIGGER {^(%w)'s pierce strikes } {#if %ismember( %lower( %1), @rescuelist) {rescue %1;#t- rescue}} "autorescue|rescue"
    #TRIGGER {<(*)/(*)hp><(*)/(*)m><(*)>} {#if (@report) {#highlight;#var HP %1;#var maxHP %2;#var mana %3;#var maxMana %4}} "Report"
    #MENU {Rescue - Add} {addrescue %selword} "autorescue"
    #MENU {Rescue - Remove} {remrescue %selword} "autorescue"
    #MENU {Rescue - Show} {showrescue} "autorescue"
    #MENU {Rescue - Clear} {clearrescue} "autorescue"
