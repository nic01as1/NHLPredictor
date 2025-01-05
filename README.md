# NHLPredictor
Predicting NHL Game Outcome

The Goal of this project is to predict NHL game outcome and having a project that has an edge over betting sites which Highly depends on human sentiment which is highly biased. 

There is alot of optimisation to be made to the project like : Adjusting the model for taking the odds in consideration , Adjusting the weight of each value / adding / removing values, Adding an automatic scraper , Adding a friendlier interface.

Project : The Project starts by extracting datas from an excel file which is an extraction from a site with all the historic games from the season 24-25. I then add some datas in excel (Which could be done in python i nthe futur)
From there,  find the winrate of all the NHL teams at home. This winrrate is higher than away which is normal for few factors like : Travel, Crowd and Familarity
In our case the wirrate at home is : 53.6% 

I will try to beat this with out model and I will run it for the full year making obvious adjustement during the year. (Even tho it's not the best for testing) 

After that , I find the F1 which will be a usefull mesure to see the quality of my model. I'll try to tweak it to be the highest possible without adding biais like overfitting. 

Assuming the home team will always win, my F1 is 0.37.
