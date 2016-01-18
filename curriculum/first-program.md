Making Your First Program with Quil
===================================

Now that you know a bit about how to write Clojure code, let's look at
how to create a standalone application.

In order to do that, you'll first create a *project*. You'll learn how
to organize your project with *namespaces*. You'll also learn how to
specify your project's *dependencies*. Finally, you'll learn how to
*build* your project to create the standalone application.

## Create a Project

Up until now you've been experimenting in a REPL. Unfortunately, all
the work you do in a REPL is lost when you close the REPL. You can
think of a project as a permanent home for your code. You'll be using
a code editor called "Nightcode" to create and edit your project.

To create a new project, first download and install Nightcode. You can
download it from [this website](https://sekao.net/nightcode/). Once it's
installed, start it up and click "New Project" in the upper left. Enter
the name "drawing" for your app, and then hit "Save":

![](/curriculum/images/nc-scrn1.png?raw=true)

At this point, Nightcode will ask you to choose the type of project you
want to start. Your project will be graphical, so choose the "Graphics"
option:

![](/curriculum/images/nc-scrn2.png?raw=true)

At this point, your project will have been created, and you should see
a directory structure that looks like this on the left side of the Nightcode
screen:

```
drawing
├── README.md
├── project.clj
├── resources
└── src
    └── drawing
        └── core.clj
```

There's nothing inherently special or Clojure-y about this project
skeleton. It's just a convention used by Nightcode. You'll be using
Nightcode to build and run Clojure apps, and Nightcode expects your
app to be laid out this way. Here's the function of each part of the
skeleton:

- `project.clj` is a configuration file. It helps answer questions
  like, "What dependencies does this project have?" and "When this
  Clojure program runs, what function should get executed first?"
- `src/drawing/core.clj` is where the Clojure code goes

Because you chose the "Graphics" option, your project is configured to
use a Clojure library called [Quil](https://github.com/quil/quil),
which creates drawings called "sketches."

By default, your project has been setup with an example sketch. Let's go
ahead and run this code to make sure everything is working.  In Nightcode,
look for the "Run" option on the bottom of the main window and click it.

![](/curriculum/images/nc-scrn3.png?raw=true)

As in the screenshot above, you should see a separate window popup with a
simple circle within it. If you don't see the window right away, wait a bit
and make sure it hasn't appeared somewhere behind your other windows.

Great, you've run your the program! Also, just so you know, in Nightcode you
can also run the project code by pressing `Ctrl + R` (or `Cmd + R`).

## Modify Project

Let's create another Quil sketch. In Nightcode in the left directory tree
click on the "drawing" directory. Then at the top of the right window, click
"New File". In the window that pops up, enter the filename lines.clj, and
then "Save":

![](/curriculum/images/nc-scrn3.png?raw=true)

## Organization

As your programs get more complex, you'll need to organize them. You
organize your Clojure code by placing related functions and data in
separate files. Clojure expects each file to correspond to a
*namespace*, so you must *declare* a namespace at the top of each
file.

Until now, you haven't really had to care about namespaces. Namespaces
allow you to define new functions and data structures without worrying
about whether the name you'd like is already taken. For example, you
could create a function named `println` within the custom namespace
`my-special-namespace`, and it would not interfere with Clojure's
built-in `println` function. You can use the *fully-qualified name*
`my-special-namespace/println` to distinguish your function from the
built-in `println`.

Create a namespace in the file `src/drawing/lines.clj`. Open it, and
type the following:

```clojure
(ns drawing.lines)
```

This line establishes that everything you define in this file will be
stored within the `drawing.lines` namespace.

## Dependencies

The final part of working with projects is managing their
*dependencies*. Dependencies are just code libraries that others have
written which you can incorporate in your own project.

To add a dependency, open `project.clj`. You should see a section
which reads

```clj
:dependencies [[org.clojure/clojure "1.6.0"]
               [quil "2.2.6"]])
```

This is where our dependencies are listed. All the dependencies we
need for this project are already included.

In order to use these libraries, we have to _require_ them in our own
project. In `src/drawing/lines.clj`, edit the ns statement you typed
before:

```clojure
(ns drawing.lines
   (:require [quil.core :as q]))
```

This gives us access to the library we will need to make our project.

There are a couple of things going on here. First, the `:require` in
`ns` tells Clojure to load other namespaces. The `:as` part of
`:require` creates an *alias* for a namespace, letting you refer to
its definitions without having to type out the entire namespace. For
example, you can use `q/fill` instead of `quil.core/fill`.

## Your first real program

### Drawing with Quil

Quil is a Clojure library that provides the powers of [Processing](https://processing.org/), a
tool that allows you to create drawings and animations. We will use
the functions of Quil to create some of our own drawings.

We will define our own functions, like so...

```clojure
(defn draw []
   ; Do some things
   )
```

... that call functions that Quil provides, like so...

```clojure
   ; Call the quil background function
   (q/background 240)
```

Put it together:
```clojure
(defn draw []
   ; Call the quil background function
   (q/background 240)
   )
```

In order to create a drawing (or sketch in Quil lingo) with Quil, you
have to define the `setup`, `draw`, and `sketch` functions. `setup` is
where you set the stage for your drawing. `draw` happens repeatedly,
so that is where the action of your drawing happens. `sketch` is the
stage itself. Let's define these functions together, and you will see
what they do.

In Light Table, in the lines.clj file, add the following after the
closing parenthesis of the ns statement from before.

```clojure
(defn setup []

  (q/frame-rate 30)

  (q/color-mode :rgb)

  (q/stroke 255 0 0))
```

This is the `setup` function that sets the stage for the
drawing. First, we call quil's `frame-rate` function to say that the
drawing should be redrawn 30 times per second. We put `q/` in front to
say that this is `frame-rate` from quil. Look up at the ns
statement. Since it says `:as q`, we can use q as a short hand for
quil, and `library-name/function-name` is the way you call a function
from a library.

Second, we set the color mode to RGB.

Third, we set the color of the lines we will draw with `stroke`. The
code 255 0 0 represents red. You can [look up RGB codes](http://xona.com/colorlist/) for other
colors if you would like to try something else.

In Light Table, in the lines.clj file, add the following after the
closing parenthesis of the setup function.

```clojure
(defn draw []

  (q/line 0 0 (q/mouse-x) (q/mouse-y))

  (q/line 200 0 (q/mouse-x) (q/mouse-y))

  (q/line 0 200 (q/mouse-x) (q/mouse-y))

  (q/line 200 200 (q/mouse-x) (q/mouse-y)))
```

Here we call the quil `line` function four times. We also call two
functions repeatedly as the arguments to the `line` function:
`mouse-x` and `mouse-y`. These get the current position (x and y
coordinates on a 2d plane) of the mouse. The `line` function takes
four arguments - two sets of x, y coordinates. The first x and y are
the starting position of the line. The second x and y are the ending
position of the line. So we start each of these lines at a fixed
position, then end them wherever the mouse is when the sketch is
drawn.

```clojure
(q/defsketch hello-lines

  :title "You can see lines"

  :size [500 500]

  :setup setup

  :draw draw

  :features [:keep-on-top])
```

This is our sketch. You can set attributes of the sketch such as the
title and size. You also tell it what are the names of the setup and
draw functions. These have to match exactly the function names we used
above. The last line is to make our drawing app window keep on top
of everything else.

Now press `Ctrl + Shift + Enter` (or `Cmd + Shift + Enter`) to
evaluate the file. Your drawing should appear.

### Exercise: Rainbow lines
Update your drawing so that:
* the lines are a different color
* the title is different
* the lines start at a different place

Bonus: Make each of the four lines a different color.

Bonus #2: Change the color of the lines based on the mouse position.

Hint: You can browse the [Quil API](http://quil.info/api) for ideas and function definitions.
