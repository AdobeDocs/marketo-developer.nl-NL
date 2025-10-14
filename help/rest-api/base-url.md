---
title: Basis-URL
feature: REST API
description: Leer Marketo REST API-aanvragen te maken, inzicht te krijgen in de bronnen en parameters van het basis-URL-pad en uw unieke basis-URL te vinden.
exl-id: 6c3f122c-3ace-4ed3-bed0-a6b89cedc99a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Basis-URL

De [&#x200B; documentatie van de Verwijzing van 0&rbrace; Eindpunt voor elke API vraag toont de REST methode, de weg, het middel, en de parameters die aan basis URL moeten worden toegevoegd om een verzoek te vormen.](endpoint-reference.md)

Hieronder ziet u een voorbeeld van een goed gevormde REST-URL:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

bestaande uit de volgende delen:

- Basis-URL: `https://284-RPR-133.mktorest.com/rest`
- Pad: `/v1/lead/`
- Bron: `318582.json`
- Query-parameter: `fields=email,firstName,lastName`

De basis-URL bevat de account-id (ook wel Munchkin-id genoemd) en is daarom uniek voor elk Marketo-abonnement. De basis-URL wordt gevonden door u aan te melden bij Marketo en naar het menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** te navigeren. Het wordt geëtiketteerd als &quot;Eindpunt:&quot;onder de &quot;REST API&quot;sectie zoals aangetoond in de volgende screenshots.

![&#x200B; Eindpunt van URL van de Basis van de Diensten van het Web &#x200B;](assets/rest-api-base-url-web-services.png)

Wanneer u de basis-URL hebt gevonden, kopieert u deze en neemt u deze op in URL&#39;s die u gebruikt wanneer u een van de REST-API&#39;s aanroept.
