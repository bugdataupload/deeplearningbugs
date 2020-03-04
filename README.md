# How can bugs affect deep learning? A mutation-based analysis
In this paper, we constructed 1,820 bugs, and inject them to deep learning applications built upon 3 libraries. 

We provide logs and summary of buggy versions and corresponding mutation operators, mutated files and modified line numbers.

The entries of summary are mutation node type, accuarcy, training time mutation line number and related code.

Dataset: www.cs.cornell.edu/people/pabo/movie-review-data/

Theano version: https://github.com/yoonkim/CNN_sentence

TensorFlow version: https://github.com/dennybritz/cnn-text-classification-tf

Keras version: https://github.com/alexander-rakhlin/CNN-for-Sentence-Classification-in-Keras

Mutation tool: https://github.com/bugdataupload/deeplearningbugs

Coverage tool: https://github.com/nedbat/coveragepy

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
