# Adobe I/O Command Line Interface (CLI): config

This module allows end users to set up and manage configuration values for usage of the Adobe I/O CLI. 

# Commands
## `aio help [COMMAND]`

display help for aio

```
USAGE
  $ aio help [COMMAND]

ARGUMENTS
  COMMAND  command to show help for

OPTIONS
  --all  see all commands in CLI
```

_See code: [@oclif/plugin-help](https://github.com/oclif/plugin-help/blob/v1.2.11/src/commands/help.ts)_

## `aio config`

get, set, delete, and clear persistent configuration data

```
USAGE
  $ aio config

EXAMPLES
  $ aio config:get KEY
  $ aio config:set KEY VALUE
  $ aio config:delete KEY
  $ aio config:del KEY
  $ aio config:clear
```

_See code: [@adobeio/aio-cli-plugin-config](https://github.com/io-sdk/aio-cli-plugin-config/blob/v1.0.1-2/src/commands/config/index.js)_

## `aio config:clear`

clears all persistent config values

```
USAGE
  $ aio config:clear
```

_See code: [@adobeio/aio-cli-plugin-config](https://github.com/io-sdk/aio-cli-plugin-config/blob/v1.0.1-2/src/commands/config/clear.js)_

## `aio config:delete [KEY]`

delete a persistent config value

```
USAGE
  $ aio config:delete [KEY]

ALIASES
  $ aio config:del
```

_See code: [@adobeio/aio-cli-plugin-config](https://github.com/io-sdk/aio-cli-plugin-config/blob/v1.0.1-2/src/commands/config/delete.js)_

## `aio config:get [KEY]`

gets a persistent config value

```
USAGE
  $ aio config:get [KEY]
```

_See code: [@adobeio/aio-cli-plugin-config](https://github.com/io-sdk/aio-cli-plugin-config/blob/v1.0.1-2/src/commands/config/get.js)_

## `aio config:set [KEY] [VALUE]`

sets a persistent configuration value

```
USAGE
  $ aio config:set [KEY] [VALUE]

OPTIONS
  -f, --file                 the key is a path to a file to read config value from

  -t, --mime-type=mime-type  the mime-type of the file path with --file/-f (defaults to plain text, available:
                             application/json)
```

_See code: [@adobeio/aio-cli-plugin-config](https://github.com/io-sdk/aio-cli-plugin-config/blob/v1.0.1-2/src/commands/config/set.js)_