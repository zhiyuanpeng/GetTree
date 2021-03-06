# useage steps
1. download the glove.6B.100d.txt and put it at the upper level catalogue
2. run clean_data.py to clean data
3. run original.py to get the baseline result
4. run the edge.py to get the predict result for each edge
5. run the enumeration.py to get the predicted result and the accuracy. Our method improves the baseline result by adding the edge info to it,
so before running the enumeration.py, make sure you have ran the edge.py and original.py
# clean_data.py
## input
sample.csv is a piece of sample data with the format: text\tl1,l2,l3,...
## Output
training data, testing data and tree structure of the top k frequent labels
generated folders:
```angular2
data-->processed_text-->X_test.txt: the test query
                     -->X_train.txt: the train query
                     -->y_test.txt: the labels of test query
                     -->y_train.txt: the labels of train query
    -->tree_img      -->tree.png
    -->store         -->lx_ly
                     -->lx_ly
                     -->...
                     -->original
```

## How to use
put the .csv file at the same folder with get_tree.py and clean_data.py. 
Then run "python clean_data.py "sample", 5 2" where sample is the data file name, 5 is the number of top frequent labels
we need to keep, and 2 is the number of minimum labels per instance should have. The generated tree structure is stored in the "data/tree_img" folder.
There are two imgs in the tree_img folder. Each label of the img with postfix ".digit" is a int number representing the index of the lable.
# edge.py
edge.py is a binary classifier for edges. It consists of the training and prediction processes. 
There are three parameter need to be filled according to the training data info printed by the clean_data.py:

"lstm_num" is the length of the output vector of the LSTM, normally shorter than the "max_length" but bigger than 1.

"max_length" is the max length of the input text we keep. For this parameter I always choose the value bigger than the average length of the training text printed by the clean_data.py.

"batchsize" I always set 32 if the dataset is not very big. 64 or 128 is Ok if the dataset is very big.

"save_data" is a command line argument. In the process of finding the point to stopping the training, you don't need to set the value. Once you find the appropriate number of epochs. You can set the "save_data" to 1 and run the file again to save the predicted results.
# original.py
original.py is a One-vs-All multi-label model. It consists of the training and prediction processes. There are 4 parameters you need to change according to your data:

"lstm_num", "max_length", and "batchsize" have the same definitions with that of the edge.py.

"dense_num" is the dimensionality of the output of the multi-label classifier which equals to the number of the labels.

"save_data" is a command line argument.  In the process of finding the point to stopping the training, you don't need to set the value. Once you find the appropriate number of epochs. You can set the "save_data" to 1 and run the file again to save the predicted results.
# enumeration.py
enumeration.py does the inference. There are two parameters you need to change according to the data:

"edge_list" is a list of the edges which are listed in your "data/store" folder. If your "data/store" folder has a list of edges: l0_l1, l0_l3, l1_l4, l3_l2,
then you can fill the edge_list with [(0, 1), (0, 3), (1, 4), (3, 2)].

"total_num" is the number of the test instances. 
# navie_bayes.py
This file implements the baseline algorithm navie bayes. There is only one parameter named "max_length" which is the same as the parameter "max_length" in original.py or edge.py. You can copy the value from original.py or edge.py.
There is one more package needed to be installed: nltk 3.4.5
# Requirement Package
all list in requirements.txt

