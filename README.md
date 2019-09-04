# MP1: Word similarity

This lab has two parts.
In the first, you will compute and evaluate several methods for computing word similarity using [WordNet](https://wordnet.princeton.edu/).
In the second, you will use a variant of log-odds ratios to find words which occur much more often in one corpus than another.

## Part 1: word similarity

In the first part of the MP you will compare various ways of measuring word similarity to human judgments.

The human judgments come from the [WordSimilarity-353 Test Collection](http://www.cs.technion.ac.il/~gabr/resources/data/wordsim353/).
This data set contains 353 word pairs rated for _relatedness_ on a scale from 0-10.
While raw per-subject scores are available, you are to use the `wordnet/combined.tab` file, a TSV file which contains relatedness score averaged across the 13-16 subjects who rated each pair.
A few pairs contain entries not present in WordNet (e.g., `Maradona`); see `wordnet/REMOVED.txt` for details.

Your assignment is to compute three types types of similarity scores for this collection, and then compute the correlation (specifically, the [Spearman rho](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient) rank correlation coefficient) between the similarity scores and human relatedness judgments.
There are six forms of WordNet similarity:

* _path similarity_,
* _Leacock-Chodorow similarity_,
* _Wu-Palmer similarity_,
* _Resnik similarity_,
* _Jiang-Conrath similarity_, and
* _Lin similarity_.

### What to turn in

* the Spearman correlation coefficient (to 4 decimals) for each of these six methods,
* your code (ideally a single Python script), and
* a description of your approach and any problems you ran into.

### Hints

* Note `wordnet/combined.tab` has a header row; skip the first line, use [`csv.DictReader`](https://docs.python.org/3/library/csv.html#csv.DictReader),
  or [`pandas.read_csv`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html).
* There are (at least) two Python libraries for WordNet:
  [NLTK](http://www.nltk.org/) and [wordnet](https://github.com/alvations/wordnet).
  Either way, you will need to figure out someway to pick an appropriate synset for each word ("lemma" in WordNet terms).
  The first one returned is supposedly to be the most frequent sense (in some corpus).
  Alternatively, you could pick the pair of synsets which gives you the highest similarity score for this method.
* [SciPy](https://www.scipy.org/) has an implementation of Spearman correlation.

### Stretch goal

Also compute positive pointwise mutual information (Jurafsky &amp; Martin, &sect;6.7), with some fixed-size window, using some background data set, for all the pairs, and then compute the Spearman correlation.
Is this more or less correlated with human relatedness judgments than the WordNet similarity methods?

## Part 2: log-odds ratios

In the second part of the MP you will use the _log-odds ratio, informative Dirichlet prior_ method to compare word frequencies in two corpora.

Your assignment is to create a script which takes as input three TSV files (containing tokens and their frequencies).
The first two corpora are the "target" corpora; the third is the "background" corpus used as a prior.
Then, your script should computes the z-scored log-odds ratios using formulae 19.9-11 from Jurafsky &amp; Martin, &sect;19.5.1,
and print out a two-column TSV file consisting of words and their z-scored log-odds ratios.
The output rows should be sorted by z-scores so that words with the highest scores are at the top and the lowest scores at the bottom.

You should build two "target" corpora for comparison, and a third background corpus.
Ideally, the two "target" will be highly comparable (e.g., political speeches in the same language), and all three corpora should be subject to roughly the same tokenization and normalization schemes.

If you are working with English, you may use the [2017 News Crawl](https://github.com/language-technology-GC/frequency-distributions/blob/master/frequencies/news.2017-1.tsv) (`logodds/news.tsv`) as a background corpus. You may also use the [horoscopes corpus](https://github.com/language-technology-GC/frequency-distributions/blob/master/frequencies/horoscopes-1.tsv) as one of the two target corpora.

### What to turn in

* a TSV file containing the 50 words which have the highest z-scored log-odds ratios and their z-scores,
* a TSV file containing the 50 words which have the lowest z-scored log-odds ratios and their z-scores,
* The TSV frequency files for your two target corpora, if they're not one of the aforementioned corpora,
* your code (ideally a single Python script), and
* a description of your approach and any problems you ran into.

### Hints

* Use the formulas as they appear in Jurafsky &amp; Martin.
* If a token does not appear in all three corpora, you can assume that the log-odds ratio is undefined and ignore it.

### Stretch goals

Add the option to use the raw log-odds ratio without the z-score scaling (e.g., e.g., formula 19.9 rather than formula 19.11).
How do the results differ?

Scale the background by dividing (and flooring) them by some positive integer.
How doe the results differ?
