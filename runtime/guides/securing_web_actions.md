# Securing Web Actions

By default, a web action can be invoked by anyone knowing the action&rsquo;s URL. If you want to restrict the access, you either use Basic Authorization or you build your own authorization layer.

Here is how you can enable Basic Authorization for a web action:
```
// create a web action with Basic Authorization on
wsk action create my-secure-web-action main.js --web true --web-secure this-is-the-secret-hash
```
or
```
// update an existing web action to enable Basic Authorization or change the secret
wsk action update my-secure-web-action main.js --web true --web-secure this-is-the-secret-hash
```

Once you&rsquo;ve enabled Basic Authorization, you&rsquo;ll have to pass *X-Require-Wishk-Auth* header, and the secret you set, when invoking the web action. Assuming that your web action is created in the default package, this is how you&rsquo;ll invoke it:
```
curl https://adobeioruntime.net/api/v1/web/[your-namespaces]]/default/my-secure-web-action -X GET -H "X-Require-Whisk-Auth: this-is-the-secret-hash"
```

If you fail in adding the authorization header or the secret is wrong, you will get an error:
```
{
  "error": "Authentication is possible but has failed or not yet been provided.",
  "code": "OWGYkWwCUT7Ta6hWpfZWTQqRsfvcFTku"
}
```