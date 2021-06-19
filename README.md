# Psymol

::INTRO TODO::


## Acknowledgements

We are grateful to the [PsychonautWiki](https://psychonautwiki.org/) community for creating and maintaining a remarkable source of molecules and experiences.


# Method

## Outline

- Query the PsychonautWiki for a seed set of compounds
- Clean where needed
- Add compounds to the set.
- Lookup SMILES description of each compound

## Detailed steps

1. Queried [PsychonautWiki](https://psychonautwiki.org/) API for all substances, giving 290 results as of 18 June 2021.

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

    Taking the three class columns together, giving 290 × 3 = 870 cells, the classes are:

    | Cell Count | Class Value                   |
    |------------|-------------------------------|
    | 616        | no value for the class cell   |
    |   4        | Antipsychotic                 | 
    |   9        | Cannabinoid                   | 
    |   4        | Deliriant                     | 
    |  33        | Depressant                    | 
    |  22        | Dissociatives                 | 
    |  22        | Entactogens                   | 
    |   1        | Eugeroic                      | 
    |   4        | Hallucinogens                 | 
    |  18        | Nootropic                     | 
    |   2        | Oneirogen                     | 
    |  22        | Opioids                       | 
    |  59        | Psychedelics                  | 
    |   1        | Sedative                      | 
    |  53        | Stimulants                    | 
               

2. We manually removed the following records:

- An "experience" included in the list of substances (reported as [issue 41](https://github.com/psychonautwiki/bifrost/issues/41)).

- References to lists of substances were removed: Substituted aminorexes, Substituted amphetamines, Substituted cathinones, Substituted morphinans, Substituted phenethylamines, Substituted phenidates, Substituted tryptamines, Stimulants, Barbiturates, Cannabinoid, Cytochrome P450 inhibitors, Depressant, Entheogen, Hallucinogens, Prodrug, Psychedelics, Sedative.

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

- In removing the lists of substance pages, we noticed that the following substances were referenced but had no page on the Wiki (or the page was an empty stub).

    We've captured these in the file _missing.txt_ in this repositiory. There are also a large number of missing entries (in red) at https://psychonautwiki.org/wiki/Depressant
