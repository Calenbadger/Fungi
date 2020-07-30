# Comparative Genomics
## OrthoMCL
### Installation
conda create --name orthomcl  
conda activate orthomcl  
conda install -c bioconda orthomcl  
conda update orthomcl  

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

cp /home/cbadger/.conda/envs/orthomcl/share/orthomcl/orthomcl.config.template /share/rwmwork/cbadger/A.rabiei/Comparative_genomics/OrthoMCL/orthomcl.config
cpanm BioPerl DBD::mysql DBI Parallel::ForkManager YAML::Tiny Set::Scalar Text::Table Exception::Class Test::Most Test::Warn Test::Exception Test::Deep Moose SVG Algorithm::Combinatorics  

#### Errors and Failures
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
