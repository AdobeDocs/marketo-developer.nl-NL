---
title: "Bulk Custom Object Import"
feature: Custom Objects
description: "Batch importeren van aangepaste objecten."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 0%

---


# Aangepast object bulksgewijs importeren

[Naslaggids voor aangepast object bulken](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects)

Als u veel aangepaste objectreeks wilt importeren, kunt u deze het beste asynchroon importeren met de bulk-API. Hiertoe importeert u een vlak bestand dat gescheiden records bevat (komma, tab of puntkomma). Het bestand kan een willekeurig aantal records bevatten, op voorwaarde dat het bestand kleiner is dan 10 MB (anders wordt een HTTP 413-statuscode geretourneerd). De inhoud van het bestand is afhankelijk van de definitie van het aangepaste object. De eerste rij bevat altijd een koptekst waarin de velden worden vermeld waarin waarden van elke rij moeten worden toegewezen. Alle veldnamen in de header moeten overeenkomen met een API-naam (zoals hieronder wordt beschreven). Resterende rijen bevatten de te importeren gegevens, één record per rij. De recordbewerking is alleen &quot;invoegen of bijwerken&quot;.

## Verwerkingslimieten

U kunt meer dan één aanvraag voor bulkimport indienen, binnen de limieten. Elke aanvraag wordt als een taak toegevoegd aan een FIFO-wachtrij die moet worden verwerkt. Er worden maximaal twee banen tegelijk verwerkt. De wachtrij kan maximaal tien taken tegelijk uitvoeren (inclusief de twee momenteel verwerkte taken). Als u het maximum van tien taken overschrijdt, wordt de fout &quot;1016, Te veel import&quot; geretourneerd.

## Voorbeeld van aangepast object

Voordat u de bulk-API kunt gebruiken, moet u eerst de gebruikersinterface van Marketo Admin gebruiken voor [een aangepast object maken](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects). Stel bijvoorbeeld dat we een aangepast object &#39;Auto&#39; hebben gemaakt met de velden &#39;Kleur&#39;, &#39;Merk&#39;, &#39;Model&#39; en &#39;VIN&#39;. Hieronder ziet u de Admin UI-schermen die het aangepaste object weergeven. U ziet dat we VIN-veld hebben gebruikt voor deduplicatie. De API-namen worden gemarkeerd, omdat ze moeten worden gebruikt bij het aanroepen van API-gerelateerde eindpunten in bulk.

![Aangepast object invoegen](assets/bulk-insert-co-car-1.png)

Hier volgen de aangepaste objectvelden die worden weergegeven in de interface voor beheer.

![Aangepast object-velden invoegen](assets/bulk-insert-co-car-fields.png)

### API-namen

