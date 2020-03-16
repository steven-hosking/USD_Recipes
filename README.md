# USD recipes.

The repositories contain README snippets of cource code for their corresponding USD libraries. 
The goal is to give people a "leg up" by showing them differents ways on how we solve common problems using Pixars USD.
If you are new to USD, I would recommend reding through: https://graphics.pixar.com/usd/docs/Getting-Help-with-USD.html

IF you know your Overs from your Defs and Payloads from your references then read on.

Let's start with the first useful tip; USD is a layered architecture where the libraries that make it up don't depend on layers
below where the USD layer encapsulates all underlying libraries, but those libraries don't rely on USD. For further reading about
their architecture, read here: https://graphics.pixar.com/usd/docs/api/_usd__overview_and_purpose.html

You will be mostly dealing with the USD and SDF libraries and those will probably have the most robust code snippet, but if something
is missing, please stick up a code review with your addition!

Happy Layering!
