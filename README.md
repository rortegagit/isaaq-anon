# ISAAQ - Mastering Textbook Questions with Pre-trained Transformersand Bottom-Up and Top-Down Attention

## Materials to reproduce the experiments and results in **ISAAQ - Mastering Textbook Questions with Pre-trained Transformersand Bottom-Up and Top-Down Attention.**

Textbook Question Answering is a complex task in the intersection of Machine Comprehension and Visual Question Answering that requires reasoning with multimodal information from text and diagrams. For the first time, this paper taps on the potential of transformer language models and bottom-up and top-down attention to tackle the language and visual understanding challenges this task entails. Rather than training a language-visual transformer from scratch we rely on pre-trained language models and fine-tuning along the whole TQA pipeline. We add bottom-up and top-down attention to identify regions of interest corresponding to diagram constituents and their relationships, improving the selection of relevant visual information for each question. Our system ISAAQ reports unprecedented success in all TQA question types, with accuracies of 80.26%, 70.92% and 55.09% on true/false, text-only and diagram multiple choice questions. ISAAQ also demonstrates its broad applicability, obtaining state-of-the-art results in other demanding datasets. In this repository we provide the necessary code and data to reproduce the experiments related to such task that are presented in the paper.


## Dependencies:
To use this code you will need:

* Python 3.6.5
* Pytorch 1.4.0
* Transformers 2.11.0
* Numpy 1.18.0
* Pillow 6.2.1
* Tqdm 4.36.0
* googledrivedownloader 0.4.0
* ~75GB free space disk (for executing all the experiments)

## How to run the experiments:

**1. Execute the script download_pretrainings.py and download_materials.py to download the pretrained weights from other datasets (RACE, OpenBookQA, ARC, VQA, AI2D) and the rest of materials:**

```
python download_materials.py
python download_pretrainings.py
```

**2. Use the different python scripts to execute our models against the TQA tasks.**

**True/False model**:

```
usage: tqa_tf_sc.py [-h] -r {IR,NSP,NN} [-d {gpu,cpu}] [-p PRETRAININGS]
                    [-b BATCHSIZE] [-x MAXLEN] [-l LR] [-e EPOCHS] [-s]

optional arguments:
  -h, --help            show this help message and exit
  -d {gpu,cpu}, --device {gpu,cpu}
                        device to train the model with. Options: cpu or gpu.
                        Default: gpu
  -p PRETRAININGS, --pretrainings PRETRAININGS
                        path to the pretrainings model. If empty, the model
                        will be the RobertForSequenceClassification with
                        roberta-large weights. Default:
                        checkpoints/pretrainings_e4.pth
  -b BATCHSIZE, --batchsize BATCHSIZE
                        size of the batches. Default: 8
  -x MAXLEN, --maxlen MAXLEN
                        max sequence length. Default: 64
  -l LR, --lr LR        learning rate. Default: 1e-5
  -e EPOCHS, --epochs EPOCHS
                        number of epochs. Default: 2
  -s, --save            save model at the end of the training

required arguments:
  -r {IR,NSP,NN}, --retrieval {IR,NSP,NN}
                        retrieval solver for the contexts. Options: IR, NSP or
                        NN

```

**Text Multiple Choice model**:

```
usage: tqa_ndq_mc.py [-h] -r {IR,NSP,NN} [-t {ndq,dq}] [-d {gpu,cpu}]
                     [-p PRETRAININGS] [-b BATCHSIZE] [-x MAXLEN] [-l LR]
                     [-e EPOCHS] [-s]

optional arguments:
  -h, --help            show this help message and exit
  -t {ndq,dq}, --dataset {ndq,dq}
                        dataset to train the model with. Options: ndq or dq.
                        Default: ndq
  -d {gpu,cpu}, --device {gpu,cpu}
                        device to train the model with. Options: cpu or gpu.
                        Default: gpu
  -p PRETRAININGS, --pretrainings PRETRAININGS
                        path to the pretrainings model. If empty, the model
                        will be the RobertForMultipleChoice with roberta-large
                        weights. Default: checkpoints/pretrainings_e4.pth
  -b BATCHSIZE, --batchsize BATCHSIZE
                        size of the batches. Default: 1
  -x MAXLEN, --maxlen MAXLEN
                        max sequence length. Default: 180
  -l LR, --lr LR        learning rate. Default: 1e-5
  -e EPOCHS, --epochs EPOCHS
                        number of epochs. Default: 4
  -s, --save            save model at the end of the training

required arguments:
  -r {IR,NSP,NN}, --retrieval {IR,NSP,NN}
                        retrieval solver for the contexts. Options: IR, NSP or
                        NN
```

