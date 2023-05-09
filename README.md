# Ames, IA Home Price Prediction

---

## Problem Statement

The goal of this project is to conduct a comprehensive analysis of the factors that exert the greatest influence on the market value of homes in Ames, IA. Additionally, we aim to develop a robust model that can accurately predict home prices, achieving an R-squared score of at least 0.8. 
By doing so, this project will enable local real estate agents to offer more informed guidance to their clients, helping them to make better decisions when buying, renting, or selling properties in the area. 

--- 

## Data

The project uses two datasets: 
* [train.csv](/data/original/train.csv): This data contains all of the training data for the model 
* [test.csv](/data/original/test.csv): This data contains the test data for the model. The target variable (`saleprice`) is removed from this data set. 

The following file is also available: 
* [sample_sub_reg.csv](/data/original/sample_sub_reg.csv): This is an example of a correctly formatted submission for this challenge (with a random number provided as predictions for `saleprice`)

Data cleaning steps were implemented for both the train and test datasets, which included handling missing and `NaN` values, renaming columns, dropping unnecessary columns, and dummifying columns.

---

## Exploratory Data Analysis (EDA)

During the EDA process, several charts were created to explore the data and identify unique trends in the housing market of Ames, IA. A histogram chart was used to show the distribution of home sale prices, which ranged from $12,789 to $611,657, with a median sale price of around $162,500. 
![home_sale_price](/images/home_sale_price.png)

To identify key factors influencing home values, the Opendoor article's 8 critical factors were taken into account, and new interaction columns were created as a feature engineering approach for the models.

The dataset revealed a positive correlation between sale price and the neighborhoods of Northridge Heights, Northridge, and Stone Brick and Brick Face. A new column named `home_size` was created by combining `gr_liv_area` and `total_bsmt_sf`, which resulted in a stronger correlation with saleprice. Newer houses tend to have a higher appraised value, and the `year_built` variable showed a strong positive correlation with saleprice. An interaction column named `age_cond` was created to address the weak negative correlation between `overall_cond` and `saleprice`. `Overall_qual` had the strongest positive correlation with `saleprice`, indicating that homes with great finish quality or a move-in-ready condition are sold at higher prices.

Updates and upgrades such as a good quality finished basement and remodels were found to increase the saleprice of a home. A new interaction column named `upgrades` was created by combining several variables related to upgrades and updates, but it did not result in a major improvement in the correlation with saleprice. Finally, the number and quality of bathrooms were found to have varying correlations with saleprice.

## Model Building

The modeling section of this project involves building several linear regression, Lasso, and Ridge models to predict sale prices based on various column features. Model 1 is a linear regression model using column features with a correlation coefficient of 0.5 or greater (or -0.5 or less) with sale price. Model 2 uses column features with a correlation coefficient of 0.25 or greater (or -0.25 or less), while Model 3 uses column features with a correlation coefficient of 0.15 or greater (or -0.15 or less). Model 4 and Model 5 both utilize Lasso and Ridge regression techniques, respectively, using column features with a correlation coefficient of 0.15 or greater (or -0.15 or less). Both scaled and unscaled versions of the Lasso and Ridge models will be built to compare their performance.

-- 

## Evaluation

Here are the scores and metrics for different models: 

|Model Type|Features|Training Score|Test Score|RMSE|
|--|--|--|--|--|
|Model 1 - OLS|Column features that have a correlation coefficient equal to or greater than 0.5/-0.5 with `saleprice`| 0.821|0.895|24241.895|
|Model 2 - OLS|Column features that have a correlation coefficient equal to or greater than 0.25/-0.25 with `saleprice`|0.868|0.904|23088.501|
|Model 3 - OLS|Column features that have a correlation coefficient equal to or greater than 0.15/-0.15 with `saleprice`|0.88|0.914|21917.108|
|Model 4 - Lasso (Unscaled)|Column features that have a correlation coefficient equal to or greater than 0.15/-0.15 with `saleprice`|0.88|0.915|21780.733|
|Model 4 - Lasso (Scaled)|Column features that have a correlation coefficient equal to or greater than 0.15/-0.15 with `saleprice`|0.88|0.914|21858.104|
|Model 5 - Ridge (Unscaled)|Column features that have a correlation coefficient equal to or greater than 0.15/-0.15 with `saleprice`|0.88|0.915|21770.103|
|Model 5 - Ridge (Scaled)|Column features that have a correlation coefficient equal to or greater than 0.15/-0.15 with `saleprice`|0.88|0.912|82248223.907|

