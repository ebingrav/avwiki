A script to store mud side configuration and filter options into
variables and restore them later. It also allows you to set defaults and
restore them with "/config_restore all" For more information, read the
comments in the first 100 lines.

This script uses [prompt.tf](prompt.tf "wikilink") and
[verbose.tf](verbose.tf "wikilink") so you'll want those if you want to
use this script. You should also have TINYPREFIX set in your config
file, pointing at the directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /loaded __SULFAR__/config.tf

    /require -q lisp.tf
    /eval /require -q %{TINYPREFIX}prompt.tf
    /eval /require -q %{TINYPREFIX}verbose.tf

    ;Scripts can use this script to store configuration and filter options from
    ;  Avatar into variables.  Example for a call from myscript
    ;
    ;       /config_store -smyscript -x/myscript_config_done
    ;
    ;/def -i myscript_config_done = \
    ;       config +notake%;\
    ;       config +nosummon%;\
    ;       /myscript_continues
    ;
    ;/def -i myscript_exit = \
    ;       /config_restore -smyscript notake%;\
    ;       /config_restore -smyscript nosummon%;\
    ;       /config_rmset myscript


    /def -i config_usage = \
            /echo -aCyellow %%% /config_store         Stores mud configuration%;\
            /echo -aCyellow %%%     [-s<name>]            name for this set of options, default: avatar%;\
            /echo -aCyellow %%%     [-x<command>]         execute <command> on exit%;\
            /echo -aCyellow %%% /config_restore <option> Restore config <option>%;\
            /echo -aCyellow %%% /config_restore all   Restore all config options from this set%;\
            /echo -aCyellow %%%     [-s<name>]            name for this set of options, default: avatar%;\
            /echo -aCyellow %%% /config_rmset <set>   Remove config <set> from memory

    /def -i filter_usage = \
            /echo -aCyellow %%% /filter_store         Stores mud filter configuration%;\
            /echo -aCyellow %%%     [-s<name>]            name for this set of options, default: avatar%;\
            /echo -aCyellow %%%     [-x<command>]         execute <command> on exit%;\
            /echo -aCyellow %%% /filter_restore <option> Restore filter <option>%;\
            /echo -aCyellow %%% /filter_restore all   Restore all filter options from this set%;\
            /echo -aCyellow %%%     [-s<name>]            name for this set of options, default: avatar%;\
            /echo -aCyellow %%% /filter_rmset <set>   Remove filter <set> from memory

    ;Default config.  If you interrupt a script or otherwise mess up your config
    ;  you can just use "/config_restore all".
    ;To produce this list I used "/config_store" and "/listvar config_avatar*".
    /set config_avatar_autoexit=    +autoexit
    /set config_avatar_autogold=    +autogold
    /set config_avatar_autogroup=   +autogroup
    /set config_avatar_autoloot=    -autoloot
    /set config_avatar_autopull=    +autopull
    /set config_avatar_autosac=     -autosac
    /set config_avatar_autosplit=   +autosplit
    /set config_avatar_autotitle=   +autotitle
    /set config_avatar_battlenone=  -battlenone
    /set config_avatar_battleother= -battleother
    /set config_avatar_battleself=  -battleself
    /set config_avatar_blank=       +blank
    /set config_avatar_blind=       -blind
    /set config_avatar_brief=       -brief
    /set config_avatar_cmdcolor=    +cmdcolor
    /set config_avatar_color=       +color
    /set config_avatar_combine=     +combine
    /set config_avatar_condition=   -condition
    /set config_avatar_demonbank=   +demonbank
    /set config_avatar_keepalive=   -keepalive
    /set config_avatar_label=       +label
    /set config_avatar_nobeep=      -nobeep
    /set config_avatar_nodisturb=   -nodisturb
    /set config_avatar_nogenesis=   -nogenesis
    /set config_avatar_nosummon=    -nosummon
    /set config_avatar_notake=      -notake
    /set config_avatar_prompt=      +prompt
    /set config_avatar_prompt2=     +prompt2
    /set config_avatar_scoreaff=    -scoreaff
    /set config_avatar_telnetga=    -telnetga
    /set config_avatar_list= autoexit autoloot autogold autogroup autopull \
            autosplit autosac autotitle battleother battleself battlenone blank blind \
            brief color cmdcolor combine condition demonbank keepalive nobeep \
            nogenesis nosummon notake nodisturb label prompt prompt2 scoreaff telnetga
    /eval /set config_set_list $(/unique %{config_set_list} avatar)

    ;Default filter, for /filter_restore all
    /set filter_avatar_objectother=0
    /set filter_avatar_roomindiv=0
    /set filter_avatar_roomtotal=1
    /set filter_avatar_spellother=0
    /set filter_avatar_walkother=0
    /set filter_avatar_list= walkother roomindiv roomtotal spellother objectother
    /eval /set filter_set_list $(/unique %{filter_set_list} avatar)

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and keep system verbosity level
    ;see verbose.tf
    /verbose -s-100 config

    /config_usage
    /filter_usage

    /def -i config_store = \
            /set config_set=avatar%;\
            /if (!getopts("x:s:")) /return 0%; /endif%;\
            /if (opt_s !~ "") /set config_set %{opt_s}%;/endif%;\
            /set config_post %{opt_x}%;\
            /eval /set config_%{config_set}_list=%;\
            /set config_set_list $(/unique %{config_set_list} %{config_set})%;\
            /set config_store 1%;\
            /verbose -o%{verbosity_config} -l1 - -aCcyan %%% /%0: Grabbing configuration, storing in set %config_set%;\
            config

    /def -i -E(config_store) -t"\[ Keyword    \] Option" config_liststart = \
            /verbose -o%{verbosity_config} -l2 - -aCyellow %%% /%0: Start of configuration list%;\
            /set prompt_exe config_store

    /def -i -E(config_store) -mregexp -t"^\[(-|\+)([a-zA-Z0-9]+) *\] .+\.$" config_option = \
            /let _sign %{P1}%;\
            /let _option $[tolower({P2})]%;\
            /verbose -o%{verbosity_config} -l2 - -aCyellow %%% /%0: Got config option: %{_option}%;\
            /eval /set config_%{config_set}_list %%{config_%{config_set}_list} %%{_option}%;\
            /eval /set config_%{config_set}_%{_option} %{_sign}%{_option}

    /def -i -E(config_store) -p991 -mglob -h'send PROMPT_EXE config_store' config_prompt_exe = \
            /let _post %{config_post}%;\
            /set prompt_exe=%;\
            /unset config_store%;\
            /unset config_post%;\
            /verbose -o%{verbosity_config} -l1 - -aCcyan %%% /%0: Stored configuration in set %{config_set}, executing %{_post}%;\
            /unset config_set%;\
            /if (_post !~ "") /eval -s0 %{_post}%; /endif


    /def -i config_restore = \
            /let _set=avatar%;\
            /set config_noset 0%;\
            /if (!getopts("s:")) /return 0%; /endif%;\
            /if (opt_s !~ "") /let _set %{opt_s}%;/endif%;\
            /eval \
                    /if (!regmatch("autoexit", config_%{_set}_autoexit)) \
                            /set config_noset 1%%;\
                    /endif%;\
            /let _option %1%;\
            /if (config_noset) \
                    /verbose -o%{verbosity_config} -l1 - -aCred %%% %;\
                    /verbose -o%{verbosity_config} -l1 - -aCred %%% /%0: No configuration set %{_set} stored!%;\
                    /verbose -o%{verbosity_config} -l1 - -aCred %%% %;\
            /elseif (_option =~ "all") \
                    /eval /mapcar /config_restore %%{config_%{_set}_list}%;\
            /elseif (_option !~ "") \
                    /eval config %%{config_%{_set}_%{_option}}%;\
            /else \
                    /config_usage%;\
            /endif%;\
            /verbose -o%{verbosity_config} -l3 - -aCyellow %%% /%0: Option %{_option} restored using set %{_set}%;\
            /unset config_noset

    /def -i config_rmset = \
            /let _set %*%;\
            /if (_set =~ "") /let _set=avatar%;/endif%;\
            /eval /set rmset_options %%{config_%{_set}_list}%;\
            /let _length $(/length %{rmset_options})%;\
            /let _counter=1%;\
            /while (_counter <= _length) \
                    /unset config_%{_set}_$(/nth %{_counter} %{rmset_options})%;\
                    /test ++_counter%;\
            /done%;\
            /set config_set_list $(/remove %{_set} %{config_set_list})%;\
            /unset config_%{_set}_list%;\
            /unset rmset_options%;\
            /if (config_set_list =~ "") /unset config_set_list%;/endif%;\
            /verbose -o%{verbosity_config} -l1 - -aCcyan %%% /%0: Configuration set %{_set} removed.

    ; Same for filter

    /def -i filter_store = \
            /set filter_set=avatar%;\
            /if (!getopts("x:s:")) /return 0%; /endif%;\
            /if (opt_s !~ "") /set filter_set %{opt_s}%;/endif%;\
            /set filter_post %{opt_x}%;\
            /eval /set filter_%{filter_set}_list=%;\
            /set filter_set_list $(/unique %{filter_set_list} %{filter_set})%;\
            /set filter_store 1%;\
            /verbose -o%{verbosity_config} -l1 - -aCcyan %%% /%0: Grabbing filters, storing in set %filter_set%;\
            filter

    /def -i -E(filter_store) -t"Filter Type           Status" filter_liststart = \
            /verbose -o%{verbosity_config} -l2 - -aCyellow %%% /%0: Start of filter list%;\
            /set prompt_exe filter_store

    /def -i -E(filter_store) -mregexp -t"^([A-Z]+).+(ON|off)" filter_option = \
            /let _option $[tolower({P1})]%;\
            /let _sign %{P2}%;\
            /verbose -o%{verbosity_config} -l2 - -aCyellow %%% /%0: Got filter: %{_option}%;\
            /eval /set filter_%{filter_set}_list %%{filter_%{filter_set}_list} %%{_option}%;\
            /if (_sign =~ "off") \
                    /eval /set filter_%{filter_set}_%{_option} 0%;\
            /else \
                    /eval /set filter_%{filter_set}_%{_option} 1%;\
            /endif

    /def -i -E(filter_store) -p991 -mglob -h'send PROMPT_EXE filter_store' filter_prompt_exe = \
            /let _post %{filter_post}%;\
            /set prompt_exe=%;\
            /unset filter_store%;\
            /unset filter_post%;\
            /verbose -o%{verbosity_config} -l1 - -aCcyan %%% /%0: Stored filters in set %{filter_set}, executing %{_post}%;\
            /unset filter_set%;\
            /if (_post !~ "") /eval -s0 %{_post}%; /endif


    /def -i filter_restore = \
            /let _set=avatar%;\
            /set filter_noset 0%;\
            /if (!getopts("s:")) /return 0%; /endif%;\
            /if (opt_s !~ "") /let _set %{opt_s}%;/endif%;\
            /eval \
                    /if (!regmatch("0|1", filter_%{_set}_objectother)) \
                            /set filter_noset 1%%;\
                    /endif%;\
            /let _option %1%;\
            /if (filter_noset) \
                    /verbose -o%{verbosity_config} -l1 - -aCred %%% %;\
                    /verbose -o%{verbosity_config} -l1 - -aCred %%% /%0: No filter set %{_set} stored!%;\
                    /verbose -o%{verbosity_config} -l1 - -aCred %%% %;\
            /elseif (_option =~ "all") \
                    /eval /mapcar /filter_restore %%{filter_%{_set}_list}%;\
            /elseif (_option !~ "") \
                    /eval \
                            /if (filter_%{_set}_%{_option}) \
                                    filter +%%{_option}%%;\
                            /else \
                                    filter -%%{_option}%%;\
                            /endif%;\
                    /verbose -o%{verbosity_config} -l3 - -aCyellow %%% /%0: Filter %{_option} restored using set %{_set}%;\
            /else \
                    /filter_usage%;\
            /endif%;\
            /unset filter_noset

    /def -i filter_rmset = \
            /let _set %*%;\
            /if (_set =~ "") /let _set=avatar%;/endif%;\
            /eval /set rmset_options %%{filter_%{_set}_list}%;\
            /let _length $(/length %{rmset_options})%;\
            /let _counter=1%;\
            /while (_counter <= _length) \
                    /unset filter_%{_set}_$(/nth %{_counter} %{rmset_options})%;\
                    /test ++_counter%;\
            /done%;\
            /set filter_set_list $(/remove %{_set} %{filter_set_list})%;\
            /unset filter_%{_set}_list%;\
            /unset rmset_options%;\
            /if (filter_set_list =~ "") /unset filter_set_list%;/endif%;\
            /verbose -o%{verbosity_config} -l1 - -aCcyan %%% /%0: Filter set %{_set} removed.

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
