<h4>

Features

</h4>

So you have a pile of armor to enchant. And a couple of weapons and bows
to go with that. Well you toss it all in fodderbag, run this script, and
collect the results from upgradebag, sparebag or wastebag. Or if you
don't configure it with your current gear, you can collect the items
from brillbag, shimmerbag, wastebag. It accepts some options:

-   -l enchanting with an unshadowed lord
-   -w<level> level we want to wear the item at
-   -x<command> execute <command> when done

The script will use "get 1. fodderbag", identifies 1., and from then on
it'll use the keywords it got from identify. It will check the names of

-   the item you got from the fodderbag
-   the first item in your inventory
-   the name it gets by looking at the keywords it got from identify

If they're not all the same it won't enchant the item.

It sets config +blind, +combine, -condition, +nosummon, +notake,
+nodisturb and filter +spellother to decrease the chance of
anything/anyone messing up the script. That should make sure the
triggers don't respond on people cutting/pasting brills to channels, the
triggers work on you inventory, your regen is not interrupted and the
triggers won't respond to other players enchants. Config +blind doesn't
affect grtz channel but that is still covered by safechannel.tf. The
script uses config.tf to store/restore your configuration.

<h4>

Example config file

</h4>

The script will try to load an enchantbag.gear.tf file with a
configuration of gear. It's simply a list of variables, like

    /set enchantbag_hero_essence_amorphous_orb                  11dr 10dr
    /set enchantbag_hero_hero_heroes_shield                     b12    b12
    /set enchantbag_hero_bow_sliver_glowing_moonlight_arc       21hr
    /set enchantbag_hero_lemans_family_seal_talisman            b12 -9 b12 -6

    ;To reuse a value for an equivalent item, you could
    /eval /set enchantbag_hero_mother_pearl %{enchantbag_hero_lemans_family_seal_talisman}

    ;Lord stuff
    /set enchantbag_lord_essence_amorphous_orb                  15dr 13dr

To make the variable name, you'd append "enchantbag\_" with either
"hero\_", "lord\_", or "lowmort\_", then append that with the keywords
you get from identify, remove "weird" characters (anything not in
\[a-zA-Z0-9 \]) and turn spaces into underscores.

The first value is the current gear, anything above that will go into
upgradebag. The second value is the lowest spare that should go into
sparebag. The script currently treats spares kinda aggressively, it'll
try to enchant a 12 -3 spare to a 12 -6 spare if the level permits, even
if you have 12 -3 as lowest spare to keep. Perhaps I should switch that
around, I don't know yet. You can omit the second value, it'll then use
the default ranges for spares.

If you're enchanting armor and want to use the configuration, you'll
also need to add the item to [identify.tf](identify.tf "wikilink") to
know the multiplier for the armor class. A bunch of them are already
there.

<h4>

Stuff you need

</h4>

This script uses [config.tf](config.tf "wikilink"),
[verbose.tf](verbose.tf "wikilink"), [enchant.tf](enchant.tf "wikilink")
and [identify.tf](identify.tf "wikilink") so you'll want those if you
want to use this script. Config.tf and identify.tf use
[prompt.tf](prompt.tf "wikilink"), and enchant.tf uses
[regen.tf](regen.tf "wikilink") and
[safechannel.tf](safechannel.tf "wikilink") so get those while you're at
it. <b>If you update this script from an older version, please update
these scripts as well,</b> or strange things might happen, or it might
not work at all. You don't need to cut and paste them all, you can
download them from [here](http://drieman.dyndns.org/avatar-tf/). People
with wget (mostly linux users) can just open a terminal window, cd to
their tf scripts directory and cut'n'past this all at once:

    wget http://drieman.dyndns.org/avatar-tf/enchantbag.tf \
      http://drieman.dyndns.org/avatar-tf/enchant.tf \
      http://drieman.dyndns.org/avatar-tf/config.tf \
      http://drieman.dyndns.org/avatar-tf/verbose.tf \
      http://drieman.dyndns.org/avatar-tf/identify.tf \
      http://drieman.dyndns.org/avatar-tf/prompt.tf \
      http://drieman.dyndns.org/avatar-tf/regen.tf \
      http://drieman.dyndns.org/avatar-tf/safechannel.tf

You should also have TINYPREFIX set in your config file, pointing at the
directory with tf scripts like

    /set TINYPREFIX=~/tinyfugue/

You'll also need some bags, look at the comments in the script for a
list of etches and christens.

<h4>

Examples

</h4>

Enchant a pile of stuff with a hero for hero tier:

    /enchantbag

Enchant a pile of mother pearls with an unshadowed lord but for hero
tier:

    /enchantbag -l

Enchant a pile of amorphous orbs or doom shards for lord and sleep
afterwards:

    /enchantbag -l -w125 -xsleep

If you'd want to see all debug spam it produces, try

    /verbose -s3

<h4>

Disclaimer

</h4>

I didn't test enchanting piles of lord eq. If you know any such piles
please let me know where I can pick them up ;)

