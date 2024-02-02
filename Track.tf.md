An auto track script.

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /echo -aCyellow %% /track             Toggle auto track
    /echo -aCyellow %% /track <on|off>    Set auto track on or off

    /set auto_track 1
    /echo %% Autotracking enabled.

    /def -i -E(auto_track) -mregexp -t"^You see your quarry's trail head (east|west|north|south|down|up) from here!" do_track = %{P1} %;/echo %%% Autotracking:  %{P1}

    /def -i track = \
            /if ({1} =~ "") /set auto_track $[!auto_track]%;\
            /elseif ({1} =~ "on") /set auto_track 1 %;\
            /elseif ({1} =~ "off")  /set auto_track 0%;\
            /else /echo Valid arguments are: on, off and <none>%;\
            /endif%;\
            /if (auto_track) /echo %%% Autotracking enabled.%;\
            /else /echo %%% Autotracking disabled%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
