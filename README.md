# Data Distillation Using Quantitative Input Influence

We used [Quantitative Input Influence tool](https://www.andrew.cmu.edu/user/danupam/datta-sen-zick-oakland16.pdf) from [this](https://github.com/cmu-transparency/tool-qii) repository and [train_test_split function](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html) from scikit-learn to perform data distillation on the 2019 American Community Survey data.   <br />

## Flowchart
The general process of evaluating our QII-awared sampling is shown in the following figure.

![DL Project-Evaluation](https://user-images.githubusercontent.com/70864873/145465078-c13ae746-f0ed-4f8e-ad49-fd9b356ddafb.png)


## File Explaination
[reference_model.ipynb](exp_QII_features.ipynb) <br />
This notebook implements our baseline MLP model.  <br />
It first loads the ACS dataset and does data preprocessing. The preprocessing includes: <br />
1) Drop all data with age under 16 (legal work age) <br />
2) One-hot encoding for categorical data. <br />
3) All NaN values will be replaced by zero. <br />
4) All flag features that have the same value for more than 99\% will be dropped.
5) All features with more than 60\% being null value will be dropped. <br />

Then, we send the processed data to the MLP model, and get the plot of R<sup>2</sup> versus epoch. <br />
We also plot the training and testing loss and accuracy.


[exp_QII_features.ipynb](exp_QII_features.ipynb) <br />
This notebook implements MLP model with the QII-aware features. <br />
It applies the same data preprocessing methods, and we only take the top N QII features (with highest Shapley values) to train the MLP. <br />
Then we plot the R<sup>2</sup> versus epoch.<br />
We also plotted the training and testing loss and accuracy. <br />

[exp_Stratification.ipynb](exp_Stratification.ipynb) <br />
This notebook implements MLP model with stratified data points.<br />
It applies the same data preprocessing methods, and we use ``` train_test_split ``` function from ```scikit-learn``` library to split the data.<br />
Then we feed those stratified data into our MLP model, and get the plot of R<sup>2</sup> versus epoch.<br />
We also plott the training and testing loss and accuracy. 


[QII.ipynb](QII.ipynb) <br />
This notebook implement QII method using functions from [this](https://github.com/cmu-transparency/tool-qii) repository.<br />
It applies the same data preprocessing methods, and then take samples from the original dataset to select most important features.<br />
Since QII functions from the repository require the label to only have 2 categories, we categorize the label in three different way: 
1) Poor vs. Rest
2) Medium vs. Rest
3) Rich vs. Rest

We apply QII method to each of the label-categorized data, and get the most important features for each of them.<br />
Then we can use the most important features to do stratification.<br />

For more detailed explanation of each part of the code, please refer to the section titles and comments inside the notebooks.

## Direction for running
For each notebook, running from top to bottom should give correct output. <br />
To run the baseline, run [reference_model.ipynb](exp_QII_features.ipynb). <br />
To apply QII on sampled dataset and get QII features, run [QII.ipynb](QII.ipynb). <br />
To use only top N features as the input to the model, run [exp_QII_features.ipynb](exp_QII_features.ipynb). <br />
To do data stratification based on the most important feature, run [exp_Stratification.ipynb](exp_Stratification.ipynb).

# Reference

[Quantitative Input Influence Tool](https://github.com/cmu-transparency/tool-qii) <br />
[Anupam Data, Shayak Sen, and Yair Zick Algorithmic Transparency via Quantitative Input Influence: Theory and Experiments with Learning Systems](https://www.andrew.cmu.edu/user/danupam/datta-sen-zick-oakland16.pdf)
