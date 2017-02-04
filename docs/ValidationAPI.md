# Validation API Specifications

The validation API specification defines a standard interface for the validation of UBL JSON instances at three levels

* Structure : that the JSON instance conforms to the corresponding JSON schema for the specific document type as referenced by the "$schema" element in the JSON instance.
* Codes : that the code values used in the JSON instance are defined in a relevant code-list, which may be either a core code-list or a context specific code list.
* Rules : that the JSON instance complies with the specific business rules for the implementation context identified by the "ProfileID" element in the JSON instance.

The JSON instances MUST comply with the [UBL Syntax 2.0 Specification](https://github.com/ausdigital/ausdigital-syn/blob/master/docs/JSONSyntax.md).

## Validation Context

As defined by the [UBL Customisation Guide](http://docs.oasis-open.org/ubl/guidelines/UBL2-Customization1.0cs01.pdf) and as implemented by the DBC, UBL allows for the notions of "CustomizationID" and "ProfileID" that are used to define additional restrictions or rules that apply in a specific geographic or industry or process context.   In general, the "CustomizationID" represents a broad context whilst "ProfileID" reflects further restrictions within that broad context.   The Ausdigital.org suite of semantic specifications uses these two identifiers as follows:

* There is one "CustomizationID" value that indicates the Australian Digital Business Council restrictions on the global UBL documents. The set of DBC restrictions is evident when comparing the DBC [CoreInvoice Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/syn-v1/spec/v1.0.0rt/maindoc/CoreInvoice-1.0.xsd) with the equivalent global UBL 2.1 [Invoice Schema](http://docs.oasis-open.org/ubl/os-UBL-2.1/xsdrt/maindoc/UBL-Invoice-2.1.xsd).  
* There are a number of "ProfileID" values that represent the rules that apply when the core invoice is used in specific business processes such as RCTI, TaXReceipt, CreditNote, etc.  The rules are defined with each semantic specification (eg [billing semantics](https://github.com/ausdigital/ausdigital-bill)

All validation rules are tagged with the relevant customizationID(s) and profileID(s) that apply.  Each instance document also contains both these identifiers.   Therefore the validation model is to examine the instance document to determine which validation rules should be applied.

## API Validator Specification

Is maintained at swaggerhub : **[UBL Syntax 2.0 Transformation and Validation API](https://app.swaggerhub.com/api/ausdigital/ausdigital-syn/1.0)**

The API is very simple - it accepts an instance document and returns a list of zero or more error responses. All error responses will comply with the ausdigital standard [RESTful Errors structure](https://app.swaggerhub.com/domains/ausdigital/ErrorModel/1.0).  

## Document Structure Error Response Codes


|Error Code | Error Message|
|-----------|--------------|
|struct-01 | The $schema reference is missing and so this document cannot be validated  |
|struct-02 |   |
|struct-03 |   |

## Code-List Error Response Codes

Are maintained together with the [code-lists specification](http://ausdigital-code.readthedocs.io/en/latest/) 

## Business Rules Error Response Codes

Are maintained together with the relevant semantic specification - for example [billing semantics](http://ausdigital-bill.readthedocs.io/en/latest/)



