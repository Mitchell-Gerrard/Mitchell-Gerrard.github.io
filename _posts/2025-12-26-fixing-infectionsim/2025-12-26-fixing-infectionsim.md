---
layout: post
title: "Fixing infectionsim: Fixing my poor university code! 🔨"
date: 2025-12-26
tagline: Fixing the poor layout and lack comments of my SIR modeling using randomly moving particles
image: /images/post_images/2025_12_25_fixing_infectionsim/sir_fit_results.png
category: Fixing Old Code
---
<h3 class="major">Introduction 👋</h3>

This was the **second** Python project I ever worked on, the first being a Connect 4 game that went off the rails. That one will be fun to fix, but for now, we are working on a project I made due to a change of plans. This was the year 2020. I was in the first year of my degree, and in March, COVID-19 struck the UK with its ability to shut down the entire globe; it sent us home from University, throwing us into disarray. We have experimental modules we have to complete, and we have been handing our work in person. What do we do? What work can we do? How do we get our lectures ahhh 😰!!

The university system was revolutionised; they recorded lectures at home, then enabled handing in PDFs on Blackboard. This made things easier, but there is still the question of how we are going to do the experimental module. It was data analysis. I cannot recall the other options for this miniproject, but having the opportunity to be able to apply skills to a current event was something I couldn’t pass up I was already tracking the growth of COVID using data from Johns Hopkins, so this came together nicely. So I looked in to the SIR model and I was hooked how a random distrubion can cause somthing so real to happen, this happens with other things of course we get patterns that occur from randomness all the time look at ratiation, but this griped me so I figured out how to implement the random walks and how to change the infection distribution, I could even compare it to covid. But it sort of moved from the project, so the write-up didn’t end up the way I wanted it to be, so here is a way I can do it the way I want to. And make the code better.

So in the next section, I will lay out how the simulation works and the background of the SIR, then in the section after, I will talk about the good parts of my simulation. Following that, I will critique my old code about how it is set up, improvements that can be made, and the lack of comments, then I will correct the simulation. After that, talk through some consequences of the simulation, adding comparisons to COVID-19, and finally, I will conclude the exercise by talking about whether it was worth rewriting all of these years later, and my development since that time in my life.

<h3 class="major">Numerical Model 🖥</h3>

This section will focus on the **SIR** model used by biologists for disease spread, and how this model comes out of random interactions between individuals and ways we can alter the model to line up with the real world.

<h3>The SIR Model 🦠</h3>

