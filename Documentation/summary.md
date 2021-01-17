---
sort: 2
---

# `gmx_MMPBSA` in a nutshell
`gmx_MMPBSA` brings in all the [MMPBSA.py](https://pubs.acs.org/doi/10.1021/ct300418h) functionalities to GROMACS users. 
In addition, few other functionalities were implemented that eases a number of calculations (_e.g._ MM/PB(GB)SA 
with different internal dielectric constant, interaction entropy calculation). A GUI application is also incorporated 
that allows for visualizing the results and saving high-quality images.

## Types of calculations you can do
There are many different options for running `gmx_MMPBSA`. Among the types of calculations you can do are:
* **Normal binding free energies**, with either PB or GB implicit solvent models. Each can be done with either
1, 2, or 3 different trajectories, but the complex, receptor, and ligand topology files must all be defined. The
complex trajectory must always be provided. Whichever trajectories of the receptor and/or ligand that are NOT
specified will be extracted from the complex trajectory. This allows a 1-, 2-, or 3-trajectory analysis. All PB
calculations and GB models can be performed with just AmberTools via the mmpbsa_py_energy program installed with 
MMPBSA.py.
* **Stability** calculations with any calculation type.
* **Alanine scanning** with either PB or GB implicit solvent models. All trajectories will be mutated to match
the mutated topology files, and whichever calculations that would be carried out for the normal systems are
also carried out for the mutated systems. Note that only 1 mutation is allowed per simulation, and it must
be to an alanine. If mutant_only is not set to 1, differences resulting from the mutations are calculated. This
option is incompatible with intermediate NetCDF trajectories (see the netcdf = 1 option above). This has the
same program requirements as option 1 above.
* **Entropy corrections**. An entropy term can be added to the free energies calculated above using either the
quasi-harmonic, the normal mode or interaction entropy approximations. Calculations will be performed for the normal 
and mutated systems (alanine scanning) as requested. Normal mode calculations are done with the
mmpbsa_py_nabnmode program included with AmberTools.
* **Decomposition schemes**. The energy terms will be decomposed according to the decomposition scheme
outlined in the idecomp variable description. This should work with all of the above, though entropy terms
cannot be decomposed. APBS energies cannot be decomposed, either. Neither can PBSA surface area terms.
This functionality requires sander from the Amber 11 (or later) package.
* **QM/MMGBSA**. This is a binding free energy (or stability calculation) using the Generalized Born solvent
model allowing you to treat part of your system with a quantum mechanical Hamiltonian. See [“Advanced
Options”](advanced#Advanced-Options) for tips about optimizing this option. 
This functionality requires sander from the Amber package.
* **MM/3D-RISM**. This is a binding free energy (or stability calculation) using the 3D-RISM solvation model.
This functionality is performed with rism3d.snglpnt built with AmberTools.
* **Membrane Protein MMPBSA**. Calculate the MMPBSA binding free energy for a ligand bound to a protein
that is embedded into a membrane. Only use_sander=1 is supported.
  
## Comparison of `gmx_MMPBSA` vs other programs
This comparison is based on the documentation of the different programs

| Feature | [g_mmpbsa](https://github.com/RashmiKumari/g_mmpbsa) | [MMPBSA.py](https://ambermd.org/doc12/Amber20.pdf#chapter.34) <sup>1</sup> | [gmx_MMPBSA](https://github.com/Valdes-Tresanco-MS/gmx_MMPBSA/) |
|:---|:---:|:---:|:---:|
| **Normal binding free energies**| PB | PB and GB  | PB and GB |
| GB models | None | 1, 2, 3, 5, 7 and 8  | 1, 2, 3, 5, 7 and 8 |
| **Stability** |  | :heavy_check_mark: | :heavy_check_mark: |
| **Alanine scanning** | :heavy_check_mark: <sup>2</sup>| :heavy_check_mark: | :heavy_check_mark: |
| **Entropy corrections** <sup>3</sup>|   | nmode and qh | nmode, qh, and IE |
| **Decomposition schemes** | Per-Residues | Per-Residues and Per-Wise | Per-Residues and Per-Wise |
| **QM/MMGBSA** |   | :heavy_check_mark: | :heavy_check_mark: |
| **MM/3D-RISM** |   | :heavy_check_mark: | :heavy_check_mark: |
| **Membrane Protein MMPBSA** |      | :heavy_check_mark: | :heavy_check_mark: |
| **GROMACS Version** | 4.x, 5.x and 2016+ <sup>4</sup> |  --- | 4.x, 5.x and 20xx.x |
| **Approximations** | ST | ST and MT | ST and MT |
| **API** |      | :heavy_check_mark: | :heavy_check_mark: |
| **Graphical Analyzer** |    |  | :heavy_check_mark: |
| Energy to PDB | :heavy_check_mark: |     | :heavy_check_mark: |
| Energetic Terms charts | Per-Frame | Average and/or Per-Frame <sup>5</sup>  | Average and Per-frame |
| Energetic Terms charts representation | xmgrace/matplotlib/gnuplot | API and graphics library | gmx_MMPBSA_gui |
| **Externals programs** | APBS (1.2.x, 1.3.x or 1.4.x) |  AmberTools20 | AmberTools20 |
| **Parallel computation** | Depends on APBS version | :heavy_check_mark: | :heavy_check_mark: |
| **Steps for:** | | | |
| Calculation and Summary | Multiple | One | One |
| Analysis| Multiple | Multiple | One |

<sup>1</sup> MMPBSA.py is included in AMBER package.

<sup>2</sup> Without documentation.

<sup>3</sup> nmode = Normal modes approximation, qh = Quasic-Harmony approximation and IE = Interaction Entropy
approximation

<sup>4</sup> GROMACS 20xx.x is not officially supported. There is a Pull Request that offers a minimum of compatibility 
with versions higher than 2016.x but with limitations 

<sup>5</sup> The user can obtain each energetic term per frame or its average values using the API. This means that
 user must be familiar with Python to handle the API, perform custom calculations or graph such data.
