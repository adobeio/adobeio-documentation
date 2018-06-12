# Adobe I/O Command Line Interface (CL): console

This module allows end users to list integrations created in the Adobe I/O Console GUI, and to select an integration to create .wskprops files for use with I/O Runtime. 

# Commands
## `aio console`

List or select console integrations for the Adobe I/O Runtime

```
USAGE
  $ aio console

EXAMPLES
  $ aio console:list-integrations
  $ aio console:ls
  $ aio console:select-integration INTEGRATION_ID
  $ aio console:sel INTEGRATION_ID
```

_See code: [@adobeio/aio-cli-plugin-console](https://github.com/io-sdk/aio-cli-plugin-console/blob/v1.0.1-0/src/commands/console/index.js)_

## `aio console:list-integrations`

lists integrations for use with runtime serverless functions

```
USAGE
  $ aio console:list-integrations

OPTIONS
  -p, --page=page          page number
  -s, --pageSize=pageSize  size of a page

ALIASES
  $ aio console:ls
```

_See code: [@adobeio/aio-cli-plugin-console](https://github.com/io-sdk/aio-cli-plugin-console/blob/v1.0.1-0/src/commands/console/list-integrations.js)_

## `aio console:select-integration [INTEGRATIONID]`

selects an integration and writes the .wskprops file to the local machine

```
USAGE
  $ aio console:select-integration [INTEGRATIONID]

ALIASES
  $ aio console:sel
```

_See code: [@adobeio/aio-cli-plugin-console](https://github.com/io-sdk/aio-cli-plugin-console/blob/v1.0.1-0/src/commands/console/select-integration.js)_