---
layout: post
title:  "My first twitter bot"
date:   2019-01-13 17:49:50 -0500
categories: projects, twitter
---

# My first twitter bot!

I just made my first twitter bot!  It periodically generates text in the style of Jane Austen and then posts it to twitter.  This is in conjunction with the fast.ai course, so I had a template for the text generation part.  One misconception (of my own!) I’ve run into is the idea that if someone has done something already that reproducing it is going to be straightforward.  Sure, it’s probably going to be much more straightforward than doing something completely new, but between “more straightforward” and “absolutely straightforward” there can still be a bunch of weeds.  What I did:
1. Create a language model
2. Create a web app that automatically generates text
3. Make it a twitter bot

## Part 1: Creating the language model

I started out with the goal of training a model to create text in the style of Jane Austen.  In class, we had gone through an example of training a sentiment classifier using a corpus of IMDB movie reviews.  For anyone wanting to do similarly, it’s the Fall 2018 version of the course, which will be released shortly if it’s not out now.  The notebook in question was lesson3-imdb.  The general strategy was to use an RNN to create a language model of movie reviews, and then use labelled movie reviews in conjunction with our language model to make a classifier.  To make our language model, we used transfer learning, taking as a starting point a model that had been pre-trained on a subset of English-language Wikipedia (https://www.sysml.cc/doc/50.pdf) and then fine-tuning it using movie reviews from IMDB.  Starting with a whole bunch of movie reviews, there were two necessary pre-processing steps to take before training the model: tokenization and numericalization.  Tokenization is the process of splitting the text into atomic elements.  Our model was word-based, so we split the reviews into words, with some special treatment for parts of contractions like the “n’t” of “doesn’t” and punctuation.  (Another common approach is using character-based models, in which case the text would be divided into individual characters.)  The next step, numericalization, is mapping each token – a word or piece of punctuation – to an integer.  Conveniently, the fastai library can take care of both of these steps for us.  Once the text was ready, we were ready to train!  One of the great things about fastai is that it makes picking a good learning rate really straightforward, and it also supplies default settings that work really well, so training is pretty painless.

 The result was a word-based model of movie reviews.  When we looked at the predictions of the model (i.e. auto-generated movie reviews), they were definitely not perfect English, and you probably wouldn’t confuse them for actual human-generated reviews, but they were impressively not bad.  My approach was to do basically the same thing, but instead of fine tuning on IMDB reviews, I used the complete works of Jane Austen.
 
All of Jane Austen’s works are in the public domain, and they’re conveniently available as a single text file from the Gutenberg project.  I had to do a small amount of cleaning first (getting rid of chapter and title markings, dropping empty lines, etc.), and then I was ready to go.  I used the same approach as we had in class to create a language model.  This was probably the part that was actually the hardest, but because fastai takes care of so many implementation details and because I had the code from class to use as a template, it ended up being the most straightforward to accomplish.  In a few hours, I had a language model that would produce surprisingly plausible snippets of text.  But having this for myself wasn’t enough; I wanted to display it to the world…

## Part 2: Create a web app that generates text

Basically all of my programming experience has been academic – either programming / CS courses in college, or data manipulation in grad school.  Nothing relating to the web AT ALL except for putting my image classifier online in an earlier week of the fast.ai course.  I figured that I should do something similar with my text generator.  Again, I was fortunate to have a template to work from.  One interesting thing I noticed is that even though I had a template, and even though the approach was straightforward (according to people who have dealt with web frameworks in the past), I learned a lot just by re-typing everything myself, multiple times.  I’m familiar with repetition as a way to learn things, but for some reason I was surprised to find it so useful in this context. 

Unfortunately, while I was trying to improve the aesthetics of the web version of my text generator, I noticed that it was spitting out complete nonsense.  It looked like it was just picking words at random – sentence structure and grammar had disappeared.  After a lot of second-guessing myself and trying to figure out how I had broken things, I learned that in the latest version of fastai (which I had just upgraded to), some of the text processing functions were broken.  So this is the downside of using things under active development: on one hand, you can get cutting edge features; on the other, sometimes things surprisingly stop working.  But after reverting to the previous version, all was well again.  I got an OK-looking web page to surround the app, and up it went on the web (check it out at https://deepjane.now.sh).  I even learned some CSS along the way.

## Part 3: Making a twitter bot

Once I had text generation up and working, the obvious next step was to put it on twitter.  I used tweepy to connect the text generating engine to twitter, and it turned out to be really straightforward.  The only thing that required any adjustment was making sure the generated text would be under the length limit for tweets.  Not too problematic.

Tweeting just once isn’t enough though – I wanted tweets to happen at a regular basis, automatically.  This actually turned out to be a bit of a challenge.  When I tried to use the same service (Zeit) that I used for the web app, I ran into a world of pain accessing secret environment variables.  I ended up putting the automatic tweet generator on AWS.  Under the free tier, you can have a linux server running constantly, so I could just keep a script running in the background.  Right now, I have it set to tweet every 12 hours.  I don’t want Jane to be too chatty!  It posts as [@AutoAusten](https://twitter.com/AutoAusten).
