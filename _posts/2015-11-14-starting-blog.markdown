---
layout: post
comments: true
title:  "Steps to start a basic GitHub blog using Jekyll"
excerpt: "These steps can be used to start a blog"
date:   2015-11-16 11:00:00
mathjax: true
---

## Installation
Make sure you install the latest version of ruby that is compatible with your OS. I run this
on Ubuntu 14.04

```bash
$ sudo apt-get install ruby1.9.1 ruby1.9.1-dev make gcc nodejs git
$ gem update --system
$ gem install jekyll # or sudo gem install jekyll (you may have to run this command twice)
```
Get a [GitHub](http://github.com) account with some *username*
Set up a [new repository](https://github.com/new) named *username*.github.io
Go to the folder where you want to store your blog files and clone the new repository (steps were taken from [here](https://pages.github.com/))
</code>

<p><div class="highlight"><pre><code class="language-bash" data-lang="bash"> <span class="nv">$ </span>git clone https://github.com/<em>username</em>/<em>username</em>.github.io
<span class="nv"> $ </span>cd <em>username</em>.github.io </code></pre></div></p>

## Quick start
From the folder <code>*username*.github.io</code>

```bash
$ jekyll new .
$ jekyll serve
```
- Browse to [http://localhost:4000](http://localhost:4000) to view blog locally
- Create posts like this one under `myblog/_post`
- Add _site to the .gitignore file

User only edits files in the `_post` folder and add data or figures to a `data` or `figures` folders created by user, and Jekyll builds the _site content. More information at [Jekyll](http://jekyllrb.com/docs/structure/)

## Post to GitHub
Navigate to your <code>*username*.github.io</code> folder

```bash
$ git add --all
$ git commit -m "new blog post"
$ git push -u origin master
```

A nice simple git tutorial can be found [here](http://rogerdudler.github.io/git-guide/)

## Improving layout (optional)
To include TeX, include in `_layout/post.html` (delete the space between `{` and `%`)

```html
<!-- mathjax --\>
{ % if page.mathjax %}
<script type="text/javascript" src="//cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
{ % endif %}
```
To set the default format of images and caption, include in `css/main.scss` 

```scss
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
```

To increase the width of the display, modify the `.wrapper` definition in  `_sass/_base.scss` replace the `* 2`'s with `* 1`'s

```
.wrapper {
    max-width: -webkit-calc(#{$content-width} - (#{$spacing-unit} * 1));
    max-width:         calc(#{$content-width} - (#{$spacing-unit} * 1));
```

To add a linkedin linked icon, include in `_include/footer.html` (delete the space between `{` and `%`) and in `_config.yml`, respectively

```
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
```

```
linkedin_username: <em>linked_username</em>
```

## Markdown tutorial
See the Markdown [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) for a complete list. In addition here are other formatting techniques and the more common techniques that I use.

```
Separate lines in the Markdown text are concatinated in the blog, e.g.,
this shows up as one line
```

Separate lines in the Markdown text are concatinated in the blog, e.g.,
this shows up as one line

```
Separate lines in the Markdown text trailed by two spaces are not concatinated in the blog, e.g.,  
  this shows up as two lines
```

Separate lines in the Markdown text trailed by two spaces are not concatinated in the blog, e.g.,  
  this shows up as two lines

```
**bold** or *italics* or **_both_** or <s>strikethrough</s>. 
Note that ~~strikethrough~~ should work but it's not working for me
```

**bold** or *italics* or **_both_** or <s>strikethrough</s>. 
Note that ~~strikethrough~~ should work but it's not working for me

```
> Indent and italicized text
```

> Indent and italicized text

```
Bullet points:

- bullet 1

- bullet 2
```

Bullet points:

- bullet 1

- bullet 2

```
Lists:

1. First
  * sublist item
  * sublist another item
2. Second
  1. numbered sublist item 1
  1. numbered sublist item 2
1. Third (in Markdown actual numbers don't matter; only that is a number)
1. Forth
```

Lists:

1. First
  * sublist item
  * sublist another item
2. Second
  1. numbered sublist item 1
  1. numbered sublist item 2
1. Third (in Markdown actual numbers don't matter; only that is a number)
1. Forth

```
Create a [hyperlink](http://hyperlink.com)  
  Create a [hyperlink](http://hyperlink.com "Title") that displays the title 
if you put your mouse over it
```

Create a [hyperlink](http://hyperlink.com)  
  Create a [hyperlink](http://hyperlink.com "Title") that displays the title 
if you put your mouse over it

```
<div class="imgcap">
<img src="/figures/tutorial.jpg">
<img src="/figures/tutorial.jpg" style="border:none;">
<img src="/figures/tutorial.jpg" width="10%">
<img src="/figures/tutorial.jpg" width="30%">
<img src="/figures/tutorial.jpg" style="max-width:10%; height:200px;" -->
<div class="thecap" style="text-align:justify">Caption: Inserting 
figures with various settings: <b>(1)</b> Unaltered image. <b>(2)</b> No border. 
<b>(3)</b> Width takes 10% of screen width. <b>(4)</b> Width takes 30% of the 
screen. <b>(5)</b> Width takes 10% of the screen and the height is 200 pixels.
</div>
<div class="thecap" style="text-align:justify; font-size:90%">And here is a 
different way to format the font size in captions
</div></div>
```

<div class="imgcap">
<img src="/figures/tutorial.jpg">
<img src="/figures/tutorial.jpg" style="border:none;">
<img src="/figures/tutorial.jpg" width="10%">
<img src="/figures/tutorial.jpg" width="30%">
<img src="/figures/tutorial.jpg" style="max-width:10%; height:200px;" -->
<div class="thecap" style="text-align:justify">Caption: Inserting 
figures with various settings: <b>(1)</b> Unaltered image. <b>(2)</b> No border. 
<b>(3)</b> Width takes 10% of screen width. <b>(4)</b> Width takes 30% of the 
screen. <b>(5)</b> Width takes 10% of the screen and the height is 200 pixels.
</div>
<div class="thecap" style="text-align:justify; font-size:90%">And here is a 
different way to format the font size in captions
</div></div>

```
Inline variables `a` and `b` can be used in equations \\( h\_t = \tanh ( a\_{t} b\_{t} ) \\) (requires MathJax--add it to `_layout/post.html`)
```

Inline variables `a` and `b` can be used in equations \\( h\_t = \tanh ( a\_{t} b\_{t} ) \\) (requires MathJax--add it to `_layout/post.html`)

<pre>
```bash
$ cd myblog # simple Bash command
```

```python
def main():
  a = 4 # simply Python code
```

```c
int main() {
  a = 4; // simply C/C++ code
  return 0;
}
```

```javascript
$('.promoted').hide(); // simple Javascript code
```

```
a = 4 -- no language indicated so no synthax highlight here
```
</pre>

```bash
$ cd myblog # simple Bash command
```

```python
def main():
  a = 4 # simply Python code
```

```c
int main() {
  a = 4; // simply C/C++ code
  return 0;
}
```

```javascript
$('.promoted').hide(); // simple Javascript code
```

```
a = 4 -- no language indicated so no synthax highlight here
```

## Further reading
- [Steps](https://pages.github.com/) to create a blog on GitHub
- [Jekyll](https://jekyllrb.com/)
- Karpathy's [switching from Wordpress to Jekyll](http://karpathy.github.io/2014/07/01/switching-to-jekyll/)
- Udacity's [How to use git and GitHub](https://www.udacity.com/course/how-to-use-git-and-github--ud775) course
- Concise [git tutorial](http://rogerdudler.github.io/git-guide/)
