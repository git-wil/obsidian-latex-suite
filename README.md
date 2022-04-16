# Obsidian Latex Suite
A plugin for Obsidian that aims to make typesetting LaTeX as fast as handwriting.

Inspired by [Gilles Castel's setup](https://castel.dev/post/lecture-notes-1/) using UltiSnips.

![demo](gifs/demo.gif)

The plugin's main feature is **snippets**, which help you write LaTeX quicker through text expansion. For example, type

- "xdot" instead of "\dot{x}"
- "a/b" instead of "\frac{a}{b}"
- "par x	y	" instead of "\frac{\partial x}{\partial y}"

See [here](https://castel.dev/post/lecture-notes-1/) for more information.


## Features
- Snippets
	- Tabstops
	- Regex
	- Options
- Multiple cursor support
- Auto-fraction
- Matrix shortcuts
- Auto-enlarge brackets
- Tabout
- Editor commands

You can switch features on/off in settings.


The plugin comes with a [set of default snippets](https://github.com/artisticat1/obsidian-latex-suite/blob/main/src/default_snippets.ts), loosely based on [Gilles Castel's](https://castel.dev/post/lecture-notes-1/#other-snippets). You can modify them, remove them, and write your own!


## Usage
To get started, type "dm" to enter display math mode. Try typing the following:

- "xsr" → "x^{2}".

- "x/y <kbd>Tab</kbd>" → "\\frac{x}{y}".

- "sin @t" → "\\sin \\theta".

**Have a look at the [cheatsheet](#cheatsheet)** for a list of commonly used default snippets.


Once these feel familiar, you can check out the [default snippets](https://github.com/artisticat1/obsidian-latex-suite/blob/main/src/default_snippets.ts) for a fuller set of commands. e.g.

- "par f <kbd>Tab</kbd> x <kbd>Tab</kbd>" → "\\frac{\\partial f}{\\partial x}".

- "dint <kbd>Tab</kbd> 2pi <kbd>Tab</kbd> sin @t <kbd>Tab</kbd> @t <kbd>Tab</kbd>" → "\\int_{0}^{2\pi} \\sin \\theta \\, d\\theta".


You can also add your own snippets!


## Documentation
### Snippets
Snippets are formatted as follows:

```typescript
{trigger: string, replacement: string, options: string, description?: string, priority?: number}
```

- `trigger` : The text that triggers this snippet.
- `replacement` : The text to replace the `trigger` with.
- `options` : See below.
- `priority` (optional): This snippet's priority. Snippets with higher priority are run first. Can be negative. Defaults to 0.
- `description` (optional): A description for this snippet.


#### Options
- `m` : Math mode. Only run this snippet inside math
- `t` : Text mode. Only run this snippet outside math
- `A` : Auto. Expand this snippet as soon as the trigger is typed. If omitted, the <kbd>Tab</kbd> key must be pressed to expand the snippet
- `r` : Regex. The `trigger` will be treated as a regular expression

Multiple options can be used at once.


#### Regex
- Use the `r` option to create a regex snippet.
- In the `trigger`, surround an expression with brackets `()` to create a capturing group.
- Inside the `replacement` string, strings of the form `[[X]]` will be replaced by matches in increasing order of X, starting from 0.
- Some characters, such as `\`, `+`, and `.`, are special characters in regex. If you want to use these literally, remember to escape them by inserting two backslashes (`\\`) before them!
  - (One backslash to escape the special character, and another to escape that backlash)

##### Example
The snippet
```typescript
{trigger: "([A-Za-z])(\\d)", replacement: "[[0]]_{[[1]]}", options: "rA"}
```
will expand `x2` to `x_{2}`.


#### Tabstops
- Insert tabstops for the cursor to jump to using `$X`, where X is a number starting from 0.
- Pressing <kbd>Tab</kbd> will move the cursor to the next tabstop.
- Tabstops can have placeholders. Use the format `${X:text}`, where `text` is the text that will be selected by the cursor on moving to this tabstop.
- Tabstops with the same number, X, will all be selected at the same time.

##### Examples
```typescript
{trigger: "//", replacement: "\\frac{$0}{$1}$2", options: "mA"}

{trigger: "dint", replacement: "\\int_{${0:0}}^{${1:\\infty}} $2 d${3:x}", options: "mA"}

{trigger: "outp", replacement: "\\ket{${0:\\psi}} \\bra{${0:\\psi}} $1", options: "mA"}
```


#### Variables
The following variables are available for use in a `trigger` or `replacement`:

- `${VISUAL}` : Can be inserted in a `replacement`. When the snippet is expanded, "${VISUAL}" is replaced with the current selection.
	- Visual snippets will not expand unless text is selected.

##### Examples
![visual snippets](gifs/visual_snippets.gif)


- `${GREEK}` : Can be inserted in a `trigger`. Shorthand for the following:

```
alpha|beta|gamma|Gamma|delta|Delta|epsilon|varepsilon|zeta|eta|theta|Theta|iota|kappa|lambda|Lambda|mu|nu|xi|Xi|pi|Pi|rho|sigma|Sigma|tau|upsilon|phi|Phi|chi|psi|Psi|omega|Omega
```

Recommended for use with the regex option "r".

- `${SYMBOL}` : Can be inserted in a `trigger`. Shorthand for the following:

```
hbar|ell|nabla
```

Recommended for use with the regex option "r".


### Auto-fraction
Lets you type "1/x" instead of "\frac{1}{x}".

For example, it makes the following expansions:

- `x/` → `\frac{x}{}`
- `(a + b(c + d))/` → `\frac{a + b(c + d)}{}`

and moves the cursor inside the brackets.

Once done typing the denominator, press <kbd>Tab</kbd> to exit the fraction.

![auto-fraction](gifs/auto-fraction.gif)


### Auto-enlarge brackets
When a snippet containing "\\sum", "\\int" or "\\frac" is triggered, any enclosing brackets will be enlarged with "\\left" and "\\right".

![auto-enlarge brackets](gifs/auto-enlarge_brackets.gif)


### Matrix shortcuts
While inside a matrix, array, align, or cases environment,

- Pressing <kbd>Tab</kbd> will insert the "&" symbol
- Pressing <kbd>Enter</kbd> will insert "\\\\" and move to a new line
- Pressing <kbd>Shift + Enter</kbd> will move to the end of the next line (can be used to exit the matrix)

![matrix shortcuts](gifs/matrix_shortcuts.gif)


### Tabout
- Pressing <kbd>Tab</kbd> while the cursor is at the end of an equation will move the cursor outside the $ symbols.
- Otherwise, pressing <kbd>Tab</kbd> will advance the cursor to the next closing bracket: ), ], }, >, or |.


### Editor commands
- Box current equation – surround the equation the cursor is currently in with a box.
- Select current equation – select the equation the cursor is currently in.



## Cheatsheet

| Trigger           | Replacement      |
| ----------------- | ---------------- |
| mk                | \$ \$            |
| dm                | \$\$<br><br>\$\$ |
| sr                | ^{2}             |
| cb                | ^{3}             |
| rd                | ^{ }             |
| \_                | \_{ }            |
| sq                | \\sqrt{ }        |
| x/y               | \\frac{x}{y}     |
| //                | \\frac{ }{ }     |
| te <kbd>Tab</kbd> | \\text{ }        |
| x1                | x_{1}            |
| xdot              | \\dot{x}         |
| xhat              | \\hat{x}         |
| xbar              | \\overline{x}         |

When running a snippet that **moves the cursor inside brackets {}, press <kbd>Tab</kbd> to exit the brackets**.


### Greek letters

| Trigger | Replacement  | Trigger | Replacement |
|---------|--------------|---------|-------------|
| @a      | \\alpha      | eta     | \\eta       |
| @b      | \\beta       | mu      | \\mu        |
| @g      | \\gamma      | nu      | \\nu        |
| @G      | \\Gamma      | xi      | \\xi        |
| @d      | \\delta      | Xi      | \\Xi        |
| @D      | \\Delta      | pi      | \\pi        |
| @e      | \\epsilon    | Pi      | \\Pi        |
| :e      | \\varepsilon | rho     | \\rho       |
| @z      | \\zeta       | tau     | \\tau       |
| @t      | \\theta      | phi     | \\phi       |
| @T      | \\Theta      | Phi     | \\Phi       |
| @k      | \\kappa      | chi     | \\chi       |
| @l      | \\lambda     | psi     | \\psi       |
| @L      | \\Lambda     | Psi     | \\Psi       |
| @s      | \\sigma      |         |             |
| @S      | \\Sigma      |         |             |
| @o      | \\omega      |         |             |
| ome     | \\omega      |         |             |

For greek letters with short names (2-3 characters), just type their name!
e.g. "pi" → "\\pi"



## Acknowledgements
- [@tth05](https://github.com/tth05)'s [Obsidian Completr](https://github.com/tth05/obsidian-completr) for the basis of the tabstop code
- [Dynamic Highlights](https://github.com/nothingislost/obsidian-dynamic-highlights/blob/master/src/settings/ui.ts) for reference
- [Quick Latex for Obsidian](https://github.com/joeyuping/quick_latex_obsidian) for inspiration