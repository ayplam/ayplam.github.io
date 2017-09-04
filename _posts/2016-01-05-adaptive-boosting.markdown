---
layout: galileo_post
title:  Adaptive Boosting
date:   2016-01-05 13:05:14 +0100
---


<style>		

    @import 'https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.2.0/katex.min.css';
    code {
        color: #c7254e;
        background-color: #f9f2f4;
        border-radius: 4px;
        font-size:90%;
    }
    code,
    kbd {
        padding: 2px 4px
    }
    kbd {
        color: #fff;
        background-color: #333;
        border-radius: 3px;
        box-shadow: inset 0 -1px 0 rgba(0, 0, 0, .25)
    }
    kbd kbd {
        padding: 0;
        font-size: 100%;
        box-shadow: none
    }
    pre {
        display: block;
        margin: 0 0 10px;
        word-break: break-all;
        word-wrap: break-word;
        color: #333;
        background-color: #f5f5f5;
        border: 1px solid #ccc;
        border-radius: 4px
    }
    pre code {
        padding: 0;
        font-size: inherit;
        color: inherit;
        white-space: pre-wrap;
        background-color: transparent;
        border-radius: 0
    }

	#tooltip1 { position: relative; }

	#tooltip1 a span { 
	display: none; 
	color: #FFFFFF;
	}

	#tooltip1 a:hover span {
	display: block;
	position: absolute;
	width: 200px;
	height: 50px;
	color: #FFFFFF;
	top:-100%;
	padding-left:70%;
	}


</style>
<script src="{{ "{{ site.baseurl }}/js/d3.v3.js" | prepend: site.baseurl }}" type="text/javascript"></script>
<script src="{{ "{{ site.baseurl }}/js/nv.d3.min.js" | prepend: site.baseurl }}" type="text/javascript"></script>
<script>

function createGraph(filename, chartName, cmap, legend) { 

    if (typeof cmap === 'undefined') { cmap = d3.scale.category10().range(); }
    if (typeof legend === 'undefined') { legend = true; }

    console.log(cmap.length)

    d3.json(filename, function (data) 
    {   

        // Creating a new colorscheme for other iterations
        var realData = []

        if (cmap.length == 0) {

            var rwb = d3.scale.linear()
                .domain([0, 5, 9])
                .range(["#8B0000", "#FFFFFF", "#00008B"]);

            for (var i=0; i < data.length; i++) {
                if (data[i].values.length != 0) {
                    console.log(i)
                    cmap.push(rwb(i))
                    realData.push(data[i])
                }
                
            }

            data = realData
        }

        nv.addGraph(function() {
          var chart = nv.models.scatterChart()
                        .showDistX(true)    //showDist, when true, will display those little distribution lines on the axis.
                        .showDistY(true)
                        // .transitionDuration(350)
                        .color(cmap)
                        .width(425)
                        .height(425)
                        .showLegend(legend)


          //Configure how the tooltip looks.
          chart.tooltipContent(function(key) {
              return '<h5> &emsp;' + key.series[0].key + '&emsp; </h5>';
          });

          //Axis settings
          // chart.xAxis.tickFormat(d3.format('.02f'));
          // chart.yAxis.tickFormat(d3.format('.02f'));


          var myData = data;
          d3.select(chartName)
              .datum(myData)
              .call(chart);

          nv.utils.windowResize(chart.update);

          return chart;
        });
    });    
}

</script>

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
	<br>
	&emsp; You might imagine that some weak classifiers are better than others. For example, frequent handwashing is probably be a better classifier than drinking orange juice. Because of this, it is also important to weight your classifiers accordingly.
</p>
</blockquote>

This is how AdaBoost works. The term "boosting" comes into play because it boosts weak predictions that are inaccurate by themselves into a strong prediction when combined together. Think Captain Planet. It is considered adaptive because it weights the misclassified predictions more heavily for the next iteration so that the algorithm gives these misclassfieid predictions more attention.

<p class="post-image-caption"> </p>

Before starting, let's define our weak classifier and our weak learner:

{% highlight python %}
class weakClassifier:
    
    def __init__(self, feat, thresh, out, alpha=0):
        self.feat = feat
        self.thresh = thresh
        self.out = out
        self.alpha = alpha        

def weakLearner(wc, data):
	# Correctly predicted values
    if (wc.out == 1):
        y = (data[:,wc.feat] >= wc.thresh)*1
    else:
        y = (data[:,wc.feat] < wc.thresh)*1
    
    # Incorrectly predicted values
    out = np.where(y == 0)    
    y[out[0]] = -1
    
    return y

{% endhighlight %}

Our <code><i>weakClassifier</i></code> has four attributes:
<br>&emsp; <i class="fa fa-angle-double-right"></i> The feature used for classification
<br>&emsp; <i class="fa fa-angle-double-right"></i> The threshold for the feature 
<br>&emsp; <i class="fa fa-angle-double-right"></i> The predicted outcome given the threshold
<br>&emsp; <i class="fa fa-angle-double-right"></i> The weight associated with the classifier
<br>
Our <code><i>weakLearner</i></code> takes the <code><i>weakClassifier</i></code> to assess the data. Correctly predicted values are given a value of <b>1</b> while incorrectly predicted values are given a value of <b>-1</b>.

Below you can find a simplified AdaBoost algorithm:

