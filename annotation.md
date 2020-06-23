# Annotation Workflow

## Funannotate Annotation

### Installation
Check for conda:  
`conda`  

Configure channels:  
`conda config --add channels defaults`  
`conda config --add channels bioconda`  
`conda config --add channels conda-forge`  

Create conda environment with funannotate software:  
`conda create -n fun python=2.7 funannotate`  

Enter environment:  
`conda activate fun`  

Setup Funannotate:  
`funannotate setup -d /share/rwmwork/cbadger/A.rabiei/Funannotate/funannotate_db`  

Paths:  
`echo "export FUNANNOTATE_DB=$HOME/funannotate_db" > /home/cbadger/.conda/envs/fun/etc/conda/activate.d/funannotate.sh`  
`echo "unset FUNANNOTATE_DB" > /home/cbadger/.conda/envs/fun/etc/conda/deactivate.d/funannotate.sh`  

Add this line to .profile file (for database location):  
`export FUNANNOTATE_DB=/share/rwmwork/cbadger/A.rabiei/Funannotate/funannotate_db`  

Source profile:  
`source /home/cbadger/.profile`  

Install dependency eggnog-mapper:  
`conda install -c bioconda eggnog-mapper`  

Install dependency genemark-ES:  
* Link: http://topaz.gatech.edu/GeneMark/license_download.cgi  
* Version: GeneMark-ES/ET/EP ver 4.57_lic LINUX 64  
* Fill out info, get email confirmation/download link, use `wget` in `/home/cbadger/.conda/envs/fun/bin/`  
* Downloaded Key, unzipped, moved to ~/ and renamed it .gm_key  
* Edited gmes_petap.pl shebang to have `#!/home/cbadger/.conda/envs/fun/bin/perl` (location of perl executables for conda env)  
Export GeneMark-ES Paths:  
`export GENEMARK_PATH=/home/cbadger/.conda/envs/fun/bin/gmes_linux_64/gmes_petap.pl`  
`export PATH=/home/cbadger/.conda/envs/fun/bin/gmes_linux_64:$PATH`  

Load dependency signalp:  
`module load signalp/4.1c`  

Check dependencies:  
`funannotate check --show-versions`  
___
### Test Run
Test run:  
`nohup funannotate test -t all --cpus 32 &`  
___
### Run 1 - No RNAseq
__DEPENDENCIES__  

Make sure profile is loaded:  
`source /home/cbadger/.profile`  

Enter conda environment:  
`conda activate fun`  

Load dependencies:  
`module load signalp/4.1c`  

GeneMark-ES Paths:  
`export GENEMARK_PATH=/home/cbadger/.conda/envs/fun/bin/gmes_linux_64/gmes_petap.pl`  
`export PATH=/home/cbadger/.conda/envs/fun/bin/gmes_linux_64:$PATH`  

Check dependencies:  
`funannotate check --show-versions`  

__PROGRAMS__  

Sorting contigs:  
`nohup funannotate sort -i Genome/Arabiei40x.contigs.fasta -b contig -o Genome/Phoma.genome.sorted.fa &`  

Softmasking genome: (Softmasked 3.02% of genome)  
`nohup funannotate mask -i Genome/Phoma.genome.sorted.fa --cpus 12 -o Genome/Phoma.genome.sorted.masked.fa &`  

Gene prediction:  
`nohup funannotate predict -i Run1/Phoma.genome.sorted.masked.fa -o Run1 --species "Phoma species" --protein_evidence $FUNANNOTATE_DB/uniprot_sprot.fasta --cpus 12 &`  

AntiSMASH:
`funannotate remote -i Run1 -m antismash -e email@gmail.com`

Interproscan:  
`conda deactivate`  
`module load interproscan/5.30-69.0`  
`export IPS_CONFIG_DIR=/software/interproscan/5.30-69.0/static/`  
`nohup interproscan.sh --input /share/rwmwork/cbadger/A.rabiei/Funannotate/Run1/predict_results/Phoma_species.proteins.fa --output-dir /share/rwmwork/cbadger/A.rabiei/Funannotate/Run1/Interproscan_results --tempdir /share/rwmwork/cbadger/A.rabiei/Funannotate/Run1/Interproscan_results/temp --formats XML --goterms --pathways &`  

Annotation generation:  
`nohup funannotate annotate -i Run1/predict_results --iprscan Run1/Interproscan_results/Phoma_species.proteins.fa.xml --antismash Run1/fungi-98f7ba83-e1c8-4710-9f8a-c7401785203a/Phoma_species.gbk --cpus 12 &`  
___
### Run 2 - Phoma multirostrata RNAseq
__ACCESSING RNASEQ DATA__  
1. Go to www.ncbi.nlm.nih.gov  
2. Choose SRA database 
3. Search for organism (Phoma multirostrata in my case)  
4. Checkbox run of interest (Accession#: SRX5084377)  
5. Send To: Run Selector  
6. Copy Run Accession # (SRR8267448) 
7. On Cluster:
* `module load sratoolkit/2.10.5`
* `vdb-config --interactive`
* Enable remote access [X]
* `nohup prefetch SRR8267448 &`
* `nohup fastq-dump SRR8267448.sra &`

To Download Directly to Computer (wget not permitted):  
7. Go to Run Browser: https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=run_browser  
8. Search using Run Accession (SRR8267448)  
9. Click 'Reads' tab, 'Filtered Download', Download Format 'Fasta', then 'Download'  

__PROGRAMS__  
Sorting contigs:  
`nohup funannotate sort -i Genome/Arabiei40x.contigs.fasta -b contig -o Genome/Phoma.genome.sorted.fa &`  

Softmasking genome: (Softmasked 3.02% of genome)  
`nohup funannotate mask -i Genome/Phoma.genome.sorted.fa --cpus 12 -o Genome/Phoma.genome.sorted.masked.fa &`

Moved sorted softmasked genome from Run1 to Run2  
Training step:  
`nohup funannotate train -i Run2/Phoma.genome.sorted.masked.fa -o Run2 --single Evidence/SRR8267448/SRR8267448.fastq --jaccard_clip --species "Phoma species" --cpus 12 &`
