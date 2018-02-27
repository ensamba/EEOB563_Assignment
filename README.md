# No.3
# Sequence alignment
mafft --auto cob_aa.fasta > cob_nt_aln
mafft --phylipout cob_nt_aln > cob_nt.phylip

# Raxml command options
raxmlHPC-PTHREADS-SSE3 -T2 -m GTRCAT -p 12345 -s cob_nt.phylip -# 20 -n T30

Substitution Model: GTRCAT; 
GTRCAT is a speed improvement for modeling rate variation under the GTRGAMMA model. It is particularly designed for modeling rate heterogeneity across very large trees and it produces trees with better scores. I saw it also process the command pretty quickly.
Input sequence file: cob_nt.phylip
Random number: 12345
Output file: T30
#20 : This command instructed raxml to carry out 20 ML searches on 20 randomized stepwise addition parsimony trees.


# No.4 
# Sequence alignment
mafft —auto —phyliout cob_aa.fasta > cob_aa.phylip

# Raxml command options
raxmlHPC -p 12345 -m PROTGAMMAAUTO -s cob_aa.phylip  -n Cob_AUTO

Substitution Model: PROTGAMMAAUTO
I used PROTGAMMAAUTO model because it is the best protein model with respect to the likelihood and can be quickly determined by raxml.

# No.5
PBS job script for cob_nt.phylip
#!/bin/bash

#copy/paste this job script into a text file and submit with the command:
#    sbatch thefilename

#SBATCH --time=4:00:00   # walltime limit (HH:MM:SS)
#SBATCH --nodes=1   # number of nodes
#SBATCH --ntasks-per-node=16   # 32 processor core(s) per node 
#SBATCH --mem=62G   # maximum memory per node
#SBATCH --job-name="my_raxml"
#SBATCH --mail-user=ensamba@iastate.edu   # email address
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL
#SBATCH --output="raxml.out6" # job standard output file (%j replaced by job id)

# LOAD MODULES, INSERT CODE, AND RUN YOUR PROGRAMS HERE
module load raxml
raxmlHPC-PTHREADS-SSE3 -f a -m GTRGAMMA -p 12345 -x 12345 -# 1000 -s cob_nt.phylip -n cob_nt_bootstrap

#Run time
SLURM Job_id=21128 Name=my_raxml Ended, Run time 00:25:06, COMPLETED, ExitCode 0


PBS job script for cob_aa.phylip
#!/bin/bash

# Copy/paste this job script into a text file and submit with the command:
#    sbatch thefilename

#SBATCH --time=4:00:00   # walltime limit (HH:MM:SS)
#SBATCH --nodes=1   # number of nodes
#SBATCH --ntasks-per-node=16   # 32 processor core(s) per node 
#SBATCH --mem=62G   # maximum memory per node
#SBATCH --job-name="my_raxml"
#SBATCH --mail-user=ensamba@iastate.edu   # email address
#SBATCH --mail-type=BEGIN
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL
#SBATCH --output="raxml.out4" # job standard output file (%j replaced by job id)

# LOAD MODULES, INSERT CODE, AND RUN YOUR PROGRAMS HERE
module load raxml
raxmlHPC-PTHREADS-SSE3 -T16 -p 12345 -x 12345 -# 1000 -f a -m PROTGAMMAAUTO -s cob_aa.phylip -n Cob_AUTO.bootstrap4

#Run time
SLURM Job_id=21130 Name=my_raxml Ended, Run time 00:20:51, COMPLETED, ExitCode 0





