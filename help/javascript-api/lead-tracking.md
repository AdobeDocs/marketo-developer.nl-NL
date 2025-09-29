---
title: Regelafstand bijhouden
description: Leer hoe u Marketo Munchkin JavaScript kunt insluiten, bezoeken en klikken kunt volgen, bekende of anonieme leads kunt beheren, interdomeincookies en de optie om te weigeren voor slimme campagnes.
feature: Munchkin Tracking Code, Javascript
exl-id: 7ece5133-9d32-4be3-a940-4ac0310c4d8b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '784'
ht-degree: 0%

---

# API voor bijhouden van leads

Met Marketo Munchkin JavaScript kunt u bezoeken aan eindgebruikerspagina&#39;s bijhouden en op Marketo-bestemmingspagina&#39;s en externe webpagina&#39;s klikken. Deze worden geregistreerd in Marketo als &quot;Bezoek Web-pagina&quot;en &quot;Geklikte Verbinding op Web-pagina&quot;activiteiten, die dan in trekkers en filters voor Slimme Campagnes en Slimme Lijsten kunnen worden gebruikt.

## De code insluiten

Uw Marketo-instantie beschikt automatisch over vooraf geconfigureerde codefragmenten voor het bijhouden van code om code in te sluiten op uw externe pagina&#39;s die de activiteit naar uw Marketo-exemplaar traceren. Het gebruik van de ingebedde code wordt geregeerd door deze [ vergunningsovereenkomst ](../munchkin-license.pdf).

Er zijn drie typen trackingcode beschikbaar:

1. Eenvoudig - Wordt synchroon geladen
1. Asynchroon - Wordt asynchroon geladen
1. Asynchrone jQuery - Wordt asynchroon geladen en vereist dat jQuery vooraf wordt geladen

Het wordt ten zeerste aanbevolen de asynchrone trackingcode te gebruiken voor het insluiten van Munchkin op externe pagina&#39;s. Sluit de asynchrone trackingcode in `<head>` van elke pagina in om ervoor te zorgen dat de uitvoering met de hoogst mogelijke successnelheid verloopt.

Bepaalde systemen voor inhoudsbeheer hebben mogelijk specifieke methoden of beperkingen bij het insluiten van willekeurige scripts.

Ter referentie moet de laatste pagina code bevatten die vergelijkbaar is met deze code in `<head>` van uw HTML-document:

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

Marketo Munchkin werkt standaard als volgt bij het laden van pagina&#39;s:

1. Controleer of de huidige browser een Munchkin-cookie heeft en maak een cookie als deze niet aanwezig is.
1. Verzend een &quot;Bezoek Web-pagina&quot;gebeurtenis aan de aangewezen instantie van Marketo gebruikend de informatie van de huidige pagina en browser. Hiermee wordt een activiteit vastgelegd in de corresponderende record in Marketo.
1. Verzend de gebeurtenis &quot;Geplikte Verbinding op Web-pagina&quot;voor om het even welke gebruiker klikt die op verbindingen voorkomen.

Het gedrag van Munchkin kan door het gebruik van de montages van de Configuratie van Munchkin [ worden gewijzigd ](configuration.md), zoals of een koekje voor alle lood op het bezoeken van de pagina met het `cookieAnon` plaatsen wordt gecreeerd, of het wijzigen van de klikvertraging met `clickTime` het plaatsen. Het verzenden van de activiteit van het Bezoek kan worden onbruikbaar gemaakt door apiOnly te plaatsen die aan waar plaatst. Vanaf versie 162 (augustus 2022) klikt u op `tel` en worden `mailto` -koppelingen naast `http/s` -koppelingen bijgehouden.

## Bekende en anonieme leads

Bij het eerste bezoek van een lead aan een pagina op uw domein wordt een nieuwe anonieme lead record gemaakt in Marketo. De primaire sleutel voor deze record is de Munchkin cookie (`_mkto_trk`) die in de browser van de gebruiker wordt gemaakt. Alle volgende webactiviteiten in die browser worden opgenomen op basis van deze anonieme record. Om aan een bekend record in Marketo te kunnen worden gekoppeld, moet een van de volgende dingen plaatsvinden:

- De lead moet een door Munchkin bijgehouden pagina met een parameter `mkt_tok` in de queryreeks bezoeken via een bijgehouden Marketo-e-mailkoppeling.
- De lead moet een Marketo-formulier invullen.
- Een REST [ associeerde Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST) vraag moet worden verzonden.

Wanneer aan een van deze voorwaarden is voldaan, worden het cookie en alle bijbehorende webactiviteit gekoppeld aan de bekende lead.

Voor elke afzonderlijke browser wordt een nieuwe anonieme webactiviteitenrecord gemaakt. Als een lead uw domein dus voor het eerst bezoekt met een nieuwe computer en/of browser, moet deze koppeling opnieuw plaatsvinden.

## Domeinen

Munchkin maakt en volgt afzonderlijke cookies per domein. Voor bekende &#39;lead tracking&#39; in verschillende domeinen moet dus voor elk domein een &#39;lead association&#39;-gebeurtenis plaatsvinden. Als ik bijvoorbeeld twee domeinen bestel, `marketo.com` en `example.com` , en een lead een formulier invult op `marketo.com` , navigeert u naar `example.com` later, waarna hun activiteit op `marketo.com` wordt bijgehouden in een bekende lead-record, maar hun activiteit op `example.com` is anoniem. Bekende leads blijven echter bestaan in verschillende subdomeinen, dus een bekende lead op `www.example.com` is ook een bekende lead op `info.example.com` .

Als uw top-level domein twee delen, zoals `.co.uk` is, dan voeg een domainLevel parameter aan uw fragment van Munchkin voor de code toe correct te volgen. Zie [ hier ](configuration.md#domainlevel) voor meer details.

## Cookie

Het Munchkin-cookie gebruikt de sleutel `_mkto_trk` en heeft een waarde volgens dit patroon:

`id:561\-HYG\-937&token:_mch\-marketo.com\-1374552656411\-90718`

of

`id:561\-HYG\-937&token:_mch\-marketo.com\-97bf4361ef4433921a6da262e8df45a`

Munchkin cookies zijn specifiek voor elk domein op het tweede niveau, dat wil zeggen `example.com` . De standaardlevensduur van de cookie is 2 jaar (730 dagen).

## Beta

Om binnen aan het bèta kanaal van Munchkin voor uw landende pagina&#39;s te kiezen, ga naar uw [ Admin -> het menu van de Borst van de Schat ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) en laat &quot;Munchkin Beta op het Aanvoeren van Pagina&#39;s&quot;toe plaatsen. Dit biedt nieuwe codefragmenten in de map **[!UICONTROL Admin]** ->  **[!UICONTROL Munchkin]** gebruiken om de bètaversie op externe sites te gebruiken.

## Uitschakelen

Bezoekers kunnen het bijhouden van Munchkin volledig uitschakelen door de parameter `querystring` &quot;marketo_opt_out=true&quot; toe te voegen aan de URL in hun browser. Wanneer de Munchkin JavaScript deze instelling detecteert, wordt geprobeerd een nieuwe cookie &quot;mkto_opt_out&quot; in te stellen met de waarde `true` . Alle andere volgcookies van Marketo worden verwijderd, er worden geen nieuwe cookies ingesteld en er worden geen HTTP-aanvragen ingediend door Munchkin wanneer deze instelling wordt gedetecteerd.
