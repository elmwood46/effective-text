# effective-text
A Text parser/markup language for animated text, supporting different colours, fonts, and sounds, written for use with GMStudio.

The main purpose of this parser is to easily allow the printing of aesthetic text. 
For example, the changing of colour of parts of the text, animating, and adding linebreaks and pauses (for when text is printed with a 'Typewriter' animation, character by character.)

The text renderer serves as the basis for more complex systems commonly
found in games.
In this repo, there's some example stuff which demonstrates
a dialogue tree, similar to old DS games. You can make choices to
navigate the text, and display an animated "speaker" portrait which
moves its mouth while the text is printing, etc.

The object oPrinter prints a text message with applied "effects".
The message is automatically linebroken into an arbitrary box and 
can be placed anywhere.
The message is printed character by character, typewriter-style.
Furthermore, multiple lines don't run outside the box.
Pauses only work with the oPrinter object (although if included
outside it, are simply ignored.)

Add effects by including EFFECT TAGS inside your string literals.

You can create your own effect tags pretty easily.
E.g. implementing rainbow text could be accomplished by
adding simple code in parse_command (to recognize the command)
and draw_char_eff (to animate the colours).

oPrinter is controlled by changing its "state" variable
to recognized strings. See oTextbox for example. 

EFFECT TAGS:
Effects are incuded inline like this: "my text <c:red>this is red</c>!"
Similar to HTML tags

      COLOUR:
           <c:ColourName> where ColourName is a gml colour code
           <c:0x000000>   where you put a 6 digit b/g/r hex string

      WAVE:
	          <w>     makes text wave with default amplitude and frequency
	          <w:x>   wave & changes the amplitude
	          <w:x:y> wave & sets the amplitude = x and frequency = y

      TWITCH:
            <t>     makes text twitch every so often
            <t:x>   twitch & sets the frequency of twitching [0, 1]
 
      SHAKE:
            <s>     makes text shake
            <s:x>   makes text shake and sets amplitude of shake

      PAUSE:
            <p>     makes text typer pause for DEFAULT_PAUSE_DURATION frames
            <p:x>   pauses for x frames

      LINEBREAK:
            <br>    I won't spoil what this does

      SET SPEAKER:
            <i:speakerName> 
                    This sets the portrait of the speaker. Only works in oTextbox. Otherwise ignored.
            <e:speakerEmote>
                    Changes the emote of the speaker. The emote must exist in the speaker's emote tree.

      COMMAND CHARS:
            <gt>    ">"
            <lt>    "<"
            <q>     renders as double quote "
             `      Grave symbol renders as apostrophe

      CANCEL EFFECTS:
            </c>    resets to default colour (set in create_effect_map script)
            </fx>   stops wave, shake and twitch and resets parameters to default
            </w>    stops wave and resets wave vars
            </s>    same but for shake
            </t>    same but for twitch
