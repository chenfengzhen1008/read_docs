# Submitting Sequence Variation

## Introduction

To submit an analysis programmatically, two XML files must be generated to describe the submission.

- **Analysis XML** - used for describing the analysis you would like to submit
- **Submission XML** - tells ENA how to process this submission

These are then submitted to ENA through the secure HTTPS protocol using POST multipart/form-data 
according to RFC1867. Please see the general guide on 
[Programmatic Submission](../general-guide/programmatic.html) for more information.

## Step 1: Create Analysis XML

This XML is used for:

- Associating the analysis with other ENA objects
- Listing all files required for submission
- Describing metadata of the object

Here is an example of a sequence annotation analysis XML:

```xml
<ANALYSIS_SET>
   <ANALYSIS alias="vcf_sub">
      <TITLE>HipSci WGS GATK HC callset (sample HPSI0614pf-miaj)</TITLE>
      <DESCRIPTION>HipSci whole-genome-sequence GVCF calls generated by GATK haplotype caller</DESCRIPTION>
      <STUDY_REF accession="PRJEB15299"/>
      <SAMPLE_REF accession="ERS1461604"/>
      <RUN_REF accession="ERR1560532"/>
      <RUN_REF accession="ERR1860029"/>
      <ANALYSIS_TYPE>
         <SEQUENCE_VARIATION>
            <ASSEMBLY>
               <STANDARD accession="GCA_000001405.1" refname="GRCh37"/>
            </ASSEMBLY>
            <SEQUENCE accession="CM000663.1" label="1"/>
            <SEQUENCE accession="CM000686.1" label="Y"/>
            <EXPERIMENT_TYPE>Whole genome sequencing</EXPERIMENT_TYPE>
         </SEQUENCE_VARIATION>
      </ANALYSIS_TYPE>
      <FILES>
         <FILE checksum="59d4ba85660c29d9a52a1ad6765ca75f" checksum_method="MD5" filename="HPSI0614pf-miaj.wgs.gatk.haplotype_caller.20161201.genotypes.vcf.gz.tbi" filetype="tabix"/>
         <FILE checksum="4b9e4c631aeb3efb49f6350f42f31c01" checksum_method="MD5" filename="HPSI0614pf-miaj.wgs.gatk.haplotype_caller.20161201.genotypes.vcf.gz" filetype="vcf"/>
      </FILES>
   </ANALYSIS>
</ANALYSIS_SET>
```

Notice that the VCF is using reads from several sequencing runs.
The VCF file maps variant calls according to their position in a reference.  The
`<ASSEMBLY>` and `<SEQUENCE>` blocks allow the VCF file identifiers to be associated with ENA accessions.
 For example, the VCF file in the example has this in the header:

```
##contig=<ID=1,length=249250621>
##contig=<ID=Y,length=59373566>
```

The header refers to a contig called '1' and the analysis XML is used to associate the contig '1' with
the ENA sequence 'CM000663'.  

In the same way, samples can be associated with ENA accessions using the `label` attribute in `<SAMPLE_REF>`:

```xml
<SAMPLE_REF accession="ERS1461604" label="19933_4#3"/>
```

The '19933_4#3' is the name for the sample used in the VCF file.

### Defining the Analysis Type

The most distinguishing part of an analysis object is contained in the `<ANALYSIS_TYPE>` block.
The content of this block determines the type of data the analysis should contain and
how it will be validated by ENA after it has been submitted.

Analysis type `<SEQUENCE_VARIATION>` is for submitting variation files into the archive by EVA.

Sequence variation analyses must be associated with a study and can be associated with
one or more samples. The analyses may also point to sequencing experiments or runs.

### Associating with Other ENA Objects

#### Associating with the Study

An analysis points to the study it is part of using the `<STUDY_REF>` element.
This can be done either by using an accession:

```
<STUDY_REF accession="ERP123456"/>
```

or a name within the submitter's account:

```
<STUDY_REF refname="mantis_religiosa"/>
```

#### Associating with Samples

An analysis can be associated with one or more samples using the `<SAMPLE_REF>` element
either using an accession or alias to refer to the sample.

#### Associating with Experiments and Runs

An analysis can be associated with any number of experiments or runs using the `<EXPERIMENT_REF>`
and `<RUN_REF>` elements. Again, either an accession or alias can be used in the reference.

### Preparing Files For Submission

#### Upload Data Files

Please [upload](../fileprep/upload.html) all data files required for submission.

Once the analysis has been submitted, all the data files described in the analysis XML will be moved 
from the Webin upload area into the archive.

You can upload your data files to the root directory of your upload area or you can
create subdirectories and upload your files there.

#### Describe Data Files for Submission

You should then describe these data files in your analysis XML with the `<FILE>` element.

