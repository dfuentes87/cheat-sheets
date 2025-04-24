# Bash

## General

- Prepend a command with `\` to override alias/builtin lookup.

## Scripting

- The principles of [Clean Code](https://www.goodreads.com/book/show/3735293-clean-code) apply to Bash as well.
- Always use long parameter notation in scripts. This makes them more readable, especially for lesser known/used commands that you or someone else don't remember all the options for.

    ```bash
    # Avoid:
    rm -rf -- "${dir}"

    # Good:
    rm --recursive --force -- "${dir}"
    ```

### Variables

- Prefer local variables within functions over global variables
- If you need global variables, make them readonly
- Capitalization:
  - Environment (exported) variables: `${ALL_CAPS}`
  - Local variables: `${lower_case}`
- Positional parameters of the script should be checked, those of functions should not

### Colors

Don't output ANSI colour codes directly - your output could redirect to a file, or perhaps the user simply prefers no colour. Use tput instead, and add a little snippet like this to the top of your script:

`command -v tput &>/dev/null && [ -t 1 ] && [ -z "${NO_COLOR:-}" ] || tput() { true; }`

This checks that the tput command exists (more reliable than `which`), that stdout is a tty, and that the NO_COLOR env var is not set. If any of these conditions are false, a no-op tput function is defined.

Summarized from [HckerNews Post](https://news.ycombinator.com/item?id=41536036)

#### Basics

```plaintext
tput sgr0
normal=$(tput sgr0)
bold=$(tput bold) # was: Bright
blink=$(tput blink)
reverse=$(tput smso)
underline=$(tput smul)
```

#### Foreground colors

```plaintext
black=$(tput setaf 0)        #000000
maroon=$(tput setaf 1)       #800000
green=$(tput setaf 2)        #008000
olive=$(tput setaf 3)        #808000
navy=$(tput setaf 4)         #000080
purple=$(tput setaf 5)       #800080
teal=$(tput setaf 6)         #008080
silver=$(tput setaf 7)       #c0c0c0
gray=$(tput setaf 8)         #808080
red=$(tput setaf 9)          #ff0000
lime=$(tput setaf 10)        #00ff00
yellow=$(tput setaf 11)      #ffff00
blue=$(tput setaf 12)        #0000ff
magenta=$(tput setaf 13)     #ff00ff
aqua=$(tput setaf 14)        #00ffff
white=$(tput setaf 15)       #ffffff
```

#### Background colors

```plaintext
black_bg=$(tput setab 0)     #000000
maroon_bg=$(tput setab 1)    #800000
green_bg=$(tput setab 2)     #008000
olive_bg=$(tput setab 3)     #808000
navy_bg=$(tput setab 4)      #000080
purple_bg=$(tput setab 5)    #800080
teal_bg=$(tput setab 6)      #008080
silver_bg=$(tput setab 7)    #c0c0c0
gray_bg=$(tput setab 8)      #808080
red_bg=$(tput setab 9)       #ff0000
lime_bg=$(tput setab 10)     #00ff00
yellow_bg=$(tput setab 11)   #ffff00
blue_bg=$(tput setab 12)     #0000ff
magenta_bg=$(tput setab 13)  #ff00ff
aqua_bg=$(tput setab 14)     #00ffff
white_bg=$(tput setab 15)    #ffffff
```

### Output and redirection

- [For various reasons](https://www.in-ulm.de/~mascheck/various/echo+printf/), `printf` is preferable to `echo`. `printf` gives more control over the output, it's more portable and its behaviour is defined better.
- Print error messages on stderr, e.g., use the following function:

    ```bash
    error() {
      printf "${red}!!! %s${reset}\\n" "${*}" 1>&2
    }
    ```

- Name heredoc tags with what they’re part of, such as:

    ```bash
    cat <<HELPMSG
    usage $0 [OPTION]... [ARGUMENT]...

    HELPMSG
    ```

- Single-quote heredocs leading tag to prevent interpolation of text between them.

    ```bash
    cat <<'MSG'
    [...]
    MSG
    ```

- When combining a `sudo` command with redirection, it's important to realize that the root permissions only apply to the command, not to the part after the redirection operator. An example where a script needs to write to a file that's only writeable as root:

    ```bash
    # this won't work:
    sudo printf "..." > /root/some_file

    # this will:
    printf "..." | sudo tee /root/some_file > /dev/null
    ```

### Functions

Bash can be hard to read and interpret. Using functions can greatly improve readability. Principles from Clean Code apply here.

- Apply the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle): a function does one thing.
- [Don't mix levels of abstraction](https://www.sivalabs.in/clean-code-dont-mix-different-levels-of-abstractions/)
- Describe the usage of each function: number of arguments, return value, output
- Declare variables with a meaningful name for positional parameters of functions

    ```bash
    foo() {
      local first_arg="${1}"
      local second_arg="${2}"
      [...]
    }
    ```

- Create functions with a meaningful name for complex tests

    ```bash
    # Don't do this
    if [ "$#" -ge "1" ] && [ "$1" = '-h' ] || [ "$1" = '--help' ] || [ "$1" = "-?" ]; then
      usage
      exit 0
    fi

    # Do this
    help_wanted() {
      [ "$#" -ge "1" ] && [ "$1" = '-h' ] || [ "$1" = '--help' ] || [ "$1" = "-?" ]
    }

    if help_wanted "$@"; then
      usage
      exit 0
    fi
    ```

### Cleanup code

An idiom for tasks that need to be done before the script ends (e.g. removing temporary files, etc.). The exit status of the script is the status of the last statement *before* the `finish` function.

```bash
finish() {
  result=$?
  # Your cleanup code here
  exit ${result}
}
trap finish EXIT ERR
```

Source: Aaron Maxwell, [How "Exit Traps" can make your Bash scripts way more robust and reliable](http://redsymbol.net/articles/bash-exit-traps/).

### Writing robust scripts and debugging

Bash is not very easy to debug. There's no built-in debugger like you have with other programming languages. By default, undefined variables are interpreted as empty strings, which can cause problems further down the line. A few tips that may help:

- Always check for syntax errors by running the script with `bash -n myscript.sh`
- Use [ShellCheck](https://www.shellcheck.net/) and fix all warnings. This is a static code analyzer that can find a lot of common bugs in shell scripts. Integrate ShellCheck in your text editor (e.g. Syntastic plugin in Vim)
- Abort the script on errors and undbound variables. Put the following code at the beginning of each script.

    ```bash
    set -o errexit   # abort on nonzero exitstatus
    set -o nounset   # abort on unbound variable
    set -o pipefail  # don't hide errors within pipes
    ```

    A shorter version is shown below, but writing it out makes the script more readable.

    ```bash
    set -euo pipefail
    ```

- Use Bash's debug output feature. This will print each statement after applying all forms of substitution (parameter/command substitution, brace expansion, globbing, etc.)
  - Run the script with `bash -x myscript.sh`
  - Put `set -x` at the top of the script
  - If you only want debug output in a specific section of the script, put `set -x` before and `set +x` after the section.
