# jQuery-ColumnView #

A simple script to transform a hierarchical HTML list into a Miller
Column UI (AKA, Mac OS X Finder style, NeXT style column view).

This plugin was expanded to include:
- using custom events instead of callbacks
- support a dblclick event
- loading of additional content with a custom callback (for example to load subtrees using ajax)
- navigating to a specific element
- clicking on nodes programmatically
- enabling/disabling adding CSS automatically

## Compatibility ##

The plugin works fine with jQuery 1.6.2. It requires jQuery 1.5.x or
newer because of its use of deferreds.

## Usage ##

### Simple list ###

Use a simple nested HTML list:

```html
      <ul id="list1">
        <li><a href="#">Node 1</a>
          <ul>
            <li><a href="#">SubNode a</a>
            </li>
            <li><a href="#">SubNode b</a>
            </li>
          </ul>
        </li>
        
        <li><a href="#">Node 2</a>
        </li>
      </ul>
```

And transform it into a columnview:

```js
$("ul#list1").columnview();
```

This will add the necessary CSS and render the arrow using canvas.

### Disabling CSS ###

In case you want to provide your own CSS for the columnview, and
disable the canvas arrow, you can disable them by passing the
appropriate options to columnview.

```js
$("ul#list2")
.columnview(
{
  addCSS: false,
  useCanvas: false
});
```

### Multi-select enabled ###

Multi-selection via the Shift or meta (Command/Apple on Mac, Control
on Windows/Linux) is available with all jQuery versions, though true
shift-clicking to select a range is only available with 1.4.x due to
changes in the `.index()` method.

```js
$("ul#list4").columnview({ multi: true });
```

### Fixed-width columns ###

By default on Firefox and Webkit browsers, columns will automatically
size horizontally to contain their contents. You may choose to use
fixed-width columns instead. On IE, Webkit and Opera, the text will
automatically be truncated with ellipsis. Note that columns are always
fixed-width on IE due to its handling of automatic widths for
absolutely positioned elements. I'm still looking for a reliable
cross-platform way to truncate text that performs well. You may notice
that text runs under the triangle widgets also. This is easy enough to
fix with CSS, though I leave that to you, since it won't work
consistently if you're using images as backgrounds in the widget.

```js
$("ul#list7").columnview({ multi: true, fixedwidth: '150px' });
```

### Loading custom data ###

If you want to provide your own data, you can override the `getSubtree` method:

```html
<ul id="list3">
</ul>
```

`getSubtree` takes the elt for which children have to be returned. The
function should return a list of jQuery `li` objects, with the
attribute `hasChildren` set to true if the nodes have children (thus
drawn with an arrow).

```js
$("ul#list3")
.columnview(
{
  getSubtree: function (elt, isRoot) {
    res = $('<li><a name="foobar">foobar</a></li>'
          + '<li><a name="bla">bla</a></li>'
          + '<li hasChildren="true"><a name="children">children</a></li>');
    return res;
  }
});
```

### Loading deferred custom data ###

Instead of returning data immediately, you return a deferred promise,
which makes AJAX loading easily possible.

```js
$("ul#list8")
.columnview(
{
getSubtree: function (elt, isRoot) {
   return $.get("test.html");
}
});
```

### Select callback ###

columnview triggers the `columnview_select` event when one or multiple
events are selected. It passes the selected elements to the event
callback. To check if the selected elements are leaf nodes or not,
query the element to see if it has the `hasChildMenu` class.

```js
$("ul#list5")
.columnview()
.bind("columnview_select", 
      function (ev, node) { 
         console.log(node); 
         console.log("is leaf node: " + !node.hasClass("hasChildMenu")); 
      });
```

```js
$("ul#list6")
.columnview({ multi: true })
.bind("columnview_select", 
      function (ev, nodes) { 
         console.log(nodes); 
      });
```

### Preview pane callback ###

In order to show something more interesting in the rightmost pane when
a user drills all the way down to the lowest level in a hierarchy, we
provide a callback to the 'preview' setting. The calling object is
passed to the preview function.

Here were using a simple function to display the text in reverse, but
you could use this callback to make AJAX calls to load external data,
etc.

```js
$("ul#list9").columnview(
{ preview:
  function (elt) {
    var reversed = $(elt).text().split("").reverse().join("");
    $(elt).parents().find(".feature").text(reversed);
  }
});
```

To disable the preview pane altogether, set `preview` to `false`;

### Double click callback ###

A double click is forwarded as a `columnview_dblclick` event.

```js
$("ul#list10").columnview()
  .bind('columnview_dblclick', 
         function (ev, elt) { 
            alert($(elt).text()); 
         });
```

## Ongoing Debate ##

Check out the [github page](http://github.com/wesen/jQuery-ColumnView).

Keep track of comments by non-github users at
[my blog](http://christianyates.com/blog/jquery/finder-column-view-hierarchical-lists-jquery).
