---
title: 'Mobile Pokedex Flavours'
description: "Learning all mobile platforms doing the best after hello world project for frontend"
publishDate: "10 JUN 2024"
updatedDate: "10 JUN 2024"
tags: ["mobile", "pokemon", "java", "objective-c", "kotlin", "swift", "flutter", "react-native"]
draft: false
---

# Mobile Pokedex Flavours
When we talk about learning to code theres nothing better than a project, and when we talk about frontend, right after the first hello world most of us think about the classic pokedex, well, i’m a mobile developer who has worked most of time with react native, i’ve done some integrations with native code and different techs some times but i’ve never did anything from scratch, and thinking about it, and how much i struggle the first time i saw objective C, i decide to make this project, the exactly same pokedex but made in all most famous (or not so famous) tech stacks.
> Disclaimer: the goal here is just understanding how each technology works, I’m not focusing in architecture or good practices of each platform, so please, if you look at my code, focus in the functionality and not in real best readability possible

Before we start this is [here](https://github.com/thalesgelinger/mobile-pokedex-flavours) is the repo if you want to take a look.

## The Goal

The goal here is, creating the same UI across platforms, fetching Pokemon data from API, showing then in a list, filtering the list, add infinite scroll making new fetches to get more Pokemons from API, show Pokemon details, and store details on device to use offline. 

## Hybrid React Native: Starting from the beginning

I have experience with react native, this is my daily driver tech in my work, sometimes I need to work with objective C and Java for some specific features, but most of time is Typescript, so for starting this Pokédex project, I decided to start with react native in something different, the new version of expo was launched at that time, and expo router was not working very well with TS, so why not use JS just because? This is what I did, the first Pokédex made in react native, use expo router and it’s make entirely in javascript, that was fun, expo really improved a lot in past few years, there’s nothing much to comment in this one, was really just a entry point to start, and at least take a look in some different react native lib.

## Hybrid Flutter: Working with the 2024 ugly duck

I’ve already worked a bit with flutter in the past, I really think it’s an interesting technology, so, was nice to focus on it to remember some stuff, and make a full aplication, using some other libraries to achieve what is needed.
At the beggining was kind of very simple to understand the concept of widgets, and how to use stateless and stateful components, i haven’t used any of blob, or mobx or any of those fancys libraries for state managements, as I said the goal is learn the technology by itself, relying the minimum possible in third party libraries, so I just used default flutter states, create the widgets most of then stateless the simple as possible.

The cool thing about flutter is that dart is for sure a very good language, but the fact that flutter is totally based on POO give me a “old school” UI feeling, maybe because I came from react-native, in the beginning things were all classes, but now it’s all functions, but even being POO, was simples to get things done.

Another thing I didn’t like that much is that after learning the widgets, it’s kind of easy to get code much more verbose than it should(?), like when I had to add opacity in a box, I had to wrap everything with a new Widget to make it works, I think it could be simpler, maybe a library could do that, but by default I didn’t like the amount of widgets that I had to use to make some stuff that are usually simpler.

Api calls, rendering list, very straight forward, sanitizing data as well, pretty simple, dart has futures that work essentially the same as promise in JS, so that was cool, I used neovim to build react native and flutter version, and for flutter there’s a plugin flutter.nvim that works pretty well, with same things we have on vscode extension, which for me personally, was very helpful, but I don’t like to rely to much on code editor, in flutter I used a lot the functions to wrap, because every time I need to wrap something it was kind of verbose, the tools helps, but relying on it is not what I consider (again, just personal option), the best option, and I tried to think when for example reading a code review, if you are on bottom of the file, you just don’t know which component are you talking about, because on editor, the IDE add a virtual comment to understand where a widget is closed, but that doesn’t happen on GitHub or any other git platform online.

## IOS ObjC: My biggest fear, the reason for all of this start

I was SO interested, excited, afraid, curious, all of this at same time, to touch Objective C, everything I knew so far, was that it was a language that started in NS, move to apple, and was the way to build IOS apps in the old times, I know that today ObjC is just legacy, and I probably would use better my time to learn swift, I but really like this thing of making one step back to make 3 steps further, so I like to understand how things were to understand the good, the bad, the ugly, and how things are evolving, and Objective C was the one of my biggest proof of resilience on this project, and probably the one I most have things to talk about.

The first thing was that I didn’t want to use Xcode for build my Pokédex, I wanted to use neovim for everything, which made me walk thought paths i don’t recommend anyone to do, eventually a moved to Xcode, but was very nice, start working with objective C, adding a .h and .m file, and noticing that it was not added to the project itself because Xcode uses a specific an very odd file for keeping dependencies and everything else, so I started to even create my own Xcode tool for use on nevoim, it’s called [xcode.nvim](https://github.com/thalesgelinger/xcode.nvim) which I just stopped but still want to keep working on that in future, I learned that Xcode is a tool that makes a lot of magic behind the scenes, and starting with neovim, makes me understand a lot of that magic, and some of interesting pains that native iOS devs felt with Xcode in past.

I had t use a bunch of different clis to build, format, indexing, I had to learn a little bit of ruby to understand some dependencies, and a lot of stuff, and this was just the setup, I barely touched objective C at this point.

One of my biggest pains, was render svg, until I quit and just use png that I learned later that was most common than svg.

Objective was the worst thing ever, it was so hard to search and found anything about it, the documentation itself, and all kind of stuff related was so old, I really thanks chatGPT to help me learn this language.

Ok, talking about the code itself, I know iOS has a lot of project using storyboard, or some other tools that we draw, add constrains and everything else, and I actually started this objc project 4 times, each of then I new greta issue to understand, and read bugs, and fix build issues, was a nightmare, I tried to use storyboard, but I didn’t like that, i had the feeling that making it all using objc would be better, and would make me understand more of the IOS architecture itself, so I moved to do everything using objc, another thing, is that I started using constrains and setting then by hand, but my code organization was not the best, so making more researches, I decided to use Yoga, it has a similar usage to flex box which is something I already knew by using react native and flutter, so Yoga was pretty simple to get things done, and I kept working with that until the end of the project.

One cool thing I want to mention, is that while I was studying objective C with UIKit, I saw a video from [Atekita Dev](https://www.youtube.com/@attekitadev) teaching about Uikit, and the way she was using constrains was so simpler, I really could use that, but at that point, I was almost finishing my project and didn’t wanted to start it all again, I know i said I wanted to start it using the default library, but for this one I made an exception.

The hardest thing of working with objective C, after documentation, was remember imperative UI programming, and as I had fictional programming a lot in my mind, it took me a while, to really get into the way UIKit and objective C works, after I get how to handle that, was actually fun.

Until I had to put a list, that was one of the craziest stuff I’ve ever made, and took me a while to REAL understand how it’s supposed to be used, but after that, I could create a list, that was virtualized, using cells, and all fancy cool stuffs.

After that most of work was just building, separating some things to not make this a real nightmare, making api calls was fun, using threads instead of the way JS does a simple async await was very interesting, navigation was SO simple, I wasn’t expecting this, and the syntax itself is actually pretty cool, I like that, and that make a lot of senses and I understand the reason why the language is that way.

## IOS Swift: Better than Rust BTW

I was so, SO excited to work with swift, after struggling for so long with object C, I could not wait anymore to work with this modern language, every time I was searching something about objective C YouTube was showing me swift videos, that I was very interested to look, but as I wanted to focus on Objective C I decided to wait, and now was the time to look at that, and I must say swift UI is SO easy, I mean, it so delightful to make UI’s with that, the way that swift is structured, and the simplicity that it brings really caught me in, I haven’t had any kind of big issue with swift UI, working with enums, extensions, just the simple fact of using structs as components, was so good, it was a very good experience to work with, and since I had that hard time with objetive C, most of the concepts was very simple, making API calls was very straight forward, and the feature for previews in Xcode was very nice as well, and the framework itself, swiftUI is very impressive, I didn’t had to install any third party library, which is fantastic.

The only thing that I really need to mention here is, WHY THE HECK WHEN THERE’S A ERROR XCODE SHOWS IN ASSEMBLY? I mean, who will understand some kind of error that way? It just doesn’t make any sense, c’mon apple, you can make it better.

Shout out for [Sean Allen](https://www.youtube.com/@seanallen), his videos were very helpful through my journey using swift, very good content

## Android Java: not that bad

This was the greatest surprise, it took me a long time to really start that, because I was not understand how android works, what to communicate to what, but when I realize it was kind of way more similar to web, having a template file and a logic file, things make way more sense, so I just started to make the changes in xml, bind using findById, adding values, and that was way easier than what I had to do on objective C, people say java is bad, I really recommend objc for those people, java was not that bad, I could get something working very fast, the imperative way of work i had took already by working with objc, so that was simple, most of the UI was very straight forward, it took me a time to understand how separate better things, it’s not THAT good, but it’s enough, I could focus on understanding activities, how to make api calls properly, debugging some stuff, was a way better experience, even adding the recycle list, was so simple, because I already got the idea from what I’ve did on iOS, implementing all those methods was easy.

When I started the details page, I notice that findById was not the best way of doing things, then I learned that findById is actually forbidden in more than 250 countries, then I was looking for something more helpful, then I learned about data binding that make my DX for making the UI SO smooth, the only bad thing is that I still need to use imperative ways to render something like multiple items in a row that is not a list, but that is ok, java is nice.

## Android Kotlin:  crème de la crème of the software development

I just cannot explain how much I really enjoy using jetpack compose, it was so, simple, so smooth, so easy, the way of writing components, it was very similar to flutter, but way less verbose, it has a preview option like swiftUi, and it work in a mix of functional programming for UI, with some functions similar to react native, which was very easy for me to understand, and using POO for some pieces to create some more controlled flow and enable use of well known software architectures.

Well strcutured, well documented, not that verbose, things very clear, not everything out of the box like swiftUI, but very good libraries, also well documented.

The only thing that I would say that it’s personally didn’t like, is that kotlin by itself, it has a lot of influences from java, which makes a language with a lot of specific words to learn, just the class can be data, sealed, object, and so on, there’s a lot of options that is not that easy to figure out what to use when, something that when I look to swift is not that hard, still a very good language and an AMAZING way of building UI tough.

Shout out to [Philipp Lackner](https://www.youtube.com/@PhilippLackner) Amazing content, very helpful videos, he has a series about a Pokédex, but I didn’t follow that, he did a way better one teaching about good practices and good patterns architectures, really recommend that.

## What’s next?

I did what I wanted, create 6 Pokédex using hybrid and native solutions, learned a lot, it took me a year to build all of this, not because was hard, but because I really wanted to learn about the technologies I was using, and because I had a lot of another project to work on at same time hehe, but that was fun, probably I do something like that with another focus in future, but for now that it, mission complete, mobile development is very cool.

