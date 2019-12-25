### 1. In order to select chloroplast related reads from total DNA sequencing data including nuclear, mitochondrial and chloroplast derived reads, the raw data of “preads.fasta” is mapped to the reference of “cp_reference.fasta” by Blasr. The output is written as “out”. The reference genome is downloaded from NCBI. Here the chloroplast of A.thaliana (NC_000932.1) is chosen as “cp-reference.fasta”.
$ blasr preads.fasta cp_reference.fasta --out out –header

### 2. Extract the first column from output data of“out” and save as “qName_list”, using space as a delimeter.
$ cut -d “ ” -f 1 out > qName_list

### 3. Extract the first 9 characters of each lines in “qName_list”, sort, save the unique name as “uniq_qName_list’.
$ cut -c -9 qName_list|sort|uniq > uniq_qName_list

### 4. The chloroplast related reads are selected from the raw data of “preads.fasta” by using the ID of “uniq_qName_list” and a Perl script “extractSeq.pl”.
$ perl extractSeq.pl uniq_qName_list preads.fasta selectedSeq.fa

### 5. PBA is a script to perform a de novo PacBio assembly of organelles (https://github.com/aubombarely/Organelle_PBA). After PBA installment, the program is run by using chloroplast reads of “SelectedSeq.fa” and the reference of “cp-reference.fasta”. The output is written to the “result” directory including the assembled chloroplast genome sequences and other related statistics.
$ PBA -i SelectedSeq.fa -r cp-reference.fasta -o result
