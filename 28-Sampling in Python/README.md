# Sampling in Python

## I. Introduction to Sampling

#### Sampling and point estimates
What is sampling and why it might be useful?

2. Estimating the population of France
Let's consider the problem of counting how many people live in France. The standard approach is to take a census. This means contacting every household and asking how many people live there.

3. There are lots of people in France
Since there are millions of people in France, this is a really expensive process. Even with modern data collection technology, most countries will only conduct a census every five or ten years due to the cost.

4. Sampling households
In 1786, Pierre-Simon Laplace realized you could estimate the population with less effort. Rather than asking every household who lived there, he asked a small number of households and used statistics to estimate the number of people in the whole population. This technique of working with a subset of the whole population is called sampling.

5. Population vs. sample
Two definitions are important for this course.
- The population is the complete set of data that we are interested in. The previous example involved the literal population of France, but in statistics, it doesn't have to refer to people. One thing to bear in mind is that there is usually no equivalent of the census, so typically, we won't know what the whole population is like - more on this in a moment.
- The sample is the subset of data that we are working with.

7. Coffee rating dataset
Here's a dataset of professional ratings of coffees. Each row corresponds to one coffee, and there are thirteen hundred and thirty-eight rows in the dataset. The coffee is given a score from zero to one hundred, which is stored in the total_cup_points column. Other columns contain contextual information like the variety and country of origin and scores between zero and ten for attributes of the coffee such as aroma and body. These scores are averaged across all the reviewers for that particular coffee. It doesn't contain every coffee in the world, so we don't know exactly what the population of coffees is. However, there are enough here that we can think of it as our population of interest.

8. Points vs. flavor: population
Let's consider the relationship between cup points and flavor by selecting those two columns. This dataset contains all thirteen hundred and thirty-eight rows from the original dataset.

9. Points vs. flavor: 10 row sample
The pandas dot-sample method returns a random subset of rows. Setting n to ten means ten random rows are returned. By default, rows from the original dataset can't appear in the sample dataset multiple times, so we are guaranteed to have ten unique rows in our sample.

10. Python sampling for Series
The dot-sample method also works on pandas Series. Here, using square-bracket subsetting retrieves the total_cup_points column as a Series, and the n argument specifies how many random values to return.

11. Population parameters & point estimates
A population parameter is a calculation made on the population dataset. We aren't limited to counting values either; here, we calculate the mean of the cup points using NumPy. By contrast, a point estimate, or sample statistic, is a calculation based on the sample dataset. Here, the mean of the total cup points is calculated on the sample. Notice that the means are very similar but not identical.

12. Point estimates with pandas
Working with pandas can be easier than working with NumPy. These mean calculations can be performed using the dot-mean pandas method.

#### Convenience sampling
1. Convenience sampling
The point estimates you calculated in the previous exercises were very close to the population parameters that they were based on, but is this always the case?

2. The Literary Digest election prediction
In 1936, a newspaper called The Literary Digest ran an extensive poll to try to predict the next US presidential election. They phoned ten million voters and had over two million responses. About one-point-three million people said they would vote for Landon, and just under one million people said they would vote for Roosevelt. That is, Landon was predicted to get fifty-seven percent of the vote, and Roosevelt was predicted to get forty-three percent of the vote. Since the sample size was so large, it was presumed that this poll would be very accurate. However, in the election, Roosevelt won by a landslide with sixty-two percent of the vote. So what went wrong? Well, in 1936, telephones were a luxury, so the only people who had been contacted by The Literary Digest were relatively rich. The sample of voters was not representative of the whole population of voters, and so the poll suffered from sample bias. The data was collected by the easiest method, in this case, telephoning people. This is called convenience sampling and is often prone to sample bias. Before sampling, we need to think about our data collection process to avoid biased results.

3. Finding the mean age of French people
Let's look at another example. While on vacation at Disneyland Paris, you start wondering about the mean age of French people. To get an answer, you ask ten people stood nearby about their ages. Their mean age is twenty-four-point-six years old. Do you think this will be a good estimate of the mean age of all French citizens?
1 Image by Sean MacEntee

5. How accurate was the survey?
On the left, you can see mean ages taken from the French census. Notice that the population has been gradually getting older as birth rates decrease and life expectancy increases. In 2015, the mean age was over forty, so our estimate of twenty-four-point-six is way off. The problem is that the family-friendly fun at Disneyland means that the sample ages weren't representative of the general population. There are generally more eight-year-olds than eighty-year-olds riding rollercoasters.

