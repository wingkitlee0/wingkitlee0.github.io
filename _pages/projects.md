---
layout: single
title: "My Projects"
permalink: /projects/
author_profile: true
toc: true
---

Here are a few projects that I have done during spare time.

## [Greemigration](http://www.planetanalytics.space)

![A demo screenshot here..]({{ site.url }}{{ site.baseurl }}/assets/images/demo_greemigration.png){: .align-center}

The time to get a Green Card / permanent residency in the US is very uncertain. In this project I built a predictive model to make prediction of the waiting time based on the data. The web app allows user to supply their application information and obtain insights on how long does the approval take. Feel free to try it out.

[Github](https://github.com/wingkitlee0/greemigration)

(Disclaimer: the prediction provided AS-IS. Do not make important decision based on the web app.)

---

## [Abstract Analyzer](https://azj31tvvek.execute-api.us-east-1.amazonaws.com/dev/)

![A demo GIF was here..]({{ site.url }}{{ site.baseurl }}/assets/images/demo.gif){: .align-center}

This is a [webapp](https://azj31tvvek.execute-api.us-east-1.amazonaws.com/dev/) that analyzes the summary of a paper and classify it into one of the astro-ph categories. I put the details [here](machinelearning.md).

---

## Alexa Skills ("Apps" on Amazon's Echo devices)

<iframe width="560" height="315" src="https://www.youtube.com/embed/PIdKXqN1x54" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I have written two Alexa skills (astroph, astroph Earth). Basically they allow you to listen to the 5 newest academic paper abstracts submitted to the preprint server (arxiv.org). The RSS feeds from [arxiv.org](https://www.arxiv.org) are updated daily. The details of how to create with `AWS Lambda` / `Flask-Ask` etc can be found [here](https://wingkitlee0.github.io/alexa astroph). In short, I wrote a short python script (hosted on AWS Lambda) to convert the RSS feed to an Alexa-readable JSON file.

---

## Contributions to open-source projects

I have also contributed to some open-source projects. I am not the owner of these packages but did make some improvements.

### [scikits.bvp_solver](https://github.com/wingkitlee0/scikits.bvp_solver)

This is a Python library for solving two-point boundary value problems, based on a well-known Fortran library ([BVP_SOLVER](http://cs.stmarys.ca/~muir/BVP_SOLVER_Webpage.shtml)). Both original Fortran and the Python wrapper were life-savers for me on my PhD thesis.
Contributions:
- Added Python 3 compatibility
- Added support on Windows + ming-w64
- Added support for equations with complex variables

### [scikit-cuda](https://scikit-cuda.readthedocs.io/en/latest/)

This is a Python wrapper for the [MAGMA](http://icl.cs.utk.edu/magma/) GPU linear algebra library, which allows drop-in replacement of the scipy package. The MAGMA library is needed because the standard linear algebra library from NVIDIA (i.e., CUSOLVER) does not have all the routines included. 
Contributions:
- Added support for non-symmetric eignvalue solver (i.e., geev in standard LAPACK library).

---

## [Arxiv Reader](https://www.microsoft.com/en-us/p/arxiv-reader/9nblggh5kb5j) (Mobile)

This is an app to browse papers via the RSS feeds from [arxiv.org](https://www.arxiv.org). Over a hundred of acquisitions on the Windows Phone platform.
