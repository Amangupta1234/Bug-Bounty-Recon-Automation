# Bug-Bounty-Recon-Automation
A automation Shell Script that makes recon easy by running a small Shell Script to make Project Documentation, Subdomains enumeration, Sorting and Filtering, Resolving subdomains and Directory Bruteforcing/Fuzzing possible in single script  so let's start...



#!/bin/bash

target=$1

REDCOLOR="\e[32m"

GREENCOLOR="\e[31m"

if [ ! -d $target]
then
    mkdir $target
fi

cd $target

echo -e "$REDCOLOR Finding subdomains with sublist3r..."

python3 /home/iamanite/Documents/subwalker/tools/Sublist3r/sublist3r.py -d $target -t 25 -o subdomains.txt

echo -e "$REDCOLOR Finding subdomains with assetfinder..."

/home/iamanite/Documents/subwalker/tools/assetfinder/assetfinder -subs-only $target >> subdomains.txt

echo -e "$REDCOLOR Sorting and Filtering outputs..."

cat subdomains.txt | sort | uniq >> subdomains

echo -e "$REDCOLOR [+]Finding alive subdomains with httprobe ..."

cat subdomains | httprobe > alive.txt

echo -e "$REDCOLOR [+]Finding js files..."

cat alive.txt | subjs > jsfiles

echo -e "$REDCOLOR [+]Finding subdomains takeovers..."

subjack -w subdomains -c /home/iamanite/go/subjack/fingerprints.json -t 25 -ssl -o takeovers.txt

echo -e "$REDCOLOR [+]Fuzzing for directories..."

while read -r line
do 
    python3 /home/iamanite/Documents/dirsearch/dirsearch.py -u alive.txt -w /home/iamanite/Documents/wordlists/common.txt -o directory_fuzzing.txt 
done 

echo "$GREENCOLOR [+] Done"




###############################################################################################################################################################

Note :
Tools required
1. go-lang--------->>sudo apt install golang-go
2. Subwalker---->> https://github.com/m8r0wn/SubWalker
3. httprobe------>> https://github.com/tomnomnom/httprobe
4. Subjs------>>https://github.com/lc/subjs
5. Subjack----->>https://github.com/haccer/subjack
6. Dirsearch---------->> https://github.com/maurosoria/dirsearch
7. for wordlist you can use ------------->> https://github.com/v0re/dirb/blob/master/wordlists/common.txt


