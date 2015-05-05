

----------------

**HOST**
-- add the host


- ai id-ul unui widget
- trebuie sa listezi toate skinurile pt widgetul ales
- trebuie sa alegi un skin (un preset sau un custom skin)
- trebuie sa construiesti un array de content care respecta schema content meta
- http://localhost:4567/?javascript#widget-schema

get widget content schema
Widgetic.api('widgets/542173f409c7e222028b4568', {context: 'lite'})
    .then(function(widget) {
        console.log(widget.contentMeta);
    })
```
schema:
{
    "options": {
        "min": 1,
        "max": 15,
        **"mainAttribute": "image"** - ce este?
    },
    "attributes": {
        "image": {
            "control": "core/controls/image",
            "options": {
                "help_text": ""
            }
        },
        "titleText": {
            "control": "core/controls/text",
            "options": {
                "help_text": "Set the title text for the slide."
            }
        },
        "descriptionText": {
            "control": "core/controls/text",
            "options": {
                "help_text": "Set the description text for the slide."
            }
        },
        "href": {
            "control": "core/controls/url",
            "options": {
                "help_text": "Select a page to open on image click."
            }
        },
        "target": {
            "control": "core/controls/dropdown",
            "options": {
                "help_text": "Select the window in which the link will open if clicked.",
                "options": [
                    {
                        "label": "_blank",
                        "value": "_blank"
                    },
                    {
                        "label": "_top",
                        "value": "_top"
                    }
                ]
            }
        }
    }
}
```
- de descris core/controls: ce tip de info trebuie trimis? plus printscreen al controllerului

A: o compozitie folosind un skin preset
```
myComp = {
    widget_id: "542173f409c7e222028b4568",
    skin_id: "p6_542173f409c7e222028b4568",
    content: [ {
        id:123,
        image: 'http://placehold.it/200/200',
        order: 0
    }]
}

Widgetic.api('compositions', 'POST', JSON.stringify(myComp))
    .then(function(composition) {        
        console.log('You have saved the ' + composition.id + ' composition!')
    })
    .fail(function(error) {        
        console.log(error)
    })
```
- construiesti un obiect cu widget id si skin id
- vei primi o eroare de tip json:
- `{"code":400,"message":"Bad Request","errors":{"content":{"message":"Not enough content items were provided. You must provide at least 1 item(s)."}}}`
- comp nu se va salva pentru ca lipseste field-ul content
- - afli care este mainAttribute (corespunzator pasul 2 din editor)
- construiesti un array de obiecte care au acel attribute tinand cont de min/max
- - ce este min/max?
- **la creare comp nu ar trebui sa fii obligat sa trimiti id si order la content**


EMBED A WIDGET

Widgetic.getEmbedCode('553e282109c7e2295e8b4567') => 
<a href="https://widgetic.com/widgets/photo/accordion-gallery/" class="widgetic-composition" data-id="553e282109c7e2295e8b4567" data-width="500" data-height="500" data-resize="allow-scale-down" data-brand="bottom-right">Accordion Gallery Widget</a>

- Widgetic.UI.composition(**holdingElement**, '553e282109c7e2295e8b4567', width, height, **resizeOption**)

EDIT COMP DETAILS

- celelalte attributes (inafara de maniAttribute) corespund pasului details (3) din editor
```
Widgetic.api('compositions/553e27c009c7e2595f8b4567')
    .then(function(composition) {
        // update some properties
        composition.content[0].titleText = 'My Title Text'

        // save the modified resource
        return Widgetic.api('compositions/' + composition.id, 'PUT', JSON.stringify(composition))
    }).then(function(composition) {
        console.log('Composition ' + composition.id + ' has been updated.')
    });
```

B: o comp folosind un nou skin:
@ see Create a new skin;
follow A

-----

1. user face comp pe site si face embed manual
2. userul face comp folosind o app (vine din 3.2.)
3. userul creeaza o aplicatie
    tip 1
    + agentie 
    + se logheaza in propriul cont si creeaza widgeturi in numele sau

    tip 2
    + site care foloseste widgetic
    + userii lui se logheaza in propriile lor conturi widgetic

    tip 3
    + site care foloseste widgetic white-label
    + userii nu stiu de widgetic
    + useri managed
    + **perechea user/host**


-------

1. modul standard v: 1.
2. face embed folosind widgetic sdk, compozitii salvate in prealabil
3. *face embed de widget si ii modifica continutul dinamic*
    Blogvio.UI.widget('123').setContent({}).setSkin({})
4. editor in pagina (dar nu edit in place)




---------------

access_token 
listate widgeturi
la click pe oricare widget sa apara editorul cu primul skin selectat
**editorul trebuie sa lanseze evenimente (saved)**
cand userul ajunge pe ultimul pas din editor (cand da publish)
editorul si widgetul va disparea


