---
layout: galileo_post
title:  560 Hours
date:   2017-10-06 18:59:14 +0100
description: Scaling up is never a problem...right?
---
<style>
code {
	font-size: 15px;
}

</style>


I ran into a bit of a wall yesterday regarding "big data". 

The tools data scientists use are supposed to be easily scalable. It doesn't matter how big your data gets, you can just add on more compute resources. After all, most of the computations required can be distributed, and scaling horizontally (many small nodes) is generally cheaper than scaling vertically (a few really large nodes).

To work efficiently, I generally test my components on a small portion of the data.
* Processing 2 records took 15 minutes
* Processing 40 records took 2.5 hours

I have **9000 records** to process. So assuming this scales linearly, that would be 560 hours, or about 3 weeks. 

Yea, that's not going to fly. But I have a feeling I'm not doing things correctly. My intuition tells me that if I double the number of workers, (`--num-executors` for you Spark people out there) the time should be cut in half. But that's not the result I'm seeing, which is strange.

Anyways, it's the first time I've had to actually think about how I can make things run faster instead of just stupidly throw more resources at the problem so that's interesting for me.