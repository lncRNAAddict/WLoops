
# WLoops

Here, we present an adapted and modified version of Mustache (Roayaei Ardakany et al. 2020) called `Wloops` which detects chromatin loops from Hi-C contact maps.

Mustache detects chromatin loops as blob-like structures in Hi-C contact maps by detecting maxima in Difference-of-Gaussians (DoG) responses in the Hi-C maps. Unlike many loop callers that compute statistical significance directly from raw contact counts, Mustache computes significance from DoG response, treating loops as image features rather than count outliers. Mustache uses an exponential distribution to model the DoG values in a local neighborhood of a loop candidate (Roayaei Ardakany et al. 2020). Therefore, Mustache outperforms the other methods and stands out as a preferred choice for chromatin loop detection. However, the Mustache paper lacks empirical evidence on the choice of background distribution. 

We show that the Weibull distribution (not exponential distribution used in the Mustache paper (Roayaei Ardakany et al. 2020)) fits the background DoG responses the best and significance levels assigned to Blobs using a background Weibull distribution uncovers previously uncovered chromatin loops. 



## Installation and Dependencies

- Please see Github page of Mustache tool https://github.com/ay-lab/mustache/tree/master for dependencies required.
- Make sure you have Python >=3.6 installed, along with all the dependencies listed.
- 



`PySmooth` can be executed by running the script `run_smooth.py`

## Running `run_smooth.py`

### Input Genotype File format

The First row is the header. Each row represents a unique marker.

The genotype file MUST have the following columns:

- Column 1: Chromosome name.
- Column 2: Genomic Position of the marker in the chromosome. For each chromosome,column 2 MUST already be sorted in ascending order.
- Column 3: Identification id of the marker location. 
- Column 4: Reference allele in the reference genome if known or can be left blank cell.
- Column 5: Alternate allele if known or blank cell.
- Column 6 and beyond: Genotype code for the individuals in the marker location. Four codes can be used. `A`: Reference parent homozygous, `B`: Alternatte parent homozygous, `H`: heterozygous, `U`: missing data.

A screeshot of a portion of an example input file is shown below

![Example Input Genotype File](https://github.com/lncRNAAddict/PySmooth/blob/main/example/GenotypeInput.PNG)

### Running PySmooth

PySmooth takes the following arguments

- `-i` or `--input`: Name of the input genotype file. This MUST be provided.
- '-o' or '--output': Prefix to name of output files to be generated. If not provided, default is `test`.
- `-c` or `--chr`: list of chromosome names to perform analysis on. Names should be separated by comma (e.g `chr1,chr2,chr3`). Default is to run through all the chromosomes in the genotype file.
- `-l` or `--lower`: Lowest threshold for identifying singletons. Default is 0.70.
- `-u`or `--upper`: Highest threshold for identifying singletons. Default is 0.98.
- `-g` or `--gap`: PySmooth iteratively identifies singletons starting with the highest threshold till the lowest threshold. This parameter is used to decreased the threshold at each iteration. Default is 0.02.
- `-k` : number of nearest neighbors to be used to assign correct genotype to singleton or missing item. Default value is 30.

First, change working directory to the folder where the `PySmooth` scripts are stored. You can do that by simply typing the following command in the `terminal`, or `command prompt`, or  `anaconda command prompt` depending on your python installation or OS.

`cd <path to where PySmooth scripts are stored>`

Once the working directory is set, shown below are two examples of running `PySmooth`.

`python run_smooth.py -i <path to the genotype file>/my_genotype_file.csv`

- In the `example` folder, there is an example genotype input file called `my_genotype_file.csv`

The code above will analyze  each chromosome detected and generate all output files with prefix `test` in the folder `<path to the genotype file>`
  
`python run_smooth.py -i <path to the genotype file>/my_genotype_file.csv -o <path to output folder>/my_output -c chr1 -l 0.80 -u 0.98 -g 0.02`

The code above will analyze for chromosome `chr1`and generate all output files with prefix `my_output` in the folder `<path to output folder>`.

### Outputs

For each chromosome, PySmooth Generates the following outputs.

- Three summary csv files: `<output>_<chr>.stats.csv`, `<output>_<chr>_singletons_stats.csv`, and `<output>_<chr>_imputed_stats.csv` that contain `%` of homozygous, heterozygous calls for each individual for the raw genoytpe file, after singleton detection, and after error correction. An example file for the `<output>_<chr>_singletons_stats.csv` is shown below. The files not only indicate the number of singletons detected in each sample but also the fraction from each category of genotype call detected as singletons.

  ![alt text](https://github.com/lncRNAAddict/PySmooth/blob/main/example/singleton_stats.JPG)


- Three bar plot png files: `<output>_<chr>.stats.png`, `<output>_<chr>_singletons_stats.png`, and `<output>_<chr>_imputed_stats.png` bar plot files that contains `%` of homozygous, heterozygous calls for each individual for the raw genoytpe file, after singleton detection, and after error correction, respectively. Example images are shown below.

![alt text](https://github.com/lncRNAAddict/PySmooth/blob/main/example/Slide3.PNG)

- Three heatmap files: `<output>_<chr>.heatmap.png`, `<output>_<chr>_singletons_heatmap.png`, and `<output>_<chr>_imputed_heatmap.png` that visualize a color-coded image of different genotype codes in the original file, after singleton detection, and after error correction, respectively. Example images are shown below.

![alt text](https://github.com/lncRNAAddict/PySmooth/blob/main/example/Slide2.PNG)

- `<output>_<chr>_singletons.csv`: genotype file with singleton detected. Singletons are marked as `S`. 
- `<output>_<chr>_imputed.csv`: genotype file after error correction.




### References
Roayaei Ardakany A, Gezer HT, Lonardi S, Ay F. 2020. Mustache: multi-scale detection of chromatin loops from Hi-C and Micro-C maps using scale-space representation. Genome Biol. 21(1):256. https://doi.org/10.1186/s13059-020-02167-0.

