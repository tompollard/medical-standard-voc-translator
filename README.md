# Medical Standard Vocabularies Translator script.

The objective of this repository is to guide the translation from one standard vocabulary to another through [OMOP](https://www.ohdsi.org/data-standardization/the-common-data-model/)  vocabulary resources.


Includes:
- voc_to_voc_translator.ipynb
- README.md


## Authors

- Xavier Borrat Frigola


## Installation

1. Download Athena files concerning the vocabularies you want to translate. See below.
2. Place the files in the same folder as voc_to_voc_translator.ipynb
3. Open voc_to_voc_translator.ipynb using any python 2.7 interpreters with pandas library.
4. Replace the vocabularies specifications and column names depending on the user's needs.


## Documentation

OMOP repository available dictionaries:

ICD10CM	-	International Classification of Diseases, Tenth Revision, Clinical Modification (NCHS) <br>
LOINC	-	Logical Observation Identifiers Names and Codes (Regenstrief Institute)<br>
SNOMED	-	Systematic Nomenclature of Medicine - Clinical Terms (IHTSDO)<br>
NDC	-	National Drug Code (FDA and manufacturers)<br>
MDC	-	Major Diagnostic Categories (CMS)<br>
ICD10	-	International Classification of Diseases, Tenth Revision (WHO)<br>
RxNorm Extension	-	RxNorm Extension (OHDSI)<br>
RxNorm	-	RxNorm (NLM)<br>
ICD9Proc	-	International Classification of Diseases, Ninth Revision, Clinical Modification, Volume 3 (NCHS)<br>
ATC	-	WHO Anatomic Therapeutic Chemical Classification<br>
CPT4	-	Current Procedural Terminology version 4 (AMA)<br>
DRG	-	Diagnosis-related group (CMS)<br>
ICD9CM	-	International Classification of Diseases, Ninth Revision, Clinical Modification, Volume 1 and 2 (NCHS)<br>
OMOP Extension	-	OMOP Extension (OHDSI)<br>


### DOWNLOADING THE DICTIONARIES:

In a  previous step, the user will need to download the files containing the dictionaries containing the selected vocabularies.

Register in [Athena](https://athena.ohdsi.org/vocabulary/list), log in, and select the desired  vocabulary source and target from [Athena](https://athena.ohdsi.org/vocabulary/list)

The Athena web app will send a link to download a ZIP file that will include the next files:

CONCEPT.csv: This dictionary contains the relation between concept_code and concept_id.<br>
CONCEPT_RELATIONSHIP.csv: This dictionary contains the relationships between the different `concept_id(s). We will use the "Maps to" type of relation to translate codes between dictionaries.
These files are dictionaries that need to be stored in the same folder as the python notebook. 


### HOW OMOP VOCABULARIES WORK:

Definitions:
`concept_code`: code from the source vocabulary that represents one concept. 
Example: For the concept  (0.5 ML Fondaparinux sodium 5 MG/ML Prefilled Syringe [Arixtra] the concept_code for NDC is: 00007323001
`concept_id`: code from OMOP that corresponds to a concept_code. 
Example: For the concept (0.5 ML Fondaparinux sodium 5 MG/ML Prefilled Syringe [Arixtra] the concept_id corresponding to concept_code 00007323001 is 44838028.

Every concept_code has a related `concept_id`. The `concept_code` does not converge to a `concept_id`: For the same concept(amoxicillin 250mg Oral Capsule), there will be different OMOP codes(concept_id) one for each vocabulary. Those  `concept_id`(s) are related in the dictionary concept_relationship.
|DRUG | VOCAB | concept_code  | concept_id |
 |-------------------------------| -------- | ------------ | ------- |
amoxicillin 250 MG Oral Capsule | RX NORM | 308182      | 19073183 |
 amoxicillin 250 MG Oral Capsule| NDC     | 44420386 | 43858035231 | 
 
 Every `concept_code` has a different `concept_id` even though they represent the same concept in the real world. In the above table, the same drug has two different `concept_code`s for each vocabulary and also a different `concept_id`.


### Example of translation:
We want to translate [amoxicillin 250 MG Oral Capsule] from RxNorm code to NDC code.

Step 1:
Translate the source code to the OMOP code.
CONCEPT_CODE(SOURCE CODE)--  CONCEPT  --> CONCEPT_ID(OMOP CODE)


 
| VOCAB | concept_code (RX NORM) | concept_id |
| -------- | ------------ | ------- |
| RX NORM | 308182      | 19073183 |


Step 2:
Use the concept_relationship dictionary to relate `concept_id` just obtained from step 1 that maps to other different `concept_id` from target vocabulary .


CONCEPT_ID_1(OMOP CODE FROM SOURCE VOCAB) --> CONCEPT_RELATIONSHIP --> CONCEPT_ID_2(OMOP CODE FROM TARGET VOCAB)

source concept_id is concept_id_1 and target concept_id is concept_id_2

| concept_id_1 | concept_id_2 |
| --------- | ---------- |
| 19073183  | 44420386 |


Step 3:

Use the new concept_id_2  to obtain the `concept_code` of the target vocabulary. 

CONCEPT_ID_2(OMOP CODE FROM TARGET VOCAB) -->CONCEPT--> CONCEPT_CODE(TARGET CODE)

| VOCAB | concept_id| concept_code (NDC) | 
| -------- | ------------ | ------- |
| NDC     | 44420386 | 43858035231 | 




