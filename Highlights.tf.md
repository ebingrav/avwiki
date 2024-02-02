Some useful highlights.

    ;;; Channel highlights/gags
    /def -mregexp -ag -t' town crier congratulates ([a-zA-Z]+) on (.*)!' gag_grtz
    /def -ag -mglob -t"People who share your buddy value:" gag_buddyshare

    ;;; Quest vial highlights (on ground and inventory).
    /def -mregexp -ag -t"^([\(\)0-9 ]*)([a-zA-Z\(\) ]+)[aA]+[n]* (.*) vial (of healing|sits here\.)$" quest_pot_sub = \
        /let vialtype=%P3%; \
        /if ({vialtype} =~ "red striped") /let hspells=heal%;/let spcolor=@{Cred}%; \
        /elseif ({vialtype} =~ "orange-brown") /let hspells=cure serious%;/let spcolor=@{Cyellow}%; \
        /elseif ({vialtype} =~ "orange") /let hspells=heal, cure light, cure critical%;/let spcolor=@{hCyellow}%; \
        /elseif ({vialtype} =~ "light brown") /let hspells=cure light%;/let spcolor=@{Cyellow}%; \
        /elseif ({vialtype} =~ "yellow") /let hspells=heal, cure serious%;/let spcolor=@{Cyellow}%; \
        /elseif ({vialtype} =~ "yellow-brown") /let hspells=heal, cure light%;/let spcolor=@{Cyellow}%; \
        /elseif ({vialtype} =~ "yellow-orange") /let hspells=heal, cure critical%;/let spcolor=@{Cyellow}%; \
        /elseif ({vialtype} =~ "reddish-brown") /let hspells=cure critical%;/let spcolor=@{Cred}%; \
        /elseif ({vialtype} =~ "orange-red") /let hspells=cure critical, cure light%;/let spcolor=@{Cred}%; \
        /elseif ({vialtype} =~ "red-brown") /let hspells=divinity%;/let spcolor=@{Cred}%; \
        /elseif ({vialtype} =~ "red-orange") /let hspells=divinity, cure light%;/let spcolor=@{Cred}%;\
        /elseif ({vialtype} =~ "red") /let hspells=divinity, cure serious%;/let spcolor=@{Cred}%;\
        /elseif ({vialtype} =~ "glowing gold") /let hspells=3x divinty%;/let spcolor=@{hCyellow}%;\
        /else /let hspells=N/A%;/let spcolor=@{Cwhite}%; \
        /endif%; \
        /echo -pw @{Cwhite}%{P1}%{P2}%{spcolor}%{vialtype} @{xCwhite}vial %P4 @{xCgreen}(%{spcolor}%{hspells}@{xCgreen})@{n}


    ;;; Battle highlights
    /def -aufhCwhite -P -mregexp -t"You are blinded\!" high_self_blinded
    /def -abufhCred -P -mregexp -t"You are zapped by" high_self_zapped
    /def -abufhCcyan -P -mregexp -t"disarms you and sends your weapon flying\!" high_self_disarmed
    /def -abufCyellow -P -mregexp -t"You frantically attempt to remove" high_self_decepted
    /def -p1 -mglob -ag -t"* flies out of *'s hand to attack *" high_gag_dwspam
    /def -p0 -mglob -ag -t"* scans in all directions, looking for trouble\!" high_gag_scan_spam

    ;;; Examine-type highlights
    /def -mregexp -aCyellow -t'^This has roughly ([0-9]+) uses remaining.' high_useremain
    /def -mglob -ahCyellow -t'Disarming traps.' high_disarmingtrap
    /def -mregexp -aCyellow -t'^It has roughly ([0-9]+) uses remaining.' high_fletchkitremain
    /def -mregexp -ahCyellow -t'^Each one carries ([a-zA-Z\-]+), a.*$' high_poisonarrowaff
    /def -mregexp -ag -mregexp -t'^It (improves|facilitates) ([a-zA-Z ]+).' high_archermodsaff = \
        /echo -pw @{Cyellow}It %P1 @{hCgreen}%P2@{xCyellow}.
    /def -mregexp -ahCyellow -t'^It carries roughly ([0-9]+) doses of ([a-zA-Z \-]+), a.*$' high_poisonweapaff
    /def -mregexp -ag -t'^Picking locks on ([a-zA-Z \-]+)' high_picklockaff = \
        /echo -pw @{Cyellow}Picking locks on @{hCgreen}%P1@{xCyellow}.@{n}

    ;;; Mob-type highlights
    ;;; If you bioscan a mob, or have racial-survey and look at a mobs 
    ;;; description these next two highlights will identify particularly nasty mobs

    ;;; hack attempt to highlight caster mobs
    /def -ah -mregexp -t", (a kinetic caster|a psion|a mage|a very learned caster|a cleric|a lich)," high_caster_mobs
    ;;; hack attempt to highligh stompers
    /def -ahCmagenta -mregexp -t", heavy-footed, " high_stomper_mob

