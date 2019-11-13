# Getting Started with EpiGEN

## Scope

EpiGEN is an easy-to-use epistasis simulation pipely written in Python. It supports epistasis models of arbitrary size, which can be specified either extensionally or via parametrized risk models. Moreover, the user can specify the minor allele frequencies (MAFs) of both noise and disease SNPs, and provide a bias target distribution for the generated phenotypes to simulate observation bias.

## Installation

EpiGen is freely available on [GitHub](https://github.com/baumbachlab/epigen). To install it on your machine, simply execute the following line in a terminal:

```sh
git clone https://github.com/baumbachlab/epigen
```

## Usage

The user interface of EpiGEN consists of three scripts:

- `simulate_data.py`
- `generate_genotype_corpus.py`
- `merge_genotype_corpora.py`

The script `simulate_data.py` simulates epistasis data on top of a pre-computed genotype corpus. For each chromosome `<CHROM>` and each HAPMAP3 population code `<POP>`, EpiGEN contains a pre-computed corpus for 10000 individuals, which is identified by the prefix `corpora/<CHROM>_<POP>_`. For example, if you want to generate epistasis data with ID 0 for 7500 individuals and 10000 SNPs on top of the pre-computed corpus `corpora/1_ASW_`, where the parametrized epistasis model `models/param_model.xml` acts upon the SNPs with IDs 156, 3, and 1076 in the corpus, you can use `simulate_data.py` as follows:

```sh
python3 simulate_data.py --sim-id 0 --corpus-id 1 --pop ASW --inds 7500 --snps 10000 --disease-snps 156 3 1076 --model models/param_model.xml  
```

As you will notice when running this command, a large fraction of the runtime of `simulate_data.py` is used for unzipping the corpora. You can therefore significantly speed-up the script by unzipping them before. 

If you want to use custom corpora instead of the pre-computed ones, you can generate them via the script `generate_genotype_corpus.py`. For example, the corpus `corpora/1_ASW_` shipped with EpiGEN was generated as follows:

```sh
python3 generate_genotype_corpus.py --corpus-id 1 --pop ASW --inds 10000 --chroms 1 --compress 
```

Finally, the script `merge_genotype_corpora.py` allows you to merge pre-computed corpora into a larger copus. For instance, the following command merges the pre-computed corpora `corpora/1_ASW_` and `corpora/2_ASW_` into a newly generated corpus `corpora/23_ASW_`:

```sh
python3 merge_genotype_corpora.py --corpus-ids 1 2 --pops ASW ASW --corpus-id 23 --append SNPS
```

Detailed descriptions of how to use the scripts can be found in the user guide.

## Requirements

EpiGen has the following dependencies:

- Python 3.3 or higher.
- Numpy 1.17.3 or higher.
- Scipy 1.3.1 or higher.
- Matplotlib 3.1.1 or higher.

Moreover, due to its HAPGEN2 dependency, the script `generate_genotype_corpus.py` needs to be run on a Linux machine or on a machine running macOS 10.14 or lower. However, you can avoid running `generate_genotype_corpus.py` by using the pre-computed corpora and merging them, if necessary.

## License

All of EpiGEN's Python sources are licensed under the [GNU General Public License 3](https://www.gnu.org/licenses/gpl-3.0.de.html). However, this license does not cover the HAPGEN2 binaries, which are distributed with EpiGEN and are called by the script `generate_genotype_corpus.py`. HAPGEN2 is property of the University of Oxford and may only be freely used for academic research and in accordance with the license found at [https://mathgen.stats.ox.ac.uk/genetics_software/hapgen/LICENCE](https://mathgen.stats.ox.ac.uk/genetics_software/hapgen/LICENCE). Copies of the GNU General Public License 3 and of the license for HAPGEN2 are distributed with EpiGEN.


## Structure of the Repository

```
.
├── README.md                        // README
├── LICENSE                          // A copy of the GNU General Public License 3
├── user_guide.pdf                   // The user guide
├── requirements.txt                 // Lists dependencies
├── simulate_data.py                 // Script the simulate epistasis data
├── generate_genotype_corpus.py      // Script to generate genotype corpus
├── merge_genotype_corpora.py        // Script to merge genotype corpora
├── sim                              // Output directory for simulated data
├── corpora                          // Output directory for genotype corpora
├── temp                             // Contains auxiliary files 
├── ext                              // Contains external libraries and data
│   ├── HAPGEN2                      // Contains HAPGEN2 binaries and license
│   └── HAPMAP3                      // Contains HAPMAP3 data
├── models                           // Contains epistasis models
│   ├── ParametrizedModel.dtd.       // Doctype definition for parametrized models
│   ├── ext_model.ini.               // An example of an extensional model
│   └── param_model.xml.             // An example of a parametrized model
└── utils                            // Contains the core of EpiGEN
    ├── __init__.py.                 // __init__ file
    ├── data_simulator.py.           // Implements simulation of epistasis data
    ├── genotype_corpus_generator.py // Implements generation of genotype corpora
    ├── genotype_corpusmerger.py     // Implements merging of genotype corpora
    ├── parametrized_model.py.       // Implements parametrized models 
    ├── extensional_model.py.        // Implements extensional models
    └── argparse_checks.py.          // Implements argparse checks
```
