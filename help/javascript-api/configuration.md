---
title: "Configuratie"
description: "Configuratie-API"
feature: Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 0%

---


# Configuratie

Munchkin kan verschillende configuratie-instellingen accepteren om het gedrag aan te passen. Configuratie-instellingen zijn eigenschappen van een JavaScript-object dat als tweede parameter wordt doorgegeven bij het aanroepen van [Munchkin.init()](lead-tracking.md#munchkin-behavior)

```json
Munchkin.init("AAA-BBB-CCC", {
        "configName":"configValue",
        "configName2":"configValue2"
    }
);
```

Het configuratieinstellingenobject kan een willekeurig aantal eigenschappen uit de onderstaande tabel bevatten.

## Eigenschappen

| Naam | Gegevenstype | Beschrijving |
|---|---|---|
| altIds | Array | Accepteert een array van Munchkin-id-tekenreeksen. Wanneer toegelaten, dupliceert dit alle Activiteit van het Web aan de gerichte abonnementen, die op hun Munchkin identiteitskaart worden gebaseerd. |
| anonymizeIP | Boolean | Anonymizes het IP adres dat in Marketo voor nieuwe bezoekers wordt geregistreerd.U kunt bepalen als uw abonnement aan Munchkin V2 wordt geleverd door te controleren of uw `{Munchkin-Id}.mktoresp.com` domein heeft één van de volgende adressen: `192.28.144.124` `134.213.193.62` `192.28.147.68` `103.237.104.82`. U kunt het onderstaande script ook uitvoeren vanuit een unix shell: nslookup {munchkin-id}.mktoresp.com | grep -E -c -e &quot;(192.28.144.124,134.213.193.62,192.28.147.68,103.237.104.82)&quot; Als output &quot;0&quot;dan is uw abonnement niet provisioned met Munchkin V2; als het 1 of groter output, dan is het provisioned. |
| apiOnly | Boolean | Indien ingesteld op true, dan `Munchkin.Init()` function will not call `visitsWebPage`. Dit is handig voor webtoepassingen van één pagina die volledige controle over elke pagina nodig hebben `visitsWebPage` gebeurtenis. |
| asyncOnly | Boolean | Indien ingesteld op true, wordt de XMLHttpRequest asynchroon verzonden. De standaardwaarde is false. |
| clickTime | Geheel | Hiermee stelt u de hoeveelheid tijd in die na een klik moet worden geblokkeerd om te klikken op een trackingverzoek (in milliseconden). Als u deze waarde vermindert, wordt de klikcontrole minder nauwkeurig. De standaardwaarde is 350 ms. |
| cookieAnon | Boolean | Als deze optie op false is ingesteld, worden nieuwe anonieme leads niet bijgehouden en in een cookie gemaakt. Leads hebben cookies en worden bijgehouden nadat ze een Marketo-formulier hebben ingevuld of door via een Marketo-e-mail door te klikken. De standaardwaarde is true. |
| cookieLifeDays | Geheel | Hiermee stelt u de vervaldatum van nieuwe Munchkin-trackingcookies in op dit aantal dagen in de toekomst. De standaardwaarde is 730 dagen (2 jaar). |
| customName | String | Aangepaste paginanaam. Alleen voor systeemgebruik. |
| domainLevel | Geheel | Stelt het aantal onderdelen van het domein van de pagina in dat moet worden gebruikt wanneer het domeinkenmerk van de cookie wordt ingesteld. Stel bijvoorbeeld dat het huidige paginadomein &quot;www.example.com&quot;.domainLevel: 2 het kenmerk van het cookie-domein instelt op &quot;.example.com&quot;domainLevel: 3 stelt het kenmerk van het cookie-domein in op &quot;.www.example.com&quot;Background:Munchkin beheert automatisch bepaalde domeinen van het hoogste niveau met twee letters. Dit is standaard ingesteld op twee delen in normale gevallen waarin het domein op het hoogste niveau uit drie letters bestaat. Bijvoorbeeld &quot;www.example.com&quot;, worden de twee meest rechtse delen gebruikt om het koekje, &quot;.example.com&quot; te plaatsen.Voor twee brievenlandcodes zoals &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot;, en &quot;.uk&quot;, blijft de code aan drie delen in gebreke. Zo gebruikt &quot;www.example.co.jp&quot; bijvoorbeeld drie meest rechtse domeinonderdelen, &quot;.example.co.jp&quot;. Als het domeinpatroon een ander gedrag vereist, moet u dit opgeven met de opdracht `domainLevel` parameter. |
| domainSelectorV2 | Boolean | Indien ingesteld op true, wordt een verbeterde methode gebruikt om te bepalen hoe het kenmerk cookie domain moet worden ingesteld. |
| httpsOnly | Boolean | De standaardwaarde is false. Wanneer ingesteld op true, wordt voor cookies de instelling Secure gebruikt wanneer de bijgehouden pagina via https wordt verzonden. |
| useBeaconAPI | Boolean | De standaardwaarde is false. Wanneer ingesteld op true, wordt de API van het baken gebruikt om niet-blokkerende aanvragen te verzenden in plaats van XMLHttpRequest. Als de browser deze API niet ondersteunt, valt de Munchkin terug op het gebruik van XMLHttpRequest. |
| wsInfo | String | Neemt een tekenreeks om een werkruimte als doel in te stellen. Deze werkruimte-id wordt verkregen door de werkruimte te selecteren in het menu Beheer > Integratie > Munchkin. Deze instelling geldt alleen voor het maken van een anonieme lead record. Zodra de waarde van het koekje Munchkin voor dat loodverslag is gevestigd, kan de parameter wsInfo niet worden gebruikt om zijn verdeling te veranderen. Aangezien dit plaatsen slechts anonieme lood beïnvloedt, is het slechts relevant voor verdeling-specifieke Anoniem Bezoekers in de Rapporten van het Web. |

## Voorbeelden

### Activiteit verzenden naar meerdere abonnementen

In dit voorbeeld worden alle webactiviteiten naar de instanties verzonden met Munchkin-id&#39;s &quot;AAA-BBB-CCC&quot; en &quot;XXX-YYY-ZZZ&quot;.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCCC', { 'altIds': ['XXX-YYY-ZZZ'] });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

### Tekstspatiëring instellen op asynchroon

In dit voorbeeld worden alle XMLHttpRequest&#39;s asynchroon vanuit de hoofdthread verzonden.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      // Add configuration settings to the init method
      Munchkin.init('AAA-BBB-CCC', { 'asyncOnly': true });
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin-beta.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```
