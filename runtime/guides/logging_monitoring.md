# Logging and Monitoring

The `wsk` CLI offers a number of tools you can use to debug your actions while running them on Adobe I/O Runtime.

You can retrieve the latest activations in a namespace by running:
```
wsk activation list
```
You can also poll for activations:
```
wsk activation poll
```
As soon as an action is executed, you'll see an entry in the activation poll window.

If you send data to logs from your actions (using `console.log()` in your code), you&rsquo;ll get this information as part of the activation record, inside the `logs` field. The shortcut command to get the logs is:
```
wsk activation logs <activation ID>
//output sample
2018-11-14T22:23:00.002Z       stdout: 1542234180001: param = John Doe
```

If you want to debug locally, you can always run your action code using Node.js. If you prefer to run the action as close as possible to how it is executed by AdobeI/O Runtime, then James Thomas of IBM has a good [tutorial](https://medium.com/openwhisk/debugging-node-js-openwhisk-actions-3da3303e6741). Just remember to use the same Node.js version as the Adobe I/O Runtime uses.