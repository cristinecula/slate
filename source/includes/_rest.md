# REST API

> API entry point URL

```
https://widgetic.com/api/v2
```

> Example API request

```
GET https://widgetic.com/api/v2/users/me
```

The Widgetic API is inspired by the REST architecture. In it's current state the API is usable, but lacks many desirable features. We are committed to improving the quality of the API to allow the flexibility and usability our third-party developers require. You can consult the [roadmap](#roadmap) to follow along with our progress. We appreciate your [feedback](mailto:support@widgetic.com) for improving it.

## User

You can get profile information about the current authenticated user. You can also create and work with managed user accounts, to [allow white-label integration](#white-label-integration).

### Get User info

> Resource endpoint

```
GET /users/me
```

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

### Managed Users

<span class="todo">Coming soon</span>

## Widgets

The main currency of Widgetic. You can query our collection of widgets and use the information to create embeddable [`compositions`](#compositions). All widgets allow advanced customization of the UI. The customizations can be saved as [`skins`](#skins). Each widget comes with a predefined set of skins, called [`presets`](#presets).

### The widget object

> Example response

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
    "contentMeta": {},
    "skinMeta": {},
    "skins": [],
    "width": 560,
    "height": 600
  }
```

#### Attributes

Field       | Description
----------- | -----------
id          | 
name        | 
slug        | 
category    | The category of the widget (photo, audio, video, util)
previews    | A collection of preview images, showcasing the widget
contentMeta | The [content meta](#the-content-meta), a hash describing the keys and value restrictions expected by the widget for it's content objects
skinMeta    | The [skin meta](#the-skin-meta), a hash describing the widget's [skin](#skins) schema
skins       | User's [skins](#skins) and [presets](#presets)
width       | The default width
height      | The default height

---

### Get all widgets

> Resource endpoint

```
GET /widgets
```

> Getting the list of widgets' names

```js
Widgetic.api('widgets', {'fields': ['name']})
    .then(function(widgets) {
        console.log(widgets);
    })
```

Query the available widgets.

#### Query Parameters

Parameter   | Description
----------- | -----------
fields      | Limit the fields
max-results | Limit the number of results
sort-by     | A field to sort the results by

---

### Get a widget

> Resource endpoint

```
GET /widgets/{id}
```

> Getting the name of a widget

```js
Widgetic.api('widgets/5195f3ec1d8a0c7a17000000')
    .then(function(widget) {
        console.log(widget.name);
    })
```

Get a widget by id.

## Skins

### Presets

> Getting a widget's presets

```js
var widgetId = '5404451409c7e20b0d8b4567'

Widgetic.api('skins', {widget_id: widgetId})
    .then(function(skins) {
        var presets = skins.filter(function(skin) {
            // test if the id ends with the widget id
            var isPreset = new RegExp(widgetId + '$', 'i');
            isPreset = isPreset.test(skin.id)
            return isPreset
        })

        console.log(presets)
    })
```

Pre-made skins that are created by the Widgetic staff for each widget. You can choose to use any of them for your embeds or use them as a base for your own customizations. Rest assured that the presets available when creating your composition will be available permanently (slight adjustments might be made to fit widget updates).

Presets are retrieved from the same endpoint as [`skins`](#get-all-skins-and-presets), or by way of the [`widget's`](#the-widget-object) `skins` attribute. You can distinguish presets from skins from the structure of the `id`. Presets have the widget's `id` concatenated to their own.

### The skin meta

### Get all skins and presets

> Resource endpoint

```
GET /skins
```

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

#### Query Parameters

Parameter   | Description
----------- | -----------
widget_id   | The widget id to filter the skins

---

### Save a new skin

> Resource endpoint

```
POST /skins
```

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

---

### Get a skin

> Resource endpoint

```
GET /skins/{id}
```

> Getting the details of a previously created skin

```js
Widgetic.api('skins/5108dcefe599d12e6f000000')
    .then(function(skin) {
        console.log('This is the ' + skin.name + ' skin.');
    })
```

Get a skin from the server by referencing it by it's id.

---

### Update a skin

> Resource endpoint

```
PUT /skins/{id}
```

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

---

### Delete a skin

> Resource endpoint

```
DELETE /widgets/{id}
```

> Deleting a skin

```js
Widgetic.api('skins/' + mySkin.id, 'DELETE')
    .then(function() {
        console.log('The skin has been deleted.');
    })
```

To delete a skin, you must issue a `DELETE` http request.

## Compositions

To create an embeddable widget you construct a `composition` resource, linking a widget, a skin and the desired content. This allows easy [embedding](#embedding-your-first-widget) and facilitates content updates. Compositions can be created using the [Widgetic Editor](#embedding-the-editor), but you can also build them manually, ensuring you adhere to [the schema](#the-composition-object).

### The composition object

> Example composition

```json
 {
    "id": "5549b54947749318327b23c8",
    "name": "My Composition",
    "content": [
        {
            "id": "c-1",
            "order": 1,
            "image": "https://widgetic.com/assets/widgets/demo/Images/Vertical/image02.jpg",
            "target": "_blank"
        },
        {
            "id": "c-2",
            "order": 2,
            "image": "https://widgetic.com/assets/widgets/demo/Images/Food/image05.jpg",
            "target": "_blank"
        }
    ],
    "skin_id": "p4_5404451409c7e20b0d8b4567",
    "widget_id": "5404451409c7e20b0d8b4567",
    "width": 560,
    "height": 355
}
```

#### Attributes

Field       | Description
----------- | -----------
id          | 
name        | 
content     | An array of content objects, with fields and values corresponding to the widget's [content schema](#the-content-meta)
widget_id   | References the widget
skin_id     | The skin or preset used for customizing the widget's looks
width       | The dimensions the embed should load with. Actual behavior depend upon the [embed resize option](#embed-resize-options) used
height      |

### The content meta

> Getting a widget's content meta

```js
Widgetic.api('widgets/5404451409c7e20b0d8b4567')
    .then(function(widget){ console.log(widget.contentMeta) })
```

> Example response

```json
{
    "options": {
        "min": 1,
        "max": 15,
        "mainAttribute": "image"
    },
    "attributes": {
        "image": {
            "control": "core/controls/image",
            "options": {
                "help_text": "",
                "placeholder": "Add URL to image file (jpg, jpeg, png, gif)",
                "readonly": true,
                "preview": true
            }
        },
        "href": {
            "control": "core/controls/url",
            "options": {
                "help_text": "Select a page to open on image click.",
                "placeholder": "Add Link",
                "label": "Web Address"
            }
        },
        "target": {
            "control": "core/controls/dropdown",
            "options": {
                "label": "Add Link Target",
                "help_text": "Select the window in which the link will open if clicked.",
                "default": "_blank",
                "options": [
                    { "label": "_blank", "value": "_blank" },
                    { "label": "_top", "value": "_top" }
                ]
            }
        }
    }
}
```

The content schema is defined per widget, as each widget requires different attributes for it's content items. This schema is accessible on the [widget resource's](#the-widget-object) `contentMeta` attribute.

Field       | Description
----------- | -----------
[options](#content-options)       | Meta information related to the content
[attributes](#content-attributes) | A hash that describes each of the widget's attributes. The keys are the names of the attributes, as required when defining the compositions' content items. The values contain other metadata related to value restrictions and descriptions

#### <a name="content-options"></a> Options

Field         | Description
------------- | -----------
min           | The minimum number of content items
max           | The maximum number of content items
mainAttribute | The name of the required attribute. All of the other attributes are optional

#### <a name="content-attributes"></a> Attributes

Field       | Description
----------- | -----------
control     | An identifier for the [`control`](#controls) that will handle validation and UI rendering in the editor
options     | Various options used for the `control`

#### Controls

<span class="TODO">Coming soon</span>

### Get all compositions

> Resource endpoint

```
GET /compositions
```

> Getting the compositions list

```js
Widgetic.api('compositions', {widget_id: '51d57c5a1d8a0cee4a000000'})
    .then(function(compositions) {
        console.log('You have ' + compositions.length + ' saved compositions!')
    })
```

Retrieve the users' saved compositions.

#### Query Parameters

Parameter   | Description
----------- | -----------
widget_id   | The widget id to filter the compositions

---

### Create a new composition

> Resource endpoint

```
POST /compositions
```

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

`POST /compositions`

---

### Get a composition

> Resource endpoint

```
GET /compositions/{id}
```

> Getting the name of a composition

```js
Widgetic.api('compositions/553758d509c7e2334a8b458a')
    .then(function(composition) {
        console.log('The composition is called ' + composition.name);
    });
```

Get a composition by it's id.

---

### Update a composition

> Resource endpoint

```
PUT /compositions/{id}
```

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

---

### Delete a composition

> Resource endpoint

```
DELETE /compositions/{id}
```

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

---

## Roadmap

Improvements we are currently working on:

**Documentation**

* add Widgetic.js Examples
* add curl examples
* add details about server-side authentication
* document managed accounts