---
layout: post
title: "Fixing infectionsim: Fixing my poor university code! 🔨"
date: 2025-12-26
tagline: Fixing the poor layout and lack comments of my SIR modeling using randomly moving particles
image: /images/post_images/2025_11_19_floating_point/floats.png
category: Fixing Old Code
---
<h3 class="major">Introduction 👋</h3>
This was the **second** Python project I ever worked on, the first being a Connect 4 game that went off the rails. That one will be fun to fix, but for now, we are working on a project I made due to a change of plans. This was the year 2020. I was in the first year of my degree, and in March, COVID-19 struck the UK with its ability to shut down the entire globe; it sent us home from University, throwing us into disarray. We have experimental modules we have to complete, and we have been handing our work in person. What do we do? What work can we do? How do we get our lectures ahhh 😰!!

The university system was revolutionised; they recorded lectures at home, then enabled handing in PDFs on Blackboard. This made things easier, but there is still the question of how we are going to do the experimental module. It was data analysis. I cannot recall the other options for this miniproject, but having the opportunity to be able to apply skills to a current event was something I couldn't pass up I was already tracking the growth of COVID using data from Johns Hopkins, so this came together nicely. So I looked in to the **SIR** model and I was hooked how a random distrubion can cause somthing so real to happen, this happens with other things of course we get patterns that occur from randomness all the time look at ratiation, but this griped me so I figured out how to implement the random walks and how to change the infection distribution, I could even compare it to covid. But it sort of moved from the project, so the write-up didn't end up the way I wanted it to be, so here is a way I can do it the way I want to. And make the code better.

So in the next section, I will lay out how the simulation works and the background of the **SIR**, then in the section after, I will talk about the good parts of my simulation. Following that, I will critique my old code about how it is set up, improvements that can be made, and the lack of comments, then I will correct the simulation. After that, talk through some consequences of the simulation, adding comparisons to COVID-19, and finally, I will conclude the exercise by talking about whether it was worth rewriting all of these years later, and my development since that time in my life.

<h3 class="major">Numerical Model 🖥</h3>
This section will focus on the **SIR** model used by biologists for disease spread, and how this model comes out of random interactions between individuals and ways we can alter the model to line up with the real world.
<h3>The SIR Model 🦠</h3>
The **SIR** model <a id="ref-1"></a>[\[1\]](#bib-1) is one of the most simple **Compartmental models**. These models are used to see how populations move between different states, in this case representing **S**usceptible, **I**nfectious, and **R**ecovered individuals. 

**S**usceptible: This group represent the people who have not caught or recovered from the disease. This group does not grow inside this model due to the assumption that the recovered group cannot catch the disease again

**I**nfectious: This group represents the people who are currently infected with the disease. This group in this model has the most complex behaviour due to this group both growing due to the infection and decreasing due to people recovering from the disease.

**R**ecovered: This group are **"Recovered"** from the disease. You may notice the quotation marks; this is due to this group not being separated into dead and non-dead categories. This may throw off the model slightly, but for a low mortality-to-recovery ratio of SARS-type viruses, it is relatively safe to assume the recovered group would dominate.

The recovered group brings up a question in this COVID modelling: Does the recovered domination assumption hold? Because we had to lock down, it must have had a high mortality, right? Johns Hopkins<a id="ref-2"></a>[\[2\]](#bib-2) gives a case mortality ratio around 1-2 %, this measure would more likely give an overestimate than an underestermate due to relying on confermed cases, people are more likly to have not got tested due to the how contagus the disease was, even if this overestermated number was close to the "truth" it is still dominated by people recovering meaning the assumtion holds.

Just existing in these groups does not a model make, we require the way these to interact. How do they do it? Well, differential equations of course! These differential equations were solved by Harko et. al <a id="ref-3"></a>[\[3\]](#bib-3) using the Abel solution, but those solutions are not needed in this post; the differential equations are defined as:

$$
\begin{aligned}
\frac{dS}{dt} &= -\beta \frac{SI}{N} \\
\frac{dI}{dt} &= \beta \frac{SI}{N} - \gamma I \\
\frac{dR}{dt} &= \gamma I
\end{aligned}
$$

Where N is the total number in all populations, $\beta$ is the infection rate, and $\gamma$ is the recovery rate. This post will only numerically solve these differential equations.




<h3>Particle Simulation</h3>
This particle simulation is relatively simple. There is random movement, any random space between -50 and 50 😕, then checks whether it is in a radius of another. If either is infected, there is a chance that they will get infected, then it goes and goes again, yeah, I hate this too 😡. I will improve this, but you'll have to wait for my criticisms to find out how.
<h3 class="major">What this guy did well?🥳<h3>
This Guy man he managed to see what was going on from some form of randomness. This was a great idea to show how mathematical equation can describe the real world, there are many application of these forms of randomness and particle simulations. Such as being able to show ensamble behavour like this and the standard gas equations.

This approach also allow for outbreak dynamics, showing how the growth of the disease can depend on the both the way people act and the spreadablility of the disease.

I also made it paramatrisaseable, meaning we can explore transmissability and people dynamics by altering diffrent things to improve the dynamics and give advice on how we can reduce the spread.

The visualisations using gif are awesome seeing the development if the infection and people interacting, spreading the disease, showing how important it was to lockdown.

The multiprocessing to be able to run multiple simulations fast is nice can so we can nomalise the randomness and run multiple parameters quickly.

<h3 class="major">What this guy did badly?😭<h3>
<h3 class="major"> Bibliography</h3>
<a id="bib-1"></a>[1] Hethcote, Herbert W, "The Mathematics of Infectious Diseases", SIAM Review, Volume 42, 4, pages 599-653, 2000, <a href="https://doi.org/10.1137/S0036144500371907" target="_blank" rel="noopener noreferrer">https://doi.org/10.1137/S0036144500371907</a>. <a href="#ref-1">↩</a>

<a id="bib-2"></a>[2] Johns Hopkins University, "Mortality Analyses", Johns Hopkins Coronavirus Resource Center, n.d. <a href="https://coronavirus.jhu.edu/data/mortality" target="_blank" rel="noopener noreferrer">https://coronavirus.jhu.edu/data/mortality</a>. <a href="#ref-2">↩</a>

<a id="bib-3"></a>[3] Tiberiu Harko, Francisco S.N. Lobo, M.K. Mak, "Exact analytical solutions of the Susceptible-Infected-Recovered (SIR) epidemic model and of the SIR mode", Applied Mathematics and Computation, Volume 236, Pages 184-194, ISSN 0096-3003, 2014 <a href="https://doi.org/10.1016/j.amc.2014.03.030" target="_blank" rel="noopener noreferrer">https://doi.org/10.1016/j.amc.2014.03.030</a>. <a href="#ref-3">↩</a>