This identify script was mostly cut from the old label script, to make
it useful for other things than just labeling. The script identifies an
item and grabs some info. You can use -e<command> to specify a command
that should run before the identify, which is useful if you'd expect to
loop the identify. See [label.tf](label.tf "wikilink").

You can use -x<command> to specify a command that should run when all
the info grabbing triggers had a chance to do their job. This is useful
for other scripts, again see [label.tf](label.tf "wikilink").

It also tries to look up the multiplier for the armor class of the slot
where its worn at, like 3.50 for a shield. It does this by converting
the object keyword to a variable name. See the list at the bottom of the
script. I currently use the values with armor opt. The list is complete
only as far as I've used it.

You can see which info it grabs by looking at identify_init, at its
triggers, or by typing "/listvar identify\*". I've only added identify
lines I've been using.

To use it in a script I have TINYPREFIX set in my config file, pointing
at the directory with my tf scripts like

    /set TINYPREFIX=~/tinyfugue/

and load it with this line in the script:

    /eval /require -q %{TINYPREFIX}identify.tf

Identify uses [verbose.tf](verbose.tf "wikilink"),
[regen.tf](regen.tf "wikilink") and [prompt.tf](prompt.tf "wikilink") so
you'll need those as well if you want to use it.

    ;DEBUG indefinately incomplete identify_armor_multiplier_ variable list

    /loaded __SULFAR__/identify.tf

    /eval /require -q %{TINYPREFIX}prompt.tf
    /eval /require -q %{TINYPREFIX}regen.tf
    /eval /require -q %{TINYPREFIX}verbose.tf

    /echo -aCyellow %% /identify <item>      Identify item and grab info
    /echo -aCyellow %%     [-e<command>]         execute <command> before identify
    /echo -aCyellow %%     [-x<command>]         execute <command> on exit
    /echo -aCyellow %% /id [-x] [-e] <item>      short version of /identify ;)

    ;When identify's finished it sets an exit code
    ; 0         finished correctly
    ; no_item   couldn't find item

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and keep system verbosity level
    ;see verbose.tf
    /verbose -s-100 identify

    ;When the identify is finished it will set global var identify_exit to
    ;0              identify script exit normally
    ;no_item        couldn't find your item to identify

    ;do NOT set identify_post here
    /def -i identify_init = \
            /verbose -o%{verbosity_identify} -l2 - -aCyellow %%% /%0 initializing .... %;\
            /set identify_pre=%;\
            /set identify_object=%;\
            /set identify_type=%;\
            /set identify_flags=%;\
            /set identify_etch=%;\
            /set identify_weight=%;\
            /set identify_value=%;\
            /set identify_level=%;\
            /set identify_type=%;\
            /set identify_enchant_armor=%;\
            /set identify_enchant_weapon=%;\
            /set identify_enchant_bow=%;\
            /set identify_armor_class=%;\
            /set identify_armor_multiplier=%;\
            /set identify_dam_min=%;\
            /set identify_dam_max=%;\
            /set identify_dam_avg=%;\
            /set identify_hr=%;\
            /set identify_dr=%;\
            /set identify_draw_strength=%;\
            /set identify_capacity=%;\
            /set identify_flametongue=%;\
            /set identify_lightning=%;\
            /set identify_exit=

    /identify_init

    /def -i id = /identify %*

    /def -i identify = \
            /verbose -o%{verbosity_identify} -l1 - -aCcyan %%% /%0: Identifying and grabbing info.%;\
            /set identify_call /%0 %*%;\
            /verbose -o%{verbosity_identify} -l2 - -aCyellow %%% /%0: Identify was called as:  %identify_call%;\
            /if (!getopts("e:x:", "")) /return 0%; /endif%; \
            /set identify_pre %{opt_e}%;\
            /set identify_post %{opt_x}%;\
            /set identify_grab 1%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: identify_pre is %identify_pre.  identify_post is %identify_post.%;\
            /if (identify_pre !~ "") /eval -s0 %identify_pre %;/endif%;\
            c identify %1

    /def -i -E(identify_grab) -p900 -F -t"You are not carrying .+!" identify_no_item = \
            /identify_exit no_item%;\
            /verbose -o%{verbosity_identify} -l2 - -aCyellow %%% /%0: Not carrying that, exit no_item.  Post command is %identify_post%;\
            /if (identify_post !~ "") /eval %{identify_post}%; /endif

    /def -i -E(identify_grab) -p900 -F -t"You failed your identify due to lack of concentration!" identify_failed = \
            /verbose -o%{verbosity_identify} -l2 - -aCyellow %%% /%0: Retrying.....%;\
            /eval -s0 %{identify_call}

    /def -i -E(identify_grab) -p900 -F -t"You do not have enough mana to cast identify." identify_no_mana = \
            /verbose -o%{verbosity_identify} -l2 - -aCyellow %%% Yikes!  Out of mana.  Calling regen script and retrying.%;\
            /regen -x'%{identify_call}'

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^Object '(.+)' type ([^ ]+), extra flags (.+).$" identify_object = \
            /verbose -o%{verbosity_identify} -l2 - -aCyellow %%% /%0: Grabbing.....  Post command is %identify_post%;\
            /identify_init%;\
            /set identify_object %{P1}%;\
            /set identify_type %{P2}%;\
            /set identify_flags %{P3}%;\
            /if (regmatch("etched", identify_flags)) \
                    /test regmatch("(.+) ([^ ]+)$$", identify_object)%;\
                    /set identify_object %{P1}%;\
                    /set identify_etch %{P2}%;\
            /endif%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Object is %identify_object, type is %identify_type, etch is %identify_etch%;\
            /set prompt_exe identify

    /def -i -E(identify_grab) -p991 -mglob -h'send PROMPT_EXE identify' identify_PROMPT_EXE = \
            /set prompt_exe=%;\
            /identify_exit 0

    /def -i identify_exit = \
            /let post %{identify_post}%;\
            /set identify_grab 0%;\
            /set identify_pre=%;\
            /set identify_exit %1%;\
            /if (identify_type =~ "armor") \
                    /let var_postfix=%{identify_object}%;\
    ;DEBUG are there any weird characters in the keywords
    ;DEBUG other than the ' in the archer's gauntlet..?
                    /test var_postfix:=replace("'", "", var_postfix)%;\
                    /test var_postfix:=replace(" ", "_", var_postfix)%;\
                    /eval /set identify_armor_multiplier %%{identify_armor_multiplier_%{var_postfix}}%;\
                    /if (identify_armor_multiplier > 0) /verbose -o%{verbosity_identify} -l1 - -aCcyan %%% /%0: Known armor.  Its base multiplier is %identify_armor_multiplier%;/endif%;\
            /endif%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Exit code: %{identify_exit}%;\
            /verbose -o%{verbosity_identify} -l1 - -aCyellow %%% /%0: Finishing up.  Post command is %identify_post%;\
            /if (post !~ "") /eval -s0 %{post}%; /endif

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^Weight ([0-9]+), value ([0-9]+), level ([0-9]+).$" identify_level = \
            /set identify_weight %{P1}%;\
            /set identify_value %{P2}%;\
            /set identify_level %{P3}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Weight %identify_weight, Value %identify_value, Level %identify_level.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^Armor class is ([0-9]+)." identify_armor_class = \
            /set identify_armor_class %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Armor class is %identify_armor_class.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^Damage is ([0-9]+) to ([0-9]+) \(average ([0-9]+)\).$" identify_dam = \
            /set identify_dam_min %{P1}%;\
            /set identify_dam_max %{P2}%;\
            /set identify_dam_avg %{P3}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Damage, minimum: %identify_dam_min, average: %identify_dam_avg, maximum: %identify_dam_max

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^'enchant armor  'Modifies armor class by (-[0-9]+) continuous.$" identify_enchant_armor = \
            /set identify_enchant_armor %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Enchanted armor, by %identify_enchant_armor continuous.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^ Modifies damage roll by (-?[0-9]+) continuous.$" identify_dr = \
            /set identify_dr %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: It has a dr mod, %identify_dr continuous.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^ Modifies hit roll by (-?[0-9]+) continuous.$" identify_hr = \
            /set identify_hr %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: It has a hr mod, %identify_dr.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^'enchant weapon 'Modifies damage roll by ([0-9]+) continuous.$" identify_enchant_weapon = \
            /set identify_enchant_weapon %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Enchanted weapon, enchanted %identify_enchant_weapon/%identify_enchant_weapon.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^'enchant bow    'Modifies hit roll by ([0-9]+) continuous.$" identify_enchant_bow = \
            /set identify_enchant_bow %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: Enchanted bow, enchanted %identify_enchant_bow.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^Draw strength is ([0-9]+).$" identify_draw_strength = \
            /set identify_draw_strength %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: This bow's draw strength is %identify_draw_strength

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^Capacity is ([0-9]+) lbs.$" identify_capacity = \
            /set identify_capacity %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: This container has a capacity of %identify_capacity lbs.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^It has ([0-9]+) charges of flametongue.$" identify_flametongue = \
            /set identify_flametongue %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: This shield has %identify_flametongue charges of flametongue.

    /def -i -E(identify_grab) -p900 -F -mregexp -t"^It has ([0-9]+) charges of arc lightning.$" identify_lightning = \
            /set identify_lightning %{P1}%;\
            /verbose -o%{verbosity_identify} -l3 - -aCyellow %%% /%0: This shield has %identify_lightning charges arc lightning.

    ;To add values, create a line like these below.  The recipe for the variable
    ;name is: identify an object, take the 'object', remove all weird characters,
    ;then change spaces to underscores, prefix identify_armor_multiplier_ .
    ;Kinda clumsy but a quick hack and i don't expect to enchant hundreds of
    ;different items or something ;)
    /set identify_armor_multiplier_Antharian_Signet_ring                  1.00
    /set identify_armor_multiplier_carved_bone_necklace                   1.00
    /set identify_armor_multiplier_embroidered_breastplate_silk           3.50
    /set identify_armor_multiplier_teardrop_helmet                        1.25
    /set identify_armor_multiplier_crown_smoldering                       1.25
    /set identify_armor_multiplier_heroic_dragonscale_dragon_scale_skirt  1.25
    /set identify_armor_multiplier_fatewalkers_mystical_sandals           1.00
    /set identify_armor_multiplier_single_silver_archers_gauntlet         1.00
    /set identify_armor_multiplier_steel_bracers                          1.25
    /set identify_armor_multiplier_hero_heroes_shield                     3.50
    /set identify_armor_multiplier_shroud_cloth_unholy                    2.25
    /set identify_armor_multiplier_collar_steel_chained                   1.25
    /set identify_armor_multiplier_orosca_enchanted_wrist_guard           1.10
    /set identify_armor_multiplier_lemans_family_seal_talisman            1.00
    /set identify_armor_multiplier_mother_pearl                           1.00

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
