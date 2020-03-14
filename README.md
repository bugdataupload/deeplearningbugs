# How can bugs affect deep learning? A mutation-based analysis

## Project summary

In this project, we analyze how bugs can affect the results of deep learning. To achieve this goal, we instrument bugs into deep learning software, and analyze how the results of buggy versions differ from those of clean versions. 

## Setting
### Our analyzed deep learning software

Our deep learning software is an application of sentence sentiment classification based on CNN. It has three versions: [Theano version](https://github.com/yoonkim/CNN_sentence), [TensorFlow version](https://github.com/dennybritz/cnn-text-classification-tf), and [Keras version](https://github.com/alexander-rakhlin/CNN-for-Sentence-Classification-in-Keras)

The three versions are built upon the three famous deep learning libraries: [Theano](https://github.com/Theano/Theano),[TensorFlow](https://github.com/tensorflow/tensorflow) and [Keras](https://github.com/keras-team/keras).

### Our dataset

The dataset can be downloaded from www.cs.cornell.edu/people/pabo/movie-review-data/. We use sentence polarity dataset v1.0, which contains 5,331 positive reviews and 5,331 negative reviews. 

### Our tools

In our study, we choose [mutmut](https://github.com/boxed/mutmut) to instrument bugs. We use [coveragepy](https://github.com/nedbat/coveragepy) to record all executed code lines in clean versions. When we generate buggy versions, we instrument only those executed code lines.

## Result
### Our instrumented bugs

In total, we constructed 1,820 bugs, and inject them to deep learning applications built upon 3 libraries. The execution logs of the buggy and clearn versions can be found in Theano_logs, TF_logs and Keras_logs directories.


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

Please explain the format of your logs.

### Our summaries

The summaries can be found in Theano_summary, TF_summary and Keras_summary directories, and we store them in json files.

The entries of summary are mutation node type, accuarcy, training time, mutation line number, node content and related code.

For example:


mutation node type  | accuracy | training time | line number | node content|related code
  ------------- | ------------- | ------------- | ------------- | ------------- | ------------- 
 operator|0.7732|57.39143395423889|(502, 20)|<Operator: +=>|self.seen += batch_size
 
 Please explain the format of your summaries.
