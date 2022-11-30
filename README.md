# Bio-312-EC
All commands for the PI3K gene family for the Term Paper project
echo 'export MYGIT="myusername"' >>~/.bash_profile
source ~/.bash_profile
echo $MYGIT
cd ~/labs/lab3-$MYGIT
gunzip proteomes/*.gz
cat  proteomes/* > allprotein.fas
makeblastdb -in allprotein.fas -dbtype prot
mkdir /home/ec2-user/labs/lab3-$MYGIT/PIK
cd /home/ec2-user/labs/lab3-$MYGIT/PIK
ncbi-acc-download -F fasta -m protein XP_021367892.1
blastp -db ../allprotein.fas -query XP_021367892.1.fa -outfmt 0 -max_hsps 1 -out PIK.blastp.typical.out
`PIK.blastp.typical.out`` using ``less``
blastp -db ../allprotein.fas -query XP_021367892.1.fa -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out PIK.blastp.detail.out
PIK.blastp.detail.out`` using the ``less -S
grep -c Hsapiens PIK.blastp.detail.out
awk '{if ($6< 1e-35 )print $1 }' PIK.blastp.detail.out > PIK.blastp.detail.filtered.out
wc -l PIK.blastp.detail.filtered.out
grep -o -E "^[A-Z][a-z]+\." PIK.blastp.detail.filtered.out  | sort | uniq -c
conda install -y -n base -c conda-forge aha
sudo yum install -y a2ps
pip install buddysuite
cd ~/labs
git clone https://github.com/Bio312/lab4-$MYGIT
cd lab4-$MYGIT
nano ~/labs/lab4-$MYGIT/toy.fas
mkdir ~/labs/lab4-$MYGIT/PIK
cd ~/labs/lab4-$MYGIT/PIK
seqkit grep --pattern-file ~/labs/lab3-$MYGIT/PIK/PIK.blastp.detail.filtered.out ~/labs/lab3-$MYGIT/allprotein.fas > ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fasseqkit grep --pattern-file ~/labs/lab3-$MYGIT/PIK/PIK.blastp.detail.filtered.out ~/labs/lab3-$MYGIT/allprotein.fas > ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fas
muscle -in ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fas -out ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas
alv -kli  ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas | less -RS
alv -kli --majority ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas | less -RS
mkdir ~/labs/lab5-$MYGIT/PIK
cd ~/labs/lab5-$MYGIT/PIK
cp ~/labs/lab4-$MYGIT/PIK/PIK.homologs.al.fas ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas
iqtree -s ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas -bb 1000 -nt 2
nw_display ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile
Rscript --vanilla ~/labs/lab5-$MYGIT/plotUnrooted.R  ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile.pdf 0.4
gotree reroot midpoint -i ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.fas.treefile -o ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile
nw_order -c n ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile  | nw_display -
nw_order -c n ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile | nw_display -w 1000 -b 'opacity:0' -s  >  ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile.svg -
nw_order -c n ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile | nw_topology - | nw_display -s  -w 1000 > ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.midCl.treefile.svg -
cd ~/labs/lab6-$MYGIT/gqr
ls ~/labs/lab6-$MYGIT/gqr/gqr.homologs.al.mid.treefile
java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s ~/labs/lab5-$MYGIT/species.tre -g ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/labs/lab6-$MYGIT/PIK/
less -S PIK.homologs.al.mid.treefile.reconciled.events.txt
grep NOTUNG-SPECIES-TREE ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled | sed -e "s/^\[&&NOTUNG-SPECIES-TREE//" -e "s/\]/;/" | nw_display -
python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled --include.species
thirdkind -Iie -D 40 -f ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled.xml -o  ~/labs/lab6-$MYGIT/PIK/PIK.homologs.al.mid.treefile.reconciled.svg
mkdir ~/labs/lab8-$MYGIT/PIK
cd ~/labs/lab8-$MYGIT/PIK
sed 's/*//' ~/labs/lab4-$MYGIT/PIK/PIK.homologs.fas > ~/labs/lab8-$MYGIT/PIK/PIK.homologs.fas
wget -O ~/data/Pfam_LE.tar.gz ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/little_endian/Pfam_LE.tar.gz && tar xfvz ~/data/Pfam_LE.tar.gz  -C ~/data
rpsblast -query ~/labs/lab8-$MYGIT/PIK/PIK.homologs.fas -db ~/data/Pfam -out ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001
sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab8-$MYGIT/plotTreeAndDomains.r ~/labs/lab5-$MYGIT/PIK/PIK.homologs.al.mid.treefile ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out ~/labs/lab8-$MYGIT/PIK/PIK.tree.rps.pdf
mlr --inidx --ifs "\t" --opprint  cat ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | tail -n +2 | less -S
cut -f 1 ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | sort | uniq -c
cut -f 6 ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | sort | uniq -c
awk '{a=$4-$3;print $1,'\t',a;}' ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out |  sort  -k2nr
sort  -k5rg ~/labs/lab8-$MYGIT/PIK/PIK.rps-blast.out | less -S