I'm guessing a hero level bow would raise 5 levels if you'd enchant it
with a lord. I'm not sure. Also don't know why anybody would want to do
that so I can't be bothered to check it. I did check all the other level
increments.

I've used this script quite a number of times now and debugged it
extensively. It works fine for me. However, if you were to vape your b12
-9 shield or heros, or blow your 13/13 hero level doom shard, I'll be
more than happy to accept your bug report but will not accept any
responsibility for the items. I guess keeping your inventory clean is
the way to be 100% sure.

<h4>

The script

</h4>

    ;Bug reports, suggestions and/or diffs are appreciated, '''sulfar''' _AT_ ''inbox'' +DOT+ ''com''

    /eval /require -q %{TINYPREFIX}verbose.tf
    /eval /require -q %{TINYPREFIX}enchant.tf
    /eval /require -q %{TINYPREFIX}config.tf

    /echo -aCyellow %% /enchantbag           Enchant all items in fodderbag and distribute results
    /echo -aCyellow %%      [-l]                 you're an unshadowed lord.  Default: you're a hero
    /echo -aCyellow %%      [-w<level>]          you wear the item at level <level>.  Default 51.
    /echo -aCyellow %%      [-x<command>]        execute <command> on exit

    ;etch           christen
    ;fodderbag      Fodderbag
    ;upgradebag     Upgradebag
    ;sparebag       Sparebag
    ;wastebag       Wastebag
    ;brillbag       Brillbag
    ;shimmerbag     Shimmerbag
    ;
    ;Only the rename of the fodderbag matters, enchantbag_got_item triggers on it
    ;Use the same name or change the trigger to match your bag

    ;When the script exits it sets global var enchantbag_exit to
    ;ok             enchantbag finished correctly with an empty fodderbag
    ;enchant_err    we got an error code from enchant.tf
    ;no_fodderbag   couldnt find the fodderbag
    ;wrong_name     names didn't check out

    ;Set to 0-3 to override system verbosity level
    ;Set to -100 to disable script verbosity level and keep system verbosity level
    ;see verbose.tf
    /verbose -s-100 enchantbag

    ;Range to use if we didn't set the lowest spare to keep in the config file
    ;Range includes the item in the gear, so if the armor spare range is 4 and the
    ;item in our gear is a 12 -9 lemans seal, the lowest upgrade would be b12 -10,
    ;and the spares could be b12 -7, b12 -8, b12 -9.  If the level permits
    ;enchanting tho, the script *will* try to make a b12 -9 spare out of a b12 -6,
    ;or enchant a 12-9 spare to a 12-12 upgrade.
    /set enchantbag_armor_spare_range=3
    /set enchantbag_weapon_spare_range=2
    /set enchantbag_bow_spare_range=3


    ;Load file with current gear
    /set enchantbag_gear_file=enchantbag.gear.tf
    /eval /set enchantbag_path=%{TINYPREFIX}
    /eval /load -q %{enchantbag_path}%{enchantbag_gear_file}

    ;should NOT set enchantbag_post here
    /def -i enchantbag_init = \
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0 initializing .... %;\
            /set enchantbag=0%;\
            /set enchantbag_name=%;\
            /set enchantbag_name_check=0%;\
            /set enchantbag_inv=0%;\
            /set enchantbag_inv_name=%;\
            /set enchantbag_level_increment=0%;\
            /set enchantbag_enchant_count=0%;\
            /set enchantbag_max_level=0%;\
            /set enchantbag_fade=0%;\
            /set enchantbag_shimmer=0%;\
            /set enchantbag_brill=0%;\
            /set enchantbag_vape=0%;\
            /set enchantbag_item_known=0%;\
            /set enchantbag_upgrade=0%;\
            /set enchantbag_possible_upgrade=0%;\
            /set enchantbag_possible_spare=0%;\
            /set enchantbag_spare=0%;\
            /set enchantbag_exit=

    /enchantbag_init

    /def -i enchantbag = \
            /enchantbag_init%;\
            /set enchantbag=1%;\
            /set enchantbag_enchanter_level=51%;\
            /set enchantbag_wear_level=51%;\
            /verbose -o%{verbosity_enchantbag} -l1 - -aCcyan %%% /%0: Start processing bag.%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: Loading %{enchantbag_path}%{enchantbag_gear_file}.%;\
            /load -q %{enchantbag_path}%{enchantbag_gear_file}%;\
            /if (!getopts("lx:w#")) /return 0%; /endif%;\
            /if (opt_l) /set enchantbag_enchanter_level 125%;/endif%;\
            /if (opt_w) /set enchantbag_wear_level %{opt_w}%;/endif%;\
            /set enchantbag_post %{opt_x}%;\
            /config_store -senchantbag -x/enchantbag_config_done

    /def -i enchantbag_config_done = \
            /filter_store -senchantbag -x/enchantbag_filter_done

    /def -i enchantbag_filter_done = \
            config +blind%;\
            config +combine%;\
            config -condition%;\
            config +nosummon%;\
            config +notake%;\
            config +nodisturb%;\
            filter +spellother%;\
            /enchantbag_next_item

    /def -i enchantbag_next_item = \
            /enchantbag_init%;\
            /set enchantbag=1%;\
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: Getting next item from the fodderbag.%;\
            get 1. fodderbag

    ;Bag empty.  We're finished.
    /def -i -E(enchantbag) -p799 -F -t"You see nothing like that in Fodderbag." enchantbag_empty = \
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: Your fodderbag is empty.  Exiting /enchantbag.%;\
            /enchantbag_exit ok

    ;Wrong alt?  Fodderbag in a bag?
    /def -i -E(enchantbag) -p799 -F -t"I see no fodderbag here." enchantbag_no_fodderbag = \
            /verbose -o%{verbosity_enchantbag} -l1 - -aBCred %%% You are not carrying the fodderbag.  Exiting /enchantbag.%;\
            /enchantbag_exit no_fodderbag

    ;New item.
    /def -i -E(enchantbag) -p799 -F -mregexp -t"^You get (.+) from Fodderbag.$" enchantbag_got_item = \
            /set enchantbag_name %{P1}%;\
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% Got %{enchantbag_name}.  Going to identify it.%;\
            /identify -x/enchantbag_identify_done 1.

    /def -i enchantbag_identify_done = \
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: Done identifying.  Doing cross reference on name.%;\
            /set enchantbag_name_check 1%;\
            inv%;\
            look "%{identify_object}"

    /def -i -E(enchantbag_name_check) -p799 -t"You are carrying:" enchantbag_inv = \
            /def -1 -mregexp -p900 -t"^(\\( ?[0-9]+\\)|    ) (\\([a-zA-Z]+\\) )\*([^\\[]+)( \\[.+)\*\$" _enchantbag_inv = \
                    /set enchantbag_inv_name=%%{P3}%%;\
                    /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%%% /%%0: First item in inventory is %%enchantbag_inv_name

    /def -i -E(enchantbag_name_check) -mregexp -t"^You look at (.+) in your inventory...$" enchantbag_name_check = \
            /let identify_name %{P1}%;\
            /set enchantbag_name_check 0%;\
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: Looking at %identify_object, we got: %identify_name%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: First item in inventory is %enchantbag_inv_name%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We got %enchantbag_name from the fodderbag.%;\
            /if ((identify_name =~ enchantbag_name) & (identify_name =~ enchantbag_inv_name)) \
                    /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: Names ok.%;\
                    /repeat -0.3 1 /enchantbag_processor%;\
            /else \
                    /verbose -o%{verbosity_enchantbag} -l1 - -aBCred %%% /%0: Names mismatch.  Exit.  Messy inventory?%;\
                    /enchantbag_exit wrong_name%;\
            /endif

    /def -i enchantbag_processor = \
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: Processing identify information.%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: brill is %enchantbag_brill, shimmer is %enchantbag_shimmer, level is %identify_level.%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: fade is %enchantbag_fade, vape is %enchantbag_vape, enchant_count is %enchantbag_enchant_count%;\
            /if (identify_type =~ "armor") \
                    /enchantbag_pre_armor%;\
            /elseif (identify_type =~ "weapon") \
                    /enchantbag_pre_weapon%;\
            /elseif (identify_type =~ "bow") \
                    /enchantbag_pre_bow%;\
            /endif%;\
            /if (enchantbag_item_known) \
                    /if (enchantbag_upgrade) \
                            put "%{identify_object}" upgradebag%;\
                            save%;\
                            /verbose -o%{verbosity_enchantbag} -l2 - -aBCcyan %%% /%0: An upgrade!  Stashing... Next!%;\
                            /enchantbag_next_item%;\
                    /elseif (enchantbag_possible_upgrade) \
                            /verbose -o%{verbosity_enchantbag} -l2 - -aBCcyan %%% /%0: A possible upgrade!  Enchanting.%;\
                            /enchant -n"%{enchantbag_name}" -i -x/enchantbag_enchant_done "%{identify_object}"%;\
                    /elseif (enchantbag_possible_spare) \
                            /verbose -o%{verbosity_enchantbag} -l2 - -aBCcyan %%% /%0: A possible spare!  Enchanting.%;\
                            /enchant -n"%{enchantbag_name}" -i -x/enchantbag_enchant_done "%{identify_object}"%;\
                    /elseif (enchantbag_spare) \
                            /verbose -o%{verbosity_enchantbag} -l2 - -aBCcyan %%% /%0: A spare!  Stashing... Next!%;\
                            put "%{identify_object}" sparebag%;\
                            /enchantbag_next_item%;\
                    /else \
                            /verbose -o%{verbosity_enchantbag} -l2 - -aBCcyan %%% /%0: This item goes to waste... Next!%;\
                            put "%{identify_object}" wastebag%;\
                            /enchantbag_next_item%;\
                    /endif%;\
            /else \
                    /verbose -o%{verbosity_enchantbag} -l2 - -aBCcyan %%% /%0: Item unknown.  Using simple distribution method.%;\
                    /if (enchantbag_brill) \
                            /verbose -o%{verbosity_enchantbag} -l3 -aCyellow %%% /%0: Putting item into brillbag.%;\
                            put "%{identify_object}" brillbag%;\
                            /enchantbag_next_item%;\
                    /elseif (enchantbag_shimmer) \
                            /verbose -o%{verbosity_enchantbag} -l3 -aCyellow %%% /%0: Putting item into shimmerbag.%;\
                            put "%{identify_object}" shimmerbag%;\
                            /enchantbag_next_item%;\
                    /elseif (identify_level < enchantbag_max_level) \
                            /enchant -n"%{enchantbag_name}" -i -x/enchantbag_enchant_done "%{identify_object}"%;\
                    /else \
                            /verbose -o%{verbosity_enchantbag} -l3 -aCyellow %%% /%0: Putting item into wastebag.%;\
                            put "%{identify_object}" wastebag%;\
                            /enchantbag_next_item%;\
                    /endif%;\
            /endif

    /def -i enchantbag_enchant_done = \
            /if (regmatch("^$|no_type|no_item|wrong_type", enchant_exit)) \
                    /verbose -o%{verbosity_enchantbag} -l1 - -aBCred %%% /%0: Enchant script exit status: %{enchant_exit}%;\
                    /verbose -o%{verbosity_enchantbag} -l1 - -aBCred %%% /%0: Exit enchantbag, enchant_err.%;\
                    /enchantbag_exit enchant_err%;\
            /elseif (enchant_exit =~ "wrong_name") \
                    /verbose -o%{verbosity_enchantbag} -l1 - -aBCred %%% /%0: Enchant script exit status: %{enchant_exit}%;\
                    /verbose -o%{verbosity_enchantbag} -l1 - -aBCred %%% /%0: Rechecking names.%;\
                    /identify -x/enchantbag_identify_done 1.%;\
            /elseif (enchant_exit =~ "vape") \
                    /verbose -o%{verbosity_enchantbag} -l2 - -aCred %%% /%0: Our item vaped...  Next!%;\
                    /enchantbag_next_item%;\
            /elseif (enchant_exit =~ "fade") \
                    /send get idontwanttoLID%;\
                    /test ++identify_level%;\
                    /set identify_enchant_armor 0%;\
                    /set identify_enchant_weapon 0%;\
                    /set identify_enchant_bow 0%;\
                    /set enchantbag_fade 1%;\
                    /set enchantbag_brill 0%;\
                    /set enchantbag_shimmer 0%;\
                    /set enchantbag_enchant_count 0%;\
                    /enchantbag_processor%;\
            /else \
                    /set enchantbag_fade 0%;\
                    /test identify_level += %enchantbag_level_increment%;\
                    /test ++enchantbag_enchant_count%;\
                    /if (enchant_exit =~ "shimmer") \
                            /test ++enchantbag_shimmer%;\
                            /if (identify_type =~ "armor") \
                                    /test identify_enchant_armor -= 2%;\
                            /elseif (identify_type =~ "weapon") \
                                    /test ++identify_enchant_weapon%;\
                            /elseif (identify_type =~ "bow") \
                                    /test identify_enchant_bow += 2%;\
                            /endif%;\
                    /elseif (regmatch("presence|brill", enchant_exit)) \
                            /test ++enchantbag_brill%;\
                            /if (identify_type =~ "armor") \
                                    /test identify_enchant_armor -= 3%;\
                            /elseif (identify_type =~ "weapon") \
                                    /test identify_enchant_weapon += 2%;\
                            /elseif (identify_type =~ "bow") \
                                    /test identify_enchant_bow += 3%;\
                            /endif%;\
                    /endif%;\
                    /enchantbag_processor%;\
            /endif

    /def -i enchantbag_pre_armor = \
            /set enchantbag_upgrade=0%;\
            /set enchantbag_possible_upgrade=0%;\
            /set enchantbag_possible_spare=0%;\
            /set enchantbag_spare=0%;\
            /set enchantbag_max_level $[enchantbag_wear_level + 3]%;\
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: What can we do with this piece of armor?%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: Our enchanter is level %enchantbag_enchanter_level%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We're enchanting to level %enchantbag_max_level max.%;\
            /if ((enchantbag_enchanter_level == 125) & (identify_level < 124)) \
                    /set enchantbag_level_increment 5%;\
            /else \
                    /set enchantbag_level_increment 1%;\
            /endif%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: The level will increase by %enchantbag_level_increment per enchant.%;\
            /if (!identify_armor_multiplier) \
                    /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We don't know the armor multiplier for %identify_object.%;\
                    /set enchantbag_item_known 0%;\
                    /return 1%;\
            /endif%;\
            /let id_total_armor=0%;\
            /let id_max_armor=0%;\
            /let acgear_stats=%;\
            /let acgear_base=0%;\
            /let acgear_enchant=0%;\
            /let acgear_total_armor=0%;\
            /let acgear_spare_base=0%;\
            /let acgear_spare_enchant=0%;\
            /let acgear_spare_armor=0%;\
            /let possible_enchants=0%;\
            /let id_total_armor $[(identify_armor_class * identify_armor_multiplier) - identify_enchant_armor]%;\
            /test acgear_stats:=enchantbag_get_stats(identify_object)%;\
            /if (acgear_stats =~ "") \
                    /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We don't know if %identify_object is in our acgear.%;\
                    /set enchantbag_item_known 0%;\
                    /return 1%;\
            /endif%;\
            /test regmatch("b([0-9]+) \*(-?\\+?[0-9]\*) \*b?([0-9]\*) \*(-?\\+?[0-9]\*)$$", acgear_stats)%;\
            /let acgear_base %{P1}%;\
            /let acgear_enchant %{P2}%;\
            /let acgear_spare_base %{P3}%;\
            /let acgear_spare_enchant %{P4}%;\
            /let acgear_total_armor $[acgear_base * identify_armor_multiplier - acgear_enchant]%;\
            /let acgear_spare_armor $[acgear_spare_base * identify_armor_multiplier - acgear_spare_enchant]%;\
            /if (!acgear_spare_armor) \
                    /let acgear_spare_armor $[acgear_total_armor - enchantbag_armor_spare_range + 1]%;\
            /endif%;\
            /test possible_enchants:=enchantbag_possible_enchants()%;\
            /let id_max_armor $[id_total_armor - (possible_enchants * -3)]%;\
            /set enchantbag_upgrade $[id_total_armor > acgear_total_armor]%;\
            /set enchantbag_possible_upgrade $[possible_enchants & (id_max_armor > acgear_total_armor)]%;\
            /set enchantbag_possible_spare $[possible_enchants & (id_max_armor >= acgear_spare_armor)]%;\
            /set enchantbag_spare $[id_total_armor >= acgear_spare_armor]%;\
            /set enchantbag_item_known 1%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCred %%% /%0: In our acgear, we have base %acgear_base, enchant %acgear_enchant%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCred %%% /%0: Lowest spare we want to keep is base %acgear_spare_base, enchant %acgear_spare_enchant%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCred %%% /%0: Armor class multiplier of this item is %identify_armor_multiplier%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCred %%% /%0: Acgear item has a total armor of %acgear_total_armor%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCred %%% /%0: Spare item has min armor of %acgear_spare_armor%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCmagenta %%% /%0: New item has a total armor of %id_total_armor%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCmagenta %%% /%0: We could make the total armor %id_max_armor%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: upgrade is %enchantbag_upgrade, possible_upgrade is %enchantbag_possible_upgrade%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: spare is %enchantbag_spare, possible_spare is %enchantbag_possible_spare

    /def -i enchantbag_pre_weapon = \
            /set enchantbag_upgrade=0%;\
            /set enchantbag_possible_upgrade=0%;\
            /set enchantbag_possible_spare=0%;\
            /set enchantbag_spare=0%;\
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: What can we do with this weapon?%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: Our enchanter is level %enchantbag_enchanter_level%;\
            /if (regmatch("essence amorphous orb|shard black doom marble", identify_object)) \
                    /set enchantbag_max_level $[enchantbag_wear_level + 3]%;\
            /else \
                    /set enchantbag_max_level $[enchantbag_wear_level + 5]%;\
            /endif%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We're enchanting to level %enchantbag_max_level max.%;\
            /if ((enchantbag_enchanter_level == 125) & (identify_level < 50)) \
                    /set enchantbag_level_increment 6%;\
            /elseif ((enchantbag_enchanter_level == 125) & (identify_level < 124)) \
                    /set enchantbag_level_increment 5%;\
            /elseif ((enchantbag_enchanter_level == 51) & (identify_level < 50)) \
                    /set enchantbag_level_increment 2%;\
            /else \
                    /set enchantbag_level_increment 1%;\
            /endif%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: The level will increase by %enchantbag_level_increment per enchant.%;\
            /let id_total_dr=0%;\
            /let id_max_dr=0%;\
            /let damgear_stats=%;\
            /let damgear_total_dr=0%;\
            /let damgear_spare_dr=0%;\
            /let possible_enchants=0%;\
            /let id_total_dr $[identify_dr+identify_enchant_weapon]%;\
            /let damgear_stats $[enchantbag_get_stats(identify_object)]%;\
            /if (damgear_stats =~ "") \
                    /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We don't know if %identify_object is in our gear.%;\
                    /set enchantbag_item_known 0%;\
                    /return 1%;\
            /endif%;\
            /test regmatch("([0-9]+)dr \*([0-9]+)?(dr)?$$", damgear_stats)%;\
            /let damgear_total_dr %{P1}%;\
            /let damgear_spare_dr %{P2}%;\
            /if (!damgear_spare_dr) \
                    /let damgear_spare_dr $[damgear_total_dr - enchantbag_weapon_spare_range + 1]%;\
            /endif%;\
            /test possible_enchants:=enchantbag_possible_enchants()%;\
            /let id_max_dr $[id_total_dr + (possible_enchants * 2)]%;\
            /set enchantbag_upgrade $[id_total_dr > damgear_total_dr]%;\
            /set enchantbag_possible_upgrade $[possible_enchants & (id_max_dr > damgear_total_dr)] %;\
            /set enchantbag_possible_spare $[possible_enchants & (id_max_dr >= damgear_spare_dr)] %;\
            /set enchantbag_spare $[id_total_dr >= damgear_spare_dr]%;\
            /set enchantbag_item_known 1%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCred %%% /%0: In our damgear, we have a %{damgear_total_dr}dr weapon%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCmagenta %%% /%0: New item has %id_total_dr dr.%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCmagenta %%% /%0: So we could make the total dr %id_max_dr%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: upgrade is %enchantbag_upgrade, possible_upgrade is %enchantbag_possible_upgrade%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: spare is %enchantbag_spare, possible_spare is %enchantbag_possible_spare

    /def -i enchantbag_pre_bow = \
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: The item is a bow.  Preprocessing.%;\
            /set enchantbag_upgrade=0%;\
            /set enchantbag_possible_upgrade=0%;\
            /set enchantbag_possible_spare=0%;\
            /set enchantbag_spare=0%;\
            /verbose -o%{verbosity_enchantbag} -l2 - -aCyellow %%% /%0: What can we do with this bow?%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: Our enchanter is level %enchantbag_enchanter_level%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We're enchanting to level %enchantbag_max_level max.%;\
            /if ((enchantbag_enchanter_level == 125) & (identify_level < 50)) \
                    /set enchantbag_level_increment 6%;\
            /elseif ((enchantbag_enchanter_level == 125) & (identify_level < 124)) \
    ;DEBUG don't actually know what the level would do if a lord enchanted a hero
    ;DEBUG bow, this is a guess.  Also wouldn't know why you'd want to do a thing
    ;DEBUG like that
                    /set enchantbag_level_increment 5%;\
            /elseif ((enchantbag_enchanter_level == 51) & (identify_level < 50)) \
                    /set enchantbag_level_increment 2%;\
            /else \
                    /set enchantbag_level_increment 1%;\
            /endif%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: The level will increase by %enchantbag_level_increment per enchant.%;\
            /let id_total_hr=0%;\
            /let id_max_hr=0%;\
            /let bow_cur_stats=%;\
            /let bow_cur_hr=0%;\
            /let bow_spare_hr=0%;\
            /let possible_enchants=0%;\
            /let id_total_hr $[identify_hr+identify_enchant_weapon]%;\
            /let bow_cur_stats $[enchantbag_get_stats(identify_object)]%;\
            /if (bow_cur_stats =~ "") \
                    /verbose -o%{verbosity_enchantbag} -l3 - -aCyellow %%% /%0: We don't know if %identify_object is in our gear.%;\
                    /set enchantbag_item_known 0%;\
                    /return 1%;\
            /endif%;\
            /test regmatch("([0-9]+)hr \*([0-9]+)?(hr)?$$", damgear_stats)%;\
            /let bow_cur_hr %{P1}%;\
            /let bow_spare_hr %{P2}%;\
            /if (!bow_spare_hr) \
                    /let bow_spare_hr $[bow_cur_hr - enchantbag_bow_spare_range + 1]%;\
            /endif%;\
            /test possible_enchants:=enchantbag_possible_enchants()%;\
            /let id_max_hr $[id_total_hr + (possible_enchants * 3)]%;\
            /set enchantbag_upgrade $[id_total_hr > bow_cur_hr]%;\
            /set enchantbag_possible_upgrade $[possible_enchants & (id_max_hr > bow_cur_hr)] %;\
            /set enchantbag_possible_spare $[possible_enchants & (id_max_hr >= bow_spare_hr)] %;\
            /set enchantbag_spare $[id_total_hr >= bow_spare_hr]%;\
            /set enchantbag_item_known 1%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCred %%% /%0: We have a %{bow_cur_hr}hr bow in our gear%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCmagenta %%% /%0: New item has %{id_total_hr}hr%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCmagenta %%% /%0: So we could make the total hr %{id_max_hr}hr%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: upgrade is %enchantbag_upgrade, possible_upgrade is %enchantbag_possible_upgrade%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aBCblue %%% /%0: spare is %enchantbag_spare, possible_spare is %enchantbag_possible_spare

    /def -i enchantbag_get_stats = \
            /if (enchantbag_wear_level == 51) \
                    /let var_mid hero%;\
            /elseif (enchantbag_wear_level == 125) \
                    /let var_mid lord%;\
            /elseif (enchantbag_wear_level <= 50) \
                    /let var_mid lowmort%;\
            /endif%;\
            /let var_right=%*%;\
    ;DEBUG are there any weird characters in the keywords
    ;DEBUG other than the ' in the archer's gauntlet..?
            /test var_right:=replace("'", "", var_right)%;\
            /test var_right:=replace(" ", "_", var_right)%;\
            /let varname enchantbag_%{var_mid}_%{var_right}%;\
            /eval /verbose -o%{verbosity_enchantbag} -l3 - -aCred %%%% /%0: Stats we have on file for %identify_object is %%{%{varname}}%;\
            /eval /return "%%{%{varname}}"

    /def -i enchantbag_possible_enchants = \
            /let pos_ench=0%;\
            /let counter %{identify_level}%;\
            /while (counter < enchantbag_wear_level - 1) \
                    /if (counter + enchantbag_level_increment <= enchantbag_max_level) \
                            /test ++pos_ench%;\
                    /endif%;\
                    /test counter+=enchantbag_level_increment%;\
            /done%;\
            /if (enchantbag_enchanter_level == enchantbag_wear_level) \
                    /test pos_ench+=$[enchantbag_max_level-(identify_level+(pos_ench*enchantbag_level_increment))]%;\
            /else \
                    /test pos_ench+=$[(enchantbag_max_level-(identify_level+(pos_ench*enchantbag_level_increment)))/enchantbag_level_increment]%;\
            /endif%;\
            /if (pos_ench < 0) /let pos_ench 0%;/endif%;\
            /verbose -o%{verbosity_enchantbag} -l3 - -aCmagenta %%% /%0: We could enchant %pos_ench more times%;\
            /return %{pos_ench}

    /def -i enchantbag_exit = \
            /let _post %enchantbag_post%;\
            /config_restore -senchantbag blind%;\
            /config_restore -senchantbag combine%;\
            /config_restore -senchantbag condition%;\
            /config_restore -senchantbag nosummon%;\
            /config_restore -senchantbag notake%;\
            /config_restore -senchantbag nodisturb%;\
            /config_rmset enchantbag%;\
            /filter_restore -senchantbag spellother%;\
            /filter_rmset enchantbag%;\
            /unset enchantbag%;\
            /unset enchantbag_name%;\
            /unset enchantbag_name_check%;\
            /unset enchantbag_inv%;\
            /unset enchantbag_inv_name%;\
            /unset enchantbag_level_increment%;\
            /unset enchantbag_enchant_count%;\
            /unset enchantbag_max_level%;\
            /unset enchantbag_fade%;\
            /unset enchantbag_shimmer%;\
            /unset enchantbag_brill%;\
            /unset enchantbag_vape%;\
            /unset enchantbag_item_known%;\
            /unset enchantbag_upgrade%;\
            /unset enchantbag_possible_upgrade%;\
            /unset enchantbag_possible_spare%;\
            /unset enchantbag_spare%;\
            /unset enchantbag_post%;\
            /unset enchantbag_enchanter_level%;\
            /unset enchantbag_wear_level%;\
            /set enchantbag_exit %1%;\
            /verbose -o%{verbosity_enchantbag} -l1 - -aCcyan %%% /%0 reports: We're finished.  Post command is: %{_post}%;\
            /if (_post !~ "") /eval -s0 /repeat -0.3 1 %{_post}%; /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
