# Sequence data format conversion pipelines on Azure

This repository is an example of running the pipelines for converting between sequence data formats on Cromwell on Azure.<br/> 

Learn more about using Azure for your Cromwell WDL workflows on our GitHub repo! - [Cromwell on Azure](https://github.com/microsoft/CromwellOnAzure).<br/>

This repository is a fork from [the original](https://github.com/gatk-workflows/seq-format-conversion) and has all the required changes to run the WDL workflow on Cromwell on Azure.<br/>

Here, you can find the WDL files and an example inputs JSON files with links to data hosted on a public Azure Storage account. You can use the "msgenpublicdata" storage account directly as a relative path, like in the inputs JSON file. 

The `bam-to-unmapped-bams.trigger.json`, `cram-to-bam.trigger.json`, `interleaved-fastq-to-paired-fastq.trigger.json` and `paired-fastq-to-unmapped-bam.trigger.json` trigger files are ready to use.

## seq-format-conversion
Workflows for converting between sequence data formats

### cram-to-bam :
This script should convert a CRAM to SAM to BAM and outputs a BAM, BAM Index, 
and validation report.
The reason this approach was chosen instead of converting CRAM to BAM directly 
using Samtools is because Samtools 1.3 produces incorrect bins due to an old version of htslib 
included in the package. Samtools versions 1.4 & 1.5 have an NM issue that 
causes them to not validate with Picard. 

#### Requirements/expectations
- Cram file 

#### Outputs 
- Bam file and index
- Validation report

### paired-fastq-to-unmapped-bam :
This WDL converts paired FASTQ to uBAM and adds read group information 

#### Requirements/expectations 
- Pair-end sequencing data in FASTQ format (one file per orientation)
- The following metada descriptors per sample: 
  - readgroup   
  - sample_name
  - library_name
  - platform_unit
  - run_date
  - platform_name
  - sequecing_center
  
#### Outputs 
- Unmapped BAM 

### bam-to-unmapped-bams :
This WDL converts BAM  to unmapped BAMs

#### Requirements/expectations 
- BAM file

#### Outputs 
- Sorted Unmapped BAMs
- Text file listing the unmapped file paths (FOFN)

### interleaved-fastq-to-paired-fastq :
This WDL takes in a single interleaved(R1+R2) FASTQ file and separates it into 
separate R1 and R2 FASTQ (i.e. paired FASTQ) files. Paired FASTQ files are the input 
format for the tool that generates unmapped BAMs (the format used in most 
GATK processing and analysis tools).

#### Requirements/expectations 
- Interleaved Fastq file

#### Outputs 
- Separate R1 and R2 FASTQ files (i.e. paired FASTQ)

### Software version requirements :
- GATK4 or later
- Samtools 1.3.1
- Picard 2.8.3
- Cromwell version support 
  - Successfully tested on v47
  - Does not work on versions < v23 due to output syntax

### Important Notes :
- The provided JSON is a ready to use example JSON template of the workflow. Users are responsible for reviewing the [GATK Tool and Tutorial Documentations](https://gatk.broadinstitute.org/hc/en-us/categories/360002310591) to properly set the reference and resource variables. 
- The following material is provided by the Data Science Platforum group at the Broad Institute. Please direct any questions or concerns to one of our forum sites : [GATK](https://gatk.broadinstitute.org/hc/en-us/community/topics) or [Terra](https://support.terra.bio/hc/en-us/community/topics/360000500432).

### LICENSING :
Copyright Broad Institute, 2019 | BSD-3
This script is released under the WDL open source code license (BSD-3) (full license text at https://github.com/openwdl/wdl/blob/master/LICENSE). Note however that the programs it calls may be subject to different licenses. Users are responsible for checking that they are authorized to run all programs before running this script.
