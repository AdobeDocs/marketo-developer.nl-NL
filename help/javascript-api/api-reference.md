---
title: Munchkin API-naslag
description: Gebruik de Munchkin Javascript API om paginabezoeken, verbindingskliks, en douanegebeurtenissen met init te volgen, createTrackingCookie, en munchkinFunction methodes.
feature: Munchkin Tracking Code, Javascript
exl-id: e9727691-5501-4223-bc98-2b4bacc33513
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 2%

---

# Munchkin API-naslag

Munchkin biedt verschillende functies die u handmatig via JavaScript kunt aanroepen. Hiermee kunt u browsergebeurtenissen op een aangepaste manier bijhouden, zoals het afspelen van video of het klikken op niet-koppelingen.

## Functies

De Munchkin API bestaat uit de volgende functies: `init`, `createTrackingCookie`, `munchkinFunction` .

<a name="munchkin_init"></a>

### Munchkin.init()

`Munchkin.init()` moet worden aangeroepen voordat andere functies worden uitgevoerd. Het plaatst omhoog Munchkin op de huidige pagina om activiteiten naar een specifieke instantie te verzenden en produceert de activiteit van de &quot;Web-pagina van Bezoekingen&quot;voor de huidige pagina.

| Parameternaam | Optioneel/vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| Munchkin-id | Vereist | String | Munchkin-account-id gevonden onder Beheer > Integratie > Munchkin-menu. Hiermee stelt u de doelinstantie in waarnaar activiteiten moeten worden verzonden. |
| [ Montages van de Configuratie ](configuration.md) | Optioneel | Object | Hiermee schakelt u alternatieve gedragsinstellingen voor Munchkin in. |

```javascript
Munchkin.init('299-BYM-827');
```

### Munchkin.createTrackingCookie()

Wanneer deze wordt aangeroepen, wordt gecontroleerd of er een `_mkto_trk` -cookie in de browser aanwezig is. Als dit niet het geval is, wordt er een cookie gemaakt. Dit is handig als u gebruikers wilt bijhouden tijdens specifieke handelingen, zoals het registreren of downloaden van elementen, als `cookieAnon` is ingesteld op false.

| Parameternaam | Optioneel/vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| forceCreate | Vereist | Boolean | Maak een cookie, zelfs als `cookieAnon` is ingesteld op false. |

```javascript
Munchkin.createTrackingCookie(true);
```

### Munchkin.munchkinFunction()

Wordt gebruikt voor het genereren van aangepaste gedragingen, zoals het afspelen en pauzeren van videospelers, of voor het verzenden van pagina&#39;s voor niet-standaardnavigatie, zoals hashcodes.

| Parameternaam | Optioneel/vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| Type functie | Vereist | String | Bepaalt de activiteit die moet worden opgenomen. Toegestane waarden: `visitWebPage`, `clickLink`, `associateLead` |
| Data | Vereist | Object | Bevat gegevens voor de activiteit die moet worden geregistreerd. |

#### visitWebPage

Door `munchkinFunction()` aan te roepen met `visitWebPage` wordt een &#39;bezoek&#39;-activiteit voor de huidige gebruiker naar Marketo verzonden. U kunt de URL en `querystring` aanpassen die met het gegevensobject in het tweede argument worden verzonden.

| Naam gegevenseigenschap | Optioneel/vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| url | Vereist | String | Het URL-bestandspad dat wordt gebruikt om een paginabezoek vast te leggen.  Deze waarde wordt toegevoegd aan de huidige domeinnaam om een volledige paginanaam te maken. Als URL bijvoorbeeld `/index.html` is en domeinnaam `www.example.com` , wordt de bezochte pagina opgenomen als `www.example.com/index.html` . |
| param | Optioneel | String | Een queryreeks met de gewenste parameters die moeten worden opgenomen. |

Bijvoorbeeld `foo=bar&biz=baz` .

```javascript
Munchkin.munchkinFunction('visitWebPage', {
        'url': '/Football/Team/Seahawks',
        'params': 'defense=legion_of_boom&qb=wilson'
    }
);
```

#### clickLink

Door `munchkinFunction()` aan te roepen met `clickLink` wordt een klikactiviteit voor de huidige gebruiker naar Marketo verzonden. U kunt de klik-URL aanpassen met de eigenschap `href` in het gegevensobject.

| Naam gegevenseigenschap | Optioneel/vereist | Type | Beschrijving |
| --- | --- | --- | --- |
| href | Vereist | String | Het URL-bestandspad dat wordt gebruikt om een koppelingsklik op te nemen. Deze waarde wordt aan de huidige domeinnaam toegevoegd om een volledige koppeling te maken. |

Als href bijvoorbeeld `/index.html` is en domeinnaam `www.example.com` is, wordt de koppelingsklik opgenomen als `www.example.com/index.html` .

```javascript
Munchkin.munchkinFunction('clickLink', {
        'href': '/Football/Team/Seahawks'
    }
);
```

#### associateLead

Deze methode is vervangen en is niet meer beschikbaar voor gebruik.
