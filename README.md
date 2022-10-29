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
