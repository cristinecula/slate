# REST API

## User

You can get profile information about the current authenticated user. You can also create and work with managed user accounts, to [allow white-label integration](#white-label-integration).

### Get User info

> Getting the logged in user's info

```js
Widgetic.api('users/me')
    .then(function(user) {
        console.log('Hello ' + user.username);
    })
```

> Example response

```json
{
  "id": "51277df103a25c7a34000000",
  "username": "user",
  "email": "email@example.com"
}
```

Retrieve information about the logged in user. Useful for "Sign in with Widgetic" functionality. By default the response is limited to their Widgetic unique ID and their username. To get the email info you must request the [`email` scope](#scopes) during the authorization step.

#### HTTP Request

`GET /api/v2/users/me`

### Managed Users

<span class="todo">Coming soon</span>

## Widgets

<span class="todo">Needs improving</span>

Widgets are the 

### Widget schema

> The widget schema

```json
{
    "id": "542945dc09c7e2ab6c8b4567",
    "name": "Accordion With Text",
    "slug": "accordion-with-text",
    "category": "util",
    "previews": {
      "full": "/assets/uploads/widgets/previews/acordeon_ca_la_turci.png",
      "large": "/assets/uploads/widgets/previews/acordeon_ca_la_turci.png",
      "small": "/assets/uploads/widgets/previews/acordeon_ca_la_turci_mic.png"
    },
    "contentMeta": <ContentMeta>,
    "skinMeta": <SkinMeta>,
    "skins": [
        <Skin>,
        <Skin>,
        "..."
    ],
    "width": 560,
    "height": 600,
    "css": "...",
    "module": "accordion_with_text_widget",
    "js": "..."
  }
```

The schema

Field       | Description
----------- | -----------
id          | The unique identifier of the resource
name        | The name of the widget
slug        | The normalized name of the widget
category    | The category of the widget (photo, audio, video, util)
previews    | A collection of preview images
contentMeta | The schema used for the content
skinMeta    | The
skins       |
width       |
height      |
css         |
module      |
js          |

### Content schema

---

### Get all widgets

> Getting the list of widgets' names

```js
Widgetic.api('widgets', {'fields': ['name']})
    .then(function(widgets) {
        console.log(widgets);
    })
```

Query the available widgets.

#### HTTP Request

`GET /api/v2/widgets`

#### Query Parameters

Parameter   | Description
----------- | -----------
fields      | Limit the fields
max-results | Limit the number of results
sort-by     | A field to sort the results by

---

### Get a widget

> Getting the name of a widget

```js
Widgetic.api('widgets/5195f3ec1d8a0c7a17000000')
    .then(function(widget) {
        console.log(widget.name);
    })
```

Get a widget by id.

#### HTTP Request

`GET /api/v2/widgets/{id}`

## Skins

### Get all skins and presets

> Getting the names of a widget's skins

```js
Widgetic.api('skins', {widget_id: '542173f409c7e222028b4568'})
    .then(function(skins) {
        skins.map(function(skin) {
            console.log(skin.name, skin.id)
        })
    })
```

Get all the skins and presets for a given widget.

Preset id's have the widget id concatenated to it's own id: `id_widgetId`

#### HTTP Request

`GET /api/v2/skins`

#### Query Parameters

Parameter   | Description
----------- | -----------
widget_id   | The widget id to filter the skins

---

### Save a new skin

> Creating a new skin, using a preset as the base

```js
// request the widgets' skins
Widgetic.api('skins', {widget_id: '5108dcefe599d12e6f000000'})
    .then(function(skins) {
        // select the first skin
        return skins[0];
    }).then(function(skin) {
        // edit some properties
        skin.name = "My red skin";
        skin.backgroundcolor = "#ff0000";

        // send the api request
        return Widgetic.api('skins', 'POST', JSON.stringify(skin));
    }).then(function(skin) {
        // use the saved skin
        console.log('Skin ' + skin.id + ' saved!');
    })
```

To create a new skin you must first create an object that has all the keys and values corresponding to the widget's `skinMeta` schema. The object must also contain the `widget_id` key to identify the widget for which this skin is intended.

The response is a skin object with the data that was sent to the API and the new id of the resource, for later use.

#### HTTP Request

`POST /api/v2/skins`

---

### Get a skin

> Getting the details of a previously created skin

```js
Widgetic.api('skins/5108dcefe599d12e6f000000')
    .then(function(skin) {
        console.log('This is the ' + skin.name + ' skin.');
    })
```

Get a skin from the server by referencing it by it's id.

#### HTTP Request

`GET /api/v2/skins/{id}`

---

### Update a skin

> Updating a previously saved skin

```js
var mySkin = { id: "553622bb09c7e25d158b4571" };
var resourceURL = 'skins/' + mySkin.id;

// get the skin resource
Widgetic.api(resourceURL).then(function(skin) {
        mySkin = skin;
        // change skin properties
        mySkin.name = "My edited skin";
        // issue a PUT request to update the skin
        return Widgetic.api(resourceURL, 'PUT', JSON.stringify(mySkin));
    }).then(function(skin) {
        console.log('The skin is now called ' + skin.name + '!');
    })
```

To update a skin you must send a `PUT` http request with the full representation of the skin resource, including the modification.

#### HTTP Request

`PUT /api/v2/skins/{id}`

---

### Delete a skin

> Deleting a skin

```js
Widgetic.api('skins/' + mySkin.id, 'DELETE')
    .then(function() {
        console.log('The skin has been deleted.');
    })
```

To delete a skin, you must issue a `DELETE` http request.

#### HTTP Request

`DELETE /api/v2/skins/{id}`

## Compositions

Compositions are resources linking a widget with a skin and user provided content.

### Get all compositions

> Getting the compositions list

```js
Widgetic.api('compositions', {widget_id: '51d57c5a1d8a0cee4a000000'})
    .then(function(compositions) {
        console.log('You have ' + compositions.length + ' saved compositions!')
    })
```

Retrieve the users' saved compositions.

#### HTTP Request

`GET /api/v2/compositions`

#### Query Parameters

Parameter   | Description
----------- | -----------
widget_id   | The widget id to filter the compositions

---

### Create a new composition

> Creating a new composition

```js
var newComp = {
    name: 'My new composition',
    content: [
        {
          id: 0,
          image: 'http://placehold.it/200x200',
          order: 0,
        },
        {
          id: 1,   
          image: 'http://placehold.it/200x200/ff33cc',
          order: 1
        }
    ],
    skin_id: 'p1_51d57c5a1d8a0cee4a000000',
    widget_id: '51d57c5a1d8a0cee4a000000'
};

Widgetic.api('compositions', 'POST', JSON.stringify(newComp))
    .then(function(composition) {
        console.log('You have saved the ' + composition.id + ' composition!')
        newComp = composition
    })
```

Create a composition.

#### HTTP Request

`POST /api/v2/compositions`

---

### Get a composition

> Getting the name of a composition

```js
Widgetic.api('compositions/553758d509c7e2334a8b458a')
    .then(function(composition) {
        console.log('The composition is called ' + composition.name);
    });
```

Get a composition by it's id.

#### HTTP Request

`GET /api/v2/compositions/{id}`

---

### Update a composition

> Updating a composition

```js
// get the composition resource
Widgetic.api('compositions/553758d509c7e2334a8b458a')
    .then(function(composition) {
        // update some properties
        composition.width = 200;
        composition.height = 200;

        // save the modified resource
        return Widgetic.api('compositions/' + composition.id, 'PUT', JSON.stringify(composition))
    }).then(function(composition) {
        console.log('Composition ' + composition.id + ' has been updated.')
        console.log('New dimensions are: '+ composition.width +'x'+ composition.height +' .')
    });
```

Update an existing compositions' properties.

#### HTTP Request

`PUT /api/v2/compositions/{id}`

---

### Delete a composition

> Deleting a composition

```js
Widgetic.api('compositions/553758d509c7e2334a8b458a', 'DELETE')
    .then(function() { 
        console.log('The composition has been deleted!')
    }).fail(function() {
        console.log('The composition does not exist!')
    });
```

Delete an existing composition.

<aside class="notice warning">The <code>DELETE</code> request does not validate if the composition is embedded. Embedded compositions will stop working. Be sure to remove all embeds before deleting a composition.</aside>

#### HTTP Request

`DELETE /api/v2/compositions/{id}`

---
