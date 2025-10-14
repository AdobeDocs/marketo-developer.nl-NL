---
title: Responstoewijzingen
feature: Webhooks
description: Marketo Webhooks-responstoewijzingen voor JSON en XML, kenmerken toewijzen aan loodvelden met SOAP API-namen, punt- en arraynotatie en typecompatibiliteit.
exl-id: 95c6e33e-487c-464b-b920-3c67e248d84e
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Responstoewijzingen

Marketo kan gegevens die door een Webhaak worden ontvangen, omzetten van twee inhoudssoorten en deze waarden terugkeren naar een hoofdveld: JSON en XML. De parameter van het Gebied van Marketo zal altijd de [&#x200B; SOAP API naam &#x200B;](../rest-api/fields.md) van het gebied gebruiken. Elke Webhaak kan een onbeperkt aantal reactietoewijzingen hebben, die worden toegevoegd en door de [!UICONTROL Edit] knoop in de ruit van de Toewijzingen van de Reactie van uw Webhaak te klikken worden uitgegeven:

![&#x200B; reactie-Toewijzing &#x200B;](assets/response-mapping.png)

De Toewijzingen van de reactie worden gecreeerd via een verbinding van een &quot;Attribuut van de Reactie&quot;, de weg aan het gewenste bezit in het document van XML of JSON, en het &quot;Gebied van Marketo&quot;, dat het Loodgebied specificeert dat de waarde heeft die aan het van het Attribuut van de Reactie wordt geschreven.

Toetsen voor eigenschappen moeten bestaan uit alfanumerieke tekens, streepje (-), onderstrepingsteken (_), dubbele punt (:) en witruimte die toegankelijk zijn via Marketo-responstoewijzingen.

## JSON Mappings

JSON-eigenschappen zijn toegankelijk met puntnotatie en arraynotatie. Arraynotatie in Marketo accepteert geen tekenreeksen als invoer en alleen gehele getallen. Als u gegevens wilt ophalen uit een JSON-document, moet het reactietype worden ingesteld op JSON:

```json
{ "foo":"bar"}
```

Als u toegang wilt krijgen tot de eigenschap `foo` in een reactietoewijzing, gebruikt u de eigenschap `name` van de eigenschap omdat deze zich op het eerste niveau van het JSON-object bevindt, `foo` . Zo ziet dat eruit in Marketo:

![&#x200B; Toewijzing van de Reactie &#x200B;](assets/json-resp.png)

Hier is een gecompliceerder voorbeeld met een array:

```json
{
    "profileId" : 1234,
    "firstName" : "Jane",
    "lastName" : "Doe",
    "orders" : [
        {
            "orderId" : 5678,
            "orderDate" : "2015-01-01",
            "orderProductId" : "4982"
        },
        {
            "orderId" : 5678,
            "orderDate" : "2014-05-07",
            "orderProductId" : "4982"
        }
    ]
}
```

We willen toegang krijgen tot orderDate vanaf het eerste element van de array orders. Gebruik de volgende opties om toegang te krijgen tot deze eigenschap: `orders[0].orderDate`

## XML-toewijzingen

Waarden zijn toegankelijk vanuit afzonderlijke elementen in XML-documenten. Hierbij wordt puntnotatie gebruikt die lijkt op de JSON-toewijzingen. Laten we naar dit eenvoudige voorbeeld kijken:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<example>
    <foo>bar</foo>
</example>
```

Gebruik de volgende opties om de eigenschap foo hier te openen: `example.foo`

Er moet eerst naar het voorbeeldelement worden verwezen voordat u `foo` opent. Als u toegang wilt krijgen tot een eigenschap, moet in de toewijzing naar alle elementen in de hiërarchie worden verwezen. XML-documenten met arrays zijn iets gecompliceerder. Gebruik het volgende voorbeeld:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<elementList>
    <element>
        <foo>baz</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
    <element>
        <foo>bar</foo>
    </element>
</elementList>
```

Het document bestaat uit de bovenliggende array `elementList` , met onderliggende elementen, element dat één eigenschap bevat: `foo` . Voor Marketo-responstoewijzingen wordt naar de array verwezen als `elementList.element` , zodat de onderliggende elementen van de elementList via `elementList.element[i]` kunnen worden benaderd. Om de waarde van foo van het eerste kind van elementList te krijgen, gebruiken wij dit reactieattribuut: `elementList.element[0].foo` dit keert de waarde &quot;baz&quot;aan ons aangewezen gebied terug. Wanneer u eigenschappen probeert te benaderen binnen elementen die zowel unieke als niet-unieke elementnamen bevatten, resulteert dit in ongedefinieerd gedrag. Elk element moet één eigenschap of array zijn, de typen kunnen niet worden gecombineerd.

## Typen

Wanneer u kenmerken toewijst aan velden, moet u ervoor zorgen dat het type in de reactie op de webhaak compatibel is met het doelveld. Als de waarde in het antwoord bijvoorbeeld een tekenreeks is en het geselecteerde veld een geheel getal is, wordt de waarde niet geschreven. Lees over [&#x200B; Types van Gebied &#x200B;](../rest-api/field-types.md).
