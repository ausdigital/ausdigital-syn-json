# Validation Specification

The validation specification defines a standard for the validation of UBL XML instances at three levels:

* Structure : that the XML instance conforms to the corresponding XSD schema
* Codes : that the code values used in the XML instance are defined in a relevant code-list, which may be either a core code-list or a context specific code list. 
* Rules : that the XML instance complies with the specific business rules for the implementation context identified by the "ProfileID" element in the XML instance.

The following standards are used there:

[Oasis Code List Representation (Genericode) Version 1.0](http://docs.oasis-open.org/codelist/cs-genericode-1.0/doc/oasis-code-list-representation-genericode.html)

[Context/value association using genericode 1.0](http://docs.oasis-open.org/codelist/cs01-ContextValueAssociation-1.0/doc/context-value-association.html)

[ISO Schematron](http://schematron.com/)



The XML instances MUST comply with the [UBL 2.1 Specification](http://docs.oasis-open.org/ubl/UBL-2.1.html).

## Validation Context

As defined by the [UBL Customisation Guide](http://docs.oasis-open.org/ubl/guidelines/UBL2-Customization1.0cs01.pdf) and as implemented by the DBC, UBL allows for the notions of "CustomizationID" and "ProfileID" that are used to define addiotnal restrictions or rules that apply in a specific geographic or industry or porcess context. In general, the "CustomizationID" represents a broad context whilst "ProfileID" reflects further restritions within that broad context. The Ausdigital.org suite of semantic specifications uses these two identifiers as follows:

* There is one "CustomizationID" value that indicates the Australian Digitial Business Council restrictions on the global UBL documents. The set of DBC restrictions is evident when comparing the DBC [CoreInvoice Schema](https://github.com/ausdigital/ausdigital-bill/blob/master/syn-v1/spec/v1.0.0/maindoc/CoreInvoice-1.0.xsd) with the equivalent global UBL 2.1 [Invoice Schema](http://docs.oasis-open.org/ubl/os-UBL-2.1/xsdrt/maindoc/UBL-Invoice-2.1.xsd).  
* There are a number of "ProfileID" values that represent the rules that apply when the core invoice is used in specific business processes such as RCTI, TaXReceipt, CreditNote, etc.  The rules are defined with each semantic specification (eg [billing semantics](https://github.com/ausdigital/ausdigital-bill)

All validation rules are tagged with the relevant customizationID(s) and profileID(s) that apply. Each instance document also contains both these identifiers. Therefore the validation model is to examine the instance document to determine which validation rules should be applied.
