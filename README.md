# sample-test-data
The following are a set of instructions on how to run the code and all the files.
This will show all tools and needed to be installed as well as how to use the code in the README file.

In order to be able to run the code below one needs to have kallisto, python, and R installed.
In order to install python one can go to the following link: https://www.python.org/downloads/. 
Usually a macbook pro has python already in the terminal. All they need to do is type python into the terminal.
I am using a server that already has kallisto on it. In case you do not have kallisto one can install it. 
 For a Macbook you most likely also need to install homebrew as well. You would type the following into the terminal.
 One would need this: /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" and 
 this: brew install kallisto. The R package can be installed by a certain package. This will be listed further on."
 

Track 1 was chosen for the Python Pipeline Project.

Part One
This part is simply run in the terminal. 

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660030/SRR5660030 #This command retrieves the Donor 1(2dpi) SRA.

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660033/SRR5660033 #This command retrieves the Donor 1(6dpi) SRA.

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660044/SRR5660044 #This command retrieves the Donor 3(2dpi) SRA.

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660045/SRR5660045 #This command retrieves the Donor 3(6dpi) SRA.

ls #This is used to make sure that you have the files.

fastq-dump -I --split-files SRR5660030 #This converts the file to a paired-end fastq file.

fastq-dump -I --split-files SRR5660033 #This converts the file to a paired-end fastq file.

fastq-dump -I --split-files SRR5660044 #This converts the file to a paired-end fastq file.

fastq-dump -I --split-files SRR5660045 #This converts the file to a paired-end fastq file.

Part Two
This part is in python. It can be run in the terminal as well. To get python simply type python on the terminal if you are using a mac.



