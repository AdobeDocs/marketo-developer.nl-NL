---
title: "Aanbevolen werkwijzen voor Marketo-integratie"
feature: REST API
description: "Aanbevolen procedures voor het gebruik van Marketo API's."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---


# Aanbevolen werkwijzen voor Marketo-integratie

## API-limieten

- **Dagelijks quotum:** De meeste abonnementen worden toegewezen 50.000 API vraag per dag (die dagelijks bij 12:00AM CST) terugstelt. Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Snelheidslimiet:** API toegang per instantie beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige limiet:**  Maximaal tien gelijktijdige API-aanroepen.
- **Batchgrootte:** Looddatabase - 300 records; Asset Query - 200 records
- **Payload Size REST API:** 1 MB
- **Grootte bulkimportbestand:** 10 MB
- **Maximale batterijgrootte SOAP:** 300 records
- **Taken voor bulkextractie:** 2 wordt uitgevoerd; 10 in de wachtrij (inclusief)

## Snelle tips

- Veronderstel dat uw toepassing voor quota, tarief, en gelijktijdig middelen met andere toepassingen zal concurreren, en conservatieve gebruiksgrenzen plaatst.
- Gebruik, indien beschikbaar en van toepassing, de Marketo-methode voor bulkgoederen en batches. Gebruik alleen enkele record- of enkele resultaataanroepen wanneer dat nodig is.
- Gebruiken [exponentiële backoff](https://en.wikipedia.org/wiki/Exponential_backoff) om API vraag opnieuw te proberen die wegens tarief of gelijktijdige grenzen ontbreekt.
- Vermijd het maken van gelijktijdige API-aanroepen als uw gebruiksscenario er geen baat bij heeft.

## Batch

Om de beste prestaties voor uw integratie te verzekeren, wanneer het uitvoeren van tussenvoegsels of updates, zouden de verslagen in zo weinig mogelijk transacties moeten worden gegroepeerd. Wanneer het terugwinnen van verslagen van een gegevensopslag voor voorlegging, zouden de verslagen altijd vóór voorlegging moeten worden samengevoegd, eerder dan het voorleggen van een verzoek voor elke individuele verandering.

## Acceptabele vertraging

Door uw latentietoleranties of de maximale hoeveelheid tijd te bepalen die kan worden doorgegeven voordat een API-aanroep wordt verzonden, worden veel, zo niet de meeste, op de hoogte gebracht van de beslissingen die u neemt bij het ontwerpen van uw integratie naar Marketo. Marketo biedt vele verschillende methoden en configuratieopties die geschikt zijn voor verschillende gebruiksgevallen en verschillende latentieklassen. Bijvoorbeeld, zou een integratie in real time om een verkoper op de hoogte te brengen van een gebruiker die zich voor een proef aantekent slechts partijen van één kunnen voorleggen als de directe follow-up wordt vereist. Nochtans, vereisen de meeste gevallen dit niet en kunnen extra latentie tolereren en kunnen efficiënter door het een rij vormen en het oppakken vraag worden beheerd.

| Acceptabele vertraging | Voorkeursmethoden | Notities |
|---|---|---|
| Laag (&lt;10s) | Synchrone API&#39;s (in batch of in niet-batches) | Zorg ervoor dat dit vereist is voor uw gebruiksgeval. Het verzenden van directe en synchrone aanroepen voor toepassingen met een hoog volume kan snel een dagelijkse API-quota verbruiken en kan zowel de frequentie- als de gelijktijdige limiet overschrijden. |
| Normaal (10 - 60 m) | Synchrone API&#39;s (in batch) | Voor binnenkomende gegevensintegratie in Marketo wordt het gebruik van een wachtrij met zowel een leeftijd- als een groottebeperking ten zeerste aanbevolen. Wanneer een van beide limieten is bereikt, verwijdert u de wachtrij en verzendt u uw API-verzoek met de geaccumuleerde records. Dit is een sterk compromis tussen snelheid en efficiency, die ervoor zorgen dat uw verzoeken bij de vereiste kapper voorkomen, terwijl het in batches zo vele verslagen zoals de leeftijd van de rij toestaat. |
| Hoog(>60 m) | Bulk importeren/exporteren (indien ondersteund) | Voor binnenkomende gegevensintegratie moeten records in de wachtrij worden geplaatst en via Marketo Bulk API&#39;s worden verzonden, indien beschikbaar. |

## Dagelijkse limieten

Elke API-Toegelaten instantie van Marketo heeft een dagelijkse toewijzing van minstens 10.000 REST API vraag per dag, maar meer algemeen 50.000 of meer, en 500MB of meer van de capaciteit van het Extraheren van het Bulk. Hoewel extra dagelijkse capaciteit kan worden aangeschaft als onderdeel van een Marketo-abonnement, moet uw toepassingsontwerp rekening houden met de algemene limieten van Marketo-abonnementen.

Aangezien de capaciteit onder alle API diensten en gebruikers in een geval wordt gedeeld, moeten de beste praktijken overtollige vraag elimineren, en aan partijverslagen in zo weinig vraag zo weinig mogelijk. De meest efficiënte manier om records te importeren, is door Marketo-API&#39;s voor bulkimport te gebruiken, die beschikbaar zijn voor [Leads/personen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads/operation/importLeadUsingPOST) en [Aangepaste objecten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Snippets/operation/createSnippetUsingPOST). Marketo biedt ook Bulk Extract voor [Leads](bulk-lead-extract.md) en [Activiteiten](bulk-activity-extract.md).

### Caching

De resultaten van de volgende bewerkingen kunnen doorgaans een dag of langer in cache op de client worden geplaatst, omdat ze niet vaak worden gewijzigd:

- Resultaten van beschrijvingsbewerkingen
- [Activiteitstypen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET)
- [Partities](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadPartitionsUsingGET)

Het in cache plaatsen van bepaalde typen middelen, zoals programma&#39;s, e-mails en mappen, is ook geschikt voor bepaalde gebruiksgevallen, zoals gegevensverrijking voor lood of activiteitenrecords.

## Snelheidslimiet

Elke Marketo-instantie heeft een snelheidslimiet van 100 aanroepen per 20 seconden, die wordt gedeeld door alle API-services van derden. Als deze limiet wordt overschreden, reageert de API met een foutcode van 606 die aangeeft dat de snelheidslimiet is overschreden. In het algemeen moeten integraties van derden hun gebruik beperken tot 50 aanroepen per 20 seconden of minder om een eerlijk gebruik van de tarieflimieten door meerdere API-integraties en gebruikers mogelijk te maken. Hoewel het in bepaalde gevallen passend kan zijn deze limiet te verzadigen, zijn toepassingen die batchverwerking gebruiken en hun doorvoer tot minder dan deze limiet richten, over het algemeen responsiever en consistenter in hun werking, tegen een kleine kostprijs van verhoogde latentie.

## Gelijktijdige limiet

Elke Marketo-instantie heeft een gedeelde limiet van tien gelijktijdige REST API-aanroepen. Net als de dagelijkse quota- en tarieflimieten die worden gedeeld, moet u er dus niet van uitgaan dat uw toepassing de exclusieve consument van deze limiet zal zijn. Marketo telt het aantal gezamenlijke vraag als die die verwerken en nog niet zijn teruggekeerd, zodat wanneer een vraag terugkeert, wordt het niet meer geteld tegen de gezamenlijke vraaggrens.

De meeste gevallen van integratiegebruik hebben geen baat bij het maken van gelijktijdige aanroepen. Denk er dus aan of uw toepassing baat heeft bij het indienen van gelijktijdige aanvragen bij Marketo. Als u wel gelijktijdige uitvoering wilt uitvoeren, moet u het aantal gelijktijdige aanvragen in het oorspronkelijke ontwerp beperken tot maximaal vijf. Verhoog dit aantal pas nadat u hebt vastgesteld dat uw toepassing meer nodig heeft.

## Fouten

Met uitzondering van enkele zeldzame gevallen, retourneren API-aanvragen een HTTP-statuscode van 200. De bedrijfslogische fouten keren ook 200 terug, maar bevatten gedetailleerde informatie in het lichaam van de reactie. Zie [Foutcodes](error-codes.md) voor een uitvoerige toelichting. De redenuitdrukking van HTTP zou niet moeten worden geëvalueerd aangezien het facultatief en onderworpen aan verandering is.