U kunt API-namen programmatisch ophalen door de API-naam van het aangepaste object aan de [Beschrijf aangepast object](#describe) eindpunt.

```
/rest/v1/customobjects/{apiName}/describe.json
```

```json
{
    "requestId": "46ff#15a686e66de",
    "result": [
        {
            "name": "car_c",
            "displayName": "Car",
            "description": "It's a car.",
            "createdAt": "2017-02-22T19:55:51Z",
            "updatedAt": "2017-02-22T19:55:51Z",
            "idField": "marketoGUID",
            "dedupeFields": [
                "vin"
            ],
            "searchableFields": [
                [
                    "vin"
                ],
                [
                    "marketoGUID"
                ]
            ],
            "fields": [
                {
                    "name": "createdAt",
                    "displayName": "Created At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "marketoGUID",
                    "displayName": "Marketo GUID",
                    "dataType": "string",
                    "length": 36,
                    "updateable": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "Updated At",
                    "dataType": "datetime",
                    "updateable": false
                },
                {
                    "name": "color",
                    "displayName": "Color",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "make",
                    "displayName": "Make",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "model",
                    "displayName": "Model",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                },
                {
                    "name": "vin",
                    "displayName": "VIN",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true
                }
            ]
        }
    ],
    "success": true
}
```

### Bestand importeren

Stel nu dat u drie aangepaste objectrecords &#39;Auto&#39; wilt importeren. Met een door komma&#39;s gescheiden indeling (CSV) kan het bestand er als volgt uitzien:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

Regel 1 is de kopbal, en lijnen 2-4 zijn de verslagen van de douaneobjecten gegevens.

## Een taak maken

Als u een aanvraag voor bulkimport wilt uitvoeren, moet u de API-naam van het aangepaste object opnemen in het pad naar het dialoogvenster [Aangepaste objecten importeren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Identity/operation/identityUsingPOST) eindpunt. U moet ook een parameter &quot;file&quot; opnemen die naar de naam van het importbestand verwijst, en een parameter &quot;format&quot; die aangeeft hoe het importbestand wordt gescheiden (&quot;csv&quot;, &quot;tsv&quot; of &quot;ssv&quot;).

```
POST /bulk/v1/customobjects/{apiName}/import.json?format=csv
```

```
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Length: 290
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo
Content-Disposition: form-data; name="file"; filename="custom_object_import.csv"
Content-Type: text/csv

color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
------WebKitFormBoundaryXjWP6BP8Ciq6bPeo--
```

```json
{
    "requestId": "c015#15a68a23418",
    "result": [
        {
            "batchId": 1013,
            "status": "Queued",
            "objectApiName": "car_c"
        }
    ],
    "success": true
}
```

In dit voorbeeld hebben we de indeling &quot;csv&quot; opgegeven en ons importbestand de naam &quot;custom_object_import.csv&quot; gegeven.

Bericht in het antwoord op onze vraag, hier is geen lijst van successen of mislukkingen zoals u van het eindpunt van de Objecten van de Douane van de Synchronisatie zou terugkomen. In plaats daarvan ontvangt u een `batchId`. Dit is omdat de vraag asynchroon is, en kan terugkeren a `status` van &quot;In wachtrij&quot;, &quot;Importeren&quot; of &quot;Mislukt&quot;. U moet de batchId behouden, zodat u de status van de importtaak kunt ophalen of fouten en/of waarschuwingen kunt ophalen wanneer de taak is voltooid. De batchId blijft zeven dagen geldig.

Een eenvoudige manier om de aanvraag voor bulkimport te repliceren is door krullen vanaf de opdrachtregel te gebruiken:

```
curl -X POST -i -F format='csv' -F file='@custom_object_import.csv' -F access_token='<Access Token>' <REST API Endpoint URL>/bulk/v1/customobjects/car_c/import.json
```

Waar het importbestand &quot;custom_object_import.csv&quot; het volgende bevat:

```
color,make,model,vin
red,bmw,2002,WBA4R7C55HK895912
yellow,bmw,320i,WBA4R7C30HK896061
blue,bmw,325i,WBS3U9C52HP970604
```

## Status opiniepeilingtaak

Nadat de importtaak is gemaakt, moet u een query uitvoeren op de status ervan. Het is aan te raden de importtaak elke 5-30 seconden te peilen. Dit doet u door de API-naam van het aangepaste object en de `batchId` in het pad naar [Aangepaste objectstatus importeren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) eindpunt.

```
GET /bulk/v1/customobjects/{apiName}/import/{batchId}/status.json
```

```json
{
    "requestId": "2a5#15a68dd9be1",
    "result": [
        {
            "batchId": 1013,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "importTime": "2 second(s)",
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

In dit antwoord ziet u een voltooide import, maar de `status` kan een van de volgende zijn: Voltooid, In wachtrij geplaatst, Importeren, Mislukt. Als de taak is voltooid, ziet u een overzicht van het aantal verwerkte rijen, met fouten en waarschuwingen. Het berichtattribuut is ook een goede plaats om naar extra baaninformatie te zoeken.

## Mislukt

Ontbrekende gegevens worden aangegeven door de `numOfRowsFailed` kenmerk in [Aangepaste objectstatus importeren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectStatusUsingGET) reactie. Als numOfRowsFailed groter is dan nul, dan wijst die waarde op het aantal mislukkingen die voorkwamen. Bellen [Importeren van aangepast object mislukt](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectFailuresUsingGET) om een dossier met mislukkingsdetail te verkrijgen. Opnieuw moet u de API-naam van het aangepaste object doorgeven en `batchId` in het pad. Als er geen foutbestand bestaat, wordt een HTTP 404-statuscode geretourneerd.

Als u doorgaat met het voorbeeld, kunt u een fout forceren door de koptekst te wijzigen en &quot;vin&quot; in &quot; vin&quot; te wijzigen (door een spatie tussen de komma en &quot;vin&quot; toe te voegen).

```
color,make,model, vin
```

Wanneer we de status opnieuw importeren en controleren, zien we deze reactie met `numRowsFailed`3. Dit wijst op drie mislukkingen.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/status.json
```

```json
{
    "requestId": "12260#15a68f491ed",
    "result": [
        {
            "batchId": 1016,
            "operation": "import",
            "status": "Complete",
            "objectApiName": "car_c",
            "numOfObjectsProcessed": 0,
            "numOfRowsFailed": 3,
            "numOfRowsWithWarning": 0,
            "importTime": "1 second(s)",
            "message": "Import completed with errors, 0 records imported (0 members), 3 failed"
        }
    ],
    "success": true
}
```

Nu maken wij Get het eindpuntvraag van Objecten van de Douane van de Invoer om extra mislukkingsdetail te krijgen:

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/failures.json
```

```
color,make,model, vin,Import Failure Reason
red,bmw,2002,WBA4R7C55HK895912,missing.dedupe.fields
yellow,bmw,320i,WBA4R7C30HK896061,missing.dedupe.fields
blue,bmw,325i,WBS3U9C52HP970604,missing.dedupe.fields
```

We kunnen zien dat we ons deduplicatieveld missen `vin`.

## Waarschuwingen

Waarschuwingen worden aangegeven door de `numOfRowsWithWarning` kenmerk in de reactie Aangepaste objectstatus importeren. Als numOfRowsWithWarning groter is dan nul, dan wijst die waarde op het aantal waarschuwingen die voorkwamen. Bellen [Waarschuwingen bij eigen object importeren ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Custom-Objects/operation/getImportCustomObjectWarningsUsingGET) om een dossier met waarschuwingsdetail te verkrijgen. Opnieuw moet u de API-naam van het aangepaste object doorgeven en `batchId` in het pad. Als er geen waarschuwingsbestand bestaat, wordt een HTTP 404-statuscode geretourneerd.

```
GET /bulk/v1/customobjects/car_c/import/{batchId}/warnings.json
```
