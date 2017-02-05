# Transformation API

The transformation API specification defiens a standard interface for lossless, schema-aware transformations 

* UBL2JSON : from standard namespace qualified UBL 2.1 XML and a simple JSON representation. 

The XML instances MUST comply with the [UBL 2.1 Specification](http://docs.oasis-open.org/ubl/UBL-2.1.html).

## API Specification

Is maintained at swaggerhub : **[UBL-JSON APIs](https://app.swaggerhub.com/api/ausdigital/ausdigital-syn/1.0)**

## UBL2JSON Error Response Codes

All error responses will comply with the ausdigital standard [RESTful Errors structure](https://app.swaggerhub.com/domains/ausdigital/ErrorModel/1.0).   

|Error Code | Error Message|
|-----------|--------------|
|ubl2json-01 |The source document is not a valid UBL 2.1 XML instance   |
|ubl2json-02 |Standard code-list scheme not found for {schemeID} at {XML element path}|
|ubl2json-02 |   |

## Sample UBL 2.1 XML

```
<n2:Invoice 
  xmlns:n2="urn:oasis:names:specification:ubl:schema:xsd:Invoice-2" 
  xmlns:cbc="urn:oasis:names:specification:ubl:schema:xsd:CommonBasicComponents-2" 
  xmlns:cac="urn:oasis:names:specification:ubl:schema:xsd:CommonAggregateComponents-2">
  <cbc:UBLVersionID>2.1</cbc:UBLVersionID>
  <cbc:CustomizationID schemeAgencyID="dbc">urn:resources.digitalbusinesscouncil.com.au:dbc:invoicing:documents:core invoice:xsd::core invoice 1##urn:resources.digitalbusinesscouncil.com.au:dbc:einvoicing:process:einvoicing03:ver1.0</cbc:CustomizationID>
  <cbc:ProfileID schemeAgencyID="dbc">urn:resources.digitalbusinesscouncil.com.au:dbc:einvoicing:ver1.0</cbc:ProfileID>
  <cbc:ID>TOSL-108-A</cbc:ID>
  <cbc:IssueDate>2016-07-01</cbc:IssueDate>
  <cbc:DueDate>2016-08-01</cbc:DueDate>
  <cbc:InvoiceTypeCode listAgencyID="6" listAgencyName="International Organization for Standardization" listName="Document Type Code" listVersionID="D10B" name="DocumentTypeCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/special-purpose/DocumentTypeCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:codelist:standard:UNECE:DocumentNameCodeAccounting">81</cbc:InvoiceTypeCode>
  <cbc:Note>Credit Note</cbc:Note>
  <cbc:DocumentCurrencyCode listID="ISO 4217 Alpha" listAgencyID="5" listAgencyName="International Organization for Standardization" listName="Currency Code" listVersionID="2012-01-12" name="CurrencyCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/default/CurrencyCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:codelist:standard:5:ISO42173A">AUD</cbc:DocumentCurrencyCode>
  <cbc:BuyerReference>CC-3352626</cbc:BuyerReference>
  <cac:InvoicePeriod>
    <cbc:StartDate>2016-05-01</cbc:StartDate>
    <cbc:EndDate>2016-06-01</cbc:EndDate>
  </cac:InvoicePeriod>
  <cac:OrderReference>
    <cbc:ID>SB002</cbc:ID>
  </cac:OrderReference>
  <cac:AccountingSupplierParty>
    <cac:Party>
      <cac:PartyIdentification>
        <cbc:ID schemeID="urn:oasis:names:tc:ebcore:partyid-type:iso6523:088" schemeAgencyID="GS1">4035811611014</cbc:ID>
      </cac:PartyIdentification>
      <cac:PartyName>
        <cbc:Name>ACME Holdings</cbc:Name>
      </cac:PartyName>
      <cac:PostalAddress>
        <cbc:CityName>Adelaide</cbc:CityName>
        <cbc:PostalZone>5000</cbc:PostalZone>
        <cbc:CountrySubentity>South Australia</cbc:CountrySubentity>
        <cac:AddressLine>
          <cbc:Line>88 Grenfell St</cbc:Line>
        </cac:AddressLine>
        <cac:Country>
          <cbc:IdentificationCode listAgencyID="5" listAgencyName="International Organization for Standardization" listName="Country Identification Code" listVersionID="SecondEdition2006VI-12" name="CountryIdentificationCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/default/CountryIdentificationCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:identifierlist:standard:5:ISO316612A">AU</cbc:IdentificationCode>
        </cac:Country>
      </cac:PostalAddress>
      <cac:PartyLegalEntity>
        <cbc:CompanyID schemeID="urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151" schemeAgencyID="GS1">987654321</cbc:CompanyID>
      </cac:PartyLegalEntity>
    </cac:Party>
  </cac:AccountingSupplierParty>
  <cac:AccountingCustomerParty>
    <cac:Party>
      <cac:PartyIdentification>
        <cbc:ID schemeID="urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151" schemeAgencyID="GS1">51083392303</cbc:ID>
      </cac:PartyIdentification>
      <cac:PartyName>
        <cbc:Name>Governmment Agency</cbc:Name>
      </cac:PartyName>
      <cac:PostalAddress>
        <cbc:CityName>Willunga</cbc:CityName>
        <cbc:PostalZone>5172</cbc:PostalZone>
        <cbc:CountrySubentity>South Australia</cbc:CountrySubentity>
        <cac:AddressLine>
          <cbc:Line>Delabole Road</cbc:Line>
        </cac:AddressLine>
        <cac:Country>
          <cbc:IdentificationCode listAgencyID="5" listAgencyName="International Organization for Standardization" listName="Country Identification Code" listVersionID="SecondEdition2006VI-12" name="CountryIdentificationCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/default/CountryIdentificationCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:identifierlist:standard:5:ISO316612A">AU</cbc:IdentificationCode>
        </cac:Country>
      </cac:PostalAddress>
      <cac:PartyLegalEntity>
        <cbc:CompanyID schemeID="urn:oasis:names:tc:ebcore:partyid-type:iso6523:0151" schemeAgencyID="GS1">51083392303</cbc:CompanyID>
      </cac:PartyLegalEntity>
    </cac:Party>
    <cac:BuyerContact>
      <cbc:ID>Tony Curtis</cbc:ID>
      <cbc:Telephone>(08) 8556 2345</cbc:Telephone>
      <cbc:ElectronicMail>curtis@willunga.gov.au</cbc:ElectronicMail>
    </cac:BuyerContact>
  </cac:AccountingCustomerParty>
  <cac:Delivery>
    <cbc:ActualDeliveryDate>2016-03-02</cbc:ActualDeliveryDate>
    <cac:DeliveryAddress>
      <cbc:CityName>Adelaide</cbc:CityName>
      <cbc:PostalZone>5000</cbc:PostalZone>
      <cbc:CountrySubentity>South Australia</cbc:CountrySubentity>
      <cac:AddressLine>
        <cbc:Line>202 North Terrace</cbc:Line>
      </cac:AddressLine>
      <cac:Country>
        <cbc:IdentificationCode listAgencyID="5" listAgencyName="International Organization for Standardization" listName="Country Identification Code" listVersionID="SecondEdition2006VI-12" name="CountryIdentificationCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/default/CountryIdentificationCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:identifierlist:standard:5:ISO316612A">AU</cbc:IdentificationCode>
      </cac:Country>
    </cac:DeliveryAddress>
  </cac:Delivery>
  <cac:PaymentMeans>
    <cbc:ID>EFT</cbc:ID>
    <cbc:PaymentMeansCode listID="UN/ECE 4461" listAgencyID="6" listAgencyName="United Nations Economic Commission for Europe" listName="Payment Means Code" listVersionID="D10B" name="PaymentMeansCode" languageID="en" listURI="http://docs.oasis-open.org/ubl/os-UBL-2.1/cl/gc/default/PaymentMeansCode-2.1.gc" listSchemeURI="urn:un:unece:uncefact:codelist:standard:UNECE:PaymentMeansCode">2</cbc:PaymentMeansCode>
    <cbc:InstructionID>888276612262653</cbc:InstructionID>
    <cac:PayeeFinancialAccount>
      <cbc:ID>2000987211</cbc:ID>
      <cac:FinancialInstitutionBranch>
        <cbc:ID schemeName="BSB">086016</cbc:ID>
        <cbc:Name>NAB Adelaide</cbc:Name>
      </cac:FinancialInstitutionBranch>
    </cac:PayeeFinancialAccount>
  </cac:PaymentMeans>
  <cac:TaxTotal>
    <cbc:TaxAmount>250.00</cbc:TaxAmount>
  </cac:TaxTotal>
  <cac:LegalMonetaryTotal>
    <cbc:LineExtensionAmount>2500.00</cbc:LineExtensionAmount>
    <cbc:TaxExclusiveAmount>2500.00</cbc:TaxExclusiveAmount>
    <cbc:TaxInclusiveAmount>2750.00</cbc:TaxInclusiveAmount>
    <cbc:PayableAmount>2750.00</cbc:PayableAmount>
  </cac:LegalMonetaryTotal>
  <cac:InvoiceLine>
    <cbc:ID>1</cbc:ID>
    <cbc:Note>A variety of Widgets</cbc:Note>
    <cbc:InvoicedQuantity>200</cbc:InvoicedQuantity>
    <cbc:LineExtensionAmount>2000.00</cbc:LineExtensionAmount>
    <cac:TaxTotal>
      <cbc:TaxAmount>200.00</cbc:TaxAmount>
      <cac:TaxSubtotal>
        <cbc:TaxAmount>10.00</cbc:TaxAmount>
        <cac:TaxCategory>
          <cac:TaxScheme>
            <cbc:ID>GST</cbc:ID>
          </cac:TaxScheme>
        </cac:TaxCategory>
      </cac:TaxSubtotal>
    </cac:TaxTotal>
    <cac:Item>
      <cbc:Description>Widget</cbc:Description>
      <cac:SellersItemIdentification>
        <cbc:ID>WDGT-A1733-0436</cbc:ID>
      </cac:SellersItemIdentification>
      <cac:StandardItemIdentification>
        <cbc:ID schemeID="GTIN" schemeAgencyID="GS1">9501101021037</cbc:ID>
      </cac:StandardItemIdentification>
    </cac:Item>
    <cac:Price>
      <cbc:PriceAmount>10.00</cbc:PriceAmount>
      <cbc:BaseQuantity>1</cbc:BaseQuantity>
    </cac:Price>
  </cac:InvoiceLine>
  <cac:InvoiceLine>
    <cbc:ID>2</cbc:ID>
    <cbc:Note>Widget attachments</cbc:Note>
    <cbc:InvoicedQuantity>200</cbc:InvoicedQuantity>
    <cbc:LineExtensionAmount>500.00</cbc:LineExtensionAmount>
    <cac:TaxTotal>
      <cbc:TaxAmount>50.00</cbc:TaxAmount>
      <cac:TaxSubtotal>
        <cbc:TaxAmount>10.00</cbc:TaxAmount>
        <cac:TaxCategory>
          <cac:TaxScheme>
            <cbc:ID>GST</cbc:ID>
          </cac:TaxScheme>
        </cac:TaxCategory>
      </cac:TaxSubtotal>
    </cac:TaxTotal>
    <cac:Item>
      <cbc:Description>Widget screws</cbc:Description>
      <cac:SellersItemIdentification>
        <cbc:ID>WDGT-A1733-0437</cbc:ID>
      </cac:SellersItemIdentification>
      <cac:StandardItemIdentification>
        <cbc:ID schemeID="GTIN" schemeAgencyID="GS1">9501101021038</cbc:ID>
      </cac:StandardItemIdentification>
    </cac:Item>
    <cac:Price>
      <cbc:PriceAmount>2.50</cbc:PriceAmount>
      <cbc:BaseQuantity>1</cbc:BaseQuantity>
    </cac:Price>
  </cac:InvoiceLine>
</n2:Invoice>
```