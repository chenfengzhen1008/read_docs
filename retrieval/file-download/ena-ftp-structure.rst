=================
ENA FTP Structure
=================

The root of the ENA FTP server is at:

::

    ftp://ftp.ebi.ac.uk/pub/databases/ena/


This server contains short pieces of assembled/annotated sequence dating back
to the early 80s, as well as larger scale data types including genome
assemblies, MAG/SAG assemblies, and transcriptome sequence assemblies.
This server does not contain other data types such as raw reads, which can be
found in the `SRA FTP server <../file-download/sra-ftp-structure.html>`_.

It is expected that users will find relevant data through the various search
interfaces available from ENA, rather than directly interacting with and
navigating the FTP server.
This page therefore does not provide an exhaustive explanation of the server's
structure, but serves to briefly describe what you will find in some of its
most important subdirectories.

For some, but not all categories, suppressed data is kept separate from public.

Most data is available in `EMBL flat file format
<https://ena-docs.readthedocs.io/en/latest/submit/fileprep/flat-file-example.html>`_,
with some also available as FASTA.


| **Doc**
| Information on the INSDC feature table format, with versions dating back to 1995
| ftp://ftp.ebi.ac.uk/pub/databases/ena/doc/

| **Genome Collections**
| An XML file containing cumulative information on all genome collection accessions
| ftp://ftp.ebi.ac.uk/pub/databases/ena/genome_collections/

| **Assembly**
| Plaintext reports on the content of genome assemblies, organised by genome collection accession (GCA)
| ftp://ftp.ebi.ac.uk/pub/databases/ena/assembly/

| **WGS**
| Whole genome shotgun contig-level assembly, one multi-record flatfile and one multi-FASTA file per sequencing project
| ftp://ftp.ebi.ac.uk/pub/databases/ena/wgs/

| **TSA**
| Transcriptome shotgun assembly records, one multi-record flatfile and one multi-FASTA file per sequencing project
| ftp://ftp.ebi.ac.uk/pub/databases/ena/tsa/

| **Sequence**
| Generalised sequence category, organised by sequence type and taxon class
| ftp://ftp.ebi.ac.uk/pub/databases/ena/sequence/

| **Coding**
| Protein-coding sequences, organised by sequence type and accession number
| ftp://ftp.ebi.ac.uk/pub/databases/ena/coding/

| **Non-Coding**
| Non-protein-coding sequences, organised by sequence type and accession number
| ftp://ftp.ebi.ac.uk/pub/databases/ena/non-coding/

| **rRNA**
| Ribosomal RNA sequences, organised by sequence type and accession number
| ftp://ftp.ebi.ac.uk/pub/databases/ena/rRNA/

| **TLS**
| Targeted locus study sequences, one multi-record flatfile and one multi-FASTA file per sequencing project
| ftp://ftp.ebi.ac.uk/pub/databases/ena/tls/