---
layout: galileo_post
title:  Adaptive Boosting
date:   2016-01-05 13:05:14 +0100
location: Florence, Italy
---


Adaptive Boosting (aka AdaBoost) is a machine learning algorithm that combines multiple weak classifiers to create a much stronger classifier. Before going into the algorithm, let's go through a quick analogy.

<blockquote>
<p>
	&emsp; 5-20% of the US population is infected with influenza every year. Let's say you wanted to classify the population into individuals who will catch influenza and individuals who will stay flu-free. Here are some examples of weak classifiers:
</p>
<p>
	<li style="text-indent:50px;line-height:140%"> If an individual received their flu vaccine, they will be flu-free.</li>
	<li style="text-indent:50px"> If an individual washes their hands at least 10 times per day, they will be flu-free. </li>
</p>
<p>
	&emsp; Clearly, on their own, these classifiers are not great predictors of who will stay flu-free. The vaccine is only 50% effective (http://www.cdc.gov/flu/professionals/vaccination/effectiveness-studies.htm), so about half of people exposed to the flu will still get sick. In addition, if the individual has poor hygiene and does not wash their hands before eating, they could also get sick. Conversely, simply practicing good hygiene does not automatically keep an individual flu-free, especially if they did not receive their flu shot. You could continue listing weak classifiers to generalize whether individuals will or will not catch the flu, and by themselves, they would not be very useful. However, by combining all of the generalizations, you could eventually create a classifier that could strongly predict whether someone would or would not catch the flu. 
</p>
</blockquote>

Generally speaking, this is how AdaBoost works. The term "boosting" comes into play because it boosts weak predictions that are inaccurate by themselves into a strong prediction when combined together. Think Captain Planet. It is considered adaptive because it weights the misclassified predictions more heavily on the next iteration so that the algorithm gives these misclassfieid predictions more attention.

<p class="post-image-caption"> </p>

<b> Iteration 1 </b>
<br>
Time to find our first weak classifier and walk through the code for how to achieve this.

<pre><code class="python">for pos in [-1,1]: 
	for dim in xrange(ndims): 

		thresholds = np.linspace(mn[dim],mx[dim],100)		

		for thr in thresholds: # 100 possible thresholds
</code></pre>

The first two for loops go through all possible outcomes (True/False) and all possible dimensions (x-pos/y-pos). Next, we create 100 different thresholds to test in the expected range of the data and loop through this. Thus, we are testing all possible outcome/dimension/threshold combinations.