The above table shows that Model 3 - OLS with features having correlation coefficient 0.15/-0.15 or greater - performed the best among all the models. This model achieved the highest test score of 0.914, the lowest RMSE of 21917.108, and a good training score of 0.88. The cross-validation score also shows good consistency in performance across folds.

Also, another unique observations include the fact that the Lasso model's performance remained the same with or without scaling, while the performance of the Ridge model degraded significantly with scaling. 

Additionally, the correlation threshold used for feature selection affected model performance, with Model 2 - OLS with features having correlation coefficient 0.25/-0.25 or greater, outperforming Model 1 - OLS with features having correlation coefficient 0.5/-0.5 or greater.

--- 

## Conclusion & Recommendations

Based on our analysis, we have developed a model that can accurately predict home prices in Ames, IA, with an R-squared score of 0.914. Our analysis identified several key factors that have a significant influence on the market value of homes in the area, including the overall quality, the garage area, and the year the house was built. By taking these factors into account, our model can provide local real estate agents with more accurate pricing information, enabling them to offer more informed guidance to their clients.

However, there are still some areas where our model can be improved. For instance, we could try feature engineering by combining garage-related features to create a new interaction column. We could also explore different methods for handling missing values and outliers during data cleaning. Additionally, we recommend testing other models, such as Random Forest, Gradient Boosting, or Neural Networks, to see if they can provide even better performance.

--- 

## Data Dictionary 

