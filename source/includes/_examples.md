# Examples and guides

## Embedding your first widget

Widgetic allows multiple embed options. We recommend using the **anchor + sdk** method, as it is really simple and allows use of advanced features (out-of-widget popups and overlays). 

### anchor + sdk

> anchor + sdk embed

```html
<script type="text/javascript" src="https://widgetic.com/sdk/sdk.js"></script>

<a href="https://widgetic.com/widgets/photo/mosaic-gallery/" class="widgetic-composition" data-id="555b5e6509c7e29e718b4569" data-width="500" data-height="500" data-resize="allow-scale-down" data-brand="bottom-right">Mosaic Gallery Widget</a>
```

The default embed mechanism. Recommended for simple use-cases. It allows multiple auto-scaling options, specifying default dimensions, and control of the branding position. 

The embed code is featured on the composition's page. Just copy and paste the code to the desired location on the website and the Widgetic SDK will transform the anchor tag into the actual widget.

### iframe

> iframe embed

```html
<iframe src="https://widgetic.com/embed/555b5e6509c7e29e718b4569?bp=top-right" width="500" height="500" frameborder="0" style="visibility:hidden;" onload="this.style.visibility='visible';"></iframe>
```

The most basic embed mechanism. The iframe embed does not allow auto-scaling  and advanced widget features (e.g.: popups and overlays). It is only recommended for websites where you cannot include the Widgetic SDK. 

### dynamic

> dynamic embed

```js
Widgetic.UI.composition(document.querySelector('.embed'), composition)
```

The most advanced of the embed methods. Allows embedding of compositions, but also dynamic creation of compositions with skin and content defined at runtime. Useful for content management systems, websites built using a templating language, etc.

## Login with Widgetic

> Using interactive login

<iframe class="live-example" data-src="examples/login.html" scrolling="no"></iframe>

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
  console.log('Login failed');
}

$('button').on('click', function() {
  // do the interactive login
  Widgetic.auth()
    .then(hideButton)
    .then(showUsername)
    .fail(showError)
})
```

To start working with any REST API, one of the first steps is always authentication. Widgetic uses the OAuth 2.0 protocol for authentication and authorization. Thus, most API endpoints require an *access token* to allow interaction. Widgetic supports common OAuth 2.0 scenarios such as those for web server and client-side applications.

The Widgetic.js SDK contains helper functions for handling the OAuth flow and retrieving a valid *access token*. To begin, obtain OAuth 2.0 client credentials from the [Widgetic Developers Area](https://widgetic.com/developers/). The SDK needs to be initialised with your API Key.

Calling `Widgetic.auth` when the user interacts with your app starts the *interactive* login process, using popups. The user will be greeted by your apps authorization screen. They can choose to authorize or deny access to their Widgetic account. Like most of the Widgetic SDK functions, `Widgetic.auth` returns a *promise*. Using the `then` and `fail` methods you can subscribe callbacks to be run when the user authorizes or denies authorization, respectively.

![Widgetic Auth Screen](images/auth.png)

---

> Using non-interactive login to reconnect the user

<iframe class="live-example" data-src="examples/login-background.html" scrolling="no"></iframe>

```js
var doInteractiveAuth = function() {
  var def = Widgetic.Aye.defer()

  console.log('Background login failed; reverting to interactive login');

  $('button').show().off('click').on('click', function() {
    Widgetic.auth()
      .then(def.resolve)
      .fail(def.reject)
  });

  return def.promise;
}

function init() {
  Widgetic.auth(false)
    .fail(doInteractiveAuth)  
    .then(hideButton)
    .then(showUsername)
    .fail(init)
}

init()
```

The other form of the `Widgetic.auth` function, invoked using the `false` parameter, will do a background non-interactive login, which succeeds only if the user has previously authorized your app and they are still logged into the Widgetic platform.

You can use this form to reconnect to a users account upon page refresh.

<aside class="info">To retry the authentication steps in the examples, remove the authorization for the Widgetic Documentation app from the <a target="_blank" href="https://widgetic.com/account/settings">settings</a> page.</aside>

## Using the REST API to create a composition

> Listing the available widgets

```js
Widgetic.api('widgets', {fields: ['id, name']})
  .then(function(widgets) {
    console.log(widgets);
  });
```

<iframe class="live-example" data-src="examples/rest-composition-widgets.html" scrolling="no"></iframe>

In this tutorial we'll create a composition using the API.

The first step is to decide on a widget to use. We'll call the `Widgetic.api` method to interact with the `/widgets` endpoint and get the list of widget names. 

<aside class="info">Most API calls require a valid access token. The `widgets` endpoint is one of the few that work anonymously (without an access token). Be sure to follow the previous tutorial on <a href="#login-with-widgetic">authentication</a> to enable user login.</aside>

---

> Listing the Mosaic Gallery's skins

```js
var getMosaicGallery = function() {
  return Widgetic.api('widgets', {fields: ['id, name']})
    .then(function(widgets) {
      // find the mosaic gallery
      return widgets.filter(function(widget) {
        return widget.name === 'Mosaic Gallery'
      })[0]
    })
    .then(function(mosaicGallery) {
      console.log('The id of the Mosaic Gallery Widget:');
      console.log(mosaicGallery.id);

      console.log('Getting the details!');
      return Widgetic.api('widgets/' + mosaicGallery.id);
    });
}