6. Convenience sampling coffee ratings
Let's return to the coffee ratings dataset and look at the mean cup points population parameter. The mean is about eighty-two. One form of convenience sampling would be to take the first ten rows, rather than the random rows we saw in the previous video. We can take the first 10 rows with the pandas head method. The mean cup points from this sample is higher at eighty-nine. The discrepancy suggests that coffees with higher cup points appear near the start of the dataset. Again, the convenience sample isn't representative of the whole population.

7. Visualizing selection bias
Histograms are a great way to visualize the selection bias. We can create a histogram of the total cup points from the population, which contains values ranging from around 59 to around 91. The numpy-dot-arange function can be used to create bins of width 2 from 59 to 91. Recall that the stop value in numpy-dot-arange is exclusive, so we specify 93, not 91. Here's the same code to generate a histogram for the convenience sample.

8. Distribution of a population and of a convenience sample
Comparing the two histograms, it is clear that the distribution of the sample is not the same as the population: all of the sample values are on the right-hand side of the plot.

9. Visualizing selection bias for a random sample
This time, we'll compare the total_cup_points distribution of the population with a random sample of 10 coffees.

10. Distribution of a population and of a simple random sample
Notice how the shape of the distributions is more closely aligned when random sampling is used.


#### Pseudo-random number generation
1. Pseudo-random number generation
You previously saw how to use a random sample to get results similar to those in the population. But how does a computer actually do this random sampling?

2. What does random mean?
There are several meanings of random in English. This definition from Oxford Languages is the most interesting for us. If we want to choose data points at random from a population, we shouldn't be able to predict which data points would be selected ahead of time in some systematic way.
1 Oxford Languages

4. True random numbers
To generate truly random numbers, we typically have to use a physical process like flipping coins or rolling dice. The Hotbits service generates numbers from radioactive decay, and RANDOM-dot-ORG generates numbers from atmospheric noise, which are radio signals generated by lightning. Unfortunately, these processes are fairly slow and expensive for generating random numbers.
1 https://www.fourmilab.ch/hotbits
2 https://www.random.org

4. Pseudo-random number generation
For most use cases, pseudo-random number generation is better since it is cheap and fast. Pseudo-random means that although each value appears to be random, it is actually calculated from the previous random number. Since you have to start the calculations somewhere, the first random number is calculated from what is known as a seed value. The word random is in quotes to emphasize that this process isn't really random. If we start from a particular seed value, all future numbers will be the same.

5. Pseudo-random number generation example
For example, suppose we have a function to generate pseudo-random values called calc_next_random. To begin, we pick a seed number, in this case, one. calc_next_random does some calculations and returns three. We then feed three into calc_next_random, and it does the same set of calculations and returns two. And if we can keep feeding in the last number, it will return something apparently random. Although the process is deterministic, the trick to a random number generator is to make it look like the values are random.

6. Random number generating functions
NumPy has many functions for generating random numbers from statistical distributions. To use each of these, make sure to prepend each function name with numpy-dot-random or np-dot-random. Some of them, like dot-uniform and dot-normal, may be familiar. Others have more niche applications.

7. Visualizing random numbers
Let's generate some pseudo-random numbers. The first arguments to each random number function specify distribution parameters. The size argument specifies how many numbers to generate, in this case, five thousand. We've chosen the beta distribution, and its parameters are named a and b. These random numbers come from a continuous distribution, so a great way to visualize them is with a histogram. Here, because the numbers were generated from the beta distribution, all the values are between zero and one.

8. Random numbers seeds
To set a random seed with NumPy, we use the dot-random-dot-seed method. random-dot-seed takes an integer for the seed number, which can be any number you like. dot-normal generates pseudo-random numbers from the normal distribution. The loc and scale arguments set the mean and standard deviation of the distribution, and the size argument determines how many random numbers from that distribution will be returned. If we call dot-normal a second time, we get two different random numbers. If we reset the seed by calling random-dot-seed with the same seed number, then call dot-normal again, we get the same numbers as before. This makes our code reproducible.

9. Using a different seed
Now let's try a different seed. This time, calling dot-normal generates different numbers.


## II. Sampling Methods

#### Simple random and systematic sampling
1. Simple random and systematic sampling
There are several methods of sampling from a population. In this video, we'll look at simple random sampling and systematic random sampling.

2. Simple random sampling
Simple random sampling works like a raffle or lottery. We start with our population of raffle tickets or lottery balls and randomly pick them out one at a time.

3. Simple random sampling of coffees
In our coffee ratings dataset, instead of raffle tickets or lottery balls, the population consists of coffee varieties. To perform simple random sampling, we take some at random, one at a time. Each coffee has the same chance as any other of being picked. When using this technique, sometimes we might end up with two coffees that were next to each other in the dataset, and sometimes we might end up with large areas of the dataset that were not selected from at all.

