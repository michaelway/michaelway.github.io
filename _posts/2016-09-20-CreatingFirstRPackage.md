---
layout: post
title: Creating your first R Package
categories: [R]
tags: [R, RStudio, devtoold]
description: A short guide on how to create your first R package

---

I was first drawn to R by its ability to produce beautiful graphics.  When I first started using it, creating the perfect plot often involved copying code and then tweaking away until it was just right... Then forgetting the code a few months later, scratching my head and starting all over again. Not only this but I began to use R for more complex tasks involving data analysis, I often found myself writing redundant functions over and over again for the same reasons.

Learning how to create my first package in R helped me overcome these problems as it allowed me to create a single place my code was stored it allowed me to install my code onto any machine in which I was running an analysis. Since I have learnt how to do this, it has saved me tonnes of time so I wanted to share some of the tips that I have for this process.


## The Basics

To get to grips with what a package is, have a play around with another package that you have already installed. I would suggest running everything through [RStudio](https://www.rstudio.com/ "RStudio webiste") software, as it makes coding in R so much simpler.

For example, load up the popular package ggplot2:

{% highlight R %}
install.packages("ggplot2")
library(ggplot2)
{% endhighlight %}

This loads all the code necessary run ggplot2 functions into your R session, whilst giving access to all of the help documentation. Try typing 'f1' next to a ggplot2 function to get a help page giving information about how to use the function, its author and examples typically with some test data.  Useful right!  You can easily create something just like this with your own code.

Before creating your first package, it is important to understand the typical structure of an R-package 
A simple R package should minimally contain four file types:
* *.R files (where you store your code)
* A NAMESPACE file (which contains all the information R needs to install the package)
* A DESCRIPTION file (which contains information about the package such as name, author, creation date etc...)
* *.rmd files (where any help documentation for your code is stored)

The package directory containing these files should include subdirectories such as R/ and /man where .r and .rmd files are store, respectively.  Dont get put off by the complexity of this - there are tools that  make generating these files a piddle.

## Creating your first package

You can generate all of the file and documentation for your package by hand.  If you have better things to do, however, the devtools packages is a lifesaver.  This package will generate your NAMESPACE, DESCRIPTION and *.rmd files with very little effort.

To install devtools:

{% highlight R %}

install.package("devtools")
library(devtools)

{% endhighlight %}

The devtools pacakge will create the directory structure of a package for you and create all of the files neccessary when builing a package.  Just run the create() function, in the directory where you want to build you package, giving it an appropriate name.

For example:

{% highlight R %}

setwd("~/")

create(FirstPackage)

{% endhighlight %}

When running this function messages should pop up in your console and a directory with the name of your package should appear in your working directory containing the files and folders shown below:

![The console](https://dl.dropboxusercontent.com/u/97084569/Blog1.png)

![The directory and file structure](https://dl.dropboxusercontent.com/u/97084569/blog2.png)


## Add your function

The most important bit of your package is your code, which must be wrapped into a function.  In my example, i've added a function that I created for printing random DNA sequences.

{% highlight R %}

generateDNA <- function(bp=20 )
  {
  #Nucleotides to use in sequence
  nucleotides <- c("A", "C", "G", "T")

  #Create empty character list to add nucleotides to
  sequence <- as.character()
  
  #Print nucleotides to sequence randomly of length bp 
  for(nucleotide in 1:bp)
    {
    sequence[nucleotide] <- sample(nucleotides[1:4],
                          size=1)
    }
  
  #Collapse spaces from the sequence
  finalSequence <- paste(sequence, collapse= "")
  
  #Return sequence 
  return(finalSequence)
  }

{% endhighlight %}

Now save this function to a .R file in the /R subdirectory of your package.

## Documentation

For devtools to package up your function, you need to add tags to your .R function file.  This involves adding the hash-apostrophe (#') at the beginning of lines that you want devtools to recognise.  Further, tags are added using the @ symbol. There are a variety of predefined tags that you can use, and their use is .  **The @export tag is essential, as it means that devtools will add your function to the NAMESPACE file; without this your function will not run when the package is run!**

{% highlight R %}
#' @title generateDNA()
#' @description This function allows you to create random DNA sequence of variable length
#' @param bp: numeric. Length of random DNA sequence that you wish to produce (Default = 20).
#' @return finalSequence: string. Function returns a random DNA sequence of variable length.
#' @export

generateDNA <- function(bp=20)
  {
  #Nucleotides to use in sequence
  nucleotides <- c("A", "C", "G", "T")

  #Create empty character list to add nucleotides to
  sequence <- as.character()
  
  #Print nucleotides to sequence randomly of length bp 
  for(nucleotide in 1:bp)
    {
    sequence[nucleotide] <- sample(nucleotides[1:4],
                          size=1)
    }
  
  #Collapse spaces from the sequence
  finalSequence <- paste(sequence, collapse= "")
  
  #Return sequence 
  return(finalSequence)
  } 

{% endhighlight %}
 
Finally, to generate all of the documention for your package just go into the package directory and run the document() function from devtools.

{% highlight R %}

setwd("~/FirstPackage")
document()

{% endhighlight %}

This will create a /man directory containing the .Rd files that contain the documentation for your package.

## Install and run

We've now created a fully functional R package, the final thing to do is install and run it.

Just run the install() function from the oh so useful devtools package:

{% highlight R %}

#Go to the directory above your package
setwd("../")

#Install package
install(FirstPackage)

#Load package
library(FirstPackage)

#Run function

generateDNA(bp=100)


{% endhighlight %}

Look at that pointless function go, and thats a quick way to create an R package!