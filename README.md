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

RESULT = TOUPPER( SOURCE_FIELDS-CUSTOMER_NAME ).

OR

**Trim Spaces**: RESULT = CONDENSE( SOURCE_FIELDS-MATERIAL ).

**Replace Special chars:**

TRANSLATE SOURCE_FIELDS-CITY TO UPPER CASE.

REPLACE ALL OCCURRENCES OF '-' IN SOURCE_FIELDS-CITY WITH ' '.

RESULT = SOURCE_FIELDS-CITY.

**Scenario**: Populate a "Discount Flag" field only if sales exceed a threshold.

IF SOURCE_FIELDS-SALES > 10000.

  RESULT = 'Y'.
  
ELSE.

  RESULT = 'N'.
  
ENDIF.






<img width="762" height="236" alt="image" src="https://github.com/user-attachments/assets/88146989-9cd0-49ae-b9e6-3bce6fe73109" />




**Expert Routine**:

**Business Requirement:**  
A company receives daily sales transactions from multiple stores. Instead of loading every transaction into BW, they want to aggregate sales at the store level before storing in the target InfoCube.


DATA: lt_result TYPE TABLE OF _ty_tgt, "Target structure

      ls_result TYPE _ty_tgt,
      
      lt_source TYPE TABLE OF _ty_src, "Source structure
      
      ls_source TYPE _ty_src.
      

lt_source = SOURCE_PACKAGE.

CLEAR ls_result.

LOOP AT lt_source INTO ls_source.

  READ TABLE lt_result INTO ls_result
  
       WITH KEY store_id = ls_source-store_id.
       
  IF sy-subrc = 0.
  
    ls_result-sales = ls_result-sales + ls_source-sales.
    
    MODIFY lt_result FROM ls_result.
    
  ELSE.
  
    ls_result-store_id = ls_source-store_id.
    
    ls_result-sales    = ls_source-sales.
    
    APPEND ls_result TO lt_result.
    
  ENDIF.
  
ENDLOOP.

RESULT_PACKAGE = lt_result.


**Variable Customer Exit**: MTD or YTD  Internal table customer exit is E_T_RANGE or C_T_RANGE

**Scenario:**  
A company wants a query that always shows sales data for the current fiscal year. Instead of asking users to manually select the year, the system should automatically derive it based on today’s date.

CASE i_vnam. "Variable name

  WHEN 'ZCURR_FY'. "Custom variable for current fiscal year
  
    DATA: lv_date TYPE sy-datum,
    
          lv_year TYPE i.

    lv_date = sy-datum. "System date
    
    lv_year = lv_date+0(4). "Extract year

    "Assume fiscal year starts in April
    
    IF lv_date+4(2) < '04'.
    
      lv_year = lv_year - 1.
      
    ENDIF.

    e_t_range-sign   = 'I'.
    
    e_t_range-option = 'EQ'.
    
    e_t_range-low    = lv_year.
    
    APPEND e_t_range.
    
ENDCASE.

Data Source Enhancement

Debug of all ABAP code





<img width="198" height="141" alt="image" src="https://github.com/user-attachments/assets/939c91f8-8e17-4728-a2e4-5fb8ed778cf5" />
