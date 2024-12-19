---
title: Migreren naar REST API
feature: SOAP
description: Migreren van SOAP naar REST API's
source-git-commit: 4c410afbe8ccfff644376670b8990fac671459eb
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---


# Migreren naar REST API

De Marketo Engage SOAP API wordt na 31 oktober 2025 ingetrokken. Alle bestaande integratie die SOAP API gebruiken zou moeten worden gepensioneerd of aan [ Marketo Engage REST API ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/rest-api) tegen deze datum gemigreerd om onderbrekingen in de dienst te vermijden.

## Migratie

De SOAP API steunt een beperkte waaier van gebruiksgevallen vergeleken bij [ REST AP ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/rest-api) I. Wanneer het bepalen van welke eindpunten om uw gebruiksgevallen in kaart te brengen, zou u [ Beste praktijken van de Integratie van Marketo ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/marketo-integration-best-practices) moeten volgen

](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/reference-architectures) de Architecturen van de Verwijzing [ zijn beschikbaar voor [ Synchronisatie van CRM ](https://experienceleague.adobe.com/docs/marketo-developer/assets/sync-architecture-whitepaper.pdf?lang=en) en [ het gebruiksgevallen van de Uitvoer van de Data Warehouse ](https://experienceleague.adobe.com/docs/marketo-developer/assets/reference_architecture.pdf?lang=en).

## Verificatie

[ Documentatie van de Authentificatie ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/authentication)

De Marketo REST-API gebruikt verificatie op basis van OAuth 2.0 met het toekenningstype Client Credentials. Toegangstokens zijn een uur geldig na het maken.

## Leads

[ Lood API Documentatie ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads)

SOAP API steunt de synchronisatie van loodgegevens, [ de koekjesvereniging van Munchkin ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/javascriptapi/leadtracking/lead-tracking), en lood het samenvoegen. Als uw toepassing de methode SOAP syncLead aanroept en de parameter `marketoCookie` instelt, kunt u migreren door:

1. Gebruikend de [ Synchronisatie leidt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) REST methode, die door [ wordt gevolgd Geassocieerde Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)
2. U kunt [ roepen voorlegt Vorm ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads&quot;%20\l%20&quot;submit-form), hoewel dit configuratie van sommige Marketing Assets en wat interactie met [ Forms API ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/forms) vereist

De toepassingen die het `foreignSysPersonId` zeer belangrijke type gebruiken, zouden aan het gebruiken van een gebied van het douaneleider moeten migreren om dit externe herkenningsteken te vertegenwoordigen, en of [ gebruiken leiden van de Synchronisatie ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/leads#create-and-update) of [ Bulk de Invoer van het Lood ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import) Methoden van de WEERSTING.

| SOAP | REST-methode(n) |
| --- | --- |
| [ getLead ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/getlead) | [ krijgt Lood door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [ krijgt Leidingen door het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) |
| [ getMultipleLeads ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/getmultipleleads) | [ krijgt Lood door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET), [ krijgen Leidingen door het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), [ krijgen Leidingen door identiteitskaart van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByProgramIdUsingGET), [ krijgen Leidingen door identiteitskaart van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByListIdUsingGET), [ BulkLood de Uitvoer van de Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads) |
| [ mergeLeads ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/mergeleads) | [ de Leads van de Fusie ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) |
| [ syncLead ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/synclead) | ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [ voorloops van de 1} Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) [ Verwante Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/associateLeadUsingPOST)[ |
| [ syncMultipleLeads ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/leads/syncmultipleleads) | [ de Synchronisatie leidt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) [ BulkInvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |

## M-objecten

M de Voorwerpen was een catch-all concept om de uitvoer van de gegevens van de Attributie van de Kans voor externe analyse te steunen, en met drie objecten types te werken: Kansen, de Rollen van de Kans, en Programma&#39;s.

REST-documentatie:

- [ Kans ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/opportunities)
- [ Rollen ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/opportunity-roles)
- [ Programma&#39;s ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/programs)

| SOAP | REST-methode(n) |
| --- | --- |
| [ deleteMObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/deletemobjects) | [ de Kansen van de Schrapping ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST), [ de Rollen van de Kanaal van de Schrapping ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunityRolesUsingPOST) |
| [ describeMObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/describemobject) | [ beschrijf Kans ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_4), [ beschrijf de Rol van de Kans ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [ getMObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/getmobjects) | [ krijgt Kansen ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getOpportunitiesUsingGET), [ krijgen de Rollen van de Kanaal ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeOpportunityRoleUsingGET) |
| [ listMObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/listmobjects) | NVT |
| [ syncMObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/marketo-objects/syncmobjects) | [ Kansen van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunitiesUsingPOST), [ de Rollen van de Kanaal van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncOpportunityRolesUsingPOST) |
| [ getChannels ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/programs/getchannels) | [ krijgt Kanalen ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllChannelsUsingGET) |
| [ getTags ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/programs/gettags) | [ krijgt de Types van Markering ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagTypesUsingGET), [ krijgen Markering door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getTagByNameUsingGET) |

## Statische lijsten

De statische gevallen van het Gebruik van de Lijst in SOAP API zijn beperkt tot opname van lidmaatschap en loodgegevens, en verwijdering van lidmaatschap dat met [ kan worden verwezenlijkt voeg aan Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) toe, [ Bulk de Hangt van de Invoer ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-lead-import), of [ verwijdert uit Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) de Methoden van de REST.

| SOAP | REST-methode(n) |
| --- | --- |
| [ getImportToListStatus ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/getimporttoliststatus) | [ Bulk de Leads van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [ importToList ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/importtolist) | [ voeg aan lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addLeadsToListUsingPOST) toe [ Bulk de Leads van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads) |
| [ listOperation ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/static-lists/listoperation) | [ verwijder uit Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE) |

## Activiteiten

De SOAP API ondersteunt alleen het ophalen van activiteiten.

REST-documentatie:

- [ Synchrone Activiteiten ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/activities)
- [ Bulk Uittreksel van de Activiteit van de Activiteit ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-extract/bulk-activity-extract)

| SOAP | REST-methode(n) |
| --- | --- |
| [ getLeadActivity ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/activities/getleadactivity) | ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [ krijgt de BulkActiviteiten van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET)[ |
| [ getLeadChanges ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/activities/getleadchanges) | ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities) [ krijgt de BulkActiviteiten van de Uitvoer {de veranderingen van de Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadChangesUsingGET)[ |

## Campagnes

REST-documentatie:

- [ Slimme Campagnes ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns&quot;%20\h%20HYPERLINK%20&quot;https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns)

SOAP API steunt slechts drie gebruiksgevallen voor slimme campagnes: [ het teweegbrengen leidt ertoe om voor een Vereiste Slimme Campagne ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns#trigger) te kwalificeren, die Vereiste Campagnes terugwinnen, en [ plannend een Toekomstige Looppas van een Slimme Campagne ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/assets/smart-campaigns#schedule).

| SOAP | REST-methode(n) |
| --- | --- |
| [ getCampaignsForSource ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/getcampaignsforsource) | [ krijgt Slimme Campagnes ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllSmartCampaignsGET) |
| [ requestCampaign ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/requestcampaign) | [ Campagne van het Verzoek ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) |
| [ planningCampaign ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/campaigns/schedulecampaign) | [ Campagne van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) |

## Aangepaste objecten

REST-documentatie:

- [ de Voorwerpen van de Douane ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/custom-objects&quot;%20\h%20HYPERLINK%20&quot;https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/lead-database/custom-objects)

De SOAP-API ondersteunt alleen CRUD-bewerkingen voor aangepaste objecten.

| SOAP | REST-methode(n) |
| --- | --- |
| [ deleteCustomObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/deletecustomobjects) | [ Schrap de Douane Voorwerpen ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteCustomObjectsUsingPOST) |
| [ getCustomObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/getcustomobjects) | [ krijgt de Voorwerpen van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCustomObjectsUsingGET) |
| [ syncCustomObjects ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/soap/custom-objects/synccustomobjects) | ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncCustomObjectsUsingPOST) [ Bulk de Voorwerp van de Invoer van de Douane van 0} de Douane van de Synchronisatie ](https://experienceleague.adobe.com/en/docs/marketo-developer/marketo/rest/bulk-import/bulk-custom-object-import)[ |