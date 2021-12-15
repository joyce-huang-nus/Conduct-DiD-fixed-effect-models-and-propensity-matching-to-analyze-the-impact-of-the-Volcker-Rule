# Conduct-DiD-fixed-effect-models-and-propensity-matching-to-analyze-the-impact-of-the-Volcker-Rule
The Volcker Rule was announced to restrict banks from conducting certain investment activities with their own account in order to decrease risks taken by the bank, and hence to have a greater stability. Due to the restrictions, banks might argue that their profitability has decreased and therefore, assumption was made to analyze the impact of the Volcker Rule. In this project, we used the Difference-in-difference (DiD) models, including the baseline DiD model and the robust DiD models that considered the heterogeneity of different time points and individual bank holding companies by adding the fixed effect technique. In addition, we also conducted a propensity match to compare the post Volcker Rule results with the most similar results before the announcement of the Volcker Rule.

## 1. Introduction
### 1.1 Problem Statement
The Volcker Rule was announced to restrict banks from conducting certain investment activities with their own account in order to decrease risks taken by the bank,  and hence to have a greater stability. Due to the restrictions, banks might argue that their profitability has decreased and therefore, assumption was made to analyze the impact of the Volcker Rule. In this project, we used the Difference-in-difference (DiD) models, including the baseline DiD model and the robust DiD models that considered the heterogeneity of different time points and individual bank holding companies by adding the fixed effect technique. In addition, we also conducted a propensity match to compare the post Volcker Rule results with the most similar results before the announcement of the Volcker Rule. 

### 1.2 Data cleaning and Feature Engineering
There are 81560 rows and 14 different columns in the original datasets we collected. We noticed that some of the grids contain null values, therefore we dropped the rows that contain null values and eventually started our modeling with 40026 rows and 14 columns. For detailed feature engineering, please see Appendix Table 01.

## 2.Assumption
The affected banks started to reduce their trading asset ratios after the announcement of the Volcker Rule.

## 3. Models
### 3.1 Simple DiD model with a time indicator

Y i,t =α + β ∗ after DFAt+ εi,t

We conducted a simple OLS model with a time indicator to see the impact of the Volcker Rule to all BHCs’ trading asset ratio. The adjusted R-squared is 0 and the coefficient is not significant at a 5% level,  so the model doesn’t explain much of the effect of the Volcker Rule. See the result in Appendix Result 01: Simple DiD model with a time indicator.

### 3.2 Simple DiD model with a time indicator and control variables

Y i,t =α + β ∗ after DFAt+ Xi,t + εi,t

Based on the simple DiD model, we added 9 control variables into the model. The adjusted R-squared had improved to 0.234, and our main independent variable  after_DFA_1 had a significant p-value of 0.0000 and a more sensible negative coefficient of -0.001. This result also is slightly consistent with our Assumption: The affected banks started to reduce their trading asset ratios after the announcement of the Volcker Rule. See the full result in Appendix Result 02: Simple OLS model with a time indicator

### 3.3 DiD model Including the interaction between the affect and after DFA

Y i,t =α +β1*Affected BHCi+β2* after DFAt+β3 ∗ (after DFAt ∗ Affected BHCi)+Xi,t+εi,t

For this model, we selected the BHCs whose trading asset ratio during the pre-DFA period is larger than 3% to be the affected BHCs.  We used a binary variable: treat_3_b_avg to distinguish the affected BHCs. Moreover, we considered the interaction between the affected BHCs and After DFA to be our main measurement of the impact of the Volcker Rule. The model’s adjusted R-squared improved again to 0.556. The coefficient (β3) of the interaction term was 0.0037, which somehow is not consistent with Assumption. See the full result in Appendix Result 03: DiD model Including the interaction between the affect and after DFA

### 3.4 Robustness tests: DiD model adding fixed effect: over time & across BHCs

Y i,t =α + β1*Affected BHCi+β2* after DFAt +β3∗ (after DFAt ∗ Affected BHCi)
+ δt +γi  + Xi,t + εi,t

To add in the fixed effect over time and across BHCs, we got dummy variables from each value in column [‘rssd9999’] and [‘rssd9001’] and plugged into our model. The adjusted R-squared had increased significantly to 0.918 and the coefficient (β3) of the interaction term changed to -0.0234, which is consistent with Assumption. 

Also, we focused on the coefficients of entity fixed effects . We found 28 banks whose fixed effect is significantly negative (p <= 0.01) and 19 treatment banks are included, which means banks with higher trading ratios responded more to the regulation. 
See the full result in Appendix Result 04: Robustness tests: DiD model adding fixed effect: over time & across BHCs and Result 05: Fixed effect models comparison

