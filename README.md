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


**Variable Customer Exit**: MTD or YTD  Internal table customer exit is E_T_RANGE(New systems) or C_T_RANGE(old systems)

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

**Data Source Enhancement**:

**Business Scenario**:
A company wants to report on Sales Orders in BW. The standard extractor (2LIS_11_VAHDR) provides header-level fields, but the business also needs a custom field (e.g., “Order Priority”) that isn’t available in the delivered DataSource.

1.**Identify the DataSource**
Choose the standard extractor closest to your requirement.

Example: 2LIS_11_VAHDR for sales order header

Check if the required field exists in standard delivery

2.**Enhance the DataSource**
Add custom fields to the extractor structure in S/4HANA.

Use transaction RSA6 or LBWE depending on extractor type

Append the custom field (e.g., Order Priority) to the DataSource structure

3.**Write the Enhancement Logic**
Populate the custom field using ABAP code.

Implement logic in CMOD project (function exit EXIT_SAPLRSAP_001)

Example: fetch Order Priority from VBAK table and assign to the appended field

4.**Activate and Replicate**
Make the enhanced DataSource available to BW.

Activate the DataSource in S/4HANA

Replicate it in BW using RSA1

5.**Adjust BW Data Flow**
Update BW transformations to include the new field.

Map the new field in Transformation

Update DSO/Cube to store the field

Test extraction with sample data

6.**Validate and Transport**
Ensure correctness and move changes to production.

Run delta/full loads to check data

Validate against source tables

Transport changes through landscape


**Debug of all ABAP code**:

If we want to debug any type of routine in the transformation

1)simply open the transfomration

2)goto extras menu bar we will find the one option like display generated program

3)once click on the disply generated program it will take you to the ABAP entire code and just search for your code (ctrl + F).

4)Place break point at select statement, loop statement and after read statement if sy-subrc nearly 

5)execute the F5 so that it will execute the line by line.

6)carefully check the internal table filling or not in the select statement level

7)Once read statement success then we will get sy-subrc eq 0 then value will updating to enhacned fields.







Remember the points here

F5 means exeute line by line

F6 means it will go inside of method or function module

F7 means it will come out of the method or function module

F8 means it will execute the entire code and it will come out of the program.









**What is Customer Exit in SAP BW?**


A Customer Exit Variable is used to derive dynamic values at runtime in BW reports using ABAP code.


Real-Time Business Scenarios


1. MTD – Month-To-Date


Requirement:


Business wants:


1st day of current month → Till today


Example:


If today is 20.04.2026


Then range should be 20260401 → 20260420


Declaration:


DATA: v_date TYPE d.


V_DATE TYPE SY-DATUM.


v_date = sy-datum.


DD type Numc(2)
MM type Numc(2)
YYYY type Numc(4)

yyyy = v_date+0(4)	2026
mm   = v_date+4(2)	04
DD = V_Date + 6(2)	26

Data : V_date1 type D.

CONCATENATE yyyy mm '01' INTO v_date1.

Range Logic

l_s_range-sign = Include(I),Exclude (E)

l_s_range-opt = Between(BT) , Equal (EQ)


l_s_range-low = v_date1

l_s_range-high = sy-datum.

APPEND l_s_range TO l_t_range.

LOW	From Date

HIGH	To Date



I_STEP Values in Customer Exit


I_STEP = 0:Used for Authorization. 


I_STEP = 1:It will be executed before the variable popup 


I_STEP = 2:It will be executed after the variable popup 


I_STEP = 3: Error Handling




**Composite Provider(CP)**: 


CP is a virtual object and it won't store data physically.


we can perform 2 operations by using CP like union and joins


we can create CP based on the below projects like


Info Object,

ADSO,

CP,

Aggregation Level,

Calculation View

Open ODS View

It will support multiple joins like Inner join, left outer join, Right outer join, Full outer join, Referential intergrity and Temporal Join( from date and To date)



**Business Scenario**

**Scenario:** 

A company wants to analyze Sales Orders vs. Deliveries. Sales data is stored in one ADSO, and Delivery data is stored in another ADSO. The business needs a single reporting view to compare ordered quantities with delivered quantities.



**Inner Join**

Returns only records that exist in both participating providers.

Example: Sales Orders that have corresponding Deliveries.




**Left Outer Join**

Returns all records from the left provider and matching records from the right provider.

Example: All Sales Orders, even if they don’t have Deliveries yet.




**Right Outer Join**

Returns all records from the right provider and matching records from the left provider.

Example: All Deliveries, even if the Sales Order record is missing in the left provider.



**Full Outer Join (BW/4HANA specific)**

Returns all records from both providers, with matches where possible.

Example: A reconciliation scenario where you want to see all Sales Orders and all Deliveries, even if they don’t match.




**Scenario:**  


You want to analyze Sales Orders vs. Deliveries.


If you use an Inner Join, you’ll only see orders that have been delivered.


If you use a Left Outer Join, you’ll see all orders, including those pending delivery.


If you use a Full Outer Join, you’ll see both unmatched orders and unmatched deliveries, which is useful for reconciliation.


































