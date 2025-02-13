# XML Transformation in ABAP

![image](https://github.com/user-attachments/assets/388a30b0-eeaf-4fe9-9124-91878bf9c420)

Came already with this template

![image](https://github.com/user-attachments/assets/21ce2b9a-0eb5-481b-9dfb-5972533f1488)

Add:

```xml
xmlns:ddic="http://www.sap.com/abapxml/types/dictionary" 
xmlns:def="http://www.sap.com/abapxml/types/defined"> 

<tt:root name="ZROOT" type="ddic:ZTT_XML3_AR"/> 
```
Final version:

PS: It is possible to set the loop.

![image](https://github.com/user-attachments/assets/741c514f-ae46-4033-9a4e-4e97f6ddcf20)

Calling the transformation in the abap code

```abap
  METHOD create_xml3_ar.

    DATA: lt_xml3      TYPE ztt_xml3_ar,
          lv_xml       TYPE xstring,
          lv_xstring   TYPE etxml_line_str,
          lo_converter TYPE REF TO cl_abap_conv_in_ce.

    lt_xml3 = VALUE #( FOR e_iva_header IN gt_iva_header ( totaliva = e_iva_header-kwert ) ).

    CALL TRANSFORMATION ztr_xml3_ar
    SOURCE zroot = lt_xml3[]
    RESULT XML lv_xml.

    lv_xstring  = lv_xml.

    TRY.

        lo_converter = cl_abap_conv_in_ce=>create( input       = lv_xstring
                                                   encoding    = 'UTF-8'
                                                   replacement = '?'
                                                   ignore_cerr = abap_true ).

      CATCH cx_parameter_invalid_range.
      CATCH cx_sy_codepage_converter_init.

    ENDTRY.

    TRY.

        CALL METHOD lo_converter->read( IMPORTING data = ch_v_xml ).

      CATCH cx_sy_conversion_codepage.
*-- Should ignore errors in code conversions
      CATCH cx_sy_codepage_converter_init.
*-- Should ignore errors in code conversions
      CATCH cx_parameter_invalid_type.
      CATCH cx_parameter_invalid_range.

    ENDTRY.

  ENDMETHOD.

ENDCLASS.![image](https://github.com/user-attachments/assets/77180864-33c6-4b04-9518-4decf1d5ceb8)

```
Se11:

![image](https://github.com/user-attachments/assets/d5fae1e4-b84c-4b52-80eb-a67ae0b5e1f0)

![image](https://github.com/user-attachments/assets/d849aa8b-46ac-4692-9199-3706f5acd9bd)



