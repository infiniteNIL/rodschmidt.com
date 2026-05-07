---
title: "ifgame - An Interactive Fiction game library for Clojure"
date: 2026-05-07T10:43:18-06:00
draft: false
tags: ['Clojure', 'Interactive Fiction']
---

On of the first programs I ever ran on a computer was a text adventure game, also known as
Interactive Fiction. I think the first one I played was [Adventureland](https://en.wikipedia.org/wiki/Adventureland_(video_game))
by [Scott Adams](https://en.wikipedia.org/wiki/Scott_Adams_(game_designer)), which was based on the
first ever text adventure called [Adventure](https://en.wikipedia.org/wiki/Colossal_Cave_Adventure)
by Crowther and Woods. Adventureland was the first text adventure available for personal computers.

Not long after that I discovered [Zork I](https://en.wikipedia.org/wiki/Zork), the first game by
[Infocom](https://en.wikipedia.org/wiki/Infocom). I loved the Infocom games, I played most of them,
spending many hours solving the games.

Where the Scott Adam's games had a parser that only allowed 2 words (verb object, such as 'get lamp'),
The Infocom games had a much more sophisticated parser. You could be as complicated as 'Robot, give the rusty lamp to the ugly troll.'.

This was about the same time I was just learning programming in basic. I wanted to write my own
text adventures, and I wondered how Infocom's parser worked. I think this is what sparked my interest
in parser, compilers, and interpreters to this day.

So, to get to the point, I wanted a project to do something more challenging in Clojure, and I
remembered that I wanted to write a adventure game when I was a kid.

[ifgame](https://github.com/infiniteNIL/ifgame) is a Clojure library for writing interactive fiction games.
It just a start, but really not a bad one. You can define rooms and their connections to other rooms,
and what objects they contain. You define objects and what properties they contain. You can also
define all the actions in the game, such as 'get', and 'drop', or whatever you want. Each room or
object can have handlers that can modify the behavior of the standard actions and really make up
the interesting parts of the game.

An example is included, which is basically the Heidi example defined in the [Inform Beginner's Manual](https://www.inform-fiction.org/manual/about_ibg.html).
Inform is a modern version of Infocom's ZIL programming language which they used to create their games.
It in itself, is a an amazing system for creating these games. ifgame is based on Inform 6. Check out
[Inform 7](https://ganelson.github.io/inform-website/) for an even more amazing system. You basically
program the game in English. Big props to [Graham Nelson](https://en.wikipedia.org/wiki/Graham_Nelson),
the creator of Inform.

I hope you find ifgame interesting and it inspires you to dive into the fascinating world of Interactive Fiction.
