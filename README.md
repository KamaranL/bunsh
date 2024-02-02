```bash
    __                     __
   / /_  __  ______  _____/ /_
  / __ \/ / / / __ \/ ___/ __ \
 / /_/ / /_/ / / / (__  ) / / /
/_.___/\__,_/_/ /_/____/_/ /_/

```

> bundle your shell scripts into a single executable script or library

- [Configuration](#configuration)
- [Installation and Usage](#installation-and-usage)
  - [Dependencies](#dependencies)
  - [Install](#install)
  - [Usage](#usage)
- [Examples](#examples)

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

1. Clone repository:

   ```bash
   git clone https://github.com/KamaranL/bunsh.git
   ```

1. Navigate to the newly downloaded repo and mark the script as executable[^2]:

   ```bash
   chmod +x bunsh
   ```

1. Add to PATH <u>or</u> create a symbolic link: *(recommended)*

   - Adding cloned repo to path:

     ```bash
     echo 'export PATH=$PATH:<absolute_path_to_cloned_repo>' >>~/.bash_profile
     ```

   - Creating a symbolic link:

     ```bash
     ln -s <absolute_path_to_cloned_repo>/bunsh /usr/local/bin/bunsh
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

## Examples

See [examples](/examples/README.md#examples)

<!-- footnotes -->

[^1]: Most, if not all, of the listed dependencies come pre-installed on MacOS and a good number of Linux distributions- provided for transparency.

[^2]: Everyone has doubts, see [my wiki page](https://github.com/KamaranL/KamaranL/wiki#validation) on validation if you're like everyone.