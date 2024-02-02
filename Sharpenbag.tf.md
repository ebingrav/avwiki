So you have a bunch of weapons to sharpen. Toss them in a weaponbag, run
the script, collect the results from sharpbag, brillbag, dullbag, or
brilldullbag. See the comments in the script for a list of
etches/christens. It accepts a -x<command> option to have a <command>
executed when it's done.

The script is not 100% fool proof. It gets 1. from weaponbag, then
sharpens 1. So if you're carrying or wielding a weapon similar to the
one you got from the weaponbag, 1. might not be the weapon you want to
sharpen. Keep the inv clean from weapons, don't wield anything, and
it'll work perfectly fine always. Or should anyways.

This script uses [verbose.tf](verbose.tf "wikilink") and
[sharpen.tf](sharpen.tf "wikilink") so you'll want those if you want to
use this script. Also, you should have TINYPREFIX set in your config
file, pointing at the directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /eval /require -q %{TINYPREFIX}sharpen.tf
    /eval /require -q %{TINYPREFIX}verbose.tf

    /echo -aCyellow %% /sharpenbag           Sharpen all items in sharpbag and distribute results
    /echo -aCyellow %%     [-x<command>]         command to execute after sharpenbag

    ;etch           christen
    ;weaponbag      a bag of sharpenable weapons
    ;sharpbag       a bag of sharpened weapons
    ;brillbag       a bag of brill sharpened weapons
    ;dullbag        a bag of dulled weapons
    ;brilldullbag   a bag of weapons that brilled, then dulled
    ;
    ;Only the christen for the weaponbag matters, sharpenbag_got_weapon triggers
    ;  on it

    ;When the script exits it sets global var sharpenbag_exit to
    ;0              sharpenbag finished correctly with an empty weaponbag
    ;no_weapon      error code from sharpen.tf
    ;no_whetstone   error code from sharpen.tf
    ;no_weaponbag   can't find the weaponbag

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and keep system verbosity level
    ;see verbose.tf
    /verbose -s-100 sharpenbag

    /def -i sharpenbag_init = \
            /set sharpenbag=0%;\
            /set sharpenbag_post=%;\
            /set sharpenbag_exit=

    /sharpenbag_init

    /def -i sharpenbag = \
            /verbose -o%{verbosity_sharpenbag} -l2 - -aCyellow %%% /%0 initializing .... %;\
            /sharpenbag_init%;\
            /set sharpenbag=1%;\
            /if (!getopts("x:", "")) /return 0%; /endif%; \
            /set sharpenbag_post %{opt_x}%;\
            /sharpenbag_start

    /def -i sharpenbag_start = \
            /verbose -o%{verbosity_sharpenbag} -l2 - -aCyellow %%% /%0: Start sharpening%;\
            stand%;hei%;hei%;\
            get 1. weaponbag

    ;Wrong alt?  Weaponbag in a bag?
    /def -i -E(sharpenbag) -p799 -F -t"I see no weaponbag here." sharpenbag_no_weaponbag = \
            /verbose -o%{verbosity_sharpenbag} -l1 - -aBCred %%% You are not carrying the weaponbag.  Exiting /sharpenbag.%;\
            /enchantbag_exit no_weaponbag

    /def -i -E(sharpenbag) -p799 -F -mregexp -t"^You get (.+) from a bag of sharpenable weapons.$" sharpenbag_got_weapon = \
            /verbose -o%{verbosity_sharpenbag} -l2 - -aCyellow %%% /%0: Got weapon, %{P1}.%;\
            /sharpen -x/sharpenbag_weapon_done 1.

    /def -i -E(sharpenbag) -p799 -F -t"You see nothing like that in a bag of sharpenable weapons."  weaponbag_empty = \
            /verbose -o%{verbosity_sharpenbag} -l1 - -aCcyan %%% Your weaponbag is empty.  Exiting /sharpenbag.%;\
            /sharpenbag_exit 0

    /def -i sharpenbag_weapon_done = \
            /verbose -o%{verbosity_sharpenbag} -l2 - -aCyellow %%% /%0: This weapon is done.%;\
            /if (regmatch("^$|no_weapon|no_whetstone", sharpen_exit)) \
                    /verbose -o%{verbosity_sharpenbag} -l1 - -aBCred %%% /%0: Sharpen script exit status: %{sharpen_exit}%;\
                    /verbose -o%{verbosity_sharpenbag} -l1 - -aBCred %%% /%0: Exit sharpenbag, sharpen_err.%;\
                    /sharpenbag_exit %{P1}%;\
            /elseif (regmatch("sharp|was_sharp", sharpen_exit)) \
                    put 1. sharpbag%;\
                    /sharpenbag_start%;\
            /elseif (sharpen_exit =~ "dull") \
                    put 1. dullbag%;\
                    /sharpenbag_start%;\
            /elseif (sharpen_exit =~ "brill") \
                    put 1. brillbag%;\
                    /sharpenbag_start%;\
            /elseif (sharpen_exit =~ "brilldull") \
                    put 1. brilldullbag%;\
                    /sharpenbag_start%;\
            /endif

    /def -i sharpenbag_exit = \
            /let _post %sharpenbag_post%;\
            /verbose -o%{verbosity_sharpenbag} -l1 - -aCyellow %%% /%0: Exiting and executing command: %_post%;\
            /sharpenbag_init%;\
            /set sharpenbag_exit %1%;\
            /verbose -o%{verbosity_sharpenbag} -l3 - -aCyellow %%% /%0: Exit code: %sharpenbag_exit%;\
            /if (_post !~ "") /eval -s0 %{_post}%; /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
