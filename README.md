# Prediction-of-Drug-Protein-Interaction
This work uses PROBABILISTIC MATRIX FACTORIZATION model to study the interactions between existing drugs and the well-known proteins in human or animals. The motivation is trying to provide a guideline for the future drug discovery research and also help to explain the side effects of certain drugs.
## Data Source
The original data set was downloaded from DrugBank database. There are 7370 drugs represented by “DrugBankID” and 4763 proteins (targets) represented by “UniProtID”.
## Model
The Probabilistic Matrix Factorization (PMF) model [1] is used for the prediction. The implementation of PMF was found from Github [2]. This author used PMF to predict the user’s rating on the movies that the user has not watched. The rating score ranges from 1 to 5 in his work. The prediction of drug and target interaction is similar to the prediction of user and movie relationship. But the scale here is 0 to 1 with 0 indicating no interaction and 1 indicating interaction with a probability of 100%. In order to use the PMF model, the drug ID, target ID and interaction scale have to be changed to integer. Below is what we did to change the labels.
All the DrugBankIDs are saved in a dictionary with the DrugBankID as the key and the integer value starting from 1 as the value:
![Image description](https://github.com/hanzheng0730/Prediction-of-Drug-Protein-Interaction/blob/master/Image/dict1.jpg)
Then by using the map() function, a new column of “DrugBankIDLabel” can be generated and its values are integers.
![Image description](https://github.com/hanzheng0730/Prediction-of-Drug-Protein-Interaction/blob/master/Image/map1.jpg)
Similarly, a new column of “UniProtIDLabel” is generated. For the report drug-target pair, I put the interaction value as "1". In order to train a good model, I have to generate negative samples. In thiss case, the negative sample means a drug-target pair with an interaction probability of 0. The Drugbank database only provides positive samples while there are no experimentally-validated negative samples. So I
can only assume some drug-target pairs have 0 probability to be interacting with each other although they must not be real negative samples. The way that I generate the negative samples is to randomly generate a drug-target pair and then verify if it exists in the positive sample set, if yes, then re-generate a new pair, if no, this pair is saved, and then go back the for loop to generate another pair until the loop ends. The below code iterates 30000 times and “data1” finally includes 20061 positive samples and 29992 negative samples:
## Parameters and Results
The optimal result occurred at n_feature=30, n_iters=100. The predicted data was exported to “PMF Generated data_30000_fea30_ite100.csv”. The current model can be used to predict the interaction probability for any drug-target pair but with limited accuracy. 
![Image description](https://github.com/hanzheng0730/Prediction-of-Drug-Protein-Interaction/blob/master/Image/resultScreenshot1.jpg)
![Image description](https://github.com/hanzheng0730/Prediction-of-Drug-Protein-Interaction/blob/master/Image/resultScreenshot2.jpg)
As shown in the result, the predicted interaction probability for the positive samples is supposed to be very close to 1 but it is actually much smaller than 1. The RMSE (0.3692) and AUC score (0.8760) still have some room to improve. The future work will have to take the chemical similarity into consideration when generating the negative samples. For example, given a known interaction of drug i and target j, for a drug k, the more difference between drug i and drug k, the less possibility that drug k interacts target j. It is expected to see improved prediction accuracy after the chemical similarity is taken into account.
## Reference
1. Ruslan Salakhutdinov and Andriy Mnih. Probabilistic matrix factorization. In Advances in Neural Information Processing Systems; NIPS, 2007 20: 1257-1264 
2. Implementation of PMF: https://github.com/chyikwei/recommend/tree/master/recommend





