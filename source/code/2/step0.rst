Step0: KNN-graph construction
==========================================================

.. toctree::
   :maxdepth: 1

::

 from sklearn.neighbors import kneighbors_graph
 import numpy as np
 import pandas as pd
 import datetime


 # Hyperparameters
 InputFolderName = "./MIBI_TNBC_KNNgraph_Input/"
 KNN_K = 72

 # Import image name list.
 Region_filename = InputFolderName + "ImageNameList.txt"
 region_name_list = pd.read_csv(
         Region_filename,
         sep="\t",  # tab-separated
         header=None,  # no heading row
         names=["Image"],  # set our own names for the columns
     )

 for graph_index in range(0, len(region_name_list)):

     print(graph_index)
     # Import target graph x/y coordinates.
     region_name = region_name_list.Image[graph_index]
     GraphCoord_filename = InputFolderName + region_name + "_Coordinates.txt"
     x_y_coordinates = np.loadtxt(GraphCoord_filename, dtype='float', delimiter="\t")

     K = KNN_K
     print(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
     print('Constructing KNN graph...')
     KNNgraph_sparse = kneighbors_graph(x_y_coordinates, K, mode='connectivity', include_self=False, n_jobs=-1)  #should NOT include itself as a nearest neighbor. Checked. "-1" means using all available cores.
     print(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
     KNNgraph_AdjMat = KNNgraph_sparse.toarray()
     # Make the graph to undirected.
     print('Making KNN graph bidirectional...')
     KNNgraph_AdjMat_fix = KNNgraph_AdjMat + KNNgraph_AdjMat.T  #2min and cost one hundred memory.
     # Extract and write the edge index of the undirected graph.
     KNNgraph_EdgeIndex = np.argwhere(KNNgraph_AdjMat_fix > 0)  #1min
     filename0 = InputFolderName + region_name + "_EdgeIndex.txt"
     np.savetxt(filename0, KNNgraph_EdgeIndex, delimiter='\t', fmt='%i')  #save as integers. Checked the bidirectional edges.
     print(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S'))


**Hyperparameters**

- KNN_K: The K value in the KNN gragh.


**Input**

The example input data of this part is a KNN graph constructed based on MIBI-TNBC data, including a image name list and cell type label, cell spatial coordinate, edge index, gragh index, gragh label and node attribute files. Here we take patient1 for an example as follows:

::

 ── MIBI-TNBC data
    ├─ patient1_CellTypeLabel.txt
    ├─ patient1_Coordinates.txt
    ├─ patient1_GraghIndex.txt
    ├─ patient1_GraghLabel.txt
    └─ patient1_NodeAttr.txt

**Output**

We use Step0 to construct KNN graghs and prepare data for the subsequent steps, and this step produces "EdgeIndex.txt" file for each input image.

::

 ── MIBI-TNBC data
    ├─ patient1_CellTypeLabel.txt
    ├─ patient1_Coordinates.txt
    ├─ patient1_GraghIndex.txt
    ├─ patient1_GraghLabel.txt
    ├─ patient1_NodeAttr.txt
    └─ patient1_EdgeIndex.txt
