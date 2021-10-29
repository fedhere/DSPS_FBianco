
**Part 1 Due by start of class on Friday**. Watch the video lecture that you find in the media gallery on canvas (lecture 12). Find a "bad" visualization, bad according to what is discussed in the lecture, save it on your device or take a screenshot, and be ready to submit it to a Google form at the beginning of class on Friday along with the reason why you thought it was bad (which rules or best practices does it violate)

We will pick a few random visualizations to discuss why the submitter thought they were "bad". While you can choose any scientific visualization it has to be a scientific visualization, meaning come from a paper published in a scientific journal or on arXiv as a pre-print. Instructions on getting the pre-prints if you're not familiar with them and you do not already have a plot in mind are in the lecture at the end.

**Part 2 Due at regular homework deadline Sunday midnight**. Take a plot you made in the past (for a glass of research or even for this class) and make it _better_ following the best practices discussed in class. Post your “before” and “after” in a README.md mark down on GitHub in HW6 folder along with a description of what was improved and why

![image](https://user-images.githubusercontent.com/1696902/139481925-9b25650c-94ec-491f-973b-645ae9110ee7.png)


![image](https://user-images.githubusercontent.com/1696902/139481812-3c4ad4f3-efde-4c1c-844f-6c2edd843ede.png)

This plot suffers from density scaling: the dynamic range of densities of points in different regions of the plot is so significant that it is impossible to perceive simultaneously how dense the dense regions are and how far the data points extend into the parameter space in the original version.

I used transparency to increase the visibility of datapoints where the density of detapoints is not critically high and used contours to represent the point density where the density is so high that the transparency alone would not enable to differences in this region of the plot as well as still see the points that are more isolated. 

Now features in the data, like the bridge of points at log-luminosity ~ 28 and log-effective temperature 3.6-3.8 which was hidden before.

I also modified the x tick labels by changing the axis notation and plotting the log of the points in their natural space, as opposed to the points in log space. This increases the readability of the values of Temperature by enabling the display of more values and on the luminosity by reducing the complexity of the tick labels