Gear hilites:

    ;locates, hero ac
    /def -i -PCred -mregexp -t"^Orb of the Shogun in Emperor's pagoda.$" locate_orb_shogun
    /def -i -PCred -mregexp -t"^An Antharian signet ring carried by an Antharian Knight.$" locate_antharian_ring
    /def -i -PCred -mregexp -t"^A Templar Ring carried by Gunther the Knight.$" locate_templar_ring
    /def -i -PCred -mregexp -t"^Ring of Truth carried by A carrion crawler.$" locate_truth_ring
    /def -i -PCred -mregexp -t"^Carved bone necklace carried by Aruna.$" locate_bone_neck
    /def -i -PCred -mregexp -t"^An embroidered breastplate carried by Tean NaamAhn'Sa.$" locate_embr_brea
    /def -i -PCred -mregexp -t"^Carburized steel plate armor carried by Jackal.$" locate_carb_jackall
    /def -i -PCred -mregexp -t"^Carburized steel plate armor carried by Panther.$" locate_carb_panther
    /def -i -PCred -mregexp -t"^Teardrop Helm carried by The Sad Man.$" locate_teardrop_helm
    /def -i -PCred -mregexp -t"^A pile of red dragon scales carried by Nigurathus.$" locate_red_dra_sca
    /def -i -PCred -mregexp -t"^Golden scissors of fate carried by Atropos.$" locate_scissors_atropos
    /def -i -PCred -mregexp -t"^A spindle of golden thread carried by Clotho.$" locate_spindle_clotho
    /def -i -PCred -mregexp -t"^The rod of fate carried by Lachesis.$" locate_rod_lachesis
    /def -i -PCred -mregexp -t"^A single silver archer's gauntlet carried by * Ecstasy *.$" locate_arc_gaunt
    /def -i -PCred -mregexp -t"^A pair of steel bracers carried by The Robber Baron.$" locate_steel_bracer
    /def -i -PCred -mregexp -t"^The Shield of Heroes carried by The paladin's ghost.$" locate_shield_hero
    /def -i -PCred -mregexp -t"^The Unholy Shroud carried by Millament the mad Necromancer.$" locate_unholy_shroud
    /def -i -PCred -mregexp -t"^Svlad's other gauntlet carried by Tryystania the DracoLich.$" locate_svlad_gaunt
    /def -i -PCred -mregexp -t"^A chained steel collar carried by The Doom.$" locate_steel_collar
    /def -i -PCred -mregexp -t"^The LeMans family seal carried by the Beast LeMans.$" locate_lemans_seal

    ;locates, hero hit
    /def -i -PCred -mregexp -t"^an ice ring carried by Kroska.$" locate_ice_ring_kroska
    /def -i -PCred -mregexp -t"^an ice ring carried by Braugi.$" locate_ice_ring_braugi
    /def -i -PCred -mregexp -t"^Medallion of Justice carried by Zan and Jayna.$" locate_medallion_justice
    /def -i -PCred -mregexp -t"^Ultimoose Power carried by the Ulti-Moose.$" locate_ultimoose_power
    /def -i -PCred -mregexp -t"^Death armor carried by The Death Knight General.$" locate_death_armor
    /def -i -PCred -mregexp -t"^A silver breast plate carried by a Knight.$" locate_silver_breastplate
    /def -i -PCred -mregexp -t"^A helm of Cherubims carried by Saeren the king.$" locate_helm_cherubims
    /def -i -PCred -mregexp -t"^Flaming pants carried by the blazing man.$" locate_flaming_pants
    /def -i -PCred -mregexp -t"^Killing gloves carried by Faralia.$" locate_killing_gloves
    /def -i -PCred -mregexp -t"^Some spiked sleeves carried by Volse Pinescu.$" locate_spiked_sleeves
    /def -i -PCred -mregexp -t"^Some dragon scales carried by The Bronze Dragon, Kastinius.$" locate_scales_kastini
    us
    /def -i -PCred -mregexp -t"^A dark dwarven brand carried by the dark master-at-arms.$" locate_dwarven_brand
    /def -i -PCred -mregexp -t"^A robe of greatness carried by Numenor the lich.$" locate_robe_greatness
    /def -i -PCred -mregexp -t"^The heavy spear, 'Ocean' carried by Tean NaamAhn'Sa.$" locate_ocean_spear
    /def -i -PCred -mregexp -t"^Mighty claws carried by Slithk the devourer.$" locate_mighty_claws
    /def -i -PCred -mregexp -t"^A scorpion tattoo in The maze.$" locate_scorp_tat
    /def -i -PCred -mregexp -t"^the Glyph of Tao carried by The Chi of Zhang Kai.$" locate_glyph_tao
    /def -i -PCred -mregexp -t"^Sword of Avengers carried by sword of Avengers.$" locate_sword_avengers
    /def -i -PCred -mregexp -t"^Baron's sword carried by The Robber Baron.$" locate_baron_sword
    /def -i -PCred -mregexp -t"^An orb of amorphous essence carried by the changeling." locate_orb_ess
    /def -i -PCred -mregexp -t"^A shard of black Doom marble carried by The Doom.$" locate_doom_shard

    ;locates, hero mana
    /def -i -PCred -mregexp -t"^The eye of the beholder carried by The beholder mother.$" locate_eye_beholder
    /def -i -PCred -mregexp -t"^A random ring carried by the Crawling Chaos.$" locate_random_ring
    /def -i -PCred -mregexp -t"^a turquoise mane in Daido's tomb..$" locate_turquoise_mane
    /def -i -PCred -mregexp -t"^A grand serpent hood carried by King Ka'plar.$" locate_grand_serpent_hood
    /def -i -PCred -mregexp -t"^Waterlace leggings carried by the giant water elemental.$" locate_waterlace_leggings
    /def -i -PCred -mregexp -t"^Silver astral gauntlets carried by Ophanya.$" locate_silver_astral_gauntlets
    /def -i -PCred -mregexp -t"^Black bracer carried by a githzerai swordsman.$" locate_black_bracer
    /def -i -PCred -mregexp -t"^The Swirl of Fumes carried by a swirl of fragrant fumes.$" locate_swirl_fumes
    /def -i -PCred -mregexp -t"^The cauldron of souls carried by the Crawling Chaos.$" locate_cauldron_souls
    /def -i -PCred -mregexp -t"^Unholy robe carried by High Priest of Hell.$" locate_unholy_robe
    /def -i -PCred -mregexp -t"^Belt of sorcery carried by Zur'lithi.$" locate_belt_sorcery
    /def -i -PCred -mregexp -t"^a sprite ribcage carried by Millament the mad Necromancer.$" locate_sprite_ribcage
    /def -i -PCred -mregexp -t"^The Gnarled Branch of Age carried by heavy knotted branches.$" locate_branch_age
    /def -i -PCred -mregexp -t"^A mystical desert stone carried by Arcanthra the Black.$" locate_mystical_desert_stone
    /def -i -PCred -mregexp -t"^The Sceptre of Solomon in The Hidden Shrine of Solomon.$" locate_sceptre_solomon

    ;locates, gear with affects
    /def -i -PCred -mregexp -t"^A feathered pair of boots carried by an ice dragon.$" locate_feather_boots
    /def -i -PCred -mregexp -t"^The Ring of the Rat in Undrehand's office.$" locate_rat_ring
    /def -i -PCred -mregexp -t"^Skinning gloves carried by the chief hunter.$" locate_skinning_gloves
    /def -i -PCred -mregexp -t"^Griffon wings bracer carried by A resting hobgoblin.$" locate_griffon_bracer


    ;locates, consumables
    /def -i -PCred -mregexp -t"^A white marble cross carried by The guardian of the white temple.$" locate_white_cross
    /def -i -PCred -mregexp -t"^A flask of holy water carried by the grand templar.$" locate_flask_water
    /def -i -PCred -mregexp -t"^Cypress sap carried by a taproot.$" locate_cypress_sap
    /def -i -PCred -mregexp -t"^Orange potion carried by a fire lizard.$" locate_orange_potion
    /def -i -PCred -mregexp -t"^A crumpled scroll in At the altar.$" locate_crumpled_scroll
    /def -i -PCred -mregexp -t"^An elixir of crimson blood in The Bottom of the Crimson Blood.$" locate_el_cr_bl
    /def -i -PCred -mregexp -t"^Gold Dragon Orb carried by The Ancient Gold Dragon.$" locate_gold_orb
    /def -i -PCred -mregexp -t"^The Black Staff of Typhus carried by The demon lord Typhus.$" locate_typhus_staff

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
