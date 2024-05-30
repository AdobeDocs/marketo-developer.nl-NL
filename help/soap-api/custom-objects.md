---
title: "Aangepaste objecten"
feature: SOAP
description: "Aangepaste objecten maken."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---


# Aangepaste objecten

Met een Marketo Custom-object kunt u een-op-vele relatie maken tussen uw Marketo-leads en de aangepaste objectrapporten. U wilt bijvoorbeeld alle routepresentaties bijhouden die worden bijgewoond door een lead. Aangezien de leads een aantal (over veelvoudige jaren) wegshows kunnen bijwonen, zijn de douanevoorwerpen geschikter om deze informatie op te slaan.

Aangepaste objecten worden ondersteund in alle Marketo-edities, met uitzondering van Spark. Wanneer het aangepaste Marketo-object met succes is ingericht in uw account, kunt u

- Records maken/lezen/bijwerken/verwijderen in het aangepaste object via de Marketo SOAP API
- De Slimme trekker van de Lijst van het gebruik wanneer de nieuwe verslagen aan het douanevoorwerp worden toegevoegd
- De gegevens van aangepaste objecten gebruiken als filter in slimme lijsten
- De gegevens van aangepaste objecten in e-mailberichten gebruiken met Marketo E-mailscripts

## Structuur van aangepaste objecten

Aangepaste objecten bestaan uit:

- Een kleine set vaste kenmerken die alle aangepaste objecten gemeen hebben:
   - Objectnaam (ook bekend als de naam van het objecttype)
   - Objectbeschrijving
   - Aangepast object naar veldnaam voor Marketo-koppeling - dit is een veld in het object lead (Person) waarnaar het aangepaste object verwijst
   - Veldnaam objectsleutel - de primaire sleutel die door het object wordt gebruikt
- Een of meer objectspecifieke velden (we ondersteunen maximaal 50 van dergelijke velden)

## Aangepaste objectbewerkingen

De volgende oproepen kunnen worden gebruikt om met CO&#39;s in wisselwerking te staan.

- [getCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/getCustomObjectsUsingGET)
- [syncCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/syncCustomObjectsUsingPOST)
- [deleteCustomObjects](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Custom-Objects/operation/deleteCustomObjectsUsingPOST)
