---
title: "Basis-URL"
feature: REST API
description: "Beschrijft hoe te URLs voor Marketo."
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---


# Basis-URL

De [Eindpuntverwijzing](endpoint-reference.md) De documentatie voor elke API vraag toont de methode REST, weg, middel, en parameters die aan basis URL moeten worden toegevoegd om een verzoek te vormen.

Hieronder ziet u een voorbeeld van een goed gevormde REST-URL:

`https://284-RPR-133.mktorest.com/rest/v1/lead/318581.json?fields=email,firstName,lastName`

bestaande uit de volgende delen:

- Basis-URL: `https://284-RPR-133.mktorest.com/rest`
- Pad: `/v1/lead/`
- Bron: `318582.json`
- Query-parameter: `fields=email,firstName,lastName`

De basis-URL bevat de account-id (ook wel Munchkin-id genoemd) en is daarom uniek voor elk Marketo-abonnement. Je basis-URL is gevonden door je aan te melden bij Marketo en naar de **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** -menu. Het wordt geëtiketteerd als &quot;Eindpunt:&quot;onder de &quot;REST API&quot;sectie zoals aangetoond in de volgende screenshots.

![URL-eindpunt basis webservices](assets/rest-api-base-url-web-services.png)

Wanneer u de basis-URL hebt gevonden, kopieert u deze en neemt u deze op in URL&#39;s die u gebruikt wanneer u een van de REST-API&#39;s aanroept.
