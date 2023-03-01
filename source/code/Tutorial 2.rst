
Turtorial 2: Supervised CytoCommunity (MIBI-TNBC dataset)
==============================================================

.. toctree::
   :maxdepth: 1

   2/step0
   2/step1
   2/step2
   2/step3
   2/step4

We can also run CytoCommunity in a supervised learning task. Given a dataset of multiple spatial omics images from different conditions, TCNs can be first identified for each image and then aligned across images for identifying condition-specific TCNs. However, TCN alignment is analogous to community alignment in graphs, which is NP-hard. To tackle this problem, we take advantage of graph pooling to generate an embedding representation of the whole graph that preserves the TCN partition information. By adapting the unsupervised graph partitioning model to a graph convolution and pooling-based graph classification framework, TCNs in different images are automatically aligned during soft TCN assignment learning, facilitating the identification of condition-specific TCNs.
