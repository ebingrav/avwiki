While writing a new script, I often wrote a quick "/echo variable is
%{variable}" in a macro to see if the values of variables were computed
correctly. When I had finished writing the script I'd remove them to
reduce the spam on my screen. Sometimes though when I decided to take a
different approach to some macros I'd find myself adding pretty much the
same /echo's again, rewrite the macro's and then remove them again.

Duh. Wouldn't it be nice if I could just leave them in there and turn
them on and off when i want to?

Also, suppose you'd write a somewhat more complicated script that can
run for a while and change a lot of variables. Sometimes you'd like to
see a lot of debug info, and sometimes you just want to let it do its
job without spamming you crazy while you're chatting.

This script makes that pretty easy. To use it in a script I have
TINYPREFIX set in my config file, pointing at the directory with my tf
scripts like

    /set TINYPREFIX=~/tinyfugue/

and load it with this line in the script:

    /eval /require -q %{TINYPREFIX}verbose.tf

See for example [identify.tf](identify.tf "wikilink") or
[label.tf](label.tf "wikilink").

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /loaded __SULFAR__/verbose.tf

    ;Flexible script verbosity..

    /def -i verbose_usage = \
            /echo -aCyellow %%% /verbose              Shows usage and current levels%;\
            /echo -aCyellow %%% /verbose <line>       /echo <line> if verbosity level is 1 or higher%;\
            /echo -aCyellow %%%     [-l<level>]           /echo if verbosity level is <level> or higher%;\
            /echo -aCyellow %%%     [-o<level>]           override verbosity level with <level>%;\
            /echo -aCyellow %%%     [-s<level>] [<script>]set verbosity level (of <script>) to <level>

    /verbose_usage

    ; Instead of just /echoing lines to the screen, scripts can use /verbose
    ;   so you can easily change the spamminess of scripts.  You can set a
    ;   sytem verbosity level in this script, or override it with a script
    ;   verbosity level in another script.

    ; The verbosity levels I use are:
    ;
    ;  0   Complete silence
    ;  1   Script announces itself and may print additional information
    ;        if it could actually be useful while playing
    ;  2   All macros announce themselves, sometimes with variables and values
    ;  3   Full debug mode, all available spam including variables and values
    ;

    ; A higher level verbosity does include a lower level.

    ; Default system wide verbosity level
    /set verbosity 1

    ; Everything after a single - is fed to /echo as is, so to use /verbose to
    ;   print a line in red if the verbosity level is 1 or higher, you could
    ;             /verbose - -aCred a line
    ;   and the -aCred will be seen as option for /echo.

    ; The -l option indicates at which verbosity level the line should be printed
    ; and defaults to 1.

    ; Individual scripts can pass their own verbosity level with -o.  If the
    ; level given by -o is 0 or positive it will override %verbosity .
    ; In other words, use /verbose -o-100  to turn the individual script level
    ; off.  Default:  -100, don't override.  Setting it to -1 would do the
    ; trick too but -100 is easier to distinguish on "/listvar verbosity*".

    ; Suppose you'd have a walk script, consisting of a trigger and some other
    ; macros.  You could set a verbosity_walk variable.  Depending on its value and
    ; on the value of verbosity in this script you could create different
    ; possibilities:
    ;
    ;   normal verbosity while playing
    ;         /set verbosity 1
    ;         /set verbosity_walk -100
    ;   the same would by accomplished by:
    ;         /verbose -s1
    ;         /verbose -s-100 walk

    ;   debug the new walk script without all the other ones spamming you crazy,
    ;         /set verbosity 1
    ;         /set verbosity_walk 3
    ;
    ;   debug all scripts but that spammy walk one that isn't the problem anyways,
    ;         /set verbosity 3
    ;         /set verbosity_walk 1
    ;
    ;   watch the flow through each individual macro of all scripts while
    ;   fully debugging the walk script
    ;         /set verbosity 2
    ;         /set verbosity_walk 3
    ;
    ; The walk script would then need to call /verbose e.g. in the body of the trigger:
    ;     /verbose -o%{verbosity_walk} -l1 - -aCyellow Walk script running!  Use /walk to walk!
    ;     /verbose -o%{verbosity_walk} -l1 - -aBCred NOTE: Water breathing or death!
    ;
    ; If you put this at the top of any macro in the script it will show which
    ; options and arguments the call used:
    ;     /verbose -o%{verbosity_walk} -l2 - /%0:  Called as /%0 %*
    ;
    ; After expressions, to see what values are computed:
    ;     /verbose -o%{verbosity_walk} -l3 - -aCyellow /%0: variable is %{variable}.

    ; The -s option sets the verbosity level.  "/verbose -s2" to set the system
    ; verbosity level to 2, "/vebose -s3 walk" to set verbosity_walk to 3.

    /def -i verbose = \
            /if ({*} =~ "") \
                    /echo -p %%% @{Cred}--------------------------------------------------------------------------%;\
                    /verbose_usage%;\
                    /echo -p %%% @{Cred}--------------------------------------------------------------------------%;\
                    /echo -p %%% @{Cyellow}Current verbosity levels:%;\
                    /echo -p %%% @{Cred}--------------------------------------------------------------------------%;\
                    /listvar verbosity*%;\
                    /echo -p %%% @{Cred}--------------------------------------------------------------------------%;\
                    /return 0%;\
            /endif%;\
            /let line_level=1%;\
            /let override_level=-100%;\
            /if (!getopts("l:o:s:", "")) /return 0%; /endif%;\
            /if (opt_s !~ "") \
                    /let _arg %1%;\
                    /if (_arg =~ "") \
                            /set verbosity %{opt_s}%;\
                            /echo -aCyellow %%% Verbosity level set to %verbosity%;\
                    /else \
                            /eval /set verbosity_%{_arg} %{opt_s}%;\
                            /eval /echo -aCyellow %%%% Verbosity level of %{_arg} set to %%{verbosity_%{_arg}}%;\
                    /endif%;\
                    /return 0%;\
            /endif%;\
            /test (opt_l !~ "") & (line_level:=opt_l)%;\
            /test (opt_o !~ "") & (override_level:=opt_o)%;\
            /if (override_level >= 0) \
                    /if (override_level >= line_level) \
                            /echo %*%;\
                    /endif%;\
            /elseif (verbosity >= line_level) \
                    /echo %*%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
