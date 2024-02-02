This script provides

-   easy alt switching, one command will log out the current char and
    login the other
-   triggers to log in quickly scrolling by the motd and what not
-   a modified /world (stdlib.tf) which will not connect if tf has an
    active connection
-   a modification to /world to set secho off during the /@connect, this
    prevends the password from being displayed if you have secho on.
-   you can use /alt -x<command> <alt> to log in <alt> and have
    <command> executed when the alt logged in, so you could easily make
    a spellup macro that'd use 2 alts :)

You'll probably want to change the value of "main_alt" at the beginning
of this script.

<B>An older version had a bug</B> that creeped in when I changed the
trigger to a disconnect hook. It didn't turn it off after an alt switch.
If you'd use it to log in (create) a denied character, <B>the script
would loop trying to log in that character.</B> I suspect it'd do the
same if you'd have a wrong password in your config file. Big oops! If
you're using this script, plz make sure and update.

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''
    /loaded __SULFAR__/alt.tf

    /echo -aCyellow %% /alt                  (Logout and) login %{main_alt}
    /echo -aCyellow %% /alt <world>          (Logout and) login <world>
    /echo -aCyellow %%     [-x<command>]         execute <command> after alt switch
    /echo -aCyellow %% /relog                (Logout and) login current world

    ;Set this variable to your main alt
    /set main_alt sulfar

    /def alt_init = \
            /set alt_get=0%;\
            /set alt=%;\
            /set alt_post=%;\
            /set alt_execute_post=0

    ;;Quick, easy and safe alt switching
    /def -i alt = \
            /alt_init%;\
            /if (!getopts("x:", "")) /return 0%; /endif%; \
            /if (opt_x !~ "") \
                    /set alt_post %{opt_x}%;\
                    /set alt_execute_post 1%;\
            /endif%;\
            /set alt %*%;\
            /if (alt =~ "") /set alt %{main_alt}%;/endif%;\
            /if (is_open())\
                    /set alt_get 1%;\
                    quit%;\
            /else \
                    /world %{alt}%;\
                    /set alt=%;\
            /endif

    /def -i relog = /alt ${world_name}

    /def -i -E(alt_get) -p999 -F -h'disconnect' alt_disconnect = \
            /set alt_get 0%;\
            /world %{alt}%;\
            /set alt=

    /def -i -E(alt_execute_post) -p999 -F -t"Welcome back to the AVATAR System*" alt_welcome_back = \
            /let post %alt_post%;\
            /alt_init%;\
            /echo -aCcyan %%% /%0: Alt switch ready!  Executing command: %post%;\
            /if (post !~ "") /eval -s0 %{post}%; /endif

    ;Some triggers to scroll by the motd and what not
    /def -i -mregexp -t"AVATAR (PLAYER|MORTAL) INFORMATION CENTER" login_enter_1 = /eval /send%;/repeat -0:00:0.5 1 /eval /send %;/echo %%% trigger: %0
    /def -i -mregexp -t"Please press <enter> to continue" login_enter_2 = /eval /send%;/repeat -0:00:0.5 1 /eval /send %; /echo %%% trigger: %0
    /def -i -mregexp -t"Please ensure that you've read and understood the Rules." login_enter_3 = /eval /send%;/repeat -0:00:0.5 1 /eval /send %; /echo %%% trigger: %0
    /def -i -mregexp -t"please press *RETURN* to continue" login_enter_4 = /eval /send%;/repeat -0:00:0.5 1 /eval /send %; /echo %%% trigger: %0
    /def -i -mregexp -t"Press <ENTER> to continue" login_enter_5 = /eval /send%;/repeat -0:00:0.5 1 /eval /send %; /echo %%% trigger: %0


    ;Modified /world (stdlib.tf)
    ; will not connect if tf has an active connection to avoid multiplay accidents
    ; will turn secho off before connecting to prevend echoing the password to the
    ;   screen in case it was on.  The value of secho is restored after connecting.
    /def -i world = \
            /if (!getopts("nlqxfb", 0)) /return 0%; \
            /endif%; \
            /let _args=%*%; \
            /if (_args =~ "") \
                    /let _args=$(/nth 1 $(/@listworlds -s))%; \
                    /if (_args =/ "default") \
                            /let _args=$(/nth 2 $(/@listworlds -s))%; \
                    /endif%; \
            /endif%; \
            /let _opts=%; \
            /if (is_open(_args)) \
                    /if (opt_n) /let _opts=%_opts -n%; /endif%; \
                    /if (opt_q) /let _opts=%_opts -q%; /endif%; \
                    /@fg %_opts %_args%; \
            /elseif (is_open())\
                    /echo %% But you *are* connected!%;\
            /else \
                    /if (opt_l) /let _opts=%_opts -l%; /endif%; \
                    /if (opt_q) /let _opts=%_opts -q%; /endif%; \
                    /if (opt_x) /let _opts=%_opts -x%; /endif%; \
                    /if (opt_f) /let _opts=%_opts -f%; /endif%; \
                    /if (opt_b) /let _opts=%_opts -b%; /endif%; \
                    /let _secho %{secho}%;\
                    /set secho off%;\
                    /@connect %_opts %_args%; \
                    /set secho %{_secho}%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
