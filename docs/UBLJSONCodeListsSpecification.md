**[Back to AusDigital.org](http://ausdigital.org/)**

# UBL Syntax 2.0 Extended Codes Lists Specification

 * ![raw](http://rfc.unprotocols.org/spec:2/COSS/raw.svg)
 * Editor: Steve Capell
 * Contributors: 

## Introduction

This specification defines a JSON representation for standard code lists (eg the ISO-3166 country code list) and also for context specific subsets and extensions.  
The specification for standard Code Lists (eg the ISO-3166 country code list) is maintained separately - [UBL Codes Lists Management Specification](http://ausdigital-code.readthedocs.io).  

The JSON code list representation and standard API definition provides a alternative to the UBL [Genericode](https://docs.oasis-open.org/codelist/cs-genericode-1.0/doc/oasis-code-list-representation-genericode.html) and [Context/Value Association](http://docs.oasis-open.org/codelist/cs01-ContextValueAssociation-1.0/doc/context-value-association.html) specifications (and the associated XSLT based runtime validation framework) for implementers that prefer a REST/JSON model.

## Context specific lists

**[DBC context specific code lists repository](https://github.com/ausdigital/ausdigital-syn/tree/master/codes/contexts)**

Code lists often need to be restricted or extended for specific business contexts.  For example, in the Australian context, it may be useful to remove most of the European specific payment means codes from the standard UBL code list and to add a code for BPAY.  
  
 * The context specific lists are maintained in the /contexts path and MUST be a copy of a code list in the /core path with only the following changes:
    * Addition of a suffix "contextnn" to the file name - for example PaymentMeansCode-2.1-context01.json
    * "ListURI" element to reflect the new code list URL.
    * A list of ProcessID references as shown in the example above.  These MUST match the "customizationID" field contents in the corresponding business documents that will use the context specific code list.
    * New codes MAY added or existing codes MAY be removed from the object array of codes as needed for the business context - but existing codes MUST NOT be redefined.
  
To create a context specific code list, clone this repository, create the new code list, notify the working group via the slack channel, and issue a pull request.

## Identifier Scheme Lists

**[identifier scheme code lists repository](https://github.com/ausdigital/ausdigital-syn/tree/master/codes/identifiers)** 

This specification introduces an additional type of code list that is a reference list of identifier schemes.  The purpose of these lists is to deliver consistency in the UBL representation of identifier schemes like the ABN as a Party identification.  The code lists also support lossless transformation between simple JSON elements like "ABN":"34132141612" and the full CCTS compliant UBL XML representation by including the CCTS Scheme identification information with each scheme code. 

```
  "CodeList": {
    "ListURI": "https://github.com/ausdigital/ausdigital-code/tree/master/syn/codes/identifiers/PartyIdentifiers-dbc.01.json",
    "SchemeIdentification": {
      "listAgencyName": "Digital Business COuncil",
      "ListID": "PartyID",
      "listSchemeURI": "urn:codes.digitalbusinesscouncil.com.au:partyIdentifiers:ver1.0",
      "ListName": "Party Identifier Schemes",
      "listAgencyID": "Uigitalbusinesscouncil.com.au",
      "listVersionID": "1.0"
    },
    "Codes": [
      {
        "Code": "ABN",
        "Name": "IAustralian Business Number",
        "SchemeIdentification":{
          "schemeID":"urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151",
          "schemeName":"ABN",
          "schemeAgencyID":"ato.gov.au",
          "schemeAgencyName":"Australian Taxation Office",
          "schemeVersionID":"1.0",
          "schemeDataURI":"http://abr.business.gov.au/abrxmlsearchRPC/Forms/SearchByABNv201408.aspx",
          "schemeURI":"urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151"
        },
      },
      {
        "etc":" "
      },
    ]
  }
}
```

## Validation API behaviour

The document validation API behaviour is designed to allow codes used within messages to be validated against a context specific code list where appropriate but to default to the standard code list where a restricted version is not defined for a given process.  The [validation API](https://github.com/ausdigital/ausdigital-syn/blob/master/docs/ValidationAPI.md) SHALL

 * For each code data type in the document, validate the code against the code list identified by the SchemeURI (eg "urn:un:unece:uncefact:codelist:standard:UNECE:PaymentMeansCode:D10B").
 * If the "customizationID" element is present in the document, then use the context specific code list with a matching identifier in the "ProcessID" array.  Otherwise, use the standard code list.
 * There are several error conditions.  The error code and error message are described in the table below and MUST be inserted into the standard error structure defined in the The [validation API](https://github.com/ausdigital/ausdigital-syn/blob/master/docs/ValidationAPI.md) specification.

|Error Code|Error Message|
|----------|-------------|
|Code-01|The value {code value} for data item {data element name} cannot be validated because there is no code list for scheme {schemeURI}.|
|Code-02|The value {code value} for data item {data element name} is not valid in {list name} code list.|
|Code-03|The value {code value} for data item {data element name} is not valid in {list name} restricted code list for context {ProcessID}.|

## Licence

Copyright (c) 2016 the Editor and Contributors. All rights reserved.

This Specification is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

This Specification is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, see [http://www.gnu.org/licenses](http://www.gnu.org/licenses).


## Change Process

This document is governed by the [2/COSS](http://rfc.unprotocols.org/spec:2/COSS/) (COSS).


## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

