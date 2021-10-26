# Psy mol

This repository contains a CSV file listing molecules found on the PsychonautWiki,
with corresponding SMILES descriptions.

## NO WARRANTY

We make no warranty, expressed or implied, including the warranties of merchantability and fitness for a particular purpose, nor assumes any legal liability  or responsibility for the accuracy, reliability, completeness, safety or utility of these data, or for the improper or incorrect use of these data.  

The user is responsible to verify the limitations of the data and to use the data accordingly.

## Acknowledgment

We are grateful to the [PsychonautWiki](https://psychonautwiki.org/) community for creating and maintaining the source of molecules and experiences used as the basis of this list.

## Description of the data

The [library.csv](library.csv) file contains 335 molecules, with the following columns:

- `name` -- the name of the molecule from PsychonautWiki.
- `url` -- the PsychonautWiki web address. 
- `class0`, `class1`, `class2` -- possibly empty columns representing one or more class names for the molecule from PsychonautWiki
- `smiles` -- the SMILES molecule description.

Please see the _Methods_, below for how we curated this dataset.

In addition, the [library+shulgin.csv](library+shulgin.csv) file contains additional molecules reported by Shulgin. See from step 8, below, for details.

## How to cite this data

TODO

---

# Method

## Outline

- Queried the PsychonautWiki for a seed set of compounds
- Clean where needed
- Add compounds to the set.
- Lookup SMILES description of each compound

## Detailed steps

1. Queried PsychonautWiki API for all substances, giving 290 results as of 18 June 2021.

    We used the following query:

    ```
    substances(limit: 999) {
        name 
        url
        class {
            psychoactive
        }	
    }
    ```

2. The output was converted to CSV with the following columns:

    - `name`, the substance name in PsychonautWiki
    - `url`, the substance page in PsychonautWiki
    - `class0`, `class1`, `class2`, up to three "psychoactive" classes.

    The classes are values such as "Psychedelics", "Sedative", and are described below.

2. We manually removed the following records:

    - An "experience" included in the list of substances.

    - Pages describing classes of substances, plants, and jokes were removed: Substituted aminorexes, Substituted amphetamines, Substituted cathinones, Substituted morphinans, Substituted phenethylamines, Substituted phenidates, Substituted tryptamines, Stimulants, Barbiturates, Cannabinoid, Cytochrome P450 inhibitors, Depressant, Entheogen, Hallucinogens, Prodrug, Psychedelics, Sedative, 2C-T-x, 2C-x, Synthetic cannabinoid, Serotonergic psychedelic, Mandragora officinarum (botany), Selective serotonin reuptake inhibitor, Serotonin-norepinephrine reuptake inhibitor, Racetams, Benzodiazepines, Amanita muscaria, Anadenanthera peregrina, Antipsychotic, Arylcyclohexylamines, Banisteriopsis caapi, Changa, Classical psychedelic, DOx, Datura (botany), Deliriant, Entactogens, Gabapentinoids, Hallucinogens, Harmala alkaloid, Hyoscyamus niger (botany), Hypnotic, MAOI, Mandragora, Morning glory, Nootropic, Opioids, Peganum harmala, Piper nigrum (botany), Poppers, RIMA, Thienodiazepines, Xanthines, Beta-Carboline, 25x-NBOMe, 25x-NBOH, Atropa belladonna, Ayahuasca, Cannabis, Cake, Inhalants, Kratom.

3. In reviewing the lists of substance pages removed, we noted the following substances had pages but were not included in the original query results (we checked the query results by "compound" and "common name"). 

    For completeness these were added to the list of substances:

    - https://psychonautwiki.org/wiki/4-AcO-DiPT
    - https://psychonautwiki.org/wiki/4-HO-DET
    - https://psychonautwiki.org/wiki/4-HO-DPT
    - https://psychonautwiki.org/wiki/Serotonin
    - https://psychonautwiki.org/wiki/5-HO-DMT
    - https://psychonautwiki.org/wiki/5-MeO-DiPT
    - https://psychonautwiki.org/wiki/5-HTP
    - https://psychonautwiki.org/wiki/Isopropylphenidate
    - https://psychonautwiki.org/wiki/3,4-CTMP
    - https://psychonautwiki.org/wiki/Proscaline
    - https://psychonautwiki.org/wiki/Allylescaline
    - https://psychonautwiki.org/wiki/Methallylescaline
    - https://psychonautwiki.org/wiki/Hexen
    - https://psychonautwiki.org/wiki/A-PHP
    - https://psychonautwiki.org/wiki/MDPV
    - https://psychonautwiki.org/wiki/Bromo-DragonFLY
    - https://psychonautwiki.org/wiki/1P-ETH-LAD
    - https://psychonautwiki.org/wiki/AL-LAD
    - https://psychonautwiki.org/wiki/ETH-LAD
    - https://psychonautwiki.org/wiki/MIPLA
    - https://psychonautwiki.org/wiki/PRO-LAD
    - https://psychonautwiki.org/wiki/EPT
    - https://psychonautwiki.org/wiki/MET
    - https://psychonautwiki.org/wiki/4-HO-DiPT
    - https://psychonautwiki.org/wiki/2C-T-21
    - https://psychonautwiki.org/wiki/2C-T-7
    - https://psychonautwiki.org/wiki/DOI
    - https://psychonautwiki.org/wiki/DOM

    ...and we also filled in class columns where given.

- In removing the lists of substance pages, we noticed that substances were referenced but had no page on the Wiki (or the page was an empty stub).

    We've captured these in the file [intermediate/missing.txt](intermediate/missing.txt) in this repository. There are also a large number of missing entries (in red) at https://psychonautwiki.org/wiki/Depressant

4. Removed any duplicate molecules based on molecule name only, resulting in 253 records in the file [psychonaut.csv](psychonaut.csv).

5. Added SMILES descriptions for each molecule.

    From manual inspection of a few records, it appears PsychonautWiki links to Wikipedia or [TiHKAL / Isomerdesign](https://isomerdesign.com) for molecular descriptions. To automate the lookup of SMILES values, we used Wikipedia.
 
    Wikipedia has already been mined for SMILES entries by [Ertl et al (2015)](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-015-0061-y). We used this data in a Jupyter notebook called [merge.ipynb](merge.ipynb) 
    to join the PsychonautWiki and Wikipedia records.

    This procedure matched all but 73 records. 

    In addition, we searched for the "missing.txt" molecules in the Wikipedia data, and found 82 molecules

    All these 335 records were saved as [intermediate/combined.csv](intermediate/combined.csv).

    | Set     | Count |
    |---------|-------|
    | PsychonautWiki matched to Wikipedia        | 180 |
    | "Missing" records matched to Wikipedia     | 82 |
    | Remaining unmatched PsychonautWiki records | 73 |
    | TOTAL | 335 | 


6. For the 73 unmatched records, we manually interrogated Wikipedia and recorded the molecules in [intermediate/manually-found.csv](intermediate/manually-found.csv). We added a `wikipedia_url` column for these records; and when not found at Wikipedia, the `isomerdesign_url` column was populated. 

    This manually matching was carried out on 23 and 24 June 2021. Where multiple SMILES values were given, we took the first one.

7. Finally, we combined the automatic and manually found records to produce [library.csv](library.csv). 

    The classes are:

    |               |   class |
    |:--------------|--------:|
    |               |     483 |
    | Psychedelics  |      77 |
    | Stimulants    |      58 |
    | Depressant    |      32 |
    | Entactogens   |      23 |
    | Dissociatives |      21 |
    | Opioids       |      21 |
    | Nootropic     |      19 |
    | Cannabinoid   |      10 |
    | Antipsychotic |       4 |
    | Hallucinogens |       4 |
    | Deliriant     |       3 |
    | Oneirogen     |       2 |
    | Eugeroic      |       1 |
    | Sedative      |       1 |

8. We additionally added the molecules reported by Shulgin in TiHKAL and PiHKAL by hand. To do this we used the following data sources:

    | Book | Molecule Count | Source | Accessed |
    |------|----------------|--------|----------|
    | PiHKAL | 179 | https://psychonautwiki.org/wiki/PiHKAL | 6 Oct 2021 |
    | TiHKAL |  55 | https://psychonautwiki.org/wiki/TiHKAL | 5 Oct 2021 |

    These records are in [intermediate/library+shulgin-raw.csv](intermediate/library+shulgin-raw.csv) (called "raw" as the SMILES have not been normalized or deduplicated). This file contains 508 rows.

    This step added the column `shulgin` with a value of `T` if the source was TiHAL, `P` for PiHKAL, and blank otherwise.

9. The script `can.ipynb` replaced SMILES strings with a canonical form so we can deduplicate the list. The form of this conversion is:

    ```
    Chem.MolToSmiles(Chem.MolFromSmiles(smi),True)
    ```

    The result is [intermediate/library+shulgin-can.csv](intermediate/library+shulgin-can.csv). This added a new column for the cannonical SMILES string, and 25 rows were unchanged and 483 were different from the original SMILES string.

10. Deduplication of SMILES records. The following records were identified as duplicates:

    ```
    $ xsv select can library+shulgin-can.csv | sort | uniq -d
    C=C(C)COc1c(OC)cc(CCN)cc1OC   (MAL and Methallylescaline)
    C=CCOc1c(OC)cc(CCN)cc1OC      (AL and Allylescaline)
    CC(C)N(C)CCc1c[nH]c2ccccc12   (MiPT duplicated)
    CCOc1cc(OC)c(CC(C)N)cc1OC     (incorrect entry for Proscaline, corrected)
    CN(C)CCc1c[nH]c2cccc(O)c12    (Psilocin and 4-HO-DMT)
    CNC(C)Cc1ccc(OC)cc1           (PMMA and METHYL-MA)
    COc1ccc(CC(C)N)cc1            (PMA and 14-MA)
    NCCc1ccccc1 (PEA and Phenethylamine)
    ```

    These were manually merged into a single records and the cannonical form of the SMILES string was saved as the `smiles` column in [library+shulgin.csv](library+shulgin.csv).

    The final file contains 502 compounds, with InChI hash keys added.

---

## Additional information

### To run the notebook locally using Conda

For Unix-like machines:

```
conda env create -f environment.yml
conda activate psymol
jupyter notebook
```

To update after changing dependencies:

```
conda env update -f environment.yml
```

