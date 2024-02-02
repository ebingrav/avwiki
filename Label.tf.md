To see the results without actually labeling just use "/label \<item\>"
while not standing at Edmuntrillion the ent. When you're confident, go
to the ent and do something like "remove all.acitem" and "/for c 1 16
/label %c.acitem" ;)  
  
New version:

-   kinda supports treasure/light
-   produces less spam in the room, it uses a get instead of a say to
    grab the keyword
-   implements a -x<command on exit> option much like the identify
    script to make it useful for other scripts.
-   fixes a minor bug, hr/dr enchants didn't make it into the label if
    the item was 0/0 by itself. Now it'll just add hr/dr no matter what.
    Also, draw strength shows up in the label.
-   identify now uses prompt_exe to get rid of the /repeat... meaning
    identify lag doesn't matter anymore

To use this script you'll need [verbose.tf](verbose.tf "wikilink"),
[identify.tf](identify.tf "wikilink") and
[prompt.tf](prompt.tf "wikilink"). The identify script tries to use
[regen.tf](regen.tf "wikilink") if you're out of mana, but you dont
really need that for the label stuff. You should have TINYPREFIX set in
your config file, pointing at the directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /loaded __SULFAR__/label.tf

    /eval /require -q %{TINYPREFIX}identify.tf
    /eval /require -q %{TINYPREFIX}verbose.tf

    /echo -aCyellow %% /label <item>         Label item
    /echo -aCyellow %%     [-x<command>]         execute command after label

    ;Current label format:
    ;
    ;armor:       l<level> b<base> [<enchant>] [<etch>]
    ;weapon:      l<level> <min>-<avg>-<max> <hr>/<dr> w<weight> [<etch>]
    ;bow:         l<level> <min>-<avg>-<max> [ds<drawstrength>] <hr>hr [<etch>]
    ;container:   l<level> c<capacity> w<weight> [<etch>]
    ;light:       l<level> [<etch>]
    ;treasure     l<level> [<etch>]

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and keep system verbosity level
    ;see verbose.tf
    /verbose -s-100 label

    /def -i label = \
            /if (!getopts("x:", "")) /return 0%; /endif%; \
            /set label_post %{opt_x}%;\
            /verbose -o%{verbosity_label} -l1 - -aCcyan %%% /%0: Labeling.  Post command is %{label_post}%;\
            /identify -e"get %1labeleok" -x/label_identify_done %1

    ;Setting global variable identify_grab to make /label work in loops
    ;That's ok because the last identify comes after this trigger and sets
    ;  identify_grab to 0 anyways.
    /def -i -p900 -F -mregexp -t"^I see no ([^ ]+)labeleok here.$" label_keyword = \
            /set identify_grab 1%;\
            /set label_keyword %{P1}%;\
            /verbose -o%{verbosity_label} -l2 - -aCyellow %%% /%0: Got keyword, %{label_keyword}

    /def -i label_identify_done = \
            /verbose -o%{verbosity_label} -l2 - -aCyellow %%% /%0: Compiling label string and labeling.%;\
            /let label_string=%;\
            /let _post=%{label_post}%;\
            /if (regmatch ("armor|weapon|bow|container|light|treasure", identify_type))\
                    /let label_string l%{identify_level}%;\
                    /if (identify_type =~ "armor") \
                            /let label_string %{label_string} b%{identify_armor_class}%;\
                            /if (identify_enchant_armor < 0) \
                                    /let label_string %{label_string} %{identify_enchant_armor}%;\
                            /endif%;\
                    /elseif (identify_type =~ "weapon") \
                            /let label_string %{label_string} %{identify_dam_min}-%{identify_dam_avg}-%{identify_dam_max}%;\
                            /let label_string %{label_string} $[identify_hr+identify_enchant_weapon]/$[identify_dr+identify_enchant_weapon]%;\
                            /let label_string %{label_string} w%{identify_weight}%;\
                    /elseif (identify_type =~ "bow") \
                            /let label_string %{label_string} %{identify_dam_min}-%{identify_dam_avg}-%{identify_dam_max}%;\
                            /if (identify_draw_strength !~ "") \
                                    /let label_string %{label_string} ds%{identify_draw_strength}%;\
                            /endif%;\
                            /let label_string %{label_string} $[identify_hr+identify_enchant_bow]hr%;\
                    /elseif (identify_type =~ "container") \
                            /let label_string %{label_string} c%{identify_capacity}%;\
                            /let label_string %{label_string} w%{identify_weight}%;\
                    /endif%;\
                    /if (identify_etch !~ "") \
                            /let label_string %{label_string} %{identify_etch}%;\
                    /endif%;\
                    /verbose -o%{verbosity_label} -l1 - -aCcyan %%% /%0: Labeling %{label_keyword} %{label_string}%;\
                    label %{label_keyword} %{label_string}%;\
            /else /verbose -o%{verbosity_label} -l1 - -aCcyan %%% /%0: I have no idea how to label %{label_keyword} of type %{identify_type}, sorry.%;\
            /endif%;\
            /unset label_post%;\
            /unset label_keyword%;\
            /if (_post !~ "") /eval -s0 %{_post}%; /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
