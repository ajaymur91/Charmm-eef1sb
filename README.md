# CHARMM-EEF1SB with gromacs + plumed

# Installation on a compute-cluster, build using spack package manager: 
spack install gromacs@2019.4 +plumed

# or alternatively on a laptop/local machine use conda for 
 conda create --name lugano-tutorials
 conda install -c plumed/label/lugano -c conda-forge plumed
 conda install -c plumed/label/lugano -c conda-forge gromacs
 conda activate lugano-tutorials (use this command everytime before using grimaces +plumed)


cd charmm36-eef1sb

# Create peptide to simulate (using ambertools tleap here)
tleap
  > source leaprc.protein.ff14sb
  > structure = sequence {GLY GLY GLU GLY GLY GLY GLY GLY LYS GLY}
  > savepdb structure structure.pdb

gmx grompp -f inp_files/em.mdp -c conf.gro -o run/em.tpr -p topol.top -pp run/system.top

cd run
gmx make_ndx -f ../structure.pdb
cp index.ndx ../inp_files/
gmx mdrun -deffnm em -table ../inp_files/table-eef1-coul.xvg -tablep ../inp_files/table-eef1-coul.xvg -plumed ../inp_files/plumed-eef1.dat
gmx grompp -f ../inp_files/run.mdp -c confout.gro -o run.tpr -p system.top
gmx mdrun -deffnm run -table ../inp_files/table-eef1-coul.xvg -tablep ../inp_files/table-eef1-coul.xvg -plumed ../inp_files/plumed-eef1.dat 

