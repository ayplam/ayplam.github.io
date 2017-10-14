---
layout: galileo_post
title:  Windows Trouble
date:   2017-10-13 18:59:14 +0100
description: I always feel stupid when I discover...
---
<style>
code {
	font-size: 15px;
}

</style>

I always feel stupid when I discover a bug that I was responsible for which results in disastrous results. But on the bright side, it turns out scaling up wasn't as bad as I thought would be.

The data I'm working with is typically a document of 200 pages. But to be able to identify sections that are useful, it's been broken down such that each row contains the:

* Document ID
* Page Number
* Block Number
* The Text

So me trying to be clever in calculating a TF-IDF decided to implement a window function in Spark so that each row also contained the entire document. Which normally wouldn't have been a problem, but then I accientally fed that into the Spark ML IDF module. This turned out to be a disaster because now, I literally multipled the amount of data that actually needs processing by a factor of over 5,000!

Moral of the story - windowing functions, while nice, can be dangerous when working with large volumes of text.

On a separate note, I learned that Spark will only repartition your data upon performing `JOIN`s or `GROUP BY` aggregations. But what if you just have a bunch of UDFs that are performing operations on the base dataframe and you never need to aggregate or join? In those cases, Spark won't bother with repartition. However, if there is significant data skew, a simple `df.repartition(200)` before doing any heavy processing actually cut down my processing time by 1/3rd! I was initially suspicious about data skew since it was clear from the YARN ApplicationMaster that:

1. Different workers were receiving very different amounts of data and 
2. There was one lone worker chugging away while everything else was done.

So if you're trying to optimize a job, it might not hurt to force a repartition. 