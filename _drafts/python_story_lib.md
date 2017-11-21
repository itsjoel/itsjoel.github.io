---
title: Python Story Library
description: A Python library for telling branching stories.
tags: [programming, python]
layout: default
last_updated: [TK]
status: alpha
---

## Background

This year, in AP Computer Science, I was assigned the task of making a Choose-Your-Own-Adventure style story in Python. While we had done this last year in Intro to Computer Science, this year, it had to have color, and pauses.

Making a Python-based branching-path story is a common task for those learning programming. There are a number of benefits to using this as a learning tool: The task allows the learner to get creative, which is inviting to someone not particularly interested in programming. Choose your-own-adventure stories are things that almost everyone is familiar with, so little explanation is needed. The task can be completed with simple functions and structures that people learn early, like `print` and `raw_input()`, or with more complex custom functions and formatted text.

The issue was that the Python story I had _already_ written last year already had all those features, and I didn't want to do the same thing again this year. So, I decided to take on a bigger challenge, and make an object-oriented Python module for writing these kind of stories.

### Development

I made the module over the course of a few weeks in Computer Science class. I started with a basic script that formatted text with `termcolor` using tokens like `#`, `@`, and `%`. Then, I built it in a class called `story()` that could take a string as an argument, format it using that script, then print it out later, when a method was called on it. I built a `decision()` class that could take a list of possible choices, and had a method that prompted the user and didn't stop until it got an answer that fuzzy-matched one of the possible choices. I consolidated their similar properties into a class called `point()` that both inherited from. Then, I had both call the next function in the 

## Usage

*The following is a guide to usage of this module. For less verbose docs, check the linked GitHub repo when it is ready.*

### The `book()` object

Create a new `book()` to begin a new story. The book's arguments are, in order, 

1. `title`, the title of the book
1. `authors` (optional), a list of the authors of the book
1. `startpoint` (optional), the start point for the book (**important**: make sure the book has a start point before starting the story)
1. `points`  (optional), a dictionary of the points of the book in this form: `{'name of point': [any  valid story() or decision() object],...}` .

Points can also be created after the fact with the methods  `.add_story()` and `.add_decision()`, or removed with `remove_point(name)`. 

In order to start the story, there must be a valid `start_point`. This can be set when initializing the `book` object or by using `.set_start_point()` afterwords.  

The story can be started with `.tell()`. When the story is started, it will display the title with this style:

**===**<span style="color:red;weight:700">[BOOK TITLE]</span>**===**

and then give the authors in a By [author] and [author2]... form. Then, it will start with the first point and continuously go to the next one until it reaches a point with no next point.

### Points

In order for a story to be a story, it has to have content. In this package, that comes in the form of *points*, either *story points* or *decision points*.

#### Story Points - the `story()` object

Story points are parts of a branched-path story that don't have any choices for the reader in them. They just show their content, then go to the designated next point in the story. In a flow chart, they'd be nodes that have either one or no arrows going to any other node.

In this package, they are represented by the `story()` object. It takes arguments:

1. `text`, which covers the content of this point in the story. This is formatted using a system described later in this documentation. 
2. `next_point`, which is the name, as listed in the `points` dictionary, of the next point in the story. If this is left out or defined as `False`, this point will be considered an end of the story.

When the story point comes up in the story, it will display its text, then go to the next point. If one hasn't been given, or if the one given is `False`, then this will be the end of this journey through the story.

#### Decision Points - the `decision()` object

Decision points are a little more complicated. They are represented by the `decision()` object. It takes arguments

1. `text`, which, like in the `story()` object, is formatted using a system described later in this documentation.
2. `options`, a dictionary containing every option in this decision point that the player has to choose from, and what point to go to depending which option they choose. This is in this form: `{'NAME_OF_OPTION_IN_CAPS': 'name of story point to go to if this decision is chosen',...}`. 

When the decision point comes up in the story, it will display its text, then prompt the player for their choice. The choices are fuzzy-matched by attempting to match the letters that the players puts in with the corresponding letters in the choices. For example, if the options are `Python` and `Javascript`, the input `Java` will match `Javascript`, as those 4 letters of that input match the first 4 letters of that choice.

**important**: every option that the player has should start with a different letter to reduce errors in fuzzy matching. 

### Parsing/Formatting Text

Here is where the magic happens; the text formatting. You can use text-formatting to make text **bold**, *italic*, and <blink>blink</blink>, and more. You have a number of options available to you.

1. `*` - Bold
2. `_` - Italic
3. `@` - Blink
4. `` ` - Escape - use this to ignore other characters. **important** - there are some limitations on this in its current implementation; see below. 
5. `%` Choice - this makes text red, **bold**, and capitalized, which is the way that choices for the player should be displayed.

But wait, there's more! `|` can be used to split strings into blocks. These blocks can either contain formatted text, as detailed above, or can represent functions. Currently, there is only one supported, although more may be added later:

1. `<n>` represents a `time.sleep(n)`. 

So, for example, ``"*HELLO* The quick brown fox jumped over the lazy `*Hen`*.|<2>|%END TEST%"`` will result in 

> "**HELLO** The quick brown fox jumped over the lazy \*Hen\* 

being displayed, followed by a 3 second wait, followed by 

> "<span style="color:red;font-weight:bold">END TEST</span>"

**important** - currently, the functions are triggered if any  of the function tokens are in the string, so, to be safe, avoid using `<` and `>` in the text of your story unless you wish to make a `time.sleep()`. 

### Example

To wrap it all up, let's show an example story.

```python
Test_Book = book("Test Book", "Jon Doe")
Test_Book.add_story("start", "*Welcome!* `*Hello.`* |Sleeping for 3 seconds... |<3>| The next decision point will ask you to make a _*decision*_ or a _*choice*_.", "d-or-c")
Test_Book.add_decision("d-or-c", "Are you going to make a %DECISION% or a %CHOICE%?", {'DECISION': 'story_d', 'CHOICE': 'story_c'})
Test_Book.add_story("story_d", "This is the end of the test. You chose to make a %Decision%", False)
Test_Book.add_story('story_c', "This is the end of the test. You decided to make a %Choice%", False)
Test_Book.set_start_point("start")
Test_Book.tell()
```

alternatively, the same can be achieved this way:

```python
Test_Book = book("Test Book", "Jon Doe", "start", {
  "start": story("*Welcome!* `*Hello.`* |Sleeping for 3 seconds... |<3>| The next decision point will ask you to make a _*decision*_ or a _*choice*_.", "d-or-c"),
  "d-or-c" decision("Are you going to make a %DECISION% or a %CHOICE%?", {'DECISION': 'story_d', 'CHOICE': 'story_c'}),
  "story_d": story("This is the end of the test. You chose to make a %Decision%", False),
  "story_c": story("This is the end of the test. You decided to make a %Choice%", False)})
Test_Book.tell()
```



This will:

1. Display :

   > **===**<span style="color:red;weight:700">[BOOK TITLE]</span>**===**
   >
   > by Jon Doe.

2. Display

   > **Welcome!** \*Hello.\*

3. Display 

   > Sleeping for 3 seconds...

4. Wait 3 seconds

5. Display

   > The next decision point will ask you to make a ***decision*** or a ***choice***.

6. Display

   > Are you going to make a <span style="color:red;weight:700">DECISION</span> or a <span style="color:red;weight:700">CHOICE</span>?

7. Prompt the player. (Assume they type in `deci` )

8. Display

   > This is the end of the story. You chose to make a <span style="color:red;weight:700">DECISION</span> .

9. Display

   > **===END===**

## Files

*Coming Soon. Check back later.*