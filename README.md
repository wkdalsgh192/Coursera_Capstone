# 1. Introduction

## 1-1. Topic Description & Background

Seoul, where over 10 millions people live, is one of the most dynamic cities in the world. Every morning, seeing tens of thousands of people taking a visit to cafes to take some caffein is a common scene in Seoul.  According to a report, an average korean adult drank more than 300 cups of coffee per year as of 2018 and the size of the coffee industry in Korea grew to over 5 billion dollars. Due to this trend, opening a cafe has become one of the most common businesses such that over 14,000 coffee shops newly open a year but one out of ten coffee stores often fails to make a profit along side with the rapid expansion.
Assum that there is one guy, who will be retired sooner or later, thinking of running his cafe somewhere in Seoul to accquire an income source in the near future. What advices can we give to him as an data scientist? For this, Arthur Rubinfeld, a former Starbucks Chief Creative Officer, ever mentioned what's important in starting the business.

> *"The first store introduces your brand to the world, and you get only one chance to make a first impression. The store establishes the level of quality and customer service that customers will come to expect. It establishes the points of differentiation between you and competitors. Too many people treat their first store almost as an orphan, thinking that a "good enough" site is in fact good enough to get started. You should be displined in your search for the best location to open your first store."*

It is selecting a good location that should be considered as the very first and key step in starting at the very beginning. Therefore, in this capstone, we will be exploring to find a location good enough to start an cafe business for individuals.

## 1-2. Data Description

Three types of data will be used in estimating the best location: **1. Foursquare API 2. Population data 3. Starbucks stats.**

1. To map all the administrative districts of Seoul and figure out how many cafes are in each district, relevant location data is necessary so I brought them up through Foursquare API .
2. Demograpics of Seoul is one of the most important and easily usuable factors to be featured in the analysis. In details, population of each borough and foot traffic will be mainly used to segment and specify a set of boroughs in which the cafe business is likely less competitive and more successful. I got the data from the Seoul Open Data Plaza  ('SODP'), which is an website where lots of public data are able to be used freely. 
3. Since the given information is not sufficient to nicely suggest the best location in business level, I assume as a complimentary information that each Starbucks store would be located at the best, which means there the most profitable place, and scrapped the data about the number of stores in district level.

# 2. Methodology

## 2-1 Boroughs of Seoul

To analyze multiple datasets, I attempted to figure out the number of stores and facilities in a limited distance, which is set as 2 kilometers here - from the center of each district. For starters, I loaded the public data including the latitude and longitude of each district of Seoul from the SODP. Seoul consists of 25 administrative districts, which have its own legislative council, representative and administrative government. 

