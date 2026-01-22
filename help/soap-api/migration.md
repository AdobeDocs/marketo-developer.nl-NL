---
title: Migreren naar REST API
feature: SOAP
description: Stap-voor-stap gids om Marketo Engage van SOAP aan REST tegen 31 te migreren, 2026, met eindpuntafbeeldingen, OAuth, lood synchronisatiemethodes, en verwijzingsarchitecturen.
exl-id: c2956db3-defe-4163-99f3-58654ce8ee2b
source-git-commit: 5881ab969eca3a37d19f56b6570e42828994eff3
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 0%

---

# Migreren naar REST API

De Marketo Engage SOAP API wordt na 31 maart 2026 gepensioneerd. Alle bestaande integratie die SOAP API gebruiken zou moeten worden gepensioneerd of aan [&#x200B; de REST API van Marketo Engage &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/rest-api) tegen deze datum gemigreerd om onderbrekingen in de dienst te vermijden.

## Migratie

SOAP API steunt een beperkte waaier van gebruiksgevallen vergeleken bij [&#x200B; REST AP &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/rest-api) I. Wanneer het bepalen van welke eindpunten om uw gebruiksgevallen in kaart te brengen, zou u [&#x200B; Beste praktijken van de Integratie van Marketo &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/marketo-integration-best-practices) moeten volgen

[&#x200B; Architecturen van de Verwijzing &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/reference-architectures) zijn beschikbaar voor [&#x200B; de Synchronisatie van CRM &#x200B;](https://experienceleague.adobe.com/docs/marketo-developer/assets/sync-architecture-whitepaper.pdf?lang=en) en [&#x200B; de 5&rbrace; gebruiksgevallen van de Uitvoer van Data Warehouse.](https://experienceleague.adobe.com/docs/marketo-developer/assets/reference_architecture.pdf?lang=en)

## Verificatie

[&#x200B; Documentatie van de Authentificatie &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/authentication)

De Marketo REST-API gebruikt verificatie op basis van OAuth 2.0 met het toekenningstype Client Credentials. Toegangstokens zijn een uur geldig na het maken.

## Leads

[&#x200B; Lood API Documentatie &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads)

SOAP API steunt loodgegevenssynchronisatie, [&#x200B; de koekjesvereniging van Munchkin &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/javascriptapi/leadtracking/lead-tracking), en lood het samenvoegen. Als uw toepassing de methode SOAP syncLead aanroept en de parameter `marketoCookie` instelt, kunt u migreren door:

1. Gebruikend de [&#x200B; Synchronisatie leidt &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) REST methode, die door [&#x200B; wordt gevolgd Geassocieerde Lood &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)
2. U kunt [&#x200B; roepen voorlegt Vorm &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads), hoewel dit configuratie van sommige Marketing Assets en wat interactie met [&#x200B; Forms API &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/forms) vereist

De toepassingen die het `foreignSysPersonId` zeer belangrijke type gebruiken, zouden aan het gebruiken van een gebied van het douaneleider moeten migreren om dit externe herkenningsteken te vertegenwoordigen, en of [&#x200B; gebruiken leiden van de Synchronisatie &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads#create-and-update) of [&#x200B; Bulk de Invoer van het Lood &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) Methoden van de WEERSTING.

| SOAP-methode | REST-methode(n) |
| --- | --- |
| [&#x200B; getLead &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/getlead) | [&#x200B; krijgt Lood door identiteitskaart &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [&#x200B; krijgt Leidingen door het Type van Filter &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) |
| [&#x200B; getMultipleLeads &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/getmultipleleads) | [&#x200B; krijgt Lood door identiteitskaart &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [&#x200B; krijgen Leidingen door het Type van Filter &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), [&#x200B; krijgen Leidingen door identiteitskaart van het Programma &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByProgramIdUsingGET), [&#x200B; krijgen Leidingen door identiteitskaart van de Lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByListIdUsingGET), [&#x200B; BulkLood de Uitvoer van de Lood &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads) |
| [&#x200B; mergeLeads &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/mergeleads) | [&#x200B; de Leads van de Fusie &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) |
| [&#x200B; syncLead &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/synclead) | [&#x200B; &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) voorloops van de 1&rbrace; Synchronisatie [&#x200B; &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) Verwante Lood [&#128279;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST) |
| [&#x200B; syncMultipleLeads &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/syncmultipleleads) | [&#x200B; de Synchronisatie leidt &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [&#x200B; BulkInvoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |

## M-objecten

M de Voorwerpen was een catch-all concept om de uitvoer van de gegevens van de Attributie van de Kans voor externe analyse te steunen, en met drie objecten types te werken: Kansen, de Rollen van de Kans, en Programma&#39;s.

REST-documentatie:

- [&#x200B; Kans &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/opportunities)
- [&#x200B; Rollen &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/opportunity-roles)
- [&#x200B; Programma&#39;s &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/programs)

| SOAP-methode | REST-methode(n) |
| --- | --- |
| [&#x200B; deleteMObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/deletemobjects) | [&#x200B; de Kansen van de Schrapping &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST), [&#x200B; de Rollen van de Kanaal van de Schrapping &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunityRolesUsingPOST) |
| [&#x200B; describeMObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/describemobject) | [&#x200B; beschrijf Kans &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_4), [&#x200B; beschrijf de Rol van de Kans &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [&#x200B; getMObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/getmobjects) | [&#x200B; krijgt Kansen &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getOpportunitiesUsingGET), [&#x200B; krijgen de Rollen van de Kanaal &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [&#x200B; listMObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/listmobjects) | NVT |
| [&#x200B; syncMObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/syncmobjects) | [&#x200B; Kansen van de Synchronisatie &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunitiesUsingPOST), [&#x200B; de Rollen van de Kanaal van de Synchronisatie &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunityRolesUsingPOST) |
| [&#x200B; getChannels &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/programs/getchannels) | [&#x200B; krijgt Kanalen &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllChannelsUsingGET) |
| [&#x200B; getTags &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/programs/gettags) | [&#x200B; krijgt de Types van Markering &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagTypesUsingGET), [&#x200B; krijgen Markering door Naam &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagByNameUsingGET) |

## Statische lijsten

De statische gevallen van het Gebruik van de Lijst in SOAP API zijn beperkt tot het opnemen van lidmaatschap en loodgegevens, en verwijdering van lidmaatschap dat met [&#x200B; kan worden verwezenlijkt voeg aan Lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) toe, [&#x200B; Bulk de Leads van de Invoer &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import), of [&#x200B; verwijder uit Lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) methodes REST.

| SOAP-methode | REST-methode(n) |
| --- | --- |
| [&#x200B; getImportToListStatus &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/getimporttoliststatus) | [&#x200B; Bulk de Leads van de Invoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [&#x200B; importToList &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/importtolist) | [&#x200B; voeg aan lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) toe [&#x200B; Bulk de Leads van de Invoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [&#x200B; listOperation &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/listoperation) | [&#x200B; verwijder uit Lijst &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) |

## Activiteiten

De SOAP API ondersteunt alleen het ophalen van activiteiten.

REST-documentatie:

- [&#x200B; Synchrone Activiteiten &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/activities)
- [&#x200B; Bulk Uittreksel van de Activiteit van de Activiteit &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-activity-extract)

| SOAP-methode | REST-methode(n) |
| --- | --- |
| [&#x200B; getLeadActivity &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/activities/getleadactivity) | [&#x200B; &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) krijgt de BulkActiviteiten van de Uitvoer [&#128279;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) |
| [&#x200B; getLeadChanges &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/activities/getleadchanges) | [&#x200B; &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) krijgt de BulkActiviteiten van de Uitvoer &lbrace;de veranderingen van de Lood [&#128279;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadChangesUsingGET) |

## Campagnes

REST-documentatie:

- [&#x200B; Slimme Campagnes &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns)

SOAP API steunt slechts drie gebruiksgevallen voor slimme campagnes: [&#x200B; het teweegbrengen leidt tot kwalificeren voor een Vereiste Slimme Campagne &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns#trigger), die die Vereiste Campagnes terugwinnen, en [&#x200B; plannend een Toekomstige Looppas van een Slimme Campagne &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns#schedule).

| SOAP-methode | REST-methode(n) |
| --- | --- |
| [&#x200B; getCampaignsForSource &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/getcampaignsforsource) | [&#x200B; krijgt Slimme Campagnes &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllSmartCampaignsGET) |
| [&#x200B; requestCampaign &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/requestcampaign) | [&#x200B; Campagne van het Verzoek &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) |
| [&#x200B; planningCampaign &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/schedulecampaign) | [&#x200B; Campagne van het Programma &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) |

## Aangepaste objecten

REST-documentatie:

- [&#x200B; de Voorwerpen van de Douane &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/custom-objects)

De SOAP API ondersteunt alleen CRUD-bewerkingen voor aangepaste objecten.

| SOAP-methode | REST-methode(n) |
| --- | --- |
| [&#x200B; deleteCustomObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/deletecustomobjects) | [&#x200B; Schrap de Douane Voorwerpen &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteCustomObjectsUsingPOST) |
| [&#x200B; getCustomObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/getcustomobjects) | [&#x200B; krijgt de Voorwerpen van de Douane &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCustomObjectsUsingGET) |
| [&#x200B; syncCustomObjects &#x200B;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/synccustomobjects) | [&#x200B; &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncCustomObjectsUsingPOST) Bulk de Voorwerp van de Invoer van de Douane van 0&rbrace; de Douane van de Synchronisatie [&#128279;](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import) |
