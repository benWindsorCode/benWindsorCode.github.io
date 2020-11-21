---
layout: post
title: Getting Started With Emacs Lisp Hands On: A Practical Beginners Tutorial
---

## What is Emacs Lisp
Emacs Lisp is the programming language that forms the foundation of the Emacs text editor. It also allows users to interact, customise and extend emacs functionality. I will assume you already know what Emacs is and would like to experiment with Elisp. I will also assume you know basic programming concepts like variables, functions and parameters. This will be an example based tutorial, centered around showing rather than telling you'll see enough to get you started with Emacs Lisp and to interact with Emacs through code.

## The Very Basics: Lets Go (Run Every Example Yourself)
Open (your preferred flavour of) Emacs and type the following into an empty buffer 
``` emacs-lisp
(+ 2 2)
```
Now put your cursor over the end of the line and type C-x C-e. You'll see at the bottom of your screen that the output '4' appears. You have sucesfully executed your first line of Emacs-Lisp. 

You will have noticed that Emacs Lisp (and Lisp dialects in general) uses a different order of operations than languages like Python or C++, Lisp's order of operations is akin to Reverse-Polish-Notaiton (RPN) and is similar to languages like Haskell where you specify 'function arg1 arg2 ...' whereas in Python you would write function(arg1, arg2, ...). For another example to calculate 2 - (3 + 4) in Emacs Lisp you'd write
``` emacs-lisp
(- 2 (+ 3 4))
```
In words: perform the minus operation on 2 and (+ 3 4), where (+ 3 4 ) means perform the + operation on 3 and 4. Hence you add 3 and 4 then perform (- 2 7) to get -5 (try running the line of code with C-x C-e to confirm this).

You can call functions in the same way, for example you could press 'M-x describe-bindings' all this is doing is calling the describe-bindings function, which you can call from code
``` emacs-lisp
(describe-bindings)
```
by pressing C-x C-e on the last char of that snippet you'll see the same result as 'M-x describe-bindings'. We will return later in this post to other emacs functions you can call that will be useful.

I will pause to mention at this point, there is a lot im skipping over in this basics, this is not to teach you Emacs Lisp, its to teach you enough to be able to go out and experiment. At the end of this post I will leave a list of next steps, I dont want to reproduce the contents of the Emacs Lisp manual as it is already excellent. I want to help you avoid the hours of searching I had initially for basic tasks like 'how do I move the cursor and return afterwards to where I started?'. 

## Flow Control
We will cover only a simlpe if/else statement as this will serve your basic purposes and help you get a better feel for the Emacs Lisp syntax. The structure of an if/else in Emacs Lisp is
``` emacs-lisp
(if (condition)
    (execute if condition true)
    (execute if condition false))
```
so for example if you want to print out something based on if your current line number we could write
``` emacs-lisp
(if (eq (what-line) "Line 20")
    (princ "Im at line 20")
    (princ "Im not at line 20"))
```
note the new function what-line I'm using, and the brackets around what-line to call it. Try calling (what-line) on its own and examine the result.

## Writing Functions
The defun keyword is used to define a function. The pattern is as follows
``` emacs-lisp
(defun function-name (parameter1 parameter2 etc.)
    (code to execute))
```
so to write a function taking two numbers and adding them we might do something like this
``` emacs-lisp
(defun add-numbers (num1 num2)
    (+ num1 num2))

```
which you can then invoke with
``` emacs-lisp
(add-numbers 3 5)
```
This is easily combined with the above flow control to allow you to start writing some useful code. The missing piece is now how to interact with Emacs itself.

## Interacting With Emacs
The main functions we will care about are:
1. Inserting text
2. Moving around the buffer

First in a new buffer paste the following
``` emacs-lisp
(search-forward "hello")
Some test text
Some more text
Hello
Some end text
```
then run C-x C-e on the first line and you'll see your cursor jump forward to the line with 'Hello' on it. You can try something similar with search-backward.

You can also try the following function
``` emacs-lisp
(goto-char 0)
```
which will take you to the 0'th character of a file. You can and should experiment with (goto-char (point-max)) and (goto-char (point-min)) too.

To actually add text to a buffer you use the insert function followed by a string or set of space separated strings and string variables. So if we wanted to insert a name at the cursor we write
``` emacs-lisp
(insert "Ted")
```
or if we want to take a surname as a parameter and add it to a first name (remember C-x C-e the function, then C-x C-e on the function call)
``` emacs-lisp 
(defun add-surname (surname)
    (insert "James " surname))
    
(add-surname "Cordon")
```

