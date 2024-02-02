Bug reports, suggestions and/or diffs are appreciated, **sulfar** \_AT\_
*inbox* +DOT+ *com*

    /echo %% /rescue            Toggle autorescue
    /echo %% /rescue <on|off>   Set autorescue on or off
    /echo %% /addrescue <name>  Add name to rescue list
    /echo %% /lsrescue          List rescue names
    /echo %% /rmrescue <name>   Remove name from rescue list

    ;/addrescue is case insensitive
    ;/rmrescue is case sensitive

    /set warn_curly_re=off
    /set rescue_fatigue 0
    /set auto_rescue 1
    /echo %% Autorescue is enabled.

    /def -i rescue = \
            /if ({1} =~ "") /set auto_rescue $[!auto_rescue] %;\
            /elseif ({1} =~ "on") /set auto_rescue 1 %;\
            /elseif ({1} =~ "off")  /set auto_rescue 0%;\
            /else /echo Valid arguments are: on, off and <none>%;\
            /endif%;\
            /if (auto_rescue) /echo %%% Autorescue is enabled.%;\
            /else /echo %%% Autorescue is disabled%;\
            /endif

    /def -i addrescue = \
        /def -i -E(auto_rescue) -mregexp -t"(?i) strikes %{1} " rescue_%{1}1 = /rescue_them %{1}%;\
        /def -i -E(auto_rescue) -mregexp -t"(?i) attacks strike %{1} ([^ ]+) times, with " rescue_%{1}2 = /rescue_them %{1}%;\
        /def -i -E(auto_rescue) -mregexp -t"(?i) attacks haven't hurt %{1}!\$" rescue_%{1}3 = /rescue_them %{1}%;\
        /def -i -E(auto_rescue) -mregexp -t"(?i) tries to stand and defend against %{1} but can't!\$" rescue_%{1}4 = /rescue_them %{1}%;\
        /def -i -E(auto_rescue) -mregexp -t"(?i) shot hits %{1} with " rescue_%{1}5 = /rescue_them %{1}%;\
        /rescue_add_name %{1}

    /def -i rmrescue = \
        /for c 1 5 /undef rescue_%{1}%%c%;\
        /set rescue_name_list=$(/remove %{1} %rescue_name_list) %;\
        /set rescue_name_index=$(/length %rescue_name_list)

    /def -i lsrescue = /echo %% rescue_name_list = %rescue_name_list

    ;I'm fatigueing the rescue trigs to not rescue 5 times
    ;with config +battleother
    /def -i rescue_them = \
        /if (!rescue_fatigue) rescue %1%;/endif%;\
        /set rescue_fatigue 1%;\
        /repeat -0:00:0.3 1 /set rescue_fatigue 0

    /def -i rescue_add_name = \
        /if (rescue_name_index = 0) \
            /set rescue_name_list=%{1} %;\
            /set rescue_name_index=1 %;\
        /else \
            /set rescue_name_list=$(/remove %{1} %rescue_name_list) %;\
            /set rescue_name_list=%rescue_name_list %{1} %;\
            /set rescue_name_index=$(/length %rescue_name_list) %;\
        /endif

    /set warn_curly_re=on

    ;High priority non-fallthru triggers to prevent rescue triggers from kicking in
    ;this list is incomplete
    /def -i -p100 -mregexp -t"chain lightning strikes ([^ ]+) with " dont_rescue1
    /def -i -p100 -mregexp -t"icestrike strikes ([^ ]+) with " dont_rescue2
    /def -i -p100 -mregexp -t"blast of gas strikes ([^ ]+) with " dont_rescue3
    /def -i -p100 -mregexp -t"disintegrate strikes ([^ ]+) with " dont_rescue4
    /def -i -p100 -mregexp -t"cyclone strikes ([^ ]+) with " dont_rescue5
    /def -i -p100 -mregexp -t"blast of acid strikes ([^ ]+) with " dont_rescue6
    /def -i -p100 -mregexp -t"vampire touch strikes ([^ ]+) with " dont_rescue7
    /def -i -p100 -mregexp -t"earthquake strikes ([^ ]+) with " dont_rescue8
    /def -i -p100 -mregexp -t"harm spell strikes ([^ ]+) with " dont_rescue9
    /def -i -p100 -mregexp -t"flamestrike strikes ([^ ]+) with " dont_rescue10
    /def -i -p100 -mregexp -t"ultrablast strikes ([^ ]+) with " dont_rescue11
    /def -i -p100 -mregexp -t"brainstorm strikes ([^ ]+) with " dont_rescue12
    /def -i -p100 -mregexp -t"psionic blast strikes ([^ ]+) with " dont_rescue13
    /def -i -p100 -mregexp -t"mindpower strikes ([^ ]+) with " dont_rescue14
    /def -i -p100 -mregexp -t"blast of flame strikes ([^ ]+) with " dont_rescue15
    /def -i -p100 -mregexp -t"high explosive ammo strikes ([^ ]+) with " dont_rescue16
    /def -i -p100 -mregexp -t"firestorm strikes ([^ ]+) with " dont_rescue17
    /def -i -p100 -mregexp -t"acid rain strikes ([^ ]+) with " dont_rescue18
    /def -i -p100 -mregexp -t"blast of frost strikes ([^ ]+) with " dont_rescue19
    /def -i -p100 -mregexp -t"smash strikes ([^ ]+) with " dont_rescue20
    /def -i -p100 -mregexp -t"Meteor Swarm strikes ([^ ]+) with " dont_rescue21
    /def -i -p100 -mregexp -t"cataclysm strikes ([^ ]+) with " dont_rescue22
    /def -i -p100 -mregexp -t"shard storm strikes ([^ ]+) with " dont_rescue23
    /def -i -p100 -mregexp -t"torment strikes ([^ ]+) with " dont_rescue24
    /def -i -p100 -mregexp -t"blast of lightning strikes ([^ ]+) with " dont_rescue25

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
