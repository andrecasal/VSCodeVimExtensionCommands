# Vim Mental Model

1. Normal Mode
	1. Editor Group Navigation
	2. Tab Navigation
	3. Document Navigation
		1. Folding
	4. Moving the cursor
		1. Left-right motions
		2. Up-down motions
		3. Various motions
		4. Text object motions
		5. Marks and jumps
	5. Searches
		1. Regex
		2. Offset
	6. Operations: verbs + modifiers + noun
		1. Operation: verbs
			1. Yank (copy) and put (paste)
			2. Delete
			3. Change (delete and insert)
				1. Complex changes
		2. Modifiers and nouns
	7. Repeat finds, searches and operations
	8. Undo and redo
	9. Record and execute
	10. External Commands
	11. Custom Commands
	12. Options
		1. Option values
	13. Command-line Mode Ranges
2. Insert Mode
	1. Moving in insert mode
	2. Commands in insert mode
	3. Special Inserts
	4. Digraphs
3. Visual Mode
	1. Moving to Visual Mode
	2. Commands in Visual Mode
4. Replace Mode



# Normal Mode
Most of the time you will be in normal mode. It's in this mode you navigate your document, move your cursor and do all operations except inserting text for the obvious reason that you need your keystrokes to be written to the document instead of working as commands to Vim.

Esc							change to normal mode

## Editor Group Navigation
:e[dit] {file}				open {file} in a new tab of current grouped editor
:new						create a new window horizontally with new file
:vne[w]						create a new window vertically with new file
Ctrl+w h					switch to the group editor on the left
Ctrl+w l					switch to the group editor on the right
:sp[lit] {file}				split window
Ctrl+w s					split window
:vsp[lit] {file}			split window vertically
Ctrl+w v					split window vertically
Ctrl+w o					close other editor groups

## Tab Navigation
{count}gt					go to next tab page or tab page {count}. The first tab page has number one
:tabn[ext] 🔢				go to next tab page or tab page {count}. The first tab page has number one
{count}<C-PageDown>			go to next tab page or tab page {count}. The first tab page has number one
{count}gT					go to the previous tab page. Wraps around from the first one to the last one
:tabp[revious] 🔢			go to the previous tab page. Wraps around from the first one to the last one
:tabN[ext] 🔢				go to the previous tab page. Wraps around from the first one to the last one
{count}<C-PageUp>			go to the previous tab page. Wraps around from the first one to the last one
:tabfir[st]					go to the first tab page.
:tabl[ast]					go to the last tab page.
:tabe[dit] {file}			open a new tab page with an empty window, after the current tab page
:[count]tabe[dit]			same as above	[count] is not supported
:[count]tabnew				same as above	[count] is not supported
:tabnew {file}				open a new tab page with an empty window, after the current tab page
:[count]tab {cmd}			execute {cmd} and when it opens a new window open a new tab page instead.
:tabc[lose][!] 🔢			close current tab page or close tab page {count}.	Code will close tab directly without saving.
:tabo[nly][!]				close all other tab pages.	! is not supported, Code will close tab directly without saving.
:tabm[ove][n]				move the current tab page to after tab page N.
:tabs						list the tab pages and the windows they contain. You can always use Code's built-in shortcut: cmd/ctrl+p
:tabd[o] {cmd}				execute {cmd} in each tab page.

## Document navigation
Ctrl
	e						window N lines downward
	d						window N lines downward (default is 1/2 window)
	f						window N pages downward
	y						window N lines upward
	u						window N lines upward (default is 1/2 window)
	b						window N pages upward
z							redraw current line
	zt						at top of window
	zz or z.				at center of window
	zb or z-				at bottom of window
	zh						scroll screen N characters to the right (only works when word wrap is off) (in VS Code, the cursor will always move when you run this command, whether the horizontal scrollbar moves or not)
	zl						scroll screen N characters to the left (only works when word wrap is off) (in VS Code, the cursor will always move when you run this command, whether the horizontal scrollbar moves or not)
	zH						half a screenwidth to the right (in VS Code, the cursor will always move when you run this command, whether the horizontal scrollbar moves or not)
	zL						half a screenwidth to the left (in VS Code, the cursor will always move when you run this command, whether the horizontal scrollbar moves or not)

