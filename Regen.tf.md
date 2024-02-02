A regen script. It sends a blank line to the server and checks if we're
done at regular intervals, currently 8 seconds. It uses variables from
[prompt.tf](prompt.tf "wikilink") to see if we're done.

This script uses both [prompt.tf](prompt.tf "wikilink") and
[verbose.tf](verbose.tf "wikilink") so you'll need those if you want to
use this script. You should also have TINYPREFIX set in your config
file, pointing at the directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

You can see examples of its use in the
[identify.tf](identify.tf "wikilink") or the
[enchant.tf](enchant.tf "wikilink") script. If it doesnt have mana for
the spell, it calls this script to regen and go back when it's done with
the regen.

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''
    /loaded __SULFAR__/regen.tf

    /eval /require -q %{TINYPREFIX}prompt.tf
    /eval /require -q %{TINYPREFIX}verbose.tf

    /echo -aCyellow %% /regen -x<command>    Fully regen, then execute <command>

    ;A regen script.  It will make you sleep to regen.  It will make you stand and
    ;execute a command when you're done, for example:
    ;  sanc dw
    ;  regen -x"e=u=u=sleep"
    ;
    ;Or it might just be more useful in other scripts.

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and use system verbosity level
    ;see verbose.tf
    /verbose -s2 regen

    /def -i regen = \
            /if (!getopts("x:", "")) /return 0%; /endif%; \
            /set regen_post %{opt_x}%;\
            /verbose -o%{verbosity_regen} -l1 - -aCcyan %%% /%0: Starting checks.  Post command is: %regen_post%;\
            sleep%;\
            /repeat -7.5 1 /send%;\
            /repeat -8 1 /regen_check

    /def -i regen_check = \
            /let _post %{regen_post}%;\
            /let _move_relative $[prompt_move*100/prompt_move_max]%;\
            /if ((prompt_hp_relative >= 100) & (prompt_mana_relative >= 100) & \
                            (_move_relative >= 100) & (!prompt_lagtimer)) \
                    /verbose -o%{verbosity_regen} -l1 - -aCcyan %%% /%0: Regen done!  Executing post command: %regen_post%;\
                    stand%;\
                    /unset regen_post%;\
                    /if (_post !~ "") /eval %{_post}%; /endif%;\
            /else \
                    /verbose -o%{verbosity_regen} -l2 - -aCyellow %%% %0: not done yet....%;\
                    /repeat -7.5 1 /send%;\
                    /repeat -8 1 /regen_check%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
