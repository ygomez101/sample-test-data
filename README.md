# sample-test-data
The following are a set of instructions on how to run the code and all the files.

Track 1 was chosen for the Python Pipeline Project.

wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660030/SRR5660030
wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660033/SRR5660033
wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660044/SRR5660044
wget https://sra-pub-run-odp.s3.amazonaws.com/sra/SRR5660045/SRR5660045
ls
fastq-dump -I --split-files SRR5660030
fastq-dump -I --split-files SRR5660033
fastq-dump -I --split-files SRR5660044
fastq-dump -I --split-files SRR5660045

