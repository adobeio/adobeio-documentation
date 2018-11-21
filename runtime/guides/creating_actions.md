# Creating Actions

On this page:
- [Invoking actions](#invokingactions)
- [Working with parameters](#workingwithparameters)
- [Setting default parameters](#settingdefaultparameters)
- [Invoking web actions](#invokingwebactions)
- [Deploying ZIP actions](#deployingzipactions)

For your code to execute as an *action* on Adobe I/O Runtime, your code has to comply with two rules:
- You either call your function *main* or you export the entry point as *main*. This is the function that will be executed when it is invoked.
- Your function accepts valid JSON objects as input and produces valid JSON objects as output, if needed

You have to configure two tools on your machine to create and invoke actions from your machine: **wsk CLI** and **wskdeploy CLI.** Refer to the [wsk CLI](../tools/wsk_install.md) page for how to install and configure it. 

Let&rsquo;s assume you have this function available on your machine:
```javascript
// this is saved in a file named first-function.js
function main(params) {
    var n = params.name || 'stranger';
    return {payload: 'Hello ' + params.name};
}

exports.main = main;
```

You can create an action called *test* using this command:
```
wsk action create test first-function.js
```
You can update an action at any time using the following command:
```
wsk action update test first-function.js
```
If you don&rsquo;t need an action anymore, you can delete it:
```
wsk action delete test
```

If you want to save an action that is deployed to your machine, then you can use this command:
```
wsk action get test --save 
```

Listing all the available actions in your current namespace is as simple as running this command:
```
wsk action list 
```

## Invoking actions

Now, that you have an action, you can call it using the following command (in this example the action name is *test*):

```
wsk action invoke test --result
```
Note the flag *--result* used in the command. This flag outputs the result of the invocation. Without it, instead of seeing the result of the invocation, you&rsquo;d get the activation ID. To get the result, you&rsquo;d use this ID to retrieve the result like this:
```
wsk activation get <activation ID>
```
When you invoke an action, as in the example above, the invocation is not blocking (it is async). If you want to execute it in a blocking style and, as a result, get the activation record instead of just getting an activation ID, you have to add the *--blocking* flag to the command:
```
wsk action invoke test --blocking
```

## Working with parameters

Actions can receive parameters when are being executed. First, any parameters you sent to the action will be available through the *params* variable. If you send two parameters called *first-name* and *last-name*, they will be available as *params.first-name* and *params.last-name*.

Second, let&rsquo;s see how you can invoke the action with parameters. Our function sample from above uses a parameter called *name*. This is how you can set the parameter when invoking the action:
```
wsk action invoke test --param name "John Doe" --result
```

## Setting default parameters

Sometimes you want to bind the same parameter values for all invocations or you just want to set default values. In either case, you have two different options.

### Default paramenters set on action

Let&rsquo;s assume that you want the default value of your parameter to be "Runtime". You can set this value when creating the action, or if the action already exists, updating the action. In both cases you add the *--param" flag:
```
// creation time
wsk action create test first-function.js --param name "Runtime"

// update
wsk action update test first-function.js --param name "Runtime"
```

Now, you can run the action without any parameters, and it will use the default one you&rsquo;ve set. If you set a parameter when invoking, this will overwrite the default one.

### Default paramenters set on package

When you create an action, it is always created in a package. If you don&rsquo;t specify a package, the *default* package is used. Similar to how you set default parameters at the action level, you can specify default parameters at the package level.

The difference is that the params set at the package level will be used for all the actions you create in that package.
```
// creation time
wsk package create my-package --param name "Runtime"

// update
wsk package update my-package --param name "Runtime"
```

At this point, you might ask yourself what is the precedence when setting parameters at the package level, action level, and invocation time. The winner is parameters set at invocation time, followed by parameters set at action level, and the last is parameters set at package level.

### Default paramenters with parameter file

A neat way for setting default params is using a dedicated file for storing their values, and then using this file to set those values as default parameter values.

This is especially useful when you are dealing with multiple parameter values for things like configuring API access keys, endpoints, and so on.

Coming back to our sample function that expects one parameter, *name*, you&rsquo;d create a JSON file to store that value:
```json
// filename is my-params.json
{
    "name": "Runtime"
}
```
Then, you use the *--param-file* flag when creating actions, creating packages, or invoking actions.
```
// update action
wsk action update test first-function.js --param-file my-params.json

// invoke action
wsk action invoke test --param-file my-params.json
```

## Invoking web actions

So far, we&rsquo;ve been invoking actions only from the CLI. While this might be good for trying out your actions, as you design actions for production systems, you might need to be able to invoke actions via HTTP REST calls. This would enable you to invoke the actions from your web application. 

You create a web action by adding the *--web* flag to the wsk action command:
```
// creation time
wsk action create test first-function.js --web true

// update
wsk action update test first-function.js --web true
```
Notice the *true* value used in the command. If you set that value to *false*, then you disable a web action.

To call the action as a web action, you need to know the full path to the action. You can find the path by adding the *--url* flag to the action command:
```
wsk action get test --url
```
This will give you something like:
```
ok: got action test
https://adobeioruntime.net/api/v1/web/[your namespace]/default/test
```
In the URL above, the `default` in the path stands for the `default` package: if you don&rsquo;t create your actions explicitly in a package, then they get under the `default` package.

You can invoke the action like this:
```
curl https://adobeioruntime.net/api/v1/web/[your namespace]/default/test -x GET
```

When creating actions to be used as web actions, you might want to send the response that follows the HTTP response structure (status code, headers, body). For example, our sample function could be rewritten:
```javascript
function main(params) {
    var n = params.name || 'stranger';
    return {
        statusCode: 200,
        body: {
            payload: 'Hello ' + params.name
        }
    }
}
```

You can also set cookies or cache control headers, perform a HTTP redirect, and so forth.

### Parameters

If a web action has default parameters defined, then those parameter values are final. They can&rsquo;t be overwritten at execution time.

Suppose that a web action has a default parameter called *name* with the value *Runtime*: 
```
wsk action update test first-function.js --web true --param name "Runtime"
```

When you invoke this web action, the value of *param.name* will always be *Runtime*. You can&rsquo;t overwrite by using a body or query parameter. 

### HTTP context

When executing an action as a web action, the `params` object is decorated with additional information:

| Method | Description |
|---|---|
| `__ow_method` | the HTTP method of the request |
| `__ow_headers` | the request headers |
| `__ow_path` | the unmatched path of the request (matching stops after consuming the action extension) |
| `__ow_body` | the request body entity, as a base64-encoded string when its content is binary or JSON object/array, or as a plain string otherwise |
| `__ow_query` | the query parameters from the request as an unparsed string |

By default, web actions are accessible to anyone who knows the URL. If you want to secure the access, you can find more info on the [Securing Web Actions](securing_web_actions.md) page.

## Deploying ZIP actions

So far, we&rsquo;ve been creating actions from a single source file. What if you need to create an action from multiple files and you also need some **npm** modules? The answer is deploying a ZIP action. 

You need to do three things to deploy a ZIP action: create a manifest file, create the package.json file, and configure/use `wskdeploy` instead of `wsk` for the actual deploy.

### wskdeploy

 If you need to install `wskdeploy`, check the page [Setting up the wskdeploy CLI](../tools/wskdeploy_insall.md).

### Package.json file

You use the package.json file to specify the dependencies. If you don&rsquo;t have one already, in the same folder where your function files are, run `npm init -y`.

Make sure you have the npm dependencies you need for your functions declared in the package file.

### wskdeploy manifest file

The last step is creating the manifest file that will be used by wskdeploy to create your action. In the parent folder of the folder where you have your function files and npm modules, create a file `manifest.yaml`. (For more information on YAML format, see the [YAML reference card](http://yaml.org/refcard.html).)

This is the folder structure:

```
--/actions
--/actions/node_modules/
--/actions/first-function.js
--/actions/package.json
--manifest.yaml
```

Here is an example manifest.yaml:

```
packages:
    # this is the package name
    test:
        actions:
            # name of the action
            test-zip:
                # source for the action; in this case it is a folder
                function: actions
                runtime: nodejs:8
```

Now you are ready to deploy by running the `wskdeploy` command from the same folder where `manifest.yaml` is:

```
wskdeploy --apihost controller-a-rtbeta-ue1-b.adobe-runtime.com
```

This should deploy an action called *test-zip* under a package called *test*. You can invoke it like this:

```
wsk action invoke test/test-zip --result
```

If you want to remove this action, you run `wskdeploy` with the *undeploy* flag:
```
wskdeploy --apihost controller-a-rtbeta-ue1-b.adobe-runtime.com undeploy
```

>**Note:** There is another way of deploying a ZIP action using the wsk command. You miss the manifest.yaml file flexibility with this mode; you can&rsquo;t define multiple actions/packages at the same time. The ZIP file has to have in the root the package.json file.  
`wsk action create my-action --kind nodejs:10 zip-file.zip`
