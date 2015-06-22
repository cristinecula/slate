# Glossary

There are a few concepts that you need to master before using the API. These concepts are created around the two core elements: Widget and Editor.

## Widget
A widget is any object you can add into a webpage through a snippet of code.

All our widgets are built on top of a Debug Widget, that has a specific structure, a meta theme and a meta content. This allows us to use the Editor for customizing any widget.

## Debug Widget
A template widget with a standard arhitecture respected by all widgets on Widgetic.

## Dynamic Widget
A dynamic widget is a widget composition created in code that is displayed temporarly, without overwriting the composition widget used to created it.

You would need such a widget if for example you’d have an e-commerce store with 100 products or more. So instead of creating 100 manual compositions, you would create a single composition and always change it’s content when the site loads.

**Notes:**

1. Dynamic widgets cannot be re-embedded.
2. To create a dynamic widget, you need to base it on an existing composition.
3. The stats are counted for the base widget composition. Assuming your products have different URLs, you would know which dynamic composition the stats apply to, based on the stats URL.

## Widget Composition
A composition is the product obtained by customising a widget. A composition represents any widget in it's ready to use form: has a custom/preset theme, has content, has details and an embed code associated.

COMPOSITION = Theme + Content + Details + Embed Code

## Widget Main Attribute
The widgets are grouped in 4 categories, based on their content type, which is their main attribute:

Category | Uses                                | File Types
---------|-------------------------------------|------------
Photo    | galleries, slideshows, banners, etc | jpg, png, gif
Audio    | audio players                       | mp3
Video    | video players                       | mp4
Utilities| chart, ticker, calendar, countdown timer, etc | text

<aside class="note">When creating a Composition, make sure the content type is specific to that widget.</aside>

## Editor

A three steps panel which allows you to customize a widget by selecting a theme, adding content, editing the details and generating the associated code for embed.

+ Step 1: Customize Theme
+ Step 2: Add Content
+ Step 3: Edit Details

You can however, implement your Editor UI completely from scratch using the Widgetic.js SDK.

## Theme 
A set of properties specific to each widget (settings, colours) that users can edit. Also called **skins**, in the API.

## Preset Theme
A predefined Theme, created by us, which is available to any user.

## Input Controller
An input controller is an input that allows the user to change a specific property of a theme.

These are the Input Controllers available in the Editor:

* Dropdown
* Number
* URL
* RSS
* Text
* Text Area
* Toggle
* Slider
* RangeSlider
* Date and Time
* Location
* Color Picker
* Browser Source
* Position (Pixel Based + Grid Based)
* Order Items
* Font

## Content Source
A content source is a web service where the user can host his content.

These are the Content Sources available in the Editor:

* Facebook (photo, video)
* Flickr (photo, video)
* Link (URL) (photo, audio, video)
* DropBox (photo, audio, video)
* Instagram (photo, video)
* Google Drive (photo, video)
* Text (data)

## App 
A standalone application using the Widgetic API and optionally Widgetic.js, which can bring the power of the Widgetic platform into your project.

Depending on your integration, you can ask users to log into their Widgetic account or you can choose to manage an account on their behalf (and provide them our widgets without the hassle). Authentication is done using the standard OAuth2 protocol.

## Users
On Widgetic there can be two types of users, depending on your integration.

### Standard Users
Users that own an account on Widgetic. All data (widget compositions, themes, etc) they author is stored in their account.
 
Using the Widgetic.js SDK, standard users can log into Widgetic Apps using OAuth.

### Managed Users
Widgetic accounts created by a Standard User. Managed Users are not able to log into Widgetic (most likely they will not even know they own a Widgetic account). All data they create is managed by the Standard User (you) for them. We call this *white label integration*.

Managed users receive the same properties of your Standard Account (basic, plus or premium).

**Note:**

1. If you don't ask your users to sign into Widgetic, you must use Managed Users (white label integration). Each Managed User will have his individual data (themes, widget compositions, stats) which you can access with your Standard Account. Check the [Terms and Conditions](https://widgetic.com/terms-of-service) for restrictions regarding users.
2. How do we price managed users?

    Each Managed User is charged $0.05 for Plus and $0.1 for Premium. For example if you have a Premium account and a white label app, you are charged as follows:

    Base price for Premium + Managed Users * $0.1.