4. Simple random sampling with pandas
We've already seen how to do simple random sampling with pandas. We call dot-sample and set n to the size of the sample. We can also set the seed using the random_state argument to generate reproducible results, just like we did pseudo-random number generation. Previously, by not setting random_state when sampling, our code would generate a different random sample each time it was run.

5. Systematic sampling
Another sampling method is known as systematic sampling. This samples the population at regular intervals. Here, looking from top to bottom and left to right within each row, every fifth coffee is sampled.

6. Systematic sampling - defining the interval
Systematic sampling with pandas is slightly trickier than simple random sampling. The tricky part is determining how big the interval between each row should be for a given sample size. Suppose we want a sample size of five coffees. The population size is the number of rows in the whole dataset, and in this case, it's one thousand three hundred and thirty-eight. The interval is the population size divided by the sample size, but because we want the answer to be an integer, we perform integer division with two forward slashes. This is like standard division but discards any fractional part. One-three-three-eight divided by five is actually two hundred and sixty-seven-point-six, and discarding the fractional part leaves two hundred and sixty-seven. Thus, to get a systematic sample of five coffees, we will select every two hundred sixty-seventh coffee in the dataset.

7. Systematic sampling - selecting the rows
To select every two hundred and sixty-seventh row, we call dot-iloc on coffee_ratings and pass double-colons and the interval, which is 267 in this case. Double-colon interval tells pandas to select every two hundred and sixty-seventh row from zero to the end of the DataFrame.

8. The trouble with systematic sampling
There is a problem with systematic sampling, though. Suppose we are interested in statistics about the aftertaste attribute of the coffees. To examine this, first, we use reset_index to create a column of index values in our DataFrame that we can plot. Plotting aftertaste against index shows a pattern. Earlier rows generally have higher aftertaste scores than later rows. This introduces bias into the statistics that we calculate. In general, it is only safe to use systematic sampling if a plot like this has no pattern; that is, it just looks like noise.

9. Making systematic sampling safe
To ensure that systematic sampling is safe, we can randomize the row order before sampling. dot-sample has an argument named frac that lets us specify the proportion of the dataset to return in the sample, rather than the absolute number of rows that n specifies. Setting frac to one randomly samples the whole dataset. In effect, this randomly shuffles the rows. Next, the indices need to be reset so that they go in order from zero again. Specifying drop equals True clears the previous row indexes, and chaining to another reset_index call creates a column containing these new indexes. Redrawing the plot with the shuffled dataset shows no pattern between aftertaste and index. This is great, but note that once we've shuffled the rows, systematic sampling is essentially the same as simple random sampling.

#### Stratified and weighted random sampling
1. Stratified and weighted random sampling
Stratified sampling is a technique that allows us to sample a population that contains subgroups.

2. Coffees by country
For example, we could group the coffee ratings by country. If we count the number of coffees by country using the value_counts method, we can see that these six countries have the most data.
1 The dataset lists Hawaii and Taiwan as countries for convenience, as they are notable coffee-growing regions.

3. Filtering for 6 countries
To make it easier to think about sampling subgroups, let's limit our analysis to these six countries. We can use the dot-isin method to filter the population and only return the rows corresponding to these six countries. This filtered dataset is stored as coffee_ratings_top.

4. Counts of a simple random sample
Let's take a ten percent simple random sample of the dataset using dot-sample with frac set to zero-point-one. We also set the random_state argument to ensure reproducibility. As with the whole dataset, we can look at the counts for each country. To make comparisons easier, we set normalize to True to convert the counts into a proportion, which shows what proportion of coffees in the sample came from each country.

5. Comparing proportions
Here are the proportions for the population and the ten percent sample side by side. Just by chance, in this sample, Taiwanese coffees form a disproportionately low percentage. The different makeup of the sample compared to the population could be a problem if we want to analyze the country of origin, for example.

6. Proportional stratified sampling
If we care about the proportions of each country in the sample closely matching those in the population, then we can group the data by country before taking the simple random sample. Note that we used the Python line continuation backslash here, which can be useful for breaking up longer chains of pandas code like this. Calling the dot-sample method after grouping takes a simple random sample within each country. Now the proportions of each country in the stratified sample are much closer to those in the population.

7. Equal counts stratified sampling
One variation of stratified sampling is to sample equal counts from each group, rather than an equal proportion. The code only has one change from before. This time, we use the n argument in dot-sample instead of frac to extract fifteen randomly-selected rows from each country. Here, the resulting sample has equal proportions of one-sixth from each country.

8. Weighted random sampling
A close relative of stratified sampling that provides even more flexibility is weighted random sampling. In this variant, we create a column of weights that adjust the relative probability of sampling each row. For example, suppose we thought that it was important to have a higher proportion of Taiwanese coffees in the sample than in the population. We create a condition, in this case, rows where the country of origin is Taiwan. Using the where function from NumPy, we can set a weight of two for rows that match the condition and a weight of one for rows that don't match the condition. This means when each row is randomly sampled, Taiwanese coffees have two times the chance of being picked compared to other coffees. When we call dot-sample, we pass the column of weights to the weights argument.

