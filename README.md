# sample-test-data
#The following are a set of instructions on how to run the code and all the files.
#This will show all tools and needed to be installed as well as how to use the code in the README #file.

#In order to be able to run the code below one needs to have kallisto, python, and R installed.
#In order to install python one can go to the following link: https://www.python.org/downloads/. 
#Usually a macbook pro has python already in the terminal. All they need to do is type python into #the terminal.
#I am using a server that already has kallisto on it. In case you do not have kallisto one can #install it. 
#For a Macbook you most likely also need to install homebrew as well. You would type the following #into the terminal.
#One would need this: /usr/bin/ruby -e "$(curl -fsSL #https://raw.githubusercontent.com/Homebrew/install/master/install)" and 
#this: brew install kallisto. The R package can be installed by a certain package. This will be #listed further on."
 

#Track 1 was chosen for the Python Pipeline Project.

#Part One
#This part is simply run in the terminal. 

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660030/SRR5660030 #This command retrieves the Donor 1(2dpi) SRA.

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660033/SRR5660033 #This command retrieves the Donor 1(6dpi) SRA.

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660044/SRR5660044 #This command retrieves the Donor 3(2dpi) SRA.

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660045/SRR5660045 #This command retrieves the Donor 3(6dpi) SRA.

ls #This is used to make sure that you have the files.

fastq-dump -I --split-files SRR5660030 #This converts the file to a paired-end fastq file.

fastq-dump -I --split-files SRR5660033 #This converts the file to a paired-end fastq file.

fastq-dump -I --split-files SRR5660044 #This converts the file to a paired-end fastq file.

fastq-dump -I --split-files SRR5660045 #This converts the file to a paired-end fastq file.

#Part Two
#This part is in python. It can be run in the terminal as well. To get python simply type python #on the terminal if you are using a mac.

#This particular section of code prints out the protein id along with the sequence. It also prints #out how many CDS there are. Each CDS is numbered.

from Bio import SeqIO #SeqIO=used for the input and output methods for varying file formats

from Bio import Entrez #Entrez=provides access to NCBI’s database

Entrez.email = "ygomez1@luc.edu" #You put your email here. Needs access to an email in cause NCBI needs to contact you as well as so they can see who is accessing their records.

handle = Entrez.efetch(db="nucleotide", id="NC_006273.2", rettype="gb", retmode="text") #To search for access IDS. efetch gets plain text records from NCBI in Fasta format. The db parameter searches the database Genbank, and rettype returns the parameter as data.

record = SeqIO.read(handle, "genbank")

cds_count = 0 #sets the cds count to 0

for feature in record.features: #For loop for the record.features.

    if feature.type == "CDS":   #If the type of feature is set equal to the CDS then the code below will occur.
    
        protein_id = feature.qualifiers.get("protein_id", [""])[0] #The protein id is set equal to the feature.qualifiers which gives the protein id.
        
        sequence = feature.qualifiers.get("translation", [""])[0]  #The sequence is set equal to the feature.qualifiers which is the translation.
        
        if protein_id and sequence: 
        
            print(">{}\n{}".format(protein_id, sequence)) #Prints out the protein id and the sequence.
            
            cds_count += 1  #There is an increase in the counter variable for cds_count.
            
            print("Number of CDS: {}".format(cds_count)) #Prints the number of CDS.
            
#This particular section of code prints out the RefSeq protein_id as the CDS identifier.   

from Bio import SeqIO #SeqIO=used for the input and output methods for varying file formats

from Bio import Entrez #Entrez=provides access to NCBI’s database

Entrez.email = "ygomez1@luc.edu"  #You put your email here. Needs access to an email in cause NCBI needs to contact you as well as so they can see who is accessing their records.

handle = Entrez.efetch(db="nucleotide", id="NC_006273.2", rettype="gb", retmode="text") #To search for access IDS. efetch gets plain text records from NCBI in Fasta format. The db parameter searches the database Genbank, and rettype returns the parameter as data.

record = SeqIO.read(handle, "genbank")

for feature in record.features: #For loop for the record.features.

    if feature.type == "CDS":   #If the type of feature is set equal to the CDS then the code below will occur.
    
        protein_id = feature.qualifiers.get("protein_id", [""])[0]#The protein id is set equal to the feature.qualifiers which gives the protein id.
        
        sequence = feature.qualifiers.get("translation", [""])[0] #The sequence is set equal to the feature.qualifiers which is the translation.
        
        if protein_id and sequence:
        
            print(len(">{}\n{}".format(protein_id, sequence))) #Prints out the RefSeq protein_id as the CDS identifier.

#This particular section of code savesthe RefSeq protein_id as the CDS identifier to the output file work.fasta.
            
from Bio import SeqIO #SeqIO=used for the input and output methods for varying file formats

from Bio import Entrez #Entrez=provides access to NCBI’s database 

Entrez.email = "ygomez1@luc.edu"  #You put your email here. Needs access to an email in cause NCBI needs to contact you as well as so they can see who is accessing their records.