{% highlight python %}
### INITIALIZATION
n_wc = 20
D = np.ones(label.shape) / len(label) 
alpha = np.zeros((n_wc,1))
err = [0]*n_wc

nfeats = data.shape[1]
nsamples = data.shape[0]

mn = np.min(data,0)
mx = np.max(data,0)
  
### ITERATION
for i in xrange(n_wc):
    err[i] = np.inf;       
    for out in [-1,1]: 
        for feat in xrange(nfeats): 

            thresholds = np.linspace(mn[feat],mx[feat],100)
            
            for thr in thresholds: 

                ## CALCULATE ERROR
                wc = weakClassifier(feat,thr,out)
                weaklearn_tmp = weakLearner(wc,data) != label
                curr_err = sum(np.multiply(D,weaklearn_tmp));

                if ( curr_err < err[i]):
                    err[i] = curr_err;
                    wc_container[i] = wc;

    ## STOP CONDITION
    if(err[i] >= 0.5):
    	print 'Convergence reached'
        break

	## KEEP CLASSIFIER
    alpha[i] = 0.5 * np.log((1-err[i])/err[i]);
    wc_container[i].alpha = alpha[i]

    ## UPDATE WEIGHTS
    weaklearn_curr = weakLearner(wc_container[i], data)
    weighted_labels = np.multiply(-alpha[i],label)
    q = np.multiply(weighted_labels,weighted_labels)
    D = np.multiply(D,np.exp(q))
    D = D/sum(D);
    
    return wc_container
  {% endhighlight %}
      
To understand the algorithm, it has been subdivided into parts.

<p class="link" id="tooltip1">
<a> <b>THE DATA</b><span> 
<img src="http://i.imgur.com/asOXxxF.png">
</span></a>
</p>


<div id="orig_data" style="height:450px; margin:auto" >
    <svg></svg>
</div>

<script>
console.log(d3.scale.category10().range())
createGraph('/data/adab/orig.json','#orig_data svg')

</script>


For this sample dataset, each point has two features (x and y coordinate) and is associated with an outcome, 1 or -1.


<p class="link" id="tooltip1">
<a> <b>INITIALIZATION </b><span> 
<img src="http://i.imgur.com/AcBlf7y.png">
</span></a>
</p>

To start off, we initialize a few variables: <br>

&emsp; <i class="fa fa-angle-right"></i> <code>n_wc</code>: The number of weakClassifiers
<br>&emsp; <i class="fa fa-angle-right"></i> <code>D</code>: The weighting of each individual point
<br>&emsp; <i class="fa fa-angle-right"></i> <code>alpha</code>: The weight of each classifier
<br>&emsp; <i class="fa fa-angle-right"></i> <code>err</code>: The error associated with the classifier
<br>
<br>&emsp; <i class="fa fa-angle-right"></i> <code>nfeats</code>: The number of features (in this case, 2)
<br>&emsp; <i class="fa fa-angle-right"></i> <code>nsamples</code>: The number of samples (in this case, 2000)
<br>
<br>&emsp; <i class="fa fa-angle-right"></i> <code>mn</code>: The minimum value for each feature
<br>&emsp; <i class="fa fa-angle-right"></i> <code>mx</code>: The maximum value for each feature

<p class="link" id="tooltip1">
<a> <b>ITERATION </b><span> 
<img src="http://i.imgur.com/TzzHeXa.png">
</span></a>
</p>
The iteration process loops through the following variables:
<br>
&emsp; <i class="fa fa-angle-right"></i> the number of classifiers
<br>&emsp; <i class="fa fa-angle-right"></i> all outcomes
<br>&emsp; <i class="fa fa-angle-right"></i> number of features 
<br>&emsp; <i class="fa fa-angle-right"></i> a range of thresholds

The first loop allows the creation of 20 weak classifiers. The next three loops allow creation of all possible weak classifiers. 

<p class="link" id="tooltip1">
<a> <b>CALCULATE ERROR </b><span> 
<img src="http://i.imgur.com/TzzHeXa.png">
</span></a>
</p>

<error> Iteration 1 </b>
<br>

{% highlight python %}
### ITERATION
for i in xrange(n_wc):
    err[i] = np.inf;       
    for out in [-1,1]: 
        for feat in xrange(nfeats): 

            thresholds = np.linspace(mn[feat],mx[feat],100)
            
            for thr in thresholds: 

{% endhighlight %}

The goal is to find the best weak classifier that will minimize the error. The first two for loops go through all possible outcomes (True/False) and all possible dimensions (x-pos/y-pos). Next, we create 100 different thresholds to test in the expected range of the data and loop through this. Thus, we are testing all possible outcome/dimension/threshold combinations, and below is what we get:

<div id="rg_iter1" style="height:425px; width:425px; float:left">
    <svg></svg>
</div>
<div id="label_iter1" style="height:425px; width:425px; float:left">
    <svg></svg>
</div>

<script>
createGraph('/data/adab/rg_iter01.json','#rg_iter1 svg',['#008B00','#8B0000'])

createGraph('/data/adab/label_iter01.json','#label_iter1 svg',[],false)


</script>
<p class="post-image-caption" style="clear:left">
The actual classifier used is shown on the left. From using just one classifier, the correctly classified points are shown in green while incorrectly classified points are shown in red. 
