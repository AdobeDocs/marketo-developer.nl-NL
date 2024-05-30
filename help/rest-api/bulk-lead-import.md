---
title: "Bulklood Import"
feature: REST API
description: "Batch importeren van gegevens voor lood."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 0%

---


# Invoer van bulklood

[Naslaggids voor invoer van bulklood](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads)

Voor grote hoeveelheden loodrecords kan de invoer van leads asynchroon verlopen met de opdracht [bulk-API](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST). Op deze manier kunt u een lijst met records importeren naar Marketo met een plat bestand met de scheidingstekens (komma&#39;s, tabs of puntkomma&#39;s). Het bestand kan een willekeurig aantal records bevatten, mits het bestand in totaal minder dan 10 MB groot is. De recordbewerking is alleen &quot;invoegen of bijwerken&quot;.

## Verwerkingslimieten

U mag meer dan één aanvraag voor bulkimport indienen, met beperkingen. Elke aanvraag wordt als een taak toegevoegd aan een FIFO-wachtrij die moet worden verwerkt. Er worden maximaal twee banen tegelijk verwerkt. De wachtrij kan maximaal tien taken tegelijk uitvoeren (inclusief de twee momenteel verwerkte taken). Als u het maximum van tien taken overschrijdt, wordt de fout &quot;1016, Te veel import&quot; geretourneerd.

## Bestand importeren

De eerste rij van het bestand moet een koptekst zijn met de corresponderende REST API-velden waarin de waarden van elke rij moeten worden toegewezen. Een typisch bestand volgt dit standaardpatroon:

```
email,firstName,lastName
test@example.com,John,Doe
```

De `externalCompanyId` kan worden gebruikt om de loodrecord te koppelen aan een bedrijfsrecord. De `externalSalesPersonId` kan worden gebruikt om de loodrecord te koppelen aan een record van een verkooppersoon.

De vraag zelf wordt gemaakt gebruikend `multipart/form-data` inhoudstype.

Dit type verzoek kan moeilijk zijn uit te voeren, zodat wordt het hoogst geadviseerd dat u een bestaande bibliotheekimplementatie gebruikt.

## Een taak maken

Als u een aanvraag voor bulkimport wilt uitvoeren, moet u de koptekst van het inhoudstype instellen op &quot;multipart/form-data&quot; en ten minste een bestandsparameter opnemen met de bestandsinhoud en een indelingsparameter met de waarde &quot;csv&quot;, &quot;tsv&quot; of &quot;ssv&quot; voor de bestandsindeling.

```
POST /bulk/v1/leads.json?format=csv
```

```
Content-Type: multipart/form-data; boundary=------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Length: 311
Host: <munchkinId>.mktorest.com
```

```
------WebKitFormBoundaryBQACkJZyaiIAXogC
Content-Disposition: form-data; name="file"; filename="leads.csv"
Content-Type: text/csv

FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
------WebKitFormBoundaryBQACkJZyaiIAXogC--
```

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

Dit eindpunt gebruikt [multipart/form-data as the content-type](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Dit kan lastig zijn om de juiste taal te kiezen, dus de beste praktijk is om een HTTP-ondersteuningsbibliotheek te gebruiken voor uw taal van keuze. Een eenvoudige manier om dit met cURL van de bevellijn te doen kijkt als dit:

```
curl -i -F format=csv -F file=@lead_data.csv -F access_token=<Access Token> <REST API Endpoint Base URL>/bulk/v1/leads.json
```

Waar het importbestand &quot;lead_data.csv&quot; het volgende bevat:

```
FirstName,LastName,Email,Company
Able,Baker,ablebaker@marketo.com,Marketo
Charlie,Dog,charliedog@marketo.com,Marketo
Easy,Fox,easyfox@marketo.com,Marketo
```

U kunt ook de optie `lookupField`, `listId`, en `partitionName` parameters in uw verzoek. `lookupField` Hiermee kunt u een specifiek veld selecteren waarop u wilt dupliceren, net als Leads synchroniseren. Standaard wordt een e-mail verzonden. U kunt `id` als `lookupField` om een bewerking alleen bijwerken aan te geven. `listId` Hiermee kunt u een statische lijst selecteren om de lijst met leads te importeren. Hierdoor worden de leads in de lijst lid van deze statische lijst, plus eventuele creaties of updates die door het importeren worden veroorzaakt. `partitionName` Hiermee selecteert u een specifieke partitie waarnaar u wilt importeren. Zie de sectie Werkruimten en Partities voor meer informatie.

Bericht in het antwoord op onze vraag, dat er geen lijst van successen of mislukkingen zoals met de Leads van de Synchronisatie, maar een batchId en een statusgebied voor het verslag in de resultaatserie is. De reden hiervoor is dat deze API asynchroon is en een status in de wachtrij kan retourneren, importeren of Mislukt. U moet batchId behouden om de status van de importtaak op te halen en om fouten en/of waarschuwingen op te halen als de taak is voltooid. De batchId blijft zeven dagen geldig.

## Status opiniepeilingtaak

Het wordt aanbevolen om de taak elke 5-30 seconden te opiniepeilen, afhankelijk van de vereiste latentie en beperkingen van de API-aanroep, om de status van de importtaak te zien. U kunt dit doen met de Get Import Lead Status API.

```
GET /bulk/v1/leads/batch/{id}.json
```

```json
{
   "requestId":"8136#146daebc2ed",
   "success":true,
   "result":[
      {
         "batchId":1022,
         "status":"Complete",
         "numOfLeadsProcessed":2,
         "numOfRowsFailed":1,
         "numOfRowsWithWarning":0,
         "message":"Import completed with errors, 2 records imported (2 members), 1 failed"
      }
   ]
}
```

In dit antwoord wordt een voltooide importbewerking getoond, maar de status kan een van de volgende zijn:

- Voltooid
- In wachtrij
- Importeren
- Mislukt

Als de taak is voltooid, ziet u een overzicht van het aantal verwerkte rijen, dat is mislukt, op rijen met waarschuwingen. De berichtparameter kan het mislukkingsbericht ook geven als de status Ontbroken is.

## Mislukt

De mislukkingen worden vermeld door het &quot;numOfRowsFailed&quot;attribuut in krijgen de Reactie van de Status van de Lood van de Invoer. Als &quot;numOfRowsFailed&quot; groter is dan nul, dan wijst die waarde op het aantal mislukkingen die voorkwamen.

Als u de records en oorzaken van mislukte rijen wilt ophalen, moet u het foutbestand ophalen:

```
GET /bulk/v1/leads/batch/{id}/failures.json
```

De API reageert met een bestand dat aangeeft welke rijen zijn mislukt, samen met een bericht dat aangeeft waarom de record is mislukt. De indeling van het bestand is gelijk aan de indeling die tijdens het maken van de taak is opgegeven in de parameter &quot;format&quot;. Aan elke record wordt een extra veld toegevoegd met een beschrijving van de fout.

## Waarschuwingen

Waarschuwingen worden aangegeven door het kenmerk &quot;numOfRowsWithWarning&quot; in het antwoord &#39;Status van lead importeren&#39;. Als &quot;numOfRowsWithWarning&quot; groter is dan nul, geeft die waarde het aantal waarschuwingen aan dat is opgetreden.

Als u de records en oorzaken van waarschuwingsrijen wilt ophalen, haalt u het waarschuwingsbestand op:

```
GET /bulk/v1/leads/batch/{id}/warnings.json
```

De API reageert met een bestand dat aangeeft welke rijen waarschuwingen hebben opgeleverd, samen met een bericht dat aangeeft waarom de record is mislukt. De indeling van het bestand is gelijk aan de indeling die tijdens het maken van de taak is opgegeven in de parameter &quot;format&quot;. Aan elke record wordt een extra veld toegevoegd met een beschrijving van de waarschuwing.
