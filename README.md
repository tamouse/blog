# blog.tamouse.org

~~This is a pretty straight-forward jekyll site, just run `rake serve` for a dev server, `rake build` to build out the site. The site is configured to be a Github Pages site, with the actual served web pages in the `docs/` folder.~~

Astro-izing this blog - turning it into a different static site generator.
- AstroJS is a NextJS app + framework that lets you create components in React, which is what I'm looking for in this case. It has a bunch of other features that I'm not interested in right now.

## Approach

My first instinct was to create a brand new directory set up with Astro, but then I wanted to see if this could be done overlaying the current directory.

It's in a separate branch, so I'm hoping it won't close the door on going back. 

## Step one: getting the lay of the land

I need to take an inventory of the current design and architecture to see what will need to be reproduced in React. There a few structures I built originally for Jekyll in the Liquid templating language for example.

- Posts and pages are all in markdown (excellent)
- Images seem to mostly be in AWS S3 (good)

## Step two: change the directory into an AstroJS app

I'm hoping it's as simple as I think it should be. There is a page on the AstroJS site that talks about importing from Jekyll.

## Step Three: convert front matter

The Jekyll and Astro front matter sections are just wildly different. I don't think there's really an analogous mapping here. Strategies for what was used in YAML and consumed by Liquid may not work, though some will be analogous, but it also should have a good look to see if there are even better ways to handle things.

Especially, image management (display, resizing, etc) are far in advance of what I was doing way back in the 2010s. 
