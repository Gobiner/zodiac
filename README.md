# zodiac

## SYNOPSIS

ZODIAC is a static website generator powered by sh and awk.

## INSTALL

    git clone ssh://github.com/nuex/zodiac.git
    
Edit the config.mk file to customize the install paths

    sudo make install

## USAGE

    zod projectdir targetdir

A typical Zodiac project will look something like this:

    site/
      index.md
      index.meta
      main.layout
      global.meta
      projects/
        project-1.md
        project-1.meta
        project-2.md
        project-2.meta
      cv.md
      cv.meta
      stylesheets/
        style.css

### Meta

`.meta` files contain a key / value pair per line. A key and its value must be separated by a ": ". A metafile looks like this:

    this: that
    title: Contact
    author: Me

Each markdown page can have its own meta file. The only requirement is that the meta file is in the same directory as the page, has the same name as the page and has the `.meta` file extension.

The optional `global.meta` file contains data that is available to all of your site's pages, like a site_title.

Page metadata will always override global metadata of the same key.

### Layouts

A `main.layout` file should look something like this:

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <link rel="stylesheet" href="/stylesheets/style.css" />
        <title>{{page_title}}</title>
      </head>
      <body>
        <header>
          <h1><a href="/">{{site_title}}</a></h1>
        </header>
        <article>
          {{{yield}}}
        </article>
        <footer>
          <p>powered by static files, compiled by <a href="http://nu-ex.com/projects/zodiac">zodiac</a>.</p>
        </footer>
      </body>
    </html>

### Helpers

The `helpers.awk` file is an awk script that can make custom data available to your templates. You also have access to the page and global data. Here is a peak at the script included in the examples folder:

    { helpers = "yes" }

    function load_helpers() {
      # your custom data settings
      data["page_title"] = page_title()
    }

    # your custom functions
    function page_title(  title) {
      if (data["title"]) {
        title = data["title"] " - "
      }
      title = title data["site_title"]
      return title
    }

Just be sure to set the data array in the load_helpers() function at the top of the script to make your custom data available to the template.

### License

MIT