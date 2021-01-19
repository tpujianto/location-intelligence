---
title: "Introduction to Reveal.js"
date: 2020-12-21T23:21:50-05:00
draft: false
---

\
[Reveal.js](https://revealjs.com/) is an open-source presentation framework that enables you to create interactive, highly customizable presentations on the web. For this tutorial we will use the Reveal framework within the context of a static site generator called Hugo! Using Hugo and Reveal together allows us to publish our work to the web for free.

### Create Github Repo

First create a github repository (commonly referred to as a 'repo') – following the instructions outlined [here](https://docs.github.com/en/github/getting-started-with-github/create-a-repo). Ensure the visibility of the repo is set to *public*.


### Cloning a repo

Next clone the newly created repo on your laptop following the steps outlined [here](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository).


### Install Hugo

[Hugo](https://gohugo.io/) is an opensource static site generator that makes it easy to build website quickly with very little coding. To install follow the below instructions:

- For Windows users [follow this](https://gohugo.io/getting-started/installing/#less-technical-users)
- For Mac users: first [install homebrew](https://brew.sh/) by typing the below command into your terminal, then install Hugo [following this](https://gohugo.io/getting-started/installing/#homebrew-macos)

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```


### Setup the Hugo folder

In your terminal (or command prompt in Windows) navigate to the repo you cloned earlier and run the following command.

```
hugo new site . --force
```

This commands sets up a new Hugo website template in this folder. Next navigate to the themes folder by typing

```
cd themes
```

Then clone the reveal github repo by typing

```
git clone https://github.com/dzello/reveal-hugo
```

Open the *config.toml* file and add the following contents to the bottom:

```
theme = "reveal-hugo"

[markup.goldmark.renderer]
unsafe = true

[outputFormats.Reveal]
baseName = "index"
mediaType = "text/html"
isHTML = true
```

Next create a new file in the *content* folder called *_index.md*

### Add content

Open the *_index.md* file and add the following content to create the first slide.

```
+++
title = "My presentation"
outputs = ["Reveal"]
+++

# Hello world!

This is my first slide.

```

To see how this looks in the browser, you'll need to create a local server. Go back to the top of the folder in your terminal or command prompt and type

```
hugo server
```

Finally, open up a browser window and navigate to 'http://localhost:1313/'  to see your presentation.

Slides in Hugo are created with the '---' symbol. So to add a new slide to the presentation add '---' to the *_index.md* file followed by some content

```
+++
title = "My presentation"
outputs = ["Reveal"]
+++

# Hello world!

This is my first slide.

---

# Second slide!

This is a test
```


### Example

An example of how your site should look locally is [here](https://carlobailey.github.io/reveal-demo/#/1)


### Customization

In Hugo files like the *_index.md* above, all content below and above the '+++' symbols are the meta-data that is used to customize and set parameters of your website. For example, to change the *theme* of the presentation add the below content to the *_index.md* file above the last '+++' symbol.

```
[reveal_hugo]
theme = "moon"
```

so now the top of your *_index.md* file should look like this

```
+++
title = "My presentation"
outputs = ["Reveal"]
[reveal_hugo]
theme = "moon"
+++
```

To see more theme and other customization options refer to the [documentation](https://revealjs.com/themes/)

There is also a demonstration of how to add customization options in Hugo [here](https://themes.gohugo.io/theme/reveal-hugo/#/30)
