# Functional connectivity: definition, analysis and utility



Functional connectivity (FC) is defined as the temporal similarity between activity of two (or more) different areas of the brain, *i.e.* theses regions co-activate to respond to a stimulus, to perform a cognitive task or to perform a spontaneous brain function. Regions that have a similar activity consistently over time form what is called a **functional network**.

In order to measure the similarity between the time signatures of two regions, different criteria can be used:
* the correlation between two signals which is the most commonly used measure in the literature,
* the mutual information
* partial correlation
* etc.

`````{admonition} Connectivity as Pearson correlation coefficient
:class: tip
Let $x_i(n)$ and $x_j(n)$ be the discrete temporal signatures of the two regions $i$ and $j$ of interest.
Let $\bar{x}_i$ and $\bar{x}_j$ be the mean of $x_i(t)$ and $x_j(t)$ and $\sigma_i$ and $\sigma_j$ their standard deviation.
Correlation coefficient of regions $i$ and $j$ is computed as follows:
```{math}
:label: corrcoef
r_{i,j} = \frac{1}{N}\sum_{n = 1}^{N} \frac{\left(x_i(n) - \bar{x}_i\right) \left(x_j(n) - \bar{x}_j\right)  }{\sigma_i \sigma_j}
```
where $N$ is the number of time samples in fMRI acquisition.

`````

In practice, Pearson correlation is the most widely used metric to quantify functional connectivity. Its main drawback is that it measures the presence of a linear relationship between the two signals. Spearman correlation or mutual information are able to capture non linear relationship but have their own limitations, in particular for further statistical analysis of connectivity. For example, the mutual information needs to be estimated, making assumptions on the distribution of the values taken by the signals. The distribution can be estimated by the histogram of the data which can be affected by noise  \ref{}




