# How can bugs affect deep learning? A mutation-based analysis

## Project summary

In this project, we analyze how bugs can affect the results of deep learning. To achieve this goal, we instrument bugs into deep learning software, and analyze how the results of buggy versions differ from those of clean versions. 


## Our analyzed deep learning software

Our deep learning software is ...  It has three versions: [Theano version](https://github.com/yoonkim/CNN_sentence), [TensorFlow version](https://github.com/dennybritz/cnn-text-classification-tf), and [Keras version](https://github.com/alexander-rakhlin/CNN-for-Sentence-Classification-in-Keras)

The three versions are built upon the three famous deep learning libraries. Please add the url of the three libs.

## Our dataset

The dataset can be downloaded from www.cs.cornell.edu/people/pabo/movie-review-data/. Please add more descriptions. 

## Our tools

In our study, we choose ... [Mutation tool](https://github.com/bugdataupload/deeplearningbugs) More details.

Coverage tool: https://github.com/nedbat/coveragepy

What is the relation between the two tools?

## Our instrumented bugs

In total, we constructed 1,820 bugs, and inject them to deep learning applications built upon 3 libraries. The execution logs of the buggy and clearn versions can be downloaded from ??


A bug example in Keras is shown as following:

The original code is

```Python
def result(self):
        if self.reduction == metrics_utils.Reduction.SUM:
            return self.total
        elif self.reduction in [metrics_utils.Reduction.WEIGHTED_MEAN, metrics_utils.Reduction.SUM_OVER_BATCH_SIZE]:
            return self.total / self.count
```
The mutated code is

```Python
def result(self):
        if self.reduction == metrics_utils.Reduction.SUM:
            return self.total
        elif self.reduction in [metrics_utils.Reduction.WEIGHTED_MEAN, metrics_utils.Reduction.SUM_OVER_BATCH_SIZE]:
            return self.total * self.count
```
Our mutation tool replaces the arithmetic operator from “/” to “*”, the modification causes a wrong calculation of a metric accuracy.

## Our logs

The entries of summary are mutation node type, accuarcy, training time mutation line number and related code.

Show an example of the entries.
