# OrthoFinder
This is documentation of my OrthoFinder analysis, not a source repository.  
For installing and running OrthoFinder, see https://github.com/davidemms/OrthoFinder  

## Download  
`cd Comparative_genomics`  
`wget https://github.com/davidemms/OrthoFinder/releases/latest/download/OrthoFinder.tar.gz`  
`tar -xvzf OrthoFinder.tar.gz`  

## Example Run  
`OrthoFinder/orthofinder -f OrthoFinder/ExampleData`  
No apparent issues.  

## Pleosporales Run
Checking hard and soft limits for opening files:  
`ulimit -Hn`  
1048576  
`ulimit -Sn`  
1024  
Updating soft limit to be over 1256:  
`ulimit -n 1256`  

Run Command:  
`nohup OrthoFinder/orthofinder -t 32 -f OrthoFinder/Pleosporales &`  
