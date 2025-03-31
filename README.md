# protmap
To map proteins to genomic DNA sequences using miniprot and identify one "best-matched" protein to each mapping locus

### Dependency
[miniprot](https://github.com/lh3/miniprot.git) must be installed and accessible in the system’s environment PATH.

### Usage
Here is an example to run protmap

```
git clone https://github.com/liu3zhenlab/protmap.git
cd test
../protmap --prot ../data/prot.fas --dna ../data/dna.fas
```

## Output
### GFF output
The GFF output is the filtered result from the miniprot GFF (--gff-only) output. The format remains the same.

### BED output
field | description
----------- | -----------
chr | chromosome or sequence ID in genomic DNA
start | 0-based start position
end | 1-based end position
mRNA | mRNA hit ID
rank | mRNA hit rank
strand | mRNA orientation
protein | query protein ID
alnmatch | ratio of the matched amino acid length to the alignment amino acid length
protmatch | ratio of the matched amino acid length to the protein length

### Algorithm
Step 1: Run miniprot to map proteins to the reference and generate a GFF file  
Step 2: Filter miniprot alignments to identify an mRNA hit from a locus aligned with the protein showing the best match if multiple proteins are aligned to the locus  
1. Extract mRNA entries in the GFF input  
2. Group mRNA hits if the regions of mRNAs overlap  
3. For mRNAs in each group, perform pairwise comparisons to see if CDS overlaps significantly. Two mRNA overlaps if the overlapping regions consist of a high ratio (>=50%) of the total CDS length to either mRNA.  
4. Subgroups are formed in a group if non-overlapping mRNAs exist  
5. For each subgroup, only pass one mRNA with the longest "match". The
 "match" is determined by multiplying the identity with the alignment length.

## Contacts
Please report isssues or email [Sanzhen Liu](liu3zhen@ksu.edu) for questions.

