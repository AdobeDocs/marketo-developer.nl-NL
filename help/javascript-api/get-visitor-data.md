---
title: Bezoekergegevens ophalen
description: Krijg bezoekersidentificatie in real time gebruikend de Context API van de Gebruiker RTP met params, callback voorbeeld, en steekproefreacties voor segmenten, ABM, en plaats.
feature: Javascript
exl-id: 39a2446d-8a31-461e-bbe6-a7edf24b4d52
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# Bezoekergegevens ophalen

Deze methode wordt gebruikt om de identificatiegegevens van de bezoeker in real time te krijgen.

- U moet een klant van Personalization van het Web worden en de [&#x200B; markering hebben RTP die &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) op uw plaats wordt opgesteld voorafgaand aan het gebruiken van de Context API van de Gebruiker.
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
