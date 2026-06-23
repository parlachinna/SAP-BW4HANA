# SAP-BW4HANA
SAP BW4HANA Interview questions and answers
**Must Learn Topic in SAP BW4HANA**:

**Start Routine** : it will execute before the transformation. it will execute packet by packet.
Mainly we will use for to filter or delete unncessary data.
it will have one internal table i.e SOURCE_PACKAGE
**Example:** when we are laoding the data from source system(SAP ECC or S/4HANA) 2 key figures are updating 0 values so business don't want to analyse those records in the report due to we are deleting the records in the start routine itsef.

sample code: DELETE SOURCE_PACKAGE WHERE AMOUNT is inital and QUANITY is initial.

**End Routine :** It will execute after the transfomration. it will execute packet by packet and it will target structure.
we will use mainly to update the data directly to the target like thorugh lookup code we will update the data for enhanced fields.
**Example: ** we have material in the source and target  where as target having some other fields like material group, material type and material category.
while loading the data from source to target we have end routine once material is matching with P material table then it will update the data for material group, material type and material categeory.

**Field Level Routine**: Generally we will write the small peice of code for this like to convert the lower case letters to upper case.

**Scenario**: Populate a "Discount Flag" field only if sales exceed a threshold.

IF SOURCE_FIELDS-SALES > 10000.
  RESULT = 'Y'.
ELSE.
  RESULT = 'N'.
ENDIF.


<img width="762" height="236" alt="image" src="https://github.com/user-attachments/assets/88146989-9cd0-49ae-b9e6-3bce6fe73109" />



Expert Routine

Variable Customer Exit

Data Source Enhancement

Debug of all ABAP code


<img width="198" height="141" alt="image" src="https://github.com/user-attachments/assets/939c91f8-8e17-4728-a2e4-5fb8ed778cf5" />
