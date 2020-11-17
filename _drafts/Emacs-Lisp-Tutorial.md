---
layout: post
title: Emacs Lisp (Elisp) Tutorial
---

## What is Elisp
Emacs-Lisp is a programming language that the text editor Emacs is programmed in, and allows users to interact, customise and extend emacs functionality. I will assume for the sake of this post that you already know what Emacs is and are here because you would like to experiment with Elisp. I will also assume you can program in some 'standard' language like Python/Java/C/C++/JavaScript etc. so you understand programming concepts like variables, functions and parameters. This will be an example based tutorial, based around showing rather than telling. Through useful examples you'll see some of the basic operations and functions you can use in Elisp. 

I would encourage you to run and experiment with each example in this file and hopefully have some fun writing some code! 

## The Very Basics
Open emacs, type into your buffer the following
``` emacs-lisp
(+ 2 2)
```
and put your cursor over the end of the line and type C-x C-e (I will also assume you know how to execute basic Emacs commands). You'll see in the line at the bottom of your screen that the output '4' appears. You have sucesfully executed your first line of Emacs-Lisp. You may have noticed that Lisp uses a different order of operations than languages like Python or C++, Lisp's order of operations is somewhat similar to Reverse-Polish-Notaiton (RPN) and is similar to languages like Haskell where you specify 'function arg1 arg2 ...'. For example in maths you may write 2 - (3 + 4) but in elisp you'd write
``` emacs-lisp
(- 2 (+ 3 4))
```
to achieve the same result. That is, perform the minus operation on 2 and (+ 3 4) where (+ 3 4 ) means perform the + operation on 3 and 4. Hence you add 3 and 4 then perform (- 2 7) to get -5 (try running the line of code with C-x C-e to confirm this).

## Writing Functions
The defun keyword is used to define a function in elisp. The pattern is as follows
``` emacs-lisp
(defun function-name (parameter1 parameter2 etc.)
    (code to execute))
```
for examxple a function taking two numbers and adding them would look like
``` emacs-lisp
(defun add-numbers (num1 num2)
    (+ num1 num2))

(add-numbers 3 5)
```

## Interacting With Emacs
In order to make a function callable from within the emacs interface you need the 'interactive' keyword. This exposes your function to M-x. If a function is marked with 'interactive' you can then call M-x function-name, and the function will be executed. For example here is a function which takes your name and inserts it at the end of the current buffer
``` emacs-lisp
(defun name-func (name)
    (interactive "MName:")
    (goto-char (point-max))
    (insert name))
```
First execute this code either with C-x C-e or with M-x eval-buffer, and then navigate to a new buffer and call M-x name-func. You can then enter a name, press enter and the name will be printed in the buffer.

You will notice a letter 'M' prefixing the word 'Name' in the interact clause of the function. This is a character code which defines the type of data the input buffer will take, there are many options here which can be found here: https://www.gnu.org/software/emacs/manual/html_node/elisp/Interactive-Codes.html

Here we have an example taking two pieces of input
``` emacs-lisp
(defun name-func (name place)
    (interactive "MName:\nMPlace:")
    (goto-char (point-max))
    (insert "I am " name " from " place))
```
printing 'I am x from y' at the end of your current file when you call it with M-x name-func.

## Manipulating The Buffer
The main functions we will care about are:
1. Inserting text
2. Moving around the buffer

There are a few functions you can use, for this, first in a new buffer try the following
``` emacs-lisp
(search-forward "hello")
Some test text
Some more text
Hello
Some end text
```
Then run C-x C-e on the first line and you'll see your cursor jump forward to the line with 'Hello' on it. You can try something similar with search-backward.

You can also try the following function
``` emacs-lisp
(goto-char 0)
```
which will take you to the 0'th character of a file. 
