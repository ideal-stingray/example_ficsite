# Seize the means of archive, or: how to make a simple static fanfiction website

In the wake of the current AO3 outage, many of us are thinking, _Fuck, I should've had a backup option for hosting my fanfiction_.  Well.  Many of _you_ are thinking this.  [_I_ already had one](https://realvirtuality.net/fic/).

This is a dirt-simple static site built with the Hugo static site generator.  I host mine on a DigitalOcean droplet (a virtual Linux box in \~the cloud\~) for about $5/month but static sites are simple enough you can also yeet them up on GitHub Pages for free.  It's easy!  It's fun!  It's a great way to procrastinate on your WIPs!  Take my hand, please, and make an archive of _your very own_.

## Prerequisites

Unfortunately you will need to use the command line for a lot of this, and I don't know how the fuck Windows machines work, so if you're on Windows you're gonna need to set up WSL (which makes Windows pretend to be Linux so that little babies like me don't get confused).  If I recall correctly the WSL install takes a hot minute to get itself sorted out, so go kick that off and then come back.

You will also need:
* an email address for your fannish pseudonym
* a [GitHub](https://github.com/) account for your fannish pseudonym (use your fannish email for this)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Hugo](https://gohugo.io/installation/), and [Pandoc](https://pandoc.org/installing.html) --- install these however is appropriate for your OS
* a text editor --- this is _not_ a word processor and _not_... whatever Ellipsus is.  It's whatever software comes up if you open a `.txt` file.  If you want to install a new one for whatever reason (the default is probably going to be super ugly and might not have syntax highlighting for Markdown), the free version of [Sublime Text](https://www.sublimetext.com/) is fine.[^1]

_A word on Git._  Git is a powerful piece of version-tracking software used by programmers, which unfortunately means it's easy to get into trouble with it, and this margin is too small to contain a full Git tutorial (although our fine fujoshi warriors at [fujoweb.dev](https://learn.fujoweb.dev/) are working on it!!).  If Git did something weird and you're not sure what happened, my askbox and DMs are always open.

## Forking the Example

In order to make things as simple as possible I've made an [example website](https://github.com/ideal-stingray/example_ficsite) for you to copy and edit as needed.  It'll look a little something like [this](https://ideal-stingray.github.io/example_ficsite/) --- of course you can (and should!) fully customize the colors, layout, etc. to suit your personal tastes.  But let's start by getting you a personal fork.

Log into your fannish GitHub account, go to the [example website repository](https://github.com/ideal-stingray/example_ficsite), and click the "Fork" button in the top right corner.  This will make a copy of the repository that belongs to _your_ account, and you can do whatever you like there.  You can also set the repository name to whatever you want, but I will proceed under the assumption that you've called it `ficsite`.

**Important note:** With the free version of GitHub, the source for a GitHub Pages site has to be a _public_ repository, which means that all of the files in that repository that _have ever been_ "checked in" or "tracked" by Git will be publicly viewable.  Be careful not to accidentally check in a file with personal information like your wallet name!  If you're not sure you've done it right, you can always ping me on Tumblr and I'll look it over for you.

Now that you have your own fic repo, head to the repository settings and enable GitHub Pages (under "Code, planning, and automation").  For Source, the default "Build from a branch" setting is fine.  Under Branch, select the `main` branch and the `/docs` folder.  Then hit Save.  GitHub will take a few seconds to think about its crimes and then (hopefully!) deploy your fic website at `<USERNAME>.github.io/ficsite`.  (It'll look exactly like mine, since you haven't made any changes yet.)  You can check on the deployment status in the sidebar on the repository's main page, under "Deployments" --- when it's done thinking you'll see a green checkmark next to the `github-pages` deployment.

The next step is to get a copy of the fic repository on your actual computer, and for this you will need to add an SSH key to your GitHub account.  This margin is too small to fully explain how SSH keys work --- basically, they're a cryptography thing that lets GitHub know it's you trying to upload things and not anyone else.  The important thing to know is that SSH keys come in pairs: a public key (which you can upload to your website, send to your friends, print on a giant banner and hang above your house, etc) and a private key (which you should never send to anyone, ever).  You can easily tell them apart because the public key will have `.pub` at the end of it.

GitHub has guides for [generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and [adding it to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).  You should generate the SSH key like so:

``` 
ssh-keygen -t ed25519 -C <YOUR PSEUDONYMOUS EMAIL>
```

When this prompts you to enter a file to save the key in, tell it `~/.ssh/fic_id_ed25519`.  Now, if you look in your `.ssh` directory, you will find two files: `fic_id_ed25519` and `fic_id_ed25519.pub`.  These are just regular text files, and you can open them in your text editor if you want to look at them.  Here's what my public key looks like:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEhi/SysXShl5fZAGhcDTY9bYUEQT40Fdju6P1uN4e7F ideal-stingray@protonmail.com
```

(It's a good idea to double-check this has the correct email in it.  If it doesn't, delete it and try again.)

Once you have your key, follow the [GitHub instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) for adding it to the ssh agent, then add the public key to your [GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

Now you can download your repo!  On the repo page, click the green "Code" dropdown and copy the SSH "Clone" link --- it should look like `git@github.com:<YOUR USERNAME>/ficsite.git`.  (If you use the HTTPS link, you will have problems later when you want to upload your changes.  Double-check that the link starts with `git@github.com`.)  Then, at the command line, go to the folder where you'd like to download your repo (you can use `cd` to go to a different folder in your filesystem, `pwd` to see what folder you're in, and `ls` to see what files are in the folder) and run:

```
git clone <SSH link>
```

When this is done downloading and unpacking everything you should see a new folder called `ficsite` with all the website files in it.

**!!!IMPORTANT!!!** Before you get sidetracked doing _anything else_, the first thing you're going to do with this is to edit the git config for `ficsite`, which is stored in `ficsite/.git/config`.  Note that `.git` is a hidden directory because it starts with a `.`, so you'll need to make your file browser show you hidden directories in order to find it.  You need to edit this config before doing anything else with Git --- Git attaches a name and email to every version ("commit") in its version history, and if you don't tell it which ones to use, it'll try to make something up based on your computer's username and hostname.  So if that's your wallet name, you're in trouble.

Right after you've cloned the repo, when you open the git config file in your text editor, it will look something like this:

```
[core]
  repositoryformatversion = 0
  filemode = true
  bare = false
  logallrefupdates = true
[remote "origin"]
  url = git@github.com:<YOUR USERNAME>/example_ficsite.git
  fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
  remote = origin
  merge = refs/heads/main
```

Under the `[core]` section, add a line like this:

```
  sshcommand = ssh -i ~/.ssh/fic_id_ed25519
```

Add a new section for your user info:
```
[user]
  name = <YOUR PSEUDONYM>
  email = <YOUR PSEUDONYMOUS EMAIL>
```

Phew.  We're safe.

## Structure

Okay, there are a lot of files in `ficsite`.  What are all those files doing?

Let's first take a look at `hugo.toml`, which is a configuration file at the top level of the `ficsite` repo.  If you open it up in your text editor it'll look something like this:

```
baseURL = "https://ideal-stingray.github.io/example_ficsite/"
locale = "en-US"
title = "archive of MY own"  # Change this to be whatever you want
theme = "archive"  # This must be a theme under themes/
...
```

You should update the `baseURL` to be the URL of your website: `https://<YOUR USERNAME>.github.io/ficsite/`.  A little further down under `[params.author]`, put in your fannish pseud and email for the `name` and `email` fields.  You shouldn't have to worry about most of the rest of this for now, although you can update the title if you want.

One thing Hugo tries to do is make it easy to configure different themes for your website, and there are [one william](https://themes.gohugo.io/) free and open-source themes you can just plug in.  Most of these are more geared towards personal blogs than fic archives, which is why I've started you off with the custom `archive` theme, but if you want a new one, download it into the `themes/` folder and update the `theme` in your Hugo config.  In addition to the layouts provided by the theme, you can also add new ones in the top-level `layouts` folder --- if you poke around in there, you'll see that I've done this for the "fandoms" and "tags" pages to configure a sort order.  The layouts in your top-level folder will override layouts that are provided by your current theme, which lets you add customizations on top of the theme.  (Or you can just edit the theme directly.)

The `archive` theme provides a CSS file in `themes/archive/assets/css/main.css`, which I've tried to make easy to edit --- there are variables up at the top to set the background color (which the text color is based off of), font family, and various spacings.  Another thing I've started you off with is a `robots.txt` that excludes the most common LLM crawlers, which you can find under `themes/archive/layouts/robots.txt`.  Update this however you like --- for example, you might want to disallow Googlebot or some additional LLM crawlers.  If you switch themes, you'll probably want to copy the `robots.txt` into your top-level layouts folder.

The most important folder is `content/`, because that's where all the fanfiction goes :)  These are the Markdown files Hugo uses to generate your website: `content/_index.md` goes on the homepage, `content/about/_index.md` goes on the about page, etc.  All the fanfiction lives under `content/works/` --- for single-chapter works you can just toss in a Markdown file; for multi-chapter works you will want to make a subfolder and then include Markdown files for the individual chapters, plus an `_index.md` file for work-level notes and metadata.  Hugo will generate placeholder tag/series/fandom pages on its own, but I've included sample custom tag and series pages under `content/tags` and `content/series` respectively (note that you will need a custom tag page for any tag that has a `/` character in it).

At the top of nearly every markdown file in `content/` you will find the _frontmatter_, which is surrounded at top and bottom by `+++` and defines metadata about the work (or page, or whatever).  This is where you put your title and publication date, plus other metadata like fandoms/series/tags.

I'd recommend you get started by exporting a few of your fics to markdown and adding them to `content/works/`, cribbing off of the example works I added.  (If you want to hide the examples afterwards you can either delete them or set `draft = true` in the frontmatter.)

## Generating and Uploading the Website

In a new terminal tab (or window), you can run the Hugo development server with `hugo server` (run this from the top-level `ficsite` folder).  This is a **local build** of your website --- it will live only on your computer, and you can look at it in your browser, tweak it as much as you want, and see your changes live.  However, your friends will not be able to see it yet.  By default it will show up at `http://localhost:1313/ficsite` (just type that into the address bar of your browser).

In general, the dev server will watch your files for any changes and hot-reload the website, but if you're tweaking CSS (e.g. if you want to change the background color) occasionally you will need to hard-refresh the page (shift+ctrl+R) to get your changes to show up.  Otherwise, you can just keep the server running while you work on importing your fic and watch as more content shows up.

When you're happy with what you've done, you can generate the "production" version of the website with `hugo build`.  This will generate a pile of new HTML files under `docs/` --- if you're not sure if it worked, open up one of those HTML files with your text editor or browser and see if it's got stuff in it.

Then go back to your `ficsite` directory and run:

```
git add .
git commit -m "<message goes here>"
```

Everything after the `-m` is your "commit message", which is a brief description of the change you just made (probably something like "Upload all the Blorbo Blinkus fic from 2013" or "tweak css again").  It will be publicly viewable to anyone who looks at your repository on GitHub, so don't, like, put your full home address in it, but otherwise it's just a note for you to remember what's in this change.

You can also run just `git commit` --- this will make Git pop open a text editor for you to write your commit messages.  This is useful if you have a lot to say in your message, but the default text editor is typically either a somewhat confusing terminal-based app called `nano` or a _very_ confusing terminal-based app called `vim`[^2].  You can configure a different text editor in `.git/config` by adding another line to the `[core]` section:

```
[core]
  ...
  editor = <YOUR EDITOR>
```

The editor command should be whatever you'd run to open your text editor from the command line --- if you're using Sublime Text this is, IIRC, `subl`.  But if you don't want to worry about any of this, you can stick with `git commit -m` to write all your commit messages and nothing bad will happen.

Anyway!  You've now made your first commit.  If you type `git status` now, you should see something like this:

```
On branch main
nothing to commit, working tree clean
```

And `git log` will show you the last few commits, which should look something like this:

```
commit b894564e205f83056b474548e4b44841723bd819 (HEAD -> main, origin/main)
Author: Ideal Stingray <ideal-stingray@protonmail.com>
Date:   Sun Nov 1 16:24:07 2026 -0800

    add README

commit 5300549d827f60eff93320b200a1eb6a418ab896
Author: Ideal Stingray <ideal-stingray@protonmail.com>
Date:   Sun Nov 1 15:39:14 2026 -0800

    build docs
```

Before you upload this, it's best to triple-check at this point that the author name and email are your fannish pseudonym and not your wallet name.  If you _did_ accidentally put your wallet name in your commit, undo it with `git reset --soft HEAD~1`, edit the config, and redo it.  Be careful not to accidentally run `git reset --hard`, which will delete everything you just did, and you'll have to go on a search-and-rescue mission in the reflog to get it back.

If you don't see your new commit message under `git log`, it means that something went wrong and the commit didn't actually go through.  Try running `git status` to see what's going on --- if your directory is "dirty" but you don't see anything under "Changes to be committed", you probably need to run `git add .` again (from the top-level `ficsite` folder) and then commit.  For our purposes, the only thing you need to do with Git is "make a new commit with all my changes since the last one", which you can do with `git add .` and `git commit`, but if you want to know more about what's going on, you can consult the [official docs](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository).

Assuming all that worked, it is _finally_ time to upload to GitHub and publish your website:

```
git push
```

(If you added a passphrase to your SSH key when you created it earlier, this will probably prompt you to type it in again.)

Once the deployment finishes (and the little green checkmark appears, as above), your fic should be live at `<USERNAME>.github.com/ficsite`.  Yay!!

Every time you change something on your computer, you'll need to go through this same three-step procedure:

1. Regenerate the production site (`hugo build`)
2. Commit (`git add .`, `git commit -m "<message>"`) --- make sure this grabs all the new changes in `docs/`
3. Push (`git push`)

If you made a change that shows up in the local `hugo server` copy but not on GitHub Pages, go check if you forgot to run `hugo build`.

## Parting Remarks

Okay, I think that's all I have to say!  Good luck, enjoy the power and majesty of the Computer, and if you want help with any of this, my askbox and DMs are open :)

---

[^1]: Technically inclined friends, please do not yell at me about Sublime Text not being open-source; I picked it because it's available cross-platform, has reasonable non-hideous defaults, and is less scary-looking than VSCode or whatever.  If you have strong opinions on open-source software, presumably you already have a favorite text editor.  If not, can I interest you in trying Vim---

[^2]: I write all my code _and_ all my fanfiction in Vim, because there's something fundamentally wrong with me.  If you get stuck in Vim by accident, you can get out by hitting the Escape key and then typing `:q!`.
