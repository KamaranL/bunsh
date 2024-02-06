# Examples

## Config File

We currently have two config files, *[bunsh.config](./bunsh.config)* and *[bunsh.prod](./bunsh.prod)*, but we're going to start off using *bunsh.config*, because it's the default name of the config file that the script will attempt to look for in the current directory- meaning we don't have to provide any command line arguments if our current working directory contains a *bunsh.config* file.

Simply execute:

```bash
$ bunsh
# [ 2024-02-01 06:21:31 ] doo (v1.0.0-rc.1) => /Users/kamaranl/Git/bunsh/examples/dist/pre/doo
```

And that's it! Your source scripts have been bundled into an executeable script, with options already included. If we add the output directory to our PATH in the current shell, we can test its functionality:

```bash
$ PATH="$PATH:/Users/kamaranl/Git/bunsh/examples/dist/pre"

$ doo -v
# 1.0.0-rc.1

$ doo -h
# usage: doo [options]
# options:
#   core                {debug}
#   greet               -
#   -v,--version        print the current version of this executable
#   -h,--help           print this help message

$ doo greet
# Hello World
```

Some time later, after more tests and revisions, we're ready to create a production release using the *bunsh.prod* config file:

```bash
$ bunsh -c bunsh.prod
# [ 2024-02-01 06:22:27 ] doo (v1.0.0) => /Users/kamaranl/Git/bunsh/examples/dist/doo
```

Assuming that we're in a new shell, we can perform the same steps as before to test our newly bundled script:

```bash
$ PATH="$PATH:/Users/kamaranl/Git/bunsh/examples/dist"

$ doo -v
# 1.0.0

$ doo core
# no arguments provided
# print help with doo core --help

$ doo core ? # '?' will return the same result as -h,--help, if help is available
# usage: doo core [options]
# options:
#   debug               -
#   --help,-h           print this help message

$ doo core debug ?
# usage: doo core debug <message>

$ doo core debug "this is your debug message"
# ### debug ### this is your debug message
```

## Command Line Options

Our bundled script is working great, but say now we have a workflow that needs to update the production bundled version without having to develop a process to read and write to the config file. We can easily accomplish this with `--opt`:

```bash
$ bunsh -c bunsh.prod --opt s.version=1.1.0
# [ 2024-02-01 06:25:26 ] doo (v1.1.0) => /Users/kamaranl/Git/bunsh/examples/dist/doo

# and with the output folder still in our PATH we can still call our bundled script directly
$ doo -v
# 1.1.0
```

In the off chance you prefer not to utilize a config file, bunsh can operate solely off the available command line options:

```bash
$ bunsh -s src -o dist -n doo-lib --opt s.libonly=true
# [ 2024-02-01 06:27:14 ] doo-lib (v1.0.0-rc.1) => /Users/kamaranl/Git/bunsh/examples/dist/doo-lib
```

Since this is a library-only bundle[^1], it won't be executable, but we can (dot) source it in other scripts, our `.bash_profile`, or even the terminal:

```bash
$ . /Users/kamaranl/Git/bunsh/examples/dist/doo-lib

$ greet ?
# usage: greet [options] <name>
# options:
#   -u,--upper          convert the entire greeting to uppercase
#   -l,--lower          convert the entire greeting to lowercase

$ greet Kam
# Hello Kam

$ greet -u Kam && greet -l Kam
# HELLO KAM
# hello kam

$ core.debug "another debug message"
### debug ### another debug message
```

[^1]: When using the `s.libonly` opt, be sure to *either* prefix variable declarations with [`local`](https://www.gnu.org/software/bash/manual/bash.html#index-local), or [`unset`](https://www.gnu.org/software/bash/manual/bash.html#index-unset) the variables declared prior to exit/return to prevent unintentional reassignment of globally declared variables- unless your intentions are to do just that.
