```bash
    __                     __
   / /_  __  ______  _____/ /_
  / __ \/ / / / __ \/ ___/ __ \
 / /_/ / /_/ / / / (__  ) / / /
/_.___/\__,_/_/ /_/____/_/ /_/
```

> bundle your shell scripts into a single executable script or library

[![latest release](https://badgen.net/github/release/KamaranL/bunsh?icon=github&cache=3600)](https://github.com/KamaranL/bunsh/releases/latest)

- [Configuration](#configuration)
- [Installation and Usage](#installation-and-usage)
  - [Dependencies](#dependencies)
  - [Install](#install)
    - [Local](#local)
    - [GitHub Actions](#github-actions)
  - [Usage](#usage)
    - [Decorators](#decorators)
- [Examples](#examples)
- [Validity](#validity)

## Configuration

bunsh can be configured via command line options, config file, or a combination of both. If no config file or args are provided, bunsh will use hardcoded defaults. Below is a table showing what configuration options are available, and their defaults.

| Config File Key  | Description                                   | Command Line Equivalent        | Default Value           |
| ---------------- | --------------------------------------------- | ------------------------------ | ----------------------- |
| b.src            | source directory                              | -s \| --source                 | ./src                   |
| b.out            | output directory                              | -o \| --out                    | ./out                   |
| b.name           | bundled file name                             | -n \| --name                   | bundled-\<short_sha256> |
| s.version        | bundled file version                          | --opt s.version=\<semver>      | 0.1.0                   |
| s.libonly        | generate library only                         | --opt s.libonly=\<bool>        | false                   |
| s.err_noargs     | make bundle error when `$# == 0` on commands  | --opt s.err_noargs=\<bool>     | false                   |
| s.err_illegalopt | make bundle error on unknown args on commands | --opt s.err_illegalopt=\<bool> | false                   |

## Installation and Usage

### Dependencies

[^1]

- bash `>=4`
- cat
- date
- dirname
- grep
- mkdir
- realpath
- sed
- sha256sum

### Install

#### Local

Copy + paste the following in your terminal to download, unpack, and link the [latest release](https://github.com/KamaranL/bunsh/releases/latest):

  ```bash
  NAME=bunsh VER="$(curl -sL "https://raw.githubusercontent.com/KamaranL/$NAME/main/VERSION.txt")" && {
     DIR="/usr/local/etc/$NAME.d"
     [ ! -d "$DIR" ] && mkdir -p "$DIR"
     curl -L "https://github.com/KamaranL/$NAME/releases/download/v$VER/$NAME-v$VER.tgz" | tar -vxz -C "$DIR"
     chmod +x "$DIR/$NAME"
     [ ! -f "/usr/local/bin/$NAME" ] && ln -s "$DIR/$NAME" "/usr/local/bin/$NAME"
     "$NAME" -v && "$NAME" -h
  }
  ```

#### GitHub Actions

  ```yml
  #...
  jobs:
    ci:
      runs-on: ubuntu-latest
      steps:
        - uses: KamaranL/bunsh@main
  #...
  ```

### Usage

```text
usage: bunsh [options]
options:
  -c,--config <file>  specify a config file to read from
  -s,--src <path>     specify a source directory to read from
  -o,--out <path>     specify an output directory to place the bundle
  -n,--name <val>     specify a name for the bundle
  --opt <key>=<val>   specify additional config option
  -d,--dry-run        do not write bundle to file, print to console
  -q,--quiet          suppress all output to console
  -x,-xx,--verbose    print verbose output to the console
  -h,--help           print this help message
  -v,--version        print version
```

#### Decorators

bunsh uses pre-defined decorators[^2] to generate the help messages for each function:

| Name     | Purpose                                                                                                                                                 |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `@desc`  | provides a brief description of the function when viewed from a parent help menu - each `@desc` line will be read altogether (as one whole description) |
| `@param` | defines the named parameters, or options, that are available to the function - each `@param` line will be read individually                             |
| `@arg`   | defines the positional parameters, or args, that are available to the function - each `@arg` line will be read individually                             |

## Examples

See [examples](/examples/README.md#examples)

## Validity

Everyone has doubts, see [my wiki page](https://github.com/KamaranL/KamaranL/wiki#validation) on validation if you're like everyone :smile:

[^1]: Most, if not all, of the listed dependencies come pre-installed on MacOS and a good number of Linux distributions- provided for transparency.

[^2]: Take a look at [this file](https://github.com/KamaranL/bunsh/blob/main/examples/src/public/greet) to see how decorators can be used.
