---
title: Bulkimport
feature: REST API
description: Marketo Bulk-import voor het laden van leads, aangepaste objecten en programmaleden via uploads van meerdere delen, het maken van asynchrone taken, de opiniepeilingstatus en het afhandelen van fouten.
exl-id: f7922fd2-8408-4d04-8955-0f8f58914d24
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---

# Bulkimport

Marketo biedt interfaces voor het invoegen van grote sets met aan personen gerelateerde gegevens, genaamd Bulk Import. Interfaces worden momenteel aangeboden voor drie objecttypen:

- Leads (personen)
- Aangepaste objecten
- Programmaleden

Bulkimport wordt uitgevoerd door een taak te maken en vervolgens wordt gewacht tot de taak het lezen van een bestand heeft voltooid. Deze taken worden asynchroon uitgevoerd en kunnen worden gepolled om de status van het importeren op te halen. Bestanden worden geüpload met HTTP multipart/form-data per RFC 2399.

Eindpunten van de Bulk-API worden niet voorafgegaan door &#39;/rest&#39;, zoals andere eindpunten.

## Verificatie

De bulkimport-API&#39;s gebruiken dezelfde OAuth 2.0-verificatiemethode als andere Marketo REST-API&#39;s.  Hiervoor is een geldig toegangstoken vereist dat als HTTP-header `Authorization: Bearer {_AccessToken_}` wordt verzonden.

>[!IMPORTANT]
>
>De steun voor authentificatie die **gebruikt access_token** vraagparameter wordt verwijderd op 30 Juni, 2025. Als uw project een vraagparameter gebruikt om het toegangstoken over te gaan, zou het moeten worden bijgewerkt om de **1&rbrace; kopbal van de Vergunning &lbrace;zo spoedig mogelijk te gebruiken.** De nieuwe ontwikkeling zou de **kopbal van de Vergunning** exclusief moeten gebruiken.

## Limieten

- Max. gelijktijdige importtaken: 2
- Max. aantal taken bij het importeren in de wachtrij (inclusief taken die momenteel worden geïmporteerd): 10
- Maximale grootte van importbestand: 10 MB

## Machtigingen

Bulk Import gebruikt hetzelfde machtigingenmodel als de Marketo REST API en vereist geen extra speciale machtigingen om te gebruiken, hoewel specifieke machtigingen vereist zijn voor elke set eindpunten.

## Opnamebewerkingen

Bulkimport is een recordbewerking &quot;invoegen of bijwerken&quot;. Als een overeenkomende record wordt gevonden in de database, wordt deze bijgewerkt. Anders wordt een nieuwe record gemaakt. De reactie op bulkimport geeft niet aan of een bepaalde record is bijgewerkt of ingevoegd.

## Een taak maken

Marketo-API&#39;s voor bulkimport gebruiken het concept van een taak voor het uitvoeren van gegevensimporten. Kijk bij het creëren van een eenvoudige lood invoerbaan gebruikend de [&#x200B; Invoer leidt &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) eindpunt.  Merk op dat dit eindpunt [&#x200B; multipart/form-data als inhoud-type &#x200B;](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html) gebruikt. Dit kan lastig zijn om de juiste taal te kiezen, dus de beste praktijk is om een HTTP-ondersteuningsbibliotheek te gebruiken voor uw taal van keuze.  Als u enkel uw voeten nat krijgt, stellen wij voor dat u [&#x200B; krullen &#x200B;](https://curl.se/) gebruikt.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=--------------------------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

firstName,lastName,email
Able,Baker,ablebaker@marketo.com
Charlie,Dog,charliedog@marketo.com
Easy,Fox,easyfox@marketo.com
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

Met dit verzoek wordt een taak samengesteld die waarden importeert uit het CSV-bestand met de naam &quot;lead.csv&quot; met de kolomkoppen &quot;FirstName&quot;, &quot;LastName&quot;, &quot;Email&quot; en &quot;Company&quot;.

```json
{
    "requestId": "d01f#15d672f8560",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Queued"
        }
    ],
    "success": true
}
```

Als we de taak verzenden, wordt er een batchId geretourneerd, die we vervolgens kunnen gebruiken om de status te controleren.

### Algemene parameters

Elk eindpunt van de baanverwezenlijking deelt sommige gemeenschappelijke parameters voor het vormen van het dossierformaat, gebiedsnamen, en filter van een bulkextractietaak.  Elk subtype van extractietaak kan extra parameters hebben:

| Parameter | Gegevenstype | Notities |
|---|---|---|
| format | String | Hiermee bepaalt u de bestandsindeling van de geïmporteerde gegevens met opties voor door komma&#39;s gescheiden waarden, door tabs gescheiden waarden en door puntkomma&#39;s gescheiden waarden. Accepteert één van: CSV, SSV, TSV. De notatie wordt standaard ingesteld op CSV. |
| file | String | Gegevens worden via meerdelige formuliergegevens in het bestand opgegeven. |

## Status opiniepeilingtaak

Het bepalen van het statuut van de baan is eenvoudig gebruikend [&#x200B; krijgt het Punt van de Status van het Lood van de Invoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadStatusUsingGET).

```
GET /bulk/v1/leads/batch/{batchId}.json
```

```json
{
    "requestId": "1f63#15d6738fd15",
    "result": [
        {
            "batchId": 3404,
            "importId": "3404",
            "status": "Complete",
            "numOfLeadsProcessed": 3,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 3 records imported (3 members)"
        }
    ],
    "success": true
}
```

Het binnenste `status` lid geeft de voortgang van de taak aan en kan een van de volgende waarden hebben: In wachtrij plaatsen, Importeren, Voltooien, Mislukt. In dit geval is onze taak voltooid, dus kunnen we ophouden met stemmen.

## Mislukt

Fouten worden aangegeven door het kenmerk `numOfRowsFailed` in het antwoord &#39;Status van lead importeren&#39;. Als `numOfRowsFailed` groter is dan nul, geeft die waarde het aantal mislukkingen aan dat is opgetreden.

Om de verslagen en de oorzaken van ontbroken rijen terug te winnen, zult u het mislukkingsdossier moeten terugwinnen gebruikend [&#x200B; krijg de Lood van de Invoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/getImportLeadFailuresUsingGET) eindpunt.

```
GET /bulk/v1/leads/batch/{batchId}/failures.json
```

Het bestand geeft aan welke rijen zijn mislukt en geeft aan waarom de record is mislukt.
