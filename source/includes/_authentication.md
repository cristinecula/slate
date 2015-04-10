# Getting started

Widgetic uses API keys to allow access to the API. You can register a new API key at our [developer portal](https://widgetic.com/developers). 

```js
// initialize Widgetic with your api key and a redirect uri
Widgetic.init('<api key>','<redirect uri>');
```

<p class="code-annotation javascript">
To use the Widgetic.js SDK you must first initialize it with your API key and redirect URI. The API key is provided on the developer portal. The redirect URI must point to a webpage on your domain that includes the widgetic.js sdk. This could be your homepage or an empty html page created for this purpose.
</p>

## Authentication and authorization

You can use Widgetic.js to connect your users to their Widgetic accounts. This allows you to access and manage their widgets and customizations, plus other features available through the API. The authentication and authorization protocol is based on OAuth2.

### Interactive login 

```js
$('button').on('click', function(){
    // do the interactive (popup) login
    Widgetic.auth().then(
        function(successResponse) {
            // the user has authorized your app
            console.log(
                successResponse.accessToken
            );
        }, function(failResponse) {
            // the user has refused authorization or has closed the popup window
            console.log(failResponse); 
        }
    );
}) 
```

> <p class="code-annotation javascript">Example success response:</p>

```js
successResponse = {
    status: "connected",
    accessToken: "MjVkOWVlOTJmYzZmNDZmNTJkZDQwMmFkZmIzYTA3YTUyYTg1Mjk2NWU3MWZiNzBjMzFiYmI3M2Y2YTEwMmVjYQ",
    expiresIn: "3600",
    scope: ""
}
```

The Javascript SDK has the `Widgetic.auth` function that initiates and handles the auth process. Calling `Widgetic.auth()` opens up a login popup where the user can log into (or set up) their Widgetic account and authorize your app. The return value of this function is a Promise object with the `then` and `fail` methods. You have to pass callbacks to these functions to get the success or fail response. 

<aside class="notice">The SDK uses the <a href="https://github.com/cburgmer/ayepromise">ayepromise</a> library for it's Promise implementation.</aside>

---

### Non-Interactive login

```js
Widgetic.auth(false).then(
    function(successResponse) {
        // the user is logged in and has authorized your app
        console.log(
            successResponse.accessToken
        );
    }, function(failResponse) {
        // the user is not logged in or the app is not authorized
        console.log(failResponse); 
    }
);
```

If you pass `true` to `Widgetic.auth`, the SDK will try a 'non-interactive login' which won't open any popups. This is useful for checking if the user has already authorized your app and reconnecting to the Widgetic API upon navigation. If the user is logged into Widgetic and has authorized your app in the past, the login process will succeed. Otherwise it will fail and you should prompt the user to do an 'interactive' login.

---

### Scopes

<aside class="warning">Documentation incomplete.</aside>

```js
Widgetic.auth(true, ['email', 'skins']);
```

Available scopes:

* email
* skins
* compositions

## API calls

<aside class="warning">Documentation incomplete.</aside>

```js
Widgetic.api('users/me').then(function(user) {
    console.log('Hello ' + user.username);
})
```

To communicate with the REST API you use the `Widgetic.api` function.