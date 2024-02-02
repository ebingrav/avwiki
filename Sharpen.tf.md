This script will sharpen an item until it dulls, or is as sharp as it is
going to get.

Aside from this script I already had a simple trigger to make me hold a
new whetstone when the old one is worn out. The script will hold a
whetstone if it tries to sharpen without it, so you won't really need
this trigger, but it'd make it run a little more graceful.

    /def -i -t"The whetstone crumbles out of your hand." byebye_whetstone = hold whetstone

This script uses [verbose.tf](verbose.tf "wikilink") so you'll want that
if you want to use this script. You should also have TINYPREFIX set in
your config file, pointing at the directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /loaded __SULFAR__/sharpen.tf

    /eval /require -q %{TINYPREFIX}verbose.tf

    /echo -aCyellow %% /sharpen <item>       Sharpen <item> until it's finished
    /echo -aCyellow %%     [-x<command>]         execute <command> after full sharpen

    ;This script will sharpen an item untill it dulls, or is as sharp as it is
    ;  going to get.  It will schedule the next sharpen at 3 minutes and 15 seconds
    ;  to give you a chance to chat some lines or some, and prevend triggers like
    ;  heighten senses to stack up the lag too much.

    ;When the script exits it sets global var sharpen_exit to
    ;no_weapon      couldn't find your weapon
    ;no_whetstone   couldn't find a whetstone to use
    ;was_sharp      the weapon was already as sharp as it could be
    ;sharp          the weapon is now as sharp as it can be
    ;dull           we slipped and dulled the weapon
    ;brill          our hands tingled and we got a brilliant green and so on
    ;brilldull      this weapon brilled but dulled afterwards
    ;
    ;re no_whetstone:  If the weapon was (brill) sharpened, the whetstone crumbles
    ;   and you don't have a new one, it'll lose the information about the result.
    ;I'm simply keeping a couple of spare whetstone on my sharpen alt.


    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and keep system verbosity level
    ;see verbose.tf
    /verbose -s-100 sharpen

    ;some hilites ... makes it easier to catch these in scrollback
    /def -i -p800 -F -PCyellow -mregexp -t"^You smile as you feel the increased sharpness of .+!$" sharpen_hi_ok
    /def -i -p800 -F -PBCyellow -mregexp -t"^Your hands tingle as a brilliant green light bursts from .+!$" sharpen_hi_brill
    /def -i -p800 -F -PBCred -mregexp -t"^You sigh as you realize you have slipped and dulled .+!$" sharpen_hi_dulled
    /def -i -p800 -F -PCwhite -mregexp -t"^Despite your efforts, nothing happens to .+" sharpen_hi_nothing
    /def -i -p800 -F -PCwhite -mregexp -t"^The weapon is now as sharp as it is going to get!" sharpen_hi_finished
    /def -i -p800 -F -PCwhite -mregexp -t"^The weapon is as sharp as it is going to get!" sharpen_hi_was_sharp

    /def -i sharpen_init = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0 initializing .... %;\
            /set auto_sharpen=0%;\
            /set sharpen_item=%;\
            /set _sharpen_next_pid=0%;\
            /set sharpen_brill=0%;\
            /set sharpen_exit=%;\
            /set sharpen_post=

    /def -i sharpen = \
            /sharpen_init%;\
            /if (!getopts("x:", "")) /return 0%; /endif%; \
            /set sharpen_post %{opt_x}%;\
            /set auto_sharpen 1%;\
            /set sharpen_item %*%;\
            /verbose -o%{verbosity_sharpen} -l1 - -aCcyan %%% /%0:  Start sharpenening %sharpen_item.  Exit command is %sharpen_post%;\
            stand%;hei%;hei%;\
            sharpen %{sharpen_item}

    /def -i sharpen_next = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: Sharpening again%;\
            /repeat -0:03:15 1 sharpen %{sharpen_item}%;\
            /set _sharpen_next_pid=%?

    /def -i -E(auto_sharpen) -p699 -F -t"You must be carrying a weapon to sharpen it!" sharpen_no_weapon = \
            /verbose -o%{verbosity_sharpen} -l1 - -aBCred %%% Can't find your weapon, %{sharpen_item}!  Bailing out.%;\
            /sharpen_exit no_weapon

    /def -i -E(auto_sharpen) -p699 -F -t"Sharpen it with what!" sharpen_no_whetstone = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: Not holding a whetstone.  Trying to hold one.%;\
            /set auto_sharpen_whetstone 1%;\
            hold whetstone

    /def -i -E(auto_sharpen_whetstone) -p699 -F -t"You hold a whetstone in your hands." sharpen_now_whetstone = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: We have a whetstone!  Alright, sharpening.%;\
            /set auto_sharpen_whetstone 0%;\
            sharpen %{sharpen_item}

    /def -i -E(auto_sharpen_whetstone) -p699 -F -t"You are not carrying a whetstone." sharpen_really_no_whetstone = \
            /verbose -o%{verbosity_sharpen} -l1 - -aBCred %%% /%0: No whetstone to use!  Bailing out.%;\
            /set auto_sharpen_whetstone 0%;\
            /sharpen_exit no_whetstone

    ;we should probably sharpen more
    ;if not, sharpen_finished will kill the next sharpen
    /def -i -E(auto_sharpen) -p699 -F -t"You smile as you feel the increased sharpness of *!" sharpen_ok = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: Sharpening again...%;\
            /sharpen_next

    /def -i -E(auto_sharpen) -p699 -F -t"Your hands tingle as a brilliant green light bursts from *!" sharpen_brill = \
            /verbose -o%{verbosity_sharpen} -l2 - -aBCyellow %%% /%0: Yeeeehaaaa!!  Teh brillz!  Sharpening again...%;\
            /test ++sharpen_brill%;\
            /sharpen_next

    /def -i -E(auto_sharpen) -p699 -F -t"Despite your efforts, nothing happens to *" sharpen_nothing = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: Sharpening again...%;\
            /sharpen_next

    /def -i -E(auto_sharpen) -p699 -F -t"You sigh as you realize you have slipped and dulled *!" sharpen_dulled = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: Exit sharpen.%;\
            /repeat -0:03:15 1 /sharpen_exit dull

    /def -i -E(auto_sharpen) -p699 -F -t"The weapon is now as sharp as it is going to get!" sharpen_finished = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: But we're done!  Killing planned sharpen and exit.%;\
            /@test _sharpen_next_pid & (kill(_sharpen_next_pid), _sharpen_next_pid:=0)%;\
            /repeat -0:03:15 1 /sharpen_exit sharp

    /def -i -E(auto_sharpen) -p699 -F -t"The weapon is as sharp as it is going to get!" sharpen_was_sharp = \
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: This weapon doesn't need sharpening.  Exit.%;\
            /sharpen_exit was_sharp

    /def -i sharpen_exit = \
            /let post %sharpen_post%;\
            /verbose -o%{verbosity_sharpen} -l1 - -aCcyan %%% /%0:  Done with this weapon. Executing command: %post%;\
            /if (sharpen_brill & {1} =~ "dull") \
                    /sharpen_init%;\
                    /set sharpen_exit brilldull%;\
            /elseif (sharpen_brill) \
                    /sharpen_init%;\
                    /set sharpen_exit brill%;\
            /else \
                    /sharpen_init%;\
                    /set sharpen_exit %1%;\
            /endif%;\
            /verbose -o%{verbosity_sharpen} -l2 - -aCyellow %%% /%0: Exit code: %{sharpen_exit}%;\
            /if (post !~ "") /eval -s0 %{post}%; /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
