# Task fMRI analysis



---
### Generalities on task fMRI analysis.

**fMRI paradigm for driving the cerebral activity**

In order to study one or more particular brain functions, the patient will be subjected in the MRI to specific cognitive tasks via an experimental paradigm specifically designed to stimulate the areas concerned. A paradigm consists of a temporal description of the cognitive tasks: time of appearance, duration, order during the examination.

There are three main types of experimental paradigms in fMRI:
* the event-driven paradigm, which consists of short tasks (clicking on a mouse, saying a word, solving an equation),
* the block paradigm, where the blocks consist of either a repetition of short events or a long event (watching a video, listening to music),
* and finally the resting state where the patient remains without doing any cognitive task.

For the first two paradigms we will speak of task fMRI and for the last one of resting fMRI.

```{figure} /images/dessins.png
---
height: 300px
name: hémo
---
Example of paradigms with two conditions in red and blue: block paradigm (a), a slow paradigm where the events are spaced in time (b), fast paradigm where the events follow each other at regular intervals (c), fast random paradigm where the events appear at random times (d).
```


When the brain is subjected to a very brief stimulus, in the BOLD signal there is a delay before the response which also has a certain duration. This can be modelled in signal processing as the impulse response of the BOLD signal, which is called the hemodynamic response. This hemodynamic response models the local increase in blood flow to support the oxygen requirement of active neurons. We have a continuous model of this impulse response, but in practice, since the temporal acquisitions are sampled every second or every two seconds, we do not have access to the entire hemodynamic response, only the red points on the following image:
```{figure} /images/hemo.png
---
height: 300px
name: hémo
---
Continuous model of hemodynamic response and its sampling every 2 seconds.
```

With the block paradigm, the hemodynamic response to all successive stimuli within a block accumulates and gives high amplitude signal. The patient is only at rest between two blocks in order to let the BOLD signal fall back to its baseline. Note that, the duration of the blocks does not interfere with the statistical analysis of the data, so the duration of the blocks can be adapted to the type of cognitive task to be performed.

The block paradigm seems ideal for analysing fMRI signals, but it has a cognitive limitation: if the patient does the same task over a long period of time, he or she gets used to and there is a risk of drowsiness. Another problem is that some cerebral functions cannot be handled by the block paradigm : this is the case of all short dynamic phenomena in the brain.

The flexibility and the randomness of event paradigm allow to eliminate the predictability of block design. It also removes the anticipatory effect thanks to the randomness, and the different conditions are sorted after the scan session, during the analysis.

Task fMRI data can be analyzed in different ways depending on the chosen paradigm and the a priori knowledge.


**Inferential methods:**
* require *a priori* model of brain activation (expected response to some tasks of the paradigm)
* compare the data to the model,
* examples: T-test, correlation, generalized linear model (GLM).

**Exploratory methods:** (not covered in this course)
* do not require *a priori* assumption,
* determination of a model from the data,
* can be used to validate a model,
* can be used to detect unpredicted changes,
* can be used to identify no-interesting components
* examples: PCA, ICA, classification, bayesian analysis.





### Statistical test analysis for task fMRI


We consider the case where the paradigm is known, it is thus possible to search for brain regions having a synchronous activity with one or more cognitive tasks imposed by the paradigm. However, it must be remembered that the signals are extremely noisy and that the variation of the signal induced by the task is very small compared to the noise. It should also be remembered that other factors can impact the fMRI signal (head movement, physiological signals, sleepiness, etc.)


Consider the following toy example: a patient is placed in the MRI and performs cognitive tasks of two different types that will be called condition A and condition B. We then try to find out which areas of the brain are activated:
* during condition A only,
* during condition B only,
* during both conditions,
* during neither of the two conditions.

Periods of rest between two blocks allows the fMRI signal to return to the baseline.


```{figure} /images/bloc1.png
---
height: 450px
name: bloc1
---
Example of fMRI signal recorded during a two-condition block paradigm session in a voxel of the brain volume showing an activation for both conditions.
```

Suppose we want to find the areas of the brain that are more strongly activated during condition A than during condition B.

For each voxel of the brain, we will conduct a test as follows:
* we consider two populations:
	*  population 1: the signal samples corresponding to the periods when condition A is realized;
	*  population 2: the signal samples corresponding to the periods when condition B is realized.
* we want to know if on average the signal is significantly higher during condition A compared to condition B.