9. Weighted random sampling results
Here, we can see that Taiwan now contains seventeen percent of the sampled dataset, compared to eight-point-five percent in the population. This sort of weighted sampling is common in political polling, where we need to correct for under- or over-representation of demographic groups.


#### Cluster sampling
1. Cluster sampling
One problem with stratified sampling is that we need to collect data from every subgroup. In cases where collecting data is expensive, for example, when we have to physically travel to a location to collect it, it can make our analysis prohibitively expensive. There's a cheaper alternative called cluster sampling.

2. Stratified sampling vs. cluster sampling
The stratified sampling approach was to split the population into subgroups, then use simple random sampling on each of them. Cluster sampling means that we limit the number of subgroups in the analysis by picking a few of them with simple random sampling. We then perform simple random sampling on each subgroup as before.

3. Varieties of coffee
Let's return to the coffee dataset and look at the varieties of coffee. In this image, each bean represents the whole subgroup rather than an individual coffee, and there are twenty-eight of them. To extract unique varieties, we use the dot-unique method. This returns an array, so wrapping it in the list function creates a list of unique varieties. Let's suppose that it's expensive to work with all of the different varieties. Enter cluster sampling.

4. Stage 1: sampling for subgroups
The first stage of cluster sampling is to randomly cut down the number of varieties, and we do this by randomly selecting them. Here, we've used the random-dot-sample function from the random package to get three varieties, specified using the argument k.

5. Stage 2: sampling each group
The second stage of cluster sampling is to perform simple random sampling on each of the three varieties we randomly selected. We first filter the dataset for rows where the variety is one of the three selected, using the dot-isin method. To ensure that the isin filtering removes levels with zero rows, we apply the cat-dot-remove_unused_categories method on the Series of focus, which is variety here. If we exclude this method, we might receive an error when sampling by variety level. The pandas code is the same as for stratified sampling. Here, we've opted for equal counts sampling, with five rows from each remaining variety.

6. Stage 2 output
Here's the first few columns of the result. Notice that there are the fifteen rows, which we'd expect from sampling five rows from three varieties.

7. Multistage sampling
Note that we had two stages in the cluster sampling. We randomly sampled the subgroups to include, then we randomly sampled rows from those subgroups. Cluster sampling is a special case of multistage sampling. It's possible to use more than two stages. A common example is national surveys, which can include several levels of administrative regions, like states, counties, cities, and neighborhoods.


#### Comparing sampling methods
1. Comparing sampling methods
Let's review the various sampling techniques we learned about.

2. Review of sampling techniques - setup
For convenience, we'll stick to the six countries with the most coffee varieties that we used before. This corresponds to eight hundred and eighty rows and eight columns, retrieved using the dot-shape attribute.

3. Review of simple random sampling
Simple random sampling uses dot-sample with either n or frac set to determine how many rows to pseudo-randomly choose, given a seed value set with random_state. The simple random sample returns two hundred and ninety-three rows because we specified frac as one-third, and one-third of eight hundred and eighty is just over two hundred and ninety-three.

4. Review of stratified sampling
Stratified sampling groups by the country subgroup before performing simple random sampling on each subgroup. Given each of these top countries have quite a few rows, stratifying produces the same number of rows as the simple random sample.

5. Review of cluster sampling
In the cluster sample, we've used two out of six countries to roughly mimic frac equals one-third from the other sample types. Setting n equal to one-sixth of the total number of rows gives roughly equal sample sizes in each of the two subgroups. Using dot-shape again, we see that this cluster sample has close to the same number of rows: two-hundred-ninety-two compared to two-hundred-ninety-three for the other sample types.

6. Calculating mean cup points
Let's calculate a population parameter, the mean of the total cup points. When the population parameter is the mean of a field, it's often called the population mean. Remember that in real-life scenarios, we typically wouldn't know what the population mean is. Since we have it here, though, we can use this value of eighty-one-point-nine as a gold standard to measure against. Now we'll calculate the same value using each of the sampling techniques we've discussed. These are point estimates of the mean, often called sample means. The simple and stratified sample means are really close to the population mean. Cluster sampling isn't quite as close, but that's typical. Cluster sampling is designed to give us an answer that's almost as good while using less data.

7. Mean cup points by country: simple random
Here's a slightly more complicated calculation of the mean total cup points for each country. We group by country before calculating the mean to return six numbers. So how do the numbers from the simple random sample compare? The sample means are pretty close to the population means.

