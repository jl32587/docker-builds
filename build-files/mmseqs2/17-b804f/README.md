# MMseqs2 container

Main tool: [MMseqs2](https://github.com/soedinglab/MMseqs2)
  
Code repository: https://github.com/soedinglab/MMseqs2

Basic information on how to use this tool:
- executable: mmseqs
- help: --help
- version: version
- description: |

> MMseqs2 (Many-against-Many sequence searching) is a software suite to search and cluster huge protein and nucleotide sequence sets. MMseqs2 is free and open source software implemented in C++ for Linux, MacOS, and (as beta version, via cygwin) Windows. The software is designed to run on multiple cores and servers and exhibits very good scalability. MMseqs2 can run 10000 times faster than BLAST. At 100 times its speed it achieves almost the same sensitivity. It can perform profile searches with the same sensitivity as PSI-BLAST at over 400 times its speed.
  
Full documentation: https://github.com/soedinglab/mmseqs2/wiki

## Example Usage

```bash
# learn more at https://github.com/soedinglab/mmseqs2/wiki#easy-workflows-in-mmseqs2
mmseqs easy-search query.fasta db.fasta alignment_results.m8 tmp
mmseqs easy-cluster db.fasta cluster_results tmp
mmseqs easy-linclust large_db.fasta cluster_results tmp
```