To conduct a statistical test, the null hypothesis $\mathcal{H}_0$ must be formulated as the assumption of no difference between the two populations. In this case, we assume that the alternative hypothesis $\mathcal{H}_1$ is such that the mean of population 1 should be higher than the mean of population 2. We will therefore conduct a right-tailed test.

````{margin}
```{note}
For the last two raws, note that a test on variances of the two populations, $\sigma_1^2$ and $\sigma_2^2$, must be conducted to chose the correct statistics of test.
```
````
```{figure} /images/table_tests.pdf
---
height: 475px
name: tab_tests
---
Test on the means of two populations according to the characteristics of the populations.
```

Once the test statistic is chosen according to the data and the knowledge of the problem, we have to decide if the test allows to reject or not the null hypothesis (i.e. to decide that the two populations are not identical, i.e. that the considered voxel does not react in the same way to the two conditions of the paradigm). Most of the time we will make the approximation that the noise in the fMRI signal is Gaussian. The theoretical variance of this noise is not known, so we will choose a T-test among the last 3 lines of the table.

The probability, under $\mathcal{H}_0$, of obtaining a value $T$ greater than or equal to the observed value $T_{obs}$ defines the *p-value*. The hypothesis $\mathcal{H}_0$ is rejected if the p-value is below a given significance level $\alpha$.


```{figure} /images/pval.pdf
---
height: 300px
name: pval
---
Illustration of p-value calculation, $p(T|\mathcal{H}_0)$ is the test statistic distribution under the null hypothesis, $\alpha$ is the significance level to which the p-value is compared.
```

This procedure allows to thresh independantly each voxel of the brain, but, in this context, it is obvious that two adjacent voxels have a high probability of having a similar activity. Moreover, the different preprocessing performed on the data introduces dependency between the neighboring voxels. If $\alpha$ is the probability of false alarm, i.e. the probability of wrongly rejecting the hypothesis $\mathcal{H}_0$, then by thresholding independently the $n$ voxels of the brain we obtain on average, $\alpha\times n$ false alarms. Now imagine that among all the voxels in the brain, only a small number $n_1$ react differently to the two conditions A and B of the paradigm. We thus have $n_1 << n_0$. Let us also assume that our test is powerful, i.e. that it is able to reject $\mathcal{H}_0$ for the $n_1$ voxels that react more strongly to condition B than to condition A. In the end, after thresholding each voxel independently we will have detected :
* the $n_1$ voxels,
* the $a = \alpha\times n$ false alarms.
It is possible, when $n$ is very large, that $\alpha\times n$ is larger than $n_1$, so we would have, in total in the detected areas, more false detections than true detections.

The configuration after decinding for each voxel to reject or not the null hypothesis is summarized in the table below.



```{figure} /images/tab_test.png
---
height: 200px
```

````{margin}
```{note}
The description of the different thresholding strategies used in the literature is not the object of this course, but it is important to remember the problems generated by an individual thresholding of a large number of tests, whatever the application domain.
```
````
Thresholding each voxel separately is generally a bad idea, it can be done at first to get an idea of the areas of interest but we understand with the example given previously that it can lead to a bad interpretation. Multiple thresholding strategies are proposed in the literature in order to have a more global control on the detection errors. Instead of thresholding according to the false alarm criterion, criteria such as the FDR (*False Discovery Rate*) or FWER (*Family Wise Error Rate*) are preferred.


```{figure} /images/FDR_FWER.png
---
height: 200px
```

In practice, the FWER control as described in the table is too conservative, i.e. it will not reject the null hypothesis when it is false. What is done is a second level analysis by working not at the level of each voxel, but rather on clusters of voxels whose p-value is sufficiently small, and the FWER criterion is formulated on the maximum of each cluster.



### Generalized Linear Model (GLM)

In the simple linear model, the fMRI signal can be decomposed into:
*  a regressor $x_1$, *i.e.* the expected response to certain stimuli (for example the condition A in previous paradigm),
* an offset,
* an unknown centered noise $\epsilon$.

````{admonition} Mathematical formulation
:class: tip

$Y = \beta_{1} .x_{1} + \beta_{2}. \mathbb{1}  + \epsilon$

where $\mathbb{1}$ is a unit vector, and $\beta_1$, $\beta_2$ are constants to be estimated.
````

```{figure} /images/SLM.png
---
height: 300px
name: slm
---
Example of simple linear model for fMRI with a bloc paradigm.
```



In order to detect different kind of activation in the whole brain volume, several regressors can be defined according to the different conditions in the paradigm, the type of paradigm, etc. This model is called **generalized linear model** and allows to define the fMRI signal as a linear combination of several regressor.

