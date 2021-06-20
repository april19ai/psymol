# Psy mol

::INTRO TODO::


## Acknowledgements

We are grateful to the [PsychonautWiki](https://psychonautwiki.org/) community for creating and maintaining the source of molecules and experiences used as the basis of this list.

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

    The classes are values such as "Psychedelics", "Sedative", and are described later in this file.

2. We manually removed the following records:

    - An "experience" included in the list of substances (reported as [issue 41](https://github.com/psychonautwiki/bifrost/issues/41)).

    - Pages describing classes of substances and plants were removed: Substituted aminorexes, Substituted amphetamines, Substituted cathinones, Substituted morphinans, Substituted phenethylamines, Substituted phenidates, Substituted tryptamines, Stimulants, Barbiturates, Cannabinoid, Cytochrome P450 inhibitors, Depressant, Entheogen, Hallucinogens, Prodrug, Psychedelics, Sedative, 2C-T-x, 2C-x, Synthetic cannabinoid, Serotonergic psychedelic, Mandragora officinarum (botany), Selective serotonin reuptake inhibitor, Serotonin-norepinephrine reuptake inhibitor, Racetams, Benzodiazepines, Amanita muscaria, Anadenanthera peregrina, Antipsychotic, Arylcyclohexylamines, Banisteriopsis caapi, Changa, Classical psychedelic, DOx, Datura (botany), Deliriant, Entactogens, Gabapentinoids, Hallucinogens, Harmala alkaloid, Hyoscyamus niger (botany), Hypnotic, MAOI, Mandragora, Morning glory, Nootropic, Opioids, Peganum harmala, Piper nigrum (botany), Poppers, RIMA, Thienodiazepines, Xanthines, Beta-Carboline, 25x-NBOMe, 25x-NBOH. 

3. In reviewing the lists of substance pages removed, we noted the following substances had pages but were not included in the original query results (we checked the query results by "compound" and "common name"). This was rasied as [issue 42](https://github.com/psychonautwiki/bifrost/issues/42).

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

    We've captured these in the file [missing.txt](missing.txt) in this repositiory. There are also a large number of missing entries (in red) at https://psychonautwiki.org/wiki/Depressant

4. Removed any duplicate molecues based on molecule name only, resulting in 259 records int the file [psychonaut.csv](psychonaut.csv).

5. Added SMILES descriptions for each molecule.

    From manual inspection of a few records, it appears PsychonautWiki links to Wikipedia or [TiHKAL / Isomerdesign](https://isomerdesign.com) for molecular descriptions. To automate the lookup of SMILES values, we used Wikipedia.
 
    Wikipedia has already been mined for SMILES entries by [Ertl et al (2015)](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-015-0061-y). We used this data in a Jupyter notebook called [merge.ipynb](merge.ipynb).

    This procedure matched all but 79 records. 

    In addition, we searched for the "missing" molecules in the Wikipedia data, and found 82 molecules

    All these 341 records were saved as [combined.csv](combined.csv).

    | Set | Count |
    |---------|-------|
    | PsychonautWiki matched to Wikipedia | 180 |
    | "Missing" records matched to Wikipedia | 82 |
    | Remaining unmatched PsychonautWiki records | 79 |
    | TOTAL | 341 | 


6. ::TODO:: We manually interrogated Wikipedia to complete the set. The resulting records are in the file [index.csv](index.csv)


## Additional information

TODO: how to run the notebook