8. Mean cup points by country: stratified
The same is true of the sample means from the stratified technique. Each sample mean is relatively close to the population mean.

9. Mean cup points by country: cluster
With cluster sampling, while the sample means are pretty close to the population means, the obvious limitation is that we only get values for the two countries that were included in the sample. If the mean cup points for each country is an important metric in our analysis, cluster sampling would be a bad idea.


## III. Sampling Distributions

#### Relative error of point estimates
1. Relative error of point estimates
Let's see how the size of the sample affects the accuracy of the point estimates we calculate.

2. Sample size is number of rows
The sample size, calculated here with the len function, is the number of observations, that is, the number of rows in the sample. That's true whichever method we use to create the sample. We'll stick to looking at simple random sampling since it works well in most cases and it's easier to reason about.

3. Various sample sizes
Let's calculate a population parameter, the mean cup points of the coffees. It's around eighty-two-point-one-five. This is our gold standard to compare against. If we take a sample size of ten, the point estimate of this parameter is wrong by about point-eight-eight. Increasing the sample size to one hundred gets us closer; the estimate is only wrong by about point-three-four. Increasing the sample size further to one thousand brings the estimate to about point-zero-three away from the population parameter. In general, larger sample sizes will give us more accurate results.

4. Relative errors
For any of these sample sizes, we want to compare the population mean to the sample mean. This is the same code we just saw, but with the numerical sample size replaced with a variable named sample_size. The most common metric for assessing the difference between the population and a sample mean is the relative error. The relative error is the absolute difference between the two numbers; that is, we ignore any minus signs, divided by the population mean. Here, we also multiply by one hundred to make it a percentage.

5. Relative error vs. sample size
Here's a line plot of relative error versus sample size. We see that the relative error decreases as the sample size increases, and beyond that, the plot has other important properties. Firstly, the blue line is really noisy, particularly for small sample sizes. If our sample size is small, the sample mean we calculate can be wildly different by adding one or two more random rows to the sample. Secondly, the amplitude of the line is quite steep, to begin with. When we have a small sample size, adding just a few more samples can give us much better accuracy. Further to the right of the plot, the line is less steep. If we already have a large sample size, adding a few more rows to the sample doesn't bring as much benefit. Finally, at the far right of the plot, where the sample size is the whole population, the relative error decreases to zero.

#### Creating a sampling distribution
1. Creating a sampling distribution
We just saw how point estimates like the sample mean will vary depending on which rows end up in the sample.

2. Same code, different answer
For example, this same code to calculate the mean cup points from a simple random sample of thirty coffees gives a slightly different answer each time. Let's try to visualize and quantify this variation.

3. Same code, 1000 times
A for loop lets us run the same code many times. It's especially useful for situations like this where the result contains some randomness. We start by creating an empty list to store the means. Then, we set up the for loop to repeatedly sample 30 coffees from coffee_ratings a total of 1000 times, calculating the mean cup points each time. After each calculation, we append the result, also called a replicate, to the list. Each time the code is run, we get one sample mean, so running the code a thousand times generates a list of one thousand sample means.

4. Distribution of sample means for size 30
The one thousand sample means form a distribution of sample means. To visualize a distribution, the best plot is often a histogram. Here we can see that most of the results lie between eighty-one and eighty-three, and they roughly follow a bell-shaped curve, like a normal distribution. There's an important piece of jargon we need to know here. A distribution of replicates of sample means, or other point estimates, is known as a sampling distribution.

5. Different sample sizes
Here are histograms from running the same code again with different sample sizes. When we decrease the original sample size of thirty to six, we can see from the x-values that the range of the results is broader. The bulk of the results now lie between eighty and eighty-four. On the other hand, increasing the sample size to one hundred and fifty results in a much narrower range. Now most of the results are between eighty-one-point-eight and eighty-two-point-six. As we saw previously, bigger sample sizes give us more accurate results. By replicating the sampling many times, as we've done here, we can quantify that accuracy.

#### Approximate sampling distributions
1. Approximate sampling distributions
In the last exercise, we saw that while increasing the number of replicates didn't affect the relative error of the sample means; it did result in a more consistent shape to the distribution.

2. 4 dice
Let's consider the case of four six-sided dice rolls. We can generate all possible combinations of rolls using the expand_grid function, which is defined in the pandas documentation, and uses the itertools package. There are six to the power four, or one-thousand-two-hundred-ninety-six possible dice roll combinations.

3. Mean roll
Let's consider the mean of the four rolls by adding a column to our DataFrame called mean_roll. mean_roll ranges from 1, when four ones are rolled, to 6, when four sixes are rolled.

