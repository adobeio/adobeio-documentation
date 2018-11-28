# Setting up Your Environment

Before you can create and run actions, you have to install and configure a couple of tools on your machine. Please note that for some of the steps (creating integrations in I/O Console) you need to have System Administrator or Developer Role permissions. If you don&rsquo;t have those, you need either to be provisioned with these permissions or someone from your team has to share the credentials.

## Step 1: Install aio CLI and wsk CLI

You need `npm` installed in order to install `aio`:
```
npm install -g @adobe/aio-cli
```

For the `wsk` CLI, download the executable from the [OpenWhisk GitHub repository](https://github.com/apache/incubator-openwhisk-cli/releases). Choose the version that matches your operating system and download the compressed archive.

Extract the executable from the compressed archive and place it in a folder of your choice.

Add the folder into which you placed the executable to your `$PATH` environment variable. This enables you to call the CLI from anywhere.

Try to run these commands to check that `aio` and `wsk` were properly installed:
```
wsk -h
```
and:
```
aio -h
```

## Step 2: Create an integration for I/O Management API

To create the credentials, you have to go to [Adobe I/O Console](https://console.adobe.io) and create an integration that is configured to access the **I/O Management API.** Note that you need System Administrator or Developer Role permissions to create this integration.

## Step 3: Configure the CLI

After you&rsquo;ve created the integration,  create a `config.json` file on your computer and navigate to the integration Overview page. From this page, copy the `client_id` and `client_secret` values to the config file; if you navigate to the JWT tab in Console, you&rsquo;ll get the value for the `jwt_payload`.

```
//config.json 
{
  "client_id": "value from your CLI integration (String)",
  "client_secret": "value from your CLI integration (String)",
  "jwt_payload": { value from your CLI integration (JSON Object Literal) },
  "token_exchange_url": "https://ims-na1.adobelogin.com/ims/exchange/jwt",
  "console_get_orgs_url":"https://api.adobe.io/console/organizations",
  "console_get_namespaces_url":"https://api.adobe.io/runtime/admin/namespaces/"
}
```

The last bit you need to have at hand is the private certificate you&rsquo;ve used to create the integration; you need the private key, not the public one. Now, you are ready to configure the `aio` CLI.

First, configure the credentials:

```
aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --mime-type=application/json
```

Then, configure the private certificate:

```
aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file --mime-type=application/x-pem-file
```

To test that aio CLI can actually authenticate against Adobe I/O, run this command to list all the integrations that your organization has:

```
aio console:list-integrations
```

## Step 4: Set the Adobe I/O Runtime namespace

Now you can use the aio CLI to select the namespace you want to use for deploying actions. Any integration you&rsquo;ve created can be used as a namespace; there is a 1:1 relationship between an integration and a namespace. 

You can&rsquo;t manage multiple namespaces at the same time. You can only have just one namespace active always, and all the actions you perform are against the active namespace.

To list the available integrations, run:

```
aio console:list-integrations
Success: Page 1 of 1, Showing 2 results of 2 results.
<NUMBER_NUMBER> : <Integration Name 1>
<NUMBER_NUMBER> : <Integration Name 2>
```

If you want to select an integration as the current namespace, run this command and pass as the argument the <NUMBER_NUMBER> you see listed when running `aio console:list-integrations`:

```
aio console:select-integration <NUMBER_NUMBER>
```

This command will generate the `.wskprops` file in your user directory (or overwrite the file if it already exists) for accessing that given namespace. Now, you can use the `wsk` CLI to manage this namespace: create/invoke actions and so forth.

## Step 5: Test wsk CLI

Once you&rsquo;ve configured the CLI, you should test it:
```
wsk list
``` 
If successful, you should see a list of the entities defined in your namespace.

You&rsquo;re ready to [deploy your first function](deploy.md).