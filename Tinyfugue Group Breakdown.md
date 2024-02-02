Ok here goes, this is a paste of my group breakdown thingee. It also
includes modifcations to color medium/low hitpoints/mana of groupies and
probably some misc other stuff. Im not going to be to detailed, you can
ask me on avatar if you have problems.

Semi messy code, live with it or clean it up yourself :p

This is based of Waites [RoAClient Group
Breakdown](RoAClient_Group_Breakdown "wikilink") so the information
there might be useful also.

## Usage

-   /bd does the breakdown
-   /cp <start of person names> checks the groupline of person whose
    start of name matches whatever you type
-   /names shows a breakdown in gtell
-   /ptot and /tot shows percantage and number based current/total in
    gtell
-   /bdbrutes /bdfp /bdhealer /bdhitter shows just the fitting line
-   /list_steel /list_invinc steels or invinces all classes in group
    that probably cant cast the spell on themself (bark can be added
    same way, I just havent bothered since I dont have anything at lord
    that barks)

## Source code

    ;attempt to make colored hitpoint/mana in grouplist

    ;/def -ag -F -mregexp -t"((Sleep|Rest|Stand) +)([0-9]+)/([0-9]+)( *)([0-9]+)/([0-9]+)" group_format =\


    /def -mregexp -t"^##| Level" start_of_group_listing=\
        /set brutehp=0%;\
        /set brutehptotal= 0%;\
        /set fpmana=0%;\
        /set fpmanatotal=0%;\
        /set healermana=0%;\
        /set healermanatotal=0%;\


    /def -mregexp -ag -F -t"\|([ 0-9]+)(Lgnd|Lord|Hero) ([^ ]+)([^0-9]+)([0-9]+)/([0-9]+)([^0-9]*)([0-9]+)/([0-9]+)" group_format=\
    ;   /echo %P3 %P5 %P8%;\
        /set lastname=%P3%;\
        /if (do_last)\
            last %P3%;\
        /endif%;\
    ;   Check vars and do the summa summarum for stuff
        /set thisclass=$[determine_class({P3})]%;\
    ;   /eval /echo %thisclass%;\
        /if (thisclass =~ "fp")\
    ;       /echo YAY%;\
            /set fpmana=$[{fpmana} + {P8}]%;\
            /set fpmanatotal=$[{fpmanatotal} + {P9}]%;\
        /elseif (thisclass =~ "brute")\
            /set brutehp=$[{brutehp} + {P5}]%;\
            /set brutehptotal=$[{brutehptotal} + {P6}]%;\
        /elseif (thisclass =~ "healer")\
            /set healermana=$[{healermana} + {P8}]%;\
            /set healermanatotal=$[{healermanatotal} + {P9}]%;\
        /endif%;\
    ;   Health part 
        /set percent=$[strcat({P5}, ".0") / {P6}]%;\
        /if (percent <= 0.2)\
            /set health=@{Cred}%P5/%P6@{n}%;\
        /elseif (percent <= 0.4)\
            /set health=@{Cyellow}%P5/%P6@{n}%;\
        /else \
            /set health=%P5/%P6%;\
        /endif%;\
    ;   Mana part   
        /set percent=$[strcat({P8}, ".0") / {P9}]%;\
        /if (percent <= 0.2)\
            /set mana=@{Cred}%P8/%P9@{n}%;\
        /elseif (percent <= 0.4)\
            /set mana=@{Cyellow}%P8/%P9@{n}%;\
        /else \
            /set mana=%P8/%P9%;\
        /endif%;\
    ;   Echo part
        /if (checkperson)\
            /if (%P3 =/ strcat(%checkpersonstring,'*'))\
                /test echo("%PL|%P1%P2 %P3%P4%health%P7%mana%PR", "Ccyan", 1)%;\
            /endif%;\
        /else \
            /test echo("%PL|%P1%P2 %P3%P4%health%P7%mana%PR", "Ccyan", 1)%;\
        /endif%;\

    /def cp =\
        /set checkperson=1%;\
        /set checkpersonstring=%1%;\
        group%;\
        /repeat -5 1 /set checkperson=0%;

    /def bd =\
        /breakdown%;
        
    /def breakdown =\
        /unset brutestring%;\
        /unset firestring%;\
        /unset healerstring%;\
        /unset hitterstring%;\
        /set brutecount=0%;\
        /set firepowercount=0%;\
        /set healercount=0%;\
        /set hittercount=0%;\
        /set bzkcount=0%;\
        /set warcount=0%;\
        /set bodcount=0%;\
        /set palcount=0%;\
        /set rancount=0%;\
        /set moncount=0%;\
        /set shfcount=0%;\
        /set sorcount=0%;\
        /set magcount=0%;\
        /set wzdcount=0%;\
        /set psicount=0%;\
        /set mndcount=0%;\
        /set stmcount=0%;\
        /set prscount=0%;\
        /set clecount=0%;\
        /set drucount=0%;\
        /set asncount=0%;\
        /set rogcount=0%;\
        /set bcicount=0%;\
        /set arccount=0%;\
        /set fuscount=0%;\
        /set bzknames=%;\
        /set warnames=%;\
        /set bodnames=%;\
        /set palnames=%;\
        /set rannames=%;\
        /set monnames=%;\
        /set shfnames=%;\
        /set sornames=%;\
        /set magnames=%;\
        /set wzdnames=%;\
        /set psinames=%;\
        /set mndnames=%;\
        /set stmnames=%;\
        /set prsnames=%;\
        /set clenames=%;\
        /set drunames=%;\
        /set asnnames=%;\
        /set rognames=%;\
        /set bcinames=%;\
        /set arcnames=%;\
        /set fusnames=%;\
        /set groupnames=%;\
        /set fpnames=%;\
        /set brutenames=%;\
        /set healernames=%;\
        /set hitternames=%;\
        /set needinvinc=%;\
        /set needsteel=%;\
        /set do_last=1%;\
        /def -mregexp -t"(^[^ ]+) the .+ is a [^ ]+ level (Lord|Hero|Legend) ([^,]*)," check_last =\
            /set groupnames=%%groupnames%%PL\\-%%;\
            /if (%%{P3} =~ "Berserker")\
                /set bzkcount=$$[bzkcount + 1]%%;\
                /set brutecount=$$[brutecount + 1]%%;\
                /set bzknames=%%bzknames %%P1%%;\
                /set brutenames=%%brutenames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Warrior")\
                /set warcount=$$[warcount + 1]%%;\
                /set brutecount=$$[brutecount + 1]%%;\
                /set warnames=%%warnames %%P1%%;\
                /set brutenames=%%brutenames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Bodyguard")\
                /set bodcount=$$[bodcount + 1]%%;\
                /set brutecount=$$[brutecount + 1]%%;\
                /set bodnames=%%bodnames %%P1%%;\
                /set brutenames=%%brutenames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Paladin")\
                /set palcount=$$[palcount + 1]%%;\
                /set brutecount=$$[brutecount + 1]%%;\
                /set palnames=%%palnames %%P1%%;\
                /set brutenames=%%brutenames%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Ranger")\
                /set rancount=$$[rancount + 1]%%;\
                /set brutecount=$$[brutecount + 1]%%;\
                /set rannames=%%rannames %%P1%%;\
                /set brutenames=%%brutenames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Monk")\
                /set moncount=$$[moncount + 1]%%;\
                /set brutecount=$$[brutecount + 1]%%;\
                /set monnames=%%monnames %%P1%%;\
                /set brutenames=%%brutenames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Shadowfist")\
                /set shfcount=$$[shfcount + 1]%%;\
                /set brutecount=$$[brutecount + 1]%%;\
                /set shfnames=%%shfnames %%P1%%;\
                /set brutenames=%%brutenames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Sorcerer")\
                /set sorcount=$$[sorcount + 1]%%;\
                /set firepowercount=$$[firepowercount + 1]%%;\
                /set sornames=%%sornames %%P1%%;\
                /set fpnames=%%fpnames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Mage")\
                /set magcount=$$[magcount + 1]%%;\
                /set firepowercount=$$[firepowercount + 1]%%;\
                /set magnames=%%magnames %%P1%%;\
                /set fpnames=%%fpnames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Wizard")\
                /set wzdcount=$$[wzdcount + 1]%%;\
                /set firepowercount=$$[firepowercount + 1]%%;\
                /set wzdnames=%%wzdnames %%P1%%;\
                /set fpnames=%%fpnames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Psionicist")\
                /set psicount=$$[psicount + 1]%%;\
                /set firepowercount=$$[firepowercount + 1]%%;\
                /set psinames=%%psinames %%P1%%;\
                /set fpnames=%%fpnames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
            /elseif (%%{P3} =~ "Mindbender")\
                /set mndcount=$$[mndcount + 1]%%;\
                /set firepowercount=$$[firepowercount + 1]%%;\
                /set mndnames=%%mndnames %%P1%%;\
                /set fpnames=%%fpnames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
            /elseif (%%{P3} =~ "Stormlord")\
                /set stmcount=$$[stmcount + 1]%%;\
                /set firepowercount=$$[firepowercount + 1]%%;\
                /set stmnames=%%stmnames %%P1%%;\
                /set fpnames=%%fpnames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Priest")\
                /set prscount=$$[prscount + 1]%%;\
                /set healercount=$$[healercount + 1]%%;\
                /set prsnames=%%prsnames %%P1%%;\
                /set healernames=%%healernames%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Cleric")\
                /set clecount=$$[clecount + 1]%%;\
                /set healercount=$$[healercount + 1]%%;\
                /set clenames=%%clenames %%P1%%;\
                /set healernames=%%healernames%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Druid")\
                /set drucount=$$[drucount + 1]%%;\
                /set healercount=$$[healercount + 1]%%;\
                /set drunames=%%drunames %%P1%%;\
                /set healernames=%%healernames%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Assassin")\
                /set asncount=$$[asncount + 1]%%;\
                /set hittercount=$$[hittercount + 1]%%;\
                /set asnnames=%%asnnames %%P1%%;\
                /set hitternames=%%hitternames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Rogue")\
                /set rogcount=$$[rogcount + 1]%%;\
                /set hittercount=$$[hittercount + 1]%%;\
                /set rognames=%%rognames %%P1%%;\
                /set hitternames=%%hitternames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Black Circle Initiate")\
                /set bcicount=$$[bcicount + 1]%%;\
                /set hittercount=$$[hittercount + 1]%%;\
                /set bcinames=%%bcinames %%P1%%;\
                /set hitternames=%%hitternames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
            /elseif (%%{P3} =~ "Archer")\
                /set arccount=$$[arccount+1]%%;\
                /set hittercount=$$[hittercount + 1]%%;\
                /set arcnames=%%arcnames %%P1%%;\
                /set hitternames=%%hitternames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /elseif (%%{P3} =~ "Fusilier")\
                /set fuscount=$$[fuscount+1]%%;\
                /set hittercount=$$[hittercount + 1]%%;\
                /set fusnames=%%fusnames %%P1%%;\
                /set hitternames=%%hitternames%%P1\\-%%;\
                /set needinvinc=%%needinvinc%%P1\\-%%;\
                /set needsteel=%%needsteel%%P1\\-%%;\
            /endif%%;\
            /if (%%{lastname} =~ %%{P1})\
                /break2%%;\
            /endif%;\
        group%;

    /def break2 =\
        /set do_last=0%;\
        /undef check_last%;\
        /set bzknames=$(/strchomp %bzknames)%;\
        /set warnames=$(/strchomp %warnames)%;\
        /set bodnames=$(/strchomp %bodnames)%;\
        /set palnames=$(/strchomp %palnames)%;\
        /set rannames=$(/strchomp %rannames)%;\
        /set monnames=$(/strchomp %monnames)%;\
        /set shfnames=$(/strchomp %shfnames)%;\
        /set sornames=$(/strchomp %sornames)%;\
        /set magnames=$(/strchomp %magnames)%;\
        /set wzdnames=$(/strchomp %wzdnames)%;\
        /set psinames=$(/strchomp %psinames)%;\
        /set mndnames=$(/strchomp %mndnames)%;\
        /set stmnames=$(/strchomp %stmnames)%;\
        /set prsnames=$(/strchomp %prsnames)%;\
        /set clenames=$(/strchomp %clenames)%;\
        /set drunames=$(/strchomp %drunames)%;\
        /set asnnames=$(/strchomp %asnnames)%;\
        /set rognames=$(/strchomp %rognames)%;\
        /set bcinames=$(/strchomp %bcinames)%;\
        /set arcnames=$(/strchomp %arcnames)%;\
        /set fusnames=$(/strchomp %fusnames)%;\
        /set brutestring=|c|%brutecount Brute(s)%;\
        /set firestring=|r|%firepowercount Firepower%;\
        /set healerstring=|w|%healercount Healer(s)%;\
        /set hitterstring=|g|%hittercount Hitter(s)%;\
        /if (bzkcount > 0)\
            /set brutestring=%brutestring |c|Bzk(|bc|%bzkcount|c|) {%bzknames}%;\
        /endif%;\
        /if (warcount > 0)\
            /set brutestring=%brutestring |c|War(|bc|%warcount|c|) {%warnames}%;\
        /endif%;\
        /if (bodcount > 0)\
            /set brutestring=%brutestring |c|Bod(|bc|%bodcount|c|) {%bodnames}%;\
        /endif%;\
        /if (palcount > 0)\
            /set brutestring=%brutestring |c|Pal(|bc|%palcount|c|) {%palnames}%;\
        /endif%;\
        /if (rancount > 0)\
            /set brutestring=%brutestring |c|Ran(|bc|%rancount|c|) {%rannames}%;\
        /endif%;\
        /if (moncount > 0)\
            /set brutestring=%brutestring |c|Mon(|bc|%moncount|c|) {%monnames}%;\
        /endif%;\
        /if (shfcount > 0)\
            /set brutestring=%brutestring |c|Shf(|bc|%shfcount|c|) {%shfnames}%;\
        /endif%;\
        /if (sorcount > 0)\
            /set firestring=%firestring |r|Sor(|br|%sorcount|r|) {%sornames}%;\
        /endif%;\
        /if (magcount > 0)\
            /set firestring=%firestring |r|Mag(|br|%magcount|r|) {%magnames}%;\
        /endif%;\
        /if (wzdcount > 0)\
            /set firestring=%firestring |r|Wzd(|br|%wzdcount|r|) {%wzdnames}%;\
        /endif%;\
        /if (psicount > 0)\
            /set firestring=%firestring |r|Psi(|br|%psicount|r|) {%psinames}%;\
        /endif%;\
        /if (mndcount > 0)\
            /set firestring=%firestring |r|Mnd(|br|%mndcount|r|) {%mndnames}%;\
        /endif%;\
        /if (stmcount > 0)\
            /set firestring=%firestring |r|Stm(|br|%stmcount|r|) {%stmnames}%;\
        /endif%;\
        /if (prscount > 0)\
            /set healerstring=%healerstring |w|Prs(|bw|%prscount|w|) {%prsnames}%;\
        /endif%;\
        /if (clecount > 0)\
            /set healerstring=%healerstring |w|Cle(|bw|%clecount|w|) {%clenames}%;\
        /endif%;\
        /if (drucount > 0)\
            /set healerstring=%healerstring |w|Dru(|bw|%drucount|w|) {%drunames}%;\
        /endif%;\
        /if (asncount > 0)\
            /set hitterstring=%hitterstring |g|Asn(|bg|%asncount|g|) {%asnnames}%;\
        /endif%;\
        /if (rogcount > 0)\
            /set hitterstring=%hitterstring |g|Rog(|bg|%rogcount|g|) {%rognames}%;\
        /endif%;\
        /if (bcicount > 0)\
            /set hitterstring=%hitterstring |g|Bci(|bg|%bcicount|g|) {%bcinames}%;\
        /endif%;\
        /if (arccount > 0)\
            /set hitterstring=%hitterstring |g|Arc(|bg|%arccount|g|) {%arcnames}%;\
        /endif%;\
        /if (fuscount > 0)\
            /set hitterstring=%hitterstring |g|Fus(|bg|%fuscount|g|) {%fusnames}%;\
        /endif%;\


    /def ptot=\
        group%;\
        /repeat -4 1 /ptot_do%; 

    /def ptot_do=\
        /set doptot=0%;\
        /if ({brutehptotal} > 0)\
            /let brutepercent=$[%brutehp*100/%brutehptotal]%;\
        /else%;\
            /let brutepercent=0%;\
        /endif%;\
        /if ({fpmanatotal} > 0)\
            /let fppercent=$[%fpmana*100/%fpmanatotal]%;\
        /else%;\
            /let fppercent=0%;\
        /endif%;\
        /if ({healermanatotal} > 0)\
            /let healerpercent=$[%healermana*100/%healermanatotal]%;\
        /else%;\
            /let healerpercent=0%;\
        /endif%;\
        gt |g|Brute hp: \{|bg|%brutepercent\%|g|\} |r|Firepower mana: \{|br|%fppercent\%|r|\} |w|Healer mana: \{|bw|%healerpercent\%|w|\}
         

    /def tot=\
        group%;\
        /repeat -4 1 /tot_do%; 

    /def tot_do=\
        /set dotot=0%;\
        gt |g|Brute hp: \{|bg|%brutehp/%brutehptotal|g|\} |r|Firepower mana: \{|br|%fpmana/%fpmanatotal|r|\} |w|Healer mana: \{|bw|%healermana/%healermanatotal|w|\} %;\

    /def names =\
        gt %brutestring%;\
        gt %firestring%;\
        gt %healerstring%;\
        gt %hitterstring%;\

    /def bdbrutes =\
        gt %brutestring%;\

    /def bdfp =\
        gt %firestring%;\

    /def bdhealer =\
        gt %healerstring%;\

    /def bdhitter =\
        gt %hitterstring%;\

    /def determine_class = \
        /let detname=$[strcat({*}, "-")]%;\
        /if (strstr({brutenames}, {detname}) != -1)\
            /return "brute"%;\
        /endif%;\
        /if (strstr({fpnames}, {detname}) != -1)\
            /return "fp"%;\
        /endif%;\
        /if (strstr({healernames}, {detname}) != -1)\
            /return "healer"%;\
        /endif%;\
        /if (strstr({hitternames}, {detname}) != -1)\
            /return "hitter"%;\
        /endif%;\
        /return "Unknown"%;\


    /def strchomp =\
        /set chomped={*}%;\
        /while ( substr(%chomped,0,1) =~ " ")\
            /eval /set chomped=$[substr(%chomped,1)]%;\
        /done%;\
        /while (substr(%chomped,-1,1) =~ " ")\
            /eval /set chomped=$[substr(%chomped,0,-1)]%;\
        /done%;\
        /result %{chomped}%;\


    ;for internal use by list macros
    /set listspell=bogus;
    /def listspell =\
        /let namelist=$[{*}]%;\
        /let limit=$[strstr(%namelist, "-")]%;\
        /while ( {limit} != -1)\
            /let thisname=$[substr(%namelist, 0, %limit)]%;\
            /let namelist=$[substr(%namelist, %limit+1)]%;\
            /let limit=$[strstr(%namelist, "-")]%;\
            cast %listspell %thisname%;\
        /done
        
    /def list_steel =\
        /set listspell=steel%;\
        /listspell %needsteel
        

    /def list_invinc =\
        /set listspell=invincibility%;\
        /listspell %needinvinc

