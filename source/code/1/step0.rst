Step0: KNN-graph construction
==========================================================

.. toctree::
   :maxdepth: 1

**Code**

::

   from sklearn.neighbors import kneighbors_graph
   import numpy as np
   import pandas as pd
   import datetime


   # Hyperparameters
   InputFolderName = "./MERFISH_Brain_KNNgraph_Input/"   
   KNN_K = 69

   # Import region name list.
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

**Input**

The example input data to the unsupervised learning mode of CytoCommunity is a KNN graph based on mouse brain MERFISH data, including cell type labels, cell spatial coordinates, edge index, gragh index and node attributes files and an image name list.

::

 ── MERFISH brain KNNgraph input data
    ├─ ImageName_CellTypeLabel.txt
    ├─ ImageName_Coordinates.txt
    ├─ ImageName_NodeAttr.txt
    └─ ImageName_GraghIndex.txt

**Output**

We use Step0 to construct KNN graghs and prepare data for the subsequent steps, and this step produces "EdgeIndex.txt" file for each input image.

::

 ── MERFISH brain KNNgraph input data
    ├─ ImageName_CellTypeLabel.txt
    ├─ ImageName_Coordinates.txt
    ├─ ImageName_NodeAttr.txt
    ├─ ImageName_GraghIndex.txt
    └─ ImageName_EdgeIndex.txt

