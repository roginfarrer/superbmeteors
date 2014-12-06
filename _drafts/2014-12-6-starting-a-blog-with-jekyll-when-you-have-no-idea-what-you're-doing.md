---
layout: post
Title: "Starting A Blog with Jekyll When You Have No Idea What You’re Doing"
category: Web Development
category_slug: web-development
permalink: getting-started-with-jekyll
description: For a few years now I’ve called myself a writer. But it seems that I’ve been writing less and less... But with a revamped roginfarrer.com, I’m making a comeback. This is hopefully the first post of what will be a regular chain of posts for a good while.
---

Last week I rebuilt this site on [Jekyll](http://jekyllrb.com), and it’s freaking awesome.

I’ve heard the name Jekyll tossed around on the interwebs for a while now, but I never completely understood what it did. When I pulled up their homepage, I immediately closed the page with just a hint that I would have to use Terminal.

But the truth is, Jekyll isn’t all that complicated, once you break it down into its parts. It took me a whole day to figure it out, port over my css and html, and configure it for live deployment. But that was because I had about 10 tabs open on trying to figure out what to do. 

So for all you non-programmers interested in Jekyll, this is for you.

**HOLD!** I want to reiterate that *I’m* no programmer. I’m only somewhat familiar with Javascript and PHP. I can’t tell you how the engine behind Jekyll works. ***But,*** I can get you up in running in laymen’s terms.

#### [Here we go!](http://youtu.be/k900hqBNc14)

## What we’re doing

Okay, so there’s many ways to build a Jekyll website. This process will all depend on where you want to host your site and how you like to work with your stylesheets. So to clarify, here’s what I’m going to explain:

1. How to install Jekyll to a local directory
2. How to incorporate [Compass](http://compass-style.org) and [SASS](http://sass-lang.com) into the workflow
3. Setting up Github Pages hosting
4. Configuring Github for Mac to easily deploy your local repository.
5. Setting up Amazon S3 and Cloudfront for a quick and dirty CDN.

Whew! Okay, now we can start.

## What you need to know first

Let’s first define Jekyll in it’s simplest terms: **Jekyll is “a simple, blog aware, static site generator.”**

If you’re a casual designer/web coder like me, you’re somewhat familiar with this generation concept. In Wordpress, for example, themes include a `header.php`, `index.php`, and a `footer.php`. When you visit your webpage, Wordpress then merges all these files together, the PHP generating HTML that is spit out to your browser.

Jekyll works in a similar way. *Except there’s no PHP.* Instead of calling the header with something like `<?php get_header(); ?>`, we will use Liquid tags, which look like this: `{ % include header.html % }`.

So now we understand two portions of the Jekyll directory. We have a `_layouts` folder that holds are page templates (i.e., posts, pages, homepage, and more). We also have an `_includes` folder for modules we use across every page, like the HTML head, header, and footer. 

We also have a `_posts` folder for our blog posts, a `_sass` folder for our not-yet-synthesized SCSS files, and a `css` folder for our synthesized CSS files.

**HOLD!** If you’re not already familiar with the SASS language, I highly suggest you read through their website. It’s pretty easy to pickup, and it makes organizing your styles *way easier* than with plain CSS.

We also have a `_config.yml` file which we’ll use later to configure our site’s “settings.” (I use quotations because that’s not *really* what it’s called, but it’s easier to understand that way.)

And finally, there’s a `_site` folder that functions similarly to a `public_html` folder. It’s where Jekyll synthesizes our static site. We’ll leave this folder alone so Jekyll can do it’s thing without us getting in the way.

## Setting up the local directory

Navigate to where you want your Jekyll installation to live, and create a folder for the project. I’ll call it `jekyll` for simplicity’s sake.

Now open up Terminal. It’s time to install Jekyll and some of the tools we need. 

### Install Jekyll

In Terminal, enter these lines to create a Jekyll installation:

{% highlight bash %}
cd /your/folder/location/jekyll
sudo gem install jekyll
jekyll new .
{% endhighlight %}

With this we navigated to the folder that we want to install jekyll into, we installed Jekyll (`sudo` to bypass permission errors), and created a new Jekyll repository in our folder. At this point, Jekyll has built a skeleton directory in the folder. I encourage you to stop at this point and poke around and get familiar with the file structure.

### Install RDiscount

If you want to write in Markdown, we need to install RDiscount. This gem processes Markdown and converts it to HTML. So in the same Terminal window:

{% highlight bash %}
sudo gem install rdiscount
{% endhighlight %}

### Installing Compass and Sass

This one took me a while to figure out, because when you install Compass and create a build, it adds its own SASS and stylesheet folders. It took me a little while to figure out how to configure Compass to find my Sass files and then synthesize them into the right folder. So I’ll break it down.

{% highlight bash %}
sudo gem install sass
sudo gem install compass
compass create
{% endhighlight %}

Okay, so at this point Compass has added three folders: `config.rb`, `sass` (as distinct from `_sass`), and `stylesheets`. It’s now time to decide how you want to organize your assets. I decided to keep the structure from the Jekyll installation, so I deleted `sass` and `stylesheets`. Then open `config.rb` in a text editor.

{% highlight ruby %}
# Change line 6 to:
css_dir = "css"
# Change line 7 to:
sass_dir = "_sass"
{% endhighlight %}

Compass should now do its job correctly.

### Now for _config.yml

In this file, you configure a lot of the major variables for your site, like its URL path, name, description, directory, etc. There’s a bunch of things you can do here, but to get you started, here’s what I use:

{% highlight ruby %}
source:      .
destination: ./_site
includes:    ./_includes

markdown:      rdiscount
permalink:     /:title.html

name: RoginFarrer.com
base_url: ""
url: http://roginfarrer.com
description: "A blog covering Tech news, design, productivity, health, and the arts by Rogin Farrer."
root_desc: "RoginFarrer.com — Tech, Design, and Theater"

exclude: ['Rakefile', 'README.markdown', 'config.rb']
include: ['.htaccess']

paginate: 5
{% endhighlight %}

Be sure to change the variables like `name` to your own!

### Local repository complete!

We’ve now got our local directory up and running! When you’re ready to open your site locally, open your terminal to a new window.

{% highlight bash %}
cd /your/folder/location/jekyll
jekyll serve -w
{% endhighlight %}

Terminal will start running Jekyll! It will also spit out a local web address like `http://127.0.0.1:4000/` which you can enter to your browser. BAM! There’s your site!