4. Exact sampling distribution
Since the mean roll takes discrete values instead of continuous values, the best way to see the distribution of mean_roll is to draw a bar plot. First, we convert mean_roll to a categorical by setting its type to category. We are interested in the counts of each value, so we use dot-value_counts, passing the sort equals False argument. This ensures the x-axis ranges from one to six instead of sorting the bars by frequency. Chaining dot-plot to value_counts, and setting kind to "bar", produces a bar plot of the mean roll distribution. This is the exact sampling distribution of the mean roll because it contains every single combination of die rolls.

5. The number of outcomes increases fast
If we increase the number of dice in our scenario, the number of possible outcomes increases by a factor of six each time. These values can be shown by creating a DataFrame with two columns: n_dice, ranging from 1 to 100, and n_outcomes, which is the number of possible outcomes, calculated using six to the power of the number of dice. With just one hundred dice, the number of outcomes is about the same as the number of atoms in the universe: six-point-five times ten to the seventy-seventh power. Long before you start dealing with big datasets, it becomes computationally impossible to calculate the exact sampling distribution. That means we need to rely on approximations.

6. Simulating the mean of four dice rolls
We can generate a sample mean of four dice rolls using NumPy's random-dot-choice method, specifying size as four. This will randomly choose values from a specified list, in this case, four values from the numbers one to six, which is created using a range from one to seven wrapped in the list function. Notice that we set replace equals True because we can roll the same number several times.

7. Simulating the mean of four dice rolls
Then we use a for loop to generate lots of sample means, in this case, one thousand. We again use the dot-append method to populate the sample means list with our simulated sample means. The output contains a sampling of many of the same values we saw with the exact sampling distribution.

8. Approximate sampling distribution
Here's a histogram of the approximate sampling distribution of mean rolls. This time, it uses the simulated rather than the exact values. It's known as an approximate sampling distribution. Notice that although it isn't perfect, it's pretty close to the exact sampling distribution. Usually, we don't have access to the whole population, so we can't calculate the exact sampling distribution. However, we can feel relatively confident that using an approximation will provide a good guess as to how the sampling distribution will behave.

#### Standard errors and the Central Limit Theorem
1. Standard errors and the Central Limit Theorem
The Gaussian distribution (also known as the normal distribution) plays an important role in statistics. Its distinctive bell-shaped curve has been cropping up throughout this course.

2. Sampling distribution of mean cup points
Here are approximate sampling distributions of the mean cup points from the coffee dataset. Each histogram shows five thousand replicates, with different sample sizes in each case. Look at the x-axis labels. We already saw how increasing the sample size results in greater accuracy in our estimates of the population parameter, so the width of the distribution shrinks as the sample size increases. When the sample size is five, the x-axis ranges from seventy-six to eighty-six, whereas, for a sample size of three hundred and twenty, the range is from eighty-one-point-six to eighty-two-point-six. Now, look at the shape of each distribution. As the sample size increases, we can see that the shape of the curve gets closer and closer to being a normal distribution. At sample size five, the curve is only a very loose approximation since it isn't very symmetric. By sample size eighty, it is a very reasonable approximation.

3. Consequences of the central limit theorem
What we just saw is, in essence, what the central limit theorem tells us. The means of independent samples have normal distributions. Then, as the sample size increases, we see two things. The distribution of these averages gets closer to being normal, and the width of this sampling distribution gets narrower.

4. Population & sampling distribution means
Recall the population parameter of the mean cup points. We've seen this calculation before, and its value is eighty-two-point-one-five. We can also calculate summary statistics on our sampling distributions to see how they compare. For each of our four sampling distributions, if we take the mean of our sample means, we can see that we get values that are pretty close to the population parameter that the sampling distributions are trying to estimate.

5. Population & sampling distribution standard deviations
Now let's consider the standard deviation of the population cup points. It's about two-point-seven. By comparison, if we take the standard deviation of the sample means from each of the sampling distributions using NumPy, we get much smaller numbers, and they decrease as the sample size increases. Note that when we are calculating a population standard deviation with pandas dot-std, we must specify ddof equals zero, as dot-std calculates a sample standard deviation by default. When we are calculating a standard deviation on a sample of the population using NumPy's std function, like in these calculations on the sampling distribution, we must specify a ddof of one. So what are these smaller standard deviation values?

6. Population mean over square root sample size
One other consequence of the central limit theorem is that if we divide the population standard deviation, in this case around 2-point-7, by the square root of the sample size, we get an estimate of the standard deviation of the sampling distribution for that sample size. It isn't exact because of the randomness involved in the sampling process, but it's pretty close.

7. Standard error
We just saw the impact of the sample size on the standard deviation of the sampling distribution. This standard deviation of the sampling distribution has a special name: the standard error. It is useful in a variety of contexts, from estimating population standard deviation to setting expectations on what level of variability we would expect from the sampling process.