To descibe files required for submission, the analysis object has a `<FILES>` block. 
This submits the data files into the archive.

For example:

```
<FILES>
    <FILE checksum="59d4ba85660c29d9a52a1ad6765ca75f" checksum_method="MD5" filename="HPSI0614pf-miaj.wgs.gatk.haplotype_caller.20161201.genotypes.vcf.gz.tbi" filetype="tabix"/>
    <FILE checksum="4b9e4c631aeb3efb49f6350f42f31c01" checksum_method="MD5" filename="HPSI0614pf-miaj.wgs.gatk.haplotype_caller.20161201.genotypes.vcf.gz" filetype="vcf"/>
</FILES>
```

If the files are uploaded to the root directory
then simply enter the file name in the Analysis XML when referring to it:

```
<FILE filename="a.vcf.gz"" ... />
```

If the files are uploaded into a subdirectory (e.g. `mantis_religiosa`) then prefix the file name
with the name of the subdirectory:

```
<FILE filename="mantis_religiosa/a.vcf.gz"" ... />
```

### Adding Additional Metadata

All other metadata used to describe the analysis can be provided using `ANALYSIS_ATTRIBUTE` elements in
the XML:

```
    <ANALYSIS_ATTRIBUTE>
       <TAG>library preparation date</TAG>
       <VALUE>2010-08</VALUE>
    </ANALYSIS_ATTRIBUTE>
```

## Step 2: Create the Submission XML

Once you have created your analysis XML, you need an accompanying submission 
XML in a separate file to tell ENA what actions you would like to take for your submission.

```
<SUBMISSION>
   <ACTIONS>
      <ACTION>
         <ADD/>
      </ACTION>
   </ACTIONS>
</SUBMISSION>
```

The submission XML declares one or more Webin submission service actions. See the general guide on 
[Programmatic Submission](../general-guide/programmatic.html>) for more information.

In this case the action is `<ADD/>` which is used to submit new objects.

The XMLs can then be submitted programmatically, using CURL on command line or
using the [Webin submissions portal](../general-guide/submissions-portal.html).

## Step 3: Submit the XMLs

### Submit the XMLs Using CURL

CURL is a Linux/Unix command line program which you can use to send the `analysis.xml` and `submission.xml`
to the Webin submission service.

```
curl -u username:password -F "SUBMISSION=@submission.xml" -F "ANALYSIS=@analysis.xml" "https://wwwdev.ebi.ac.uk/ena/submit/drop-box/submit/"
```

Please provide your Webin submission account credentials using the `username` and `password`.

After running the command above a receipt XML is returned. It will look like the one below:

```
<?xml version="1.0" encoding="UTF-8"?>
<RECEIPT receiptDate="2017-08-11T15:07:36.746+01:00" submissionFile="sub.xml" success="true">
   <ANALYSIS accession="ERZ0151578" alias="08-1736" status="PRIVATE"/>
   <SUBMISSION accession="ERA986371" alias="08-1736"/>
   <MESSAGES>
       <INFO>This submission is a TEST submission and will be discarded within 24 hours</INFO>
   </MESSAGES>
   <ACTIONS>ADD</ACTIONS>
   <ACTIONS>ADD</ACTIONS>
</RECEIPT>
```

### Submit the XMLs Using Webin submissions portal

XMLs can also be submitted interactively using the [Webin submissions portal](../general-guide/submissions-portal.html).
Please refer to the [Webin submissions portal](../general-guide/submissions-portal.html) document for an example how
to submit a study using XML. Other types of XMLs can be submitted using the same approach.

### The Receipt XML

To know if the submission was successful look in the first line of the `<RECEIPT>` block.

The attribute `success` will have value `true` or `false`. If the value
is false then the submission did not succeed. In this case check the rest of
the receipt for error messages and after making corrections, try the submission again.

If the success attribute is true then the submission was successful. The receipt will
contain the accession numbers of the objects that you have submitted.

### Test and Production Services

Note the message in the receipt:

```
<INFO>This submission is a TEST submission and will be discarded within 24 hours</INFO>
```

It is advisable to first test your submissions using the Webin test service where changes are not permanent
and are erased every 24 hours.

Once you are happy with the result of the submission you can use the CURL command again
but this time using the production service. Simply change the part in the URL from `wwwdev.ebi.ac.uk` to
`www.ebi.ac.uk`:

```
curl -u username:password -F "SUBMISSION=@submission.xml" -F "ANALYSIS=@analysis.xml" "https://www.ebi.ac.uk/ena/submit/drop-box/submit/"
```

Similarly, if you are using the [Webin submissions portal](../general-guide/submissions-portal.html) change the URL from
`wwwdev.ebi.ac.uk` to `www.ebi.ac.uk`.