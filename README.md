# lita-cmd

Run scripts from your lita machine and get the output back.

## Install

Add lita-cmd to your Lita instance's Gemfile:

``` ruby
gem "lita-cmd"
```

## Configuration

| Config Option  | Description                          | Type   | Notes    |
|----------------|--------------------------------------|--------|----------|
|`scripts_dir`   |Full path to location of scripts      |`String`|*required*|
|`stdout_prefix` |Prefix for text returned to STDOUT    |`String`|          |
|`stderr_prefix` |Prefix for text returned to STDERR    |`String`|          |
|`output_format` |Format string used to encapsulate code|`String`|          |
|`command_prefix`|Command to use for executing scripts  |`String`|          |
|`ignore_script` |regexp of non-script files to ignore  |`Regexp`|          |

### Example

```ruby
Lita.configure do |config|
  # Lita CMD - required parameters
  config.handlers.cmd.scripts_dir = "/path/to/dir/you/expose"

  # Lita CMD - optional parameters

  # Set the output format. Default: "%s"
  # Note that %s will contain the returned text
  config.handlers.cmd.output_format = "/code %s"

  # Set the prefix of stdout and stderr.
  config.handlers.cmd.stdout_prefix = ""
  config.handlers.cmd.stderr_prefix = "ERROR: "

  # Set the prefix for running scripts.
  config.handlers.cmd.command_prefix = "run "

  # Set the characters that, if present, will cause a file to not be flagged as a script
  config.handlers.cmd.ignore_script = Regexp.union(/~/, /#/)

end
```

## Usage

In your chatroom, try one of these commands

### `lita cmd list`

Query the configured directory for filenames and return the list

### `lita cmd <file>`

Execute a file in the configured directory

### `lita cmd <file> "option with spaces"`

Scripts can be passed in options that contain spaces by surrounding with double quotes


## Group Control

You can control what groups have access to your Lita scripts.

In your `scripts` directory make a sub directory named after each of your
groups. Only users that belong to these groups can list and execute the
scripts inside them.

This is the basic directory structure

```
scripts/
  |- devops/
  |  - secret_script
  |- script1
  |- script2
```

When you run `lita cmd list` you will only see scripts that you have access
to. For example:

```
me:   lita cmd list

lita: devops/secret_script
      script1
      script2

me:   lita cmd devops/secret_script

lita: Executing the secret script
```

## Notes

- The user name of the calling user will be saved in an environment variable
  called `LITA_USER`.
- Make sure that your files are executables (`chmod +x FILE`)
- Make sure that your files have the proper sheband (`#!/bin/bash`)

## Todo

- [x] Include support for directory-based access control
- [ ] Help text for individual commands
- [ ] Add tests
