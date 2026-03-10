# How to debias hierarchical CAVs?

Most debiasing methods for computer vision models require two elements: (1) the CAV, as a direction in a latent space, and (2) a debiasing procedure. There are many approaches to (2): starting from simple orthogonalisation of the latent space to the CAV, ending on methods from the CLARC family ([A-CLARC](https://arxiv.org/abs/1912.11425), [P-CLARC](https://arxiv.org/abs/1912.11425), [R-CLARC](https://arxiv.org/pdf/2404.09601)). However, all existing CAVs are working in the context of the full latent space, so the natural question arises - should hierarchical CAVs be treated differently by existing debiasing procedures? **This project aims to verify whether existing debiasing methods work for hierarchical CAVs and how we can improve them to incorporate hierarchies.**

## Debiasing methods

Debiasing methods are designed to remove some information from the model when it's misused. For example, a model may learn to classify each person with long hair as a woman, which is obviously incorrect. Why can't we just train a model so it does not contain bias?

- Bias is created via data distribution, and sometimes filtering out observations that can create some bias means removing all data.
- the (re)training is costly and time-consuming,
- The original volume of data might not be accessible.

At the same time, removing bias via latent space is fast and does not require much data. A-CLARC adds random bias to each observation, so the model cannot generalise knowledge from it. P-CLARC subtracts a vector from each observation, so in the context of a specific concept, all examples "look like" they don't have it.

The important thing in debiasing models is to define bias and model it in latent space. For this, CAVs are just perfect.

## Project structure and problem statement

During this project, we would like to evaluate existing debiasing methods on concepts that are hierarchical and verify whether they perform better when CAVs are created to incorporate these hierarchies. Then, we will try to find better alternatives for debiasing these CAVs. Notably, we will verify if using debiasing on one concept will affect other concepts in the hierarchy. The project can be divided into the following parts:

1. Creating CAVs and hierarchical CAVs
  1.1 Choosing dataset and model, at least 1
  1.2 Making ~2 CAV methods on them (including hierarchical ones), ensure they have *decent* quality
  1.3 Code: mostly copy-paste from labs, methodology: mostly presented during labs
  1.4 Final results - ready to go CAVs on specific model/dataset
2. Performing debiasing
  2.1 Choosing debiasing method (will be shown on labs one day)
  2.2 Making debiasing (may require retraining last layers of the model - generally lightweight operation)
  2.3 Making analysis of the results (how debiasing affect model)
  2.4 Code: partially copy-paste from labs, methodology: mostly presented during labs
  2.5 Final results - debiasing analysis
3. Creating hierarchical debiasing
  3.1 Basically the same as in (2), except the debiasing method will be different
  3.2 The method will be suggested later, but you are encouraged to experiment with your own ideas
4. Analysis of the results (cross-concepts effects and accuracy metrics)
  4.1 This step is basically combining results from (2) and (3)
