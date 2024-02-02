A prompt script:

-   Values are grabbed
-   prompt1 is substituted, hp/mana/monitor are colored to reflect their
    status
-   prompt2 containing surge/augment/quicken levels is gagged, the
    levels go into the status line and are colored
-   Provides an easy way for other scripts to trigger on a prompt
-   If you log in with a prompt that's incompatible with prompt.tf,
    it'll set the prompt to be compatible

Changes:

-   prompt_exe: instead of setting prompt_exe to 1, you can now set it
    to a string and it'll send "PROMPT_EXE <string>". Makes it easier to
    have multiple scripts use prompt_exe. See the chkaff macro in
    [affects.tf](affects.tf "wikilink") for an example.

Read the comments in the script for more information.

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /loaded __SULFAR__/prompt.tf

    ;These are the prompts I set on the mud.
    ;prompt <%h/%H(%p) %m/%M(%o) %v/%V [%T] %w/%W(%P) t%s>%n
    ;prompt2 <s%S a%A q%Q>%n
    ;also type "prompt2" once or twice and make sure it's on since the trigger below
    ;  gags it

    ;Prompt1 is substituted by substitute_prompt1.  It adds values to see the loss
    ;  in hp, mana and monitor's hp.  It also changes the color of hp/mana/monitor.
    ;  If hp is higher than 70% it's colored green, if it's between 30% and 70% it's
    ;  colored yellow, if it's lower than 30% it's colored red.  Moves are just
    ;  green currently.
    ;
    ;The substitute currently looks like:
    ;$&<D>[S]!@#<10/30(20|33%) 20/80(60|25%) 90mv 100/700(600|14%) t3>
    ;first the prompt symbols as usual,
    ;then hp: you're at 10 hp out of 30, that's 33%, you lost 20,
    ;then mana:  you're at 20 mana out of 80, that's 25% used 60,
    ;then moves: you're at 90 mv,
    ;then monitor: is at 100 out of 700, that's 14%, lost 600
    ;then the lag timer, 3 seconds.

    ;To save space on the prompt line, I put tnl in tf's statusline as [320] for
    ;  example.  Tnl also shifts color, green if it's above 300, yellow for 300 or
    ;  lower, bright yellow for 130 or lower.
    ;The AFK part of the prompt is captured but not displayed anywhere.

    ;prompt2 has values for surge, augment, quicken, is gagged and its values are
    ;  captured to status_ variables to use in the tf status line.  "Off" values
    ;  are changed to "0" to save some space on the satusline.  Surge, augment
    ;  and quicken levels are colored.  Surge and augment are green if they're off,
    ;  yellow if 2 or 3, bright yellow if 4 or higher.  Quicken is green if it's
    ;  off, bright yellow if it's on.

    ;I set my status line in a single command in my config file, this is what it
    ;  looks like with the right variables for this script
    ;/status_add -c @more:8:Br @world:9 :1 status_surge:2 :1 status_augment:2 :1 status_quicken:2 :1 status_tnl :1  insert:2 :1 @clock:16

    ;If you'd like to use the script and the prompt setting above contains all the
    ;  information you want but just doesn't look the way you want, I'd recommend
    ;  setting it to the format above anyways and then change the substitute below
    ;  to make it appear the way you want.

    ;If prompt_exe is set to a string tf will send a "PROMPT_EXE <string>" to Avatar
    ;  when it receives a prompt.  However, it intercepts the "PROMPT_EXE*" string
    ;  with a send hook of priority 990.  This allowes you to make a higher priority
    ;  send hook in a script to do something when you get a prompt in.  Please don't
    ;  change the value here, it's just here to initialize when the script loads.
    /set prompt_exe=

    ;set mud side prompt to be compatible with prompt.tf
    /def -i prompt_set = \
            prompt <%%h/%%H(%%p) %%m/%%M(%%o) %%v/%%V [%%T] %%w/%%W(%%P) t%%s>%%n%;\
            prompt2 <s%%S a%%A q%%Q>%%n%;\
            config +prompt%;\
            config +prompt2

    ;Hooray for regexps!
    /def -i -F -p400 -mregexp -t"^(\$|)(\&|)(\<D\>|)(\[S\]|)(\!|)(\@|)(\#|)(\*Deaf\* |)(\(AFK\) |)<(-?[0-9]*)/(-?[0-9]*)\((-?[0-9]*)\%\) (-?[0-9]*)/(-?[0-9]*)\((-?[0-9]*)\%\) (-?[0-9]*)/(-?[0-9]*) \[(-?[0-9]*)\] (-?[0-9]*)/(-?[0-9]*)\((-?[0-9]*)\%?\) t([0-9]+)>" substitute_prompt1 =\
    ;set prompt vars
            /set prompt_sanctuary %{P1}%;\
            /set prompt_werrebocler %{P2}%;\
            /set prompt_demonfire %{P3}%;\
            /set prompt_stance %{P4}%;\
            /set prompt_invis %{P5}%;\
            /set prompt_move_hidden %{P6}%;\
            /set prompt_sneak %{P7}%;\
            /set prompt_deaf %{P8}%;\
            /set prompt_afk %{P9}%;\
            /test prompt_hp:=strip_attr({P10})%;\
            /test prompt_hp_max:=strip_attr({P11})%;\
            /test prompt_hp_relative:=strip_attr({P12})%;\
            /test prompt_mana:=strip_attr({P13})%;\
            /test prompt_mana_max:=strip_attr({P14})%;\
            /test prompt_mana_relative:=strip_attr({P15})%;\
            /test prompt_move:=strip_attr({P16})%;\
            /test prompt_move_max:=strip_attr({P17})%;\
            /test prompt_tnl:=strip_attr({P18})%;\
            /test prompt_monitor:=strip_attr({P19})%;\
            /test prompt_monitor_max:=strip_attr({P20})%;\
            /test prompt_monitor_relative:=strip_attr({P21})%;\
            /test prompt_lagtimer:=strip_attr({P22})%;\
            /set prompt_hp_diff $[prompt_hp_max - prompt_hp]%;\
            /set prompt_mana_diff $[prompt_mana_max - prompt_mana]%;\
            /set prompt_monitor_diff -%;\
            /test (prompt_monitor_max !~ "-") & (prompt_monitor_diff:=prompt_monitor_max - prompt_monitor)%;\
    ;color hp
            /if (prompt_hp_relative <= 30) \
                    /test (prompt_hp:=decode_attr(prompt_hp, "Cred"), \
                            prompt_hp_max:=decode_attr(prompt_hp_max, "Cred"), \
                            prompt_hp_diff:=decode_attr(prompt_hp_diff, "Cred"), \
                            prompt_hp_relative:=decode_attr(prompt_hp_relative, "Cred"))%;\
            /elseif (prompt_hp_relative <= 70)  \
                    /test (prompt_hp:=decode_attr(prompt_hp, "Cyellow"), \
                            prompt_hp_max:=decode_attr(prompt_hp_max, "Cyellow"), \
                            prompt_hp_diff:=decode_attr(prompt_hp_diff, "Cyellow"), \
                            prompt_hp_relative:=decode_attr(prompt_hp_relative, "Cyellow"))%;\
            /else \
                    /test (prompt_hp:=decode_attr(prompt_hp, "Cgreen"), \
                            prompt_hp_max:=decode_attr(prompt_hp_max, "Cgreen"), \
                            prompt_hp_diff:=decode_attr(prompt_hp_diff, "Cgreen"), \
                            prompt_hp_relative:=decode_attr(prompt_hp_relative, "Cgreen"))%;\
            /endif%;\
    ;color mana
            /if (prompt_mana_relative <= 30) \
                    /test (prompt_mana:=decode_attr(prompt_mana, "Cred"), \
                            prompt_mana_max:=decode_attr(prompt_mana_max, "Cred"), \
                            prompt_mana_diff:=decode_attr(prompt_mana_diff, "Cred"), \
                            prompt_mana_relative:=decode_attr(prompt_mana_relative, "Cred"))%;\
            /elseif (prompt_mana_relative <= 70)  \
                    /test (prompt_mana:=decode_attr(prompt_mana, "Cyellow"), \
                            prompt_mana_max:=decode_attr(prompt_mana_max, "Cyellow"), \
                            prompt_mana_diff:=decode_attr(prompt_mana_diff, "Cyellow"), \
                            prompt_mana_relative:=decode_attr(prompt_mana_relative, "Cyellow"))%;\
            /else \
                    /test (prompt_mana:=decode_attr(prompt_mana, "Cgreen"), \
                            prompt_mana_max:=decode_attr(prompt_mana_max, "Cgreen"), \
                            prompt_mana_diff:=decode_attr(prompt_mana_diff, "Cgreen"), \
                            prompt_mana_relative:=decode_attr(prompt_mana_relative, "Cgreen"))%;\
            /endif%;\
    ;color monitor
            /if (prompt_monitor_relative <= 30) \
                    /test (prompt_monitor:=decode_attr(prompt_monitor, "Cred"), \
                            prompt_monitor_max:=decode_attr(prompt_monitor_max, "Cred"), \
                            prompt_monitor_diff:=decode_attr(prompt_monitor_diff, "Cred"), \
                            prompt_monitor_relative:=decode_attr(prompt_monitor_relative, "Cred"))%;\
            /elseif (prompt_monitor_relative <= 70)  \
                    /test (prompt_monitor:=decode_attr(prompt_monitor, "Cyellow"), \
                            prompt_monitor_max:=decode_attr(prompt_monitor_max, "Cyellow"), \
                            prompt_monitor_diff:=decode_attr(prompt_monitor_diff, "Cyellow"), \
                            prompt_monitor_relative:=decode_attr(prompt_monitor_relative, "Cyellow"))%;\
            /else \
                    /test (prompt_monitor:=decode_attr(prompt_monitor, "Cgreen"), \
                            prompt_monitor_max:=decode_attr(prompt_monitor_max, "Cgreen"), \
                            prompt_monitor_diff:=decode_attr(prompt_monitor_diff, "Cgreen"), \
                            prompt_monitor_relative:=decode_attr(prompt_monitor_relative, "Cgreen"))%;\
            /endif%;\
    ;color mv green
            /test (prompt_move:=decode_attr(prompt_move, "Cgreen"), \
                    prompt_move_max:=decode_attr(prompt_move_max, "Cgreen"))%;\
    ;color tnl
            /if (prompt_tnl > 500) /set prompt_tnl $[decode_attr(prompt_tnl, "Cgreen")]%;\
            /elseif (prompt_tnl > 300) /set prompt_tnl $[decode_attr(prompt_tnl, "BCgreen")]%;\
            /elseif (prompt_tnl > 150) /set prompt_tnl $[decode_attr(prompt_tnl, "Cyellow")]%;\
            /else /set prompt_tnl $[decode_attr(prompt_tnl, "BCyellow")]%;\
            /endif%;\
    ;for the status line
            /set status_tnl [%{prompt_tnl}]%;\
    ;color lagtimer
            /test prompt_lagtimer:=decode_attr(prompt_lagtimer, "Cgreen")%;\
    ;finally, the substitute
            /substitute -aCgreen \
    ;prompt symbols
            %{prompt_sanctuary}%{prompt_werrebocler}%{prompt_demonfire}\
            %{prompt_stance}%{prompt_invis}%{prompt_move_hidden}%{prompt_sneak}\
            %{prompt_deaf}\
    ;hp
            <%{prompt_hp}/%{prompt_hp_max}(%{prompt_hp_diff}|%{prompt_hp_relative}%%) \
    ;mana
            %{prompt_mana}/%{prompt_mana_max}(%{prompt_mana_diff}|%{prompt_mana_relative}%%) \
    ;mv
            %{prompt_move}mv \
    ;
    ;[tnl] -- uncomment the next line to restore tnl to the prompt you see
    ;       [%{prompt_tnl}] \
    ;
    ;monitor
            %{prompt_monitor}/%{prompt_monitor_max}(%{prompt_monitor_diff}|%{prompt_monitor_relative}%%) \
    ;lag timer
            t%{prompt_lagtimer}\
            >

    ;Grab prompt2 and set the status line variables
    ;Also, pretend to send PROMPT_EXE to the server if prompt_exe is set...
    /def -i -ag -F -p400 -mregexp -t"^<s([^ ]+) a([^ ]+) q([^ ]+)>$" substitute_prompt2 =\
            /test prompt_surge:=strip_attr({P1})%;\
            /test prompt_augment:=strip_attr({P2})%;\
            /test prompt_quicken:=strip_attr({P3})%;\
            /set status_surge s%{prompt_surge}%;\
            /set status_augment a%{prompt_augment}%;\
            /set status_quicken q%{prompt_quicken}%;\
            /test (status_surge =~ "soff") & (status_surge:="s0")%;\
            /test (status_augment =~ "aoff") & (status_augment:="a0")%;\
            /test (status_quicken =~ "qoff") & (status_quicken:="q0")%;\
    ;color surge
            /if (prompt_surge >= 4) \
                    /set status_attr_var_status_surge BCyellow%;\
            /elseif (prompt_surge >= 2) \
                    /set status_attr_var_status_surge Cyellow%;\
            /else \
                    /set status_attr_var_status_surge=%;\
            /endif%;\
    ;color augment
            /if (prompt_augment >= 4) \
                    /set status_attr_var_status_augment BCyellow%;\
            /elseif (prompt_augment >= 2) \
                    /set status_attr_var_status_augment Cyellow%;\
            /else \
                    /set status_attr_var_status_augment=%;\
            /endif%;\
    ;color quicken
            /if (prompt_quicken >= 1) \
                    /set status_attr_var_status_quicken BCyellow%;\
            /else \
                    /set status_attr_var_status_quicken=%;\
            /endif%;\
            /if (prompt_exe !~ "") /send -h PROMPT_EXE %{prompt_exe}%;/endif

    ;... except that we intercept the PROMPT_EXE with this send hook.  Other scripts
    ;  can now use a send hook with priority >990 to know when we got a prompt in.
    ;If you'd change the hook please be aware that unlike triggers, hooks apparently
    ;  need a body to do anything at all!
    /def -i -p990 -mglob -h'send PROMPT_EXE*' send_hook_PROMPT_EXE = /test

    ;Set the prompts right automagically at login
    /def -i -p900 -F -i -t"Welcome back to the AVATAR System*" prompt_wb = \
            /set prompt_lagtimer err%;\
            /set prompt_mana_diff err%;\
            /set prompt_surge err%;\
            /set prompt_quicken err%;\
            /repeat -0.5 1 /prompt_check

    /def -i prompt_check = \
            /if ((prompt_surge =~ "err") | \
                            (prompt_lagtimer =~ "err") | \
                            (prompt_surge =~ "err") | \
                            (prompt_quicken =~ "err")) \
                    /echo -aCcyan %%% Setting prompt compatible to prompt.tf%;\
                    /prompt_set%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
