# Notes on General Security

## Environmental Variables

### UNIX

Used [this article](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-environment-variables-in-linux/) as reference

* To set: `export NAME=VALUE` (ie. `export JAVA_HOME=/opt/openjdk11`)
* To view: `echo $JAVA_HOME`
* To unset: `unset VARIABLE_NAME` (ie. `unset JAVA_HOME`)
* To list all variables: `set`

TODO: Include setting for user using .bashrc, .env etc (and explaining what actually is .bashrc)

### Python

Using the `os` library. Note `os.environ` doesn't overwrite variables system-wide - just in the specific process. In order to permanently delete, you need a shell environment (like bash)

* To list all variables: `print(os.environ)`
* To get value: `os.environ.get("USER")` - note this returns `None` if there's no match
* To set: `os.environ["USER"] = "Bob"`
* To unset:
    * Single variable: `os.environ.pop('USER')`
    * All variables: `os.environ.clear()`

TODO: Look into usage of `dotenv` for managing multiple keys using a single file
