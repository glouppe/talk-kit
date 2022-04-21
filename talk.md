class: middle, center, title-slide
count: false

# Simulation-based inference:<br> Proceed with caution!

Likelihood-free in Paris<br>
April 21, 2022

<br>

Gilles Louppe<br>
[g.louppe@uliege.be](mailto:g.louppe@uliege.be)

---

class: middle, center

.grid[
.kol-1-5.center[.width-100[![](figures/faces/kyle.png)] Kyle Cranmer]
.kol-1-5.center[.width-100[![](figures/faces/johann.png)] Johann Brehmer]
.kol-1-5.center[.width-100.circle[![](figures/faces/kagan.jpg)] Michael Kagan]
.kol-1-5.center[.width-100[![](figures/faces/joeri.png)] Joeri Hermans]
.kol-1-5.center[.width-90[![](figures/faces/antoine.png)] Antoine Wehenkel]
]

.grid[
.kol-1-5.center[.width-100.circle[![](figures/faces/norman.jpg)] Norman Marlier]
.kol-1-5.center[.width-100.circle[![](figures/faces/arnaud.jpg)] Arnaud Delaunoy]
.kol-1-5.center[.width-100.circle[![](figures/faces/maxime.jpeg)] Maxime Vandegar]
.kol-1-5.center[.width-100.circle[![](figures/faces/malavika.jpg)] Malavika Vasist]
.kol-1-5.center[.width-90.circle[![](figures/faces/francois.jpg)] Francois Rozet]
]

---

class: middle

.center.width-100[![](./figures/simulators.png)]

---

class: middle

.center.width-80[![](./figures/prediction.png)]

.center.width-70[![](./figures/unconditioned-program.png)]

.center[$$\theta, z, x \sim p(\theta, z, x)$$]

---

class: middle, center

.center.width-80[![](./figures/inference.png)]

.center.width-70[![](./figures/conditioned-program.png)]

.red.bold[Warning:] The likelihood
$p(x | \theta) = \int p(x, z| \theta) dz$
is intractable.

---

class: middle
count: false

# Simulation-based inference 

---

class: middle

Start with
- a simulator that can generate $N$ samples $x\_i \sim p(x\_i|\theta\_i)$,
- a prior model $p(\theta)$,
- observed data $x\_\text{obs} \sim p(x\_\text{obs} | \theta\_\text{true})$.

Then, estimate the posterior $$p(\theta|x\_\text{obs}) = \frac{p(x\_\text{obs} | \theta)p(\theta)}{p(x\_\text{obs})}$$

.center.width-35[![](./figures/posterior.png)]

---

class: middle

.avatars[![](figures/faces/kyle.png)![](figures/faces/johann.png)]

.center.width-100[![](./figures/frontiers-sbi0.png)]

