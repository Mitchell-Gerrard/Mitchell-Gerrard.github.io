---
layout: post
title: "Fixing infectionsim: Fixing my poor university code! 🔨"
date: 2025-12-26
tagline: Fixing the poor layout and lack comments of my SIR modeling using randomly moving particles
image: /images/post_images/2025_11_19_floating_point/floats.png
category: Fixing Old Code
---
<h3 class="major">Introduction 👋</h3>
This was the **second** python project I ever worked on, the first being a connect 4 game that went off the rails, that one will be fun to fix, but for now we are working on a project I made due to a change of plans. This was the year 2020 I was in my first year of my degree, and in march the covid-19 struck the UK with it's ability to shutdown the entire globe, it sent us home from University throwing us in to disaray. We have experimental modules we have to complete, we have been handing our work inperson what do we do, what work can we do, how do we get our lectures ahhh 😰!!

The university system was reveluionised, they recored lectures at home then enabled handing in PDFs on Blackboard, this made things easier but there is still the question of how are we going to do the experimental module? It was data analysis. I cannot recall the other options for this miniproject, but having the opotunity to be able to apply skills to a current even was somthing I couln't pass up, I was allready tracking the growth of covid using data from John Hopkins, so this came together nicely. So I looked in to the **SIR** model and I was hooked how a random distrubion can cause somthing so real to happen, this happens with other things of course we get patterns that occur from randomness all the time look at ratiation, but this griped me so I figured out how to implement the random walks and how to change the infection distribution, I could even compare it to covid. But it sorted of moved from the project so the write up didnt end up the way I wanted it to be so here is a way I can do it the way I want to. And make the code better.

So in the next section I will layout how the simulation works and the background of the **SIR**, then in the section after I will talk about the good parts of my simulation. Following that I will critic my old code about how it is set up improvements that can be made and the lack of comments, then I will correct the simulation. After that talk through some consequences of the simulation adding comparisions to covid-19, finaly I will conclude the excersise talking about weather it was worth the rewrite all of these years later, and my development since that time in my life.

<h3 class="major">Numerical Model 🖥</h3>
This section will focus on the **SIR** model used by biologests for desise spread and how this model comes out of a random interactions between indviduals and ways we can alter the model to line up with the real world.
<h3>The SIR Model 🦠</h3>
The **SIR** model <a id="ref-1"></a>[\[1\]](#bib-1) is one of the most simple **Compartmental models** these model are used to see how populations move between diffrent states, in this case representing **S**usceptible, **I**nfectious, and **R**ecovered individuals. 

**S**usceptible: This group represent the people that have not caught or recoved from the disease, this group does not grow inside this model due to the assumtion the recovered group cannot catch the disease again

**I**nfectious: This group reprisents the people that are curently infected with the disease. This group in this model has the most comlex behavour due to this group both growing due to the infection and decreasing from people recovering from the disease.

**R**ecovered: This group are **"Recovered"** from the disease, you may notice the quotation marks, this is due to this group not being seperated in to dead and non dead catagories, this may throw off the model slightly but for low mortyality to recovered ratio of sars type viruses it is reletivly safe to assume the recovered group would dominate.

The recovered group brings up a question in this covid modeling, does the recovered domination assumption hold, because we had to lock down is it must of had a high mortality right? John Hopkins<a id="ref-2"></a>[\[2\]](#bib-2) give a case mortality ratio around 1-2 %, this measure would morelikly give an overestermate than an underestermate due to relying on confermed cases, people are more likly to have not got tested due to the how contagus the disease was, even if this overestermated number was close to the truth it is still dominated by people recovering meaning the assumtion holds. 







<h3 class="major"> Bibliography</h3>
<a id="bib-1"></a>[1] Hethcote, Herbert W, "The Mathematics of Infectious Diseases", SIAM Review, Volume 42, 4, pages 599-653, 2000, <a href="https://doi.org/10.1137/S0036144500371907" target="_blank" rel="noopener noreferrer">https://doi.org/10.1137/S0036144500371907</a>. <a href="#ref-1">↩</a>

<a id="bib-2"></a>[2] Johns Hopkins University, "Mortality Analyses", Johns Hopkins Coronavirus Resource Center, n.d. <a href="https://coronavirus.jhu.edu/data/mortality" target="_blank" rel="noopener noreferrer">https://coronavirus.jhu.edu/data/mortality</a>. <a href="#ref-2">↩</a>

<a id="bib-3"></a>[3] Tiberiu Harko, Francisco S.N. Lobo, M.K. Mak, "Exact analytical solutions of the Susceptible-Infected-Recovered (SIR) epidemic model and of the SIR mode", Applied Mathematics and Computation, Volume 236, Pages 184-194, ISSN 0096-3003, 2014 <a href="https://doi.org/10.1016/j.amc.2014.03.030" target="_blank" rel="noopener noreferrer">https://doi.org/10.1016/j.amc.2014.03.030</a>. <a href="#ref-3">↩</a>