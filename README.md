# alteryx_text_classification

## Overview
This post will focus on the Text Classification and Zero-Shot Text Classification tools and includes a demo that sorts e-commerce listings from this Kaggle dataset into four categories: Household, Electronics, Books, and Clothing & Accessories.  8,113 records were classified, totaling 934,619 words.  An average employee might take over 65 hours to just read all 8,113 records. The provided workflow took about 30 minutes to create and 2.5 hours to run both Text Classification and Zero-Shot Text Classification, unlocking over 60 hours of productivity for this single task


## Data Preparation
I used a [Kaggle dataset](https://www.kaggle.com/datasets/saurabhshahane/ecommerce-text-classification?resource=download) with 8,113 records of ecommerce listings to classify into four category: Household, Electronics, Books Books, Clothing & Accessories. An average employee might take over 65 hours to just read all 8,113 records. The provided workflow took about 30 mins to create and 2.5 hours to run both Text Classification and Zero-Shot Text Classification. 


![Workflow Image](https://github.com/alibinkhalid/alteryx_text_classification/blob/main/Alteryx%20Workflow%20-%20Text%20Classification.png)


## Zero-shot Text Classification
Zero-shot Text Classification was released with Alteryx Designer version 22.3 and classifies text into user-defined groups using a pre-trained Hugging Face model that comes installed with Alteryx Intelligence Suite and executes using ONNX Runtime for quick execution.  Because Zero-shot comes with a pre-trained model, there’s no need for pre-labeled training data or any up-front training.

Zero-shot Text Classification is great for classifying text into well-known categories that aren’t highly specialized or expertise-dependent. No significant data preparation is required for Zero-shot text classification

Tool configuration is also simple.  In the “Column with Text” field, select the text column to classify (from the D input anchor).  In the “Column with Labels” field, select the column with potential categories (from the L input anchor).  If the “Multi-label Classification” box is unchecked, each record will be assigned to the most likely category.

![Tool Configuration](https://github.com/alibinkhalid/alteryx_text_classification/blob/main/Tool%20Config%20-%20Zero-shot.png)


## Text Classification
The Text Classification tool, released in Designer version 23.1, allows users to leverage their own labeled data to train a customized model for text classification.

In the case of the Text Classification tool, some preparation with the Text Pre-processing tool is recommended – specifically the option ‘Convert to Word Root (Lemmatize)’.  This option converts variants of a word (such as changing, changed, changes, changer) to the root word (change), and may help the classification model contextualize the text content.  Optionally, digits or punctuation might also be removed with the Text Pre-processing tool.

The create samples tool was used to split the data into training, validation, and holdout datasets.  The E and V anchors output from the tool are input as the Training (T) and Validation (V) input anchors of the Text Classification tool.  The tool outputs are a model object (M anchor), and information about the model (E anchor). 

To configure the tool, select the columns with text and labels for both the training and validation text (T and V input anchors, respectively).  In the Advanced area, select the algorithm to use for the model.  Currently, two methodologies are available: Multinomial Naïve Bayes, and Linear SVC, etc. with tunable hyperparameters described in the tool documentation.  A third option, Auto Mode, evaluates both methods and identifies an ideal set of hyperparameters. Auto Mode was used in the provided workflow, to demonstrate how simple it is to configure the tool, and how well the model performs without users needing deep expertise in text classification algorithms and parameters.

![Tool Configuration](https://github.com/alibinkhalid/alteryx_text_classification/blob/main/Tool%20Config%20-%20Text%20Classification.png)

## Results
Contingency Table tools quantified the accuracy of both text classification methods. Zero-shot Text Classification accurately classified products as Books, Clothing & Accessories, Electronics, or Household 75.3% of the time (1100+1351+1125+2530 / 8113). 
![Contingency Table tools](https://github.com/alibinkhalid/alteryx_text_classification/blob/main/Result%201.png)

The Text Classification tool, which was trained specifically to the dataset (but not the specific data that was scored) performed even better, with 95.9% accuracy (1844+1449+1265+3224 / 8113).

![The Text Classification tool](https://github.com/alibinkhalid/alteryx_text_classification/blob/main/Result%202.png)

