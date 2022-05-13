# TaxID to Taxonomy
If you have a file or lists with a number of taxonomic ID or TaxID, this command will produce taxonomic information from kingdom to Species and origins addition to that tax IDS.

## 1. TaxID to taxonomy assignment
Details can be find here  https://www.ncbi.nlm.nih.gov/books/NBK179288/ 

###  1.1. First, Install Entrez: E-utilities on the Unix Command Line

### Installation
EDirect will run on Unix and Macintosh computers, and under the Cygwin Unix-emulation environment on Windows PCs. To install the EDirect software, open a terminal window and execute one of the following two commands:

  `sh -c "$(curl -fsSL ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/install-edirect.sh)"`

  `sh -c "$(wget -q ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/install-edirect.sh -O -)"`
This will download a number of scripts and several precompiled programs into an "edirect" folder in the user's home directory. It may then print an additional command for updating the PATH environment variable in the user's configuration file. The editing instructions will look something like:

 `echo "export PATH=\$PATH:\$HOME/edirect" >> $HOME/.bash_profile`
As a convenience, the installation process ends by offering to run the PATH update command for you. Answer "y" and press the Return key if you want it run. If the PATH is already set correctly, or if you prefer to make any editing changes manually, just press Return.

`export PATH=${PATH}:${HOME}/edirect`

to set the PATH for the current terminal session. now we can run taxonomy assignment

### 1.2. TaxID list
You may need to create taxid list first and save  use it as input in next command

`awk '{print $4}' BLAST_OUTPUT_FILE > taxid_list`  # here number 4 is the taxid column in my blast output file, specify yours

details can find here
https://www.biostars.org/p/346343/
https://www.biostars.org/p/423201/

### 1.3. Assign taxonomy 
Assign to each tax ID from the list avobe

`cat taxid_list | while read line; do efetch -db taxonomy -id $line -format xml | \
xtract -pattern Taxon -first TaxId -element Taxon -block "*/Taxon"  \
-unless Rank -equals "no rank" -tab "," -sep "_" -element Rank,ScientificName; done > taxonomy.txt`

### 1.4 Merged the BLAST and Taxonomy file
If they are equal in length (no. of rows are equal), you can run following command

`paste BLAST_OUTPUT_FILE  taxonomy.txt > MERGED.txt`

One can combine all the command lines and execute in the terminal or in server terminal

### END
