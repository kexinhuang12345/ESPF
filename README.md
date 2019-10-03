# Explainable Substructure Partition Fingerprint (ESPF)
Repository for "Explainable Substructure Partition Fingerprint for Protein, Drug, and More" (NeurIPS Learning Meaningful Representation of Life Workshop)

## Installation
```
pip install subword-nmt
git clone this repo
```

## Requirements 
```
pandas
codecs
```

## Usage
```
from subword_nmt.apply_bpe import BPE
import codecs

dataFolder = path to this repo

# For Proteins
vocab_path = dataFolder + '/codes_protein.txt'
bpe_codes_protein = codecs.open(vocab_path)
pbpe = BPE(bpe_codes_protein, merges=-1, separator='')
sub_csv = pd.read_csv(dataFolder + '/subword_units_map_protein.csv')

idx2word_p = sub_csv['index'].values
words2idx_p = dict(zip(idx2word_p, range(0, len(idx2word_p))))

# For Drugs
vocab_path = dataFolder + '/codes_drug.txt'
bpe_codes_drug = codecs.open(vocab_path)
dbpe = BPE(bpe_codes_drug, merges=-1, separator='')
sub_csv = pd.read_csv(dataFolder + '/subword_units_map_drug.csv')

idx2word_d = sub_csv['index'].values
words2idx_d = dict(zip(idx2word_d, range(0, len(idx2word_d))))

# Example: Given Drug SMILES String s, output a bit vector v
t = dbpe.process_line(s).split()  
i = [words2idx_d[i] for i in t]
v = np.zeros(len(idx2word_d), )
v[i] = 1

```

## Repo Files Pointer
### Proteins
1. 'codes_protein.txt' & '/subword_units_map_protein.csv': Using BindingDB target sequences 
2. 'codes_protein_uniprot.txt' & '/subword_units_map_protein_uniprot.csv': Using Uniprot target sequences (~500K) with minimum frequency 500. This generates ~20,000 substructures.
3. 'codes_protein_uniprot_2000.txt' & '/subword_units_map_protein_uniprot_2000.csv': Using Uniprot target sequences (~500K) with minimum frequency 2000. This generates ~4,000 substructures. We find this already has good predictive performance.

### Drugs
1. 'codes_drug.txt' & '/subword_units_map_drug.csv': Using FooDB & DrugBank SMILES strings 
2. 'codes_drug_chembl.txt' & '/subword_units_map_drug_chembl.csv': Using ChEMBL drug SMILES (~2M) with minimum frequency 100. This generates ~23,000 substructures.
3. 'codes_drug_chembl_1500.txt' & '/subword_units_map_drug_chembl_1500.csv': Using ChEMBL drug SMILES (~2M) with minimum frequency 1500. This generates ~2,700 substructures. We find this already has good predictive performance.

## Generate New ESPF with your own Dataset
As ESPF is a data-driven algorithm, we find sometimes it is better to generate a customized partition set for your own dataset in hand.
[Generation](
        ./generate_drug.ipynb
      ): this notebook is a tutorial to generate your own ESPF.

## Cite
```
@article{espf,
author = {Kexin Huang, Cao Xiao, Lucas Glass, Jimeng Sun},
title = {Explainable Substructure Partition Fingerprint for Protein, Drug, and More},
year = {2019},
journal = {NeurIPS Learning Meaningful Representation of Life Workshop},
}

```
