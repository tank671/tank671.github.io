---
layout: post
title:  "Training was the easy part... Part II"
date:   2020-07-16 16:29:33 -0400
categories: projects
---

This is my second post about furnituresorter, my image classification project that identifies and moves images of furniture based on what’s in the picture.  In the [first part](https://tank671.github.io/projects/2020/06/22/training-was-the-easy-part.html), I covered curating the dataset, training the model, and getting the classifier to work in Windows.  

At this point in the process, the next thing I needed to do was assemble everything so it would work on someone else’s computer.  I didn’t have a clear vision of how I wanted the eventual users to interact with the classifier, but I knew I couldn’t expect them to install Python (and PyTorch, and fastai, and everything these depend on), and realistically, I couldn’t even ask them to interact with a command line.  I had to prepare the project so they would just need to double click something and have it work.  

I didn’t have any prior experience packaging Python projects, so I went into this part of the process expecting a challenge.  My general understanding is that it’s sort of complicated anyway, but there are several tools that aim to make it less painful.  I tried py2exe, pyinstaller, nuitka, and feet with no success.  To say that this was discouraging was an understatement, especially with py2exe and pyinstaller, because they seem to have pretty large user bases, so I was worried that something related to PyTorch or fastai was fundamentally incompatible with whatever these tools were doing behind the scenes.  Eventually, someone in my study group mentioned that he’d recently heard a talk about a new tool, Briefcase, that sounded like it might be helpful.  From its docs: “Briefcase is a tool for converting a Python project into a standalone native application.”  In other words, exactly what I was looking for.  It had a nice tutorial that produced a simple application that could be distributed with an installer, and furthermore it came with a widget framework already supported and recommended.  It turned out that this ended up being the winning solution, but there were still many points in the process when I seriously doubted whether it would ever work at all.  

(Have I mentioned I’d never done any GUI programming before this?  I hadn’t. But now I (kind of) have.)

Getting my application working in developer mode as a GUI was actually pretty straightforward, and also fun!  Things started off not working and gradually improved, and the end result, while not incredibly beautiful, looks like a fairly standard Windows program, which is actually extremely gratifying.  But then it was time to package everything up.

The first serious difficulty I ran into is that the file describing how to package a Briefcase application is pyproject.toml.  What the heck is pyproject.toml?  Actually, that’s the title of a blog post referenced in the Briefcase talk recommended to me (online [here](https://www.youtube.com/watch?v=WjMDXDHBn1I)) on basically exactly that question, and also one of the top search results for “pyproject.toml”.  Briefly, it’s a configuration file in toml, which stands for “Tom’s Obvious, Minimal Language”.  I did not appreciate this name, because if it was so obvious, why was I googling around trying to figure it out?  In particular, I needed to figure out how to indicate dependencies that aren’t available from default pip.  Some sleuthing provided me with the relevant answers, and I persuaded Briefcase to install the versions of torch and torchvision from wheels on the PyTorch website.

Relevant links:
* https://www.python.org/dev/peps/pep-0518/ 
* https://www.python.org/dev/peps/pep-0508/ 
* https://www.python.org/dev/peps/pep-0440/

WTH is pyproject.toml?  Underdocumented, in my opinion!

With my dependencies installed, I proceeded with the application creation process, which was deceptively smooth.  After successfully building my project (brilliantly named furnituresorter), when I tried to start the built version, I was greeted with a frustratingly simple error: “Unable to start app furnituresorter”.

Big sigh.

Eventually, my way forward was returning to the bare minimum application from the tutorial, and gradually adding back the parts of my classifier until I found what made it not work any more.

It was fastai.

After some experimenting, I determined that vanilla PyTorch could be used with Briefcase, but importing anything from fastai (including some pretty innocuous and useful utility functions) would cause my built application to not run.  So having trained my model using the fastai framework, I had to turn everything back into generic PyTorch.  This was a bit of a headache, but honestly not as bad as I had expected.  The only really annoying part was reproducing the model architecture.  Fastai champions the advantages of transfer learning, and its built-in ResNets are the ones from PyTorch, with the default behavior being using Imagenet-pretrained weights (whereas the default in PyTorch is not to use the pretrained weights.)  However, this isn’t the only difference.  Imagenet has 1000 classes, so its final layer outputs 1000 activations.  When fastai prepares a ResNet (or most of its built-in available architectures) for training an image classifier, it uses the number of classes in the dataset actually being used for training and attaches a layer with that number of activations instead of the one with 1000 for Imagenet.  That’s hugely convenient!  But that’s not all fastai does.  It actually adds 9 additional layers (in my case).  I assume that the motivation behind this is the result of some experimentation from the fastai team, but I don’t know what it is.  It’s undocumented, uncommented, and written in characteristically telegraphic fastai code style, which made deciphering what was going on more difficult than I think it needed to be.  But after I extracted everything I wanted to use from fastai from the rest of the library, building the app….. worked!!

Briefcase created an .msi file, which successfully installed furnituresorter on another computer with no pain, complication, or even typing.  I was frankly amazed at how well it worked.  And happily, my user was also amazed at how well the classifier worked!  There are still some changes and improvements I’m planning to make in the future, but having now successfully built the application and knowing that the process is possible, I’m much more excited about making those improvements.  One piece of advice I’m glad I took was to start working on the application part of the project before I was completely satisfied with the model’s performance.  Many of the improvements I have in mind are related to the modeling aspect -- different ideas for data augmentation, maybe adding more classes to be identified, and so on.  I could have spent a really long time on this before confronting the difficulties related to building and packaging the application.  And now, I can return to those improvements while I already have something to show for my efforts -- and while my user’s image library is already being automatically organized.
