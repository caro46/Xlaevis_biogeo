# Models

```
sbatch ~/project/[account_user]/scripts/running_jmodeltest_arg.sh ~/projects/rrg-[storage_account]/[account_user]/Xlaevis_biogeo_project/X_laevis_12_JM.nex
```
`running_jmodeltest_arg.sh`
```bash
#!/bin/sh
#SBATCH --job-name=jmodeltest
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=15
#SBATCH --time=01:00:00
#SBATCH --mem=50gb
#SBATCH --output=jmodeltest.%J.out
#SBATCH --error=jmodeltest.%J.err
#SBATCH --account=[running_account]
#SBATCH --mail-user=[email_adress]
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL

#nexus file as arg1
java -jar ~/project/[account_user]/programs/jmodeltest-2.1.10/jModelTest.jar -d $1 -tr 15 -i -f -g 4 -BIC -AIC -AICc -DT -v -a  
```

# Phylogeny

```
sbatch ~/project/[account_user]/scripts/running_mrbayes.sh ~/projects/rrg-[storage_account]/[account_user]/Xlaevis_biogeo_project/mrbayes_command.txt
```
`running_mrbayes.sh`
```bash
#!/bin/sh
#SBATCH --job-name=mrbayes
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=10
#SBATCH --time=03:00:00
#SBATCH --mem=50gb
#SBATCH --output=mrbayes.%J.out
#SBATCH --error=mrbayes.%J.err
#SBATCH --account=[running_account]
#SBATCH --mail-user=[email_adress]
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL

#mrbayes command file as arg1
module load nixpkgs/16.09  gcc/5.4.0  openmpi/2.0.2 mrbayes/3.2.6
mpirun -np 10 mb $1 
```
`mrbayes_command.txt`
```bash
begin mrbayes;
        set autoclose=yes nowarn = yes;
        execute projects/rrg-[storage_account]/[account_user]/Xlaevis_biogeo_project/X_laevis_12_JM.nex;
        lset nst=6 rates=gamma; #from Jmodeltest
        mcmc nruns=5 Nchains=4 ngen=1000000 samplefreq=1000 relburnin=yes burninfrac=0.25 savebrlens=yes Mcmcdiagn = yes file=projects/rrg-[storage_account]/[account_user]/Xlaevis_biogeo_project/laevis2019_MrBayes.nex;
        sump relburnin=yes burninfrac=0.25;
        sumt relburnin=yes burninfrac=0.25;
end;
```
