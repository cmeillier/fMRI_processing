# Lab session : task fMRI analysis

In this tutorial we will implement the notions of general linear model, contrast and statistical map seen in class. For this we will use an fMRI acquisition performed by Geraint Rees under the direction of Karl Friston (Functional Imaging Laboratory). The subject of their study was to explore the possibilities offered by functional MRI imaging in its infancy. The data is available to the academic world at  [SPM website](http://www.fil.ion.ucl.ac.uk/spm/data/auditory/). Le jeu de données fourni a été acquis avec un IRM 2T Siemens. The dataset provided was acquired with a Siemens 2T MRI. Each (3D) acquisition is a $64\times 64\times 64$ matrix with voxels of size  $3 \times 3 \times 3$ mm$^3$. Each 3D acquisition actually takes exactly 6.05s (for the 64 slices) then the acquisitions were finally performed with a repetition time TR = 7s. In the studied dataset, there are 96 acquisitions performed on a subject with a block paradigm.  Each block is composed of 6 acquisitions of silence and 6 acquisitions during which the patient hears bisyllabic words. This gives a paradigm of 8 blocks of 84s.

````{margin}
```{note}
The subject is written in Matlab, but if you prefer to code in Python, it is also possible. For loading the .mat data, use the following instruction : \\
from scipy.io import loadmat.
data = loadmat('filename.mat')
```
````

In this tutorial, the data was pre-processed, the first block of 12 acquisitions was removed and the images were trimmed on the edges (of the 3D cubes).

**The objective of this tutorial is to study the brain areas activated by the subject's hearing of words in MRI.**

---


## Data preparation

The pre-processed data is available in the archive 'data.zip'. Unpack the archive and place the files in your current working directory.

### Loading images and signals useful in this course

1. Load the fMRI acquisition contained in the file 'swfM00223.mat', the 4D matrix is stored in the variable 'I'.
2.  Save the dimensions of image I in 4 variables N1, N2, N3, N4.
3. Load the mask contained in the file 'mask.mat', the 3D mask is contained in the variable 'mask'.
4.  Load the hemodynamic response contained in the file 'hrf.mat', the vector is stored in the variable 'hrf'.


### Hemodynamic response

1. What is the time sampling rate of the data ?
2.  Display the hemodynamic response provided (it has been sampled with the same frequency as the data). What can be said about this response and the time sampling rate of the fMRI acquisition?

### Extraction of brain time courses

Before analysing the fMRI image, we need to extract the voxels belonging to the brain (the skull and everything else in the periphery of the brain is a nuisance for the analysis). We provide a binary mask in the file 'mask.mat' which contains 1 in the voxels belonging to the brain and 0 everywhere else.


1. How many voxels does the mask contain? We will store this value in the variable  $\tt{nbVox}$.
2. Create a matrix Y of size $N4 \times nbVox$ which will contain in each column the timecourses of the brain voxels.

In order to extract the timecourses of interest, the following code will be used:

	for i = 1:N4

	      tmp = squeeze(I(:,:,:,i));

	      % Extraction of the 3D acquisition at time i

	      Y(i,:) = tmp(mask >0);

	end

3. Extract the timecourses with the above code.


We will now work with this matrix Y where each column corresponds to a time course of size $N4\times 1$ (vector $y$ in the course formulas).

---
## Generalized linear model

### Creating the paradigm design

According to the description of the experimental paradigm, we want to create a vector that reflects the alternation of audio sequences and sequences of silence.

1. Create the vector *paradigm* containing the experimental paradigm described in the introduction.
2. Display this vector and check that it is consistent with the experiment (the better the vector modelling the paradigm, the better the regression).
3. This vector *paradigm* can be combined with the hemodynamic response to best synthesise the expected response to the audio stimulation. We will call this *paradigme_hrf* the convoluted paradigm to the haemodynamic response contained in the vector *hrf*.


### Creating the design matrix

Recall that the generalized linear model for a single timecourse is written :
```{math}
Y = \sum_{i} \beta_i x_i + \epsilon = X\beta + \epsilon
```
where $x_i$ are known regressors (chosen according to the paradigm!),$\beta_i$ are the unknown parameters to be estimated.


The model can be represented graphically :
```{figure} /images/GLM.png
---
height: 300px
name: glm2
---
Matrix model of generalized linear model for fMRI signal.
```

In the matrix  $X$  regressors $p = 3$ are used :
* a constant vector (to model a possible offset, or the average of the noise if it is non-zero, etc),
* the paradigm vector (convoluted or not to the haemodynamic response)
* a vector $\tt{meanY}$ which contains the average of the time courses.


The last regressor is related to the presence of a hypersignal in the ventricles (filled with cerebrospinal fluid) which is found in the average timecourse.

4. Construct the 3 regressors as 3 vectors $x_1, x_2$ and $x_3$ of size $N4 \times 1$ containing the information given above.
5. Construct the matrix X by putting in the first column the vector $x_1$, in the second column the vector $x_2$ and in the third column, the vector $x_3$.


### Least squares estimation and contrast map

The least squares estimator of the vector $\beta$ containing the 3 regression coefficients is obtained by the equation :
```{math}
\hat{\beta} = (X^TX)^{-1}X^Ty
```
when $y$ contains a single timecourse.

We want to calculate here the 3 regression coefficients for all the brain voxels contained in the matrix  $Y$ of size $N4\times nbVox$. The calculation of $\hat{\beta}$ can be done for all voxels by rewriting the problem in matrix form:
```{math}
 \hat{\beta} = (X^TX)^{-1}X^TY.
```
This model can be represented graphically:
```{figure} /images/glm3.png
```

1. Calculate the matrix $\hat{\beta}$.
2. Compare the size of the matrix $\hat{\beta}$ to the size of the 3D brain mask.
3. Conclude on how to visualize in 3D the values of $\hat{\beta}$ for each regressor.


We are interested in the contrast $\beta_2 - \beta_3$ where $\beta_2$ is the regression coefficient related to the paradigm and  $\beta_3$ is the regression coefficient related to the mean value of the timecourses.


4. Create the vector $\tt{c}$ of contrast.

The decision statistic is formed:
```{math}
T = \frac{c^T\hat{\beta}}{\sqrt{\vphantom{P^T}\hat{\sigma}^2 c^T(X^TX)^{-1}c}}
```

with $\hat{\sigma}^2 = \frac{1}{n-p-1}\|Y - X\hat{\beta} \|^2_2$

In the matrix case, $T$ and $\hat{\sigma}^2$ are vectors of size $nbVox \times 1$.

The density probability function $p(T|\mathcal{H}_0)$ of the statistics  $T$ under $\mathcal{H}_0$ is the Student's law at $(N4 - p -2)$ degrees of freedom. The distribution function of the Student's law at $(N4 - p -2)$ degree of freedom is obtained in Matlab with the following code:

	tcdf(T, N4 - p -2)

5. Calculate the vector $\hat{\sigma}^2$.
6. Calculate the vector $T$.
7. Calculate the vector of p-values.
8. Visualise the p-values in 3D using the mask and code of the question 3.
9. The p-values can be thresholded for a significance level $\alpha = 0.01$ to visualise the voxels for which the contrast is most significant.
10. Overlay the thresholded map on one of the acquisitions $I(:,:,:,i)$ to visualise the areas of the brain activated by hearing.
11. What are the areas identified using the previous contrast ?
12. Do T-test analysis and correlation analysis give the same result? If you have time, you can implement either method to compare the results.


