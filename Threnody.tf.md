Bug reports, suggestions and/or diffs are appreciated, **sulfar** \_AT\_
*inbox* +DOT+ *com*

    /echo %% /thren             Toggle auto threnody
    /echo %% /thren <on|off>    Set auto threnody on or off

    /set auto_thren 0
    /echo %%% Auto threnody disabled.

    ;I'm fatigueing this trigger because somehow i got
    ; two lord infos for the same threnody
    /def -i -mregexp -E(auto_thren) -t"\[LORD INFO\]: ([^ ]+) initiates a Threnody dirge for corpse of ([^ ]+).+" thren_init = \
        /if (!auto_thren_fatigue) \
            /echo %%% Automagically joining %{P1} in performing a threnody ritual for %{P2}%;\
            stand%;\
            c thren %{P2}%;\
            sleep%;\
        /endif%;\
        /set auto_thren_fatigue 1%;\
        /repeat -1 1 /set auto_thren_fatigue 0

    /def -i thren = \
        /if ({1} =~ "") /set auto_thren $[!auto_thren]%;\
        /elseif ({1} =~ "on") /set auto_thren 1 %;\
        /elseif ({1} =~ "off")  /set auto_thren 0%;\
        /else /echo Valid arguments are: on, off and <none>%;\
        /endif%;\
        /if (auto_thren) /echo %%% Auto threnody enabled.%;\
        /else /echo %%% Auto threnody disabled.%;\
        /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
