Non issue: The script won't revert 3w200s, max supported is 99.

    /echo -aCyellow %% /dirrev <path>        Revert speedwalk path

    /def -i dirrev = \
            /if ({#}=0) \
                    /echo -p %%% @{Cred}Syntax: /%{0} <path>@{n}%;\
            /else \
                    /let _dir_str %1 %;\
                    /let _length $[strlen(_dir_str)] %;\
                    /let _double_digit=0%;\
                    /set dirrev=%;\
                    /for c 0 $[_length-1] \
                            /let _position $$[_length-c] %%;\
                            /let _cur_char $$[substr(_dir_str, _position-1, 1)] %%;\
                            /if (_cur_char =~ "e") /let _new_char w %%;\
                            /elseif (_cur_char =~ "s") /let _new_char n %%;\
                            /elseif (_cur_char =~ "w") /let _new_char e %%;\
                            /elseif (_cur_char =~ "n") /let _new_char s %%;\
                            /elseif (_cur_char =~ "u") /let _new_char d %%;\
                            /elseif (_cur_char =~ "d") /let _new_char u %%;\
                            /else  /let _new_char %%{_cur_char} %%;\
                            /endif %%;\
                            /if ((_new_char =/ "[0-9]") & (!_double_digit)) \
                                    /set dirrev $$[strcat(substr(dirrev, 0, c-1), _new_char, substr(dirrev, c-1, 1))] %%;\
                                    /if (substr(_dir_str, _position-2, 1) =/ "[0-9]") \
                                            /set dirrev $$[strcat(substr(dirrev, 0, c-1), substr(_dir_str, _position-2, 1), substr(dirrev, c-1, 2))] %%;\
                                            /let _double_digit 1 %%;\
                                    /endif %%;\
                            /elseif ( regmatch("[nsewud]", _new_char) ) \
                                    /set dirrev $$[strcat(dirrev, _new_char)] %%;\
                                    /let _double_digit 0 %%;\
                            /endif %;\
                    /echo -p %%% @{Cwhite}The reverse of @{Cgreen}%{_dir_str} @{Cwhite}is @{Cgreen}%dirrev@{n}%;\
                    /return "%dirrev"%;\
            /endif

[Category: TinyFugue
Scripting](Category:_TinyFugue_Scripting "wikilink")
