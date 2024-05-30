---
title: Bulkprogramma-lid importeren
feature: REST API
description: "Batch importeren van lidgegevens."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---


# Bulkprogramma-lid importeren

[Referentie voor eindpunt van import van bulkprogramma-lid](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members)

Voor grote hoeveelheden records van programmaleden kunnen programmaleden asynchroon worden geïmporteerd met de [bulk-API](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members). Op deze manier kunt u een lijst met records importeren naar Marketo met een plat bestand met de scheidingstekens (komma&#39;s, tabs of puntkomma&#39;s). Het bestand kan een willekeurig aantal records bevatten, mits het bestand in totaal minder dan 10 MB groot is. De recordbewerking is alleen &quot;invoegen of bijwerken&quot;.

## Verwerkingslimieten

U mag meer dan één aanvraag voor bulkimport indienen, met beperkingen. Elke aanvraag wordt als een taak toegevoegd aan een FIFO-wachtrij die moet worden verwerkt. Er worden maximaal twee banen tegelijk verwerkt. De wachtrij kan maximaal tien taken tegelijk uitvoeren (inclusief de twee momenteel verwerkte taken). Als u het maximum van tien taken overschrijdt, wordt de fout &quot;1016, Te veel import&quot; geretourneerd.

## Bestand importeren

De eerste rij van het bestand moet een header zijn die de corresponderende REST API-namen vermeldt als velden waarin de waarden van elke rij moeten worden toegewezen. REST API-namen kunnen worden opgehaald met [Lead beschrijven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) en/of [Lid van programma beschrijven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeProgramMemberUsingGET) eindpunten. Records kunnen loodvelden, aangepaste doorloopvelden en aangepaste lidvelden van een programma bevatten.

Een typisch bestand volgt dit standaardpatroon:

```
email,firstName,lastName
test@example.com,John,Doe
```

De vraag zelf wordt gemaakt gebruikend `multipart/form-data` inhoudstype.

Dit type verzoek kan moeilijk zijn uit te voeren, zodat wordt het hoogst geadviseerd dat u een bestaande bibliotheekimplementatie gebruikt.

## Een taak maken

De [Programmsleden importeren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/importProgramMemberUsingPOST) het eindpunt leest een dossier dat de verslagen van het programmalid bevat en voegt hen aan een programma met een bepaalde status toe. De records kunnen zowel hoofdvelden als aangepaste velden voor programmaleden bevatten. Alle records moeten het e-mailveld bevatten, dat wordt gebruikt voor deduplicatiedoeleinden.

De `programId` path parameter specifies the program to which the members are added.

Er zijn drie vereiste vraagparameters. De `format` parameter geeft de bestandsindeling voor import aan (CSV, TSV of SSV), de `programMemberStatus` geeft de programmastatus aan van de leden die aan het programma worden toegevoegd, en `file` parameter bevat de naam van het importbestand dat de records van het programmalid bevat.

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

Bericht in antwoord op onze oproep dat er een `batchId` en `status` veld voor de record in de resultaatarray. Aangezien dit eindpunt asynchroon is, kan het een status van In de rij geplaatst terugkeren, Importeren, of Ontbroken. U moet de `batchId` om de status van de importtaak op te halen en fouten en/of waarschuwingen op te halen wanneer deze zijn voltooid. De `batchId` blijft zeven dagen geldig.

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

Nadat de importtaak is gemaakt, moet u een query uitvoeren op de status ervan. Het is aan te raden de importtaak elke 5-30 seconden te peilen. Doe dit door de `batchId` padparameter naar de [Lidstatus van importprogramma ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) eindpunt.

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

Ontbrekende gegevens worden aangegeven door de `numOfRowsFailed` kenmerk in [Lidstatus van importprogramma ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) reactie. Als numOfRowsFailed groter is dan nul, dan wijst die waarde op het aantal mislukkingen die voorkwamen.

Gebruik de [Fout door lid van importprogramma ophalen](http://TODO) eindpunt om verslagen en oorzaken van ontbroken rijen terug te winnen door het overgaan van `batchId` padparameter.

```
GET /bulk/v1/program/members/import/{batchId}/failures.json
```

Het eindpunt antwoordt met een dossier erop wijst die welke rijen, samen met een bericht erop wijzen die waarom het verslag ontbrak. De bestandsindeling is dezelfde als die in `format` parameter tijdens het maken van taken. Aan elke record wordt een extra veld toegevoegd met een beschrijving van de fout.

Stel dat u het volgende bestand met een ongeldige loodscore importeert:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,Aerys@Targaryen.com,Targaryen,House Targaryen,TEXT_VALUE_IN_INTEGER_FIELD
```

Wanneer u de taakstatus controleert, ziet u `numOfRowsFailed` is 1 wat erop wijst dat een mislukking voorkwam:

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

Waarschuwingen worden aangegeven door de `numOfRowsWithWarning` kenmerk in [Lidstatus van importprogramma ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberStatusUsingGET) reactie. Indien `numOfRowsWithWarning` is groter dan nul, dan wijst die waarde op het aantal waarschuwingen die voorkwamen.

Gebruik de [Waarschuwingen voor leden van importprogramma ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Program-Members/operation/getImportProgramMemberWarningsUsingGET) eindpunt om verslagen en oorzaken van waarschuwingsrijen terug te winnen door over te gaan `batchId` padparameter.

```
GET /bulk/v1/program/members/import/{batchId}/warnings.json
```

Het eindpunt antwoordt met een dossier erop wijst die welke rijen tot waarschuwingen, samen met een bericht erop wijzen die waarom het verslag een waarschuwing produceerde. De bestandsindeling is dezelfde als die in `format` parameter tijdens het maken van taken. Aan elke record wordt een extra veld toegevoegd met een beschrijving van de waarschuwing.

Stel dat u het volgende bestand met een ongeldig e-mailadres importeert:

```
firstName,lastName,email,title,company,leadScore
Aerys,Targaryen,INVALID_EMAIL,Targaryen,House Targaryen,0
```

Wanneer u de taakstatus controleert, ziet u `numOfRowsWithWarning` is 1 wat erop wijst dat een waarschuwing voorkwam:

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