## Syntax highlighted source code

Syntax highlighted code made using vims :TOhtml

<font color="#0000ff">`;attempt to make colored hitpoint/mana in grouplist`</font>  
  
<font color="#0000ff">`;/def -ag -F -mregexp -t"((Sleep|Rest|Stand) +)([0-9]+)/([0-9]+)( *)([0-9]+)/([0-9]+)" group_format =\`</font>  
  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` `<font color="#804040"><b>`-`</b></font>`mregexp `<font color="#804040"><b>`-`</b></font>`t"`<font color="#ff00ff">`^##| Level`</font>`" start_of_group_listing`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutehp`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutehptotal`<font color="#804040"><b>`=`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpmana`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpmanatotal`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healermana`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healermanatotal`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` `<font color="#804040"><b>`-`</b></font>`mregexp `<font color="#804040"><b>`-`</b></font>`ag `<font color="#804040"><b>`-`</b></font>`F `<font color="#804040"><b>`-`</b></font>`t"`<font color="#ff00ff">`\|([ 0-9]+)(Lgnd|Lord|Hero) ([^ ]+)([^0-9]+)([0-9]+)/([0-9]+)([^0-9]*)([0-9]+)/([0-9]+)`</font>`" group_format`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
<font color="#0000ff">`;       /echo %P3 %P5 %P8%;\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` lastname`<font color="#804040"><b>`=`</b></font><font color="#008080">`%`</font><font color="#008080">`P3`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`do_last`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                last `<font color="#008080">`%`</font><font color="#008080">`P3`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
<font color="#0000ff">`;       Check vars and do the summa summarum for stuff`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` thisclass`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font>`determine_class`<font color="#6a5acd">`({`</font><font color="#008080">`P3`</font><font color="#6a5acd">`})]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
<font color="#0000ff">`;       /eval /echo %thisclass%;\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`thisclass `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`fp`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
<font color="#0000ff">`;               /echo YAY%;\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpmana`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[{`</font>`fpmana`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`+`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P8`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpmanatotal`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[{`</font>`fpmanatotal`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`+`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P9`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font>`thisclass `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`brute`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutehp`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[{`</font>`brutehp`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`+`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P5`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutehptotal`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[{`</font>`brutehptotal`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`+`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P6`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font>`thisclass `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`healer`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healermana`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[{`</font>`healermana`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`+`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P8`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healermanatotal`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[{`</font>`healermanatotal`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`+`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P9`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
<font color="#0000ff">`;       Health part     `</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` percent`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`strcat`</font><font color="#6a5acd">`({`</font><font color="#008080">`P5`</font><font color="#6a5acd">`}`</font>`, "`<font color="#ff00ff">`.0`</font>`"`<font color="#6a5acd">`)`</font>` `<font color="#804040"><b>`/`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P6`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`percent `<font color="#804040"><b>`<`</b></font><font color="#804040"><b>`=`</b></font>` `<font color="#ff00ff">`0`</font><font color="#ff00ff">`.2`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` health`<font color="#804040"><b>`=`</b></font>`@`<font color="#6a5acd">`{`</font>`Cred`<font color="#6a5acd">`}`</font><font color="#008080">`%`</font><font color="#008080">`P5`</font><font color="#804040"><b>`/`</b></font>`%P6@`<font color="#6a5acd">`{`</font>`n`<font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font>`percent `<font color="#804040"><b>`<`</b></font><font color="#804040"><b>`=`</b></font>` `<font color="#ff00ff">`0`</font><font color="#ff00ff">`.4`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` health`<font color="#804040"><b>`=`</b></font>`@`<font color="#6a5acd">`{`</font>`Cyellow`<font color="#6a5acd">`}`</font><font color="#008080">`%`</font><font color="#008080">`P5`</font><font color="#804040"><b>`/`</b></font>`%P6@`<font color="#6a5acd">`{`</font>`n`<font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`else`</b></font>` `<font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` health`<font color="#804040"><b>`=`</b></font><font color="#008080">`%`</font><font color="#008080">`P5`</font><font color="#804040"><b>`/`</b></font>`%P6`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
<font color="#0000ff">`;       Mana part       `</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` percent`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`strcat`</font><font color="#6a5acd">`({`</font><font color="#008080">`P8`</font><font color="#6a5acd">`}`</font>`, "`<font color="#ff00ff">`.0`</font>`"`<font color="#6a5acd">`)`</font>` `<font color="#804040"><b>`/`</b></font>` `<font color="#6a5acd">`{`</font><font color="#008080">`P9`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`percent `<font color="#804040"><b>`<`</b></font><font color="#804040"><b>`=`</b></font>` `<font color="#ff00ff">`0`</font><font color="#ff00ff">`.2`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mana`<font color="#804040"><b>`=`</b></font>`@`<font color="#6a5acd">`{`</font>`Cred`<font color="#6a5acd">`}`</font><font color="#008080">`%`</font><font color="#008080">`P8`</font><font color="#804040"><b>`/`</b></font>`%P9@`<font color="#6a5acd">`{`</font>`n`<font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font>`percent `<font color="#804040"><b>`<`</b></font><font color="#804040"><b>`=`</b></font>` `<font color="#ff00ff">`0`</font><font color="#ff00ff">`.4`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mana`<font color="#804040"><b>`=`</b></font>`@`<font color="#6a5acd">`{`</font>`Cyellow`<font color="#6a5acd">`}`</font><font color="#008080">`%`</font><font color="#008080">`P8`</font><font color="#804040"><b>`/`</b></font>`%P9@`<font color="#6a5acd">`{`</font>`n`<font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`else`</b></font>` `<font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mana`<font color="#804040"><b>`=`</b></font><font color="#008080">`%`</font><font color="#008080">`P8`</font><font color="#804040"><b>`/`</b></font>`%P9`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
<font color="#0000ff">`;       Echo part`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`checkperson`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%`</font><font color="#008080">`P3`</font>` `<font color="#804040"><b>`=/`</b></font>` `<font color="#008080">`strcat`</font><font color="#6a5acd">`(`</font><font color="#008080">`%checkpersonstring`</font>`,'`<font color="#ff00ff">`*`</font>`'`<font color="#6a5acd">`))`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`test`</b></font>` `<font color="#008080">`echo`</font><font color="#6a5acd">`(`</font>`"`<font color="#008080">`%`</font><font color="#008080">`PL`</font><font color="#ff00ff">`|`</font><font color="#008080">`%`</font><font color="#008080">`P1`</font><font color="#008080">`%`</font><font color="#008080">`P2`</font><font color="#ff00ff">` `</font><font color="#008080">`%`</font><font color="#008080">`P3`</font><font color="#008080">`%`</font><font color="#008080">`P4`</font><font color="#008080">`%health%`</font><font color="#008080">`P7`</font><font color="#008080">`%mana%`</font><font color="#008080">`PR`</font>`", "`<font color="#ff00ff">`Ccyan`</font>`", `<font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`else`</b></font>` `<font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`test`</b></font>` `<font color="#008080">`echo`</font><font color="#6a5acd">`(`</font>`"`<font color="#008080">`%`</font><font color="#008080">`PL`</font><font color="#ff00ff">`|`</font><font color="#008080">`%`</font><font color="#008080">`P1`</font><font color="#008080">`%`</font><font color="#008080">`P2`</font><font color="#ff00ff">` `</font><font color="#008080">`%`</font><font color="#008080">`P3`</font><font color="#008080">`%`</font><font color="#008080">`P4`</font><font color="#008080">`%health%`</font><font color="#008080">`P7`</font><font color="#008080">`%mana%`</font><font color="#008080">`PR`</font>`", "`<font color="#ff00ff">`Ccyan`</font>`", `<font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` cp `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` checkperson`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` checkpersonstring`<font color="#804040"><b>`=`</b></font>`%`<font color="#ff00ff">`1`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        group`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`repeat`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`5`</font>` `<font color="#ff00ff">`1`</font>` `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` checkperson`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` bd `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/breakdown`</b></font><font color="#6a5acd">`%;`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` breakdown `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`unset`</font>` brutestring`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`unset`</font>` firestring`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`unset`</font>` healerstring`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`unset`</font>` hitterstring`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firepowercount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healercount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hittercount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bzkcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` warcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bodcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` palcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rancount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` moncount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` shfcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` sorcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` magcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` wzdcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` psicount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mndcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` stmcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` prscount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` clecount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` drucount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` asncount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rogcount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bcicount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` arccount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fuscount`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bzknames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` warnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bodnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` palnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rannames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` monnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` shfnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` sornames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` magnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` wzdnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` psinames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mndnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` stmnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` prsnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` clenames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` drunames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` asnnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rognames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bcinames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` arcnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fusnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` groupnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpnames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healernames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitternames`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` do_last`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` `<font color="#804040"><b>`-`</b></font>`mregexp `<font color="#804040"><b>`-`</b></font>`t"`<font color="#ff00ff">`(^[^ ]+) the .+ is a [^ ]+ level (Lord|Hero|Legend) ([^,]*),`</font>`" check_last `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` groupnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%groupnames%%`</font><font color="#008080">`PL`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Berserker`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bzkcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`bzkcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`brutecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bzknames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%bzknames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%brutenames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Warrior`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` warcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`warcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`brutecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` warnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%warnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%brutenames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Bodyguard`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bodcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`bodcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`brutecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bodnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%bodnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%brutenames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Paladin`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` palcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`palcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`brutecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` palnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%palnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%brutenames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Ranger`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rancount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`rancount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`brutecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rannames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%rannames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%brutenames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Monk`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` moncount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`moncount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`brutecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` monnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%monnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%brutenames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Shadowfist`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` shfcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`shfcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`brutecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` shfnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%shfnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%brutenames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Sorcerer`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` sorcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`sorcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firepowercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`firepowercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` sornames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%sornames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%fpnames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Mage`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` magcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`magcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firepowercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`firepowercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` magnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%magnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%fpnames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Wizard`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` wzdcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`wzdcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firepowercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`firepowercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` wzdnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%wzdnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%fpnames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Psionicist`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` psicount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`psicount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firepowercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`firepowercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` psinames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%psinames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%fpnames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Mindbender`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mndcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`mndcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firepowercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`firepowercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mndnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%mndnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%fpnames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Stormlord`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` stmcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`stmcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firepowercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`firepowercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` stmnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%stmnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fpnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%fpnames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Priest`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` prscount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`prscount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`healercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` prsnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%prsnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healernames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%healernames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Cleric`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` clecount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`clecount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`healercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` clenames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%clenames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healernames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%healernames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Druid`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` drucount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`drucount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`healercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` drunames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%drunames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healernames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%healernames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Assassin`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` asncount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`asncount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hittercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`hittercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` asnnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%asnnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitternames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%hitternames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Rogue`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rogcount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`rogcount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hittercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`hittercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rognames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%rognames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitternames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%hitternames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Black Circle Initiate`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bcicount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`bcicount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hittercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`hittercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bcinames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%bcinames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitternames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%hitternames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Archer`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` arccount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`arccount`<font color="#804040"><b>`+`</b></font><font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hittercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`hittercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` arcnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%arcnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitternames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%hitternames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`elseif`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P3`</font><font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">`Fusilier`</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fuscount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`fuscount`<font color="#804040"><b>`+`</b></font><font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hittercount`<font color="#804040"><b>`=$$`</b></font><font color="#6a5acd">`[`</font>`hittercount `<font color="#804040"><b>`+`</b></font>` `<font color="#008080">`1`</font><font color="#6a5acd">`]`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fusnames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%fusnames`</font>` `<font color="#008080">`%%`</font><font color="#008080">`P1`</font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitternames`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%hitternames%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needinvinc`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needinvinc%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` needsteel`<font color="#804040"><b>`=`</b></font><font color="#008080">`%%needsteel%%`</font><font color="#008080">`P1`</font>`\\`<font color="#804040"><b>`-`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`%%`</font><font color="#6a5acd">`{`</font>`lastname`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`=~`</b></font>` `<font color="#008080">`%%`</font><font color="#6a5acd">`{`</font><font color="#008080">`P1`</font><font color="#6a5acd">`})`</font><font color="#6a5acd">`\`</font>  
`                        `<font color="#2e8b57"><b>`/break2`</b></font><font color="#6a5acd">`%%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        group`<font color="#6a5acd">`%;`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` break2 `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` do_last`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`undef`</font>` check_last`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bzknames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%bzknames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` warnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%warnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bodnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%bodnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` palnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%palnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rannames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%rannames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` monnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%monnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` shfnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%shfnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` sornames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%sornames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` magnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%magnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` wzdnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%wzdnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` psinames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%psinames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` mndnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%mndnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` stmnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%stmnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` prsnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%prsnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` clenames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%clenames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` drunames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%drunames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` asnnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%asnnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` rognames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%rognames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` bcinames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%bcinames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` arcnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%arcnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` fusnames`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`(`</font><font color="#2e8b57"><b>`/strchomp`</b></font>` `<font color="#008080">`%fusnames`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#008080">`%brutecount`</font>` Brute`<font color="#6a5acd">`(`</font>`s`<font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firestring`<font color="#804040"><b>`=|`</b></font>`r`<font color="#804040"><b>`|`</b></font><font color="#008080">`%firepowercount`</font>` Firepower`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healerstring`<font color="#804040"><b>`=|`</b></font>`w`<font color="#804040"><b>`|`</b></font><font color="#008080">`%healercount`</font>` Healer`<font color="#6a5acd">`(`</font>`s`<font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitterstring`<font color="#804040"><b>`=|`</b></font>`g`<font color="#804040"><b>`|`</b></font><font color="#008080">`%hittercount`</font>` Hitter`<font color="#6a5acd">`(`</font>`s`<font color="#6a5acd">`)`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`bzkcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%brutestring`</font>` `<font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font>`Bzk`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bc`<font color="#804040"><b>`|`</b></font><font color="#008080">`%bzkcount`</font><font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%bzknames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`warcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%brutestring`</font>` `<font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font>`War`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bc`<font color="#804040"><b>`|`</b></font><font color="#008080">`%warcount`</font><font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%warnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`bodcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%brutestring`</font>` `<font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font>`Bod`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bc`<font color="#804040"><b>`|`</b></font><font color="#008080">`%bodcount`</font><font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%bodnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`palcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%brutestring`</font>` `<font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font>`Pal`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bc`<font color="#804040"><b>`|`</b></font><font color="#008080">`%palcount`</font><font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%palnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`rancount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%brutestring`</font>` `<font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font>`Ran`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bc`<font color="#804040"><b>`|`</b></font><font color="#008080">`%rancount`</font><font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%rannames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`moncount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%brutestring`</font>` `<font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font>`Mon`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bc`<font color="#804040"><b>`|`</b></font><font color="#008080">`%moncount`</font><font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%monnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`shfcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` brutestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%brutestring`</font>` `<font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font>`Shf`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bc`<font color="#804040"><b>`|`</b></font><font color="#008080">`%shfcount`</font><font color="#804040"><b>`|`</b></font>`c`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%shfnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`sorcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%firestring`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Sor`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%sorcount`</font><font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%sornames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`magcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%firestring`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Mag`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%magcount`</font><font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%magnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`wzdcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%firestring`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Wzd`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%wzdcount`</font><font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%wzdnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`psicount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%firestring`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Psi`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%psicount`</font><font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%psinames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`mndcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%firestring`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Mnd`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%mndcount`</font><font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%mndnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`stmcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` firestring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%firestring`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Stm`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%stmcount`</font><font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%stmnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`prscount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healerstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%healerstring`</font>` `<font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font>`Prs`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bw`<font color="#804040"><b>`|`</b></font><font color="#008080">`%prscount`</font><font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%prsnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`clecount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healerstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%healerstring`</font>` `<font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font>`Cle`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bw`<font color="#804040"><b>`|`</b></font><font color="#008080">`%clecount`</font><font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%clenames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`drucount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` healerstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%healerstring`</font>` `<font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font>`Dru`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bw`<font color="#804040"><b>`|`</b></font><font color="#008080">`%drucount`</font><font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%drunames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`asncount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitterstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%hitterstring`</font>` `<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`Asn`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bg`<font color="#804040"><b>`|`</b></font><font color="#008080">`%asncount`</font><font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%asnnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`rogcount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitterstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%hitterstring`</font>` `<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`Rog`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bg`<font color="#804040"><b>`|`</b></font><font color="#008080">`%rogcount`</font><font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%rognames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`bcicount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitterstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%hitterstring`</font>` `<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`Bci`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bg`<font color="#804040"><b>`|`</b></font><font color="#008080">`%bcicount`</font><font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%bcinames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`arccount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitterstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%hitterstring`</font>` `<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`Arc`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bg`<font color="#804040"><b>`|`</b></font><font color="#008080">`%arccount`</font><font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%arcnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font>`fuscount `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` hitterstring`<font color="#804040"><b>`=`</b></font><font color="#008080">`%hitterstring`</font>` `<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`Fus`<font color="#6a5acd">`(`</font><font color="#804040"><b>`|`</b></font>`bg`<font color="#804040"><b>`|`</b></font><font color="#008080">`%fuscount`</font><font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font><font color="#6a5acd">`)`</font>` `<font color="#6a5acd">`{`</font><font color="#008080">`%fusnames`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` ptot`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        group`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`repeat`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`4`</font>` `<font color="#ff00ff">`1`</font>` `<font color="#2e8b57"><b>`/ptot_do`</b></font><font color="#6a5acd">`%;`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` ptot_do`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` doptot`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`({`</font>`brutehptotal`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` brutepercent`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`%brutehp`</font><font color="#008080">`*100`</font><font color="#804040"><b>`/`</b></font>`%brutehptotal`<font color="#6a5acd">`]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`else`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` brutepercent`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`({`</font>`fpmanatotal`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` fppercent`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`%fpmana`</font><font color="#008080">`*100`</font><font color="#804040"><b>`/`</b></font>`%fpmanatotal`<font color="#6a5acd">`]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`else`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` fppercent`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`({`</font>`healermanatotal`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`>`</b></font>` `<font color="#ff00ff">`0`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` healerpercent`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`%healermana`</font><font color="#008080">`*100`</font><font color="#804040"><b>`/`</b></font>`%healermanatotal`<font color="#6a5acd">`]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`else`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` healerpercent`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`Brute hp`<font color="#804040"><b>`:`</b></font>` \`<font color="#6a5acd">`{`</font><font color="#804040"><b>`|`</b></font>`bg`<font color="#804040"><b>`|`</b></font><font color="#008080">`%brutepercent`</font>`\%`<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`\`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Firepower mana`<font color="#804040"><b>`:`</b></font>` \`<font color="#6a5acd">`{`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%fppercent`</font>`\%`<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`\`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font>`Healer mana`<font color="#804040"><b>`:`</b></font>` \`<font color="#6a5acd">`{`</font><font color="#804040"><b>`|`</b></font>`bw`<font color="#804040"><b>`|`</b></font><font color="#008080">`%healerpercent`</font>`\%`<font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font>`\`<font color="#6a5acd">`}`</font>  
  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` tot`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        group`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`repeat`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`4`</font>` `<font color="#ff00ff">`1`</font>` `<font color="#2e8b57"><b>`/tot_do`</b></font><font color="#6a5acd">`%;`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` tot_do`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` dotot`<font color="#804040"><b>`=`</b></font><font color="#ff00ff">`0`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`Brute hp`<font color="#804040"><b>`:`</b></font>` \`<font color="#6a5acd">`{`</font><font color="#804040"><b>`|`</b></font>`bg`<font color="#804040"><b>`|`</b></font><font color="#008080">`%brutehp`</font><font color="#804040"><b>`/`</b></font>`%brutehptotal`<font color="#804040"><b>`|`</b></font>`g`<font color="#804040"><b>`|`</b></font>`\`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`Firepower mana`<font color="#804040"><b>`:`</b></font>` \`<font color="#6a5acd">`{`</font><font color="#804040"><b>`|`</b></font>`br`<font color="#804040"><b>`|`</b></font><font color="#008080">`%fpmana`</font><font color="#804040"><b>`/`</b></font>`%fpmanatotal`<font color="#804040"><b>`|`</b></font>`r`<font color="#804040"><b>`|`</b></font>`\`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font>`Healer mana`<font color="#804040"><b>`:`</b></font>` \`<font color="#6a5acd">`{`</font><font color="#804040"><b>`|`</b></font>`bw`<font color="#804040"><b>`|`</b></font><font color="#008080">`%healermana`</font><font color="#804040"><b>`/`</b></font>`%healermanatotal`<font color="#804040"><b>`|`</b></font>`w`<font color="#804040"><b>`|`</b></font>`\`<font color="#6a5acd">`}`</font>` `<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` names `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%brutestring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%firestring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%healerstring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%hitterstring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` bdbrutes `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%brutestring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` bdfp `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%firestring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` bdhealer `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%healerstring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` bdhitter `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        gt `<font color="#008080">`%hitterstring`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` determine_class `<font color="#804040"><b>`=`</b></font>` `<font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` detname`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`strcat`</font><font color="#6a5acd">`({`</font><font color="#008080">`*`</font><font color="#6a5acd">`}`</font>`, "`<font color="#ff00ff">`-`</font>`"`<font color="#6a5acd">`)]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`strstr`</font><font color="#6a5acd">`({`</font>`brutenames`<font color="#6a5acd">`}`</font>`, `<font color="#6a5acd">`{`</font>`detname`<font color="#6a5acd">`})`</font>` `<font color="#804040"><b>`!=`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/return`</b></font>` "`<font color="#ff00ff">`brute`</font>`"`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`strstr`</font><font color="#6a5acd">`({`</font>`fpnames`<font color="#6a5acd">`}`</font>`, `<font color="#6a5acd">`{`</font>`detname`<font color="#6a5acd">`})`</font>` `<font color="#804040"><b>`!=`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/return`</b></font>` "`<font color="#ff00ff">`fp`</font>`"`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`strstr`</font><font color="#6a5acd">`({`</font>`healernames`<font color="#6a5acd">`}`</font>`, `<font color="#6a5acd">`{`</font>`detname`<font color="#6a5acd">`})`</font>` `<font color="#804040"><b>`!=`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/return`</b></font>` "`<font color="#ff00ff">`healer`</font>`"`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`if`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`strstr`</font><font color="#6a5acd">`({`</font>`hitternames`<font color="#6a5acd">`}`</font>`, `<font color="#6a5acd">`{`</font>`detname`<font color="#6a5acd">`})`</font>` `<font color="#804040"><b>`!=`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/return`</b></font>` "`<font color="#ff00ff">`hitter`</font>`"`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`endif`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/return`</b></font>` "`<font color="#ff00ff">`Unknown`</font>`"`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` strchomp `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` chomped`<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`{`</font><font color="#008080">`*`</font><font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`while`</b></font>` `<font color="#6a5acd">`(`</font>` `<font color="#008080">`substr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%chomped`</font>`,`<font color="#ff00ff">`0`</font>`,`<font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">` `</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`eval`</b></font>` `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` chomped`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`substr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%chomped`</font>`,`<font color="#ff00ff">`1`</font><font color="#6a5acd">`)]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`done`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`while`</b></font>` `<font color="#6a5acd">`(`</font><font color="#008080">`substr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%chomped`</font>`,`<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`1`</font>`,`<font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font>` `<font color="#804040"><b>`=~`</b></font>` "`<font color="#ff00ff">` `</font>`"`<font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`eval`</b></font>` `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` chomped`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`substr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%chomped`</font>`,`<font color="#ff00ff">`0`</font>`,`<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`)]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`done`</b></font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/result`</b></font>` `<font color="#008080">`%`</font><font color="#6a5acd">`{`</font>`chomped`<font color="#6a5acd">`}`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
  
  
<font color="#0000ff">`;for internal use by list macros`</font>  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` listspell`<font color="#804040"><b>`=`</b></font>`bogus;`  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` listspell `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` namelist`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[{`</font><font color="#008080">`*`</font><font color="#6a5acd">`}]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` limit`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`strstr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%namelist`</font>`, "`<font color="#ff00ff">`-`</font>`"`<font color="#6a5acd">`)]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`while`</b></font>` `<font color="#6a5acd">`(`</font>` `<font color="#6a5acd">`{`</font>`limit`<font color="#6a5acd">`}`</font>` `<font color="#804040"><b>`!=`</b></font>` `<font color="#804040"><b>`-`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`)`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` thisname`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`substr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%namelist`</font>`, `<font color="#ff00ff">`0`</font>`, `<font color="#008080">`%limit`</font><font color="#6a5acd">`)]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` namelist`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`substr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%namelist`</font>`, `<font color="#008080">`%limit`</font><font color="#804040"><b>`+`</b></font><font color="#ff00ff">`1`</font><font color="#6a5acd">`)]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`let`</b></font>` limit`<font color="#804040"><b>`=$`</b></font><font color="#6a5acd">`[`</font><font color="#008080">`strstr`</font><font color="#6a5acd">`(`</font><font color="#008080">`%namelist`</font>`, "`<font color="#ff00ff">`-`</font>`"`<font color="#6a5acd">`)]`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`                cast `<font color="#008080">`%listspell`</font>` `<font color="#008080">`%thisname`</font><font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#804040"><b>`done`</b></font>  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` list_steel `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` listspell`<font color="#804040"><b>`=`</b></font>`steel`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/listspell`</b></font>` `<font color="#008080">`%needsteel`</font>  
  
  
<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`def`</font>` list_invinc `<font color="#804040"><b>`=`</b></font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/`</b></font><font color="#a020f0">`set`</font>` listspell`<font color="#804040"><b>`=`</b></font>`invincibility`<font color="#6a5acd">`%;`</font><font color="#6a5acd">`\`</font>  
`        `<font color="#2e8b57"><b>`/listspell`</b></font>` `<font color="#008080">`%needinvinc`</font>

[Category:TinyFugue Scripting](Category:TinyFugue_Scripting "wikilink")
