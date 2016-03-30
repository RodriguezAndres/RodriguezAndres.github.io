---
layout: post
comments: true
title:  "Steps to start a basic GitHub blog using Jekyll"
date:   2016-03-30 01:00:00
mathjax: true
---

* TOC
{:toc}

## Installation
Make sure you install the latest version of ruby that is compatible with your OS. I run this
on Ubuntu 14.04

~~~ bash
# Adding the ruby repository
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
# Installing necessary packages
sudo apt-get install ruby2.2 ruby2.2-dev make gcc nodejs git
sudo gem update --system
sudo gem install jekyll
~~~ 

- Get a [GitHub](http://github.com) account with some *username*
- Set up a [new repository](https://github.com/new) named *username*.github.io
- Go to the folder where you want to store your blog files and clone the new repository

<p><div class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="nv"></span>git clone https://github.com/<em>username</em>/<em>username</em>.github.io
<span class="nv"></span>cd <em>username</em>.github.io
</code></pre></div></p>

## Quick start
From the folder <code>*username*.github.io</code>

~~~ bash
jekyll new . # only for new projects
jekyll serve
~~~ 

- Browse to [http://localhost:4000](http://localhost:4000) to view blog locally
- Create posts like this one under `myblog/_post` named `yyyy-mm-dd-name-of-file.markdown`

User adds blog entries in the `_post` folder and adds data or figures to a `data` or `figures` folder created by user, and Jekyll builds the `_site` content. Never edit the `_site` folder directly. More information at [Jekyll](http://jekyllrb.com/docs/structure/).

## Post to GitHub
Navigate to your <code>*username*.github.io</code> folder. It's a good idea to create a `.gitignore` file and ignore the `_site` directory and temporary files (e.g., `.swp`, `*~`, etc). The `_site` directory is build by Jekyll and used to run the blog locally; GitHub does not use it.

~~~ bash
echo "_site" >> .gitignore
echo "_post/*swp" >> .gitignore
echo "_post/*~" >> .gitignore
~~~ 

Git creates a local repository. Files can be *added* and *committed* to the local repository multiple times without affecting the master respository, i.e., the GitHub blog. To modify the actual blog, the local repository is *pushed* to the master repository.

First time push...

~~~ bash
git add --all
git commit -m "new blog post"
git push -u origin master
~~~ 

Second+ time push...

- add the modified or new files
- commit the changes to the local repository
- push local repository to the master repository


~~~ bash
git status # view the files that have been modified and decide which ones to add (see note below)
git add _post/mypost.md # use git add -A to add all modified files and delete deleted files
git commit -m "added cool stuff to mypost"
git push origin master
~~~ 
Note that every modified file (including those already in the local repository) needs to be added prior to committing (this is a different use of `add` than in SVN; in SVN only new files into the repo need are added).

For the curious, [here](http://stackoverflow.com/questions/572549/difference-between-git-add-a-and-git-add) is an example showing the various ways to add files

~~~ bash
git init
echo Change me > change-me
echo Delete me > delete-me
git add change-me delete-me
git commit -m initial

echo OK >> change-me
rm delete-me
echo Add me > add-me

git status
# Changed but not updated:
#   modified:   change-me
#   deleted:    delete-me
# Untracked files:
#   add-me

git add .
git status

# Changes to be committed:
#   new file:   add-me
#   modified:   change-me
# Changed but not updated:
#   deleted:    delete-me

git reset

git add -u
git status

# Changes to be committed:
#   modified:   change-me
#   deleted:    delete-me
# Untracked files:
#   add-me
~~~

Summary using `add`:

- `git add -A` stages All
- `git add .` stages new and modified, without deleted
- `git add -u` stages modified and deleted, without new


## Improving layout (optional)
To add a table of contents (more information [here](http://www.seanbuscay.com/blog/jekyll-toc-markdown/)), add the following snippet at top of the file under the header

~~~ bash
*TOC
{:toc}
~~~

To use TeX formatting, include in `_layout/post.html` (**important**: delete the space between `{` and `%`)

~~~ html
<!-- mathjax --\>
{ % if page.mathjax %}
<script type="text/javascript" src="//cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
{ % endif %}
~~~ 
To set the default format of images and caption, include in `css/main.scss` 

~~~ scss
.imgright { float: right; margin-left: 10px; }
.imgcap img {
  border: 1px solid #999;
  max-width: 100%;
}
.imgcap {
  color: #555;
  font-size: 14px;
  text-align: center;
}
~~~ 

To increase the width of the display, modify the `.wrapper` definition in  `_sass/_base.scss` replace the `* 2`'s with `* 1`'s

~~~ 
.wrapper {
    max-width: -webkit-calc(#{$content-width} - (#{$spacing-unit} * 1));
    max-width:         calc(#{$content-width} - (#{$spacing-unit} * 1));
~~~ 

To add a linkedin icon, include in `_include/footer.html` (**important**: delete the space between `{` and `%`) and in `_config.yml`, respectively

~~~ 
          { % if site.linkedin_username %}
          <li>
            <a href="https://www.linkedin.com/in/{{ site.linkedin_username }}">
              <span class="icon  icon--linkedin">
                <svg viewBox="0 0 16 16">
      <path fill="#828282" d="M0.119,5.093l3.224-0.04v10.556l-3.224,0.039V5.093L0.119,5.093z"></path>
      <path fill="#828282" d="M6.04,5.093l3.08-0.039v1.342l0.001,0.38c0.91-0.894,1.852-1.57,3.354-1.57c1.773,0,3.486,0.742,3.486,3.163v7.241 l-3.125,0.046v-5.532c0-1.219-0.307-2.005-1.764-2.005c-1.281,0-1.818,0.23-1.818,1.918v5.573L6.04,15.647V5.093L6.04,5.093z"></path>
      <path fill="#828282" d="M3.529,1.764c0,0.975-0.79,1.765-1.765,1.765C0.79,3.529,0,2.739,0,1.764S0.79,0,1.764,0C2.739,0,3.529,0.79,3.529,1.764 L3.529,1.764z"></path>
                </svg>
              </span>

              <span class="username">{{ site.linkedin_username }}</span>
            </a>
          </li>
          { % endif %}
~~~ 

~~~ 
linkedin_username: <em>linked_username</em>
~~~ 

## Adding a comments section (optional)
- Register your blog at [Disqus](https://disqus.com/)
- Copy the `_include/comments.html` file used for this blog to your `_include` folder replacing my disqus\_shortname with yours
- Include comments.html into your `_layouts/post.html` (show comments only on posts) or `_layouts/defaut.html` (show comments on all pages including home page) file (**important**: delete the space between `{` and `%`):

~~~ 
  { % include comments.html %}
~~~ 


## Kramdown tutorial
The kramdown syntax is a superset of the Markdown syntax, and most of the kramdown syntax is available in standard Markdown syntax. Below are the common formatting techniques that I use. For a more complete list see the kramdown [quick reference](http://kramdown.gettalong.org/quickref.html). 

~~~ 
Separate lines are concatinated in the blog, e.g.,
this shows up as one line
~~~ 

Separate lines are concatinated in the blog, e.g.,
this shows up as one line

~~~ 
Separate lines trailed by two spaces are not concatinated in the blog, e.g.,  
  this shows up as two lines
~~~ 

Separate lines trailed by two spaces are not concatinated in the blog, e.g.,  
  this shows up as two lines

~~~ 
**bold** or *italics* or **_both_** or <s>strikethrough</s>. 
Note that ~~strikethrough~~ should work but it's not working for me
~~~ 

**bold** or *italics* or **_both_** or <s>strikethrough</s>. 
Note that ~~strikethrough~~ should work but it's not working for me

~~~ 
> Indent and italicized text
~~~ 

> Indent and italicized text

~~~ 
Bullet points:

- bullet 1

- bullet 2
~~~ 

Bullet points:

- bullet 1

- bullet 2

~~~ 
Lists:

1. First
    * sublist item requires indent of four spaces
    * sublist another item
2. Second
    1. numbered sublist item 1
    1. numbered sublist item 2
1. Third (in Markdown actual numbers don't matter; only that is a number)
1. Forth
~~~ 

Lists:

1. First
    * sublist item requires indent of four spaces
    * sublist another item
2. Second
    1. numbered sublist item 1
    1. numbered sublist item 2
1. Third (in Markdown actual numbers don't matter; only that is a number)
1. Forth

~~~ 
Create a [hyperlink](http://hyperlink.com)  
  Create a [hyperlink](http://hyperlink.com "Title") that displays the title 
if you put your mouse over it
~~~ 

Create a [hyperlink](http://hyperlink.com)  
  Create a [hyperlink](http://hyperlink.com "Title") that displays the title 
if you put your mouse over it

~~~ 
<div class="imgcap">
<img src="/figures/tutorial.jpg">
<img src="/figures/tutorial.jpg" style="border:none;">
<img src="/figures/tutorial.jpg" width="10%">
<img src="/figures/tutorial.jpg" width="30%">
<img src="/figures/tutorial.jpg" style="max-width:10%; height:200px;">
<div class="thecap" style="text-align:justify">
Caption: Inserting figures with various settings: 
<b>(1)</b> Unaltered image.
<b>(2)</b> No border. 
<b>(3)</b> Width takes 10% of screen width.
<b>(4)</b> Width takes 30% of the screen
<b>(5)</b> Width takes 10% of the screen and the height is 200 pixels.
</div>
<div class="thecap" style="text-align:justify; font-size:90%">
And here is a different way to format the font size in captions
</div>
</div>
~~~ 

<div class="imgcap">
<img src="/figures/tutorial.jpg">
<img src="/figures/tutorial.jpg" style="border:none;">
<img src="/figures/tutorial.jpg" width="10%">
<img src="/figures/tutorial.jpg" width="30%">
<img src="/figures/tutorial.jpg" style="max-width:10%; height:200px;">
<div class="thecap" style="text-align:justify">
Caption: Inserting figures with various settings: 
<b>(1)</b> Unaltered image.
<b>(2)</b> No border. 
<b>(3)</b> Width takes 10% of screen width.
<b>(4)</b> Width takes 30% of the screen
<b>(5)</b> Width takes 10% of the screen and the height is 200 pixels.
</div>
<div class="thecap" style="text-align:justify; font-size:90%">
And here is a different way to format the font size in captions
</div>
</div>

~~~ 
Inline variables `a` and `b` can be used in equations \\( h\_t = \tanh ( a\_{t} b\_{t} ) \\) (requires MathJax--add it to `_layout/post.html`)
~~~ 

Inline variables `a` and `b` can be used in equations \\( h\_t = \tanh ( a\_{t} b\_{t} ) \\) (requires MathJax--add it to `_layout/post.html`)

<pre>
~~~ bash
cd myblog # simple Bash command
~~~ 

~~~ python
def main():
  a = 4 # simply Python code
~~~ 

~~~ c
int main() {
  a = 4; // simply C/C++ code
  return 0;
}
~~~ 

~~~ javascript
$('.promoted').hide(); // simple Javascript code
~~~ 

~~~ 
a = 4 -- no language indicated so no synthax highlight here
~~~ 
</pre>

~~~ bash
cd myblog # simple Bash command
~~~ 

~~~ python
def main():
  a = 4 # simply Python code
~~~ 

~~~ c
int main() {
  a = 4; // simply C/C++ code
  return 0;
}
~~~ 

~~~ javascript
$('.promoted').hide(); // simple Javascript code
~~~ 

~~~ 
a = 4 -- no language indicated so no synthax highlight here
~~~ 

## VIM
VIM assumes all text after the underscore \_ is italicized and should be hightlighted. This can be fixed by placing [these](https://github.com/tpope/vim-markdown) files in `~/.vim/` and replacing all the <code>```</code> with `\~\~\~` in  `~/.vim/syntax/markdown.vim`. It needs to be replaced because kramdown uses code snippets that start with `~~~` instead of the regular markdown <code>```</code>.

## Further reading
- [Steps](https://pages.github.com/) to create a blog on GitHub
- [Jekyll](https://jekyllrb.com/)
- [kramdown](http://kramdown.gettalong.org/quickref.html) quick reference
- [Disqus](https://disqus.com/)
- Karpathy's [switching from Wordpress to Jekyll](http://karpathy.github.io/2014/07/01/switching-to-jekyll/)
- Git tutorials: [Atlassian](https://www.atlassian.com/git/tutorials/setting-up-a-repository), [Dudler](http://rogerdudler.github.io/git-guide/), [Udacity](https://www.udacity.com/course/how-to-use-git-and-github--ud775)