The **SIR** model <a id="ref-1"></a>[\[1\]](#bib-1) is one of the most simple **Compartmental models**. These models are used to see how populations move between different states, in this case representing **S**usceptible, **I**nfectious, and **R**ecovered individuals.

Susceptible: This group represent the people who have not caught or recovered from the disease. This group does not grow inside this model due to the assumption that the recovered group cannot catch the disease again.

Infectious: This group represents the people who are currently infected with the disease. This group in this model has the most complex behaviour, as both growing due to the infection and decreasing due to people recovering from the disease.

Recovered: This group are “Recovered” from the disease. You may notice the quotation marks; this is due to this group not being separated into dead and non-dead categories. This may throw off the model slightly, but for a low mortality-to-recovery ratio of SARS-type viruses, it is relatively safe to assume the recovered group would dominate.

The recovered group brings up a question in this COVID modelling: Does the recovered domination assumption hold? Because we had to lock down, it must have had a high mortality, right? Johns Hopkins[2] gives a case mortality ratio of around 1-2 %. This measure would more likely give an overestimate than an underestimation due to relying on confirmed cases; people are more likely to have not been tested due to how contagious the disease is. Even if this overestimated number was close to the “truth”, it is still dominated by people recovering, meaning the assumption holds.

Just existing in these groups does not a model make, we require the way these interact. How do they do it? Well, differential equations of course! These differential equations were solved by Harko et. al [3] using the Abel solution, but those solutions are not needed in this post; the differential equations are defined as:

$$
\begin{aligned}
\frac{dS}{dt} &= -\beta \frac{SI}{N} \
\frac{dI}{dt} &= \beta \frac{SI}{N} - \gamma I \
\frac{dR}{dt} &= \gamma I
\end{aligned}
$$

Where N is the total number in all populations, $\beta$ is the infection rate, and $\gamma$ is the recovery rate. This post will only numerically solve these differential equations.

<h3>Particle Simulation</h3>

This particle simulation is relatively simple. There is random movement, any random space between -50 and 50 😕, then checks whether it is in a radius of another. If either is infected, there is a chance that they will get infected, then it goes and goes again, yeah, I hate this too 😡. I will improve this, but you'll have to wait for my criticisms to find out how.

<h3 class="major">What this guy did well?🥳</h3>

This Guy man he managed to see what was going on from some form of randomness. This was a great idea to show how mathematical equations can describe the real world. There are many applications of these forms of randomness and particle simulations. Such as being able to show ensemble behaviour like this and the standard gas equations.

This approach also allows for outbreak dynamics, showing how the growth of the disease can depend on both the way people act and the spreadability of the disease.

I also made some of it parametrisable, meaning we can explore transmissibility and people dynamics by altering different things to improve the dynamics and give advice on how we can reduce the spread.

The visualisations using GIF are awesome, seeing the development of the infection and people interacting, spreading the disease, showing how important it was to lockdown.

The multiprocessing to be able to run multiple simulations fast is nice, so we can normalise the randomness and run multiple parameters quickly.

<h3 class="major">What this guy did badly?😭</h3>

I use unrealistic sizes in the random walk, like $\pm 50$ 😲 massive, it may have been to speed up the simulation.

This brings us to the second issue, the time scale, like omg so arbitrary and just the steps of the gif, by running the simulation in each step of the gif means it takes longer than I need to run the simulations and doesn't allow for further analysis of the data.

The deterministic recovery rate doesn't allow for changing it, trying to model the real-life disease. As well as the infection roll, it does not describe the infection rate well.

The inefficient search algorithm is like super slow, and one place that can be massively improved, allowing for convergence testing of the randomness.

Not to mention the README sucks 🤢, like just going, you can only run it in a command prompt, like bro explain the code omg.

<h3 class="major">What the code is now?😎</h3>

Let's start with the movement of these particles. They now have a velocity distribution that is picked from there; they pick a direction to be able to go, making this random walking feel more realistic to humans (they are always unpredictable when walking, meaning I have to make fast decisions 😡). With them averageing $1.4\,\frac{m}{s}$ (average walking speed in rail transit, a place where disese spreads <a id="ref-4"></a>[\[4\]](#bib-4)) we have somthing scalable with time if needed and we are able to get realistic numbers from the simulation.

Now, how do they get infected? We have added the ability to have a functional probability of getting infected, and a cutoff for when the probability is effectively 0. This is useful to test things like social distancing, comparing with real-world data to see what effective distance may be. Like with COVID being given a $2,m$ social distancing [5].

What about recovery, humans don't all recover on the 14th day of the illness, they recover on a distribution that fits with real life and the assumption of the SIR model. This can be adjusted to show what the possible recovery could be.

Now, in the previous model, it took like 10 minutes to run 500 steps of the model 😢, this was mostly due to the inefficient way of performing the nearest neighbours search. Now we are using the scipy module KDTree. A KD-tree is a binary space‑partitioning structure that alternates splitting the point set along coordinate axes so each node partitions its subset into two axis‑aligned regions; this nested partitioning limits nearest‑neighbour queries to only nearby regions, greatly speeding up searches [6]. This means I can run 15 seconds instead of running for half a day per step. This then raises issues of using KDTrees too often, so we implement a contact interval to speed up the simulation while also keeping realistic interactions. This now runs in 2 minutes for significantly more data and steps, allowing for a more precise and interesting simulation.

This increased number of steps we perform means we need to change how we log the data otherwise we get massive amounts. We have a records_interval giving how many times per day we record the infections, giving a way of memory management, especially if you want to run many simulations.

That is the particle simulation done, but now we need to understand the output. So I built a fitting function for the SIR model. fiting_SIR takes time series S, I, R and a sampling interval dt, computes total population N and a time vector, then forms finite-difference estimates for per-step changes (ΔS, ΔR). It uses mid-step values and a mask (requires sufficiently large counts) to compute noisy per-step $\gamma \approx \frac{\Delta R}{I\cdot dt}$ and $\beta\approx \frac{-\Delta S \cdot N}{S\cdot I\cdot dt}$, then reduces those to robust initial guesses (gamma_med, beta_med) with safe fallbacks if the estimates are invalid.

Using those initials, the function defines the standard SIR ODEs and a simulation that integrates them with odeint for any (beta,gamma). Residuals are built from the model vs. data infected curve (normalised by N), and least_squares (bounded, soft‑L1 loss) refines the parameters. The function returns the fitted beta_fit/gamma_fit, the model trajectories (S_fit,I_fit,R_fit), initial medians, time/N arrays, and the optimiser result; note it assumes homogeneous mixing, fits only I, and depends on the valid threshold and correct dt. In the next section, we will discuss the results it outputs and how well the SIR model emulates the particle infection simulation.

I then have a multirun simulation that looks at the randomness and generates a median gamma and beta to simulate, to see if multiple separate populations should be used to find the information. This can be like using multiple countries to find the spread and recovery.

<h3 class="major">The Simulation, The Fit and The Pictures</h3>

Now, here it is, the moment we have all been waiting for: the GIF.

<p style="text-align:center">

  <img src="/images/post_images/2025_12_25_fixing_infectionsim/infection_simulation.gif" alt="Infection simulation GIF" style="max-width:100%;height:auto;">

</p>

The left panel (scatter) shows individual particles moving in a toroidal 2D area: blue points = susceptible, red = currently infected, green = recovered. Particles perform a random-walk displacement each simulation step, and positions wrap at the edges. When two particles come within the transmission radius (checked at regular contact-check ticks), an infection may occur according to the model's distance→probability function; recovered particles are permanently moved to the recovered state. The small text shows simulated time.

The right panel plots the aggregated counts over time: susceptible (blue), infected (red), recovered (green). Watch how spikes or plateaus in the red curve correspond to visible local outbreaks on the map. Explain that infection events are stochastic (random), contact checks happen at a configurable cadence, and recovery is modelled probabilistically with a recovery-time parameter — so single runs show one possible epidemic trajectory, not a deterministic prediction. Mention key parameters (population size, transmission radius/probability, contact interval, dt, recovery_time) and caveats (spatial homogeneity assumptions, sampling/recording cadence can miss brief contacts, and fitted SIR parameters are an approximate, population-level summary).

<p style="text-align:center">

  <img src="/images/post_images/2025_12_25_fixing_infectionsim/sir_fit_results.png" alt="Infection simultaion with fitting" style="max-width:100%;height:auto;">

</p>

In this plot, I see we get a good fit from the SIR model to the data, but the peak of the infections seems to be slightly off. We get the general pattern, and looking at the gamma factor, we get that we have an average recovery time of 14.93 days, which is 1 day longer than the simulation parameters implemented. This is due to the randomness in the simulation, which may show the importance of using multiple populations to get a better approximation.

<p style="text-align:center">

  <img src="/images/post_images/2025_12_25_fixing_infectionsim/multi_run_sir_fit.png" alt="Infection simultaion with fitting for multiple runs" style="max-width:100%;height:auto;">

</p>

Now, in this multirun plot, we see the necessity of these runs. I see there is a spread in the curves caused by the likelihood of people interacting, but they are similar, so possibly taking the median of these simulations gives us a good result.

Now look at the gamma factor, which gives us a recovery time of 14 days to within 2 decimal places. This shows that populations can mislead scientists due to the random ways they move, but with multiple sample populations, we can get a more precise result.

<h3 class="major">Conclusions</h3>

The rewrites make the simulation both more realistic and more usable: particles now move with velocity-based steps rather than arbitrarily large jumps, infections and recoveries are sampled from probabilistic rules instead of fixed deterministic times, and recording is throttled so long runs produce manageable data. These changes preserve the visual intuition of the original demo while producing time-scaled, analysable output that can be compared directly to population-level SIR estimates.

Replacing the brute-force neighbour search with a KD-tree [6] and adding a contact-interval strategy dramatically improves runtime: what used to take many minutes or hours per step can now be run interactively for many more steps and parameter combinations. That performance gain makes systematic exploration practical (parameter sweeps, sensitivity testing and multirun ensembles) rather than a single ad-hoc demo. Fitting a homogeneous SIR model to the simulated infected curve provides useful summary estimates of β and γ, but single-run fits are noisy because events are stochastic and the simulation violates homogeneous-mixing assumptions; aggregating repeated independent runs (reporting medians and spread) yields more robust parameter estimates.

Important limitations remain. The simulation uses a toroidal 2D domain, a simple distance→probability transmission kernel, and no demographic or behavioural heterogeneity; the SIR fit assumes homogeneous mixing and only summarises I(t). Recording cadence and KD-tree refresh intervals introduce sampling and approximation trade-offs that can bias short-timescale events. Treat the model as a controllable experiment rather than a literal representation of real outbreaks. Practical next steps are already clear: keep the README improvements, run systematic multirun experiments and report medians/intervals, replace large GIFs with compressed video for the web, and consider adding heterogeneity or a richer compartmental structure if you need fitted parameters to reflect specific real‑world mechanisms.



<h3 class="major"> Bibliography</h3>
<a id="bib-1"></a>[1] Hethcote, Herbert W, "The Mathematics of Infectious Diseases", SIAM Review, Volume 42, 4, pages 599-653, 2000, <a href="https://doi.org/10.1137/S0036144500371907" target="_blank" rel="noopener noreferrer">https://doi.org/10.1137/S0036144500371907</a>. <a href="#ref-1">↩</a>


<a id="bib-2"></a>[2] Johns Hopkins University, "Mortality Analyses", Johns Hopkins Coronavirus Resource Center, n.d. <a href="https://coronavirus.jhu.edu/data/mortality" target="_blank" rel="noopener noreferrer">https://coronavirus.jhu.edu/data/mortality</a>. <a href="#ref-2">↩</a>


<a id="bib-3"></a>[3] Tiberiu Harko, Francisco S.N. Lobo, M.K. Mak, "Exact analytical solutions of the Susceptible-Infected-Recovered (SIR) epidemic model and of the SIR mode", Applied Mathematics and Computation, Volume 236, Pages 184-194, ISSN 0096-3003, 2014 <a href="https://doi.org/10.1016/j.amc.2014.03.030" target="_blank" rel="noopener noreferrer">https://doi.org/10.1016/j.amc.2014.03.030</a>. <a href="#ref-3">↩</a>

<a id="bib-4"></a>[4] Kasehyani, N. H. . (2019). Evaluation of Pedestrian Walking Speed in Rail Transit Terminal. International Journal of Integrated Engineering, 11(9), 026-036. <a href="https://publisher.uthm.edu.my/ojs/index.php/ijie/article/view/5425" target="_blank" rel="noopener noreferrer">https://publisher.uthm.edu.my/ojs/index.php/ijie/article/view/5425</a> <a href="#ref-4">↩</a>

<a id="bib-5"></a>[5] Qureshi, Z., Jones, N., Temple, R., Larwood, J. P. J., Greenhalgh, T., & Bourouiba, L. (2020). What is the evidence to support the 2 metre social distancing rule to reduce COVID-19 transmission? Centre for Evidence-Based Medicine. <a href="https://www.cebm.net/covid-19/what-is-the-evidence-to-support-the-2-metre-social-distancing-rule-to-reduce-covid-19-transmission/" target="_blank" rel="noopener noreferrer">https://www.cebm.net/covid-19/what-is-the-evidence-to-support-the-2-metre-social-distancing-rule-to-reduce-covid-19-transmission/</a> <a href="#ref-5">↩</a>

<a id="bib-6"></a>[6] Maneewongvatana, S., & Mount, D. M. (1999). Analysis of approximate nearest neighbor searching with clustered point sets. arXiv. <a href="https://doi.org/10.48550/arXiv.cs/9901013" target="_blank" rel="noopener noreferrer">https://doi.org/10.48550/arXiv.cs/9901013</a> <a href="#ref-6">↩</a>

