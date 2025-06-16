# xdg-utils - Scripts and Documentation

The actual scripts are generated from `xdg-*.in`,
[xdg-utils-common.in](xdg-utils-common.in) and
[desc/xdg-*.xml](desc) which contains the
command line descriptions.

These are the files that you want to edit.
To add a new script, you must also provide 
a `desc/xdg-*.xml` file.

**Do not make changes to the generated scripts themselves!**

---

Use `make scripts-clean` to delete all generated files and use
`make scripts` to re-generate them.

The manual files in [man/](man) and [html/](html)
are also generated from the [desc/xdg-*.xml](desc) files.

Use `make scripts html man` to update all generated files

Use `make release` to remove everything but the generated files.

## Shellcheck

[Shellcheck](https://shellcheck.net) is a linting tool to avoid common mistakes with shell scripts.

The provided [shellcheck-xdg-util.sh](./shellcheck-xdg-util.sh) script wraps shellcheck in a way that it is preconfigured and calculates the line numbers back to the source file.

Please run it with the path to the source file as its first argument and see if any of the reported issues have been caused by your changes.

Example:

```sh
./shellcheck-xdg-util.sh xdg-open.in
```

Long term goal is to get all xdg-utils to pass shellcheck without any warnings.

## Writing Documentation

The files in [desc/](desc) make use of the [DocBook format](https://tdg.docbook.org/).

If you want to use more recent tags please update to the minimum version of DocBook that is required by changing the `DOCTYPE`.

Example:

You want to use the `<code>` tag which requires DocBook 4.3.

Replace the lines …

```xml
<!DOCTYPE refentry PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"
    "http://www.oasis-open.org/docbook/xml/4.1.2/docbookx.dtd" [
]>
```

… with:

```xml
<!DOCTYPE refentry PUBLIC "-//OASIS//DTD DocBook XML V4.3//EN"
    "http://www.oasis-open.org/docbook/xml/4.3/docbookx.dtd" [
]>
```

For spellchecking one can use the following command:
```sh
LANG=en_US aspell --mode=sgml check desc/<file>.xml
```

## Coding Conventions

Here are some recommendations that should make working together easier.

### Prefer Tabs for Indentation

New xdg-utils code should use tabs for indentation, reason being that tabs are mode for indentation and are easily configured to match the preferred width.

Old xdg-utils code is a colourful mix of indentation, in that case please try to match the surrounding code.

### Comment Functions

Help others understand the code by commenting what each function does.

```sh
# Short description and possible explanations
#
# (list, of, arguments)
# shellcheck disable=SC0000 # Reason for the shellcheck being disabled
descriptive_function_name() {
	# code goes here
}
```

The first section is a short description followed by an explanation of what the function does. The explanation should focus on non-obvious pitfalls or more complex logic that might not be immediately visible. For readability the description is followed by an empty comment line.

If the function is simple enough the explantion and even description can be omitted.

---

What follows is a list of positional arguments that the function takes.

If none simply open and close the parenthesis `()`.

If an argument being empty or omitted is a non-obvious valid input write ` (or empty)` after its name.

If the last argument can be repeated write ` ...` after its name.

---

Contributions that add these for legacy code are welcome!