Data dictionary is also available on [Kaggle](https://www.kaggle.com/competitions/project-2-regression-challenge-123/data). 

|Feature|Type|Dataset|Description|
|-------|----|-------|-----------|
|Id|int64|
|PID|int64  
|MS SubClass|int64|20 1-STORY 1946 & NEWER ALL STYLES/ 30 1-STORY 1945 & OLDER/ 40 1-STORY W/FINISHED ATTIC ALL AGES/ 45 1-1/2 STORY - UNFINISHED ALL AGES/ 50 1-1/2 STORY FINISHED ALL AGES/ 60 2-STORY 1946 & NEWER/ 70 2-STORY 1945 & OLDER/ 75 2-1/2 STORY ALL AGES/ 80 SPLIT OR MULTI-LEVEL/ 85 SPLIT FOYER/ 90 DUPLEX - ALL STYLES AND AGES / 120 1-STORY PUD (Planned Unit Development) - 1946 & NEWER/ 150 1-1/2 STORY PUD - ALL AGES/ 160 2-STORY PUD - 1946 & NEWER/ 180 PUD - MULTILEVEL - INCL SPLIT LEV/FOYER/ 190 2 FAMILY CONVERSION - ALL STYLES AND AGES|MSSubClass: The building class|
|MS Zoning|object|A Agriculture/ C Commercial/ FV Floating Village Residential/ I * Industrial/ RH Residential High Density/ RL Residential Low Density/ RP Residential Low Density Park/ RM Residential Medium Density|Identifies the general zoning classification of the sale|
|Lot Frontage|float64||Linear feet of street connected to property|
|Lot Area|int64||Lot size in square feet|
|Street|object|Grvl Gravel / Pave Paved|Type of road access to property|
|Alley|object|Grvl Gravel/ Pave Paved/ NA No alley access|Type of alley access to property|
|Lot Shape|object|Reg Regular/ IR1 Slightly irregular/ IR2 Moderately Irregular/ IR3 Irregular|General shape of property|
|Land Contour|object|Lvl Near Flat/Level/ Bnk Banked - Quick and significant rise from street grade to building/ HLS Hillside - Significant slope from side to side/ Low Depression|Flatness of the property|
|Utilities|object|AllPub All public Utilities (E,G,W,& S)/ NoSewr Electricity, Gas, and Water (Septic Tank)/ NoSeWa Electricity and Gas Only/ ELO Electricity only|Type of utilities available|
|Lot Config|object|Inside Inside lot/ Corner Corner lot/ CulDSac Cul-de-sac/ FR2 Frontage on 2 sides of property/ FR3 Frontage on 3 sides of property|Lot configuration|
|Land Slope|object|Gtl Gentle slope/ Mod Moderate Slope/ Sev Severe Slope|Slope of property|
|Neighborhood|object|Blmngtn Bloomington Heights/ Blueste Bluestem/ BrDale Briardale/ BrkSide Brookside/ ClearCr Clear Creek/ CollgCr College Creek/ Crawfor Crawford/ Edwards Edwards/ Gilbert Gilbert/ IDOTRR Iowa DOT and Rail Road/ MeadowV Meadow Village/ Mitchel Mitchell/ Names North Ames/ NoRidge Northridge/ NPkVill Northpark Villa/ NridgHt Northridge Heights/ NWAmes Northwest Ames/ OldTown Old Town/ SWISU South & West of Iowa State University Sawyer Sawyer/ SawyerW Sawyer West/ Somerst Somerset/ StoneBr Stone Brook/ Timber Timberland/ Veenker Veenker|Physical locations within Ames city limits| 
|Condition 1|object|Artery Adjacent to arterial street/ Feedr Adjacent to feeder street/ Norm Normal/ RRNn Within 200' of North-South Railroad/ RRAn Adjacent to North-South Railroad/ PosN Near positive off-site feature--park, greenbelt, etc./ PosA Adjacent to postive off-site feature/ RRNe Within 200' of East-West Railroad/ RRAe Adjacent to East-West Railroad|Proximity to main road or railroad|
|Condition 2|object|Artery Adjacent to arterial street/ Feedr Adjacent to feeder street/ Norm Normal/ RRNn Within 200' of North-South Railroad/ RRAn Adjacent to North-South Railroad/ PosN Near positive off-site feature--park, greenbelt, etc./ PosA Adjacent to postive off-site feature/ RRNe Within 200' of East-West Railroad/ RRAe Adjacent to East-West Railroad|Proximity to main road or railroad (if a second is present)|
|Bldg Type|object|1Fam Single-family Detached/ 2FmCon Two-family Conversion; originally built as one-family / dwelling/ Duplx Duplex/ TwnhsE Townhouse End Unit/ TwnhsI Townhouse Inside Unit|Type of dwelling|
|House Style|object|1Story One story/ 1.5Fin One and one-half story: 2nd level finished/ 1.5Unf One and one-half story: 2nd level unfinished/ 2Story Two story/ 2.5Fin Two and one-half story: 2nd level finished/ 2.5Unf Two and one-half story: 2nd level unfinished/ SFoyer Split Foyer/ SLvl Split Level|Style of dwelling|
|Overall Qual|int64|10 Very Excellent/ 9 Excellent/ 8 Very Good/ 7 Good/ 6 Above Average/ 5 Average/ 4 Below  Average/ 3 Fair/ 2 Poor/ 1 Very Poor|Overall material and finish quality|
|Overall Cond|int64|10 Very Excellent/ 9 Excellent/ 8 Very Good/ 7 Good/ 6 Above Average/ 5 Average/ 4 Below Average/ 3 Fair/ 2 Poor/ 1 Very Poor|Overall condition rating|
|Year Built|int64||Original construction date|
|Year Remod/Add|int64||Remodel date (same as construction date if no remodeling or additions)|
|Roof Style|object|Flat Flat/ Gable Gable/ Gambrel Gabrel (Barn)/ Hip Hip/ Mansard Mansard/ Shed Shed|Type of roof|
|Roof Matl|object|ClyTile Clay or Tile/ CompShg Standard (Composite) Shingle/ Membran Membrane/ Metal Metal/ Roll Roll/ Tar&Grv Gravel & Tar/ WdShake Wood Shakes/ WdShngl Wood Shingles|Roof material|
|Exterior 1st|object|AsbShng Asbestos Shingles/ AsphShn Asphalt Shingles/ BrkComm Brick Common/ BrkFace Brick Face CBlock Cinder Block/ CemntBd Cement Board/ HdBoard Hard Board/ ImStucc Imitation Stucco/ MetalSd Metal Siding/ Other Other/ Plywood Plywood/ PreCast PreCast/ Stone Stone/ Stucco Stucco/ VinylSd Vinyl Siding/ Wd Sdng Wood Siding/ WdShing Wood Shingles|Exterior covering on house|
|Exterior 2nd|object|AsbShng Asbestos Shingles/ AsphShn Asphalt Shingles/ BrkComm Brick Common/ BrkFace Brick Face/ CBlock Cinder Block/ CemntBd Cement Board/ HdBoard Hard Board/ ImStucc Imitation Stucco/ MetalSd Metal Siding/ Other Other/ Plywood Plywood/ PreCast PreCast/ Stone Stone/ Stucco Stucco/ VinylSd Vinyl Siding/ Wd Sdng Wood Siding/ WdShing Wood Shingles|Exterior covering on house (if more than one material)|
|Mas Vnr Type|object|BrkCmn Brick Common/ BrkFace Brick Face/ CBlock Cinder Block/ None None/ Stone Stone|Masonry veneer type|
|Mas Vnr Area|float64||Masonry veneer area in square feet|
|Exter Qual|object|Ex Excellent/ Gd Good/ TA Average/Typical/ Fa Fair/ Po Poor|Exterior material quality|
|Exter Cond|object|Ex Excellent/ Gd Good/ TA Average/Typical/ Fa Fair/ Po Poor|Present condition of the material on the exterior|
|Foundation|object|BrkTil Brick & Tile/ CBlock Cinder Block/ PConc Poured Contrete/ Slab Slab/ Stone Stone/ Wood Wood|Type of foundation|
|Bsmt Qual|object|Ex Excellent (100+ inches)/ Gd Good (90-99 inches)/ TA Typical (80-89 inches)/ Fa Fair (70-79 inches)/ Po Poor (<70 inches)/ NA No Basement|Height of the basement|
|Bsmt Cond|object|Ex Excellent/ Gd Good/ TA Typical - slight dampness allowed/ Fa Fair - dampness or some cracking or settling/ Po Poor - Severe cracking, settling, or wetness/ NA No Basement|General condition of the basement|
|Bsmt Exposure|object|Gd Good Exposure/ Av Average Exposure (split levels or foyers typically score average or above)/ Mn Mimimum Exposure/ No No Exposure/ NA No Basement|Walkout or garden level basement walls|
|BsmtFin Type 1|object|GLQ Good Living Quarters/ ALQ Average Living Quarters/ BLQ Below Average Living Quarters/ Rec Average Rec Room/ LwQ Low Quality/ Unf Unfinshed/ NA No Basement|Quality of basement finished area|
|BsmtFin SF 1|float64||Type 1 finished square feet|
|BsmtFin Type 2|object|GLQ Good Living Quarters/ ALQ Average Living Quarters/ BLQ Below Average Living Quarters/ Rec Average Rec Room/ LwQ Low Quality/ Unf Unfinshed/ NA No Basement|Quality of second finished area (if present)|
|BsmtFin SF 2|float64||Type 2 finished square feet|
|Bsmt Unf SF|float64||Unfinished square feet of basement area|
|Total Bsmt SF|float64||Total square feet of basement area|
|Heating|object|Floor Floor Furnace/ GasA Gas forced warm air furnace/ GasW Gas hot water or steam heat/ Grav Gravity furnace/ OthW Hot water or steam heat other than gas/ Wall Wall furnace|Type of heating|
|Heating QC|object|Ex Excellent/ Gd Good/ TA Average/Typical/ Fa Fair/ Po Poor|Heating quality and condition|
|Central Air|object|N No/ Y Yes|Central air conditioning|
|Electrical|object|SBrkr Standard Circuit Breakers & Romex/ FuseA Fuse Box over 60 AMP and all Romex wiring (Average)/ FuseF 60 AMP Fuse Box and mostly Romex wiring (Fair)/ FuseP 60 AMP Fuse Box and mostly knob & tube wiring (poor)/ Mix Mixed|Electrical system|
|1st Flr SF|int64||First Floor square feet|
|2nd Flr SF|int64||Second floor square feet|
|Low Qual Fin SF|int64||Low quality finished square feet (all floors)|
|Gr Liv Area|int64||Above grade (ground) living area square feet|
|Bsmt Full Bath|float64||Basement full bathrooms|
|Bsmt Half Bath|float64||Basement half bathrooms| 
|Full Bath|int64||Full bathrooms above grade| 
|Half Bath|int64||Half baths above grade|
|Bedroom AbvGr|int64||Number of bedrooms above basement level|
|Kitchen AbvGr|int64||Number of kitchens|
|Kitchen Qual|object|Ex Excellent/ Gd Good/ TA Typical Average/ Fa Fair/ Po Poor|Kitchen quality|
|TotRms AbvGrd|int64||Total rooms above grade (does not include bathrooms)|
|Functional|object|Typ Typical Functionality/ Min1 Minor Deductions 1/ Min2 Minor Deductions 2/ Mod Moderate Deductions/ Maj1 Major Deductions 1/ Maj2 Major Deductions 2/ Sev Severely Damaged/ Sal Salvage only|Home functionality rating|
|Fireplaces|int64||Number of fireplaces|
|FireplaceQu|int64|Ex Excellent - Exceptional Masonry Fireplace/ Gd Good - Masonry Fireplace in main level/ TA Average - Prefabricated Fireplace in main living area or Masonry Fireplace in basement/ Fa Fair - Prefabricated Fireplace in basement/ Po Poor - Ben Franklin Stove/ NA No Fireplace|Fireplace quality|
|Garage Type|object|2Types More than one type of garage/ Attchd Attached to home/ Basment Basement Garage/ BuiltIn Built-In (Garage part of house - typically has room above garage)/ CarPort Car Port/ Detchd Detached from home/ NA No Garage|Garage location|
|Garage Yr Blt|float64||Year garage was built|
|Garage Finish|object|Fin Finished/ RFn Rough Finished/ Unf Unfinished/ NA No Garage|Interior finish of the garage|
|Garage Cars|float64||Size of garage in car capacity|
|Garage Area|float64||Size of garage in square feet|
|Garage Qual|object|Ex Excellent/ Gd Good/ TA Typical, Average/ Fa Fair/ Po Poor/ NA No Garage|Garage quality|
|Garage Cond|object|Ex Excellent/ Gd Good/ TA Typical, Average/ Fa Fair/ Po Poor/ NA No Garage|Garage condition|
|Paved Drive|object|Y Paved/ P Partial Pavement/ N Dirt/Gravel|Paved driveway|
|Wood Deck SF|int64||Wood deck area in square feet|
|Open Porch SF|int64||Open porch area in square feet| 
|Enclosed Porch|int64||Enclosed porch area in square feet|
|3Ssn Porch|int64||Three season porch area in square feet|
|Screen Porch|int64||Screen porch area in square feet|
|Pool Area|int64||Pool area in square feet|
|PoolQC|object|Ex Excellent/ Gd Good/ TA Average/Typical/ Fa Fair/ NA No Pool|Pool quality|
|Fence|object|GdPrv Good Privacy/ MnPrv Minimum Privacy/ GdWo Good Wood/ MnWw Minimum Wood/Wire/ NA No Fence|Fence quality|
|MiscFeature|object|Elev Elevator/ Gar2 2nd Garage (if not described in garage section)/ Othr Other/ Shed Shed (over 100 SF)/ TenC Tennis Court/ NA None|Miscellaneous feature not covered in other categories|
|Misc Val|int64||$Value of miscellaneous feature|
|Mo Sold|int64||Month Sold|
|Yr Sold|int64||Year Sold|
|Sale Type|object|WD Warranty Deed - Conventional/ CWD Warranty Deed - Cash/ VWD Warranty Deed - VA Loan/ New Home just constructed and sold/ COD Court Officer Deed/Estate/ Con Contract 15pct Down payment regular terms/ ConLw Contract Low Down payment and low interest/ ConLI Contract Low Interest/ ConLD Contract Low Down/ Oth Other|Type of sale|
|SalePrice|int64|n/a|the property's sale price in dollars. This is the target variable that you're trying to predict for this challenge|