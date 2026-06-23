# SAP-BW4HANA
SAP BW4HANA Interview questions and answers
**Must Learn Topic in SAP BW4HANA**:

**Start Routine** : it will execute before the transformation. it will execute packet by packet.
Mainly we will use for to filter or delete unncessary data.
it will have one internal table i.e SOURCE_PACKAGE
Example: when we are laoding the data from source system(SAP ECC or S/4HANA) 2 key figures are updating 0 values so business don't want to analyse those records in the report due to we are deleting the records in the start routine itsef.

sample code: DELETE SOURCE_PACKAGE WHERE AMOUNT is inital and QUANITY is initial.

End Routine :

Field Level Routine

Expert Routine

Variable Customer Exit

Data Source Enhancement

Debug of all ABAP code


<img width="198" height="141" alt="image" src="https://github.com/user-attachments/assets/939c91f8-8e17-4728-a2e4-5fb8ed778cf5" />
