# DeepTSS


This repository contains codes developped to find transcription start site (TSS) in a genome with a deep convolutional neural network (CNN).

The first step is to generate the data and to label them. 

Data_augmentation.ipynb is usable to generate 299 sequences of 299 base pair long around a TSS, with an offset of one base pair.  All those sequences are the positively labeled sequences to be used in the CNN.The genes positions on every chromosome need to be stored in a csv file with columns precising the chromosome number, the start position, stop position and the strand (it takes the start position as TSS if the strand is positive and vice versa). The nucleotid sequences are stored in hdf5 files, one for each chromosome. Then we delete 299*2 bp of nucleotid at each side of TSS and the reluctant gives the negatively labeled sequences. It gives a relatively balanced dataset of positively labeled sequences and negatively labeled sequences.

Data_treatement.ipynb is usable to generate a sequence of 299 bp long around each TSS as positively labeled sequences. It generates also n times more negatively labeled sequences that are taken randomly at more than 299 bp from any TSS. Using this strategie we can create dataset of wanted positive/negative ratio. Nucleotid sequence and genes position must be stored in the same manner as before.

CNN.ipynb is usable to train a CNN model to detect TSS. The first step is to get the sequence one-hot-incoded so that we can think at our sequences as 2D black and white images (299,4). It then splits the data in training, validation and testing sets. Keras sequentiel model is used to define a 3 convolutional layers, one dense layer with max pooling, LeakyRelu and dropout. It also prevents overfitting by an early stopping strategy (stopping the training after 10 epochs if no improvement in the validation loss has been shown). The best model obtained during training is stored in a .hdf5 file. To evaluate the model it calculates the loss (binary crossentropy) and the metrics (accuracy and Mattew correlation coefficient) on the testing set. It also predicts TSS from the testing data and evaluates the area under the recall precision curve (AUPRC) and the receiver opperating characteristic (AUROC) using sklearn.
