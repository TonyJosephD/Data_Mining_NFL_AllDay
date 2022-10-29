# Data_Mining_NFL_AllDay

## The Problem:

Sports memorabilia and collectibles have been around for a long time. Much of our society is already familiar with the physical form of these products, but very few understand or even aware of the digital renditions. "Dapper Labs" is a company at the forefront of digital sports collectibles. They have partnered with some of the largest major leagues such as the NBA and NFL to offer officially licensed digital products. Each of their products consists of video highlights known as "Moments" from a particular game/event that can be certifiably owned through block-chain technology known as Non-Fungible Tokens. In addition to owning these digital highlights, a secure secondary marketplace is made available for collectors to buy and sell as they please 24/7. These marketplaces see considerable volume and those discovering the products are often confused and unsure how to navigate them. At the time of this writing the market cap for the "NFL All Day" product is $92,808,113 USD. So what exactly is driving that value? We don't need to justify any reasoning for the product itself, but it would be beneficial to learn what aspects of the product drive value. 

## Question and Objective:
- What Features if any, contribute to higher priced collectibles on the NFL All Day marketplace?
- Objective: Establish a baseline understanding of how value is determined on the marketplace.


## The Impact

The findings from this analysis hope to provide benefit for current and incoming collectors of the NFL All Day product by developing a clearer understanding of price dynamics in the marketplace. The goal is that the understanding will lead to more informed and confident marketplace purchases and sales. The collectible space has always been a high financial risk area largely due in part to an extremely volatile market sentiment. Characteristics the market favors today are subject to be out of favor tomorrow. Therefore, it is important to note that any trends or notable attributes in these findings are at risk of being non-existent at any point in the future.

