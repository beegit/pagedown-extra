#Overview

This is a collection of [Pagedown][2] plugins to enable support for 
Markdown Extra syntax. Open `demo/demo.html` to try it yourself.
To run the tests, just open `spec/SpecRunner.html` in your browser. Or, to
run a browser-less test from the command line as travis-ci would, run `bundle install` followed
by `bundle exec rake`. You'll need Ruby and the rake gem if you use
the second method.

These extensions work equally well with both the default and sanitizing converters.

![travis](https://secure.travis-ci.org/jmcmanus/pagedown-extra.png)

## Usage

In order to use the extensions, you'll need to include
`Markdown.Extra.js` after the Pagedown sources. Check out the
demo for a working example.

```javascript
var converter = new Markdown.Converter();
Markdown.Extra.init(converter, {extensions: "all"});
// options object is unnecessary in this case -- "all" is the default
```

Otherwise, you can choose from the extensions currently supported:

```javascript
// ascii tables
Markdown.Extra.init(converter, {extensions: "tables"});
// gfm fenced code blocks 
Markdown.Extra.init(converter, {extensions: "fenced_code_gfm"});
```

If you're using multiple converters on the same page, you can just call
`Markdown.Extra.init` once for each converter and you're all set.

### Tables

Markdown.Extra supports ascii-formatted tables:

```markdown
| Item      | Value | Qty |
| --------- | -----:|:--: |
| Computer  | $1600 | 5   |
| Phone     |   $12 | 12  |
| Pipe      |    $1 |234  |
```

Which will render to something like this depending on how you choose to style it:

| Item      | Value | Qty |
| --------- | -----:|:--: |
| Computer  | $1600 | 5   |
| Phone     |   $12 | 12  |
| Pipe      |    $1 |234  |

You can also specify a class for the generated tables using
`Markdown.Extra.init(converter, {table_class: "table table-striped"})` for instance.

Within markdown tables, markdown inside of table cells will also be converted. By
default a sanitizing converter is used, but you can change this by adding `sanitize: false`
to the options object passed to `init`. Only span-level tags are retained
inside of table cells, per the PHP Markdown Extra spec.

### Fenced Code Blocks

Fenced code blocks are supported &agrave; la GitHub. This markdown:

    ```
    var x = 2;
    ```

Will be transformed into:

```html
<pre>
    <code>var x = 2;</code>
</pre>
```

You can specify a syntax highlighter in the options object passed to `Markdown.Extra.init`,
in which case the html generated from fenced code blocks will be compatible with it.
Both [google-code-prettify][3] and [Highlight.js][4] are currently supported:

```javascript
// highlighter can be either `prettify` or `highlight`
Markdown.Extra.init(converter, {highlighter: "prettify"});
```

If either of those is specified, the language type will be added to the code tag, e.g.
`<code class="language-javascript">`. If `prettify` is specified, `<pre>` also becomes
`<pre class="prettyprint">`. Otherwise, the markup is the same as what Pagedown
produces for regular indented code blocks.  For example, when using
`{highlighter: "prettify"}` as shown above, this:

    ```javascript
    var x = 2;
    ```

Would generate the following html:

```html
<pre class="prettyprint">
    <code class="language-javascript">var x = 2;</code>
</pre>
```
##Extension / Option Reference
You can enable all of the currently supported extensions with `{extensions: "all"}`. This is also
the default. If specifying multiple extensions, you must provide them as an array. Here
is a list of the current and planned options and extensions. I've chosen to use the 
same naming scheme as the excellent Python Markdown library.

| Extension       | Description |
| --------------- | ----------- |
| fenced_code_gfm | GFM fenced code blocks |
| tables          | Pretty tables! |
| *def_list*      | Definition lists |
| *attr_list*     | Special attributes list for headers and fenced code blocks |
| *fenced_code*   | PHP Markdown Extra fenced code blocks |
| *smart_strong*  | No strong emphasis in the middle of words |

| Option          | Description |
| --------------- | ----------- |
| table_class     | Class added to all markdown tables. Useful when using frameworks like bootstrap. |
| highlighter     | Code highlighter. Must be one of `highlight` and `prettify` for now |
| sanitize        | Whether to sanitize inline html (in generated tables, for instance). Defaults to true. |

*Italicized are planned, and will be added in roughly the order shown*

See PHP Markdown Extra's [documentation][1] for a more complete overview
of syntax. In situations where it differs from how things are done on GitHub --
alignment of table headers, for instance -- I've chosen compatibility with gfm, which
seems to be quickly becoming the most widely used markdown implementation.

##License

MIT: http://justinm.mit-license.org

[1]: http://michelf.ca/projects/php-markdown/extra/#table "Markdown Extra Table Documentation"
[2]: http://code.google.com/p/pagedown/ "Pagedown - Google Code"
[3]: http://code.google.com/p/google-code-prettify/ "Prettify"
[4]: http://softwaremaniacs.org/soft/highlight/en/ "HighlightJs"

