# Bio2byte :: PeptideBuilder

This repository is a fork of **PeptideBuilder: A simple Python library to generate model peptides**
by *Matthew Z. Tien*, *Dariya K. Sydykova*, *Austin G. Meyer*, and *Claus O. Wilke*.

## Installation

You can install bio2byte-peptidebuilder directly from GitHub:

```sh
# Create a virtual environment (optional)
python -m venv peptidebuilder
source peptidebuilder/bin/activate

# Install directly from GitHub
pip install git+https://github.com/Bio2Byte/bio2byte-peptidebuilder
```

## Usage

A simple example is shown below. Other examples can be found in the `examples/` folder.

```python
# Import the library
from Bio.PDB import PDBIO
import bio2byte.PeptideBuilder as PeptideBuilder
from bio2byte.PeptideBuilder import Geometry

# Dihedral angles for an alpha-helix
helix_phipsi = (-60., -40.)

# Sequence (case insensitive)
sequence = "LYS-GLY-GLU-ARG-GLN-SEP-ALA-VAL-ASP-ILE-ASP".split("-")

# Build an alpha-helical peptide with N-terminal acetyl and
# C-terminal N-methyl capping groups
structure = PeptideBuilder.initialize_ACE()
for res in sequence:
    geo = Geometry.geometry(res)
    geo.phi, geo.psi_im1 = helix_phipsi
    PeptideBuilder.add_residue(structure, geo)
PeptideBuilder.add_terminal_NME(structure)

# Write to PDB file
pdbwriter = PDBIO()
pdbwriter.set_structure(structure)
pdbwriter.save("peptide.pdb")
```

## Added features

### Version 1.2.0

* Include new residue geometries
    * SEP (phosphoserine)
    * TPO (phosphothreonine)
    * PTR (phosphotyrosine)
* Include new initialization/termination residues
    * ACE (N-terminal acetyl group)
    * NME (C-terminal *N*-methyl group)
    * NH2 (C-terminal amide group)
* Allow three-letter residue names in `geometry` funtion
* Allow alternative residue names for protonation states:
    * ASH (neutral aspartatic acid)
    * GLH (neutral glutamic acis)
    * HIP (protonated histidine)
    * S1P (protonated phosphoserine, net charge = -1)
    * T1P (protonated phosphothreonine, net charge = -1)
    * Y1P (protonated phosphotyrosine, net charge = -1)

## Misc

The software is provided to you under the MIT license (see file `LICENSE.txt`).
The most up-to-date version of this software is available at
https://github.com/clauswilke/PeptideBuilder.

To test whether your installation works properly, run `pytest` in the top-level project folder.

## Reference

Please cite the original package:

> M. Z. Tien, D. K. Sydykova, A. G. Meyer, C. O. Wilke (2013). PeptideBuilder:
> A simple Python library to generate model peptides. PeerJ 1:e80.
