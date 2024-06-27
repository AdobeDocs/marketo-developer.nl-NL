---
title: Definities van Swagger downloaden
feature: REST API, Programs
description: Download Swagger-definitiebestanden voor lokaal verbruik.
source-git-commit: 85062243d57a3fc6d15251163e926495858edf2a
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Definities van Swagger downloaden

[Swagger](https://swagger.io/) of [OpenAPI](https://www.openapis.org/) definities zijn gegevensbestanden die alle methoden en parameters van een REST API beschrijven. U kunt deze gegevensbestanden lokaal downloaden en gebruiken als uw persoonlijke API-referentie.

Als u de definities Marketo Engage Swagger/OpenAPI wilt gebruiken, gaat u naar `host` moet het veld van elk document worden bijgewerkt zodat dit overeenkomt met de REST API-hostnaam van uw Marketo Engage-instantie.

Download eerst de OpenAPI-definitie die u wilt gebruiken:

* [Element](assets/swagger-asset.json)
* [Lood](assets/swagger-mapi.json)
* [Identiteit](assets/swagger-identity.json)
* [Gebruikersbeheer](assets/swagger-user.json)

Vervolgens haalt u de hostnaam van de REST API op bij Marketo Admin. Ga naar de _Beheerder_-> _Webservices_ in Marketo Engage en kopieer hostname van het gebied van het Eindpunt. De `hostname` de tekenreeks tussen het protocol is; `https://`, en `/rest`, die er zo uitziet `AAA-999-AAA.mktorest.com`

Open uw OpenAPI-bestand in een teksteditor en zoek de `host` in de JSON en wijzig de waarde ervan in uw REST API-hostnaam: `"host":"localhost:8080"` tot `"host":"AAA-999-AAA.mktorest.com"`.

Nadat u het definitiebestand hebt opgeslagen, kunt u het uitvoeren in uw Swagger UI-instantie of een andere OpenAPI-software.
