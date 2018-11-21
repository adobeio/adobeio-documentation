# Deploying your First Adobe I/O Runtime Function

Test your installation and configuration of the CLI by creating and deploying your first function. Create the following function in any editor and save it as `hello.js`:

```js
function main(params) {
  return { payload: 'Hello ' + params.name };
}
```

> Note: This is similar to the function used as an example in [How Adobe I/O Runtime Works](../about/howitworks.md 'How it works'), but takes a name as a parameter.

Next, open a command-line window and navigate to the folder where you saved the function. Type the following command to create an action from this function:

`wsk action create hello hello.js`

This command uploads the code contained in `hello.js` and stores it in Runtime as an action named `hello`. That&rsquo;s really all there is to it: your function is deployed. If the command is successful, you should see the following acknowledgement in the command-line window:

`ok: created action hello`

Now, to test the function, invoke it as an action:

`wsk action invoke --result hello --param name <your name>`

You should get this output:

```json
{
  "payload": "Hello <your name>"
}
```

That&rsquo;s it! You&rsquo;re ready to begin developing on Adobe I/O Runtime. For ideas on what to next, see [Adobe I/O Runtime Quickstarts](quickstarts.md 'Quickstarts').
