This script changes the format and color of the list you see when you
type "group". See the comments in the script for more information.

Also included is a substitute for "groupstat" to include percentages for
hp and mana. It strips mv.

    ;Colors
    ;  hp and mana are yellow if they're below 50%, red if they're below 20%.
    ;  tnl is yellow if it's below 250, red if it it's below 100.
    ;  align is yellow if it's between -450 and 450, red if it's between -200 and
    ;    200.

    ;The original group list has this green, cyan, etc pattern.  With no default
    ;  color set it'll keep as much from that pattern as it can, which is nice by
    ;  itself, but might be confusing if the group is hurt and you get red, cyan,
    ;  green, yellow, green, cyan.  Setting a default would change that to red,
    ;  <default>, <default>, yellow, <default>, <default>.
    ;So if gls_default_color is an empty string it will keep the colors it got from
    ;  avatar.  If it's set to a value that value will be used as default color.
    ;/set gls_default_color=
    /set gls_default_color=cyan

    ;I also made the columns align differently, so the hp/hp_max and mana/mana_max
    ;  aren't just left-aligned, but the '/' is at a fixed position.  I like that
    ;  much better.
    ;
    ;Original:
    ; 1| 24 Mag  Darkfader    Stand 213/213     538/538     268/268     1296   1000
    ; 2|973 Hero Enchantress  Stand 1765/1765   6039/6110   4114/5216   819    1000
    ; 3|  6 Lord Kimla        Stand 2086/2086   1095/4686   4283/4283   14458  1000
    ;
    ;With this substitute:
    ; 1| 24 Mag  Darkfader    Stand   213/213     538/538     268/268   1296   1000.
    ; 2|973 Hero Enchantress  Stand  1765/1765   6037/6110   4114/5216  819    1000.
    ; 3|  6 Lord Kimla        Stand  2086/2086   1020/4686   4283/4283  14458  1000.

    ;It appends a dot to each line so you can easily see the trigger is working.

    /def -i -mregexp -t"^ *([0-9]+)[^ ] *([0-9]+) *([a-zA-Z]+) *([a-zA-Z]+) *([a-zA-Z]+) *(-?[0-9]+)/([0-9]+) *(-?[0-9]+)/([0-9]+) *(-?[0-9]+)/([0-9]+) *([0-9]+) *(-?[0-9]+)$" group_list_substitute = \
            /let _id %{P1}%;\
            /let _level %{P2}%;\
            /let _tier %{P3}%;\
            /let _name %{P4}%;\
            /let _pos %{P5}%;\
            /let _hp %{P6}%;\
            /let _hp_max %{P7}%;\
            /let _mana %{P8}%;\
            /let _mana_max %{P9}%;\
            /let _mv %{P10}%;\
            /let _mv_max %{P11}%;\
            /let _tnl %{P12}%;\
            /let _align %{P13}%;\
    ;color hp
            /if (_hp_max & _hp*100/_hp_max <= 20) \
                    /test (_hp:=decode_attr(strip_attr(_hp), "Cred"), _hp_max:=decode_attr(strip_attr(_hp_max), "Cred"))%;\
            /elseif (_hp_max & _hp*100/_hp_max <= 50) \
                    /test (_hp:=decode_attr(strip_attr(_hp), "Cyellow"), _hp_max:=decode_attr(strip_attr(_hp_max), "Cyellow"))%;\
            /elseif (gls_default_color !~ "") \
                    /test (_hp:=decode_attr(strip_attr(_hp), "C%{gls_default_color}"), _hp_max:=decode_attr(strip_attr(_hp_max), "C%{gls_default_color}"))%;\
            /endif%;\
    ;color mana
            /if (_mana_max & _mana*100/_mana_max <= 20) \
                    /test (_mana:=decode_attr(strip_attr(_mana), "Cred"), _mana_max:=decode_attr(strip_attr(_mana_max), "Cred"))%;\
            /elseif (_mana_max & _mana*100/_mana_max <= 50) \
                    /test (_mana:=decode_attr(strip_attr(_mana), "Cyellow"), _mana_max:=decode_attr(strip_attr(_mana_max), "Cyellow"))%;\
            /elseif (gls_default_color !~ "") \
                    /test (_mana:=decode_attr(strip_attr(_mana), "C%{gls_default_color}"), _mana_max:=decode_attr(strip_attr(_mana_max), "C%{gls_default_color}"))%;\
            /endif%;\
    ;color move if there's a default
            /if (gls_default_color !~ "") \
                    /test (_mv:=decode_attr(strip_attr(_mv), "C%{gls_default_color}"), _mv_max:=decode_attr(strip_attr(_mv_max), "C%{gls_default_color}"))%;\
            /endif%;\
    ;color tnl
            /if (_tnl <= 100) \
                    /test _tnl:=decode_attr(strip_attr(_tnl), "Cred")%;\
            /elseif (_tnl <= 250) \
                    /test _tnl:=decode_attr(strip_attr(_tnl), "Cyellow")%;\
            /elseif (gls_default_color !~ "") \
                    /test _tnl:=decode_attr(strip_attr(_tnl), "C%{gls_default_color}")%;\
            /endif%;\
    ;color align
            /if ((_align >= -200) & (_align <= 200)) \
                    /test _align:=decode_attr(strip_attr(_align), "Cred")%;\
            /elseif ((_align >= -450) & (_align <= 450)) \
                    /test _align:=decode_attr(strip_attr(_align), "Cyellow")%;\
            /elseif (gls_default_color !~ "") \
                    /test _align:=decode_attr(strip_attr(_align), "C%{gls_default_color}")%;\
            /endif%;\
    ;substitute the mud output and do some padding
            /substitute -aCcyan -p @{n}$[pad(_id, 2, "|", 1, _level, 3, " ", 1, _tier, -5, \
                    _name, -13, _pos, -6, _hp, 5, "/", 1, _hp_max, -6, _mana, 5, "/", 1, \
                    _mana_max, -6, _mv, 5, "/", 1, _mv_max, -6, _tnl, -6, _align, 5)].



    /def -i -mregexp -t"^([a-zA-Z]+) is leading ([0-9]+) player(s?) with ([0-9]+)/([0-9]+) hp, ([0-9]+)/([0-9]+) ma, and [0-9]+/[0-9]+ mv.$" groupstat_substitute = \
            /let _name %{P1}%;\
            /let _players %{P2}%;\
            /let _hp %{P4}%;\
            /let _hp_max %{P5}%;\
            /let _hp_rel=-%;\
            /test _hp_max & (_hp_rel:=_hp*100/_hp_max)%;\
            /let _mana %{P6}%;\
            /let _mana_max %{P7}%;\
            /let _mana_rel=-%;\
            /test _mana_max & (_mana_rel:=_mana*100/_mana_max)%;\
    ;color hp
            /if (_hp_max & _hp*100/_hp_max <= 20) \
                    /test (_hp:=decode_attr(strip_attr(_hp), "Cred"), \
                            _hp_max:=decode_attr(strip_attr(_hp_max), "Cred"), \
                            _hp_rel:=decode_attr(strip_attr(_hp_rel), "Cred"))%;\
            /elseif (_hp_max & _hp*100/_hp_max <= 50) \
                    /test (_hp:=decode_attr(strip_attr(_hp), "Cyellow"), \
                            _hp_max:=decode_attr(strip_attr(_hp_max), "Cyellow"), \
                            _hp_rel:=decode_attr(strip_attr(_hp_rel), "Cyellow"))%;\
            /endif%;\
    ;color mana
            /if (_mana_max & _mana*100/_mana_max <= 20) \
                    /test (_mana:=decode_attr(strip_attr(_mana), "Cred"), \
                            _mana_max:=decode_attr(strip_attr(_mana_max), "Cred"), \
                            _mana_rel:=decode_attr(strip_attr(_mana_rel), "Cred"))%;\
            /elseif (_mana_max & _mana*100/_mana_max <= 50) \
                    /test (_mana:=decode_attr(strip_attr(_mana), "Cyellow"), \
                            _mana_max:=decode_attr(strip_attr(_mana_max), "Cyellow"), \
                            _mana_rel:=decode_attr(strip_attr(_mana_rel), "Cyellow"))%;\
            /endif%;\
            /substitute -aCgreen %_name is leading %_players player%{P3} with %_hp/%_hp_max(%_hp_rel%%) hp, and %_mana/%_mana_max(%_mana_rel%%) mana.

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
