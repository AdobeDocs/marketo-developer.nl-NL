---
title: Velden
feature: REST API, Field Management
description: Leer REST en SOAP lood gebiedsnaming, lijstgebieden via REST beschrijf Lood, eigenschapsafbeelding, waarom sfdcId ongeldig is, en gebruik sfdcLeadId of sfdcContactId.
exl-id: 9033f32a-c7cb-4bbf-abcf-38ca4112139f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---

# Velden

De REST API en de SOAP API gebruiken verschillende naamconventies voor hoofdvelden.

## De lijst met veldnamen ophalen

Haal de lijst van alle gesteunde gebiedsnamen terug beschikbaar op uw loodverslagen door REST te gebruiken &quot;beschrijf lood&quot;eindpunt.

## Waar moet u het type veldnaam gebruiken?

Soms is het moeilijk om te weten welk type veldnaam u moet gebruiken wanneer het leveraging van een bepaalde op integratie betrekking hebbende eigenschap. Hieronder volgt een snelle referentie waarvoor functies gebruikmaken van de veldnaamtypen REST of SOAP.

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

Het veld `sfdcId` is een formules-veld dat ten onrechte is opgenomen in de oorspronkelijke veldkaart voor de REST API. Records die via de REST API zijn opgehaald, berekenen de waarde van formulevelden niet, zodat de waarde altijd null is. Als u de echte SFDC-id wilt vastleggen, gebruikt u de velden `sfdcLeadId` en `sfdcContactId` .
