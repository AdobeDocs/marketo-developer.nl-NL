---
title: Configuratie
description: Configureer Marketo Munchkin met de JavaScript API. Leer Munchkin.init-instellingen zoals altIds, anonymizeIP, asyncOnly, cookie, domainLevel, Beacon-API.
feature: Munchkin Tracking Code, Javascript
exl-id: 4700ce7b-f624-4f27-871e-9a050f203973
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 0%

---

# Configuratie

Munchkin kan verschillende configuratie-instellingen accepteren om het gedrag aan te passen. De montages van de configuratie zijn eigenschappen van een voorwerp van JavaScript dat als tweede parameter wordt overgegaan wanneer het roepen van [&#x200B; Munchkin.init () &#x200B;](api-reference.md#munchkin_init)

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
| altIds | Array | Accepteert een array van Munchkin ID-tekenreeksen. Wanneer toegelaten, dupliceert dit alle Activiteit van het Web aan de gerichte abonnementen, die op hun identiteitskaart van Munchkin worden gebaseerd. |
| anonymizeIP | Boolean | Anonymizes het IP adres dat in Marketo voor nieuwe bezoekers wordt geregistreerd. |
| apiOnly | Boolean | Indien ingesteld op true, roept de functie `Munchkin.Init()` `visitsWebPage` niet aan. Dit is handig voor webtoepassingen van één pagina die volledige controle over elke `visitsWebPage` -gebeurtenis nodig hebben. |
| asyncOnly | Boolean | Indien ingesteld op true, wordt de XMLHttpRequest asynchroon verzonden. De standaardwaarde is false. |
| clickTime | Geheel | Hiermee stelt u de hoeveelheid tijd in die na een klik moet worden geblokkeerd om te klikken op een trackingverzoek (in milliseconden). Als u deze waarde vermindert, wordt de klikcontrole minder nauwkeurig. De standaardwaarde is 350 ms. |
| cookieAnon | Boolean | Als deze optie op false is ingesteld, worden nieuwe anonieme leads niet bijgehouden en in een cookie gemaakt. Leads hebben cookies en worden bijgehouden nadat ze een Marketo-formulier hebben ingevuld of door via een Marketo-e-mail door te klikken. De standaardwaarde is true. |
| cookieLifeDays | Geheel | Hiermee stelt u de vervaldatum van nieuwe Munchkin-volgcookies in op dit aantal dagen in de toekomst. De standaardwaarde is 730 dagen (2 jaar). |
| customName | String | Aangepaste paginanaam. Alleen voor systeemgebruik. |
| <a name="domainlevel"></a> domainLevel | Geheel | Stelt het aantal onderdelen van het domein van de pagina in dat moet worden gebruikt wanneer het domeinkenmerk van het cookie wordt ingesteld. Stel bijvoorbeeld dat het huidige paginadomein &quot;www.example.com&quot;.domainLevel: 2 het kenmerk van het cookie-domein instelt op &quot;.example.com&quot;domainLevel: 3 stelt het kenmerk van het cookie-domein in op &quot;.www.example.com&quot;Background :Munchkin beheert automatisch bepaalde domeinen van het hoogste niveau met twee letters. Dit is standaard ingesteld op twee delen in normale gevallen waarin het domein op het hoogste niveau uit drie letters bestaat. Bijvoorbeeld &quot;www.example.com&quot;, worden de twee meest rechtse delen gebruikt om het koekje, &quot;.example.com&quot; te plaatsen.Voor twee brievenlandcodes zoals &quot;.jp&quot;, &quot;.us&quot;, &quot;.cn&quot;, en &quot;.uk&quot;, blijft de code aan drie delen in gebreke. &quot;www.example.co.jp&quot; gebruikt bijvoorbeeld drie meest rechtse domeinonderdelen, &quot;.example.co.jp&quot;. Als het domeinpatroon een ander gedrag vereist, moet dit worden opgegeven met de parameter `domainLevel` . |
| domainSelectorV2 | Boolean | Indien ingesteld op true, wordt een verbeterde methode gebruikt om te bepalen hoe het kenmerk cookie domain moet worden ingesteld. |
| httpsOnly | Boolean | De standaardwaarde is false. Wanneer ingesteld op true, wordt voor cookies de instelling Secure gebruikt wanneer de bijgehouden pagina via https wordt verzonden. |
| useBeaconAPI | Boolean | De standaardwaarde is false. Wanneer reeks aan waar, gebruikt [&#x200B; baken API &#x200B;](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) om niet-blokkerende verzoeken in plaats van [&#x200B; XMLHttpRequest &#x200B;](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) te verzenden. Als de browser deze API niet ondersteunt, gebruikt de Munchkin XMLHttpRequest opnieuw. |
| wsInfo | String | Neemt een tekenreeks om een werkruimte als doel in te stellen. Deze werkruimte-id wordt verkregen door de Workspace te selecteren in het menu Beheer > Integratie > Munchkin. Deze instelling geldt alleen voor het maken van een anonieme lead record. Zodra de het koekjeswaarde van Munchkin voor dat loodverslag is gevestigd, kan de parameter wsInfo niet worden gebruikt om zijn verdeling te veranderen. Aangezien dit plaatsen slechts anonieme lood beïnvloedt, is het slechts relevant voor verdeling-specifieke [&#x200B; Anonieme Bezoekers in de Rapporten van het Web &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/reporting/basic-reporting/report-activity/display-people-or-anonymous-visitors-in-web-reports). |

## Voorbeelden

### Activiteit verzenden naar meerdere abonnementen

In dit voorbeeld worden alle webactiviteiten naar de instanties verzonden met Munchkin-id&#39;s &quot;AAA-BBB-CCC&quot; en &quot;XXX-YYY-ZZ&quot;.

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
