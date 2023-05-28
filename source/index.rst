.. CytoCommunity documentation master file, created by
   sphinx-quickstart on Mon Feb 20 09:59:36 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

CytoCommunity - Unsupervised and supervised discovery of tissue cellular neighborhoods from cell phenotypes
=====================================================================================================================


.. toctree::
   :maxdepth: 2

   code/Tutorial 1
   code/Tutorial 2

.. image:: ../image/overview.png


Introduction
=====================

We developed the CytoCommunity algorithm for identifying TCNs that can be applied in either an unsupervised or a supervised learning framework. The direct usage of cell phenotypes as initial features to learn TCNs makes it applicable to both single-cell transcriptomics and proteomics data, with the interpretation of TCN functions facilitated as well. Additionally, CytoCommunity can not only infer TCNs for individual images but also identify condition-specific TCNs for a set of images by leveraging graph pooling and image labels, which effectively addresses the challenge of TCN alignment across images.

CytoCommunity is the first computational tool for end-to-end unsupervised and supervised analyses of single-cell spatial maps and enables direct discovery of conditional-specific cell-cell communication patterns across variable spatial scales.

Citation
================

- Hu Y, Rong J, Xie R, Xu Y, Peng J, Gao L, Tan K. Learning predictive models of tissue cellular neighborhoods from cell phenotypes with graph pooling. *bioRxiv*, 2022.
https://www.biorxiv.org/content/10.1101/2022.11.06.515344v1
