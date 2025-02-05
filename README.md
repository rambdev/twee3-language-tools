# Twee 3 Language Tools

Syntax highlighting for HTML and select storyformats (see [Features](#features)) on top of Twee 3 code.

Made possible through contributions from:
- [@Goctionni](https://github.com/Goctionni)
- [@MinusGix](https://github.com/MinusGix)
- [@rambdev](https://github.com/rambdev)

And feedback from the folks over at the Twine Games [Discord Server](https://discord.com/invite/n5dJvPp).

---

## **Requirements**

The extension relies on a workspace (or a folder) being open. If single files are to be edited, the storyformat must be configured manually.

Supported file extensions:

- `.tw`
- `.twee`

To set the correct storyformat for the files, a `StoryData` passage with the storyformat (and version) (see example below) mentioned in it is preferred. If not, the extension provides the option to set the format explictly. (See [Extension Settings.](#extension-settings))

```json
:: StoryData
{
	"ifid": "<ifid here>",
	"format": "<story format here, i.e. 'SugarCube'>",
	"format-version": "<story format version here, i.e. '2.35.0'>"
}
```

---

## **Features**

### Twee
- Syntax highlighting.  

- Snippet to generate the `StoryData` special passage: start typing `StoryData` in a Twee document and press <kbd>Tab</kbd> when prompted with the snippet. This populates the IFID field with a newly generated one.

- Command palette tool to generate IFID: open the command palette (<kbd>Ctrl/Cmd + Shift + P</kbd> or <kbd>F1</kbd> by default) and search for "IFID".  

- A list of passages for quick jumps (can be grouped by files, folders, or passage tags. See [extension-settings](#extension-settings).) Open from the Twee 3 Language Tools tab on the activity bar (the forking paths logo.)  

    ![Passage List](docs/images/twee-passage-list.png)  

- Workspace statistics Status bar item (WIP) (Contributed by @rambdev).  
	- Total passage count (includes Story passages, Special passages, and Script/Stylesheet-tagged passages)  
	- Story passage count  

- A story-map view (Contributed by @Goctionni) that opens in the browser! Still in *very* early stages. Also accessible from the Twee 3 Language Tools tab on the activity bar.

	- Currently implemented features:  
		- Snap to grid (button on top-left.)  
		- Arrows to linked passages.  
		- Passage position, size, and tags can be edited via a sidebar. Changes are *not* currently autosaved, and a manual save button is present.  
		- Multi-select, and thereby mass editing of position, size, and tags.  
		- Ability to move passages across files.

	- Usage:
		- Use the middle-mouse button, or hold down <kbd>Shift</kbd> while dragging the mouse to pan the map grid.
		- Scroll the mousewheel or stretch/pinch on trackpad to zoom in/out.
		- Hold <kbd>Ctrl/Cmd</kbd> while selecting to add new passages to selection, or remove already added passages from it.

	![Story Map](docs/images/twee-storymap.png)

### SugarCube
*(id: `sugarcube-2`)*
- Syntax highlighting:  
    ![SugarCube syntax](docs/images/hl-sc2.png)
- Macro documentation on hover. (Contributed by @MinusGix) (Custom definitions can be added via `*.twee-config.yml`. See: [Custom macro definitions for SC](#custom-macro-definitions-for-sugarcube)):  
	- [Screenshot - macro documentation](docs/images/sc2-hovertips.png)
- Container macro pair highlights:  
	- [Screenshot - macro pairs](docs/images/sc2-macro-tag-matching.png)
- Snippets. (Contributed by @rambdev) Type macro names to get code snippet inserts with placeholder values:  
	- [Screenshot - snippet](docs/images/sc2-snippets.png)
	- [Screenshot - snippet insert](docs/images/sc2-snippets-insert.png)
	- [Screenshot - wrapping snippets](docs/images/sc2-snippets-wrap.png)
- Diagnostics:  
	- Macros with opening tags but no closes (and vice-versa):  
		- [Screenshot - diagnostics](docs/images/sc2-unclosed-macro.png)
	- Deprecated macros:  
		- [Screenshot - diagnostic](docs/images/sc2-deprecated-macro.png)
	- Deprecated `<<end...>>` closing macros:  
		- [Screenshot - diagnostic](docs/images/sc2-endvariant-macro.png)
		- [Screenshot - quick fix](docs/images/sc2-endvariant-quickfix.png)
	- Unrecognized macros. New/custom macros can be defined manually (see: [Custom macro definitions for SC](#custom-macro-definitions-for-sugarcube)), but anything else will throw a warning. This can be turned off by the `twee3LanguageTools.sugarcube-2.undefinedMacroWarnings` setting ([see settings](#extension-settings)):  
		- [Screenshot - diagnostic](docs/images/sc2-unrecognized-macro.png)
		- [Screenshot - quick fix](docs/images/sc2-unrecognized-quickfix.png) (Writes definitions to `t3lt.twee-config.yml` in the root of the first workspace folder.)
	- Invalid argument syntax in macros (Contributed by @MinusGix):  
		- [Screenshot - diagnostics](docs/images/sc2-parameter-validation.png)
	- Argument validation (Contributed by @MinusGix): [Read here](docs/parameters.md) for more information.

### Chapbook
*(id: `chapbook-1`)*
- Syntax highlighting.  
    ![Chapbook syntax](docs/images/hl-cb1.png)

### Harlowe
*(id: `harlowe-3`)*
- Syntax highlighting.  
    ![Harlowe syntax](docs/images/hl-h3.png)

---

## **twee-config**

### Custom Macro definitions for SugarCube

The extension adds diagnostics for erroneous usage of macros in TwineScript for the `sugarcube-2` storyformat. By default, only the definitions for the core SugarCube library are present, but custom definitions can be added. The process is as follows:

1. Add a `*.twee-config.yaml` (or `.yml`) **OR** `*.twee-config.json` (`*` represents any valid file name) file to your project folder (or anywhere in the workspace.)
2. Define custom macros as follows:
	- If using `*.twee-config.yaml` (indentation is important for YAML files):
		```yaml
		sugarcube-2:

		  macros:

		    customMacroName:
		      container: true

		    anotherOne: {}
		```
	- If using `*.twee-config.json`:
		```json
		{
			"sugarcube-2": {
				"macros": {
					"customMacroName": {
						"container": true
					},
					"anotherOne": {}
				}
			}
		}
		```
The following properties are currently programmed, even though not all of them are used as of now:
- **name** `(string)` *optional*: Name of the macro (currently unused in code; the name of the object suffices for now.)
- **description** `(string)` *optional*: Description of macro. Shown on hover. Supports markdown.
- **container** `(boolean)` *optional*: If the macro is a container (i.e. requires a closing tag) or not. `false` by default.
- **selfClose** `(boolean)` *optional*: If the macro is a self-closable. Requires macro to be a container first. `false` by default.
- **children** `(string array)` *optional*: If the macro has children, specify their names as an array (currently unused in code.)
- **parents** `(string array)` *optional*: If the macro is a child macro, specify the names of its parents as an array (currently unused in code.)
- **deprecated** `(boolean)` *optional*: If the macro is deprecated or not. `false` by default.
- **deprecatedSuggestions** `(string array)` *optional*: If the macro is deprecated, specify any alternatives to the macro as an array.
- **parameters** `(object)` *optional*: Allows for macro argument validation. [Read here](docs/parameters.md) for more information.
- **decoration** `(object)` *optional*: Allows for declaring decorations to be displayed on that macro. Uses [DecorationRenderOptions](https://code.visualstudio.com/api/references/vscode-api#DecorationRenderOptions)' fields. Requires `definedMacroDecorations` setting to be enabled.

**NOTE:** Multiple `twee-config` files can be present in a workspace. They will stack and add to the macro definitions for the workspace. The recommended strategy is to make separate files for separate macro sets/libraries, e.g. (the following file can also be used as an example):
- `click-to-proceed.twee-config.yaml` ([Link](https://github.com/cyrusfirheir/cycy-wrote-custom-macros/blob/master/click-to-proceed/click-to-proceed.twee-config.yaml))

---

## **Extension Settings**

This extension contributes the following settings:

Automatically set by the `StoryData` special passage (if it exists):
- `twee3LanguageTools.storyformat.current`: Identifier of the storyformat in use.

Manual settings:

**NOTE:** It's recommended to change these settings separately for each workspace instead of globally.

- `twee3LanguageTools.storyformat.override` : Identifier of the storyformat to override the format set by `StoryData`.  
⠀
- `twee3LanguageTools.directories.include`: Directories in which to look for twee files. Use glob patterns *relative* to the root of the workspace folders (e.g. `src/unprocessed/twee`, `src/static`, `external`). (Searches the entire workspace by default.)  
- `twee3LanguageTools.directories.exclude`: Directories to exclude from the search of twee files. Use *absolute* glob patterns (e.g. `**/src/processed/**`). (Excludes `**/node_modules/**` by default.) If passage listing is active, excluded files will not be scanned for passages. They also will not be scanned for errors until manually opened.  
⠀
- `twee3LanguageTools.storyMap.unusedPortClosingDelay`: Duration in milliseconds before the Story Map server port is closed after the UI in the browser window has been closed. (`5000` by default.)
⠀
- `twee3LanguageTools.passage.list`: Display list of passages with quick 'jump' links? (`false` by default.)  
- `twee3LanguageTools.passage.group`: Group passages by? (`None` by default. Can be grouped by file of origin, folder of origin, or passage tags.)  
⠀
- `twee3LanguageTools.twee-3.warning.spaceAfterStartToken`: Warn about missing space after the start token (`::`) in passage headers? (`true` by default.)  
- `twee3LanguageTools.twee-3.error.storyData.format`: Throw an error when missing the story format field (`format`) in `StoryData` special passage? (`true` by default.)  
- `twee3LanguageTools.twee-3.error.storyData.formatVersion`: Throw an error when missing the story format version field (`format-version`) in `StoryData` special passage? (`true` by default.)  
⠀
- `twee3LanguageTools.sugarcube-2.definedMacroDecorations`: Whether to use the decorations field in macro definitions. (`false` by default.)
- `twee3LanguageTools.sugarcube-2.warning.undefinedMacro`: Warn about macros/widgets which were not found in definitions (`*.twee-config.yaml` or `*.twee-config.json` files) or the core SugarCube macro library? (`true` by default.)  
- `twee3LanguageTools.sugarcube-2.warning.deprecatedMacro`: Warn about deprecated macros/widgets? (`true` by default.)  
- `twee3LanguageTools.sugarcube-2.warning.endMacro`: Warn about the deprecated `<<end...>>` closing tag syntax? (`true` by default.)  
- `twee3LanguageTools.sugarcube-2.warning.barewordLinkPassageChecking`: Provides warnings for links like `[[passage]]` when `passage` is not a valid passage name. This could cause false positives in cases where you are using a global variable. (`true` by default.)  
⠀
- `twee3LanguageTools.sugarcube-2.error.argumentParsing`: Provide errors about invalid argument syntax being passed into macros? (`true` by default.)  
- `twee3LanguageTools.sugarcube-2.error.parameterValidation`: Provide errors about invalid argument types being passed into macros? (`true` by default.)  
⠀
- `twee3LanguageTools.sugarcube-2.features.macroTagMatching`: Highlight opening and closing tags of container macros? (`true` by default.)  
⠀
- `twee3LanguageTools.experimental.sugarcube-2.selfClosingMacros.enable`: Enable self-closing syntax for container macros? [Read here](#sugarcube-2-self-closing-macros) for more information. (`false` by default.)  
- `twee3LanguageTools.experimental.sugarcube-2.selfClosingMacros.warning.irrationalSelfClose`: Warn about self-closed instances of content focused macros (e.g. `<<script />>`)? (`true` by default.)  

---

## **Experimental Stuff**

### Passage Auto-packer

Uses a simple packing algorithm to space out passages into clusters based on the file they originate from.

To use, search for `Pack passages to clusters` from the command palette (<kbd>Ctrl/Cmd + Shift + P</kbd> or <kbd>F1</kbd> by default).

---

### SugarCube-2: Add All Unrecognized Macros to Definition File

Adds every unrecognized macro to the definition file, instead of doing it one by one.

To use, search for `Unrecognized Macros` from the command palette (<kbd>Ctrl/Cmd + Shift + P</kbd> or <kbd>F1</kbd> by default).

However, it is still recommended to add definitions one at a time.

---

### SugarCube-2: Self-closing macros

***NOTE:*** SugarCube 2 does *NOT* have a self-closing syntax for container macros, this feature is just to support custom passage processing functions.

Example of such a function which replaces self-closed instances with the actual closing macro tag (i.e. `<<macro />>` with `<<macro>><</macro>>`):
```js
Config.passages.onProcess = function(p) {
	const macroNamePattern = `[A-Za-z][\\w-]*|[=-]`;

	const selfCloseMacroRegex = new RegExp(`<<(${macroNamePattern})((?:\\s*)(?:(?:/\\*[^*]*\\*+(?:[^/*][^*]*\\*+)*/)|(?://.*\\n)|(?:\`(?:\\\\.|[^\`\\\\])*\`)|(?:"(?:\\\\.|[^"\\\\])*")|(?:'(?:\\\\.|[^'\\\\])*')|(?:\\[(?:[<>]?[Ii][Mm][Gg])?\\[[^\\r\\n]*?\\]\\]+)|[^>]|(?:>(?!>)))*?)\\/>>`, 'gm');

	return p.text.replace(selfCloseMacroRegex, "<<$1$2>><</$1>>");
};
```

The `twee3LanguageTools.experimental.sugarcube-2.selfClosingMacros.enable` setting enables detection of self-closed macros.

---

## **Known issues**

Argument validation is still a work in progress. Passage name validation, especially. Shouldn't hinder workflow, however.

---

## **Changelog**

Changelog [here](CHANGELOG.md).

---
