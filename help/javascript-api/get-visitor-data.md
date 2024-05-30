---
title: "Bezoekergegevens ophalen"
description: "Bezoekergegevens ophalen"
feature: Javascript
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---


# Bezoekergegevens ophalen

Deze methode wordt gebruikt om de identificatiegegevens van de bezoeker in real time te krijgen.

- U moet een klant van de Personalisatie van het Web worden en hebben [RTP-tag geïmplementeerd](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw site voordat u de Context-API van de gebruiker gebruikt.
- RTP ondersteunt geen accountgebaseerde marketing met benoemde accountlijsten. ABM-lijsten en -code hebben alleen betrekking op de geüploade accountlijsten (CSV-bestanden) die in RTP worden beheerd.

Als er een fout optreedt, wordt er een foutbericht weergegeven als onderdeel van het antwoord JSON. Als een code van 500 is teruggekeerd, contacteer steun met het verzoek u maakte.

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| `get` | Vereist | String | Methode, actie. |
| `visitor` | Vereist | String | Naam van methode. |
| `callback` | Vereist | Functie | Callback functie die voor elke teruggekeerde campagne moet worden teweeggebracht. |

## Voorbeelden

Identiteitsgegevens bezoeker ophalen:

```javascript
function callbackFunction() {
    console.log('RTP is awesome!');
}
rtp('get', 'visitor', callbackFunction);
```

Reactie met segmentovereenkomst:

Hieronder ziet u een voorbeeldreactie die wordt geretourneerd wanneer de bezoeker realtime segmenten bijkwam voordat de API-aanroep voor bezoekergegevens werd opgehaald.

```json
{
    "status": 200,
    "results": {
        "matchedSegments": [
            {
                "name": "first click",
                "id": 177
            }
        ],
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```

Reactie zonder segmentovereenkomst:

Hieronder ziet u een voorbeeldreactie die wordt geretourneerd voor het geval de bezoeker geen realtime-segmenten aanpast voordat de API-aanroep voor bezoekersgegevens wordt uitgevoerd.

```json
{
    "status": 200,
    "results": {
        "abm": [
            {
                "code": 4,
                "name": "abm_saleforce_customers"
            },
            {
                "code": 5,
                "name": "abm_top_customers"
            }
        ],
        "org": "Marketo",
        "location": {
            "country": "United States",
            "city": "San Mateo",
            "state": "CA"
        },
        "industries": [
            "Software & Internet"
        ],
        "isp": false
    }
}
```
