---
title: Foutcodes
feature: REST API
description: Marketo-foutcodebeschrijvingen.
exl-id: a923c4d6-2bbc-4cb7-be87-452f39b464b6
source-git-commit: 71e685efb25acffb5c1000293daaea4e936fc4f5
workflow-type: tm+mt
source-wordcount: '2320'
ht-degree: 2%

---

# Foutcodes

Hieronder vindt u een lijst met REST API-foutcodes en een uitleg van de manier waarop fouten worden geretourneerd naar toepassingen.

## Uitzonderingen voor afhandeling en registratie

Wanneer het ontwikkelen voor Marketo, is het belangrijk dat de verzoeken en de reacties het programma worden geopend wanneer een onverwachte uitzondering wordt ontmoet. Terwijl bepaalde soorten uitzonderingen, zoals verlopen authentificatie, veilig door re-authentificatie kunnen worden behandeld, kunnen anderen steuninteractie vereisen, en de verzoeken en de reacties zullen altijd in dit scenario worden gevraagd.

## Fouttypen

De Marketo REST-API kan drie verschillende fouttypen retourneren onder normale werking:

* HTTP-niveau: deze fouten worden aangegeven door een `4xx` -code.
* Responsniveau: deze fouten worden opgenomen in de array &quot;errors&quot; van de JSON-respons.
* Recordniveau: deze fouten worden opgenomen in de &quot;resultaat&quot;-array van de JSON-respons en worden op individuele recordbasis aangegeven met het veld &quot;status&quot; en de array &quot;reason&quot;.

Voor fouttypen op antwoordniveau en op recordniveau wordt een HTTP-statuscode van 200 geretourneerd. Voor alle fouttypen moet de HTTP-redenuitdrukking niet worden geëvalueerd omdat deze optioneel is en kan worden gewijzigd.

### Fouten op HTTP-niveau

Onder normale bedrijfsomstandigheden mag Marketo slechts twee fouten in de HTTP-statuscode `413 Request Entity Too Large` en `414 Request URI Too Long` retourneren. Deze zijn zowel terugwinbaar door de fout te vangen, het verzoek te wijzigen en opnieuw te proberen, maar met slimme codeerpraktijken, zou u deze nooit in het wild moeten ontmoeten.

Marketo retourneert 413 als de Request Payload groter is dan 1 MB, of 10 MB in het geval van Import Lead. In de meeste gevallen is het onwaarschijnlijk dat deze limieten worden overschreden, maar door een controle toe te voegen aan de grootte van het verzoek en records te verplaatsen die ertoe leiden dat de limiet wordt overschreden naar een nieuw verzoek, moeten omstandigheden worden voorkomen die ertoe leiden dat deze fout door eindpunten wordt geretourneerd.

