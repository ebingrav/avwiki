Bug reports, suggestions and/or diffs are appreciated, **sulfar** \_AT\_
*inbox* +DOT+ *com*

    ;;;=== ircII style tabbing for tinyfugue  ===
    ;; 1> You give or get a tell.
    ;; 2> Name of person is stored in a list.
    ;; 3> When you hit tab, "tell <person>" will appear on the input line.
    ;;    Where <person> is the last person you added.
    ;; 4> If you hit tab again, the name of the person prior to the last
    ;;    is used. Etcetc. If last name is reached, next tab will cycle thru.
    ;; 5> ESC-Tab will remove the name currently on the input line from the list,
    ;;    and clear the input line.
    ;; 6> Try it... it rocks :-)

    /require lisp.tf

    /set tabbing_name_list=
    /set tabbing_name_index=0

    /def -i tabbing_get_name = \
        /if (tabbing_name_index > 1) \
            /nth $[tabbing_name_index] %tabbing_name_list %;\
            /set tabbing_name_index=$[tabbing_name_index-1] %;\
        /else \
            /nth $[tabbing_name_index] %tabbing_name_list %;\
            /set tabbing_name_index=$(/length %tabbing_name_list) %;\
        /endif

    /def -i tabbing_add_name = \
        /if (tabbing_name_index = 0) \
            /set tabbing_name_list=%{1} %;\
            /set tabbing_name_index=1 %;\
        /else \
            /set tabbing_name_list=$(/remove %{1} %tabbing_name_list) %;\
            /set tabbing_name_list=%tabbing_name_list %{1} %;\
            /set tabbing_name_index=$(/length %tabbing_name_list) %;\
        /endif

    ;Bind to TAB key:
    ;/def -i -b'^I' keyForwardName  = \
    /def -i key_tab = \
        /if (tabbing_name_index > 0) \
            /grab tell $(/tabbing_get_name) %;\
            /test input (" ") %;\
        /else \
            /echo %%% No names in list! %;\
        /endif

    ;ESC - TAB will remove the name displayed on the input line and clear the line:
    ;/def -ib'^[^I' removeCurrentName = \
    /def -i key_esc_tab = \
        /test kbgoto(kbwordleft()) %;\
        /set name=$[kbtail()] %;\
        /set tabbing_name_list=$(/remove %{name} %tabbing_name_list) %;\
        /set tabbing_name_index=$(/length %tabbing_name_list) %;\
        /test kbgoto(kblen()) %;\
        /test kbdel(0) %;\
        /unset name

    /def -i -mregexp -F -p100 -t"^([^ ]+) tells you '" tabbing_tells_you = \
        /if (({P1} !~ "Someone") & ({P1} !~ "someone")) \
            /tabbing_add_name %{P1} %;\
        /endif

    /def -i -mregexp -F -p100 -t"^You dream of ([^ ]+) telling you '" tabbing_you_dream_telling = \
        /if (({P1} !~ "Someone") & ({P1} !~ "someone")) \
            /tabbing_add_name %{P1} %;\
        /endif

    /def -i -mregexp -F -p100 -t"^You tell ([^ ]+) "  tabbing_you_tell = \
        /if (({P1} !~ "the") & ({P1} !~ "Someone") & ({P1} !~ "someone")) \
            /tabbing_add_name %{P1} %;\
        /endif

    /def -i -mregexp -F -p100 -t"^([^ ]+) is asleep, but you tell (her|him|it) '" tabbing_tell_they_sleep = \
        /if (({P1} !~ "Someone") & ({P1} !~ "someone")) \
            /tabbing_add_name %{P1} %;\
        /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
