# Examples and guides

## Embedding your first widget

<span class="todo">Coming soon</span>

## Login with Widgetic

> Using interactive login

<iframe class="live-example" src="examples/login.html" scrolling="no"></iframe>

```html
<button>Login with Widgetic!</button>
<p style="display:none"> Hello <span>visitor</span>! </p>
```

```js
// initialize the sdk with your api key
Widgetic.init('554de62709c7e23f7f8b4567_4bgmtqx250aoookowwook40gk0408s4gs8w0o4o8o0cc4cscc8', 'http://docs.widgetic.com/examples/proxy.html');

var hideButton = function() {
  $('button').off('click').hide();
}

var showUsername = function() {
  $('p').show();
  Widgetic.api('users/me').then(function(user) {
    $('span').text(user.username);
  })
}

var showError = function() {
  alert('Login failed');
}

$('button').on('click', function() {
  // do the interactive login
  Widgetic.auth()
    .then(hideButton)
    .then(showUsername)
    .fail(showError)
})
```

To start working with any REST Api, one of the first steps is always authentication. Widgdetic uses the OAuth 2.0 protocol for authentication and authorization. Widgetic supports common OAuth 2.0 scenarios such as those for web server and client-side applications.

The Widgetic.js SDK contains helper functions for handling the OAuth flow. To begin, obtain OAuth 2.0 client credentials from the [Widgetic Developers Area](https://widgetic.com/developers/). The SDK needs to be initialised with your API Key.

Calling `Widgetic.auth` when the user interacts with your app starts the *interactive* login process, using popups. The user will be greeted by your apps authorization screen. They can choose to authorize or deny access to their Widgetic account. Like most of the Widgetic SDK functions, `Widgetic.auth` returns a *promise*. Using the `then` and `fail` methods you can subscribe callbacks to be run when the user authorizes or denies authorization, respectively.

![Widgetic Auth Screen](images/auth.png)

---

> Using non-interactive login to reconnect the user

<iframe class="live-example" src="examples/login-background.html" scrolling="no"></iframe>

```js
var showButton = function() {
  $('button').show().on('click', function() {
    Widgetic.auth()
      .then(hideButton)
      .then(showUsername)
      .fail(showError)
  });  
}

// do the background login
Widgetic.auth(false)
  .then(hideButton)
  .then(showUsername)
  .fail(showButton)

```

The other form of the `Widgetic.auth` function, invoked using the `false` parameter, will do a background non-interactive login, which succeeds only if the user has previously authorized your app and they are still logged into the Widgetic platform.

You can use this form to reconnect to a users account upon page refresh.

<aside class="info">To retry the authentication steps in the examples, remove the authorization for the Widgetic Documentation app from the <a target="_blank" href="https://widgetic.com/account/settings">settings</a> page.</aside>

## Using the REST API to create a composition

## White-label integration

<span class="todo">In progress</span>

A white-label product or service is a product or service produced by one company (the producer) that other companies (the marketers) rebrand to make it appear as if they had made it.

Some websites use white labels to enable a successful brand to offer a service without having to invest in creating the technology and infrastructure itself. Many IT and modern marketing companies outsource or use white-label companies[3] and services to provide specialist services without having to invest in developing their own product.

Using the [managed users](#managed-users) api, you can
