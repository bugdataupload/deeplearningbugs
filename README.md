# Bugs in Deep Learning Software
In this paper, we we constructed 1,820 bugs of 3 implementations of a deep learning application. 

In dataset, we provide the program logs and a database storing the link between a buggy version and its mutation operator, mutated file and modified line number.

The entries of Excel file are mutation node type, accuarcy, training time mutation line number and related code.

Dataset: www.cs.cornell.edu/people/pabo/movie-review-data/

Theano version: https://github.com/yoonkim/CNN_sentence

TensorFlow version: https://github.com/dennybritz/cnn-text-classification-tf

Keras version: https://github.com/alexander-rakhlin/CNN-for-Sentence-Classification-in-Keras

Mutation tool: https://github.com/bugdataupload/deeplearningbugs

Coverage tool: https://github.com/nedbat/coveragepy
