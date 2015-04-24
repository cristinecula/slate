# Examples

## Embedding your first widget

<div data-height="268" data-theme-id="5418" data-slug-hash="yyWrqO" data-default-tab="js" data-user="cristinecula" class='codepen'><pre><code>Blogvio.init(&#x27;5525287d09c7e201498b4567_5ep4alabc9wk00kc08c8o4kw008ksowogsg4w0wwkog8ww80o0&#x27;,&#x27;http://s.codepen.io/cristinecula/debug/yyWrqO?&#x27;);
Blogvio.auth(false)
    .then(function() {
    $(&#x27;body&#x27;).append(&#x27;&lt;span&gt;&#x27; + Blogvio.auth.status().accessToken)
  })
Widgetic = Blogvio

$(&#x27;button&#x27;).on(&#x27;click&#x27;, function(ev){
  ev.preventDefault();
  run();
}) 

function run() {
// get an existing skin (preset) to act as a base
// request the widgets&#x27; skins
Widgetic.api(&#x27;skins&#x27;, {widget_id: &#x27;5108dcefe599d12e6f000000&#x27;})
    .then(function(skins) {
        // select the first skin
        return skins[0];
    }).then(function(skin) {
        // edit some properties
        skin.name = &quot;My red skin&quot;;
        skin.backgroundcolor = &quot;#ff0000&quot;;

        return Widgetic.api(&#x27;skins&#x27;, &#x27;POST&#x27;, JSON.stringify(skin));
    }).then(function(skin) {
        console.log(&#x27;Skin &#x27; + skin.id + &#x27; saved!&#x27;);
    })
}</code></pre>
<p>See the Pen <a href='http://codepen.io/cristinecula/pen/yyWrqO/'>yyWrqO</a> by Cristian Necula (<a href='http://codepen.io/cristinecula'>@cristinecula</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
</div><script async src="//assets.codepen.io/assets/embed/ei.js"></script>