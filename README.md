# CHARMM-EEF1SB with gromacs + plumed
Installation on a compute-cluster, build using spack package manager: 
```
spack install gromacs@2019.4 +plumed
```
# or alternatively on a laptop/local machine use conda (quick lousy installation) 
```
 conda create --name lugano-tutorials
 conda install -c plumed/label/lugano -c conda-forge plumed
 conda install -c plumed/label/lugano -c conda-forge gromacs
 conda activate lugano-tutorials (use this command everytime before using grimaces +plumed)
```
# Usage 
```
git clone https://github.com/ajaymur91/Charmm-eef1sb.git
cd Charmm-eef1sb
```

# Create peptide to simulate (using ambertools: tleap)
```
tleap
  > source leaprc.protein.ff14sb
  > structure = sequence {GLY GLY GLU GLY GLY GLY GLY GLY LYS GLY}
  > savepdb structure structure.pdb
```
# Create a standalone topology file and a gromacs run file inside run/em.tpr 
```
gmx pdb2gmx -f structure.pdb -ignh
gmx grompp -f inp_files/em.mdp -c conf.gro -o run/em.tpr -p topol.top -pp run/system.top
```
# Steps to run
```
cd run
gmx make_ndx -f ../structure.pdb
cp index.ndx ../inp_files/
gmx mdrun -deffnm em -table ../inp_files/table-eef1-coul.xvg -tablep ../inp_files/table-eef1-coul.xvg -plumed ../inp_files/plumed-eef1.dat
gmx grompp -f ../inp_files/run.mdp -c confout.gro -o run.tpr -p system.top
gmx mdrun -deffnm run -table ../inp_files/table-eef1-coul.xvg -tablep ../inp_files/table-eef1-coul.xvg -plumed ../inp_files/plumed-eef1.dat 
```
# Credits 
https://www.plumed.org/doc-v2.5/user-doc/html/isdb-1.html

https://github.com/sbottaro/EEF1-SB

Reference: @article{bottaro2013variational, title={Variational optimization of an all-atom implicit solvent force field to match explicit solvent simulation data}, author={Bottaro, Sandro and Lindorff-Larsen, Kresten and Best, Robert B}, journal={Journal of chemical theory and computation}, volume={9}, number={12}, pages={5641--5652}, year={2013}, publisher={ACS Publications} }
