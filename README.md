
<div align="center">

![tidier-day](https://user-images.githubusercontent.com/8259221/162104589-ab22ecd7-cc6f-4886-bcf0-cf897ef427e3.png#gh-light-mode-only)
![tidier-night](https://user-images.githubusercontent.com/8259221/162104591-325e0c1a-52fd-4369-a992-809dea865df4.png#gh-dark-mode-only)

</div>

<div align="center">

[![tidier version](https://img.shields.io/npm/v/tidier?logo=npm&label=tidier)](https://www.npmjs.com/package/tidier)&nbsp;
[![tidier-core version](https://img.shields.io/npm/v/tidier-core?logo=npm&label=tidier-core)](https://www.npmjs.com/package/tidier-core)&nbsp;
[![Release status](https://img.shields.io/github/workflow/status/mausworks/tidier/release?event=push&logoColor=ffffff&logo=github-actions&label=Release)](https://github.com/mausworks/tidier/actions/workflows/release.yml)&nbsp;
[![Test status](https://img.shields.io/github/workflow/status/mausworks/tidier/release?event=push&logoColor=ffffff&logo=github-actions&label=Tests)](https://github.com/mausworks/tidier/actions/workflows/test.yml)

</div>

<div align="center">

_Tidier allows you to configure file & folder name conventions,
and actively helps you follow them!_

</div>

https://user-images.githubusercontent.com/8259221/162093220-ec9e15e4-b59c-4f07-b643-5709389ffb9d.mp4

## Installation

Installing the [VSCode extension](https://marketplace.visualstudio.com/items?itemName=mausworks.tidier-vscode):

```plaintext
ext install mausworks.tidier-vscode
```

Installing the CLI:

```shellscript
yarn add --dev tidier
```

Run the CLI with npx:

```shellscript
npx tidier --help
```

## Usage

To start using Tidier, create a `.tidierrc` to the root of your project.
This is where you will configure your file & folder name conventions.
By default, Tidier will ignore files specified in your project's `.gitignore`,
but you can add additional patterns in the "ignore"-section in the configuration file if need be.

The example configuration below uses common React conventions,
but it can be adapted to work with any project of any framework or language.

```json
{
  "ignore": [ "**/build" ],
  "files": {
    "**/src/(setupTests|reportWebVitals).*": "camelCase",
    "**/src/**/index.*": "camelCase",
    "**/src/**/*.{tsx,jsx,css,scss,sass}": "PascalCase.kebab-case",
    "**/(README|LICENSE)*": "UPPER CASE.lc",
    "**/*": "kebab-case"
  },
  "folders": {
    "**/*": "kebab-case"
  }
}
```

The configuration consists two sets of naming conventions: one for files, and one for folders.
Each convention consists of a glob, and a _name format_.

Globs are always matched from the location of the configuration file. 
The first glob that matches gets priority, so conventions with higher specificity should be declared at the top.

## Tidier & names

Tidier conceptualizes each file or folder name as a sequence of _fragments_ delimited by the `.` (period) character.

<div align="center">

![tidier-name-taxonomy-filename](https://user-images.githubusercontent.com/8259221/162204030-90f0f90a-40cc-451e-b958-9978c746566a.png)

</div>

The file named `FileBrowser.spec.tsx` consists of three fragments: `FileBrowser`, `spec` and `tsx`.
If a name consists of more than one fragment, Tidier will consider the last one 
to be the sole _extension fragment_. Note: This also applies to folders.

### Name formats

The name format is the part of the convention that decides _how_ a name should be formatted.
Like with file & folder names, Tidier also conceptualizes them as a sequence of fragments: `PascalCase.kebab-case.lc`.
The idea is that the name format structure should appear similar to a desired name of a file: `FileBrowser.spec.tsx`.

<div align="center">

![tidier-name-taxonomy-name-format](https://user-images.githubusercontent.com/8259221/162204028-4b426e6a-0a4d-46a5-9cef-e21590d51719.png)

</div>

To better understand the mechanics of how Tidier reformats names, let's start with a basic example:
Using the general casing `UPPER CASE` denotes that upper case
must be used for _all fragments_ of the file or folder name—including the extension.

```
UPPER CASE -> README.MD
```

To change the extension to use lower case instead, specify `UPPER CASE.lc` as the name format. 
Two character casings such as `lc` are _extension casings_ (lc = "lower case"),
and in contrast to the general casings, they are only ever applied to the _extension fragment_.

The extension casing can only be used _once_ within the name format,
and it always has to be the last fragment.

```
UPPER CASE.lc -> README.md
```

Using the _general casing_ `lower case` has a similar effect,
but in this case all subsequent fragments of the first will be in lower case, including the extension.

```
UPPER CASE.lower case 
-> README.md, 
-> SOME.longer.file.name.txt
```

### Border characters

Tidier will ignore leading and trailing border characters for each fragment, these are: `_`, `[`, `]`, `(` and `)`.
This means that if you have a folders such as `__mocks__`  or `__test__`, or a file named `[id].tsx`,
Tidier is simply going to ignore these characters, and format what's in between them.

However: Including these characters in the middle of a fragment (and not at the boundaries),
they might be removed or replaced depending on the casing specified in the name format.

### Supported Casings

The table below lists all the fragment casings which are supported by Tidier.
Unless specified in a comment, Tidier uses recasing functions from [change-case](https://github.com/blakeembrey/change-case).

| Name & Aliases                                      | Comment                                               |
|-----------------------------------------------------|-------------------------------------------------------|
| `preserve`                                          | Tidier will make no attempt to re-format the fragment |
| `lower case`                                        | Calls `toLowerCase()` for the fragment                |
| `UPPER CASE`                                        | Calls `toUpperCase()` for the fragment                |
| `Title Case`                                        |                                                       |
| `camelCase`                                         |                                                       |
| `PascalCase`, `UpperCamelCase`                      |                                                       |
| `kebab-case`, `dash-case`, `lower-header-case`      |                                                       |
| `COBOL-CASE`, `UPPER-KEBAB-CASE`, `UPPER-DASH-CASE` |                                                       |
| `Train-Case`, `Header-Case`                         |                                                       |
| `snake_case`                                        |                                                       |
| `Snake_Title_Case`                                  |                                                       |
| `UPPER_SNAKE_CASE`                                  |                                                       |
| `p`                                                 | Extension casing for `preserve`                       |
| `lc`                                                | Extension casing for `lower case`                     |
| `UC`                                                | Extension casing for `upper case`                     |
| `Tc`                                                | Extension casing for `Title Case`                     |
| `sPoNGEcAsE`                                        | _When your naming convention is chaos_                |

# Glossary

For convenience and consistency; the most noteworthy concepts and their meanings within Tidier are defined below:

- **Fragment** — Part of a file name, delimited by the period character (`.`).
- **Extension fragment** — The last fragment of a file or folder name.
- **Name convention** — Defines which name format should apply to which files or folders.
- **Name format** — A sequence of casings, delimited by the period character (`.`).
- **General casing** — A casing which may be used to format to any fragment of a name; including the extension.
- **Extension casing** — A casing which appears at the end of a name format and exclusively formats the extension fragment.
- **Project** — A set of files folder for which the conventions specified in the configuration applies.
- **Configuration** — Refers to the `.tidierrc` file, and denotes the root of a project.
- **Ignorefile** — A file which specifies files and folders to ignore within a project, e.g: `.gitignore`.
- **Recase** — The mechanism of changing the casing of a file or folder name.
