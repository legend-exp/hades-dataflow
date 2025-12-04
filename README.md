# hades-dataflow

Implementation of an automatic data processing flow for HADES HPGe
detector characterization data, based on
[Snakemake](https://snakemake.readthedocs.io/).


## Configuration

Data processing resources are configured via a single site-dependent (and
possibly user-dependent) configuration file, named "dataflow-config.yaml" in the
following.

## Execution

Data generation is based on key-lists, which are flat text files
(extension ".keylist") containing one entry of the form
`{detector}-{campaign}-{measurement}-{run}-{timestamp}` per line. Key-lists
can be auto-generated based on the available DAQ or raw tier files
using Snakemake targets.

The special output target `{label}-{tier}.gen` is used to
generate all files listed in `{label}-{tier}.filelist`. After the files
are created, the empty file `{label}-{tier}.filelist` will be created to
mark the successful data production.

Snakemake targets like `all-{detector}-{campaign}-{measurement}-{tier}.gen` may be used
to automatically generate key-lists and file-lists (if not already present)
and produce all possible output for the given data tier, based on available
DAQ or raw files which match the target.

* `all.gen`
* `all-{detector}.gen`
* `all-{detector}-{campaign}.gen`
* `all-{detector}-{campaign}-{measurement}.gen`
* `all-{detector}-{campaign}-{measurement}-{run}.gen`
* `all-{detector}-{campaign}-{measurement}-{run}-{timestamp}.gen`
* `all-{detector}-{campaign}-{measurement}-{run}-{timestamp}-{tier}.gen`

which will run the full production for all detectors, resp.
a specific detector, or a specific detector and campaign, etc.

For example:

```shell
snakemake all-char_data-V06649A-c1-bkg-dsp.gen
```

will run the production of data from the detector `V06649A`, campaign `c1`, measurement `bkg`
up to the DSP tier.
