# 340-implementation

Throughout the course of implementation we took for our approach, we noticed some of the results indicating some interesting findings. Upon closer examination of the parent paper, it was understood that when fitting predictive models, the dataset included several variables that were effectively surrogate to the chosen response variable in the dataset. These were specifically for the chosen response variable “Please tell us how the amount of alcohol you are consuming has changed?”:

“In January 2020, approximately how often did you have a drink containing alcohol?”
“In the last month, approximately how often did you have a drink containing alcohol?”

The feature importance representations of the models containing these variables are highly telling of these variables pseudo-response status (In figure 1 these are Q19 and Q20 respectively).
Removing these response-surrogate variables from the dataset, and applying the methods from the parent paper allowed for investigation of the impact of other features, that were otherwise dominated in the model prediction process, on model fitting. Examining this adjustment through the lens of an investigator, without a pre-existing hypothesis allowed for a deeper understanding of predictive drivers.

In the parent text, Rezapour makes the explicit assumption that change in alcohol consumption is reflective of change in mental health. While not an inherently unreasonable assumption, it fails to consider that individuals may consume more or less alcohol for reasons pertaining to leisure, time, or even the availability of alcohol itself.

Assessing change in mood as an indicator of mental health accomplishes two key tasks. It firstly considers the problem from a different lens, and provides a benchmark that fundamentally relies on fewer assumptions. On a secondary level however, using feature weights, can provide a reasonable picture of how effective a predictor it is. This can directly test the underlying assumption in the parent implementation - that change in alcohol consumption reflected change in mental health.

Directly predicting change in mood required rewriting Rezapour’s data cleaning and encoding, as the column pertaining to how mood had changed (Q29a), he elected to remove. The question “Please tell us how your mood has changed.  My mood has been:” (Q29a) was a sparse field, and only contained values when the response to “Has your mood changed?” (Q29) was “yes”. Using Q29, it was possible to create a dense Q29a. Using the original pre-processed dataset, and its sparse indices, an index map was created to assign new dense Q29a values to all cases. This value was then encoded.

Reapplying the same model types used in the parent paper, it immediately became apparent that the encoding scheme greatly dictated test accuracy. In the dataset, there are three levels of change (slight, moderate, and great) for both improvement and worsening. Thus, including the “no change” response, there are seven effective categories. When treating each label as a category, the model performed very poorly, due to the number of possible classifications. A reductionist approach - labeling as -1, 0, and 1 for negative change, no change, and positive change, while better, yielded only marginally better results. The naive reductionist labeling likely created a confounding variable. Slight change in mood was likely a more random, frequent occurrence than severe change in mood. Change in mental health likely does not correlate to the latter - being  in a slightly worse mood could be the result of any number of things that could occur by nothing more than happenstance. High correlation was only exposed by the model when the threshold for no change was moved to include the labels “slightly worse” and “slightly better”.

Simple models were applied to the dataset, to determine the predictability of the Q29a label. Note that during modeling, the Q29 attribute was removed from the dataset due to Q29a’s inherent dependency on it. The models performed reasonably well considering that there were not two but three classes. The model that performed the best was the SVM, suggesting a moderate degree of linear separability in the data, and that linearly separable groupings correspond well to classes. While MLPs generally perform well against other classifiers for most datasets, the MLP performed the worst by a reasonable margin, despite Perceptrons also being separability methods. This may be attributable to insufficient dataset size - Neural Networks generally require more data than other algorithms.
