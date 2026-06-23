# SAP-BW4HANA
SAP BW4HANA Interview questions and answers
**Must Learn Topic in SAP BW4HANA**:

**Start Routine** : it will execute before the transformation. it will execute packet by packet.
Mainly we will use for to filter or delete unncessary data.
it will have one internal table i.e SOURCE_PACKAGE
Example: when we are laoding the data from source system(SAP ECC or S/4HANA) 2 key figures are updating 0 values so business don't want to analyse those records in the report due to we are deleting the records in the start routine itsef.

sample code: DELETE SOURCE_PACKAGE WHERE AMOUNT is inital and QUANITY is initial.

End Routine :
1.	End Routine will execute after the transformation.
2.	End Rouine will have a target structure.
3.	End Routine will have internal table RESULT_PACKAGE.
4.	End Routine will execute packet by packet.
5.	Mainly we will use end routine to update the data for enhanced fields in the ADSO through ABAP logic.This is called LOOK UP>
6.	End Routine will give the better performance comparing start routine and field level routine.

sample code: LOOP AT RESULT_PACKAGE ASSIGNING <RESULT_FIELDS>.
ENDLOOP.

Field Level Routine :
1.	When we use Field Level Routine, it will degrade the loading performance due to record-by-record execution.
2.	It won’t execute packet by packet (one packet represents minimum 50,000 records).
3.	When we have a small piece of code to update the flag for a field, then only we will go for Field Level Routine.
Example: If we want to convert the data from lower case to upper case, then we will use Field Level Routine.


sample code: MATERIAL GROUP CODE
READ TABLE IT_MAT INTO WA_MAT WITH KEY MATERIAL = SOURCE_FIELDS-MATERIAL
BINARY SEARCH.
IF SY-SUBRC EQ 0.
RESULT = WA_MAT-MATL_GROUP.
ENDIF.


Expert Routine :1.	When we use Expert Routine, whatever routines are available in transformation, it will delete automatically all routines and mappings.
2.	It will have only Expert Routine.
3.	In the Expert Routine, SOURCE_PACKAGE and RESULT_PACKAGE both internal tables are available.
4.	We have to map the fields through ABAP code in Expert Routine


Variable Customer Exit

Data Source Enhancement

Debug of all ABAP code


<img width="198" height="141" alt="image" src="https://github.com/user-attachments/assets/939c91f8-8e17-4728-a2e4-5fb8ed778cf5" />