### Folding
zf{motion}					Operator to create a fold
zF							create a fold for [count] lines
zd							delete one fold under the cursor
zD							delete all folds under the cursor
zE							eliminate all folds in the window
zo							open one fold under the cursor
zO							open all folds under the cursor
zc							close one fold under the cursor
zC							close all folds under the cursor
za							When on a closed fold: open it. When on an open fold: close it and set 'foldenable'
zA							when on a closed fold: open it recursively. When on an open fold: close it recursively and set 'foldenable'
zv							when on a closed fold: open it. When on an open fold: close it and set 'foldenable'
zx							update folds: Undo manually opened and closed folds: re-apply 'foldlevel', then do "zv": View cursor line
zX							undo manually opened and closed folds
zm							fold more: decrease 'foldlevel'
zM							close all folds: make 'foldlevel' zero
zr							reduce folding: increase 'foldlevel'
zR							open all folds: make 'foldlevel' max
zn							fold none: reset 'foldenable'
zN							fold normal: set 'foldenable'
zi							invert 'foldenable'
[z							move to the start of the current open fold
]z							move to the end of the current open fold
zj							move downward to the start of the next fold
zk							move upward to the end of the previous fold

## Moving the cursor

### Left-right motions
h							left N characters
Left key					left N characters
l							right N characters
Right key					right N characters
0							to first character in the line
^							to first non-blank character in the line
$							to the last character in the line N-1 lines lower
g0							to first character in screen line (differs from "0" when lines wrap)
g^							to first non-blank character in screen line (differs from "^" when lines wrap)
g$							to last character in screen line (differs from "$" when lines wrap)
gm							to middle of screen line
|							to column N (default: 1)
f{char}						to the Nth occurence of {char} to the right
F{char}						to the Nth occurence of {char} to the left
t{char}						till before the Nth occurrence of {char} to the right
T{char}						till before the Nth occurrence of {char} to the left

### Up-down motions
k							up N lines
Up key						up N lines
j							down N lines
Down key					down N lines
-							up N lines, on the first non-blank character
+							down N lines, on the first non-blank character
_							down N-1 lines, on the first non-blank character
gg							go to line N (default: first line), on the first non-blank character
G							go to line N (default: last line), on the first non-blank character
%							go to line N percentage down in the file. N must be given, otherwise it's the find next brace, bracket, comment, "#if", "#else", "#endif" in this line and go to its match
gk							up N screen lines (differs from "k" when line wraps)
gj							down N screen lines (differs from "j" when line wraps)

### Various motions
H							go to Nth line in the window, on the first non-blank character
M							go to the middle of the line in the window, on the first non-blank character
L							go to the Nth line from the bottom, on the first non-blank character
go							go to Nth byte in the buffer
:[range]go[to] [offset]		go to [offset] byte in the buffer