handle = Entrez.efetch(db="nucleotide", id="NC_006273.2", rettype="gb", retmode="text")#To search for access IDS. efetch gets plain text records from NCBI in Fasta format. The db parameter searches the database Genbank, and rettype returns the parameter as data.

record = SeqIO.read(handle, "genbank") 

with open("work.fasta","w") as output file: #The work.fasta will be opened as an output file.

for feature in record.features:

    if feature.type == "CDS": 
    
        protein_id=feature.qualifiers.get("protein_id", [""])[0] #The protein id is set equal to the feature.qualifiers which gives the protein id.
        
        sequence = feature.qualifiers.get("translation", [""])[0] #The sequence is set equal to the feature.qualifiers which is the translation.
        
        if protein_id and sequence:
        
  output_file.write(">{}\n{}\n".format(protein_id, sequence)) #Saves the work.fasta to the output file.

 kallisto index -i project.idx work.fasta #Makes the index in kallisto.
            
 #Part Three
 #Should use kallisto for the part below. 
time kallisto quant -i project/project.idx #Quantification of TPM for this transcriptome(Donor 1(2dpi) SRA.).
 
-o results/SRR5660030 -b30 -t2

data/SRR5660030.fastq.gz

data/SRR5660030.fastq.gz


time kallisto quant -I project/project.idx #Quantification of TPM for this transcriptome(Donor 1(6dpi) SRA).

-o results/SRR5660033  -b30 -t2

data/SRR5660033.fastq.gz

data/SRR5660033.fastq.gz


time kallisto quant -i project/project.idx  #Quantification of TPM for this transcriptome(Donor 3(2dpi) SRA).

-o results/SRR5660044  -b30 -t2

data/SRR5660044.fastq.gz

data/SRR5660044.fastq.gz


time kallisto quant -i project/project.idx  #Quantification of TPM for this transcriptome(Donor 3(6dpi) SRA).

-o results/SRR5660045  -b30 -t2

data/SRR5660045.fastq.gz

data/SRR5660045.fastq.gz

#This part requires python.

import pandas as pd #Use pandas. 

import numpy as np #Tells you to bring NumPy library into the current environment. The np is to shorten NumPy to np.

samples = ['SRR5660030', 'SRR5660033', 'SR5660044', 'SRR5660045'] #List of the different samples.

dfs = [] #Set dfs equal to an empty list. Holds it as a place value.

for s in samples: 

df = pd.read_csv(f'{s}_abundance.tsv', sep='\t') #read the output file abundance.tsv

df = df[['target_id', 'tpm']] #Sets the dataframe equal to the target id and the tpm.

df = df.rename(columns={'target_id': 'CDS'}) #The columns are specifically the target id and CDS.

dfs.append(df)#Appends the dataframe.


df_all = pd.concat(dfs, axis=1) #The dataframes are concatenated into one.

df_all['min_TPM'] = df_all[samples].min(axis=1) #Calculates the minimum TPM for each CDS.

df_all['max_TPM'] = df_all[samples].max(axis=1) #Calculates the maximum TPM for each CDS.

df_all['mean_TPM'] = df_all[samples].mean(axis=1) #Calculates the mean TPM for each CDS. 

df_all['median_TPM'] = df_all[samples].median(axis=1) #Calculates the median TPM for each CDS.


print(df_all[['CDS', 'min_TPM', 'max_TPM', 'mean_TPM', 'median_TPM']]) #Prints the output


#Part Four
#This is the Sleuth package.

SRR5660030 kallisto results/SRR5660030

SRR5660033 kallisto results/SRR5660033

SRR5660044 kallisto results/SRR5660044

SRR5660045 kallisto results/SRR5660045

#This part requires the use of R.

library(sleuth) #Loads the sleuth package. 

stab=read.table("sleuth_table.txt",header=TRUE,stringsAsFactors=FALSE)#Read in the table for the sleuth package and kallisto output.

so=sleuth_prep(stab) #Causes an initializatino for the sleuth_prep.

so=sleuth_fit(so,~condition,'full')#The two conditions are being compared to see if a model fits. 

so=sleuth_fit(so,~1,'reduced') ) #The likelihood ratio test is compared to the fit reduced model.

so-sleuth_lrt(so,'reduced','full') #The likelihood ratio test between conditions shows differential expression. Gets the differential expressed genes between 2dpi and 6dpi.

library(dplyr) #Loads the dplyr package.

sleuth_table-sleuth_results(so, 'reduced:full','lrt',show_all=FALSE) #Results are taken from sleuth object.

sleuth_significant=dplyr::filter(sleuth_table,qval<=0.05) |>dplyr::arrange(pval) #Filters out the most significant results (FDR<0.05)

write.table(sleuth_significant, file="results.txt",quote=FALSE,row.names=FALSE) #Writes FDR<0.05 to an output file.

#Part Five
