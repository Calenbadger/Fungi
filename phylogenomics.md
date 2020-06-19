# Phylogenomics Workflow
## BUSCO Phylogenomics 
#### Data Download/Generation
__36 Genomes__ from https://genome.jgi.doe.gov/portal/pages/dynamicOrganismDownload.jsf?organism=pleosporales 
`Alternaria_alternata_MPI_PUGE_AT_0064_Assembly,
Alternaria_alternata_SRC1lrK2f_Assembly,
Alternaria_rosae_Assembly,
Ascochyta_rabiei_ArDII_Assembly,
Ascochyta_rabiei_Me14_Assembly,
Boeremia_exigua_Assembly,
Clathrospora_elynae_Assembly,
Cochliobolus_carbonum_Assembly,
Cochliobolus_heterostrophus_C4_Assembly,
Cochliobolus_heterostrophus_C5_Assembly,
Cochliobolus_lunatus_Assembly,
Cochliobolus_miyabeanus_Assembly,
Cochliobolus_sativus_Assembly,
Cochliobolus_victoriae_Assembly,
Cucurbitaria_berberidis_Assembly,
Didymella_exigua_Assembly,
Dothidotthia_symphoricarpi_Assembly,
Fenestella_fenestrata_Assembly,
Leptosphaeria_maculans_JN3,
Lizonia_empirigonia_Lizem1,
Macroventuria_anomochaeta_Assembly,
Ophiobolus_disseminans_Assembly,
Paraphoma_chrysanthemicola_Assembly,
Parastagonospora_nodorum_ASM226702v1,
Phaeosphaeria_poagena_Assembly,
Phoma_multirostrata_Assembly,
Phoma_tracheiphila_Assembly,
presence_absence.csv
Pyrenochaeta_lycopersici_Assembly,
Pyrenochaeta_sp_Assembly,
Pyrenophora_teras_f_teres_PTT_W1_1,
Pyrenophora_tritici_repentis_Ptr_V0001,
Setosphaeria_turcica_Et28A_Assembly,
Setosphaeria_turcica_NY001_Assembly,
Stagonospora_sp_Assembly,
Stemphylium_lycopersici_Assembly,
Unknown_sp_Assembly`

__Running BUSCO on the 36 genomes__

    for file in genomelist  
    do  
    echo Starting $file  
    python /software/busco/3.0.2/lssc0-linux/scripts/run_BUSCO.py -i $file -o ${file/fasta/busco_asco.out} -l /share/rwmwork/cbadger/A.rabiei/Quality_check/Assembly/Busco/Datasets/ascomycota_odb9 -m geno
    echo Finished $file
    done

#### Command Workflow
__Reading BUSCO directories and generating species-based and gene-based fasta files__  
For protein: `python3 geneset_busco.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Output --key run_ --species /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Protspecies --prot /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots`  
For nucleotide: `python3 geneset_busco.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Output --key run_ --species /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucspecies --prot /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs`

__Running ClustalW for multiple sequence alignment on by-gene fasta files__  
For protein: `python3 super_clustal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Protalns`  
For nucleotide: `python3 super_clustal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucalns`

__Running Trimal for MSA trimming on ClustalW result files__  
Enter environment with trimAl v1.4.rev15 build[2013-12-17]: `conda activate phylo`  
For protein: `python3 super_trimal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Protalns --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prottrims`  
For nucleotide: `python3 super_trimal.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs --aln /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucalns --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nuctrims`

__Concatenating trimmed alignments into supersequences for each species__  
For protein: `python3 supersample.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prots --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Prottrims --super /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Raxml/Phylip --phy`  
For nucleotide: `python3 supersample.py --dir /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nucs --trim /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Busco/Nuctrims --super /share/rwmwork/cbadger/A.rabiei/Phylogenomics/Pyani/Inputs/Super36 --ani`
