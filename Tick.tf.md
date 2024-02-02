A modified tick.tf. It's backwards compatible with tick.tf as found in
tflib. Changes are in the comments below.

It also provides a means to other scripts to have something executed on
the tick. See tick_exe in the script, or
[affects.tf](affects.tf "wikilink") for a usage example of this.

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    ;;;; Tick counting
    ;;;; This file implements several tick counting commands similar to those
    ;;;; found in tintin, useful on Diku muds.

    ;;;; usage:
    /echo -aCyellow %% /tick                 Display the time remaining until next tick
    /echo -aCyellow %% /tickon               Reset and start the tick counter
    /echo -aCyellow %% /tickoff              Stop the tick counter
    /echo -aCyellow %% /tickset              Reset and start the tick counter
    /echo -aCyellow %% /ticksize <n>         Set the tick length to <n> seconds (default is 28.45)
    /echo -aCyellow %% /tickshow             Toggle tick messages
    /echo -aCyellow %% /tick+                Increase the ticksize with 0.05 seconds
    /echo -aCyellow %% /tick-                Decrease the ticksize with 0.05 seconds

    ;;;; This is a modified version of tick.tf as found in tf-lib
    ;;;; Modifications:
    ;;; Mod - Made tick warning catch the eye
    ;;; New - 5 seconds tick warning
    ;;; Mod - /tickoff also kills the 5 seconds warning
    ;;; New - /tickshow, you can now run the counter without seeing a message
    ;;;                  if you need the counter, you can /tickshow to see the
    ;;;                  messages and have a synced tick counter ready
    ;;; Mod - Made /tickset show a message if show_tick is 1
    ;;; New - /tick+ and /tick-
    ;;; New - Triggers to sync tick counter
    ;;; New - Send TICK_EXE to the server but intercept it, see tick_exe below

    /loaded __TFLIB__/tick.tf

    ;Time between ticks in seconds
    /set ticksize=28.45

    /set next_tick=0
    /set _tick_pid1=0
    /set _tick_pid2=0
    /set _tick_pid3=0
    /set show_tick=1
    /set tickon_time 0

    /def -i tick_warn = /if (show_tick) /echo %%% Next tick in 10 seconds.%;/endif
    /def -i tick_warn2 = /if (show_tick) /echo %%% Next tick in 5 seconds.%;/endif
    /def -i tick_action = /if (show_tick) /echo %%%   ---===> TICK <===---   ---===> TICK <===---   ---===> TICK <===---%;/endif

    /def -i tick = \
            /if (next_tick) \
                    /eval /echo %%% $$[next_tick - $(/time @)] seconds until tick%;\
            /else \
                    /echo -e %% Tick counter is not running.%;\
            /endif

    ;Send TICK_EXE to the server,
    /def tick_exe = /send -h TICK_EXE

    ;but intercept it with a send hook.
    ;If you'd change the hook please be aware it needs a body to do anything at all!
    /def -i -p990 -mglob -h'send TICK_EXE' send_hook_TICK_EXE = /test

    ;If an other script needs to do anything on the tick all you need to do is
    ;create a similar send hook with a higher priority, like
    ;/def -E(some_boolean) -i -p991 -F -mglob -h'send TICK_EXE' myscript_tick = /some_command


    ;tick_on calls /tick_exe if it didn't do so in the past 4 seconds
    /def -i tickon = \
            /let tick_cur_t $(/time @)%;\
            /if (tickon_time + 4 < tick_cur_t) \
                    /tick_exe%;\
            /endif%;\
            /set tickon_time $(/time @)%;\
            /tickoff%;\
            /@test next_tick := $(/time @) + ticksize %;\
            /repeat -$[ticksize - 10] 1 \
                    /set _tick_pid1=0%%;\
                    /tick_warn%;\
            /set _tick_pid1=%?%;\
            /repeat -$[ticksize - 5] 1 \
                    /set _tick_pid3=0%%;\
                    /tick_warn2%;\
            /set _tick_pid3=%?%;\
            /repeat -%ticksize 1 \
                    /set _tick_pid2=0%%;\
                    /tick_action%%;\
                    /tickon%;\
            /set _tick_pid2=%?

    /def -i tickoff = \
            /@test _tick_pid1 & (kill(_tick_pid1), _tick_pid1:=0)%;\
            /@test _tick_pid3 & (kill(_tick_pid3), _tick_pid3:=0)%;\
            /@test _tick_pid2 & (kill(_tick_pid2), _tick_pid2:=0)%;\
            /set next_tick=0

    /def -i tickset = \
            /if (show_tick) \
                    /eval /echo %%% Tick counter offset $$[next_tick - $(/time @)]%;\
                    /echo %%%   ---===> TICK <===---   ---===> TICK <===---  synchronized%;\
    ;A spellup dropping was causing a lot of tick sync spam, so:
                    /set show_tick 0%;\
                    /repeat -0:0:2 1 /set show_tick 1%;\
            /endif%;\
            /tickon


    /def -i ticksize = /set ticksize %*

    /def -i tickshow = \
            /set show_tick $[!show_tick] %;\
            /if (show_tick) \
                    /echo %% Now showing tick messages%;\
            /else \
                    /echo %% No longer showing tick messages%;\
            /endif

    ;macros to fiddle with ticksize
    /def -i tick+ = /set ticksize $[ticksize + 0.05] %;/echo Ticksize is now %{ticksize} seconds.
    /def -i tick- = /set ticksize $[ticksize - 0.05] %;/echo Ticksize is now %{ticksize} seconds.


    ;triggers to sync tick counter
    /def -i -p200 -F -t"Corpse of * decays into dust." tick_corpse_decays = /tickset
    /def -i -p200 -F -t"Corpse of * decays leaving only a stench." tick_corpse_decays2 = /tickset
    /def -i -p200 -F -t"Corpse of * dissolves into smoke." tick_corpse_smokes = /tickset
    /def -i -p200 -F -t"Corpse of * gets taken by imps." tick_corpse_imped = /tickset
    /def -i -p200 -F -t"Corpse of * spontaneously combusts leaving only ash." tick_corpse_combusts = /tickset
    /def -i -p200 -F -t"Corpse of * liquifies into nothing." tick_corpse_liquifies = /tickset
    /def -i -p200 -F -t"Corpse of * is consumed by maggots." tick_corpse_consumed = /tickset
    /def -i -p200 -F -t"Corpse of * breaks apart into pieces." tick_corpse_breaks = /tickset
    /def -i -p200 -F -t"An imp grabs * and vanishes." tick_imp_grabs = /tickset
    /def -i -p200 -F -t"* decomposes." tick_something_decomposes = /tickset
    /def -i -p200 -F -t"The portal crackles suddenly, flares brightly, and is gone!" tick_portal = /tickset
    /def -i -p200 -F -t"The room shudders as the nexus implodes in on itself and vanishes!" tick_nexus = /tickset
    /def -i -p200 -F -t"You can't take the bright sunlight!" tick_no_sun_self = /tickset
    /def -i -p200 -F -mregexp -t"^([^ ]+) screams in pain from the sunlight!$" tick_no_sun_other = /tickset
    /def -i -p200 -F -t"The sky is getting cloudy." tick_sky_cloudy = /tickset
    /def -i -p200 -F -t"The clouds disappear." tick_clouds_disappear = /tickset
    /def -i -p200 -F -t"It starts to rain!" tick_rain_starts = /tickset
    /def -i -p200 -F -t"The rain has stopped." tick_rain_stops = /tickset
    /def -i -p200 -F -t"Lightning flashes in the sky." tick_lightning_flashes = /tickset
    /def -i -p200 -F -t"The lightning has stopped." tick_lightning_stopped = /tickset
    /def -i -p200 -F -t"The day has begun." tick_day_begun = /tickset
    /def -i -p200 -F -t"The night has begun." tick_night_begun = /tickset
    /def -i -p200 -F -t"The sun slowly disappears in the west." tick_sun_disappears = /tickset
    /def -i -p200 -F -t"The sun rises in the east." tick_sun_rises = /tickset
    /def -i -p200 -F -t"The protective aura fades from around your body." tick_sanctuary = /tickset
    /def -i -p200 -F -t"You slowly come out of your rage." tick_frenzy = /tickset
    /def -i -p200 -F -t"You feel less perceptive." tick_alertness = /tickset
    /def -i -p200 -F -t"Your senses return to normal." tick_heighten_senses = /tickset
    /def -i -p200 -F -t"You feel less righteous." tick_bless = /tickset
    /def -i -p200 -F -t"You feel less focused." tick_concentration = /tickset
    /def -i -p200 -F -t"Your battle sense has faded." tick_anticipate = /tickset
    /def -i -p200 -F -t"Your God's presence disappears." tick_prayer = /tickset
    /def -i -p200 -F -t"You no longer perceive auras." tick_detect_alignment = /tickset
    /def -i -p200 -F -t"You feel less armored." tick_armor = /tickset
    /def -i -p200 -F -t"Your Aura of Holiness fades..." tick_holy_aura = /tickset
    /def -i -p200 -F -t"You are no longer protected by your God." tick_holy_armor = /tickset
    /def -i -p200 -F -t"You feel less fatigued." tick_racial_fatigue = /tickset
    /def -i -p200 -F -t"One of your Exhausted spells has refreshed." tick_exhausted = /tickset
    /def -i -p200 -F -t"Your pulse slows and your body returns to normal." tick_regeneration = /tickset
    /def -i -p200 -F -t"You no longer feel invincible!" tick_invincibility = /tickset
    /def -i -p200 -F -t"Your lungs adapt to oxygen once again." tick_water_breathing = /tickset
    /def -i -p200 -F -t"Your skin softens and returns to normal." tick_iron_skin = /tickset
    /def -i -p200 -F -t"Your skin feels soft again." tick_stone_skin = /tickset
    /def -i -p200 -F -t"Your force shield shimmers then fades away." tick_shield = /tickset
    /def -i -p200 -F -t"You feel weaker." tick_giant_strength = /tickset
    /def -i -p200 -F -t"Your calcified flesh softens and returns to normal." tick_calcify = /tickset
    /def -i -p200 -F -t"You feel lighter as your bones return to normal." tick_steel_skeleton = /tickset
    /def -i -p200 -F -t"You gain a sense of reality." tick_overconfidence = /tickset
    /def -i -p200 -F -t"The voices in your head fall silent." tick_scramble = /tickset
    /def -i -p200 -F -t"You feel less savvy." tick_savvy = /tickset
    /def -i -p200 -F -t"You no longer feel quite so mellow." tick_calm = /tickset
    /def -i -p200 -F -t"You feel less insightful." tick_acumen = /tickset
    /def -i -p200 -F -t"Your innate frenzy subdues down to normal." tick_racial_frenzy = /tickset

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
