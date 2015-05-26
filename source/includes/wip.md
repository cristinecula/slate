

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

------

- cum iau un access_token server <-> server
- cum folosesc un access_token luat server <-> server
* editorul face comp read-only
+ editor.close trebuie sa dezactiveze celelalte metode (warn('the editor has been closed'))
+ composition.setWidth, setHeight
+ composition.getEmbedCode()
+ composition.isSaved() sau isPersisted()
+ composition.delete() ???

Ca sa folosesti dinamic widgets trebuie sa ai access token? NU
E de ajuns client_id? Cred ca da. VERIFICA!

Ce se intampla daca un user oarecare da composition.save() pe un widget dinamic?


compozitii dinamice -> salveaza comp data la el
comp dinamice pt widgets pro


+comp dinamice pot pleca numai de la o comp salvata !!! pentru ca limita de widgets sa conteze
+comp dinamice trebuie sa astepte continut/skin


var 1
<a href="https://local.widgetic.com/widgets/photo/before-after-gallery/" class="widgetic-composition" data-id="556304b34774932d598b4595" data-width="560" data-height="341" data-resize="allow-scale-down" data-dynamic="true">Before and After Gallery Widget</a>

<script type="text/javascript">
    comp = Widgetic.UI.composition.get('556304b34774932d598b4595');
    comp.clearContent()
    comp.setContent([{image: 'http://ceva.com/image.jpg'}])
    comp.display()
</script>

var 2

<script type="text/javascript">    
this.widget = Widgetic.UI.composition(
    this.widgetArea[0], {
        widget_id: widget.id,
        wait_editor_init: true
    }
);


this.widget = new Widgetic.UI.composition(
    this.widgetArea[0], composition.id
);

</script>

---

<script>
var widget = Widgetic.UI.composition('.widget', {
    widget: '123'
    content: [...]
    skin: {...} | 'skin-id'
})

Widgetic.auth.token('1234-token')

appX ->                 userY ->         comp123
http://sitekit.com      http://gica.com
http://app.sitekit.com
---

comp -> embeddable = true
daca e dinamic nu se poate da share

data-share-parent va fi url-ul de unde s-a dat share
daca lipseste va fi folosit primul url pe care stim ca s-a dat embed
=> pe masura ce se da embed, sa stocam url-urile in baza de date


<a href="https://local.widgetic.com/widgets/photo/before-after-gallery/" class="widgetic-composition" data-id="556304b34774932d598b4595" data-width="560" data-height="341" data-resize="allow-scale-down" data-dynamic="true" data-share-parent="http://lefigaro.com/123/ceva.articol">Before and After Gallery Widget</a>

---

var comp = Widgetic.UI.composition('.widget', {
    composition: '123',
    width: 300,
    height: 200,
    resize: 'full-width',
    brandingPosition: 'top-left',
    showBranding: true,
    content: [...]
    skin: {...} | 'skin-id'
})

Widgetic.UI.composition.getAll('555b5e6509c7e29e718b4569')
Widgetic.UI.composition.get('123')
</script>

3 conturi managed
5 widgets