### 3.5 Robustness tests: Propensity score Matching

In this model, we used propensity score matching for the treatment and control group to reduce bias from the estimation of groupings of before and after the announcement of the Volcker Rule. 

First of all, we chose to use the data from the third quarter of 2004 in our propensity matching. We used logistic regression to execute propensity score matching, and took ‘‘Affected BHC’ as a dependent variable, and the control variables as the independent variables. We then computed and selected 3 control banks that  have the closest propensity score for each treated bank. 

After matching, 1684 observations left. The adjusted R-squared of the model is 0.939, and the interaction term is significantly negative. See the result in Appendix Result 06: Propensity Matching.  

### 3.6 Robustness tests: Propensity score Matching Excluding Non-Trading BHCs

In the final model, we only select the data whose trading asset ratio is greater than 0 (4776 rows), and the result is similar to model 3.5, meaning that those who traded assets were truly affected. See the full result in Appendix Result 07: Propensity Matching Excluding Non-Trading BHCs.

## Conclusion

### (i) Did the banks decrease their trading assets after the announcement of the new regulation?

The answer is an obvious yes. Depending on the statistical findings we got from 3.2 Simple DiD model with a time indicator and control variables, the coefficient of the time indicator is significantly negative, meaning that the banks decreased their trading assets after the announcement of the Volcker Rule. 

### (ii) If they responded to the regulation, which banks responded most and which banks least? Why?

<img width="517" alt="ratio" src="https://user-images.githubusercontent.com/88580416/146179618-0b3ebe0d-2382-46b0-ac00-1451da4d8caf.PNG">

From the simple graph above, we can see that banks with high trading assets ratios responded more to the Volcker Rule, especially the top 10, decreasing from 11% to 8.5%. And those who did not trade assets a lot were not affected. In 3.3 DiD model Including the interaction between the affect and after DFA, the significantly negative coefficient of interaction term further demonstrated that those who were truly trading assets indeed responded.

After we added fixed effects into the model 3.4 Robustness tests: DiD model adding fixed effect: over time & across BHCs, we found most of the banks whose fixed effect is significantly negative (p <= 0.01) are treatment ones, which means banks with higher trading ratios responded more to the regulation. 

And in 3.5 Robustness tests: Propensity score Matching, the statistical result actually did not change very much compared to model 3.4. So, we confirm that the treatment group responded more than the control group.

In conclusion, banks who were affected by the regulation (treatment group) responded most, and those who traded assets little prior to the rule responded least.

### (iii) Remember robustness, and how should banks or regulators use these results?

 The Volcker Rule aims to protect bank customers by prohibiting banks from making a few types of risky investments which caused the 2008 financial crisis. Therefore, from the regulator’s perspective, they hope that the bank can be compliant to the Volcker Rule and would have a lower trading asset ratio after the announcement. To analyze whether the announcement of the Volcker Rule has a negative impact on the trading asset ratio, we can look at the coefficient (β) of the interaction term in the most rigorous model (propensity score matching excluding non-trading BHCs) and the β is  -0.021. This indicates that, overall,  the Volcker Rule has successfully limited most banks to trade in the high risk investment instruments with their own accounts and therefore, can avoid another disastrous financial crisis. 

However, the absolute value of the coefficient is quite small, we speculated that some banks are still not willing to comply with the new rule because they think that their profitability has significantly decreased after the announcement of the Volcher rule. Moreover, the banks that comply with the rule might also think of some loopholes, such as increasing the risks of the permitted investments, and there might be unseen problems in the future. To understand the rule’s effectiveness, the regulator should further figure out whether the affected banks’ risk indeed decreased and by how much.

# Appendix

In the beginning, we used the simple DiD model with a time indicator, and got the adjusted R-squared 0, explaining nothing about the Volcker Rule. We tried to include the interaction between the effect and after DFA, and got the better result, adjusted R-squared improved to 0.566. Moreover, after adding fixed effects across BHCs, the adjusted R-squared increased to 0.917. Finally we got the best result from the Propensity score matching model with all BHCs, the adjusted R-squared is 0.939. We get the statistical significance in our final result.

### Table 01: Definition of the columns 

<img width="526" alt="feature" src="https://user-images.githubusercontent.com/88580416/146179877-a0dd7216-dcbb-49c1-85a2-bc46a1ce8bd9.PNG">
<img width="526" alt="feature_02" src="https://user-images.githubusercontent.com/88580416/146179953-b50dd9cc-9fd6-4029-bacb-754a030f391a.PNG">

### Result 01: Simple DiD model with a time indicator
bhc_avgtradingratio =0.0024+0.0004 after_DFA_1 