## IV. Bootstrap Distributions

#### Introduction to bootstrapping 
So far, we've mostly focused on the idea of sampling without replacement.

##### With or without?
- **Simple random sampling without replacement:** each row of the dataset, or each type of coffee, can only appear once in the sample.
It is like dealing a pack of cards. When we deal the ace of spades to one player, we can't then deal the ace of spades to another player.

- **Simple random sampling with replacement or resampling:** each row of the dataset, or each coffee, can be sampled multiple times.
It is like rolling dice. If we roll a six, we can still get a six on the next roll. We'll use the terms interchangeably.

##### Why sample with replacement?
So far, we've been treating the `coffee_ratings` dataset as the population of all coffees. Of course, it doesn't include every coffee in the world, so we could treat the coffee dataset as just being a big sample of coffees. To imagine what the whole population is like, we need to approximate the other coffees that aren't in the dataset. Each of the coffees in the sample dataset will have properties that are representative of the coffees that we don't have.   

##### **Resampling lets us use the existing coffees to approximate those other theoretical coffees.**
- **Coffee data preparation:** To keep it simple, let's focus on three columns of the coffee dataset. To make it easier to see which rows ended up in the sample, we'll add a row index column called index using the reset_index method.

- **Resampling with .sample():** To sample with replacement, we call `sample` as usual but set the `replace` argument to `True`. Setting `frac` to `1` produces a sample of the same size as the original dataset.

- **Repeated coffees:** Counting the values of the index column shows how many times each coffee ended up in the resampled dataset. Some coffees were sampled **four or five times.**

- **Missing coffees:** That means that some coffees didn't end up in the resample. By taking the number of distinct index values in the resampled dataset, using len on drop_duplicates, we see that eight hundred and sixty-eight different coffees were included. By comparing this number with the total number of coffees, we can see that four hundred and seventy coffees weren't included in the resample.

##### Bootstrapping
We're going to use resampling for a technique called bootstrapping. **In some sense, bootstrapping is the opposite of sampling from a population.** 
- With sampling, we treat the dataset as the population and move to a smaller sample.
- With bootstrapping, we treat the dataset as a sample and use it to build up a theoretical population. **A use case of bootstrapping is to try to understand the variability due to sampling.** This is important in cases where we aren't able to sample the population multiple times to create a sampling distribution.

##### Bootstrapping process
The bootstrapping process has three steps. 
- **First, randomly sample with replacement** to get a resample the same size as the original dataset.
- **Then, calculate a statistic**, such as a mean of one of the columns. Note that the mean isn't always the choice here and bootstrapping allows for complex statistics to be computed, too.
- **Then, replicate this many times to get lots of these bootstrap statistics.**

Earlier in the course, we did something similar. We took a simple random sample, then calculated a summary statistic, then repeated those two steps to form a sampling distribution. This time, when we've used **resampling** instead of sampling, we get a bootstrap distribution.


##### Bootstrap distribution histogram
Here's a histogram of the bootstrap distribution of the sample mean. Notice that it is close to following a normal distribution.


#### Comparing sampling and bootstrap distributions | Standard error

2. Coffee focused subset
we took a focused subset of the coffee dataset. Here's a five hundred row sample from it.

Here, we generate a bootstrap distribution of the mean coffee flavor scores from that sample. dot-sample generates a resample, np-dot-mean calculates the statistic, and the for loop with dot-append repeats these steps to produce a distribution of bootstrap statistics.

##### **Sample, bootstrap distribution, population means:** Here's the mean flavor score from the original sample. In the bootstrap distribution, each value is an estimate of the mean flavor score. Recall that each of these values corresponds to one potential sample mean from the theoretical population. If we take the mean of those means, we get our best guess of the population mean.
**The two values are really close. However, there's a problem. The true population mean is actually a little different.**

- **Interpreting the means:** The behavior that you just saw is typical. The bootstrap distribution mean is usually almost identical to the original sample mean. However, that is not often a good thing. If the original sample wasn't closely representative of the population, then the bootstrap distribution mean won't be a good estimate of the population mean. **Bootstrapping cannot correct any potential biases due to differences between the sample and the population.**

##### Sample sd vs. bootstrap distribution sd
While we do have that limitation in estimating the population mean, one great thing about distributions is that we can also quantify variation. The standard deviation of the sample flavors is around zero-point-three-five-four. Recall that pandas dot-std calculates a sample standard deviation by default. If we calculate the standard deviation of the bootstrap distribution, specifying a ddof of one, then we get a completely different number. So what's going on here?

