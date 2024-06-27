---
title: Regelafstand bijhouden
description: API voor bijhouden van leads
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 66add4c38d0230c36d57009de985649bb67fde3e
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 0%

---

# API voor bijhouden van leads

Met Marketo Munchkin JavaScript kunt u bezoeken aan eindgebruikerspagina&#39;s bijhouden en op Marketo-bestemmingspagina&#39;s en externe webpagina&#39;s klikken. Deze worden geregistreerd in Marketo als &quot;Bezoek Web-pagina&quot;en &quot;Geklikte Verbinding op Web-pagina&quot;activiteiten, die dan in trekkers en filters voor Slimme Campagnes en Slimme Lijsten kunnen worden gebruikt.

## De code insluiten

Uw Marketo-instantie beschikt automatisch over vooraf geconfigureerde codefragmenten voor het bijhouden van code om code in te sluiten op uw externe pagina&#39;s die de activiteit naar uw Marketo-exemplaar traceren. Het gebruik van de insluitcode wordt door deze [licentieovereenkomst](../munchkin-license.pdf).

Er zijn drie typen trackingcode beschikbaar:

1. Eenvoudig - Wordt synchroon geladen
1. Asynchroon - Wordt asynchroon geladen
1. Asynchrone jQuery - Wordt asynchroon geladen en vereist dat jQuery vooraf wordt geladen

Het wordt ten zeerste aanbevolen de asynchrone trackingcode te gebruiken voor het insluiten van Munchkin op externe pagina&#39;s. Sluit de asynchrone trackingcode in om ervoor te zorgen dat de uitvoering met de hoogst mogelijke successnelheid verloopt `<head>` van elke pagina.

Bepaalde systemen voor inhoudsbeheer hebben mogelijk specifieke methoden of beperkingen bij het insluiten van willekeurige scripts.

Ter referentie moet de laatste pagina code bevatten die hierop lijkt `<head>` van uw HTML-document:

```html
<head>
    <script type="text/javascript">
    (function() {
        var didInit = false;
        function initMunchkin() {
            if(didInit === false) {
                didInit = true;
                Munchkin.init('CHANGE-ME');
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
    ...
</head>
```

## Munchkin-gedrag

Marketo Munchkin functioneert standaard als volgt bij het laden van pagina&#39;s:

1. Controleer of de huidige browser een Munchkin-cookie heeft en maak er een als deze er niet is.
1. Verzend een &quot;Bezoek Web-pagina&quot;gebeurtenis aan de aangewezen instantie van Marketo gebruikend de informatie van de huidige pagina en browser. Hiermee wordt een activiteit vastgelegd in de corresponderende record in Marketo.
1. Verzend de gebeurtenis &quot;Geplikte Verbinding op Web-pagina&quot;voor om het even welke gebruiker klikt die op verbindingen voorkomen.

Het gedrag van Munchkin kan worden gewijzigd door het gebruik van Munchkin [Configuratie-instellingen](lead-tracking.md#lead-tracking-api), bijvoorbeeld of een cookie is gemaakt voor alle leads bij het bezoeken van de pagina met de `cookieAnon` instellen of de klikvertraging wijzigen met `clickTime` instellen. Het verzenden van de activiteit van het Bezoek kan worden onbruikbaar gemaakt door apiOnly te plaatsen die aan waar plaatst. Vanaf versie 162 (augustus 2022) klikt u op `tel` en `mailto` koppelingen worden naast `http/s` koppelingen.

## Bekende en anonieme leads

Bij het eerste bezoek van een lead aan een pagina op uw domein wordt een nieuwe anonieme lead record gemaakt in Marketo. De primaire sleutel voor deze record is het Munchkin-cookie (`_mkto_trk`) wordt gemaakt in de browser van de gebruiker. Alle volgende webactiviteiten in die browser worden opgenomen op basis van deze anonieme record. Om aan een bekend record in Marketo te kunnen worden gekoppeld, moet een van de volgende dingen plaatsvinden:

- De lead moet naar een pagina gaan die door Munchkin wordt bijgehouden en die een `mkt_tok` parameter in de queryreeks van een bijgehouden Marketo-e-mailkoppeling.
- De lead moet een Marketo-formulier invullen.
- A SOAP [syncLead](../soap-api/leads.md) of REST [Associate Lead](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) de vraag moet worden verzonden.

Wanneer aan een van deze voorwaarden is voldaan, worden het cookie en alle bijbehorende webactiviteit gekoppeld aan de bekende lead.

Voor elke afzonderlijke browser wordt een nieuwe anonieme webactiviteitenrecord gemaakt. Als een lead uw domein dus voor het eerst bezoekt met een nieuwe computer en/of browser, moet deze koppeling opnieuw plaatsvinden.

## Domeinen

Munchkin maakt en volgt afzonderlijke cookies per domein. Voor bekende &#39;lead tracking&#39; in verschillende domeinen moet dus voor elk domein een &#39;lead association&#39;-gebeurtenis plaatsvinden. Als ik bijvoorbeeld twee domeinen bestel, `marketo.com`, en `example.com`en een lead een formulier invult op `marketo.com`, navigeert vervolgens naar `example.com` later, dan hun activiteit op `marketo.com` wordt getraceerd op een bekende loodrecord, maar hun activiteit wordt `example.com` is anoniem. Bekende leads blijven echter bestaan over subdomeinen, dus een bekende lead op `www.example.com` is ook bekend dat `info.example.com`.

In het geval dat uw bovenste domein twee delen is, zoals `.co.uk`voegt u vervolgens een domainLevel-parameter toe aan uw Munchkin-fragment, zodat de code correct wordt bijgehouden. Zie [hier](lead-tracking.md#domains) voor meer informatie .

## Cookie

Het Munchkin-cookie gebruikt de toets `_mkto_trk`en heeft een waarde die volgt op dit patroon:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

Munchkin-cookies zijn specifiek voor elk domein op het tweede niveau, dat wil zeggen: `example.com`. De standaardlevensduur van de cookie is 2 jaar (730 dagen).

## Beta

Ga naar uw [Admin -> Treasure Chest](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) en schakelt u de instelling &quot;Munchkin Beta on Landing Pages&quot; in. Dit levert nieuwe codefragmenten in **[!UICONTROL Admin]** ->  **[!UICONTROL Munchkin]** gebruiken op externe sites.

## Uitschakelen

Bezoekers kunnen Munchkin-tracking volledig uitschakelen door de `querystring` parameter &quot;marketo_opt_out=true&quot; aan de URL in hun browser. Wanneer de JavaScript-code voor Munchkin deze instelling detecteert, wordt geprobeerd een nieuwe cookie &quot;mkto_opt_out&quot; in te stellen met de waarde `true`. Alle andere volgcookies van Marketo worden verwijderd, er worden geen nieuwe cookies ingesteld en er worden geen HTTP-aanvragen ingediend door Munchkin wanneer deze instelling wordt gedetecteerd.
