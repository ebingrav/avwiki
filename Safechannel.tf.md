Sometimes people will cut and paste lines of mud output to channels.
That could mess with triggers in scripts.

You should have TINYPREFIX set in your config file, pointing at the
directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

This script uses [verbose.tf](verbose.tf "wikilink") so you'll need that
if you want to use it.

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /loaded __SULFAR__/safechannel.tf

    /eval /require -q %{TINYPREFIX}verbose.tf

    /echo -aCyellow %% /safechannel          Toggle safechannel
    /echo -aCyellow %% /safechannel [on|off]     turn safechannel on or off
    /echo -aCyellow %%     [-q]                  quiet mode

    ;Regexps that you anchor with ^ can not be fooled by channels.
    ;Regexps that you can't anchor with ^ but can end with $ are safe for
    ;  channels if you append something to the channels before processing
    ;  the rest of the triggers.  So these are fall-thru triggers with
    ;  priority 999 doing just that.

    ;In black, to make it invisible
    ;/set safechannel_postfix=@{Cblack}sc_pf
    ;Or in eh... "bright black" to make it somewhat (in?)visible
    /set safechannel_postfix=@{BCblack}sc_pf

    ;Turn it off by default
    /set safechannel=0

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and keep system verbosity level
    ;see verbose.tf
    /verbose -s-100 safechannel


    /def -i -E(safechannel) -p999 -F -mregexp -t"^([A-Za-z]+)\> " safechannel_hero = \
            /verbose -o%{verbosity_safechannel} -l2 - -aCyellow %%% /%0%;\
            /substitute -p %* %safechannel_postfix

    /def -i -E(safechannel) -p999 -F -mregexp -t"^\(([A-Za-z]+)\) " safechannel_lord = \
            /verbose -o%{verbosity_safechannel} -l2 - -aCyellow %%% /%0%;\
            /substitute -p %* %safechannel_postfix

    /def -i -E(safechannel) -p999 -F -mregexp -t"^([A-Za-z]+) congratulates " safechannel_grtz = \
            /verbose -o%{verbosity_safechannel} -l2 - -aCyellow %%% /%0%;\
            /substitute -p %* %safechannel_postfix

    /def -i -E(safechannel) -p999 -F -mregexp -t"^\{([A-Za-z]+)\} " safechannel_buddy = \
            /verbose -o%{verbosity_safechannel} -l2 - -aCyellow %%% /%0%;\
            /substitute -p %* %safechannel_postfix

    /def -i safechannel = \
            /let sf_quiet=0%;\
            /if (!getopts("q")) /return 0%; /endif%;\
            /if (opt_q) /let sf_quiet %{opt_q}%; /endif%;\
            /if ({1} =~ "") /toggle safechannel%;\
            /elseif ({1} =~ "on") /set safechannel 1%;\
            /elseif ({1} =~ "off")  /set safechannel 0%;\
            /else /echo -aCyellow %%% /%0: Valid arguments are: on, off and <none>%;\
            /endif%;\
            /if (!sf_quiet) \
                    /if (safechannel) /verbose -o%{verbosity_safechannel} -l1 - -aCcyan %%% Safechannel postfixing enabled.%;\
                    /else /verbose -o%{verbosity_safechannel} -l1 - -aCcyan %%% Safechannel postfixing disabled.%;\
                    /endif%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
