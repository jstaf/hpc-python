---
date: 2016-04-23T15:21:22+02:00
title: Pipelining with Python
type: homepage
menu: main
weight: 1 
---

Python is probably the most versatile language in existence.
However one of its most useful features is its ability to tie things 
together and automate the execution of other programs.

This tutorial focuses on using Python in HPC environments to automate 
data analysis pipelines with Snakemake.
We'll start with the basics and cover everything you need to get started.
Some elements of writing performance-oriented code will be covered,
but it is not the main focus.
There is no prerequisite knowlege for this tutorial,
although having some prior experience with the command-line will be very helpful.

## Snakemake

Snakemake is a “pipelining tool”. It replaces the role of the researcher in running an analysis. 
A researcher specifies their workflow’s tasks in a special file (called “Snakefile”) and runs Snakemake. 
Snakemake then handles the specifics of running the researchers’ tasks as efficiently as possible.

This has several key advantages over just running things ourselves:

* Our work is reproducible. Snakemake will run our analysis the same way every time. We can demonstrate that repeating the workflow returns the same result.

* Snakemake runs in parallel and has a built-in scheduler designed to maximize throughput. When we run a workflow, we can be confident that our work is getting done as fast as possible.

* Snakemake automatically takes into account updates to our starting data and code. If we make a change, Snakemake will only re-run the analysis affected files and downstream output. By the same token, if a workflow fails halfway through, it will pick up from where things left off.

* Workflows are portable - the same pipeline can be used on any system, anywhere. Snakemake handles all of the intricacies of running jobs (no need to write scripts for a cluster scheduler, Snakemake does that for you!).

Long story short, Snakemake makes the process of running our code more efficient and saves us work!

## Setup

You will want to have Python 3 and your favorite Python IDE preinstalled
and ready to go.
If you don't know where to get things or what to install, 
just install Anaconda (the Python 3 version) from [https://www.continuum.io/downloads](https://www.continuum.io/downloads). 
Anaconda is an extremely comprehensive Python distribution that comes 
with all of the bells and whistles ready to go.

To install snakemake, please run the following in a command-line terminal:
```
pip install --user snakemake
```

The files used in this lesson can be downloaded [here](snakemake-lesson.tar.gz).

## Credits

The Snakemake materials were adapted from Software Carpentry's 
[Automation and Make](http://swcarpentry.github.io/make-novice/) lesson.

## [Get started](./basics/)