````{admonition} Mathematical formulation
:class: tip

$$Y = \sum_{i} \beta_i x_i + \epsilon = X\beta + \epsilon$$

where $x_i$ are the regressors chosen according to the paradigm (including the unit vector) and $\beta_i$ are the unknown parameters to be estimated.
````




```{figure} /images/GLM.png
---
height: 300px
name: glm
---
Matrix model of generalized linear model for fMRI signal.
```

**Principle:**
* The term "generalized" refers to cases where the model contains at least two regressors in addition to the constant regressor.
* Each condition (event) is represented by at least one regressor.
* Non-interest effect: may also motivate the definition of additional regressors (e.g. low frequency drift, etc)

**The assumptions:**
* The shape of the response induced by an event is assumed to be known.
* Linearity: the effects produced by different events add up.
* Stationarity: the responses to two events of the same nature are identical at all times

**Design matrix:**
* Each column represents the BOLD response to an experimental condition (e.g. response to viewing a human face, landscape, etc).
* the response to an experimental condition may be present with different time lags (delays in the BOLD response to an unknown stimulus).

**Estimation of the $\beta$ parameters:**

Under the assumption that $\epsilon$ is a white noise (centered and uncorrelated) with the same variance $\sigma^2$ during the whole fMRI session, the least square error estimation of $\beta$ can be formulated as:
```{math}
\hat{\beta } = \min_{\beta} \| Y - X\beta \|_2^2 = \min_{\beta} \| \epsilon \|_2^2
```
This leads to solve:
```{math}
\frac{d \| Y - X\beta \|_2^2}{d\beta}\left( \hat{\beta} \right) = 0
```

The solution is:
```{math}
\hat{\beta} = (X^TX)^{-1}X^TY
```

This estimator is unbiased $\mathbb{E}[\hat{\beta}] = \beta$ and the variance of the estimator is $Var[\hat{\beta}] = (X^TX)^{-1}\sigma^2$.

When $\sigma^2$ is unknown, an estimator of this variance must be used:
* $\hat{\sigma}^2 = \frac{1}{n-p}\|Y - X\hat{\beta} \|^2_2$ $\rightarrow$ biaised,
* $\hat{\sigma}^2 = \frac{1}{n-p-1}\|Y - X\hat{\beta} \|^2_2$ $\rightarrow$ unbiased.


```{admonition} Activity
:class: tip

To deepen your understanding of the GLM: read section "A few examples on the early use of the GLM in fMRI - and some warnings" of the following paper:

[*The general linear model and fMRI: Does love last forever?*, Jean-Baptiste Poline and Matthew Brett, NeuroImage 62 (2012) 871–880.](https://projects.iq.harvard.edu/files/imagenesmedicas/files/polinekd.pdf)

Identify the regressors that can be added to the design matrix as well as their advantages and possible disadvantages. In groups, discuss your answers and designate a reporter who will write the group's answers on the board, or complete the answers already given by the classmates.
```



**Contrast and test:**

A contrast is a defined as a difference between two or more experimental conditions, it is a generalization of the T-test inference since more than two conditions can be combined.
the contrast is modelled by a linear combination $c = [c_1, ..., c_p]^T$  of parameters: $c^T\beta$.

In practice, $\beta$ is unknown, its estimation $\hat{\beta}$ is used, so the contrast is $c^T\hat{\beta}$. Generally coefficients of vector $c$ are set to:
* 0 for regressor(s) we don't want to study,
* 1 for regressor(s) that should activate the region of interest in the brain,
* -1 for regressor(s) that are not expected to significantly influence activation.

Under the null hypothesis, contrast is equal to 0 : $c^T\beta =  0$ ($\mathcal{H}_0$).

```{note}
The objective is to build a map of the brain indicating for each voxel if the contrast is significantly different from 0 to detect the brain regions activating for the conditions selected with the c vector.
```

For deciding if the contrast is significant, the following decision statistic is formed:
```{math}
T = \frac{c^T\hat{\beta}}{\sqrt{\vphantom{P^T}\hat{\sigma}^2 c^T(X^TX)^{-1}c}}
```

Under $\mathcal{H}_0$ the distribution of $T$ is a Student law of $(n-p-1)$ degrees of freedom in the unbiased case ($n-p$ in the biased case).

The rest of the procedure consists in thresholding the test on the contrast values obtained for all the voxels of the brain. For this we need :
* calculate the p-value for each test
* perform a thresholding of these p-values according to the chosen procedure ($p_{FA}$, FDR, FWER, etc).