### Text object motions
w							N words forward
W							N blank-separated words forward
e							N words forward to the end of the Nth word
E							N words forward to the end of the Nth blank-separated word
b							N words backwards
B							N blank-separated words backwards
ge							N words backward to the end of the Nth word
gE							N words backward to the end of the Nth blank-separated word
)							N senteces forward
(							N senteces backward
}							N paragraphs forward
{							N paragraphs backward
]]							N sections forward, at start of section
[[							N sections backward, at start of section
][							N sections forward, at end of section
[]							N sections backward, at end of section
[(							N times back to unclosed '('
[{							N times back to unclosed '{'
[m							N times back to start of method (for Java)
[M							N times back to end of method (for Java)
])							N times forward to unclosed ')'
]}							N times forward to unclosed '}'
]m							N times forward to start of method (for Java)
]M							N times forward to end of method (for Java)
[#							N times back to unclosed "#if" or "#else"
]#							N times forward to unclosed "#else" or "#endif"
[*							N times back to start of a C comment "/*"
]*							N times forward to end of a C comment "*/"

### Marks and jumps
m							mark position
	m{a-z}					current file with mark {a-z}
	m{A-Z}					globally (for all files) with mark {A-Z}
`							go to
	`{a-z}					mark in current file
	`{A-Z}					mark in any file
	`{0-9}					where Vim was previously exited
	``						position before last jump
	`"						position when last editing this file
	`[						start of the previously operated text
	`]						end of previously operated text
	`<						start of previous visual area
	`>						end of previous visual area
	`.						position of the last change in this file
'							go to, but on the first non-blank character
	'.						position of the last change in this file, on the first non-blank character
	'{a-z}					mark in current file, on the first non-blank character
	'{A-Z}					mark in any file, on the first non-blank character
	'{0-9}					where Vim was previously exited, on the first non-blank character
	''						position before last jump, on the first non-blank character
	'<						start of previous visual area, on the first non-blank character
	'>						end of previous visual area, on the first non-blank character
	']						end of previously operated text
	'[						start of previously operated text
Ctrl+o						go to Nth previous position in jump list
Ctrl+i						go to Nth next position in jump list
:ju[mps]					print the jump list

## Searches
/{pattern}/[offset]			search forward for the Nth occurrence of {pattern} and place the cursor at [offset] from that
?{pattern}/[offset]			search backward for the Nth occurrence of {pattern} and place the cursor at [offset] from that
/Enter						repeat last search (count is not supported)
?Enter						repeat last search, in the backward direction (count is not supported)
n							repeat last search
N							repeat last search, in the opposite direction
*							search forward for the identifier under the cursor
\#							search backward for the identifier under the cursor
g*							search forward for the identifier under the cursor, but also find partial matches
g#							search backward for the identifier under the cursor, but also find partial matches
gd							go to local declaration of identifier under the cursor
gD							go to global declaration of identifier under the cursor

### Regex
The usual JS regex way. I need to confirm that vim patterns work just like regular expressions.

### Offsets
[num]						[num] lines downwards, in column 1
+[num]						[num] lines downwards, in column 1
-[num]						[num] lines upwards, in column 1
e[+num]						[num] characters to the right of the end of the match
e[-num]						[num] characters to the left of the end of the match
s[+num]						[num] characters to the right of the start of the match
s[-num]						[num] characters to the left of the start of the match
b[+num]						[num] identical to s[+num] above (mnemonic: begin)
b[-num]						[num] identical to s[-num] above (mnemonic: begin)
;{search-command}			execute {search-command} next

## Operations: verb + modifier + noun
You can do 4 operations on text: delete, yank (copy), change (delete and insert) and insert. Change will do the delete operation and automatically get you into Insert Mode. Insert Mode is a special mode where, for obvious reasons, keys stop working as commands and insert text in the document instead. We do still have access to some commands in insert mode though. We'll cover them all in the Insert Mode section ahead.

Operations become extremelly simple if you think about them as a spoken language: verb + modifier + noun. Delete around word would be daw. Yank a sentence would be yas. And so forth. The Verb + Modifier + Noun mental model works extremelly well here.

### Operations as verbs

#### Yank (copy) and put (paste)
"{char}						use register {char} for the next delete, yank or put
"*							use register * to access system clipboard
:reg						show the contents of all registers
:reg {arg}					show the contents of registers mentioned in {arg}
y							yank (copy)
	y{motion}				yank (copy) the text moved over with {motion} into a register
	yy						yank (copy) N lines into a register
	Y						yank N lines into a register
p							put a register after the cursor N times
	P						put a register before the cursor N times
	]p						put a register after the cursor N times and adjust indent to current line
	[p						put a register before the cursor N times and adjust indent to current line
	gp						put a register after the cursor N times and leave cursor after the next text
	gP						put a register before the cursor N times and leave cursor after the next text

#### Delete
x							delete N characters under and after the cursor
DEL							delete N characters under and after the cursor
X							delete N characters before the cursor
d							delete
	dd						delete N lines
	d{motion}				delete the text that is moved over with {motion}
	D						delete to the end of the line (and N-1 more lines)
J							join N-1 lines (delete EOLs)
	gJ						join N-1 lines (delete EOLs) without inserting spaces
:[range]d [x]				delete range lines into register x

#### Change (delete and insert)
The change operation is essentially a delete operation which also changes you to Insert Mode automatically.

r{char}						replace N characters with {char}
	R						enter Replace Mode
gr{char}					replace N characters without affecting layout
	gR						enter virtual Replace Mode: like Replace Mode but without affecting layout
c{motion}					change the text that is moved over with {motion}
	cc						change N lines
	C						change to the end of the line (and N-1 more lines)
s							change N characters
	S						change N lines
	~						switch case for N characters and advance cursor
g~{motion}					switch case for the text that is moved over with {motion}
	gu{motion}				make the text that is moved over with {motion} lowercase
	gU{motion}				make the text that is moved over with {motion} uppercase
	g?{motion}				perform rot13 enconding on the text that is moved over with {motion}
	Ctrl+a					add N to the number at or after the cursor
	Ctrl+x					subtract N from the number at or after the cursor
	<{motion}				move the lines that are moved over with {motion} one shiftwidth left
	<<						move N lines one shiftwidth left
	>{motion}				move the lines that are moved over with {motion} one shiftwidth right
	>>						move N lines one shiftwidth right
	gq{motion}				format the lines that are moved over with {motion} to 'textwidth' length
:[range]ce[nter] [width]	center the lines in [range]
:[range]le[ft] [indent]		left-align the lines in [range] (with [indent])
:[range]ri[ght] [width]		right-align the lines in [range]

##### Complex changes
!{motion}{command}<CR>		filter the lines that are moved over through {command}
!!{command}<CR>				filter N lines through {command}
:[range]! {command}<CR>		filter [range] lines through {command}
={motion}					filter the lines that are moved over through 'equalprg'
==							filter N lines through 'equalprg'
:[range]s[ubstitute]/{pattern}/{string}/[g] [c]
							substitute {pattern} by {string} in [range] lines; with [g], replace all occurrences of {pattern}; with [c], confirm each replacement
:[range]s[ubstitute][g][c]
							repeat previous ":s" with new range and options
&							repeat previous ":s" on current line without options
:[range]ret[ab][!] [tabstop]
							set 'tabstop' to new value and adjust white space accordingly

### Modifiers and nouns
Modifiers change what some cursor movement commands mean. There are only two modifiers: a (around) and i (inner or inside).

aw							Select "a word"
aW							Select "a WORD"
as							Select "a sentence"
ap							Select "a paragraph"
ab							Select "a block" (from "[(" to "])")
aB							Select "a Block" (from "[{" to "]}")
a>							Select "a <> block"
at							Select "a tag block" (from <aaa> to </aaa>)
a'							Select "a single quoted string"
a"							Select "a double quoted string"
a`							Select "a backward quoted string"
iw							Select "inner word"
iW							Select "inner WORD"
is							Select "inner sentence"
ip							Select "inner paragraph"
ib							Select "inner block" (from "[(" to "])")
iB							Select "inner Block" (from "[{" to "]}")
i>							Select "inner <> block"
it							Select "inner tag block" (from <aaa> to </aaa>)
i'							Select "inner single quoted string"
i"							Select "inner double quoted string"
i`							Select "inner backward quoted string"

## Repeat finds, searches and operations
;							repeat the last "f", "F", "t" or "T" N times
,							repeat the last "f", "F", "t" or "T" N times in the opposite direction
.							repeat last operation N times

## Undo and Redo
u							undo last N changes
	U						restore last changed line
Ctrl+r						redo last N undone changes

## Record and execute
q							toggle recording
	q{a-Z}					record typed characters into register {a-z}
	q{A-Z}					record typed characters, appended to register {a-z}
@							execute recording
	@{a-z}					execute the contents of register {a-z} N times
	@@						repeat the previous @{a-z} N times
:							Command-line Mode's Ex commands
	:@{a-z}					execute the contents of register {a-z} as an Ex command
	:@@						repeat previous :@{a-z}
	:[range]g[lobal]/{pattern}/[cmd]
							execute Ex command [cmd] (default: ":p") on the lines within [range] where {pattern} matches
	:[range]g[lobal]!/{pattern}/[cmd]
							execute Ex command [cmd] (default: ":p") on the lines within [range] where {pattern} does not matche
:so[urce] {file}			read Ex commands from {file}
:so[urce]! {file}			read Vim commands from {file}
:sl[eep] [sec]				sleep for [sec] seconds
gs							go to sleep for N second

## External Commands
:sh[ell]					start a shell
:!{command}					execute {command} with a shell
K							lookup keyword under the cursor with 'keywordprg' program (default: "man")

## Custom Commands
gh							show the hover tooltip
gb							add an additional cursor at the next place that matches *

## Options
:se[t]						show all modified options
:se[t] all					show all non-termcap options
:se[t] termcap				show all termcap options
:se[t] {option}				set boolean option (switch it on), show string or number option
:se[t] no{option}			reset boolean option (switch it off)
:se[t] inv{option}			invert boolean option
:se[t] {option}={value}		set string/number option to {value}
:se[t] {option}+={value}	append {value} to string option, add {value} to number option
:se[t] {option}-={value}	remove {value} to string option, subtract {value} from number option
:se[t] {option}?			show value of {option}
:se[t] {option}&			reset {option} to its default value
:setl[ocal]					like ":set" but set the local value for options that have one
:setg[lobal]				like ":set" but set the global value of a local option
:fix[del]					set value of 't_kD' according to value of 't_kb'
:opt[ions]					open a new window to view and set options, grouped by functionality, a one line explanation and links to the help

### Option values
tabstop (ts)				4. we use Code's default value tabSize instead of Vim
							Number of spaces that <Tab> in file uses
hlsearch (hls)				false
							When there is a previous search pattern, highlight all its matches.
ignorecase (ic)				true
							Ignore case in search patterns.
smartcase (scs)				true
							Override the 'ignorecase' option if the search pattern contains upper case characters.
iskeyword (isk)				@,48-57,_,128-167,224-235
							Keywords contain alphanumeric characters and '_'. If there is no user setting for iskeyword, we use editor.wordSeparators properties.
scroll (scr)				20
							Number of lines to scroll with CTRL-U and CTRL-D commands.
expandtab (et)				True. We use Code's default value insertSpaces instead of Vim
							Use spaces when <Tab> is inserted
autoindent					true
							Keep indentation when doing cc or S in normal mode to replace a line.

## Command-line Mode Ranges
Command-line mode is used to enter Ex commands (":"), search patterns ("/" and "?"), and filter commands ("!").

### Ranges
,							separates two line numbers
;							separates two line numbers, set cursor to the first line number before interpreting the second one. The cursor movement is not included.
{number}					an absolute line number
.							the current line
$							the last line in the file
%							equal to 1,$ (the entire file)
*							equal to '<,'> (visual area)
't							position of mark t
️/{pattern}[/]				the next line where {pattern} matches
?{pattern}[?]				the previous line where {pattern} matches
+[num]						add [num] to the preceding line number (default: 1)
-[num]						subtract [num] from the preceding line number (default: 1)

# Insert Mode
Insert Mode is the mode in which you insert text into the document. For this reason the keys can no longer function as commands to Vim as they will insert text into the document instead. We do still have a few commands we can use by pressing keys (like delete) or chords (like Ctrl+v) which send commands to Vim.

But first we need to discuss which commands we can issue that will change us into Insert Mode.

## Operations that will change us to Insert Mode
a							append text after the cursor N times
	A						append text at the end of the line N times
o							open a new line below the current line and append text N times
	O						open a new line above the current line and append text N times
i							insert text before the cursor N times
	I						insert text before the first non-blank in the line N times
gi							to end of last change and insert
	gI						to column 1 and insert text N times (change mode to insert)

## Commands in Insert Mode
### Moving in insert mode
Arrow keys					move cursor left, right, up and down
Alt+left arrow key			move word left
Alt+right arrow key			move word right
Alt+up arrow key			move line up
Alt+down arrow key			move line down
Home						move cursor to first character in line
End							move cursor to last character in line

### Commands in insert mode
Ctrl+o {command}			execute {command} and return to insert mode
Ctrl+v {char}				insert character literally or enter decimal byte value
Ctrl+e						insert the character below the cursor
Ctrl+y						insert the character above the cursor
Ctrl+a						insert the previously inserted character
Ctrl+@						insert the previously inserted text and stop Insert Mode
Ctrl+r {register}			insert the contents of a register
Ctrl+n						insert next match of identifier before the cursor
Ctrl+p						insert previous match of identifier before the cursor
Ctrl+x						complete the word before the cursor in various ways
Ctrl+h						delete the character before the cursor
Delete						delete the character under the cursor
Ctrl+w						delete word before the cursor
Ctrl+u						delete all entered characters in the current line
Ctrl+t						insert one shiftwidth of indent in front of the current line
Ctrl+d						delete one shiftwidth of indent in front of the current line
0+Ctrl+d					delete all indent in the current line
^+Ctrl+d					delete all indent in the current line and restore indent in the next line

### Special Inserts
:r [file]					insert the contents of [file] below the cursor
:r! {command}				insert the standard output of {command} below the cursor

### Digraphs
Ctrl+k+{key}+{key}			Insert digraph (digraphs are mostly used to enter printable non-ASCII characters like ½ or ω)
:dig[raphs]					show current list of digraphs
:dig[raphs] {char1}{char2} {number}
							add digraphs(s) to the list

# Visual Mode
Visual mode works just like normal mode but you first move the cursor and only then choose the operation you want on the highlighted text. So while in normal mode the mental model is verb + modifier + noun, in visual mode the mental model is modifier + noun + verb. A good reason to use Visual Mode is to test what text your commands are selecting before applying a command.

All normal mode commands that make sense in visual mode work in visual mode.

## Moving to Visual Mode
v							toggle visual mode
	V						toggle visual mode linewise
	Ctrl+v					toggle to visual mode blockwise
o							exchange cursor position with start of highlighting, i.e. toggle cursor position between start and end of highlighting

## Commands in Visual Mode
I							insert the same text in front of all the selected lines
A							append the same text after all the selected lines
gv							start highlighting on previous visual area

## Visual: Folding
{visual}zf				operator to create a fold
## Visual: Yank (copy) and put (paste) text
{visual}y				yank (copy) the highlighted text into a register
## Visual: Delete
{visual}d				delete the highlighted text
{visual}J				join the highlighted lines
{visual}gJ				join the highlighted lines without inserting spaces
## Visual: Change (delete and insert)
{visual}r{char}			in Visual block mode: replace each char of the selected text with {char}
{visual}c				change the highlighted text
{visual}c				in Visual block mode: change each of the selected lines with the entered text
{visual}C				in visual block mode: change each of the selected lines until end-of-line with the entered text
{visual}~				switch case for highlighted text
{visual}u				make highlighted text lowercase
{visual}U				make highlighted text uppercase
{visual}g?				perform rot13 encoding on highlighted text
## Visual: Complex changes
{visual}!{command}<CR>	filter the highlighted lines through {command}
{visual}=				filter the highlighted lines through 'equalprg'

# Replace Mode


## Tags
You don't really need tags because VS Code has very good support for tags with the Goto Symbol shortcut.
