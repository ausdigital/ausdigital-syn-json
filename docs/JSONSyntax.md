# JSON Syntax for UBL documents

The normative form for UBL documents is XML and the normative definition is the UBL XSD Schema library.  Therefore the JSON syntax implementation for UBL occasionally refers to the XML form when defining the JSON rules.

## JSON Schema Rules

* Each JSON Schema document MUST comply with the [JSON Schema Specification](http://json-schema.org/documentation.html) and contain the JSON element "$schema": "http://json-schema.org/draft-04/schema#".  
* The JSON schema MUST be a standalone single root schema that does not import external fragments but MAY make use of local schema references ($ref) for re-use of common structures.  It is recognised that several different document schema will define the same common structures but this SHOULD be managed at the semantic model level and not at the runtime schema level.
* JSON type definitions MUST use exactly the same name as the corresponding UBL XML element name but without any namespace prefix (ie use the UNQUALIFIED UBL element name).
* JSON schema type definitions MUST include a "description" property and it's value SHOULD equal the corresponding UBL schema <ccts:Definition> property.
* JSON schema type definitions SHOULD NOT use "enum" types for validation of code list values.  This is to allow codes lists to be managed and versioned separately from their use in business document schema.  Please refer to the [code lists](https://github.com/ausdigital/ausdigital-code) specification for the treatment of code lists.

## JSON Instance Rules

* The JSON instance MUST conform to the corresponding JSON schema which is recommended to be attached to the instance by using a Link header in http request according to http://json-schema.org/latest/json-schema-core.html#rfc.section.9.1. For example:
`Link: <https://raw.githubusercontent.com/ausdigital/ausdigital-bill/master/spec/v1.0.0/Invoice.json>; rel="describedby"`
* The JSON instance SHOULD have a "customizationID" element that contains the URI of the relevant implementation context because that will define the relevant code-list values (eg BPay as a payment means in Australia) and business rules (eg that tax invoices over $1000 must contain the buyers ABN) that would be validated.
* If "customizationID" is present then the JSON instance MUST comply with the code-list and business rules defined for that context.  Note that the code-list rules are defined in the [code-lists specification](http://ausdigital-code.readthedocs.io/en/latest/) and the business rules are defined together with each semantic specification (eg [billing semantics](http://ausdigital-bill.readthedocs.io/en/latest/))

## CCTS Property Mapping

All UBL types inherit from the UN/CEFACT Core Component Technical Specification (CCTS) set of 10 core data types. Each CCTS data type has a number of optional properties that are implemented as UBL XML element attributes - for details please refer to the [UBL CCTS Schema Module](https://docs.oasis-open.org/ubl/cs-UBL-2.0/xsd/common/CCTS_CCT_SchemaModule-2.0.xsd).  There are two concerns with mapping CCTS properties to JSON.
* JSON has no equivalent of XML element attributes
* All the CCTS attributes are optional with often overlapping definitions and little implementation guidance for allowed values.  This leads to confusion and interoperability problems. For example, 8 of the 9 attributes of the CCTS "Code" type all serve to identify different attributes of the code list scheme (listID, listAgencyID, listAgencyName, listName, listVersionID, listURI, listSchemeURI).  A single unique identifier of the code list scheme would be sufficient.

In mapping UBL to JSON there is a balance to be struck between a simple mapping algorithm (leading to relatively more complex JSON) and a more complex set of mapping rules (that could deliver simpler JSON).  This specification prioritises simple JSON even at the cost of more complex mapping rules when transforming to/from UBL XML.

### CCTS AmountType

JSON representation SHALL NOT specify currency for every data element.  Instead it SHALL use the UBL document level currency indicator "documentCurrencyCode":"AUD"

Therefore the UBL XML 

```<cbc:LineExtensionAmount currencyID="AUD">500.00</cbc:LineExtensionAmount>``` 

Maps to the UBL JSON

``` "lineExtensionAmount":10.00 ```

### CCTS BinaryObjectType 

UBL supports either embedded or referenced binary attachments.  The JSON implementation SHALL always use the UBL "ExternalReference" Model.  

Therefore the UBL XML

```<cbc:EmbeddedDocumentBinaryObject format="pdf" characterSetCode="utf-8" mimeCode="application/pdf" encodingCode="base64" uri="https://mydomain.com/invoice1234.pdf" filename="invoice1234.pdf">UjBsR09EbGhjZ0dTQUxNQU</cbc:EmbeddedDocumentBinaryObject> ```


Maps to the UBL JSON


```"externalReference":{"uri":"https://mydomain.com/invoice1234.pdf"} ```

Or

```<cac:ExternalReference><cbc:URI>https://mydomain.com/invoice1234.pdf</cbc:URI> <cbc:DocumentHash>H5DGjk67g3SDllk</cbc:DocumentHash><cbc:HashAlgorithmMethod>SHA-1</cbc:HashAlgorithmMethod></cac:ExternalReference> ```

Maps to the UBL JSON

```"externalReference":{"uri":"https://mydomain.com/invoice1234.pdf", "documentHash":"H5DGjk67g3SDllk", "hashAlgorithmMethod":"SHA-1"}```

### CCTS CodeType

The reference codelist scheme for each coded element in a UBL JSON is defined as part of the implementation profile and is referenced through the CustomizationID as described in the [code-list specification](http://ausdigital-code.readthedocs.io/en/latest/).

Therefore the UBL XML

```<cac:Country> <cbc:IdentificationCode listAgencyID="5" listAgencyName="International Organization for Standardization" listName="Country Identification Code" listVersionID="SecondEdition2006VI-12" name="CountryIdentificationCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/default/CountryIdentificationCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:identifierlist:standard:5:ISO316612A">AU</cbc:IdentificationCode> </cac:Country>```

Maps to the UBL JSON

 ```"country":"AU" ``` 
 
### CCTS DateTimeType
 
JSON representation SHALL always use ISO-8601 standard datetime format.
 
Therefore the UBL XML

```<cbc:IssueDate format="iso-8601">2016-07-01</cbc:IssueDate>```
 
Maps to the UBL JSON

```"issueDate":"2016-07-01" ```

### CCTS IdentifierType

The JSON code-lists specification SHALL maintain normative code-list of unique short names (eg "ABN") for each identifier category (eg party), thereby allowing a XML code type to map to a simple name-value pair in JSON.

Therefore the UBL XML

```<cac:PartyIdentification> <cbc:ID schemeID="urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151" schemeName="ABN" schemeAgencyID="ato.gov.au" schemeAgencyName="Australian Taxation Office" schemeVersionID=" " schemeDataURI="http://abr.business.gov.au/abrxmlsearchRPC/Forms/SearchByABNv201408.aspx" schemeURI="urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151"> 51083392303</cbc:ID> </cac:PartyIdentification>``` 
 
Maps to the UBL JSON

```"partyIdentification":[{"ABN":"51083392303"}] ``` 

### CCTS IndicatorType 

JSON representation SHALL always use the JSON boolean type

Therefore the UBL XML

```<cbc:ChargeIndicator>false</cbc:ChargeIndicator> ```
 
Maps to the UBL JSON
 
```"chargeIndicator":false ```

### MeasureType 

JSON representation SHALL be as a value/unit object tuple and, like UBL XML, will always use UN/ECE Rec20 code list

Therefore the UBL XML

```<cbc:GrossWeightMeasure unitCode="KGM" unitCodeListVersionID="urn:un:unece:uncefact:codelist:specification:66411:8e">130 </cbc:GrossWeightMeasure>``` 
 
Maps to the UBL JSON
 
```"grossWeightMeasure":{"value":130, "unitCode":"KGM"} ```

### CCTS NumericType

JSON representation SHALL always use the JSON Number type 

Therefore the UBL XML

```<cbc:Percent format="decimal">5.0 </cbc:Percent>``` 
 
Maps to the UBL JSON
 
```"percent"51.0 ```

### CCTS QuantityType

JSON representation SHALL be as a value/unit object tuple and, like UBL XML, will always use UN/ECE Rec20 code list

Therefore the UBL XML

```<cbc:InvoicedQuantity unitCode="KG" unitCodeListID="urn:un:unece:uncefact:codelist:specification:66411:8e" unitCodeListAgencyID="UN/ECE" unitCodeListAgencyName="United Nations Economic Commission for Europe">100 </cbc:InvoicedQuantity>``` 
 
Maps to the UBL JSON
 
 ```"invoicedQuantity":{"value":100, "unitCode":"KG"} ```
 
### CCTS TextType
 
JSON representation SHALL NOT specify language for every string data element.  Instead it will use the UBL document level language indicator "Language":"EN" using ISO 639-1 language codes

Therefore the UBL XML

```<cbc:Name languageID="EN" languageLocaleID="AU">ACME Pty Ltd</cbc:Name>``` 
 
Maps to the UBL JSON
 
```"name":"ACME Pty Ltd" ```
