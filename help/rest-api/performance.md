---
title: "Prestaties"
feature: REST API
description: "Tips voor de prestaties voor het werken met de Marketo API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---


# Prestaties

Deze pagina bevat een lijst met prestatiegerelateerde onderwerpen die u kunt gebruiken om de prestaties van uw integratie te verbeteren.

## HTTP-compressie

De Marketo REST API ondersteunt HTTP-compressie van responsinstanties met behulp van standaarden die zijn gedefinieerd in de HTTP 1.1-specificatie.  Compressie inschakelen wordt aanbevolen, omdat hierdoor het bandbreedtegebruik en de tijd die nodig is om gegevens op te halen, afnemen.

**Opmerking:**  Payloads van minder dan 1024 bytes worden niet gecomprimeerd.

Als u compressie wilt inschakelen, neemt u de volgende HTTP-header op in de aanvraag:

```html
Accept-Encoding: gzip
```

De Marketo REST API comprimeert de reactiehoofdtekst en neemt deze header op:

```html
Content-Encoding: gzip
```

Hier is een voorbeeld van het gebruiken van Krol om [Regels ophalen op filtertype](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) eindpunt om 5 leads op te halen:

```bash
$ curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
