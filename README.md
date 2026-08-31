
# WLoops

Here, we present an adapted and modified version of Mustache (Roayaei Ardakany et al. 2020) called `Wloops` which detects chromatin loops from Hi-C contact maps.

Mustache detects chromatin loops as blob-like structures in Hi-C contact maps by detecting maxima in Difference-of-Gaussians (DoG) responses in the Hi-C maps. Unlike many loop callers that compute statistical significance directly from raw contact counts, Mustache computes significance from DoG response, treating loops as image features rather than count outliers. Mustache uses an exponential distribution to model the DoG values in a local neighborhood of a loop candidate (Roayaei Ardakany et al. 2020). Therefore, Mustache outperforms the other methods and stands out as a preferred choice for chromatin loop detection. However, the Mustache paper lacks empirical evidence on the choice of background distribution. 

We show that the Weibull distribution (not exponential distribution used in the Mustache paper (Roayaei Ardakany et al. 2020)) fits the background DoG responses the best and significance levels assigned to Blobs using a background Weibull distribution uncovers previously uncovered chromatin loops. 



## Installation and Dependencies

- Please see Github page of Mustache tool https://github.com/ay-lab/mustache/tree/master for dependencies required.
- Make sure you have Python >=3.6 installed, along with all the dependencies listed.
- To install `Wloops`, simply download `WLoops/WLoops.py` file.

`WLoops` can be executed by running the script `WLoops.py`

## Executing `Wloops.py`

Similar to `Musatche`, `WLoops` can be executed in different ways (Please see more details in `Mustache` site)
- a contact map and a normalization/bias vector
- .hic file
- .cool file
- 
### Parameters

`WLoops` takes all the arguments that `Mustache` takes. `WLoops` take one additional argument `-B or --background`.

- `-B` or `--background`: user can provide `E` for exponential background or `W` for the Weibull background.


### Example executing `WLoops`

`wget ftp://cooler.csail.mit.edu/coolers/hg19/Rao2014-GM12878-MboI-allreps-filtered.5kb.cool
mustache -f ./Rao2014-GM12878-MboI-allreps-filtered.5kb.cool -ch chr12 chr19 -r 5kb -pt 0.05 -o cooler_out.tsv
OR
mustache -f ./Rao2014-GM12878-MboI-allreps-filtered.5kb.cool -r 5kb -pt 0.05 -o cooler_out.tsv`
For each chromosome, PySmooth Generates the following outputs.


### References
Roayaei Ardakany A, Gezer HT, Lonardi S, Ay F. 2020. Mustache: multi-scale detection of chromatin loops from Hi-C and Micro-C maps using scale-space representation. Genome Biol. 21(1):256. https://doi.org/10.1186/s13059-020-02167-0.

