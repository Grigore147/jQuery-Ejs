## [jQuery-Ejs](https://github.com/Shogun147/jQuery-Ejs)

###jQuery wrapper for [EJS](https://github.com/visionmedia/ejs) template engine, with template loading.

## Options

* `path` Public root path for templates, default `/public/views/`
* `extension` Templates file extension, default `.html`
* `open` Open tag, default `<?`
* `close` Close tag, default `?>`
* `async` Load templates async or not, requires callback for methods calls, default `true`
* `cache` Template requests caching, default `true`
* `memory` Cache in memory, template will load only once and no more requests will be sent for it until page reload, default `true`

## Methods

* ### render (template [, data, options, callback])
  
  Render template and return result or undefined on error, if async then callback will get error as 1'st and result as second argument on done.

  * `template` - template name path
  * `data` - template locals
  * `options` - call [options](#options)
  * `callback` - callback for async calls

* ### compile (template [, options, callback])
  Compile template into a function. Return error or template fn.

* ### partial (template [, data, options])

  This is actually a render call but with `async: false` so could be used in templates to render partials. For multiple calls will be much faster to use compiled templates.

There are also few jQuery like methods.

* ###`$method`(el, template [, data, options, callback])
  `el` - Element for which we do the action


  * `update` - Update element content with render result
  * `before` - Add result before element
  * `after` - Add result after element
  * `prepend` - Prepend result to element top
  * `append` - Append result to element bottom
  * `replace` - Replace element with result
  
## Usage

Download and include `./ejs.js` or `./ejs.min.js` and `jquery.ejs.js`.

Create templates under public accessible directory, `/public/views/` as default or set other path in options.

Get view instance:
    // Load view instance, accept an object for custom options
    var View = $.Ejs({ async: false });

<hr/><hr/>

Template: 

`/public/views/welcome.html`

    <h2><?-title?></h2>

Render:

    var html = View.render('welcome', { title: 'jQuery Ejs!!' });

Result: 

    <h2>jQuery Ejs!!</h2>

<hr/><hr/>

Template: 

`/public/views/welcome.html`

    <h2><?-title?></h2>
    <?-View.partial('content', { message: 'Support for partials' })?>

`/public/views/content.html`

    <p><?-message?></p>

Render:

    var html = View.render('welcome', { title: 'jQuery Ejs!!' });

Result: 

    <h2>jQuery Ejs!!</h2>
    <p>Support for partials</p>

<hr/><hr/>
    
Template:

`/public/views/teams.html`

    <h2>Top Football Teams</h2>
    <ul id="teams">
    <?teams.forEach(function(t){?>
      <?-team({ team: t })?>
    <?})?>
    </ul>

`/public/views/teams/team.html`

    <li><?-team.name?>, <?-team.country?></li>

Render:

    var teams = [
      { name: 'Real Madrid', country: 'Spain' },
      { name: 'Barcelona', country: 'Spain' },
      { name: 'Man. United', country: 'England' },
      { name: 'AC Milan', country: 'Italy' },
      { name: 'Arsenal', country: 'England' },
    ]
 
    var team = View.compile('teams/team', { async: false });
    
    View.render('teams', { teams: teams, team: team }, function(error, html) {
      if (error) { return console.log('Error while render teams!');  }

      console.log(html);
    })

## License

The MIT License

Copyright © 2012 D.G. Shogun <Shogun147@gmail.com>