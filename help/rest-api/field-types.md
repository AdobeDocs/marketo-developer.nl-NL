---
title: Veldtypen
feature: REST API
description: Een lijst met Marketo-veldtypen
exl-id: a0ba9e02-ed42-4be3-9cdd-a97fee9a726e
source-git-commit: fc9b9037986a35036dbd909339f59bd33aa67e71
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 2%

---

# Veldtypen

Hier volgt een beschrijving van veldtypen in Marketo. De extra informatie over gebiedstypes kan [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/custom-field-type-glossary) worden gevonden. De extra informatie over gebiedstype grenzen kan [ hier ](https://nation.marketo.com/t5/knowledgebase/marketo-field-limits-by-field-type/ta-p/251613) worden gevonden.

| Veldtype | Beschrijving | Voorbeeld |
| --- | --- | --- |
| Datumtijd | Wordt gebruikt voor het invoeren van een datum en tijd. Volgt [ formaat W3C ](https://www.w3.org/TR/NOTE-datetime) (ISO 8601). U kunt het beste de verschuiving van de tijdzone opnemen. Volledige datum plus uren en notulen: YYYY-MM-DDThh :mm: ssTZD waar TZD &quot;+hh:mm&quot;of &quot;-hh:mm&quot;Nota is: Sommige activa APIs keren &quot;Z+0000&quot;als TZD voor `updatedAt` en `createdAt` terug. | 2010-05-07T15 :41: 32-05:00 |
| E-mail | Een tekenreeksveld dat e-mailadressen accepteert | example@example.com |
| Float | Een getalveld dat echte getallen bevat en een decimale waarde kan gebruiken. | 10,4 |
| Geheel | Hele getallen | 10 |
| Formule | Velden waarvan de waarden worden gegenereerd door gegevens te manipuleren uit andere velden in een lead record. Ze worden niet geëxporteerd en kunnen niet worden gebruikt in slimme campagnes. | Zie dit [ artikel ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/field-management/create-and-use-a-concatenated-string-formula-field) |
| Percentage | Een percentage uitgedrukt als een geheel getal | 30 |
| URL | Een tekstveld dat invoer beperkt tot URL&#39;s, inclusief het protocol van de URL. | http://example.com/ |
| Telefoonnummer | Telefoonnummer | 111-111-111 |
| Tekstgebied | Langere tekst. | Ondersteunt maximaal 30.000 bytes. Standaard ASCII-tekens gebruiken 1 byte per teken (maximaal 30.000 tekens). Unicode-tekens kunnen maximaal 4 bytes per teken gebruiken (het verminderen van de  aantal tekens toegestaan tot minder dan 30.000 tekens). |
| String | Kortere tekst | Tekst tot 255 tekens |
| Score | Een geheel-getalveld dat met de de stroomstap van de Score van de Verandering kan worden gemanipuleerd | 10 |
| Boolean (voorheen Selectievakje) | Hiermee kunnen gebruikers een waarde voor Waar (ingeschakeld) of Onwaar (uitgeschakeld) selecteren. | Waar |
| Valuta | Een zwevend veld dat staat voor het standaardvalutatype dat is geselecteerd voor het Marketo-abonnement | 10,40 |
| Datum | Wordt gebruikt voor datum. volgt de W3C-indeling. | 05-07-2010 |
| Referentie | Een tekenreeksveld met een sleutel tot een andere record (een buitenlandse sleutel). | Contactpersoon |
