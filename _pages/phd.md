---
layout: single
author_profile: true
permalink: /phd/
classes: wide

feature_row1:
  - image_path: images/3xBBNcM.gif
    excerpt: "Atrial fibrillation is an electrical defect in the upper chambers of heart that causes the atria to quiver rapidly, reducing the heart's ability to pump blood. It is the most common sustained cardiac arrhythmia, affecting nearly 1% of the population. Fibrillation starts with electrical triggers originating at the pulmonary veins that then continuously circulate around the atria."

feature_row2:
  - image_path: images/gqA7BrI.gif
    excerpt: To prevent the triggers from reaching the atria, patients can undergo pulmonary vein isolation therapy. This can be achieved by a variety of methods, including most recently, **cryoballoon ablation**. In this case, a balloon is inflated and pressed against the pulmonary vein opening and nitrous oxide is injected into the balloon to cryogenically ablate tissue.

feature_row3:
  - image_path: images/angio-wide.gif
    url: images/angio-wide.gif
    title: Angiography 
  - image_path: images/lge-wide.gif
    url: images/angio-wide.gif
    title: Late Gd Enhanced Scar
---

{% include feature_row id="feature_row1" type="left" %}

{% include feature_row id="feature_row2" type="right" %}


Regardless of the method of ablation, approximately **1/3 patients still experience AF** post-therapy. To investigate this, a contrast-enhanced MR sequence that can image both angiography and scar by sharing K-space data was developed. This provides multiple benefits including:
* Faster acquisition for 3D scar images
* Accurate atrial segmentation on angiography images 
* Simplified co-registration between angiography/scar images 


{% include gallery id="feature_row3" caption="Left: Angiography images, taken immediately after gadolinium injection. Right: Scar images, taken 10 minutes post-contrast injection" %}

The high contrast at the blood-atrial border of the angiography images makes segmentation simple. The borders are then directly transferred to the LGE images and expanded to extract the atrial wall.

<!-- 
		    		<h2> <strong> <center> Angiography </center></strong> </h2>
		    		&emsp;
					<video class="share-video center" id="share-video" poster="https://thumbs.gfycat.com/JampackedOilyEuropeanfiresalamander-poster.jpg" autoplay controls muted loop>
						<source id="webmSource" src="https://zippy.gfycat.com/JampackedOilyEuropeanfiresalamander.webm" type="video/webm">
						<source id="mp4Source" src="https://zippy.gfycat.com/JampackedOilyEuropeanfiresalamander.mp4" type="video/mp4">
		    	</div>

		    	<div class="col-md-5">
		    		<h2> <strong> <center> LGE Scar - Segmented Wall </center></strong> </h2>
		    		&emsp;
					<video class="share-video center" id="share-video" poster="https://thumbs.gfycat.com/ElderlyTenderArcticfox-poster.jpg" autoplay controls muted loop>
						<source id="webmSource" src="https://zippy.gfycat.com/ElderlyTenderArcticfox.webm" type="video/webm">
						<source id="mp4Source" src="https://zippy.gfycat.com/ElderlyTenderArcticfox.mp4" type="video/mp4">
		    	</div>
		    </div>
	    	<div class="row" id="gfyinfo">
		    	<div class="col-md-5 col-md-offset-1" style="text-align:justify;">
		    		<h3> Segmentation on angiography images is simpler and more accurate than LGE segmentation due to high 
		    			contrast at the blood-atrial border. A <b> 3D reconstruction </b> of the left atrium can be created 
		    			using these borders</h3>
		    	</div>

		    	<div class="col-md-5" style="text-align:justify;">
		    		<h3> Borders can be directly transferred to LGE images after a rigid co-registration. Dilating the borders
		    		 allows quick extraction of the atrial wall. The 3D shell can be colored from the <b>pixel intensities</b> of
		    		 the atrial wall with a <b>K-Nearest Neighbors</b> algorithm </h3> 
		    	</div>
		    </div>
		    <br><br>	
			<div id="3drecon" style="text-align:center">
				<h1><center> <b><i> Left Atrium with Scar Intensity Overlay </b></i> </center></h1>
				<iframe src="http://ayplam.github.io/atrium/" style="height:100%; width:90%; float:center" align="center" scrolling="no" frameborder=0 id="atriumstl">
					<p>Your browser does not support iframes.</p>
				</iframe>
			</div>

			<div class="row" id="consAndPVb">
				<div class="col-md-5 col-md-offset-2" style="text-align:justify">

			    	<h3>
			    		<br>
			    		&emsp; While 3D reconstructions are useful for planning re-ablations, a new method of visualization is 
			    		necessary to quantify the <b>degree of encirclement from ablation </b> and to assess whether local regions of scar or
			    		healthy tissue can be correlated to freedom from AF
			    		<br><br>
			    		&emsp; This can be achieved by creating a <b>Pulmonary Vein Bullseye </b> that projects atrial wall points onto a 
			    		polar plot. A surface plot of the pixel intensities can be interpolated from all the projected points.
		    		</h3>

			    	<h3>
			    </div>
    		    	<img src="http://i.imgur.com/AELgt9X.png?3" alt="PVBullseye">
		    	</div>
			</div>

			<div class="row">
				<h1> 
					<b><i><center> Sample Atria </center></b></i>
				</h1>

				<div class="col-md-8 col-md-offset-2">
					<img src="http://i.imgur.com/SwSuJQn.png" style="width:auto;height:auto;max-width:1000px">
				</div>


		    </div>
		<div>



	</section>




</section> -->
