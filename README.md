# T-Brain E-Sun_Credit_Card_Fraud_Detection  
Hello everyone, we are Drunkard on the Way, here are our solution  

## FEATURE ENGINEERING  
>1. Simply Aggregations  
 >>a) Mean  
 >>b) Std  
 >>c) Nunique  
>2. Combine Features
>2. Frequency Encoding  
>3. Lable Encoding  
>4. Count Encoding  
>5. Target Encoding  
 >>a) Target Mean  
 >>b) Bayesian Target Mean  

## FEATURE SELECTION  
>1. RFECV  
>2. LightGBM Feature Importances  
>3. Forward Feature Selection  
>4. Permutation Importance(eli5)  
>5. Time Consistency

## MODEL  
Our solution was based on stacking. We create 5 lightGBM models, 1 XGBoost model, 1 Catboost model and 1 NN model all use 5-KFlod unshuffled.  
>1. LightGBM  
  >>a) Arctic's Base                                   LB:0.664281 public/ 0.667221 private  
  >>b) Jeff's Base                                     LB:0.672959 public/ 0.673165 private  
  >>c) Arctic's Base add scale_pos_weight              LB:0.648713 public/ 0.658079 private  
  >>d) Arctic's Base add DAE features                  LB:0.636788 public/ 0.647698 private	 
  >>e) Stack XGBoost and CatBoost                      LB:0.664136 public/ 0.670637 private  
>2. XGBoost  
  >>Arctic's LightGBM features                         LB:0.662387 public/ 0.665913 private  
>3. CatBoost  
  >>Arctic's LightGBM features                         LB:0.647348 public/ 0.657357 private  
>4. DAE+NN  
  >>First, We create a Denosing Autoencoder(DAE) with 3 hidden layers, normalized categorical features using RankGauss, binary features transfer to 1/-1 and continuous features using StandardScaler, relu as the activation function, optimizer use SGD.  
  >>Second, NN is trained with both lightGBM features and DAE latent features, NN have 3 hidden layers(256-128-64),dropouts(.25-.2-.15), use batch normalization after activation, relu as the activation function, optimizer use Adam, focal loss as the loss functuin.  

## VALIDATION STRATEGY  
>1. train 75% and predict 25%  
>2. train first 30 days and predict last 30 days  
>3. GroupKFlod by month  
>4. StratifiedKFold  
>5. KFlod  

## ENSEMBLING METHODOLOGY  
>>First, We blend 4 LightGBM model(Arctic's base + Jeff's Base + train with DAE features*0.5 + Arctic's Base add scale_pos_weight*0.5), set fraud_ind >= 1 to 1
>>Second, We blend XGBoost, CatBoost and NN(1+1+1), we just keep there intersection (fraud_ind == 3)
>>Final, stack two blend model


## Reference
> Beta Target Encoding: https://www.kaggle.com/c/avito-demand-prediction/discussion/60059  
> Denosing Autoencoder: https://www.kaggle.com/c/porto-seguro-safe-driver-prediction/discussion/44629  
> Time Consistency: https://www.kaggle.com/c/ieee-fraud-detection/discussion/111308  
