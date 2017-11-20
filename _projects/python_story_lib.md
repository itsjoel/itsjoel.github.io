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

##  Usage

*The following is a guide to usage of this module. For less verbose docs, check the linked GitHub repo when it is ready.*

### The `book()` object

Create a new `book()` to start a new story. The book's arguments are, in order, 

1. `title`, the title of the book
2. `authors` (optional), a list of the authors of the book
3. `startpoint` (optional), the start point for the book (**important**: make sure the book has a start point before starting the story)
4. `points`  (optional), a dictionary of the points of the book in this form: `{'name of point': [any  valid story() or decision() object],...}` .

Points can also be created after the fact with the methods  `.add_story()` and `.add_decision()`, or removed with `remove_point(name)`. 

In order to start the story, there must be a valid `start_point`. This can be set when initializing the `book` object or by using `.set_start_point()` afterwords. 

### Points

In order for a story to be a story, it has to have content. In this package, that comes in the form of points, either *story points* or *decision points*.

#### Story Points - the `story()` object

Story points are parts of a branched-path story that don't have any choices for the reader in them. They just show their content, then go to the designated next point in the story. In a flow chart, they'd be nodes that have either one or no arrows going to any other node.

In this package, they are represented by the `story()` object. It takes arguments:

1. `text`, which covers the content of this point in the story. This is formatted using a system described later in this documentation. 
2. `next_point`, which is the name, as listed in the `points` dictionary, of the next point in the story. 

#### Decision Points - the `decision()` object

Decision points are a little more complicated. They are represented by the `decision()` object. It takes arguments

1. `text`, which, like in the `story()` object, is formatted using a system described later in this documentation.
2. `options`, a dictionary containing every option in this decision point that the player has to choose from, and what point to go to depending which option they choose. This is in this form: `{'NAME_OF_OPTION_IN_CAPS': 'name of story point to go to if this decision is chosen',...}`. **important**: every option that the player has should start with a different letter to reduce errors in fuzzy matching. 

The fuzzy 

