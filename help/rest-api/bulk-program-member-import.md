---
title: Bulkprogramma-lid importeren
feature: REST API
description: Leer hoe u programmaleden bulksgewijs importeert via de Marketo REST API met CSV TSV- of SSV-bestanden onder 10 MB, wachtrijbeperkingen, vereiste params en de status van de opiniepeilingtaak.
exl-id: b0e1039a-fe9b-4fb7-9aa6-9980a06da673
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# Bulkprogramma-lid importeren

{de Verwijzing van het Eindpunt van het Punt van de Invoer van het Lid van het BulkProgramma [](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Voor grote hoeveelheden verslagen van het programmalid, kunnen de programmaleden asynchroon met [ bulk API ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members) worden ingevoerd. Op deze manier kunt u een lijst met records importeren naar Marketo met een plat bestand met de scheidingstekens (komma&#39;s, tabs of puntkomma&#39;s). Het bestand kan een willekeurig aantal records bevatten, mits het bestand in totaal minder dan 10 MB groot is. De recordbewerking is alleen &quot;invoegen of bijwerken&quot;.

## Verwerkingslimieten

U mag meer dan één aanvraag voor bulkimport indienen, met beperkingen. Elke aanvraag wordt als een taak toegevoegd aan een FIFO-wachtrij die moet worden verwerkt. Er worden maximaal twee banen tegelijk verwerkt. De wachtrij kan maximaal tien taken tegelijk uitvoeren (inclusief de twee momenteel verwerkte taken). Als u het maximum van tien taken overschrijdt, wordt de fout &quot;1016, Te veel import&quot; geretourneerd.

## Bestand importeren

De eerste rij van het bestand moet een header zijn die de corresponderende REST API-namen vermeldt als velden waarin de waarden van elke rij moeten worden toegewezen. REST API de namen kunnen worden teruggewonnen gebruikend [ beschrijf Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) en/of [ beschrijft de eindpunten van het Lid van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET). Records kunnen loodvelden, aangepaste doorloopvelden en aangepaste lidvelden van een programma bevatten.

Een typisch bestand volgt dit standaardpatroon:

```
email,firstName,lastName
test@example.com,John,Doe
```

De aanroep zelf wordt gemaakt met het inhoudstype `multipart/form-data` .

Dit type verzoek kan moeilijk zijn uit te voeren, zodat wordt het hoogst geadviseerd dat u een bestaande bibliotheekimplementatie gebruikt.

## Een taak maken

Het [ eindpunt van de Leden van het Programma van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) leest een dossier dat de verslagen van het programmalid bevat en voegt hen aan een programma met een bepaalde status toe. De records kunnen zowel hoofdvelden als aangepaste velden voor programmaleden bevatten. Alle records moeten het e-mailveld bevatten, dat wordt gebruikt voor deduplicatiedoeleinden.

De padparameter `programId` geeft het programma op waaraan de leden worden toegevoegd.

Er zijn drie vereiste vraagparameters. De parameter `format` geeft de indeling van het importbestand (CSV, TSV of SSV) op, de parameter `programMemberStatus` geeft de status van het programma op voor de leden die aan het programma worden toegevoegd en de parameter `file` bevat de naam van het importbestand dat de records van de programmaleden bevat.

```
POST /bulk/v1/program/{programId}/members/import.json?format=csv&programMemberStatus=On List
```

```
Content-Type: multipart/form-data; boundary=--------------------------118046853683028616211319
Content-Length: 772
Host: <munchkinId>.mktorest.com
```

```
----------------------------118046853683028616211319
Content-Disposition: form-data; name="file"; filename="Lead-House-Lannister.csv"
Content-Type: text/csv

firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0

----------------------------118046853683028616211319--
```

```json
{
    "requestId": "17f4a#16f87f87325",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Queued"
        }
    ],
    "success": true
}
```

In het antwoord op onze oproep ziet u dat er een `batchId` - en een `status` -veld zijn voor de record in de resultaatarray. Aangezien dit eindpunt asynchroon is, kan het een status van In de rij geplaatst terugkeren, Importeren, of Ontbroken. U moet de `batchId` behouden om de status van de importtaak op te halen en om fouten en/of waarschuwingen op te halen als de taak is voltooid. De `batchId` blijft zeven dagen geldig.

Gebruikend het voorbeeld hierboven, is een eenvoudige manier om het eindpunt te roepen cURL van de bevellijn te gebruiken:

```bash
curl -i -F format='csv' -F programMemberStatus='On List' -F file='@Lead-House-Lannister.csv' -F access_token='<Access Token>' <REST API Endpoint Base URL>/bulk/v1/program/{programId}/members/import.json
```

Indien het invoerbestand &quot;Lead-House-Lannister.csv&quot; het volgende bevat:

```
firstName,lastName,email,title,company,leadScore
Joanna,Lannister,Joanna@Lannister.com,Lannister,House Lannister,0
Tywin,Lannister,Tywin@Lannister.com,Lannister,House Lannister,0
Cersei,Lannister,Cersei@Lannister.com,Lannister,House Lannister,0
Jamie,Lannister,Jamie@Lannister.com,Lannister,House Lannister,0
Tyrion,Lannister,Tyrion@Lannister.com,Lannister,House Lannister,0
Kevan,Lannister,Kevan@Lannister.com,Lannister,House Lannister,0
Dorna,Lannister,Dorna@Lannister.com,Lannister,House Lannister,0
Lancel,Lannister,Lancel@Lannister.com,Lannister,House Lannister,0
```

## Status opiniepeilingtaak

Nadat de importtaak is gemaakt, moet u een query uitvoeren op de status ervan. Het is aan te raden de importtaak elke 5-30 seconden te peilen. Doe dit door de `batchId` wegparameter tot het [ overgaan krijgt de Status van het Lid van het Programma van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) eindpunt.

```
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "e0cb#16f87f8b177",
    "result": [
        {
            "batchId": 1040,
            "importId": "1040",
            "status": "Complete",
            "numOfLeadsProcessed": 8,
            "numOfRowsFailed": 0,
            "numOfRowsWithWarning": 0,
            "message": "Import succeeded, 8 records imported (8 members)"
        }
    ],
    "success": true
}
```

In dit antwoord wordt een voltooide importbewerking weergegeven. De status kan een van de volgende zijn: Voltooid, In wachtrij geplaatst, Importeren, Mislukt.

Als de taak is voltooid, ziet u een overzicht van het aantal rijen dat is verwerkt, is mislukt of is er een waarschuwing. De berichtparameter kan het mislukkingsbericht ook geven als de status Ontbroken is.

## Mislukt

De mislukkingen worden vermeld door het `numOfRowsFailed` attribuut in [ krijgen de Status van het Lid van het Programma van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) reactie. Als numOfRowsFailed groter is dan nul, dan wijst die waarde op het aantal mislukkingen die voorkwamen.

Gebruik het Get Lid van het Programma van de Invoer eindpunt van Mislukt om verslagen en oorzaken van ontbroken rijen terug te winnen door de `batchId` wegparameter over te gaan.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Het eindpunt antwoordt met een dossier erop wijst die welke rijen, samen met een bericht erop wijzen die waarom het verslag ontbrak. De bestandsindeling is dezelfde als die is opgegeven in de parameter `format` tijdens het maken van taken. Aan elke record wordt een extra veld toegevoegd met een beschrijving van de fout.

Stel dat u het volgende bestand met een ongeldige loodscore importeert:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Wanneer u de taakstatus controleert, ziet u dat `numOfRowsFailed` 1 is, wat aangeeft dat er een fout is opgetreden:

```
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
    "requestId": "4c2d#16f8b32c8ef",
    "result": [
        {
            "batchId": 1046,
            "importId": "1046",
            "status": "Complete",
            "numOfLeadsProcessed": 0,
            "numOfRowsFailed": 1,
            "numOfRowsWithWarning": 0,
            "message": "Import completed with errors, 0 records imported (0 members), 1 failed"
        }
    ],
    "success": true
}
```

Vervolgens haalt u het foutbestand op voor meer informatie over de fout:

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

```
firstName,lastName,email,title,company,leadScore,Import Failure Reason
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD,Invalid data type in field Lead Score
```

## Waarschuwingen

De waarschuwingen worden vermeld door het `numOfRowsWithWarning` attribuut in [ krijgen de Status van het Lid van het Programma van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) reactie. Als `numOfRowsWithWarning` groter is dan nul, geeft die waarde het aantal waarschuwingen aan dat is opgetreden.

Gebruik [ krijgen de Waarschuwingen van het Lid van het Programma van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) eindpunt om verslagen en oorzaken van waarschuwingsrijen terug te winnen door de `batchId` wegparameter over te gaan.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Het eindpunt antwoordt met een dossier erop wijst die welke rijen tot waarschuwingen, samen met een bericht erop wijzen die waarom het verslag een waarschuwing produceerde. De bestandsindeling is dezelfde als die is opgegeven in de parameter `format` tijdens het maken van taken. Aan elke record wordt een extra veld toegevoegd met een beschrijving van de waarschuwing.

Stel dat u het volgende bestand met een ongeldig e-mailadres importeert:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Wanneer u de taakstatus controleert, ziet u dat `numOfRowsWithWarning` 1 is, wat aangeeft dat er een waarschuwing is opgetreden:

```
GET /bulk/v1/program/members/import/{batchId}/status.json
```

```json
{
   "requestId":"4ca1#16f883c2003",
   "result":[
      {
         "batchId":1041,
         "importId":"1041",
         "status":"Complete",
         "numOfLeadsProcessed":1,
         "numOfRowsFailed":0,
         "numOfRowsWithWarning":1,
         "message":"Import succeeded, 1 records imported (1 members), 1 warning."
      }
   ],
   "success":true
}
```

Vervolgens haalt u het waarschuwingsbestand op voor meer informatie over de waarschuwing:

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

```
firstName,lastName,email,title,company,leadScore,Import Warning Reason
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0,Invalid email address
```
