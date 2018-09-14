<!-- All the Things I Googled to Write a Pong Clone -->
<!-- 2018-05-26 -->

In the past, I've written [a game using the HTML5 canvas](https://jdw1996.github.io/touch-the-bees) and [a project in Dart](https://jdw1996.github.io/pub-name-generator) but this is my first project using the two together.
Since this is something a bit new for me, I thought it would be interesting to keep track of all the things I have to search for in order to create the game.

A couple of things to note before I start:

* This isn't intended to be a particularly complex or impressive project.  My goal here is to get to know Dart and the canvas better.
* I will be using code from my previous projects for reference, so that will reduce my googling somewhat.

I'd also like to point out that this project has no affiliation whatsoever with Atari, Atari's *Pong* game, or any other game containing similar mechanics.

With all that in mind, below are my searches and the solutions to my problems.

## "delete master branch github"

When I create a project that's solely meant to be viewed in a browser, I like to delete the `master` branch and just use the `gh-pages` branch (see [here](https://pages.github.com/) for more on GitHub Pages).
I've done it a few times, but have to look up how every time.

[**Selected Solution.**](http://matthew-brett.github.io/pydagogue/gh_delete_master.html)

The article is pretty concise, but I'll quickly go over the specific steps I used:

1. Create and clone the repository.
2. Execute:
```
$ git checkout -b gh-pages
$ git branch -D master
$ git push origin gh-pages
```
3. Navigate to your repo on GitHub and, under **Settings > Branches > Default branch** change the default branch to `gh-pages`.
4. Execute:
```
$ git push origin :master
```

## "ubuntu install dart"

Apparently I haven't used Dart since I reinstalled my OS.

[**Selected Solution.**](https://www.dartlang.org/install/linux)

## "dart initializer list"

In a few of my class constructors, it seemed all I was doing was initializing all the fields directly from arguments.
I remembered seeing an initializer list in Dart, but wanted to check the syntax.

[**Selected Solution.**](https://www.dartlang.org/guides/language/language-tour#initializer-list)

<del>It's pretty much the same as in C++.</del>
Clearly it had been a while since I'd used C++ when I wrote this.
In Dart, the initializer list goes after a colon after the method signature, like in C++, but the list itself is written as a bunch of comma-separated assignments.

## "dart abstract class"

I want to have one parent class `Screen` from which all display modes inherit.
There may not be a lot of common content, but I'll be able to create a common interface this way.

[**Selected Solution.**](https://stackoverflow.com/questions/33904494/abstract-base-class-in-dart)

You can mark a class as abstract with the `abstract` keyword, and then to have pure virtual methods, you can simply leave out the function body.
Then to create a child class, you can use `class Child extends Parent`, and override any methods from the parent by marking them with the `@override` decorator.

## "dart get mouse click on canvas"

While my plan is to have the game itself controlled with the arrow keys, it makes the most sense to use mouse navigation for menus.

[**Selected Solution.**](https://books.google.ca/books?id=EcvjAwAAQBAJ&pg=PA108&lpg=PA108&dq=dart+get+mouse+click+on+canvas&source=bl&ots=eEZ-bI6MdI&sig=WPhmajDYn5yZmCGHLTG8Ol61Z9g&hl=en&sa=X&ved=0ahUKEwjJ4sPjtZfaAhVLY6wKHeukAUgQ6AEIdDAI#v=onepage&q=dart%20get%20mouse%20click%20on%20canvas&f=false)

I ended up using a few different code snippets from here.
First, to get the canvas element, use:
```
CanvasElement canvas = querySelector("#mycanvas");
```

Now you can add a function to listen for mouse clicks:
```
canvas.onClick.listen(clickListener);
```

And, once you have a mouse event `e`, you can get the coordinates of the click by:
```
int clickX = e.offset.x;
int clickY = e.offset.y;
```

## "dart canvas draw new frame"

To make the game work, I'll need to continually and repeatedly redraw the canvas.
I recalled seeing a tutorial on building a Snake game with Dart and the HTML5 canvas, and I was hoping to find that.
Sure enough, I did.

[**Selected Solution.**](https://dart.academy/web-games-with-dart-and-the-html5-canvas/)

If you're interested in this topic, it would be best to go ahead and read the part of the tutorial that covers `window.animationFrame`.
I plan on using the approach outlined there for displaying the game.

## "dart tuple" and "dart multiple return"

I was hoping to be able to return the x- and y-coordinates of a mouse click from a function in a tuple, as I would in Python.

[**Selected Solution.**](https://stackoverflow.com/questions/45326310/dart-function-return-multiple-values)

There is no such functionality built into the language, but it is possible to return a list containing different types of values.

## "dart check if variables point to same object"

When dealing with transitions between different `Screen`s, I'll need to know what the current screen is.
The easiest way I can see to do this is to compare my `currentScreen` with the screens themselves, to see which `currentScreen` is an alias for.

[**Selected Solution.**](https://stackoverflow.com/questions/18428735/how-do-i-compare-two-objects-to-see-if-they-are-the-same-instance-in-dart)

Determining if two variables `a` and `b` point to the same object is as simple as calling `identical(a, b)`.

## "dart ternary operator"

I'd been assuming Dart had a ternary operator of the form `condition ? ifTrue : ifFalse`, like the one in C and many other languages, but I figured I should make sure.

[**Selected Solution.**](https://code-maven.com/slides/dart-programming/ternary-operator)

My assumption was correct, this does exist in Dart.

## "dart enum"

To simplify the movement of the ball, I'm planning to have three set angles at which it can travel.
To store which angle it is currently moving at, I'd like to set up an `enum`, ideally.

[**Selected Solution.**](https://stackoverflow.com/questions/13899928/does-dart-support-enumerations)

Dart supports `enum`s with very simple syntax:

```
enum Animal {
  DOG, CAT, RABBIT
}
```

## "dart trig functions"

I'll need to use some basic trig functions to calculate the movement of the ball as it bounces around the screen.

[**Selected Solution.**](https://api.dartlang.org/stable/1.24.3/dart-math/dart-math-library.html)

Importing `'dart:math'` gives access to `sin`, `cos`, and `tan`, among other functions.
Each of these takes an angle in radians.
To make this easier to deal with, the library also provides the constant `PI`.

## "dart cast double to int"

The trig functions I'm using to figure out the movement of the ball output `double`s, but I need to convert these to integers so that I can use them as a number of pixels.

[**Selected Solution.**](https://stackoverflow.com/questions/20788320/how-to-convert-a-double-to-an-int-in-dart)

Dart `double`s have two different methods that do roughly what I want.
To round a `double` to the nearest `int`, you can use `myDouble.round()` and to truncate it, you can use `myDouble.toInt()`.

## "dart html canvas example"

The Snake tutorial I found above showed the `fillRect` method for drawing rectangles, as for the paddles, on the canvas, but I had more trouble finding a
way to draw a circle for the ball.

[**Selected Solution.**](https://sites.google.com/site/dartlangexamples/api/dart-html/interface/eventtarget/node-nodeselector/element/canvaselement)

There's an example on this page that shows how to draw a circle.
It boils down to creating an arc, then calling `stroke` or `fill`.
This is very similar to the way I drew circles for the bees in my *Touch the Bees!* game.

## "how to debug dart js"

I'm at the point now where I can transpile my Dart code to JavaScript without any warnings or errors, but the canvas remains blank.
The console shows the error message: `TypeError: J.p(...) is null`.
My code doesn't have variables called `J` or `p`; rather, these are created by the transpiler.
That does mean, though, that I don't know how to interpret them.
In my previous Dart project I didn't have any problem with this sort of thing, because it was a much smaller program.

I didn't end up selecting a specific solution found online, but [this StackOverflow question](https://stackoverflow.com/questions/25624397/dart-to-js-how-to-debug-the-generated-javascript-errors) prompted me to look again at the command I'm using to transpile.
I was just using the same one as I had for my [Pub Name Generator](https://jdw1996.github.io/pub-name-generator), but I realized the `-m` flag was causing the JavaScript output to be minified.
Removing it gives more easily understandable output.

## "dart write text to html5 canvas"

For the settings screen and the end screen of the game both, I will need to display some text on the canvas.

[**Selected Solution.**](https://stackoverflow.com/questions/19020591/how-to-stroke-a-text-in-dart-using-canvas-2d-context)

Using a `CanvasRenderingContext2D` object, here named `context`, you can set the `font` and `fillStyle` attributes and then call `fillText` with arguments for the text to display, the `x` coordinate to display at, and the `y` coordinate to display at (note that the coordinates represent the bottom-left corner of the text).
This can easily be written using Dart's cascade operator as follows:

```
context
  ..font = "12pt Open Sans"
  ..fillStyle = "black"
  ..fillText("Hello, World!", 10, 10);
```

## "dart zero padded number"

To display the score required to win the game on the settings screen, and keep it aligned properly, I think the best way is to zero-pad it, so that it's always two characters wide.

[**Selected Solution.**](https://api.dartlang.org/stable/1.24.3/dart-core/String/padLeft.html)

To pad a string `numericString` with zeros on the left, to a total width of two characters, we can use the following code:

```
numericString.padLeft(2, "0");
```

## Conclusion

Despite a roughly seven month break in the middle of this project and after many searches, I now have the game finished, and you can play it [here](https://jdw1996.github.io/retro-table-tennis/).
