# Extra-Credit
cd ~/labs
git clone https://github.com/Bio312/lab3-$MYGIT

cd ~/labs/lab3-$MYGIT
>getting  the materials needed for the experiment and moving into the directory
>
gunzip proteomes/*.gz 
cat  proteomes/* > allprotein.fas
> uncompressing the proteomes and placing them all into 1 file

makeblastdb -in allprotein.fas -dbtype prot
> creating the blast database

mkdir /home/ec2-user/labs/lab3-$MYGIT/MyGene
cd MyGene
> creating a new folder named "MyGene" and moving into it
ncbi-acc-download -F fasta -m protein XP_021369433.1
> downloading  my GGT gene 
```
blastp -db ../allprotein.fas -query XP_021369433.1.fa -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out ggt.blastp.detail.out
```
> doing a blast search on GGT with several requested parameters
```
awk '{if ($6< 1e-35 )print $1 }' ggt.blastp.detail.out > ggt.blastp.detail.filtered.out
```
> filtering the output with a cutoff e-value of 1e-35

wc -l gqr.blastp.detail.filtered.out
> counting how many proteins there are after the cutoff filtering


conda install -y -n base -c conda-forge aha
sudo yum install -y a2ps
pip install buddysuite
> these 3 packages need to be installed in order to perform all that is needed for the study

cd ~/labs
git clone https://github.com/Bio312/lab4-$MYGIT

cd lab4-$MYGIT
> copying the files needed and moving into the new folder

mkdir ~/labs/lab4-$MYGIT/MyGene
cd ~/labs/lab4-$MYGIT/MyGene
> making a new folder and moving into it

```
seqkit grep --pattern-file ~/labs/lab3-$MYGIT/MyGene/ggt.blastp.detail.filtered.out ~/labs/lab3-$MYGIT/allprotein.fas > ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.fas 
```
> gets the sequences from the BLAST output file and outputs a .fas file
```
muscle -in ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.fas -out ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.al.fas
```
> muscle aligns all the sequences with each other
```
alv -kli --majority ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.al.fas | less -RS
```
> used to examine the output alignment

alignbuddy  -al  ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.al.fas
> finding the length of the alignment

t_coffee -other_pg seq_reformat -in ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.al.fas -output sim
>calculating the percent identity of the sequences with t_coffee
```
mkdir ~/labs/lab5-$MYGIT/MyGene

cp ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.al.fas ~/labs/lab5-$MYGIT/MyGene/ggt.homologs.al.fas

cd ~/labs/lab5-$MYGIT/MyGene
```
> making a new folder, copying files from lab 4 to 5, then moving into the lab 5 folder


iqtree -s ~/labs/lab5-$MYGIT/MyGene/ggt.homologs.al.fas -bb 1000 -nt 2 
>finding the maximum likelihood tree estimate with ultrafast bootstrap support using the flag -bb

```
gotree reroot midpoint -i ~/labs/lab5-$MYGIT/MyGene/ggt.homologs.al.fas.treefile -o ~/labs/lab5-$MYGIT/MyGene/ggt.homologs.al.mid.treefile
```
> rerooting a tree according to a midpoint estimate 

```
cd ~/labs
git clone https://github.com/Bio312/lab6-$MYGIT
cd ~/labs/lab6-$MYGIT
```
> moving back into the parent lab folder, copying the files needed, and moving into the lab 6 files

```
cp~/labs/lab5-$MYGIT/MyGene/ggt.homologs.al.mid.treefile ~/labs/lab6-$MYGIT/MyGene/ggt.homologs.al.mid.treefile
```
>copying files from lab 5 into lab 6 folder
```
java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s ~/labs/lab5-$MYGIT/species.tre -g ~/labs/lab6-$MYGIT/MyGene/ggt.homologs.al.mid.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/labs/lab6-$MYGIT/MyGene/
```
> reconciles the gene tree and species tree together
```
python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/labs/lab6-$MYGIT/MyGene/ggt.homologs.al.mid.treefile.reconciled --include.species
thirdkind -Iie -D 40 -f ~/labs/lab6-$MYGIT/MyGene/ggt.homologs.al.mid.treefile.reconciled.xml -o  ~/labs/lab6-$MYGIT/MyGene/ggt.homologs.al.mid.treefile.reconciled.svg
```
> creates an XML file that allows thirdkind to view the tree, creating an svg file

```
cd ~/labs
git clone https://github.com/Bio312/lab8-$MYGIT 
cd ~/labs/lab8-$MYGIT
```
>moves back into the parent lab folder, copies the files needed for the study, and moves into the lab8 folder

```
mkdir ~/labs/lab8-$MYGIT/MyGene
cd ~/labs/lab8-$MYGIT/MyGene
```
>creates a new folder, moves into the MyGene folder

```
sed 's/*//' ~/labs/lab4-$MYGIT/MyGene/ggt.homologs.fas > ~/labs/lab8-$MYGIT/MyGene/ggt.homologs.fas
```
>copies the unaligned sequence from lab 4 and pastes it into the lab 8 folder, also substituting any asterisks (stop codons) with nothing to get rid of them

```
wget -O ~/data/Pfam_LE.tar.gz ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/little_endian/Pfam_LE.tar.gz && tar xfvz ~/data/Pfam_LE.tar.gz  -C ~/data
```
>the code to get Pfam. it only needs to be run once

```
rpsblast -query ~/labs/lab8-$MYGIT/MyGene/ggt.homologs.fas -db ~/data/Pfam -out ~/labs/lab8-$MYGIT/MyGene/ggt.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001
```
> rpsblast creates domain predictions and places them in a file named "ggt.rps-blast.out"

```
sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab8-$MYGIT/plotTreeAndDomains.r ~/labs/lab5-$MYGIT/MyGene/ggt.homologs.al.mid.treefile ~/labs/lab8-$MYGIT/MyGene/ggt.rps-blast.out ~/labs/lab8-$MYGIT/MyGene/ggt.homologs.fas   ~/labs/lab8-$MYGIT/MyGene/ggt.tree.rps.pdf
```
> a line of code that takes several things previously created in labs to create a pdf with domain estimations alongside a phylogenetic tree. uses a script that Dr. R wrote, "plotTreeAndDomains.r". 
