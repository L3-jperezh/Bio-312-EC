# Bio 312 EC
*All commands for the PI3K gene family for the Term Paper project*

```bash
echo 'export MYGIT="myusername"' >>~/.bash_profile
```
```bash
source ~/.bash_profile
```
```bash
echo $MYGIT
```
```bash
cd ~/labs/lab3-$MYGIT
```
```bash
gunzip proteomes/*.gz
```
```bash
cat  proteomes/* > allprotein.fas
```
```bash
makeblastdb -in allprotein.fas -dbtype prot
```
```bash
mkdir /home/ec2-user/labs/lab3-$MYGIT/PIK
```
```bash
cd /home/ec2-user/labs/lab3-$MYGIT/PIK
```
```bash
ncbi-acc-download -F fasta -m protein XP_021367892.1
```
```bash
blastp -db ../allprotein.fas -query XP_021367892.1.fa -outfmt 0 -max_hsps 1 -out PIK.blastp.typical.out
```
```bash
`PIK.blastp.typical.out`` using ``less``
```
```bash
blastp -db ../allprotein.fas -query XP_021367892.1.fa -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out PIK.blastp.detail.out
```
```bash
PIK.blastp.detail.out`` using the ``less -S
```
```bash
grep -c Hsapiens PIK.blastp.detail.out
```
```bash
awk '{if ($6< 1e-35 )print $1 }' PIK.blastp.detail.out > PIK.blastp.detail.filtered.out
```
```bash
wc -l PIK.blastp.detail.filtered.out
```
```bash
grep -o -E "^[A-Z][a-z]+\." PIK.blastp.detail.filtered.out  | sort | uniq -c
```
```bash
conda install -y -n base -c conda-forge aha
```
```bash
sudo yum install -y a2ps
pip install buddysuite
```
```bash
cd ~/labs
```
```bash
git clone https://github.com/Bio312/lab4-$MYGIT
```
```bash
cd lab4-$MYGIT
```
```bash
nano ~/labs/lab4-$MYGIT/toy.fas
```
```bash
mkdir ~/labs/lab4-$MYGIT/PIK
```
```bash
cd ~/labs/lab4-$MYGIT/PIK
```
```bash
seqkit grep --pattern-file ~/labs/lab3-$MYGIT/PIK/PIK.blastp.detail.filtered.out ~/labs/lab3-$MYGIT/allprotein.fas > ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fasseqkit grep --pattern-file ~/labs/lab3-$MYGIT/PIK/PIK.blastp.detail.filtered.out ~/labs/lab3-$MYGIT/allprotein.fas > ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fas
```
```bash
muscle -in ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fas -out ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas
```
```bash
alv -kli  ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas | less -RS
```
```bash
alv -kli --majority ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas | less -RS
```
```bash
mkdir ~/labs/lab5-$MYGIT/PIK
```
```bash
cd ~/labs/lab5-$MYGIT/PIK
```
```bash
cp ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas
```
```bash
iqtree -s ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas -bb 1000 -nt 2
```
```bash
nw_display ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile
```
```bash
Rscript --vanilla ~/labs/lab5-$MYGIT/plotUnrooted.R  ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile.pdf 0.4
```
```bash
gotree reroot midpoint -i ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile -o ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile
```
```bash
nw_order -c n ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile  | nw_display -
```
```bash
nw_order -c n ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile | nw_display -w 1000 -b 'opacity:0' -s  >  ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile.svg -
```
```bash
nw_order -c n ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile | nw_topology - | nw_display -s  -w 1000 > ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.midCl.treefile.svg -
```
```bash
cd ~/labs/lab6-$MYGIT/gqr
```
```bash
ls ~/labs/lab6-$MYGIT/gqr/gqr.homologs.al.mid.treefile
```
```bash
java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s ~/labs/lab5-$MYGIT/species.tre -g ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/labs/lab6-$MYGIT/PIK/
```
```bash
less -S PIK.homologs.al.mid.treefile.reconciled.events.txt
```
```bash
grep NOTUNG-SPECIES-TREE ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled | sed -e "s/^\[&&NOTUNG-SPECIES-TREE//" -e "s/\]/;/" | nw_display -
```
```bash
python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled --include.species
```
```bash
thirdkind -Iie -D 40 -f ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled.xml -o  ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled.svg
```
```bash
mkdir ~/labs/lab8-$MYGIT/PIK
```
```bash
cd ~/labs/lab8-$MYGIT/PIK
```
```bash
sed 's/*//' ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fas > ~/labs/lab8-$MYGIT/PIK/PIK.homologs.fas
```
```bash
wget -O ~/data/Pfam_LE.tar.gz ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/little_endian/Pfam_LE.tar.gz && tar xfvz ~/data/Pfam_LE.tar.gz  -C ~/data
```
```bash
rpsblast -query ~/labs/lab8-$MYGIT/PIK/PIK.homologs.fas -db ~/data/Pfam -out ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001
```
```bash
sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab8-$MYGIT/plotTreeAndDomains.r ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out ~/labs/lab8-$MYGIT/PIK/PIK.tree.rps.pdf
```
```bash
mlr --inidx --ifs "\t" --opprint  cat ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | tail -n +2 | less -S
```
```bash
cut -f 1 ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | sort | uniq -c
```
```bash
cut -f 6 ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | sort | uniq -c
```
```bash
awk '{a=$4-$3;print $1,'\t',a;}' ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out |  sort  -k2nr
```
```bash
sort  -k5rg ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | less -S
```