After all this moving around the file, you may want to leave your cursor where it started. That's what save-excursion is for, if you wrap your code in a save-excursion then after the statements are executed your cursor returns to where it was. For example
``` emacs-lisp
(defun there-and-back ()
    (save-excursion
        (previous-line)
        (insert "ABOVE")))

(there-and-back)
```
this snippet moves up a line, inserts the word 'ABOVE' and then returns to where you pressed C-x C-e.

Together with defun we could now write a funciton chaining (goto-char 0) and then a search-forward allowing you to search a buffer for a piece of text, insert something at that point and return to where you started. You can see already how this could be powerful for automating simple tasks. The question is now, how do we access our functions from around emacs? 

## Exposing Your Functions To Emacs 
In order to make a function callable from within the emacs interface you need the 'interactive' keyword. This exposes your function to M-x. If a function is marked with 'interactive' you can then call M-x function-name, and the function will be executed. The format of an interactive function is as follows
``` emacs-lisp
(defun function-name (param1, param2, ...)
    "String describing the function and what param1, param2 etc. are used for"
    (interactive "special string to fetch your params")
    (statement 1 to execute)
    (statement 2 to execute)
    (...))
```
That special string to fetch your params is made up of character codes followed by text to display in the buffer. For example to capture a string we use 'M', this can be used to capture a name for exmaple here is a function which takes your name and inserts it at the end of the current buffer
``` emacs-lisp
(defun name-func (name)
    "A basic function inserting NAME at the end of the file."
    (interactive "MName:")
    (goto-char (point-max))
    (insert name))
```
First execute this code either with C-x C-e or with M-x eval-buffer, and then navigate to a new buffer and call M-x name-func. You can then enter a name, press enter and the name will be printed in the buffer.

There are many more character code options here which can be found here: [Interactive Codes](https://www.gnu.org/software/emacs/manual/html_node/elisp/Interactive-Codes.html).

You can use a new line to separate input from the user as demonstrated here taking a name and place from the user
``` emacs-lisp
(defun name-func (name place)
    "Take a name and place from the user and print a string with these values at the end of the file."
    (interactive "MName:\nMPlace:")
    (save-excursion 
        (goto-char (point-max))
        (insert "I am " name " from " place)))
```
printing 'I am x from y' at the end of your current file when you call it with M-x name-func.

## Adding Key Bindings
Now that you have an interactive function you can bind it to a global keyboard shortcut or a mode specific shortcut.
``` emacs-lisp
(global-set-key (kbd "C-h C-h") 'name-func)
```
Now execute the above line, and when you press C-h C-h the name-func we wrote above will be called. This is a global binding so will work accross all modes. You can add this line to your emacs initialisation file along with your function to make this permanent.

You can also bind per mode using define-key and passing a mode's key map, taking the name of the mode and appending '-map' will give you the object that mode stores its mde specific bindings in, so we could do
``` emacs-lisp
(define-key org-mode-map (kbd "C-h C-h") 'name-func)
```
and now our new binding will only work if you're in org-mode.

The above should be enough for you to start looking through various emacs docs and function definitions and understanding how you might call these functions yourself to augment your emacs experience. I am no Emacs Lisp expert myself, but have found the above examples to answer the main questions I had when I wanted to start writing my own extension. See the below section for resrouces I found very useful and more formal documents to learn about Emacs Lisp from.

## Next Steps
Congratulations you can now write and evaluate some Emacs Lisp, this is just the tip of the iceburg. Next up you should understand the groundings of Emacs Lisp better, ideally from the manual itself, in particular:
- [List Processing](https://www.gnu.org/software/emacs/manual/html_node/eintr/List-Processing.html#List-Processing)
- [Writing Functions](https://www.gnu.org/software/emacs/manual/html_node/eintr/Writing-Defuns.html#Writing-Defuns)
- [Getting And Switching Buffers](https://www.gnu.org/software/emacs/manual/html_node/eintr/Practicing-Evaluation.html#Practicing-Evaluation)
- [List Operations](https://www.gnu.org/software/emacs/manual/html_node/eintr/car-cdr-_0026-cons.html#car-cdr-_0026-cons) (given this is a list based language...)
- [Loops And Recursion](https://www.gnu.org/software/emacs/manual/html_node/eintr/Loops-_0026-Recursion.html#Loops-_0026-Recursion)
- [Binding Functions To Keys](https://www.masteringemacs.org/article/mastering-key-bindings-emacs)

Also on a similar note, Scheme is another Lisp dialect and Andy Balam's excellent introduction will tech you a lot which applies to Emacs Lisp too, [check it out!](https://www.youtube.com/watch?v=byofGyW2L10).
