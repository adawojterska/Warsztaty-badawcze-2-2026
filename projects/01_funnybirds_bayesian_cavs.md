# How latent space *should* work? Conditioning latent on Bayesian network

There are some works that have some bold assumptions about what the latent space looks like. [PATCAV](https://link.springer.com/chapter/10.1007/978-3-031-63787-2_8) is, in fact, a simple difference of means when concepts are binary, but it changes its formula for concepts with multiple possible values (e.g., colors). [HCEP](https://arxiv.org/pdf/2602.11448) operates on the idea of hierarchy of concepts, postulating that, if $A\rightarrow B$, then $SV(A) - SV(B) \perp SV(A)$. [Pahde et al.](https://link.springer.com/chapter/10.1007/978-3-032-08317-3_4) postulates that all concepts should be orthogonal $SV(A) \perp SV(B)$, ignoring any potential relations.

The obvious problem of all of these approaches is the bold assumption that the latent space of models follows the postulated geometry. Apart from that, all of these concepts can be easily combined using Bayesian networks, allowing us to evaluate all of them at once. **This project aims to address these issues by introducing a synthetic framework to evaluate theses stated in these papers, potentially extending them to new relations**.

## Bayesian networks

Bayesian network models the probability of variables by a Directed Acyclic Graph (DAG). It is essentially a tree (forest) describing what depends on what. For example, $A \rightarrow B \rightarrow C$ represents that C depends on B, and B depends on A. Notably, $A|B \perp C|B$. Another example is $A \rightarrow B \leftarrow C$, which represents that B depends on both A and C.

Bayesian networks can model complex relationships; for us, it is important that they can model 1) independence, 2) hierarchies, and 3) non-binary variables. Moreover, they can release these assumptions continuously. For example, let's say that $A \leftarrow B \rightarrow C$. A and C are clearly dependent if no information about B is observed. We can, however, manipulate the Normalized Mutual Information (NMI) of (A, B) and (B, C) so that (A, C) are either barely dependent or strongly dependent.

## FunnyBirds dataset

Now consider images and their concepts. How can we model concepts in the image? Well, there are some super complicated ways to do it by diffusion/flow models, but let's skip it for now. So the answer is: we cannot do it really. So, all considerations from the previous paragraph are irrelevant to the vision models? Not really.

[FunnyBirds](https://openaccess.thecvf.com/content/ICCV2023/html/Hesse_FunnyBirds_A_Synthetic_Vision_Dataset_for_a_Part-Based_Analysis_of_ICCV_2023_paper.html) dataset is actually a framework to produce renders of simple birds' images. Because it can be regenerated with any combination of birds' parts, we can easily regenerate it to create a synthetic dataset with any relation of concepts. Thus, we can generate a dataset that realizes any Bayesian network!

## Project structure and problem statement

During this project, we would like to verify the geometry of the latent space when we have full control over concepts' relations. In particular, we would like to verify assumptions from papers listed in the first paragraph. The project can be divided into the following parts:

1. Bayesian networks definition (potentially across scales in NMI for the same structure)
  1.1 Developing a code for generation of tabular dataset which will contain attributes available for FunnyBirds dataset
  1.2 Ensuring that generation process is parametrized by mututal information - you can generate datasets that produces different level of dependency between attributes
  1.3 Coding: rather easy, methodology: requires understanding a bit of Bayesian Networks (easy if you find MSO course easy)
  1.4 Final results - generated tables of attributes that follows given Bayesian network generation procedure
2. Data generation: FunnyBirds generated according to the (1)
  *This is probably the most boring part of the project, but may take a while - be sure you don't postpone it*
  2.1 Launching FunnyBirds codebase, including server etc.
  2.2 Possibly changing code to enable custom-defined birds
  2.3 (this step should be done in parallel with the first one, so you know how exactly you can pass dataset with attributes to FunnyBirds' codebase)
  2.4 Coding: probably annoying and require vague understanding of frontend code, methodology: None
  2.5 Final results - generated ~50 samples per experiment setup (defined by (1))
3. Model training (potentially small ConvNext or ResNet)
  3.1 This is claimed to be done in ~4h in PATCAV work on 1 GPU - so it is possible to do it on Collab; however, I can share HPC (MINI's cluster) access to make mass experiments (contact me before this step to ensure the code will be usable on HPC)
  3.2 Code might be copy-pasted from any repo, we only need reasonable model, no custom code is really necessery here
  3.3 Number of models to train may vary depending on the resources, ideally (and unrealistically) hundrests of instances, but making <10 also will do
  3.4 Coding: easy to None, but adjusting a code will be necessery if you wnt to run it on HPC, methodology: None
  3.5 Final results - trained and saved instances of models, one for each experiment setup (defined by (1))
4. Latent space analysis, including testing assumptions from the papers
  4.1 Making rather simple tests, including computing cosine similarity; here, the methodology of the analysis is the most important thing
  4.2 Coding: a lot, but of rather in simple numpy/pandas operations, methodology: requires your own intuition what is important, but not reading additional literature
  4.3 Final results - set of metrics, analysis, etc., that show how latent is structured in each experiment

## Technical notes

This project is a bit ambitious, but also really promising for further research. There's some interest in this topic at the Centre of Credible AI, so it *might* be a starting point for a bachelor's thesis (or not, things happen).
