+++
title = "weesnaw/pebbit"
+++

If you want to use tags with `/` in them, you'll need to do something like this.  In the frontmatter of your work you would write:

```
tags = ["weesnaw-pebbit", "au", "complete"]
```

Once you create the `weesnaw-pebbit/_index.md` file and make the title `"weesnaw/pebbit"`, the tag will show up properly as `"weesnaw/pebbit"`.  (Tags without the `/` character should Just Work without you needing to create any files for them, although you can also do that if you want to write text on the tag page, like this.)
