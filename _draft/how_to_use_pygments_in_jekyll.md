###How to use Pygments in Jekyll

The Kramdown will parse the code as below.

```
    <div class="highlight"><pre>
    ...
    </pre></div>
```

So the css class name should be named as *.highlight*.

We can generate the css with vim style in the following way. 

```
$ pygmentize -f html -S vim -a .highlight > vim.css
```

The vim.css need be linked in the template file in the _layout folder.

```
    <!-- syntax highlighting CSS -->
    <link rel="stylesheet" href="/css/vim.css">
```