![img1](https://user-images.githubusercontent.com/50606172/101274731-d379a280-37e3-11eb-9f4b-67cb5fb10511.png)

Next, I tried to call as many venues as possible within the radius of 2km using the Foursquare API. For the maxinum number of calls was set as 100 calls, 2,136 venues from 194 catetories were loaded in total. It means that most of districts got the maximum calls but some did not. out of 25, 9 districts less than 100 places was called in 9 out of 25 districts, of which three  - Dobong-gu, Geumcheon-gu and Guro-gu - failed to reach the half. Based on the data, it comes natually to assum that an average population of these disctircts may be less than that of the rest. To examine the hypothesis, I plotted a choropleth map with the demographic data showing how many people live in eact district . However, there was no coefficient  between the number of venues and population. I also tried to relate it to the foot traffic, which was no meaningful connection to the population data as well. Thus, two different kinds of data - categories and population - were created independently in this model.

## 2-2 Clustering

Each venue has its own category relying on what it sells or provides. For example, stores selling cocktails or whisky is labeled as 'bar' and restaurants serving Japanese cuisines as 'japanese restaurant'. So, now we have 187 categories in total but need to get rid of or merge some of them becasue they were clearly alike such that the similarity might give a bias to our model. Cafe and coffee shop in the list of categories are technically same places and it is reasonable to put them into one group. Otherwise, our model might be built on inaccurate results. As reclassifying all the categories was not easy and, what's even worse, it was so boring to manually type and double-check the spells (even though they were bounded as dictionaries), I was almost about to give up but finally decreased by 138 categories. The whole venues were encoded in one-hot vectors and I put them in our K-means clutering model as input.  After a few seconds, the model segmented all the boroughs into two groups as set before, the result was visualized below.   

![img2](https://user-images.githubusercontent.com/50606172/101274748-f0ae7100-37e3-11eb-88ee-bb7e37ff2b57.png)

Combined with the choropleth map I made earlier, it shows interesting result that although we never mentioned any other features other than venues, our model clustered the most boroughs with high foot traffic as one group, and the rest as the other. Taking a closer look at the two clusters, cafe is at the first top in almost every regions (given the amount of coffee Koreans consume annually, It is not surprising that cafe prevails all the other), they gets distinctive from the second top common venues. The model seems to take restaurants and bakery into account as a pair of cafe to draw a line. in the below charts, korean and some other restaurants are more frequent in the first cluster whereas bakery or dessert shops are in the second. From the result, I could get an insight that a newly opening cafe has to be located in the cluster 1 area.

![img3](https://user-images.githubusercontent.com/50606172/101274749-f4da8e80-37e3-11eb-8db6-8e1dcd3fa87e.png)
(Top 10 venues of the second cluster boroughs)

![img4]()

(Top 10 venues of the first cluster boroughs) 

## 2-3 How Starbucks do

![img5]()

Number of Starbucks store located in Seoul

Starbucks stores are distributed in Seoul with the similar pattern of the result. The numbers of the circles marked on the above map means the number of Starbucks located within 10 kilometers (≒ 6miles) from the pin point. Most of them are inside the boundary of the Cluster 2 where drinks are the main selling items rather than foods. Other famous coffee brands are not so much different. It is not a coincidence because according to what Arthur said, higher foot traffic normally can lead to higher sales. And what you need to do is to open your store in the middle of the busiest downtown area, attract folks walking nearby, and expand the business gradually. As you might already know, this is well known as the 'hub-and-spoke' strategy of Starbucks.

Then, what is the meaning of all the things we've done, racking our brains as to choose the best location? If we start selling coffee next to one of the Starbucks stores, are we all good? Well, may be or may be not. It totally depends on what kinds of coffee shops you want to run.
If it is a kind of franchise cafes, running it on the street close to other big coffee stores would be better because easy accessibility and the brand's systematic marketing can guarantee the minimum sales profit. In return for its advisory role, instead,  you need to pay not simply high rent and a startup fee but plus an ongoing percentage of gross revenues to the franchisor, which are the main reasons that many franchisees go out of business. Opening a charicteristic cafe in a bedtown area is a good alternative for some who cannot afford them but are willing to invest all of you into building your own identity. You don't have to struggle with the burdens that you have to sell as many drinks as possible so as to earn the net income after paying all the bills your franchise or building owner requested to pay back every month. You might be in the red at first, but if you could make a clear difference between your menus and other franchised medicores, getting money is only a matter of time.

# 3. Result

   To sum up, Seoul can be segmented with two groups in choosing a perfect location to show off your coffee brand; one is competitive building forests and the other less competitive bed towns. The former has more cafes and dessert shops than the latter due to the profitability and accessbility. If someone opens a new cafe in the commercial area, he might expect to get decent income but enduring a hard competition while paying all the fees with a high monthly rent is burdensome such that branded ones are expected to fit better. Meanwhile, we have shown that although running a cafe in the residential area might not give him as much money as he would expect in the downtown area, less severe competition would provide more opportunity to build your own identity as a local cafe and developing signiture menues that folks can taste ONLY in your cafe will be more likely profitable in the long term.  Thus, our works suggest that location of a store is dependant on the type of cafes individuals want to build so that anyone who are thinking about running a cafe in the future should ask themselves first. 'For what do I want to run my coffee business' Location comes from the answer. 

# 4. Discussion

Ahead of clustering, I attempted the elbow method with distortion and inertia respectively, a common way to find the optimal value of K. In the two graphs below, when K increases by 2, both decrease from 0.45 nearly to 0.35, shown as the biggest reduction in any value of K. However, the lines keep going down in small step until K converge towards the number of boroughs and additional features would be necessary to choose the optimum more clearly.  

![img6]()
