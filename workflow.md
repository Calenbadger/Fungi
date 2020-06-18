## Phoma/Unknown Fungal Genomics Full Workflow
### Pacbio Data
### Genome Assembly
### Genome Annotation
### Phylogenomics
#### BUSCO Phylogenomics 
Reading BUSCO directories and generating species-based and gene-based fasta files

For protein: `python3 geneset_busco.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Output --key run_ --species /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Protspecies --prot /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots`

For nucleotide: `python3 geneset_busco.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Output --key run_ --species /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucspecies --prot /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs`

Running ClustalW for multiple sequence alignment on by-gene fasta files

For protein: `python3 super_clustal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Protalns`

For nucleotide: `python3 super_clustal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucalns`

Running Trimal for MSA trimming on ClustalW result files

Enter environment with trimAl v1.4.rev15 build[2013-12-17]: `conda activate phylo`

For protein: `python3 super_trimal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Protalns --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prottrims`

For nucleotide: `python3 super_trimal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucalns --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nuctrims`

Concatenating trimmed alignments into supersequences for each species

For protein: `python3 supersample.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prottrims --super /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Raxml/Phylip --phy`

For nucleotide: `python3 supersample.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nuctrims --super /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Pyani/Inputs/Super36 --ani`
