## Color markup

This is a simple little markup for colors using python. It's useful in
python scripts that accept input that is colored from the client.

The format of the string sent to the python script should be: "@GThis is
Bold green@d, @yand some bright Yellow@d!"

fairly self explanatory.


    import re

    COLOR_REGEXP = re.compile('\@[a-zA-Z]')

    STYLE_MAP = {
                 "d": "0",
                 "s": "1",
                 "u": "4",
                 "r": "0;31",
                 "g": "0;32",
                 "y": "0;33",
                 "b": "0;34",
                 "m": "0;35",
                 "c": "0;36",
                 "w": "0;37",
                 "R": "1;31",
                 "G": "1;32",
                 "Y": "1;33",
                 "B": "1;34",
                 "M": "1;35",
                 "C": "1;36",
                 "W": "1;37"
               }


    def get_ansi_code(code):
        if STYLE_MAP.has_key(code):
            return chr(27) + '[' + STYLE_MAP[code] + 'm'
        else:
            return '**bad code: ' + code + '**'

    def add_color_codes(text):
        regobj = COLOR_REGEXP.search(text)
        marker = 0
        while regobj:
            (b, e) = regobj.span()
            torep = text[b:e]
            c = torep[-1]
            text = text.replace(torep, get_ansi_code(c))
            regobj = COLOR_REGEXP.search(text)
        return text

[Category: Scripting](Category:_Scripting "wikilink")
