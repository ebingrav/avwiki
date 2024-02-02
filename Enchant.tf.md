An enchant script. It'll enchant an item until something happens to it,
and execute the command given by option -x when it's done. The -i option
makes it use information gathered by the
[identify.tf](identify.tf "wikilink") script, and is its default way of
getting the item's type. When the enchant is finished it will set global
var enchant_exit to some useful exit status.

This script uses [verbose.tf](verbose.tf "wikilink"),
[regen.tf](regen.tf "wikilink") and
[safechannel.tf](safechannel.tf "wikilink") so you'll want those if you
want to use this script. Regen.tf uses
[prompt.tf](prompt.tf "wikilink"). You should also have TINYPREFIX set
in your config file, pointing at the directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /loaded __SULFAR__/enchant.tf


    /eval /require -q %{TINYPREFIX}verbose.tf
    /eval /require -q %{TINYPREFIX}regen.tf
    /eval /require -q %{TINYPREFIX}safechannel.tf

    ;All tf commands have the options before the arguments, e.g.
    ;/enchant -a ring
    /def -i enchant_usage = \
            /echo -aCyellow %%% /enchant <item>       Enchant <item> until something happens to it%;\
            /echo -aCyellow %%%     [-a]                  item is an armor%;\
            /echo -aCyellow %%%     [-w]                  item is a weapon%;\
            /echo -aCyellow %%%     [-b]                  item is a bow%;\
            /echo -aCyellow %%%     [-i]                  item is a %%{identify_type} <- default%;\
            /echo -aCyellow %%%     [-n"<name>"]          use <name> to verify triggers%;\
            /echo -aCyellow %%%     [-x"<command>"]       execute <command> on exit

    /enchant_usage

    ;When the enchant is finished it will set global var enchant_exit to
    ;no_type        couldn't determine item type
    ;no_item        you're not carrying the item
    ;wrong_type     the items type is not what was expected
    ;wrong_name     the triggers responded to something but the name was wrong
    ;fade           we enchanted and got a fade
    ;shimmer        we enchanted and got a shimmer
    ;presence       we enchanted and got a shimmer and a presence
    ;brill          we enchanted and got a brilliant
    ;vape           we enchanted and the item vaporized or exploded
    ;
    ;re wrong_name:  the script does ignore the next line of mud output when someone
    ;   else enchants something.  If it still exits with wrong_name it probably
    ;   means something weird is going on.

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and use system verbosity level
    ;see verbose.tf
    /verbose -s-100 enchant

    /def -i enchant_init = \
            /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Initializing .... %;\
            /set auto_enchant=0%;\
            /set enchant_timeout=0%;\
            /set enchant_keyword=%;\
            /set enchant_name=%;\
            /set enchant_type=%;\
            /set enchant_exit=%;\
            /set enchant_post=

    /def -i enchant = \
            /enchant_init%;\
            /if (!getopts("awbin:x:")) /return 0%;/endif%; \
            /set enchant_name %{opt_n}%;\
            /safechannel -q on%;\
            /set enchant_post %{opt_x}%;\
            /if (opt_a) /set enchant_type armor%;\
            /elseif (opt_w) /set enchant_type weapon%;\
            /elseif (opt_b) /set enchant_type bow%;\
            /elseif (identify_type !~ "") /set enchant_type %{identify_type}%;\
            /else \
                    /echo -aCyellow %%% No idea how to enchant %*.%;\
                    /enchant_exit no_type%;\
                    /return 1%;\
            /endif%;\
            /set enchant_keyword %*%;\
            /set auto_enchant 1%;\
            /verbose -o%{verbosity_enchant} -l1 - -aCcyan %%% /%0: Start enchanting %enchant_keyword.%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Type is %enchant_type, post command is %enchant_post%;\
            /enchant_do%;\

    /def -i enchant_do = \
            /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Enchanting %{enchant_keyword} as %{enchant_type}.%;\
            c 'enchant %{enchant_type}' %{enchant_keyword}

    ;Stuff that could go wrong
    /def -i -E(auto_enchant) -p699 -F -t"You are not carrying *!" enchant_no_item = \
            /verbose -o%{verbosity_enchant} -l2 - -aBCred %%% Can't find your item, %{enchant_keyword}!  Bailing out.%;\
            /enchant_exit no_item

    /def -i -E(auto_enchant) -p699 -F -mregexp -t"^You do not have enough mana to cast enchant (weapon.md|bow|armor).$" enchant_no_mana = \
            /verbose -o%{verbosity_enchant} -l1 - -aCcyan %%% /%0: Yikes!  Out of mana.  Calling regen script and returning.%;\
            /regen -x/enchant_do

    /def -i -E(auto_enchant) -p699 -F -mregexp -t"^That isn't (an|a) (armor|weapon|bow).$" enchant_wrong_type = \
            /verbose -o%{verbosity_enchant} -l1 - -aBCred %%% /%0: WRONG TYPE!  Exiting.%;\
            /enchant_exit wrong_type

    ;If someone else enchants we dont want our triggers to grab their results
    /def -i -E(auto_enchant) -t"* utters the words, 'enchant *'." enchant_timeout = \
            /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Someone enchanted something.  Disabling enchant triggers.%;\
            /set enchant_timeout 1
    ;So we ignore the next line of mud output with a highest priority non-fallthru trigger
    /def -i -E(enchant_timeout) -p999 -t"*" enchant_exit_timeout = \
            /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: We got their result.  Enabling enchant triggers.%;\
            /set enchant_timeout 0
    ;DEBUG? triggers are still not 100% fool proof (but getting really close)
    ;someone could enchant something just a millisecond before we do so both results
    ;would be on consecutive lines of mud output.  If their enchant fails or does
    ;nothing, we'll be ignoring the result of ours, stalling the script.

    ;armor, weapon, bow
    /def -i -E(auto_enchant) -p699 -F -mregexp -t"^Nothing seemed to happen.$" enchant_nothing = \
            /verbose -o%{verbosity_enchant} -l1 - -aCyellow %%% /%0: Sigh... *yawn* ... Mumble mumph ZZZZZZZZZ ... retrying...%;\
            /enchant_do

    /def -i -E(auto_enchant) -p699 -F -mregexp -t" glows brightly, then fades...oops.$" enchant_fade = \
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from -n: %enchant_name%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from trigger: %{PL}%;\
            /if ((enchant_name =~ "") | (enchant_name =/ {PL})) \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Item fades!  Beh.%;\
                    /enchant_exit fade%;\
            /else \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Name mismatch.  Exit.%;\
                    /enchant_exit wrong_name%;\
            /endif

    ;a presence kills the enchant_exit /repeated by the shimmer
    /def -i -E(auto_enchant) -p699 -F -mregexp -t"^You feel the presence of (Shizaga|Quixoltan|Durr)!$" enchant_presence = \
            /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0:  Gotta love %{P1}!%;\
            /@test _enchant_exit_pid & (kill(_enchant_exit_pid), _enchant_exit_pid:=0)%;\
            /enchant_exit presence

    ;armor
    /def -i -E(auto_enchant) -p699 -F -mregexp -t" shimmers with a gold aura.$" enchant_shimmers_gold = \
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from -n: %enchant_name%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from trigger: %{PL}%;\
            /if ((enchant_name =~ "") | (enchant_name =/ {PL})) \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: We got gold... But where is Shizaga?%;\
                    /repeat -0.5 1 /enchant_exit shimmer%;\
                    /set _enchant_exit_pid=%?%;\
            /else \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Name mismatch.  Exit.%;\
                    /enchant_exit wrong_name%;\
            /endif

    /def -i -E(auto_enchant) -p699 -F -mregexp -t" glows a brilliant gold!$" enchant_brilliant_gold = \
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from -n: %enchant_name%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from trigger: %{PL}%;\
            /if ((enchant_name =~ "") | (enchant_name =/ {PL})) \
                    /verbose -o%{verbosity_enchant} -l2 - -aBCyellow %%% /%0: Yesss!!!  A brill!.%;\
                    /enchant_exit brill%;\
            /else \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Name mismatch.  Exit.%;\
                    /enchant_exit wrong_name%;\
            /endif

    /def -i -E(auto_enchant) -p699 -F -mregexp -t" flares blindingly... and evaporates!$" enchant_vape = \
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from -n: %enchant_name%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from trigger: %{PL}%;\
            /if ((enchant_name =~ "") | (enchant_name =/ {PL})) \
                    /verbose -o%{verbosity_enchant} -l2 - -aBCred %%% /%0: Oh nooooooooo.... another one vaped!%;\
                    /enchant_exit vape%;\
            /else \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Name mismatch.  Exit.%;\
                    /enchant_exit wrong_name%;\
            /endif

    ;weapon, bow
    /def -i -E(auto_enchant) -p699 -F -mregexp -t" glows blue.$" enchant_shimmers_blue = \
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from -n: %enchant_name%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from trigger: %{PL}%;\
            /if ((enchant_name =~ "") | (enchant_name =/ {PL})) \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: This one enchanted...  any presence?%;\
                    /repeat -0.5 1 /enchant_exit shimmer%;\
                    /set _enchant_exit_pid=%?%;\
            /else \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Name mismatch.  Exit.%;\
                    /enchant_exit wrong_name%;\
            /endif

    /def -i -E(auto_enchant) -p699 -F -mregexp -t" glows a brilliant blue!$" enchant_brilliant_blue = \
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from -n: %enchant_name%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from trigger: %{PL}%;\
            /if ((enchant_name =~ "") | (enchant_name =/ {PL})) \
                    /verbose -o%{verbosity_enchant} -l2 - -aBCblue %%% /%0: I wish this would happen all the time!%;\
                    /enchant_exit brill%;\
            /else \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Name mismatch.  Exit.%;\
                    /enchant_exit wrong_name%;\
            /endif

    /def -i -E(auto_enchant) -p699 -F -mregexp -t" shivers violently and explodes!$" enchant_explode = \
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from -n: %enchant_name%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Name from trigger: %{PL}%;\
            /if ((enchant_name =~ "") | (enchant_name =/ {PL})) \
                    /verbose -o%{verbosity_enchant} -l2 - -aBCred %%% /%0: Boom boom boom boom... Byebye!!%;\
                    /enchant_exit vape%;\
            /else \
                    /verbose -o%{verbosity_enchant} -l2 - -aCyellow %%% /%0: Name mismatch.  Exit.%;\
                    /enchant_exit wrong_name%;\
            /endif

    ;Finally, we exit!
    /def -i enchant_exit = \
            /let _post %{enchant_post}%;\
            /safechannel -q off%;\
            /unset auto_enchant%;\
            /unset enchant_timeout%;\
            /unset enchant_keyword%;\
            /unset enchant_name%;\
            /unset enchant_type%;\
            /unset enchant_post%;\
            /set enchant_exit %1%;\
            /verbose -o%{verbosity_enchant} -l3 - -aCyellow %%% /%0: Exit code: %{enchant_exit}%;\
            /verbose -o%{verbosity_enchant} -l1 - -aCcyan %%% /%0: Enchant script done!  Executing post command: %{_post}%;\
            /if (_post !~ "") /eval -s0 /repeat -0.4 1 %{_post}%; /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
