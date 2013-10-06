# Why

Visual directory-structures are valuable in documentation, tutorials, and "how-to" type articles, as they provide clearer breaks in content and help show the user explicitly where to put a file or how a directory structure should look.

Creating and maintaining deply nested `<ul></ul>` tags is very tedius and complicated; folder-tree automates this process!

**Examples**

folder-tree is used extensively in the [Ruhoh Documentation](http://ruhoh.com/docs/2/) which also supports the [ruby version of folder-tree](https://gist.github.com/plusjade/6604117).


# Usage

- Declare folder-trees within `<pre class="folder-tree"></pre>` tags.
- Trees may be indented arbitrarily but all subsequent lines must indent correctly relative to the initial offset.
- Each line denotes a folder or file. 
- 2 spaces must be used for indentation. Tabs and other space lengths will throw a line error.
- Files are identified by having an extension name. This is not perfect but it's how it works right now.  
- Lines having only whitespace will be silently ignored.

<span></span>

    <pre class="folder-tree">
      app
        assets
          images
          javascripts
          stylesheets
        controllers
          concerns
        helpers
        mailers
        models
          concerns
        views
          layouts
          pages
          visitors
      bin
      config
        environments
        initializers
        locales
    </pre>

Load the script and initialize it relative to the aformentioned pre tags:

      <script src="../folder-tree.js"></script>
      <script>
        var elements = document.getElementsByClassName('folder-tree'),
            length = elements.length;

        for (var i = length - 1; i >= 0; --i) {
          var node = elements[i],
              container = document.createElement('span');
          container.innerHTML = folderTree(node.innerHTML);

          node.parentNode.replaceChild(container.firstChild, node);
        }
      </script>
    </body>


## Dependencies

folder-tree has no dependencies. Styling libraries are optional.

## Styling

**Icons**  
[Font-Awesome](http://fortawesome.github.io/Font-Awesome/) icon classes are used by the default HTML renderer.  
Enable the icons by installing [Font-Awesome](http://fortawesome.github.io/Font-Awesome/) on your website.

**Markup**  
The markup generated aims to be semantic and lean:

    <ul class="depth-0 folder-tree"> <!-- root UL node always included -->
      <li>
        <i class="icon-folder-close"></i> <!-- "icon" HTML renderer -->
        <span class="name">app</span> <!-- "name" HTML renderer -->
        <ul class="depth-1"> <!-- child UL nodes denote depth --> 
          <li>
            <i class="icon-folder-close"></i><span class="name">assets</span>
            <ul class="depth-2">
              <li><i class="icon-file-alt"></i><span class="name">pic.jpg</span></li> <!-- file node identified by extention -->
              <li><i class="icon-file-alt"></i><span class="name">hello.txt</span></li>
              <li><i class="icon-folder-close"></i><span class="name">javascripts</span></li>
              <li><i class="icon-folder-close"></i><span class="name">stylesheets</span></li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>


## Custom Markup

Markup is rendered via a standalone object of simple rendering functions:

- The argument `node` denotes a single line in your tree which is wrapped by `<li></li>` tags.
- The argument `nodes` denotes a collection of nodes at a given depth which is wrapped by `<ul></ul>` tags.

<span></span>

A node is an object:

    {
      // trimmed text found on the line
      name: "hello",
      // depth of this node starting from 0
      depth: 1,
      // number of indent spaces (used internally to verify indentation)
      indent: 2,
      // child nodes OR nil
      children : [<node>, <node>, ...] 
    }

The rendering functions :

    var Render = {
        // renders markup for the icon
        icon : function(node) {
            return node.children
                ? '<i class="icon-folder-close"></i>'
                : '<i class="icon-file-alt"></i>';
        }
        ,
        // renders markup for the node name (the text on each line)
        name : function(node) {
            return '<span class="name">' + trim(node.name) + '</span>';
        }
        ,
        // renders class list to insert on each unordered list tag.
        // note it's preferred to specify the depth here  so CSS can scope based on depth.
        classes : function(nodes) {
            var classes = "depth-" + nodes[0].depth;
            if (nodes[0].depth === 0) {
                classes += " folder-tree";
            }
            return classes;
        }
    }

Pass in a custom rendering object as the second argument to `folderTree()`. 
The custom object will merge with the default object so you may specify only functions you wish to override:

    folderTree(node.innerHTML, {
        icon : function(node) {
            return node.children
                ? '<i class="i-dont-like-fontawesome-folder"></i>'
                : '<i class="i-dont-like-fontawesome-file"></i>';
        }
        ,
        name : function(node) {
            return '<span class="name"> ===&#gt; ' + trim(node.name) + '</span>';
        }
    });

## License

[MIT](http://opensource.org/licenses/mit-license.html)


## Download

[https://github.com/plusjade/folder-tree](https://github.com/plusjade/folder-tree)