.footnote[Credits: [Cranmer, Brehmer and Louppe](https://doi.org/10.1073/pnas.1912789117), 2020.]

---

class: middle
count: false

.avatars[![](figures/faces/kyle.png)![](figures/faces/johann.png)]

.center.width-100[![](./figures/frontiers-sbi2.png)]

.footnote[Credits: [Cranmer, Brehmer and Louppe](https://doi.org/10.1073/pnas.1912789117), 2020.]

---

class: middle
count: false

.avatars[![](figures/faces/kyle.png)![](figures/faces/johann.png)]

.center.width-100[![](./figures/frontiers-sbi.png)]

.footnote[Credits: [Cranmer, Brehmer and Louppe](https://doi.org/10.1073/pnas.1912789117), 2020.]

---

class: middle

.avatars[![](figures/faces/kyle.png)![](figures/faces/johann.png)]

.width-100[![](./figures/inference-algorithms.png)]

.footnote[Credits: [Cranmer, Brehmer and Louppe](https://doi.org/10.1073/pnas.1912789117), 2020.]

---

class: middle

.avatars[![](figures/faces/kyle.png)![](figures/faces/joeri.png)![](figures/faces/johann.png)]

## Neural ratio estimation (NRE)

The likelihood-to-evidence $r(x|\theta) = \frac{p(x|\theta)}{p(x)} = \frac{p(x, \theta)}{p(x)p(\theta)}$ ratio can be learned, even if neither the likelihood nor the evidence can be evaluated:

.grid[
.kol-1-4.center[

<br>

$x,\theta \sim p(x,\theta)$

<br><br><br><br>

$x,\theta \sim p(x)p(\theta)$

]
.kol-5-8[<br>.center.width-70[![](./figures/classification-2.png)]]
.kol-1-8[<br><br><br><br>

$\hat{r}(x|\theta)$]
]

???

Interesting observations:
- MI(x,theta) = E_p(x,theta) log r(x,theta)
- The likelihood ratio is a sufficient statistic. 

---

class: middle

$$p(\theta|x) = \frac{p(x|\theta) p(\theta)}{p(x)} \approx \hat{r}(x|\theta) p(\theta)$$

<br>

.center.width-100[![](./figures/carl.png)]

---

class: middle 

# ... but proceed with caution!

aka model checking, evaluation, and criticism.

---

class: middle
count: false

.grid[
.kol-1-2[<br>
.center[![](./figures/latent1.svg)]]
.kol-1-2[]
]

---

class: middle
count: false

.grid[
.kol-1-2[<br>
.center[![](./figures/latent1.svg)]]
.kol-1-2[
  
.center[<video controls autoplay loop muted preload="auto" height="300" width="400">
  <source src="./figures/galton.mp4" type="video/mp4">
</video>]

]
]

---

class: middle
count: false

.grid[
.kol-1-2[.center[![](./figures/latent2.svg)]]
.kol-1-2[Prior model $p(\theta)$
<br><br><br><br>
Observational model $p(x|\theta)$]
]

???

This results in the Bayesian joint distribution $p(x, \theta)$.

---

class: middle, black-slide

.center.circle.width-50[![](./figures/georgebox.jpg)]

.center["All models are wrong, but some are useful" - George Box]

---

class: middle

## The observational model $p(x | \theta)$

$p(x | \theta)$ should capture the pertinent structure of the true data generating process for the inference results to be useful.

A model that does not capture every precise detail of the true data generating process can still be useful if it captures the details relevant to the particular analysis goals. 

.footnote[Credits: [Michael Betancourt](https://betanalpha.github.io/assets/case_studies/principled_bayesian_workflow.html#14_Model_Adequacy), 2020.]

???

Put as much physics in the model as you can!

The richness of the model can be probed with prior and posterior predictive checks.

---

class: middle

The observational model can often be made richer by including in it additional .bold[nuisance parameters] $\nu$ that capture known unknowns. 

In this case, the likelihood becomes $$p(x | \theta) = \int p(x | \theta, \nu) p(\nu|\theta) d\nu.$$

Although nuisance parameters can reduce model misspecification, their presence and marginalization will result in increased uncertainties for the parameters $\theta$ of interest.

---

class: middle

.avatars[![](figures/faces/norman.jpg)]

.center[

.width-70[![](figures/norman-setup.jpeg)]

<video controls preload="auto" height="225" width="500" autoplay loop>
  <source src="https://video.twimg.com/ext_tw_video/1445251463261872128/pu/vid/478x270/pVbx4507NgMLv3tn.mp4" type="video/mp4">
</video>

]

Nuisance parameters are used to model known unknowns in a robotic setup (e.g., camera position, table position, etc).

.footnote[Credits: [Marlier et al](https://arxiv.org/abs/2109.14275), 2021.]

---

class: middle

## The prior model $p(\theta)$

The prior model $p(\theta)$ specifies one's beliefs about the model parameters. It should reflect domain expertise.

---

class: middle

The consequences of the prior model in the context of the observational model can be diagnosed with .bold[prior predictive checks] to evaluate what data sets would be consistent with the prior. 

A prior predictive check generates data $x^\text{sim}$ according to the prior predictive distribution $p(x)$ as 
$$\begin{aligned}
\theta^\text{sim} \sim p(\theta)\\\\
x^\text{sim} \sim p(x | \theta^\text{sim}),
\end{aligned}$$
or summary statistics $T(x^\text{sim})$ thereof.

---

class: middle

.width-100[![](figures/prior-check.png)]

.footnote[Credits: [Gabry et al](https://arxiv.org/abs/1709.01449), 2017.]

---

class: middle

.avatars[![](figures/faces/maxime.jpeg)![](figures/faces/antoine.png)![](figures/faces/kagan.jpg)]

In the absence of a good prior, .bold[neural empirical Bayes] can be used to estimate a prior distribution $p\_\phi(\theta)$ by maximizing the (log) evidence of a set of observations
$$\log p\_\phi(\\{x\_i\\}\_{i=1}^N) = \sum\_{i=1}^N \log \int p(x\_i | \theta) p\_\phi(\theta) d\theta.$$

.footnote[Credits: [Vandegar et al](https://arxiv.org/abs/2011.05836), 2021.]

---

class: middle

.avatars[![](figures/faces/maxime.jpeg)![](figures/faces/antoine.png)![](figures/faces/kagan.jpg)]

.center.width-60[![](figures/neb.png)]

.footnote[Credits: [Vandegar et al](https://arxiv.org/abs/2011.05836), 2021.]

---

class: middle

.grid[
.kol-1-2[

## Computational faithfulness

$$\hat{p}(\theta|x) = \text{sbi}(p(x | \theta), p(\theta), x)$$

We must make sure our approximate simulation-based inference algorithms can (at least) actually realize faithful inferences on the observations we expect a priori -- i.e. those $x^\text{sim} \sim p(x)$.

]
.kol-1-2[.center.width-100[![](figures/corner-exoplanet.png)]]
]

---

class: middle

.avatars[![](figures/faces/kyle.png)![](figures/faces/johann.png)![](figures/faces/siddarth.png)![](figures/faces/joeri.png)]

.italic[Mode convergence:]

The maximum a posteriori estimate converges towards the nominal value $\theta^\*$ for an increasing number of independent and identically distributed observables $x\_i \sim p(x|\theta^\*)$:
$$\begin{aligned}
&\lim\_{N \to \infty} \arg\max\_\theta p(\theta | \\{ x\_i \\}\_{i=1}^N) \\\\
=& \lim\_{N \to \infty} \arg\max\_\theta p(\theta) \prod\_{x\_i} r(x\_i | \theta) = \theta^\*
\end{aligned}$$

.center.width-100[![](figures/dm-posterior.gif)]

.footnote[Credits: [Brhemer et al](https://iopscience.iop.org/article/10.3847/1538-4357/ab4c41/meta), 2019.]

---

class: middle

.avatars[![](figures/faces/joeri.png)![](figures/faces/arnaud.jpg)![](figures/faces/francois.jpg)![](figures/faces/antoine.png)]

A common observation at the root of several other diagnostics is to check for the .bold[self-consistency] of the Bayesian joint distribution,
$$p(\theta) = \int p(\theta') p(x|\theta') p(\theta|x) d\theta'\, dx.$$

.grid[
.kol-2-3[
.italic[Coverage diagnostic:]
- For $x,\theta \sim p(x,\theta)$, compute the $1-\alpha$ credible interval based on $\hat{p}(\theta|x)$.
- If the fraction of samples for which $\theta$ is contained within the interval is larger than the nominal coverage probability $1-\alpha$, then the approximate posterior $\hat{p}(\theta|x)$ has coverage.]
.kol-1-3[.center.width-100[![](./figures/coverage.png)]]
]

.footnote[Credits: [Hermans et al](https://arxiv.org/abs/2110.06581), 2021; [Siddharth Mishra-Sharma](https://arxiv.org/abs/2110.01620), 2021.]

---

class: middle

.avatars[![](figures/faces/joeri.png)![](figures/faces/arnaud.jpg)![](figures/faces/francois.jpg)![](figures/faces/antoine.png)]

<br>

.center.width-90[![](figures/coverage-crisis.png)]

.footnote[Credits: [Hermans et al](https://arxiv.org/abs/2110.06581), 2021.]

---

class: middle

Faithfulness diagnostics require the ability to repeatedly compute $\hat{p}(\theta|x)$, which is immediate for amortized approaches but .bold[computationally very heavy for sequential inference algorithms].

---

class: middle

What if the diagnostic fails?

???

- Use more data
- Use better NN architectures
- Use an ensemble

---

class: middle

.avatars[![](figures/faces/joeri.png)![](figures/faces/arnaud.jpg)![](figures/faces/francois.jpg)![](figures/faces/antoine.png)]

Neural ratio estimation can be forced to be more .bold[conservative], hence increasing the reliability of the approximate posteriors and reducing the risk of false inferences.

.center.width-100[![](figures/cnre.png)]

---

class: middle

## Posterior predictive checks

If a model is a good fit, then we should be able to use it to generate data that resemble the data we observe.

Formally, this can be diagnosed with posterior predictive checks that generates data $x^\text{sim}$ according to the posterior predictive distribution $$p(x^\text{sim}|x) = \int p(x^\text{sim}|\theta) p(\theta|x) d\theta,$$
or summary statistics $T(x^\text{sim})$ thereof. 

---

class: middle

.width-100[![](figures/ppc.png)]

.footnote[Credits: [Gabry et al](https://arxiv.org/abs/1709.01449), 2017.]

---

class: middle

.avatars[![](figures/georgebox.jpg)]

## Box's loop: build, compute, critique, repeat

.center.width-90[![](figures/box-loop.png)]

Science does not end at the inference results. Instead, they should inform the next revision of the model.

.footnote[Credits: [Blei](https://www.annualreviews.org/doi/full/10.1146/annurev-statistics-022513-115657), 2014.]

---

# Proceed with caution!

<br>

.question[Simulation-based inference is a major evolution in the statistical capabilities for science, enabled by advances in machine learning.]

.alert[Need to reliably and efficiently assess the adequacy of the full Bayesian model.]

.alert[Need to reliably and efficiently evaluate the quality of the posterior approximations.]

.alert[Need to efficiently generate simulated data and use it to train ML components.]
  
---

class: end-slide, center
count: false

The end.
