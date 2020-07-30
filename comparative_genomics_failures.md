# Comparative Genomics
## OrthoMCL
### Installation
conda create --name orthomcl  
conda activate orthomcl  
conda install -c bioconda orthomcl  
conda update orthomcl  


 

cp /home/cbadger/.conda/envs/orthomcl/share/orthomcl/orthomcl.config.template /share/rwmwork/cbadger/A.rabiei/Comparative_genomics/OrthoMCL/orthomcl.config  

**For OrthoMCL Pipeline (Automation, not actually OrthoMCL)** 
Perl Modules to Install:  
BioPerl  
DBD::mysql  
DBI  
Parallel::ForkManager  
Schedule::DRMAAc  
YAML::Tiny  
Set::Scalar  
Text::Table  
Moose  
SVG  
Algorithm::Combinatorics  

#### Errors and Failures
cpanm BioPerl DBD::mysql DBI Parallel::ForkManager YAML::Tiny Set::Scalar Text::Table Exception::Class Test::Most Test::Warn Test::Exception Test::Deep Moose SVG Algorithm::Combinatorics  
Catastrophic failure  

Attempt 2: 7-30-2020  
conda env remove --name orthomcl  
rm -r /home/cbadger/.conda/envs/orthomcl  
conda create --name orthomcl  
conda install gxx_linux-64  
conda config --add channels defaults  
conda config --add channels bioconda  
conda config --add channels conda-forge  
conda install -c bioconda orthomcl  
cpanm Failure again  

cpanm Bio::SeqIO  
DB_File Failed  
XML::DOM::XPath Failed  
PadWalker Failed  
DBI failed  
Configure failed for Moose-2.2013  
Algorithm::Combinatorics failed  
Installing the dependencies failed: Module 'Test::Memory::Cycle' is not installed, Module 'XML::LibXML' is not installed, Module 'DB_File' is not installed, Module 'XML::LibXML::Reader' is not installed, Module 'XML::DOM::XPath' is not installed  

cpanm Bio::SeqIO Failed  
cpanm DB_File Failed  

Ran this - possible requirement for DB_File  
conda install -c bioconda libdb  
Did not fix  

### Data/Input
### Running
