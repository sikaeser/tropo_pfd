<p align="center">
Tropolone and PFD Potential Energy Surfaces<br>
Meuwly Group, University of Basel
</p>

# General
The present repository provides access to the raw data and potential energy surfaces
for tropolone and (propiolic acid)–(formic acid) dimer (PFD), which are described in detail
in Reference [1]. The PESs are obtained following a rational procedure based on transfer
learning to reach CCSD(T) quality for large molecules and are based on PhysNet [2].


This repository contains instructions for the installation and dependencies, a description of the different PESs and corresponding raw data. This is followed by examples on using the neural network-based PESs. The explicity examples on how to use the PESs are based on tropolone; however, the same evaluations can be done for PFD, too. The ab initio raw data is available in *tropolone/raw_data* and *pfd/raw_data*.


# Installations & Dependencies

The following installation steps were tested on a Ubuntu 20.04 workstation and using
Conda 23.7.2 (see https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).
The installation of the dependencies (excluding the installation of Miniconda) takes
less than 5 min. 

a) If not installed already, install Miniconda on your machine (see https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html)

b) Create an environment named (e.g.) physnet_env, install Python 3.6:

    conda create --name physnet_env python=3.6

   Activate it:

    conda activate physnet_env

    (deactivating it by typing: conda deactivate)


c) With activated environment, all dependencies can be installed.

    pip install ase==3.19.1
    pip install tensorflow==1.12
    pip install tensorflow_estimator


# Potential Energy Surfaces

The PESs for both systems are obtained using transfer learning (TL): TL builds on the
knowledge acquired by solving one task (representing a lower level PES) to solve a new, 
related task (representing a higher level PES). This encompasses training a lower level model
on quantum chemical reference data that is rapid to evaluate (here we used MP2/cc-pVTZ).
This allows us to generate large datasets with structures that span the entire (relevant)
configuration space. Then, the optimal set of weights and biases for the low-level data
is retrained based on very small amouts of high level ab initio data (here CCSD(T)/aug-cc-pVTZ).

The raw ab initio data (Molpro outputs) are given for both the MP2 and the CCSD(T) level of 
theory in *tropolone/raw_data* and *pfd/raw_data*. For tropolone this includes 19997 MP2 and 
100 CCSD(T) data points while for PFD this includes 40290 (this also includes monomer structures)
MP2 and 406 (106 PFD + 300 monomer structures) CCSD(T) data points. 

Due to the rather small high-level data set sizes, TL was repeated 10 times on different (random)
splits of the data. For each of the systems, the 10 resulting TL models are provided in 
*Tropolone/evaluation/models_tropo_tlcc/* and *PFD/evaluation/models_pfd_tlcc/*, respectively.
The corresponding MP2 level models are also provided in *Tropolone/evaluation/models_tropo_mp2/* 
and *PFD/evaluation/models_pfd_tlcc/*


# Examples

### Evaluations in Python/ASE
Most Python scripts that are used to evaluate the PhysNet PESs make use of the atomic simulation environment (ASE) [3] and are written in Python. It is important to get used to ASE, which has very good tutorials online (https://wiki.fysik.dtu.dk/ase/tutorials/tutorials.html#ase). Scripts on how to use the PESs that have been used throughout the evalulation of Reference [1] are given in the *evaluation* folder. These can for example be used to 

- predict the energy of a given structure in .xyz format (predict_mol.py)

    ./predict_mol.py -i min-tropo.xyz
    
- or to optimize a given structure in .xyz format (optimize.py)

    ./optimize.py -i min-tropo.xyz -o opt_min-tropo.xyz --fmax 0.000005
    
- or to calculate the harmonic frequencies of an optimized molecule (NN_NM.py).

    ./NN_NM.py -i opt_min-tropo.xyz

To allow reproduction of the results, a text file (*Tropolone/evaluation/outputs_for_reproduction* and *PFD/evaluation/outputs_for_reproduction*) is available for exemplary observables (energy barrier for hydrogen transfer, harmonic frequencies, ...).

The models and evaluation scripts are also available for PFD, see *PFD/evaluation*.

# How to cite 

When using the PhysNet or the Tropolone/PFD PESs, please cite the following papers:

#### For PhysNet:
Oliver T. Unke and Markus Meuwly "PhysNet: A Neural Network for Predicting Energies,
Forces, Dipole Moments, and Partial Charges", J. Chem. Theory Comput., 2019,
15, 6, 3678–3693

#### For the Tropolone/PFD PESs
Silvan Käser, Jeremy Richardson, and Markus Meuwly "Accurate Tunneling Splittings for Ever-Larger Molecules from Transfer-Learned, CCSD(T) Quality Energy Functions" https://arxiv.org/abs/2407.21366



# References
[1] Silvan Käser, Jeremy Richardson, and Markus Meuwly "Accurate Tunneling Splittings for Ever-Larger Molecules from Transfer-Learned, CCSD(T) Quality Energy Functions" https://arxiv.org/abs/2407.21366

[2] Oliver T. Unke, and Markus Meuwly "PhysNet: A Neural Network for Predicting Energies, Forces, Dipole Moments, and Partial Charges" J. Chem. Theory Comput. 2019, 15, 6, 3678–3693

[3] Ask Hjorth Larsen et al, "The atomic simulation environment—a Python library for working with atoms", 2017, J. Phys.: Condens. Matter, 29, 273002,  DOI 10.1088/1361-648X/aa680e

# Contact

If you have any questions about the codes free to contact Prof. Markus Meuwly (m.meuwly@unibas.ch)


