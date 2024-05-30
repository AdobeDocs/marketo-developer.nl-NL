---
title: "Eindpuntverwijzing"
feature: REST API
description: "Marketo API-eindpuntverwijzingen"
source-git-commit: 2454f126dc4275697ef6773420453ad8853eae73
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# Eindpuntverwijzing

- [Element](https://developer.adobe.com/marketo-apis/api/asset/)
- [Identiteit](https://developer.adobe.com/marketo-apis/api/identity/)
- [Database lead](https://developer.adobe.com/marketo-apis/api/mapi/)
- [Gebruikersbeheer](https://developer.adobe.com/marketo-apis/api/user/)

## Eindpuntverwijzing

Marketo gebruikt Swagger om een formele definitie te geven van de openbare interface voor zijn REST API&#39;s. Swagger biedt een uitgebreid definitiemodel voor URL-structuren, aanvraagmodellen en responsmodellen, en beschikt over een ontwikkeld ecosysteem van gereedschappen voor gebruik met API-interactie, testen en het genereren van clients.

De eindpuntverwijzing gebruikt het [Swagger-UI](https://swagger.io/tools/swagger-ui/) JavaScript-pakket om de referentiepagina&#39;s op de client te genereren. Elk openbaar eindpunt wordt vermeld en geeft de structuur van het reactiemodel, vereiste verzoekparameters, en het verzoekmodel indien nodig.

## Definities Marketo-wagerdefinities gebruiken

De Swagger-standaard vereist dat er een host wordt opgegeven of dat de host wordt gegenereerd door de host die het bestand aanbiedt. Het is belangrijk dat u de host in de definitie corrigeert voordat u bestaande gereedschappen gebruikt, aangezien Marketo een lege hostparameter met de definitie biedt. De REST API-host voor elke Marketo-instantie is uniek en volgt het patroon:

`{Munchkin ID}.mktorest.com`

## Elementen-API&#39;s

De Marketo Asset API&#39;s gebruiken `application/x-www-url-formencoded` stijlparameters in verzoeken om eindpunten die een methode van de POST vereisen. In sommige gevallen, zoals mapparameters, kan de waarde voor de parameter echter een JSON-array of -object zijn. Deze parameters worden genoteerd in de eindpuntverwijzing.
