---
title: "Fields"
feature: REST API, Field Management
description: "Een lijst met ondersteunde veldnamen."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---


# Velden

De REST API en SOAP API gebruiken verschillende naamconventies voor hoofdvelden.

## De lijst met veldnamen ophalen

Haal de lijst van alle gesteunde gebiedsnamen terug beschikbaar op uw loodverslagen door REST te gebruiken &quot;beschrijf lood&quot;eindpunt.

## Waar moet u het type veldnaam gebruiken?

Soms is het moeilijk om te weten welk type veldnaam u moet gebruiken wanneer het leveraging van een bepaalde op integratie betrekking hebbende eigenschap. Hieronder volgt een snelle referentie waarvoor functies REST- of SOAP-veldnaamtypen gebruiken.

| Functie | Te gebruiken veldnaamtype |
|--- |--- |
| API voor het bijhouden van leads (Munchkin) | SOAP |
| Forms 2.0 API | SOAP |
| List Import (UI) | SOAP |
| List Import (REST API) | REST |
| Responsafbeeldingen webhaak | SOAP |
| E-mailscripting (snelheid) | SOAP |
| SOAP API | SOAP |
| REST API | REST |

### Waarom retourneert het veld sfdcId van REST API altijd de waarde null?

Het veld `sfdcId` is een formuleveld dat ten onrechte is opgenomen in de oorspronkelijke veldkaart voor de REST API. Records die via de REST API zijn opgehaald, berekenen de waarde van formulevelden niet, zodat de waarde altijd null is. Om de echte identiteitskaart van SFDC te vangen, zou u de geroepen gebieden moeten gebruiken `sfdcLeadId` en `sfdcContactId`.
