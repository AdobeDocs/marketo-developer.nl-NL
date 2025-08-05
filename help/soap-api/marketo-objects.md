---
title: Marketo-objecten
feature: SOAP
description: Overzicht van Marketo-objecten
exl-id: 99b9aed4-94e8-46e8-84d9-2cc5215b0c13
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# Marketo-objecten

Marketo gebruikt Marketo-objecten (MObjects) om verschillende klassen te vertegenwoordigen, zoals Program, Opportunity en OpportunityPersonRole.

## Structuur van MO-objecten

MObjects kunnen van drie typen zijn: Standaard, Aangepast of Virtueel. Standaard- en aangepaste MObjecten vertegenwoordigen afzonderlijke entiteiten, zoals Lead of Company, terwijl virtuele objecten, zoals LeadRecord, bestaan uit velden van een of meer objecten. Virtuele objecten zijn gebruiksvoorwerpen die binnen de API worden gebruikt, maar bestaan niet binnen de Marketo-toepassing.

MObjects bestaan uit:

- Een kleine set vaste kenmerken die alle MObjects gemeen hebben
   - Vereist type
   - Optionele externeKey
   - read-only id, createdAt, updatedAt
- Een lijst met een of meer objectspecifieke kenmerken, zoals naam-/waardeparen, waarvan er enkele vereist kunnen zijn. Geef bijvoorbeeld een naam op Opportunity.
- Een lijst met gekoppelde objectverwijzingen, als object-naam plus
   - Marketo-id of
   - External-key als attribuut-name/attribute-value paar.

### Externe toetsen

Externe sleutels zijn aangepaste velden die zijn gedefinieerd voor Marketo-objecten, zoals Lead of Opportunity. De naam is de veldnaam en waarde is de veldwaarde die in een extern systeem wordt gegenereerd. **Marketo dwingt geen unieke beperking op deze waarden af.** De API-gebruiker moet ervoor zorgen dat de waarden uniek zijn. Als er een duplicaat optreedt, gebruikt Marketo het object dat het laatst is toegevoegd. Dit is vergelijkbaar met het gedrag voor het standaardveld E-mailadres.

### Beschikbare API&#39;s

| API | Kan werken aan |
|---|---|
| describeMObject | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
| getMObjects | Opportunity, OpportunityPersonRole, Program |
| syncMObjects | Opportunity, OpportunityPersonRole, Program |
| deleteMObjects | Opportunity, OpportunityPersonRole |
| listMObjects | ActivityRecord, LeadRecord, Opportunity, OpportunityPersonRole |
