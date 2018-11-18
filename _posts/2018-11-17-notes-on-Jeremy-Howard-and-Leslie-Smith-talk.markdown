---
layout: post
title:  "Notes from the conversation between Jeremy Howard and Leslie Smith"
date:   2018-11-17 23:50:31 -0800
categories: fastai talks research
---

Last week, I was lucky enough to score a spot at the Meetup titled “Conversation between Jeremy Howard and Leslie Smith”.  For those unfamiliar with these names, Jeremy Howard is one of the founders of fast.ai, the main lecturer of the course “Deep Learning for Coders” (part I of which I’m taking as we speak), and the former President of Kaggle.  One of the primary goals of fast.ai is making deep learning (and machine learning in general, I think) more accessible – presenting the subject in a straightforward way and developing tools that make it easier to use deep learning methods without needing to develop a lot of esoteric expertise first.  

Leslie Smith is an AI researcher who’s done some terrific work on hyper-parameter optimization.  Fast.ai, both the library and the courses, use his results to take finding good hyper-parameters more automatic and less of an art.  I think Jeremy called him one of the most under-appreciated researchers out there; he doesn’t even show up on the first page of Google results for “Leslie Smith”.

The conversation took place at USF’s downtown campus, and was recorded live and posted on [YouTube](https://www.youtube.com/watch?v=6N-WUQwG1Lw).  I got the time wrong – the time the doors opened was the time I thought the conversation began – so I had the benefit of a really good seat and of watching Jeremy and Leslie chat before the formal part of the conversation started.  They appeared to have the rapport of old friends, which would have made sense given how much Leslie’s work shows up in the fastai library.  However, I was surprised to learn that they had just met an hour before!

An interesting point of commonality between the two was how they both were motivated to make AI more accessible.  Fast.ai of course runs several courses and additionally puts all the lectures online for free, and the library seems designed to make deep learning tools easily usable.  One of the goals of Leslie’s research is finding ways to set hyper-parameters without using a grid or random search, since these can be extremely computationally expensive.  Another stated goal was decreasing the amount of training data needed, since needing an enormous dataset to train on can also make deep learning less accessible.

Another point that Jeremy and Leslie have in common is that, at least in their public personas, they are extremely humble.  I got the sense that each thought of himself as a bit of an underdog, which simultaneously made sense and made no sense whatsoever.  Jeremy very vocally doesn’t have a Ph.D., and the impression I’ve acquired is that until some point in the fairly recent past, the general assumption was that you needed a Ph.D. to do deep learning.  Leslie has a Ph.D. in quantum chemistry, but said that basically until Jeremy started publicizing his work, nobody cared about it.  At the same time, from the outside they’re both doing badass work and seemingly crushing it.  I guess the takeaway is that even successful people can still be human and not turn into arrogant assholes.

Some pieces of advice and interesting ideas I collected during the talk:
* Do thought experiments and write down what you think will happen before you test it
* Try things out with code! You need to be a pretty good coder though, so your code is most likely doing what you think it’s doing
* If paper authors make their code available, it’s a good idea to download and run it yourself
* Be willing to question everything: even when something seems like it’s working, question whether what you think is going on is really what’s at play
* When you start tackling a question, start with something you know the answer to and gradually make small steps towards what you want to find out
* It’s useful to think
* Papers about using large batch sizes are still relevant to people with regular computing resources because the same thing that let you use large batch sizes also let you use large learning rates (and therefore train faster)

I’m also on the lookout for things to read next, so I made note of some things that were mentioned as interesting ideas that I hadn’t heard of (there’s so much to learn!  Where do I start? Aggggh).  Among these were
* Incremental learning
* Few shot learning
* Prototypical networks
* Goyal et al.: https://arxiv.org/abs/1706.02677 
* DeVISE: https://papers.nips.cc/paper/5204-devise-a-deep-visual-semantic-embedding-model
* Leslie’s new favorite paper, Bansal et al.: https://arxiv.org/abs/1806.00730
