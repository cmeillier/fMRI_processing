# Functional connectivity: definition and analysis

We have seen in the previous chapter a voxel-based analysis of connectivity in task fMRI with GLM. This type of modeling is not adapted to resting-state fMRI and does not allow to analyze connectivity between different brain regions. We have seen in the previous chapter a voxel-based analysis of connectivity in task fMRI with GLM. This type of modeling is not adapted to resting-state fMRI since it is impossible to define regressors in the absence of a paradigm and does not allow the analysis of connectivity at the scale of brain regions of interest to the neurologist/psychiatrist. The focus in this chapter is on the analysis of functional connectivity at the scale of anatomical regions or functional networks in task or resting state fMRI, and for group or single subject analyses.

### Defintion(s)
Functional connectivity (FC) is defined as the temporal similarity between activity of two (or more) different areas of the brain, *i.e.* theses regions co-activate to respond to a stimulus, to perform a cognitive task or to perform a spontaneous brain function. Regions that have a similar activity consistently over time form what is called a **functional network**.

In order to measure the similarity between the time signatures of two regions, different criteria can be used:
* the correlation between two signals which is the most commonly used measure in the literature,
* the mutual information
* partial correlation
* coherence (in the frequency domain)
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

Two signals are strongly correlated in the Pearson sense if :
* they move in the same direction
* they evolve in a synchronous way
The correlation is equal to 1 if and only if $x_i$ can be written proportionally to $x_j$, i.e. if $x_i = \alpha x_j + \beta$ with $alpha$ and $beta$ constants.


In practice, Pearson correlation is the most widely used metric to quantify functional connectivity. Its main drawback is that it measures the presence of a linear relationship between the two signals. Spearman correlation or mutual information are able to capture non linear relationship but have their own limitations, in particular for further statistical analysis of connectivity. For example, the mutual information needs to be estimated, making assumptions on the distribution of the values taken by the signals. The distribution can be estimated by the histogram of the data which can be affected by noise (due to acquisition, preprocessings, etc.).

### Identification of functional networks and their brain activity during rest

For task fMRI, GLM analysis is widely used, but for rest

In some studies, certain anatomical regions are of interest for the pathology concerned. The neuroscientists then use an atlas (i.e. an anatomical segmentation of the brain) to extract timecourses of the voxels belonging to these regions of interest (ROI). A mean signal is then computed for each ROI and the connectivity can be measured by correlation, mutual information, etc. Since the registration of fMRI data to the atlas (or vice versa) is never perfect and the spatial resolution of voxels in fMRI is very coarse, more robust strategies than simple averaging can be used to extract the time course for each ROI. Since the registration of fMRI data to the atlas (or vice versa) is never perfect and the spatial resolution of voxels in fMRI is very coarse, more robust strategies than simple averaging can be used to extract the timecourse for each ROI. From a signal and image processing point of view, signal unmixing methods are adapted to refine the estimation of the timecourse, even if most of the time, simplicity prevails over accuracy. Functional networks are then created by gathering ROIs with high connectivity.


```{figure} /images/atlas.png
---
height: 300px
name: atlas
---
Example of the Allen Human Brain atlas in sagital plane (left) and coronal plane (right), with two anatomical regions highlighted: hippocampal formation (orange) and amygdala (purple). Screenshot of Brain Explorer 2 application delivered by the Allen Institute for Brain Science.
```

When one does not wish to work from predefined anatomical regions, functional networks can be directly extracted along with their temporal course via blind source separation methods. The most commonly used in fMRI is the independent component analysis (ICA). The size of the functional networks then depends on the number of components desired. With a low number of components, we obtain spatially extended networks, which then prevents a fine analysis of the connectivity, in particular in the case of the search for change of the functional connectivity due to a pathology. Conversely, with a very large number of independent components, we will estimate small networks, even functional sub-networks, but also many components that are in fact only noise. It then requires the help of a neuroscientist to exclude these nuisance components.
These blind source separation methods allow to obtain for each spatial source its time course. As for the ROI-based methods, a connectivity measure is then computed and the networks or sub-networks can be associated to form larger ones.

ICA-based methods perform well in group analyses because they take advantage of the redundancy within each group to produce the best possible independent components. On the other hand, at the single-subject level, these methods produce very noisy components, making analysis at the individual level impossible.


### Correlation analysis


[Activity : Analysing correlation matrices](https://moodle.unistra.fr/mod/quiz/view.php?id=627410)