- **Sample, bootstrap dist'n, pop'n standard deviations:** Remember that one goal of bootstrapping is to quantify what variability we might expect in our sample statistic as we go from one sample to another. Recall that this quantity is called the **standard error** as measured by the standard deviation of the sampling distribution of that statistic. The standard deviation of the bootstrap means can be used as a way to estimate this measure of uncertainty.
**If we multiply that standard error by the square root of the sample size, we get an estimate of the standard deviation in the original population.** Our estimate of the standard deviation is around point-three-five-three. The true standard deviation is around point-three-four-one, so our estimate is pretty close. **In fact, it is closer than just using the sample standard deviation alone.**

- **Interpreting the standard errors:** To recap, the estimated standard error is the standard deviation of the bootstrap distribution values for our statistic of interest. **This estimated standard error times the square root of the sample size gives a really good estimate of the standard deviation of the population.** That is, although bootstrapping was poor at estimating the population mean, it is generally great for estimating the population standard deviation.


#### Confidence intervals
In the last few exercises, you looked at relationships between the sampling distribution and the bootstrap distribution.
One way to quantify these distributions is the idea of "values within one standard deviation of the mean", which gives a good sense of where most of the values in a distribution lie. In this final lesson, we'll formalize the idea of values close to a statistic by defining the term "confidence interval".

##### Predicting the weather
Consider meteorologists predicting weather in one of the world's most unpredictable regions - the northern Great Plains of the US and Canada. Rapid City, South Dakota was ranked as the least predictable of the 120 US cities with a National Weather Service forecast office. Suppose we've taken a job as a meteorologist at a news station in Rapid City. Our job is to predict tomorrow's high temperature.

- Our weather prediction
We analyze the weather data using the best forecasting tools available to us and predict a high temperature of 47 degrees Fahrenheit. In this case, 47 degrees is our point estimate. Since the weather is variable, and many South Dakotans will plan their day tomorrow based on our forecast, we'd instead like to present a range of plausible values for the high temperature. On our weather show, we report that the high temperature will be between forty and fifty-four degrees tomorrow.

- We just reported a confidence interval!
This prediction of forty to fifty-four degrees can be thought of as a confidence interval for the unknown quantity of tomorrow's high temperature. Although we can't be sure of the exact temperature, we are confident that it will be in that range. These results are often written as the point estimate followed by the confidence interval's lower and upper bounds in parentheses or square brackets. When the confidence interval is symmetric around the point estimate, we can represent it as the point estimate plus or minus the margin of error, in this case, seven degrees.

##### Bootstrap distribution of mean flavor
Here's the bootstrap distribution of the mean flavor from the coffee dataset.

- Mean of the resamples
We can calculate the mean of these resampled mean flavors.

-  Mean plus or minus one standard deviation
If we create a confidence interval by adding and subtracting one standard deviation from the mean, we see that there are lots of values in the bootstrap distribution outside of this one standard deviation confidence interval.

##### Quantile method for confidence intervals
If we want to include ninety-five percent of the values in the confidence interval, we can use quantiles. Recall that quantiles split distributions into sections containing a particular proportion of the total data. To get the middle ninety-five percent of values, we go from the point-zero-two-five quantile to the point-nine-seven-five quantile since the difference between those two numbers is point-nine-five. To calculate the lower and upper bounds for this confidence interval, we call quantile from NumPy, passing the distribution values and the quantile values to use. The confidence interval is from around seven-point-four-eight to seven-point-five-four.

- **Inverse cumulative distribution function**
There is a second method to calculate confidence intervals. To understand it, we need to be familiar with the normal distribution's inverse cumulative distribution function. The bell curve we've seen before is the probability density function or PDF. Using calculus, if we integrate this, we get the cumulative distribution function or CDF. If we flip the x and y axes, we get the inverse CDF. We can use scipy-dot-stats and call norm-dot-ppf to get the inverse CDF. It takes a quantile between zero and one and returns the values of the normal distribution for that quantile. The parameters of loc and scale are set to 0 and 1 by default, corresponding to the standard normal distribution. Notice that the values corresponding to point-zero-two-five and point-nine-seven-five are about minus and plus two for the standard normal distribution.

##### Standard error method for confidence interval
This second method for calculating a confidence interval is called the standard error method. First, we calculate the point estimate, which is the mean of the bootstrap distribution, and the standard error, which is estimated by the standard deviation of the bootstrap distribution. Then we call norm-dot-ppf to get the inverse CDF of the normal distribution with the same mean and standard deviation as the bootstrap distribution. Again, the confidence interval is from seven-point-four-eight to seven-point-five-four, though the numbers differ slightly from last time since our bootstrap distribution isn't perfectly normal.

##### The most important things
- **The std. deviation of a bootstrap statistic is a good approximation of the standard error**
- **Can assume bootstrap distributions are normally distributed for confidence intervals**