**Diagram Multiple Choice model**:

```
usage: tqa_dq_mc.py [-h] -r {IR,NSP,NN} [-d {gpu,cpu}] [-p PRETRAININGS]
                    [-b BATCHSIZE] [-x MAXLEN] [-l LR] [-e EPOCHS] [-s]

optional arguments:
  -h, --help            show this help message and exit
  -d {gpu,cpu}, --device {gpu,cpu}
                        device to train the model with. Options: cpu or gpu.
                        Default: gpu
  -p PRETRAININGS, --pretrainings PRETRAININGS
                        path to the pretrainings model. Default:
                        checkpoints/AI2D_e11.pth
  -b BATCHSIZE, --batchsize BATCHSIZE
                        size of the batches. Default: 1
  -x MAXLEN, --maxlen MAXLEN
                        max sequence length. Default: 180
  -l LR, --lr LR        learning rate. Default: 1e-6
  -e EPOCHS, --epochs EPOCHS
                        number of epochs. Default: 4
  -s, --save            save model at the end of the training

required arguments:
  -r {IR,NSP,NN}, --retrieval {IR,NSP,NN}
                        retrieval solver for the contexts. Options: IR, NSP or
                        NN
```

**2. Ensemble different models.**

**True/False Ensembling**:

```
usage: tqa_tf_ensembler.py [-h] [-d {gpu,cpu}] [-p PRETRAININGSLIST]
                           [-x MAXLEN] [-b BATCHSIZE]

optional arguments:
  -h, --help            show this help message and exit
  -d {gpu,cpu}, --device {gpu,cpu}
                        device to train the model with. Options: cpu or gpu.
                        Default: gpu
  -p PRETRAININGSLIST, --pretrainingslist PRETRAININGSLIST
                        list of paths of the pretrainings model. They must be
                        three. Default: checkpoints/tf_roberta_IR_e1.pth,
                        checkpoints/tf_roberta_NSP_e2.pth,
                        checkpoints/tf_roberta_NN_e1.pth
  -x MAXLEN, --maxlen MAXLEN
                        max sequence length. Default: 64
  -b BATCHSIZE, --batchsize BATCHSIZE
                        size of the batches. Default: 512
```

**Text Multiple Choice Ensembling**:

```
usage: tqa_ndq_ensembler.py [-h] [-d {gpu,cpu}] [-p PRETRAININGSLIST]
                            [-x MAXLEN] [-b BATCHSIZE]

optional arguments:
  -h, --help            show this help message and exit
  -d {gpu,cpu}, --device {gpu,cpu}
                        device to train the model with. Options: cpu or gpu.
                        Default: gpu
  -p PRETRAININGSLIST, --pretrainingslist PRETRAININGSLIST
                        list of paths of the pretrainings model. They must be
                        three. Default: checkpoints/tmc_ndq_roberta_IR_e2.pth,
                        checkpoints/tmc_ndq_roberta_NSP_e2.pth,
                        checkpoints/tmc_ndq_roberta_NN_e3.pth
  -x MAXLEN, --maxlen MAXLEN
                        max sequence length. Default: 180
  -b BATCHSIZE, --batchsize BATCHSIZE
                        size of the batches. Default: 512
```

**Diagram Multiple Choice Ensembling**:

```
usage: tqa_dq_ensembler.py [-h] [-d {gpu,cpu}] [-p PRETRAININGSLIST]
                           [-x MAXLEN] [-b BATCHSIZE]

optional arguments:
  -h, --help            show this help message and exit
  -d {gpu,cpu}, --device {gpu,cpu}
                        device to train the model with. Options: cpu or gpu.
                        Default: gpu
  -p PRETRAININGSLIST, --pretrainingslist PRETRAININGSLIST
                        list of paths of the pretrainings model. They must be
                        three. Default: checkpoints/tmc_dq_roberta_IR_e4.pth,
                        checkpoints/tmc_dq_roberta_NSP_e4.pth,
                        checkpoints/tmc_dq_roberta_NN_e2.pth,
                        checkpoints/dmc_dq_roberta_IR_e4.pth,
                        checkpoints/dmc_dq_roberta_NSP_e2.pth,
                        checkpoints/dmc_dq_roberta_NN_e4.pth
  -x MAXLEN, --maxlen MAXLEN
                        max sequence length. Default: 180
  -b BATCHSIZE, --batchsize BATCHSIZE
                        size of the batches. Default: 512
```