==T-Brain E-Sun_Credit_Card_Fraud_Detection  
Hello everyone, we are Drunkard on the Way, here are our solution  

##FEATURE ENGINEERING  
>1. Simply Aggregations  
 >>a) mean  
 >>b) std  
 >>c) nunique  
>2. Combine Features
>2. Frequency Encoding  
>3. Lable Encoding  
>4. Count Encoding  
>5. Target Encoding  
 >>a) target mean  
 >>b) beta target mean  
>6. FastICA  
>7. SparsePCA  
>8. TruncatedSVD

##FEATURE SELECTION  
>1. RFECV  
>2. lightGBM feature importances  
>3. forward feature selection

##MODEL  
Our solution was based on stacking. We create 5 lightGBM models, 1 XGBoost model, 1 Catboost model and 1 NN model all use 5-Kflod unshuffled.  
>1. LightGBM  
  >>a) Arctic's Base  
  >>b) Jeff's Base  
  >>c) Arctic's Base add scale_pos_weight  
  >>d) Arctic's Base add DAE features  
  >>e) Stack XGBoost and CatBoost  
>2. XGBoost  
  >>Arctic's LightGBM features  
>3. CatBoost  
  >>Arctic's LightGBM features  
>4. DAE+NN  
  >>first, We create a Denosing Autoencoder(DAE) with 3 hidden layers, normalized categorical features using RankGauss, binary features transfer to 1/-1 and continuous features using StandardScaler, relu as the activation function, optimizer use SGD.  
  >>second, NN is trained with both lightGBM features and DAE latent features, NN have 3 hidden layers(256-128-64),dropouts(.25-.2-.15), use batch normalization after activation, gelu as the activation function, optimizer use Adam, focal loss ad th loss functuin.  

##Validation Strategy  
>1. train 75% and predict 25%  
>2. train first 30 days and predict last 30 days  
