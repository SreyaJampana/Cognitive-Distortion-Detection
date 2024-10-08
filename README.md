
# **SUMMARY**
# Cognitive distortions are irrational thoughts influencing mental health and are closely linked to self-harm. This paper explores the use of natural language processing (NLP) techniques to identify cognitive distortions and suicide risks in written text. Various models were tested, with BERT-base showing the highest accuracy of 53%. Cognitive distortions were inferred on a dataset of Reddit posts labelled as suicidal and non-suicidal, showing that distortions like overgeneralization and mental filtering are prevalent in suicidal texts. This research highlights the relationship between cognitive distortions and suicide risk, suggesting that certain distortions could be early indicators of suicidal ideation. Findings support using NLP models for mental health interventions, but future improvements, such as more training epochs, multi-task models, and larger datasets, are needed to enhance accuracy and intervention efficacy.
-----
# **KEYWORDS**
**Cognitive Distortions, Suicide-Risk, Natural Language Processing, BERT-base**
#
# **INTRODUCTION**
Cognitive distortions are inaccurate or irrational thoughts which negatively affect how individuals perceive themselves and others. Statements such as “I’m never good enough”, “I always fail” and more, are all examples of distorted thought patterns. This skewed interpretation of events or experiences can often contribute to negative emotional states, such as anxiety, depression and low self-esteem. Psychiatrist, Aaron Beck[<sup>1</sup>](#bookmark=kix.ubpcirn03h2k) was the first to identify cognitive distortion. Today, these distortions play an important role in signaling for mental health intervention. Current methods for identifying cognitive distortions largely rely on manual review and self report questionnaires. These are time-consuming, highly subject to bias and not feasible for large datasets.[<sup>2</sup>](#bookmark=kix.albcbsa11j7g) Models designed for sentiment analysis or topic modeling, are not tailored to recognize the complex linguistic patterns which indicate cognitive distortions as well. Research has shown cognitive distortions (β = 0.188, t=3.940) being positive and significant predictors of self-harming behaviors in adolescents.[<sup>3</sup>](#bookmark=id.3ib0b69vzgic) Self harm is highly prevalent amongst young teens. 2024 statistics indicate that 1 out of 14 people commit acts of self harm[<sup>4</sup>](#bookmark=kix.2d5i5e3f786k). By taking on a clinical approach to self harm, early signs often go undetected. In contrast, if individuals' speech, social media posts and texts could be analyzed for indication of self harm, several lives could be saved. The need for a novel approach to suicide-risk detection exists. 

# **METHOD**
This study makes use of existing free data sets related to cognitive distortion and suicidal risk. In order to map out the relationship between cognitive distortion and suicide risk, a data set where both are labelled is needed. To obtain this, a semi-supervised approach will be implemented. Starting with a smaller cognitive distortion labeled data set, a model will be trained. This model will then be used to augment the results into a larger dataset. It will provide soft labels on cognitive distortion for the suicide risk data set. By comparing the soft labels for the suicide risk data set with the “suicidal” and “non-suicidal” labels, meaningful conclusions can be drawn regarding the connection between cognitive distortions and self harm. 

**Data Collection**

The cognitive distortions data set being used is "Detecting Cognitive Distortions from Patient-Therapist Interactions" by Sagarika Shreevastava and Peter W. Foltz - 10.18653/v1/2021.clpsych-1.17[<sup>5</sup>](#bookmark=kix.g6hn4kxhzeka). This data set has been compiled from therapist Q&A and was annotated by licensed therapists.The total data set consists of 2530 annotated samples of the patient's input. The cognitive distortions identified are according to the 10 distortions published by Burns[<sup>6</sup>](#bookmark=kix.vl6o36qvhpk0).The suicide-risk dataset being used is “Suicide and Depression Detection” sourced from Kaggle.[<sup>7</sup>](#bookmark=id.nhr240bw3l4w) The data set is a collection of posts from r/SuicideWatch and r/teenagers subreddits. They were collected using PushShift API. All of the posts from the SuicideWatch are labelled as suicide since they come from a space where teens are reaching out for help. The “non-suicide” labelled posts are taken from r/teenagers. All posts on these sub reddits from Dec 2008 till Jan 2, 2021 were collected, summing up to a total of 232074 unique samples. 

**Data Pre-Processing**

Prior to training the model, data pre-processing (tokenization and vectorization) was necessary to provide the model with input which was easier to understand. Since different types of models were experimented with, the tokenization process slightly differed according to the approach.

To prepare the data for the rule-based and logistic regression model, standard level tokenization was used. This was achieved using TFIDF Vectorization. The entire corpus of text is transformed into a matrix of numerical values, where each row corresponds to a document and each column corresponds to a word from the vocabulary, with the values representing the TF-IDF score of each word in the document. For the BERT models, a more advanced WordPiece Tokenizer[<sup>8</sup>](#bookmark=kix.6c9uulnb8f82) was used. This tokenizer creates sub-words and keeps context in mind by preserving the relationship between words. ![](Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.001.png)

Another important step in pre-processing was over sampling the data. Since the distribution of labels was not even in the cognitive distortions data set, the other labels had to be increased. This was achieved using RandomOverSampler which duplicates random instances of the minority classes. 

**Training the Model**

To build the model with the highest accuracy, several training approaches were tested. 4 different models were built, from which the one with the highest accuracy was used. The primary model was a simple rule-based one. The rule-based model operated on key-words found for each cognitive distortion. The key words were identified using WordClouds. However due to the lack of context and simplicity of the model, the accuracy was extremely low. The next model tested was logistic regression.[<sup>9</sup>](#bookmark=kix.205qec7v82im). This model is typically advantageous in distinguishing between two classes. Hence, for training a model on cognitive distortions it was ineffective and only had an accuracy of 24%. 

Finally, BERT was used to identify cognitive distortions. In the first test DistillBert was used with the original data set in order to save on RAM usage. However, the accuracy remained low at 35%. Then the data was oversampled, increasing the data points. A full fledged BERT-base architecture was also utilized. The AdamW optimizer was employed with a learning rate of 5e-5, a crucial hyperparameter that governs the model's learning pace. A learning rate scheduler was also utilized to adjust the rate during training for enhanced convergence. The model was trained over four epochs, with each epoch processing the complete training set. A decrease in loss with each epoch was observed, showing that the model’s predictions improved with each iteration. 

This approach drastically improved the F-1 score to 54%. A summary of the models tested is given below. 
|*Table 1.3: Full Accuracy Report for BERT-base* |*1.2: Accuracy of Trained Models*|
| :-: | :-: |
|![](Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.002.png)|![](Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.003.png)|

**Inferring Cognitive Distortions in Suicide-Risk Dataset**

This model was then called upon to infer cognitive distortions within the suicide-risk dataset. The complete dataset of 232074 samples was successfully labeled. 

# **RESULTS AND DISCUSSION**
Using the dataset annotated with both cognitive distortions and suicide classifications, several visualizations were employed to provide insights into their relationship

To understand the distribution of cognitive distortions within the dataset, a pie chart was used (Figure 1.3). This chart showed that the majority of texts lacked cognitive distortions. However, within the set of cognitive distortions the most common ones were “overgeneralization”, “mental filtering” and “fortune telling”. This indicates that these might be the most dangerous or risky distortions amongst teens since they are interrelated with suicide. A proportion bar chart (Figure 1.4) was also generated to show the percentage of cognitive distortions present in suicidal versus non-suicidal texts. The diagram highlighted that certain distortions such as “labeling” were not as risky and found commonly within non-suicidal cases. However, certain cognitive distortions such as “mental filtering” and overgeneralization were high indicators of suicide risk. Word clouds (Figure 1.5) were also created to visualize the most frequently occurring terms within suicidal and non-suicidal texts, offering an intuitive understanding of the linguistic features that may differentiate the two groups. It is evident that suicidal texts have more repetition of words, while non-suicidal thoughts use a wider dictionary of words. 

|<p>![](Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.004.png)</p><p>*Figure 1.3*</p>|<p>![](Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.005.png)</p><p>*Figure 1.4*</p><p></p>|<p>![ref1]</p><p>![ref2]</p><p>*Figure 1.5*</p><p></p>|
| :- | :- | :- |

Finally, a bar chart (Figure 1.6) compares the presence of cognitive distortions overall in suicidal vs non-suicidal texts. This chart shows that suicidal texts have almost double the number of cognitive distortions. This clearly suggests that cognitive distortions are heavy indicators of suicide-risk.

|![](Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.008.png)|
| :- |
|*Figure 1.6*|

Overall, these visualizations demonstrate a clear relationship between cognitive distortions and suicidal risk, suggesting that certain distortions may be indicative of heightened suicide risk. Future work could leverage these insights to develop targeted interventions aimed at reducing cognitive distortions in at-risk populations, potentially mitigating suicidal ideation and behavior.

#

# **CONCLUSIONS**
The findings of this study hold significant implications for clinical practice and mental health interventions, providing insight into the fact that distortions are predictors of suicide-risk. This targeted awareness allows for more effective cognitive-behavioral strategies to be employed, facilitating the modification of harmful thought patterns. By integrating cognitive distortion assessments into routine screenings, mental health systems can proactively identify at-risk individuals, ultimately contributing to suicide prevention efforts and improving overall mental health outcomes. In this paper, we have been able to effectively build a BERT-base model which can identify a range of cognitive distortions within text. The model has reached an accuracy of 53%. Our findings showed that distortions such as “Overgeneralization” and “Mental Filtering” are significant indicators of suicidal thoughts, since they are highly prevelant amongst suicidal posts. The findings also showed that cognitive distortions are much more common amongst suicidal posts than non-suicidal ones. This indicates that distortions are early signs of self harm risk. This research also has potential for further expansion. Due to time and resource constraints, certain aspects like improving the cognitive distortion prediction model through increasing the number of training epochs or synthetic data augmentation were not explored. Enhancing the model would require greater processing power and access to a stronger GPU. From the data set created, a multi-task model could be developed too. This would give insight into how cognitive distortions can improve a suicide-detection model’s accuracy. Future studies with a more diverse dataset, especially with nuanced or implicit mentions of suicide, would also yield better results. Future research could expand upon these findings by examining the role of cognitive distortions in diverse populations or utilizing longitudinal data to track changes over time. In summary, understanding the interplay between cognitive distortions and suicidal risk is crucial for developing effective mental health interventions that could save lives.

# **REFERENCES** 
1. <a name="bookmark=kix.ubpcirn03h2k"></a>Beck, A.T., Rush, A., Shaw, B., & Emery, G. (1979) Cognitive therapy of depression. New York, NY, USA: Guilford. 
1. <a name="bookmark=kix.albcbsa11j7g"></a>Kendall, P.C., Hollon, S.D. Anxious self-talk: Development of the Anxious Self-Statements Questionnaire (ASSQ). *Cogn Ther Res* **13**, 81–93 (1989). <https://doi.org/10.1007/BF01178491>
1. F<a name="bookmark=id.3ib0b69vzgic"></a>ard, Marzieh & Neudehi, Masoumeh & Jamshiddoust, Fatemeh & Solgi, Zahra. (2023). The Role of Childhood Trauma, Cognitive Flexibility, and Cognitive Distortions in Predicting Self-harming Behaviors among Female Adolescents. Caspian Journal of Health Research. 8. 85-92. 10.32598/CJHR.8.2.445.1. 
1. <a name="bookmark=kix.2d5i5e3f786k"></a>Brownlee, M. (2024, September 5). *Suicide statistics: 2024*. Champion Health, https://championhealth.co.uk/insights/suicidal-thoughts-statistics/
1. Shreevastava, S. (2024) Cognitive distortion detection dataset. Kaggle. <a name="bookmark=kix.yogflmqtbfa1"></a><https://www.kaggle.com/datasets/sagarikashreevastava/cognitive-distortion-detetction-dataset>
1. <a name="bookmark=kix.vl6o36qvhpk0"></a>Burns, D. D. (1980). *Feeling good: The new mood therapy*. New York, NY, USA: Signet.
1. Komati, N. (2024) Suicide watch dataset. Kaggle. <a name="bookmark=id.nhr240bw3l4w"></a><https://www.kaggle.com/datasets/nikhileswarkomati/suicide-watch>
1. Hugging Face. (2024) Tokenizer documentation. Hugging Face. <a name="bookmark=kix.6c9uulnb8f82"></a><https://huggingface.co/docs/transformers/en/main_classes/tokenizer>
1. Amazon Web Services (2024) What is logistic regression? <a name="bookmark=kix.205qec7v82im"></a><https://aws.amazon.com/what-is/logistic-regression/#:~:text=Logistic%20regression%20is%20a%20data,outcomes%2C%20like%20yes%20or%20no>.

[ref1]: Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.006.png
[ref2]: Aspose.Words.422cb70a-a769-4387-8005-5d7d508dd126.007.png