## The Data
![](https://team-zero.dev/static/ccb55ceb1a14bd1de0c3c25c955e2557/b17f8/13_NFL-ALL-DAY.jpg)

- Data is sourced from Own the Moment which is a third-party platform providing data analytics and additional functionality for products created by Dapper Labs. This data set focuses on the NFL All Day product, which is a collection of digital highlights (referenced as “Moments”) from the National Football League tokenized for fans to collect through buy, sell, and trade.  The data used for this analysis was recorded at 12:18 pm Eastern Time on September 2nd 2022 and consists of every “Moment” available on the official NFL All Day marketplace. Some important features include Player’s Name and Position, Lowest Asking Price, Rarity Tier, and Special attributes such as Rookie or Championship Year.
- You can access the current version of the data set [here](https://www.otmnft.com/nflallday/market/momentsLinks) which is dynamically updated. **Note: As of November 1st 2022, the data from this source is no longer exportable without a subscription, but the data is still displayed through their GUI.


## The Analysis
*All relevant code and outputs displayed below are in relation to points being 
discussed. To view the entire code and all outputs explored, you may visit the jupyter notebook file within this repository.

### To start, I become familiar with the dataset by identifying the most relevant variables and disregarding the irrelevant ones.
```python
#Pre-Processing Looking at first few lines, features, null values, and data types
raw_data.head()

raw_data.columns

raw_data.isnull().sum()

raw_data.dtypes

raw_data['Jersey Number'].head()
#Each row represents a specific collectible ("Moment") which has a 'Circulation Count'. The 'Jersey Number' attribute would be useful in matching a player's jersey number to the number within the Circulation Count. Since this level of Analysis is broader than what the 'Jersey Number' benefits, I will remove that column**

raw_data.drop('Jersey Number', axis=1, inplace=True)
raw_data.columns

raw_data.describe()
```
### Dropping Irrelevant Columns

* *Observing the features in the describe table, I notice some columns that can be dropped along with others not included.
They are as follows:**

    -'User Owns Count' simply tracks a users collection and is not relevant to the market.
    -'4h', '24h', and '7d' are tracking the price movement over time. Since I am exploring price variation in respect to 
    other  Moments available and not in relation to time, I will also remove these columns. 
    - I will however keep the volume columns as these may be used as additional characteristics.
    - 'Set ID' will be removed since the "Set" characteristic is already accounted for in the 'Set' column
    - 'Play ID' and 'Link' are references and provide no additional measure of analysis, so they will be dropped
    - 'Favorite' is another collection tool specific to individual users and not relevant to the market.
```python    
raw_data.drop(['User Owns Count','4h','24h','7d','Set ID','Play ID','Link','Favorite'], axis=1, inplace=True)
raw_data.columns
```
### Next, I modify some key features that will help in the analysis. But first I noticed the minimum Low Ask was $0 which I address as well.

## Altering Badge Columns
**'All Day Debut' 'Rookie Mint' 'Rookie Year' and 'Championship Year' are all special attributes that indicate if a particular moment has the respective badge. In order to perform a price regression analysis, I will replace their boolean data type columns with dummy columns.**
```python
raw_data['All Day Debut'] = raw_data['All Day Debut'].apply(lambda x: 1 if x==True else 0)
raw_data['Rookie Mint'] = raw_data['Rookie Mint'].apply(lambda x: 1 if x==True else 0)
raw_data['Rookie Year'] = raw_data['Rookie Year'].apply(lambda x: 1 if x==True else 0)
raw_data['Championship Year'] = raw_data['Championship Year'].apply(lambda x: 1 if x==True else 0)
raw_data[['All Day Debut', 'Rookie Mint', 'Rookie Year', 'Championship Year']]

raw_data[raw_data['Low Ask']==0.0]
```
![image](https://user-images.githubusercontent.com/93283651/198840003-7efc6a4d-f4ee-422b-b56a-93a4ad1669d3.png)
**There are two moments in the dataset with a low ask of 0.0. There are also no listings for those moments in the marketplace which means they are not actually available and can be thrown out for this analysis.**
```python
raw_data = raw_data.drop([0,1])
raw_data
```
### Now that I have a decent understanding of the data, I want to start finding relations to price among the other variables. I do this by separating categorical variables from numerical, and looping through them all to create quick charts. There was only one that caught my attention which is shown after the code below along with a correlation matrix that also provides some valuable insight.
```python
cat_data = raw_data[['Player Name','Set','Tier','Series','Play','Team','Week','Position','Edition State']]
num_data = raw_data[['Season','Circulation Count','Owned','In Packs','Minted','Burned','Low Ask','Volume 24h','Volume 7d','Listings','All Day Debut','Rookie Mint','Rookie Year','Championship Year']]

#Looking at distribution of median 'Low Ask' compared to all other variables
for x in cat_data.columns:
    num_data['Low Ask'].groupby(cat_data[x]).mean().plot(kind='bar')
    plt.title(x)
    plt.show()
    
for x in num_data.columns:
    num_data['Low Ask'].groupby(num_data[x]).mean().plot(kind='bar')
    plt.title(x)
    plt.show()
    
#Now observing correlations between numericals
corr = num_data.corr()
sns.heatmap(corr)
```
![tier_correlation](https://user-images.githubusercontent.com/93283651/198838573-2ecd678a-80d5-4105-a057-0690090e71cc.png)

The bar graph above shows the low asking price of each rarity Tier and clearly demonstrates distinctive price brackets.
 It is overwhelmingly apparent that Tier is a driving factor for price. We found our first feature contributing to higher priced moments!

![heatmap](https://user-images.githubusercontent.com/93283651/198838549-75598f12-83d1-4113-8597-cb2555be20f9.png)

The correlation heat map shows there are a few highly correlated numerical variables such as 'Volume 24h' with 'Volume 7d', 'Circulation Count' with 'Owned' and 'In Packs', as well as the badges 'Rookie Mint' with 'Rookie year'. 
This leads to a hypothesis some badges are more meaningful than others. There are more moments with a 'Rookie Year' badge then there are with a Rookie Mint so the rest of the Analysis will only use 'Rookie Year'.

### Since there is now an understanding that Tier is an indicator of price, I will run more visualizations across all variables and observe those attributes in each Tier. I overlay Low Asking prices since the objective is understanding market pricing. The image shown provided the most guidance in moving forward with the analysis
```python
%matplotlib inline
for row in raw_data.columns:
    raw_data.plot.scatter(x=row, y='Tier', c='Low Ask', colormap='viridis')
```
![circulation](https://user-images.githubusercontent.com/93283651/198838684-9008992e-9155-47f5-af77-f553297047a2.png)

- Observing the relationship between Tier, Circulation Count, and Price there is more confirmation that Tier has an impact on price.
- However, Circulation count is directly correlated with Tier and each Tier has multiple Circulation Counts.
- Therefore Circulation Count is a better indicator of Price than Tier alone!

### Now I choose the optimal subset to find more insights by filtering to the largest circulation count. Once I have that subset, I want to explore my hypothesis about badges and see if any of them have a larger impact on price than others. However, I am also curious if a players position could impact price as well. Below I show the distribution of prices by position by accounting for a moment having the 'Rookie Year' badge.
```python
raw_data.groupby('Circulation Count').size()

#Narrowing moments to 10,000 circulation count since it contains the largest sample size
high_circulation_data = raw_data[raw_data['Circulation Count']==10000]
high_circulation_data

#Checking to see if a players position and/or Badge type makes an impact on the price
high_circulation_data.plot.scatter(x='Position',y='Low Ask', c='Rookie Year', colormap='coolwarm')
```
![rookie_badge](https://user-images.githubusercontent.com/93283651/198838758-d9f7004c-8f8e-470d-bfe3-add26019c97a.png)
- There are some positions that appear more valuable than others such as QB, WR, and TE. Defensive positions tend to have a lower price ceiling.
- But wait, why is there a moment priced so much higher than the rest of them? Let's Look below.
```python
raw_data[(raw_data['Circulation Count']==10000) & (raw_data['Low Ask']>200)]
```
![image](https://user-images.githubusercontent.com/93283651/198840417-958a3927-5ee9-4b9a-80bd-de80519269b5.png)

- Oh, the outlier that was priced well above the others is a Tom Brady Moment! That makes sense, he is the GOAT.

### Let's finish up by looking at the last two badges.
```python
high_circulation_data.plot.scatter(x='Position',y='Low Ask', c='All Day Debut', colormap='coolwarm')
high_circulation_data.plot.scatter(x='Position',y='Low Ask', c='Championship Year', colormap='coolwarm')
```
![alldaydebut](https://user-images.githubusercontent.com/93283651/198838832-e50c7327-c447-4032-bcce-0b733c2c0563.png)
![champ_badge](https://user-images.githubusercontent.com/93283651/198838837-7c84234e-dfa0-445d-9c63-24447de78480.png)

Looking at the charts above we can see for every position, the highest priced moments are those that have the badge 'All Day Debut'.
I can conclude that any moment with the 'All Day Debut' badge carriers a premium in the market. The Championship Year Badge either does not carry a premium, or there is not a large enough sample size to make a conclusion yet

## Conclusion

The data exploration and charts provided in this analysis have helped to identify key attributes that contribute to higher priced moments on the NFL All Day marketplace. The attributes identified are:

-Circulation Count
-Position
-All Day Debut Badge

During the process of identifying these features, there has been a deeper understanding of what is valuable on the marketplace. Rarity Tier was initially found to be a contributor of higher prices, but with further examination it was found that Rarity Tier is determined by Circulation Count and contains various circulation counts. Therefore it is expected that large price variations occur even within the same tier of moments.

This analysis also shows that there are multiple attributes associated with each listing on the marketplace, and value is associated mostly with popular attributes among market participants. The position attribute clearly demonstrates this with offensive positions having higher price ceilings than defensive positions. Additionally, a moment does not increase in price value simply by having a "Badge". The "All Day Debut" badge appears to be more popular among market participants and carries a higher premium than the others.

This should provide a baseline understanding of market pricing for new and current market participants. None of the information presented should be taken as financial advice. Further investigation should be done in regards to more complex features such as moments available per player, amount available compared to circulation count, and statistics related to player performances.