getMosaicGallery()
  .then(function(mosaicGallery) {
    return mosaicGallery.skins[0];
  })
  .then(function(skin) {
    console.log('The first skin:');
    console.log(skin)
  });
```

<iframe class="live-example" data-src="examples/rest-composition-skins.html" scrolling="no"></iframe>

Let's use the **Mosaic Gallery** widget. To create a composition we need a widget, a skin and some content. We can build a custom skin or use one of the presets. Let's select a preset for now. We'll get the skins from the `widget/{id}` endpoint.

---

> Listing the Mosaic Gallery's content meta

```js
getMosaicGallery()
  .then(function(mosaicGallery) {
    console.log('The content meta of the Mosaic Gallery Widget:');
    console.log(mosaicGallery.contentMeta);
  });
```

<iframe class="live-example" data-src="examples/rest-composition-content-meta.html" scrolling="no"></iframe>

We have the widget and the skin. All we need now is some content. Each widget expects the content to adhere to a different [content schema](#the-content-meta). 

By looking at the `options.mainAttribute` we deduce that the content items must contain an `image` property. The `attributes.image.control` property informs us that an `image control` will be used for validation. This is the minimum knowledge we need to construct a valid content collection.

---

> Getting the errors

```js
getMosaicGallery()
  .then(function(mosaicGallery) {
    var newComp = {
        name: 'My new composition',
        widget_id: mosaicGallery.id,
        skin_id: mosaicGallery.skins[0].id,
        content: [
          {
            something: "wrong"
          }
        ]
    };

    return Widgetic.api('compositions', 'POST', JSON.stringify(newComp))
  })
  .then(function(composition) {
    console.log('Your composition has been saved!')
    console.log(composition);
  })
  .fail(function(errors) {
    console.log(errors);
  })
```

<iframe class="live-example" data-src="examples/rest-composition-errors.html" scrolling="no"></iframe>

Using this information, let's have a first attempt at creating and saving a composition. We'll experiment a bit by sending some content which does not conform to the schema. 

The Widgetic API tries to return useful error information to ease app development. We can catch and handle these errors on the `.fail` branch of the `api` calls.

---

> Saving the composition

```js
getMosaicGallery()
  .then(function(mosaicGallery) {
    var newComp = {
        name: 'My new composition',
        widget_id: mosaicGallery.id,
        skin_id: mosaicGallery.skins[0].id,
        content: [
            {
              id: 0,
              image: 'https://widgetic.com/assets/widgets/demo/Images/Vertical/image02.jpg',
              order: 0
            },
            {
              id: 1,
              image: 'https://widgetic.com/assets/widgets/demo/Images/Vertical/image01.jpg',
              order: 1
            }
        ]
    };

    return Widgetic.api('compositions', 'POST', JSON.stringify(newComp))
  })
  .then(persist('rest-example-comp'))
  .then(function(composition) {
    console.log('Your composition has been saved!')
    console.log(composition);
  })
  .fail(function(errors) {
    console.log(errors);
  })
```

<iframe class="live-example" data-src="examples/rest-composition-save.html" scrolling="no"></iframe>

With this information we are ready to create the composition object. 

<aside class="notice">The newly created composition resource is saved in local storage to be used across examples.</aside>

---

> Embedding the composition

```html
<div class="embed"></div>
```

```js
composition = retrieve('rest-example-comp')
Widgetic.UI.composition(document.querySelector('.embed'), composition)
```

<iframe class="live-example" data-src="examples/rest-composition-embed.html" scrolling="no"></iframe>

Congratulations! You've created your first composition using the REST API. Here it is embedded in it's full glory.

## Building a custom editor

<iframe class="live-example" data-src="examples/sample-editor/index.html" scrolling="no"></iframe>

This is a sample project showcasing the use of the Widgetic SDK to build a custom editor for a widget.

Get the source-code at [https://github.com/blogvio/sample-custom-editor](https://github.com/blogvio/sample-custom-editor).

## Building a custom CMS

<iframe class="live-example" data-src="examples/sample-cms/index.html" scrolling="no"></iframe>

This is a sample project showcasing the use of the Widgetic SDK to build a custom CMS panel.

Get the source-code at [https://github.com/blogvio/sample-quick-cms](https://github.com/blogvio/sample-quick-cms).