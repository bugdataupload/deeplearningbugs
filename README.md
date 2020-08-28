# A Mutation-based Analysis on Deep Learning Bugs

## Project summary

Researchers [1]–[4] conducted various empirical studies on bugs in deep learning systems. In these studies, researchers read source files, bug reports, pull requests, fixes, and Stack Overflow discussions of deep learning bugs. In this project, we analyze how bugs can affect the results of deep learning. To achieve this goal, we instrument bugs into deep learning software, and analyze how the results of buggy versions differ from those of clean versions. 

## Setting
### Our analyzed deep learning application

Our deep learning software is an application of sentence sentiment classification based on CNN. It has three versions: [Theano version](https://github.com/yoonkim/CNN_sentence), [TensorFlow version](https://github.com/dennybritz/cnn-text-classification-tf), and [Keras version](https://github.com/alexander-rakhlin/CNN-for-Sentence-Classification-in-Keras)

The three versions are built upon the three famous deep learning libraries: [Theano](https://github.com/Theano/Theano),[TensorFlow](https://github.com/tensorflow/tensorflow) and [Keras](https://github.com/keras-team/keras).

### Our dataset

The dataset can be downloaded from www.cs.cornell.edu/people/pabo/movie-review-data/. We use sentence polarity dataset v1.0, which contains 5,331 positive reviews and 5,331 negative reviews. 

### Our tools

In our study, we choose [mutmut](https://github.com/boxed/mutmut) to instrument bugs. We use [coveragepy](https://github.com/nedbat/coveragepy) to record all executed code lines in clean versions. When we generate buggy versions, we instrument only those executed code lines.

## Result
### Our instrumented bugs

In total, we constructed 1,820 bugs, and inject them to deep learning applications built upon 3 libraries. The execution logs of the buggy and clearn versions can be found in [Theano_logs](https://github.com/bugdataupload/deeplearningbugs/tree/master/Theano_logs), [TF_logs](https://github.com/bugdataupload/deeplearningbugs/tree/master/TF_logs) and [Keras_logs](https://github.com/bugdataupload/deeplearningbugs/tree/master/Keras_logs) directories.


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

A sample log of Theano version is shown as below:

>loading data... data loaded!  
>model architecture: CNN-non-static  
>using: word2vec vectors  
>[('image shape', 64, 300), ('filter shape', [(100, 1, 3, 300), (100, 1, 4, 300), (100, 1, 5, 300)]), ('hidden_units', [100, 2]), ('dropout', [0.5]), ('batch_size', 50), ('non_static', True), ('learn_decay', 0.95), ('conv_non_linear', 'relu'), >('non_static', True), ('sqr_norm_lim', 9), ('shuffle_batch', True)]  
>... training  
>epoch: 1, training time: 409.35 secs, train perf: 81.55 %, val perf: 78.11 %  
>epoch: 2, training time: 399.31 secs, train perf: 82.68 %, val perf: 74.42 %  
>epoch: 3, training time: 415.92 secs, train perf: 91.78 %, val perf: 81.79 %  
>epoch: 4, training time: 402.08 secs, train perf: 95.87 %, val perf: 80.42 %  
>epoch: 5, training time: 410.46 secs, train perf: 96.24 %, val perf: 82.74 %  
>cv: 0, perf: 0.800186741363212
The accuracy is obtained from the end and the training time can be calculated by adding training time of each epoch. 

### Our summaries

The summaries can be found in [Theano_summary](https://github.com/bugdataupload/deeplearningbugs/tree/master/Theano_summary), [TF_summary](https://github.com/bugdataupload/deeplearningbugs/tree/master/TF_summary) and [Keras_summary](https://github.com/bugdataupload/deeplearningbugs/tree/master/Keras_summary) directories, and we store them in json files.

The entries of summary are mutation node type, accuarcy, training time, mutation line number, node content and related code.

For example, a bug summary in base_layer of Keras is recorded as following:


>{  
>    "node type": "operator",  
>    "accuracy": "0.7723\n",  
>    "training time": "52.12230920791626\n",  
>    "line number": "(1453, 21)",  
>    "node content": "<Operator: !=>",  
>    "related code": "    if insecure[0] != '_':\n"  
>  }  

[1] N. Humbatova, G. Jahangirova, G. Bavota, V. Riccio, A. Stocco, and P. Tonella. Taxonomy of real faults in deep learning systems. In Proc. ICSE, page to appear, 2020.

[2] M. J. Islam, G. Nguyen, R. Pan, and H. Rajan. A comprehensive study on deep learning bug characteristics. In Pro. ESEC/FSE, pages 510–520, 2019.

[3] L. Jia, H. Zhong, X. Wang, L. Huang, and X. Lu. An empirical study on bugs inside tensorflow. In Proc. DASFAA, page to appear, 2020.

[4] Y. Zhang, Y. Chen, S. Cheung, Y. Xiong, and L. Zhang. An empirical study on TensorFlow program bugs. In Proc. ISSTA, pages 129–140, 2018.
