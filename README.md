Citation.js converts formats like BibTeX, Wikidata JSON and ContentMine JSON to CSL-JSON to convert to other formats like APA, Vancouver and back to BibTeX.

[![NPM version](https://img.shields.io/npm/v/citation-js.svg)](https://www.npmjs.org/citation-js)
[![NPM total downloads](https://img.shields.io/npm/dt/citation-js.svg)](https://npmjs.org/citation-js)
[![Build Status](https://travis-ci.org/larsgw/citation.js.svg?branch=master)](https://travis-ci.org/larsgw/citation.js)
[![Dependency Status](https://david-dm.org/larsgw/citation.js/status.svg)](https://david-dm.org/larsgw/citation.js)

##### Table of Contents

* [Citation.js](#citation)
  * [Get Started](#citation.starting)
    * [Install](#citation.install)
      * [Node.js](#citation.starting.install.node)
        * [Dependencies](#citation.starting.install.node.dependencies)
      * [Browser](#citation.starting.install.browser)
        * [Dependencies](#citation.starting.install.browser.dependencies)
    * [Example](#citation.starting.example)
  * [Use](#citation.use)
    * [Cite](#citation.cite)
      * [Input](#citation.cite.in)
        * [Input types](#citation.cite.in.type)
        * [Options](#citation.cite.in.options)
      * [Ouput](#citation.cite.out)
        * [Type](#citation.cite.out.type)
        * [Style](#citation.cite.out.style)
        * [CSL Templates](#citation.cite.out.templates)
        * [CSL Locales](#citation.cite.out.locales)
      * [Misc](#citation.cite.misc)
    * [Async](#citation.async)
* [jquery.Citation.js](#jquery)
  * [Use](#jquery.use)
    * [jQueryCite](#jquery.cite)
      * [HTML templates](#jquery.cite.form)
        * [Input form](#jquery.cite.form.in)
          * [Input form fields](#jquery.cite.form.in.fields)
        * [Output form](#jquery.cite.form.out)
  * [Dependencies](#jquery.dependencies)
* [More](#more)
  * [More Docs](#more.docs)
  * [Demo](#more.demo)
    * [NPM Demo](#more.demo.npm)
    * [Browser Demos](#more.demo.browser)

# <a id="citation" href="#citation">Citation.js</a>

## <a id="citation.starting" href="#citation.starting">Get Started</a>

### <a id="citation.starting.install" href="#citation.starting.install">Install</a>

#### <a id="citation.starting.install.node" href="#citation.starting.install.node">Node.js</a>

Install the package ([citation-js](https://www.npmjs.org/package/citation-js)) like this:

    npm install -g citation-js

To run the program, use

    citation-js  [options]

    Options:

      -h, --help                      output usage information
      -V, --version                   output the version number
      
      -i, --input <path>              Input file
      -u, --url <url>                 Input url
      -t, --text <string>             Input text
      
      -o, --output <path>             Output file (omit file extension)
      
      -R, --output-non-real           Do not output the file in its mime type, but as a string
      -f, --output-type <option>      Output structure type: string, html, json
      -s, --output-style <option>     Ouput scheme. A combination of --output-format json and --output-style citation-* is considered invalid. Options: csl (Citation Style Lanugage JSON), bibtex, citation-* (where * is any formatting style)
      -l, --output-language <option>  Output language. [RFC 5646](https://tools.ietf.org/html/rfc5646) codes

To use the [`Cite`](#citation.cite) constructor, `require()` the module like this:

```js
var Cite = require('citation-js')
```

##### <a id="citation.starting.install.node.dependencies" href="#citation.starting.install.node.dependencies">Dependencies</a>

* [babel-polyfill](https://www.npmjs.com/package/babel-polyfill)
* [citeproc](https://www.npmjs.com/package/citeproc)
* [commander](https://www.npmjs.com/package/commander)
* [deep-freeze](https://www.npmjs.com/package/deep-freeze)
* [striptags](https://www.npmjs.com/package/striptags)
* [sync-request](https://www.npmjs.com/package/sync-request)
* [wikidata-sdk](https://www.npmjs.com/package/wikidata-sdk)

#### <a id="citation.starting.install.browser" href="#citation.starting.install.browser">Browser</a>

With the following code, you can `require('citation-js')` to get the [`Cite`](#citation.cite) contructor.

```html
<script src="path/to/browser.js" type="text/javascript"></script>
```

##### <a id="citation.starting.install.browser.dependencies" href="#citation.starting.install.browser.dependencies">Dependencies</a>

Dependencies are the same as the ones above, with the exception of `commander`. They are all included in `citation.js`.

### <a id="citation.starting.example" href="#citation.starting.example">Example</a>

```js
var Cite = require('citation-js@0.3.0-6')
 
var data = new Cite('Q21972834', {
  format: 'string',
  type: 'html',
  style: 'citation-apa',
  lang: 'en-US'
})
 
data.get() // Should implicitly display 
```

To test this code, go to [RunKit](https://runkit.com/larsgw/591b5651bd9b40001113931c).

## <a id="citation.use" href="#citation.use">Use</a>

### <a id="citation.cite" href="#citation.cite">Cite</a>

Use the object constructor `Cite()` to parse input and get output.

#### <a id="citation.cite.in" href="#citation.cite.in">Input</a>

Make a `Cite` object like this:

```js
var example = new Cite( <data>, <options> )
```

1. In the first parameter you pass the input data. [Input types](#citation.cite.in.type)
2. In the second parameter you pass the [options]().

##### <a id="citation.cite.in.type" href="#citation.cite.in.type">Input types</a>
Input type doesn't have to be specified. The identifiers below are used by internal functions.

###### <a id="citation.cite.in.type.wikidata" href="#citation.cite.in.type.wikidata">Wikidata</a>
* `url/wikidata`: URL with [Wikidata](https://www.wikidata.org/) [Entity ID](https://www.wikidata.org/wiki/Wikidata:Glossary#Entities.2C_items.2C_properties_and_queries). Gets and parses the entity data
* `list/wikidata`: List of Wikidata Entity IDs, separated by spaces, newlines or commas. Gets and parses the entity data
* `string/wikidata`: Single Wikidata Entity ID. Gets and parses the entity data
* `api/wikidata`: Wikidata API URL. Gets and parses the entity data
* `json/wikidata`: Wikidata Entity data. Parses the entity data

###### <a id="citation.cite.in.type.bibtex" href="#citation.cite.in.type.bibtex">BibTeX</a>
* `string/bibtex`: [BibTeX](http://www.bibtex.org/) string. Parses the data

###### <a id="citation.cite.in.type.bibjson" href="#citation.cite.in.type.bibjson">BibJSON</a>
* `json/contentmine`: Actually BibJSON, all references to ContentMine will be removed when the parser is fully done. Parses the data

###### <a id="citation.cite.in.type.csl" href="#citation.cite.in.type.csl">CSL-JSON</a>
* `json/csl`: [CSL-JSON](https://github.com/citation-style-language/schema#csl-json-schema). Adds the data
* `array/csl`: Array of CSL-JSON. Adds the data

###### <a id="citation.cite.in.type.inter" href="#citation.cite.in.type.inter">Intermediary formats</a>
These formats are not input-ready, but are rather parsed and re-evaluated, e.g. `html/else` (DOM element) to `string/json` to `json/csl`.

* `string/json`: JSON or JavaScript Object string. Parses and re-evaluates the data
* `jquery/else`: jQuery element. Fetches and re-evaluates the contents
* `html/else`: HTML DOM element. Fetches and re-evaluates the contents
* `url/else`: URL. Fetches and re-evaluates the file
* `array/else`: JavaScript array. Re-evaluates every element in the array

##### <a id="citation.cite.in.options" href="#citation.cite.in.options">Options</a>
These are the default options for using `.get()`. More options will follow.

  1. `format`: The output format: `"real"` (default) or `"string"`
  2. `type`: The output type: `"html"`, `"string"` or `"json"` (default).
  3. `style`: The output style. See [Output](#citation.cite.out). `"csl"` is default
  4. `lang`: The language of the output. [RFC 5646](https://tools.ietf.org/html/rfc5646) codes. Currently supported: `"en-US"` (default), `"fr-FR"`, `"es-ES"` ,`"de-DE"` and `"nl-NL"`

#### <a id="citation.cite.out" href="#citation.cite.out">Ouput</a>

When using the `.get()` function, your output depends on the options you pass. If you don't pass any options, the values you passed as default are used. When you didn't pass default options, standard options are passed.

##### <a id="citation.cite.out.type" href="#citation.cite.out.type">Type</a>

* `json`: Output as JSON. Not possible together with `style:"citation-*"`
* `html`: Output as HTML
* `string`: Output as string

##### <a id="citation.cite.out.style" href="#citation.cite.out.style">Style</a>

* `csl`: Outputs raw CSL-JSON data
* `bibtex`: Outputs a BibTeX string, or BibTeX-JSON if `type: "json"`
* `citation-*`: Formatted citation, formatted with citeproc-js. `*` is a [CSL Template](#citation.cite.out.templates) name.

##### <a id="citation.cite.out.templates" href="#citation.cite.out.templates">CSL Templates</a>

Currently, the following CSL Templates are suppported in Citation.js.

* `apa`
* `vancouver`
* `harvard1`

Different [CSL Templates](https://github.com/citation-style-language/styles) can be used by passing an XML string to `.get()` with the option `template:<string>`. E.g.

```js
var data = new Cite(...)

data.get({
  format: 'string',
  type: 'html',
  style: 'citation-<TEMPLATE NAME>',
  lang: 'en-US',
  
  template: '...' // XML String
})
```

Replace `<TEMPLATE NAME>` with the template name you want to use. After calling `.get()` with these options once, you can omit the template property, if you use the same locale.

##### <a id="citation.cite.out.locales" href="#citation.cite.out.locales">CSL Locales</a>

If you want different languages than the standard, you can pass a [CSL Locale](https://github.com/citation-style-language/locales) as an XML string to `.get()` with the option `locale:<string>`. E.g.

```js
var data = new Cite(...)

data.get({
  format: 'string',
  type: 'html',
  style: 'citation-apa',
  
  locale: '...' // XML String
})
```
#### <a id="citation.cite.misc" href="#citation.cite.misc">Misc</a>

`Cite` instances have some more functions:

* `.options(<options>)`: Change default options
* `.set(<data>)`: Replace all data with new data
* `.add(<data>)`: Add data
* `.reset()`: Remove all data and options
* `.currentVersion()`: Get current version number
* `.retrieveVersion(<version number>)`: Retrieve a certain version of the object
* `.retrieveLastVersion()`: Retrieve the last saved version of the object
* `.undo(<number>)`: Restore the n to last version (default: `1`)
* `.save()`: Save the current object
* `.sort()`: Sort all entries on basis of their BibTeX label

`Cite` holds all internal functions, too. These are documentated [here](https://larsgw.github.io/citation.js/api/global.html) and can be accessed like this:

```js
{
  async: [Function: async],
  parse: 
   { input: 
      { type: [Function: parseInputType],
        data: [Function: parseInputData],
        chain: [Function: parseInput],
        chainLink: [Function: parseInputChainLink],
        async: [Object] },
     wikidata: 
      { list: [Function: parseWikidata],
        json: [Function: parseWikidataJSON],
        prop: [Function: parseWikidataProp],
        type: [Function: fetchWikidataType],
        async: [Object] },
     bibjson: [Function: parseContentMine],
     date: [Function: parseDate],
     name: [Function: parseName],
     json: [Function: parseJSON] },
  get: 
   { bibtex: 
      { json: [Function: getBibTeXJSON],
        text: [Function: getBibTeX],
        label: [Function: getBibTeXLabel],
        type: [Function: fetchBibTeXType] },
     dict: { htmlDict: [Object], textDict: [Object] },
     json: [Function: getJSON],
     date: [Function: getDate],
     name: [Function: getName],
     label: [Function: getLabel] },
  CSL: 
   { style: [Function: fetchCSLStyle],
     locale: [Function: fetchCSLLocale],
     engine: [Function: fetchCSLEngine],
     item: [Function: fetchCSLItemCallback] },
  util: 
   { attr: 
      { getAttributedEntry: [Function: getAttributedEntry],
        getPrefixedEntry: [Function: getPrefixedEntry] },
     deepCopy: [Function: deepCopy],
     fetchFile: [Function: fetchFile],
     fetchFileAsync: [Function: fetchFileAsync],
     fetchId: [Function: fetchId] } }
```

### <a id="citation.async" href="#citation.async">Async</a>

Use the async api (recommended for Wikidata, URL, and DOI input) like this (with callback):

```js
Cite.async(<DATA>, <OPTIONS>, function (data) {
  data // instance of Cite
  
  // Further manipulations...
  console.log(data.get())
})
```

Or with a promise, like this:

```js
Cite.async(<DATA>, <OPTIONS>).then(function (data) {
  data // instance of Cite
  
  // Further manipulations...
  console.log(data.get())
})
```

Where `<DATA>` is the input data and `<OPTIONS>` is the input options. `<OPTIONS>` is optional in both examples.

# <a id="jquery" href="#jquery">jquery.Citation.js</a>

This plugin builds a form for input for the `Cite` object.

## <a id="jquery.use" href="#jquery.use">Use</a>

After including the necessary files like below,
you can make a new `jQueryCite` object.

```html
<script type="text/javascript" src="path/to/jquery.js"></script>
<script type="text/javascript" src="path/to/citeproc.js"></script>
<script type="text/javascript" src="path/to/citation-0.2.js"></script>
<script type="text/javascript" src="path/to/jquery.citation-0.2.js"></script>
```

### <a id="jquery.cite" href="#jquery.cite">jQueryCite</a>

Make a new `jQueryCite` object like this:

```js
var example = new jQueryCite( <options> )
```

The options are:

1. `defaultOptions`: [Options](#citation.cite) to be passed to `Cite`
2. `saveInCookies`: Save data in cookies when `.save()`d
3. `add`: Callback to execute when data is submitted to collection
4. `inputForm` and `outputForm`: HTML template (see [docs](#jquery.form))

See also [API Docs](jQueryCite.html#jQueryCite)

### <a name="jquery.form"><a id="jquery.cite.form" href="#jquery.cite.form">HTML templates</a></a>

Of course, you can include all sorts of things in the templates, but the following things are going to get used. Templates below are in [Jade/Pug](https://github.com/pugjs/pug). Elements may be wrapped in containers, but the general hierarchy should be like this

#### <span name="jquery.form.in"><a id="jquery.cite.form.in" href="#jquery.cite.form.in">Input form</a></span>

```
.cjs-in
  .cjs-piece.cjs-input
    // List of fieldsets, see below
  .cjs-piece.cjs-import
    .cjs-import-name // Text input to hold input value names
    .cjs-import-file // File input
  .cjs-piece.cjs-preview
    .cjs-draft // Element holding draft
    .cjs-add // Submit (and clear) draft
    .cjs-delete // Clear draft
```

##### <a name="jquery.form.in.fields"><a id="jquery.cite.form.in.fields" href="#jquery.cite.form.in.fields">Input form fields</a></a>

Form fields consist of a `fieldset` element and inside an `input` element, with the following attributes:

* `fieldset`
  * `data-cjs-field-type`: For what publication types should this field be visible (omit when it should always be visible)
  * `data-cjs-field-state`: `"hidden"`, omitted or `"visible"`. Assigned by program
* `input`
  * `data-cjs-field`: Name of the corresponding CSL property. When CSL properties are complex, `jQueryCite` usually helps out
  * `type`: Usually `"text"`, but depends on CSL property. If it gives input to jQuery in the correct format, it's okay

Exceptions:

* The field or the `page` property should have two `input`s
* Fields `author`, `container-author`, `editor` and `publisher-title` get `.CJSMultipleInput()`, so multiple `input`s aren't necessary, as they're added dynamically
* One of the fields, preferably the first one, should be a `select` (`data-cjs-field="type"`), containing publication type options

#### <a name="jquery.form.out"><a id="jquery.cite.form.out" href="#jquery.cite.form.out">Output form</a></a>

```
.cjs-out
  .cjs-piece.cjs-settings
    .cjs-opt
      fieldset
        select.cjs-type // HTML text or plain text
          option(value="html")
          option(value="string")
      fieldset
        select.cjs-style // Formatting style
          option(value="citation.apa")
          option(value="citation.vancouver")
          option(value="citation.harvard1")
          // Formatted citations can be expanded, if correct material is provided to Cite
          option(value="bibtex")
          option(value="csl")
      fieldset
        select.cjs-lan // Output language
          option(value="en-US")
          option(value="es-ES")
          option(value="du-DU")
          option(value="fr-FR")
          option(value="nl-NL")
          // Langs can be expanded, if correct material is provided to Cite
  .cjs-piece
    .cjs-output // Holds data
  .cjs-piece.cjs-export
    .cjs-export-copy // Copy data on click
    .cjs-export-save // Download data on click
```

## <a id="jquery.dependencies" href="#jquery.dependencies">Dependencies</a>

* jQuery
* Citeproc-js
* Citation.js

# <a id="more" href="#more">More</a>

## <a id="more.docs" href="#more.docs">More Docs</a>

Further explanation can be found [here](https://larsgw.github.io/citation.js/api/). The explanation of the jQuery plugin can be found there too.

## <a id="more.demo" href="#more.demo">Demo</a>

### <a id="more.demo.npm" href="#more.demo.npm">NPM Demo</a>

[NPM Demo](https://runkit.com/npm/citation-js). Example code:

```js
var Cite = require('citation-js')

var data = new Cite('Q21972834', {
  format: 'string',
  type: 'html',
  style: 'citation-apa',
  lang: 'en-US'
})

data.get() // Should implicitly display
```

### <a id="more.demo.browser" href="#more.demo.browser">Browser Demos</a>

* [Normal demo](https://larsgw.github.io/citation.js/demo/demo.html)
* [Demo including jQuery plugin](https://larsgw.github.io/citation.js/demo/jquery.html)