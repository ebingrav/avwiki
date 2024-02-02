Provides

-   custom colors of affects list
-   /aff <spell> shows affects list just for <spell>
-   /sick shows how sick you are
-   /chkaff shows which spells you are \*not\* affected by
-   Sanctuary ticker on the satus line
-   Spellup ticker on the status line

The tickers grab the duration of sanctuary and holy aura from the
affects list, colors them and puts them in a status variable. It then
uses [tick.tf](tick.tf "wikilink") to make the tickers count down on the
tick. That also means that if the tick counter isn't running yet it
won't count down ;)

This script uses [prompt.tf](prompt.tf "wikilink") and
[tick.tf](tick.tf "wikilink") so you'll need those too. Also, you should
have TINYPREFIX set in your config file, pointing at the directory with
tf scripts like

    /set TINYPREFIX=~/tinyfugue/

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /require -q lisp.tf

    /eval /require -q %{TINYPREFIX}prompt.tf
    /eval /require -q %{TINYPREFIX}tick.tf

    /echo -aCyellow %% /aff <spell>          Shows affects list for <spell>
    /echo -aCyellow %% /sick                 Shows how sick you are
    /echo -aCyellow %% /chkaff               Shows which spells you are *not* affected by

    ;I set my status line in a single command in my config file, this is what it
    ;  looks like with the right variables for this script:
    ;/status_add -c @more:8:Br @world:9 :1 status_sanctuary:3 :1 status_spellup:3 :1 status_position insert:2 :1 @clock:16


    ;Custom color the affects list..
    ;Paint all of the affects list green
    /def -i -p999 -F -PCgreen -mregexp -t"^Spell: '.*" affects_green

    ;yellow for some spells i might want to check more than others
    /def -i -p998 -F -PCyellow -mregexp -t"^Spell: '(steel|iron|bark|pass|fly|water|heigh|conc).*" affects_yellow

    ;white for spells i most probably want to find quickly
    /def -i -p998 -F -PCwhite -mregexp -t"^Spell: '(sanc|regen|scramble|hand).*" affects_white

    ;You can "/aff sanc" and it'll only show the sanctuary line
    ;or use a regexp, "/aff sanc|fren"
    /def -i aff = \
            /def -t"You are affected by:" _aff_affected = \
                    /def -p900 -mregexp -t"^Spell: '%{*}.*" _aff_spell%%;\
                    /def -p899 -ag -mregexp -t"^Spell: '" _aff_gag_other_affects%%;\
                    /repeat -0.3 1 /undef _aff_affected _aff_spell _aff_gag_other_affects%;\
            affects

    ;Just a /sick macro.
    /def -i sick = \
            /set sick_poison 0%;\
            /set sick_disease 0%;\
            /def -t"You are affected by:" _sick_affected = \
                    /def -p900 -mregexp -t"^Spell: '(poison|toxin|biotoxin|venom)'.*" _sick_poison = /test $$$[++sick_poison] %%;\
                    /def -p900 -mregexp -t"^Spell: '(virus|plague)'.*" _sick_disease = /test $$$[++sick_disease]%%;\
                    /def -p899 -ag -mregexp -t"^Spell: '" _sick_gag_other_affects%%;\
                    /repeat -0.3 1 \
                            /undef _sick_affected _sick_poison _sick_disease _sick_gag_other_affects%%%;\
                    gtell %%%%% I am affected by: %%%sick_poison poisons and %%%sick_disease diseases%;\
            affects

    ;/chkaff will show you the spells from the chkaff_list by which
    ;you're *not* affected.
    /def -i chkaff = \
            /set chkaff_list= |BR| sanctuary |R| awen foci fortitudes \
                    invincibility barkskin steel_skeleton iron_skin |W| protection_evil bless holy_aura \
                    holy_armor armor water_breathing energy_shield displacement body_brace \
                    mental_barrier calcify_flesh biofeedback anticipate adrenaline_pump concentrate \
                    pass_door fly shield stone_skin giant_strength |G| frenzy |B| heighten_senses%;\
            /set chkaff 1%;\
            affects

    /def -i -E(chkaff) -p100 -F -t"You are affected by:" chkaff_affected = \
            /set prompt_exe chkaff

    /def -i -E(chkaff) -p100 -F -mregexp -t"^Spell: '(.+)' .+" chkaff_spell = \
            /let affect=$[replace(" ", "_", {P1})]%;\
            /set chkaff_list=$(/remove %affect %chkaff_list)

    /def -i -E(chkaff) -p991 -mglob -h'send PROMPT_EXE chkaff' chkaff_PROMPT_EXE = \
            /set prompt_exe=%;\
            /set chkaff 0%;\
            gtell %%% I am *not* affected by %chkaff_list

    ;Sanc and spellup tickers on the status line
    /def -i -p999 -F -t"You are affected by:" tickers_init = \
            /set sanctuary_duration=%;\
            /set spellup_duration=%;\
            /sanctuary_status%;\
            /spellup_status

    /tickers_init

    /def -i -p999 -F -mregexp -t"^Spell: 'sanctuary'  for ([0-9]+) hours.$" aff_sanc = \
            /test sanctuary_duration:=strip_attr({P1})%;\
            /sanctuary_status

    /def -i -p999 -F -t"The protective aura fades from around your body." sanctuary_fades = \
            /set sanctuary_duration=

    /def -i -p991 -F -mglob -h'send TICK_EXE' sanctuary_tick = \
            /if (sanctuary_duration > 0) /test --sanctuary_duration%;\
            /else /set sanctuary_duration=%;\
            /endif%;\
            /sanctuary_status

    /def -i sanctuary_status = \
            /set status_sanctuary \$%{sanctuary_duration}%;\
            /if (sanctuary_duration =~ "") \
                    /set status_attr_var_status_sanctuary=%;\
            /elseif (sanctuary_duration == 0) \
                    /set status_attr_var_status_sanctuary BCred%;\
            /elseif (sanctuary_duration <= 2) \
                    /set status_attr_var_status_sanctuary Cred%;\
            /elseif (sanctuary_duration <= 5) \
                    /set status_attr_var_status_sanctuary Cyellow%;\
            /else \
                    /set status_attr_var_status_sanctuary=%;\
            /endif

    /def -i -t"You are surrounded by a white aura." got_sanc = /aff sanc|holy aura

    ;Do the same for holy aura to have a spellup ticker
    /def -i -p999 -F -mregexp -t"^Spell: 'holy aura'  modifies armor class by -[0-9]+ for ([0-9]+) hours.$" aff_spellup = \
            /test spellup_duration:=strip_attr({P1})%;\
            /spellup_status

    /def -i -p999 -F -t"Your Aura of Holiness fades..." spellup_fades = \
            /set spellup_duration=

    /def -i -p991 -F -mglob -h'send TICK_EXE' spellup_tick = \
            /if (spellup_duration > 0) /test --spellup_duration%;\
            /else /set spellup_duration=%;\
            /endif%;\
            /spellup_status

    /def -i spellup_status = \
            /set status_spellup s%{spellup_duration}%;\
            /if (spellup_duration =~ "") \
                    /set status_attr_var_status_spellup=%;\
            /elseif (spellup_duration == 0) \
                    /set status_attr_var_status_spellup BCred%;\
            /elseif (spellup_duration <= 1) \
                    /set status_attr_var_status_spellup Cred%;\
            /elseif (spellup_duration <= 3) \
                    /set status_attr_var_status_spellup Cyellow%;\
            /else \
                    /set status_attr_var_status_spellup=%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
