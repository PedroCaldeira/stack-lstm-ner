# stack-lstm-ner
PyTorch implementation of Transition-based NER system [1].

### Requirements
  * Python 3.x
  * PyTorch 0.3.0


### Task

Given a sentence, give a tag to each word. A classical application is Named Entity Recognition (NER). Here is an example

```
John   lives in New   York
B-PER  O     O  B-LOC I-LOC
```
Corresponding sequence of actions

```
SHIFT
REDUCE(PER)
OUT
OUT
SHIFT
SHIFT
REDUCE(LOC)
```
<!--
#### Data structures

 * **buffer** - sequence of tokens, read from left to right
 * **stack** - working memory
 * **output buffer** - sequence of labeled segments constructed from left to right

#### Operations

 * `SHIFT` - move word from **buffer** to top of **stack**
 * `REDUCE(X)` - all words on **stack** are popped, combined to form a segment and labeled with `X` and copied to **output buffer**
 * `OUT` - move one token from **buffer** to **output buffer**-->

### Data format


The training data must be in the following format (identical to the CoNLL2003 dataset).

A default test file is provided to help you getting started.


```
John B-PER
lives O
in O
New B-LOC
York I-LOC
. O
```

### Training

To train the model, run ```train.py``` with the following parameters:
```
--rand_embedding      # use this if you want to randomly initialize the embeddings
--emb_file           # file dir for word embedding
--char_structure        # choose 'lstm' or 'cnn'
--train_file		  # path to training file
--dev_file		  	  # path to development file
--test_file		  	  # path to test file
--gpu 				  # gpu id, set to -1 if use cpu mode
--update              # choose from 'sgd' or adam
--batch_size  		  # batch size, default=100
--singleton_rate        # the rate for changing the words with low frequency to '<unk>'
--checkpoint 		  # path to checkpoint and saved model
```
### Decoding

To tag a raw file, simpliy run ```predict.py``` with the following parameters:
```
--load_arg            # path to saved json file with all args
--load_check_point    # path to saved model
--test_file           # path to test file
--test_file_out         # path to test file output
--batch_size            # batch size
--gpu                   # gpu id, set to -1 if use cpu mode
```
Please be aware that when using the model in ```stack_lstm.py```, ```--batch_size``` must be 1.

### Result

When models are only trained on the CoNLL 2003 English NER dataset, the results are summarized as below.

|Model | Variant| F1 | Time(h) |
| ------------- |-------------| -----| -----|
| [Lample et al. 2016](https://github.com/clab/stack-lstm-ner) | pretrain | 86.67 |  
| | pretrain + dropout | 87.96 | 
| | pretrain + dropout + char | 90.33 | 
| Our Implementation | pretrain + dropout |  | |
| |  pretrain + dropout + char (BiLSTM) |  | |
| |  pretrain + dropout + char (CNN) |  | |

### Author
 Huimeng Zhang: zhang_huimeng@foxmail.com

## References

[1] [ Lample et al., Neural Architectures for Named Entity Recognition, 2016](http://www.aclweb.org/anthology/N16-1030.pdf)
