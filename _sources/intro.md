# Introduction to fMRI data


As part of the 4-hour course on functional MRI, you are invited to read the course and do the activities proposed to illustrate the theoretical part. You can go at your own pace, the practical application with Matlab lasts about 1h30. If you have not finished reading the course by the end of the 4 hours, you must finish before the final exam which will include questions on all the concepts covered in this document. During the whole session, you can work in groups, you can also ask questions that we will answer with the whole class when it is necessary to make a point.


----

## Caracterization of fMRI data

### Origin of fMRI data



When the subject is performing an action (motor action, cognitive task, etc.) or at rest with spontaneous cerebral activities, specific neurons activate. Their activation leads to an increase in oxygen consumption, which implies an increasing of the blood flow locally in the brain to bring more oxygen. The oxygen, brought by hemoglobin (HbO2), is diamagnetic (Fe$^{++}$ bound to O$_2$) and does not disturb the magnetic field, contrary to deoxyhemoglobin (Hb) which is paramagnetic since ions Fe$^{++}$ remain unpaired.
Intuitively one might think that it is the oxygen consumption that is measured via the BOLD signal (Blood oxygen level dependent), but in reality it is the increase in blood flow which implies an overall increase in HbO2/Hb ratio. It is the variation of this rate, called BOLD contrast, that leads to a local disturbance of the magnetic field of the MRI and that is measured. We talk about **indirect measure** of brain activity because it is the disturbance of the magnetic field induced by the variation of oxygen in the blood caused by the activity of neurons that is acquired.
A nice animation to understand the BOLD contrast principle [IMAIOS ressource](https://www.imaios.com/en/e-Courses/e-MRI/Functional-MRI/brain-activation-bold-contrast)

The BOLD effect is very faint, it induces a variation of a few percents in the measured signal. The concentration of HbO2 and Hb are the following:

* at rest:
```{math}
\left\{
    \begin{array}{l}
        60\% \mbox{ HbO}_2  \\
        40\% \mbox{ Hb}
    \end{array}
\right.
```
* during task:
```{math}
\left\{
    \begin{array}{l}
        63\% \mbox{ HbO}_2  \\
        37\% \mbox{ Hb}
    \end{array}
\right.
```

During a task, only voxels containing the neurons activated to respond to the required function are expected to exhibit a change in the BOLD signal. Since the signal variation is very small and the fMRI signal is very noisy, it is necessary to repeat the action several times in order to statistically guarantee the detection of activated regions (see section on paradigm design).

A nice animation to understand the statistical analysis of fMRI data : [IMAIOS ressource](https://www.imaios.com/en/e-Courses/e-MRI/Functional-MRI/analysis-functional-mri-data)



### Numerical and mathematical model of fMRI data
From image processing point of view, fMRI data is a collection of 3D images aquired every TR seconds where TR is the repetition time between two consecutive brain volume acquisitions.

From a signal processing point of view, fMRI data is a collection of time signal with a spatial coherence : each voxel of the brain volume contains a signal and there is a high probability that two neighboring voxels have similar temporal activity. The TR value corresponds to the sampling period of the signal.

As with any multidimensional acquisition system, there is a compromise to be made between the spatial and temporal dimensions of an fMRI dataset. If we want to increase the spatial resolution, i.e. increase the number of voxels in a brain volume, we must increase the TR (and therefore the signal sampling period). Inversely, if we want to increase the temporal resolution of the signals, we must decrease the number of voxels acquired in each volume and thus decrease the spatial resolution.

As the interest of an fMRI examination is to study brain activity over time, it is necessary to privilege temporal resolution over spatial resolution. This is why the anatomical structures of the brain are less clear on the images composing an fMRI data set than for a structural MRI acquisition.

```{figure} /images/MRI_fMRI.png
---
height: 300px
name: fMRI
---
Comparison of spatial resolutions of strucural (left) and functional (right) MRI data
```


As stated in the Nyquist–Shannon sampling theorem, with a sampling rate (or sampling frequency) of $f_s = \frac{1}{TR}$, to represent a continuous signal whose spectrum is defined on the frequency band [-$B$, $B$] it is necessary that $f_s \geqslant 2B$.

## Studying fMRI signal with signal processing tools

Many signal processing tools are used to analyse fMRI signals :
* convolution to model physiological process action on BOLD signal,
* harmonic analysis (Fourier transform) for stationary signals,
* wavelet analysis or time-frequency analysis for transient signals,
* low-pass and high-pass filters for removing artifacts, noise, etc.,
* confounding signal regression to remove any nuisance sources before connectivity analysis,
* etc.

## Objectives of fMRI data analyses







----

## Table of contents
```{tableofcontents}
```

