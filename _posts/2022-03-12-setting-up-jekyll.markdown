---
layout: post
title: "Setting up Jekyll"
date: 2022-03-17 19:13:27 -0600
categories: jekyll
---

Setting up this blog required me to google around a bit, so I figured I would make a step by step guide on how to get a [Jekyll](https://jekyllrb.com/) blog created, running locally on mac, and deployed to github pages.

# **1) Install homebrew (if you dont already have it)**

The link to homebrew is [here](https://brew.sh/)

# **2) Install Ruby**

Once you have homebrew installed

Run `brew install ruby` in termial to install ruby

# **3) Ensure you have the latest version of Ruby**

- Run `ruby -v` in terminal

- If version number is less than 3 link ruby by running: `brew link --overwrite ruby --force` in terminal

- Run `ruby -v` in terminal and confirm verison number is greater than 3

# **4) Create a new GitHub Repository**

1. [Create](https://docs.github.com/en/get-started/quickstart/create-a-repo) a new GitHub repository
2. `git clone <repository name>` to your local machine

# **5) Install Jekyll**

1. `cd <new repository name>`
2. Run `gem install bundler jekyll`
3. After the install is done run `jekyll new .`

At this point, you should have your directory populated with the default jekyll files.

# **6) Add webrick**

Open the `Gemfile` file in your newly populated directory and add the following line: `gem "webrick"`

I added webrick to the Gemfile so I could run the site locally. This may or may not be necessary depending on your setup.

# **7) Run the site locally**

1. Run `bundle update` to build
2. Start your site with `bundle exec jekyll serve`
3. Open a browser and navigate to `http://localhost:4000`

At this point you should see your site running locally.

# **8) Push and Deploy**

1. Push the changes to GitHub
2. Deploy to GitHub Pages using this [guide](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site#creating-your-site)

# **9) Final Remarks**

Congrats you have created a [Jekyll](https://jekyllrb.com/) site!

Some helpful links I found when looking to make edits to my site:

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Jekyll GitHub](https://github.com/jekyll/jekyll) has a very thorough readme
- Andrei Karpathy's blog [repository](https://github.com/karpathy/karpathy.github.io)
