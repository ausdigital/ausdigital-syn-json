#OASIS UBL JSON Specification Review


The main idea of the specification:
>Future versions of UBL may raise the cardinality of any given object and so this approach is future-proof to revisions.
This approach is also consistent and is used without thought of cardinality and so should be easier for the programmer 
(if a little verbose). Although a change in cardinality does not apply to the Document ABIE, the array notation is used 
for consistency with ASBIE expressions using arrays.

It works for cardinality changes in different UBL versions, but only partially.

## Potential data loss

For example, in UBL-Invoice-2.1.xsd:

`<xsd:element ref="cac:InvoicePeriod" minOccurs="0" maxOccurs="unbounded"/>`

And let's assume in UBL-Invoice-2.2.xsd maxOccurs changes from "unbound" to "1":

`<xsd:element ref="cac:InvoicePeriod" minOccurs="0" maxOccurs="1"/>`

In this case in JSON schema UBL 2.1:

```
"InvoicePeriod": {
	"title": "Invoice. Invoice_ Period. Period",
	"description": "A period to which the Invoice applies.",
	"items": {
		"$ref": "../common/UBL-CommonAggregateComponents-2.1.json#/definitions/InvoicePeriod"
	},
	"minItems": 1,
	"additionalProperties": false,
	"type": "array"
},
```

And in 2.2:


```
"InvoicePeriod": {
	"title": "Invoice. Invoice_ Period. Period",
	"description": "A period to which the Invoice applies.",
	"items": {
		"$ref": "../common/UBL-CommonAggregateComponents-2.1.json#/definitions/InvoicePeriod"
	},
	"maxItems": 1,
	"minItems": 1,
	"additionalProperties": false,
	"type": "array"
},
```
The good point is that property type hasn't been changed - it remains array. That means that no need to re-write code 
which is parsing JSON documents.

But if cardinality change is of the other type - increase maxOccurs.

For example:

`<xsd:element ref="cac:OrderReference" minOccurs="0" maxOccurs="1"/>`

and 

```

"OrderReference": {
	"title": "Invoice. Order Reference",
	"description": "A reference to the Order with which this Invoice is associated.",
	"items": {
		"$ref": "../common/UBL-CommonAggregateComponents-2.1.json#/definitions/OrderReference"
	},
	"maxItems": 1,
	"minItems": 1,
	"additionalProperties": false,
	"type": "array"
}

```

Let's assume maxOccurs is changed from "1" to "unbound":

`<xsd:element ref="cac:OrderReference" minOccurs="0" maxOccurs="unbound"/>`

In this case, we get for an array with unlimited number of or items:

```

"OrderReference": {
	"title": "Invoice. Order Reference",
	"description": "A reference to the Order with which this Invoice is associated.",
	"items": {
		"$ref": "../common/UBL-CommonAggregateComponents-2.1.json#/definitions/OrderReference"
	},
	"minItems": 1,
	"additionalProperties": false,
	"type": "array"
}

```

Again, the type remains the same, so code, which is processing JSON will still be working. But here is the potential issue - 
all the items except first will be ignored, if a developer will not update processing logic, which is very likely as the code is working.

If increasing maxOccurs is not a valid case for newer versions, it is highly recommended to defined properties with maxOccurs="1"
as objects and not as arrays.

## Processing code is a mess:

To get IdentifierContent you need to go through 5 loops:

`ROOT.Invoice[i].InvoiceLine[j].Item[k].StandardItemIdentification[l].ID[m].IdentificationSchemeIdentifier`

If to define properties as objects when maxOccurs="1", you will need only 1 loop:

`ROOT.Invoice.InvoiceLine[i].Item.StandardItemIdentification.ID.IdentificationSchemeIdentifier`

Of course, you can always access items but their index without loop if you know that maxItems = 1:

`ROOT.Invoice[0].InvoiceLine[i].Item[0].StandardItemIdentification[0].ID[0].IdentificationSchemeIdentifier`

But to know this you need to go to json schema and check.

This is very likely that a lot of developers will generate POJOs from json schema and they will get a bunch of classes, 
where each property is a List.

## Namespace URI properties

Although they are optional still they do not make much sense and likely will not be processed as they do not add anything 
to validation against json schemas. 
It would be good to add recommendation for using json schemas according to http://json-schema.org/latest/json-schema-core.html#rfc.section.9
