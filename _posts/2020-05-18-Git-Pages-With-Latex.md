---
layout: post
title: How To Setup Git Pages With Latex Rendering
---

Embedding LaTeX into your blog is a must-have feature for anyone who wants to blog about mathematics topics. Having just set this up I wanted to document it for easy use for anyone setting up a Git Pages blog using Jekyll. Main credit for the below code goes to [Varun Agrawal](https://varunagrawal.github.io/2018/03/27/latex-jekyll/), the below transplants this setup into the basic git jekyll-now setup, upgrades katex version and fixes a bug where jQuery wasn't included.

The steps are as follows:

1. Fork the [jekyll-now](https://github.com/barryclark/jekyll-now) repo and follow the instructions there to get your own \[username\].github.io site setup.

2. In your layouts/default.html file, add the following into your header to bring in jQuery and KaTex
    
		```
		<!-- Load jquery -->
		<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js" type="text/javascript"></script>

		<!-- Load KaTeX -->
		<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/KaTeX/0.11.1/katex.min.css">
		<script src="//cdnjs.cloudflare.com/ajax/libs/KaTeX/0.11.1/katex.min.js"></script>
		```
3. Enable KaTeX in your \_config.yml file by adding two lines. Firstly add the line 'markdown: kramdown' into your config, then under then 'kramdown:' section of your config, add the setting 'math_engine: mathjax'. After completing this, that part of my config looks as follows.
		```	
		markdown: kramdown 
		# Jekyll 3 now only supports Kramdown for Markdown
		kramdown:
			# Use GitHub flavored markdown, including triple backtick fenced code blocks
			input: GFM
			math_engine: mathjax
			# Jekyll 3 and GitHub Pages now only support rouge for syntax highlighting
			syntax_highlighter: rouge
			syntax_highlighter_opts:
				# Use existing pygments syntax highlighting css
				css_class: 'highlight'
		```
4. In your layouts/default.html file create a footer tag at the end of your page just before the close of your html tag and add the following code to setup all text included between \$\$ signs to be rendered properly as LaTeX

		```
		<footer>
			<!-- Parse the Latex divs with Katex-->
			<script type="text/javascript">
				$("script[type='math/tex']").replaceWith(
					function(){
						var tex = $(this).text();
						return katex.renderToString(tex, {displayMode: false});
				});

				$("script[type='math/tex; mode=display']").replaceWith(
					function(){
						var tex = $(this).text();
						return katex.renderToString(tex.replace(/%.*/g, ''), {displayMode: true});
				});
			</script>
		</footer>
		```
5. Embed any LaTeX as you would inside tags such as  \$\$H\_1(X) = \\Z \$\$ to see the maths properly rendered as $$H_1(x) = \Z$$. (Note the slight oddity with KaTeX where some of the standard maths fonts are not quite as you may expect, for example instead of \\mathbb\{Z\} you have to use \\Z, see more info [here](https://katex.org/docs/supported.html).)

