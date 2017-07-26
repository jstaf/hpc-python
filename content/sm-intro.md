---
title: "Snakemake"
menu: main
weight: 11
---

Let's imagine that we're interested in
testing Zipf's Law in some of our favorite books.

## Zipf's Law

The most frequently-occurring word occurs approximately twice as
often as the second most frequent word. This is [Zipf's Law][zipfs-law].
{: .callout}

We've compiled our raw data i.e. the books we want to analyze
and have prepared several Python scripts that together make up our
analysis pipeline.

Let's take quick look at one of the books using the command `head books/isles.txt`.

Our directory has the Python scripts and data files we
we will be working with:

```
|- books
|  |- abyss.txt
|  |- isles.txt
|  |- last.txt
|  |- LICENSE_TEXTS.md
|  |- sierra.txt
|- plotcount.py
|- wordcount.py
|- zipf_test.py
```

The first step is to count the frequency of each word in a book.

```
python wordcount.py books/isles.txt isles.dat
```

Let's take a quick peek at the result.

```
head -5 isles.dat
```

This shows us the top 5 lines in the output file:

```
the 3822 6.7371760973
of 2460 4.33632998414
and 1723 3.03719372466
to 1479 2.60708619778
a 1308 2.30565838181
```

We can see that the file consists of one row per word.
Each row shows the word itself, the number of occurrences of that
word, and the number of occurrences as a percentage of the total
number of words in the text file.

We can do the same thing for a different book:

```
python wordcount.py books/abyss.txt abyss.dat
head -5 abyss.dat
```
```
the 4044 6.35449402891
and 2807 4.41074795726
of 1907 2.99654305468
a 1594 2.50471401634
to 1515 2.38057825267
```

Let's visualize the results.
The script `plotcount.py` reads in a data file and plots the 10 most
frequently occurring words as a text-based bar plot:

```
python plotcount.py isles.dat ascii
```
```
the   ########################################################################
of    ##############################################
and   ################################
to    ############################
a     #########################
in    ###################
is    #################
that  ############
by    ###########
it    ###########
```

`plotcount.py` can also show the plot graphically:

```
python plotcount.py isles.dat show
```

Close the window to exit the plot.

`plotcount.py` can also create the plot as an image file (e.g. a PNG file):

```
python plotcount.py isles.dat isles.png
```

Finally, let's test Zipf's law for these books:

```
python zipf_test.py abyss.dat isles.dat
```
```
Book	First	Second	Ratio
abyss	4044	2807	1.44
isles	3822	2460	1.55
```

So we're not too far off from Zipf's law.

Together these scripts implement a common workflow:

1. Read a data file.
2. Perform an analysis on this data file.
3. Write the analysis results to a new file.
4. Plot a graph of the analysis results.
5. Save the graph as an image, so we can put it in a paper.
6. Make a summary table of the analyses

Running `wordcount.py` and `plotcount.py` at the shell prompt, as we
have been doing, is fine for one or two files. If, however, we had 5
or 10 or 20 text files,
or if the number of steps in the pipeline were to expand, this could turn into
a lot of work.
Plus, no one wants to sit and wait for a command to finish, even just for 30
seconds.

The most common solution to the tedium of data processing is to write
a shell script that runs the whole pipeline from start to finish.

Using your text editor of choice (e.g. nano), add the following to a new file named
`run_pipeline.sh`.

```
# USAGE: bash run_pipeline.sh
# to produce plots for isles and abyss
# and the summary table for the Zipf's law tests

python wordcount.py books/isles.txt isles.dat
python wordcount.py books/abyss.txt abyss.dat

python plotcount.py isles.dat isles.png
python plotcount.py abyss.dat abyss.png

# Generate summary table
python zipf_test.py abyss.dat isles.dat > results.txt
```

Run the script and check that the output is the same as before:

```
bash run_pipeline.sh
cat results.txt
```

This shell script solves several problems in computational reproducibility:

1.  It explicitly documents our pipeline,
    making communication with colleagues (and our future selves) more efficient.
2.  It allows us to type a single command, `bash run_pipeline.sh`, to
    reproduce the full analysis.
3.  It prevents us from _repeating_ typos or mistakes.
    You might not get it right the first time, but once you fix something
    it'll stay fixed.

Despite these benefits it has a few shortcomings.

Let's adjust the width of the bars in our plot produced by `plotcount.py`.

Edit `plotcount.py` so that the bars are 0.8 units wide instead of 1 unit.
(Hint: replace `width = 1.0` with `width = 0.8` in the definition of
`plot_word_counts`.)

Now we want to recreate our figures.
We _could_ just `bash run_pipeline.sh` again.
That would work, but it could also be a big pain if counting words takes
more than a few seconds.
The word counting routine hasn't changed; we shouldn't need to recreate
those files.

Alternatively, we could manually rerun the plotting for each word-count file.
(Experienced shell scripters can make this easier on themselves using a
for-loop.)

```
for book in abyss isles; do
    python plotcount.py $book.dat $book.png
done
```


With this approach, however,
we don't get many of the benefits of having a shell script in the first place.

Another popular option is to comment out a subset of the lines in
`run_pipeline.sh`:

```
# USAGE: bash run_pipeline.sh
# to produce plots for isles and abyss
# and the summary table for the Zipf's law tests

# These lines are commented out because they don't need to be rerun.
#python wordcount.py books/isles.txt isles.dat
#python wordcount.py books/abyss.txt abyss.dat

python plotcount.py isles.dat isles.png
python plotcount.py abyss.dat abyss.png

# This line is also commented out because it doesn't need to be rerun.
python zipf_test.py abyss.dat isles.dat > results.txt
```

Then, we would run our modified shell script using `bash run_pipeline.sh`.

But commenting out these lines, and subsequently uncommenting them,
can be a hassle and source of errors in complicated pipelines.

What we really want is an executable _description_ of our pipeline that
allows software to do the tricky part for us:
figuring out what steps need to be rerun.

## [Next section](../sm-snakefiles/)

