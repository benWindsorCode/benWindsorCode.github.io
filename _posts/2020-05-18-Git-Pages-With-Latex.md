---
layout: post
title: Git Pages Setup With Latex
---

Setting up a Git Pages blog was surprisingly easy, however one issue which was took some fiddling to solve was the ability to embed $$\LaTeX$$ into my blog so that I can easily blog about mathematical topics. I wanted to start by detailing how you can setup $$\LaTeX$$ in your blogs to save you the same fiddling I had to go through.

The steps are as follows:
1) Fork the [jekyll-now](https://github.com/barryclark/jekyll-now) repo and follow the instructions there to get your own \[username\].github.io site setup.
2) In your layouts/default.html file, add the following into your header to bring in jQuery and KaTex
```
<!-- Load jquery -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js" type="text/javascript"></script>

<!-- Load KaTeX -->
<link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/KaTeX/0.1.1/katex.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/KaTeX/0.1.1/katex.min.js"></script>
```
3) In your layouts/default.html file create a footer tag at the end of your page just before the close of your html tag and add the following code to setup all text included between \$\$ signs to be rendered properly as LaTeX
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
4) Embed any maths you want inside your pages within the \$\$ tags to see rendered maths such as $$x\inX$$.
