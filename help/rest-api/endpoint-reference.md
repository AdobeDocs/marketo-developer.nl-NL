---
title: Eindpuntverwijzing
feature: REST API
description: Verwijzingen naar Marketo API-eindpunt
exl-id: 27d16b6f-865a-4e40-ab9c-cbabe2927472
source-git-commit: 8a019985fc9ce7e1aa690ca26bfa263cd3c48cfc
workflow-type: tm+mt
source-wordcount: '4676'
ht-degree: 1%

---

# Eindpuntverwijzing

Hieronder vindt u koppelingen naar de Marketo REST API-referenties.

- [ Activa ](https://developer.adobe.com/marketo-apis/api/asset/)
- [ Identiteit ](https://developer.adobe.com/marketo-apis/api/identity/)
- [ Lood Gegevensbestand ](https://developer.adobe.com/marketo-apis/api/mapi/)
- [ Gebruikersbeheer ](https://developer.adobe.com/marketo-apis/api/user/)

## Eindpuntlijst

Hier volgt een uitgebreide lijst met REST API-eindpunten.

| Naam | Groep | Methode | URI | Vereiste toestemming |
|---|---|---|---|---|
| Aangepaste activiteiten toevoegen | Activiteiten | POST | /rest/v1/activities/external.json | Leesschrijfactiviteit |
| Aangepast activiteitstype goedkeuren | Activiteiten | POST | /rest/v1/activities/external/type/{apiName} /approve.json | Metagegevens lezen/schrijven-activiteit |
| Aangepaste activiteitstypekenmerken maken | Activiteiten | POST | /rest/v1/activities/external/type/{apiName} /attributes/create.json | Metagegevens lezen/schrijven-activiteit |
| Aangepaste activiteitstypen maken | Activiteiten | POST | /rest/v1/activities/external/type.json | Metagegevens lezen/schrijven-activiteit |
| Aangepast activiteitstype verwijderen | Activiteiten | POST | /rest/v1/activities/external/type/{apiName} /delete.json | Metagegevens lezen/schrijven-activiteit |
| Aangepaste activiteitstypekenmerken verwijderen | Activiteiten | POST | /rest/v1/activities/external/type/{apiName} /attributes/delete.json | Metagegevens lezen/schrijven-activiteit |
| Beschrijf het type aangepaste activiteit | Activiteiten | GET | /rest/v1/activities/external/type/{apiName} /describe.json | Metagegevens activiteit alleen-lezen |
| Concept voor aangepast activiteitstype negeren | Activiteiten | POST | /rest/v1/activities/external/type/{apiName} /discardDraft.json | Metagegevens lezen/schrijven-activiteit |
| Activiteitstypen ophalen | Activiteiten | GET | /rest/v1/activities/types.json | Alleen-lezen activiteit |
| Aangepaste activiteitstypen ophalen | Activiteiten | GET | /rest/v1/activities/external/types.json | Metagegevens activiteit alleen-lezen |
| Verwijderde leads ophalen | Activiteiten | GET | /rest/v1/activities/deletedleads.json | Alleen-lezen activiteit |
| Leadactiviteiten ophalen | Activiteiten | GET | /rest/v1/activities.json | Alleen-lezen activiteit |
| Wijzigingen in lead ophalen | Activiteiten | GET | /rest/v1/activities/leadchanges.json | Alleen-lezen activiteit |
| Paginasken ophalen | Activiteiten | GET | /rest/v1/activities/pagingtoken.json | Alleen-lezen activiteit |
| Type aangepaste activiteit bijwerken | Activiteiten | POST | /rest/v1/activities/external/type/{apiName}.json | Metagegevens lezen/schrijven-activiteit |
| Kenmerken van aangepast activiteitstype bijwerken | Activiteiten | POST | /rest/v1/activities/external/type/{apiName} /attributes/update.json | Metagegevens lezen/schrijven-activiteit |
| Identiteit | Verificatie | GET of POST | /identity/oauth/token | Geen |
| Taak exportactiviteit annuleren | Bulkexportactiviteiten | POST | /bulk/v1/activities/export/{exportid} /cancel.json | Alleen-lezen activiteit |
| Taak voor exportactiviteit maken | Bulkexportactiviteiten | POST | /bulk/v1/activities/export/create.json | Alleen-lezen activiteit |
| Taak voor exportactiviteit opgeven | Bulkexportactiviteiten | POST | /bulk/v1/activities/export/{exportid} /enqueue.json | Alleen-lezen activiteit |
| Exportactiviteitenbestand ophalen | Bulkexportactiviteiten | GET | /bulk/v1/activities/export/{exportid} /file.json | Alleen-lezen activiteit |
| Taakstatus exportactiviteit ophalen | Bulkexportactiviteiten | GET | /bulk/v1/activities/export/{exportid} /status.json | Alleen-lezen activiteit |
| Exporttaken ophalen | Bulkexportactiviteiten | GET | /bulk/v1/activities/export.json | Alleen-lezen activiteit |
| Aangepaste objecttaak exporteren annuleren | Aangepaste objecten bulksgewijs exporteren | POST | /bulk/v1/customobjects/export/{exportid} /cancel.json | Alleen-lezen aangepast object |
| Aangepaste objecttaak exporteren | Aangepaste objecten bulksgewijs exporteren | POST | /bulk/v1/customobjects/export/create.json | Alleen-lezen aangepast object |
| Aangepaste objecttaak exporteren | Aangepaste objecten bulksgewijs exporteren | POST | /bulk/v1/customobjects/export/{exportid} /enqueue.json | Alleen-lezen aangepast object |
| Aangepast objectbestand exporteren | Aangepaste objecten bulksgewijs exporteren | GET | /bulk/v1/customobjects/export/{exportid} /file.json | Alleen-lezen aangepast object |
| Taakstatus van aangepast object exporteren | Aangepaste objecten bulksgewijs exporteren | GET | /bulk/v1/customobjects/export/{exportid} /status.json | Alleen-lezen aangepast object |
| Aangepaste objecttaken exporteren | Aangepaste objecten bulksgewijs exporteren | GET | /bulk/v1/customobjects/export.json | Alleen-lezen aangepast object |
| Taak lead exporteren annuleren | Voorlooptekens voor bulkexport | POST | /bulk/v1/leads/export/{exportid} /cancel.json | Lead, alleen-lezen |
| Taak voor exportlead maken | Voorlooptekens voor bulkexport | POST | /bulk/v1/leads/export/create.json | Lead, alleen-lezen |
| Taak voor exportlead in wachtrij plaatsen | Voorlooptekens voor bulkexport | POST | /bulk/v1/leads/export/{exportid} /enqueue.json | Lead, alleen-lezen |
| Voorloopbestand exporteren | Voorlooptekens voor bulkexport | GET | /bulk/v1/leads/export/{exportid} /file.json | Lead, alleen-lezen |
| Status van lead-exporttaak ophalen | Voorlooptekens voor bulkexport | GET | /bulk/v1/leads/export/{exportid} /status.json | Lead, alleen-lezen |
| Exportlead-taken ophalen | Voorlooptekens voor bulkexport | GET | /bulk/v1/leads/export.json | Lead, alleen-lezen |
| Taak voor programmalid annuleren | Leden van het bulkexportprogramma | POST | /bulk/v1/program/members/export/{exportid} /cancel.json | Lead, alleen-lezen |
| Taak voor lid van exportprogramma maken | Leden van het bulkexportprogramma | POST | /bulk/v1/program/members/export/create.json | Lead, alleen-lezen |
| Taak van het Lid van het Programma van de Uitvoer | Leden van het bulkexportprogramma | POST | /bulk/v1/program/members/export/{exportid} /enqueue.json | Lead, alleen-lezen |
| Lid-bestand van exportprogramma ophalen | Leden van het bulkexportprogramma | GET | /bulk/v1/program/members/export/{exportid} /file.json | Lead, alleen-lezen |
| Status van lid van exportprogramma ophalen | Leden van het bulkexportprogramma | GET | /bulk/v1/program/members/export/{exportid} /status.json | Lead, alleen-lezen |
| Taken voor leden van exportprogramma&#39;s ophalen | Leden van het bulkexportprogramma | GET | /bulk/v1/program/members/export.json | Lead, alleen-lezen |
| Importeren van aangepast object mislukt | Aangepaste objecten bulkimport | GET | /bulk/v1/customobjects/import/{id} /failures.json | Aangepast object lezen/schrijven |
| Aangepaste objectstatus importeren | Aangepaste objecten bulkimport | GET | /bulk/v1/customobjects/import/{id} /status.json | Aangepast object lezen/schrijven |
| Waarschuwingen bij eigen object importeren ophalen | Aangepaste objecten bulkimport | GET | /bulk/v1/customobjects/import/{id} /warnings.json | Aangepast object lezen/schrijven |
| Aangepaste objecten importeren | Aangepaste objecten bulkimport | POST | /bulk/v1/customobjects/{apiName} /import.json | Aangepast object lezen/schrijven |
| Fout bij ophalen van lead | Bulkimportleads | GET | /bulk/v1/leads/batch/{id} /failures.json | Lead lezen |
| Status importlead ophalen | Bulkimportleads | GET | /bulk/v1/lead/batch/{id}.json | Lead lezen |
| Waarschuwingen bij lead importeren ophalen | Bulkimportleads | GET | /bulk/v1/leads/batch/{id} /warnings.json | Lead lezen |
| Leads importeren | Bulkimportleads | POST | /bulk/v1/leads.json | Lead lezen |
| Fout door lid van importprogramma ophalen | Leden van het bulkimportprogramma | GET | /bulk/v1/program/members/import/{id} /failures.json | Lead lezen |
| Lidstatus van importprogramma ophalen | Leden van het bulkimportprogramma | GET | /bulk/v1/program/members/import/{id} /status.json | Lead lezen |
| Waarschuwingen voor leden van importprogramma ophalen | Leden van het bulkimportprogramma | GET | /bulk/v1/program/members/import/{id} /warnings.json | Lead lezen |
| Programmsleden importeren | Leden van het bulkimportprogramma | POST | /bulk/v1/program/{programId} /members/import.json | Lead lezen |
| Campagne ophalen op id | Campagnes | GET | /rest/v1/campagnes/{id}.json | Alleen-lezen campagnes |
| Campagnes ophalen | Campagnes | GET | /rest/v1/campaigns.json | Alleen-lezen campagnes |
| Campagne aanvragen | Campagnes | POST | /rest/v1/campagnes/{id} /trigger.json | Campagnes lezen |
| Campagne plannen | Campagnes | POST | /rest/v1/campagnes/{id} /schedule.json | Campagnes lezen |
| Kanaal op naam ophalen | Kanalen | GET | /rest/asset/v1/channel/byName.json | Alleen-lezen element |
| Kanalen ophalen | Kanalen | GET | /rest/asset/v1/channels.json | Alleen-lezen element |
| Bedrijven verwijderen | Bedrijven | POST | /rest/v1/companies/delete.json | Read-Write Bedrijf |
| Beschrijf bedrijven | Bedrijven | GET | /rest/v1/companies/describe.json | Alleen-lezen bedrijf |
| Bedrijven ophalen | Bedrijven | GET | /rest/v1/companies.json | Alleen-lezen bedrijf |
| Bedrijven synchroniseren | Bedrijven | POST | /rest/v1/companies.json | Read-Write Bedrijf |
| Veld van bedrijf op naam ophalen | Bedrijven | GET | /rest/v1/companies/schema/fields/{fieldApiName}.json | Aangepast veld schema voor lezen/schrijven |
| Bedrijfsvelden ophalen | Bedrijven | GET | /rest/v1/companies/schema/fields.json | Aangepast veld schema voor lezen/schrijven |
| Aangepaste objecttekstvelden toevoegen | Aangepaste objecten | POST | /rest/v1/customobjects/schema/{apiName} /addField.json | Aangepast objecttype lezen-schrijven |
| Aangepast objecttype goedkeuren | Aangepaste objecten | POST | /rest/v1/customobjects/schema/{apiName} /approve.json | Aangepast objecttype lezen-schrijven |
| Aangepaste objecten verwijderen | Aangepaste objecten | POST | /rest/v1/customobjects/{name} /delete.json | Aangepast object lezen/schrijven |
| Aangepast objecttype verwijderen | Aangepaste objecten | POST | /rest/v1/customobjects/schema/{apiName} /delete.json | Aangepast objecttype lezen-schrijven |
| Aangepaste objecttekstvelden verwijderen | Aangepaste objecten | POST | /rest/v1/customobjects/schema/{apiName} /deleteField.json | Aangepast objecttype lezen-schrijven |
| Aangepaste objecten beschrijven | Aangepaste objecten | GET | /rest/v1/customobjects/{name} /describe.json | Alleen-lezen aangepast object |
| Beschrijf het type aangepast object | Aangepaste objecten | GET | /rest/v1/customobjects/schema/{apiName} /describe.json | Aangepast objecttype, alleen-lezen |
| Concept Aangepast objecttype negeren | Aangepaste objecten | POST | /rest/v1/customobjects/schema/{apiName} /discardDraft.json | Aangepast objecttype lezen-schrijven |
| Aangepaste objecten ophalen | Aangepaste objecten | GET | /rest/v1/customobjects/{name}.json | Alleen-lezen aangepast object |
| Aangepast object aanpasbare objecten ophalen | Aangepaste objecten | GET | /rest/v1/customobjects/schema/linkableObjects.json | Aangepast objecttype, alleen-lezen |
| Assets voor aangepast object ophalen | Aangepaste objecten | GET | /rest/v1/customobjects/schema/{apiName} /dependentAssets.json | Aangepast objecttype, alleen-lezen |
| Gegevenstypen van aangepast objecttype ophalen | Aangepaste objecten | GET | /rest/v1/customobjects/schema/fieldDataTypes.json | Aangepast objecttype, alleen-lezen |
| Lijst met aangepaste objecten | Aangepaste objecten | GET | /rest/v1/customobjects.json | Alleen-lezen aangepast object |
| Lijst met aangepaste objecttypen | Aangepaste objecten | GET | /rest/v1/customobjects/schema.json | Aangepast objecttype, alleen-lezen |
| Aangepaste objecten synchroniseren | Aangepaste objecten | POST | /rest/v1/customobjects/{name}.json | Aangepast object lezen/schrijven |
| Aangepast objecttype synchroniseren | Aangepaste objecten | POST | /rest/v1/customobjects/schema.json | Aangepast objecttype lezen-schrijven |
| Veld voor aangepast objecttype bijwerken | Aangepaste objecten | POST | /rest/v1/customobjects/schema/{apiName} /updateField.json | Aangepast objecttype lezen-schrijven |
| Concept voor e-mailsjabloon goedkeuren | E-mailsjablonen | POST | /rest/asset/v1/emailTemplate/{id} /approveDraft.json | Read-Write-element |
| E-mailsjabloon klonen | E-mailsjablonen | POST | /rest/asset/v1/emailTemplate/{id} /clone.json | Read-Write-element |
| E-mailsjabloon maken | E-mailsjablonen | POST | /rest/asset/v1/emailTemplates.json | Read-Write-element |
| E-mailsjabloon verwijderen | E-mailsjablonen | POST | /rest/asset/v1/emailTemplate/{id} /delete.json | Read-Write-element |
| Concept e-mailsjabloon negeren | E-mailsjablonen | POST | /rest/asset/v1/emailTemplate/{id} /discardDraft.json | Read-Write-element |
| E-mailtemplate ophalen op ID | E-mailsjablonen | GET | /rest/asset/v1/emailTemplate/{id}.json | Alleen-lezen element |
| E-mailsjabloon op naam ophalen | E-mailsjablonen | GET | /rest/asset/v1/emailTemplate/byName.json | Alleen-lezen element |
| E-mailsjablooninhoud ophalen op ID | E-mailsjablonen | GET | /rest/asset/v1/emailTemplate/{id} /content.json | Alleen-lezen element |
| E-mailtemplate ophalen gebruikt door | E-mailsjablonen | GET | /rest/asset/v1/emailTemplates/{id} /usedBy.json | Alleen-lezen element |
| E-mailsjablonen ophalen | E-mailsjablonen | GET | /rest/asset/v1/emailTemplates.json | Alleen-lezen element |
| Concept e-mailsjabloon niet goedkeuren | E-mailsjablonen | POST | /rest/asset/v1/emailTemplate/{id} /unapprove.json | Read-Write-element |
| E-mailsjablooninhoud bijwerken | E-mailsjablonen | POST | /rest/asset/v1/emailTemplate/{id} /content.json | Read-Write-element |
| Metagegevens e-mailsjabloon bijwerken | E-mailsjablonen | POST | /rest/asset/v1/emailTemplate/{id}.json | Read-Write-element |
| E-mailmodule toevoegen | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId} /add.json | Read-Write-element |
| Concept voor e-mail goedkeuren | E-mails | POST | /rest/asset/v1/email/{id} /approveDraft.json | Read-Write-element |
| E-mail klonen | E-mails | POST | /rest/asset/v1/email/{id} /clone.json | Read-Write-element |
| E-mail maken | E-mails | POST | /rest/asset/v1/emails.json | Read-Write-element |
| E-mail verwijderen | E-mails | POST | /rest/asset/v1/email/{id} /delete.json | Read-Write-element |
| Module verwijderen | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId} /delete.json | Read-Write-element |
| Concept voor e-mail negeren | E-mails | POST | /rest/asset/v1/email/{id} /discardDraft.json | Read-Write-element |
| E-mailmodule dupliceren | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId} /duplicate.json | Read-Write-element |
| E-mail ophalen op id | E-mails | GET | /rest/asset/v1/email/{id}.json | Alleen-lezen element |
| E-mail ophalen op naam | E-mails | GET | /rest/asset/v1/email/byName.json | Alleen-lezen element |
| E-mailinhoud ophalen | E-mails | GET | /rest/asset/v1/email/{id} /content.json | Alleen-lezen element |
| E-maildynamische inhoud ophalen | E-mails | GET | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId} .json | Alleen-lezen element |
| Volledige e-mail ophalen | E-mails | GET | /rest/asset/v1/email/{id} /fullContent.json | Alleen-lezen element |
| E-mailvariabelen ophalen | E-mails | GET | /rest/asset/v1/email/{id} /variables.json | Alleen-lezen element |
| E-mailCC-velden ophalen | E-mails | GET | /rest/asset/v1/email/ccFields.json | Alleen-lezen element |
| E-mails ophalen | E-mails | GET | /rest/asset/v1/emails.json | Alleen-lezen element |
| E-mailmodules opnieuw rangschikken | E-mails | POST | /rest/asset/v1/email/{id} /content/rearrange.json | Read-Write-element |
| Naam e-mailmodule wijzigen | E-mails | POST | /rest/asset/v1/email/{id}/content/{moduleId} /rename.json | Read-Write-element |
| Voorbeeldmail verzenden | E-mails | POST | /rest/asset/v1/email/{id} /sendSample.json | Read-Write-element |
| E-mail niet goedkeuren | E-mails | POST | /rest/asset/v1/email/{id} /unapprove.json | Read-Write-element |
| E-mailinhoud bijwerken | E-mails | POST | /rest/asset/v1/email/{id} /content.json | Read-Write-element |
| Sectie E-mailinhoud bijwerken | E-mails | POST | /rest/asset/v1/email/{id}/content/{htmlId}.json | Read-Write-element |
| Sectie Dynamische inhoud e-mail bijwerken | E-mails | POST | /rest/asset/v1/email/{id}/dynamicContent/{dynamicContentId} .json | Read-Write-element |
| Volledige inhoud e-mailen bijwerken | E-mails | POST | /rest/asset/v1/e-mails/{id} /fullContent.json | Read-Write-element |
| E-mailmetagegevens bijwerken | E-mails | POST | /rest/asset/v1/email/{id}.json | Read-Write-element |
| E-mailvariabele bijwerken | E-mails | POST | /rest/asset/v1/email/{id}/variable/{name}.json | Read-Write-element |
| Bestand maken | Bestanden | POST | /rest/asset/v1/files.json | Read-Write-element |
| Bestand ophalen op id | Bestanden | GET | /rest/asset/v1/file/{id}.json | Alleen-lezen element |
| Bestand op naam ophalen | Bestanden | GET | /rest/asset/v1/file/byName.json | Alleen-lezen element |
| Bestanden ophalen | Bestanden | GET | /rest/asset/v1/files.json | Alleen-lezen element |
| Bestandsinhoud bijwerken | Bestanden | POST | /rest/asset/v1/file/{id} /content.json | Read-Write-element |
| Map maken | Mappen | POST | /rest/asset/v1/folders.json | Read-Write-element |
| Map verwijderen | Mappen | POST | /rest/asset/v1/folder/{id} /delete.json | Read-Write-element |
| Map ophalen op id | Mappen | GET | /rest/asset/v1/folder/{id}.json | Alleen-lezen element |
| Map ophalen op naam | Mappen | GET | /rest/asset/v1/folder/byName.json | Alleen-lezen element |
| Inhoud van map ophalen | Mappen | GET | /rest/asset/v1/folder/{id} /content.json | Alleen-lezen element |
| Mappen ophalen | Mappen | GET | /rest/asset/v1/folders.json | Alleen-lezen element |
| Metagegevens van map bijwerken | Mappen | POST | /rest/asset/v1/folder/{id}.json | Read-Write-element |
| Veld toevoegen aan formulier | Formuliervelden | POST | /rest/asset/v1/form/{id} /fields.json | Read-Write-element |
| Veldset toevoegen aan formulier | Formuliervelden | POST | /rest/asset/v1/form/{id} /fieldSet.json | Read-Write-element |
| Zichtbaarheidsregels voor formuliervelden toevoegen | Formuliervelden | POST | /rest/asset/v1/form/{formId}/field/{fieldId} /visibility.json | Read-Write-element |
| RTF-veld toevoegen | Formuliervelden | POST | /rest/asset/v1/form/{id} /richText.json | Read-Write-element |
| Veld uit veldset verwijderen | Formuliervelden | POST | /rest/asset/v1/form/{id}/fieldSet/{fieldSetId}/field/{fieldId} /delete.json | Read-Write-element |
| Formulierveld verwijderen | Formuliervelden | POST | /rest/asset/v1/form/{id}/field/{fieldId} /delete.json | Read-Write-element |
| Beschikbare formuliervelden ophalen | Formuliervelden | GET | /rest/asset/v1/form/fields.json | Alleen-lezen element |
| Beschikbare velden voor formulierprogrammaleden ophalen | Formuliervelden | GET | /rest/asset/v1/form/programMemberFields.json | Alleen-lezen element |
| Formuliervelden ophalen | Formuliervelden | GET | /rest/asset/v1/form/{id} /fields.json | Alleen-lezen element |
| Veldposities bijwerken | Formuliervelden | POST | /rest/asset/v1/form/{id} /reArrange.json | Read-Write-element |
| Formulierveld bijwerken | Formuliervelden | POST | /rest/asset/v1/form/{id}/field/{fieldId} .json | Read-Write-element |
| Formulierontwerp goedkeuren | Forms | POST | /rest/asset/v1/form/{id} /approveDraft.json | Read-Write-element |
| Kloonformulier | Forms | POST | /rest/asset/v1/form/{id} /clone.json | Read-Write-element |
| Formulier maken | Forms | POST | /rest/asset/v1/forms.json | Read-Write-element |
| Formulier ophalen gebruikt door | Forms | GET | /rest/asset/v1/form/{id} /usedBy.json | Read-Write-element |
| Formulier verwijderen | Forms | POST | /rest/asset/v1/form/{id} /delete.json | Read-Write-element |
| Concept formulier negeren | Forms | POST | /rest/asset/v1/form/{id} /discardDraft.json | Read-Write-element |
| Formulier ophalen op id | Forms | GET | /rest/asset/v1/form/{id}.json | Alleen-lezen element |
| Formulier ophalen op naam | Forms | GET | /rest/asset/v1/form/byName.json | Alleen-lezen element |
| Forms ophalen | Forms | GET | /rest/asset/v1/forms.json | Alleen-lezen element |
| Pagina met dank ophalen op formulier-id | Forms | GET | /rest/asset/v1/form/{id} /thankYouPage.json | Alleen-lezen element |
| Metagegevens van formulier bijwerken | Forms | POST | /rest/asset/v1/form/{id}.json | Read-Write-element |
| Knop Verzenden bijwerken | Forms | POST | /rest/asset/v1/{id} /submitButton.json | Read-Write-element |
| Pagina met bedankt bijwerken | Forms | POST | /rest/asset/v1/form/{id} /thankYouPage.json | Read-Write-element |
| Sectie Landingspagina-inhoud toevoegen | Inhoud bestemmingspagina | POST | /rest/asset/v1/landingPage/{id} /content.json | Read-Write-element |
| Sectie Landingspagina-inhoud verwijderen | Inhoud bestemmingspagina | POST | /rest/asset/v1/landingPage/{id}/content/{contentId} /delete.json | Read-Write-element |
| Inhoud landingspagina ophalen | Inhoud bestemmingspagina | GET | /rest/asset/v1/landingPage/{id} /content.json | Alleen-lezen element |
| Dynamische inhoud van bestemmingspagina ophalen | Inhoud bestemmingspagina | GET | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId} .json | Alleen-lezen element |
| Sectie Landingspagina-inhoud bijwerken | Inhoud bestemmingspagina | POST | /rest/asset/v1/landingPage/{id}/content/{contentId} .json | Read-Write-element |
| Sectie Dynamische inhoud van bestemmingspagina bijwerken | Inhoud bestemmingspagina | POST | /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId} .json | Read-Write-element |
| Concept landingspaginasjabloon goedkeuren | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate/{id} /approveDraft.json | Read-Write-element |
| Landingspagina klonen, sjabloon | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate/{id} /clone.json | Read-Write-element |
| Landingspagina-sjabloon maken | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate.json | Read-Write-element |
| Landingspagina-sjabloon verwijderen | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate/{id} /delete.json | Read-Write-element |
| Concept van sjabloon voor bestemmingspagina negeren | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate/{id} /discardDraft.json | Read-Write-element |
| Landingspagina-sjabloon ophalen op id | Landingspagina-sjablonen | GET | /rest/asset/v1/landingPageTemplate/{id}.json | Alleen-lezen element |
| Landingspagina-sjabloon op naam ophalen | Landingspagina-sjablonen | GET | /rest/asset/v1/landingPageTemplates/byName.json | Alleen-lezen element |
| Inhoud landingspaginasjabloon ophalen | Landingspagina-sjablonen | GET | /rest/asset/v1/landingPageTemplate/{id} /content.json | Alleen-lezen element |
| Landingspaginasjablonen ophalen | Landingspagina-sjablonen | GET | /rest/asset/v1/landingPageTemplates.json | Alleen-lezen element |
| Niet-goedgekeurde sjabloon landingspagina | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate/{id} /unapprove.json | Read-Write-element |
| Inhoud landingspagina bijwerken | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate/{id} /content.json | Read-Write-element |
| Sjabloonmetagegevens bestemmingspagina bijwerken | Landingspagina-sjablonen | POST | /rest/asset/v1/landingPageTemplate/{id}.json | Read-Write-element |
| Concept voor bestemmingspagina goedkeuren | Landingspagina&#39;s | POST | /rest/asset/v1/landingPage/{id} /approveDraft.json | Read-Write-element |
| Openingspagina klonen | Landingspagina&#39;s | POST | /rest/asset/v1/landingPage/{id} /clone.json | Read-Write-element |
| Openingspagina&#39;s maken | Landingspagina&#39;s | POST | /rest/asset/v1/landingPages.json | Read-Write-element |
| Openingspagina verwijderen | Landingspagina&#39;s | POST | /rest/asset/v1/landingPage/{id} /delete.json | Read-Write-element |
| Concept bestemmingspagina negeren | Landingspagina&#39;s | POST | /rest/asset/v1/landingPage/{id} /discardDraft.json | Read-Write-element |
| Openingspagina ophalen op id | Landingspagina&#39;s | GET | /rest/asset/v1/landingPage/{id}.json | Alleen-lezen element |
| Openingspagina ophalen op naam | Landingspagina&#39;s | GET | /rest/asset/v1/landingPage/byName.json | Alleen-lezen element |
| Variabelen voor bestemmingspagina ophalen | Landingspagina&#39;s | GET | /rest/asset/v1/landingPage/{id} /variables.json | Alleen-lezen element |
| Landingspagina&#39;s ophalen | Landingspagina&#39;s | GET | /rest/asset/v1/landingPages.json | Alleen-lezen element |
| Voorvertoning openingspagina | Landingspagina&#39;s | GET | /rest/asset/v1/landingPage/{id} /preview.json | Alleen-lezen element |
| Openingspagina niet goedkeuren | Landingspagina&#39;s | POST | /rest/asset/v1/landingPage/{id} /unapprove.json | Read-Write-element |
| Metagegevens bestemmingspagina bijwerken | Landingspagina&#39;s | POST | /rest/asset/v1/{id}.json | Read-Write-element |
| Variabelen voor bestemmingspagina bijwerken | Landingspagina&#39;s | POST | /rest/asset/v1/landingPage/{id}/variable/{variableId} .json | Read-Write-element |
| Regels voor omleiding bestemmingspagina maken | Landingspagina&#39;s | POST | /rest/asset/v1/redirectRules.json | Leesschrijven omleidingsregels |
| Regel voor omleiding van bestemmingspagina verwijderen | Landingspagina&#39;s | POST | /rest/asset/v1/redirectRule/{id} /delete.json | Leesschrijven omleidingsregels |
| Regels voor omleiding van bestemmingspagina ophalen | Landingspagina&#39;s | GET | /rest/asset/v1/redirectRules.json | Regels voor alleen-lezen omleiden |
| Regel voor omleiding van bestemmingspagina ophalen op id | Landingspagina&#39;s | GET | /rest/asset/v1/redirectRule/{id}.json | Regels voor alleen-lezen omleiden |
| Regel voor omleiding van bestemmingspagina bijwerken | Landingspagina&#39;s | POST | /rest/asset/v1/redirectRule/{id}.json | Leesschrijven omleidingsregels |
| Domeinen landingspagina&#39;s ophalen | Landingspagina&#39;s | GET | /rest/asset/v1/landingPageDomains.json | Regels voor alleen-lezen omleiden |
| Associate Lead | Leads | POST | /rest/v1/leads/{id} /associate.json | Lead lezen |
| Status van lead-programma wijzigen | Leads | POST | /rest/v1/leads/programs/{programId} /status.json | Lead lezen |
| Leads verwijderen | Leads | POST | /rest/v1/leads.json | Lead lezen |
| Lead beschrijven | Leads | GET | /rest/v1/leads/describe.json | Lead, alleen-lezen |
| Lead2 beschrijven | Leads | GET | /rest/v1/leads/describe2.json | Lead, alleen-lezen |
| Lid van programma beschrijven | Leads | GET | /rest/v1/program/members/describe.json | Lead, alleen-lezen |
| Lead ophalen op id | Leads | GET | /rest/v1/lead/{id}.json | Lead, alleen-lezen |
| Regelafstand ophalen | Leads | GET | /rest/v1/leads/partitions.json | Lead, alleen-lezen |
| Regels ophalen op filtertype | Leads | GET | /rest/v1/leads.json | Lead, alleen-lezen |
| Leads ophalen op basis van programma-id | Leads | GET | /rest/v1/lead/programs/{programId}.json | Lead, alleen-lezen |
| Leads samenvoegen | Leads | POST | /rest/v1/leads/{id} /merge.json | Lead lezen |
| Lijsten ophalen op hoofd-id | Leads | GET | /rest/v1/lead/{leadId}.json | Alleen-lezen element |
| Programma&#39;s ophalen op regel-id | Leads | GET | /rest/v1/leads/{leadId} programMembership.json | Alleen-lezen element |
| Slimme campagnes ophalen op hoofd-id | Leads | GET | /rest/v1/leads/{leadId} /smartCampaignMembership.json | Alleen-lezen element |
| Regelafstand naar Marketo | Leads | POST | /rest/v1/leads/partitions.json | Lead lezen |
| Formulier verzenden | Leads | POST | /rest/v1/leads/submitForm.json | Lead lezen |
| Leads synchroniseren | Leads | POST | /rest/v1/leads.json | Lead lezen |
| Loodverdeling bijwerken | Leads | POST | /rest/v1/leads/partitions.json | Lead lezen |
| Leadveld op naam ophalen | Leads | GET | /rest/v1/lead/schema/fields/{fieldApiName}.json | Aangepast veld schema voor lezen/schrijven |
| Velden met leads ophalen | Leads | GET | /rest/v1/leads/schema/fields.json | Aangepast veld schema voor lezen/schrijven |
| Velden voor lead maken | Leads | POST | /rest/v1/leads/schema/fields.json | Aangepast veld schema voor lezen/schrijven |
| Leadveld bijwerken | Leads | POST | /rest/v1/lead/schema/fields/{fieldApiName}.json | Aangepast veld schema voor lezen/schrijven |
| Benoemde accountlijstleden toevoegen | Lijsten met benoemde accounts | POST | /rest/v1/namedaccountlist/{id} /namedaccounts.json | Benoemd account voor lezen/schrijven |
| Lijsten met benoemde accounts verwijderen | Lijsten met benoemde accounts | POST | /rest/v1/namedaccountlists/delete.json | Benoemde accountlijst lezen |
| Benoemde accountlijstleden ophalen | Lijsten met benoemde accounts | GET | /rest/v1/namedaccountlist/{id} /namedaccounts.json | Benoemd account (alleen-lezen) |
| Lijsten met benoemde accounts ophalen | Lijsten met benoemde accounts | GET | /rest/v1/namedaccountlists.json | Benoemde accountlijst, alleen-lezen |
| Benoemde accountlijstleden verwijderen | Lijsten met benoemde accounts | POST | /rest/v1/namedaccountlist/{id} /namedaccounts/remove.json | Benoemd account voor lezen/schrijven |
| Lijsten met benoemde accounts synchroniseren | Lijsten met benoemde accounts | POST | /rest/v1/namedaccountlists.json | Benoemde accountlijst lezen |
| Benoemde accounts verwijderen | Benoemde accounts | POST | /rest/v1/namedaccounts/delete.json | Benoemd account voor lezen/schrijven |
| Benoemde accounts beschrijven | Benoemde accounts | GET | /rest/v1/namedaccounts/describe.json | Benoemd account (alleen-lezen) |
| Benoemde accounts ophalen | Benoemde accounts | GET | /rest/v1/namedaccounts.json | Benoemd account (alleen-lezen) |
| Benoemde accounts synchroniseren | Benoemde accounts | POST | /rest/v1/namedaccounts.json | Benoemd account voor lezen/schrijven |
| Veld voor benoemde account ophalen op naam | Benoemde accounts | GET | /rest/v1/namedaccounts/schema/fields/{0.json{fieldApiName} | Aangepast veld schema voor lezen/schrijven |
| Benoemde accountvelden ophalen | Benoemde accounts | GET | /rest/v1/namedaccounts/schema/fields.json | Aangepast veld schema voor lezen/schrijven |
| Kansen verwijderen | Kansen | POST | /rest/v1/opportunities/delete.json | Opportunity voor lezen/schrijven |
| Opportuniteitsrollen verwijderen | Kansen | POST | /rest/v1/opportunities/roles/delete.json | Opportunity voor lezen/schrijven |
| Opportunity beschrijven | Kansen | GET | /rest/v1/opportunities/describe.json | Opportunity, alleen-lezen |
| Beschrijf opportuniteitsrol | Kansen | GET | /rest/v1/opportunities/roles/describe.json | Opportunity, alleen-lezen |
| Opportuniteiten ophalen | Kansen | GET | /rest/v1/opportunities.json | Opportunity, alleen-lezen |
| Opportunity-rollen ophalen | Kansen | GET | /rest/v1/opportunities/roles.json | Opportunity, alleen-lezen |
| Synchronisatiemogelijkheden | Kansen | POST | /rest/v1/opportunities.json | Opportunity voor lezen/schrijven |
| Opportuniteitsrollen synchroniseren | Kansen | POST | /rest/v1/opportunities/roles.json | Opportunity voor lezen/schrijven |
| Opportunity-veld op naam ophalen | Kansen | GET | /rest/v1/Opportunity/schema/fields/{fieldApiName}.json | Aangepast veld schema voor lezen/schrijven |
| Opportunity-velden ophalen | Kansen | GET | /rest/v1/opportunities/schema/fields.json | Aangepast veld schema voor lezen/schrijven |
| Programmeleden verwijderen | Programmaleden | POST | /rest/v1/programs/{programId} /members/delete.json | Lead lezen |
| Lid van programma beschrijven | Programmaleden | GET | /rest/v1/programs/members/describe.json | Lead, alleen-lezen |
| Programmaleden ophalen | Programmaleden | GET | /rest/v1/programs/{programId} /members.json | Lead, alleen-lezen |
| Gegevens programmalid synchroniseren | Programmaleden | POST | /rest/v1/programs/{programId} /members.json | Lead lezen |
| Status van programmalid synchroniseren | Programmaleden | POST | /rest/v1/programs/{programId} /members/status.json | Lead lezen |
| Veld voor programmalid ophalen op naam | Programmaleden | GET | /rest/v1/program/members/schema/fields/{0.json{fieldApiName} | Aangepast veld schema voor lezen/schrijven |
| Veld voor programmalid ophalen | Programmaleden | GET | /rest/v1/programs/members/schema/fields.json | Aangepast veld schema voor lezen/schrijven |
| Veld voor programmaleden maken | Programmaleden | POST | /rest/v1/programs/members/schema/fields.json | Aangepast veld schema voor lezen/schrijven |
| Veld van programmalid bijwerken | Programmaleden | POST | /rest/v1/program/members/schema/fields/{0.json{fieldApiName} | Aangepast veld schema voor lezen/schrijven |
| Programma goedkeuren | Programma&#39;s | POST | /rest/asset/v1/program/{id} /approve.json | Read-Write-element |
| Kloonprogramma | Programma&#39;s | POST | /rest/asset/v1/program/{id} /clone.json | Read-Write-element |
| Programma&#39;s maken | Programma&#39;s | POST | /rest/asset/v1/programs.json | Read-Write-element |
| Programma verwijderen | Programma&#39;s | POST | /rest/asset/v1/program/{id} /delete.json | Read-Write-element |
| Programma ophalen op id | Programma&#39;s | GET | /rest/asset/v1/program/{id}.json | Alleen-lezen element |
| Programma ophalen op naam | Programma&#39;s | GET | /rest/asset/v1/program/byName.json | Alleen-lezen element |
| Programma&#39;s ophalen | Programma&#39;s | GET | /rest/asset/v1/programs.json | Alleen-lezen element |
| Programma&#39;s ophalen op tag | Programma&#39;s | GET | /rest/asset/v1/program/byTag.json | Alleen-lezen element |
| Slimme lijst ophalen op basis van programma-id | Programma&#39;s | GET | /rest/asset/v1/program/{id} /smartList.json | Alleen-lezen element |
| Programma niet goedkeuren | Programma&#39;s | POST | /rest/asset/v1/program/{id} /unapprove.json | Read-Write-element |
| Programmametagegevens bijwerken | Programma&#39;s | POST | /rest/asset/v1/program/{id}.json | Read-Write-element |
| Programmatag bijwerken | Programma&#39;s | POST | /rest/asset/v1/program/{id}/tag/{tagType} .json | Read-Write-element |
| Programmatag verwijderen | Programma&#39;s | POST | /rest/asset/v1/program/{id}/tag/{tagType} /delete.json | Read-Write-element |
| Verkoopmedewerkers verwijderen | Verkopers | POST | /rest/v1/salespersons/delete.json | Verkooppersoon voor lezen/schrijven |
| Beschrijf verkooppersonen | Verkopers | GET | /rest/v1/salespersons/describe.json | Alleen-lezen verkoper |
| Verkoopmedewerkers ophalen | Verkopers | GET | /rest/v1/salespersons.json | Alleen-lezen verkoper |
| Verkoopmedewerkers synchroniseren | Verkopers | POST | /rest/v1/salespersons.json | Verkooppersoon voor lezen/schrijven |
| Segmentaties ophalen | Segmenten | GET | /rest/asset/v1/segmentation.json | Alleen-lezen element |
| Segmenten ophalen voor segmentaties | Segmenten | GET | /rest/asset/v1/segmentation/{id} /segments.json | Alleen-lezen element |
| Slimme campagne activeren | Slimme campagnes | POST | /rest/asset/v1/smartCampaign/{id} /activate.json | Campagne activeren |
| Slimme klonen | Slimme campagnes | POST | /rest/asset/v1/smartCampaign/{id} /clone.json | Read-Write-element |
| Slimme campagne maken | Slimme campagnes | POST | /rest/asset/v1/smartCampaigns.json | Read-Write-element |
| Slimme campagne deactiveren | Slimme campagnes | POST | /rest/asset/v1/smartCampaign/{id} /deactivate.json | Campagne deactiveren |
| Slimme campagne verwijderen | Slimme campagnes | POST | /rest/asset/v1/smartCampaign/{id} /delete.json | Read-Write-element |
| Slimme campagnes ophalen | Slimme campagnes | GET | /rest/asset/v1/smartCampaigns.json | Alleen-lezen element |
| Slimme campagne ophalen op id | Slimme campagnes | GET | /rest/asset/v1/smartCampaign/{id}.json | Alleen-lezen element |
| Slimme campagne op naam ophalen | Slimme campagnes | GET | /rest/asset/v1/smartCampaign/byName.json | Alleen-lezen element |
| Slimme lijst ophalen met slimme-campagne-id | Slimme campagnes | GET | /rest/asset/v1/smartCampaign/{id} /smartList.json | Alleen-lezen element |
| Slimme campagne bijwerken | Slimme campagnes | POST | /rest/asset/v1/smartCampaign/{id}.json | Read-Write-element |
| Slimme lijst klonen | Slimme lijsten | POST | /rest/asset/v1/smartList/{id} /clone.json | Read-Write-element |
| Slimme lijst verwijderen | Slimme lijsten | POST | /rest/asset/v1/smartList/{id} /delete.json | Read-Write-element |
| Slimme lijst ophalen op id | Slimme lijsten | GET | /rest/asset/v1/smartList/{id}.json | Alleen-lezen element |
| Slimme lijst op naam ophalen | Slimme lijsten | GET | /rest/asset/v1/smartList/byName.json | Alleen-lezen element |
| Slimme lijsten ophalen | Slimme lijsten | GET | /rest/asset/v1/smartLists.json | Alleen-lezen element |
| Concept van fragment goedkeuren | Fragmenten | POST | /rest/asset/v1/snippet/{id} /approveDraft.json | Read-Write-element |
| Kloonfragment | Fragmenten | POST | /rest/asset/v1/snippet/{id} /clone.json | Read-Write-element |
| Fragment maken | Fragmenten | POST | /rest/asset/v1/snippets.json | Read-Write-element |
| Fragment verwijderen | Fragmenten | POST | /rest/asset/v1/snippet/{id} /delete.json | Read-Write-element |
| Concept fragment negeren | Fragmenten | POST | /rest/asset/v1/snippet/{id} /discardDraft.json | Read-Write-element |
| Dynamische inhoud ophalen | Fragmenten | GET | /rest/asset/v1/snippet/{id} /dynamicContent.json | Alleen-lezen element |
| Fragment ophalen op id | Fragmenten | GET | /rest/asset/v1/snippet/{id}.json | Alleen-lezen element |
| Fragmentinhoud ophalen | Fragmenten | GET | /rest/asset/v1/snippet/{id} /content.json | Alleen-lezen element |
| Fragmenten ophalen | Fragmenten | GET | /rest/asset/v1/snippets.json | Alleen-lezen element |
| Fragment niet goedkeuren | Fragmenten | POST | /rest/asset/v1/snippet/{id} /unapprove.json | Read-Write-element |
| Fragmentinhoud bijwerken | Fragmenten | POST | /rest/asset/v1/snippet/{id} /content.json | Read-Write-element |
| Dynamische inhoud van fragment bijwerken | Fragmenten | POST | /rest/asset/v1/snippet/{id}/dynamicContent/{segmentId} .json | Read-Write-element |
| Metagegevens van fragment bijwerken | Fragmenten | POST | /rest/asset/v1/snippet/{id}.json | Read-Write-element |
| Toevoegen aan lijst | Statische lijsten | POST | /rest/v1/lists/{listId} /leads.json | Lead lezen |
| Statische lijst maken | Statische lijsten | POST | /asset/v1/staticLists.json | Read-Write-element |
| Statische lijst verwijderen | Statische lijsten | POST | /asset/v1/staticList/{id} /delete.json | Read-Write-element |
| Leden ophalen op lijst-id | Statische lijsten | GET | /rest/v1/lists/{listId} /leads.json | Lead, alleen-lezen |
| Lijst ophalen op id | Statische lijsten | GET | /rest/v1/lists/{id}.json | Lead, alleen-lezen |
| Lijsten ophalen | Statische lijsten | GET | /rest/v1/lists.json | Lead, alleen-lezen |
| Statische lijst ophalen op id | Statische lijsten | GET | /asset/v1/staticList/{id}.json | Alleen-lezen element |
| Statische lijst ophalen op naam | Statische lijsten | GET | /asset/v1/staticList/byName.json | Alleen-lezen element |
| Statische lijsten ophalen | Statische lijsten | GET | /asset/v1/staticLists.json | Alleen-lezen element |
| Lid van de lijst | Statische lijsten | GET | /rest/v1/lists/{listId} /leads/ismember.json | Lead, alleen-lezen |
| Verwijderen uit lijst | Statische lijsten | DELETE | /rest/v1/lists/{listId} /leads.json | Lead lezen |
| Metagegevens statische lijst bijwerken | Statische lijsten | POST | /asset/v1/staticList/{id}.json | Read-Write-element |
| Tag op naam ophalen | Tags | GET | /rest/asset/v1/tagType/byName.json | Alleen-lezen element |
| Tagtypen ophalen | Tags | GET | /rest/asset/v1/tagTypes.json | Alleen-lezen element |
| Token maken | Tokens | POST | /rest/asset/v1/folder/{id} /tokens.json | Read-Write-element |
| Token op naam verwijderen | Tokens | POST | /rest/asset/v1/folder/{id} /tokens/delete.json | Read-Write-element |
| Tokens ophalen op basis van map-id | Tokens | GET | /rest/asset/v1/folder/{id} /tokens.json | Alleen-lezen element |
| Rollen toevoegen | Gebruikersbeheer | POST | /userservice/management/v1/users/{userid} /roles/create.json | Toegang tot API voor gebruikersbeheer |
| Uitgenodigde gebruiker verwijderen | Gebruikersbeheer | POST | /userservice/management/v1/users/{userId} /invite/delete.json | Toegang tot API voor gebruikersbeheer |
| Rollen verwijderen | Gebruikersbeheer | POST | /userservice/management/v1/users/{userid} /roles/delete.json | Toegang tot API voor gebruikersbeheer |
| Gebruiker verwijderen | Gebruikersbeheer | POST | /userservice/management/v1/users/{userId} /delete.json | Toegang tot API voor gebruikersbeheer |
| Uitgenodigde gebruiker ophalen op gebruikersnaam | Gebruikersbeheer | GET | /userservice/management/v1/users/{userid} /invite.json | Toegang tot API voor gebruikersbeheer |
| Rollen ophalen | Gebruikersbeheer | GET | /userservice/management/v1/users/roles.json | Toegang tot API voor gebruikersbeheer |
| Rollen en werkruimten ophalen op id | Gebruikersbeheer | GET | /userservice/management/v1/users/{userid} /roles.json | Toegang tot API voor gebruikersbeheer |
| Gebruikers ophalen | Gebruikersbeheer | GET | /userservice/management/v1/users/allusers.json | Toegang tot API voor gebruikersbeheer |
| Gebruiker ophalen op gebruikersnaam | Gebruikersbeheer | GET | /userservice/management/v1/users/{userid} /user.json | Toegang tot API voor gebruikersbeheer |
| Werkruimten ophalen | Gebruikersbeheer | GET | /userservice/management/v1/users/workspaces.json | Toegang tot API voor gebruikersbeheer |
| Gebruiker uitnodigen | Gebruikersbeheer | POST | /userservice/management/v1/users/invite.json | Toegang tot API voor gebruikersbeheer |
| Gebruikerskenmerken bijwerken | Gebruikersbeheer | POST | /userservice/management/v1/users/{userId} /update.json | Toegang tot API voor gebruikersbeheer |