414 wordt geretourneerd wanneer de URI van een GET-aanvraag groter is dan 8 kB. Om het te vermijden, controleer tegen de lengte van uw vraagkoord om te zien of overschrijdt het deze grens. Als het uw verzoek in een methode van de POST verandert, dan voer uw vraagkoord als verzoeklichaam met de extra parameter `_method=GET` in. Hiermee wordt de beperking op URI&#39;s genegeerd. Het is zeldzaam om deze grens in de meeste gevallen te raken, maar het is enigszins gemeenschappelijk wanneer het terugwinnen van grote partijen verslagen met lange individuele filterwaarden zoals een GUID.
Het [ eindpunt van de Identiteit ](https://developer.adobe.com/marketo-apis/api/identity/) kan een 401 Onbevoegde fout terugkeren. Dit is doorgaans het gevolg van een ongeldige client-id of een ongeldig clientgeheim. Foutcodes op HTTP-niveau

<table>
  <thead>
    <tr>
      <th> Antwoordcode </th>
      <th> Beschrijving </th>
      <th colspan="1"> Opmerking </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="413"></a>413</td>
      <td>Aanvraagentiteit te groot</td>
      <td>De lading overtrof de grens 1 MB.</td>
    </tr>
    <tr>
      <td><a name="414"></a>414</td>
      <td>Request-URI te lang</td>
      <td>De URI van de aanvraag is groter dan 8 kB. Het verzoek zou als POST met param `_method=GET ` in URL, en de rest van het vraagkoord in het lichaam van het verzoek opnieuw moeten worden geprobeerd.</td>
    </tr>
  </tbody>
</table>

#### Fouten op responsniveau

Er zijn fouten in het responsniveau aanwezig wanneer de parameter `success` van de reactie is ingesteld op false en als volgt is gestructureerd:

```json
{
    "requestId": "e42b#14272d07d78",
    "success": false,
    "errors": [
        {
            "code": "601",
            "message": "Unauthorized"
        }
    ]
}
```

Elk object in de array &#39;errors&#39; heeft twee leden, `code` . Dit is een geheel getal tussen 601 en 799 en een `message` dat de reden voor de fout in normale tekst aangeeft. 6xx-codes geven altijd aan dat een aanvraag volledig is mislukt en niet is uitgevoerd. Een voorbeeld is 601, het &quot;teken van de Toegang ongeldig,&quot;dat kan terugkrijgen door het nieuwe toegangstoken met het verzoek opnieuw voor authentiek te verklaren en over te gaan. 7xx fouten wijzen erop dat het verzoek ontbrak, of omdat geen gegevens werden teruggekeerd, of het verzoek verkeerd geparametereerd was, zoals het omvatten van een ongeldige datum, of het missen van een vereiste parameter.

#### Foutcodes op responsniveau

Een API-aanroep die deze responscode retourneert, wordt niet in mindering gebracht op uw dagelijkse quotum of uw tarieflimiet.

<table>
  <thead>
    <tr>
      <th> Antwoordcode </th>
      <th> Beschrijving </th>
      <th> Opmerking </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a name="502"></a>502</td>
      <td>Onjuiste gateway</td>
      <td>De externe server heeft een fout geretourneerd. Waarschijnlijk een time-out. Het verzoek zou met exponentiële steun opnieuw moeten worden berecht.</td>
    </tr>
    <tr>
      <td><a name="601"></a>601*</td>
      <td>Toegangstoken is ongeldig</td>
      <td>Een parameter van het Toegangstoken was inbegrepen in het verzoek, maar de waarde was geen geldig toegangstoken.</td>
    </tr>
    <tr>
      <td><a name="602"></a>602*</td>
      <td>Toegangstoken is verlopen</td>
      <td>Het toegangstoken inbegrepen in de vraag is niet meer geldig toe te schrijven aan afloop.</td>
    </tr>
    <tr>
      <td><a name="603"></a>603</td>
      <td>Toegang geweigerd</td>
      <td>Verificatie is geslaagd, maar de gebruiker heeft onvoldoende machtigingen om deze API aan te roepen. [De extra toestemmingen] (douane-services.md) kunnen aan de gebruikersrol moeten worden toegewezen, of <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-an-allowlist-for-ip-based-api-access"> Lijst van gewenste personen voor IP-Gebaseerde API Toegang </a> kan worden toegelaten.</td>
    </tr>
    <tr>
      <td><a name="604"></a>604*</td>
      <td>Verzoek om time-out</td>
      <td>Het verzoek liep te lang (bijvoorbeeld, ondervond gegevensbestandgeschil), of overschrijdt de onderbreking die in de kopbal van de vraag wordt gespecificeerd.</td>
    </tr>
    <tr>
      <td><a name="605"></a>605*</td>
      <td>HTTP-methode wordt niet ondersteund</td>
      <td>GET wordt niet ondersteund voor het eindpunt Leads synchroniseren. POST moet worden gebruikt.</td>
    </tr>
    <tr>
      <td><a name="606"></a>606</td>
      <td>Maximumsnelheid `%s'; overschreden met in `%s` sec</td>
      <td>Het aantal vraag in de afgelopen 20 seconden was groter dan 100</td>
    </tr>
    <tr>
      <td><a name="607"></a>607</td>
      <td>Dagelijks quotum bereikt</td>
      <td>Het aantal vraag overtrof vandaag het quotum van het abonnement (stelt dagelijks om 12:00AM CST terug).&gt;Uw quota vindt u in het menu Admin-&gt;Webservices. U kunt uw quota verhogen via uw accountmanager.</td>
    </tr>
    <tr>
      <td><a name="608"></a>608*</td>
      <td>Tijdelijk niet beschikbaar voor API</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="609"></a>609</td>
      <td>Ongeldige JSON</td>
      <td>De in het verzoek opgenomen instantie is geen geldige JSON.</td>
    </tr>
    <tr>
      <td><a name="610"></a>610</td>
      <td>Aangevraagde bron niet gevonden</td>
      <td>De URI in de aanroep komt niet overeen met een REST API-brontype. Dit wordt vaak veroorzaakt door een onjuist gespelde of onjuist opgemaakte aanvraag-URI</td>
    </tr>
    <tr>
      <td><a name="611"></a>611</td>
      <td>Systeemfout</td>
      <td>Alle niet-verwerkte uitzonderingen</td>
    </tr>
    <tr>
      <td><a name="612"></a>612</td>
      <td>Ongeldig inhoudstype</td>
      <td>Als deze fout optreedt, voegt u een koptekst voor het inhoudstype toe waarin de JSON-indeling is opgegeven. Probeer bijvoorbeeld het inhoudssoort application/json te gebruiken. <a href="https://stackoverflow.com/questions/28181325/why-invalid-content-type"> zie deze vraag StackOverflow </a> voor meer details.</td>
    </tr>
    <tr>
      <td><a name="613"></a>613</td>
      <td>Ongeldige multipart-aanvraag</td>
      <td>De meerdelige inhoud van de POST is niet correct opgemaakt</td>
    </tr>
    <tr>
      <td><a name="614"></a>614</td>
      <td>Ongeldig abonnement</td>
      <td>Het doelabonnement kan niet worden gevonden of is onbereikbaar. Dit geeft meestal tijdelijke ontoegankelijkheid aan.</td>
    </tr>
    <tr>
      <td><a name="615"></a>615</td>
      <td>Gelijktijdige toegangslimiet bereikt</td>
      <td>Aanvragen worden hoogstens 10 keer per abonnement verwerkt. Dit wordt teruggegeven als er reeds 10 lopende verzoeken zijn.</td>
    </tr>
    <tr>
      <td><a name="616"></a>616</td>
      <td>Ongeldig abonnementstype</td>
      <td>Het juiste Marketo-abonnementstype is vereist voor toegang tot de API voor metagegevens van aangepaste objecten. Raadpleeg uw CSM voor meer informatie.</td>
    </tr>
    <tr>
      <td><a name="701"></a>701</td>
      <td>%s mag niet leeg zijn</td>
      <td>Het gerapporteerde veld mag niet leeg zijn in het verzoek</td>
    </tr>
    <tr>
      <td><a name="702"></a>702</td>
      <td>Geen gegevens gevonden voor een bepaald zoekscenario</td>
      <td>Er zijn geen records die overeenkomen met de opgegeven zoekparameters.
        Opmerking: veel mislukte zoekbewerkingen retourneren "success = true", geen fouten en stellen een informatieve tekenreeks voor waarschuwingen in.</td>
    </tr>
    <tr>
      <td><a name="703"></a>703</td>
      <td>De functie is niet ingeschakeld voor het abonnement</td>
      <td>Een bètafunctie die niet is ingeschakeld in een gebruikersabonnement</td>
    </tr>
    <tr>
      <td><a name="704"></a>704</td>
      <td>Ongeldige datumnotatie</td>
      <td><ul>
          <li>Er is een datum opgegeven die niet de juiste notatie heeft</li>
          <li>Er is een ongeldige id voor dynamische inhoud opgegeven</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="709"></a>709</td>
      <td>Overtreding van bedrijfsregels</td>
      <td>De aanroep kan niet worden uitgevoerd omdat deze in strijd is met een vereiste om een element te maken of bij te werken, bijvoorbeeld wanneer u een e-mail wilt maken zonder sjabloon. Het is ook mogelijk deze fout op te halen wanneer u probeert:
        <ul>
          <li>Inhoud ophalen voor bestemmingspagina's die sociale inhoud bevatten.</li>
          <li>Kloon een programma dat bepaalde activa types (zie <a href="programs.md#clone"> van de Kloon van het Programma 0} {voor meer informatie) bevat.</a></li>
          <li>Een element goedkeuren zonder concept (dat wil zeggen dat het al is goedgekeurd).</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="710"></a>710</td>
      <td>Bovenliggende map niet gevonden</td>
      <td>Kan de opgegeven bovenliggende map niet vinden</td>
    </tr>
    <tr>
      <td><a name="711"></a>711</td>
      <td>Niet-compatibel maptype</td>
      <td>De opgegeven map is niet van het juiste type om aan het verzoek te voldoen</td>
    </tr>
    <tr>
      <td><a name="712"></a>712</td>
      <td>De bewerking Samenvoegen tot persoonlijke account is ongeldig</td>
      <td>Een samenvoegen van leads is mislukt als gevolg van een poging om leads samen te voegen die Salesforce Person Accounts zijn.  Salesforce Person Accounts moet in Salesforce worden samengevoegd.</td>
    </tr>
    <tr>
      <td><a name="713"></a>713</td>
      <td>Fout van overgang</td>
      <td>Een systeembron was tijdelijk niet beschikbaar op het moment van de API-aanroep. Als deze fout optreedt, wordt u geadviseerd om op tijd te wachten en de aanvraag vervolgens opnieuw uit te voeren.</td>
    </tr>
    <tr>
      <td><a name="714"></a>714</td>
      <td>Kan het standaardrecordtype niet vinden</td>
      <td>Een samenvoegen van leads is mislukt omdat er geen standaardrecordtype is gevonden.</td>
    </tr>
    <tr>
      <td><a name="718"></a>718</td>
      <td>ExternalSalesPersonID niet gevonden</td>
      <td>Een vraag van de Kanalen van de Synchronisatie werd gemaakt met een niet-bestaande ` waarde ExternalSalesPersonID.</td>
    </tr>
    <tr>
      <td>719</td>
      <td>Time-outuitzondering vergrendelen</td>
      <td>Een vraag van het Programma van de Kloon werd gemaakt en uit wachtend op een slot.</td>
    </tr>
  </tbody>
</table>

### Recordniveau {#record_level_errors}

Fouten op recordniveau geven aan dat een bewerking niet kan worden voltooid voor een afzonderlijke record, maar dat het verzoek zelf geldig was. Een reactie met fouten op recordniveau volgt dit patroon:

#### Antwoord

```json
{
   "requestId":"e42b#14272d07d78",
   "success":true,
   "result":[
      {
         "id":50,
         "status":"created"
      },
      {
         "id":51,
         "status":"created"
      },
      {
         "status":"skipped",
         "reasons":[
            {
               "code":"1005",
               "message":"Lead already exists"
            }
         ]
      }
   ]
}
```

Records die zijn opgenomen in de resultaatarray van aanroepen worden op dezelfde manier geordend als de invoerarray van een aanvraag.
Elke record in een succesvol verzoek kan slagen of ontbreken op een individuele basis, die door het statusgebied van elk verslag inbegrepen in de resultaatserie van een reactie wordt vermeld. Het veld status van deze records wordt overgeslagen en er is een array &#39;reason&#39; aanwezig. Elke reden bevat een &quot;code&quot;lid, en een &quot;bericht&quot;lid. De code is altijd 1xxx, en het bericht wijst erop waarom het verslag werd overgeslagen. In een voorbeeld zou een &#39;handeling&#39; in een &#39;handeling&#39; van een &#39;Leads synchroniseren&#39; zijn ingesteld op &#39;AlleenMaken&#39;, maar er bestaat al een lead voor een van de sleutels in de verzonden records. Deze kwestie keert een code van 1005 terug, en een bericht van &quot;Lood bestaat reeds&quot;zoals hierboven getoond.

#### Foutcodes op recordniveau

>[!NOTE]
>
><table>
<tbody>
    <tr>
      <td>Antwoordcode</td>
      <td>Beschrijving</td>
      <td>Opmerking</td>
    </tr>
    <tr>
      <td><a name="1001"></a>1001</td>
      <td>Ongeldige waarde '%s'. Vereist van type '%s'</td>
      <td>Er wordt een fout gegenereerd wanneer een parameterwaarde niet overeenkomt met het type. Bijvoorbeeld de tekenreekswaarde die is opgegeven voor een parameter integer.</td>
    </tr>
    <tr>
      <td><a name="1002"></a>1002</td>
      <td>Ontbrekende waarde voor de vereiste parameter '%s'</td>
      <td>Er wordt een fout gegenereerd wanneer een vereiste parameter ontbreekt in de aanvraag</td>
    </tr>
    <tr>
      <td><a name="1003"></a>1003</td>
      <td>Ongeldige gegevens</td>
      <td>Wanneer de overgelegde gegevens geen geldig type voor het bepaalde eindpunt of de wijze zijn; zoals wanneer identiteitskaart voor een lood met actie wordt voorgelegd die als createOnly wordt aangewezen of wanneer het gebruiken van de Campagne van het Verzoek op een partijcampagne wordt aangewezen.</td>
    </tr>
    <tr>
      <td><a name="1004"></a>1004</td>
      <td>Geen lood gevonden</td>
      <td>Voor syncLead, wanneer de actie "updateOnly"is en als lood niet wordt gevonden</td>
    </tr>
    <tr>
      <td><a name="1005"></a>1005</td>
      <td>De lead bestaat al</td>
      <td>Voor syncLead, wanneer de actie "createOnly"is en als een lood reeds bestaat</td>
    </tr>
    <tr>
      <td><a name="1006"></a>1006</td>
      <td>Veld '%s' niet gevonden</td>
      <td>Een inbegrepen gebied in de vraag is geen geldig gebied.</td>
    </tr>
    <tr>
      <td><a name="1007"></a>1007</td>
      <td>Meerdere leads komen overeen met de opzoekcriteria</td>
      <td>Meerdere leads komen overeen met de opzoekcriteria. Updates kunnen alleen worden uitgevoerd wanneer de sleutel overeenkomt met één record</td>
    </tr>
    <tr>
      <td><a name="1008"></a>1008</td>
      <td>Toegang geweigerd voor partitie '%s'</td>
      <td>De gebruiker voor de douanedienst heeft geen toegang tot een werkruimte met de verdeling waar het verslag bestaat.</td>
    </tr>
    <tr>
      <td><a name="1009"></a>1009</td>
      <td>Partitienaam moet worden opgegeven</td>
      <td></td>
    </tr>
    <tr>
      <td><a name="1010"></a>1010</td>
      <td>Partitie-update niet toegestaan</td>
      <td>De opgegeven record bestaat al in een aparte hoofdpartitie.</td>
    </tr>
    <tr>
      <td><a name="1011"></a>1011</td>
      <td>Veld '%s' wordt niet ondersteund</td>
      <td>Bij opzoekveld of 'filterType' opgegeven met niet-ondersteunde standaardvelden (bijv. firstName, lastName)</td>
    </tr>
    <tr>
      <td><a name="1012"></a>1012</td>
      <td>Ongeldige cookiewaarde '%s'</td>
      <td>Kan voorkomen wanneer het roepen van <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/associateLeadUsingPOST"> associeerde lood </a> met een ongeldige waarde voor de "koekjesparameter".
        Dit komt ook voor wanneer het roepen <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET"> leidt door het Type van Filter </a> met ` filterType=cookies' en een ongeldige waarde voor de parameter ` filterValues `.</td>
    </tr>
    <tr>
      <td><a name="1013"></a>1013</td>
      <td>Object niet gevonden</td>
      <td>Hiermee wordt het object (list, campagne) van id opgehaald en wordt deze foutcode geretourneerd</td>
    </tr>
    <tr>
      <td><a name="1014"></a>1014</td>
      <td>Kan object niet maken</td>
      <td>Kan object niet maken (lijst)</td>
    </tr>
    <tr>
      <td><a name="1015"></a>1015</td>
      <td>Lood niet in lijst</td>
      <td>De aangewezen lood is geen lid van de doellijst</td>
    </tr>
    <tr>
      <td><a name="1016"></a>1016</td>
      <td>Te veel invoer</td>
      <td>Er staan te veel importen in de wachtrij. Er is maximaal 10 toegestaan</td>
    </tr>
    <tr>
      <td><a name="1017"></a>1017</td>
      <td>Object bestaat al</td>
      <td>Maken is mislukt omdat de record al bestaat</td>
    </tr>
    <tr>
      <td><a name="1018"></a>1018</td>
      <td>CRM ingeschakeld</td>
      <td>De handeling kan niet worden uitgevoerd, omdat voor de instantie een native CRM-integratie is ingeschakeld.</td>
    </tr>
    <tr>
      <td><a name="1019"></a>1019</td>
      <td>Importeren wordt uitgevoerd</td>
      <td>De doellijst wordt al geïmporteerd naar</td>
    </tr>
    <tr>
      <td><a name="1020"></a>1020</td>
      <td>Te veel klonen voor programma</td>
      <td>Het abonnement heeft het toegewezen gebruik van "cloneToProgramName" in het Programma voor de dag bereikt</td>
    </tr>
    <tr>
      <td><a name="1021"></a>1021</td>
      <td>Bedrijfsupdate niet toegestaan</td>
      <td>Bedrijfsupdate niet toegestaan tijdens syncLead</td>
    </tr>
    <tr>
      <td><a name="1022"></a>1022</td>
      <td>Object in gebruik</td>
      <td>Verwijderen is niet toegestaan wanneer een object door een ander object wordt gebruikt</td>
    </tr>
    <tr>
      <td><a name="1025"></a>1025</td>
      <td>Programmastatus niet gevonden</td>
      <td>Er is een status opgegeven om de status van het leidingprogramma te wijzigen die niet overeenkomt met een status die beschikbaar is voor het kanaal van het programma.</td>
    </tr>
    <tr>
      <td><a name="1026"></a>1026</td>
      <td>Aangepast object niet ingeschakeld</td>
      <td>De handeling kan niet worden uitgevoerd omdat de integratie van aangepaste objecten niet is ingeschakeld voor de instantie.</td>
    </tr>
    <tr>
      <td><a name="1027"></a>1027</td>
      <td>Maximale maximale activiteitstypen bereikt</td>
      <td>Het abonnement heeft het maximumaantal beschikbare types van douaneactiviteit bereikt.</td>
    </tr>
    <tr>
      <td><a name="1028"></a>1028</td>
      <td>Max. veldlimiet bereikt</td>
      <td>Aangepaste activiteiten hebben maximaal 20 secundaire kenmerken.</td>
    </tr>
    <tr>
      <td><a name="1029"></a>1029</td>
      <td><ul>
          <li>Te veel taken in de wachtrij</li>
          <li>Dagelijks exportquotum overschreden</li>
          <li>Taak al in de wachtrij</li>
        </ul></td>
      <td><ul>
          <li>Abonnementen mogen maximaal 10 bulkextractietaken tegelijk in de wachtrij plaatsen.</li>
          <li>Standaard zijn extractietaken beperkt tot 500 MB per dag (worden dagelijks opnieuw ingesteld om 12:00 AM CST).</li>
          <li>De export-id is al in de wachtrij geplaatst.</li>
        </ul></td>
    </tr>
    <tr>
      <td><a name="1035"></a>1035</td>
      <td>Niet-ondersteund filtertype</td>
      <td>In sommige abonnementen, worden de volgende de filtertypes van Extraheren van de Lood van de Bulk niet gesteund: updateAt, smartListId, smartListName.</td>
    </tr>
    <tr>
      <td><a name="1036"></a>1036</td>
      <td>Dubbel object gevonden in invoer</td>
      <td>Er is een aanroep gemaakt om twee of meer records bij te werken met dezelfde externe sleutel. Bijvoorbeeld, roept een vraag van de Bedrijven van de Synchronisatie gebruikend zelfde externalCompanyId voor meer dan één bedrijf.</td>
    </tr>
    <tr>
      <td><a name="1037"></a>1037</td>
      <td>Lood is overgeslagen</td>
      <td>De lead is overgeslagen omdat deze al in of voorbij deze status is.</td>
    </tr>
    <tr>
      <td><a name="1042"></a>1042</td>
      <td>Ongeldige runAt-datum</td>
      <td>De runAt-datum die is opgegeven voor de planningscampagne was te ver in de toekomst (het maximum is 2 jaar).</td>
    </tr>
    <tr>
      <td><a name="1048"></a>1048</td>
      <td>Concept voor verwijderen van aangepast object is mislukt</td>
      <td>Er is een aanroep gemaakt om de conceptversie van een aangepast object te verwijderen.</td>
    </tr>
    <tr>
      <td><a name="1049"></a>1049</td>
      <td>Kan activiteit niet maken</td>
      <td>Kenmerkarray is te lang.
        De array met kenmerken die aan de record zijn doorgegeven, overschrijdt de maximale lengte van 65536 bytes</td>
    </tr>
    <tr>
      <td><a name="1076"></a>1076</td>
      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST"> Samenvoegen leidt </a> vraag met de vlag mergeInCRM is 4.</td>
      <td>U maakt een gedupliceerde record. U wordt aangeraden een bestaande record te gebruiken.
        Dit is de foutmelding die Marketo ontvangt bij het samenvoegen in Salesforce.</td>
    </tr>
    <tr>
      <td><a name="1077"></a>1077</td>
      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST"> de vraag van de Leads van de Fusie </a> ontbrak toe te schrijven aan ` de lengte van het Gebied van SFDC</td>
      <td>Een samenvoegen van Leads-aanroep waarbij mergeInCRM is ingesteld op true, is mislukt omdat het SFDC-veld de limiet van toegestane tekens overschrijdt. U corrigeert de fout door de lengte van het veld SFDC Field te verkleinen of mergeInCRM in te stellen op false.</td>
    </tr>
    <tr>
      <td><a name="1078"></a>1078</td>
      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST"> ontbroken vraag van de Leads van 0} Fusie {wegens geschrapte entiteit, niet een lood/contact, of de criteria van de gebiedsfilter past niet aan.</a></td>
      <td>Fout bij samenvoegen, kan samenvoegbewerking niet uitvoeren in native gesynchroniseerde CRM
        Dit is de foutmelding die Marketo ontvangt bij het samenvoegen in Salesforce.</td>
    </tr>
    <tr>
      <td><a name="1079"></a>1079</td>
      <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/mergeLeadsUsingPOST"> de vraag van de Leads van de Fusie </a> ontbrak toe te schrijven aan het Gepersonaliseerde conflict URL in dubbele verslagen</td>
      <td>Bij een oproep voor samenvoegen van leads zijn veel leads opgegeven met dezelfde persoonlijke URL. Om op te lossen gebruikt u de gebruikersinterface van Marketo Engage om deze verslagen samen te voegen.</td>
    </tr>
  </tbody>
</table>
