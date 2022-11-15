All data used in this paper are publicly available and can be accessed here:  
- PDBbind v2016 and v2019: http://www.pdbbind.org.cn/download.php
- 2013 and 2016 core sets: http://www.pdbbind.org.cn/casf.php
- DUD-E and DEKOIS2.0: https://zenodo.org/record/3975554#.Y1VoT44zaUk

You can download the preprocessed data from: https://drive.google.com/file/d/1oGUP4z7htNXyxTqx95HNSDLsaoxa3fX7/view?usp=share_link

## Requirements  
dgl==0.9.0  
networkx==2.5  
numpy==1.19.2  
pandas==1.1.5  
pymol==0.1.0  
rdkit==2022.3.5  
scikit_learn==1.1.2  
scipy==1.5.2  
torch==1.10.2  
tqdm==4.63.0  
openbabel==3.3.1 (conda install -c conda-forge openbabel)

## Descriptions of folders and files
+ **./data**: This folder contains information about train, valid, test2013, test2016, and test2019 data sets. You should first download the preprocessed datasets from https://drive.google.com/file/d/1oGUP4z7htNXyxTqx95HNSDLsaoxa3fX7/view?usp=share_link, and put them into this folder and organize them as './data/train', './data/valid', './data/test2013/', './data/test2016/', and  './data/test2019/'. We also provide a toy set with 50 examples to explain how to process raw data and train EHIGN model from scratch. 
+ **EHIGN.py**: The implementation of EHIGN.
+ **HGN.py**: The implementation of the heterogeneous graph neural network, where most of the contents are copied from the source code of dgl, but we have made some modifications so that it can process edge features.
+ **preprocess_complex.py**: Prepare input complexes. The input ligand and protein should be .mol2 and .pdb formats, respectively. Proteins are first cropped around the ligand within 5 angstrom using pymol. Each protein and ligand is then processed into a protein-ligand complex using rdkit (i.e., a tuple that contains a ligand and a protein).
+ **graph_constructor.py**: Convert protein-ligand complexes into heterogeneous graphs.
+ **train.py**: Train EHIGN model.
+ **test.py**, Test a trained model on 2013 core set, 2016 core set, and 2019 holdout sets and print the results.
+ **utils.py** The File that includes useful tools for model training.

## Step-by-step running:  

### 1. Reproduce the reported results
The data folder contains heterogeneous graphs after preprocessing for 2013 and 2016 core sets. Therefore, run test.py using `python preprocessing.py` can obtain the results reported in our study ( $R_p=0.850$ for 2013 core set, $R_p=0.861$ for 2016 core set). If you preprocess the 2019 holdout set correctly, you can also obtain $R_p=0.655$ for the 2019 holdout set.

### 2. Test the trained model in other external test sets
Firstly, please organize the data as a structure similar to './data/toy_set' folder.  
-data  
&ensp;&ensp;-external_test  
&ensp; &ensp;&ensp;&ensp; -pdb_id  
&ensp; &ensp; &ensp;&ensp;&ensp;&ensp;-pdb_id_ligand.mol2  
&ensp; &ensp; &ensp;&ensp;&ensp;&ensp;-pdb_id_protein.pdb  
Secondly, run preprocess_complex.py using `python preprocess_complex.py`.  
Thirdly, run graph_constructor.py using `python graph_constructor.py`.  
Fourth, run test.py using `python test.py`.  
You may need to modify some file paths in the source code before running it.

### 3. Train EHIGN from scratch
Firstly, please organize the data as a structure similar to './data/toy_set' folder.  
-data  
&ensp;&ensp;-train  
&ensp; &ensp;&ensp;&ensp; -pdb_id  
&ensp; &ensp; &ensp;&ensp;&ensp;&ensp;-pdb_id_ligand.mol2  
&ensp; &ensp; &ensp;&ensp;&ensp;&ensp;-pdb_id_protein.pdb  
Secondly, run preprocess_complex.py using `python preprocess_complex.py`.  
Thirdly, run graph_constructor.py using `python graph_constructor.py`.  
Fourth, run train.py using `python train.py`.  
You may need to modify some file paths in the source code before running it.
