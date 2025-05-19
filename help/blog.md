---
title: Blogarchief
description: Een archief van het Marketo Developers-blog van 2014-2023
exl-id: d7ae88dd-9938-4957-9798-db43090dab4e
source-git-commit: 206724e5177eb9040fa36202d91b2ed9daa734c3
workflow-type: tm+mt
source-wordcount: '61742'
ht-degree: 0%

---

# Blogarchief

>[!INFO]
>
>Dit is een archief van de Marketo-blog, dat loopt van 2014 tot 2023. Het wordt hier alleen als historische referentie verstrekt.
>Sommige informatie is mogelijk verouderd.  Controleer altijd de huidige documentatie op de recentste functionaliteit.
>

>[!IMPORTANT]
>De SOAP API wordt afgekeurd en zal na 31 oktober 2025 niet meer beschikbaar zijn. De Marketo REST API moet alle nieuwe ontwikkelingen ondergaan en de bestaande services moeten tegen die datum zijn gemigreerd om onderbreking van de service te voorkomen. Als u de dienst hebt die SOAP API gebruikt, te raadplegen gelieve de [ Gids van de Migratie van SOAP API ](https://experienceleague.adobe.com/nl/docs/marketo-developer/marketo/soap/migration) voor informatie over hoe te migreren.
>

>[!IMPORTANT]
>Ondersteuning voor verificatie met behulp van de query-parameter `access_token` wordt verwijderd op 31 oktober 2025. Als uw project een vraagparameter gebruikt om het toegangstoken over te gaan, zou het moeten worden bijgewerkt om de [ kopbal van de Vergunning ](https://experienceleague.adobe.com/nl/docs/marketo-developer/marketo/rest/authentication#using-an-access-token) zo spoedig mogelijk te gebruiken. De nieuwe ontwikkeling zou de kopbal van de Vergunning uitsluitend moeten gebruiken.
>

## Welkom bij het Marketo Developer Blog

Welkom Marketo Developers! We zijn hier erg druk bezig geweest met Marketo en hebben nieuwe functies uitgebracht om Marktdeelnemers te helpen meer succes te boeken dan ooit tevoren. De huidige marktspelers worden ondersteund door een fenomenale ontwikkelaarsgemeenschap. Dit weblog is gewijd aan webontwikkelaars en software-engineers die de snel veranderende behoeften van de moderne markt ondersteunen. Naarmate het Marketo-platform zich ontwikkelt, kondigen we nieuwe integratieopties en API-versies aan, zodra deze beschikbaar zijn. We zullen ook een nieuwe serie &#39;Hoe kan ik?-artikelen introduceren waarin we codevoorbeelden en beste praktijken over integratie met het Marketo Platform delen. Het eerste artikel in deze reeks zal u door hoe te om informatie over de mensen (klanten/contacten/lood) efficiënt terug te winnen die binnen Marketo gebruikend API worden opgeslagen. Meld u aan met het bovenstaande formulier om up-to-date te blijven. U ontvangt een e-mail met updates wanneer nieuwe artikelen worden gepubliceerd.

Gepost op _2014-03-06_ door _David_

## Updates release januari 2014

### Forms 2.0

Met Forms 2.0 kunnen marketers prachtige, stabiele en flexibele webformulieren maken zonder programmeerkennis. Naast de sterk verbeterde redacteur, voorwaardelijke logica en robuust ontwerp, is het nu gemakkelijker dan ooit om Marketo-formulieren in te sluiten om het even welke pagina van uw website. Ontwikkelaars kunnen JavaScript gebruiken om de kernfunctionaliteit uit te breiden. Hierbij worden gevallen gebruikt:

* Een formulier verbergen nadat het is verzonden, zodat de bezoeker op de pagina blijft staan nadat hij het formulier heeft ingevuld
* Een aangepast foutbericht weergeven bij verzenden op basis van aangepaste bedrijfslogica
* Het formulier weergeven in een lichtbakdialoogvenster

De documentatie van de ontwikkelaar is beschikbaar [ hier ](/help/javascript-api/forms-api-reference.md).

### SOAP API versie 2_3 nu beschikbaar

* [ getLeadChanges:](/help/soap-api/getleadchanges.md) Ingevoerd verzoekgebied `activityNameFilter`
* [ ListOperation:](/help/soap-api/listoperation.md) Verwijderd verzoekgebied `skipActivityLog`

**Nota:** de revisies van SOAP API zijn achteruit compatibel

Gepost op _2014-01-26_ door _Travis Kaufman_

## Zapier Part II: Marketo Integration Notice

In een eerder artikel bespraken we hoe u Zapier kunt gebruiken om externe gegevensbronnen te integreren met Marketo. In het artikel werd een praktische aanpak gepresenteerd voor het ontwikkelen van uw eigen Zapier-workflow (of &quot;Zap&quot;), van nul tot en met de integratie van Marketo en andere apps. Dus je vindt het leuk om Zapier met Marketo te gebruiken, maar je hebt hulp nodig om aan de slag te gaan? Goed nieuws! Zapier heeft zojuist een aantal voorbeelden van Zaps voor Marketo uitgebracht waarmee je snel kunt gaan:

* Nieuwe leads vastleggen
* Uw klanten voeden
* Contactgegevens distribueren
* Reageren op nieuwe vooruitzichten

Naast deze voorbeelden, kunt u de [ de integraties van Marketo ](https://zapier.com/apps/marketo/integrations) pagina met honderden andere apps op Zapier doorbladeren, en uw eigen geautomatiseerde werkschema&#39;s in notulen bouwen. Er is geen codering vereist. Bespaar tijd en laat automatisering uw handwerk doen. Creatief. De hemel is de grens!

Gepost op _2016-06-01_ door _David_

## De klant- en perspectiefinformatie van Marketo ophalen met behulp van de API

U kunt informatie over klanten en vooruitzichten terugwinnen die binnen Marketo gebruikend `getLead` en [&#128279;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) SOAP API worden opgeslagen. `getMultipleLeads` Het is vaak gewenst om deze informatie op een terugkomende basis te halen om een ander systeem bij te houden aangezien de klanten en de perspectiefinformatie wordt bijgewerkt of nieuwe verslagen in Marketo worden gecreeerd. We tonen u het codevoorbeeld dat op terugkerende basis zou worden uitgevoerd om Marketo voor updates te opiniepeilen. Het onderstaande diagram toont de API-aanroepen die op een ingestelde periodieke timer zijn gemaakt. Afhankelijk van het gebruiksgeval kan de periodieke timer zo worden ingesteld dat deze elke 10 minuten de onderstaande code uitvoert.

De eerste vraag aan [`getMultipleLeads` ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads) zal de tijdwaaier, batchSize plaatsen en welke gebieden moeten zijn teruggekeerd. Alle leads in Marketo die in het opgegeven tijdbereik zijn bijgewerkt, worden samen met een streamPosition geretourneerd wanneer er meer records beschikbaar zijn dan de batchSize die is opgegeven.

**SOAP Verzoek voor eerste vraag om getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector>
  <batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```

**Reactie van SOAP van de eerste vraag aan getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
 <returnCount>2</returnCount><remainingCount>24</remainingCount><newStreamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</newStreamPosition><leadRecordList>
      <leadRecord>
        <Id>84105</Id>
        <Email>eimang@marketo.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>French</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>Lead</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
      <leadRecord>
        <Id>1089965</Id>
        <Email>t@t.com</Email>
        <ForeignSysPersonId xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <ForeignSysType xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
        <leadAttributeList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true">
          <attribute>
            <attrName>FirstName</attrName>
            <attrType>string</attrType>
            <attrValue>George</attrValue>
          </attribute>
          <attribute>
            <attrName>LastName</attrName>
            <attrType>string</attrType>
            <attrValue>of the Jungle</attrValue>
          </attribute>
        </leadAttributeList>
      </leadRecord>
    </leadRecordList>
  </result>
</ns2:successGetMultipleLeads>
```

Zolang de waarde `<remainingCount/>` groter is dan 0, doet u volgende aanroepen aan `getMultipleLeads` om door de rest te pagineren door de waarde `<newStreamPosition/>` door te geven die in de vorige aanroep is geretourneerd naar de parameter `<streamPosition/>` . **SOAP Verzoek voor verdere vraag aan getMultipleLeads:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsGetMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadSelector xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ns2:LastUpdateAtSelector">
    <latestUpdatedAt>2014-02-13T11:51:08.710-08:00</latestUpdatedAt>
    <oldestUpdatedAt>2014-02-12T11:51:08.713-08:00</oldestUpdatedAt>
  </leadSelector><streamPosition>id:1089965:to:1392234668:tl:1392321068:os:2:rc:24</streamPosition><batchSize>2</batchSize>
  <includeAttributes>
    <stringItem>FirstName</stringItem>
    <stringItem>LastName</stringItem>
    <stringItem>Email</stringItem>
  </includeAttributes>
</ns2:paramsGetMultipleLeads>
```


Deze logica gaat door zolang `<remainingCount/>` groter is dan nul. **zie onder een programma van steekproefJava dat het hierboven beschreven scenario uitvoert.**


```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.GregorianCalendar;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import javax.xml.datatype.DatatypeFactory;
import javax.xml.datatype.XMLGregorianCalendar;

public class GetMultipleLeads {
  public static void main(String[] args) {
    System.out.println("Executing GetMultipleLeads");
        try {
        URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
        String marketoUserId = "CHANGE ME";
        String marketoSecretKey = "CHANGE ME";
        QName serviceName = new QName("<http://www.marketo.com/mktows/>", 
        "MktMktowsApiService");
        MktMktowsApiService service = new 
        MktMktowsApiService(marketoSoapEndPoint, serviceName);
        MktowsPort port = service.getMktowsApiSoapPort();

        // Create Signature
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String text = df.format(new Date());
        String requestTimestamp = text.substring(0, 22) + ":" +         text.substring(22);
        String encryptString = requestTimestamp + marketoUserId ;

        SecretKeySpec secretKey = new         SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");

        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(secretKey);
        byte[] rawHmac = mac.doFinal(encryptString.getBytes());
        char[] hexChars = Hex.encodeHex(rawHmac);
        String signature = new String(hexChars);

        // Set Authentication Header
        AuthenticationHeader header = new AuthenticationHeader();
        header.setMktowsUserId(marketoUserId);
        header.setRequestTimestamp(requestTimestamp);
        header.setRequestSignature(signature);

        // Create Request
        ParamsGetMultipleLeads request = new ParamsGetMultipleLeads();

        // Build Request Using LastUpdateAtSelector
        LastUpdateAtSelector leadSelector = new LastUpdateAtSelector();
        GregorianCalendar gc = new GregorianCalendar();
        gc.setTimeInMillis(new Date().getTime());
        gc.add( GregorianCalendar.DAY_OF_YEAR, -20);
        DatatypeFactory factory = DatatypeFactory.newInstance();
        ObjectFactory objectFactory = new ObjectFactory();
        JAXBElement<XMLGregorianCalendar> until =         objectFactory.createLastUpdateAtSelectorLatestUpdatedAt(factory.newXMLGregorianCalendar(gc));

        GregorianCalendar since = new GregorianCalendar();
        since.setTimeInMillis(new Date().getTime());
        since.add( GregorianCalendar.DAY_OF_YEAR, -21);
        leadSelector.setOldestUpdatedAt(factory.newXMLGregorianCalendar(since));
        leadSelector.setLatestUpdatedAt(until);
        request.setLeadSelector(leadSelector);

        ArrayOfString attributes = new ArrayOfString();
        attributes.getStringItems().add("FirstName");
        attributes.getStringItems().add("LastName");
        attributes.getStringItems().add("Email");
        request.setIncludeAttributes(attributes);

        JAXBElement<Integer> batchSize = new 
        ObjectFactory().createParamsGetMultipleLeadsBatchSize(2);
        request.setBatchSize(batchSize);

        SuccessGetMultipleLeads result = null;

        int count = 0;

        do {
        if (count > 0) {
        // Set the streamPosition on subsequent calls to paginate 
        through large result sets
        String pos = result.getResult().getNewStreamPosition();
        JAXBElement<String> streamPos = new 
        ObjectFactory().createParamsGetMultipleLeadsStreamPosition(pos);
        request.setStreamPosition(streamPos);
        }

        JAXBContext context = 
        JAXBContext.newInstance(ParamsGetMultipleLeads.class);
        Marshaller m = context.createMarshaller();
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
        System.out.println("REQUEST:");
        m.marshal(request, System.out);
        result = port.getMultipleLeads(request, header);
        System.out.println("RESPONSE:");
        m.marshal(result, System.out);
        count = result.getResult().getReturnCount();
        } while (result.getResult().getRemainingCount() > 0);
        }

        catch(Exception e) {
        // Update to include appropriate retry/error handling
        e.printStackTrace();
        }

  }

}
```

### Tips en trucs:

Wanneer u grote hoeveelheden contactpersonen uit Marketo haalt, wordt u aangeraden de API-aanvraag als volgt af te stemmen:

* `<includeAttributes/>`: U wordt aangeraden alleen de velden aan te vragen die u wilt gebruiken in overeenstemming met uw systeem. Dit vermindert de lading van de reactie en verhoogt vraagprestaties.
* `<batchSize/>`: De API ondersteunt maximaal 1000 records die in één aanroep moeten worden geretourneerd. Als u deze waarde instelt op 500 records, wordt ook de lading van de reactie verminderd.
* `<LastUpdatedAtSelector/>`: het wordt aanbevolen om zowel de parameter `<oldestUpdatedAt/>` als de parameter `<latestUpdatedAt/>` in te stellen om het datumbereik te beperken. Bijvoorbeeld, in plaats van één enkel verzoek om de waarde van een jaar van gegevens te doen. Breek de API-aanroepen op om kleinere datumbereiken aan te vragen.

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepost op _2014-03-05_ door _Travis Kaufman_

## Updates release februari 2014

### SOAP API-update:

* [ syncMObjects ](/help/soap-api/syncmobjects.md): U kunt markeringen en kanalen voor bestaande programma&#39;s nu toevoegen en bijwerken.

De updates worden opgenomen in [ 2_3 WSDL ](http://app.marketo.com/soap/mktows/2_3?WSDL).

Gepost op _2014-02-26_ door _Travis Kaufman_

## Updates release maart 2014

### SOAP API-update:

* De verbeteringen van prestaties aan [ syncLead ](/help/soap-api/synclead.md) en [ syncMultipleLeads ](/help/soap-api/syncmultipleleads.md)

De updates worden opgenomen in [ 2_3 WSDL ](http://app.marketo.com/soap/mktows/2_3?WSDL).

Gepost op _2014-03-20_ door _Travis Kaufman_

## Activiteit anonieme bezoeker samenvoegen wanneer bezoeker formulier invult

In het blogbericht getiteld &quot;Capture Anonymous Visitor Activity Based on Business Logic&quot; hebben we besproken hoe we anonieme leadrecords in Marketo kunnen maken op basis van aangepaste gebeurtenissen. In dit blogbericht bouwen we op dat bericht en koppelen we een anonieme lead record aan een bekende gebruiker nadat u de contactgegevens van de gebruiker hebt ontvangen. Marketo [ het volgen code van Munchkin ](/help/javascript-api/lead-tracking.md) helpt u bezoeken aan uw website volgen. De eerste keer dat iemand een pagina op uw website bezoekt met de trackingcode van Munchkin, maakt Marketo een anonieme lead en gebruikt een browsercookie om deze bij te houden. Zodra ze zichzelf hebben geïdentificeerd, worden ze een bekende lead en wordt de geschiedenis van hun browsercookie samengevoegd in hun Marketo lead record. **Anonieme lood wordt gecreeerd wanneer iemand:**

1. Bezoek voor het eerst je Marketo-landingspagina
1. Bezoek een pagina met een trackingcode voor Munchkin
1. Klik op de koppeling Weergeven als webpagina in een Marketo-e-mail

**een anonieme lood wordt samengevoegd in een nieuw of bestaand gekend lood wanneer iemand:**

1. Klik op een koppeling naar een Marketo-e-mailbericht
1. Een Marketo-formulier invullen
1. Gebruikt een van de onderstaande technieken om een anonieme lead te koppelen aan een bekende record.

Om persoongegevens voor te leggen, of een browser web-tracking koekje met het overeenkomstige persoonverslag in Marketo te associëren, gebruik één van de volgende technieken: **browser-KantVerzending** als uw gebruiksgeval indiening van persoongegevens van browser vereist, zou u achtergrondvormvoorlegging moeten gebruiken. **server-KantVerzending** als u browser-zij geen voorlegging nodig hebt, REST API biedt vele methodes voor de voorlegging van persoongegevens, en een doel-gebouwde methode voor het associëren van koekjes met persoonverslagen aan.

Gepast op _2014-04-22_ door _Murta_

## Updates van de release van april 2014

### Marketo Forms Security Update:

Wij introduceerden een grens op het aantal en de frequentie van vorm na het indienen van één enkel IP adres. Deze limiet geldt nu voor 30 berichten per minuut om onze klanten te beschermen tegen kwaadwillig gebruik van geprogrammeerde formulieren. [ syncLead API ](https://experienceleague.adobe.com/nl/docs/marketo-developer/marketo/soap/leads/synclead) is het geadviseerde integratievehikel voor programmatic voorlegging van nieuwe contacten in Marketo.    

Gepost op _2014-04-29_ door _Travis Kaufman_

## Plaats het formulier na de integratie in de Marketo API

Het is gebruikelijk dat marketeers die Marketo gebruiken, succes koppelen aan een bepaalde marketingcampagne op basis van het aantal contactpersonen/leads dat een specifiek webformulier heeft ingevuld. De marketeter zou eenvoudig een nieuw Marketo-formulier maken, dit op een landingspagina plaatsen en vervolgens een triggercampagne in Marketo opzetten om alle contactpersonen die dat specifieke formulier hebben ingevuld, te koppelen aan het succes van dat marketingprogramma. (zie onderstaande schermafbeelding). Het is een natuurlijke uitbreiding om deze zelfde benadering te gebruiken wanneer het registreren van programmasucces zelfs wanneer het campagnesucces buiten iemand verblijft die een vorm invult. Bijvoorbeeld, wanneer een marktspeler een promotieaanbieding uitvoert en de omzetting een benoeming moet binnen roepen en plannen. De potentiële klant vult op geen enkel moment in het proces een formulier in. In het volgende artikel, tonen wij u hoe te om Marketo te gebruiken syncLead API om een nieuw contact tot stand te brengen als zij nieuwe contacten zijn, en dan requestCampaign API om het succes voor een bepaald marketing programma te registreren.

Gepost op _1970-01-01_ door _Travis Kaufman_

## Een lead verwijderen met de Marketo API

Laten we zeggen dat u een lead wilt verwijderen via de Marketo API. U kunt dit verwezenlijken door requestCampaign API op een campagne te roepen die een vooraf bepaalde stroomactie heeft om een lood te schrappen. We laten u eerst zien hoe u een slimme campagne maakt, ten tweede hoe u een trigger instelt voor het uitvoeren van een campagne via de API, ten derde hoe u het verwijderen van een lead definieert als onderdeel van een flowactie en ten vierde een codevoorbeeld voor het uitvoeren van deze campagne. **hoe te om een Nieuwe Slimme Campagne in Marketo** Slimme Campagnes in Marketo tot stand te brengen voert elk van uw marketing activiteiten uit. In een Slimme Campagne, kunt u opstelling een reeks geautomatiseerde acties om een slimme lijst van contacten te nemen. Als u een lead verwijdert, stelt u een trigger in de campagne in zoals hieronder wordt weergegeven. Laten we eerst de slimme campagne instellen.

1. Kies bij Marketingactiviteiten een programma en klik vervolgens onder het vervolgkeuzemenu Nieuw lokaal element 1. Klik op Slimme campagne
1. Voer de naam van de slimme campagne in en klik op Triggers toevoegen aan een slimme campagne** Door Triggers aan een slimme campagne toe te voegen, kunt u een slimme campagne op één persoon tegelijk uitvoeren op basis van een livegebeurtenis. In dit geval is dit een aanvraag via de requestCampaign-API.
1. Zoek de trigger &#39;Campaign is Requested&#39; en sleep deze naar het canvas.
1. Selecteer in de trigger &quot;is&quot; en &quot;Web Service API&quot;.

**hoe te om Schrapping tot stand te brengen een Actie van de Stroom van het Lood op een Campagne** klik op Stroom in het hoogste menu. Zoek in het menu aan de rechterkant naar Lead verwijderen en sleep deze vervolgens naar het midden om deze als trigger aan de campagne toe te voegen. Opmerking: als u een lead alleen uit Marketo verwijdert en deze in uw CRM laat staan, wordt de lead opnieuw gemaakt in Marketo wanneer een van de gegevens van die lead wordt bijgewerkt.  **Steekproef van de Code om requestCampaign API** te roepen na vestiging de campagne en trekkers in de interface van Marketo, tonen wij u hoe te om API te gebruiken om deze campagne in werking te stellen. Het eerste voorbeeld is een XML-aanvraag, het tweede een XML-reactie en het laatste voorbeeld is een Java-codevoorbeeld dat kan worden gebruikt om de XML-aanvraag te genereren. We tonen u ook hoe u de campagne-id kunt vinden die wordt gebruikt wanneer u de requestCampaign-API aanroept. De API-aanroep vereist ook dat u de id van de Marketo-campagne vooraf kent. U kunt de campagne-id op een van de volgende manieren bepalen:

1. Gebruik [ getCampaignsForSource ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET) API
1. Open de Marketo-campagne in een browser en bekijk de URL-adresbalk. De campagne-id (vertegenwoordigd als een 4-cijferig geheel getal) is direct na &quot;SC&quot; te vinden. Bijvoorbeeld `https://app-stage.marketo.com/#SC**1025**A1` . Het bolle gedeelte is de campagne-id - &quot;1025&quot;. SOAP-aanvraag voor requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP Response for requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Zie hieronder een voorbeeld van een Java-programma dat het hierboven beschreven scenario uitvoert.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
 
 
public class RequestCampaign {
 
    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";
             
            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();
             
            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);           
            String encryptString = requestTimestamp + marketoUserId ;
             
            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars); 
             
            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);
             
            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();
             
            request.setSource(ReqCampSourceType.MKTOWS);
             
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);
             
            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");
             
            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");
             
            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);
             
            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);
 
            SuccessRequestCampaign result = port.requestCampaign(request, header);
 
            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);
             
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-05-16_ door _Murta_

## Aangepaste activiteiten bijhouden met de Munchkin AP-

Laten we aangepaste activiteiten in Marketo volgen. U hebt bijvoorbeeld een video op een webpagina en u wilt bezoekers bijhouden die meer dan 50% van een video bekijken. Dit kunt u doen met de functie voor het bijhouden van aangepaste activiteiten in Munchkin. Dit wordt geïmplementeerd door te luisteren naar een gebeurtenis op de pagina. Dit is de video die 50% bereikt en vervolgens de Munchkin API aan te roepen. Hiervoor moeten we in Marketo een aangepaste activiteit instellen om te bellen op basis van deze gebeurtenis op de pagina. Wij gebruiken YouTube voor de videospeler, en gebruiken hun [ YouTube Iframe API ](https://developers.google.com/youtube/iframe_api_reference) om de methode op Munchkin API te roepen.

We tonen u eerst hoe u code voor het bijhouden van Munchkin in Marketo genereert, en ten tweede hoe u uw Munchkin-voorbeeldcode wijzigt om te activeren op basis van paginagebeurtenissen, ten derde hoe u een campagne instelt met een slimme lijst die wordt gedefinieerd door acties op de pagina met stroomstappen, en ten vijfde hoe u kunt controleren of een paginabezoek van een anonieme gebruiker is opgenomen in Marketo. ==== Dit blogbericht is een live voorbeeld van de code die wordt uitgelegd. Vul dit formulier in zodat u een bekende gebruiker in Marketo bent. Als u 50% van de video bekijkt, stuurt het u de rest van de video en als u 100% van de video bekijkt, stuurt het u een koppeling naar een ander blogbericht. === <https://developers.google.com/youtube/iframe_api_reference>

**hoe te om Munchkin het Volgen Code** te produceren de volgende code van Munchkin staat u toe om bezoeken aan uw website te volgen. Er zijn drie typen Munchkin-code die hieronder worden beschreven, maar in dit voorbeeld gebruiken we de Asynchronous Munchkin tracking code. A) Eenvoudig: heeft de minste coderegels, maar is niet geoptimaliseerd voor het laden van webpagina&#39;s. Deze code laadt de jQuery-bibliotheek telkens wanneer een webpagina wordt geladen. B) Asynchroon: verkort de laadtijd van de webpagina. Deze code controleert of de jQuery-bibliotheek al bestaat, laadt deze als deze ontbreekt en gebruikt deze voor het uitvoeren van trackingcode zodra de rest van de webpagina is geladen. C) Asynchrone jQuery: verkort de laadtijd van de webpagina en verbetert ook de systeemprestaties. In deze code wordt ervan uitgegaan dat u al jQuery hebt en wordt niet gecontroleerd om deze te laden.

1. Klik op Beheer rechtsboven in de app.
1. Klik op Munchkin in de boomstructuur aan de linkerkant.
1. Selecteer Asynchroon bij Type code bijhouden.
1. Klik en kopieer de code voor het bijhouden van JavaScript die u op uw website wilt plaatsen. **de Code van YouTube** <https://developers.google.com/youtube/js_api_reference#EventHandlers> <https://developers.google.com/youtube/iframe_api_reference> speler.

`getCurrentTime()` Geeft de verstreken tijd in seconden sinds het afspelen van de video is gestart. `player.getDuration()` Geeft de duur in seconden van de video die momenteel wordt afgespeeld. `getDuration()` retourneert 0 totdat de metagegevens van de video zijn geladen. Dit gebeurt normaal gesproken vlak nadat de video wordt afgespeeld. Wanneer een niet-gekoelde gebruiker naar een pagina gaat met de trackingcode van Munchkin, wordt een nieuw cookie gemaakt in de browser van de gebruiker en wordt een nieuwe anonieme lead gemaakt in Marketo. Als de gebruiker al is gekookt en de gebruiker een bestaande lead is in Marketo, wordt het bezoek aan de pagina opgenomen in het activiteitenlogboek van de gebruiker in Marketo. **Steekproef van de Code aan de Gebruiker van het Koekje en Gebeurtenis van het Spoor** Plaats de volgende code op uw Web-pagina&#39;s recht vóór de `</body>` markering. De het landen pagina&#39;s die in Marketo worden gecreeerd bevatten automatisch het volgen code, zodat te hoeven u niet om deze code op hen te zetten. Dit codevoorbeeld roept de Munchkin API aan nadat het script is geladen:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Dit codevoorbeeld roept de Munchkin API aan nadat de gebruiker 5 seconden op de pagina is geweest en ook 500 pixels omlaag op de pagina heeft geschoven:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script> 
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked        
  
  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }
     
     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }   
 },5000); //Sets time delay before tracking user
});
</script>
```

**hoe te om het Bezoek van de Pagina te verifiëren door Anonieme Gebruiker werd geregistreerd in Marketo**

1. Klik op Analytics in het bovenste menu en klik vervolgens op New Report. Kies de Activiteit van de Pagina van het Web als rapporttype, en noem dan uw rapport.
1. Nadat u een rapport hebt gemaakt, klikt u op Slimme lijst. Selecteer vervolgens het filter Bezochte webpagina in het vak aan de rechterkant. Voer de webpagina in waarop u de trackingcode voor Munchkin plaatst.
1. Klik op Instellen. Selecteer Anonieme Bezoekers van ISPs, en verander optie aan Getoonde.
1. Klik op Rapport. De activiteit wordt nu bijgehouden op de webpagina die u hebt geselecteerd.
1. Dubbelklik op de lead record. Hiermee wordt het activiteitenlogboek weergegeven waarin u de specifieke pagina kunt zien die de anonieme gebruiker heeft bezocht.

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Voor pagina&#39;s met multimedia-inhoud wilt u bijvoorbeeld aangepaste tracking uitvoeren. U kunt bijvoorbeeld code voor het bijhouden van Munchkin-gegevens toevoegen aan de pagina en de Munchkin API gebruiken om gebeurtenissen in uw Marketo-instantie te genereren voor activiteiten zoals het afspelen van een video of het luisteren naar een audioclip. We raden je aan om code voor het bijhouden van Munchkin op de meeste of alle webpagina&#39;s te plaatsen. Munchkin-trackingcode wordt automatisch opgenomen in bestemmingspagina&#39;s die u met Marketo maakt. Gebruik deze aanroep om op te nemen dat de gebruiker iets heeft gedaan, zoals een pagina bezoeken in Ajax, Flash of een andere RIA-omgeving. De URL mag geen &quot; of een willekeurig domein bevatten, maar het kan verwijzen naar elke pagina, zelfs naar pagina&#39;s die niet bestaan. Als u URL-parameters wilt toevoegen, gebruikt u het argument params.
De gebeurtenis verschijnt als gebeurtenis van de Pagina van het Bezoek in het activiteitenlogboek van de gebruiker onder het domein van de roepende Web-pagina. Als u `mktoMunchkin()` voor het eerst aanroept, wordt altijd een bezoekgebeurtenis voor de webpagina gemaakt voor de huidige pagina. U hoeft `visitWebPage` niet aan te roepen tenzij u een extra webpaginabezoek wilt bijhouden. `mktoMunchkinFunction('visitWebPage', { url: '/MyFlashMovie/Story1', params: 'x=y&2=3' });` Opmerking: zorg dat u toegang hebt tot een ervaren JavaScript-ontwikkelaar. Marketo Technical Support is niet ingesteld als hulp bij het oplossen van problemen met aangepaste JavaScript. Met de Munchkin JavaScript API kunt u een extern websysteem integreren met uw Marketo-account. Bij sommige webontwikkeling kunt u nieuwe leads vastleggen of huidige leads bijwerken met bestaande toepassingen op uw website. Stel dat u een webtoepassing voor klantenregistratie hebt waarmee nieuwe klantgegevens worden vastgelegd. Met slechts een beetje programmeren, kunt u ook loodinformatie voor die gebruikers hebben die in Marketo worden gevangen, en een koekje van Marketo die voor toekomstige Web het volgen wordt geplaatst.

Daarnaast kunnen uw webontwikkelaars met een andere functie gegevens over webactiviteiten vastleggen en bijhouden in veelzijdige webomgevingen, zoals Flash of Ajax. Opmerking: als u over de juiste ontwikkelingsbronnen beschikt, kunt u beter onze SOAP API voor integratie gebruiken in plaats van deze API. De SOAP API is robuuster en heeft meer functionaliteit dan de Munchkin API. Marketo SOAP API-vereisten U moet de Munchkin JavaScript-code op uw webpagina opnemen om dit te laten werken. U vindt de benodigde scripttags in de zelfstudie voor Munchkin. Schakel ook de Munchkin API in, die ook wordt beschreven in de zelfstudie.
Onder Hood nadat u een Munchkin API-aanroep maakt, wordt de gebruiker automatisch in een cookie geplaatst als hij of zij geen cookie heeft. In Marketo wordt de gebeurtenis geregistreerd (klik op een koppeling, bezoek een webpagina of een nieuwe lead) in het activiteitenlog van de persoon. Als u de klikverbinding gebruikt of een Web-pagina vraag bezoekt, wordt de gebeurtenis toegevoegd aan het (gekende of anonieme) Logboek van de Activiteit van die lood. Als dit een nieuwe lood is en u de associeerde hoofdvraag gebruikt, wordt die lood een bekende lood en hun activiteitengeschiedenis zal worden bewaard. Als dit een bestaande lead is (op basis van overeenkomende e-mailadressen), worden eventuele gewijzigde of nieuwe waarden bijgewerkt in de record van die lead.

Hier is de algemene vorm van een `munchkinFunction` oproep. Neem deze als tags op in uw webpagina, waar u deze ook wilt aanroepen. U kunt dit net als elke andere JavaScript-functie aanroepen. U moet echter de functie Munchkin tracking `mktoMunchkin()` aanroepen voordat u `mktoMunchkinFunction()` -aanroepen kunt uitvoeren:

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"> mktoMunchkin("###-###-###"); mktoMunchkinFunction('function', { key: 'value', key2: 'value'}, 'hash');
```

Waar: `###-###-###` is de Munchkin-account-id voor uw accountfunctie de aanroep die u wilt maken dat params een array zijn met parameters die nodig zijn voor de aanroephash, is alleen nodig voor `associateLead`

Gepast op _1970-01-01_ door _Murta_

## Gegevens ophalen in Marketo

In de onderstaande presentatie ziet u de verschillende manieren waarop u gegevens in Marketo kunt opnemen. De focus ligt op formulieren, aangepaste objecten en integratie.

[ het krijgen van Gegevens in Marketo ](https://www.slideshare.net/MurtzaManzur/getting-data-into-marketo-35662408) van [ Murtza Manzur ](https://www.slideshare.net/MurtzaManzur)

Gepast op _2014-06-06_ door _Murta_

## Leads maken in een Workspace

Laten we zeggen dat je bedrijf twee divisies heeft: Noord-Amerika en Europa. Je wilt je leads segmenteren op basis van bedrijf-divisie in Marketo. U kunt dit bereiken met behulp van werkruimten. Dit is een functie in Marketo waarmee u de toegang tot leads kunt beperken. Om dit te doen, zou u een werkruimte voor Noord-Amerika en een andere voor Europa creëren. U kunt een lood in een bepaalde werkruimte dan tot stand brengen gebruikend [ syncLead API ](/help/soap-api/synclead.md). U zou het gebruiken van werkruimten en loodverdelingen moeten overwegen als uw organisatie heeft:

1. Afzonderlijke marketingteams voor meerdere productlijnen
1. Afzonderlijke marketingteams voor verschillende gebieden of landen
1. Een moedermaatschappij en dochterondernemingen
1. Een moedermaatschappij en wederverkopers

Wanneer u leadverdelingen en werkruimten gebruikt, kunt u:

1. Toegang tot leads beperken in uw organisatie
1. Toegang tot middelen in uw organisatie beperken
1. Elementen delen over marketingteams

Wij tonen u eerst hoe te om een werkruimte in Marketo via UI tot stand te brengen, en tweede hoe te om een lood aan die werkruimte te schrijven gebruikend [ syncLead API ](/help/soap-api/synclead.md). **Creërend een werkruimte van Workspace** A is een reeks lood en de activa van Marketo. In een werkruimte kunt u alleen de leads van die werkruimte en elementen (e-mails, campagnes, lijsten, enzovoort) in die werkruimte zien. Slimme campagnes in die werkruimte zijn alleen van invloed op leads in die werkruimte. De werkruimten in uw account bekijken:

1. Ga naar de pagina Workspaces &amp; Lead Partitions in de sectie Admin. De werkruimten worden weergegeven op het tabblad Werkruimten. 1. Klik op de knop Nieuwe Workspace op de menubalk op het tabblad Werkruimten om een nieuwe werkruimte te maken.
1. In het dialoogvenster moet u informatie toevoegen over de nieuwe werkruimte:

* **Naam van Workspace** - de naam van deze werkruimte aangezien het in de interface verschijnt
* **Beschrijving** - een facultatieve tekstbeschrijving van de werkruimte
* **de Verdelingen van de Lood** - die de lood in deze verdeling zichtbaar zijn.
* **Alle Verdelingen van het Lood** - zal lood van alle huidige en toekomstige verdelingen zien
* **Uitgezochte individuele verdelingen** - slechts leidt van die verdelingen (en geen toekomstige verdelingen) zijn zichtbaar
* **Primaire Verdeling van het Lood** - de lood die uw het landen pagina&#39;s bezoeken wordt toegevoegd aan deze verdeling door gebrek
* **Taal** - de bedrijfstaal van de werkruimte

Wij tonen u hoe te om API te gebruiken schrijft naar een bepaalde werkruimte leidt. Het eerste voorbeeld is een XML-aanvraag, het tweede een XML-reactie en het laatste voorbeeld is een Ruby-codevoorbeeld dat kan worden gebruikt om de XML-aanvraag te genereren. 1. Nadat u een lead hebt gemaakt, is Lead Partition een veld op Informatie over lead. SOAP-aanvraag voor `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP Response for requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Zie hieronder een voorbeeld van een Java-programma dat het hierboven beschreven scenario uitvoert.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
 
 
public class RequestCampaign {
 
    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";
             
            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();
             
            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);           
            String encryptString = requestTimestamp + marketoUserId ;
             
            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars); 
             
            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);
             
            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();
             
            request.setSource(ReqCampSourceType.MKTOWS);
             
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);
             
            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");
             
            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");
             
            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);
             
            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);
 
            SuccessRequestCampaign result = port.requestCampaign(request, header);
 
            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);
             
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

**hoe ik omhoog voor Werkruimten en de Verdelingen van het Lood teken?** Voor Marketo Enterprise-klanten hebt u gratis toegang tot werkruimten en leadpartities. Neem contact op met de manager van de klantenservice voor meer informatie over het inschakelen en implementeren van deze services. Neem voor andere klanten contact op met uw Marketo-verkoopmedewerker of stuur een e-mail naar ons verkoopteam voor meer informatie over dit product.

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _1970-01-01_ door _Murta_

## Updates release juni 2014

### Marketo Real-Time Personalization API

De Marketo Real-Time Personalization (RTP) JavaScript API breidt het geautomatiseerde verpersoonlijkingsvermogen van het platform uit. Zo kunt u gebeurtenissen bijhouden en een webpagina dynamisch aanpassen. Zie volledige documentatie [ hier ](/help/javascript-api/web-personalization.md).

Gepost op _2014-06-20_ door _Travis Kaufman_

## Een buitenlandse sleutel opslaan in Marketo

Wanneer het synchroniseren van contact en lood verslagen tussen systemen zoals merkgebonden CRM of gegevenspakhuis, is het een gemeenschappelijke vereiste om een loodverslag met een unieke systeemherkenningsteken te associëren. In Marketo kunt u een loodverslag door a [ syncMultipleLeads API ](/help/soap-api/syncmultipleleads.md) vraag tot stand brengen of bijwerken gebruikend uw uniek systeemherkenningsteken. Hiervoor slaat u uw unieke systeem-id (primaire sleutel) op als een externe sleutel in Marketo. De naam van dit veld in Marketo voor het opslaan van een buitenlandse sleutel is foreignSysPersonId. Hier zijn drie belangrijke dingen om op te merken:

1. foreignSysPersonId is niet zichtbaar in de gebruikersinterface van Marketo. Het wordt daarom aanbevolen om ook een aangepast kenmerkveld met deze waarde te vullen.
1. foreignSysPersonId is uniek aan een lood, maar een lood kan meer dan één foreignSysPersonId hebben.
1. foreignSysPersonId kan niet worden bijgewerkt of worden geschrapt, maar kan aan een ander verslag worden opnieuw toegewezen.

Wij tonen hoe te om een vraag aan [ syncMultipleLeads API ](/help/soap-api/syncmultipleleads.md) te maken om een waarde foreignSysPersonId aan twee bestaande loodverslagen in Marketo te schrijven. **hoe te om foreignSysPersonId te schrijven Gebruikend syncMultipleLeads API** U kunt een nieuw loodverslag opnemen en ForeignSysPersonId specificeren. U kunt het aan een bestaande lood ook toevoegen door zowel Marketo identiteitskaart als foreignSysPersonId te specificeren. We lopen je door de laatste zaak. **Verzoek XML voor syncMultipleLeads SOAP API Vraag**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809934544BFABAE58E5D27</mktowsUserId>
      <requestSignature>b5e21953ae9f1b263e644da5eccce9ff33802513</requestSignature>
      <requestTimestamp>2013-08-01T18:22:30-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsSyncMultipleLeads>
      <leadRecordList>
        <leadRecord>
          <leadId>1090240</leadId>
          <foreignSysPersonId>1224191</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1000</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1000</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
        <leadRecord>
          <leadId>1090239</leadId>
          <foreignSysPersonId>1224192</foreignSysPersonId>
          <leadAttributeList>
            <attribute>
              <attrName>Company</attrName>
              <attrValue>Marketo1001</attrValue>
            </attribute>
            <attribute>
              <attrName>Phone</attrName>
              <attrValue>650-555-1001</attrValue>
            </attribute>
          </leadAttributeList>
        </leadRecord>
      </leadRecordList>
      <dedupEnabled>true</dedupEnabled>
    </ns1:paramsSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Reactie XML voor syncMultipleLeads SOAP API Vraag**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successSyncMultipleLeads>
      <result>
        <syncStatusList>
          <syncStatus>
            <leadId>1090240</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
          <syncStatus>
            <leadId>1090239</leadId>
            <status>UPDATED</status>
            <error xsi:nil="true" />
          </syncStatus>
        </syncStatusList>
      </result>
    </ns1:successSyncMultipleLeads>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**zie onder een programma van de steekproefRuby dat hierboven verzoekXML zal uitvoeren.**

```java
require 'savon' # Use version 2.0 Savon gem
require 'date'

mktowsUserId = "" # CHANGE ME
marketoSecretKey = "" # CHANGE ME
marketoSoapEndPoint = "" # CHANGE ME
marketoNameSpace = "<http://www.marketo.com/mktows/>"

# Create Signature
Timestamp = DateTime.now
requestTimestamp = Timestamp.to_s
encryptString = requestTimestamp + mktowsUserId
digest = OpenSSL::Digest.new('sha1')
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)
requestSignature = hashedsignature.to_s

# Create SOAP Header
headers = { 
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,      
 "requestTimestamp"  => requestTimestamp 
 }
}

client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')

# Create Request
request = {
        :lead_record_list => {
                :lead_record => {
                        :lead_id => "1090240",
                        :foreign_sys_person_id => "1224191",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1000" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1000" }
                }
        },
        :lead_record! => {
                        :lead_id => "1090239",
                        :foreign_sys_person_id => "1224192",
                :lead_attribute_list => {
                        :attribute => {
                                :attr_name => "Company",
                                :attr_value => "Marketo1001" },
                         :attribute! => {
                                :attr_name => "Phone",
                                :attr_value => "650-555-1001" }
                }
        }
        },
    :dedup_enabled => "True"
}

response = client.call(:sync_multiple_leads, message: request)

puts response
```

 

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-06-27_ door _Murta_

## Het e-mailadres van een lead bijwerken

Stel dat een gebruiker een Marketo-formulier op uw site invult. Wat gebeurt er? Marketo cookies de gebruiker en koppelt deze aan de e-mail die ze hebben verzonden. Wat gebeurt er als de gebruiker de volgende keer uw website bezoekt en hetzelfde formulier opnieuw invult met een andere e-mail. Wat gebeurt er? Marketo maakt een nieuwe lead record en overschrijft de eerste cookie in de browser van de gebruiker. De gebruiker is nu een nieuwe/andere lead in Marketo. Wij tonen u vier manieren om het e-mailadres van een lood in Marketo met inbegrip van de [ syncLead API methode ](/help/soap-api/synclead.md) bij te werken, het douanegebied in een vormmethode, Marketo UI, en door een lijst in te voeren. **via syncLead API** U kunt [ syncLead API ](/help/soap-api/synclead.md) gebruiken om een loodverslag bij te werken gebruikend hun identiteitskaart van Marketo en nieuw e-mailadres. Verzoek om XML voor `syncMultipleLeads` SOAP API-aanroep

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">  
  <SOAP-ENV:Header>  
    <ns1:AuthenticationHeader>  
      <mktowsUserId>bigcorp1_461839624B16E06BA2D663</mktowsUserId>  
      <requestSignature>92f05a7be4838ae1c0e5aafe814891ee72968a08</requestSignature>  
      <requestTimestamp>2013-07-31T12:38:47-07:00</requestTimestamp>  
    </ns1:AuthenticationHeader>  
  </SOAP-ENV:Header>  
  <SOAP-ENV:Body>  
    <ns1:paramsSyncLead>  
      <leadRecord>  
        <leadId>1090240</leadId>  
        <Email>t@t.com</Email>  
      </leadRecord>  
      <returnLead>false</returnLead>  
    </ns1:paramsSyncLead>  
  </SOAP-ENV:Body>  
</SOAP-ENV:Envelope>
```

Response XML for syncMultipleLeads SOAP API Call

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">  
  <SOAP-ENV:Body>  
    <ns1:successSyncLead>  
      <result>  
        <leadId>1090240</leadId>  
        <syncStatus>  
          <leadId>1090240</leadId>  
          <status>UPDATED</status>  
          <error xsi:nil="true" />  
        </syncStatus>  
        <leadRecord xsi:nil="true" />  
      </result>  
    </ns1:successSyncLead>  
  </SOAP-ENV:Body>  
</SOAP-ENV:Envelope>
```

Zie onder een voorbeeldprogramma van Ruby dat hierboven verzoekXML zal uitvoeren.

```java
require 'savon' # Use version 2.0 Savon gem  
require 'date'  
  
mktowsUserId = "" # CHANGE ME  
marketoSecretKey = "" # CHANGE ME  
marketoSoapEndPoint = "" # CHANGE ME  
marketoNameSpace = "<http://www.marketo.com/mktows/>"  
  
# Create Signature  
Timestamp = DateTime.now  
requestTimestamp = Timestamp.to_s  
encryptString = requestTimestamp + mktowsUserId  
digest = OpenSSL::Digest.new('sha1')  
hashedsignature = OpenSSL::HMAC.hexdigest(digest, marketoSecretKey, encryptString)  
requestSignature = hashedsignature.to_s  
  
# Create SOAP Header  
headers = {   
 'ns1:AuthenticationHeader' => { "mktowsUserId" => mktowsUserId, "requestSignature" => requestSignature,        
 "requestTimestamp"  => requestTimestamp   
 }  
}  
  
client = Savon.client(wsdl: '<http://app.marketo.com/soap/mktows/2_3?WSDL>', soap_header: headers, endpoint: marketoSoapEndPoint, open_timeout: 90, read_timeout: 90, namespace_identifier: :ns1, env_namespace: 'SOAP-ENV')  
  
# Create Request  
request = {  
 :lead_record => {  
  :Email => "<t@t.com>",  
  :lead_id => "1090240",  
 :return_lead => "false"  
}  
  
response = client.call(:sync_lead, message: request)  
  
puts response
```

**via een Gebied van de Douane in een Vorm** U kunt een douaneveld voor het &quot;Nieuwe E-mailadres&quot;in Marketo tot stand brengen. Vervolgens vraagt u de gebruiker een formulier in te vullen dat dit nieuwe veld bevat. Maak vervolgens in Marketo een programma waarmee de gegevenswaarde van het veld E-mailadres van het systeem wordt gewijzigd met de token `{{lead.newEmailAddress}}` wanneer het nieuwe aangepaste veld Nieuw e-mailadres wordt gewijzigd. **via Marketo UI** U kunt het e-mailadres van een lood via de UI van Marketo manueel bijwerken. Hier is a [ hulpartikel ](https://nation.marketo.com/) dat beschrijft hoe te om dit (login van Marketo te doen vereist om het artikel te zien). **via het Invoeren van een Lijst** U kunt het e-mailadres van een lood bijwerken gebruikend de invoer een lijstmethode in Marketo die [ hier ](https://nation.marketo.com/) wordt beschreven (login van Marketo die wordt vereist om artikel te zien).  

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _1970-01-01_ door _Murta_

## Een tweede e-mailadres opslaan voor een lead

Laten we zeggen dat u de score van een lead in Marketo wilt wijzigen met de API&#39;s. Dit is mogelijk om met REST API te doen gebruikend het Create/Update eindpunt van de Lood. Als u meer dan één e-mail op een lead record wilt opslaan, moet u een aangepast veld maken en de tweede e-mail daar opslaan. Hier is een hulpartikel met meer informatie: Hieronder is een codesteekproef in Ruby die toont hoe te om deze vraag te maken.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>" 
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab" 
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] } 

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

Gepast op _2015-02-20_ door _Murta_

## Een aangepast veld maken in Marketo en dit veld bijwerken via AP

Stel dat u aanvullende gegevens over uw leads hebt die niet in de standaard Marketo-velden passen. Dit aangepaste veld kan bijvoorbeeld een score van derden zijn. U kunt een douanegebied in Marketo voor uw derdescore tot stand brengen, en dan de waarde van dit gebied via of Marketo [ REST APIs ](https://developer.adobe.com/marketo-apis/) of [ SOAP APIs ](https://experienceleague.adobe.com/nl/docs/marketo-developer/marketo/soap/activity-type-filters) bijwerken. We tonen u eerst hoe u een aangepast veld maakt in Marketo en vervolgens hoe u dit veld bijwerkt met de REST API.

### Een aangepast veld maken in Marketo

1. Klik onder Beheer op Veld beheren.
1. Klik op de knop Nieuw aangepast veld.
1. Kies het veldtype. Hierdoor wordt de weergave in slimme lijsten en Forms in Marketo gewijzigd.
1. Voer de naam in zoals u deze wilt weergeven in Marketo. Kies de naam en de API-naam zorgvuldig omdat het wijzigen van de naam van velden moeilijk kan zijn en in sommige situaties niet mogelijk.
1. Het aangepaste veld dat u hebt gemaakt, is nu toegankelijk via de API&#39;s.

### Aangepast veld bijwerken via REST API

In de vorige sectie hebben we een aangepast veld met de naam `myCustomField` gemaakt met de tekenreeks voor het gegevenstype. Om de waarde van dat gebied bij te werken, gebruiken wij het REST API eindpunt genoemd Create/Update Leads. Voordat u een aanvraag kunt indienen bij de REST API, moet u de verificatie uitvoeren. Dit is buiten het werkingsgebied van dit artikel, maar de diepgaande informatie [ is beschikbaar op de plaats van de ontwikkelaars van Marketo ](/help/rest-api/authentication.md).

**Eindpunt**

`/rest/v1/leads.json`

**Lichaam van het Verzoek**

```json
{  
   "action":"createOrUpdate",
   "input":[  
      {  
         "email":"<example@example.com>",
         "myCustomField":"examplestring"
      }
   ]
}
```

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-08-19_ door _Murta_

## Integratie van Unbounce en Marketo

**NOTA: Dit is een gast blogpost door Fab Capodicasa. Hij is een Marketo-Verklaarde adviseur bij [ Hoosh Marketing ](http://hooshmarketing.com.au/), een de agentschappartner van Marketo LaunchPoint die in B2C specialiseert. Hij heeft de afgelopen 13 jaar zowel in SaaS als in de marketing gewerkt. Zijn achtergrond is een mengsel van hardcore IT, directe marketing, en ondernemingsverkoop. Fab is ook een vroegere werknemer van Marketo.**

**Overzicht** in dit artikel tonen wij hoe te om Unbounce, een populair het landen paginahulpmiddel, met Marketo te integreren. We laten u eerst zien hoe u Marketo-tracking invoegt in Unbounce en vervolgens hoe u Unbounce-formulieren kunt wijzigen om gegevens rechtstreeks in Marketo in te voegen. De uitdaging van het integreren van Unbounce met Marketo is dat Unbounce niet toelaat standaardgebieden worden anders genoemd (bijvoorbeeld, kan first_name niet in FirstName worden veranderd). Het is ook niet toegestaan dat veldlabels verschillen van veldnamen. Bij deze integratie is JavaScript betrokken, dat de bestaande formulieren bijstelt om ze compatibel te maken met Marketo. Ik adviseer dat u minstens een beginnersniveau van JavaScript en een middenniveau van Marketo kennis hebt om de taken in dit artikel te voltooien.
**Deel 1: Voeg Marketo het Volgen Code aan Unbounce** toe Toevoegend Munchkin het volgen manuscript van Marketo aan Unbounce pagina&#39;s toevoegen wordt vereist zowel voor analyse als de vormintegratie om te werken. Voer de volgende stappen uit: kopieer uw Munchkin-code van Marketo: ga naar Admin -> Munchkin en kopieer de versie &#39;Eenvoudig&#39; van de JavaScript. Open de openingspagina voor het opheffen van de stuit en klik op JavaScript-> Nieuwe JavaScript toevoegen.  Klik op Toevoegen, roep het script &#39;Munchkin&#39; aan, selecteer &#39;Voor body End-tag&#39; en plak de Munchkin-code. Klik op de knop Gereed. Voor toekomstige Unbounce pagina&#39;s, ga u naar JavaScript en laat het manuscript van Munchkin toe wij creeerden. Het is niet nodig het opnieuw te maken.
**Deel 2: Converteer de Unbounce Vorm in een Vorm van Marketo** nu moeten wij de Unbounce vorm wijzigen door sommige nieuwe verborgen gebieden en JavaScript toe te voegen om uw Unbounce landende pagina&#39;s toe te staan om loodinformatie direct in Marketo voor te leggen. We maken eerst een plaatsaanduidingsformulier voor Marketo. Maak in Marketo een leeg formulier en keur dit goed.

Dit is het proxyformulier in Marketo dat het Unbounce-formulier vertegenwoordigt. Voeg verborgen velden toe aan het formulier voor ongewenste bijschriften. Deze verborgen velden worden door Marketo vereist om te bepalen op welke Marketo-instantie en op welke formulier en gebruikerssessie dit formulier wordt verzonden. Open het formulier in Unbounce door erop te dubbelklikken. Voeg een verborgen veld met de naam `_mkt_trk` toe. Voeg een tweede verborgen veld met de naam `formid` toe.  233 moet worden vervangen door de id van het formulier, die u vindt in de insluitcode van het Marketo-formulier in Marketo. Open in Marketo het formulier en selecteer Formulierhandelingen->Code insluiten. Voeg een verborgen veld met de naam `returnurl` toe. `http://hooshmarketing.com.au/thank-you` moet worden vervangen door een follow-up-URL. Dit is de URL waarnaar gebruikers moeten worden omgeleid nadat ze het formulier hebben verzonden. Dit kan bijvoorbeeld uw pagina voor bedankt zijn.
**Deel 3: Directe Unbounce Vorm aan Marketo** De follow-up URL is de pagina waaraan uw lood zal worden opnieuw gericht nadat hun lood aan Marketo wordt voorgelegd. Voer de volgende stappen uit in Unbounce: klik op het formulier. Wijzig de sectie Formulierbevestiging. Wijzig de bevestiging in Formuliergegevens verzenden naar een URL. Stel de URL in op de gewenste vervolgpagina. `fpmarkets` moet worden vervangen door de tekenreeks van uw Marketo-account, die u kunt vinden in Marketo onder Beheer->Landingpagina&#39;s.
**Deel 4: Voeg JavaScript aan de Unbounce Pagina** toe Deze JavaScript zal de vorm om compatibel met Marketo omzetten en het voorleggen aan Marketo. Voer de volgende stappen uit bij Stuiteren: Open de openingspagina voor Stuiteren en klik op JavaScript-> Nieuwe JavaScript toevoegen. Klik op Toevoegen, roep het script &#39;Marketo Form Convert&#39; aan en selecteer &#39;Before Body End Tag&#39;. Plak de JavaScript-code hieronder:

```javascript
var MARKETO_MUNCHKIN_ID='614-CGT-700';
var MARKETO_ACCOUNT_STRING = 'fpmarkets';
  
var UNBOUNCE_MARKETO_FIELD_MAP = new Object(); 
  
//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
UNBOUNCE_MARKETO_FIELD_MAP['last_name'] = 'LastName';
UNBOUNCE_MARKETO_FIELD_MAP['phone_number'] = 'Phone';

function getMarketoField(k) {
    return UNBOUNCE_MARKETO_FIELD_MAP[k];
}
  
  
var formFields = [];
var hiddenClonedFields = [];
var firstForm = document.forms[0];  
  
//Convert Unbounce form names to Marketo field names  
for(i=0; i<firstForm.elements.length; i++){
 var formField = firstForm.elements[i];
 var newFieldName = getMarketoField(formField.name);
  
  if(newFieldName != undefined) {
        
    
    //save original field as hidden field
    var hiddenField = document.createElement("input");
    hiddenField.setAttribute("type", "hidden");
    hiddenField.setAttribute("name", formField.name);
    hiddenField.setAttribute("id", formField.id);
    hiddenClonedFields.push(hiddenField);
    
    //change original field
    console.log ( 'Changed form field name: ' + formField.name + '=>' + newFieldName );
    formField.name = newFieldName;
    formField.id = newFieldName;
    formFields.push(formField);
    
    
  } else {
    console.log ( 'Couldn't map:' + formField.name );
  }
}
  
//Add hidden cloned Unbounce fields to form
//for Unbounce validation
for(i=0; i<hiddenClonedFields.length; i++){
    firstForm.appendChild(hiddenClonedFields[i]);
    formFields[i].onchange = (function(hf) {
        return function(event) {
            hf.value = event.target.value;
          console.log('Changed hidden cloned field:' + hf.name + '=>' + hf.value);
        };
    }(hiddenClonedFields[i]));
    console.log ( 'Added cloned field: ' + hiddenClonedFields[i].name );
}
  
   
//Add MunchkinId to form
var input = document.createElement("input");
input.setAttribute("type", "hidden");
input.setAttribute("name", "munchkinId");
input.setAttribute("value", MARKETO_MUNCHKIN_ID);
firstForm.appendChild(input);
console.log('Added hidden field:' + input.name + '=' + input.value);
```

Als u aangepaste velden hebt met API-namen die niet compatibel zijn met Unbounce, kunt u deze toevoegen aan de toewijzing in de JavaScript. Als een van uw aangepaste velden in Marketo bijvoorbeeld `Comments_c` was, maar u wilt dat het veldlabel als Opmerkingen wordt weergegeven, dan kunt u de veldnaam niet wijzigen in `Comments_c` .

```
//default field mappings
UNBOUNCE_MARKETO_FIELD_MAP['comments'] = 'Comments_c';
UNBOUNCE_MARKETO_FIELD_MAP['first_name'] = 'FirstName';
UNBOUNCE_MARKETO_FIELD_MAP['email'] = 'Email';
```

_comments is de naam van het veld in Unbounce. _Comments_c_ is de naam van het gebied in Marketo. Voor toekomstige Unbounce pagina&#39;s, zult u eenvoudig naar JavaScript gaan en het manuscript van Munchkin toelaten wij creeerden. Het is niet nodig het opnieuw te maken.
**Deel 5: Het testen** de laatste stap is deze vormintegratie te testen werkt. Maak een trigger in Marketo die activeert op het Marketo-formulierinvullen en zorg ervoor dat de leads correct worden ingevoegd in Marketo. Nadat het formulier is verzonden, moet de pagina u omleiden naar de URL voor de follow-up.

GEPast op _2014-08-04_ door _

## Updates van de release van juli 2014

### Munchkin-updates

De nieuwe versie van Munchkin is 141. Versie 142 wordt niet ondersteund en wordt begin september 2011 verwijderd. In Versie 142 waren er openbaar toegankelijke functies die niet op de site van de ontwikkelaars waren gedocumenteerd. Deze niet-gedocumenteerde functies zijn niet langer openbaar. Gedocumenteerde functies op de site van ontwikkelaars worden op lange termijn ondersteund.

### RTP-updates

De RTP API heeft een nieuwe functie genoemd krijgt de Gegevens van de Bezoeker. Met deze RTP API-aanroep kunt u realtime bezoekersgegevens ophalen, zoals organisatie, industrie, locatie en segmentcodeovereenkomst.

Gepast op _2014-07-30_ door _Murta_

## Meerdere Munchkin-volgcodes gebruiken op één pagina

Stel dat u meerdere Marketo-instanties hebt en dat u webtraceringsgebeurtenissen wilt verzenden, zoals paginabezoeken of aangeklikte koppelingen naar deze meerdere exemplaren, dan is het mogelijk dit te doen met Munchkin. Marketo houdt bezoekers van uw website per domein bij (bijvoorbeeld &quot;marketo.com&quot;). Als u dit Munchkin-script host op een ander domein dan uw primaire domein (bijvoorbeeld &quot;bedrijf.com&quot;), worden deze bezoekers weergegeven als anonieme leads totdat ze een formulier invullen op dat andere domein. Hiervoor voegt u de parameter `altIds` toe aan uw `Munchkin.init` -aanroep. De parameter altIds bevat een array van de extra Munchkin-id&#39;s waarnaar webgebeurtenissen moeten worden verzonden. Vervang met behulp van het onderstaande voorbeeld de gemarkeerde Munchkin-id&#39;s (XXX-XXX-XXX, YYY-YYY en ZZZ-ZZZ-ZZZ) door de Munchkin-id&#39;s van elke Marketo-instantie waar de trackinggegevens moeten worden verzonden.

```javascript
<script src="http://munchkin.marketo.net/munchkin.js" type="text/javascript"></script>
<script>Munchkin.init('XXX-XXX-XXX', { altIds:['YYY-YYY-YYY', 'ZZZ-ZZZ-ZZZ'] });</script>
```

Voor extra informatie over de initialisatieparameters van Munchkin, zie [ dit document ](/help/javascript-api/configuration.md).

Gepast op _2014-08-08_ door _Murta_

## Munchkin integreren met Google-tagbeheer

Met Google Tag Manager kunt u tags toevoegen aan uw website. Eerder dan manueel elk volgend manuscript zoals Munchkin aan de broncode van uw website toevoegen, kunt u [ de Manager van de Markering van Google ](https://marketingplatform.google.com/about/tag-manager/) op uw plaats zetten, en dan markeringen zoals [ Munchkin ](/help/javascript-api/lead-tracking.md) toevoegen door UI van de Manager van de Markering van Google. In dit artikel laten we eerst zien hoe we code voor het bijhouden van Munchkin&#39;s kunnen genereren in Marketo. Vervolgens laten we zien hoe we deze code voor het bijhouden van Munchkin kunnen toevoegen aan Google Tag Manager.

### Munchkin-trackingcode genereren

1. Klik **Admin** bij het hoogste recht van app.
1. Klik **Munchkin** in de boom op de linkerzijde.
1. Selecteer **Asynchroon** voor het Volgen van het Type van Code.
1. Klik op de trackingcode voor JavaScript en kopieer deze.

**hoe te om Munchkin het Volgen Code aan de Manager van de Markering van Google toe te voegen**

1. Login aan uw rekening van de Manager van de Markering van Google en **voeg een nieuwe markering toe.**
1. Creeer een nieuwe **Markering van HTML van de Douane**.
1. Kopieer en kleef uw code van Munchkin in het **HTML** gebied en klik **verdergaan**.
1. Selecteer **Vuur op Alle Pagina&#39;s** en klik **creeer Markering.** Nota: Als u een uiterst hoge verkeerswebsite hebt, kunt u secties van uw plaats uitsluiten gebruikend **Vuur op sommige Pagina&#39;s**.
1. Klik op Opslaan en controleer vervolgens of de trackingcode voor Munchkin nu op uw website wordt geladen.

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-08-05_ door _Murta_

## Lichtbak verbergen na formulierverzending

### Overzicht

De huidige, door Marketo gegenereerde insluitcode van de lichtbak verdwijnt niet wanneer het formulier wordt verzonden. De pagina wordt opnieuw geladen nadat het formulier is verzonden en het formulier wordt opnieuw weergegeven. Als u een lichtbak wilt maken die zich na het verzenden van het formulier verbergt, volgt u de onderstaande stappen.

### Instructies

1. Nadat u het formulier in Marketo hebt gemaakt, genereert u uw insluitcode. Klik hiertoe op Formulierhandelingen en klik vervolgens op Lichtbak als het type code. Kopieer en plak deze code in een teksteditor, zodat u deze in de volgende stap kunt bewerken.
1. Vervang alle code na &quot;(form)&quot; in de insluitcode door de onderstaande code:

```javascript
{
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Na stap 2 moet de voltooide versie eruit zien zoals de onderstaande code. De code kan nu op uw website worden gebruikt.

```javascript
<script src="http://app-sj04.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_0000"></form>
<script>MktoForms2.loadForm("http://app-sj04.marketo.com", "AAA-BBB-CCC", 0000, function (form){
var lightbox = MktoForms2.lightbox(form).show();
form.onSuccess(function(){
lightbox.hide();
return false;
});
});
</script>
```

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-08-21_ door _Murta_

## Handleiding Snel starten voor Marketo REST API

In deze handleiding ziet u hoe u de Marketo REST API binnen tien minuten voor het eerst aanroept. Wij tonen u hoe te om één enkele lood terug te winnen gebruikend [ krijg Lood door Identiteitskaart ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) REST API eindpunt. Om dit te doen, lopen wij u door het authentificatieproces om een toegangstoken te produceren, die u gebruikt om een verzoek van HTTP GET te maken om [ Lood door Id ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) te krijgen. Vervolgens beschikken we over de code om een aanvraag in te dienen die informatie over leads retourneert die is opgemaakt als JSON.

### Een verificatietoken genereren

>[!NOTE]
> Vanaf juni 2025 wordt het verificatietoken niet meer ondersteund. U moet de koptekst Verificatie gebruiken.
>


Met een aangepaste service in Marketo kunt u beschrijven tot welke gegevens uw toepassing toegang heeft en definiëren. U moet als beheerder van Marketo worden aangemeld om de Dienst van de Douane tot stand te brengen en die dienst met één enkele API-Enige gebruiker te associëren.

1. Ga naar het beheergebied van de Marketo-toepassing.
1. Klik op de knoop van Gebruikers &amp; van Rollen op het linkerpaneel.
1. Maak een nieuwe rol. Maak de lijst met rolinachtigingen zichtbaar door op Toegang API te klikken. Blader omlaag en selecteer alleen de machtigingen die u nodig hebt. In dit geval selecteren we gewoon Lead-machtiging alleen-lezen.
1. De volgende stap bestaat uit het maken van een gebruiker met alleen een API en het koppelen van deze gebruiker aan de API-rol die u in de vorige stap hebt gemaakt. U kunt dit doen door het selectievakje Alleen API in te schakelen op het moment dat de gebruiker wordt gemaakt.
1. Een aangepaste service is vereist om uw clienttoepassing op unieke wijze te kunnen identificeren. Als u een aangepaste toepassing wilt maken, gaat u naar het scherm Admin > LaunchPoint en maakt u een nieuwe service.
1. Geef de weergavenaam op, kies &quot;Aangepast&quot; servicetype, geef de beschrijving op en geef het e-mailadres van de gebruiker dat u in stap 1 hebt gemaakt. We raden u aan een beschrijvende weergavenaam te gebruiken die het bedrijf of het doel van deze aangepaste REST API-service vertegenwoordigt.
1. Klik op de koppeling &quot;Details weergeven&quot; in het raster om de client-id en het clientgeheim op te halen. Uw cliënttoepassing kan Cliënt ID en Geheim gebruiken om een toegangstoken te produceren.
1. Kopieer en plak uw verificatietoken in een teksteditor. Uw verificatietoken ziet er ongeveer als volgt uit:

`cdf01657-110d-4155-99a7-f986b2ff13a0:int`

### URL van eindpunt bepalen

Als u een aanvraag wilt indienen bij de Marketo API, moet u de Marketo-instantie opgeven in de URL van het eindpunt. Alle niet-bulk API-aanvragen aan de Marketo REST API volgen de onderstaande indeling:

`<REST API Endpoint URL>/rest/`

De URL van het eindpunt van de REST API vindt u in het deelvenster Marketo Admin > Webservices. De URL-structuur van het eindpunt van Marketo moet er ongeveer als volgt uitzien:

`https://100-AEK-913.mktorest.com/rest/v1/lead/{id}.json`

**Nota: Bulk API URLs is niet vooraf bepaald met &quot;/rest/&quot;. Voor Bulk API, zou u de gastheer slechts moeten gebruiken, en de aangewezen weg als zo toevoegen:**

`https://100-AEK-913.mktorest.com/bulk/v1/leads/export/create.json`

### Hoe te om de Token van de Authentificatie te gebruiken om Vraag krijg Lood door AP van identiteitskaart

In de vorige secties, produceerden wij een authentificatietoken en vonden het eindpunt URL. Wij zullen nu een verzoek aan een REST API eindpunt genoemd [ indienen krijg Lood door Id ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). De eenvoudigste manier om uw verzoek in te dienen bij de Marketo REST API is de URL te plakken in de adresbalk van uw webbrowser. Volg onderstaande notatie:

`https://<REST API Endpoint URL for your Marketo instance>/rest/v1/<API that you are calling>?access_token=<access_token>`

### Voorbeeld

`https://100-AEK-913.mktorest.com/rest/v1/lead/318581.json?access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

Als uw vraag succesvol is, dan keert het JSON met het formaat hieronder terug:

```json
{
    "requestId": "d82a#14e26755a9c",
    "result": [
        {
            "id": 318581,
            "updatedAt": "2015-06-11T23:15:23Z",
            "lastName": "Doe",
            "email": "<jdoe@marketo.com>",
            "createdAt": "2015-03-17T00:18:40Z",
            "firstName": "John"
        }
    ],
    "success": true
}
```

Als u in het leren van meer over Marketo REST APIs geinteresseerd bent, is dit a [ goede plaats om ](https://developer.adobe.com/marketo-apis/) te beginnen.

Gepost op _2015-09-04_ door _David_

## Marketo-volgcookie verwijderen

Vraag: &quot;We hebben op onze website een formulier ingesteld waarmee het verkoopteam personen kan afmelden die mondeling hebben gevraagd om te worden verwijderd uit een e-mailbericht. Wat we echter zien, is wanneer ze in de e-mail komen en melden dat ze met dat e-mailadres zijn geassocieerd en berichten beginnen te ontvangen voor hun activiteiten op onze website. Het formulier dat we hebben, is momenteel een ingesloten formulier. Heeft iemand een manier gevonden om het bijhouden van koken op een ingesloten formulier uit te schakelen, dan begrijp ik hoe ik het kan doen met een Marketo-landingspagina.&quot; We kunnen dit doen. Als u dit wilt implementeren, versnelt u het verlopen van de cookie. Nadat de cookie door het formulier is gemaakt, kunt u deze direct laten verlopen. Omdat de cookie is verlopen, verwijdert de browser deze automatisch. Om dit proces te starten, gebruiken we de native formulierfunctionaliteit om aan te roepen onder de functie `deleteCookie` .

Gepost op _2014-08-26_ door _Travis Kaufman_

## Een Marketo-landingpagina integreren met Wordpress

Stel dat uw website is gemaakt met Wordpress en u wilt een Marketo-bestemmingspagina insluiten op een van de pagina&#39;s. Dit is mogelijk met een iframe. Met een iframe kunt u een pagina in een andere pagina insluiten. In dit artikel ziet u hoe u een Marketo-landingspagina insluit op een Wordpress-pagina. **het kiezen van een Insteekmodule Wordpress** Wordpress Er zijn aantal hier is een insteekmodule Wordpress die u laat: `http://wordpress.org/plugins/iframe/` hier zijn sommige pro&#39;s en con&#39;s op het gebruiken van deze benadering: `http://www.elixiter.com/marketo-landing-page-and-form-hosting-options/` Grote post Murtza! Colby, het iframe behoudt het deel waar u na het verzenden van het formulier naar een PDF bent omgeleid. We hebben iframes lang op onze site gebruikt. Geweldig bericht Murtza! Colby, het iframe behoudt het deel waar u na het verzenden van het formulier naar een PDF bent omgeleid. We hebben iframes lang op onze site gebruikt. Als u URL-parameters van de hoofd-URL naar het iframe wilt doorgeven, moet u wel wat coderen. Zorg er ook voor dat de Google Analytics juist is ingesteld. U wilt niet dat paginaweergaven twee keer worden geteld voor elk bezoek aan de pagina met het iframe.

Gepast op _1970-01-01_ door _Murta_

## Optimalisatie integreren met een Marketo-bestemmingspagina

[ Optimizely ](https://www.optimizely.com/) geeft u de capaciteit om het testen A/B, het multipage, en het multivariate testen op uw plaats te leiden. U kunt Optimizely gebruiken met een Marketo-landingspagina. Dit doet u als volgt:

1. Zoek en kopieer het codefragment Optimizely.** Ga naar het dashboard in Optimizely en klik op de koppeling Projectcode. Kopieer de coderegel die in pop-up verschijnt.
1. Meld u aan bij Marketo en selecteer de sjabloon van de bestemmingspagina. Klik vervolgens op Concept bewerken.**
1. Klik op Handelingen voor bestemmingspagina. Klik vervolgens op Metacodes van pagina bewerken**
1. Plak het codefragment Optimizely in de sectie Custom HEAD HTML en klik op Opslaan.
1. Test uw landingspagina om te bevestigen dat het fragment Optimizely werkt

Gepast op _2014-09-18_ door _Murta_

## Zoeken op volledige naam via Marketo REST API

**Vraag:** is er een manier om lood te vragen gebruikend Marketo APIs met enkel de volledige naam van een lood?
**Antwoord:** het is niet direct mogelijk. Met de hieronder beschreven tijdelijke oplossing kunt u dit echter wel doen.

1. Maak een aangepast veld met de naam Volledige naam in Marketo.
1. Het gebruik of [ getMultipleLeads ](/help/soap-api/getmultipleleads.md) SOAP API of [ krijgt Veelvoudige Leads door het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) om uw loodgegevensbestand te vragen. Neem uw voornaam en achternaam als kenmerken op in uw aanvraag voor REST- of SOAP-API&#39;s.
1. Nadat u uw loodgegevensbestand vraagt, schakelt &quot;Voornaam&quot;en &quot;Achternaam&quot;voor elke lood samen, en slaat deze gegevens in een kolom &quot;Fullname&quot;op. 1. Gebruik [ syncMultipleLeads ](/help/soap-api/syncmultipleleads.md) SOAP API om deze gegevens aan &quot;Fullname&quot;douanegebied te duwen. Alternatief, kunt u de [ Lood van de Invoer gebruiken ](/help/rest-api/leads.md) API, of een CSV of XLS invoeren gebruikend Marketo UI.
1. Nu, kunt u door volledige naam vragen gebruiken [ krijgt Veelvoudige Leidingen door Type API van de Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) om naar dit douanegebied te zoeken. Geef &quot;Fullname&quot; op als &quot;filterType&quot; en &quot;filterValue&quot; als &quot;Joe Johnson&quot; met een aanroep van &#39;Get Multiple Leads&#39; op basis van REST API-filtertype.

Gepast op _2014-09-09_ door _Murta_

## Marketo Tracking Cookie verwijderen

Deze methode mag alleen worden gebruikt als het gewenste effect bestaat uit het volledig verwijderen van het cookie.

Dit codevoorbeeld kan worden gebruikt om een Marketo-volgcookie te verwijderen uit de browser van een gebruiker nadat het Marketo-formulier is verzonden. Dit werkt door de methode `deleteMarketoCookie` aan te roepen nadat een gebruiker een formulier heeft verzonden. Deze methode vervalt het cookie door de vervaldatum in te stellen op een datum in het verleden. Het standaardgedrag van de browser is dan het verwijderen van deze cookie, omdat deze verlopen is.

Gepast op _2014-09-09_ door _Murta_

## Gratis e-maildomeinen beperken bij invullen formulier

Stel dat u bezoekers van de site wilt beperken in het verzenden van een formulier met een gratis e-maildomein, zoals Gmail of Yahoo. Hiervoor neemt u het onderstaande script op in de broncode van de pagina met het Marketo-formulier. Er wordt gecontroleerd of een gebruiker een e-mailbericht buiten het bedrijf heeft ingevoerd (Gmail, Hotmail, enz.) en het verzenden van een Marketo-formulier wordt voorkomen als een gebruiker dit invoert. U kunt de lijst met geblokkeerde e-maildomeinen uitbreiden door regel 9 te wijzigen en er domeinen in op te nemen die u wilt beperken.

Gepast op _2014-09-09_ door _Murta_

## Fouten opsporen in een veld dat niet toegankelijk is via de API

Als u naar dit veld komt <https://nation.marketo.com/> Wanneer we proberen het veld Jaarlijkse inkomsten bij te werken met de SOAP API, wordt in het antwoord aangegeven dat de record wordt bijgewerkt, maar dat de wijziging niet in het veld Jaarlijkse inkomsten wordt doorgevoerd. Als we het veld handmatig proberen bij te werken met behulp van de Lead-database, worden de wijzigingen in dit veld ook niet voortgezet. Controleer of het veld is geblokkeerd in Admin. Het andere wat soms kan gebeuren, is dat het een Account Field is dat van SFDC wordt gesynchroniseerd. We blokkeren wijzigingen in deze velden standaard, omdat SFDC in veel gevallen het registratiesysteem voor deze velden is. 1) Gemaakt op 2) Marketo SFDC ID 3) Marketo Unique Code 4) SFDC Type 5) Bijgewerkt op 6) Urency 7) Priority 8) Sales Created Date

Gepast op _1970-01-01_ door _Murta_

## Aangepaste code toevoegen aan een Marketo-landingspagina

Stel dat u een script voor het bijhouden van wijzigingen door derden wilt toevoegen, zoals Google Analytics aan uw bestemmingspagina van Marketo. Dit is mogelijk via de gebruikersinterface van Marketo. Meer in het algemeen kunt u aangepaste code (HTML, CSS, JavaScript) toevoegen aan uw Marketo-landing door de onderstaande instructies te volgen.

1. Selecteer de openingspagina en klik op Concept bewerken.
1. Sleep in het HTML-element.
1. Voer uw aangepaste code in en klik op Opslaan.

Gepast op _2014-09-10_ door _Murta_

## Formulierbericht op de server

`https://community.marketo.com/MarketoResource?id=kA650000000GsXXCA0`

Gepast op _2014-09-11_ door _Murta_

## Marketo Tracking Cookie wissen uit Forms 2.0-verzendingen

### Overzicht

Forms 1.0 bevat de waarde voor het Munchkin tracking-cookie als een veld in het DOM. Dit werd samen met alle andere inputs ingediend. [ Forms 2.0 ](/help/javascript-api/forms-api-reference.md) laat dit gebied weg, en bevolkt dynamisch de waarde op voorlegging eerder dan op vormlading. Hoewel dit over het algemeen aanvaardbaar is, wordt er wel een klasse van gebruiksgevallen gemaakt, waarvoor het trackingcookie moet worden gewist om ongewenste tracking en prefilling te voorkomen. Dit kan bijvoorbeeld gebeuren op een handelslocatie waar een Marketo-klant hetzelfde formulier op hetzelfde apparaat gebruikt en contactgegevens van meerdere personen ophaalt. Met het onderstaande fragment kunt u de cookiewaarde wissen wanneer het formulier wordt verzonden, zonder dat u de cookie zelf uit de browser van de gebruiker hoeft te verwijderen.

### Codevoorbeeld

Dit fragment verwacht dat één formulier op de pagina wordt geladen. Als er veelvoudige vormen zijn, zou u [ loadForm of getForm methodes ](/help/javascript-api/forms-api-reference.md) moeten gebruiken om uw callbacks uit te voeren.

```javascript
<script>
//add a callback to the first ready form on the page
MktoForms2.whenReady( function(form){ 
 //add the tracking field to be submitted
        form.addHiddenFields({"_mkt_trk":""});
        //clear the value during the onSubmit event to prevent tracking association
 form.onSubmit( function(form){
  form.vals({"_mkt_trk":""}); 
 })
})
</script>
```

Gepast op _2014-09-11_ door _Kenny_

## Updates van de release van september 2014

### Updates voor REST API

Toegevoegd een nieuwe facultatieve gebiedswaarde aan [ krijgt Veelvoudige Leidingen ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) API die de het koekjeswaarden van Munchkin verbonden aan een loodverslag zal terugkeren. Voeg eenvoudig &quot;?fields=cookies&quot;aan het verzoek toe.    

Gepast op _2014-09-16_ door _Murta_

## Locatiegegevens van RTP API toevoegen aan Marketo-formulierinvulling

**u hebt actieve abonnementen RTP en MLM nodig om het gebruiksgeval uit te voeren dat in dit blogbericht wordt beschreven.**
Gebruikend [ RTP JavaScript APIs ](/help/javascript-api/web-personalization.md) en [ Marketo Forms 2.0 ](/help/javascript-api/forms-api-reference.md), kunt u de afgeleide plaatsgegevens van RTP trekken en het in Marketo via een vorminvulling duwen. Op deze manier kunt u de afgeleide locatie van de gebruiker (gebaseerd op IP-adres) tijdens de meest recente formulieractiviteit zien. Om aan de slag te gaan, moet u drie aangepaste tekenreeksvelden maken in Marketo. U kunt dit doen of via uw CRM als het een inheemse integratie met Marketo heeft, of van het menu van het Beheer van het Gebied in Marketo admin sectie. Ik raad aan deze velden een naam te geven: &#39;Meest recent land&#39;, &#39;Meest recente staat&#39; en &#39;Meest recente stad&#39;. We gaan verder met dit blog met deze naamgevingsconventie. De API-namen voor deze velden zijn &#39;mostRecentCountry&#39;, &#39;mostRecentState&#39; en &#39;mostRecentCity&#39;. Om de plaatsgegevens terug te winnen, gebruiken wij de [ methode RTP om de de plaatsgegevens van de bezoeker ](/help/javascript-api/web-personalization.md) te krijgen, en dan het in de vorm over te gaan gebruikend [ addHiddenFields en vals methodes ](/help/javascript-api/forms-api-reference.md) van Marketo Forms 2.0. Voeg op de pagina uw JS-tag RTP en een Marketo-formulier toe. Voeg vervolgens het onderstaande script in. U moet de namen van de doelformuliervelden in de voorbeeldcode wijzigen als u een andere naamgevingsconventie gebruikt dan de bovenstaande conventie.

```javascript
<script>
//modify the form and grab the user
MktoForms2.whenReady( function(form) { 
        //add the hidden fields to the form
 form.addHiddenFields({
  "mostRecentCountry":"",
  "mostRecentState":"",
  "mostRecentCity":""});
 //Grab the visitor data, a JS object with it is passed in the callback function of the third argument
 rtp('get','visitor',function(visitor){
  //add the desired info from the visitor object to the form fields
  form.vals({
   "mostRecentCountry":visitor.results.location.country,
   "mostRecentCity":visitor.results.location.city,
   "mostRecentState":visitor.results.location.state});
  }
  );
 });
</script>
```

Gepast op _2014-09-17_ door _Kenny_

## Blokkruipende en zoekindexering van een Marketo-landingspagina

Laten we zeggen dat je wilt voorkomen dat een Marketo-landingspagina wordt gekropen en in resultaten wordt weergegeven door zoekmachines zoals Google. Dit doet u als volgt:

1. Meld u aan bij Marketo en selecteer de bestemmingspagina. Klik vervolgens op Concept bewerken.
1. Klik op Handelingen voor bestemmingspagina. Klik vervolgens op Metacodes pagina bewerken
1. Het veld Robots wijzigen in Geen index, Geen bewerking. Klik vervolgens op Opslaan.

Gepast op _2014-09-19_ door _Murta_

## Veelgestelde vragen over Marketo REST vs SOAP API&#39;s

**Bijgewerkt: Maart 2016** hier zijn antwoorden aan de vaakst gestelde vragen over Marketo [ REST ](/help/rest-api/rest-api.md) en [ SOAP ](/help/soap-api/soap-api.md) APIs. **Q: Wat zijn de belangrijkste verschillen tussen Marketo REST en SOAP APIs?** A: Hoewel de mogelijkheid om specifieke gegevens via REST- en SOAP-API&#39;s te push/pull-specifieke gegevens grotendeels overlapt, bestaat er bepaalde functionaliteit die alleen in REST- of SOAP-API&#39;s bestaat. In termen van prestaties, heeft REST API betere [ productie ](https://en.wikipedia.org/wiki/Throughput) dan SOAP API. In termen van het authentificatiemodel, heeft REST API een authentificatiemodel dat een het verlopen teken gebruikt. Onze REST API verleent ook toegang tot de activa van Marketo [&#128279;](https://developer.adobe.com/marketo-apis/api/asset/).   **Q: Welke eigenschappen beschikbaar in REST API die niet in SOAP API beschikbaar zijn?** A: [ Lijst van lijsten API ](/help/rest-api/list-of-standard-fields.md), [ verwijdert een lood uit een lijst API ](/help/rest-api/lead-database.md), [ Gebruik API van het Gebruik ](/help/rest-api/rest-api.md), en [ Fout API ](/help/rest-api/rest-api.md) zijn slechts beschikbaar met REST API. **Q: Zijn er plannen om het aantal APIs te verhogen beschikbaar voor SOAP API?** A: Nee. **Q: Zijn er plannen om het aantal APIs te verhogen beschikbaar voor REST API?** A: Ja. REST is op dit moment het belangrijkste aandachtspunt bij de ontwikkeling van de Marketo API.

Gepast op _2014-09-20_ door _Murta_

## Formulierbericht op de server

**Nota: Dit is een niet-openbare en niet gestaafde API, wordt het niet gesteund en zijn gedrag kan op elk ogenblik veranderen** Als u uw eigen vormen op uw website gebruikt, kunt u deze gegevens nog verzenden naar Marketo gebruikend een server-zijpost. Het voordeel van deze aanpak is dat u uw bestaande formulier- en toepassingslogica kunt behouden, maar dat u nog steeds een daadwerkelijke formulierpost in Marketo kunt gebruiken. Dit geeft de gebruikers van Marketo een &quot;Vult uit de gebeurtenis van de Vorm&quot;, die kan worden gebruikt om geautomatiseerde processen of voor segmentatie teweeg te brengen. **NOTA: Er is een tariefgrens van 30 server-zijvormposten per minuut van één enkel IP adres.** In elke scripting- of programmeertaal zijn er verschillende opties voor het verzenden van formulieren op de server en kunnen er verschillende objecten of methoden zijn die kunnen worden gebruikt om Post Call uit te voeren. In PHP gebruiken veel mensen bijvoorbeeld de cURL-bibliotheek. In alle gevallen plaatst u naam-waardeparen naar een opgegeven URL. De namen moeten identiek zijn aan de API-namen van de Marketo-velden. Daarnaast zijn er een aantal systeemvelden die moeten worden opgenomen om de formulierverzending correct vast te leggen.

1. Maak een formulier. De eerste stap bestaat uit het maken van een formulier in Marketo of het gebruiken van een bestaand formulier dat u wilt verzenden. De naam van het formulier moet beschrijvend zijn, maar er zijn eigenlijk geen formuliervelden voor nodig. Als u een nieuw formulier maakt, voert u gewoon een naam in. Schakel vervolgens het vakje &quot;Formuliereditor openen&quot; uit en u bent klaar.
1. Een formulier-id zoeken. Selecteer het formulier in de gebruikersinterface van Marketo en bekijk de URL: het moet de indeling `https://app-x.marketo.com/#FO8B2ZN12` hebben. Kijk achter het #-teken naar het getal dat direct volgt op &quot;FO&quot; om de formulier-id te zoeken. In dit geval is de formulier-id 8. In sommige gevallen is het eerste formulier genummerd tot 1001 en tellen vanaf dat formulier. De formulier-id is een variabele, zodat u de verzending van verschillende formulieren kunt activeren.
1. Vraag je Marketo-account-id op. Ga naar Beheer > Munchkin en kopieer de Munchkin-account-id, met de indeling 000-AAA-000. U hebt dit nodig om het formulier naar het juiste Marketo-exemplaar te verzenden.
1. Bepaal de POST-URL. Let in de Marketo-gebruikersinterface op het domein in de locatiebalk, meestal in de notatie `<http://app-x.marketo.com/>` . Gooi alles na de schuine streep weg en voeg vervolgens &quot;index.php/leadCapture/save&quot; toe om het volledige formulier POST URL op te halen. Opmerking 1: dit is hoofdlettergevoelig. Opmerking 2: Marketo Sandboxen hebben mogelijk een ander domein dan uw Production Marketo-systeem. Een voorbeeld-URL zou dan als volgt kunnen zijn: `http://app-x.marketo.com/index.php/leadCapture/save` u kunt ook HTTPS gebruiken in plaats van HTTP (gebruik de CNAME niet omdat dit een uitzondering op de beveiliging geeft).
1. Zoek de namen van formuliervelden** Ga naar Beheer > Veld beheren en klik op de knop Veldnamen exporteren om een spreadsheet met de API-veldnamen te downloaden. Gebruik de API-naam als naam in de naam-waardeparen.
1. Bepaal welke velden moeten worden gepost. U kunt elk Marketo Lead-veld in uw formulierverzending opnemen. Veldnamen zijn hoofdlettergevoelig. Naast de velden die u wilt verzenden, zijn er twee verplichte velden en twee aanbevolen velden: verplichte velden op het formulier: (1) `munchkinId` - Dit veld wordt gebruikt voor uw Munchkin-account-id (2) `formid` - Dit veld geeft aan welk formulier in Marketo is verzonden Aanbevolen velden op het formulier: (1) E-mail - dit veld wordt gebruikt als de primaire sleutel voor deduplicatie. Als Marketo een overeenkomend e-mailadres vindt in de Marketo-database, wordt de bestaande record bijgewerkt. Als dit niet het geval is, wordt een nieuwe record gemaakt. Als er meerdere overeenkomsten zijn, wordt de meest recente bijgewerkte record (2) `_mkt_trk` bijgewerkt. In dit veld staan de cookie-gegevens, zodat u de bezoeken aan de webpagina van de persoon kunt bijhouden. Als u Munchkin op uw formulierpagina hebt, voert Munchkin automatisch een waarde in dit verborgen formulierveld in. Als dat niet het geval is, leest u het uit het cookie met dezelfde naam en geeft u het bestand in dit veld door aan Marketo. Opmerking: de hoofdtekst van het POST-formulier naar een Marketo-formulier moet URL-gecodeerd zijn.
1. Zie Reactie** De reactie op de formulierpost is een HTTP 302-omleidingscode. In sommige systemen wordt dit als een fout weergegeven. In dit geval betekent dit echter dat de lead is gemaakt of bijgewerkt. Als er een fout optreedt, ontvangt u een foutcode van 4 x of 5 x.

Hier is a [ post ](https://nation.marketo.com:443/t5/product-blogs/how-to-build-an-external-subscription-center/ba-p/242185) over het gebruiken van deze techniek voor unsubscribe scenario&#39;s [ door Justin Cooperman, Sr. de Manager van het Product ]

Gepast op _2014-11-07_ door _Murta_

## Leads zoeken bijgewerkt op specifiek datumbereik

Laten wij zeggen u lood wilt vinden die op specifieke data via [ Marketo API ](/help/soap-api/soap-api.md) werden bijgewerkt. Dit is mogelijk met [ getMultipleLeads SOAP API ](/help/soap-api/getmultipleleads.md). Deze methode retourneert eventuele leads met een wijziging van de gegevenswaarde of nieuwe activiteit in Marketo voor het datumbereik dat u aanvraagt. Voor `leadSelector`, zou u `LastUpdateAtSelector` specificeren. Vervolgens definieert u de datumbereiken met `oldestUpdatedAt` - en `latestUpdatedAt` -tijdgrenzen. Zie de voorbeeldaanvraag XML hieronder, die u laat zien hoe u leads kunt vinden die op 6 juni 2014 tussen 12.00 uur PST en op 7 juni 2011 tussen 12.00 uur PST zijn bijgewerkt. Opmerking: het datumbereik mag niet langer zijn dan 30 dagen.

**XML van het Verzoek van de Steekproef om Leads te vinden die door Datum** worden bijgewerkt

```xml
<soapenv:Envelope xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
<soapenv:Header>
<mkt:AuthenticationHeader>
<mktowsUserId>REPLACE</mktowsUserId>
<requestSignature>REPLACE</requestSignature>
<requestTimestamp>2014-10-23T12:19:51-07:00</requestTimestamp>
</mkt:AuthenticationHeader>
</soapenv:Header>
<soapenv:Body>
<mkt:paramsGetMultipleLeads>
<leadSelector xsi:type="mkt:LastUpdateAtSelector">
<oldestUpdatedAt>2014-06-06T00:00:00-08:00</oldestUpdatedAt>
<latestUpdatedAt>2014-06-07T00:00:00-08:00</latestUpdatedAt>
</leadSelector>
</mkt:paramsGetMultipleLeads>
</soapenv:Body>
</soapenv:Envelope>
```

Gepast op _2014-09-24_ door _Murta_

## Dynamische inhoud toevoegen aan een e-mail

Stel dat u een dagelijkse e-mail verzendt en dat u de datum van die dag automatisch wilt opnemen in de e-mailsjabloon. Hiervoor gebruikt u tokens en e-mailscripts in Marketo.

1. Maak een token.** Navigeer naar het programma waar u het token wilt gebruiken. Klik op Mijn tokens.
1. Dubbelklik op E-mailscript. Geef dit token vervolgens een naam. Klik vervolgens op Bewerken.
1. Plak het e-mailscript hieronder in dit venster. Klik vervolgens op Opslaan.

## Het agenda-object van Access Velocity

`set($x = $date.calendar)`

## Indelingsdatum

`set($current_date = $date.format('dd-MM-yyyy', $x.getTime()))`

## Retourneert de datum van vandaag

`$current_date`

1. Verwijs naar het token in de e-mailsjabloon.** Noteer de naam van de token. Ga naar uw e-mailconcept. Neem het token op.  Wanneer het e-mailbericht wordt verzonden, wordt de waarde van het token ingevuld. Voor meer informatie, gelieve te zien de [ e-mailScripting ontwikkelaarsdocumentatie ](https://experienceleague.adobe.com/nl/docs/marketo-developer/marketo/email-scripting).

Gepast op _2014-11-22_ door _Murta_

## Bash Security Advisory

Marketo heeft een grondig onderzoek van de kwetsbaarheid van Bash uitgevoerd, die ook als [ wordt bekend Shellshock (CVE-2014-6271) ](https://nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-6271), en is tot de conclusie gekomen dat wij niet vatbaar voor deze aanvallen zijn. Bovendien hebben wij preventieve actie genomen door de software aan de recentste versie bij te werken om naleving van de [ aanbeveling van de CERT ](https://www.cisa.gov/news-events/alerts/2014/09/25/gnu-bourne-again-shell-bash-shellshock-vulnerability-cve-2014-6271) te verzekeren.

Gepast op _2014-09-26_ door _Murta_

## SOAP API-referenties bijwerken

Het is beste praktijken om uw [ SOAP API ](/help/soap-api/soap-api.md) geloofsbrieven regelmatig bij te werken. Op dit moment is er geen manier om dit via de Marketo API programmatisch te doen. In de onderstaande instructies ziet u hoe u uw SOAP API-referenties kunt bijwerken via de gebruikersinterface van Marketo.

1. Ga naar de Admin sectie en klik op de Diensten van het Web.
1. Stel een coderingssleutel in van ten minste 10 tekens. Klik op Wijzigingen opslaan.

Gepast op _2014-09-29_ door _Murta_

## Uw Marketo-database opschonen

**NOTA: Dit is een bericht van het gastblog. Josh Hill is de Marketo Practice Lead in Perkuto, een marketingbureau. Josh werkt op het snijpunt van marketing, verkoop, en technologie om opbrengstgeneratiesystemen te leveren. Hij schrijft over marketing automatisering en vraaggeneratie bij [ MarketingRockstarGuides.com ](https://www.marketingrockstarguides.com/).** Het opschonen van uw Marketo-database is een belangrijk onderdeel van het beheer van dit krachtige systeem. Als uw Marketo-gegevens vies zijn of uw CRM-gegevens vies zijn, wordt uw baan als marketingmanager veel moeilijker omdat u verklaart waarom campagnes naar de verkeerde mensen gingen of dat uw rapporten &quot;directioneel&quot; zijn. [ A 2011 de studie van Gartner merkte ](https://www.data.com/export/sites/data/common/assets/pdf/DS_Gartner.pdf) op dat de slechte gegevenskwaliteit arbeidsproductiviteit met 20% verminderde. Dat is 20% van je tijd (8 uur/week of 1 dag per week) die verspild is omdat je gegevens moest corrigeren. Maar dit verlies lijkt te liggen in vergelijking met de inkomsten die verloren zijn gegaan omdat je het doel verkeerd had. Hier zijn de vijf belangrijkste redenen om te investeren in het schoon houden van uw gegevens: - De betere segmentatie van lood en cliënten kan worden gedaan, die u toestaan om uw bericht op de juiste mensen op de juiste tijd te concentreren. Die 20% korting zou alleen naar nieuwe vooruitzichten moeten gaan, niet naar je beste klanten, toch? - Vermijd dubbele verzending van e-mails. Ondanks alle voorzorgsmaatregelen is het mogelijk meerdere e-mails naar dezelfde lead te verzenden, meestal omdat dubbele records niet worden samengevoegd. - Nauwkeurige rapportage aan uw baas. Omdat de GMO een zetel bij de opbrengstlijst heeft, moet u nauwkeurige, verenigbare, en herhaalbare rapporten hebben...of u kunt geen opbrengstteller meer zijn. - Hangende deals als je het verkeerde marketingmateriaal via e-mail naar belangrijke vooruitzichten stuurt tijdens verkooponderhandelingen.

Als uw gegevensbestand niet dat detail op het levenscyclusstadium verstrekt, zou dat een overeenkomst of twee kunnen zinken. - Verwijder slechte of oude leads om onder de prijsdrempels te blijven. Er is veel geschreven over de kosten van slechte gegevens. Je meest directe zorg is dat te veel slechte, dubbele of oude leads je over je prijsniveau van Marketo of Salesforce drukken, wat kan leiden tot duizenden kosten per jaar. Dus, hoe houdt u uw gegevensbestand schoon? Je kan deze principes van gegevenszuiverheid volgen, maar hoe kom je daar in Marketo? Ik maak enkele veronderstellingen over uw systeem: - U hebt een standaard Marketo-systeem. - U hebt een standaard Salesforce-instelling voor een contactpersoon voor leads. - U synchroniseert alle records tussen systemen. - U gebruikt geen doelgerichte duplicaten.

De juiste leads zoeken naar Fix** Laten we eerst de mogelijke problemen vinden. Met elke cliënt, ga ik door een reeks slimme lijsten om de gezondheid van hun gegevensbestand te bepalen. Ik stel voor dat u dat maandelijks doet. Als de slimme lijsten eenmaal werken, duurt het maximaal 15 minuten per maand. Dit is een goede manier om het rendement van uw werk en Marketo te demonstreren. Maak een tabel waarin u precies kunt zien wat de totale impact op uw database is. Het onderstaande voorbeeld bevat de criteria die ik zoek, dus zorg ervoor dat u ook andere belangrijke gebieden voor uw bedrijf opneemt. Verdeel elke groep door Alleen Marketo, SFDC Leads en SFDC Contacts.

Automatisering gebruiken om gegevenswaarden te corrigeren: het gebruik van automatiseringsregels voor het corrigeren van veelvoorkomende spelfouten of ontbrekende gegevens gaat enkele decennia terug. In de jaren &#39;60, zou de slechte gegevenskwaliteit wanneer het verzenden van direct mail in duizenden dollars kunnen resulteren die worden verspild. Tegenwoordig worden imago&#39;s sneller beschadigd door fouten in de gegevenskwaliteit dan door een verkeerd georiënteerde post. De reputatie van e-mail, de keuze van taal, en de ervaring van de klant zijn van belang en zij maken meer uit omdat de fouten in notulen openbaar viraal kunnen gaan. Bespaar de reputatie van uw bedrijf met geautomatiseerde gegevensreiniging. Deze gegevensbeheerstromen zijn een van de eerste dingen die ik in Marketo heb ingesteld. De meeste bedrijven zetten vergelijkbare stromen op: - Landcorrectie (hoewel u beginsel 1 had moeten volgen om dit niet nodig te hebben) - State Corrector of Mapper - vaak nuttig als u land en afgeleide staat hebt. - Aantal werknemers aan de Waaier van de Werknemer. - Onjuiste lead Source aan Good Lead Source - E-mail ongeldig voor e-mail is goed als de e-mail is gewijzigd. - Vertaal oude veldwaarden naar een nieuw veld (Volledig naar leeg). Bijvoorbeeld, past deze stroom de Waaier van de Werknemer aan die op Aantal van de Werknemer wordt gebaseerd.

Hulpprogramma&#39;s voor het toevoegen van gegevens Het invullen van lege velden is van essentieel belang om uw database beter te segmenteren. Leads vullen niet altijd de juiste industrie of functie of zelfs Titel in een formulier. Soms hebt u gegevens van oudere systemen. Dit gebrek aan gegevens betekent dat je die e-mail moet sturen naar minder mensen, of mogelijk naar de verkeerde mensen. U hebt gereedschappen voor het toevoegen van gegevens nodig om dit probleem op te lossen.

Optie 1: Vul deze zelf in. Mogelijk kunt u gegevens interpoleren om lege velden te herstellen. Misschien hebt u een SIC code in plaats van de naam van de Industrie of de Jaarlijkse Inkomsten vs. de Waaier van de Inkomsten. Marketo kan deze correcties eenvoudig automatiseren.

Optie 2: Vind gegevens toevoegend/verrijkt verkoper via LaunchPoint Er zijn [ verscheidene verkopers in Lanceerpunt ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO), zoals NetProspex en ReachForce die u kunnen helpen uw loodgegevens verrijken. Sommigen vragen u om een blad met uw gegevens, dat ze vervolgens opschonen en terugsturen. De beste optie is een geautomatiseerd gereedschap in Marketo of Salesforce, waarmee u de velden controleert die u wilt en vervolgens de juiste gegevens terugduwt. De meeste verkopers gebruiken [ Marketo API of Webhooks ](/help/home.md) om dit te verwezenlijken.

Optie 3: gebruik Marketo API&#39;s om leads bij te werken U kunt de Marketo API&#39;s gebruiken om leads te identificeren die moeten worden gewist, en deze vervolgens bijwerken via de API. [ krijgt Veelvoudige Leidingen door het Type van Filter REST API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) is een goede uitgangspunt voor het terugtrekken van gegevens uit Marketo die bepaalde criteria aanpassen. Om lood bij te werken, gelieve te bekijken [ leidt tot/bijwerkt leest REST API ](/help/rest-api/leads.md).

Alternatief, kunt u opstelling a [ Marketo Webhaak ](/help/webhooks/webhooks.md) opstelling om een extern systeem op de hoogte te brengen dat een bepaalde gebeurtenis, zoals een vorminvulling is gebeurd. Vervolgens kunt u reageren met waarden om de lead bij te werken.

Wat moet je automatiseren? Ik heb enkele suggesties gedaan in stap 1. Maar als we meer van de database willen opschonen, moeten we verder gaan dan het vastleggen van gegevenswaarden. - Mededinger Blacklist: zorg ervoor dat je de verzameling en zwarte lijst van je concurrenten automatiseert. Als u concurrenten-partners hebt, is het nog goed om hen behoorlijk te merken en hen in een lijst te plaatsen om misschien te onderdrukken.  - Meerdere harde grenzen: automatiseer deze stroom zeker. Als een lood hard meer dan tweemaal in een periode van 30 dagen stuitert, plaats hen aan Opgeschort of Ongeldig. Voer vervolgens één keer per maand de controle uit of het een typefout of iets anders betreft.  - Meerdere zachte golven in 30 dagen: stel deze in op marketing Susp=TRUE gedurende 30 dagen.  - Spam-overvulling/verwijdering: wees voorzichtig als uw product inhoudt dat mensen mogelijke spamoveradressen gebruiken. Zie een lijst met spamvallen. De slimme lijst.

**wat niet om** te automatiseren nooit de schrapping van lood automatiseren, omdat er teveel risico in toevallig massaal schrapping van de verkeerde verslagen is. In plaats daarvan wordt één keer per maand de regel Prullenbak weergegeven. Maar als je platte leads wilt verwijderen, voer dan zo&#39;n flow met een wachttijd van 30 dagen.

Bewegingen: Het gebruik van automatisering is een geweldige manier om tijd te besparen en uw database schoon te houden. Automatisering is een sword met dubbele randen, omdat als u het verkeerd instelt, dit binnen een paar minuten tot havoc kan leiden. Wees voorzichtig als u deze processen doorloopt en houd iedereen op de hoogte. Over het algemeen adviseer ik tegen het schrappen van om het even welke Contacten van SFDC omdat zij eerder actieve Kansen, Clients, of ex-Clients zijn. Mogelijk moet u van Financiën of Juridische Dienst bepaalde gegevens bewaren en zijn de contactpersonen bij deze administratie gevoegd. In plaats daarvan, concentreer zich op Contacten die de Harde Stuiteren, Ongeldig worden, of worden niet meer daar. Nu kent u een aantal technieken om uw Marketo-database schoon te houden. Laat ons weten of u andere manieren hebt bedacht om deze processen te automatiseren met de API of Webhooks!

Gepast op _2014-10-08_ door _Josh_

## Updates van de release van oktober 2014

### Vooraf ingevulde externe pagina

Marketo-formulieren bieden geen native vooraf ingevulde functionaliteit wanneer ze buiten een Marketo-bestemmingspagina worden geladen. Nochtans, kunnen wij dit nog uitvoeren gebruikend [ Marketo APIs ](/help/rest-api/rest-api.md) en [ Forms 2.0 JavaScript API ](/help/javascript-api/forms-api-reference.md/). De eerste stap bestaat uit het ophalen van de hoofdgegevens van Marketo via een REST-aanroep van uw server. Ervan uitgaande dat wij geen directe manier aan crossreference lood IDs of een ander uniek herkenningsteken van de server hebben, moeten wij het koekje van Munchkin, &quot;_mkto_trk&quot;gebruiken, om gegevens van de server van Marketo terug te winnen, gebruikend [ krijgen lood door de methode van het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).
Om deze vraag te maken, hebben wij uw Authentificatie en REST eindpunten van uw instantie nodig. Zodra u met uw instantie van Marketo voor authentiek hebt verklaard dat wij een vraag aan de lood API in `https://<host>/rest/v1/leads.json` moeten maken. Vervolgens moet een querytekenreeks worden gemaakt die op de Marketo-cookie als deze `?filterType=cookie&filterValues=` wordt gefilterd. U moet de specifieke waarde ophalen uit de sleutel &#39;_mkto_trk&#39; die door de client naar de server wordt verzonden. OPMERKING: De cookie_mkto_trk-waarde bevat een en-teken en moet URL-gecodeerd zijn naar `%26` om correct te worden geaccepteerd door het Marketo-eindpunt. Standaard retourneert de API voor leads vier velden: `id`, `email`, `firstName` en `updatedAt` . Als u een specifieke set velden wilt instellen, moet u een query-parameter `fields` opnemen, met veldnamen gescheiden door komma&#39;s als deze: `&fields=email,firstName,lastName,company` . Uiteindelijk zal onze oproep er als volgt uitzien:

`https://<host>/rest/v1/leads.json?filterType=cookie&filterValues=<cookie>&fields=email,firstName,lastName,company&access_token=<token>`

Wanneer we deze aanroep maken, wordt een JSON-object geretourneerd dat er als volgt uitziet:

```json
{  
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[  
        {  
        "id":50,
        "firstName":"Kenny",
 "lastName":"Elkington",
        "email":"<mkto@example.com>",
 "company":"Marketo Inc."
        }]
};
```

Nu we over onze hoofddetails beschikken, kunnen we deze in een Marketo-formulier in kaart brengen, met behulp van de &#39;whenReady&#39;-methode en &#39;vals&#39;-methode in JavaScript. Eerst moeten we de resultaten van de lead instellen als een variabele op onze pagina:

```javascript
<script>
//print your JSON object dynamically as the mktoLead variable
var mktoLead = {  
    "requestId":"e42b#14272d07d78",
    "success":true,
    "result":[  
        {  
        "id":50,
        "firstName":"Kenny",
  "lastName":"Elkington",
        "email":"mkto@example.com",
  "company":"Marketo Inc."
        }]
}
</script>
```

Nu we onze resultaten op de pagina hebben, kunnen we ze in onze formuliervelden opnemen:

```javascript
<script>
MktoForms2.whenReady( function(form) {
 //set the first result as local variable
 var mktoLeadFields = mktoLead.result[0];
    //map your results from REST call to the corresponding field name on the form
 var prefillFields = { 
   "Email" : mktoLeadFields.email,
   "FirstName" : mktoLeadFields.firstName,
   "LastName" : mktoLeadFields.lastName,
   "Company" : mktoLeadFields.company
   };
 //pass our prefillFields objects into the form.vals method to fill our fields
 form.vals(prefillFields);
 }
 );
</script>
```

Gepast op _2014-10-24_ door _Kenny_

## E-mailadres HTML vervangen

In dit bericht wordt getoond hoe u de HTML kunt verwijderen die door Marketo is gegenereerd voor een e-mail. Vervolgens kunt u uw eigen HTML gebruiken zonder dat Marketo de opmaak opnieuw opmaakt.

1. Navigeer naar het e-mailbericht en klik op Concept bewerken.
1. Klik op Handelingen via e-mail, klik op Gereedschappen van HTML en vervolgens op HTML vervangen.
1. Vervang de code in dit vak door je HTML. Klik vervolgens op Opslaan.

Gepast op _2014-10-28_ door _Murta_

## Vraag de Cookie-id van een bezoeker op en vervolgens de bijbehorende gegevens voor leads opvragen

Gebruikend [ krijgt Veelvoudige Leidingen door het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) REST eindpunt, kunt u loodgegevens vragen die op koekjesidentiteitskaart van een gebruiker worden gebaseerd. U kunt deze methode bijvoorbeeld gebruiken om een formulier vooraf in te vullen op een landingspagina die geen Marketo is. Deze post toont u hoe te om de het koekjeswaarde van de gebruiker tijdens een webpaginabezoek te vangen, vraag [ krijgt Veelvoudige Leidingen REST API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) met die koekjesidentiteitskaart, en keert dan de loodgegevens van de gebruiker terug. Eerst hebben we de waarde van de Munchkin cookie van de gebruiker nodig, &#39;_mkto_trk&#39;. Hier volgt een voorbeeld van een JavaScript-functie die u kunt gebruiken om de cookiewaarde op te halen. Gelieve te zien [ deze StackOverflow pagina ](https://stackoverflow.com/questions/10730362/get-cookie-by-name) voor meer informatie over deze benadering. Ik adviseer het plaatsen van een vertraging van 500 ms na de gebeurtenis van de paginading alvorens deze functie te roepen. Dit geeft Munchkin tijd om te laden, en de gebruiker te kooksen.

```javascript
//Function to read value of a cookie
function readCookie(name) {
    var cookiename = name + "=";
    var ca = document.cookie.split(';');
    for(var i=0;i < ca.length;i++) {
        var c = ca[i];
        while (c.charAt(0)==' ') c = c.substring(1,c.length);
        if (c.indexOf(cookiename) == 0) return c.substring(cookiename.length,c.length);
    }
    return null;
}

//Call readCookie function to get value of user's Marketo cookie
var value = readCookie('_mkto_trk');
```

Geef vervolgens de waarde van het cookie &#39;_mkto_trk&#39; door aan uw server. Om loodgegevens terug te winnen, van uw server maakt u een vraag aan [ krijgt Veelvoudige Leidingen REST API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) met deze koekjeswaarde. U hebt uw authentificatie en REST eindpunten van uw instantie nodig. Uw vraag zou als volgt moeten worden gestructureerd:

OPMERKING: de waarde van `_mkto_trk` cookie bevat een en-teken en moet URL-gecodeerd zijn naar `%26` om correct te worden geaccepteerd door het Marketo-eindpunt.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "cde42eff-aca0-48cf-a1ac-576ffec65a84:ab"
# Replace with filter type and values
filter_type_and_values = "&filterType=cookies&filterValues=id:AAA-BBB-CCC%26token:_mch-marketo.com-1418418733122-51548&fields=cookies,email"
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

In het bovenstaande voorbeeld worden het e-mailbericht en alle cookies die aan de gebruiker zijn gekoppeld, geretourneerd. U kunt deze gegevens vervolgens gebruiken om de pagina die de gebruiker bezoekt, aan te passen.

`{"requestId":"aa00#14a405aa786","result":[{"id":583,"email":"<testaccount@gmail.com>","cookies":"_mch-marketo.com-1418418733122-51548"}],"success":true}`

Gepast op _2014-10-30_ door _Murta_

## Wanneer hebt u een ontwikkelaar nodig om u te helpen met de automatisering van marketing

OPMERKING: Dit is een bericht op een gastblog. Josh Hill is de Marketo Practice Lead in Perkuto, een marketingbureau. Josh werkt op het snijpunt van marketing, verkoop, en technologie om opbrengstgeneratiesystemen te leveren. Hij schrijft over marketing automatisering en vraaggeneratie bij [ MarketingRockstarGuides.com ](https://www.marketingrockstarguides.com/).

De automatiseringsplatforms van de marketing zijn enorm krachtig uit de doos en in de handen van een ervaren exploitant. Platforms staan per definitie het gebruik van extensietoepassingen toe om het systeem nog verbluffende dingen voor uw team te laten doen. Je zou kunnen denken dat de logicamotor van Marketo zo veel kan (en het is), maar er zijn beperkingen. Marketo kan niet alles voor je doen, en dat zou ook niet moeten.

Er zijn andere gereedschappen die hun functie beter uitvoeren dan Marketo zou kunnen bouwen. Het platform van Marketo is zeer open, toelatend het [ ecosysteem van LaunchPoint van toepassingen ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) om te bestaan. U kunt deze openheid ook gebruiken om de mogelijkheden van uw site en Marketo uit te breiden en aan uw bedrijfsbehoeften te voldoen. Het geweldige aan een platform als Marketo is dat het de typische markeerstitel toestaat om pagina&#39;s, e-mails, en het verpletteren van logica te bouwen zonder een volwaardige programmeur te zijn.
Een marketeter moet tegenwoordig wel de logica begrijpen, maar een echte programmering kan het best aan de experts worden overgelaten. Hoe weet je wanneer je een ontwikkelaar moet bellen? Ik heb een paar basisregels, of heuristiek, om te beslissen wanneer een programmeur betrokken zou moeten worden: - wanneer Marketo geen duidelijk filter, trekker, of eigenschap voor de behoefte heeft, kan het vaak met sommige JavaScript of jQuery worden gedaan. - Is dit op zich al te complex voor Marketo? - Kan Marketo dit zelfs doen? - Wordt deze aanpassing van een website niet gemakkelijk ondersteund? - Moet Marketo met een website of een andere database spreken? - klinkt het als iets wat een computer kan doen, maar Marketo heeft er geen functie voor? Houd er rekening mee dat Marketo geen functie buiten de box kan bieden, maar wel verbinding maakt met veel integratie van derden en met aangepaste verbindingen.

Neem een blik bij een paar van deze categorieën bij de [ markt van StartPoint ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO): - [ Hulpmiddelen van de Analyse ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) - [ Gegevens die ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) toevoegen - [ De Systemen van het Beheer van de Inhoud ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) Sommige derdetoepassingen verstrekken intuïtieve controlepanelen en opstellingshulpmiddelen recht binnen het platform (GoToWebinar). Dit zijn &#39;native&#39; integraties waarbij het meeste werk dat u moet doen, bestaat uit het instellen van de aanmeldingsnaam en het vervolgens gebruiken in Marketo. Voor andere extensies moet u echter de complexere API gebruiken die directer moet worden geprogrammeerd.

**de Opties van de Integratie van Marketo** - de Integratie van LaunchPoint - gewoonlijk login of gemakkelijke montages. - API Integratie - vereist opstelling van API en programmering: (1) [ REST API ](/help/rest-api/rest-api.md) (2) [ SOAP API ](/help/soap-api/soap-api.md) (3) [ Integratie Webhaak ](/help/webhooks/webhooks.md) - vereist opstelling van speciale code, maar vrij gemakkelijk. (4) [ E-mailScripting ](./email-scripting.md) (Snelheid) - JavaScript en jQuery: (1) [ Forms 2.0 ](/help/javascript-api/forms-api-reference.md) (2) [ Lood Volgen (Munchkin) ](/help/javascript-api/lead-tracking.md) (3) [ Sociale JS ](/help/javascript-api/social.md) (4) [ RTP JS ](/help/javascript-api/web-personalization.md) hier zijn een paar gebruiksgevallen voor het gebruiken van een ontwikkelaar om de mogelijkheden van het platform van Marketo uit te breiden. Heeft u een van deze gevallen van gebruik? Als dat het geval is, kan het tijd zijn om met een ontwikkelaar te spreken. [ bezoek de sectie van de dienstenpartner op LaunchPoint ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO).

Gepast op _2014-11-06_ door _Josh_

## Slack integreren met Marketo

[ Slack is een platform van de ondernemingssamenwerking ](https://slack.com/). Als uw team op Slack werkt, kunt u eenvoudig Marketo-berichten in uw workflow opnemen. In dit bericht ziet u hoe u een melding aan uw chatlogboek kunt toevoegen wanneer een bepaalde lead-activiteit in Marketo plaatsvindt. Mogelijke gebruiksgevallen zijn onder andere het melden van een formuliervulling aan uw hele team, een bezoek aan een pagina met prijzen of een lead waarmee binnen 30 dagen geen contact is opgenomen. In de onderstaande schermafbeelding ziet u hoe de Marketo-melding er in Slack uitziet nadat u de stappen in dit Help-artikel hebt uitgevoerd.

1. Meld u aan bij Slack. Klik op Integraties in Slack
1. Klik op de knop Toevoegen voor Binnenkomende webhaken
1. Kies het Kanaal voor het Bericht van Marketo. Klik vervolgens op Binnenkomende WebHaak-integratie toevoegen.
1. URL van Webhaak kopiëren en plakken (nodig voor stap 8)
1. Kies een naam voor de melding
1. Meld u aan bij Marketo. Ga naar Beheer. Klik op Webhooks.
1. Klik op Nieuwe webhaak
1. Ga een Naam voor de Webhaak in. Ga Webhaak URL van Stap vier in. Voer Post in als type aanvraag. Voer een Payload-sjabloon in.

Hier is het ladingsmalplaatje van het schermschot. Er worden voornamen, bedrijven en e-mailadrestokens op hoofdniveau gebruikt.

`payload={"text": "DEVELOPER SITE ALERT: {{lead.First Name:default=edit me}} {{lead.Company=edit me}}, {{lead.Email Address:default=no email address}}" }`

1. Triggercampagne instellen in Marketo. De Stap van de stroom moet Webhaak aan Slack roepen. Slimme lijst is een webpaginabezoek.
1. Controleer of het werkt.

Gelieve te zien de [ ontwikkelaardocumentatie ](./webhooks/webhooks.md) voor meer informatie over websites in Marketo.

Gepast op _2014-11-10_ door _Murta_

## Litmus integreren met Marketo

[ Litmus is de dienst ](https://www.litmus.com/) voor het testen van e-mails over browsers en e-mailcliënten. In Litmus vindt u ook analyses van e-mailberichten, zoals klikken, openen en verwijderen. Dit artikel laat zien hoe je Marketo met Litmus kunt integreren.

1. Als u uw e-mailprogramma instelt in Marketo, klikt u op Mijn tokens op het dashboard van het programma
1. Sleep het token &quot;Email Script&quot; naar het middelste deelvenster om het toe te voegen.
1. Geef het token een naam en klik op &quot;Klik om te bewerken&quot;
1. Rechts onder Standaardobjecten vouwt u de categorie Lead uit. Zoek het veld E-mailadres en schakel het selectievakje in. In de lege manuscriptruimte op de linkerzijde van deze zelfde pagina, deeg in de volgende code die door Litmus wordt verstrekt. Vervang in de Litmus-code elke instantie van `{{lead.Email Address}}` door `${lead.Email}` . Klik op Opslaan om het lichtbakvenster te sluiten en klik nogmaals op Opslaan op de pagina voor tokens.
1. Noteer de naam van de token `{{my.LitmusToken}}` . Open het e-mailbericht dat u wilt bijhouden. Plaats uw nieuwe scripttoken helemaal onder aan uw e-mail. U kunt ook standaardgegevens toevoegen die overeenkomen met de versie Litmus `{{my.LitmusToken:default=editme}}` .

Wanneer het e-mailbericht wordt verzonden, wordt het script door Marketo in de e-mail geplaatst.

Gepast op _2014-11-18_ door _Murta_

## Een afbeelding opgeven wanneer een Marketo-openingspagina wordt gedeeld op Facebook

Stel dat u een afbeelding automatisch wilt laten weergeven wanneer u een Marketo-landingspagina op Facebook deelt. Misschien wilt u ook dat deze afbeelding niet op de Marketo-landingspagina zelf staat, maar alleen op de Facebook-pagina. U kunt dit doen door een metatag in een open grafiek toe te voegen aan uw Marketo-landingspagina. Hieronder vindt u de stappen die u hiervoor moet ondernemen.

1. Selecteer de openingspagina. Klik vervolgens op Concept bewerken.
1. Klik op Metatags voor pagina bewerken.
1. Voeg open-grafiekmeta aan de sectie van OG van Facebook Markeringen toe. Klik vervolgens op Opslaan. Dit is de indeling: `<meta property="og:image" content="http://example.com/example.jpg"/>`

[ zie de ontwikkelaarsdocumentatie van Facebook ](https://developers.facebook.com/docs/sharing/best-practices) over open-grafiekmetatags voor meer informatie.

Gepast op _2014-11-17_ door _Murta_

## Pagina omleiden op basis van verwijzing

Stel dat u direct verkeer naar een Marketo-landingspagina wilt voorkomen. Stel dat deze pagina downloadbare inhoud bevat, zoals een PDF, die u als gebruiker wilt laten invullen voordat deze het formulier ontvangt. U kunt dit oplossen door te controleren of de gebruiker van een bepaalde pagina afkomstig is. In dat geval is het de pagina waarop de gebruiker een formulier moet invullen. Als de gebruiker niet van die pagina komt, kunt u de gebruiker dan omleiden naar de pagina voor het invullen van het formulier. Hiervoor moet u controleren of de verwijzingspagina naar de bestemmingspagina met inhoud de pagina is die het formulier invult.
Vervang beide instanties van `http://example.com/PageWithForm` in het onderstaande fragment door een koppeling naar de pagina waarvan u de gebruiker wilt maken. Dit kan de pagina voor het invullen van formulieren zijn.**

```javascript
<script>
window.onload = function() {
  if (document.referrer !== "http://example.com/PageWithForm") {
    document.location.href = "http://example.com/PageWithForm";
  };
 };
</script>
```

Neem het aangepaste JavaScript-fragment op vóór de afsluitende tag op de Marketo-bestemmingspagina met inhoud.** Als de gebruiker niet naar de bestemmingspagina wordt gestuurd met inhoud van de pagina voor het invullen van het formulier, wordt de gebruiker nu omgeleid naar de pagina voor het invullen van het formulier.

Gepast op _2014-11-18_ door _Murta_

## Trello integreren met Marketo

Trello is a [ populaire web-based toepassing van het projectbeheer ](https://trello.com/). Als uw team Trello gebruikt, kunt u Marketo-meldingen eenvoudig overbrengen naar uw workflow. In dit bericht ziet u hoe u een kaart met een Marketo-melding aan uw Trello-bord kunt toevoegen. Deze kaart wordt toegevoegd wanneer een bepaalde lead-activiteit plaatsvindt in Marketo. Mogelijke gebruiksgevallen zijn onder andere het melden van een formuliervulling aan uw hele team, een bezoek aan een pagina met prijzen of een lead waarmee binnen 30 dagen geen contact is opgenomen.

1. Meld u aan bij Trello. Navigeer naar het Trello-bord waar Marketo-berichten worden toegevoegd. Klik op Een lijst toevoegen en geef deze een naam.
1. Klik op Zijbalk tonen. Klik op Instellingen voor e-mail naar board. Noteer het e-mailadres in het vak &#39;Uw e-mailadres voor dit bord&#39;. Deze e-mail wordt gebruikt in Stap 6. Kies welke lijst u wilt toevoegen aan het Marketo-bericht.
1. Meld u aan bij Marketo. Klik op de nieuwe slimme campagne. Voer een naam in en klik op Opslaan.
1. Navigeer naar de slimme lijst. Kies een trigger voor deze slimme campagne. In dit voorbeeld gebruiken we de trigger Formuliervulling. Sleep Vullingen uit de trekker van de Vorm aan MiddenComité. Selecteer het formulier dat u wilt activeren voor deze melding.
1. Maak een e-mail. Klik op Nieuw. Klik op Lokaal element. Klik op Nieuwe e-mail. Geef het e-mailadres een naam. Klik vervolgens op Maken.
1. Klik op Concept bewerken voor de e-mail die u in Stap 5 hebt gemaakt. Sleep de relevante tokens die u op uw Trello-kaart wilt weergeven. De onderwerpregel van de e-mail van Marketo wordt weergegeven in de titel van de Trello-kaart en de tekst van de e-mail van Marketo wordt weergegeven in de beschrijving van de Trello-kaart. U kunt bijvoorbeeld &quot;LEAD-WAARSCHUWING: `{{lead.First Name:default=edit me}}` `{{lead.Last Name:default=edit me}}` gebruiken als u de voor- en achternaam van de lead in de titel van de Trello-kaart wilt opnemen. Voer vervolgens de e-mail in.
1. Navigeer naar slimme campagne. Klik op Stroom. Sleep Waarschuwing verzenden naar het middelste deelvenster. Selecteer het zojuist gemaakte e-mailbericht. Selecteer &#39;Verzenden naar&#39; als Geen. Selecteer &#39;Aan andere e-mails&#39; als de Trello-e-mail in stap twee.
1. Klik op Planning in het bovenste menu. Klik op Activeren. Klik op Bevestigen.
1. Test de integratie. Een kaart met de voornaam en achternaam van de leider in de titel verschijnt op het bord van Trello. Zie {de documentatie van 0} Trello [&#128279;](https://support.atlassian.com/trello/) voor meer informatie.

Gepast op _2014-11-18_ door _Murta_

## Regels zoeken op basis van aangepaste veldwaarde

Laten we zeggen dat u leads wilt ophalen van de Marketo API die voldoen aan bepaalde activiteit- of inactiviteitscriteria. Misschien wilt u bijvoorbeeld een leads vinden waarvan de score in de afgelopen 30 dagen niet is gewijzigd. Door de stappen in dit bericht te volgen, kun je deze lijst met leads ophalen. Om dit te doen, creëren wij een slimme campagne in Marketo die leads identificeert waarvan de score de afgelopen 30 dagen niet is veranderd, en dan een waarde op deze lood opslaan om hen te identificeren. Vervolgens wordt de API met deze waarde opgezocht.

1. Maak een nieuw aangepast veld met de naam customLeadStatus** Aanmelden bij Marketo en ga naar het deelvenster Beheer. Klik op Veld beheren. Klik op Nieuw aangepast veld.  Geef het veld een naam. Klik vervolgens op Maken.
1. Maak een slimme campagne met een slimme lijst die naar leads zoekt die niet in 30 dagen zijn bijgewerkt.** Klik op Nieuwe slimme campagne. Geef de nieuwe slimme campagne een naam. Slepen niet score is gewijzigd van rechterdeelvenster naar middelste deelvenster.
1. Voeg een stroomstap aan de slimme campagne van Stap 3 toe om het customLeadStatus gebied met een nieuwe waarde bij te werken.** Sleep Gegevenswaarde wijzigen van het rechterdeelvenster naar het middelste deelvenster.
1. Werk de slimme Campagne bij om lood toe te staan om door veelvoudige tijden te lopen.** Klik op Schema. Klik vervolgens op Bewerken.  Selecteer deze optie telkens. Klik vervolgens op Opslaan. De campagne zal nu van start gaan.
1. Vraag [ krijgt Veelvoudige Leidingen door het Type van Filter REST API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Filtertype=customLeadStatus &amp; filterValue=needsEnrichment.**

Dit is een voorbeeldverzoek dat deze gegevens terugkeert.

`<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?access_token=><yourAccessToken>&filterType=customLeadStatus&filterValues=needsEnrichment`

Een geslaagde API-aanroep retourneert JSON-gegevens met leads waarvan het customLeadStatus-veld overeenkomt met de waarde van needsEnrichment. Herzie [ krijgt Veelvoudige Leidingen door het Type van Filter REST API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) voor meer informatie.

Gepast op _2014-11-22_ door _Murta_

## Opportunity Sync via SOAP API

In dit artikel wordt beschreven hoe u Opportunity kunt invoegen in Marketo via de SOAP API en deze kunt koppelen aan bedrijven en leads. Het begint met een verklaring van hoe dit proces werkt en verstrekt dan codesteekproeven voor elk van de scenario&#39;s.

**Diagram van de Structuur van de Lijst** eerst van allen, beschrijft het diagram hieronder de lijststructuur. De id wordt automatisch gegenereerd bij het maken van een record (volgnummer). Het veld vet is vereist: alleen Opportunity Person Role heeft verplichte velden. De velden tussen haakjes zijn optioneel en dat geldt ook voor de verbindingen met een stippellijn.

**Basistoevoeging van de Kans** u neemt eerst de Kans op, en toen de Rol van de Persoon van de Kans, die de Kans aan Lood(en) verbindt. Voor dit voorbeeld, gebruik duidelijk identiteitskaart van de Lood en identiteitskaart van de Kans om de Rol van de Persoon van de Kans te specificeren. De opportunity-id wordt weergegeven in de SOAP-respons wanneer u de Opportunity maakt. De lead-id is zichtbaar op elke lead in de Marketo Lead-database.

**Verbinding van het Bedrijf** in de meeste gevallen, wilt u een Kans aan een Bedrijf, naast een individu verbinden. In Marketo hebt u geen toegang tot de bedrijfsrecords via de SOAP API, alleen tot de leidende records (de lead-records bevatten de bedrijfsvelden). Het is nog mogelijk om Kansen aan een Bedrijf te verbinden door een uniek herkenningsteken van het Bedrijf aan elke Lood toe te voegen, en die identiteitskaart in uw Kans te gebruiken. Stap 1 moet een gebied van identiteitskaart van het Bedrijf op het loodverslag tot stand brengen en het vullen met een uniek herkenningsteken, gewoonlijk van een achterste deelsysteem. Stap 2 moet een gebied van het &quot;Bedrijf van identiteitskaart&quot;op de Kans tot stand brengen: u moet de Steun van Marketo of het Consulting vragen om zulk een gebied voor u tot stand te brengen. Dan bevolk dit gebied wanneer het creëren van Kansen, die de Kans aan het Bedrijf verbindt. Dit is vooral belangrijk als u de Analyse van de Cyclus van de Opbrengst van Marketo gebruikt en u de Analysator van de Invloed van de Kans wilt gebruiken, die van de informatie van het Bedrijf afhangt.

**Gebruikend Externe Herkenningstekens** in veel gevallen, kunt u uw eigen unieke herkenningstekens wanneer het integreren met een achterste deelsysteem hebben. Het is mogelijk deze unieke id&#39;s in Marketo te gebruiken via buitenlandse sleutels. Voor Leads gebruikt u doorgaans de FSPID (Foreign System Person Id), die wordt gebruikt in plaats van de Marketo-id of het e-mailadres als unieke id. De FSPID is een verborgen systeemveld dat niet zichtbaar is in Marketo. Als u dit nog niet doet, is het nodig dat Opportunity-synchronisatie de FSPID ook in een aangepast veld opslaat, bijvoorbeeld &#39;Buitenlandse id&#39; (u kunt het veld een naam geven). U kunt dit veld zelf maken als Marketo-beheerder. Voor Kansen, hebt u de Steun van Marketo een ander douaneveld op Kansen, bijvoorbeeld genoemd &quot;Buitenlandse Id&quot;tot stand brengen (u kunt het om het even wat noemen). Vul dit veld vervolgens in wanneer u Opportunity invoegt. Tot slot wanneer u de Rol van de Person van de Opportunity creeert, gebruikt u zowel buitenlandse sleutels om het verband tussen Lood en Kansen te specificeren, in plaats van het gebruiken van Marketo IDs. U kunt de buitenlandse sleutels ook gebruiken om Leidingen van Kansen bij te werken. Op dit ogenblik, is het niet mogelijk om een buitenlandse sleutel aan de verslagen van de Rol van de Person van de Opportunity toe te voegen, zodat zult u auto-geproduceerde identiteitskaart van Marketo voor dit moeten gebruiken (die u in de reactie van SOAP krijgt nadat het creëren van de Rol van de Persoon van de Opportunity).

**het Voorbeeld van de Code** Stappen: 1. Voeg de lead in/werk deze bij met de externe sleutel en bedrijfs-id 1. Voeg Opportunity in met External Key 1. Voeg de Rol van de Person van de Kans met Buitenlandse Sleutels 1 in. SOAP-verzoek - bestaande lead bijwerken met Foreign Key &amp; Company ID Dit werkt een bestaande lead (Marketo-id &quot;6&quot;) bij met de Foreign Key &quot;12346&quot; en de account-/bedrijfs-id &quot;C123&quot;. We sparen ook de Buitenlandse Sleutel op een douanegebied, omdat wij dat voor de Persrol van de Opportunity nodig hebben. Het gebruik van een externe sleutel is optioneel: u kunt de Marketo-id ook gebruiken om deze lead aan de opportunity te koppelen. Het gebruiken van bedrijfs identiteitskaart is ook facultatief, maar het is vereist als u de Analysator van de Invloed van de Kans in RCA wilt gebruiken. Verzoek:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncLead>
         <leadRecord>
            <Id>6</Id>
            <ForeignSysPersonId>12346</ForeignSysPersonId>
            <leadAttributeList>
               <attribute>
                  <attrName>FSPID</attrName>
                  <attrValue>12346</attrValue>
               </attribute>
               <attribute>
                  <attrName>cAccountFSID</attrName>
                  <attrValue>C123</attrValue>
               </attribute>
            </leadAttributeList>
         </leadRecord>
         <returnLead>false</returnLead>
      </mkt:paramsSyncLead>
   </soapenv:Body>
</soapenv:Envelope>
```

Reactie:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:xsi="<http://www.w3.org/2001/XMLSchema-instance>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncLead>
         <result>
            <leadId>6</leadId>
            <syncStatus>
               <leadId>6</leadId>
               <status>UPDATED</status>
               <error xsi:nil="true"/>
            </syncStatus>
            <leadRecord xsi:nil="true"/>
         </result>
      </ns1:successSyncLead>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-aanvraag - Opportunity maken In dit geval zijn er twee aangepaste velden gemaakt in de tabel Opportunity: - `opportunityId` → bevat de unieke opportunity-id - `cAccountFSID` → bevat de bedrijfreferentie in plaats van uw eigen opportuniteid op te geven. U kunt ook de door Marketo gegenereerde opportunity-id gebruiken. In dat geval laat u het knooppunt External Key weg. De vereniging van het Bedrijf is ook facultatief, maar vereist als u de Analysator van de Invloed van de Kans in RCA wilt gebruiken. Verzoek:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:03:28-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_4</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 4 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>501.00</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>501</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Reactie:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>40</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_4</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP-aanvraag - Opportunity Person Rol Deze aanvraag koppelt de lead aan de opportunity. U kunt meerdere koppelingen opgeven in één SOAP-aanvraag (dit voorbeeld koppelt de opportunity aan slechts 1 lead). Dit gebruikt de buitenlandse sleutels om de verbinding te specificeren, maar in de commentaren toont het ook hoe te om daadwerkelijke IDs te gebruiken (in dit geval: 6 voor Lood Id en 40 voor de Identiteitskaart van de Mogelijkheid). Deze velden &#39;&#39;IsPrimary&#39;&#39; en &#39;&#39;Role&#39;&#39; zijn optioneel. Verzoek:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:18:30-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <!--id>40</id-->
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_4</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Reactie:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>11</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Alternatieve Benadering (doe Stap 2 en 3 in Één Vraag)** terwijl u de kans, dan de Rol van de Persoon van de Kans kunt eerst opnemen, is het ook mogelijk om dit in één Vraag van SOAP te doen. Nochtans, moet u de Buitenlandse Sleutel voor Kans gebruiken (u kunt niet auto-geproduceerde identiteitskaart van de Kans in de Rol van de Persoon van de Kans gebruiken, omdat de Kans nog niet is geproduceerd). Natuurlijk, kunt u veelvoudige Leads aan deze Kans in deze zelfde API Vraag ook verbinden (dit voorbeeld koppelt de Kans aan slechts 1 Lood). Verzoek:

```xml
<soapenv:Envelope xmlns:soapenv="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:mkt="<http://www.marketo.com/mktows/">
   <soapenv:Header>
      <mkt:AuthenticationHeader>
         <mktowsUserId>\*\*\*</mktowsUserId>
         <requestSignature>\*\*\*</requestSignature>
         <requestTimestamp>2014-11-20T15:44:08-07:00</requestTimestamp>
      </mkt:AuthenticationHeader>
   </soapenv:Header>
   <soapenv:Body>
      <mkt:paramsSyncMObjects>
         <mObjectList>
            <!--Zero or more repetitions:-->
            <mObject>
               <type>Opportunity</type>
               <externalKey>
                  <name>opportunityId</name>
                  <value>Opportunity_5</value>
               </externalKey>
               <attribList>
                  <attrib>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </attrib>
                  <attrib>
                     <name>Name</name>
                     <value>Opportunity 5 for ACME</value>
                  </attrib>
                  <attrib>
                     <name>IsClosed</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>IsWon</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Amount</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>CloseDate</name>
                     <value>2014-10-24</value>
                  </attrib>
                  <attrib>
                     <name>ExpectedRevenue</name>
                     <value>1500</value>
                  </attrib>
                  <attrib>
                     <name>Probability</name>
                     <value>100</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Company</mObjType>
                     <externalKey>
                        <name>cAccountFSID</name>
                        <value>C123</value>
                     </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
             <mObject>
               <type>OpportunityPersonRole</type>
               <attribList>
                  <attrib>
                     <name>IsPrimary</name>
                     <value>1</value>
                  </attrib>
                  <attrib>
                     <name>Role</name>
                     <value>Marketing Manager</value>
                  </attrib>
               </attribList>
               <associationList>
                  <mObjAssociation>
                     <mObjType>Lead</mObjType>
                     <!--id>6</id-->
                     <externalKey>
                      <name>FSPID</name>
                      <value>12346</value>
                     </externalKey>
                  </mObjAssociation>
                  <mObjAssociation>
                     <mObjType>Opportunity</mObjType>
                     <externalKey>
                      <name>opportunityId</name>
                      <value>Opportunity_5</value>
                    </externalKey>
                  </mObjAssociation>
               </associationList>
            </mObject>
         </mObjectList>
         <operation>UPSERT</operation>
      </mkt:paramsSyncMObjects>
   </soapenv:Body>
</soapenv:Envelope>
```

Reactie:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
   <SOAP-ENV:Body>
      <ns1:successSyncMObjects>
         <result>
            <mObjStatusList>
               <mObjStatus>
                  <id>41</id>
                  <externalKey>
                     <name>opportunityId</name>
                     <value>Opportunity_5</value>
                  </externalKey>
                  <status>CREATED</status>
               </mObjStatus>
               <mObjStatus>
                  <id>12</id>
                  <status>CREATED</status>
               </mObjStatus>
            </mObjStatusList>
         </result>
      </ns1:successSyncMObjects>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

GEPast op _2014-11-26_ door _Jep_

## Verzoeken voor REST API met meerdere verbindingen

Als u de prestaties wilt verbeteren bij het aanroepen van de Marketo API, kunt u meerdere aanvragen tegelijk indienen. Deze benadering staat u toe om meer gegevens in een kortere periode te krijgen. Wanneer het doen van een API verzoek, is een deel van de round-trip tijd tussen de cliënt en de server de overdrachtstijd op de draad. Dus als we de overdrachtstijd op de kabel voor de aanvragen samen kunnen verminderen, verbeteren we de prestaties. In de voorbeeldcode hieronder ziet u hoe u dit in Ruby doet. Het gebruikt EventMachine, die een [ gebeurtenis-verwerkende bibliotheek is die voor het maken van multithreaded verzoeken ](https://github.com/igrigorik/em-http-request/wiki/Parallel-Requests) wordt gebruikt. Het voorbeeld hieronder roept [ Activiteiten API van de Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET), en maakt twee gezamenlijke verzoeken. Deze benadering elimineert de overdrachtstijd van de cliënt aan de server voor het tweede verzoek. Zij doet dit door het tweede verzoek op hetzelfde tijdstip als het eerste verzoek in te dienen. De API-reacties worden naar een tekstbestand geschreven.

```java
require 'em-http-request'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>"
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = ["&nextPageToken=A5YMOYZQBOGD2OSYYBYDAQGEMGLBDGDANAABQGRAQWAAKKID", "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"]
# Specify activities needed
activity_type_ids = "&activityTypeIds=1&activityTypeIds=12"
requesturl_a = marketo_instance + endpoint + auth_token + since_date_time.at(0) + activity_type_ids
requesturl_b = marketo_instance + endpoint + auth_token + since_date_time.at(1) + activity_type_ids

# Make request
EventMachine.run do
  http1 = EventMachine::HttpRequest.new(requesturl_a).get
  http2 = EventMachine::HttpRequest.new(requesturl_b).get

# When API response is received, write response to a text file
  http1.callback {
  File.open('response1.txt', 'w') do |t|
  t.puts http1.response
  end }

  http2.callback {
  File.open('response2.txt', 'w') do |t|
  t.puts http2.response
  end }
end
```

Gepast op _2014-12-03_ door _Murta_

## Aanvragen voor afstemmen van prestaties voor API

In dit artikel worden strategieën besproken om de prestaties te verbeteren wanneer gegevens worden aangevraagd van de Marketo API. U moet echter de voordelen van deze strategieën afwegen tegen de operationele beperkingen van de dagelijkse limieten van Marketo API.
**Strategie 1 - verzoek Minder Gegevens in Elke API Vraag** over het algemeen, aangezien u om meer gegevens in een API vraag verzoekt, de hoeveelheid tijd het neemt om omhoog de gegevens in het gegevensbestand door de de serververhogingen van Marketo te kijken. Als u een API vraag met datumwaaiers, zoals [ getMultipleLeads SOAP API ](/help/soap-api/getmultipleleads.md) maakt, verkort de tijdwaaier per vraag en compenseert met meer vraag. Bijvoorbeeld, in plaats van het vragen van gegevens van 1 Juni aan 1 juli, verzoek één enkele dag tegelijkertijd, zoals één vraag voor 1 Juni aan 2, en dan een andere vraag voor 2 juni aan 1. Als u een API-aanroep maakt die gegevens retourneert vanuit hoofdvelden van Marketo, vraagt u alleen die velden aan. Elke extra loodveld verhoogt incrementeel de tijd die een API-aanroep nodig heeft. Een andere manier is de partijgrootte te verminderen, of het aantal lood gevraagde per vraag.
**Strategie 2 - maak Gelijktijdige Verzoeken** om prestaties te verbeteren en meer gegevens in één keer te trekken. U kunt meerdere aanvragen tegelijk indienen bij de API. Deze benadering verkort de tijd op draadAPI verzoeken in totaal uit te geven. Bijvoorbeeld, laten wij zeggen u verzoeken aan Get Veelvoudige Leidingen door het Type van Filter doet. U kunt meerdere aanvragen tegelijk indienen voor een aanvraag waarin de vragen 1 tot en met 300 worden gesteld en voor een andere aanvraag waarin de vragen 301 tot en met 600 worden gesteld.
**Strategie 3 - de Gegevens van het Geheime voorgeheugen** Sommige gegevens in Marketo worden minder vaak veranderd, zoals de lijst van loodgebieden, dan andere gegevens, zoals de gegevens van de loodactiviteit. Als u gegevens in cache plaatst die minder vaak worden bijgewerkt, verlaagt u het aantal API-aanroepen dat u moet maken. U krijgt ook betere prestaties omdat het omhoog zoeken van de gegevens plaatselijk sneller is dan het toegang tot van de verre Webdienst.

Gepast op _2014-12-05_ door _Murta_

## Marketo-formuliergegevens naar Google Analytics verzenden

In Google Analytics kunt u aangepaste gegevensgebeurtenissen verzenden en deze vervolgens gebruiken om de prestaties van uw website te segmenteren en te analyseren. Met het onderstaande JavaScript-codefragment kunt u automatisch Marketo 2.0-formuliergegevens naar Google Analytics duwen nadat een bezoeker een webformulier heeft verzonden. Hieronder wordt beschreven hoe u dit instelt.

**Stap Één** Tussenvoegsel de markering van JavaScript op om het even welke pagina die Marketo Forms bij de bodem van de code (vóór de markering) omvat. De JavaScript verzendt alleen niet-verborgen velden (sendHiddenFields : false). Dit kan worden aangepast door sendHiddenFields van vals in waar te veranderen. U kunt ook velden selecteren die u wilt uitsluiten door extra veld-id&#39;s toe te voegen in de array &#39;fieldsToExclude&#39;.

```javascript
function pushFormDataToGa(a){
setTimeout(function () {
document.getElementsByTagName('form')[0].getElementsByClassName(a.submitButton)[0].addEventListener('click', function() { 
  allFields = document.getElementsByTagName('form')[0].getElementsByTagName('input');
  for(i=0;i<allFields.length;i++){
   if( (allFields[i].type !="hidden" && allFields[i].type !="submit" && allFields[i].value !="" && a.fieldsToExclude.indexOf(allFields[i].id) === -1  ) || (allFields[i].type === "hidden" && a.sendHiddenFields) ){
    console.log( allFields[i].name + ": "  + allFields[i].value);
    if(typeof(_gaq) != "undefined"){
    //Classic
    _trackEvent("Marketo Form Submission", allFields[i].value , allFields[i].name 
{'nonInteraction': 1});
    }else if(typeof(ga) !="undefined"){
    //Universal
    ga('send', 'event',"Marketo Form Submission", allFields[i].value , allFields[i].name, {'nonInteraction': 1});
}}}}, false);
}, 3000);}
pushFormDataToGa({
 submitButton: "mktoButton",
 fieldsToExclude: ["Email","LastName", "FirstName"],
 sendHiddenFields : false  
});
```

**Stap Twee** De gegevens in GA verschijnen in de Rapporterende sectie. Ga naar Gedrag > Gebeurtenissen > Belangrijkste gebeurtenissen. **Beperkingen van het Manuscript:** - Deze codesteekproef is slechts compatibel met [ Marketo Forms 2.0 ](/help/javascript-api/forms-api-reference.md). - Vanwege het privacybeleid van Google kun je geen persoonlijke gegevens (e-mail of naam) verzenden. Naast potentiële privacyzorgen, is dit persoonlijk identificeerbare info en zo schendt [ de Termen van de Analyse van Google van Dienst ](https://marketingplatform.google.com/about/analytics/terms/us/):

&quot;U zult (en zal derden niet toestaan) de Service niet gebruiken om gegevens te volgen, te verzamelen of te uploaden die een individu persoonlijk identificeren (zoals een naam, e-mailadres of factureringsinformatie), of andere gegevens die redelijkerwijs aan dergelijke informatie door Google kunnen worden gekoppeld.&quot;

Gepost op _2014-12-16_ door _Yanir_

## Een veld Volledige naam toevoegen aan een Marketo-formulier

We weten dat kortere webformulieren de conversiekoersen verbeteren. In het onderstaande JavaScript-codevoorbeeld kunt u uw formulieren nog korter maken door de velden Voornaam en Achternaam samen te voegen tot één veld Volledige naam. Wanneer bezoekers hun volledige naam invoeren, wordt de tekst door het script automatisch omgezet in de velden voor voornaam en achternaam. Voor bekende bezoekers wordt het script samengevoegd met de eerste en laatste naam en worden deze naar het nieuwe veld gekopieerd, zodat ze het veld niet opnieuw hoeven te vullen. Hieronder wordt beschreven hoe u dit instelt.

**Stap Één** leidt tot een nieuw douanegebied in Marketo genoemd Volledige Naam. U hoeft het niet te maken op uw CRM-platform, omdat het script alleen dit veld gebruikt om de volledige naam weer te geven.
**Stap Twee** voegt dit gebied aan al uw Webvormen toe. Stel de velden voor uw voornaam en achternaam in als verborgen. Wijzig in de JavaScript de configuratie &quot;splitFullName&quot; zodat deze de drie veldnamen bevat. Opmerking: zorg dat deze namen nergens anders op de pagina voorkomen.
**Stap Drie** Tussenvoegsel JavaScript in al uw landende pagina&#39;s bij de bodem van de code, vóór de markering.

```javascript
<script>
MktoForms2.whenReady(function (form){
        function splitFullName(a,b,c){
                String.prototype.capitalize = function(){
                        return this.replace( /(^|s)([a-z])/g , function(m,p1,p2){ return p1+p2.toUpperCase(); } );
                };
                document.getElementsByName[c](0).oninput=function(){
                        var fullName = document.getElementsByName[c](0).value;
                        if((fullName.match(/ /g) || []).length ===0 || fullName.substring(fullName.indexOf(" ")+1,fullName.length) === ""){
                                var first = fullName.capitalize();;
                                var last = "null";
                        }else if(fullName.substring(0,fullName.indexOf(" ")).indexOf(".")>-1){
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize() + " " + fullName.substring(fullName.indexOf(" ")+1,fullName.length).substring(0,fullName.substring(fullName.indexOf(" ")+1,fullName.length).indexOf(" ")).capitalize();
                                var last = fullName.substring(first.length +1,fullName.length).capitalize();
                        }else{
                                var first = fullName.substring(0,fullName.indexOf(" ")).capitalize();
                                var last = fullName.substring(fullName.indexOf(" ")+1,fullName.length).capitalize();
                        }
                        document.getElementsByName[a](0).value = first;
                        document.getElementsByName[b](0).value = last;
                };
                //Initial Values
                if(document.getElementsByName[c](0).value.length < 2 && document.getElementsByName[b](0).value.length.length >2 && document.getElementsByName[a](0).value.length.length >2 ){
                        var first = document.getElementsByName[a](0).value.capitalize();
                        var last = document.getElementsByName[b](0).value.capitalize();
                        var fullName =  first + " " + last ;
                        console.log(fullName);
                        document.getElementsByName[c](0).value = fullName;
                }
        }
        splitFullName("FirstName","LastName","leadFullName");
});
</script>
```

Opmerking: deze code werkt alleen met Marketo Forms 2.0.

Gepost op _2014-12-16_ door _Yanir_

## Gebruik cURL om leads te importeren via de REST API

Wilt u leads vanuit een CSV-bestand importeren via de REST API, maar dit is lastig om te doen met de Postman Chrome-extensie. In dit artikel bekijken we hoe we dit met cURL kunnen doen.

1. [ Download en installeer cURL ](https://curl.se/download.html), een hulpmiddel van de bevellijn wij gebruiken om gegevens aan Marketo REST API te duwen.
1. Open de opdrachtregel en navigeer naar de locatie waar het CSV-bestand zich bevindt. De kolomkoppen in het CSV-bestand moeten overeenkomen met de API-veldnamen, niet met de Marketo-veldnamen.
1. U hebt een toegangstoken nodig. Meld u aan bij Marketo, ga naar Beheer en vervolgens naar LaunchPoint. Zoek de REST API-gebruiker en klik op Details weergeven. Klik op de knop Token ophalen.
1. U zult ook uw REST eindpunt nodig hebben dat voor uw instantie van Marketo specifiek is. Meld u aan bij Marketo, ga naar Admin en vervolgens naar Webservices. In de sectie met de markering &quot;REST API&quot; vindt u de URL van het eindpunt.
1. Voor de bevellijn, volg dit formaat voor de cURL vraag. Vervang `<accesstoken>` door uw toegangstoken uit Stap drie en vervang `<REST API Endpoint URL>` door uw URL van het Eindpunt van REST API van Stap vier. Meer info is [ beschikbaar hier ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/importLeadUsingPOST). De &quot;/bulk&quot;hier zal &quot;/rest&quot;aan het eind van het Eindpunt URL vervangen. Als u het eindpunt hebt dat voor /rest/bulk wordt geplaatst keert het een fout terug.

`curl -i -F format=csv -F file=@leaddata.csv -F access_token=<accesstoken> <REST API Endpoint URL>/bulk/v1/leads.json`

Gepast op _2014-12-16_ door _Jordaan_

## Een bevestigingsbericht toevoegen aan een Marketo voor

Wanneer een gebruiker op de knop &quot;Verzenden&quot; op een Marketo-formulier klikt, wilt u bijvoorbeeld een bericht weergeven waarin de gebruiker wordt gevraagd of het echt oké is om te verzenden. Dit is mogelijk door een paar regels JavaScript te implementeren, die een bevestigingsvak weergeven wanneer iemand op de knop Verzenden klikt. Hier is een voorbeeld van hoe dit te doen. Voeg de functie onSubmit toe aan uw Marketo-formulier, zoals hieronder wordt weergegeven. Voor meer informatie over Marketo Forms API, gelieve [ de ontwikkelaardocumentatie ](/help/javascript-api/forms-api-reference.md) uit te checken.

```javascript
<script src="//app-e.marketo.com/js/forms2/js/forms2.js"></script>
<form id="mktoForm_19"></form>
<script>
MktoForms2.loadForm("//app-e.marketo.com", "212-RBI-463", 19,function(form){
 
//Add this function to your Marketo form script
form.onSubmit(function(){
alert("Do you really want to submit the form?");
});
});
</script>
```

Gepost op _2014-12-17_ door _David_

## Bericht met dank weergeven zonder opvolgingspagina

Als u Marketo-formulieren gebruikt, maakt u doorgaans twee bestemmingspagina&#39;s: een pagina waarop het formulier wordt geplaatst en een pagina waarnaar het formulier wordt omgeleid nadat het formulier is ingevuld. In sommige gevallen wilt u echter wellicht geen twee aparte, maar zeer vergelijkbare bestemmingspagina&#39;s behouden. U kunt dezelfde bestemmingspagina gebruiken voor het formulier en voor het bedankbericht met de Forms 2.0 JavaScript API. Hiertoe maakt u eerst de bestemmingspagina en het formulier voor registratie en plaatst u het formulier op de bestemmingspagina zoals u dat gewoonlijk zou doen. Voeg vervolgens een HTML-element aan de pagina toe. In dit element voegen we code toe die wordt geactiveerd op het moment dat het formulier wordt verzonden. Vervolgens wordt het formulier verborgen en wordt een verborgen formulier weergegeven <div> dat bevat de boodschap van dank aan u . Je JavaScript moet er als volgt uitzien:

```javascript
//Edit host with your Marketo instance info
<script src="//<host>/js/forms2/js/forms2.js"></script>
<script>
MktoForms2.whenReady(function (form){
 //Add an onSuccess handler
  form.onSuccess(function(values, followUpUrl){
   //get the form's jQuery element and hide it
   form.getFormElem().hide();
   document.getElementById('confirmform').style.visibility = 'visible';
   //return false to prevent the submission handler from taking the lead to the follow up url.
   return false;
 });
});
</script>
```

Bewerk de berichttekst van bedankt.

`<div id="confirmform" style="visibility:hidden;"><p><strong>Thank you. Check your email for details on your request.</strong></p></div>`

U zult de gastheernaam en dank u bericht in de codesteekproef willen uitgeven. De eerste moet verwijzen naar uw Marketo-exemplaar (bijvoorbeeld &quot;//app-sj06.marketo.com/js/forms2/js/forms2.js&quot;) en de tweede moet de tekst bevatten die u wilt weergeven nadat het formulier is voltooid. De tekst wordt op de bestemmingspagina weergegeven op de exacte positie waar u het HTML-element plaatst, dus zorg dat u dat bewerkt in het eigenschappenblad. Zorg er ook voor dat de laag van het HTML-element kleiner is dan de laag voor het formulier. Standaard worden beide lagen op Laag 15 geplaatst, zodat u veilig bent als u HTML-elementenlaag 11 maakt. Als u dit niet doet, kunt u geen formulierveldvakken typen die het bedankbericht overlappen. Het is niet nodig het vervolgtype op het formulier of op de bestemmingspagina te wijzigen, aangezien de JavaScript deze instellingen overschrijft. Voor meer informatie over Marketo Forms API, gelieve de [ ontwikkelaardocumentatie ](/help/javascript-api/forms-api-reference.md) te controleren.

GEPast op _2014-12-19_ door _Kristin_

## Open Source-projecten markeren die zijn gebaseerd op het Marketo-platform

Dit is de eerste post in een lopende reeks waarin openbronprojecten worden belicht die door de ontwikkelaarsgemeenschap rond het Marketo-platform worden gebouwd. Wij handhaven [ een lijst op de rekening van GitHub van Marketo ](https://github.com/Marketo/Community-Supported-Client-Libraries) waar wij cliëntbibliotheken en projecten volgen die door de ontwikkelaarsgemeenschap van Marketo worden gecreeerd. Hieronder staan drie projecten die zijn ontwikkeld rond de Marketo REST- en SOAP-API&#39;s. Daniel Chesterton creeerde [ een cliëntbibliotheek in PHP voor Marketo REST API ](https://github.com/dchesterton/marketo-rest-api). De clientbibliotheek heeft momenteel dekking voor 12 REST API-eindpunten.** Kyle Halstvedt van Elixiter creeerde een project om [ lood van de statische lijsten van Marketo in een Spreadsheet van Google ](https://github.com/Elixiter/mkto_google-spreadsheet) te trekken. Kyle&#39;s project gebruikt de Marketo REST API.  David Santoso creeerde a [ Ruby gem voor Marketo SOAP API.](https://github.com/davidsantoso/markety) Met dit project kunt u de Marketo SOAP API sneller integreren met een Ruby-app op rails.  We zijn blij dat er meer projecten worden gemaakt door de ontwikkelaarscommunity op het Marketo-platform. Als u aan een open bronproject voor het platform van Marketo werkt, gelieve [ het aan deze repo GitHub via een trekkingsverzoek ](https://github.com/Marketo/Community-Supported-Client-Libraries) voor te leggen.

Gepast op _2015-01-02_ door _Murta_

## Pagina-inhoud dynamisch wijzigen op basis van de locatie van een gebruiker

Stel dat u het telefoonnummer op een bestemmingspagina dynamisch wilt wijzigen, afhankelijk van de locatie van de gebruiker. Bijvoorbeeld, als de persoon in Californië is zou u hen het telefoonaantal voor uw bureau van Californië op uw landingspagina willen tonen, maar als zij in Japan zijn, zou u hen het telefoonaantal voor het Japanse bureau willen tonen.  Één manier om dit uit te voeren gebruikt JavaScript en [ HTML5 Geolocation API ](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API). Het voordeel van deze aanpak is dat we één landingspagina kunnen maken en deze dynamisch kunnen wijzigen op basis van de locatie van de gebruiker, in plaats van meerdere statische landingspagina&#39;s. We doorlopen de details van de technische implementatie hieronder. We maken een object voor kantoorlocaties met breedte- en lengtecoördinaten en een tweede object met kantoortelefoonnummers. In de productie zou het beter zijn om deze twee objecten samen te voegen tot één object.

```json
//Coordinates for Marketo offices
var officeLocations = {
    "San Mateo": {latitude: 37.5596465, longitude: -122.2870142},
    "Atlanta": {latitude: 33.8547013, longitude: -84.35552349999999},
    "Tokyo": {latitude: 35.6895, longitude: 139.6917},
    "Dublin": {latitude: 53.3478, longitude: -6.2603097},
    "Sydney": {latitude: -33.873651, longitude: 151.2068896},
    "Portland": {latitude: 45.512089, longitude: -122.6763367},
    "Tel Aviv": {latitude: 32.0852999, longitude: 34.78176759999999}
}

//Phone numbers for Marketo offices
var officePhoneNumbers = {
    "San Mateo": "+1-650-376-2300",
    "Atlanta": "+1-877-260-6586",
    "Tokyo": "+81-03-6759-8280",
    "Dublin": "+353-1-242-3000",
    "Sydney": "+61-2-9045-2711",
    "Portland": "+1-877-260-6586",
    "Tel Aviv": "+1-877-260-6586"
}
```

We maken een methode om de locatie van een gebruiker aan te vragen. Om fouten te behandelen, als de plaats van de gebruiker niet toegankelijk is, zullen wij aan het hoofdkwartier van Marketo en zijn telefoonaantal in gebreke blijven.

```javascript
//Method to get user's current location. Returns a position object with user's geo coordinates
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(findNearestOffice);
    } else {
        x.innerHTML = "Marketo Location: San Mateo
Marketo Phone Number: +1-877-260-6586";
    }
}
```

Tot slot creëren wij een methode om het dichtste bureau aan de plaats van de gebruiker te vinden, en dan het telefoonaantal voor het dichtste bureau op de pagina terug te keren. Deze methode gebruikt de findNearest methode van [ Geolib, die een bibliotheek van JavaScript is die georuimtelijke verrichtingen ](https://github.com/manuelbieh/Geolib) verstrekt.

```javascript
//Find nearest Marketo office to user's location
function findNearestOffice(position) {
        var nearestOffice = geolib.findNearest({latitude: position.coords.latitude, longitude: position.coords.longitude}, officeLocations);
        x.innerHTML = "Marketo Location: " + nearestOffice.key + "
Marketo Phone Number: " +  officePhoneNumbers[nearestOffice.key];
}
```

Hier is de volledige implementatie. We activeren de methode getLocation wanneer de gebruiker op de knop op de pagina klikt. Dit [ GitHub repo ](https://github.com/MurtzaM/Find-Nearest-Marketo-Office) heeft de dossiers nodig aan opstelling deze demo.

Gepast op _2014-12-20_ door _Murta_

## Regelafstand bijhouden en meerdere domeinen

Met de trackingcode van Marketo Munchkin kunt u bezoeken aan uw website volgen. U wilt waarschijnlijk code voor het bijhouden van Munchkin gebruiken om anonieme leads te cookiëren voor de meeste of alle pagina&#39;s op uw website. Laten we doorlopen hoe Munchkin werkt. Bezoeken naar de pagina worden geregistreerd voor bestaande leads en een bezoek van een niet-gekoelde bezoeker aan de pagina zorgt ervoor dat een nieuwe cookie wordt gemaakt en opgeslagen en dat een nieuwe anonieme lead in uw Marketo-database wordt gemaakt. De Munchkin-tracker cookie automatisch een bezoeker als deze nog geen bestaande cookie voor het huidige domein heeft. In Marketo wordt de gebeurtenis geregistreerd (klik op een koppeling, bezoek een webpagina of een nieuwe lead) in het activiteitenlog van de lead. De in het cookie opgeslagen waarde is uniek voor een bepaalde bezoeker. De waarde bestaat uit een combinatie van de unieke id voor het bijhouden van Munchkin-accounts, de domeinnaam, het tijdstempel en een willekeurig geheel getal.
**wat gebeurt als ik veelvoudige domeinen heb?** Hier kunt u twee sites opgeven die u wilt bijhouden: `<www.apples.com>` en `<www.bananas.com>` . U kunt de volgende code op beide sites plaatsen, maar u moet het volgende overwegen. Marketo cookies zijn &#39;first-party cookies&#39; en zijn daarom domeinspecifiek. Dit betekent dat een bezoeker van site 1 wordt aangemaakt als een anonieme lead in Marketo. Als dezelfde lead vervolgens naar site 2 gaat, wordt een tweede anonieme lead in Marketo gemaakt. Als de lead een formulier invult op locatie 1, dan wordt deze record gekend, blijft de anonieme record voor site 2 behouden en blijven de volgende bezoeken aan die site zich opstapelen. Als de lead vervolgens een formulier op site 2 invult met exact hetzelfde e-mailadres als gebruikt op site 1, worden beide bekende leads automatisch samengevoegd en wordt al het oude en toekomstige gedrag bijgehouden in één record in Marketo. Beide cookie-id&#39;s zijn gekoppeld aan dezelfde lead en alle webactiviteit (van een van beide domeinen) bevindt zich op die lead.
**wat over veelvoudige subdomeinen?** Subdomeinen zijn geen probleem. Laten we Marketo.com als voorbeeld gebruiken. Het heeft meerdere subdomeinen voor verschillende talen, zoals fr.marketo.com en de.marketo.com. Met subdomeinen worden alle activiteiten opgenomen tegen dezelfde lead record/cookie.

Gepost op _2015-01-13_ door _David_

## De tekstkleur voor de tip wijzigen in een Marketo-formulier

Stel dat u de kleur van de hinttekst (ook wel de plaatsaanduidingstekst genoemd) wilt wijzigen in Forms 2.0. Dit is mogelijk via aangepaste CSS. In de onderstaande schermafbeelding heb ik bijvoorbeeld de hinttekst in dit Marketo-formulier blauw gemaakt. Er zijn drie opties om dit te doen afhankelijk van hoe u Marketo Forms gebruikt.

**Optie 1: Als u een vorm van Marketo inbedt, voeg direct CSS hieronder aan uw belangrijkste CSS dossier toe.**

```css
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder { 
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder {
  color: blue;
}
```

**Optie 2: Wanneer u een vorm van Marketo inbedt, kunt u CSS direct op de pagina tussen `<style></style>` markeringen in de `<head>` sectie toevoegen.**

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder { 
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder { 
  color: blue;
}
</style>
```

**Optie 3: Als u een vorm van Marketo op een Marketo landende pagina gebruikt, kunt u dit douanecCSS door Marketo UI toevoegen.** Zoek de openingspagina in de Marketo-navigatiestructuur. Klik vervolgens op Concept bewerken. Klik op Metatags voor pagina bewerken. Voeg de CSS hieronder toe aan de sectie Custom HEAD HTML. De `<style></style>` -tags moeten worden opgenomen.

```css
<style>
::-webkit-input-placeholder {
  color: blue;
}
::-moz-placeholder { 
  color: blue;
}
:-ms-input-placeholder {
  color: blue;
}
:-moz-placeholder { 
  color: blue;
}
</style>
```

Klik op Concept goedkeuren. Wanneer u nu de openingspagina van Marketo bezoekt, is de hinttekst de kleur die u in de CSS hebt gedefinieerd. Voor meer informatie over Marketo Forms, [ gelieve te bezoeken gelieve de documentatie ](/help/javascript-api/forms-api-reference.md).

Gepast op _2015-01-14_ door _Murta_

## Activiteitsgegevens ophalen via de REST API

Laten we zeggen dat je alle leads wilt ophalen die deze maand aan een lijst zijn toegevoegd. Gebruikend [ krijg de Activiteiten van het Lood REST API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET), kunt u deze gegevens krijgen. Alvorens te roepen krijg de Activiteiten API van de Leiding, is het noodzakelijk om een toegangstoken van de Authentificatie API te krijgen en ook een beginnend datumteken van [ krijgen Paging Symbolische API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getActivitiesPagingTokenUsingGET). Hieronder ziet u een voorbeeldcode in Ruby die de afzonderlijke API-eindpunten doorloopt die u moet aanroepen om alle leads die deze maand aan een lijst zijn toegevoegd, te retourneren. 1. Get Access Token**

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com/identity/oauth/token?grant_type=client_credentials>"
# Relace with your client id
client_id = "99985d09-22a9-3jl2-84av-f5baae7c3a45"
# Replace with your your  client secret
client_secret = "tZPVrKiEmUDezE18yZfeaPlTJ2vKn2fw"
request_url = marketo_instance + "&client_id=" + client_id + "&client_secret=" + client_secret

# Make request
response = RestClient.get request_url

# Parse reponse and return only access token
results = JSON.parse(response.body)
access_token = results["access_token"]
puts access_token
```

1. Paginasken ophalen

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>" 
endpoint = "/rest/v1/activities/pagingtoken.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify date
since_date_time = "&sinceDatetime=2015-01-01T00:00:00-08:00"
request_url = marketo_instance + endpoint + auth_token + since_date_time

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. Krijg Gegevens van de Activiteit** om identiteitskaart van het Type van Activiteit nodig voor deze vraag te bepalen, vraag [ vergaarde de Types API van Activiteit ](/help/rest-api/activities.md). De Get API van de Types van Activiteit keert een schema met alle activiteitentypes en bijbehorende ids terug. Zo wordt bijvoorbeeld id 12 geretourneerd voor nieuwe leads en id 1 voor een bezoek aan de webpagina.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>" 
endpoint = "/rest/v1/activities.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Specify datetime needed as nextPageToken
since_date_time = "&nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ"
# Specify activities needed
activity_type_ids = "&activityTypeIds=24"
request_url = marketo_instance + endpoint + auth_token + since_date_time + activity_type_ids

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. De Get Activiteiten API keert een het pagineren teken met elke reactie terug die u kunt gebruiken om door de resultaatreeks te pagineren.** Voor meer informatie, gelieve te zien de [ REST API documentatie ](/help/rest-api/rest-api.md).

Gepast op _2015-01-20_ door _Murta_

## Open Source-projecten markeren die zijn gebaseerd op het Marketo-platform: Deel twee

Dit is de tweede post in een lopende reeks waarin openbronprojecten worden belicht die door de ontwikkelaarsgemeenschap rond het Marketo-platform worden gebouwd. Wij handhaven [ een lijst op de rekening van GitHub van Marketo ](https://github.com/Marketo/Community-Supported-Client-Libraries) waar wij cliëntbibliotheken en projecten volgen die door de ontwikkelaarsgemeenschap van Marketo worden gecreeerd. Hieronder staan drie projecten die zijn ontwikkeld rond de Marketo SOAP en Munchkin API&#39;s. [ PunchTab ](https://www.punchtab.com/) creeerde [ een cliëntbibliotheek in Python voor Marketo SOAP API ](https://github.com/PunchTab/suds-marketo). [ Flickerbox ](https://www.flickerbox.com/) creeerde [ een cliëntbibliotheek in PHP voor Marketo SOAP API ](https://github.com/flickerbox/marketo).* [ Richard Morrison ](https://x.com/mozz100) creeerde [ een PHP manuscript om loodgegevens van Marketo SOAP API te krijgen, en dan deze gegevens tot de cliënt over te gaan gebruikend JavaScript.](https://github.com/mozz100/marketo-whodat) Met dit project kunt u een pagina wijzigen op basis van gebruikersgegevens in Marketo.  We zijn blij dat er meer projecten worden gemaakt door de ontwikkelaarscommunity op het Marketo-platform. Als u aan een open bronproject voor het platform van Marketo werkt, gelieve [ het aan deze repo GitHub via een trekkingsverzoek ](https://github.com/Marketo/Community-Supported-Client-Libraries) voor te leggen.

Gepast op _2015-01-20_ door _Murta_

## Aanbevolen de Motor van de Aanbeveling RTP klikt naar Analytische Google

Hier is een oplossing voor Marketo Real-Time Personalization (RTP) gebruikers om kliks van de Motor van de Aanbeveling van de Inhoud binnen Google Analytics te zien. Wanneer een bezoeker op de balk met aanbevelingen voor inhoud klikt, wordt een gebeurtenis naar Google Analytics verzonden onder Gebeurteniscategorie &quot;RTP-Recommendations&quot;. In Analytics wordt de Tekst van de Aanbeveling (zoals deze op de balk wordt weergegeven) toegevoegd aan het Gebeurtenislabel en wordt de URL van het aanbevolen element toegevoegd aan de Actie van de Gebeurtenis. Het script werkt voor zowel Classic Google Analytics als Google Universal Analytics. Deze tag moet aan het einde van de HTML-paginacode worden geplakt, zodat het de laatste tag voor de `</body>` -tag is.

```javascript
$( document ).ready(function() {
if(document.getElementsByClassName("insightera-bar-content").length
 >0){
document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].addEventListener("click",
 function(){
assetName
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].innerText;
assetURL
 = document.getElementsByClassName("insightera-bar-content")[0].getElementsByTagName('a')[0].href;
assetURL=
 assetURL.substring(assetURL.lastIndexOf("/"),assetURL.indexOf("?iesrc"));
console.log(assetName

 * " | " + assetURL);
if(typeof(_gaq)
 != "undefined"){
//Classic
_trackEvent("RTP-Recommendations",
 assetName , assetURL , {'nonInteraction': 1});
}else
 if(typeof(ga) !="undefined"){
//Universal
ga('send',
 'event',"RTP-Recommendations", assetName , assetURL, {'nonInteraction': 1});
}
});
}
});
```

GEPast op _2015-01-22_ door _Yanir_

## Marketo REST API met Boomi gebruiken: een REST Authentication Toke ophalen en opslaan

Het instellen van een automatische export van lood die aan bepaalde criteria voldoet, is een zeer gebruikelijk geval in Marketo. Hoewel dit op dit moment niet mogelijk is in de Marketo-interface, is het vrij eenvoudig om dit te doen met het hulpprogramma van derden, zoals Dell Boomi, een statische lijst met enkele gegevensbeheercampagnes en de Marketo REST API. De REST API? Ik dacht dat Boomi geen Marketo REST API-connector had! Op dit moment is dat niet het geval, maar het is wel mogelijk om hetzelfde te bereiken met de HTTP-connector en de jSON-responsvormen handmatig te definiëren. De eerste stap is opstelling uw instantie van Marketo om REST API te gebruiken zoals die in de [ wordt geschetst REST API de pagina van de Ontwikkelaar van Marketo ](/help/rest-api/rest-api.md). Ik ga er ook van uit dat u toegang hebt tot een Dell Boomi-account en over de Boomi-vaardigheden beschikt om dit soort integratieprocessen te maken. Het uiteindelijke proces ziet er als volgt uit en bevat oproepen naar de volgende Marketo REST API-bewerkingen, die elk een gekoppelde jSON-responsvorm hebben die op de ontwikkelaarssite te vinden is. Om tijd te besparen, heb ik hen onder Voorbeeld JSON voor [ Authentificatie ](/help/rest-api/authentication.md) vermeld

```json
{
  "access_token": "",
  "token_type": "",
  "expires_in": 0,
  "scope": ""
}
```

Voorbeeld JSON voor [ krijgt veelvoudige lood door identiteitskaart van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Voorbeeld JSON voor [ verwijdert Leads uit Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

Bepaal Eigenschappen: Alvorens wij beginnen REST te roepen, is het belangrijk om de variabelen te externaliseren en in te kapselen u gebruikt. Ik heb het volgende hieronder getoond bepaald.

* ClientID: haal dit op bij de REST Launchpoint Service
* Clientgeheim: haal dit van uw REST Launchpoint Service
* AccessToken: We krijgen dit via een REST-oproep
* Statische ListID: De LIJST-id van de statische lijst waarop we gaan werken. Haal dit op van de URL in Marketo
* Velden: een door komma&#39;s gescheiden lijst met velden die de rest van de service krijgt van Marketo voor elke lead. Mijnis &quot;id, email,firstName,lastName&quot; * IDStringToDelete: bevat uiteindelijk de id van alle leads in de statische lijst die moeten worden gebruikt om ze uit de lijst te verwijderen
* ActivityTypes: zal in Deel 2 van dit blog worden gebruikt, waar ik op dit uitspreek!
* SinceDateTime: Wordt gebruikt in deel 2 van deze blog, waar ik hier op uitga!
* PagingToken: Wordt gebruikt in deel 2 van dit blog, waar ik hier verder op uitga!
* Map - uitgaand: het pad naar de uitgaande map op de SFTP-server. Ik gebruik &quot;/data/outgoing&quot; in dit voorbeeld. Het staat ons toe om van de Verrichting van SFTP parameters te bepalen om het algemeen te maken.

De verificatietoken: zoals ik al zei, plaatsen we een connector op het canvas nadat we het proces hebben gemaakt met een beginvorm &quot;Geen gegevens&quot; (dit is gewoon een persoonlijke keuze, ik houd van al mijn connectors die er als Britse stekkers uitzien).
De connector moet als volgt worden geconfigureerd: - De connector is een HTTP GET-client - Verbinding gebruikt URL: `https://123-ABC-456.mktorest.com` (let op geen /rest aan het einde zodat we dit kunnen gebruiken voor REST-aanroepen en het kunnen gebruiken om het token voor identiteitstoegang te verkrijgen. en wijzig 123-ABC-456 in de juiste voor uw Marketo-instantie) - Bewerking is &quot;Get Auth Token&quot; (new!) - Verzoek Profiel = Geen - Reactieprofiel = JSON - Nieuw profiel genaamd &quot;Authentication Token Response&quot; - Inhoudstype: Plain - HTTP Methode: GET - Resource Path (add 4 zonder aanhalingstekens): &quot;identity/oAuth/token?type=type client_credentials&amp;client_id=&quot;; &quot;ClientID (Replacement variable)&quot;; &quot;&amp;client_SECret=&quot;; &quot;ClientSecret (Replacement variable)&quot; - Parameters instellen onder Configure —> Parameters —>(+): ClientID = Process Property ID; ClientSecret = Process Property Client Secret After this, sla het succestoken op in de variabele Process Properties &quot;AccessToken&quot;, zoals getoond, en extraheren de jSON-respons.
Het patroon voor deze stap wordt herhaald voor de volgende stappen, maar met nieuwe bewerkingen met verschillende jSON-retourprofielen. In feite zullen veel van de REST-oproepen op dezelfde manier worden behandeld met kleine veranderingen! In de volgende tranche, zullen wij op dit uitbreiden en een lijst van lood van een statische lijst krijgen gebruikend REST! Voer voorlopig het proces uit, maar plaats een stopvorm na uw &quot;Eigenschappen instellen&quot; en voer de foutopsporing uit om te controleren of u hetzelfde token ziet als in Marketo. Ze moeten perfect overeenkomen!

Gepast op _2015-01-26_ door _John_

## Een Google-API voor lettertypen gebruiken om een aangepast lettertype toe te voegen aan een Marketo-landingspagina

**Nota: Dit is een blogpost door [ Murtza Manzur ](https://www.linkedin.com/in/murtzam). Murtza is een Marketo Developer Evangelist uit de Bay Area van San Francisco.**
Stel dat u een bestemmingspagina maakt in Marketo en een aangepast lettertype wilt gebruiken. Dit is mogelijk met de Google Font API.  Voeg een importmethode toe aan uw CSS-bestand met verwijzing naar Google-lettertypen:

`@import url(http://fonts.googleapis.com/css?family=Open+Sans:400,300,600);`

Gepast op _2015-01-26_ door _Murta_

## De voornaam van een lead in hoofdletters maken met behulp van e-mailscripts

Laten we zeggen dat een lead zijn naam in kleine letters invoert, zoals &quot;John doe&quot;. Maar als je een e-mailcampagne stuurt, wil je de naam van de leider in de e-mail kapitaliseren, net als Jan Doe. U kunt de naam van een lead een hoofdletter geven met behulp van e-mailscripts. Zo doe je dit.

1. Klik in uw e-mailprogramma op het tabblad &quot;Mijn tokens&quot;.
1. Maak een nieuw e-mailscripttoken door &quot;E-mailscript&quot; van het rechterdeelvenster naar het middelste deelvenster te slepen. Geef het token een naam.
1. Plak de onderstaande code in het tekstvak Scripttoken bewerken. Schakel in het rechterdeelvenster onder Object Lead het selectievakje Voornaam in. Klik vervolgens op Opslaan.

```javascript
# set($name = ${lead.FirstName})
# set($formattedFirstName = $name.substring(0).toUpperCase())
$formattedFirstName
```

1. Verwijs naar het token in uw e-mailmiddel. De voornaam van de lead wordt weergegeven met de eerste letter een hoofdletter. Voor meer informatie over e-mailscripting, gelieve te bezoeken de [ e-mail scripting documentatie ](/help/email-scripting.md).

Gepast op _2015-01-26_ door _Murta_

## Alle leads ophalen van de Marketo REST API

Er was a [ vraag op StackOverflow vragend hoe te om een lijst van alle lood van Marketo door REST API ](https://stackoverflow.com/questions/28184900/how-do-i-get-the-list-of-all-the-leads-in-marketo) te krijgen. U kunt deze gegevens vragen gebruikend [ krijgt Veelvoudige Leidingen door het EIND API van het Type van Filter eindpunt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET). Leads in Marketo krijgen loodid toegewezen in opeenvolgende volgorde die begint met 1. Gebruikend [ krijgt Veelvoudige Leidingen door het EIND API van het Type van Filter eindpunt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), kunt u 300 lood door loodidentiteitskaart met elke vraag vragen vragen. U zou identiteitskaart als filterType en loodids als filterValues met elke vraag aan dit eindpunt moeten specificeren. Om alle leads te krijgen, doorloopt u het totale aantal leads 300 tegelijk. Y
U kunt het totale aantal leads in een Marketo-instantie ophalen via de interface van Marketo. Ga in de gebruikersinterface van Marketo naar het tabblad Lead Database, klik op System Smart Lists, klik op All Leads Smart List en klik ten slotte op het tabblad Leads. Klik vervolgens op de kolom Id en sorteer aflopend. Nadat de leads zijn gesorteerd, is de id van de eerste lead de bovenste binding voor de leads-id wanneer u alle leads opvraagt. Als u geen toegang tot Marketo UI hebt om de totale telling van lood te krijgen, is er een [ afwisselende benadering om deze waarde te krijgen gebruikend Get de Activiteiten van de Leiding REST API ](https://stackoverflow.com/questions/28419967/get-all-leads-programmatically-in-marketo-v1).

1. Eerste API-aanroep: vervang ... met alle waarden tussen:

`/rest/v1/leads.json?filterType=Id&filterValues=1,2,3,...,298,299,300`

Hier is een steekproefcode in Ruby voor de eerste vraag.

```java
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "<https://AAA-BBB-CCC.mktorest.com>" 
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab"
# Replace with filter type and values
ids_needed = (1..300).to_a.join(",")
filter_type_and_values = "&filterType=Id&filterValues=" + ids_needed
request_url = marketo_instance + endpoint + auth_token + filter_type_and_values

# Make request
response = RestClient.get request_url

# Returns Marketo API response
puts response
```

1. De tweede API-aanroep en elke volgende API-aanroep volgen hetzelfde patroon totdat het totale aantal leads wordt bereikt:

```
//replace ... with all the values in between
/rest/v1/leads.json?filterType=Id&filterValues=301,302,303,...,598,599,600
```

Voor meer informatie, gelieve te zien de [ REST API documentatie ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET).

Gepast op _2015-01-28_ door _Murta_

## Formulierverzendacties uitvoeren van iFrame naar bovenliggende pagina

We hebben een paar gevallen gezien waarin gebruikers iframe-formulieren gebruiken en bezoekers die het formulier hebben ingevuld, willen doorverwijzen naar een pagina voor bedankt, PDF, video, enzovoort. Het probleem is dat aangezien het formulier is ingesloten op een bestemmingspagina die afwijkt van de bovenliggende pagina, de handeling alleen plaatsvindt op de binnenpagina waar het formulier zich bevindt. Hieronder vindt u 2 JavaScript-tags die we hebben gemaakt. Voeg in als HTML-element op uw iframe-pagina&#39;s of rechtstreeks op de sjabloon voor de bestemmingspagina die u voor iframes gebruikt. Plaats het voor de laatste `</body>` -tag. De eerste tag voert de actie uit op de bovenliggende pagina en de tweede tag opent deze op een nieuw tabblad.

**Actie van de Vorm op een Ouderlijke Pagina**

```javascript
<script>
MktoForms2.whenReady(function (form){
form.onSuccess(function (values, url){
window.parent.location.assign(url);
return false;
           });
});
</script>
```

**Actie van de Vorm in een Nieuw Lusje**

```javascript
<script>
MktoForms2.whenReady(function (form){
var newWin;
form.onSubmit(function (){
newWin = window.open('about:blank', 'myWindow');
});
form.onSuccess(function (values, url){
newWin.location.replace(url);
return
 false;
});
});
</script>
```

GEPast op _2015-02-02_ door _Yanir_

## Weergavegegevens van een YouTube-video naar de markt verzenden

Laten we zeggen dat u leads in Marketo wilt segmenteren op basis van of ze een specifieke video hebben gestart of voltooid. Dit is mogelijk bij gebruik van Munchkin, YouTube Iframe API en Smart Lists in Marketo. Met de voorbeeldcode in dit artikel kunt u video starten en video voltooide gebeurtenissen naar Marketo verzenden via Munchkin. Dit werkt alleen als Munchkin op de pagina is geladen voordat u videoweergavegebeurtenissen naar Marketo kunt verzenden. De video die is gestart en voltooid, wordt weergegeven in het activiteitenlogboek van de lead. Nadat de gegevens in Marketo zijn opgeslagen, kunt u een slimme lijst en segmentleads maken die een video hebben gestart of voltooid.

1. Haal de id op van de YouTube-video die u wilt insluiten.** Let op de id (de reeks willekeurige tekens na `v=` ) vanaf de URL van de YouTube-video die u wilt gebruiken.
1. Plaats de video-id van YouTube uit stap 1 op de achtste regel van dit codevoorbeeld. Plaats de code vervolgens voor de `</body>` -pagina in de HTML van uw pagina.

```javascript
<div id="player"></div>
<script>
var tag = document.createElement('script');
tag.src = "https://www.youtube.com/iframe_api";
document.getElementsByTagName('head')[0].appendChild(tag);

//Change 'iiqxcjxJ5Us' to video needed
var player, videoId = 'iiqxcjxJ5Us';
function onYouTubeIframeAPIReady() {
player = new YT.Player('player', {
height: '390',
width: '640',
videoId: videoId,
events: {
'onStateChange': onPlayerStateChange
}
});
}

function onPlayerStateChange(event) {
switch( event.data ) {
//Send video started event to Marketo 
case YT.PlayerState.PLAYING: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=started'
}
);
break;
//Send video finished event to Marketo
case YT.PlayerState.ENDED: Munchkin.munchkinFunction('visitWebPage', {
url: '/video/'+videoId
, params: 'video=finished'
}
);
break;
}

}
</script>
```

1. Maak een slimme lijst in Marketo met de URL van de video en de weergavegebeurtenis die u zoekt als de waarde van &quot;Querystring bevat&quot;. Voor meer informatie over YouTube Iframe API, [ gelieve te bezoeken YouTube API documentatie ](https://developers.google.com/youtube/iframe_api_reference). Voor meer informatie over Munchkin, [ herzie de de ontwikkelaarsdocumentatie van Marketo ](/help/javascript-api/lead-tracking.md).

Gepast op _2015-02-02_ door _Murta_

## Tips en trucs voor Marketo SOAP API

OPMERKING: Dit is een bericht op een gastblog. [ Ed Blachman is een Hogere Architect ](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D2777965) bij [ Software TIBCO, een bekende verkoper van ondernemingssoftware ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO). Ed werkt aan producten die toestaan wat Gartner &quot;burgerontwikkelaars&quot;noemt om de wolkendiensten te integreren die zij gebruiken zonder enige programmering zelf te hoeven doen. [ Marketo SOAP API ](/help/soap-api/soap-api.md) is een krachtig hulpmiddel waardoor de ontwikkelaars de macht van Marketo kunnen gebruiken en het met onze eigen toepassingen integreren. Tussen [ de formele documentatie ](./getting-started.md) en [ de communautaire middelen ](https://nation.marketo.com/), is er veel informatie beschikbaar betreffende hoe te om het te gebruiken. Toen ik aan de slag ging, leunde ik zwaar op die informatie en vond het van onschatbare waarde. Maar in dat proces heb ik wat tips en trucs opgebouwd die ik op geen van die plaatsen had gezien. Hier is wat van wat ik erachter kwam.

**Sandbox van de Ontwikkelaars** Sandbox is natuurlijk, een prachtig middel voor API ontwikkelaars: een veilige plaats waarin u met de eigenschappen van Marketo kunt experimenteren, toevoegend en verwijderend voorwerpen zonder zich het mengen in echte marketing activiteiten die door de daadwerkelijke gebruikers van Marketo van uw organisatie worden gedragen. De sandbox is echter geen wondermiddel.
Ik moest bijvoorbeeld onze Sandbox delen met een andere ontwikkelingsgroep, en dit deed wat, omdat ze gewend waren geraakt aan het idee dat ze de Sandbox bezaten. Uiteindelijk hebben we een aantal aanbevolen werkwijzen voor delen ontwikkeld: - Schrijf geen tests die afhankelijk zijn van volledige kennis van de inhoud van uw sandbox. Als gedeelde bron kunnen schema&#39;s zonder voorafgaande kennisgeving worden gewijzigd, evenals volledige vermeldingen in uw leads-database of -programma&#39;s of andere entiteiten. Als uw tests veronderstellen volledige kennis van de zandbak, leidt uw ontwikkelingscyclus tot stroomonderbrekingsperiodes voor de groepen met wie u het deelt. Aangezien hun ontwikkelingscyclus gewoonlijk niet samenvalt met die van u, komt dit neer op het vasthouden van de resource-not cool. Het is ook niet nodig, als je het erdoor denkt. - Gebruik wel een conventie om al uw dingen, uw leads, uw hoofdschemavelden, uw programma&#39;s, wat dan ook, te labelen. Als u elk uw eigen voorwerpen kunt identificeren, en als u met uw medehuurders kunt overeenkomen dat elk van u de voorwerpen van anderen alleen zal verlaten, zou u op een stevige basis voor het delen moeten zijn. Voor leads kunt u een aangepast veld maken en een conventie maken met dit aangepaste veld om deze leads te identificeren als de test leidt. Voor lijsten of programma&#39;s kunt u de namen van uw objecten beginnen met een tekenreeks die deze objecten identificeert als onderdeel van u. - Overweeg tests te schrijven die u opschoont nadat u de objecten hebt gemaakt waarin u geïnteresseerd bent, en deze vervolgens te openen, bij te werken of selectief te verwijderen en vervolgens definitief te verwijderen. (Merk op dat dit niet 100% in de SOAP API haalbaar is omdat niet alles in de Sandbox, of in een echte instantie voor dat dossier, via SOAP API kan worden beheerd. Toch is het nog steeds de moeite waard om dit zo veel mogelijk te doen.)

**Echte Instanties** het probleem met Sandbox is dat het niet in productie wordt gebruikt, zodat is het moeilijk om een idee van te krijgen hoe het echte gebruik als in een instantie van Marketo kijkt. Als je het geluk hebt dat je een Marketo-gebruiker in je team hebt, of als je een speciale ontwikkeling voor interne Marketo-gebruikers doet, is dat niet zo&#39;n probleem. Maar in het geval van mijn team was het inderdaad heel veel. Geen van ons was Marketo-experts. Omdat ons werd gevraagd om een groot aantal cloudservices te begrijpen, hadden we gewoon niet de titel om experts te worden. Hier zijn een aantal inzichten die we hebben geleerd van toegang tot een echte instantie: - Grote loodschema&#39;s. Het hoofdschema in de productieinstantie die we hebben geopend, heeft meer dan 200 velden. Dat maakte het glashelder aan onze ontwerpers UI dat UI zij ontwerpen schema&#39;s van die grootte (of groter) moest aanpassen. - Bursty-gebruik. We hebben twee grootteverschillen gezien tussen de hoogste en lage gebruikstijden (in termen van aantal gemaakte of bijgewerkte leads). Dit beïnvloedde zowel het volume van gegevens wij (duidelijk) terug van API vraag en de tijd het voor een (misschien minder voor de hand liggende) vraag van API zouden nemen om te antwoorden.

**de Tijd van de Reactie van de Vraag van API** Afhankelijk van de tijd van dag, de details van uw API vraag, en de inhoud van uw instantie, kunt u de de reactietijd van SOAP vinden API langer dan gemiddeld duurt. Soms hadden we API-oproepen die anderhalve minuut duurden om te reageren. U moet zich bewust zijn van de mogelijkheid om ermee om te gaan: - Test. Misschien is dit geen probleem voor uw gebruik. Maar ga er niet van uit, doe wat testen. - Tik op uw gebruik. In ons geval, was de grootste kwestie dat wij de paginagrootte voor onze vraag aan [ getMultipleLeads ](/help/soap-api/getmultipleleads.md) plaatsen om zo groot te zijn zoals API toestaat. In onze context is dat enigszins logisch, omdat ons doel is zo efficiënt mogelijk te zijn met de API-quota van onze klant. Maar in uw context hoeft u zich misschien niet zo intensief zorgen te maken over de quota&#39;s voor API-oproepen van uw gebruikers. In dat geval zult u zeker een betere reactietijd krijgen door te vragen naar kleinere pagina&#39;s met gegevens.

**Leiding het Partitioneren** Marketo verstrekt krachtige hulpmiddelen-verdelingen en werkruimten-die veelvoudige marketing groepen toestaan om één enkele instantie van Marketo te delen. Deze gereedschappen worden echter niet rechtstreeks weerspiegeld in de SOAP API. Bijvoorbeeld, wanneer u getMultipleLeads gebruikt om alle lood te krijgen die sinds wat datetime zijn bijgewerkt of gecreeerd, u terugkrijgt alle lood in uw instantie waarvoor dat het geval is, ongeacht (en met niets om te wijzen) welke verdeling of werkruimte om het even welke bepaalde lood bevat. Het creëren van lood en het toevoegen van lood aan lijsten zijn andere contexten waarin het leiden het verdelen kan beïnvloeden wat uw API vraag eigenlijk doet. Merk op dat dit betekent dat de verdelingen en de werkruimten niet de oplossing kunnen zijn u aan het probleem van het delen Sandbox hierboven bespreekt. Dus, hoe kom je erachter of dit een probleem voor jou is? Ik heb al deze dingen behulpzaam gevonden: De Ontwikkelaars Evangelisten zijn geëngageerd aan ons succes in het gebruiken van APIs, en waar er vragen zijn, zijn zij verbazend goed in het werken om antwoorden te vinden. - [ API Documentatie ](./getting-started.md). De Evangelisten hebben dit probleem al in een deel van de documentatie opgenomen, en als onderdeel van hun inzet voor ons succes, zijn ze echt goed in het bijwerken van het doc. - Uw eigen testcase. Hoewel het gebruiken van verdelingen en werkruimten voor het delen van de zandbak geen goed idee kan zijn, is Sandbox een grote plaats om met verdelingen en werkruimten te spelen om te weten te komen of zij uitdagingen voor uw voorgenomen gebruik stellen. (Het is ook een goede manier om uw vragen voor de Evangelisten te versmallen, die altijd een goed idee zijn.)

**TIMTOWTDI en het Testen** &quot;Er is meer dan één manier om het&quot;te doen - de Perl programmeringsmotto-eigenlijk is in bepaalde contexten op Marketo SOAP API van toepassing. Bijvoorbeeld, wilde ik het bijwerken van een reeks lood met het toevoegen van die lood aan één of andere lijst combineren. De SOAP API biedt u twee manieren om dit te doen: 1. [ importToList ](/help/soap-api/importtolist.md) + [ getImportToListStatus ](/help/soap-api/getimporttoliststatus.md). Dit is duidelijk de &quot;normale&quot; manier om de documentatie te lezen. Maar het feit dat je moet opiniepeilen voor de status van je importbewerking heeft me een gele vlag opgeleverd. Was dit echt de manier waarop ik mijn import wilde implementeren? 1. [ syncMultipleLeads ](/help/soap-api/syncmultipleleads.md) + [ listOperation ](/help/soap-api/listoperation.md). Dit lijkt veel minder elegant dan een unitaire importToList-aanroep, maar het is niet afhankelijk van opiniepeiling. Was het een haalbare optie? Gevallen als deze zijn moeilijk voor de Evangelisten om te behandelen, omdat zij echt afhankelijk zijn van de aard van de instanties die u behandelt en precies wat u probeert te doen. Gelukkig, als u opstelling een robuuste eenheid testende milieu hebt, zou u het moeten kunnen gebruiken om vragen als deze eveneens te onderzoeken. In dit specifieke geval bleek dat optie 2 beter was voor mijn gebruik dan optie 1 - niet vanwege de opiniepeiling, maar omdat ik de op het veld gerichte beperkingen van importToList had, en ook omdat ik probeerde code te schrijven die kon worden gebruikt in contexten en gevallen waarover ik geen controle had. Maar je manier van gebruik kan anders zijn en het testen is de enige manier waarop je het kunt achterhalen.

**Conclusie** ik denk geen van dit een reusachtig geheim is. Aan de andere kant had ik voor het spel gestaan als ik dit al had gekend voordat ik begon. Ik hoop dat u het nuttig vindt.

Gepost op _2015-02-05_ door _David_

## Marketo REST API met Boomi gebruiken: regels ophalen en verwijderen uit statische lijsten

In deel 1 van deze reeks, besprak ik hoe het mogelijk was te beginnen REST API door Boomi met de schakelaar van HTTP Boomi te gebruiken, specifiek het krijgen van het authentificatietoken nodig om tot REST API toegang te hebben, en het op te slaan in een Variabele van het Proces. Daarna omhoog, zullen wij beginnen het maken van vraag in Marketo, en in dit voorschot, toon ik u hoe u zowel [ Veelvoudige Leads door identiteitskaart van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists) kunt krijgen en [ Leads uit Lijst ](/help/rest-api/lead-database.md) verwijdert. Let met name op het verwijderen van leads van een lijst omdat er een zeer &quot;licht gedocumenteerd&quot; en subtiel aspect van Boomi op het werk is dat ik verder ga als we daar komen.

In de volgende tranche breidt we deze functionaliteit uit om interessante dingen te gaan doen zoals het krijgen van Loodactiviteit, maar dat is een blog voor een andere dag. Voor deze tranche bekijken we de tweede en derde gemarkeerde gebieden. Als overzicht heb ik de JSON-reacties opgenomen die we hieronder nodig hebben. Als u een JSON-profiel wilt maken in Boomi, hoeft u alleen maar een profielcomponent van het type JSON te maken. Klik vervolgens op Importeren en selecteer het bestand. Boomi doet de rest door dingen te extrapoleren als er meerdere id&#39;s zijn toegestaan. Voorbeeld JSON voor [ krijgt veelvoudige lood door identiteitskaart van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Static-Lists)

```json
{
  "requestId": "",
  "success": true,
  "nextPageToken": "",
  "result": [
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    },
    {
      "id": 0,
      "email": "",
      "firstName": "",
      "lastName": ""
    }
  ]
}
```

Voorbeeld JSON voor [ verwijdert Leads uit het Verzoek van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
   "input":[
      {
         "id": ""
      },
      {
         "id": ""
      }
   ]
}
```

Voorbeeld JSON voor [ verwijdert Leads uit de Reactie van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/removeLeadsFromListUsingDELETE)

```json
{
  "requestId": "",
  "success": true,
  "result": [
    {
      "id": 1,
      "status": ""
    },
    {
      "id": 2,
      "status": "",
      "reasons": [
        {
          "code": "",
          "message": ""
        }
      ]
    }
  ]
}
```

De Get Veelvoudige Leidingen door identiteitskaart van de Lijst ** Zet een andere schakelaar (krijgen) in uw proces, gebruikend de zelfde verbinding zoals die in het vorige artikel wordt bepaald. Maak een nieuwe bewerking met de naam &#39;Meerdere leads ophalen op lijst-id&#39; (Ik ben een stickler voor consistentie) De kenmerken zijn als volgt: - Verzoek om profiel: Geen (deze gebruikt de aanvraag-URL) - Type reactieprofiel: jSON - Reactieprofiel: maak een nieuw profiel op basis van de bovenstaande reactie Meerdere leads ophalen op lijst-id. U kunt deze wijzigen, zodat de gewenste velden worden geretourneerd, en niet alleen de vermelde velden. Het is belangrijk om te onthouden dat het JSON-responsprofiel echt overeenkomt met de lijst met velden die u vraagt vanuit de REST API, en dat u alleen de velden opvraagt die u nodig hebt. In het object Process Properties (Proceseigenschappen) hebben we een eigenschap met de naam &#39;fields&#39; gedefinieerd. Dit is een door komma&#39;s gescheiden lijst met de velden die u REST wilt retourneren. en dat is de lijst die met het profiel moet overeenkomen. Inhoudstype: text/plain (dit is alleen een URL-aanvraag) HTTP-methode: GET (u kijkt dit op in de REST API-documenten, het wordt altijd vermeld) Resource Path (add 5) rest/v1/list/ listID (replacement variable) /leads.json?access_token= access_token (replacement variable) &amp;fields= fields (replacement variable). Dan in het parameterlusje op de schakelaar, kunt u de veranderlijke waarden ingaan, die wij eerder in de proceseigenschappen bevolkten. In de volgende sectie zal ik vertellen hoe u kunt vermijden manueel bevolkt deze. Ik ga het deel van het proces overslaan waar ik de reactie voor krijgen Veelvoudige Lood door Identiteitskaart van de Lijst in een plat dossierprofiel toebreng en het op een server van FTP vastzetten omdat dat ongecompliceerde functionaliteit Boomi is.

Schrap Leidingen van een Lijst zodat dit interessant is, één van mijn medewerkers, [ Ken Niwa ](https://www.linkedin.com/uas/login?session_redirect=https%3A%2F%2Fwww.linkedin.com%2Fprofile%2Fview%3Fid%3D7429494) leerde me deze volgende techniek en het is behoorlijk cool en gebaseerd op een artikel van Boomi getiteld &quot;hoe te een POST- Verzoek voor een RESTful Toepassing bouwen,&quot;hieronder getoond.  ...maar eerst en vooral. In het proces, die uit &quot;krijgen Veelvoudige Lopen door Identiteitskaart van de Lijst&quot;komt hebben wij de Get Veelvoudige Leidingen door de Vorm van de Reactie van identiteitskaart van de Lijst, en wij moeten dat in kaart brengen &quot;verwijdert Leidingen uit het Verzoek van de Lijst&quot;dat afbeelding vrij eenvoudig is, enkel identiteitskaart in kaart brengen wij van de lood in de originele lijst in de identiteitskaart- lijst hebben wij in schrapping jSON overgaan. Daarna, laat vallen een andere Schakelaar met een actie van &quot;verzendt,&quot;gebruikend de zelfde Verbinding leidt tot een nieuwe verrichting genoemd &quot;verwijdert Leads uit het Verzoek van de Lijst&quot;. De waarvan attributen zijn het Profiel van het Verzoek: Het Type van Inhoud jSON: toepassing/JSON het Profiel van het Verzoek: [ JSON Profiel ] verwijdert Leads uit het Verzoek van de Lijst (gecreeerd uit het bovengenoemde dossier) Type van Reactie van het Profiel van de Reactie jSON: [ JSON Profiel ] verwijdert Leads uit de Reactie van de Lijst (gecreeerd uit het bovengenoemde dossier) Inhoudstype: application/json HTTP Methode Methode van HTTP: application/van HTTP: Toepassing: De Pad van het Pad van het Hulpmiddel van DELETE (voeg 4): De Pad 4) rest/v1/v1)/ listID (vervangingsvariabele) /leads.json?access_token= access_token (vervangingsvariabele) Hier is het interessante ding aan deze connector. Wij gaan NIET uitdrukkelijk de parameters in de schakelaartabel toevoegen. In plaats daarvan, zoals het artikel verklaart, creëren wij dynamische documenteigenschappen die de zelfde namen zoals de vervangingsvariabelen hebben. In dit geval, die variabelen listID en access_token. Wanneer u dit doet, loopt de jSON-vorm door in de REST-aanroep en verschijnen de parameters op de juiste plaats op de URL. We kunnen dit niet doen met de vorige oproep omdat het een GET is, geen POST. Op dit punt hebt u een GET en een POST REST API-aanroep gezien en kunt u het patroon voor deze REST-aanroepen zien. In de volgende tranche zullen wij beginnen te bekijken de uitvoer van de Activiteit van de Leiding door REST API, die een beetje meer betrokken is.

GEPast op _2015-02-06_ door _John_

## YouTube-video insluiten met regelafstand op een Marketo-bestemmingspagina

In een eerder blogbericht beschreef ik hoe u leads kunt segmenteren in Marketo op basis van of ze een specifieke YouTube-video hebben gestart of voltooid. In dit blogbericht bekijken we hoe we de implementatie van dat artikel kunnen gebruiken op een Marketo-landingspagina.

1. Navigeer naar het programma in Marketo waar u de nieuwe bestemmingspagina wilt maken. Klik op Nieuw lokaal element en vervolgens op Openingspagina.
1. Geef de landingspagina een naam. Wijs een pagina-URL toe. Selecteer een sjabloon. Klik vervolgens op Maken.
1. Klik op Concept bewerken nadat de openingspagina is gemaakt.
1. Sleep de HTML-knop vanuit het rechterdeelvenster naar het hoofdcanvas aan de linkerkant.
1. In het vak Custom HTML Editor dat verschijnt. Klik vervolgens op Opslaan.
1. Pas de grootte van een HTML-element aan door de omtrek van het vak te slepen. Klik vervolgens op Goedkeuren en Sluiten.
1. Test de live versie van de landingspagina door op Goedgekeurde pagina weergeven te klikken. Er wordt een openingspagina met de YouTube geopend in een nieuw venster. De video die is gestart en voltooid, wordt weergegeven in het activiteitenlogboek van de lead, zoals hieronder in de eerste en tweede schermafbeelding wordt getoond. Nadat de gegevens zich in Marketo bevinden, kunt u een slimme lijst en segmentleads maken die een video hebben gestart of voltooid, zoals getoond in de onderstaande schermafbeelding. Voor meer informatie over YouTube Iframe API, [ gelieve te bezoeken YouTube API documentatie ](https://developers.google.com/youtube/iframe_api_reference). Voor meer informatie over Munchkin, [ te herzien gelieve de de ontwikkelaarsdocumentatie van Marketo ](/help/javascript-api/lead-tracking.md).

Gepast op _2015-02-09_ door _Murta_

## Web Tracking van toepassing op één pagina met Munchkin

Een toepassing voor één pagina is een website die alle bronnen laadt die nodig zijn om op de eerste laadpagina door de site te navigeren. Wanneer een gebruiker op een koppeling klikt, wordt de inhoud geladen vanaf de eerste pagina waarop gegevens worden geladen. Voor de gebruiker gedraagt de website zich zoals verwacht, omdat de URL in de adresbalk vergelijkbaar is met de traditionele paginanavigatie. Munchkin werkt goed met traditionele websites omdat Munchkin elke keer wordt uitgevoerd wanneer de gebruiker een nieuwe pagina laadt. Als u echter geen nieuwe pagina laadt, wordt Munchkin slechts één keer uitgevoerd met een toepassing die uit één pagina bestaat. De aanpak die ik doorloop in dit bericht is om te volgen wanneer een gebruiker op een koppeling klikt en deze informatie vervolgens naar Munchkin te sturen. We implementeren dit met de functie `clickLink` Munchkin. Hieronder ziet u een voorbeeldimplementatie in jQuery die voor klikgebeurtenissen aan de methode van `clickLink` Munchkin bindt. Wanneer u de Munchkin-methode `clickLink` aanroept, geeft deze de parameter door voor de URL waarop is geklikt.

```javascript
<script>
$("a").on('click', function(event) {
    var urlThatWasClicked = $(this).attr('href');
    Munchkin.munchkinFunction('clickLink', { href: urlThatWasClicked});
});
</script>
```

Gepast op _2015-02-11_ door _Murta_

## De score van een lead wijzigen via de REST API

Laten we zeggen dat u de score van een lead in Marketo wilt wijzigen met de API&#39;s. Dit is mogelijk om met REST API te doen gebruikend het Create/Update eindpunt van de Lood. Hieronder volgt een codevoorbeeld in Ruby dat toont hoe te om deze vraag te maken.

```ruby
require 'rest_client'
require 'json'

# Build request URL
# Replace AAA-BBB-CCC with your Marketo instance
marketo_instance = "https://AAA-BBB-CCC.mktorest.com" 
endpoint = "/rest/v1/leads.json"
# Replace with your access token
auth_token =  "?access_token=" + "ac756f7a-d54d-41ac-8c3c-f2d2a39ee325:ab" 
request_url = marketo_instance + endpoint + auth_token

# Build request body
data = { "action" => "updateOnly", "input" => [ { "email" => "<example@email.com>", "leadScore" => "30" } ] } 

# Make request
response = RestClient.post request_url, data.to_json, :content_type => :json, :accept => :json

# Returns Marketo API response
puts response
```

In de JSON-hoofdtekst van het verzoek geven we `updateOnly` op als de handeling. Dit betekent dat het verzoek alleen werkt als de leiding bestaat, anders mislukt het. Als u een lead wilt maken als deze niet bestaat, geeft u `createOrUpdate` op als de actie. We gebruiken de e-mail van de lead als primaire id om de lead record in Marketo te zoeken. Tot slot specificeren wij de waarde voor de score van de lood gebruikend de sleutel `leadScore`. Met deze methode kunnen 300 leads tegelijk worden bijgewerkt.

Gepast op _2015-02-19_ door _Murta_

## Open Source-projecten markeren die zijn gebaseerd op het Marketo-platform: Deel drie

Dit is de derde post in een lopende reeks waarin openbronprojecten worden belicht die door de ontwikkelaarsgemeenschap rond het Marketo-platform worden gebouwd. Wij handhaven [ een lijst op de rekening van GitHub van Marketo ](https://github.com/Marketo/Community-Supported-Client-Libraries) waar wij cliëntbibliotheken en projecten volgen die door de ontwikkelaarsgemeenschap van Marketo worden gecreeerd. Hieronder staan drie projecten die zijn ontwikkeld rond de Marketo REST API&#39;s. **[Gebruikergeest ](http://www.usermind.com/) creeerde [ a Node.js cliëntbibliotheek voor Marketo REST API ](https://github.com/MadKudu/node-marketo).** **[de Beend Samat ](https://github.com/asamat) creeerde [ een cliëntbibliotheek in Python voor Marketo REST API ](https://github.com/asamat/python_marketo).** **[Jacques Lemieux van Marketo ](https://www.linkedin.com/in/jalemieux) creeerde [ een cliëntbibliotheek in Ruby voor Marketo REST API.](https://github.com/jalemieux/mkto_rest)** We zijn blij dat er meer projecten zijn gemaakt door de ontwikkelaarscommunity op het Marketo-platform. Als u aan een open bronproject voor het platform van Marketo werkt, gelieve [ het aan deze repo GitHub via een trekkingsverzoek ](https://github.com/Marketo/Community-Supported-Client-Libraries) voor te leggen.

Gepast op _2015-02-20_ door _Murta_

## Een Marketo-formulier invoegen in een RTP-campagne

Veel marketers zijn geïnteresseerd in het plaatsen van een Marketo-formulier in een Marketo Real-Time Personalization (RTP)-campagne. Of het nu een Dialog, In Zone of Widget RTP campagne type is, u kunt uw Form HTML code kopiëren en deze in de campagneeditor van RTP plakken. Ik heb deze voorbeelden gezien die worden gebruikt: - Bezoekers krijgen om zich aan te melden bij uw nieuwsbrief na een tweede of derde klik op uw site - Snel, effectief aanmeldingsformulier voor webinars - Een gedateerde casestudy downloaden - Aanbiedingen die in het verleden zijn afgebroken om een formulier opnieuw in te schrijven Vul een formulier in de campagne en ontvang de gewenste dankbetuiging of inhoud, zodat u minder klikt om uw doelen te bereiken. Hier is een uitleg van hoe je dit doet en een Marketo Form 2.0 inbedt in een Marketo RTP campagne. Zie een groot voorbeeld hieronder van eMarketer, gebruikers RTP die het één stap verder namen en in plaats van het leiden van de bezoekers aan een dank u pagina - besloten om een Dank u bericht binnen de campagne te tonen RTP. De code voor deze optie is ook hieronder. Veel plezier en ik hoor graag over uw ervaring met dit onderwerp!

1. Klik met de rechtermuisknop op een goedgekeurd formulier. Selecteer **bed Code in.**
1. Kopieer de **Code.**
1. In Marketo RTP, ga naar **Campagnes**.
1. Klik **CREËREN NIEUWE CAMPAGNE**.
1. In de Rijke Redacteur van de Tekst, klik op het **pictogram van HTML**.
1. Plak de insluitcode van het formulier in de HTML Source Editor. Klik **Update**.
1. Het formulier wordt niet weergegeven in de editorweergave, maar u kunt het voorbeeld bekijken om te zien hoe het in een campagne wordt weergegeven.
1. Klik **Lancering** om de campagne te beginnen.

### Opmerking

Wijzigingen in het formulier moeten worden aangebracht in de marketingactiviteiten van Marketo in het concept van het formulier bewerken.

### Verwante artikelen

* [Forms 2.0](/help/javascript-api/forms-api-reference.md)

Gepost op _2015-12-20_ door _Yanir_

## Een knop Herstellen toevoegen aan een Marketo-formulier

```javascript
<script src="//app-sj01.marketo.com/js/forms2/js/forms2.min.js"></script>
<form id="mktoForm_116"></form>
<script>MktoForms2.loadForm("//app-sj01.marketo.com", "410-XOR-673", 116,
function(form) { form.getFormElem()[0].querySelector('button[type="submit"]').insertAdjacentHTML('afterend','<button type="reset" class="mktoButton">Reset</button>') });
</script>
```

Gepast op _2015-03-18_ door _Murta_

## Updates release maart 2015

[ Marketo REST Asset API werd vrijgegeven in de versie van Maart 2015 ](https://developer.adobe.com/marketo-apis/api/asset/). Met deze API hebt u toegang tot Marketo-sjabloonobjecten voor bestanden, mappen, token, e-mail en e-mail. Let op: er zijn twee rolmachtigingen toegevoegd om toegang te verlenen tot de Asset API-eindpunten: Read-Only Assets, Read-Write Assets. Als uw API-gebruikersrol dateert van vóór de release van de API&#39;s voor middelen, moet u een nieuwe API-gebruikersrol met deze machtigingen maken om toegang mogelijk te maken. Anders, ontvangt u een 603 &quot;Afgewezen Toegang&quot;foutenreactie. Naast de release van de REST Asset API waren er updates van bestaande REST API-eindpunten. Het [ EIND API van de Leiding van de Fusie eindpunt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) werd bijgewerkt om voor het samenvoegen van veelvoudige lood toe te staan. Het [ Eind van de Campagne van het Programma REST API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) werd bijgewerkt om voor het klonen van een campagne toe te staan terwijl het plannen van een campagne.

Gepast op _2015-03-23_ door _Murta_

## Het teweegbrengen van Campagnes RTP met een Vertraging

Met deze aangepaste JavaScript kunnen RTP-gebruikers campagnes weergeven enkele seconden nadat de webpagina is geladen. Dit wordt aanbevolen voor dialoogvenster- en widgetcampagnes. Deze kan worden gebruikt om een campagne na een vertraging weer te geven, zodra de bezoeker de normale inhoud op de pagina heeft bekeken. Het wordt aanbevolen deze code alleen te implementeren op specifieke pagina&#39;s waarop de campagne(s) moeten worden weergegeven. Het gebruik van deze code op alle pagina&#39;s wordt afgeraden, omdat dit van invloed kan zijn op de prestaties. **Instructies van de Opstelling** De douanecode verzendt een Gebeurtenis van de Gegevens van de Douane RTP (t=timeOnPage, d.w.z.: t=60) en laadt dan de campagne die deze gebeurtenis aanpast. Standaard wordt deze na 60 seconden geactiveerd. U kunt het aanpassen door de sendCustomRTPEvent parameter in een ander aantal te veranderen. Plaats de code onmiddellijk na de standaardRTP code:

```javascript
<script>
function sendCustomRTPEvent(a){
 var eventValue="t="+a;
 setTimeout(function(){ 
  rtp('send', 'event', {value: eventValue}); 
  rtp('get', 'campaign',true);
 }, 1000 \* a);
}
sendCustomRTPEvent(60); //Seconds
</script>
```

Om een RTP campagne op te zetten om na een tijdvertraging te reageren: 1. Meld u aan bij uw RTP-account 1. Maak een nieuw segment 1. Voeg in de sectie Segmentgebeurtenissen het volgende toe: `t=60` . Een bezoeker kan elk segment RTP slechts één keer per zitting aanpassen, daarom die elke campagne slechts één keer zien (tenzij het aan kleverig wordt geplaatst).

GEPast op _2015-03-24_ door _Yanir_

## Updates van de release van april 2015

### Marketo Mobile Engagement SDK v0.3.2

Marketo omvat nu marketingautomatisering en betrokkenheid van gebruikers voor mobiele apps. Het installeren van [ Marketo Mobile SDK ](/help/mobile/mobile.md) in uw iOS of Android app staat Marketers toe om naar in toepassingsgebeurtenissen te luisteren en relevante dupberichten te verzenden.

### Verbeteringen voor REST API

* Aangepaste objecten

De nieuwe [ eindpunten van het douanevoorwerp ](/help/rest-api/custom-objects.md) zijn geïntroduceerd die u toestaan programmatically van een lijst te maken, de gegevens te beschrijven en te CRUD die met een douanevoorwerp van Marketo verblijven.

Rolmachtigingen zijn toegevoegd om toegang te verlenen tot de API-eindpunten voor aangepaste objecten: Alleen-lezen aangepast object, Aangepast object lezen-schrijven. Als uw API-gebruikersrol voorafgaat aan de release van de API&#39;s voor aangepaste objecten, moet u een nieuwe API-gebruikersrol met deze machtigingen maken om toegang mogelijk te maken. Anders, ontvangt u een 603 &quot;Afgewezen Toegang&quot;foutenreactie.

* Campagne plannen - Kloonprogramma

Een nieuwe facultatieve parameter &quot;cloneToProgramName&quot;werd geïntroduceerd in de [ campagne API van de planningscampagne ](/help/rest-api/data-ingestion.md). Als deze parameter aanwezig is, wordt het bovenliggende programma van de campagne gekloond en wordt de nieuwe campagne gepland. De parameter geeft de gewenste naam voor het resulterende programma op.

Gepost op _2015-04-28_ door _Travis Kaufman_

## E-mailabonnementen synchroniseren over verschillende instanties

Beheert u meerdere exemplaren van Marketo? Het kan lastig zijn om de informatie over leads gesynchroniseerd te houden in verschillende instanties. Hier volgt een manier om e-mailabonnementen te synchroniseren over instanties die een webhaak gebruiken die een externe webservice aanroept. De externe webservice doorloopt elke instantie die op zoek is naar de bekende lead die de unsubscribe-gebeurtenis heeft geactiveerd. Wanneer een overeenkomende lead wordt gevonden, wordt het veld &quot;niet-geabonneerd&quot; in de bijbehorende lead record bijgewerkt. Hier is een diagram dat het idee illustreert.  Het is aan u om de Webdienst uit te voeren, maar de code hieronder zou u moeten helpen het proces beginnen!

### Externe webservice

De externe webservice voert de volgende stappen uit voor elke Marketo-instantie die moet worden gesynchroniseerd:

1. Stelt instantie-specifieke REST API [ Eindpunt URL ](/help/rest-api/endpoint-reference.md) samen
1. Verkrijgt toegangstoken gebruikend [ Identiteit ](/help/rest-api/authentication.md)
1. Verkrijgt een lijst van loodverslagen die e-mailadres aanpassen gebruikend [ krijgt Veelvoudige Leidingen door het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET)
1. Hiermee wordt het veld &#39;unsubscript&#39; van elke lead-record bijgewerkt met Leads maken/bijwerken

Hier volgt nog een diagram waarin de externe webserviceaanroep en Marketo REST API-aanroepen in detail worden weergegeven.  De voorbeeldcode hieronder is geen out of the box-webservice. In plaats daarvan, is het een programma van de consolewijze dat u argumenten tot via de bevellijn kunt overgaan. De bedoeling is hier om te tonen hoe u de juiste Marketo API&#39;s kunt aanroepen om lead records bij te werken over verschillende instanties. De implementatie van de webservice blijft een oefening voor de lezer.
**Code van de Steekproef** om de steekproefcode in werking te stellen en te krijgen, moet u een project van Java in uw favoriete winde tot stand brengen. Daarna moet u de volgende wijzigingen aanbrengen: 1. De steekproefcode gebruikt [ json-eenvoudige ](https://code.google.com/archive/p/json-simple) om koorden te ontleden JSON. Voeg de json-eenvoudige pot toe aan uw Java-project. 1. De voorbeeldcode heeft een structuur die metagegevens bevat voor elke Marketo-instantie. Plaats de werkelijke waarden van de instanties als volgt in de structuur:

```java
public static String instanceInfo[][] = {
{ "AccountId1", "ClientId1","ClientSecret1" },    // Instance 1 metadata
{ "AccountId2", "ClientId2","ClientSecret2" },    // Instance 2 metadata
{ "AccountId3", "ClientId3","ClientSecret3" }     // Instance 3 metadata
};
```

U vindt de metagegevens voor de instantie in het deelvenster Marketo Admin:

* Account ID Admin > Integration > Munchkin > Munchkin Account
* Client ID &amp; Client Secret Admin > Integration > LaunchPoint > Email Unsubscribe Sync > View Details

De voorbeeldcode neemt twee opdrachtregelargumenten die de queryparameters &quot;id&quot; en &quot;email&quot; voor de hierboven beschreven externe webservice simuleren.

1. args [ 0 ] = Identiteitskaart van de Rekening
1. args [ 1 ] = E-mailadres

Geef feitelijke waarden van uw instantie als argumenten door aan het programma. Hier is een het schermschot van de projectconfiguratie van Intellij IDEA.

**SyncEmailUnsubscribe.java**

```java
package com.marketo;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Iterator;
import java.util.NoSuchElementException;
import java.util.Scanner;

public class SyncEmailUnsubscribe {
    // Define Marketo instance meta data here.  
    // Each row contains three elements: Account Id, Client Id, Client Secret.
    // For example:
    //  public static String instanceData[][] = {
    //    {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    //    {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    //  };
    //
    public static String instanceData[][] = {
            // ADD YOUR INSTANCE META DATA HERE
    };

    public static void main(String[] args) {
        String accountId = args[0];     // Account id that processed the unsubscribe
        String emailAddress = args[1];  // Email address of lead that unsubscribed

        SyncEmailUnsubscribe seu = new SyncEmailUnsubscribe();

        // Loop through each Marketo instance
        for (int i = 0; i < instanceData.length; i++) {

            // Make sure we skip instance that triggered the webhook
            if (!accountId.equals(instanceData[i][0])) {
                String endpointUrl = String.format("https://%s.mktorest.com", instanceData[i][0]);

                // Generate access token
                String identityUrl = String.format("%s/identity/oauth/token?grant_type=client_credentials&client_id=%s&client_secret=%s", endpointUrl, instanceData[i][1], instanceData[i][2]);
                String token = seu.getToken(identityUrl);

                // Get lead records for given email address (may be duplicates)
                String getLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=email&filterValues=%s", endpointUrl, token, emailAddress);
                String leads = seu.getLeads(getLeadsUrl);

                // Update unsubscribed field in lead record
                String updateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", endpointUrl, token);
                seu.updateLeads(updateLeadsUrl, leads, accountId);
            }
        }

        System.exit(0);
    }

    // Call Identity Service to generate access token
    public String getToken(String url) {
        // Call Identity Service
        String tokenData = getData(url);

        // Convert response into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(tokenData);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }

        // Retrieve access_token
        JSONObject jsonObject = (JSONObject)obj;
        return jsonObject.get("access_token").toString();
    }

    // Call Get Multiple Leads by Filter Type Service to get lead records
    public String getLeads(String url) {
        return getData(url);
    }

    // Call Create/Update Lead Service to update "unsubscribed" flag in lead record
    public void updateLeads(String url, String leads, String account) {
        JSONObject body = composeBody(leads, account);
        if (body != null) {
            postData(url, body);
        }
    }

    // Compose JSON body for Create/Update Leads Service
    private JSONObject composeBody(String leads, String account) {
        JSONObject body = new JSONObject();

        // Convert leads into JSONObject
        JSONParser parser = new JSONParser();
        Object obj = null;
        try {
            obj = parser.parse(leads);
        } catch (ParseException pe) {
            System.out.println("position: " + pe.getPosition());
            System.out.println(pe);
        }
        JSONObject leadsObj = (JSONObject)obj;

        Object success = leadsObj.get("success");
        if (success.equals(true)) {
            body.put("action", "updateOnly");
            body.put("lookupField", "id");
            body.put("asyncProcessing", "true");

            // Build array of lead objects
            JSONArray input = new JSONArray();
            JSONArray result = (JSONArray) leadsObj.get("result");
            Iterator<JSONObject> iterator = result.iterator();
            while (iterator.hasNext()) {
                JSONObject leadIn = (JSONObject)iterator.next();
                JSONObject lead = new JSONObject();
                lead.put("id", leadIn.get("id"));
                lead.put("unsubscribed", "true");
                lead.put("unsubscribedReason", "Cross instance synch triggered by webhook from: " + account);
                input.add(lead);
            }

            body.put("input", input);
        }
        return body;
    }

    // HTTP POST request
    private String postData(String endpoint, JSONObject body) {
        String data = "";
        try {
            // Make request
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("POST");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "application/json");
            urlConn.connect();
            OutputStream os = urlConn.getOutputStream();
            os.write(body.toJSONString().getBytes());
            os.close();
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    // HTTP GET request
    private String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                System.out.println("Status: 200");
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
                System.out.println(data);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

### Marketo Setup

Voer de volgende stappen uit voor elke Marketo-instantie die u wilt synchroniseren.

1. Creeer de Dienst van de Douane met roltoestemming: Lead lezen-schrijven. Als u niet vertrouwd met het creëren van de Dienst van de Douane bent, klik [ hier ](/help/rest-api/custom-services.md).
1. Creeer een Webhaak die uw externe Webdienst roept. Als u niet vertrouwd met het creëren van Webhooks bent, klik [ hier ](/help/webhooks/webhooks.md).
1. Voeg WebHaak als stroomstap in Slimme Campagne toe.

De schermafbeelding hieronder laat zien hoe u een webhaak kunt maken om de hierboven opgegeven service aan te roepen met behulp van tokens om de queryparameters automatisch te vullen. Nu we onze website hebben gemaakt, kunnen we deze toevoegen aan een slimme campagne als een flowactie.  De Smart List moet de trigger &#39;Unsubsubscribes from Email&#39; bevatten.

### Validatie

Als u dit alles wilt testen, maakt u in verschillende Marketo-gevallen een lead met hetzelfde e-mailadres. Zorg ervoor dat u het e-mailadres hebt! In één geval activeert u een verzendactie voor e-mailberichten, opent u het resulterende e-mailbericht en klikt u op Abonnement opzeggen. Als u resultaten wilt valideren, meldt u zich aan bij elk van de andere instanties en inspecteert u de hoofdrecords die aan het e-mailadres zijn gekoppeld. Het selectievakje &#39;Niet-geabonneerd&#39; moet worden ingeschakeld en het veld &#39;Reden zonder abonnement&#39; moet een opmerking bevatten met de id van de bronaccount waarmee de synchronisatie is gestart.

Gepost op _2015-05-11_ door _David_

## Wijzigingen van gegevens op lood synchroniseren met REST API

In dat bericht werd een codevoorbeeld weergegeven dat op terugkerende basis kan worden uitgevoerd om Marketo te vragen of er updates beschikbaar zijn. Het idee was om Marketo APIs te gebruiken om veranderingen in loodgegevens te identificeren, en de loodgegevens te halen die waren veranderd. Deze gegevens kunnen vervolgens voor synchronisatiedoeleinden naar een extern systeem worden gestuurd. Het codevoorbeeld dat wordt weergegeven, heeft onze SOAP API gebruikt. Goed hebben wij a [ nieuwe manier te lopen ](https://www.youtube.com/watch?v=G-7ZJjLy5D8&amp;feature=youtu.be), en die manier gebruikt [ REST API van Marketo ](/help/rest-api/rest-api.md). Deze post toont u hoe te om dat zelfde doel te verwezenlijken gebruikend twee REST eindpunten: [ krijgt de Veranderingen van het Lood ](/help/rest-api/rest-api.md), [ krijgen Lood door Identiteitskaart ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET). Het programma bevat twee hoofdstappen:

1. De vraag krijgt de Veranderingen van de Lood om een lijst van alle Lood Ids te produceren die of specifieke veranderde loodgebieden hadden, of tijdens een bepaalde tijdspanne werden toegevoegd.
1. De vraag krijgt identiteitskaart van de Lood voor elke Lood ID in de lijst om gebiedsgegevens van het loodverslag terug te winnen.

We nemen de opgehaalde gegevens in stap 2 en formatteren deze voor gebruik door een extern systeem.  **Input van het Programma** door gebrek gaat het programma &quot;terug&quot;één dag van de huidige datum om veranderingen te zoeken. Je zou dit programma bijvoorbeeld elke dag op hetzelfde tijdstip kunnen uitvoeren. Om verder terug in tijd te gaan, kunt u het aantal dagen als argument van de bevellijn specificeren, effectief verhogend het tijdvenster. Het programma bevat verscheidene variabelen die u kunt wijzigen: CUSTOM_SERVICE_DATA - dit bevat uw gegevens van de Dienst van de Douane van Marketo [&#128279;](/help/rest-api/custom-services.md) (Identiteitskaart van de Rekening, Identiteitskaart van de Cliënt, Geheime cliënt).  LEAD_CHANGE_FIELD_FILTER - Dit bevat een door komma&#39;s gescheiden lijst met loodvelden die we controleren op wijzigingen. READ_BATCH_SIZE - Dit is het aantal verslagen dat tegelijkertijd moet terugwinnen. Gebruik dit om de reactie op lichaamsomvang aan te passen. **Output van het Programma** het programma verzamelt omhoog alle veranderde loodverslagen en formatteert hen in JSON als serie van loodvoorwerpen als volgt:

```json
{
    "result": [
        {
            "leadId": "318592",
            "updatedAt": "2015-07-22T19:19:07Z",
            "firstName": "David",
            "lastName": "Everly",
            "email": "<deverly@marketo.com>"
        },

        ...more lead objects here...
    ]
}
```

Het idee is dat u dit JSON dan als verzoeklading aan een externe Webdienst kon overgaan om de gegevens te synchroniseren. {de Logische 1} Logica van het 0&rbrace; Programma &lbrace;vestigen wij eerst ons tijdvenster, stellen ons eindpuntURL van REST samen, en verkrijgen ons toegangstoken van de Authentificatie. **&#x200B;**&#x200B;Vervolgens zetten we een &#39;Get Paging Token/Get Lead Changes&#39;-lus in werking, die loopt tot we de doorvoertoevoer van de loodwijzigingen uitputten. Het doel van deze lus is een lijst met unieke lead-id&#39;s te verzamelen, zodat we deze later in het programma kunnen doorgeven aan &#39;lead by id&#39;. In dit voorbeeld geven we Lead-wijzigingen op om te zoeken naar wijzigingen in de volgende velden: firstName, lastName, email. U kunt een willekeurige combinatie van velden selecteren voor uw doeleinden. Hiermee worden Lead-wijzigingen geretourneerd naar &#39;result&#39;-objecten die een Activity Type ID bevatten, die we kunnen gebruiken om resultaten te filteren. Nota: U kunt een lijst van de Types van Activiteit krijgen door [ te roepen krijgt de Types van Activiteit ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getAllActivityTypesUsingGET) REST eindpunt. We zijn geïnteresseerd in twee soorten activiteiten die worden geretourneerd: 1. Nieuwe lead (12)

```json
{
    "id": 12024682,
    "leadId": 318581,
    "activityDate": "2015-03-17T00:18:41Z",
    "activityTypeId": 12,
    "primaryAttributeValueId": 318581,
    "primaryAttributeValue": "David Everly",
    "attributes": [
        {
            "name": "Created Date",
            "value": "2015-03-16"
        },
        {
            "name": "Source Type",
            "value": "New lead"
        }
    ]
}
```

1. Gegevenswaarde wijzigen (13) We kunnen zien welk veld is gewijzigd door de eigenschap &quot;name&quot; te inspecteren in de reactie Gegevenswaarde wijzigen.

```json
{
    "id": 12024689,
    "leadId": 318581,
    "activityDate": "2015-03-17T22:58:18Z",
    "activityTypeId": 13,
    "fields": [
        {
            "id": 31,
            "name": "lastName",
            "newValue": "Evely",
            "oldValue": "Everly"
        }
    ],
    "attributes": [
        {
            "name": "Source",
            "value": "Web form fillout"
        }
    ]
}
```

Wanneer één van beide soorten activiteiten zijn teruggekeerd, slaan wij bijbehorende Lood Id in een lijst op. Zodra wij onze lijst hebben, kunnen wij het doorlopen roepend Lood door Id voor elk punt. Hiermee worden de meest recente gegevens over leads voor elke lead in de lijst opgehaald. In dit voorbeeld worden de volgende hoofdvelden opgehaald: `leadId` , `updatedAt` , `firstName` , `lastName` en `email` . U kunt een willekeurige combinatie van velden selecteren voor uw doeleinden. Dit wordt gedaan door de gebiedsparameter te specificeren om Lood door Id te krijgen. En ten slotte JSONify de resultaten als een array van hoofdobjecten zoals hierboven beschreven.

**Code van het Programma**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadChanges {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            {INSERT YOUR CUSTOM SERVICE DATA HERE};

    // Lead fields that we are interested in
    private static final String LEAD_CHANGE_FIELD_FILTER = "firstName,lastName,email";

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Activity type ids that we are interested in
    private static final int ACTIVITY_TYPE_ID_NEW_LEAD = 12;
    private static final int ACTIVITY_TYPE_ID_CHANGE_DATA_VALE = 13;

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String leadChangesUrl = String.format("%s/rest/v1/activities/leadchanges.json?access_token=%s&fields=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_CHANGE_FIELD_FILTER, READ_BATCH_SIZE);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        HashSet leadIdList = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {

            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;

            do {
                moreResult = false;

                // Call Get Lead Changes API
                JsonObject leadChangesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadChangesUrl, nextPageToken)));
                if (leadChangesObj.get("success").asBoolean()) {
                    moreResult = leadChangesObj.get("moreResult").asBoolean();
                    nextPageToken = leadChangesObj.get("nextPageToken").asString();

                    if (leadChangesObj.get("result") != null) {
                        JsonArray resultAry = leadChangesObj.get("result").asArray();
                        for (JsonValue resultObj : resultAry) {
                            int activityTypeId = resultObj.asObject().get("activityTypeId").asInt();


                            // Store lead ids for later use
                            boolean storeThisId = false;
                            if (activityTypeId == ACTIVITY_TYPE_ID_NEW_LEAD) {
                                storeThisId = true;
                            } else if (activityTypeId == ACTIVITY_TYPE_ID_CHANGE_DATA_VALE) {
                                // See if any of the changed fields are of interest to us
                                JsonArray fieldsAry = resultObj.asObject().get("fields").asArray();
                                for (JsonValue fieldsObj : fieldsAry) {
                                    String name = fieldsObj.asObject().get("name").asString();
                                    if (LEAD_CHANGE_FIELD_FILTER.contains(name)) {
                                        storeThisId = true;
                                    }
                                }

                            }

                            if (storeThisId) {
                                leadIdList.add(resultObj.asObject().get("leadId").toString());
                            }
                        }
                    }
                }

            } while (moreResult);
        }

        JsonObject result = new JsonObject();
        JsonArray leads = new JsonArray();

        for (Object o : leadIdList) {
            String leadId = o.toString();

            // Compose Get Lead by Id URL
            String getLeadUrl = String.format("%s/rest/v1/lead/%s.json?access_token=%s",
                    baseUrl, leadId, accessToken);

            // Call Get Lead by Id API
            JsonObject leadObj = JsonObject.readFrom(getData(getLeadUrl));
            if (leadObj.get("success").asBoolean()) {
                if (leadObj.get("result") != null) {
                    JsonArray resultAry = leadObj.get("result").asArray();
                    for (JsonValue resultObj : resultAry) {

                        // Create lead object
                        JsonObject lead = new JsonObject();
                        lead.add("leadId", leadId);
                        lead.add("updatedAt", resultObj.asObject().get("updatedAt").asString());
                        lead.add("firstName", resultObj.asObject().get("firstName").asString());
                        lead.add("lastName", resultObj.asObject().get("lastName").asString());
                        lead.add("email", resultObj.asObject().get("email").asString());

                        // Add lead object to leads array
                        leads.add(lead);
                    }
                }
            }
        }

        // Add leads array to result object
        result.add("result", leads);

        // Print out result object
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Dus daar heb je het, [ leven is een gelukkig lied ](https://www.youtube.com/watch?v=zFaBwZDywLk). Veel plezier!

Gepost op _2015-07-31_ door _David_

## Updates van de release van mei 2015

### REST API

* Opportunity-API. De nieuwe [ opportuniteeindpunten ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Opportunities) zijn geïntroduceerd die u toestaan programmatically van de gegevens een lijst te maken, te beschrijven en te CRUD die binnen een de opportuniteitsvoorwerp van Marketo verblijven.

Nota: De toestemmingen van de rol werden toegevoegd om toegang tot de eindpunten van de Kans te verlenen: Read-Only Kans, Read-Write Kans. Als uw API-gebruikersrol dateert van vóór de release van de Opportunity API&#39;s, moet u een nieuwe API-gebruikersrol met deze machtigingen maken om toegang mogelijk te maken. Anders, ontvangt u een 603 &quot;Afgewezen Toegang&quot;foutenreactie.

* Asset API - Fragmenten. De nieuwe [ activa eindpunten voor fragmenten ](https://developer.adobe.com/marketo-apis/api/asset/#snippet_endpoints) zijn geïntroduceerd om u toe te staan om fragmentvoorwerpen programmatically te manipuleren. Fragmenten kunnen worden gebruikt als dynamische inhoudsblokken in e-mails en bestemmingspagina&#39;s.
* Leads-API - Leads-partitie bijwerken. Een nieuw [ lood eindpunt voor verdelingen ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updatePartitionsUsingPOST) is toegevoegd om u toe te staan om de verdeling voor één of meerdere lood bij te werken.
* Er ontbrak een tijdzoneverschuiving in de kenmerken &quot;createdAt&quot; en &quot;updatedAt&quot; aan API&#39;s die betrekking hadden op leads. Dit probleem is nu opgelost.
* Het probleem waarbij de Campagne van het Programma niet de juiste foutencode terugbracht wanneer het dagelijkse maximum aantal vraag was overschreden.
* Oplossing voor een probleem waarbij Map ophalen op id soms null zou retourneren voor kenmerken &quot;parent&quot; en &quot;description&quot;.
* Goedkeuren van e-mail via id gaf in bepaalde gevallen een systeemfout. Dit probleem is nu opgelost.
* Token maken op basis van map-id veroorzaakte een token dat in bepaalde gevallen onbruikbaar was. Dit probleem is nu opgelost.

### Real-Time Personalization (RTP)

* API voor veeleisende media De nieuwe [ rijke media aanbeveling ](/help/javascript-api/web-personalization.md) mogelijkheden zijn toegevoegd aan RTP JavaScript API. De Rich Media Content Recommendation verbindt uw webbezoekers met de meest relevante inhoud aangedreven door machinaal leren en voorspellende analyses. Verbeter uw inhoudselementen met tekstbeschrijvingen en afbeeldingen en sluit meerdere aanbevelingen voor inhoud in op uw website.

### SDK voor mobiele betrokkenheid

iOS v0.3.4/Android v0.3.3

* Aangepaste handelingen. De mogelijkheid toegevoegd om gebruikersinteractie te volgen door aangepaste handelingen te verzenden. Voor details, zie &quot;Verzendende Acties van de Douane&quot;[ hier ](/help/mobile/mobile.md).
* De trackPushNotification-methode is afgekeurd.

Gepost op _2015-05-26_ door _David_

## Updates van de release van juni 2015

### REST API

* Bedrijfs-API

De nieuwe [ bedrijfeindpunten ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Companies) zijn geïntroduceerd die u toestaan programmatically van de gegevens een lijst te maken, te beschrijven en te CRUD die binnen een het bedrijfvoorwerp van Marketo verblijven.

Nota: De toestemmingen van de rol werden toegevoegd om toegang tot de eindpunten van het Programma te verlenen: Read-Only Bedrijf, Read-Write Bedrijf. Als uw API gebruikersrol de versie van het Bedrijf APIs voorafgaat, dan moet u uw API gebruikersrol met deze toestemmingen bijwerken om toegang toe te laten. Anders ontvangt u een foutreactie van 603 &quot;Toegang geweigerd&quot;

* API voor middelen - Programma&#39;s

De bijgewerkte API voor middelen biedt ondersteuning voor zowel toepassingsmappen als programmamap. Verschillende API&#39;s van Asset hadden een map-id in de aanvraag geaccepteerd in de vorm van een geheel getal. De map-id kan, afhankelijk van de API, worden doorgegeven als onderdeel van de URI of als onderdeel van de aanvraaginstantie.

Nu moet u het maptype naast de map-id opgeven. Het maptype is &quot;Program&quot; of &quot;Folder&quot;. &quot;Map&quot; geeft een toepassingsmap aan, terwijl &quot;Programma&quot; een programmamap aangeeft. Het maptype wordt op twee verschillende manieren opgegeven, afhankelijk van de API die u aanroept:

1. Gebruik de gegevensstructuur &quot;FolderIdType&quot;. FolderIdType is een eenvoudig JSON-blok dat de id en het type paar als volgt bevat:

`{ "id" : **id**, "type" : "**type**" }`

Waar **identiteitskaart** omslagidentiteitskaart is, en &quot;**type**&quot;is het omslagtype. De toegestane waarden voor &quot;**type**&quot;zijn &quot;Omslag&quot;of &quot;Programma&quot;.

Voorbeeld - Map maken

`POST /asset/v1/folders.json`

`parent={"id":416,"type":"Folder"}&name=Test Folder&description=This is a test folder`

1. Gebruik de bestaande parameter id en specificeer zijn type gebruikend een &quot;type&quot;vraagparameter.

Voorbeeld: Map ophalen op id

`GET /rest/asset/v1/folder/1016.json?type=Program`

Alle API-reacties die een map-id in het Result-object hadden, bevatten nu ook een folderId-kenmerk waarvan de waarde een FolderIdType is. Hiermee kunt u het maptype voor een bepaalde map-id bepalen.

Voorbeeld - Map ophalen op naam

`GET /rest/asset/v1/folder/byName.json?name=Social Media`

```json
"result": [
{
...

"folderId": {"id":341, "type": "Program"},
...

"id":"341"
}
]
```

Om het omslagtype voor bepaalde identiteitskaart te bepalen, kunt u [ gebruiken doorbladert Omslagen ](https://developer.adobe.com/marketo-apis/api/asset/browse-folders/) API.

Het kenmerk &quot;type&quot; in API-reacties heeft de naam &quot;folderType&quot; gewijzigd. Dit moet vermijden verwarring met het &quot;type&quot;element in FolderIdType.

Bijvoorbeeld op basis van:

&quot;**type**&quot;:&quot;de Omslag van de Marketing&quot;

op dit punt:

&quot;**folderType**&quot;: &quot;de Omslag van de Marketing&quot;


### SDK voor mobiele betrokkenheid

iOS 0.3.5

* Probleem verholpen waarbij het dialoogvenster Testapparaat instellen werd uitgevoerd op de hoofdthread. [ MOB-638 ]
* Toegevoegde foutafhandeling bij niet-registratie van het testapparaat. [ MOB-639 ]

Android 0.3.3

* Toegevoegd android:configChanges-kenmerk aan AndroidManifest.xml `<activity>` -element om te voorkomen dat het voortgangsdialoogvenster wordt gesloten wanneer u een testapparaat hebt toegevoegd en de oriëntatie hebt gewijzigd. [ MOB-687 ]

Gepost op _2015-06-30_ door _David_

## In-app Web Personalization (Beta) die RTP API gebruikt

Verschillende van onze klanten bieden oplossingen voor webapps voor hun gebruikers en wij ontvangen aanvragen als zij Marketo Real-Time Personalization (RTP) kunnen gebruiken in hun beveiligde webtoepassingsomgeving. Het antwoord is ja! We hebben een API uitgebracht voor In-App-berichten, zodat u inhoud kunt personaliseren en marketingactiviteiten kunt promoten, zoals webinars, releases van nieuwe functies, upsells en inspraak voor uw gebruikers op basis van uw webtoepassingsgegevens. Bijvoorbeeld het aanpassen van In-App-inhoud voor:

* Proefaanbiedingen op basis van gebruikersactiviteit
* Verschillende typen abonnementen (up-sell, cross-sell of webinar training)
* Nieuwe functies die relevant zijn voor de gebruikersactiviteit

**Het teamgebruik van het Geval van het Geval van het Gebruik van de 1&rbrace; Marketo het Team van de Succes van de Klant gebruikt In-App Web Personalization om met specifieke abonnementstypes (Vonk, Norm, Uitgezocht, of Onderneming) met gepersonaliseerde inhoud te communiceren, ervoor zorgen zij progressieve campagnes zien en in-app gebruikers voeden die op hun overeenkomst worden gebaseerd.** Zie hoe dit kan worden gedaan voor een gebruiker met een Enterprise-abonnementstype. **Vereiste** begrijp de [ Context API van de Gebruiker RTP ](/help/javascript-api/web-personalization.md). **laat de Context API van de Gebruiker** Verzoek van de Steun van Marketo toe om de Context API van de Gebruiker voor uw rekening toe te laten RTP. **plaats de Variabele van de Douane** Er zijn 5 groeven van de douanevariabele beschikbaar in RTP om gegevens naar te verzenden. In dit voorbeeld sturen we een type gebruikersabonnement naar Aangepaste variabele 1.

`rtp('set', 'customVar1', 'Enterprise');`

**creeer een Nieuw Segment RTP** ga naar **Segmenten**, klik **CREËREN NIEUW**.

1. Sleep de **Context API van de Gebruiker** filter aan de segmentbouwer.
1. Selecteer de **Variabele van de Douane 1** (het Type van Abonnement) dat **waarde** **Onderneming** is.

**toon Campagne die op de Vorige Geschiedenis van het Bezoek** wordt gebaseerd om de bezoeker een andere campagne te tonen als zij reeds op een campagne in een vorig bezoek klikte.

1. Binnen de **Context API van de Gebruiker**, klik **(+)** om een ander attribuut van de Context API van de Gebruiker toe te voegen
1. Voeg de **exploitant (EN of OF) toe.**
1. Selecteer **Campagnes - geklikt.** plaats **identiteitskaart van de Campagne** aan identiteitskaart van de campagne. (Zie de onderstaande opmerking voor informatie over het zoeken naar de campagne-id.)
1. Klik **OPSLAAN &amp; DEFINIEER CAMPAIGN** om de campagne creatief tot stand te brengen.

Over het geheel genomen, zal dit segment aanpassen als een bezoeker met de douanevariabele (abonnementstype) gelijk aan Onderneming wordt geassocieerd en als zij op de campagne (identiteitskaart: 5390) in een vorig bezoek klikte. De volgende stap is een gepersonaliseerde campagne voor dit segment te bepalen. In de onderstaande screenshot ziet u een RTP-dialoogcampagne (linksonder) op de pagina Mijn Marketo waarin een webinar voor Enterprise-gebruikers wordt gepromoot.  **NOTA:** **Vestigend identiteitskaart van de Campagne** gaat naar **Campagnes**, over de **Naam van de Campagne** om van identiteitskaart van de Campagne de plaats te bepalen.
Gepost op _2015-06-17_ door _David_

## Transactiee-mails verzenden met de Marketo REST API: Deel 1

Er zijn een paar configuratievereisten binnen Marketo om de vereiste vraag met de Marketo REST API uit te voeren.

* De ontvanger moet een record in Marketo hebben
* Er moet een Transactiee-mail zijn gemaakt en goedgekeurd in uw Marketo-exemplaar.
* Er moet een actieve triggercampagne zijn met de campagne is aangevraagd, Source: Web Service API, die is ingesteld om de e-mail te verzenden

Eerst [ creeer en keur uw e-mail ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) goed. Als de e-mail werkelijk transactie is, zult u waarschijnlijk het aan operationeel moeten plaatsen, maar zeker zijn dat het wettelijk als operationeel kwalificeert. Dit is geconfigureerd met Scherm bewerken onder E-mailhandelingen > E-mailinstellingen. Goedkeuren en we zijn klaar om onze campagne te maken. Als u aan het creëren van campagnes nieuw bent, controle uit [ creeer een Nieuw Slimme Campagne ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/creating-a-smart-campaign/create-a-new-smart-campaign) artikel op docs.marketo.com. Zodra u uw campagne hebt gecreeerd, moeten wij door deze stappen gaan. Configureer uw slimme lijst met de trigger voor de campagne: nu moeten we de stroom configureren om een stap E-mail verzenden naar onze e-mail te sturen. Voordat u de activering uitvoert, moet u een aantal instellingen op het tabblad Planning opgeven. Als deze specifieke e-mail maar één keer naar een bepaalde record moet worden verzonden, laat u de kwalificatie-instellingen ongewijzigd. Als het vereist is dat zij de e-mail veelvoudige tijden ontvangen, niettemin, zult u dit aan of elke keer of aan één van de beschikbare wegen willen aanpassen. Nu zijn we klaar om te activeren.

### Het verzenden van de API Vraag

**Nota:** in de voorbeelden hieronder van Java, gebruiken wij het minimum-json pakket om vertegenwoordiging JSON in onze code te behandelen. U kunt meer over dit project hier lezen: [ https://github.com/ralfstx/minimal-json ](https://github.com/ralfstx/minimal-json) het eerste deel van het verzenden van een transactie-e-mail door API ervoor zorgt dat een verslag met het overeenkomstige e-mailadres in uw instantie van Marketo bestaat en dat wij toegang tot zijn lood identiteitskaart hebben. In het kader van dit bericht gaan we ervan uit dat de e-mailadressen al in Marketo staan en dat we alleen de id van de record hoeven op te halen. Voor dit, gebruiken wij [ krijgen Veelvoudige Leidingen door de vraag van het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET), en wij gebruiken sommige code van Java van de vorige post op opnieuw. Neem een blik bij onze Belangrijkste methode om de campagne te verzoeken:

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App 
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("<requestCampaign.test@marketo.com>");

        //Create and parameterize an instance of Leads
        //Set your email filterValue appropriately
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("test.requestCamapign@example.com");

        //Get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();

        //Get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();

        //Set the ID of your campaign from Marketo
        int campaignId = 0;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Send the request to Marketo
        rc.postData();
    }
}
```

Om aan deze resultaten van de reactie JsonObject van leadRequest te krijgen, moeten wij één of andere code schrijven. Om het eerste resultaat in de Serie terug te winnen, moeten wij de Serie uit JsonObject halen en het voorwerp krijgen dat bij 0 wordt geïndexeerd:

`JsonArray leadsResult = leadsRequest.getData().get("result").asArray();`
`int leadId = leadsResult.get(0).asObject().get("id").asInt();`

Van nu af aan moeten we alleen maar de oproep tot het indienen van een campagne doen. Hiervoor zijn de vereiste parameters id in de URL van de aanvraag en een array van JSON-objecten met één lid, &#39;id&#39;. Laten we eens kijken naar de code:

```java
package dev.marketo.blog_request_campaign;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class RequestCampaign {
 private String endpoint;
 private Auth auth;
 public ArrayList leads = new ArrayList();
 public ArrayList tokens = new ArrayList();
 
 public RequestCampaign(Auth auth, int campaignId) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/campaigns/" + campaignId + "/trigger.json";
 }
 public RequestCampaign setLeads(ArrayList leads) {
  this.leads = leads;
  return this;
 }
 public RequestCampaign addLead(int lead){
  leads.add(lead);
  return this;
 }
 public RequestCampaign setTokens(ArrayList tokens) {
  this.tokens = tokens;
  return this;
 }
 public RequestCampaign addToken(String tokenKey, String val){
  JsonObject jo = new JsonObject().add("name", tokenKey);
  jo.add("value", val);
  tokens.add(jo);
  return this;
 }
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   System.out.println("Executing RequestCampaign calln" + "Endpoint: " + s + "nRequest Body:n"  + requestBody);
   URL url = new URL(s); 
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   System.out.println("Result:n" + result);
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }
 
 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonObject input = new JsonObject();
  JsonArray leadsArray = new JsonArray();
  for (int lead : leads) {
   JsonObject jo = new JsonObject().add("id", lead);
   leadsArray.add(jo);
  }
  input.add("leads", leadsArray);
  JsonArray tokensArray = new JsonArray();
  for (JsonObject jo : tokens) {
   tokensArray.add(jo);
  }
  input.add("tokens", tokensArray);
  requestBody.add("input", input);
  return requestBody;
 }

}
```

Deze klasse heeft één aannemer die een Auth, en Id van de campagne neemt. Leads worden aan het object toegevoegd door een ArrayList `<Integer>` door te geven die de ID&#39;s van de records aan setLeads bevat, of door addLead te gebruiken, die één geheel getal neemt en dit aan de bestaande ArrayList in de eigenschap Lead toevoegt. Om de API vraag teweeg te brengen om de belangrijkste verslagen tot de campagne over te gaan, moet postData worden geroepen, die een JsonObject terugkeert die de reactiegegevens van het verzoek bevat. Wanneer een verzoekcampagne wordt geroepen, zal elk lood dat tot de vraag wordt overgegaan door de doeltrekkercampagne in Marketo worden verwerkt en wordt zijn verzonden e-mail die eerder werd gecreeerd. U hebt een e-mail getriggerd via de Marketo REST API. Houd een oogje in het zeil voor Deel 2 waar wij dynamisch het aanpassen van de inhoud van een e-mail door de Campagne van het Verzoek zullen bekijken.

Gepast op _2015-07-17_ door _Kenny_

## Lead-gegevens verifiëren en ophalen van Marketo met de REST API

**Nota:** in de voorbeelden hieronder van Java, gebruiken wij het minimum-json pakket om vertegenwoordiging JSON in onze code te behandelen. U kunt meer over dit project hier lezen: [ https://github.com/ralfstx/minimal-json ](https://github.com/ralfstx/minimal-json) Één van de gemeenschappelijkste vereisten wanneer het integreren met Marketo is de herwinning van loodgegevens. De meesten, als niet alle integraties of de herwinning of de voorlegging van loodgegevens van een abonnement van Marketo zullen vereisen, zodat zullen wij vandaag een blik bij een basistaak van de de terugwinning van loodinformatie nemen, door [&#128279;](/help/rest-api/authentication.md) met een abonnement voor authentiek te verklaren en dan loodgegevens van het terug te winnen. Om onze leads op te halen, moeten we eerst verifiëren met het doel-Marketo-exemplaar met OAuth 2.0. Er zijn drie gegevens die we moeten verifiëren met Marketo, de client-id, het clientgeheim en de host van de Marketo-instantie. Hier is de klasse die wij gebruiken om voor authentiek te verklaren:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Auth {
 protected String marketoInstance; //Instance Host obtained from Admin -> Web Service
 private String clientId; //clientId obtained from Admin -> Launchpoint -> View Details for selected service
 private String clientSecret; //clientSecret obtained from Admin -> Launchpoint -> View Details for selected service
 private String idEndpoint; //idEndpoint constructed to authenticate with service when constructor is used
 private String token; //token is stored for reuse until expiration
 private Long expiry; //used to store time of expiration
 
 //Creates an instance of Auth which is used to Authenticate with a particular service on a particular instance
 public Auth(String id, String secret, String instanceUrl) {
  this.clientId = id;
  this.clientSecret = secret;
  this.marketoInstance = instanceUrl;
  this.idEndpoint = marketoInstance + "/identity/oauth/token?grant_type=client_credentials&client_id=" + clientId + "&client_secret=" + clientSecret;
 }
 //The only public method of Auth, used to check if the current value of Token is valid, and then to retrieve a new one if it is not
 public String getToken(){
  long now  = System.currentTimeMillis();
  if (expiry == null || expiry < now){
   System.out.println("Token is empty or expired. Trying new authentication");
   JsonObject jo = getData();
   System.out.println("Got Authentication Response: " + jo);
   this.token = jo.get("access_token").asString();
                        //expires_in is reported as seconds, set expiry to system time in ms + expires \* 1000
   this.expiry = System.currentTimeMillis() + (jo.get("expires_in").asLong() \* 1000);
  }
  return this.token;
 }
 //Executes the authentication request
 private JsonObject getData(){
  JsonObject jsonObject = null;
  try {
   URL url = new URL(idEndpoint);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
   urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "application/json");
            System.out.println("Trying to authenticate with " + idEndpoint);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                Reader reader = new InputStreamReader(inStream);
                jsonObject = JsonObject.readFrom(reader);
            }else {
                throw new IOException("Status: " + responseCode);
            }
  } catch (MalformedURLException e) {
   e.printStackTrace();
  }catch (IOException e) {
            e.printStackTrace();
        }
  return jsonObject;
 }
}
```

Met deze code kunt u een instantie Auth maken met onze client-id, clientgeheim en host (als markketoInstance) vanuit Admin -> Launchpoint (ID en Secret) en Admin -> Web Services (Host). Het stelt de getToken methode bloot die test of het momenteel opgeslagen teken ongeldig of verlopen is, en keert dan of het bestaande teken terug, of voert een nieuw authentificatieverzoek dan het nieuwe teken van het &quot;access_token&quot;lid van de reactie JSON terug. Nu u kunt verifiëren met uw Marketo-exemplaar, is de volgende stap om onze leads op te halen. We gebruiken deze klasse:

```java
package dev.marketo.blog_leads;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import javax.net.ssl.HttpsURLConnection;
import com.eclipsesource.json.JsonObject;

public class Leads {
 private StringBuilder endpoint;
 private Auth auth;
 public String filterType;
 public ArrayList filterValues = new ArrayList();
 public Integer batchSize;
 public String nextPageToken;
 public ArrayList fields = new ArrayList();
 
 public Leads(Auth a) {
  this.auth = a;
  this.endpoint = new StringBuilder(this.auth.marketoInstance + "/rest/v1/leads.json");
 }
 public Leads setFilterType(String filterType) {
  this.filterType = filterType;
  return this;
 }
 public Leads setFilterValues(ArrayList filterValues) {
  this.filterValues = filterValues;
  return this;
 }
 public Leads addFilterValue(String filterVal) {
  this.filterValues.add(filterVal);
  return this;
 }
 public Leads setBatchSize(Integer batchSize) {
  this.batchSize = batchSize;
  return this;
 }
 public Leads setNextPageToken(String nextPageToken) {
  this.nextPageToken = nextPageToken;
  return this;
 }
 public Leads setFields(ArrayList fields) {
  this.fields = fields;
  return this;
 }

 public JsonObject getData() {
        JsonObject result = null;
        try {
         endpoint.append("?access_token=" + auth.getToken() + "&filterType=" + filterType + "&filterValues=" + csvString(filterValues));
         if (batchSize != null && batchSize > 0 && batchSize <= 300){
             endpoint.append("&batchSize=" + batchSize);
            }
            if (nextPageToken != null){
             endpoint.append("&nextPageToken=" + nextPageToken);
            }
            if (fields != null){
             endpoint.append("&fields=" + csvString(fields));
            }
            URL url = new URL(endpoint.toString());
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setRequestProperty("accept", "text/json");
            InputStream inStream = urlConn.getInputStream();
            Reader reader = new InputStreamReader(inStream);
            result = JsonObject.readFrom(reader);
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return result;
    }
 private String csvString(ArrayList fields) {
  StringBuilder fieldCsv = new StringBuilder();
     for (int i = 0; i < fields.size(); i++){
      fieldCsv.append(fields.get(i));
      if (i + 1 != fields.size()){
       fieldCsv.append(",");
      }
     }
  return fieldCsv.toString();
 }
}
```

Deze klasse heeft één constructor die een object Auth accepteert en vervolgens verschillende setters voor zowel optionele als vereiste parameters beschikbaar maakt. In dit geval hoeven we ons alleen bezig te houden met het instellen van het filterType en filterValues om &#39;get&#39;-leads per e-mailadres uit te voeren. Zo zullen wij setFilterType voor &quot;e-mail,&quot;en addFilterValue voor het e-mailadres gebruiken dat wij een identiteitskaart voor moeten terugwinnen. Wanneer u uw parameters hebt geplaatst, kunt u de methode getData gebruiken om een JsonObject van het loodeindpunt terug te winnen, die een resultaatserie bevat die een vertegenwoordiging JSON van de loodverslagen heeft die werden teruggewonnen.

### Samenvoegen

Nu we de voorbeeldcode hebben doorlopen die we gebruiken, laten we een eenvoudig voorbeeld bekijken om leads op te halen die overeenkomen met een teste-mailadres, <testlead@marketo.com> . Hiervoor moeten we setFilterType gebruiken voor &quot;email&quot; en addFilterValue voor het e-mailadres waarvoor we informatie moeten ophalen. Wanneer u uw parameters hebt geplaatst, kunt u de methode getData gebruiken om een JsonObject van het loodeindpunt terug te winnen, die een resultaatserie bevat die een vertegenwoordiging JSON van de loodverslagen heeft die werden teruggewonnen.

```java
package dev.marketo.blog_leads;

import com.eclipsesource.json.JsonObject;

public class App 
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        //Change the credentials here to your own
        Auth auth = new Auth("CHANGE ME", "CHANGE ME", "CHANGE ME");
        //Create and parameterize an instance of Leads
        Leads getLeads = new Leads(auth)
              .setFilterType("email")
              .addFilterValue("<testlead@marketo.com>");
        //get the inner results array of the response
        JsonObject leadsResult = getLeads.getData();
        System.out.println(leadsResult);
    }
}
```

In dit belangrijkste methodevoorbeeld, leiden wij tot een geval van Auth, en dan dit tot een nieuwe aannemer van Leads over. Gebruikend setFilterType, en addFilterValue, vormen wij onze instantie van Leads om enkel lood terug te winnen die het e-mailadres &quot;<testlead@marketo.com>&quot;aanpassen. In dit voorbeeld wordt dit naar de console afgedrukt:

Token is leeg of verlopen. Nieuwe verificatie proberen
Proberen te verifiëren met <https://299-BYM-827.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id=b417d98f-9289-47d1-a61f-db141bf0267f&client_secret=0DipOvz4h2wP1ANeVjlfwMvECJpo0ZYc>
De Reactie van de Authentificatie van het krijgen: {&quot;access_token&quot;:&quot;ec0f02c0-28ac-4d6c-b7d7-00e47ae85ff1:st&quot;, &quot;token_type&quot;:&quot;betoonder&quot;,&quot;expired_in&quot;:538,&quot;scope&quot;:&quot;<apiuser@mktosupport.com>&quot;}
{&quot;requestId&quot;:&quot;14fb6#14e6a7a9ad6&quot;, &quot;resultaat&quot;:[{&quot;id&quot;:1026322, &quot;updatedAt&quot;:&quot;2015-07-07T21 :43: 25Z&quot;, &quot;lastName&quot;:&quot;Lead&quot;, &quot;email&quot;:&quot;<testlead@marketo.com>&quot;, &quot;createdAt&quot;:&quot;2015-07-07T21 :43: 25Z&quot;, &quot;firstName&quot;:&quot;Test&quot;},{&quot;id&quot;:1026323,&quot;updatedAt&quot;:&quot;2015-07-07T 21 :43: 43Z&quot;, &quot;lastName&quot;:&quot;Lead2&quot;, &quot;e-mail&quot;:&quot;<testlead@marketo.com>&quot;, &quot;createdAt&quot;:&quot;2015-07-07T21 :43: 43Z&quot;, &quot;firstName&quot;:&quot;Test&quot;} ], &quot;succes&quot;:waar}

Nu hebben we leidende gegevens die we op welke manier dan ook kunnen verwerken. Bedankt voor het lezen en geef feedback in de opmerkingen.

Gepast op _2015-07-10_ door _Kenny_

## Updates van de release van juli 2015

REST API

* Verkoper-API

De nieuwe [ eindpunten van de verkooppersoon ](/help/rest-api/sales-persons.md) zijn geïntroduceerd die u toestaan programmatically van een lijst te maken, te beschrijven, en de gegevens te CRUD die binnen een voorwerp van de verkooppersoon van Marketo verblijven. Bovendien kan een verkooppersoon aan een lood, een kans, of een bedrijf worden toegewezen. Dit wordt gedaan door een &quot;externalSalesPersonId&quot;attribuut te specificeren wanneer het roepen van Create/Update/Upsert eindpunt voor lood, kans, of bedrijf.

Opmerking: Rolmachtigingen zijn toegevoegd om toegang te verlenen tot de eindpunten van het programma: Alleen-lezen verkoper, een lezenverkoper. Als uw API-gebruikersrol dateert van vóór de release van de verkoop-API&#39;s, moet u uw API-gebruikersrol bijwerken met deze machtigingen om toegang mogelijk te maken. Anders, ontvangt u een 603 &quot;Afgewezen Toegang&quot;foutenreactie.

* Element-API - Landingspagina-sjabloon

De nieuwe [ het landen van paginamalanduidpoints ](https://developer.adobe.com/marketo-apis/api/asset/#landing_page_templates_endpoints) zijn geïntroduceerd die u toestaan om programmatically van de lijst, tot stand te brengen en, de gegevens bij te werken verbonden aan een het landen paginamalplaatje.

* Asset API - Segmenten

Er zijn twee aan segmenten gerelateerde eindpunten ingevoerd:

[ krijg Segmenten ](https://developer.adobe.com/marketo-apis/api/asset/get-segments)

[ krijg Segmentatie door Id ](https://developer.adobe.com/marketo-apis/api/asset/get-segmentation-by-id)

* Oplossing voor een probleem waarbij Map op naam ophalen niet overeenkwam met de parameter &quot;workSpace&quot;. [ LM-61059 ]
* Verschillende prestatieverbeteringen zijn aangebracht in de API&#39;s voor aangepaste objecten.

Gepost op _2015-07-17_ door _David_

## Leads, bedrijven en mogelijkheden maken en koppelen met de Marketo REST API

Om ten volle te kunnen profiteren van Marketo-analyses is het van cruciaal belang om correcte en robuuste koppelingen tot stand te brengen tussen uw hoofd-, bedrijfs- en opportuniteitsgegevens. Als u geen gebruik maakt van een native CRM-sync, kan het bouwen van deze relaties enige problemen opleveren. Daarom lopen we vandaag door ze te bouwen.

### Objectrelaties

In Marketo zijn er een paar vitale relaties om de rapportage van kansen volledig tot stand te brengen:

* De leiders en de Kansen hebben een vele aan vele verhouding met het voorwerp OpportunityRole.
* OpportunityRole heeft zowel een leadId- als een extern opportunityid-veld om de relatie van &#39;lead&#39; tot &#39;opportunity&#39; te maken.
* Om in aanmerking te komen voor een Has Opportunity Smart-lijstfilter, moet een lead een OpportunityRole hebben dat gerelateerd is aan een opportuniteit.
* De kansen hebben een vele-aan-één verhouding aan het voorwerp van het Bedrijf via het externalCompanyId gebied.
* De leiders hebben een één-aan-vele verhouding aan Bedrijven via het externalCompanyId gebied.
* De kansen worden toegeschreven aan een programma dat op het Programma van de Aankoop van een lood of hun lidmaatschap en succes in een programma wordt gebaseerd (zie [ Begrip Attributie ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/reporting/revenue-cycle-analytics/revenue-tools/attribution/understanding-attribution)).

Als u deze relaties opbouwt in uw hoofddatabase, kunt u Marketo-analysemogelijkheden volledig benutten en zien welke invloed uw programma&#39;s hebben op het creëren van kansen en het winnen van de resultaten.

### Bedrijven

De eenvoudigste manier om deze verhoudingen uit te bouwen is door met bedrijfverwezenlijking te beginnen. Dit zorgt ervoor dat wij externalCompanyId tot onze kansen tijdens verwezenlijking kunnen overgaan, in plaats van het moeten extra API vraag uitvoeren om kansen bij te werken nadat zij zijn gecreeerd. Afhankelijk van de bestaande configuratie, kan dit al dan niet een noodzakelijke stap zijn, maar de netto nieuwe lood en de contacten met verbonden bedrijven moeten deze verslagen hebben aan uw instantie van Marketo worden toegevoegd opdat de verhoudingen worden gebouwd, zodat nemen een blik bij één of andere code om bedrijfverslagen tot stand te brengen.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertCompanies {
 public List<JsonObject> input; //a list of Companies to use for input.  Each must have a member "externalCompanyId".
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate 
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters(externalCompanyId), idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object
 
 
 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertCompanies(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/companies.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertCompanies(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 //adds input to existing list, creates arraylist if it was built without a list
 public UpsertCompanies addCompanies(JsonObject... companies){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : companies) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s); 
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }
 
 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our company records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertCompanies instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 //sets or replaces existing input with list
 public UpsertCompanies setInput(List input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertCompanies setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertCompanies setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }
 
}
```

De bedrijf-API biedt twee deduplicatieopties, ingesteld door de parameter dedupeBy in de aanvraag, &quot;dedupeFields&quot; en &quot;idField&quot;. Deze kunnen uitdrukkelijk worden teruggewonnen door te roepen beschrijven Bedrijven. Als dedupeBy unset is, is het gebrek aan dedupeFields. In het geval van bedrijfsverslagen, dedupeFields altijd aan &quot;externalCompanyId,&quot;beantwoordt dat een willekeurige koord door een externe bron wordt geplaatst, en idField, die aan het &quot;marketoId&quot;gebied beantwoorden, dat een geheel is dat door Marketo na verwezenlijking wordt geproduceerd en is teruggekeerd. Afhankelijk van de selectie voor dedupeBy, moet één van externalCompanyId of marketoId in om het even welke upsert vraag naar een bedrijfverslag worden omvat. Deze vereisten gelden ook voor de API&#39;s van de objecten Opportunity en Opportunity Role. Onze code stelt twee aannemers bloot: één die één enkel argument van een voorwerp van de Auth goedkeuren, en een andere die Auth en een lijst van [ JsonObject ](https://github.com/ralfstx/minimal-json) bedrijfverslagen goedkeurt. Als geconstrueerd zonder een inputlijst, dan moeten de bedrijfverslagen door de addCompanies methode worden toegevoegd, die zal controleren creeert een nieuwe ArrayList als de input ongeldig is, en dan alle argumenten JsonObject toevoegen aan de inputlijst. Hier volgt een voorbeeld van gebruik:

```java
//Create a new company to associate to
JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
JsonObject companiesResult = upsertCompanies.postData();
System.out.println(companiesResult);
```

We creëren één bedrijf JsonObject met slechts één veld, `externalCompanyId`, en bouwen vervolgens een instantie van UpsertCompanies, en voegen ons bedrijf toe aan de invoerlijst met `addCompanies` .

### Kansen

Net als bij de bedrijfsobjecten heeft de opportunityid een parameter `dedupeBy` , waarbij &quot;dedupeFields&quot; of &quot;idField&quot; wordt geaccepteerd die overeenkomt met respectievelijk &quot;external opportunityid&quot; en &quot;marketoGUID&quot;. Hier is onze code, die er ongeveer hetzelfde uitziet als de klasse UpsertCompanies:

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunities {
 public List<JsonObject> input; //a list of Opportunities to use for input.  Each must have a member "externalopportunityid".  Each can optionally include "externalCompanyId" for company association
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate 
 public String dedupeBy; //select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object
 
 
 //Constructs an UpsertOpportunities with Auth, but with no input set
 public UpsertOpportunities(Auth auth){
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunities(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunities addOpportunities(JsonObject... opp){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : opp) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s); 
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream(); //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }
 
 private JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject(); //Create a new JsonObject for the Request Body
  JsonArray in = new JsonArray(); //Create a JsonArray for the "input" member to hold Opp records
  for (JsonObject jo : input) {
   in.add(jo); //add our Opportunity records to the input array
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action); //add the action member if available
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy); //add the dedupeBy member if available
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunities setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunities setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunities setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }
 
}
```

Dezelfde constructoropties worden opgegeven, waarbij een Auth of `Auth+List<JsonObject>` wordt gebruikt en een `addOpportunities` -methode wordt gebruikt om JsonObject-mogelijkheden in te voeren. Hier volgt een gebruiksvoorbeeld:

```java
//Create some JsonObjects for Opportunity Data
JsonObject opp1 = new JsonObject().add("name", "opportunity1")
    .add("externalopportunityid", "Opportunity1Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");
JsonObject opp2 = new JsonObject().add("name", "opportunity2")
    .add("externalopportunityid", "Opportunity2Test")
    .add("externalCompanyId", "myCompany")
    .add("externalCreatedDate", "2015-01-01T00:00:00z");

//Create an Instance of UpsertOpportunities and POST it
UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                        .setAction("createOnly")
                        .addOpportunities(opp1, opp2);
JsonObject oppsResult = upsertOpps.postData();
System.out.println(oppsResult);
```

Hier, creëren wij twee voorbeeldkansen en dan geven zij waarden voor de naam, externalopportunityid, externalCompanyId, en externalCreatedDate gebieden. ExternalCreatedDate is nog niet besproken, maar het is belangrijk om te gebruiken aangezien het als hoofdgebied in RCE wordt behandeld voor wanneer een kans werd gecreeerd, die het voor correcte attributie belangrijk maken. U kunt de bedrijfslogica van uw organisatie gebruiken om te bepalen wat u op dit gebied invoert, gebaseerd op of u bestaande opportuniteitsgegevens terugvult, of nieuwe creeert op de vlucht. Wij creëren onze instantie van UpsertOpportunity en voegen dan onze JsonObjects via addOpportunity toe. Nu de instantie is geconfigureerd, kunt u dit naar Marketo drukken met postData en uw resultaat afdrukken

### Rollen

Rollen lijken veel op de voorafgaande twee objecten, behalve dat ze een iets andere vereiste hebben bij het instellen van dedupeBy op dedupeFields. Rollen vereisen dat drie velden worden opgenomen wanneer u een record maakt of bijwerkt met deze methode, &quot;leadId&quot;, &quot;role&quot; en &quot;externalOpportunityid&quot;. &quot;rol&quot; kan elke tekenreekswaarde zijn, maar de andere twee moeten verwijzen naar respectievelijk een geldige id van een lead en een geldige id van een opportunity.

```java
package dev.marketo.opportunities;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.Reader;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

import javax.net.ssl.HttpsURLConnection;

import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

public class UpsertOpportunityRoles {
 public List<JsonObject> input; //Array of Opportunity Roles as JsonObjects, must have "leadId", "role" and "externalopprtunityid"
 public String action; //specify the action to be undertaken, createOnly, updateOnly, createOrUpdate, defaults to createOrUpdate if unset
 public String dedupeBy;//select mode of Deduplication, dedupeFields for all dedupe parameters, idField for marketoId
 private String endpoint; //endpoint URL created with Constructor
 private Auth auth; //Marketo Auth Object
 
 //Constructs an UpsertOpportunityRoles with Auth, but with no input set
 public UpsertOpportunityRoles(Auth auth) {
  this.auth = auth;
  this.endpoint = this.auth.marketoInstance + "/rest/v1/opportunities/roles.json";
 }
 //Constructs and UpsertOpportunities with Auth and input set
 public UpsertOpportunityRoles(Auth auth, List<JsonObject> input) {
  this(auth);
  this.input = input;
 }
 public UpsertOpportunityRoles addRoles(JsonObject... role){
  if (this.input == null){
   this.input = new ArrayList();
  }
  for (JsonObject jo : role) {
   System.out.println(jo);
   this.input.add(jo);
  }
  return this;
 }
 //executes the request to Marketo, body will be empty if input is not set
 public JsonObject postData(){
  JsonObject result = null;
  try {
   JsonObject requestBody = buildRequest(); //builds the Json Request Body
   String s = endpoint + "?access_token=" + auth.getToken(); //takes the endpoint URL and appends the access_token parameter to authenticate
   URL url = new URL(s);
   HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection(); //Return a URL connection and cast to HttpsURLConnection
   urlConn.setRequestMethod("POST");
   urlConn.setRequestProperty("Content-type", "application/json");//"application/json" content-type is required.
            urlConn.setRequestProperty("accept", "text/json");
            urlConn.setDoOutput(true);
   OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
   wr.write(requestBody.toString());
   wr.flush();
   InputStream inStream = urlConn.getInputStream();  //get the inputStream from the URL connection
   Reader reader = new InputStreamReader(inStream);
   result = JsonObject.readFrom(reader); //Read from the stream into a JsonObject
   urlConn.disconnect();
  } catch (MalformedURLException e) {
   e.printStackTrace();
  } catch (IOException e) {
   e.printStackTrace();
  }
  return result;
 }
 
 public JsonObject buildRequest(){
  JsonObject requestBody = new JsonObject();
  JsonArray in = new JsonArray();
  for (JsonObject jo : input) {
   in.add(jo);
  }
  requestBody.add("input", in);
  if (this.action != null){
   requestBody.add("action", action);
  }
  if (this.dedupeBy != null){
   requestBody.add("dedupeBy", dedupeBy);
  }
  return requestBody;
 }
 //Getters and Setters
 //Setters return the UpsertOpportunites instance to allow simple formatting:
 public List<JsonObject> getInput() {
  return input;
 }
 public UpsertOpportunityRoles setInput(List<JsonObject> input) {
  this.input = input;
  return this;
 }
 public String getAction() {
  return action;
 }
 public UpsertOpportunityRoles setAction(String action) {
  this.action = action;
  return this;
 }
 public String getDedupeBy() {
  return dedupeBy;
 }
 public UpsertOpportunityRoles setDedupeBy(String dedupeBy) {
  this.dedupeBy = dedupeBy;
  return this;
 }
 
}
```

We volgen een vergelijkbaar patroon voor de constructors en de methode addRoles als de vorige voorbeelden. Hier is een voorbeeld.

```java
//Create Some opp roles now
JsonObject opp1Role = new JsonObject()
                        .add("role", "Captain")
                        .add("externalopportunityid", opp1.get("externalopportunityid").asString())
                        .add("leadId", 318794);
JsonObject opp2Role = new JsonObject()
                        .add("role", "Commander")
                        .add("externalopportunityid", opp2.get("externalopportunityid").asString())
                        .add("leadId", 318795);

//Create an Instance of UpsertOpportunityRoles and POST it
UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
                        .setAction("createOnly")
                        .addRoles(opp1Role, opp2Role);
JsonObject rolesResult = upsertRoles.postData();
System.out.println(rolesResult);
```


Hier creëren we de nieuwe JsonObjects voor onze 2 voorbeeldrollen, en voegen hun vereiste dedupeFields toe, die de externalopportunityid van de kansen halen die we reeds hebben gecreëerd, en hen dan naar Marketo duwen.

### Alles samenvoegen

Hier is het volledige voorbeeld van onze belangrijkste methode:

```java
package dev.marketo.opportunities;

import com.eclipsesource.json.JsonObject;

public class App 
{
    public static void main( String[] args )
    {
     //create an Instance of Auth 
        Auth auth = new Auth("CLIENT_ID_CHANGE_ME", "CLIENT_SECRET_CHANGE_ME", "MARKETO_HOST_CHANGE_ME");
        
        //Create a new company to associate to
        JsonObject myCompany = new JsonObject().add("externalCompanyId", "myCompany");
        UpsertCompanies upsertCompanies = new UpsertCompanies(auth).addCompanies(myCompany);
        JsonObject companiesResult = upsertCompanies.postData();
        System.out.println(companiesResult);
        
        //Create some JsonObjects for Opportunity Data
        JsonObject opp1 = new JsonObject().add("name", "opportunity1")
           .add("externalopportunityid", "Opportunity1Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");
        JsonObject opp2 = new JsonObject().add("name", "opportunity2")
           .add("externalopportunityid", "Opportunity2Test")
           .add("externalCompanyId", "myCompany")
           .add("externalCreatedDate", "2015-01-01T00:00:00z");

        //Create an Instance of UpsertOpportunities and POST it
        UpsertOpportunities upsertOpps = new UpsertOpportunities(auth)
                                .setAction("createOnly")
                                .addOpportunities(opp1, opp2);
        JsonObject oppsResult = upsertOpps.postData();
        System.out.println(oppsResult);
        
        //Create Some opp roles now
        JsonObject opp1Role = new JsonObject()
           .add("role", "Captain")
           .add("externalopportunityid", opp1.get("externalopportunityid").asString())
           .add("leadId", 318794);
        JsonObject opp2Role = new JsonObject()
           .add("role", "Commander")
           .add("externalopportunityid", opp2.get("externalopportunityid").asString())
           .add("leadId", 318795);
        
        //Create an Instance of UpsertOpportunityRoles and POST it
        UpsertOpportunityRoles upsertRoles = new UpsertOpportunityRoles(auth)
           .setAction("createOnly")
           .addRoles(opp1Role, opp2Role);
        JsonObject rolesResult = upsertRoles.postData();
        System.out.println(rolesResult);

    }
}
```

U kunt de opeenvolging zien van het creëren van bedrijven, kansen, en rollen. Nu, bent u allen geplaatst om uw gegevens van Bedrijf en van de Kans aan Marketo te synchroniseren.

GEPast op _2015-08-07_ door _Kenny_

## Transactiee-mails verzenden met de Marketo REST API: Deel 2, Aangepaste inhoud

Deze week bekijken we hoe we dynamische inhoud kunnen doorgeven aan onze e-mails via de API-oproep voor aanvraagcampagne. De Campagne van het verzoek staat niet alleen het teweegbrengen van e-mail extern toe, maar u kunt de inhoud van [ Mijn Tokens ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/programs/tokens/understanding-my-tokens-in-a-program) binnen e-mail ook vervangen. Mijn tokens zijn herbruikbare inhoud die kan worden aangepast op het niveau van de programma- of marketingmap. Deze kunnen ook als placeholders bestaan om door uw vraag van de verzoekcampagne worden vervangen.

### Uw e-mail maken

Om onze inhoud aan te passen, moeten wij eerst a [ programma ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/create-a-program) en een [ e-mail ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) in Marketo vormen. Om onze aangepaste inhoud te genereren, moeten we tokens in het programma maken en deze in de e-mail plaatsen die we gaan verzenden. Eenvoudigheidshalve gebruiken we slechts één token in dit voorbeeld, maar u kunt elk aantal tokens in een e-mail vervangen, in het vak Van e-mail, Van naam, Antwoord naar of elke andere inhoud in de e-mail. Laten we dus één token Rich Text maken voor vervanging en deze &#39;bodyReplacement&#39; noemen. Met RTF kunnen we alle inhoud van de token vervangen door willekeurige HTML die we willen invoeren. Tokens kunnen niet worden opgeslagen als ze leeg zijn. Plaats hier een tijdelijke aanduiding voor tekst. Nu moeten we ons token invoegen in de e-mail: Dit token is nu toegankelijk voor vervanging via een oproep tot een campagne. Deze token kan zo eenvoudig zijn als een enkele tekstregel die per e-mail moet worden vervangen, of kan bijna de volledige opmaak van de e-mail bevatten.

### De code

We breiden de code van vorige week uit om aangepaste tokens door te geven aan ons verzoek campagnegesprek.

```java
package dev.marketo.blog_request_campaign;

import com.eclipsesource.json.JsonArray;

public class App 
{
    public static void main( String[] args )
    {
     //Create an instance of Auth so that we can authenticate with our Marketo instance
        Auth auth = new Auth("Client ID - CHANGE ME", "Client Secret - CHANGE ME", "Host - CHANGE ME");
        
        //Create and parameterize an instance of Leads
        Leads leadsRequest = new Leads(auth).setFilterType("email").addFilterValue("requestCampaign.test@marketo.com");
        
        //get the inner results array of the response
        JsonArray leadsResult = leadsRequest.getData().get("result").asArray();
        
        //get the id of the record indexed at 0
        int lead = leadsResult.get(0).asObject().get("id").asInt();
        
        //Set the ID of our campaign from Marketo
        int campaignId = 1578;
        RequestCampaign rc = new RequestCampaign(auth, campaignId).addLead(lead);

        //Create the content of the token here, and add it to the request
        String bodyReplacement = "<div class="replacedContent"><p>This content has been replaced</p></div>";
        rc.addToken("{{my.bodyReplacement}}", bodyReplacement);
        rc.postData();
    }
}
```

Dit keer creëren wij de inhoud van ons teken in de bodyReplacement variabele en dan gebruikend de addToken methode om het aan het verzoek toe te voegen. addToken neemt een sleutel en een waarde en leidt dan tot een vertegenwoordiging JsonObject en voegt het aan de interne tokens serie toe. Dit wordt dan geserialiseerd tijdens de postData methode en leidt tot een lichaam dat als dit kijkt:

`{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}`

Gecombineerd ziet onze consoleoutput als dit:

```bash
Token is empty or expired. Trying new authentication
Trying to authenticate with&nbsp;...
Got Authentication Response: {"access_token":"19d51b9a-ff60-4222-bbd5-be8b206f1d40:st","token_type":"bearer","expires_in":3565,"scope":"<apiuser@mktosupport.com>"}
Executing RequestCampaign call
Endpoint: .../rest/v1/campaigns/1578/trigger.json?access_token=19d51b9a-ff60-4222-bbd5-be8b206f1d40:st
Request Body:
{"input":{"leads":[{"id":1}],"tokens":[{"name":"{{my.bodyReplacement}}","value":"<div class="replacedContent"><p>This content has been replaced</p></div>"}]}}
Result:
{"requestId":"1e8d#14eadc5143d","result":[{"id":1578}],"success":true}
```

### Omloop omhoog

Deze methode kan op allerlei manieren worden uitgebreid, waarbij de inhoud in e-mails binnen afzonderlijke lay-outsecties of buiten e-mails wordt gewijzigd, zodat aangepaste waarden kunnen worden doorgegeven aan taken of interessante momenten. Overal waar een teken van binnen een programma kan worden gebruikt kan worden aangepast gebruikend deze methode. De gelijkaardige functionaliteit is ook beschikbaar met de [ vraag van de Campagne van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST) die u zal toestaan om tokens over een volledige partijcampagne te verwerken. Deze kunnen niet per lood worden aangepast, maar zijn zeer nuttig om inhoud over een brede reeks lood aan te passen.

GEPast op _2015-07-24_ door _Kenny_

## Loodgegevens bijwerken in Marketo met de API

Meer dan een jaar geleden plaatsten wij het Bijwerken klant en perspectiefinformatie in Marketo gebruikend API. In dat bericht werd een codevoorbeeld weergegeven dat op terugkerende basis kan worden uitgevoerd om een extern systeem voor updates te opvragen. Het idee was om de externe gegevens op te halen en deze naar Marketo te sturen om de informatie over leads bij te werken. Het codevoorbeeld dat wordt weergegeven, heeft onze SOAP API gebruikt. REST-eindpunt om hetzelfde doel te bereiken. **Invoer van het Programma** het is waarschijnlijk dat u de gegevens van het externe systeem in een formaat moet omzetten dat door Marketo REST APIs kan worden verbruikt. Aangezien wij Create/Update Leads API gebruiken, moeten de gegevens als JSON worden geformatteerd die in het verzoeklichaam wordt verzonden. Dit wordt het best verklaard door een voorbeeld. In de onderstaande Java-voorbeeldcode hebben we modelgegevens in een array van tekenreeksen geplaatst. Elke tekenreeks die volgende gegevens bevat voor elke lead: voornaam, achternaam, e-mailadres, functie.

```java
private static String externalLeadData[] = {
        "Henry, Adams, <henry@superstar.com>, Director of Demand Generation",
        "Suzie, Smith, <ssmith@gmail.com>, VP Marketing"
};
```

De voorbeeldcode transformeert de array naar het JSON-blok hieronder.

```json
{
"input":
    [
        {"firstName":"Henry", "lastName":"Adams", "email":"<henry@superstar.com>", "title":"Director of Demand Generation"},
        {"firstName":"Suzie", "lastName":"Smith", "email":"<ssmith@gmail.com>", "title":"VP Marketing"}
    ]
}
```

Elk &quot;input&quot; array-item komt overeen met een individuele lead in Marketo. De arrayitems zijn JSON-objecten die een of meer Marketo-namen voor lead-velden en hun respectieve waarden bevatten. De veldnamen die u opgeeft (in dit geval firstName, lastName, email en title) moeten overeenkomen met de REST API-naam die is gedefinieerd voor het Marketo-abonnement. U kunt de REST API-namen vinden in de sectie voor veldbeheer in het Marketo-beheervenster door de veldnamen te exporteren. De veldnamen worden naar een Excel-bestand geëxporteerd, zoals hieronder wordt weergegeven. Alternatief, kunt u gebiedsnamen programmatically vinden door [ te roepen beschrijft lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2) API. Hier ziet u bijvoorbeeld een fragment met de REST API-naam voor het veld Voornaam.

```json
{
    "id": 29,
    "displayName": "First Name",
    "dataType": "string",
    "length": 255,
    "soap": {
        "name": "FirstName",
        "readOnly": false
    },
    "rest": {
        "name": "firstName",
        "readOnly": false
    }
},
```

**Logische van het Programma** Zodra de nuttige lading in het juiste formaat is, zijn wij bereid om Create/Update Leads te roepen. In dit voorbeeld geven we geen van de optionele parameters op. Standaard worden hoofdrecords (upsert) gemaakt of bijgewerkt, wordt e-mail gebruikt voor deduplicatiedoeleinden en wordt synchrone verwerking gebruikt. Als de aanroep naar Create/Update leads succesvol is, bevat de hoofdtekst van de reactie een array met resultaten die de Marketo Lead Id en de status van de bewerking Maken/bijwerken bevat. Afhankelijk van de parameter &quot;action&quot; die u in de aanvraag hebt doorgegeven, kan de status &quot;update&quot;, &quot;created&quot; of &quot;skipped&quot; zijn. Als u doorgaat met ons voorbeeld, stel dat de eerste lead niet bestond (Henry Adams) en de tweede lead wel bestond (Suzie Smith). Een reactie gelijkend op het volgende zou erop wijzen de eerste lood werd gecreeerd, en de tweede lood werd bijgewerkt.

```json
{
    "success":true,
    "requestId":"118f8#14f1a0b82fc",
    "result":[
        {
            "id":318798,
            "status":"created"
        },
        {
            "id":318797,
            "status":"updated"
        }
    ]
}
```

Dat is het voor nu. Fijne codering! **Code van het Programma**

```java
package com.marketo;

// minimal-json library (<https://github.com/ralfstx/minimal-json>)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import javax.net.ssl.HttpsURLConnection;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStreamWriter;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

public class CreateUpdateLeads {
    //
    // Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    //    public static String CUSTOM_SERVICE_DATA[] =
    //      {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"};
    //
    private static final String CUSTOM_SERVICE_DATA[] =
            { INSERT YOUR CUSTOM SERVICE DATA HERE };

    //
    // Mock up data that could be read from an external data source.
    // An array of CSV strings the form:
    //   "firstName, lastName, email, title"
    private static String externalLeadData[] = {
        "Henry, Adams, henry@superstar.com, Director of Demand Generation",
        "Suzie, Smith, ssmith@gmail.com, VP Marketing"
    };

    public static void main(String[] args) {
        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
                CUSTOM_SERVICE_DATA[0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[1], CUSTOM_SERVICE_DATA[2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URL for Create/Update Leads
        String createUpdateLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s", baseUrl, accessToken);

        // Convert String array into JsonArray to pass as part of request body
        JsonArray input = convertExternalLeadDataToJson(externalLeadData);

        // Build request body JSON
        JsonObject requestObj = new JsonObject();
        requestObj.add("input", input);

        // Call Create/Update Lead API
        JsonArray result = new JsonArray();
        JsonObject leadObj = JsonObject.readFrom(httpRequest("POST", createUpdateLeadsUrl, requestObj.toString()));
        if (leadObj.get("success").asBoolean()) {
            if (leadObj.get("result") != null) {
                result = leadObj.get("result").asArray();
            }
        }

        // Print out results object
        System.out.println(result);
        System.exit(0);
    }

    // Convert array of CSV formatted Strings into an array of JSON objects
    private static JsonArray convertExternalLeadDataToJson(String input[]) {
        JsonArray output = new JsonArray();

        // Loop through each CSV row in array
        for (int i = 0; i < input.length; i++) {
            // Split CSV row into separate fields
            List items = Arrays.asList(input[i].split(","));

            // Add fields to JSON object
            JsonObject lead = new JsonObject();
            lead.add("firstName", items.get(0));
            lead.add("lastName", items.get(1));
            lead.add("email", items.get(2));
            lead.add("title", items.get(3));
            output.add(lead);
        }
        return output;
    }

    // Perform HTTP request
    private static String httpRequest(String method, String endpoint, String body) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setDoOutput(true);
            urlConn.setRequestMethod(method);
            switch (method) {
                case "GET":
                    break;
                case "POST":
                    urlConn.setRequestProperty("Content-type", "application/json");
                    urlConn.setRequestProperty("accept", "text/json");
                    OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                    wr.write(body);
                    wr.flush();
                    break;
                default:
                    System.out.println("Error: Invalid method.");
                    return data;
            }
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Gepost op _2015-08-14_ door _David_

## Verkoopgegevens toevoegen aan Marketo


Met de nieuwe SalesPerson-API&#39;s kunt u Marketo-leads zonder native CRM-integratie in alle vrijheid koppelen aan SalesPerson-records. Dit maakt het gebruik van {{lead.Lead Owner Email Address}} en verwante velden en tokens in Marketo mogelijk.

### SalesPerson-records maken

Als we leads aan SalesPerson records willen koppelen, moeten we eerst onze SalesPerson-records in Marketo invoeren. Hier is een voorbeeldklasse in PHP:

```php
<?php

class SalesPerson{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $dedupeBy;//dedupeFields or idField
 private $input;//array of salesperson objects for input
 
 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }
 
 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->dedupeBy)){
   $body->dedupeBy = $this->dedupeBy;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/salespersons.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction($action){
  return $this->action;
 }
 public function setDedupeBy($dedupeBy){
  $this->dedupeBy = $dedupeBy;
 }
 public function getDedupeBy($dedupeBy){
  return $this->dedupeBy;
 }
 public function addSalesPersons($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else{
   $this->input = $input;
  }
 }
 public function getInput(){
  return $this->input;
 }
 
}
```

In de bovenstaande klasse is de variabele $input een array van objecten stdClass met mogelijke leden:

* externalSalesPersonId(alleen geldig bij maken)
* id (alleen als modus Alleen sleutelinvoer voor updateOnly)
* email
* fax
* firstName
* lastName
* mobilePhone
* telefoon
* titel

Meer gedetailleerde informatie over types en gebiedslengten kan door de beschrijf vraag worden teruggewonnen SalesPerson.

### Leads synchroniseren

Hier volgt een snelle voorbeeldklasse voor het synchroniseren van de leads die we nodig hebben:

```php
<?php

class Leads{
 private $auth;//auth object
 private $action;// string designating request action, createOnly, updateOnly, createOrUpdate
 private $lookupField;//dedupeFields or idField
 private $input;//array of salesperson objects for input
 private $asyncProcessing;//specify whether input should be processed asynchronously
 private $partitionName;//partition name for update if requires

 //takes an Auth object as the first argument
 public function _construct($auth, $input){
  $this->auth = $auth;
  $this->input = $input;
 }

 //constructs the json request body
 private function bodyBuilder(){
  $body = new stdClass();
  $body->input = $this->input;
  if (isset($this->action)){
   $body->action = $this->action;
  }
  if (isset($this->lookupField)){
   $body->lookupField = $this->lookupField;
  }
  return json_encode($body);
 }
 //submits the request to Marketo and returns the response as a string
 public function postData(){
  $url = $this->auth->getHost() . "/rest/v1/leads.json?access_token=" . $this->auth->getToken();
  $ch = curl_init($url);
  $requestBody = $this->bodyBuilder();
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  curl_setopt($ch, CURLOPT_POST, 1);
  curl_setopt($ch, CURLOPT_POSTFIELDS, $requestBody);
  curl_getinfo($ch);
  $response = curl_exec($ch);
  curl_close($ch);
  return $response;
 }
 //getters and setters
 public function setAction($action){
  $this->action = $action;
 }
 public function getAction(){
  return $this->action;
 }
 public function setDedupeBy($lookupField){
  $this->dedupeBy = $lookupField;
 }
 public function getDedupeBy(){
  return $this->dedupeBy;
 }
 public function addLeads($input){
  if($this->input != null){
   if (is_array($input)){
    foreach($input as $item){
     array_push($this->input, $item);
    }
   }else{
    array_push($this->input, $input);
   }
  }else if (is_array($input)){
   $this->input = $input;
  }else{
   $this->input = [$input];
  }
 }
 public function getInput(){
  return $this->input;
 }
 public function setAsyncProcessing($asyncProcessing){
  $this->asyncProcessing = $asyncProcessing;
 }
 public function getAsyncProcessing() {
  return $this->asyncProcessing;
 }
 public function setPartitionName($partitionName){
  $this->partitionName = $partitionName;
 }
 public function getPartitionName() {
  return $this->partitionName;
 }
 
}
```

### Proces

Hier is een voorbeeld dat tot twee verkoopersverslagen leidt en hen associeert aan twee loodverslagen:

```php
<?php

require 'Auth.php';
require 'SalesPerson.php';
require 'Leads.php';

//Create an Auth object for authentication
$auth = new Auth("https://284-RPR-133.mktorest.com", "7f99bd49-0e0e-4715-a72a-0c9319f75552", "O5lZHhrQDfDKRhulY89IU42PfdhRSe6m");

//Compose new SalesPerson Records
$sales1 = new stdClass();
$sales1->externalSalesPersonId = "SalesPerson 1";
$sales1->email = "sales1@example.com";
$sales1->firstName = "Jane";
$sales1->lastName = "Doe";

$sales2 = new stdClass();
$sales2->externalSalesPersonId = "SalesPerson 2";
$sales2->email = "sales2@example.com";
$sales2->firstName = "John";
$sales2->lastName = "Doe";

//Compose lead records
$lead1 = new stdClass();
$lead1->email = "marketoDev@example.com";
//Add the external id of the desired salesperson
$lead1->externalSalesPersonId = $sales1->externalSalesPersonId;

$lead2 = new stdClass();
$lead2->email = "devBlog@example.com";
$lead2->externalSalesPersonId = $sales2->externalSalesPersonId;

//Create a new instance of SalesPerson to submit records
$salesPerson = new SalesPerson($auth, [$sales1, $sales2]);
$spResponse = $salesPerson->postData();
print_r($spResponse . "rn");
$json = json_decode($spResponse);

//Check for success on synching SalesPersons
if ($json->success){
 $leads = new Leads($auth, [$lead1, $lead2]);
 $leadsResponse = $leads->postData();
 print_r($leadsResponse . "rn");
}else{
 print_r("SalesPerson request failed with errors:rn");
 foreach ($json->errors as $error){
  print_r("code: " . $error->code . ", message: " . $error->message . "rn");
 }
}
```

Voor onze voorbeeldklassen, creëren wij enkel stdClass voorwerpen om onze verslagen te vertegenwoordigen SalesPerson en Lood die moeten worden gesynchroniseerd, met elk gewenst gebied dat als lid wordt toegevoegd. Nadat deze code is uitgevoerd, worden voor de leads <marketoDev@example.com> en <devBlog@example.com> de velden E-mail van de eigenaar van de lead, Voornaam eigenaar en Achternaam eigenaar van lead ingevuld, zodat ze de relevante tokens voor deze velden kunnen gebruiken en kunnen worden gefilterd met de relevante filters voor slimme lijsten.

### Auth, klasse

```php
<?php

class Auth{
 private $host;//host of the target Marketo instance
 private $clientId;//client Id
 private $clientSecret;//client secret
 private $token;//access_token
 private $expiry;
 
 function _construct($host, $clientId, $clientSecret){
  $this->host = $host;
  $this->clientId = $clientId;
  $this->clientSecret = $clientSecret;
 }
 public function getToken(){
  if (!isset($this->token) || $this->expiry > time()){
   $data = $this->getData();
   $this->expiry = time() + $data->expires_in;
   $this->token = $data->access_token;
   return $this->token;
  }else{
   return $this->token;
  }
 }
 private function getData(){
  $url = $this->host . "/identity/oauth/token?grant_type=client_credentials&client_id=" . $this->clientId . "&client_secret="
     . $this->clientSecret;
  $ch = curl_init($url);
  curl_setopt($ch,  CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('accept: application/json','Content-Type: application/json'));
  $response = curl_exec($ch);
  $json = json_decode($response);
  curl_close($ch);
  return $json;
 }
 public function getHost(){
  return $this->host;
 }
}
```

GEPast op _2015-08-21_ door _Kenny_

## Aanbevolen procedures voor API-gebruikers en aangepaste services

Marketo REST API&#39;s gebruiken aangepaste services voor verificatie en elk van deze services is eigendom van een Marketo-gebruiker die alleen voor API werkt. De mogelijkheden van elke douanedienst worden bepaald door de toestemmingen van elke rol die aan die gebruiker wordt toegewezen. Het toewijzen van individuele gebruikers en de douanediensten aan uw integratie geeft u veelvoudige voordelen:

* U kunt de [ toestemmingen ](/help/rest-api/custom-services.md) verfijnen die aan elke individuele dienst door de rol wordt gegeven aan uw gebruiker.
* U kunt de individuele Webdiensten van het maken vraag aan uw instantie onbruikbaar maken door de overeenkomstige douanedienst te schrappen, zonder anderen onbruikbaar te maken.
* De rapportering over het gebruik van de API vraag zal door gebruiker worden verdeeld, toestaand u om hoog en abnormaal gebruik te identificeren
* Het is gemakkelijker om te bepalen tot welke gegevens elke Webdienst toegang heeft
* Met Workspace compatibele instanties kunnen de toegang tot specifieke bedrijfseenheden beperken door alleen rollen toe te wijzen aan toegankelijke werkruimten

### API-gebruik

Elk van uw API-gebruikers wordt afzonderlijk vermeld in het API-gebruiksrapport, dus als u uw webservices opgesplitst naar gebruiker, kunt u gemakkelijk rekenschap geven van het gebruik van elk van uw integratie. Als het aantal API vraag aan uw instantie de grens overschrijdt en verdere vraag veroorzaakt om te ontbreken, staat het gebruiken van deze praktijk u toe om voor het volume van elk van uw diensten rekenschap te geven en laat u evalueren hoe te om de kwestie op te lossen. Zie uw gebruik door naar Admin -> de Diensten van het Web te gaan en op het aantal vraag in de afgelopen 7 dagen te klikken.

### Een service uitschakelen

Als een integratie ongewenste gevolgen heeft, kan het vervelend en moeilijk zijn om onbruikbaar te maken als u niet elk een individuele douanedienst hebt toegewezen. Als u deze een voor een hebt uitgesplitst, kunt u de service in uw beheerprogramma -> Launchpoint net zo eenvoudig verwijderen als de service in kwestie.

### Workspace Management

Voor de abonnementen van de Onderneming van Marketo, is het gemeenschappelijk voor de dienst om toegang tot één enkele werkruimte slechts te vereisen, en dit kan [ door roltaak aan de API Gebruiker ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/allow-user-access-to-a-workspace) worden afgedwongen. Elke gebruikersrol kan of globaal, of op een per-werkruimtasebasis worden toegewezen, zodat kan de toegang in werkruimten waar aangewezen worden beperkt, die de zo minimaal mogelijke reeks toestemmingen verstrekken.

GEPast op _2015-08-28_ door _Kenny_

## Hoe te om de Gedeelten van de Lood te specificeren die REST APII gebruiken

**Lood het Partitioneren van de Lood** de Verdelingen van de Lood van Marketo verstrekken een geschikte manier om lood te isoleren. Met partities kunnen verschillende marketinggroepen binnen uw organisatie één Marketo-exemplaar delen. Voor meer informatie, zie [ Begrijpend Werkruimten en de Verdelingen van de Lood ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/workspaces-and-person-partitions/understanding-workspaces-and-person-partitions). Stel dat u hoofdpartities gebruikt en via de Marketo REST-API leads maakt. Hoe zorgt u ervoor dat de leads die u maakt, in de juiste partitie terechtkomen? In dit artikel wordt getoond hoe. In het belang van dit voorbeeld gebruiken we Workspaces en Partitions om onze leads te isoleren op basis van geografie.

Eerst definiëren we een werkruimte met de naam &#39;Land&#39;. Vervolgens maken we binnen die werkruimte twee partities met de naam &quot;Mexico&quot; en &quot;Canada&quot;.  **creeer Lood in Verdeling** veronderstel nu dat wij twee lood in de &quot;Mexico&quot;verdeling willen tot stand brengen. Om leads te creëren noemen we de. Om de verdeling te specificeren, moeten wij het &quot;partitieName&quot;attribuut in het verzoeklichaam omvatten. Hoe weten wij wat om voor de waarde te gebruiken partitionName? Wij kunnen een lijst van geldige waarden van de verdelingsnaam voor onze instantie terugwinnen door [ te roepen krijgt de Verdelingen van de Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeProgramMemberUsingGET) API als volgt:

`GET /rest/v1/leads/partitions.json`

```json
{
    "requestId": "20ae#14f9a1a5a30",
    "result": [
        {
            "id": 1,
            "name": "Default",
            "description": "Initial system lead partition"
        },
        {
            "id": 2,
            "name": "Mexico",
            "description": "Leads that live in Mexico"
        },
        {
            "id": 3,
            "name": "Canada",
            "description": "Leads that live in Canada"
        }
    ],
    "success": true
}
```

In dit geval is de juiste waarde &quot;Mexico&quot;, dus geven we die als volgt door aan Create/Update Leads:

`POST /rest/v1/leads.json`

```json
{
    "input": [
        {"email":"enrique.pena-nieto@gob.mx"},
        {"email":"angelica.rivera@gob.mx"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Hier zijn onze nieuwe leads in Marketo.  **Lood van de Update in Verdeling** om bestaande lood in een verdeling bij te werken, roepen wij eenvoudig Create/Update Leads en specificeren partitieName het zelfde zoals wij vroeger deden. Voor deze update wijzen we voornaam, achternaam en titel toe aan de leads die we hierboven hebben gemaakt.

`POST /rest/v1/leads.json`

```json
{
"input": [
        {"email":"enrique.pena-nieto@gob.mx", "firstName":"Enrique", "lastName":"Pena Neito", "title": "El Presidente"},
        {"email":"angelica.rivera@gob.mx", "firstName":"Angelica", "lastName": "Rivera", "title": "Primera Dama"}
    ],
    "action":"createOrUpdate",
    "partitionName":"Mexico"
}
```

Hier zijn onze onlangs bijgewerkte leads in Marketo.
**identificeer Verdeling voor een Lood** Hoe weten wij welke verdeling dat een lood binnen is? Voor dit gebruiken wij [ krijgen lood door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadByIdUsingGET) API en specificeren &quot;leadPartitionId&quot;in de &quot;gebieden&quot;vraagparameter. In dit geval zullen wij de informatie voor loodidentiteitskaart 318816 terugwinnen die wij hierboven creeerden.

`GET /rest/v1/lead/318816.json?fields=leadPartitionId,email,firstName,lastName,title`

```json
{
    "requestId": "5c57#14f9a495b1f",
    "result": [
        {
            "id": 318816,
            "lastName": "Pena Neito",
            "title": "El Presidente",
            "email": "enrique.pena-nieto@gob.mx",
            "firstName": "Enrique",
            "leadPartitionId": 2
        }
    ],
    "success": true
}
```

Merk op dat wij terug leadPartitionId eerder dan partitionName krijgen. Om partitieName te krijgen moeten we de uitvoer van de Get Lead Partitions API van bovenaf opnieuw bekijken.

```json
{
    "id": 2,
    "name": "Mexico",
    "description": "Leads that live in Mexico"
},
```

In dit geval is een leadPartitionId-waarde van 2 toegewezen aan de partitieName van &quot;Mexico&quot;. Dat is allemaal voor nu. Fijne partitionering!

Gepost op _2015-09-04_ door _David_

## Score-velden vergelijken in Marketo-e-mailscripts

**Nota:** dit is een gastpost door Cathal Moran. Cathal is een Solutions Consultant, die werkt vanuit het Marketo EMEA Office in Dublin, Ierland.

### Score-velden vergelijken

Veel Marketo-klanten, met name klanten die zich richten op cross-selling, hebben meerdere scorevelden en dit wordt vaak gebruikt om de interesse van een lead in een bepaald product/gebied te meten. Stel je voor dat ik appels en bananen verkoopt. Als een lead een score van 50 heeft voor appels en 10 voor bananen, dan is het duidelijk waar de voorkeur ligt, zou het niet leuk zijn als mijn inhoud deze voorkeur weerspiegelt. E-mailscripting kan worden gebruikt om scores te vergelijken en om inhoud in een e-mail te personaliseren afhankelijk van welke score het hoogst (of laagste) is voor die bepaalde lead die de e-mail ontvangt.

### Terwijl ik twee getallen wil vergelijken, moet ik mijn veldwaarden omzetten in gehele getallen

```
#set ($score1 = $math.toInteger(${lead.Apple_Score})) 
#set ($score2 = $math.toInteger(${lead.Banana_Score})) 
##check if the lead score is greater than feature score
#if($score1 >= $score2)
                ##if Apple score is greater
                #set($Interest = "Special offer on Apples")
##check is the feature score is higher
#elseif($score2 >= $score1)
                ##if Feature score is greater
                #set($Interest = "Special offer on Bananas")
#else
                ##otherwise
                #set($Interest = "Special offer on Fruit")
#end
##display the Interest as content
${Interest}
```

In het bovenstaande voorbeeld personaliseer ik alleen tekst, maar ik kon net zo gemakkelijk bijvoorbeeld een andere afbeelding weergeven op basis van de hogere score.

Gepast op _2015-09-14_ door _Kenny_

## Een Marketo-formulierverzending maken op de achtergrond

Als uw organisatie veel verschillende platforms heeft voor het hosten van webinhoud en klantgegevens, wordt het redelijk gebruikelijk om parallelle verzendingen van een formulier nodig te hebben, zodat de resulterende gegevens op verschillende platforms kunnen worden verzameld. Er zijn verschillende strategieën om dit te doen, maar de beste is vaak de eenvoudigste: de Forms 2 API gebruiken om een verborgen Marketo-formulier te verzenden. Dit werkt met elk nieuw Marketo-formulier, maar bij voorkeur moet u hiervoor een leeg formulier maken, dat geen velden heeft. Zo zorgt u ervoor dat het formulier niet meer gegevens laadt dan nodig is, omdat we niets hoeven te renderen. Behaal nu enkel [ bed code ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) van uw vorm in en voeg het aan het lichaam van uw gewenste pagina toe, makend een kleine wijziging. Uw insluitcode bevat een formulierelement zoals:

`<form id="mktoForm_1068"></form>`

U wilt &#39;style=&quot;display:none&quot; aan het element toevoegen zodat het niet zichtbaar is, zoals hier:

`<form id="mktoForm_1068" style="display:none"></form>`

Als het formulier eenmaal is ingesloten en verborgen, is de code voor het verzenden van het formulier heel eenvoudig:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"test@example.com",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms heeft op deze manier een formulier ingediend dat precies hetzelfde gedrag vertoont als wanneer de lead was ingevuld en een zichtbaar formulier heeft ingediend. Het activeren van de verzending varieert per implementatie, aangezien elke toepassing een andere actie heeft om de verzending te vragen, maar u kunt deze actie in feite uitvoeren op elke actie. Het belangrijkste onderdeel is het correct instellen van uw velden en waarden. Ben zeker om de SOAP API naam van uw gebieden te gebruiken die u met [ Namen van het Gebied van de Uitvoer ](/help/rest-api/list-of-standard-fields.md) kunt vinden om correcte voorlegging van waarden te verzekeren.

Migreren van Munchkin Associate Lead

Het verzenden van achtergrondformulieren is een van de aanbevolen vervangingsmethoden voor Munchkin Associate Lead. De HTML-voorbeeldpagina hieronder laat zien hoe u op een hoog niveau kunt migreren door dezelfde waarden opnieuw te gebruiken als die welke zijn ingediend bij Associate Lead.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName('head')[0].appendChild(s);
        })();
        </script>
</head>

<body> 
  <!--
    Start Embed code.  
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);
            
            //submit the form
            form.submit();
            
            
        })
    </script>
</body>
</html>
```

Gepast op _2015-09-25_ door _Kenny_

## Uitzondering en foutafhandeling van Marketo REST API: Deel 1

De Marketo REST API kan een uitzondering of fout retourneren, die we, voor het gemak, hier alleen maar fouten aanroepen. Fouten kunnen zich op drie verschillende niveaus voordoen:

* HTTP-niveau, deze fouten worden aangegeven door een 4xx-code
* Responsniveau, deze fouten worden opgenomen in de &quot;fouten&quot;-array van de JSON-respons
* Recordniveau, deze fouten worden opgenomen in de &quot;result&quot;-array van de JSON-respons en worden op individuele recordbasis aangegeven met het veld &quot;status&quot; en de array &quot;reason&quot;

### HTTP-fouten

Onder normale bedrijfsomstandigheden zou Marketo slechts twee HTTP- statusfouten, 413 Verzoek om Entiteit te Groot, en 414 Verzoek URI te Lang moeten terugkeren. Deze zijn zowel terugwinbaar door de fout te vangen, het verzoek te wijzigen en opnieuw te proberen, maar met slimme codeerpraktijken, zou u deze nooit in het wild moeten ontmoeten. Marketo zal 413 terugkeren als het Loodje van het Verzoek 1 MB overschrijdt, of 10MB in het geval van [ Lood van de Invoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Import-Leads). In de meeste gevallen is het onwaarschijnlijk dat deze limieten worden overschreden, maar door een controle toe te voegen aan de grootte van het verzoek en records te verplaatsen die ertoe leiden dat de limiet wordt overschreden naar een nieuw verzoek, moet worden voorkomen dat omstandigheden die tot deze fout leiden, door eindpunten worden geretourneerd. 414 wordt geretourneerd wanneer de URI van een GET-aanvraag groter is dan 8KiB. Om het te vermijden, controleer tegen de lengte van uw vraagkoord om te zien of overschrijdt het deze grens. Als het uw verzoek in een methode van de POST verandert, dan voer uw vraagkoord als verzoeklichaam met de extra parameter &#39;_method=GET&#39; in. Hiermee wordt de beperking op URI&#39;s genegeerd. Het is zeldzaam om deze grens in de meeste gevallen te raken, maar het is enigszins gemeenschappelijk wanneer het terugwinnen van grote partijen verslagen met lange individuele filterwaarden zoals een GUID.

### Fouten op responsniveau

Fouten in het responsniveau zijn aanwezig wanneer de parameter &quot;success&quot; van de reactie is ingesteld op false. Elk item in &#39;errors&#39; heeft twee leden, &#39;code&#39; een getal, 6xx of 7xx, en een &#39;message&#39; die de plaintext reden voor de fout geeft. 6xx-codes geven altijd aan dat een aanvraag volledig is mislukt en niet is uitgevoerd. Een voorbeeld van dit is 601, &quot;ongeldige het teken van de Toegang,&quot;dat terugwinbaar door het nieuwe toegangstoken met het verzoek opnieuw voor authentiek te verklaren en over te gaan is. 7xx fouten wijzen erop dat het verzoek ontbrak, of omdat geen gegevens werden teruggekeerd, of het verzoek verkeerd geparametereerd was, zoals het omvatten van een ongeldige datum, of het missen van een vereiste parameter.

### Fouten op recordniveau

Fouten op recordniveau geven aan dat een bewerking niet kan worden voltooid voor een afzonderlijke record, maar dat het verzoek zelf geldig was. Deze komen binnen individuele verslag in de &quot;resultaat&quot;serie van een reactie voor. Het veld status van deze records wordt overgeslagen en er is een array &#39;reason&#39; aanwezig. Elke reden bevat een &quot;code&quot;lid, en een &quot;bericht&quot;lid. De code zal altijd 1xxx zijn, en het bericht zal erop wijzen waarom het verslag werd overgeslagen. Een voorbeeld zou zijn waar een Create/Update Leads verzoek &quot;actie&quot;plaatste aan &quot;createOnly&quot;heeft maar een lood reeds voor één van de sleutels in de voorgelegde verslagen bestaat. Deze kwestie keert een code van 1005 terug, en een bericht van &quot;Lood bestaat reeds.&quot; In de volgende paar posts bekijken we herstelbare fouten en enkele voorbeelden van hoe deze in de code verwerkt kunnen worden.

Gepast op _2015-10-09_ door _Kenny_

## Gastpost - SQL-connectors rechtstreeks naar de Marketo-database

**dit is een gastpost van Sumit Sarkar van Voortgang, Inc.**

**SQL de schakelaars aan het gegevensbestand van Marketo** Marketo heeft goed gedocumenteerde APIs voor de toegang tot van gegevens. Soms hebben organisaties echter directe SQL-toegang nodig. We zien de vervaging in Marketo-winkels tussen Marketing en IT, waardoor de vraag naar op standaarden gebaseerde SQL-connectiviteit toeneemt. De directe SQL schakelaars aan Marketo zijn beschikbaar door [ de Cloud van de Voortgang DataDirect ](https://www.progress.com/connectors/cloud-data-integration). DataDirect Cloud is een service voor gegevensconnectiviteit die Marketo-gegevens toegankelijk maakt via open industriestandaarden voor SQL-toegang via ODBC (&quot;Open Database Connectivity&quot;) of JDBC (&quot;Java Database Connectivity&quot;) en REST with OData (&quot;Open Data Protocol&quot;). Hieronder vindt u een aantal veelgebruikte gebruiksscenario&#39;s voor het offline verbinden van Marketo-gegevens met DataDirect Cloud:

* Gegevensdetectie en -visualisatie (Qlik, Tableau, SAP Lumira)
* Bedrijfsrapportage (SAP Business Objects, Microstrategy, Cognos)
* Gegevensintegratie (SQL Server Integration Services - SSIS, Oracle Data Integrator - ODI, Informatica)
* Gegevensfederatie (SQL Server Linked Server, SAP Hana SDA, Oracle Database Gateway)
* Ad hoc Vraag (Microsoft Office, DB Visualizer, Aqua Data Studio)
* Gegevensvoorbereiding (Alteryx, Trifecta, Paxata)

**hoe werkt SQL toegang tot Marketo?**

* DataDirect Cloud maakt een logisch schema voor gegevens die worden vrijgegeven door integratie-API&#39;s van Marketo.
* DataDirect Cloud verwerkt SQL-aanvragen van lichtgewicht ODBC- of JDBC-clients met behulp van een elastische real-time query-engine voor gegevens die zijn opgehaald van de Marketo API&#39;s.
* De DataDirect Cloud-connectiviteit is direct en realtime zonder dat gegevens worden gedupliceerd.
* DataDirect Cloud Service abstraheert de Marketo API zodanig dat eindgebruikers zich geen zorgen hoeven te maken over wijzigingen in de API-versie, of SOAP gebruiken in plaats van REST.

DataDirect is al sinds 2006 bezig met het ontwikkelen van deze connectiviteitsstijl voor SaaS-gegevensbronnen, te beginnen met het eerste Salesforce ODBC-stuurprogramma dat bovenop de webservice-API&#39;s wordt gebouwd. **Beginnen verbindend met Marketo:**

1. [ Register voor een Login van de Wolk DataDirect ](https://pacific.progress.com/console/register?productName=d2c&amp;ignoreCookie=true)
1. Klik op &quot;Gegevensbronnen&quot; en vervolgens op de knop &quot;+New Data Source&quot;
1. Selecteer Marketo en voer de verbindingsgegevens in. U kunt met uw beheerder van Marketo of login controleren om [ verbindingsinformatie voor de integratie van SOAP ](/help/soap-api/soap-api.md) te vinden.
1. Klik op de knop Verbinding testen. Let op: er is een OData tab om OData uit Marketo te produceren en we zullen het in een toekomstig blogbericht bespreken.
1. Klik op &quot;SQL Testing&quot; als u het Marketo-schema wilt inspecteren of standaard SQL query&#39;s wilt geven vanuit de gebruikersinterface.
1. Klik op &quot;Downloads&quot; links en selecteer het ODBC- of JDBC-stuurprogramma voor DataDirect Cloud voor uw toepassing en platform.
1. Nadat het ODBC- of JDBC-stuurprogramma voor DataDirect Cloud is geïnstalleerd, kunt u op standaarden gebaseerde toepassingen verbinden met Marketo.

Hier is een videovoorbeeld van [ verbindend het gebruiken van de cliënt DataDirect van de Wolk ODBC ](https://www.youtube.com/watch?v=H6PHra56Iig). Hier volgen nog andere zelfstudies voor DataDirect Cloud die van toepassing zijn op Marketo:

* [ Analytics van de Gegevens van SAP ](http://scn.sap.com/community/lumira/blog/2015/08/05/connect-sap-lumira-to-eloqua-marketo-google-analytics)
* [ Meldende van de Onderneming van de Microstrategie ](https://community.microstrategy.com/t5/Tech-Corner/What-MSTR-developers-should-know-about-Cloud-Data-Sources/ba-p/253083)
* [ de Integratie van Gegevens van Oracle ](https://www.ateam-oracle.com/post/a-universal-cloud-applications-adapter-for-odi/)

**O&amp;O uitdagingen bouwend SQL connectiviteit over wolkenbronnen zoals Marketo**

Niet alle SaaS APIs stelt een standaardvraagtaal bloot. In die gevallen bekijkt het engineeringteam elk object afzonderlijk. Elk object kan worden blootgesteld aan een andere API met unieke regels voor het aanroepen, zoeken en filteren. Het vereiste een significante inspanning om een standaardervaring te verstrekken die over het volledige gegevensmodel vraagt.

Afhandeling van volledige verbindingsmogelijkheden. In gevallen waar SaaS APIs geen vraagtaal met vermogen JOIN steunt, moet het technische team die verrichting uitvoeren. Dit vereist een vertaling van SQL om Marketo APIs efficiënt te roepen om de minimale hoeveelheid gegevens terug te keren alvorens uit te voeren toetreedt. Wanneer twee zeer grote objecten worden samengevoegd, kan de laag voor gegevenstoegang aanzienlijke bronnen op de toepassingsserver of desktop gebruiken. Daarom is de implementatie van de laag voor gegevenstoegang tot een elastische cloudservice zoals DataDirect Cloud om twee redenen erg logisch:

1. Snellere prestaties en minder geheugen/CPU-bronnen op de client-toepassingsserver of -desktop
1. Maak gebruik van de superieure bandbreedte tussen DataDirect Cloud en Marketo, waar vooraf gekoppelde gegevenssets worden uitgewisseld.

Hoe te om gegevensmodellen te behandelen? Is het statisch of dynamisch? Hoe worden wijzigingen gedetecteerd en aan de client doorgegeven? Elke SaaS-gegevensbron is anders en in het geval van Marketo kunnen bepaalde objecten beter worden gecontroleerd door weergaven en andere door tabellen. Het afhandelen van deze matrix van gegevensmodellen en -objecten in alle SaaS-bronnen was zeker een uitdaging.

**Verwijzing van Marketo en van de Wolk DataDirect voor Ontwikkelaars:**

* Eigenschappen van de Verbinding van Marketo ([ verbinding aan documenten ](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Ondersteunde SQL en uitbreidingen met Marketo ([ verbinding aan docs ](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html,-hubspot,-and-marketo.html))
* De blootgestelde Lijsten en de Meningen van Marketo ([ verbinding aan docs ](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))
* Gemeenschappelijke foutenmeldingen die van Marketo ([ worden teruggekeerd verbinding aan docs ](https://www.progress.com/output/DataDirect/DataDirectCloud/index.html))

**Biografie:** Sumit Sarkar is een Chief Data Evangelist bij Voortgang, met meer dan 10 jaar ervaring die op het gebied van de gegevensconnectiviteit werkt. De toonaangevende consultant van de wereld op het gebied van open gegevensstandaarden voor connectiviteit met cloudgegevens, Sumit&#39;s interesses omvatten het afstemmen van de prestaties van de gegevenstoegangslaag waarvoor hij een octrooi in behandeling heeft voor zijn analyse; bedrijfsintelligentie en gegevensopslag voor SaaS-platforms; en gegevensconnectiviteit voor PaaS-omgevingen, met nadruk op standaarden zoals ODBC, JDBC, ADO.NET en ODATA. Hij is een IBM Certified Consultant voor IBM Cognos Business Intelligence en een TDWI-lid. Hij heeft sessies gepresenteerd over gegevensconnectiviteit op diverse conferenties, onder andere op Dreamweaver, Oracle OpenWorld, Strata Hadoop, MongoDB World en SAP Analytics and Business Objects Conference. Twitter: @SAsInSumit LinkedIn: [ www.linkedin.com/in/meetsumit](http://www.linkedin.com/in/meetsumit)

Gepast op _2015-12-07_ door _Kenny_

## Een dashboard maken voor gebruik van de API en voor aantal fouten

Als Marketo API-consument is dit nuttige informatie die u in de gaten moet houden. Wat als u historische gebruiksgegevens kon krijgen helpen tendensen in tijd ontdekken? Wat gebeurt er als u een overzicht van API-foutcodes kunt krijgen om de status van uw integratie te meten? Als Marketo Technology Partner, wat als u gebruik en foutengegevens over al uw klantenrekeningen in één dashboard kon krijgen? Dit artikel zal een benadering bieden voor het beantwoorden van bovenstaande vragen. Sluiten, hier gaan we.
**Gepland Baan voor Stats wint terug** maak een app die gebruik en foutengegevens terugwint gebruikend [ krijgt Dagelijks Gebruik ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLast7DaysErrorsUsingGET) en [ krijgt Dagelijkse Fouten ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyErrorsUsingGET) eindpunten. De app is ontworpen om één keer per dag te worden uitgevoerd. Elke keer dat de app wordt uitgevoerd, worden er gebruiksgegevens van een dag aan het ene bestand en gegevens van een dag aan een ander bestand toegevoegd. Aan het begin van elke maand wordt een nieuw paar bestanden gemaakt. Deze bestanden dienen als een historisch record dat we op elk gewenst moment kunnen openen. Dit is de toepassingslogica..

* Lees Marketo-accountgegevens (Munchkin-id en clientgegevens) van een externe bron. Opmerking: deze bron moet veilig zijn om te voorkomen dat anderen toegang krijgen tot accountgegevens.
* Doorlopen op elke account en...
   * Vraag krijgt Dagelijks Gebruik om gebruiksgegevens voor één dag terug te winnen
   * Dagelijkse gebruiksgegevens toevoegen aan een maandelijks &quot;gebruiksbestand&quot;
   * De vraag krijgt Dagelijkse Fouten van de Vraag om foutengegevens voor één dag terug te winnen
   * Dagelijkse foutgegevens toevoegen aan een maandelijks bestand met &#39;fouten&#39;

Uitvoerbestandsindeling De indeling voor de uitvoerbestanden is JSON die overeenkomt met de array &#39;result&#39; die wordt geretourneerd van de respectievelijke API-aanroepen (Gebruik en Fout). Elk element van de array &#39;result&#39; is een JSON-object dat gegevens voor één dag bevat. Uitvoerbestandsnaam De uitvoerbestanden krijgen de volgende naam:

`<type>_<yyyy>_<mm>_<account>.json`

Wanneer

`<type>` - Het type gegevens (&quot;gebruik&quot; of &quot;fouten&quot;) `<yyyy>` - Het jaar (4 cijfers) `<mm>` - De maand (2 cijfers) `<account>` - De account-id (Munchkin-id)

Uitvoerbestand voorbeelden_2015_10_111-AAA-222.json

```json
[ 
    { "date": "2015-10-15", "total": 0, "users" : [] }, 
    { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] }, 
    { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] },
]
```

`errors_2015_10_111-AAA-222.json:`

```json
[
    { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
    { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
    { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
]
```

De code voor deze app wordt weergegeven aan het einde van dit bericht (Stats.java). **de Dienst van het Web van Staten** zo nu hebben wij een manier nodig om deze gegevens in onze browser te krijgen. Het voorstel is om een webservice op te zetten voor de levering van de gegevens. De webservice gebruikt de uitvoerbestanden van de app en stuurt gegevens terug naar de browser in een formulier dat gemakkelijk kan worden weergegeven. Voor het belang van eenvoud, en om rond de beperkingen van het zelfde-oorsprongbeleid te krijgen, zullen wij hefboomwerking het [ JSONP ](https://en.wikipedia.org/wiki/JSONP) patroon. Hier is de voorgestelde REST eindpuntspecificatie voor de externe Webdienst: **URI**: /stats **Methode**: GET

**Parameter**

**Beschrijving**

**Voorbeeld**

maand

Haal de gegevens voor deze maand op. Lijst met maanden die door komma&#39;s worden gescheiden (2-cijferige weergave). Standaard voor alle maanden.

10,11

jaar

Hiermee haalt u de gegevens voor dit jaar op. Lijst met jaren die door komma&#39;s worden gescheiden (4-cijferige weergave). Standaard voor alle jaren.

2015

account

Haal gegevens voor dit account op (Munchkin-id).

111-AAA-222

callback

Naam van de functie waarmee JSON-inhoud moet worden omsloten.

processStats

Voorbeeld-aanvraag

`GET //localhost:8080/stats?month=10&year=2015&account=111-AAA-222&callback=processStats`

De webservice leest &#39;usage&#39;- en &#39;error&#39;-bestanden en combineert deze bestanden en retourneert ze in deze indeling:


```html
**<Name of Callback here>**

<Contents of Usage file here>,

<Contents of Error file here>
```

Dit is een JSONP callback met 2 argumenten. Het eerste argument is de &#39;result&#39;-array voor gebruik. Het tweede argument is de &#39;result&#39;-array met fouten. Voorbeeld-reactie

```json
processStats(
    [
        { "date": "2015-10-15", "total": 0, "users" : [] },
        { "date": "2015-10-16", "total": 9, "users": [ { "userId": "some.body@yahoo.com", "count": 9 } ] },
        { "date": "2015-10-17", "total": 1120, "users": [ { "userId": "some.body@yahoo.com", "count": 200 }, { "userId": "some.body@marketo.com", "count": 200 }, { "userId": "some.body@gmail.com", "count": 720 } ] }
    ],
    [
        { "date": "2015-10-15", "total":80, "errors": [ { "errorCode":"1003", "count":80 } ] },
        { "date": "2015-10-16", "total":148, "errors": [ { "errorCode":"612", "count":40 }, { "errorCode":"609", "count":70 }, { "errorCode":"1008", "count":38 } ] },
        { "date": "2015-10-17", "total":73, "errors": [ { "errorCode":"604", "count":1 }, { "errorCode":"609", "count":56 }, { "errorCode":"610", "count":16 } ] },
    ]
);
```

Zoals u ziet, heeft de webservice de inhoud van de twee uitvoerbestanden vanuit onze app verpakt. Wij hebben tot een mock reactie van de Webdienst geleid gebruikend [ Mocky ](https://designer.mocky.io). Een voorbeeld van de Webdienst is het mock [ hier.](http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&amp;year=2015&amp;account=111-AAA-222&amp;callback=processStats) De verwezenlijking van deze Webdienst wordt verlaten als oefening voor de lezer: **Web-pagina van het Dashboard** zo nu al is wij een Web-pagina die onze Webdienst roept en de gegevens formatteert. Als u het JSONP-patroon wilt gebruiken, moet u gewoon een `<script>` -tag toevoegen die de webservice aanroept:

`<script src="http: //<hostname>/stats?month=10&year=2015&account=284-RPR-133&callback=processStats"></script>`

Hiermee injecteert u de reactie van de webservice rechtstreeks op de HTML-pagina. Daarna voegen wij de callback functie JSONP toe:

```javascript
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
;
```


Deze functie wordt automatisch aangeroepen na het aanroepen van de webservice. In dit voorbeeld, roepen wij een eenvoudige &quot;veranderlijke dumper van JavaScript&quot;genoemd [ prettyPrint.js ](https://github.com/padolsey-archive/prettyprint.js/tree/master) op elke serie. De prettyPrint functie produceert eenvoudig een HTML lijst gebruikend de inhoud van de serie. Hier is een schermafbeelding van de HTML-tabellen. Voilà, dat is ons dashboard! Toegegeven, dit is niet erg elegant, maar het zou je een idee moeten geven van wat mogelijk is. Niets weerhoudt je ervan om de gegevens te transformeren op een manier die je zelf opvallende visualisaties maakt. De HTML-pagina bevindt zich onder (Index.html) U kunt de bovenstaande tabellen live in uw browser weergeven door de volgende stappen uit te voeren:

1. Een lokale kopie van Index.html opslaan
1. Een lokale kopie van prettyPrint.js opslaan
1. Index.html openen in uw browser

Dat is het. Hopelijk heeft dit bericht u enkele ideeën gegeven over het controleren van uw Marketo API-status. Fijne codering! **Stats.java**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;

import java.io.\*;
import java.lang.reflect.Array;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.\*;

import java.nio.file.Files;
import java.nio.file.Paths;

import javax.net.ssl.HttpsURLConnection;

public class Stats {
    //
    // Define Marketo instance meta data here. Each row contains three elements: Account Id, Client Id, Client Secret.
    //
    // Note: that this information would typically be stored read from an external source (i.e. database)
    //
    // For example:
    //
    // private static String final CUSTOM_SERVICE_DATA[][] = {
    // {"111-AAA-222", "2f4a4435-f6fa-4bd9-3248-098754982345", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"222-BBB-333", "5f4a6657-f6fa-4cd9-4356-123083238821", "gfjgfIVE9h4Jjcl59cOMAKFSk78ut12W"},
    // {"444-CCC-444", "9f4a4678-f6fa-4dd9-7735-908713247721", "xzcxvbVE9h4Jjcl59cOMAKFSk78ut12W"}
    // };
    //
    private static final String CUSTOM_SERVICE_DATA[][] = {
    };

    // Output directory for stats files
    private static final String OUTPUT_DIR = "C:stats";

public static void main(String[] args) {

    // Loop through each Marketo instance
    for (int i = 0; i < CUSTOM_SERVICE_DATA.length; i++) {

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA[i][0]);

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
            baseUrl, "client_credentials", CUSTOM_SERVICE_DATA[i][1], CUSTOM_SERVICE_DATA[i][2]);

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(httpRequest("GET", identityUrl, null));
        String accessToken = identityObj.get("access_token").asString();

        // Compose Get Last 7 Days Usage URL
        String usageUrl = String.format("%s/rest/v1/stats/usage/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Compose Get Last 7 Days Errors URL
        String errorsUrl = String.format("%s/rest/v1/stats/errors/last7days.json?access_token=%s",
            baseUrl, accessToken);

        // Process usage data
        JsonObject usageObj = JsonObject.readFrom(httpRequest("GET", usageUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (usageObj.get("result") != null) {
                updateFile(usageObj, "usage", CUSTOM_SERVICE_DATA[i][0]);
            }
        }

        // Process errors data
        JsonObject errorsObj = JsonObject.readFrom(httpRequest("GET", errorsUrl, null));
        if (usageObj.get("success").asBoolean()) {
            if (errorsObj.get("result") != null) {
                updateFile(errorsObj, "errors", CUSTOM_SERVICE_DATA[i][0]);
            }
        }
    }
    System.exit(0);
}

// Write yesterday's data to output file
private static void updateFile(JsonObject usageObj, String statsType, String account){

    JsonArray usageResultAry = usageObj.get("result").asArray();
    JsonObject yesterdayUsageResultObj = usageResultAry.get(1).asObject();

    String yesterdayDate = yesterdayUsageResultObj.get("date").asString();
    String[] yesterdayDateAry = yesterdayDate.split("[-]+");

    // "C:statsstats_yyyy_mm_account.json"
    String statsFile = String.format("%s%s_%s_%s_%s.json",
        OUTPUT_DIR, statsType, yesterdayDateAry[0], yesterdayDateAry[1], account);

    // Create file
    File file = new File(statsFile);
    try {
        if (file.createNewFile()) {
            // created new file, seed with empty array
            FileWriter fw = new FileWriter(file.getAbsoluteFile());
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[n]");
            bw.close();
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Read file
    String content = null;
    try {
        content = new String(Files.readAllBytes(Paths.get(statsFile)));
    } catch (IOException e) {
        e.printStackTrace();
    }

    // Remove trailing "]", append new record, append trailing "]"
    content = content.substring(0, content.length() - 1);
    content += yesterdayUsageResultObj.toString();
    content += "n]";

    // Write file
    FileWriter fw = null;
    try {
        fw = new FileWriter(file.getAbsoluteFile());
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write(content);
        bw.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
}

// Perform HTTP request
private static String httpRequest(String method, String endpoint, String body) {
    String data = "";
    try {
        URL url = new URL(endpoint);
        HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
        urlConn.setDoOutput(true);
        urlConn.setRequestMethod(method);
        switch (method) {
            case "GET":
                break;
            case "POST":
                urlConn.setRequestProperty("Content-type", "application/json");
                urlConn.setRequestProperty("accept", "text/json");
                OutputStreamWriter wr = new OutputStreamWriter(urlConn.getOutputStream());
                wr.write(body);
                wr.flush();
                break;
            default:
                System.out.println("Error: Invalid method.");
                return data;
        }
        int responseCode = urlConn.getResponseCode();
        if (responseCode == 200) {
            InputStream inStream = urlConn.getInputStream();
            data = convertStreamToString(inStream);
        } else {
            System.out.println(responseCode);
            data = "Status:" + responseCode;
        }
    } catch (MalformedURLException e) {
        System.out.println("URL not valid.");
    } catch (IOException e) {
        System.out.println("IOException: " + e.getMessage());
        e.printStackTrace();
    }
    return data;
}

private static String convertStreamToString(InputStream inputStream) {
    try {
        return new Scanner(inputStream).useDelimiter("A").next();
    } catch (NoSuchElementException e) {
        return "";
    }
}
}
```

**Index.html**

```html
<html>
<head>
<title>Marketo API Stats</title>
<!-- Browser JavaScript variable dumper                  -->
<!-- https://github.com/padolsey-archive/prettyprint.js  -->
<script src="prettyPrint.js"></script>
</head>
<body>

<h1>Marketo API Stats</h1>

<script>
// JSONP callback that uses prettyPrint to format API stats
function processStats(usage, errors) {
    var cfg = { maxDepth: 5};
    document.write("<h2>Usage</h2>");
    document.body.appendChild(prettyPrint(usage, cfg));
    document.write("<h2>Errors</h2>");
    document.body.appendChild(prettyPrint(errors, cfg));
};
</script>

<!-- Web service for you to implement as an exercise                                                        -->
<!-- <script src="http://localhost:8080/stats?month=10&account=111-AAA-222&callback=processStats"></script> -->

<!-- Mock web service that returns sample payload -->
<!-- http://www.mocky.io/                         -->
<script src="http://www.mocky.io/v2/5627b2f9270000f2226eec63?month=10&year=2015&account=111-AAA-222&callback=processStats"></script>"
</body>
</html>
```

Gepost op _2015-10-22_ door _David_

## Uitzondering en foutafhandeling van Marketo REST API: Deel 3

Voor het grootste deel zullen de fouten die door de Marketo REST API worden ontvangen niet automatisch kunnen worden hersteld. Er zijn echter enkele gevallen waarin u automatisch kunt herstellen of waarin u nooit een bepaalde fout ziet.

### Fouten in aanvraaggrootte

Zoals we in de laatste post van deze reeks bekeken, zal Marketo HTTP Status code 414 uitzenden als uw URI 8KiB in lengte overschrijdt, of 413 als uw verzoeklichaam 1MB, of 10MB voor de Lood van de Invoer overschrijdt. Alhoewel 414s zeldzaam zal zijn, zou u hen kunnen zien als u Get lood door het type van Filter gebruikt om verslagen te verzoeken die op 300 afzonderlijke GUIDs of gelijkaardige criteria worden gebaseerd. Stel dat u het volgende verzoek hebt: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?filterType=customGUID&fields=email` , bedrijf...firstName, lastName> Wanneer u het verzoek verzendt, retourneert Marketo een status van 414 omdat de URI groter is dan 8KiB.

Om dit te behandelen, moeten wij het patroon van dit verzoek veranderen, en een POST in plaats van een GET voorleggen, voegt &#39;_method=GET&#39; aan URI toe, en gaat het vraagkoord in het verzoeklichaam als x-www-vorm-urlencoded verzoek in plaats daarvan over: URI: `<https://AAA-BBB-CCC.mktorest.com/rest/v1/leads.json?_method=GET>` Hoofdtekst van verzoek: filterType=customGUID&amp;fields=email,bedrijf...firstName, lastName in plaats van het vangen van deze uitzondering De reactie van HTTP, echter, kunnen wij de totale lengte van het verzoek bij runtime controleren, en stelt dit alternatieve patroon op als URI 8k overschrijdt. U kunt ook de POST-methode in alle gevallen gebruiken voor het ophalen van batches van records. Voor 413s, kunnen wij een gelijkaardig patroon volgen, controlerend de lengte van het verzoeklichaam wanneer het toevoegen van verslagen tijdens de rangschikkingsstap, en het splitsen van het verzoek in veelvoudige delen als deze grens zou worden overschreden.

### Verificatiefouten

Onze volgende klasse van terugwinnbare fouten is verwant met authentificatie. Wanneer een vroeger geldig toegangstoken wordt gebruikt nadat zijn period expired_in is verstreken, zal het eerste gebruik een foutencode van 602 terugkeren, &quot;het teken van de Toegang verliep.&quot; Daarna, gebruikend het zelfde teken zal 601 terugkeren, &quot;ongeldige het teken van de Toegang.&quot; Elk ander gebruik van een tekenreeks die geen geldige toegangstoken voor het doelabonnement is, resulteert in een 601-teken. In beide gevallen kan deze fout worden hersteld door het nieuwe toegangstoken opnieuw te verifiëren en door te geven met een nieuwe poging van het mislukte verzoek.

### Tijdstippen

In zeer zeldzame omstandigheden, kan een vraag een 604 terugkeren, &quot;Verzoek uit getimed,&quot;nadat de periode van 30 tweede onderbreking is verstreken. Voor gegroepeerde verzoeken, zoals Create/Update Leads, kan het verzoek in kleinere partijen worden verdeeld en opnieuw geprobeerd tot het succes is teruggekeerd (als de partij aan minder dan 100 verslagen wordt verdeeld en het verzoek nog wordt getimed, zou u waarschijnlijk een steungeval moeten indienen). Het gemeenschappelijkste andere geval is met activa goedkeuringsvraag, waar een slot op het huidige goedgekeurde verslag door een andere gebruiker of de dienst, zoals het geval van een [ E-mail ](https://developer.adobe.com/marketo-apis/api/asset/approve-email-by-id/) of [ E-mailMalplaatje ](https://developer.adobe.com/marketo-apis/api/asset/approve-email-template-by-id/) kan worden gehouden. In deze gevallen, [ exponentiële backoff ](https://en.wikipedia.org/wiki/Exponential_backoff) zou voor pogingen moeten worden gebruikt om voor om het even welke bestaande sloten toe te staan om worden opgelost. Bekijk in de komende weken het laatste deel van de reeks waar we nader zullen kijken naar bepaalde specifieke, niet-herstelbare fouten.

Gepast op _2015-10-30_ door _Kenny_

## Verbeterde Marketo-beveiliging

In Marketo nemen we veiligheid serieus. Als deel van een **[industrie-brede initiatief ](https://security.googleblog.com/2014/09/gradually-sunsetting-sha-1.html)**, bevordert Marketo onze Webauthentificatie en encryptie om onze veiligheidsbescherming te verbeteren. De introductie vindt plaats op 12 december 2011. **wie zal worden beïnvloed?** Slechts een klein aantal gebruikers zal worden beïnvloed, slechts degenen die een integratie aan Marketo van een systeem hebben dat meer dan tien jaar oud is en onlangs niet is bijgewerkt. Zie **[deze lijst ](https://www.digicert.com/faq/sha2/sha-2-compatibility)** voor meer informatie over welke systemen en versies worden gesteund. **de volgende gebruikers zullen niet worden beïnvloed:**

* Eindgebruikers die tot Marketo.com door moderne browsers toegang hebben ([ zie lijst ](https://www.digicert.com/faq/sha2/sha-2-compatibility))
* Klanten die gebruikmaken van integratiepartners zoals Informatica, Dell Boomi en Scribe.
* Klanten die partners gebruiken Launchpoint.

Alle andere Marketo-domeinen gebruiken al SHA2-certificaten.

Gepast op _2015-11-18_ door _Kenny_

## Opiniepeiling voor activiteiten met REST API

Activiteiten vormen een kernobject in het Marketo-platform. Activiteiten zijn de gedragsgegevens die worden opgeslagen over elke webpaginabezoek, elke e-mail die wordt geopend, elke webinar die aanwezig is of elke handelsshow. Een veelvoorkomend geval van gebruik is het combineren van activiteitengegevens met gegevens van andere delen van een organisatie. Dit voorbeeldprogramma bevat 6 stappen:

1. De vraag [ krijgt de Activiteiten van het Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) om een lijst van alle activiteitenverslagen te produceren die bij een bepaalde datum/tijd worden gecreeerd. We gebruiken een filter om het type activiteitenrecords te beperken dat wordt geretourneerd.
1. Velden van belang extraheren uit elke activiteitenverslag.
1. De vraag [ krijgt Veelvoudige Leidingen door het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadsByFilterUsingGET) om een lijst van loodverslagen te produceren die met de activiteiten van Stap 1 beantwoorden. Wij gebruiken het leadId gebied dat uit activiteitenverslag in Stap 2 wordt gehaald als onze filter om te specificeren welke lood zijn teruggekeerd.
1. Velden van belang extraheren uit elke loodrecord.
1. Sluit de activiteitsgegevens van stap 2 aan bij de loodgegevens van Stap 4.
1. Transformeer de gegevens van Stap 5 in een formaat dat door een extern systeem kan worden verbruikt.

Zoals uit het onderstaande diagram blijkt, hebben we er voor gekozen om e-mailgerelateerde activiteiten vast te leggen.

**Invoer van het Programma** door gebrek gaat het programma in tijd één dag van de huidige datum terug om veranderingen te zoeken. Je zou dit programma bijvoorbeeld elke dag op hetzelfde tijdstip kunnen uitvoeren. Om verder terug in tijd te gaan, kunt u het aantal dagen als argument van de bevellijn specificeren, effectief verhogend het venster van tijd. Het programma bevat verschillende variabelen die u kunt wijzigen: CUSTOM_SERVICE_DATA - Dit bevat uw Marketo Custom Service-gegevens (account-id, client-id, clientgeheim). READ_BATCH_SIZE - Dit is het aantal verslagen dat tegelijkertijd moet terugwinnen. Gebruik dit om de reactie op lichaamsomvang aan te passen. LEAD_FIELDS - Dit bevat een lijst van loodgebieden die wij willen verzamelen. ACTIVITY_TYPES - Dit bevat een lijst van activiteitentypes die wij willen verzamelen.

{de Logische 1} Logica van het 0&rbrace; Programma &lbrace;vestigen wij eerst ons tijdvenster, stellen ons eindpuntURL van REST samen, en verkrijgen ons toegangstoken van de Authentificatie. **&#x200B;**&#x200B;Vervolgens zetten we een &#39;Get Paging Token/Get Lead Activity&#39;-lus in werking, die loopt tot we de levering van activiteiten uitputten. Het doel van deze lus is activiteitsrecords op te halen en velden van belang uit die records te extraheren. Wij vertellen krijgen de Activiteiten van de Lood om slechts voor de volgende activiteitentypes te zoeken:

* E-mail bezorgd
* E-mailadres opzeggen
* E-mail openen
* Klik op E-mail.

Wij halen de volgende gebieden van belang uit elk activiteitenverslag:

* leadId
* activityType
* activityDate
* primaryAttributeValue

U kunt een willekeurige combinatie van activiteitstypen en activiteitsvelden selecteren die bij uw doel past. Daarna zetten we een Get Multiple Leads door de lijn van het Type van Filter in werking die loopt tot wij de levering van lood uitputten. Merk op dat wij de parameter &quot;filterType=id&quot;in combinatie met een reeks &quot;filterValues&quot;parameters gebruiken om de loodverslagen te beperken die aan slechts die lood worden teruggewonnen verbonden met activiteiten die wij vroeger terugwogen. Wij halen de volgende gebieden van belang uit elk loodverslag:

* firstName
* lastName
* email

Ook hier hebt u het gevoel om de gewenste loodvelden te selecteren. Vervolgens voegen we de hoofdvelden samen met de activiteitsvelden door ze aan elkaar te koppelen met behulp van de hoofd-id. Tot slot herhalen wij door alle gegevens, transformeren het in formaat JSON, en drukken het aan de console. **Output van het Programma** hier is een voorbeeld van output van het steekproefprogramma. Hierin worden de activiteitsvelden en de loodvelden samen weergegeven als JSON-&quot;result&quot;-objecten. Het idee hier is dat u deze JSON als verzoeklading aan een externe Webdienst kon overgaan.

```json
{
    "result": [
        {
            "leadId": 318581,
            "activityType": "Email Delivered",
            "activityDate": "2015-03-17T20:00:06Z",
            "primaryAttributeValue": "My Email Program",
            "firstName": "David",
            "lastName": "Everly",
            "email": "everlyd@marketo.com"
        },
        {
            "leadId":318581,
            "activityType":"Open Email",
            "activityDate":"2015-03-17T23:23:12Z",
            "primaryAttributeValue":"My Email Program - Auto Response",
            "firstName":"David",
            "lastName":"Everly",
            "email":"everlyd@marketo.com"
       },
        ... more result objects here...
    ]
}
```

**Code van het Programma**

```java
package com.marketo;

// minimal-json library (https://github.com/ralfstx/minimal-json)
import com.eclipsesource.json.JsonArray;
import com.eclipsesource.json.JsonObject;
import com.eclipsesource.json.JsonValue;

import java.io.\*;
import java.net.MalformedURLException;
import java.net.URL;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.\*;

import javax.net.ssl.HttpsURLConnection;

public class LeadActivities {
//
// Define Marketo REST API access credentials: Account Id, Client Id, Client Secret.  For example:
    private static Map<String, String> CUSTOM_SERVICE_DATA;
    static {
        CUSTOM_SERVICE_DATA = new HashMap<String, String>();
//        CUSTOM_SERVICE_DATA.put("accountId", "111-AAA-222");
//        CUSTOM_SERVICE_DATA.put("clientId", "2f4a4435-f6fa-4bd9-3248-098754982345");
//        CUSTOM_SERVICE_DATA.put("clientSecret", "asdf6IVE9h4Jjcl59cOMAKFSk78ut12W");
    }

    // Number of lead records to read at a time
    private static final String READ_BATCH_SIZE = "200";

    // Lookup lead records using lead id as primary key
    private static final String LEAD_FILTER_TYPE = "id";

    // Lead record lookup returns these fields
    private static final String LEAD_FIELDS = "firstName,lastName,email";

    // Lookup activity records for these activity types
    private static Map<Integer, String> ACTIVITY_TYPES;
    static {
        ACTIVITY_TYPES = new HashMap<Integer, String>();
        ACTIVITY_TYPES.put(7, "Email Delivered");
        ACTIVITY_TYPES.put(9, "Unsubscribe Email");
        ACTIVITY_TYPES.put(10, "Open Email");
        ACTIVITY_TYPES.put(11, "Click Email");
    }

    public static void main(String[] args) {
        // Command line argument to set how far back to look for lead changes (number of days)
        int lookBackNumDays = 1;
        if (args.length == 1) {
            lookBackNumDays = Integer.parseInt(args[0]);
        }

        // Establish "since date" using current timestamp minus some number of days (default is 1 day)
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.DAY_OF_MONTH, -lookBackNumDays);
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
        String sinceDateTime = sdf.format(cal.getTime());

        // Compose base URL
        String baseUrl = String.format("https://%s.mktorest.com",
            CUSTOM_SERVICE_DATA.get("accountId"));

        // Compose Identity URL
        String identityUrl = String.format("%s/identity/oauth/token?grant_type=%s&client_id=%s&client_secret=%s",
                baseUrl, "client_credentials", CUSTOM_SERVICE_DATA.get("clientId"), CUSTOM_SERVICE_DATA.get("clientSecret"));

        // Call Identity API
        JsonObject identityObj = JsonObject.readFrom(getData(identityUrl));
        String accessToken = identityObj.get("access_token").asString();

        // Compose URLs for Get Lead Changes, and Get Paging Token
        String activityTypesUrl = String.format("%s/rest/v1/activities/types.json?access_token=%s",
                baseUrl, accessToken);
        String pagingTokenUrl = String.format("%s/rest/v1/activities/pagingtoken.json?access_token=%s&sinceDatetime=%s",
                baseUrl, accessToken, sinceDateTime);

        // Use activity ids to create filter parameter
        String activityTypeIds = "";
        for (Integer id : ACTIVITY_TYPES.keySet()) {
            activityTypeIds += "&activityTypeIds=" + id.toString();
        }

        // Compose URL for Get Lead Activities
        // Only retrieve activities that match our defined activity types
        String leadActivitiesUrl = String.format("%s/rest/v1/activities.json?access_token=%s%s&batchSize=%s",
                baseUrl, accessToken, activityTypeIds, READ_BATCH_SIZE);

        Map<Integer, List> activitiesMap = new HashMap<Integer, List>();
        Set leadsSet = new HashSet();

        // Call Get Paging Token API
        JsonObject pagingTokenObj = JsonObject.readFrom(getData(pagingTokenUrl));
        if (pagingTokenObj.get("success").asBoolean()) {
            String nextPageToken = pagingTokenObj.get("nextPageToken").asString();
            boolean moreResult;
            do {
                moreResult = false;

                // Call Get Lead Activities API to retrieve activity data
                JsonObject leadActivitiesObj = JsonObject.readFrom(getData(String.format("%s&nextPageToken=%s",
                        leadActivitiesUrl, nextPageToken)));
                if (leadActivitiesObj.get("success").asBoolean()) {
                    moreResult = leadActivitiesObj.get("moreResult").asBoolean();
                    nextPageToken = leadActivitiesObj.get("nextPageToken").asString();

                    if (leadActivitiesObj.get("result") != null) {
                        JsonArray activitiesResultAry = leadActivitiesObj.get("result").asArray();
                        for (JsonValue activitiesResultObj : activitiesResultAry) {

                            // Extract activity fields of interest (leadID, activityType, activityDate, primaryAttributeValue)
                            JsonObject leadObj = new JsonObject();
                            int leadId = activitiesResultObj.asObject().get("leadId").asInt();
                            leadObj.add("activityType", ACTIVITY_TYPES.get(activitiesResultObj.asObject().get("activityTypeId").asInt()));
                            leadObj.add("activityDate", activitiesResultObj.asObject().get("activityDate").asString());
                            leadObj.add("primaryAttributeValue", activitiesResultObj.asObject().get("primaryAttributeValue").asString());

                            // Store JSON containing activity fields in a map using lead id as key
                            List leadLst = activitiesMap.get(leadId);
                            if (leadLst == null) {
                                activitiesMap.put(leadId, new ArrayList());
                                leadLst = activitiesMap.get(leadId);
                            }
                            leadLst.add(leadObj);

                            // Store unique lead ids for use as lead filter value below
                            leadsSet.add(leadId);
                        }
                    }
                }
            } while (moreResult);
        }

        // Use unique lead id values to create filter parameter
        String filterValues = "";
        for (Object object : leadsSet) {
            if (filterValues.length() > 0) {
                filterValues += ",";
            }
            filterValues += String.format("%s", object);
        }

        // Compose Get Multiple Leads by Filter Type URL
        // Only retrieve leads that match the list of lead ids that was accumulated during activity query
        String getMultipleLeadsUrl = String.format("%s/rest/v1/leads.json?access_token=%s&filterType=%s&fields=%s&filterValues=%s&batchSize=%s",
                baseUrl, accessToken, LEAD_FILTER_TYPE, LEAD_FIELDS, filterValues, READ_BATCH_SIZE);
        String nextPageToken = "";

        do {
            String gmlUrl = getMultipleLeadsUrl;

            // Append paging token to Get Multiple Leads by Filter Type URL
            if (nextPageToken.length() > 0) {
                gmlUrl = String.format("%s&nextPageToken=%s", getMultipleLeadsUrl, nextPageToken);
            }

            // Call Get Multiple Leads by Filter Type API to retrieve lead data
            JsonObject multipleLeadsObj = JsonObject.readFrom(getData(gmlUrl));
            if (multipleLeadsObj.get("success").asBoolean()) {
                if (multipleLeadsObj.get("result") != null) {
                    JsonArray multipleLeadsResultAry = multipleLeadsObj.get("result").asArray();

                    // Iterate through lead data
                    for (JsonValue leadResultObj : multipleLeadsResultAry) {
                        int leadId = leadResultObj.asObject().get("id").asInt();

                        // Join activity data with lead fields of interest (firstName, lastName, email)
                        List leadLst = activitiesMap.get(leadId);
                        for (JsonObject leadObj : leadLst) {
                            leadObj.add("firstName", leadResultObj.asObject().get("firstName").asString());
                            leadObj.add("lastName", leadResultObj.asObject().get("lastName").asString());
                            leadObj.add("email", leadResultObj.asObject().get("email").asString());
                        }
                    }
                }
            }

            nextPageToken = "";
            if (multipleLeadsObj.asObject().get("nextPageToken") != null) {
                nextPageToken = multipleLeadsObj.get("nextPageToken").asString();
            }
        } while (nextPageToken.length() > 0);

        // Now place activity data into an array of JSON objects
        JsonArray activitiesAry = new JsonArray();
        for (Map.Entry<Integer, List> activity : activitiesMap.entrySet()) {
            int leadId = activity.getKey();
            for (JsonObject leadObj : activity.getValue()) {
                // do something with leadId and each leadObj
                leadObj.add("leadId", leadId);
                activitiesAry.add(leadObj);
            }
        }

        // Print out result objects
        JsonObject result = new JsonObject();
        result.add("result", activitiesAry);
        System.out.println(result);

        System.exit(0);
    }

    // Perform HTTP GET request
    private static String getData(String endpoint) {
        String data = "";
        try {
            URL url = new URL(endpoint);
            HttpsURLConnection urlConn = (HttpsURLConnection) url.openConnection();
            urlConn.setRequestMethod("GET");
            urlConn.setAllowUserInteraction(false);
            urlConn.setDoOutput(true);
            int responseCode = urlConn.getResponseCode();
            if (responseCode == 200) {
                InputStream inStream = urlConn.getInputStream();
                data = convertStreamToString(inStream);
            } else {
                System.out.println(responseCode);
                data = "Status:" + responseCode;
            }
        } catch (MalformedURLException e) {
            System.out.println("URL not valid.");
        } catch (IOException e) {
            System.out.println("IOException: " + e.getMessage());
            e.printStackTrace();
        }
        return data;
    }

    private static String convertStreamToString(InputStream inputStream) {
        try {
            return new Scanner(inputStream).useDelimiter("A").next();
        } catch (NoSuchElementException e) {
            return "";
        }
    }
}
```

Dat is het. Fijne codering!

Gepost op _2015-11-20_ door _David_

## De SoundCloud-speler integreren met de Munchkin API

SoundCloud biedt een ongelofelijk audiohostingplatform met rijke analyses en functionaliteit voor alles, van het streven naar indie rock-acten, tot EDM-artiesten boven aan de muziekindustrie, tot verhalende podcasts. Samen met de ongelooflijke native functionaliteit van het platform is er een API-programma van wereldklasse waarmee u uw gegevens kunt verplaatsen en luistergedrag kunt bijhouden. Dit is vooral handig voor podcasters en u kunt specifieke luisterhandelingen, zoals afspelen, pauzeren en delen, koppelen aan specifieke inhoud in het script en de audio. Vandaag nemen wij een blik bij het leveraging van [ SoundCloud widget API ](https://developers.soundcloud.com/docs/api/html5-widget) van SoundCloud om deze activiteiten in Marketo te verzenden en te volgen. Laten we eerst eens kijken naar het genereren van een Munchkin-activiteit die wordt opgenomen in de activiteitenaanmelding van een lead Marketo. Op zijn meest basale manier, richten wij een vraag aan Munchkin.munchkinFunction en gaan &quot;visitWebPage&quot;als eerste argument over. Dit zal een activiteit van de Web-pagina van Bezoekingen met Marketo registreren, en om het even welke willekeurige gegevens URL en van het Koord van de Vraag registreren die wij tot de methode overgaan. Het tweede argument accepteert een JavaScript-object met onze gegevens, dat twee leden heeft: &quot;url&quot; en &quot;params&quot;, beide tekenreeksen. Het URL-lid komt overeen met de webpagina van de activiteit in Marketo, terwijl params overeenkomen met de Querystring. Voor onze doeleinden gebruiken we de URL als id voor acties met betrekking tot SoundCloud, &#39;soundCloudInteraction&#39;, terwijl params aanvullende gegevens over de specifieke activiteit zullen bevatten. Hier is de functie die wij gebruiken om elke actie te volgen:

```javascript
var trackActivity = function(action){

    //set action param to be the string passed to the function
    var qs = "action=" + action;

    //use getCurrentSound callback to get the name of the current track
    soundCloudMunchkin.widget.getCurrentSound(function(currentSound){

        //add it to our querystring
        qs = qs + "&sound=" + currentSound.title;

        //use the getPosition callback to get the position of the track in ms
        soundCloudMunchkin.widget.getPosition(function(position){

            //add it to the querystring
            qs = qs + "&position=" + position;

            //assemble our data object for the munchkin activity
            var dataObject = {
                "url": "soundCloudInteraction",
                "params": qs
            }

            //call the munchkinFunction to submit the activity
            Munchkin.munchkinFunction("visitWebPage", dataObject);
        });
    });
}
```

Aangezien de standaard SoundCloud-widget is ingesloten in een iframe, gebruikt de widget postberichten om te communiceren en moeten callbacks worden gebruikt om gegevens te verkrijgen, zoals u kunt zien met de methoden currentSound en getPosition. De SoundCloud-widget-API biedt een set JavaScript-callbacks die we kunnen gebruiken om te reageren op individuele gebeurtenissen in de speler en deze naar Marketo te verzenden. De gebeurtenissen waarin we geïnteresseerd zijn, zijn waarnaar de gebruiker luistert, hoe lang de gebruiker luistert en interacties die de gebruiker maakt met de speler. We kijken dus naar de volgende gebeurtenissen:

* AFSPELEN
* PAUZE
* FINISH
* ZOEKEN
* CLICK_DOWNLOAD
* KLIKKEN_KOPEN
* OPEN_SHARE_PANEL

We moeten ook de methode bind() van de widget gebruiken om callbacks aan elk van deze gebeurtenissen toe te voegen. Laten we naar één voorbeeld kijken:

```javascript
widget.bind(SC.Widget.Events.PLAY, function(){
    soundCloudMunchkin.trackPlay();
});
```

Hierdoor wordt het zo dat wanneer een track wordt afgespeeld, we de trackPlay-methode activeren om een gebeurtenis naar Marketo te verzenden met gegevens over de huidige track. U kunt het volledige manuscript [ hier ](https://gist.github.com/kelkingtron/6750bb07c1397d93d9c7#file-soundcloudmunchkin-js) vinden. Het soundCloudMunchkin-object heeft een init-methode, die een SoundCloud-widgetobject als enig argument accepteert, die de trackingmethoden aan de relevante callbacks bindt en uw widget zodanig instelt dat de activiteit tot aan Marketo wordt bijgehouden. Uw pagina zal uw [ geladen code van Munchkin ](/help/javascript-api/lead-tracking.md), evenals de [ SoundCloud API bibliotheek ](https://w.soundcloud.com/player/api.js) moeten hebben. U moet ook alles initialiseren, naast het insluiten van de daadwerkelijke SoundCloud-widget:

```javascript
window.onload=function(){
var iframe = document.getElementById(iframeId);
    if(iframe) {
        widget = SC.Widget(iframe);
        soundCloudMunchkin.init(widget);
    };
};
```

Gepast op _2015-12-21_ door _Kenny_

## RTP en de EU-richtlijn inzake elektronische privacy

In dit artikel wordt uitgelegd hoe u RTP kunt gebruiken om websitebezoekers die worden bijgehouden, op de hoogte te stellen of het bijhouden van gegevens automatisch uit te schakelen voor Europese bezoekers. Sinds 2012 moet elke website die voor Europese bezoekers beschikbaar is, voldoen aan de EU-richtlijn inzake e-privacy. In 2011 zijn nieuwe wetten in werking getreden die voorkomen dat identificatiegegevens op de computer van een gebruiker worden opgeslagen zonder dat de gebruiker hiervan op de hoogte is. Als u cookies of andere technologieën gebruikt voor niet-essentiële tracking, moet u:

1. Vertel gebruikers dat de volgende technologieën worden gebruikt.
1. Verklaar de redenen om die technologieën te gebruiken.
1. Vraag de toestemming van de gebruiker voordat deze technologie wordt gebruikt en geef hem de mogelijkheid om de toestemming op elk gewenst moment in te trekken.

Gepost op _1970-01-01_ door _Yanir_

## Klant- en perspectiefinformatie bijwerken in Marketo met behulp van AP

Er zijn scenario&#39;s waar de merkgebonden systemen worden gebruikt om klant en perspectiefinformatie bij te werken. Het marketingteam zou graag willen dat deze updates teruggezet worden in Marketo, zodat ze over het meest nauwkeurige registratiesysteem beschikken dat ze in hun marketingcampagnes kunnen gebruiken. Met de onderstaande aanpak kunt u periodieke uploads naar Marketo instellen om uw Marketo-contactgegevens bij te werken met de gegevens die in dat bedrijfseigen systeem zijn gewijzigd. Het diagram hieronder toont de API vraag die op een vastgestelde periodieke tijdopnemer wordt gemaakt. Aangezien de periodieke tijdopnemer wordt teweeggebracht zal de cliëntlogica eerst bijgewerkte contacten van het merkgebonden systeem terugwinnen. Hoe dit wordt gedaan zal van systeem aan systeem verschillen gebruikend of APIs of gegevensuitvoer van het merkgebonden systeem. We geven details over de Marketo API&#39;s die worden uitgevoerd zodra de bijgewerkte contactgegevens zijn opgehaald. SOAP-aanvraag voor synchronisatieMultipleLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:paramsSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <leadRecordList>
    <leadRecord>
      <Email>henry@superstar.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Henry</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Adams</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>Director of Demand Generation</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
    <leadRecord>
      <Email>ssmith@gmail.com</Email>
      <ns2:leadAttributeList>
        <attribute>
          <attrName>FirstName</attrName>
          <attrValue>Suzie</attrValue>
        </attribute>
        <attribute>
          <attrName>LastName</attrName>
          <attrValue>Smith</attrValue>
        </attribute>
        <attribute>
          <attrName>Title</attrName>
          <attrValue>VP Marketing</attrValue>
        </attribute>
      </ns2:leadAttributeList>
    </leadRecord>
  </leadRecordList>
  <dedupEnabled>true</dedupEnabled>
</ns2:paramsSyncMultipleLeads>
```

SOAP Response van syncMultilpeLeads:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ns2:successSyncMultipleLeads xmlns:ns2="<http://www.marketo.com/mktows/">
  <result>
    <syncStatusList>
      <syncStatus>
        <leadId>1094593</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
      <syncStatus>
        <leadId>1094594</leadId>
        <status>UPDATED</status>
        <error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true" />
      </syncStatus>
    </syncStatusList>
  </result>
</ns2:successSyncMultipleLeads>
```

`syncMultipleLeads` voert een UPSERT-bewerking uit. Als er al een contactpersoon in Marketo bestaat op basis van het verzonden e-mailadres, worden de kenmerken bijgewerkt. Als een contactpersoon niet bestaat, wordt deze gemaakt. Het antwoord van `syncMultipleLeads` keert de status voor elk van de voorgelegde contacten terug. De `<attrName/>` -waarden in `<leadAttributeList/>` moeten overeenkomen met de SOAP API-naam die is gedefinieerd voor dat Marketo-abonnement. U kunt de SOAP API-namen detecteren in de sectie voor veldbeheer in het deelvenster Marketo-beheer door de veldnamen te exporteren.

Zie het onderstaande Java-voorbeeldprogramma waarin het hierboven beschreven scenario wordt uitgevoerd:

```java
 import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
import java.util.\*;

public class SyncMultipleLeadsExample {

  public static void main(String[] args) {

    try {
      URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
      String marketoUserId = "CHANGE ME";
      String marketoSecretKey = "CHANGE ME";

      QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
      MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
      MktowsPort port = service.getMktowsApiSoapPort();

      // Create Signature
      DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
      String text = df.format(new Date());
      String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);      
      String encryptString = requestTimestamp + marketoUserId ;

      SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
      Mac mac = Mac.getInstance("HmacSHA1");
      mac.init(secretKey);
      byte[] rawHmac = mac.doFinal(encryptString.getBytes());
      char[] hexChars = Hex.encodeHex(rawHmac);
      String signature = new String(hexChars); 

      // Set Authentication Header
      AuthenticationHeader header = new AuthenticationHeader();
      header.setMktowsUserId(marketoUserId);
      header.setRequestTimestamp(requestTimestamp);
      header.setRequestSignature(signature);

      // Create Request
      ParamsSyncMultipleLeads request = new ParamsSyncMultipleLeads();

      ObjectFactory objectFactory = new ObjectFactory();

      JAXBElement dedup = objectFactory.createParamsSyncMultipleLeadsDedupEnabled(true);
      request.setDedupEnabled(dedup);

      // The list of contacts defined here would be retrieved from the proprietary system
      Contact contact = new Contact("Henry","Adams","henry@superstar.com", "Director of Demand Generation");
      Contact contact2 = new Contact("Suzie","Smith","ssmith@gmail.com", "VP Marketing");

      ArrayList updatedContacts = new ArrayList();
      updatedContacts.add(contact);
      updatedContacts.add(contact2);

      ArrayOfLeadRecord arrayOfLeadRecords = new ArrayOfLeadRecord();

      Iterator it = updatedContacts.iterator();
      while(it.hasNext())
      {
        Contact c = it.next();

        LeadRecord leadRec = new LeadRecord();

        JAXBElement email = objectFactory.createLeadRecordEmail(c.email);
        leadRec.setEmail(email);      

        Attribute attr1 = new Attribute();
        attr1.setAttrName("FirstName");
        attr1.setAttrValue(c.fname);

        Attribute attr2 = new Attribute();
        attr2.setAttrName("LastName");
        attr2.setAttrValue(c.lname);

        Attribute attr3 = new Attribute();
        attr3.setAttrName("Title");
        attr3.setAttrValue(c.title);

        ArrayOfAttribute aoa = new ArrayOfAttribute();
        aoa.getAttributes().add(attr1);
        aoa.getAttributes().add(attr2);
        aoa.getAttributes().add(attr3);

        QName qname = new QName("http://www.marketo.com/mktows/", "leadAttributeList");
        JAXBElement attrList = new JAXBElement(qname, ArrayOfAttribute.class, aoa);

        leadRec.setLeadAttributeList(attrList);
        arrayOfLeadRecords.getLeadRecords().add(leadRec);

      }

      request.setLeadRecordList(arrayOfLeadRecords);      

      JAXBContext context = JAXBContext.newInstance(SuccessSyncMultipleLeads.class);
      Marshaller m = context.createMarshaller();
      m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
      m.marshal(request, System.out);

      SuccessSyncMultipleLeads result = port.syncMultipleLeads(request, header);

      m.marshal(result, System.out);
    }

    catch(Exception e) {
      e.printStackTrace();
    }
  }

  public static class Contact {
    public String fname, lname, email, title;

      public Contact(String fname, String lname, String email, String title) {
          this.fname = fname;
          this.lname = lname;
          this.email = email;
          this.title = title;
      }
  }
}
```

 
Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepost op _2014-03-24_ door _Travis Kaufman_

## Transactiee-mail verzenden vanuit Marketo met de API

Hiervoor moet een bestaande slimme campagne worden gemaakt met de gebruikersinterface van Marketo. De ontvanger van de e-mail moet ook in Marketo bestaan. Zo alvorens requestCampaign API te roepen, gebruik [ getLead API ] (/help/soap-api/getlead.md om te verifiëren als e-mail in Marketo bestaat. Nadat u een vraag via requestCampaign API maakt, kunt u het bevestigen door te controleren om te zien of heeft de Slimme Campagne in Marketo in werking gesteld. We laten u eerst zien hoe u een slimme campagne maakt, ten tweede hoe u een trigger instelt voor het verzenden van een campagne via de API, ten derde hoe u een e-mail definieert als onderdeel van een flowactie en ten vierde een codevoorbeeld dat wordt gebruikt om deze campagne uit te voeren.
**hoe te om een Nieuwe Slimme Campagne in Marketo** Slimme Campagnes in Marketo tot stand te brengen voert elk van uw marketing activiteiten uit. U kunt een reeks geautomatiseerde acties opzetten om een slimme lijst van contacten te nemen. Als u transactie-e-mailberichten verzendt, stelt u een trigger in de campagne in, zoals hieronder wordt weergegeven, om e-mails via de API te verzenden. Laten we eerst de slimme campagne instellen. 1. Kies bij marketingactiviteiten een programma en klik vervolgens onder het vervolgkeuzemenu Nieuw lokaal element.

1. Klik op Slimme campagne
1. Voer een slimme naam voor de campagne in en klik op Maken

**voegt de Trekkers aan een Slimme Campagne** toe Toevoegend Trekkers aan een Slimme Campagne u toestaat om een Slimme Campagne te maken die op één persoon tegelijkertijd op een levende gebeurtenis wordt gebaseerd, die in dit geval een verzoek via [ requestCampaign API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) is. 1. Zoek de trigger &#39;Campaign is Requested&#39; en sleep deze naar het canvas.

1. Selecteer in de trigger &quot;is&quot; en &quot;Web Service API&quot;.

**hoe te om de Actie van de Stroom E-mail op een Campagne** tot stand te brengen de vereniging van een e-mail met een Slimme Campagne staat marketers toe om te beheren hoe zij een e-mail willen kijken, en staat de derdetoepassing toe om te bepalen wie het en wanneer ontvangt. Nadat u een e-mailbericht hebt gemaakt als een nieuw lokaal element, kunt u dit instellen als een stroomactie in een campagne.  Zoek en selecteer het e-mailbericht dat u wilt verzenden.

**Steekproef van de Code om requestCampaign API** te roepen na vestiging de campagne en trekkers in de interface van Marketo, tonen wij u hoe te om API te gebruiken om een e-mail te verzenden. Het eerste voorbeeld is een XML-aanvraag, het tweede een XML-reactie en het laatste voorbeeld is een Java-codevoorbeeld dat kan worden gebruikt om de XML-aanvraag te genereren. We tonen u ook hoe u de campagne-id kunt vinden die wordt gebruikt wanneer u de `requestCampaign` API aanroept.
De API-aanroep vereist ook dat u de id van de Marketo-campagne vooraf kent. U kunt de campagne-id op een van de volgende manieren bepalen: 1. Gebruik [ getCampaignsForSource ](/help/soap-api/getcampaignsforsource.md) API 1. Open de Marketo-campagne in een browser en bekijk de URL-adresbalk. De campagne-id (vertegenwoordigd als een 4-cijferig geheel getal) is direct na &quot;SC&quot; te vinden. Bijvoorbeeld `<https://app-stage.marketo.com/#SC**1025**A1>` . Het bolle gedeelte is de campagne-id - &quot;1025&quot;. SOAP-aanvraag voor `requestCampaign`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>anotherlead@company.com</keyValue>
        </leadKey>
      </leadList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP Response for requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Zie hieronder een voorbeeld van een Java-programma dat het hierboven beschreven scenario uitvoert.

```java
import com.marketo.mktows.*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
 
 
public class RequestCampaign {
 
    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";
             
            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();
             
            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);           
            String encryptString = requestTimestamp + marketoUserId ;
             
            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars); 
             
            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);
             
            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();
             
            request.setSource(ReqCampSourceType.MKTOWS);
             
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);
             
            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");
             
            LeadKey key2 = new LeadKey();
            key2.setKeyType(LeadKeyRef.EMAIL);
            key2.setKeyValue("anotherlead@company.com");
             
            leadKeyList.getLeadKeies().add(key);
            leadKeyList.getLeadKeies().add(key2);
             
            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);
 
            SuccessRequestCampaign result = port.requestCampaign(request, header);
 
            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);
             
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-03-27_ door _Murta_

## Een e-mail verzenden met dynamische inhoud vanuit Marketo met behulp van het toegangspunt

Stel je voor dat je de follow-upe-mails van je callcenter wilt automatiseren. Nadat uw supportmedewerker met een klant heeft gesproken, wilt u automatisch een e-mail verzenden waarin u deze wordt bedankt voor het contact met uw bedrijf. Laten wij dit een stap verder nemen, en zeggen u het specifieke gespreksonderwerp wilt omvatten dat met de klant wordt besproken die u in uw CRM volgt. U kunt dit vanuit Marketo doen met de requestCampaign SOAP API om een e-mail met dynamische inhoud te verzenden. Met de requestCampaign-API kunt u een lead of lead doorgeven. Het staat u ook toe om in de Tokens van het Programma over te gaan die met een bestaande Campagne kunnen worden gebruikt om dynamische inhoud te verzenden. De requestCampaign SOAP API vereist dat de e-mailontvanger in Marketo bestaat. Zo alvorens requestCampaign API te roepen, gebruik [ getLead API ](/help/soap-api/getlead.md) om te verifiëren als e-mail in Marketo bestaat. We tonen u eerst hoe u een slimme campagne maakt, ten tweede hoe u een trigger instelt voor het verzenden van een campagne via de API, ten derde hoe u een e-mail maakt die dynamische inhoud accepteert via Program Tokens, ten vierde hoe u een e-mail definieert als onderdeel van een flowactie en ten vijfde een codevoorbeeld dat wordt gebruikt om deze campagne uit te voeren. **hoe te om een Nieuwe Slimme Campagne in Marketo** Slimme Campagnes in Marketo tot stand te brengen voert elk van uw marketing activiteiten uit. U kunt een reeks geautomatiseerde acties opzetten om een slimme lijst van contacten te nemen. Als u transactie-e-mailberichten verzendt, stelt u een trigger in de campagne in, zoals hieronder wordt weergegeven, om e-mails via de API te verzenden. Laten we eerst de slimme campagne instellen. 1. Kies bij marketingactiviteiten een programma en klik vervolgens in het vervolgkeuzemenu Nieuw lokaal element op

1. Klik op Slimme campagne
1. Ga de slimme campagnenaam in en de klik leidt **tot Triggers aan een Slimme Campagne** Toevoegend Triggers aan een Slimme Campagne u toestaat om een Slimme Campagne in werking te stellen die op één persoon tegelijkertijd op een levende gebeurtenis wordt gebaseerd, die in dit geval een verzoek via [ requestCampaign API ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/triggerCampaignUsingPOST) is.
1. Zoek de trigger &#39;Campaign is Requested&#39; en sleep deze naar het canvas.
1. Selecteer in de trigger &quot;is&quot; en &quot;Web Service API&quot;.

**hoe te om in Dynamische Inhoud over te gaan Gebruikend API** in Marketo, zijn Mijn Tokens variabelen die u in uw Programma kunt gebruiken. Met Mijn tokens kunt u informatie met betrekking tot uw programma op één locatie invoeren, die informatie vervangen door een waarde die u opgeeft, en deze informatie ophalen in andere delen van de toepassing, zoals een e-mailsjabloon. Met de requestCampaign SOAP API kunt u een array met Program Tokens doorgeven, die bestaande tokens overschrijven. Nadat de campagne is gestart, worden de tokens genegeerd. U creeert Mijn Tokens op of het de omslagniveau van de Campagne, of op het niveau van het Programma. Mijn tokens op het de omslagniveau van de Campagne zullen neer aan alle Programma&#39;s erven binnen de omslag van de Campagne. Als u Mijn Tokens op het de omslagniveau van de Campagne creeert, kunt u de geërfte waarde op het niveau van het Programma overschrijven. Als u bijvoorbeeld tokens definieert voor de datum van het programma en de beschrijving van het programma op het mapniveau van de campagne, kunt u deze waarden overschrijven op het niveau van het individuele programma.

Zo doe je dit. 1. Selecteer in de boomstructuur Marketingactiviteiten de map Campagne of het Programma waar u de tokens wilt maken. Selecteer Mijn tokens in de bovenste menubalk. Vervolgens wordt het canvas Mijn tokens weergegeven. Sleep vanuit de rechterzijstructuur een Symbolische Type naar het canvas, in dit geval &quot;Tekst&quot;. Markeer Mijn token in het veld Tokennaam en voer een unieke token-naam in. In dit geval is dit &quot;my.conversationtopic&quot;. Voer in het veld Waarde een relevante waarde in voor de token, die in dit geval &quot;Bedankt dat u ons vandaag hebt gebeld&quot; is. Door de API te gebruiken, overschrijven we de standaardwaarde Mijn token. Klik op Opslaan om het aangepaste token op te slaan.  1. Klik op Nieuw om een nieuwe e-mail te maken. Klik vervolgens op Nieuwe lokale Assets en selecteer E-mail. Vul vervolgens de relevante velden in om uw e-mail een naam te geven. Klik bij het schrijven van uw e-mail op het pictogram Token om tokens in uw e-mail op te nemen. Nu u uw sjabloon-e-mail met tokens hebt gemaakt, voegen we het e-mailbericht toe als een stroomactie voor de campagne in de volgende stap. Dus wanneer u de campagne via de API oproept, wordt de e-mail verzonden.\
**hoe te om de Actie van de Stroom E-mail op een Campagne** tot stand te brengen de vereniging van een e-mail met een Slimme Campagne staat marketers toe om te beheren hoe zij een e-mail willen kijken, en staat de derdetoepassing toe om te bepalen wie het en wanneer ontvangt. Nadat u een e-mailbericht hebt gemaakt als een nieuw lokaal element, kunt u dit instellen als een stroomactie in een campagne. Zoek en selecteer het e-mailbericht dat u wilt verzenden.
**Steekproef van de Code om requestCampaign API** te roepen na vestiging de campagne en trekkers in de interface van Marketo, tonen wij u hoe te om API te gebruiken om een e-mail te verzenden. Het eerste voorbeeld is een XML-aanvraag, het tweede een XML-reactie en het laatste voorbeeld is een Java-codevoorbeeld dat kan worden gebruikt om de XML-aanvraag te genereren. We tonen u ook hoe u de campagne-id kunt vinden die wordt gebruikt wanneer u de requestCampaign-API aanroept. De API-aanroep vereist ook dat u de id van de Marketo-campagne vooraf kent. U kunt de campagne-id op een van de volgende manieren bepalen: 1. Gebruik [ getCampaignsForSource ](/help/soap-api/getcampaignsforsource.md) API 1. Open de Marketo-campagne in een browser en bekijk de URL-adresbalk. De campagne-id (vertegenwoordigd als een 4-cijferig geheel getal) is direct na &quot;SC&quot; te vinden. Bijvoorbeeld `<https://app-stage.marketo.com/#SC&#x200B;**1025**&#x200B;A1>` . Het bolle gedeelte is de campagne-id - &quot;1025&quot;. SOAP-aanvraag voor requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Header>
    <ns1:AuthenticationHeader>
      <mktowsUserId>demo17_1_809939944BFABAE58E5D27</mktowsUserId><requestSignature>48397ad47b71a1439f13a51eea3137df46441979</requestSignature><requestTimestamp>2013-08-01T12:31:14-07:00</requestTimestamp>
    </ns1:AuthenticationHeader>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <ns1:paramsRequestCampaign>
      <source>MKTOWS</source>
      <campaignId>4496</campaignId>
      <leadList>
        <leadKey>
          <keyType>EMAIL</keyType>
          <keyValue>lead@company.com</keyValue>
        </leadKey>
      </leadList>
      <programTokenList>
        <attrib>
          <name>{{my.conversationtopic}}</name>
          <value>Thank you for calling about adding a line of service to your current plan.</value>
        </attrib>
      </programTokenList>
    </ns1:paramsRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

SOAP Response for requestCampaign

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="<http://schemas.xmlsoap.org/soap/envelope/>" xmlns:ns1="<http://www.marketo.com/mktows/">
  <SOAP-ENV:Body>
    <ns1:successRequestCampaign>
      <result>
        <success>true</success>
      </result>
    </ns1:successRequestCampaign>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Zie hieronder een voorbeeld van een Java-programma dat het hierboven beschreven scenario uitvoert.

```java
import com.marketo.mktows.\*;
import java.net.URL;
import javax.xml.namespace.QName;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import org.apache.commons.codec.binary.Hex;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBElement;
import javax.xml.bind.Marshaller;
 
 
public class RequestCampaign {
 
    public static void main(String[] args) {
        System.out.println("Executing Request Campaign");
        try {
            URL marketoSoapEndPoint = new URL("CHANGE ME" + "?WSDL");
            String marketoUserId = "CHANGE ME";
            String marketoSecretKey = "CHANGE ME";
             
            QName serviceName = new QName("http://www.marketo.com/mktows/", "MktMktowsApiService");
            MktMktowsApiService service = new MktMktowsApiService(marketoSoapEndPoint, serviceName);
            MktowsPort port = service.getMktowsApiSoapPort();
             
            // Create Signature
            DateFormat df = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ssZ");
            String text = df.format(new Date());
            String requestTimestamp = text.substring(0, 22) + ":" + text.substring(22);           
            String encryptString = requestTimestamp + marketoUserId ;
             
            SecretKeySpec secretKey = new SecretKeySpec(marketoSecretKey.getBytes(), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(secretKey);
            byte[] rawHmac = mac.doFinal(encryptString.getBytes());
            char[] hexChars = Hex.encodeHex(rawHmac);
            String signature = new String(hexChars); 
             
            // Set Authentication Header
            AuthenticationHeader header = new AuthenticationHeader();
            header.setMktowsUserId(marketoUserId);
            header.setRequestTimestamp(requestTimestamp);
            header.setRequestSignature(signature);
             
            // Create Request
            ParamsRequestCampaign request = new ParamsRequestCampaign();
             
            request.setSource(ReqCampSourceType.MKTOWS);
             
            ObjectFactory objectFactory = new ObjectFactory();
            JAXBElement<Integer> campaignId = objectFactory.createParamsRequestCampaignCampaignId(4496);
            request.setCampaignId(campaignId);
             
            ArrayOfLeadKey leadKeyList = new ArrayOfLeadKey();
            LeadKey key = new LeadKey();
            key.setKeyType(LeadKeyRef.EMAIL);
            key.setKeyValue("lead@company.com");
             
            leadKeyList.getLeadKeies().add(key);
             
            JAXBElement<ArrayOfLeadKey> arrayOfLeadKey = objectFactory.createParamsRequestCampaignLeadList(leadKeyList);
            request.setLeadList(arrayOfLeadKey);

            ArrayOfAttrib aoa = new ArrayOfAttrib();
             
            Attrib attrib = new Attrib();
            attrib.setName("{{my.conversationtopic}}");
            attrib.setValue("Thank you for calling about adding a line of service to your current plan.");
             
            aoa.getAttribs().add(attrib);
             
            JAXBElement<ArrayOfAttrib> arrayOfAttrib = objectFactory.createParamsRequestCampaignProgramTokenList(aoa);
            request.setProgramTokenList(arrayOfAttrib);
 
            SuccessRequestCampaign result = port.requestCampaign(request, header);
 
            JAXBContext context = JAXBContext.newInstance(SuccessRequestCampaign.class);
            Marshaller m = context.createMarshaller();
            m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, true);
            m.marshal(result, System.out);
             
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

Nadat u een vraag via requestCampaign API maakt, kunt u het bevestigen door te controleren om te zien of heeft de Slimme Campagne in Marketo in werking gesteld.

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-04-03_ door _Murta_

## Anonieme bezoekersactiviteit vastleggen op basis van zakelijke logica

Stel dat u gebruikers wilt volgen die een specifiek bericht op uw bedrijfsblog bezoeken. Laten we zeggen dat we gebruikers die een artikel bezoeken alleen maar willen volgen die hun interesse kenbaar maken door minstens 5 seconden door te brengen en de pagina omlaag te schuiven. Voor anonieme gebruikers wilt u een nieuwe lead maken in Marketo met deze gebeurtenis. Voor bekende gebruikers wilt u hun lead-activiteiten bijwerken met deze gebeurtenis. U kunt dit verwezenlijken door [ het volgen van Munchkin code ](/help/javascript-api/lead-tracking.md) op uw website te gebruiken. Wanneer een niet-gekoelde gebruiker naar een pagina gaat met de trackingcode van Munchkin, wordt een nieuw cookie gemaakt in de browser van de gebruiker en wordt een nieuwe anonieme lead gemaakt in Marketo. Als de gebruiker al is gekookt en de gebruiker een bestaande lead is in Marketo, wordt het bezoek aan de pagina opgenomen in het activiteitenlogboek van de gebruiker in Marketo. We tonen u eerst hoe u code voor het bijhouden van Munchkin kunt genereren in Marketo, laten u vervolgens zien hoe u uw Munchkin-voorbeeldcode kunt wijzigen om alleen te activeren als aan bepaalde voorwaarden is voldaan en laten u ten derde zien hoe u kunt controleren of een paginabezoek van een anonieme gebruiker is opgenomen in Marketo.

**hoe te om Munchkin het Volgen Code** te produceren de volgende code van Munchkin staat u toe om bezoeken aan uw website te volgen. Er zijn drie typen Munchkin-code die hieronder worden beschreven, maar in dit voorbeeld gebruiken we de Asynchronous Munchkin tracking code. A) Eenvoudig: heeft de minste coderegels, maar is niet geoptimaliseerd voor het laden van webpagina&#39;s. Deze code laadt de jQuery-bibliotheek telkens wanneer een webpagina wordt geladen. B) Asynchroon: verkort de laadtijd van de webpagina. Deze code controleert of de jQuery-bibliotheek al bestaat, laadt deze als deze ontbreekt en gebruikt deze voor het uitvoeren van trackingcode zodra de rest van de webpagina is geladen. C) Asynchrone jQuery: verkort de laadtijd van de webpagina en verbetert ook de systeemprestaties. In deze code wordt ervan uitgegaan dat u al jQuery hebt en wordt niet gecontroleerd om deze te laden. 1. Klik op Beheer rechtsboven in de app.  1. Klik op Munchkin in de structuur aan de linkerkant.  1. Selecteer Asynchroon bij Type code bijhouden. 1. Klik en kopieer de code voor het bijhouden van JavaScript die u op uw website wilt plaatsen.
**Steekproef van de Code aan de Gebruiker van het Koekje en Gebeurtenis van het Spoor** Plaats de volgende code op uw Web-pagina&#39;s recht vóór de `</body>` markering. De het landen pagina&#39;s die in Marketo worden gecreeerd bevatten automatisch het volgen code, zodat te hoeven u niet om deze code op hen te zetten. Dit codevoorbeeld roept de Munchkin API aan nadat het script is geladen:

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('XXX-XXX-XXX');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName('head')[0].appendChild(s);
})();
</script>
```

Dit codevoorbeeld roept de Munchkin API aan nadat de gebruiker 5 seconden op de pagina is geweest en ook 500 pixels omlaag op de pagina heeft geschoven:

```javascript
<script src="https://code.jquery.com/jquery-2.1.0.min.js"></script> 
<script type="text/javascript">
$(function(){
 setTimeout(function(){
  $(window).scroll(function() {
      var y_scroll_position = window.pageYOffset;
      var scroll_position = 500; //Sets number of pixels user must scroll to be tracked        
  
  if(y_scroll_position > scroll_position) {
  //Munchkin tracking code
   (function() {
     var didInit = false;
     function initMunchkin() {
      if(didInit === false) {
        didInit = true;
        Munchkin.init('XXX-XXX-XXX');
      }
     }
     
     var s = document.createElement('script');
     s.type = 'text/javascript';
     s.async = true;
    s.src = '//munchkin.marketo.net/munchkin.js';
     s.onreadystatechange = function() {
      if (this.readyState == 'complete' || this.readyState == 'loaded') {
          initMunchkin();
      }
     };
     s.onload = initMunchkin;
     document.getElementsByTagName('head')[0].appendChild(s);
   })();
   }   
 },5000); //Sets time delay before tracking user
});
</script>
```

**hoe te om het Bezoek van de Pagina te verifiëren door Anonieme Gebruiker werd geregistreerd in Marketo**

1. Klik op Analytics in het bovenste menu en klik vervolgens op New Report. Kies de Activiteit van de Pagina van het Web als rapporttype, en noem dan uw rapport.
1. Nadat u een rapport hebt gemaakt, klikt u op Slimme lijst. Selecteer vervolgens het filter Bezochte webpagina in het vak aan de rechterkant. Voer de webpagina in waarop u de trackingcode voor Munchkin plaatst.
1. Klik op Instellen. Selecteer Anonieme Bezoekers van ISPs, en verander optie aan Getoonde.
1. Klik op Rapport. De activiteit wordt nu bijgehouden op de webpagina die u hebt geselecteerd.
1. Dubbelklik op de lead record. Hiermee wordt het activiteitenlogboek weergegeven waarin u de specifieke pagina kunt zien die de anonieme gebruiker heeft bezocht.

Dit artikel bevat code die wordt gebruikt om douaneserveringen uit te voeren. Vanwege de aangepaste aard van de service kan het team van technische ondersteuning van Marketo geen problemen met aangepaste werkzaamheden oplossen. Probeer niet om het volgende codevoorbeeld uit te voeren zonder aangewezen technische ervaring, of toegang tot een ervaren ontwikkelaar.

Gepast op _2014-04-17_ door _Murta_

## Lokaal telefoonnummer dynamisch wijzigen met RTP

Personalization is alles - dat hebben we allang ontdekt. Met dit gezegd zijnde, is het voor mij nog steeds verrassend dat het zo moeilijk is om, telkens als ik directe hulp nodig heb, de relevante lokale telefoonnummers op een website te vinden. Het goede ding wij [ Marketo in real time Personalization ](https://business.adobe.com/products/marketo/content-personalization.html) (RTP) hebben geïnstalleerd op <https://business.adobe.com/products/marketo/adobe-marketo.html>. Wij kunnen hefboomwerking [ RTP Bezoeker API ](/help/javascript-api/web-personalization.md) om het telefoonaantal dynamisch te veranderen dat een Webbezoeker in verschillende secties van de website ziet. Wow! Kan je dat geloven? Hoe werkt deze magie? Eerst, moet u RTP hebben die op uw website wordt geïnstalleerd zoals [ hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/web-personalization/rtp-tag-implementation/deploy-the-rtp-javascript) wordt beschreven. Volg daarna de onderstaande instructies en implementeer de JavaScript-code op uw website:

1. Neem uw internationaal telefoonaantal in de **defaultPhone** configuratie op
1. Tussenvoegsel het element identiteitskaart(en) van HTML in de **divIds** configuratie
1. Als u het telefoonaantal een klik-aan-vraag verbinding voor mobiele browsers wilt maken, dan plaats de **configuratie 0&rbrace; mobileLink &lbrace;aan** waar **.**
1. Wijs de verschillende plaatsen met hun telefoonaantallen in **cityPhone**, **statePhone**, en **countryPhone** configuraties in kaart

Hier volgen bijvoorbeeld voorbeeldwaarden voor configuratie-instellingen:

```json
  defaultPhone:"+1.503.608.4679", // Optional
  divIds:["phoneId1","phoneId2"],
  mobileLink: true,
  cityPhone: {
    "<a href="#">yanir</a>": ["San Mateo", "San Francisco"],
    "+353.1.242.3000": ["tel-aviv"]
  },
  statePhone: {
    "+1.650.376.2300": ["CA"],
    "+1.650.376.2302": ["OR"]
  },
  countryPhone: {
    "+1.650.376.2300": ["United States"],
    "+353.1.242.3000": ["Israel"]
  }
```

Tot slot neem een het ankermarkering van HTML op die identiteitskaart die één van ids in **afd.Ids** (van stap 2 hierboven) aanpast. Bijvoorbeeld, als u &quot;phoneId1&quot;in **divIds** specificeerde, dan zou uw het ankermarkering van HTML als dit kijken:

`  <a href="tel:+1800229933" id="phoneId1">+1800229933</a>`

Het script controleert of er een overeenkomst in deze volgorde is: cityPhone > statePhone > countryPhone > defaultPhone U kunt de telefoonnummers ook vervangen door tekst (Voorbeeld: &quot;Word lid van onze San Francisco-gebruikersgroep!&quot;) of HTML-code en de inhoud dynamisch wijzigen op basis van de geo-locatie. Veel plezier!

```html
<a href="tel:+1800229933" id="phoneId1">+1800229933</a>
<script>
(function(a){
    rtp('get','visitor',function(yc){
        var location = yc.results.location;
        var loop = true;
        var phoneChanged = false;
        console.log(yc.results);
        function checkObj(obj){
            return Object.getOwnPropertyNames(obj).length >0;
        }
        function changePhone(p){
            d=a.divIds;
            for(i=0;i<d.length;i++){
                if(document.getElementById(d[i]) !== null){
                    document.getElementById(d[i]).innerHTML = p;
                    if(a.mobileLink){
                        document.getElementById(d[i]).href= "tel:" + p;
                    }
                    console.log(p);
                }                
            }
            loop = false;
            phoneChanged = true;
        }
        function matchLocation(loc,objc){
            for (var key in objc) {
                for(i=0;i<objc[key].length && loop;i++){
                    if (!loop) { return true;};
                    val = objc[key][i];
                    //console.log(loc + location[loc] + " ? " + val);
                    if(location[loc].toLowerCase() === val.toLowerCase()){
                        changePhone(key);
                    }
                }
            }
        }
        if(checkObj(a.cityPhone)){
            matchLocation("city",a.cityPhone);
        }else if(checkObj(a.statePhone)){
            matchLocation("state",a.statePhone);
        }else if(checkObj(a.countryPhone)){
            matchLocation("country", a.countryPhone);
        }else if(!phoneChanged && a.defaultPhone.length > 0 ){
            changePhone(a.divId,a.defaultPhone);
        }
    });
})({
    defaultPhone:"",  //  [Optional] the number to show if visitor does not match the mapping below
    divIds:["phoneId1","Floater"],  //the phone HTML element ID, can be <div>, <a>, <span>, <p> etc.
    mobileLink: true,  //if you use click to call link (with href="tel:") you can also change its number

    cityPhone: {
        "<a href='#'>yanir</a>": ["San Mateo", "San Francisco"],        
        "+353.1.242.3000": ["tel-aviv"]
    },
    statePhone: {
        "+1.650.376.2300": ["CA"],
        "+1.650.376.2302": ["OR"]
    },
    countryPhone: {
        "+1.650.376.2300": ["United States"],
        "+353.1.242.3000": ["Israel"]
    }
});
</script>
```

GEPast op _2016-02-02_ door _Yanir_

## Updates van winter 2016

### Aangepaste objecten

* [ de Voorwerpen N van de Douane N:N verhoudingen nu gesteund ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/marketo-custom-objects/create-marketo-custom-objects)
   * Lood- of accountrecords kunnen nu vele tot vele relaties hebben via aangepaste objecten via de definitie van tussenliggende objecten. Nadat u een zelfstandig aangepast objecttype hebt gemaakt, kunt u een tussentijds objecttype maken met koppelingsvelden voor zowel het zelfstandige object als voor leads of accounts.
   * Er zijn geen nieuwe API-aanroepen voor deze functie, maar de objectdefinities moeten correct zijn geconfigureerd om deze relaties via de API te benutten.
* `getLeadActivities` en `getLeadChanges` retourneren geen activiteiten meer van anonieme leads. Zie [ Volgende Generatie Munchkin die FAQ ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) volgen voor meer informatie

Gepast op _2016-02-05_ door _Kenny_

## Activiteiten ophalen voor één lead met REST API

Dit is een vraag die onze ontwikkelaarsgemeenschap ons herhaaldelijk stelt:

&quot;Hoe krijg ik een lijst met activiteiten uit het verleden voor een individuele lead?&quot;

Tot voor kort was er geen eenvoudige manier om dit te bereiken met de REST API. Maar nu is het zover! De Winter 2016-release van onze REST API bevat een mooie kleine verbetering. [ krijgt de Activiteiten van het Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) keurt nu de **leadIds** parameter goed die kan worden gebruikt om loodidentiteitskaart te specificeren. Wanneer de **leadIds** parameter wordt gespecificeerd, slechts zullen de activiteiten voor die lood identiteitskaart worden teruggekeerd. U kunt dit zien als een filter voor loodid. Merk op dat de **leadIds** parameter een komma gescheiden lijst van loodids kan nemen voor het geval u resultaten op meer dan één lood (tot 30) zou willen filtreren. Dit kan handig zijn, bijvoorbeeld wanneer activiteiten worden beperkt tot leads voor een bepaalde onderneming. **Voorbeeld** hieronder is een steekproefverzoek aan [ krijgen de Activiteiten van het Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadActivitiesUsingGET) die de **leadIds** parameter bevat. Ik heb een waarde van &quot;50&quot;voor de **leadIds** parameter gespecificeerd, die aan een willekeurige lood in mijn instantie van Marketo beantwoordt. Ik heb de waarde &quot;129&quot; opgegeven voor de parameter activityTypeIds, die overeenkomt met de activiteit &quot;Mobile App Session&quot; op mijn Marketo-instantie.

`<https://123-abc-456.mktorest.com/rest/v1/activities.json?leadIds=50&activityTypeIds=129&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3J4SMAZRQO4RKIXCEMLFCM2APRSQ====>`

Hieronder volgt een uittreksel van het antwoord van bovengenoemd verzoek. Zoals u kunt zien, bevat het alleen resultaatobjecten met &quot;leadId&quot;: 50 en &quot;activityTypeId&quot;: 129.

```json
{
    "id": 846,
    "leadId": 50,
    "activityDate": "2015-04-06T21:58:59Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 7
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 879,
    "leadId": 50,
    "activityDate": "2015-04-07T00:45:11Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 5
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1114,
    "leadId": 50,
    "activityDate": "2015-04-08T00:02:41Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 241
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1551,
    "leadId": 50,
    "activityDate": "2015-04-09T23:31:56Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
{
    "id": 1716,
    "leadId": 50,
    "activityDate": "2015-04-15T22:44:19Z",
    "activityTypeId": 129,
    "primaryAttributeValueId": 13,
    "primaryAttributeValue": "Sample App",
    "attributes": [
        {
            "name": "Device Type",
            "value": "iPhone"
        },
        {
            "name": "Mobile App Session Length",
            "value": 223
        },
        {
            "name": "Platform",
            "value": "ios"
        }
    ]
}
```

## Naadloze integratie met Marketo en meer dan 500 toepassingen met Zapier

Dit is een artikel van Philippe Delle Case, Principal Solutions Consultant, Marketo.

### Doelstellingen

In dit artikel wordt gedetailleerd uitgelegd hoe u Marketo kunt integreren met meer dan 500 Cloud Apps, dankzij Zapier. Hiervoor bouwen we vanaf nul een Zapier-connector voor Marketo en implementeren we twee praktijkvoorbeelden van integratiegebruik: Hoofdzaak 1: Unidirectionele integratie van FullContact Card Reader naar Marketo

* Scan de visitekaartje van een contactpersoon met de toepassing FullContact Mobile Card Reader en ontvang automatisch een lead in Marketo.
* Voeg een bestaande lead toe aan een statische lijst in Marketo en zoek de lead die automatisch aan uw Google-werkblad wordt toegevoegd.
* Wijzig een lead in het Google-blad en zoek de wijziging terug naar Marketo.

### Vereisten

**Teken-up voor een vrije rekening met Zapier** [ Zapier ](https://zapier.com/) is de Dienst van de Automatisering van de Web-app die u gemakkelijk taken tussen andere online apps zonder de behoefte aan programmeurs of om het even welke middelen van IT laat automatiseren. Een korte inleiding kan [ hier ](https://zapier.com/api/v4/helpdocs/category/redirect/what-is-zapier) worden gevonden. Zapier biedt tegenwoordig ondersteuning voor meer dan 500 apps op vele verschillende domeinen, zoals marketing, CRM, CMS, Klantenondersteuning, Elektronische handtekening, Forms, enz. Een enkele integratie tussen de ene app en de andere wordt een Zap genoemd. Controle Zapier [ zapbook ](https://zapier.com/apps) voor een uitvoerige lijst van gesteunde Web apps.  Teken omhoog voor een vrije rekening [ hier ](https://zapier.com/sign-up/). U krijgt toegang tot maximaal 100 taken/maand, 5 zaps, en zaps die om de 15 minuten lopen. Je kunt natuurlijk veel meer bereiken door je in te schrijven op de betaalde plannen van Zapier (basis, bedrijf, bedrijf plus, etc.)

**Toegang tot een Instantie van Marketo als Beheerder of met een verstrekte rekening van de Gebruiker van API** Onze Zapier schakelaar zal Marketo REST API gebruiken om Leidingsgegevens aan Marketo te duwen. Als u deze API wilt gebruiken, hebt u een API-gebruiker en een aangepaste service nodig die u zelf kunt maken als beheerder van uw Marketo-instantie. Als niet, dan zal een beheerder die aan u moeten verstrekken. Er is ook een WebHaak om tot stand te brengen, slechts toegankelijk voor een Beheerder van Marketo. Een geleidelijke verklaring van hoe te om de Gebruiker van Marketo API en de Dienst van de Douane tot stand te brengen kan [ hier ](http://rest-api/custom-services/) worden gevonden. Als u klaar bent, hebt u de volgende gebruikersgegevens nodig om de Marketo REST API aan te roepen: client-id, clientgeheim, Munchkin-account-id, Munchkin-account-id

U kunt de Munchkin-account-id opvragen in het scherm Munchkin of Web Services Admin. Het patroon ziet er als volgt uit: `000-XXX-000`.  Geen behoefte om een toegangstoken te krijgen aangezien het slechts één enkel uur geldig zou zijn. De Schakelaar zal automatisch tokens voor u produceren.
**Meld u aan voor een gratis account bij Google Docs, Sheets en Slides zijn productiviteitstoepassingen waarmee u verschillende soorten onlinedocumenten kunt maken, er in real-time aan kunt werken met anderen en deze online kunt opslaan in uw Google Drive. Voor ons gebruik is een Google-overzicht nodig. Verschillende eigenschappen van Google Docs en verwezenlijking van een rekening met Google kunnen [ hier ](https://workspace.google.com/products/docs/) worden gevonden.
**Teken-omhoog voor een vrije rekening met FullContact** FullContact houdt u volledig verbonden met de mensen die het meest door in al uw contacten te trekken en hen onophoudelijk te synchroniseren met veranderingen in sociale profielen, foto&#39;s, e-mailhandtekeningen, bedrijfinformatie, en meer van belang zijn. Ze bieden een mobiele visitekaartjes lezer die kaarten kan scannen in 250 of meer webtoepassingen, waaronder Zapier. U kunt omhoog voor een vrije rekening [ hier ](https://app.fullcontact.com/login) ondertekenen. Je kunt je ook abonneren op een betaalrekening met een premie die meer functies en capaciteit biedt. De mobiele app kan van [ Apple AppStore ](https://apps.apple.com/us/app/fullcontact-business-card/id576780807), of van [ Google Play ](https://play.google.com/store/apps/details?id=com.fullcontact.cardreader) worden gedownload. De Zaps FullContact zijn gedocumenteerd [ hier ](https://zapier.com/apps/contacts-plus/integrations).

### Implementatie van de Marketo-connector voor Zapier

**creeer de app van Marketo** van de het Webinterface van Zapier, ga naar het Portaal van Ontwikkelaars. Klik **toevoegen Nieuwe App** en vul minstens de Titel (bijvoorbeeld &quot;Marketo&quot;) en de Beschrijving in. Het logo is optioneel, maar leuk om te hebben.\ **Authentificatie** In deze sectie verklaren wij de verschillende gebieden die voor de authentificatie van de VERTONING van Marketo worden gebruikt REST API en de authentificatiemontages. Maak eerst de volgende velden:

Bewerk de &#39;verificatie-instellingen&#39;:

* Audetype: Session Auth
* Authandtekening:

  `{"access_token":"{{access_token}}"}`

* Toegang tot token-plaatsing&#x200B;**:** Token in Querystring

Nadat een aangepaste Marketo-service is gemaakt, worden de client-id en het clientgeheim beschikbaar. Wij gebruiken cliënt identiteitskaart en cliëntgeheim om een toegangstoken via het REST API [ eindpunt van de Authentificatie &lbrace;te produceren. ](/help/rest-api/authentication.md) Vervolgens kunnen we deze toegangstoken gebruiken om volgende aanvragen voor de REST API in te dienen. Het token verloopt na een uur en moet opnieuw worden gegenereerd om door te gaan met het aanroepen van de REST API. Wij kozen authentificatietype = &quot;Auth van de Zitting&quot;aangezien het ons toestaat om een manuscript van de douaneauthentificatie uit te voeren telkens als ons zittingsteken wordt verlopen. In de sectie &#39;API voor scripts&#39; wordt uitgelegd hoe u dit mechanisme implementeert dat alleen met dit type verificatie kan werken.
**Triggers** Zapier Triggers zijn er om gegevens in Zapier te brengen. We hebben er geen nodig voor onze gebruiksgevallen, omdat we in plaats daarvan een Marketo Webhaak zullen gebruiken. We moeten echter nog steeds een dummy Trigger schrijven als verplichte test voor onze Marketo-connector. Wij gaan tot een Testen Trigger leiden die Marketo REST API [ roepen krijgt het Dagelijkse eindpunt van het Gebruik ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getDailyUsageUsingGET). Klik **toevoegen Nieuwe Trekker** om de tovenaar en vulling-up de volgende gebieden (de niet vermelde gebieden kunnen leeg worden verlaten) te beginnen: Naam en Beschrijving

* Naam: Trigger testen
* Sleutel: test_trigger
* Beschrijving: De testtrigger van de Marketo App
* Belangrijk? Niet ingeschakeld
* Verbergen? Ingeschakeld

Velden activeren

* Geen

Waarvan gegevens afkomstig zijn

* Source-gegevens: opiniepeiling
* URL van opiniepeiling: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/stats/usage.json`

Voorbeeldresultaat

* Leeg laten

Klik **leiden de Montages van de Trekker** en plaatsen onze Testrekker om te zijn die wij zullen gebruiken om de geloofsbrieven van een gebruiker te verifiëren. **De Acties van het Zapier van 0&rbrace; &lbrace;zijn daar om gegevens uit Zapier te verzenden.** We gaan de Create_Update Lead-actie implementeren die de Marketo REST API aanroept. Met deze handeling kunnen we een nieuwe lead in Marketo maken, of als de lead al bestaat, wordt deze bijgewerkt met de verzonden waarden. We gebruiken het veld &#39;email&#39; voor deduplicatie. Klik **toevoegen Nieuwe Actie** om de tovenaar en vulling-up de volgende gebieden (de niet vermelde gebieden kunnen leeg zijn) te beginnen: Naam en Beschrijving

* Naam: maken_regel bijwerken
* Nieuw: Lood
* Sleutel: create-update-lead
* Beschrijving: Maak een nieuwe lead in Marketo of werk de lead al bij met de verzonden waarden
* Belangrijk? Ingeschakeld
* Verbergen? Niet gecontroleerd

Actievelden voor velden zijn de velden waarin gebruikers gegevens willen toewijzen. Kies deze zorgvuldig op basis van uw eigen behoeften, aangezien deze alle gegevens vertegenwoordigen die u in Marketo kunt bijwerken. In Zapier is een optie beschikbaar waarmee de eindgebruiker alle velden kan krijgen die in Marketo beschikbaar zijn, maar dat zou leiden tot meer code en complexiteit, die niet vereist zijn voor een wegwerpaansluiting. We hebben bijvoorbeeld de volgende velden geselecteerd.

De naam van de partitie is in ons geval verplicht, omdat ons Marketo-exemplaar de optie Partities voor leads in service heeft. Anders kan dit worden weggelaten. We hebben het gescheiden van de &#39;input&#39;-groep, zodat de eindgebruiker begrijpt dat dit geen veld is om te synchroniseren. Het veld Notities is afkomstig van een synch tussen Marketo en Salesforce. Gebruik het veld niet als u het niet hebt in uw Marketo-exemplaar. Het veld &#39;Called&#39; is gemaakt in onze Marketo-instantie. Gebruik dit veld niet als u het niet hebt in uw Marketo-instantie. Het doel is natuurlijk om de velden die je nodig hebt, te laten kiezen uit Marketo. Het wordt aanbevolen om klein te beginnen en de extra velden later toe te voegen. Locatie voor het verzenden van gegevens

* URL van eindpunt van handeling: `https://{{munchkin_account_id}}.mktorest.com/rest/v1/leads.json`

Voorbeeldresultaat

* Leeg laten

### Zapier-API voor scripts

Met de scriptfunctie van Zapier kunt u de aanvragen en reacties manipuleren die worden uitgewisseld tussen de API van uw app en Zapier. U kunt HTTP-aanvragen wijzigen vlak voordat ze worden verzonden en reacties parseren voordat Zapier er iets mee doet. We hebben deze nodig om de aangepaste verificatie &#39;Session Auth&#39; te voltooien, zodat deze werkt met Marketo. Meer informatie is [ hier ](https://zapier.com/developer/documentation/scripting/). Kopieer de volgende code en we zullen later een aantal uitleg geven:

```javascript
var Zap = {
     
    get_session_info: function(bundle) {
  
       console.log('Entering get_session_info method ...');
    
         var access_token,
            access_token_request_payload,
            access_token_response;

    
        // Assemble the meta data for our Access Token swap request
         console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
        access_token_request_payload = {
            method: 'POST',
            url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
            params: {
                'grant_type' : 'client_credentials',
                'client_id' : bundle.auth_fields.client_id,
                'client_secret' : bundle.auth_fields.client_secret
            },
            headers: {
                'Content-Type': 'application/json',  // Could be anything.
                Accept: 'application/json' 
            }
        };

        // Fire off the Access Token request.
        access_token_response = z.request(access_token_request_payload);

        // Extract the Access Token from returned JSON.
        access_token = JSON.parse(access_token_response.content).access_token;
        console.log('New Access_Token=' + access_token);
   
        // This will be mixed into bundle.auth_fields in future calls.
        //bundle.auth_fields.access_token=access_token;
        return {'access_token': access_token};
    },
  
  
    test_trigger_pre_poll: function(bundle) {
     
         console.log('Entering test_trigger_pre_poll method ...');
         
         bundle.request.params = {
         'access_token':bundle.auth_fields.access_token
         };
         
         return bundle.request;
        
    },
  

    test_trigger_post_poll: function(bundle) {
    
        console.log('Entering test_trigger_post_poll method ...');
        
        var data = JSON.parse(bundle.response.content);
        if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
            
           throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }

        return JSON.parse(bundle.response.content);
    },
     
    create_update_lead_pre_write: function(bundle) {
    
       bundle.request.params = {'access_token':bundle.auth_fields.access_token};  
       return bundle.request;
    },

    create_update_lead_post_write: function(bundle) {
         
         var data = JSON.parse(bundle.response.content);
         if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
            console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
            throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
        }
        return JSON.parse(bundle.response.content);
    } 
};
```

**Methoden** **get_session_info**

* Deze methode is verantwoordelijk voor het produceren van of het regenereren van een toegangstoken die het Marketo REST API [&#128279;](/help/rest-api/authentication.md) eindpunt van de Authentificatie  roepen.
* Het wordt geroepen telkens als om het even welke methode &quot;post_poll&quot;een &quot;Verlopen van het Token van de Toegang&quot;fout ontmoet. Een toegangstoken is gepland om elke 1 uur te verlopen zodat wordt dit verwacht.
* Het Eindpunt van de actie URL: https://{{munchkin_account_id}} .mktorest.com/identity/oauth/token

**pre_poll, pre_write**

* We moeten een &#39;pre-opiniepeilingsmethode&#39; maken voor elke trigger die we hebben gemaakt, om de HTTP-aanvraag te wijzigen vlak voordat deze wordt verzonden, zodat we het Marketo Access Token in de parameters kunnen toevoegen.
* Om dezelfde reden moeten we een methode van &#39;pre-write&#39; creëren voor elke actie die we hebben opgezet.

**post_poll, post_write**

* We moeten een &#39;post-opiniepeilingsmethode&#39; creëren voor elke trigger die we hebben gemaakt, om reacties te parseren voordat Zapier er iets mee doet, en uiteindelijk de fout &#39;Access Token Expired&#39; onderscheppen.
* Om dezelfde reden moeten we een methode van &#39;post-write&#39; creëren voor elke actie die we hebben opgezet.
* Als een dergelijke fout is voorgekomen, werpen wij een InvalidSessionException die Zapier zal vertellen om de authentificatie opnieuw te spelen en opnieuw de get_session_info methode uit te voeren.

U hebt toegang tot de bundellogbestanden via de API voor scripts via het menu &#39;Snelle koppelingen&#39; in de rechterbovenhoek van het scherm. Dit is echt nuttig om de manuscripten te zuiveren.

Gebruiksscenario 1: integratie van Marketo met FullContact Card Reader

Voor deze integratie maken we één Zap van FullContact naar Marketo. Met deze Zap kunt u visitekaartjes scannen met de FullContact Mobile Card Reader en de leads naar Marketo duwen.   **Zap FullContact -> Marketo** van het Dashboard van Zapier klikt de knoop &quot;maakt een nieuwe Zap&quot;.

**Trekker in Zapier**

* Kies de App FullContact
* Kies FullContact-trigger Nieuwe visitekaartje
* Verbinding maken met uw FullContact-account
* De toepassing FullContact testen

**Actie in Zapier**

* Kies de app Marketo die we eerder hebben gemaakt en deze moet worden weergegeven in Beta
* Marketo-actie &#39;Create_Update Lead&#39; kiezen
* Maak verbinding met uw Marketo-account en vul de verificatieparameters in (Munchkin-account-id, client-id, clientgeheim)
* De velden van FullContact toewijzen aan Marketo
* Uiteindelijk vult u een Partitienaam in waar de nieuwe leads moeten gaan (alleen als er partities bestaan in uw Marketo-instantie)
* De Marketo-toepassing testen
* Je zeep activeren

Opmerking: download de visitekaartjes Reader van FullContact en activeer de optie Zapier Integration van uw mobiele apparaat.

Gebruiksscenario 2: Integratie van Marketo met Google Sheets-

Voor deze integratie creëren we twee zaps. Een van Marketo tot Google Sheets en een andere van Google Sheets tot Marketo. Met deze Zap kunt u enkele van uw leads of contactpersonen synchroniseren tussen Marketo en een Google-pagina.   **Zap Marketo Webhaak -> de Bladen van Google**
Voor de eerste Zap vertrouwen we niet op een aangepaste connector voor Marketo, maar we gebruiken Marketo Webhooks en de &#39;Webhooks by Zapier&#39; Trigger. Klik op het Zapier-dashboard op de knop &#39;Nieuwe zap maken&#39;. **Trigger Deel 1 in Zapier**

* Kies de Trigger-app &#39;Webhooks&#39; van Zapier
* Schakel &#39;Catch Hook&#39; in waarmee u kunt wachten op een POST of GET naar een Zapier-URL
* Een onderliggende toets hoeft niet te worden gekozen
* Zapier produceerde een douane **webhaak URL** voor u om verzoeken te verzenden naar, en het te kopiëren in het klembord

**Webhaak in Marketo (stappen die door een Beheerder moeten worden gedaan)**

* Ga naar Beheer -> Webhaken
* Maak een nieuwe webhaak met de naam &#39;Push Lead to Zapier&#39; en bewerk de Webhaak-vorm.

Geef in het veld van de sjabloon alle velden van de lead op die u naar Zapier wilt overbrengen en gebruik de Marketo-tokens. Voor onze Gebruiksscenario&#39;s gebruiken we dezelfde velden die we voor de aangepaste Zapier-aansluiting hebben gedefinieerd en die Leads naar Marketo duwen:

```json
{
"firstName":"{{lead.First Name}}",
"lastName":"{{lead.Last Name}}",
"email":"{{lead.Email Address}}",
"phone":"{{lead.Phone Number}}",
"leadOwner":"{{lead.Lead Owner First Name}} {{lead.Lead Owner Last Name}}",
"leadOwnerEmail":"{{lead.Lead Owner Email Address}}",
"leadNotes":"{{lead.Lead Notes:default=edit me}}",
"called":"{{lead.Called}}"
}
```

* Het formulier opslaan
* Geen behoefte aan een Toewijzing van de Reactie, zodat wordt u gedaan met Webhaak

**Campagne van de Test in Marketo (stappen die door een Marketer of een Beheerder moeten worden gedaan)**

* Van de Activiteiten van de Marketing, creeer een nieuwe Slimme Campagne

Voor testdoeleinden maken we een campagne die onze Webhaak activeert telkens wanneer een status voor leads verandert in MQL. Natuurlijk kunt u Webhaak voor om het even welk ander bedrijfsdoel gebruiken.

* De slimme lijst bewerken
* Roep de Webhaak in de Stroom
* De campagne plannen
* Zorg ervoor elke lood door de stroom kan lopen telkens als
* De slimme campagne activeren

**Trigger Deel 2 in Zapier**

* Om de &#39;Webhooks by Zapier&#39; Trigger App te voltooien, moeten we de Marketo Smart Campaign één keer starten en de Webhaak in Zapier vangen
* In ons testgeval moeten we gewoon naar Marketo Lead Database gaan, een lead openen en de status ervan wijzigen in &#39;MQL&#39;

**creeer het spreadsheet in de Bladen van Google**

* Een nieuw werkblad maken
* Een werkblad maken of de standaardwerkblad gebruiken
* Een kolom toevoegen voor elk veld dat u wilt synchroniseren vanuit Marketo (de velden die zijn gedeclareerd op de Marketo-webhaak)

  **Actie in Zapier**

* De Google-bladen voor de app kiezen
* Schakel de optie Werkbladrij maken in.
* Verbinding maken met uw Google Sheets-account
* Werkblad Google-bladen selecteren
* Werkblad selecteren
* Wijs alle velden toe tussen de &#39;Webhooks by Zapier&#39; Trigger App en Google Sheets.

* De Google Sheets-app testen
* Je zeep activeren

**Zap Google Sheets -> Marketo**

Klik op het Zapier-dashboard op de knop &#39;Nieuwe zap maken&#39;.

**Trekker in Zapier**

* De app Google Sheets Trigger kiezen
* Tik op de rij &#39;Bijgewerkte spreadsheet&#39; die wordt geactiveerd wanneer een nieuwe rij wordt toegevoegd of gewijzigd in een spreadsheet
* Verbinding maken met uw Google-account
* Selecteer het werkblad waarvan u een trigger wilt maken (dit moet hetzelfde zijn als in de vorige app) en het werkblad
* Triggerkolom instellen op &#39;any_column&#39;
* De Google Sheets-app testen

**Actie in Zapier**

* Kies de app Marketo die we eerder hebben gemaakt en deze moet worden weergegeven in Beta
* Marketo-actie &#39;Create_Update Lead&#39; kiezen
* Maak verbinding met uw Marketo-account en vul de verificatieparameters in (Munchkin-account-id, client-id, clientgeheim)
* De velden van Google Sheets toewijzen aan Marketo
* Uiteindelijk vult u een Partitienaam in waar de nieuwe leads moeten gaan (alleen als er partities bestaan in uw Marketo-instantie)
* De Marketo-toepassing testen
* Je zeep activeren

### Conclusie

Hier volgen enkele ideeën voor verbetering van onze Marketo-connector voor Zapier:

* Andere triggers en handelingen toevoegen die betrekking hebben op verschillende Marketo-objecten (lijsten, aangepaste objecten, enz.)
* In plaats van de velden van Marketo hard te coderen, is het mogelijk om de velden dynamisch uit Marketo te halen, maar dat zou wat technisch vertaalwerk tussen Marketo en Zapier vereisen.
* Het delen van de schakelaar met het ontwikkelingsteam en uiteindelijk het algemeen beschikbaar maken.

Het is mogelijk dat Zapier een Premium Marketo-adapter implementeert, waardoor het veel eenvoudiger wordt om onze gebruiksgevallen te implementeren. In elk geval kan dit artikel altijd worden gebruikt om Marketo met Zapier te integreren met een gratis Zapier-abonnement en om aangepaste gebruiksscenario&#39;s te maken die mogelijk niet door een premium adapter worden ondersteund. We hopen dat u dit artikel hebt genoten en dat het u zal helpen nog meer succes te boeken met Marketo en Zapier. Bedankt!

Gepost op _2016-04-17_ door _David_

## Updates van voorjaar 2016

**REST API**

* Asset API - Webpagina&#39;s
   * **het Aanlanden Pagina&#39;s** worden nu blootgesteld via vijftien nieuwe eindpunten die het creëren, het bijwerken, het schrappen, het klonen en het ontwerp beheer voor het landen van pagina&#39;s zullen toestaan. Aanlandingspaginasjablonen bevatten nu ook conceptbeheereindpunten
      * Landingspagina&#39;s ophalen
      * Openingspagina ophalen op id
      * Openingspagina ophalen op naam
      * Openingspagina maken
      * Metagegevens bestemmingspagina bijwerken
      * Inhoud landingspagina ophalen
      * Sectie Landingspagina-inhoud toevoegen
      * Sectie Landingspagina-inhoud bijwerken
      * Sectie Landingspagina-inhoud verwijderen
      * Sectie Dynamische inhoud ophalen
      * Sectie Dynamische inhoud bijwerken
      * Concept bestemmingspagina negeren
      * Openingspagina goedkeuren
      * Concept bestemmingspagina ongedaan maken
      * Openingspagina verwijderen
   * **het Bestaan de Malplaatjes van de Pagina**
      * Concept van sjabloon voor bestemmingspagina negeren
      * Landingspaginamplate goedkeuren
      * Landingspagina-sjabloon ongedaan maken
      * Landingspagina-sjabloon verwijderen
   * **Forms** heeft 21 nieuwe vrijgegeven eindpunten die volledige verwezenlijking, het uitgeven en beheersmogelijkheden via API verstrekken. De API&#39;s ondersteunen geen wijzigingen in Forms 1.0-formulieren.
      * Forms ophalen
      * Formulier ophalen op id
      * Formulier ophalen op naam
      * Lijst met formuliervelden ophalen
      * Lijst met formuliervelden bijwerken
      * Formulier maken
      * Formulier bedankt pagina ophalen
      * Formulier Bedankt pagina bijwerken
      * Formulier bijwerken
      * Concept formulier negeren
      * Formulier goedkeuren
      * Formulier niet goedkeuren
      * Kloonformulier
      * Formulier verwijderen
      * Formulierveld bijwerken
      * Formulierveld verwijderen
      * Zichtbaarheidsregels voor formuliervelden bijwerken
      * RTF-formulierveld toevoegen
      * Veldset toevoegen
      * Veld uit veldset verwijderen
      * Beschikbare formuliervelden ophalen
      * Formulierveldposities wijzigen
      * Knop Verzenden bijwerken
   * Wanneer het gebruiken van **krijgt of doorbladert Programma&#39;s**, zal identiteitskaart van de Campagne van SFDC voor Programma&#39;s zijn teruggekeerd die met een Campagne van SFDC verbonden zijn

**de Voorwerpen van de Douane** de Voorwerpen van de Douane zullen nu datatypes van het Gebied van de Tekst steunen, die voor koordgebieden van maximaal 2000 karakters toestaan om op de gebieden van douaneobjecten van dit type worden opgeslagen. **Admin de gebruikers van het Adres van 0&rbrace; IP Whitelisting {zullen een whitelist van IP adressen nu kunnen beheren om onbevoegde toegang via APIs te verhinderen.** [ u kunt meer over deze eigenschap hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) lezen. **Admin de gebruikers van de Activiteit UI van de Douane van 0} zullen de types van Activiteit van de Douane in hun admin menu nu kunnen bepalen, en verslagen toevoegen aan lood via [ voeg de Activiteiten van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST) API toe.** [ u kunt over het bepalen van de types van douaneactiviteit hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) lezen.

GEPast op _2016-06-01_ door _Kenny_

## Updates voor zomer 2016

Voor de zomerrelease van 23 september 2016 worden drie ontwikkelaarsgerichte functies uitgebracht.

### E-mailondersteuning 2.0 in de REST API

Alle [ reeds bestaande activa APIs ](/help/rest-api/assets.md) die slechts compatibel met v1.0 E-mail en Malplaatjes waren worden nu toegelaten voor gebruik met v2.0 e-mailactiva.

### Regelafstand naar Marketo

[ de Duw Lood ](/help/rest-api/leads.md) is een afwisselende methode van de loodsynchronisatie die voor gemakkelijkere teweegbrengen in Slimme Campagnes wordt ontworpen. U kunt één enkel punt van het activiteitenlogboek tot stand brengen, een lood associëren, en het loodverslag in één vraag bijwerken. Dit werkt op dezelfde manier als één formulier dat wordt ingevuld door een lead en kan gemakkelijker worden gebruikt als een proxymethode voor het verzenden van formulieren in plaats van de bestaande methode voor het synchroniseren van leads.

### HTTP-compressie

De REST API kan nu reacties comprimeren met de standaard die is gedefinieerd in de HTTP 1.1-specificatie. Dit kan helpen de grootte van de reactie verminderen die overdrachtsnelheid zal verhogen, en bandbreedtegebruik minimaliseren.  

GEPast op _2016-09-23_ door _Kenny_

## Werken met gwagercode met Marketo

[ de kwikger-code ](https://github.com/swagger-api/swagger-codegen) is een krachtige bibliotheek van Java die zowel serverstubs als API cliënten van kwikdefinities kan produceren. Hierdoor kunnen de problemen en kosten van het genereren van klanten voor elke specifieke taal aanzienlijk worden verminderd. Om aan de slag te gaan en uw eerste klant te genereren, wilt u eerst een exemplaar van een van de Marketo-definities voor wagenopnemers vastleggen. Voer de Munchkin-id in van de instantie waarmee u wilt testen. Begin met de identiteitsdefinitie. Nu u een definitie specifiek voor uw instantie hebt, moet u swagger-codegen downloaden en installeren. Het proces is specifiek voor uw werkend systeem, en u kunt de instructies [ hier ](https://github.com/swagger-api/swagger-codegen#prerequisites) met de standaardmontages krijgen, zal codegen een cliënt die alle verstrekte eindpunten en modellen behandelt uitvoeren. Deze worden typisch beheerd door een klasse genoemd DefaultApi, die methodes bevat om de beschikbare eindpunten met voorbeelden te roepen die in een omslag &quot;docs&quot;worden verstrekt (niet alle talen omvatten malplaatjes voor dit door gebrek). Laten we nu de eerste client maken. Creeer een omslag waar u uw code wilt leven, en ga daar in uw eindzitting en gebruik het produceren bevel om een cliënt in de taal te bouwen die u wilt (wij zullen veronderstellen u de homebrew installatiemethode hebt gebruikt):

swagger-codegen genereren -i $definitionLocation -l $yourLanguage -o $yourLocation

Hierdoor wordt de clientcode naar de gewenste locatie uitgevoerd. Nu bekijken het gebruiken van dit om het identiteitseindpunt te roepen en een toegangstoken te krijgen:

```java
 public static void main(String[] args){
  String client_id = args[0];
  String client_secret = args[1];
  ApiClient client = new ApiClient();
  DefaultApi id = new DefaultApi();
  try {
   String token = id.identityOauthTokenGet(client_id, client_secret, "client_credentials").getAccessToken();
   System.out.println(token);
  } catch (ApiException e) {
   e.printStackTrace();
  }
 }
```

```php
<?php
require_once(_DIR_ . '/vendor/autoload.php');

$api_instance = new Swagger\\Client\\Api\\DefaultApi();
$client_id = "client_id_example";
$client_secret = "client_secret_example";
$grant_type = "grant_type_example";

try { 
    $result = $api_instance->identityOauthTokenGet($client_id, $client_secret, $grant_type);
    print_r($result->getAccessToken);
} catch (Exception $e) {
    echo 'Exception when calling DefaultApi->identityOauthTokenGet: ', $e->getMessage(), "\\n";
}
?>
```


```c#
  public static void Main(string[] args)
  {
      string clientId = "CHANGE ME";
      string clientSecret = "CHANGE ME";

      IdentityApi instance = new IdentityApi();
      ResponseOfIdentity response = instance.IdentityUsingGET(clientId, clientSecret, "client_credentials");

      string message = string.Format("Access Token: {0}, Expires In: {1}, Scope: {2}, Token Type: {3}",
          response.AccessToken, response.ExpiresIn, response.Scope, response.TokenType);
      Console.WriteLine(message);
  }
```

Nu we weten hoe we geautoriseerd moeten worden, zullen we in de komende weken bekijken of er meer geavanceerde gebruiksgevallen van automatisch gegenereerde clients zijn.

Gepast op _2016-10-10_ door _Kenny_

## Excel Integration Part 1: Extract &amp; Shape Marketo Data Using Power Query

Dit is het eerste artikel van een reeks artikelen waarin wordt uitgelegd hoe u de Power BI-technologie die in Microsoft Excel is ingebouwd, kunt gebruiken om een echte zelfbedieningsanalysemogelijkheid met Marketo te creëren. Met de concepten die in deze artikelen worden behandeld, kunt u:

* Gegevens uit Marketo importeren in Excel
* Gegevens uit andere bronnen importeren en combineren (SaaS-toepassingen, databases, platte bestanden, enz.)
* Vormgegevens voor zakelijke behoeften en analysedoeleinden
* Op verzoek de gegevens uit Excel vernieuwen
* Berekende kolommen en maatregelen maken met behulp van formules
* Relaties maken tussen heterogene gegevens
* Gegevens analyseren en geavanceerde rapporten samenstellen met draaitabellen en draaitekaarten
* Ongekende gegevensvisualisaties maken

### Power Query voor Excel

In dit eerste artikel wordt het importeren en vormgeven van gegevens beschreven met behulp van Power Query-technologie. De Vraag van de macht is een technologie van de gegevensverbinding die u toelaat om gegevensbronnen te ontdekken, aan te sluiten, te combineren en te raffineren om aan uw analysebehoeften te voldoen. Functies in Power Query zijn beschikbaar in Excel en Power BI Desktop. De Vraag van de macht kan met vele gegevensbronnen zoals gegevensbronnen zoals gegevensbestanden, Facebook, Salesforce, MS Dynamics CRM, enz. verbinden. Marketo wordt niet vanuit de verpakking ondersteund, maar gelukkig kunnen we Marketo REST API&#39;s gebruiken voor het op afstand uitvoeren van veel van de mogelijkheden van het systeem. Power Query wordt geleverd met een uitgebreide set formules (informeel bekend als &quot;M&quot;) waarmee u een aangepaste gegevensbron kunt scripten.

### Aangepaste connector

Het scripting van één enkele vraag van REST API is triviaal met de Vraag van de Macht, maar het wordt moeilijker om de volgende vereisten te behandelen:

* Toegangstoken beheer met inbegrip van authentificatiemechanismen en periodieke symbolenverfrissing
* Pagineringsmechanisme voor een grote set gegevens
* Foutafhandeling

In dit artikel wordt uitgelegd hoe u een robuuste aangepaste aansluiting kunt maken die de REST API&#39;s van Marketo kan gebruiken om allerlei soorten gegevens te verkrijgen (Leads, Activiteiten, Aangepaste objecten, Programma&#39;s, enz.). Uw enige beperking is de dagelijkse Marketo API-aanvraaglimiet. De hier beschreven concepten richten zich op Marketo, maar zij zouden ook kunnen worden gebruikt om andere oplossingen SaaS te integreren die REST API verstrekken.

### Vereisten

#### Power Query

Vóór de release van Excel 2016 werkte Microsoft Power Query for Excel als een Excel-add-in die werd gedownload en geïnstalleerd in Excel 2010 of Excel 2011. In Excel 2016 is deze technologie een native functie die is geïntegreerd in het &#39;Data&#39;-lint onder &#39;Get &amp; Transform&#39;-sectie. Alle scripts die voor dit artikel zijn gemaakt, zijn getest op Excel 2016 voor Windows. De concepten zouden het zelfde voor Excel 2013 of Excel 2010 moeten zijn, maar sommige aanpassingen zouden kunnen worden vereist.  Power Query is momenteel alleen beschikbaar in de Microsoft Windows-versie van Excel. De Mac-versie wordt helaas niet ondersteund.

#### Marketo

Power Query gebruikt de Marketo REST API&#39;s om toegang te krijgen tot gegevens van Marketo. Als u deze API&#39;s wilt gebruiken, hebt u een API-gebruiker en een aangepaste service nodig die u zelf kunt maken als beheerder van uw Marketo-instantie. Als niet, dan zal een beheerder die aan u moeten verstrekken. Een geleidelijke verklaring van hoe te om de Gebruiker van Marketo API en de Dienst van de Douane tot stand te brengen kan [ hier ](/help/rest-api/custom-services.md) worden gevonden. Zodra u wordt gedaan, zou u de volgende geloofsbrieven moeten hebben om Marketo REST APIs aan te halen: **Identiteitskaart van de Cliënt** en **Geheim van de Cliënt**. Het **REST API Eindpunt** kan in de REST API sectie van Admin van de Diensten van het Web in Marketo worden gevonden en het zou het volgende patroon moeten hebben:

`<https://XXX-XXX-XXX.mktorest.com/rest>`

Marketo heeft een daglimiet voor aanvragen voor de bijbehorende API en deze limiet is te vinden in de beheerder van webservices en in een verbruiksrapport. **zorg ervoor om uw dagelijkse grens nooit te overschrijden wanneer u uw vragen ontwerpt aangezien u sommige gegevens in uw rapporten** kunt missen.

### Power Query Workbook maken

Laat met een nieuw werkboek van Excel beginnen. We maken een specifiek configuratiewerkblad voor het declareren van alle Marketo REST API-instellingen. In dit aantekenvel, creëren wij drie lijsten:

Lijst &#39;**REST_API_Authentication**&#39; met de kolommen: **URL**: uw Marketo REST API Eindpunt. **identiteitskaart van de Cliënt**: van uw Marketo REST API OAuth2.0 referentie. **Geheim van de Cliënt**: van uw Marketo REST API OAuth2.0 referentie.
Lijst &#39;**Scoping**&#39; met de kolommen: **het Pagelen Token SinceDatetime**: een datum na ISO 8601 standaarddatumaantekening (bijvoorbeeld &quot;2016-10-06T13 :22: 17-08:00,&quot;2016-10 -06&quot; zijn geldige datum/tijd) die wordt gebruikt om Marketo-activiteiten op te halen sinds een bepaalde periode, dankzij een eerste &#39;date-based&#39; pagineringstoken. Deze datum wordt hoofdzakelijk gebruikt om de hoeveelheid gegevens te beperken in het werkboek in te voeren. **identiteitskaart van de Lijst**: identiteitskaart van een statische lijst in Marketo die alle lood/contacten van verwijzingen voorzien wij behandelen. Deze statische lijst kan in Marketo vrij worden beheerd (een slimme campagne kan deze periodiek of in real time van lood en contacten voorzien).
Als u de id van een statische lijst wilt ophalen, opent u deze in Marketo en haalt u de numerieke id op van de URL, bijvoorbeeld `<https://myorg.marketo.com/#ST3517A1LA1>`, Lijst-id=3511. **Max de Pagina&#39;s van Verslagen**: dit wordt gebruikt voor onze pseudo-recursieve algoritmen die door de outputgegevens van Marketo herhalen, gebruikend op positie-gebaseerde het pagineren tokens, met een capaciteit van 300 maximum verslagen per pagina. Omdat dit ons interesseert om zo veel mogelijk records per pagina te krijgen, zullen wij aan 300 blijven. Meestal betekent een Max Records-pagina ingesteld op 33,333 een capaciteit van 33,333 X 300 = 9,9999 miljoen records; maar het betekent ook 33,333 K op uw Marketo API Daily Request Limit. De algoritmen stoppen hoe dan ook zodra alle gegevens van de query&#39;s zijn verkregen. Deze parameter is dus slechts een veiligheidslimiet voor een lus.

Lijst `Leads` met de kolom: **Lood Gebieden**: komma scheidde loodgebieden om van Marketo te verzamelen wanneer het vragen van de lood en de contacten. Het declareren van een tabel in Excel is eenvoudig. Voer twee rijen in het werkblad in met de namen en waarden van de kolommen, markeer met de muis de omtrek van de tabel en selecteer de pictogramtabel in het menu &#39;Invoegen&#39; en geef deze een naam. De namen die aan de lijsten en hun kolommen worden gegeven zijn belangrijk aangezien zij direct door onze manuscripten zullen worden geroepen.

## Verificatie en toegangstoken

### Informatie over Marketo REST API-verificatie

Marketo REST API&#39;s worden geverifieerd met OAuth 2.0 met twee poten. Client ID&#39;s en Client Secrets worden geleverd door aangepaste services die u definieert. Elke douanedienst is eigendom van een API-Enige gebruiker die een reeks rollen en toestemmingen heeft die de dienst machtigen om specifieke acties uit te voeren. Een toegangstoken wordt geassocieerd met één enkele douanedienst.
Het volledige mechanisme van de Authentificatie wordt gedocumenteerd [ hier ](/help/rest-api/authentication.md) op de plaats van de Ontwikkelaar van Marketo. Wanneer een toegangstoken oorspronkelijk wordt gecreeerd, is zijn levensduur 3600 seconden of 1 uur. Elke opeenvolgende authentificatievraag voor de zelfde douanedienst keert het huidige toegangstoken met zijn resterende levensduur terug. Zodra het teken is verlopen, keert de authentificatie een gloednieuw toegangstoken terug. Het beheren van de vervaldatum van toegangstoken is belangrijk om ervoor te zorgen dat uw integratie regelmatig werkt en onverwachte authentificatiefouten tijdens normale verrichting voorkomt.

#### Query maken

Maak een nieuwe query door te klikken op het pictogram Nieuwe query in de sectie &#39;Ophalen&amp;transformeren&#39; van het menu &#39;Gegevens&#39;. Selecteer een lege vraag om met te beginnen en het een naam zoals &quot;MktoAccessToken&quot; te geven. Start de Geavanceerde Editor vanuit de Query-editor, zodat u handmatig scripts kunt uitvoeren voor bepaalde formules van Power Query. Voer de volgende code in de geavanceerde editor in:

```
let
    // Get url and credentials from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
    clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

    // Calling Marketo API Get Access Token
    getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr, 
    TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

    // Parsing access token
    accessTokenStr = TokenJson [access_token]
       
in
    accessTokenStr
```

De opmerkingen die zijn ingesloten in de broncode, voorafgegaan door &quot;//&quot;, geven de code een toelichting. Als u een functieverwijzing nodig hebt, raadpleegt u de koppelingen in de sectie Referentie van dit artikel. Klik op de knop Gereed. Controleer of het toegangstoken correct in uitvoer wordt weergegeven voor de laatste toegepaste stap &#39;accessTokenStr&#39;.  Eén korte opmerking over de beveiliging in Excel. Mogelijk wordt u gevraagd om soms Externe gegevensverbindingen in te schakelen via de gele banner. Dit wordt vereist om de Vragen behoorlijk te laten werken.

#### Een query converteren naar een functie

Ga terug naar de geavanceerde Editor en laat de code omlopen met de volgende functiedeclaratie:

```
let
    FnMktoGetAccessToken =()=>

        let
            // Get url and credentials from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
            clientIdStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client ID],
            clientSecretStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[Client Secret],

            // Calling Marketo API Get Access Token
           getAccessTokenUrl = mktoUrlStr & "/identity/oauth/token?grant_type=client_credentials&client_id=" & clientIdStr & "&client_secret=" & clientSecretStr, 
           TokenJson = try Json.Document(Web.Contents(getAccessTokenUrl)) otherwise "Marketo REST API Authentication failed, please check your credentials",

            // Parsing access token from Json 
           accessTokenStr = TokenJson [access_token]
       
        in
            accessTokenStr 

in FnMktoGetAccessToken
```

De functie neemt geen parameters in input maar krijgt die van het configuratieaantekenvel. Het produceert het toegangstoken als output. Wijzig de naam van de query `FnMktoGetAccessToken` en sla deze op. Merk op dat u al uw vragen op elk ogenblik in Excel kunt zien door de knoop &quot;tonen Vragen&quot;in de sectie &quot;krijgen &amp; van de Transformatie&quot;van het menu van Gegevens te klikken. Uw functie moet nu worden gemarkeerd met het functiepictogram &#39;Fx&#39;.

### Leden van statische lijst laden

#### Leads ophalen

De Marketo-API voor leads biedt eenvoudige CRUD-bewerkingen tegen leadrecords, de mogelijkheid om het lidmaatschap van een lead in statische lijsten en programma&#39;s te wijzigen en de verwerking van Smart Campagne voor leads te starten. Al deze mogelijkheden worden gedocumenteerd [ hier ](/help/rest-api/leads.md). Een grote set leadrecords kan worden opgehaald op basis van lidmaatschap van een statische lijst of programma. Gebruikend identiteitskaart van een statische lijst, kunt u alle loodverslagen terugwinnen die lid van die statische lijst zijn. Identiteitskaart van de lijst is een wegparameter in de vraag. Zie het hoofdstuk &quot;Lijst en programmamededeling&quot; in de documentatie voor Marketo-ontwikkelaars voor meer informatie. Het maximumaantal leadrecords dat per API-aanroep kan worden opgehaald, is 300. Daarom moeten we paginatokens gebruiken om de records per pagina van 300 records te verzamelen. We krijgen het pagineringstoken in het Json-antwoord na de eerste oproep en we weten dat we klaar zijn wanneer het pagineringstoken zich niet meer in de uitvoer bevindt.

### Standaardquery

Laten we beginnen met een volledig functionerende query die is bedoeld om alle leads van een statische lijst te downloaden. Maak een nieuwe lege query met de naam &#39;MktoLeads&#39; en voer de volgende code in de geavanceerde editor in:

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the number of iterations (pages of 300 records) - Table Scoping
    iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,
   
    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    pagingTokenParamStr = "",

    // Function iterating though the pages
    FnProcessOnePage =
    (accessTokenParamStr, pagingTokenParamStr) as record =>
        let
        
            // Send REST API Request             
            content = Web.Contents(getMultipleLeadsByListIdUrl & accessTokenParamStr & pagingTokenParamStr),
            
            // Recover Json output and watch if token is expired, in that case, regenerate access token
            newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
            getMultipleLeadsByListIdJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(getMultipleLeadsByListIdUrl & newAccessTokenParamStr & pagingTokenParamStr)),
            
            // Parse Json outputs: data and next page token     
            data = try getMultipleLeadsByListIdJson[result] otherwise null,          
            next  = try  "&nextPageToken=" & getMultipleLeadsByListIdJson[nextPageToken] otherwise null,
            res = [Data=data, Next=next, Access=newAccessTokenParamStr]
        in
            res,

    // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
    GeneratedList =
        List.Generate(
            ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
            each [i]<iterationsNum and [res][Data]<>null,
            each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
            each [res][Data])
in
    GeneratedList
```

De Vraag van de macht biedt geen traditionele het van een lus voorzien functies (b.v. For-loop, while-loop) en biedt geen ondersteuning voor recursie. Een goede oplossing is voor-lijn uit te voeren gebruikend List.Generate. Deze functie wordt gedocumenteerd [ hier ](http://msdn.microsoft.com/query-bi/m/list-generate). Met lijst. Genereren is mogelijk om de pagina&#39;s te doorlopen. Bij elke stap van de herhaling extraheren wij een pagina van gegevens, die URL houdt die het paginerende teken voor de volgende pagina omvat, en slaan de resultaten in het volgende punt van de geproduceerde lijst op. De blog van [ Datachant ](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) was een groot middel om dit op te lossen. Onze parameter &#39;Max Records Pages&#39; is hier om het aantal pagina&#39;s te beperken tot een realistisch bereik en een oneindige lus te vermijden. Een andere uitdaging is ervoor te zorgen dat het toegangstoken nooit wordt verlopen. Het bijhouden van de resterende levensduur zou te complex zijn met Power Query. Dus alle aanroepen van de REST API worden ondersteund door een foutcontrole. Als er een fout optreedt, gaan we ervan uit dat de token is verlopen en wordt deze eerst vernieuwd en wordt de aanroep opnieuw afgespeeld. Als de tweede vraag ontbreekt, dan zal de tweede mislukking aan Excel worden meegedeeld (in het slechtste geval, krijgt u geen gegevens als resultaat). Start de query nadat u deze hebt opgeslagen of klik op de knop Vernieuwen.  In ons geval werden 1364 leadrecords uitgepakt, die in vijf bladzijden met gegevens op vijf lijsten passen.

### De gegevens vormgeven

We moeten de gegevens zodanig vormgeven dat al deze records in één enkele platte lijst met records worden geplaatst. Er zijn twee manieren om dit te doen:

* Meer code gebruiken
* Gebruik de interface voor Power Query

Klik met de rechtermuisknop op het uitvoerraster en kies &#39;Naar tabel&#39; in het contextmenu om de tabel om te zetten in een lijst met lijsten.  Laat in het pop-upvenster &#39;Aan tabel&#39; de standaardwaarden in de twee picklists staan.  Vouw nu de resulterende tabel met lijsten uit. Nu hebben we alle records in één lijst. De records die in Json-indeling zijn gecodeerd, bevatten de velden en de bijbehorende waarden. Opnieuw uitvouwen. Schakel in het pop-upvenster alle velden die u wilt behouden uit en schakel het selectievakje &#39;Oorspronkelijke kolomnaam gebruiken als voorvoegsel&#39; uit.  Et voila! Alle verslagen worden mooi getoond in onze lijst. Als wij de geavanceerde redacteur opnieuw openen, kunnen wij zien dat drie lijnen van code zijn toegevoegd om onze gegevens te vormen:

```
# "Converted to Table" = Table.FromList(GeneratedList, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
# "Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
# "Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"}, {"id", "updatedAt", "lastName", "email", "createdAt", "firstName"})
```

U kunt veel meer doen met de Vraag van de Macht, als het creëren van extra kolommen met gegevens verwerkte waarden, zullen wij sommige meer mogelijkheden later zien. Sla deze query op en sluit deze. Het kan nu op elk moment handmatig worden vernieuwd of automatisch via achtergrondvernieuwingen.

### De resultaten omleiden

Nu is de vraag: waar moeten de resultaatgegevens naartoe? Houd de muisaanwijzer boven de query en selecteer het menu &#39;Laden naar..&#39; in het contextmenu. In popup, dan kunt u selecteren:

* &#39;Tabel&#39; als u alle gevormde gegevens naar een (nieuw of bestaand) werkblad wilt verzenden,
* &#39;Alleen verbinding maken&#39; als het uw bedoeling is om verdere analyse uit te voeren in de draaischijf van de voeding.

Met het selectievakje &#39;Add this to the Data Model&#39; kunt u de gegevens in de Power draaitraai gebruiken. Dit is wat we voor het tweede deel van dit artikel willen.

### Paginering beheren

Aangezien het doel van ons project is vele meer vragen te bouwen, laten wij wat refactoring doen en een herbruikbare functie halen die de paginering zou leiden. Creeer een nieuwe lege vraag genoemd **FnMktoGetPagedData** en ga de volgende code in de geavanceerde redacteur in:

```
let
    FnMktoGetPagedData =(url, accessTokenParamStr, pagingTokenParamStr)=>

    let
    
        // Get the number of iterations (pages of 300 records) - Table Scoping
        iterationsNum = Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Max Records Pages],

        // Sub-function iterating though the REST API service result pages
        FnProcessOnePage =
        (accessTokenParamStr, pagingTokenParamStr) as record =>
            let
        
                // Send REST API Request             
                content = Web.Contents(url& accessTokenParamStr & pagingTokenParamStr),
            
                // Recover Json output and watch if token is expired, in that case, regenerate access token
                newAccessTokenParamStr = if Json.Document(content)[success]=true then accessTokenParamStr else "?access_token=" & FnMktoGetAccessToken(),
                contentJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(url & newAccessTokenParamStr & pagingTokenParamStr)),
            
                // Parse Json outputs: data and next page token     
                data = try contentJson[result] otherwise null,          
                next  = try  "&nextPageToken=" & contentJson[nextPageToken] otherwise null,
                res = [Data=data, Next=next, Access=newAccessTokenParamStr]
            in
                res,

        // Generates a list of values given four functions that generate the initial value initial, test against a condition, and if successful select the result and generate the next value next. An optional parameter, selector, may also be specified
        GeneratedList =
            List.Generate(
                ()=>[i=0, res = FnProcessOnePage(accessTokenParamStr, pagingTokenParamStr)],
                each [i]<iterationsNum and [res][Data]<>null,
                each [i=[i]+1, res = FnProcessOnePage([res][Access],[res][Next])],
                each [res][Data])
    in
        GeneratedList

in FnMktoGetPagedData
```

Sla de query op. We gaan het vervolgens gebruiken.

### Vereenvoudigde query

Nogmaals onze vraag &quot;MktoLeads&quot;die **FnMktoGetPagedData** functie zal roepen.

```
let
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),
    // Get the Lead fields to extract - Table Leads
    LeadFieldsStr = Excel.CurrentWorkbook(){[Name="Leads"]}[Content]{0}[Lead Fields],


    // Build Multiple Leads by List Id URL
    getMultipleLeadsByListIdUrl = mktoUrlStr & "/rest/v1/list/" & listIdStr & "/leads.json?fields=" & LeadFieldsStr,
   
    // Build Marketo Access Token URL parameter
    accessTokenParamStr = "&access_token=" & FnMktoGetAccessToken(),

    // No initial paging token required for this call
    pagingTokenParamStr = "",

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getMultipleLeadsByListIdUrl , accessTokenParamStr, pagingTokenParamStr) 
        
in
    result
```

Zoals u kunt zien, is onze vraag nu echt eenvoudig te lezen en te handhaven. Wij gaan opnieuw hefboomwerking gebruiken **FnMktoGetPagedData** functie voor de andere vragen.

### Specifieke activiteiten laden vanaf een bepaalde periode

#### Activiteiten ophalen met paginering

Marketo staat een groot aantal verschillende soorten activiteiten toe die verband houden met loodrecords. Bijna elke verandering, actie of stroomstap wordt geregistreerd tegen het activiteitenlogboek van een lood en kan via API worden teruggewonnen of leveraged in Slimme Lijst en Slimme filters en trekkers van de Campagne. Activiteiten zijn altijd weer gerelateerd aan het hoofdrecord via de leadId, overeenkomend met de id van de record, en hebben ook een eigen unieke id voor gehele getallen. U vindt de volledige REST API documentatie [ hier ](/help/rest-api/activities.md).

Er zijn een zeer groot aantal potentiële activiteitstypen, die van abonnement aan abonnement kunnen variëren, en unieke definities voor elk hebben. Terwijl elke activiteit zijn eigen unieke id, leadId en activityDate zal hebben, zullen primaireAttributeValueId en primaryAttributeValue in hun betekenis variëren. We gaan ons richten op de Interessant Moments, een soort Marketo-activiteiten die met de ID 41 worden gevolgd. De nieuwe uitdagingen die we gaan oplossen zijn:

* We moeten een &#39;date-based&#39; paginering-token starten om de periode te bepalen waarin de activiteiten plaatsvonden,
* Het vormen van de gegevens is wat lastiger, aangezien afhankelijk van de activiteitstypes, een lijst van activiteit-specifieke attributen in Json wordt verstrekt en moet worden geparseerd en worden afgevlakt om de analyse te versnellen.

#### Op datum gebaseerd paginapunt

We moeten eerst deze functie bouwen om het eerste &#39;date-based&#39; paginering token te genereren, dat nodig is om de periode voor onze Activity query&#39;s te bepalen. U vindt hier de documentatie over het pagineren teken [&#128279;](/help/rest-api/paging-tokens.md). Creeer een nieuwe lege vraag genoemd **FnMktoGetPagingToken** en ga de volgende code in de geavanceerde redacteur in:

```
let
    FnMktoGetPagingToken =(accessTokenStr)=>

        let
            // Get url from config worksheet - Table REST_API_Authentication
            mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],

            // Get Paging Token SinceDatetime from config worksheet - Table Scoping
            mktoPTSinceDatetimeStr = DateTime.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[Paging Token SinceDatetime], "yyyy-MM-ddThh:mm:ss"),

            // Building URL for API Call
            getPagingTokenUrl = mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & accessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr, 

            // Calling Marketo API Get Paging Token
            content = Web.Contents(getPagingTokenUrl),

            // Recover Json output and watch if access token is expired, in that case, regenerate it
            newAccessTokenStr = if Json.Document(content)[success]=true then accessTokenStr else "?access_token=" & FnMktoGetAccessToken(),
            pagingTokenJson = if Json.Document(content)[success]=true then Json.Document(content) else Json.Document(Web.Contents(mktoUrlStr & "/rest/v1/activities/pagingtoken.json?access_token=" & newAccessTokenStr & "&sinceDatetime=" & mktoPTSinceDatetimeStr)),           

            // Parsing Paging Token
            pagingTokenStr = pagingTokenJson[nextPageToken]
       
        in
            pagingTokenStr 

in FnMktoGetPagingToken
```

Sla de functie op. We gaan het vervolgens gebruiken.

#### Interesserende momenten

Laten wij nu de vraag &quot;MktoInterestingMomentsActiviteiten&quot;schrijven die **FnMktoGetPagedData** en **FnMktoGetPagingToken** functies zal roepen.

```
let
    
    // Get Url from config worksheet - Table REST_API_Authentication
    mktoUrlStr = Excel.CurrentWorkbook(){[Name="REST_API_Authentication"]}[Content]{0}[URL],
    // Get the List id - Table Scoping
    listIdStr = Number.ToText(Excel.CurrentWorkbook(){[Name="Scoping"]}[Content]{0}[List ID], "D", ""),

    // Build Get Activities URL
    getActivitiesUrl = mktoUrlStr & "/rest/v1/activities.json?ListId=" & listIdStr & "&activityTypeIds=46",
   
    // Build Marketo Access Token URL parameter
    accessTokenStr = FnMktoGetAccessToken(),
    accessTokenParamStr = "&access_token=" & accessTokenStr,

    // Obtain date-based paging token used to scope in time the activities
    pagingTokenParamStr = "&nextPageToken=" & FnMktoGetPagingToken(accessTokenStr),

    // Invoke the multiple REST API calls through the FnMktoGetPagedData function
    result = FnMktoGetPagedData (getActivitiesUrl , accessTokenParamStr, pagingTokenParamStr)
   
in
    result
```

Het resultaat van deze query is opnieuw een lijst met lijsten, zodat het enige verdere gegevensverwerking nodig heeft voor analyse.

### De gegevens vormgeven

Laten we dezelfde vormende bewerkingen uitvoeren als voor de Leads:

* Klik met de rechtermuisknop op het uitvoerraster en kies &#39;Naar tabel&#39; in het contextmenu om de tabel om te zetten in een lijst met lijsten.
* Vouw de resulterende tabel met lijsten uit,
* Vouw de selectie nogmaals uit en selecteer in het pop-upvenster alle velden die u wilt behouden (schakel het selectievakje &#39;Oorspronkelijke kolomnaam gebruiken als voorvoegsel&#39; uit).

U kunt de kolommen met hun waarden zien, behalve de kolom &quot;attributen&quot;die nog een lijst van specifieke attributen verbonden aan de interessante momenten bevatten. Laten we deze kenmerken uitbreiden. Nu is de lijst uitgevouwen tot records, worden we weer uitgevouwen en selecteren we de gewenste velden (naam en waarde van elk kenmerk). Schakel het selectievakje &#39;Oorspronkelijke kolomnaam gebruiken als voorvoegsel&#39; uit.  Hierdoor zijn al onze gegevens zichtbaar, inclusief kenmerken, maar wordt elke interessante momentele activiteit over drie regels verdeeld. Dit zal moeilijk te gebruiken zijn voor onze analyse.  In het ideale geval willen we slechts één regel per activiteit, waarbij alle kenmerken als extra kolommen worden weergegeven. Dat kunnen we eenvoudig doen door de drie kenmerken van onze tafel te draaien. Selecteer de twee kolommen &#39;Naam&#39; en &#39;Waarde&#39; in de activiteitenkenmerken en klik op &#39;Draaiingskolom&#39; in het menu &#39;Transformeren&#39;. Vraag naar de opties voor voorschotten in het pop-upmenu en selecteer &#39;Kolom waarden&#39; = waarde en &#39;Niet samenvoegen&#39; waardefunctie.  Klik op OK en u moet één regel gegevens per activiteit uitvoeren.  De volgende &#39;gegevens vormend&#39; lijnen van code zouden automatisch aan het manuscript van uw vraag moeten zijn toegevoegd:

```
  #"Converted to Table" = Table.FromList(result, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandListColumn(#"Converted to Table", "Column1"),
    #"Expanded Column2" = Table.ExpandRecordColumn(#"Expanded Column1", "Column1", {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}, {"id", "leadId", "activityDate", "activityTypeId", "campaignId", "primaryAttributeValue", "attributes"}),
    #"Expanded attributes" = Table.ExpandListColumn(#"Expanded Column2", "attributes"),
    #"Expanded attributes1" = Table.ExpandRecordColumn(#"Expanded attributes", "attributes", {"name", "value"}, {"name", "value"}),
    #"Pivoted Column" = Table.Pivot(#"Expanded attributes1", List.Distinct(#"Expanded attributes1"[name]), "name", "value")
in
    #"Pivoted Column"
```

### Volgende stappen

U moet nu alle query&#39;s kunnen ontwerpen die u nodig hebt om toegang te krijgen tot specifieke Marketo-gegevens die beschikbaar zijn via de REST API&#39;s. We hopen dat u van dit artikel hebt geprofiteerd en dat het u heeft geholpen de grote voordelen van Excel en Marketo samen te benutten. Een steekproefwerkboek met alle vragen wordt ook verstrekt in het tweede artikel.

### Verwijzingen

#### Power Query

* [ Vraag van de Macht - Overzicht en het Leren ](https://support.microsoft.com/en-us/article/Power-Query-Overview-and-Learning-ed614c81-4b00-4291-bd3a-55d80767f81d)
* [ Verwijzing van de Formule van de Vraag van de Macht ](http://msdn.microsoft.com/query-bi/m/power-query-m-reference)
* [ Mat Masson Blog ](http://www.mattmasson.com/tag/power-query/) verstrekt sommige goede middelen over de Vraag van de Macht
* [ Blog DataChant ](https://datachant.com/2016/06/27/cursor-based-pagination-power-query/) is zeer nuttig voor de implementatie van het pagineringsmechanisme 

Gepast op _2016-10-18_ door _Filip_

## Updates voor herfst 2016

In de Herfst 2016-release voegen we CRUD-ondersteuning toe voor e-mailvariabelen en -modules en CRUD-ondersteuning voor benoemde accounts. U kunt de inhoud nu lokaal lezen en bewerken met Marketo REST API&#39;s en deze verplaatsen, opnieuw ordenen en verwijderen. Zie de volledige lijst met updates hieronder:

### Database-API&#39;s openen

* [**Benoemde accounts**](/help/rest-api/named-accounts.md)
   * Nieuwe eindpunten voor het lezen, bijwerken en verwijderen van benoemde accounts
   * Bekende problemen:
      * Vanaf de Fall 2016-release kunnen leads niet worden gekoppeld aan benoemde accounts via de API

### Elementen-API&#39;s

* [**E-mail** ](https://developer.adobe.com/marketo-apis/api/asset/#operation/describeUsingGET_5)
   * Nieuwe eindpunten voor het manipuleren van e-mailvariabelen v2
   * Nieuwe eindpunten voor het manipuleren van e-mailmodules v2
   * Bekende problemen:
      * Zoekopdrachten en updates voor secties die voorspellende tokens bevatten, retourneren een fout
      * E-mails met inhoudssecties die voorspellende tokens bevatten, worden mogelijk niet goedgekeurd met de API

Gepast op _2016-12-07_ door _Kenny_

## Waarom we @MarketoDev Twitter Handl3 hadden ingedrukt

We hebben besloten de greep van @MarketoDev op Twitter af te schaffen. De rekening wordt op 9 december 2011 gedeactiveerd. Vreemd waarom? **Terugspoelen terug naar begin 2014...** wij hebben enkel de Plaats van de Ontwikkelaars van Marketo gelanceerd om ontwikkelaars te helpen tegen onze APIs bouwen. We wilden het Marketo-platform meer bekendheid geven en de toegangsbelemmeringen voor ontwikkelaars verminderen. In combinatie met deze inspanning creëerden we @MarketoDev om samen te werken met onze technologiepartners en klanten die geneigd waren geïntegreerde oplossingen met Marketo te bouwen. Zoals elke dappere nieuwe onderneming, wisten we niet zeker hoe dit zou uitkomen. In eerste instantie tweetten we nieuwe blogberichten en nieuwe API-releases. De dingen waren goed; het verkeer naar de website van de Ontwikkelaars ging omhoog! We kregen ook een reeks vragen, die we naar behoren hebben beantwoord. **snel door:sturen naar eind 2016..** aangezien het [ 3&rbrace; ecosysteem van de Partner LaunchPoint &lbrace;groeide, verhoogde de hoeveelheid tweetactiviteit. ](https://exchange.adobe.com/apps/browse/ec?product=MRKTO) Als reactie op het aantal vragen die naar @MarketoDev waren gestuurd, was het een grote baan voor ons kleine team geworden. We hebben steeds meer algemene vragen over het product en de verkoop ontvangen, die we hebben doorgestuurd naar @Marketo of @MarketoCares. We hadden ook vastgesteld dat 140 tekens simpelweg niet voldoende waren voor ontwikkelingsgerelateerde vragen. Het beantwoorden van deze vaak leidde tot lange en betrokken draden van gesprek, een benadering die niet schrapte. Tot slot analyseerden we de verkeersbronnen voor bezoekers van de Developers Site en ontdekten we dat de meeste afkomstig zijn van organische zoekopdrachten en de functionaliteit &quot;Nu abonneren&quot; op onze blog. Om deze redenen hebben we besloten om de stekker op @MarketoDev te plaatsen. **Van hier op uit...** als u een Twitter-fan (en wie niet) bent, vrees niet! Onze Twitter-afdeling houdt rekening met @Marketo en met de klantenondersteuning van @MarketoCares.

Gepost op _2016-12-02_ door _David_

## Excel Integration Part 2: Ontwikkel geavanceerde Marketo-rapporten en gegevensvisualisaties met Power Pivot en Power View-

Dit is de tweede in een reeks van twee artikelen die verklaren hoe te om de technologie van Power BI te gebruiken die in Microsoft Excel wordt ingebouwd om een ware ervaring van de zelfbediening bedrijfsanalyse met Marketo tot stand te brengen. Met de concepten die in deze artikelen worden behandeld, kunt u:

* Gegevens uit Marketo importeren in Excel
* Gegevens uit andere bronnen importeren en combineren (SaaS-toepassingen, databases, platte bestanden, enz.)
* Vormgegevens voor bedrijfsbehoeften en analysedoeleinden
* Gegevens op aanvraag vernieuwen in Excel
* Berekende kolommen en maatregelen maken met behulp van formules
* Relaties maken tussen heterogene gegevens
* Gegevens analyseren en geavanceerde rapporten samenstellen met draaitabellen en draaitekaarten
* Ongekende gegevensvisualisaties maken

### Krachtige draaitrootweergave en stroomweergave voor Excel

In dit artikel, verstrekken wij voorbeelden van hoe te om het volgende te bouwen:

* Advanced Marketo rapporteert dat relaties tussen verschillende verzamelingen Marketo-gegevens worden benut met Power Pivot
* Koel statische en geanimeerde visualisaties met de Mening van de Macht

**Macht** is toe:voegen-binnen van Excel, reeds inbegrepen in Excel 2016, die u kunt gebruiken om krachtige gegevensanalyse uit te voeren en verfijnde gegevensmodellen tot stand te brengen. Met de Draai van de Macht, kunt u grote volumes van gegevens uit diverse bronnen maskeren, informatieanalyse snel uitvoeren, en inzichten gemakkelijk delen. Gegevens uit verschillende gegevensbronnen met Power Query kunnen naar het gegevensmodel, naar het Excel-werkblad of naar beide worden verzonden. In het eerste artikel importeerden en vormden we gegevens uit Marketo en stuurden het naar het gegevensmodel om een geavanceerdere analyse uit te voeren voordat het op het spreadsheet beschikbaar werd gemaakt. **Mening van de Macht** is een alternatief aan de visualisatielaag van Excel. Het is een interactieve ervaring op het gebied van gegevensverkenning, visualisatie en presentatie die intuïtieve ad-hocrapportage en dashboarding aanmoedigt.

Alle stappen die in dit artikel worden uitgelegd, zijn getest op Excel 2016 voor Windows. De concepten zouden het zelfde voor Excel 2013 of Excel 2010 (zonder de Mening van de Macht) moeten zijn maar sommige aanpassingen zouden kunnen worden vereist. Power Pivot en Power View zijn momenteel alleen beschikbaar in de Microsoft Windows-versie van Excel 2016, de Office 365-versie van Excel 2016 biedt geen ondersteuning voor volledig Power Pivot en Power View.

### De Marketo Power Workbook

#### Werkboek downloaden

In het eerste artikel hebben we het importeren en vormgeven van gegevens behandeld met Power Query-technologie. We hebben geleerd hoe we geavanceerde energievragen kunnen implementeren om leads en activiteiten van Marketo te extraheren. Sommigen van jullie willen rechtstreeks naar het punt gaan waar ze rapporten en visualisaties maken, zonder codering. Dit werkboek bevat alle vragen die in het eerste artikel en een paar meer worden gedetailleerd. We hebben de foutafhandeling verbeterd en enkele extra parameters toegevoegd aan het configuratiewerkblad. Als u het eerste artikel hebt doorlopen, raden we u nog steeds aan om het Marketo Power Workbook te downloaden en uit te checken wat er is toegevoegd.

Disclaimer: De Marketo Power Workbook is geen officieel Marketo-product en wordt daarom niet ondersteund door Marketo. Voel vrij om voor uw persoonlijke bedrijfsbehoeften te gebruiken en uit te breiden, maar doe dit op uw eigen risico.

#### Werkboek configureren

Vul alle vereiste informatie in op het Marketo-configuratiewerkblad:

* **Marketo REST API Authentificatie:** vereist
* **Scoping:** plaats het Paging Token SinceDatetime en Identiteitskaart van uw statische lijst van Marketo die alle lood bevat die u wilt analyseren
* **Leidingen:** voor de te komen rapporten, moet u minstens de volgende gebieden van de Lood specificeren: `id`, `firstName`, `lastName`, `email`, creeer `edAt`, `updatedAt`, `title`, `company`, `industry`, `inferredCountry`, `inferredCity`
   * Als de stadsgegevens in een van uw aangepaste velden nauwkeuriger zijn, kunt u in plaats daarvan uw eigen veld gebruiken
* **Activiteiten:** De types van Activiteit om van het gegevensbestand van Marketo te halen worden hier gespecificeerd voor elke reeks van de Activiteit, te hoeven om dit nu te veranderen.
   * Merk op dat wij een nutsvraag op het werkboek verstrekten dat lijsten, recht in het werkboek van Excel, alle bestaande types van Activiteit als u deze informatie later op wilt aanpassen

Let op: u ziet mogelijk enkele pop-ups met betrekking tot beveiliging. Externe verbindingen vertrouwen en deze instellen op Openbaar. Als u de onderstaande pop-up ziet, blijft u bij &#39;Anoniem&#39; webtoegangsinhoud. De authentificatie aan Marketo wordt direct beheerd door onze douanevragen, zodat te hoeven om geen ander soort toegang toe te laten.

#### Marketo-gegevens downloaden

Zorg er eerst voor dat de parameter die u definieert in het gebied Scoping van het Marketo-configuratiewerkblad er niet toe leidt dat te veel gegevens worden gedownload, waardoor de dagelijkse limiet voor Marketo API-aanvragen wordt overschreden. Wanneer klaar, klik &quot;verfrissen allen&quot;knoop van het menu van &quot;Gegevens&quot;en wacht dat alle gegevens in het werkboek worden gedownload. Als de het formatteren foutenmeldingen worden getoond wanneer het downloaden van de gegevens, gelijkend op &quot;column1 niet gevonden&quot;, betekent dat één of meerdere vragen er niet in slagen om de gegevens te krijgen, zodat het formatteren ook ontbreekt. Probeer later opnieuw, als de fout voortduurt, dan controleer uw versie van Excel (gebruik geen Excel 2016 van Bureau 365). Het is ook belangrijk dat de latentie van het Marketo-platform wordt gerespecteerd. Als u om het even welke veranderingen in een statische lijst, of in uw loodgegevens doet, dan is het verkieslijk om te wachten alvorens de Vragen van de Macht te lanceren.

### Gegevensmodellering in Power Drawing

Open het Draaien van de Macht door de knoop van &quot;Beheren&quot;van het menu van de Draaiing van de Macht te klikken, beschikbaar in de hoogste menubar (als niet beschikbaar controleer uw versie van Excel, kan de Draairichting van de Macht als toe:voegen-binnen in sommige versies van Excel worden geïnstalleerd). Alle gegevens die van Marketo zijn gedownload en naar het gegevensmodel zijn verzonden, moeten toegankelijk zijn vanaf de verschillende tabbladen onder aan het venster voor het draaien van de voeding.

### Expressies voor gegevensanalyse (DAX)

We moeten de gegevens voor bepaalde rapporten verrijken of opnieuw opmaken. Laten wij de Uitdrukkingen van de Analyse van Gegevens van de Draai van de Macht (DAX) gebruiken om sommige douaneberekeningen als berekende kolommen en maatregelen (ook te bepalen die als berekende gebieden worden bekend). Zie de koppeling &#39;DAX in Power draaitraject&#39; in de sectie Verwijzingen voor meer informatie over DAX. Zorg ervoor dat het berekeningsgebied wordt weergegeven in het venster Draaien van voeding; schakel dit in via het menu Startpagina voor roteren van stroom als dit niet het geval is.  Selecteer het **MktoLeads** lusje en voeg de **3&rbrace; maatregel van de Telling van Leads &lbrace;in het Gebied van de Berekening van Leads toe:** Ladentelling:= **&#x200B;**&#x200B;DISTINCTCOUNT&#x200B;**&#x200B;** ([ identiteitskaart ]) **.** Deze maatregel telt de verschillende leads die in de lijst beschikbaar zijn, op basis van hun id. Zij zou ook rekening houden met de eventuele filters die in het kader van een verslag worden toegepast. Deze maatregel is niet echt nodig, omdat de verslagen het aantal leads kunnen samenvatten, maar we hebben wel een aantal leads met een kortere naam dan &quot;som van MktoLeads&quot; geboekt. Het is ook een eenvoudig voorbeeld waarmee u zich eenvoudig een aantal complexere maatstaven kunt voorstellen voor gemiddelden, min, max voor een bepaald type gegevensinvoer (bijvoorbeeld alle leads met een score hoger dan 50, gemiddelde score, enz.). ...).  Nu selecteren **MktoWebActiviteiten** tabel en creeer drie berekende kolommen. Voeg de volgende berekende kolommen in door helemaal rechts van de tabel te schuiven en op de kolom &#39;Kolom toevoegen&#39; te klikken. **Activiteit:** verkrijg het gebruikersvriendelijke etiket van de Activiteit door omhoog identiteitskaart van de Activiteit in de lijst MktoActivtyTypes te kijken. **\= &#x200B;**&#x200B;**LOOKUPVALUE**&#x200B;**&#x200B; (MktoActivityTypes [ naam ], MktoActivityTypes [ identiteitskaart ], [ activityTypeId ])** **jaar-Maand:** reformat de datum van de Activiteit met een patroon &quot;YYYYYmm&quot;dat geschikter is sommige verslagen . **\= &#x200B;**&#x200B;**LEFT**&#x200B;**&#x200B; ([ activityDate ], 4) &amp;**&#x200B;**MID**&#x200B;**&#x200B; ([ activityDate ], 6, 2)** **Datum:** De Datum van de Activiteit is enkel een Koord van onze originele vraag, zet het aan een juiste datum om. **\= &#x200B;**&#x200B;**DATUM**&#x200B;**&#x200B; (**&#x200B;**LINKS**&#x200B;**&#x200B; ([ activityDate ], 4), &#x200B;**&#x200B;**MID**&#x200B;**&#x200B; ([ activityDate ], 6, 2), &#x200B;**&#x200B;**MID**&#x200B;**&#x200B; ([ activityDate ], 9, 2)** Nu creeer de drie zelfde maatregelen voor **MktoEmailActivity** tabel, en 2 extra degenen: **Campagne:** verkrijg de gebruikersvriendelijke naam van de Campagne door omhoog identiteitskaart van de Campagne in de lijst MktoCampaigns te kijken. **\= &#x200B;**&#x200B;**LOOKUPVALUE**&#x200B;**&#x200B; (MktoCampaigns [ naam ], MktoCampaigns [ identiteitskaart ], [ campagneId ])** **Programma:** verkrijg de gebruikersvriendelijke naam van het Programma door omhoog te kijken identiteitskaart van de Campagne in de lijst MktoCampagne panden. De tabel MktoPrograms kan meer details over het Programma zoals omslag, werkruimte, enz. verstrekken. **\= &#x200B;**&#x200B;**LOOKUPVALUE**&#x200B;**&#x200B; (MktoCampaigns [ programName ], MktoCampaigns [ identiteitskaart ], [ campagneId ])**

### Entiteitsrelaties

We hebben eerder een manier gezien om informatie uit een andere tabel in het model op te zoeken en ontbrekende gegevens in te vullen. Kracht Draaien biedt een krachtigere optie om de verhoudingen tussen sommige lijsten van het gegevensmodel te bepalen, die ons toestaat om die verhoudingen direct van de rapporten te gebruiken. Laten we de belangrijkste relaties voor onze verslagen definiëren. Selecteer de mening van het Diagram van het Venster van de Draairichting van de Macht. Traceer de volgende verhoudingen binnen het modeldiagram van Gegevens:

* **MktoInterestingMomentActiviteiten:leadId →** **MktoLeads:id**
* **MktoScoringActivities:leadId →** **MktoLeads:id**
* **MktoRevenueStageActivity:leadId →** **MktoLeads:id**
* **MktoWebActiviteiten:leadId →** **MktoLeads:id**
* **MktoEmailActivities:leadId →** **MktoLeads: id**

We gebruiken al deze relaties en objecten niet in onze rapporten, alleen de Leads, Webactiviteiten en e-mailactiviteiten. Nu is het tijd om wat rapporten op te stellen.

### draaitrafiek voor e-mailprestaties

Dit eerste rapport toont e-mailprestaties KPIs die op een standaardGrafiek van de Draairichting van Excel wordt gebaseerd. Het staat ons toe om gegevens door Industrie en/of Campagne te filtreren. U kunt een draaitrafiek rechts van het menu van de Draairichting van de Macht tot stand brengen door &quot;Draaiende Grafiek&quot;van de selecteur van de &quot;Draaiende Lijst&quot;te selecteren.  Een alternatief moet een Grafiek van de Draairichting van Excel direct tot stand brengen spreadsheet, die de optie &quot;het Model van Gegevens van dit werkboek gebruiken&quot;tikt.  Sleep en laat vallen de gebieden van **MktoEmailActiviteiten** en **MktoLeads** lijsten, als hieronder het cijfer: **MktoEmailActiviteiten.Activity →** **Legend** (dit gebruik de DAX berekende kolom die wij op **MktoEmailActiviteiten** vroeger uitvoerden) **Mkto EmailActiviteiten.Date →** **As** (dit gebruik de DAX berekende kolom die wij op **MktoEmailActiviteiten** vroeger) **MktoEmailActivity.Id →** **hebben uitgevoerd Cames** **MktoEmailActivities.Activities. paign →** **filter** **MktoLeads.industrie →** **Filter**

U kunt een aangepaste naam maken door &#39;Instellingen waardeveld&#39; te selecteren voor elk neergezet veld. In dit geval hebben we het veld E-mailactiviteit in de sectie &#39;Instanties waarden&#39; neergezet en de aangepaste naam als &#39;Aantal activiteiten&#39; bewerkt. Nu vormen de Grafiek van de Draairichting. Klik rechtstreeks op het diagram en selecteer de optie &#39;Type diagram wijzigen&#39; in het contextmenu. Zo hebben we het verschillende diagramtype voor alle gegevensreeksen geselecteerd.

### Leads Kaart met stroomweergave

Het tweede rapport toont uw Leidingen en Contacten door geografie op een wereldkaart en door Industrie. We hebben Power View nodig voor dit rapport. Volg de onderstaande koppeling om het menu in Excel in te schakelen. U kunt ook gewoon &#39;energieweergave&#39; typen in het Excel-zoekvak. Selecteer &#39;Een Power View-rapport invoegen&#39;.  Op het lege rapport van de Mening van de Macht, selecteer de **MktoLeads** lijst op het juiste paneel en sleep en laat vallen het gebied van de loodplaats (b.v. **inferredCity**). Het menu &#39;Design&#39; wordt nu weergegeven in het hoofdmenu.

Schakel over naar de Kaartweergave door &#39;Kaart&#39; te selecteren in het menu Ontwerp in de weergave Macht. De belemmering en laat vallen de gebieden van de **MktoLeads** lijst, als hieronder het cijfer: **MktoLeads.industrie →** **Kleur** **MktoLeads.inferredCity →** **Locaties** **MktoLeads.Leads Telling →** **de omvang van de namen** (dit gebruikt de maatregel DAX die wij op **MktoLeads** vroeger uitvoerden) en uw kaart van Leads is klaar! U hoeft alleen de grootte van de kaart aan te passen en de titel en legenda aan te passen. Met de energieweergave kunt u geavanceerde dashboards maken met meerdere grafieken op één spreadsheet. Controle uit het referenced leerprogramma hieronder &#39; [ leidt tot de Verbluffende Rapporten van de Mening van de Macht ](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)&#39; om te zien hoe te met meer dashboardcomponenten met de Mening van de Macht te werk te gaan.

### Webactiviteiten die worden geanimeerd op een 3D-kaart

In dit derde rapport worden uw toonaangevende webactiviteiten per branche weergegeven op een 3D-wereldkaart. We hebben een 3D-kaart nodig voor dit verslag. Typ gewoon &#39;3D&#39; in het Excel-zoekvak en selecteer &#39;3D-kaart&#39;. Maak een nieuwe rondleiding in het pop-upvenster.  Selecteer het Bubblediagram in het rechterdeelvenster. Sleep en laat vallen de gebieden, van **MktoLeads** en **MktoWebActiviteiten** lijsten, als hieronder het cijfer: **MktoLeads.industrie →** **Categorie** **MktoLeads.inferredCity →** **Plaats** Mkto2&rbrace; ktoWebActiviteiten → **&#x200B;**&#x200B;Tijd **(dit gebruik de DAX berekende kolom wij op** MktoWebActiviteiten **vroeger uitvoerden.** Het id gebied kon ook voor tellende activiteiten worden gebruikt.) **MktoWebActiviteiten.Date →** **Tijd** (dit gebruik de DAX berekende kolom die wij op **MktoWebActiviteiten** vroeger) **MktoWebActiviteiten.Activity** uitvoerden kan ook als filter worden gebruikt om de verschillende soorten Webactiviteiten uit te filtreren.

Met de knop Thema&#39;s kunt u het kleurenschema van uw 3D-kaart wijzigen. Open de optie Scèneopties om uw animaties aan te passen.
En je bent klaar met de 3D World Map, nu kun je plezier hebben om de wereldbol te animeren en er video van te maken.

### Volgende stappen

We hebben net gekrast wat we kunnen doen met de Excel Power BI tools. Wij adviseren u om het Web naar andere grote artikelen en leerprogramma&#39;s te zoeken om uw vaardigheden van Excel uit te breiden en de rapporten te ontwerpen u uw bedrijfsdoelstellingen moet bereiken. We hopen dat u van deze artikelen hebt genoten en dat ze u hebben geholpen de geweldige voordelen van Excel en Marketo samen te benutten.

### Verwijzingen

#### Draaien van voeding

* [ Macht Draaien: Krachtige gegevensanalyse en gegevensmodellering in Excel ](https://support.microsoft.com/en-us/article/Power-Pivot-Powerful-data-analysis-and-data-modeling-in-Excel-d7b119ed-1b3b-4f23-b634-445ab141b59b)
* [ Uitdrukkingen van de Analyse van Gegevens (DAX) in de Draai van de Macht ](https://support.microsoft.com/en-us/article/Data-Analysis-Expressions-DAX-in-Power-Pivot-bab3fbe3-2385-485a-980b-5f64d3b0f730)

#### Energieweergave

* [ Draai-op de Mening van de Macht in Excel 2016 ](https://support.microsoft.com/en-us/article/Turn-on-Power-View-in-Excel-2016-for-Windows-f8fc21a6-08fc-407a-8a91-643fa848729a)
* [ Leerprogramma: Creeer de Verbluffende Rapporten van de Mening van de Macht ](https://support.microsoft.com/en-us/article/Tutorial-Create-Amazing-Power-View-Reports-Part-1-e2842c8f-585f-4a07-bcbd-5bf8ff2243a7)

Gepast op _2017-02-02_ door _Filip_

## Belangrijke wijziging in activiteitenrecords in Marketo API

**Nota: Deze post zal worden bijgewerkt om op veranderingen te wijzen die aan activiteitenverslagen worden aangebracht door API wegens migratie aan nieuwe infrastructuur zijn teruggekeerd.** **Laatste Update: 13 september, 2018** met de uitrol van de volgende-generatiedienst van de Activiteit van Marketo die in September 2017 begint, zullen wij niet de uniciteit of de aanwezigheid van het geheel &quot;identiteitskaart&quot;gebied in activiteiten, veranderingen van de gegevenswaarde, of de verslagen van de loodschrapping kunnen afdwingen die door Marketo APIs zijn teruggekeerd. Om onderbreking van de dienst voor integraties te vermijden die activiteitenverslagen terugwinnen, zou het id gebied als facultatief moeten worden behandeld. Overboeking van deze wijziging zal gevolgen hebben voor abonnementen en komende release. Deze wijziging heeft invloed op de volgende eindpunten: REST API


De betrokken SOAP-typen zijn `ActivityRecord` en `LeadChangeRecord` .

### Voorbeelden

In de volgende voorbeelden worden de recordtypen weergegeven die worden beïnvloed. In beide voorbeelden wordt het veld &#39;id&#39; genoemd.

**Voorbeeld REST Gebied: id**

```json
  {
      "id" : 102988,
      "leadId" : 1,
      "activityDate" : "2015-01-16T23:32:19Z",
      "activityTypeId" : 1,
      "primaryAttributeValueId" : 71,
      "primaryAttributeValue" : "localhost/munchkintest2.html",
      "attributes" : [
          {
              "name" : "Client IP Address",
              "value" : "10.0.19.252"
          },
          {
              "name" : "Query Parameters",
              "value" : ""
          },
          {
              "name" : "Referrer URL",
              "value" : ""
          },
          {
              "name" : "User Agent",
              "value" : "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
          },
          {
              "name" : "Webpage URL",
              "value" : "/munchkintest2.html"
          }
      ]
  }
```

**het Gebied van SOAP van het Voorbeeld: id**

```xml
  <activityRecord>
    <id>1030680</id>
    <activityDateTime>2013-07-09T16:44:28-05:00</activityDateTime>
    <activityType>Visit Webpage</activityType>
    <mktgAssetName>ClickDemo</mktgAssetName>
    <activityAttributes>
      <attribute>
        <attrName>Webpage ID</attrName>
        <attrType xsi:nil="true" />
        <attrValue>2547</attrValue>
      </attribute>
      <attribute>
        <attrName>Webpage URL</attrName>
        <attrType xsi:nil="true" />
        <attrValue>/ClickDemo.html</attrValue>
      </attribute>
    </activityAttributes>
    <campaign />
    <personName xsi:nil="true" />
    <mktPersonId>1089965</mktPersonId>
    <foreignSysId xsi:nil="true" />
    <orgName xsi:nil="true" />
    <foreignSysOrgId xsi:nil="true" />
  </activityRecord>
```

Gepast op _2017-03-01_ door _Kenny_

## Updates van winter 2017

In de release Winter 2017 voegen we de mogelijkheid toe om aangepaste objectgegevens asynchroon te importeren en variabelen op geleide bestemmingspagina&#39;s te manipuleren. Zie de volledige lijst met updates hieronder.

### Database-API&#39;s openen

#### Bulkimport van aangepaste objecten

Nieuwe eindpunten ter ondersteuning van Bulk Import of Custom Objects. De details kunnen [ hier ](/help/rest-api/custom-objects.md) worden gevonden.

#### Kennisgeving van aanstaande verandering in activiteiten

Houd rekening met een belangrijke wijziging die optreedt wanneer Marketo de Next-Generation Activity Service loslaat. Deze wijziging heeft invloed op de volgende eindpunten:

SOAP

[ getLeadActivity ](/help/soap-api/getleadactivity.md), [ getLeadChanges ](/help/soap-api/getleadchanges.md)

Het veld integer &quot;id&quot; in records die door deze eindpunten worden geretourneerd, wordt niet langer gegarandeerd uniek. Dit zal activiteit, de verandering van de gegevenswaarde, en de types van het loodschrappingsverslag beïnvloeden. Om onderbreking van de dienst voor integraties te vermijden die deze verslagtypes terugwinnen, zou het id gebied als facultatief moeten worden behandeld.

#### Token verwijderen

De mogelijkheid om pushtokens te verwijderen via de SDK API is toegevoegd. De details kunnen hier worden gevonden: [ iOS ](/help/mobile/push-notifications.md), [ Android ](/help/mobile/push-notifications.md).

Gepost op _2017-03-01_ door _David_

## Updates van voorjaar 2017

In de release van het voorjaar van 2017 voegen we de mogelijkheid toe om lood- en activiteitenobjectgegevens asynchroon uit te pakken en benoemde accountlijsten te manipuleren. Zie de volledige lijst met updates hieronder.

### Bulkextractie van paden

Nieuwe eindpunten ter ondersteuning van de winning van lood in bulk. Geef criteria voor recordselectie op met behulp van diverse opties. De details kunnen [ hier ](/help/rest-api/bulk-lead-extract.md) worden gevonden.

### Bulkextractie van activiteiten

Nieuwe eindpunten ter ondersteuning van de winning van &quot;Activiteiten in bulk&quot;. Geef criteria voor recordselectie op met gebruik van datumbereik en activiteitenlijst. De details kunnen [ hier ](/help/rest-api/bulk-extract.md) worden gevonden.

### Lijsten met benoemde accounts

Nieuwe eindpunten ter ondersteuning van CRUD-bewerkingen in lijsten met benoemde accounts. De details kunnen [ hier ](/help/rest-api/named-account-lists.md) worden gevonden.

### Overige verbeteringen

* Het [ krijgt Campagnes ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignByIdUsingGET) eindpunt nu staat u toe om &quot;trekbare&quot;campagnes te filtreren. Dit wordt bereikt door &quot;isTriggerable=true&quot;als vraagparameter door te geven.
* Het [&#128279;](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) eindpunt van het Programma van de Kloon  &lbrace;steunt nu programma&#39;s die alle activa behalve het Bericht van SMS bevatten.

### Ionic

U kunt Marketo Mobile MME en met het [ Ionic ](https://ionicframework.com/) toepassingskader nu gebruiken. De details kunnen [ hier ](/help/mobile/ionic.md) worden gevonden.

### Token voor pushmelding verwijderen

Er is een nieuwe methode toegevoegd aan de SDK waarmee u de registratie van de token voor pushberichten bij Marketo kunt ongedaan maken. Dit is bijvoorbeeld handig wanneer een gebruiker zich afmeldt bij uw app. Voor gebruik zie `unregisterPushDeviceToken` (iOS) of `uninitializeMarketoPush` (Android) methode [ hier ](/help/mobile/push-notifications.md).

Gepost op _2017-06-16_ door _David_

## Internet van dingen voor Marketers met IFTTT en Zapier

Het internet van de dingen (IoT) is de onderlinge netwerkverbinding van aangesloten apparaten, apparaten, mobiele apparatuur, voertuigen, enz. met ingebouwde elektronica, software, sensoren, en netwerkconnectiviteit die deze voorwerpen toelaten om gegevens met wolkeninformatiesystemen te verzamelen en uit te wisselen. Deze technologieën groeien en trenderen zo snel dat ze van invloed zijn op hoe we leven, hoe we werken en hoe we zaken doen in een tijd. Marketo het toonaangevende Marketing Engagement Platform is klaar voor de IoT met zijn mogelijkheden om met om het even welke vorm van communicatie kanaal te schrapen en in wisselwerking te staan. Marketo kan reeds meer dan 70 types van activiteiten met betrekking tot e-mail, Web, mobiel, CRM, enz. volgen en het steunt ook [ douaneactiviteiten ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-activities/create-a-custom-activity.html?lang=nl-NL) die door om het even welk derdesysteem kunnen worden gevoed. Marketo [ douanevoorwerpen ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/marketo-custom-objects/understanding-marketo-custom-objects.html?lang=nl-NL) maken het mogelijk om alle soorten 3de partijmetriek met betrekking tot uw zaken te volgen, en staat marketers toe om die metriek recht van Marketo slimme campagnecilters en trekkers te hefboomwerking. Voor de implementatie van IoT voor consumenten zou een gecentraliseerde server nodig zijn voor interactie met consumentenapparaten en deze server zou gegevens uitwisselen met het open platform van Marketo, met mogelijkheden zoals REST API, Custom Objects, Custom Activity, enz. - gedocumenteerde [ hier ](http://eto.com/). Niet gemakkelijk te demonstreren via een blogbericht. In plaats daarvan gaan we de IFTTT-service integreren met Marketo om een aantal coole IoT-gebruiksscenario&#39;s voor de marketers te implementeren, zoals:

* Het keurig uw team van de Marketing telkens als een lood aan een wegshow wordt geregistreerd door een gekleurd licht in het bureau te knipperen
* Het keurig uw team van de Verkoop telkens als een overeenkomst wordt gewonnen door automatisch een bel te bevestigen die aan een aangesloten machtstekker wordt gestopt
* Automatisch mijlpalen voor het succes van de marketing plaatsen op sociale netwerken zoals LinkedIn, Facebook, Slack, enz. ...
* Automatisch een marketingcampagne starten op basis van:
   * wanneer een weerwaarschuwing optreedt (wind, temperatuur, regen, enz.)
   * wanneer een nieuw artikel wordt gepubliceerd door een krant als de New York Times, die voldoet aan een aantal specifieke criteria
   * wanneer de Senaat of het Huis van Afgevaardigden van de Verenigde Staten
   * wanneer het internationale ruimtestation een bepaalde locatie overgaat
   * enz. ...

Je vindt die scenario&#39;s misschien leuk maar nutteloos, maar ze zijn hier om een nieuwe conceptuele manier te demonstreren om marketing niet alleen met mensen, maar ook met dingen in onze verbonden wereld te doen. Een ander interessant punt dat in dit artikel wordt besproken, is hoe u een open platform voor webintegratie, zoals Zapier, kunt gebruiken als een &quot;serende luik&quot; tussen een systeem van derden en Marketo, bijvoorbeeld om de verificatie te beheren.

### De IFTTT-service

IFTTT is een acroniem voor &quot;ALS dit toen dat&quot;doet. Het is een gratis webservice die mensen gebruiken om ketens te maken van eenvoudige voorwaardelijke instructies, applets genaamd. Een applet wordt teweeggebracht door veranderingen die binnen sommige diensten van het partnerWeb voorkomen en dientengevolge, worden de acties verzonden naar de andere diensten van het partnerWeb. IFTTT werd in 2011 gelanceerd door Linden Tibbets, Jesse Tane, Scott Tong en Alexander Tibbets in San Francisco. Op eerste zicht, kijkt IFTTT gelijkaardig aan de dienst als [ Zapier ](https://zapier.com/) bijvoorbeeld, heeft het een veel sterkere nadruk op consumenten en apparaten IoT (verre, alarm, lichten, thermostaten, auto&#39;s, printers, mobiele telefoons, en zo veel meer).  Eerst van alles, moet u een rekening voor IFTTT van de [ IFTTT website ](https://ifttt.com/explore) tot stand brengen. U kunt alle beschikbare coole applets ontdekken, zodat u zeker nog andere scenario&#39;s krijgt.

### Het Maker-kanaal

Een Webtoepassing die geen kanaal heeft, betekenend een partnerschap met IFTTT, moet het Kanaal van de Maker gebruiken. Met het Maker-kanaal kunt u applets maken die werken met elk apparaat of elke app die een webaanvraag kan maken of ontvangen. Het biedt de volgende integraties aan:

* Binnenkomende triggers die een webaanvraag van een systeem van derden ontvangen om een actie te activeren
* Uitgaande Acties om een Webverzoek aan een systeem van de derde openbaar te maken dat op Internet

Van IFTTT, onderzoek naar de &quot;Maker&quot;dienst en klik op het.  De eerste keer moet u het activeren door op de knop &quot;Verbinden&quot; te klikken.  Nu is het Maker Kanaal actief. U kunt uw geheime sleutel verkrijgen door op de knoop van de Montages van de Maker te klikken: Exemplaar en kleef verstrekte URL aan uw browser voor meer details.

### Rechtstreeks een IFTTT-handeling vanuit de markt activeren

Ten eerste zullen we ons richten op het initiëren van allerlei webserviceacties van derden vanuit Marketo. Voor dat gaan wij a [ Marketo Webhaak ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/administration/additional-integrations/create-a-webhook.html?lang=nl-NL) gebruiken. We beginnen met een pushbericht op uw mobiele telefoon of tablet via de mobiele IFTTT-app en implementeren vervolgens een IoT-scenario dat een Philips Hue-licht knippert.

### Marketo Webhaak

Het is eenvoudig om een gebeurtenis vanuit Marketo te activeren, waarbij wordt gehandeld als &#39;if&#39; van IFTTT. U hoeft alleen een POST-webaanvraag naar IFTTT te verzenden met een naam van de gebeurtenis en uw geheime sleutel, volgens de URL van dit patroon:

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}>`

De Maker maakt het ook mogelijk om tot 3 parameters via het Webverzoek mee te delen. Dit kan worden gedaan gebruikend vraagparameters,

`<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={value1}&value2={value2}&value3={value3}>`

of met behulp van een JSON-body die uit maximaal drie waarden bestaat:

`{ "value1" : "{value1}", "value2" : "{value2}", "value3" : "{value3}" }`

In Marketo, creeer een nieuwe Webhaak van de interface Admin.  Geef de volgende informatie op voor uw nieuwe webhaak:

**Naam Webhaak:** Het Succes van het Programma IFTTT

**Beschrijving:** Trigger een gebeurtenis op IFTTT van een Slimme Campagne voor het Succes van het Programma

**URL:** `<https://maker.ifttt.com/trigger/{event_name}/with/key/{secret_key}?value1={{program.name}}&value2={{lead.Email> Address}}&value3={{lead.Full Name}}`

event_name, gebruik MarketoProgramSuccess bijvoorbeeld

geheime_key, gebruik de geheime sleutel van uw Dienst van de Maker IFTTT

Gebruik statische tekst of Marketo-tokens voor de drie beschikbare waarden. U kunt meer interactieve berichten duwen door uw eigen tokens op programmaniveau te bepalen en hen door deze waarden over te gaan.

**Type van Verzoek:** POST

**Malplaatje:** verlaten leeg

**Symbolische Codering van het Verzoek:** Vorm/Url

**Type van Reactie:** niets

U hoeft geen responstoewijzing te definiëren.

### IFTTT-applet

Selecteer Mijn applets in het hoofdmenu van de IFTTT-webportal.  Klik de knoop &quot;Nieuwe applet&quot;en klik op de sectie **+this**.  Zoek naar de dienst van de Maker.  Creeer de Trekker die zal in brand steken telkens als de dienst van de Maker een Webverzoek ontvangt om het van een gebeurtenis op de hoogte te brengen. Gebruik dezelfde naam voor de gebeurtenis als de naam die u hebt opgegeven in de URL van uw Marketo Webhaak, bijvoorbeeld &quot;MarketoProgramSuccess&quot; en klik op de knop trigger maken.  Nu is het tijd om de Dienst van de Actie te specificeren door de sectie **+dat** te klikken.  We beginnen met een eenvoudige actiedienst die iedereen zou kunnen testen zonder in om het even welke apparaten te hoeven investeren IoT, de Notifications Service. Zoek en selecteer de Notifications Service.
Kies de actie &quot;verzend een bericht&quot;dat een bericht naar uw apparaten verzendt.  U kunt de drie waarden die u van Marketo hebt verzonden benutten door deze als ingrediënten toe te voegen voor een betekenisvolle melding aan de gebruiker, net als in het onderstaande voorbeeld... en vervolgens op de knop &quot;Handeling maken&quot; te klikken. Bekijk en voltooi het IFTTT-applet. Zorg ervoor dat deze functie is ingeschakeld.

### Het IFTTT-applet testen

Als u op de hoogte wilt worden gesteld van uw mobiele telefoon, moet u eerst de IFTTT-app voor uw apparaat downloaden.  U kunt een Marketo Program Success-gebeurtenis activeren door de Webhaak in een Marketo Smart Campagne Flow te gebruiken. Houd er rekening mee dat Marketo Webhooks uitsluitend werken met getriggerde slimme campagnes (bijvoorbeeld activeren wanneer een formulier met contactgegevens eenmaal is ingevuld en aan een lijst is toegevoegd, enz.).  Hier is een voorbeeld van een IFTTT-melding op je mobiele telefoon.

### Laten we Creative ophalen met IFTTT

IFTTT biedt de Acties van de applet met meer dan 300 partners aan, zodat zijn uw portefeuille van apps en apparaten samen met uw verbeelding de grenzen... Neem een voorbeeld met de [ Lichten van de Kleurtoon van Philips ](https://www.philips-hue.com/en-us) dat u overal in elektronicawinkels of online kunt kopen. De volgende applets knipperen een van uw lichten met de huidige toegewezen kleur als Marketo een programma succesvol start. Dit kan uw marketingteam op kantoor stimuleren. We maken een nieuwe Applet, in dezelfde stappen als voorheen, waar Marketo wordt geactiveerd met een webhaak, maar deze keer kiezen we de actie van de Philips Hue-service.

Laten we de actie &#39;Knipperlichten&#39; selecteren. De app vraagt van Philips Hue al uw beschikbare lichten aan, zodat u de ene kunt kiezen om te knipperen. U moet eerst een account openen bij Philips Hue, de Hue-bridge en natuurlijk ten minste één Hue-lamp, lichtstrip, projector of lamp.  We hebben zojuist een nieuw Applet toegevoegd dat een gekleurd licht knippert telkens wanneer een lead wordt geregistreerd bij een roadshow of webinar. Uw marketing team zal elke dag met die opstelling in het bureau juichen.

### Marketo-actie uitvoeren vanuit IFTTT via Zapie

Nu gaan we een Marketo Smart Campaign starten vanuit het IFTTT Platform. Voor dat gaan wij [ Marketo REST API ](/help/rest-api/rest-api.md) gebruiken. Aangezien deze API wordt beveiligd en een Authentificatie OAuth2 voorafgaand vereist om om het even wat aan te halen, moeten wij die authentificatie via een ander platform zoals Zapier behandelen, omdat IFTTT niet toestaat om twee opeenvolgende vraag op API met het Kanaal van de Maker te ketenen. Wij kozen [&#128279;](https://zapier.com/) de Dienst van de Automatisering van de Web-app van Zapier  Zapier &lbrace;omdat wij reeds introduceerden en die stap voor stap verklaren hoe te om een schakelaar van de douaneMarketo voor Zapier uit te voeren. Andere platforms zoals [ Werkato ](https://www.workato.com/integrations/marketo) zou een oplossing ook kunnen zijn.

### Marketo-campagne

Maak uw Marketo-programma met een geplande slimme campagne. Voor testdoel, kon u de volgende Slimme Campagne als voorbeeld tot stand brengen: **Slimme Lijst** slechts filters van het Gebruik, niet trekkers. Zorg ervoor dat je ten minste in aanmerking komt. **Stroom** verzendt u een e-mail of een ander soort bericht. **Programma** zorg ervoor u door de stroom kunt lopen telkens om uw veelvoudige tests te behandelen. U kunt de slimme campagne-id opvragen via de URL. Voorbeeld: _https:// {{marketo_url}}/#SC4289A1_ - Slimme identiteitskaart van de Campagne zou 4289 zijn. U kunt deze campagne activeren via de Marketo REST API. U kunt bijvoorbeeld de [ insteekmodule van Postman ](https://www.postman.com/) voor Chrome gebruiken en de 2 volgende opeenvolgende vraag verzenden HTTPS: **de stap van de Authentificatie:**

`https://{{Your Munchkin_Account_id}}.mktorest.com/identity/oauth/token?grant_type=client_credentials&client_id={{Your_Client_Id}}&client_secret={{Your_Client_Secret}}`

Herstel het toegangstoken van de reactie JSON. **de schop-off stap van de Campagne:**

`https://{{Your_Munchkin_Account_id}}.mktorest.com/rest/v1/campaigns/{{Campaign_Id}}/schedule.json?access_token={{access_token}}`

### Tussenliggende Zapier Custom Connector om de Marketo Campagne te starten

We moeten een aangepaste Zapier-connector maken die wordt geverifieerd met de Marketo REST API en onze Smart Campaign uitschakelt.

* Vereisten
* Implementatie van de Marketo-connector voor Zapier
* Een andere titel gebruiken, zoals &quot;Marketo Campaign&quot;
   * Voer de stap Verificatie uit
   * Voer de stap &quot;Triggers&quot; uit (vereist voor Zapier-tests)
   * Voer de volgende specifieke stap &quot;Acties&quot; uit, die verantwoordelijk is voor het starten van een Marketo-campagne, zoals hieronder wordt uitgelegd:


Waar moet URL van eindpunt van handeling van gegevens worden verzonden:

` https://{{munchkin_account_id}}.mktorest.com/rest/v1/campaigns/{{CampaignId}}/schedule.json`

Laat de andere optionele velden leeg.

#### Scripting-API

Met de scriptfunctie van Zapier kunt u de aanvragen en reacties manipuleren die worden uitgewisseld tussen de API van uw app en Zapier. U kunt HTTP-aanvragen wijzigen vlak voordat ze worden verzonden en reacties parseren voordat Zapier er iets mee doet. We hebben het nodig om de aangepaste verificatie &#39;Session Auth&#39; te voltooien. Meer informatie is beschikbaar in het originele artikel. Kopieer de volgende code zeer gelijkend op origineel, wij veranderden enkel de actiemethodes:

```javascript
var Zap = {
 
 get_session_info: function(bundle) {
 
 console.log('Entering get_session_info method ...');
 
 var access_token,
 access_token_request_payload,
 access_token_response;

 
 // Assemble the meta data for our Access Token swap request
 console.log('building Request with client_id=' + bundle.auth_fields.client_id + ', and client_secret=' + bundle.auth_fields.client_secret);
 access_token_request_payload = {
 method: 'POST',
 url: 'https://' + bundle.auth_fields.munchkin_account_id + '.mktorest.com/identity/oauth/token',
 params: {
 'grant_type' : 'client_credentials',
 'client_id' : bundle.auth_fields.client_id,
 'client_secret' : bundle.auth_fields.client_secret
 },
 headers: {
 'Content-Type': 'application/json', // Could be anything.
 Accept: 'application/json' 
 }
 };

 // Fire off the Access Token request.
 access_token_response = z.request(access_token_request_payload);

 // Extract the Access Token from returned JSON.
 access_token = JSON.parse(access_token_response.content).access_token;
 console.log('New Access_Token=' + access_token);
 
 // This will be mixed into bundle.auth_fields in future calls.
 //bundle.auth_fields.access_token=access_token;
 return {'access_token': access_token};
 },
 
 test_trigger_pre_poll: function(bundle) {
 
 console.log('Entering test_trigger_pre_poll method ...');
 
 bundle.request.params = {
 'access_token':bundle.auth_fields.access_token
 };
 
 return bundle.request;
 
 },
 
 test_trigger_post_poll: function(bundle) {
 
 console.log('Entering test_trigger_post_poll method ...');
 
 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
 
 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }

 return JSON.parse(bundle.response.content);
 },
 
 launch_campaign_pre_write: function(bundle) {
 
 bundle.request.params = {'access_token':bundle.auth_fields.access_token}; 
 return bundle.request;
 },

 launch_campaign_post_write: function(bundle) {
 
 var data = JSON.parse(bundle.response.content);
 if ((!data.success)&&((data.errors[0].code=="601")||(data.errors[0].code=="600"))){
 console.log('Access Token expired or invalid, requesting new one - data.success=' + data.success + ', data.errors[0].code=' + data.errors[0].code);
 throw new InvalidSessionException(); // Calling get_session_info() to regenerate Access Token
 }
 return JSON.parse(bundle.response.content);
 }
 
};
```

##### Nieuwe zap

Klik op het Zapier-dashboard op de knop &quot;Nieuwe zap maken&quot;. **Trekker**

* Kies de Trigger-app &quot;Webhooks op Zapier&quot;
* &quot;Vangsthaak&quot; controleren
* Een onderliggende toets hoeft niet te worden gekozen
* Zapier genereerde een aangepaste web-haak-URL waarnaar u aanvragen kunt verzenden, zodat u deze ergens veilig kunt houden
* Test de URL van de webhaak door het IFTTT-applet te starten dat het onderstaande Zapier Webhaak-scenario aanroept. Zo kan Zapier meer weten over de payload van de webhaak en kunt u de campagne-id aan de handeling toewijzen

**Actie**

* Selecteer eerder gemaakte Marketo Campaign-connector
* Kies de enige beschikbare actie: **Campagne van de Lancering**
* Maak verbinding met uw Marketo-account en vul de verificatieparameters in (Munchkin-account-id, client-id, clientgeheim)
* Bewerk de sjabloon en koppel de campagne-id van de trigger aan de parameter Campagne starten
* Test de stap en controleer of de Marketo-campagne wordt gestart

### IFTTT Applet dat Zapier Webhaak roept

We beginnen met een eenvoudig scenario dat gemakkelijk te testen is. We kiezen in IFTTT een datum- en tijdtrigger die elk uur de Marketo-campagne start. De handeling is een webaanvraag die naar de Zapier Webhaak-URL wordt gepost en die de slimme campagne-id doorgeeft. Zorg ervoor dat de Zapier Zap en de IFTTT Applet beide actief zijn en test of alles werkt zoals u had verwacht.

### Laten we Creative ophalen met IFTTT

IFTTT biedt de Trekkers van Applet met meer dan 300 partners aan, zodat opnieuw zijn uw portefeuille van apps en apparaten samen met uw verbeelding de grenzen... Neem een voorbeeld met de [ Weather Ondergrond ](https://www.wunderground.com/) dienst die wij zullen gebruiken om onze campagne van Marketo op weeralarm te lanceren. De volgende trigger wordt geactiveerd wanneer een Regenvoorwaarde wordt aangekondigd. Dan associeer de Trekker met de Actie van Webhaak van de Maker, en enkel zoals eerder vult de Webhaak Zapier parameters in.  Et voila, je hebt nu een goede regen nodig om te komen controleren of dit werkt.

We hopen dat u veel plezier hebt met het toepassen van de concepten in dit artikel. Maar het belangrijkste is dat het iedereen helpt die Marketo wil integreren met andere systemen van derden, dankzij de belangrijkste concepten uit dit artikel:

* MARKETO REST API
* Marketo Webhooks
* Hoe u een open webintegratieplatform zoals Zapier kunt gebruiken als een &#39;serende hatch&#39; tussen een systeem van derden en Marketo, bijvoorbeeld om de verificatie te beheren

Gepast op _2017-06-20_ door _Filip_

## Updates van zomer 2017

In de zomer van 2017 brengen wij enkele kleine verbeteringen aan onze programma-APIs vrij.

### Door programma&#39;s bladeren

Wij voegen de capaciteit toe om programma&#39;s door datumwaaier aan ons [ te krijgen krijgt het eindpunt van Programma&#39;s ](https://developer.adobe.com/marketo-apis/api/asset/#operation/browseProgramsUsingGET), via de toevoeging van de facultatieve vroegeUpdatedAt en latestUpdatedAt parameters. U kunt één van beide of beide parameters met de datetime van uw keus plaatsen om slechts programma&#39;s terug te keren die tussen de twee datetimes zijn gecreeerd of bijgewerkt. Dit is nuttig om nieuwe en bijgewerkte reeksen van marketing onderpand terug te winnen, vooral voor vertaling en bedrijfsintelligentiegebruiksgevallen.

Gepast op _1970-01-01_ door _Kenny_

## Marketo Business Logic uitbreiden met Google Cloud-functie-

In dit artikel wordt een oplossing voorgesteld om Marketo met bepaalde bedrijfslogische mogelijkheden uit te breiden met Google Cloud Platform (GCP), op basis van het volgende eenvoudige voorbeeld: 3 aangepaste velden in de Marketo Lead-record:

* **OnLinePreference**: een stijgende score die op een vooruitzicht/klantenverschijning voor online mededelingen wijst.
* **OfflinePreference**: een stijgende score die op een vooruitzicht/klantenverschijning voor off-line mededelingen wijst.
* **Voorkeur**: een gebied dat door GCP wordt gegevens verwerkt dat &quot;off-line&quot;toont als de off-line score hoger is dan online één, en &quot;online&quot;de andere manier rond

Deze technologie opent de weg voor geavanceerdere bedrijfslogica en uiteindelijk voor het roepen van externe Webdiensten, die de resultaten in Marketo transformeren en consolideren.  

### Informatie over Google Cloud Platform en Functies

[ het Platform van de Wolk van Google ](https://cloud.google.com/) (GCP) is een reeks van de diensten van de wolk die op de zelfde infrastructuur loopt die Google intern voor zijn eindgebruikersproducten, zoals Google Onderzoek en YouTube gebruikt. Naast een reeks beheerhulpmiddelen, verstrekt het een reeks modulaire wolkendiensten met inbegrip van gegevensverwerking, gegevensopslag, gegevensanalyse, machine het leren, grote gegevens en veel meer. Wij zouden vele verschillende diensten GCP voor onze behoefte, zoals de Motor van de Compute, Motor van de App of Motor Kubernetes kunnen gebruiken, maar wij kozen voor de [ Functies van de Wolk ](https://cloud.google.com/functions) (nog in Beta) voor de volgende belangrijkste voordelen:

* De serverloze wolkenverwerking is waar de logica op bestelling als reactie op gebeurtenissen zoals de vraag van HTTP kan worden gesponnen.
* Vermindert het grootste deel van de pijn die door serveronderhoud en plaatsingen wordt veroorzaakt.
* Rendabel, aangezien u GCP slechts voor elke functievraag en niet voor het houden van een server in werking stelt.
* Eenvoudig en snel te implementeren aangezien u zich alleen op uw toepassingslogica richt.
* Automatisch schalen, klaar voor zeer hoge werklasten.

Gelieve te controleren de [ GCP website ](https://cloud.google.com/) voor meer informatie over deze technologie en zijn tarifering. Deze zelfstudie mag doorgaans geen belangrijke kosten met zich meebrengen en past perfect binnen het gratis krediet van een GCP-proef.  

### Voorbereiding van uw Google Cloud-omgeving

U hebt een Google Cloud-account nodig. U kunt GCP voor vrije met een krediet proberen dat meer dan genoeg is om dit leerprogramma in werking te stellen, enkel &quot;probeer het vrije&quot;knoop op de [ GCP website ](https://cloud.google.com/). Volg alle stappen van de sectie &quot;alvorens u&quot;in [ Zelfstudie van HTTP ](https://cloud.google.com/functions/docs/calling) van Google begint:

1. Een Cloud Platform-project maken: GA NAAR DE PAGINA BRONNEN BEHEREN
1. Laat het factureren voor uw project toe: [ SCHAKEL HET VERPLETTEN ](https://cloud.google.com/billing/docs/how-to/modify-project?visit_id=638816637273392093-1926929734&amp;rd=1) TOE
1. Laat de Functies API van de Wolk toe: [ SCHAKEL API ](https://accounts.google.com/InteractiveLogin?continue=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&amp;followup=https://console.cloud.google.com/flows/enableapi?apiid%3Dcloudfunctions%26redirect%3Dhttps://cloud.google.com/functions/docs/tutorials/http&amp;osid=1&amp;passive=1209600&amp;service=cloudconsole&amp;ifkv=ASKV5Mh81NGNsqcJqhx7hst0KFnyA0MJ-2zay8ovyluBfpvDoM820nF9Wq_SKbC1m_sjQvvRNoKSuA) TOE
1. [ installeer en initialiseer de Cloud SDK ](https://cloud.google.com/sdk/docs/)
1. Gcloud-componenten bijwerken en installeren

   **de updates van wolkencomponenten &amp;&amp;** **de componenten van de wolk installeren bèta**

1. Bereid uw milieu voor de ontwikkeling Node.js voor: [ GA NAAR DE GIDS VAN DE OPSTELLING ](https://cloud.google.com/nodejs/docs/setup)

### Implementatie van de functie voor het vergelijken van de muziek

1. Maak een emmertje voor cloudopslag om uw bestanden voor cloudfuncties in te stellen. U kunt dit doen met de opdrachtregel:

   ` gsutil mb gs://[YOUR_STAGING_BUCKET_NAME]`

   of vanuit de Google Cloud-webinterface door uw project te selecteren en op het menu Opslag te klikken:
   * Geef uw opslagemmertje een unieke naam
   * De standaardopslagklasse selecteren
   * Selecteer de meest geschikte regionale locatie

1. Maak op uw lokale systeem een map voor de toepassingscode.
1. Maak een bestand index.js in deze map met de volgende JavaScript-code: de code is heel eenvoudig te begrijpen. Het ontleedt de twee inputparameters van het HTTP- verzoeklichaam in JSON, doet de verwerking en codeert in JSON de reactie van HTTP.

```javascript
 /\*\*
     \* HTTP scoreCompare Cloud Function.
     \*
     \* @param {Object} req Cloud Function request context.
     \* @param {Object} res Cloud Function response context.
     \*/
    exports.scoreCompare = function scoreCompare (req, res) {
     var onlineScore=parseInt(req.body.onlineScore);
     var offlineScore=parseInt(req.body.offlineScore); 
     console.log('/scoreCompare: got values onlineScore =' + onlineScore + ', offlineScore =' + offlineScore);
     var result;
     if (onlineScore>offlineScore) {result = 'online';} else {result = 'offline';}
     console.log('/scoreCompare: and result is ' + result);
     res.status(200).json({output: result}).end();
    };
```

Implementeer de functiescoreCompare met een HTTP-trigger. Voer de volgende opdracht uit vanuit uw map:

**wolkenbètafuncties stellen [ FUNCTIE ] op - stadium-emmer [ UW_STAGING_BUCKET_NAME ] - trekker-http**

Waar [ UW_STAGING_BUCKET_NAME ] de naam van uw het opvoeren emmertje van de Opslag van de Wolk is. In ons voorbeeld:

**de functies van de wolkenbèta opstellen scoreCompare —stage-bucket mktostorage —trigger-http**

1. Noteer de URL voor de functie Cloud (httpsTrigger URL) van de uitvoer van de console die er als volgt uitziet: `https://[YOUR_REGION]-[YOUR_PROJECT_ID].cloudfunctions.net/[FUNCTION]` waar

   * [ UW_REGION ] is het gebied waar uw functie wordt opgesteld. Dit is zichtbaar in uw terminal wanneer uw functie klaar is met het implementeren.
   * [ UW_PROJECT_ID ] is uw het projectidentiteitskaart van de Wolk. Dit is zichtbaar in uw terminal wanneer uw functie klaar is met het implementeren.
   * [ FUNCTIE ] is uw functienaam.

   In ons voorbeeld: [**https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare** ](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
1. Test uw functie met een hulpmiddel zoals [ Postman ](https://www.postman.com/):
   * HTTP-werkruimte: POST
   * URL: [ https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare](https://us-central1-marketo-cloud-logic.cloudfunctions.net/scoreCompare)
   * Kopteksten: content-type = application/json
   * Body: {&quot;onlineScore&quot;:110, &quot;offlineScore&quot;:200} Output zou moeten geven: {&quot;output&quot;: &quot;off-line&quot;}.

### De functie Cloud aanroepen vanuit een Marketo Webhaak

De volgende drie aangepaste velden moeten worden gemaakt in de Lead-record in Marketo:

* **OnlinePreference**: Geheel
* **OfflinePreference**: Integer
* **Voorkeur**: Koord

Maak de volgende webhaak van de Marketo-beheerinterface met de URL van de cloudfunctie &#39;scoreCompare&#39; en de tokens van het aangepaste veld. Test de webhaak met een door Marketo getriggerde slimme campagne.

* **de webhooks van Marketo kunnen slechts van teweeggebrachte slimme campagnes worden aangehaald, niet partij slimme campagnes.**
* **als u uw wolkenfunctie niet gebruikt, schrap het of schrap het volledige project, om het veroorzaken van lasten aan uw rekening van het Platform van de Wolk van Google te vermijden.**

Wij hopen dat deze zelfstudie uw tijd waard was en u zal laten nadenken over meer geavanceerde scenario&#39;s die complexe verwerking en derdediensten impliceren. Een goed voorbeeld zou zijn om Google Cloud AI, de machine leerservices van Google, te benutten. U kunt bijvoorbeeld een vrije tekst uit een Marketo-formulier parseren en Google Natural Language API vragen om de structuur en betekenis van de tekst te onthullen en deze analyse vervolgens terug te slaan in Marketo; gewoon de poorten openen voor ideeën.

Gepast op _2017-11-21_ door _Filip_

## Updates voor herfst 2017

In de Fall 2017-release publiceren we verschillende verbeteringen voor onze API&#39;s voor bedrijfsmiddelen. Zie de volledige lijst met updates hieronder.

### Door programma&#39;s bladeren op datumbereik

Wij hebben de capaciteit toegevoegd om programma&#39;s door datumwaaier aan ons [ te krijgen krijgt het eindpunt van Programma&#39;s ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST). Dit gebeurt met de parameters `earliestUpdatedAt` en `latestUpdatedAt` . U kunt één van beide of beide parameters met de datetime van uw keus plaatsen om slechts programma&#39;s terug te keren die tussen de twee datetimes zijn gecreeerd of bijgewerkt.

### E-mail voorvertonen

U velen nu voorproef een e-mail gebruikend [ krijgt het Volledige eindpunt van de Inhoud E-mail ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailFullContentUsingGET), dat de geserialiseerde versie van HTML van een e-mail terugkeert. Alle tokens, fragmenten, dynamische inhoud en ingesloten componenten worden volledig gerenderd. Een facultatieve **leadId** parameter kan worden overgegaan om een bepaalde lood na te bootsen.

### HTML van e-mail 2.0 vervangen

Wij hebben het [ Ee-maileindpunt van de Update Volledige Inhoud ](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailFullContentUsingPOST) toegevoegd om u toe te staan om blokken van de e-mailinhoud van HTML te vervangen. Als u de code van HTML van een e-mail van Marketo gebruikend Marketo E-mail 2.0 Redacteur uitgeeft, dan is het verband tussen e-mail en zijn malplaatje gebroken, meer over dat [ hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html). Gebruikend dit eindpunt, kunt u de inhoud van HTML van een e-mail programmatically bijwerken waarvan verhouding is gebroken. Daarnaast hebben we alle andere e-maillevenscyclusgerelateerde eindpunten aangepast zodat deze compatibel zijn met e-mails waarin de relatie is verbroken:

* Concept voor e-mail goedkeuren
* E-mail niet goedkeuren
* E-mail verwijderen
* Concept voor e-mail negeren
* E-mail klonen
* E-mailmetagegevens bijwerken

### Overige verbeteringen

* De maximale tijdspanne voor datumbereikfilters is verhoogd tot 31 dagen. Dit heeft tot [ Bulk leidt de filters van het Extraheren ](/help/rest-api/bulk-lead-extract.md) (createdAd of updatedAt), en [ het filter van het Extraheren van de Activiteit van de Bulk ](/help/rest-api/bulk-activity-extract.md) (createdAt).

Gepost op _2017-12-15_ door _David_

## TEST - Externe videokoppeling op communautair niveau

[ de video van de het Tutorial van het Pak van PowerPack E-mail van de Levering ](https://nation.marketo.com:443/t5/product-space-archive-videos/email-deliverability-power-pack-tutorial-video/m-p/283550)  

Gepost op _1970-01-01_ door _David_

## Updates van winter 2018

In de Winterrelease van 2018 geven we een aantal verbeteringen aan onze API&#39;s uit. Zie de volledige lijst met updates hieronder.

### Triggercampagnes activeren/deactiveren

We hebben de mogelijkheid toegevoegd om triggercampagnes te activeren en te deactiveren, waardoor het automatiseren van de sjablonen voor uw programma&#39;s eenvoudiger kan worden. Dit wordt bereikt door twee onlangs toegevoegde eindpunten te roepen: [ activeer Slimme Campagne ](https://developer.adobe.com/marketo-apis/api/asset/#operation/activateSmartCampaignUsingPOST), [ Deactivate Slimme Campagne ](https://developer.adobe.com/marketo-apis/api/asset/#operation/deactivateSmartCampaignUsingPOST). Zie de sectie van de Trekker in [ Campagnes ](/help/rest-api/assets.md) documentatie voor details.

### Programma ophalen op naam

Toegevoegde twee parameters aan [ krijgen het Programma door het eindpunt van de Naam ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getProgramByNameUsingGET) om terugwinning van programmakosten en programmamarkeringen te vergemakkelijken. Zie **includeCosts** en **includeTags** parameters in de [ Programma&#39;s ](/help/rest-api/assets.md) documentatie voor details.

### Overige verbeteringen

De Bulk Extract-API is nu &#39;werkruimte-bewust&#39;. Wanneer u een API-Enige Gebruiker voor de Dienst van de Douane van a [&#128279;](/help/rest-api/custom-services.md) creeert, moet u een Rol van de Gebruiker met API Toegang voor één of meerdere werkruimten selecteren. Eerder, werd de Dienst van de Douane verleend toegang tot alle werkruimten. De aangepaste service krijgt nu alleen toegang tot de werkruimte(n) die zijn geselecteerd tijdens het maken van gebruikers met alleen API.

Gepost op _2018-03-02_ door _David_

## Updates van voorjaar 2018

In de lente van 2018 brengen wij nieuwe REST APIs, en Web het volgen privacyverbeteringen vrij. Zie de volledige lijst met updates hieronder.

REST API

### Statische lijst CRUD

Hiermee kunnen gebruikers op afstand statische lijstrecords maken, lezen, bijwerken en verwijderen. Maakt beheer van de volledige levenscyclus van een statische lijst door REST APIs, met inbegrip van het bevolken en handhaven van lidmaatschap mogelijk. De details kunnen [ hier ](/help/rest-api/static-lists.md) worden gevonden.

### Metagegevens aangepaste activiteit

Hiermee kunnen gebruikers op afstand aangepaste activiteitenrecords maken. Maakt beheer van de volledige levenscyclus van douaneactiviteiten door REST APIs, met inbegrip van manipulatie van types en typeattributen mogelijk. De details kunnen [ hier ](/help/rest-api/activities.md) worden gevonden.

### Metagegevens slimme lijst

Hiermee kunnen gebruikers records met slimme lijsten op afstand lezen, klonen en verwijderen. Schakelt het beheer van slimme lijsten via REST API&#39;s in. De details kunnen [ hier ](/help/rest-api/smart-lists.md) worden gevonden.

### Privacy voor webtracering

De webtrackingcode van Munchkin JavaScript is uitgebreid en bevat nu de volgende verbeteringen op het gebied van privacy:

* Uitschakelen - Mogelijkheid voor bezoekers om zich permanent af te melden voor webtracering. De details kunnen [ hier ](/help/javascript-api/lead-tracking.md) worden gevonden.
* IP de Anonymization van het Adres - naleving van lokale en internationale privacyverordeningen door de capaciteit te verstrekken om de IP adressen van Webbezoekers te anonimiseren. Zie **anonymizeIP** configuratieparameter [ hier ](/help/javascript-api/configuration.md).

### Overige verbeteringen

* Het [ krijgt Omslagen ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getFolderUsingGET) eindpunt keert nu alle omslagen terug wanneer een niet-wortelouder en maxDepth=1 worden gespecificeerd.
* Het [ krijgt het Bestaan van de Pagina door het eindpunt van Id ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getLandingPageByIdUsingGET) keert nu attributen URL met het protocol in alle gevallen terug (http:// of https://).

Gepost op _2018-06-29_ door _David_

## Updates van zomer 2018

De release van de zomer van 2018 is voornamelijk een onderhoudsrelease die bestaat uit kleine verbeteringen en defecte resoluties. Zie de volledige lijst met updates hieronder.

### REST API

* Extra ondersteuning voor velden voor e-mailverwijdering die oorspronkelijk onnodig zijn weggelaten. Deze velden zijn nu, indien van toepassing, beschikbaar voor lezen en schrijven via REST.
   * Zwarte aanbieding
   * Marketing opgeschort
   * E-mail opgeschort
   * Relatieve urgentie
* [ krijgt Leidingen door het eindpunt van het Type van Filter ](https://developer.adobe.com/marketo-apis/api/lead-database-endpoint-reference/#/Leads/getLeadsByFilterUsingGET) nu steunt leadPartionId als filterType.

### Resoluties beschermen

**REST Eindpunt**

**Beschrijving**

[ keur Programma ](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveProgramUsingPOST) goed

Als u Niet-operationele e-mails blokkeren had ingesteld op false bij het maken van het programma, zou een aanroep om het programma goed te keuren worden teruggezet op true.

[Bulk extraheren](/help/rest-api/bulk-extract.md)

Bepaalde Unicode-tekens zijn beschadigd in het extractie-uitvoerbestand.

[ Programma van de Kloon ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

* Als u het e-mailprogramma hebt gekloond, is de logica voor het filter SmartList ingesteld op Alles in het resulterende programma, ongeacht de aanvankelijke instelling.
* Als u een programma probeerde te klonen dat een statische lijst had bevat (die werd geschrapt), dan ontving u 709, &quot;de volgende activa zijn niet gestaafd:Lijst&quot;fout.
* Als u een programma over werkruimten probeerde te klonen, dan ontving u een fout 611, &quot;Kan programma&quot;niet klonen.

[ krijgt Statische Lijst door Id ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getStaticListByIdUsingGET)

Als uw Dienst van de Douane de machtiging van het Activum van het Read-Only had, dan zou u een 603 ontvangen, &quot;Ontkende Toegang&quot;fout.

[ Duw Lood aan Marketo ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/pushToMarketoUsingPOST)

Als **de koekjes** attribuut in een **input** voorwerp van het serielood werd gespecificeerd, werd de vroegere anonieme activiteit niet geassocieerd met pas gecreeerd Lood.

[ Campagne van het Programma ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/scheduleCampaignUsingPOST)

Als u a **runAt** datum ver in de toekomst specificeerde, toen werd de campagne nooit in werking gesteld en **success=true** was teruggekeerd. Nu als **runAt** datum verder dan 2 jaar in de toekomst is, is een fout teruggekeerd [ 1042 ](/help/rest-api/error-codes.md).

[ de Leads van de Synchronisatie ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST)

Als u `action=createDuplicate` en een `externalCompanyId` parameter (om de nieuwe Lood met een bestaand Bedrijf te associëren) specificeerde, dan werd de Lood geassocieerd met een leeg Bedrijf (in plaats van het gespecificeerde Bedrijf).

#### Overige

* Verwijderd de volgende Waarde van de Verandering van Gegevens van alle eindpuntreacties: mktoClientReqId. Dit was alleen voor intern gebruik.
* Opzoekfunctionaliteit voor fouten toegevoegd. Voer een REST API-foutcode in het zoekvak in en selecteer een van de onderstaande lijst voor automatisch aanvullen om naar de beschrijving van de fout te gaan.
* Toegevoegde [ pagina van de Verwijzing van het Eindpunt ](/help/rest-api/endpoint-reference.md). Dit is een sorteerbare lijst met alle REST API-eindpunten op één locatie. U kunt deze pagina ook gebruiken om de minimale set machtigingen te genereren die uw toepassing nodig heeft. Dit is handig wanneer u een aangepaste service maakt.

Gepost op _2018-10-12_ door _David_

## Updates voor einde 2018

De release van Fall 2019 is voornamelijk een onderhoudsrelease die bestaat uit kleine verbeteringen en defecte resoluties. Zie de volledige lijst met updates hieronder.

### Verbeteringen

* Toegevoegde [ E-mailCC Gebieden ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/email-cc) steun voor [ Activa API ](/help/rest-api/assets.md). CC-veldinstellingen worden doorgegeven zoals u had verwacht tijdens goedkeurings-/kloonbewerkingen (conceptgoedkeuring van e-mail- of e-mailsjabloon, e-mail- of programmacolone). Alle op e-mail betrekking hebbende eindpunten keren nu de waarden van de Gebieden van CC in het **ccFields** bezit terug. Schuif omlaag in de onderstaande reactie om een voorbeeld te zien. Deze verandering beïnvloedt de volgende eindpunten: [ krijgt E-mail door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByIdUsingGET), [ krijgt E-mail door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailByNameUsingGET), [ krijgt E-mail ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailUsingGET), [ keurt het Ontwerp van de E-mail- Opstelling ](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST) goed, [ het Ontwerp van het E-mailMalplaatje ](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST_1), [ E-mail van de Kloon ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST).[&#128279;](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)

```json
{
    "success": true,
    "errors": [],
    "requestId": "cc96#166e836348b",
    "warnings": [],
    "result": [
        {
            "id": 2061,
            "name": "Test Email",
            "description": "This is a test",
            "createdAt": "2018-11-06T05:27:10Z+0000",
            "updatedAt": "2018-11-06T08:33:16Z+0000",
            "url": "<https://app-sjqe.marketo.com/#EM2061A1LA1>",
            "subject": {
                "type": "Text",
                "value": "This is a test with CC Fields"
            },
            "fromName": {
                "type": "Text",
                "value": "Tommy Tester"
            },
            "fromEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "replyEmail": {
                "type": "Text",
                "value": "<tommy.tester@marketo.com>"
            },
            "folder": {
                "type": "Program",
                "value": 7494,
                "folderName": "Initiative"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "approved",
            "template": 1218,
            "workspace": "Default",
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
                {
                    "attributeId": "855",
                    "objectName": "lead",
                    "displayName": "emailcc",
                    "apiName": "emailcc"
                },
                {
                    "attributeId": "857",
                    "objectName": "lead",
                    "displayName": "leadDetails",
                    "apiName": "leadDetails"
                },
                {
                    "attributeId": "859",
                    "objectName": "company",
                    "displayName": "headquarters",
                    "apiName": "headquarters"
                }
            ]
        }
    ]
}
```

### Resoluties beschermen

* Aangepast [ Veelvoudige het brandmerken Domeinen ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) steun voor [ Activa API ](/help/rest-api/assets.md). Eerder werden instellingen voor Meerdere brandingdomeinen niet doorgegeven bij het goedkeuren van een e-mailconcept, het klonen van een e-mail of het klonen van een programma. Dit is gecorrigeerd. Deze verandering beïnvloedt de volgende eindpunten: [ keur E-mailOntwerp ](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveDraftUsingPOST) goed, [ Kloon E-mail ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneEmailUsingPOST), [ Kloonprogramma.](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST)
* Toegevoegde [ apiOnly ](/help/javascript-api/configuration.md) configuratie het plaatsen. Standaard wordt op webpagina&#39;s die de Munchkin-tag bevatten de gebeurtenis &quot;Visits Web Page&quot; (Webpagina bezoeken) uitgevoerd wanneer de webpagina in de browser wordt geladen. In sommige gevallen is dit ongewenst. Webtoepassingen van één pagina die bijvoorbeeld volledige controle moeten hebben over het moment waarop deze gebeurtenis wordt gestart. Om dit gebruiksgeval te steunen, hebben wij een nieuwe **apiOnly** configuratie die plaatsen toegevoegd. Als de waarde true is, genereert de Munchkin-tag geen activiteit &quot;Visits Web Page&quot; tijdens het laden van de pagina.
* Toegevoegde [ domainSelectorV2 ](/help/javascript-api/configuration.md) configuratie het plaatsen. Door gebrek, behandelt de markering van Munchkin correct Web-pagina&#39;s die op plaatsen met twee-brief [ domeinen van het landcode top-level ](https://en.wikipedia.org/wiki/Country_code_top-level_domain) (voorbeelden: .io, .co, .ly) worden ontvangen. Hierdoor wordt het Munchkin-cookie-domeinkenmerk onjuist ingesteld. Om een betere uit dooservaring te bereiken, hebben wij een nieuwe **domainSelectorV2** configuratie het plaatsen toegevoegd. Wanneer ingesteld op true, wordt een verbeterd algoritme gebruikt om het Munchkin cookie domain-kenmerk automatisch in te stellen.
* Aangepast [ opt-uit ](/help/javascript-api/lead-tracking.md) koekjesdomein. In bepaalde gevallen is het domeinkenmerk van het Munchkin Opt-Out-cookie (mkto_opt_out) onjuist ingesteld. Het opt-uit koekje van Munchkin gebruikt nu de zelfde logica zoals het koekje van Munchkin (_mkto_trk) om het attribuut van het domeinkoekje te bepalen, met inbegrip van het eerbiedigen van **domainLevel** configuratie die plaatst.
* Ontwikkelaars van Android-toepassingen kunnen nu direct Google [ Firebase Cloud Messaging ](https://firebase.google.com/docs/cloud-messaging/) (FCM) gebruiken met deze SDK. De details kunnen [ hier ](/help/mobile/installation.md) worden gevonden.

Gepost op _2018-12-07_ door _David_

## Updates van voorjaar 2019

De release van voorjaar 2019 is voornamelijk een onderhoudsrelease die bestaat uit kleine verbeteringen en defecte resoluties. Zie de volledige lijst met updates hieronder.

Gepost op _2019-03-19_ door _David_

## Updates in juni 2019

De release van juni 2019 is voornamelijk een onderhoudsrelease die bestaat uit kleine verbeteringen en defecte resoluties. Zie de volledige lijst met updates hieronder.

### REST API

1. Er is een controlesom toegevoegd aan de eindpunten van de exportstatus Bulk. U kunt de controlesom vergelijken met een hash van het opgehaalde bestand om de integriteit van het opgehaalde bestand te controleren. De controlesom is een SHA-256-hash van het geëxporteerde bestand en wordt opgeslagen in het kenmerk fileCheckSum wanneer een exporttaak is voltooid.

De volgende eindpunten keren de controlesom terug: [ krijgen de Status van de Taak van de Lood van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [ krijgen de Banen van de Lood van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET), [ krijgen de Status van de Taak van de Activiteit van de Activiteit van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET), [ krijgt de Banen van de Activiteit van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportActivitiesUsingGET)

#### Resoluties beschermen

1. De Vaste kwestie met [ Bulk de Invoer van het Voorwerp van de Douane ](/help/rest-api/bulk-custom-object-import.md) wanneer het invoeren van decimale aantallen in geheelgebieden. Vóór de correctie werd het decimale getal omgezet in een geheel getal door het gehele getalgedeelte toe te wijzen en het breukgedeelte te verwijderen (5.432 werd bijvoorbeeld omgezet in 5). Er wordt nu een fout met het gegevenstype &quot;Invalid data type in field Source ID&quot; gegenereerd voor elke rij waarin gegevens voorkomen die niet overeenkomen.
1. Vaste kwestie waar een E-mailprogramma dat gebruikend het [&#128279;](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) eindpunt van het Programma van de Kloon  werd gecreeerd Communicatie montages van Limieten in bepaalde gevallen niet respecteerde.
1. Vaste kwestie met [ goedkeurde het Landing Concept van de Pagina ](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) eindpunt waar het 611 zou terugkeren. &quot;Systeemfout&quot; wanneer de landingspagina het formulier E-mail opzeggen bevatte.
1. Vaste kwestie met [ goedkeurde het Landing Concept van de Pagina ](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST) eindpunt waar het 611 zou terugkeren. &quot;De fout van het systeem&quot;wanneer de het landen pagina gebruikend het [ Klonen Landing van de Pagina ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneLandingPageUsingPOST) eindpunt was gekloond.

#### Afgeschafte elementen

1. Als deel van [ E-mailRedacteur 1.0 Verdringing ](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666), zal 1.0 E-mail Assets read-only aan eind 2019 worden. Al 1.0 E-mail Assets moet in 2.0 als descriDiscard E-mailontwerp worden omgezet, [ hier ](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) insluiten. Om ontwikkelaars te helpen zich voor te bereiden op die gebeurtenis, hebben wij waarschuwingen aan alle op e-mail betrekking hebbende eindpunten toegevoegd die proberen om 1.0 E-mail Assets te wijzigen. Hier volgt een voorbeeld van een reactie die de waarschuwing bevat:

`{` `"success": true,` `"errors": [],` `"requestId": "15c57#16b338d6e75",` `"warnings": [` `"This is a v1 email asset. API support for modifying v1 emails is being dropped, and this operation will not work on v1 emails in the future. To avoid service interruptions, upgrade this and related assets by editing them in the User Interface."` `],` `"result": [` `"{\"service\":\"sendTestEmail\",\"result\":true}"` `]` `}`

De volgende e-mailgerelateerde eindpunten retourneren de waarschuwing:

* Volledige inhoud e-mailen bijwerken
* E-mailinhoud bijwerken
* Voorbeeldmail verzenden
* E-mail niet goedkeuren
* Kloonprogramma
* E-mail klonen
* Concept voor e-mail goedkeuren
* Sectie Dynamische inhoud e-mail bijwerken
* E-mailmetagegevens bijwerken
* Programma goedkeuren

E-mailscripting (Apache-snelheid)


#### Afgeschafte elementen

1. Een subset van de functionaliteit van het Manuscript van de Snelheid werd onbruikbaar gemaakt voor veiligheidsdoeleinden. Dit omvat de volgende methoden en variabelen: getClass(), $class, $context, $text. Meer informatie kan [ hier ](https://nation.marketo.com:443/t5/knowledgebase/unsupported-velocity-tools-disabled-in-june-2019-release/ta-p/251177) worden gevonden.

Gepost op _2019-06-14_ door _David_

## Updates voor augustus 2019

In augustus 2019 publiceren we nieuwe REST API&#39;s, verbeteren we bestaande API&#39;s en verhelpen we fouten. Zie de volledige lijst met updates hieronder.

### Verbeteringen

1. Verbeterde mogelijkheden voor de levenscyclus van slimme campagnes. Nieuwe eindpunten toegevoegd waarmee u de volgende bewerkingen op slimme campagnes kunt uitvoeren: ophalen op naam, maken, bijwerken, klonen en verwijderen. De volledige informatie kan [ hier ](/help/rest-api/smart-campaigns.md) worden gevonden.
1. Verbeterde slimme eindpunten van de Lijst om vraagmogelijkheden te verbeteren.
   1. [ krijgt Slimme Lijst door Identiteitseur ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByIdUsingGET) eindpunt keert nu Slimme beschrijvingen van de Lijstregel (trekkers en filters) terug wanneer u de **includeRules** booleaanse parameter overgaat.
   1. [ krijgt het Slimme eindpunt van Lijsten ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListsUsingGET) staat u nu toe om de resultaten door datumwaaier te filtreren wanneer u **firstUpdatedAt** en **latestUpdatedAt** datetime parameters overgaat. Bovendien, keert dit eindpunt nu Slimme Lijsten terug die lid van campagnes en e-mailprogramma&#39;s zijn.
1. Toegevoegde eindpunten voor het extraheren van definities van slimme lijsten.
   1. Krijg [ Slimme Lijst door het Slimme eindpunt van identiteitskaart van de Campagne ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListBySmartCampaignIdUsingGET) het slimme lijstverslag voor een bepaalde slimme campagne identiteitskaart terugkeert.
   1. Krijg [ Slimme Lijst door het eindpunt van identiteitskaart van het Programma ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getSmartListByProgramIdUsingGET) keert het slimme lijstverslag voor bepaalde programma identiteitskaart terug.
1. Verbeterde het [ eindpunt van de Inhoud E-mail van de Update ](https://developer.adobe.com/marketo-apis/api/asset/#operation/updateEmailContentUsingPOST) om updates aan e-mailkopbalgebieden voor e-mailkopbal toe te staan die van hun malplaatje (onderwerp, van naam, van e-mail, antwoord aan) zijn gebroken. Gebroken van het malplaatje wordt beschreven [ hier ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/edit-an-emails-html&#39;sHTML-BreakinganEmailFromitsTemplate).

### Resoluties beschermen

1. Vaste kwestie waar het roepen van [ Schrapping die Pagina ](https://developer.adobe.com/marketo-apis/api/asset/#operation/deleteLandingPageByIdUsingPOST) op een goedgekeurde het landen pagina landde de landende pagina zou schrappen. De fout &#39;709, goedgekeurde bestemmingspagina kan niet worden verwijderd&#39; wordt nu correct geretourneerd. [ LM-127271 ]
1. Vaste kwestie met [ verzendt het eindpunt van de Steekproef E-mail ](https://developer.adobe.com/marketo-apis/api/asset/#operation/sendSampleEmailUsingPOST) waar het 611 zou terugkeren. &quot;Systeemfout&quot; wanneer e-mail niet langer in de sjabloon staat. [ LM-127288 ]
1. Vaste kwestie met [ schrapte het eindpunt van het Programma ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST) waar in sommige gevallen het een in-gebruiks programma in plaats van het uitgeven van &quot;709 zou schrappen, kan programma niet schrappen. De activa zijn elders in gebruik of niet deltable&quot; fout. [ LM-125431 ]

### Afgeschafte elementen

1. De API-ondersteuning voor e-maileditor 1.0 wordt in januari 2020 afgekeurd. Denk eraan dat u uw elementen vóór die datum omzet in 2.0. Pogingen om naar E-mail 1.0-middelen te schrijven of te klonen na januari resulteren in fouten in plaats van waarschuwingen. Leer meer over E-mail APIs [ hier ](https://nation.marketo.com:443/t5/knowledgebase/email-2-0-and-email-api-faq-s/ta-p/251423).
1. Om zich aan Adobe te richten wereldklasse norm voor veiligheid, zullen wij steun voor de Veiligheid van de Laag van het Vervoer (TLS) 1.0 en 1.1 vanaf 13 December, 2019 verwerpen. Systemen die met Marketo integreren en niet voldoen aan het 1.2-protocol, kunnen mogelijk de toegang tot Marketo Engage-services verliezen. Zorg ervoor dat alle clientsystemen vóór 13 december 2019 compatibel zijn met TLS 1.2 om uw Marketo Engage-toegang te behouden. De meer gedetailleerde informatie kan [ hier ](https://nation.marketo.com:443/t5/knowledgebase/tls-1-0-1-1-deprecation-faq/ta-p/249085) worden gevonden.


1. Alle Slimme verwante inhoud van de Campagne verblijft nu in het [ Slimme 1&rbrace; menupunt van Campagnes &lbrace;(onder REST API > Assets).](/help/rest-api/smart-campaigns.md)

Gepost op _2019-08-16_ door _David_

## Updates voor januari 2020

In januari 2020 publiceren we nieuwe REST API&#39;s, verbeteren we bestaande API&#39;s en verhelpen we fouten. Zie de volledige lijst met updates hieronder.

* De mogelijkheid toegevoegd om programmatisch aangepaste objectschemadefinities te maken. Op deze manier kunt u een aangepast object eenmaal definiëren en zo vaak u wilt instellen dat dit object wordt gebruikt. Hierdoor kunt u gebruikers in staat stellen op effectieve wijze gebruik te maken van sandbox- en center-of-excellence-modellen. ISV&#39;s kunnen ook het instapproces voor klanten vereenvoudigen. U hebt een geschikt abonnementstype nodig om toegang te krijgen tot de API voor metagegevens van aangepaste objecten.
* De mogelijkheid om programmaleden in bulk te importeren en te exporteren toegevoegd. Deze nieuwe reeks eindpunten volgt het bestaande Marketo REST API-patroon voor het maken van asynchrone bulkverwerkingstaken. De verslagen van het Lid van het programma kunnen de douanegebieden van het Lid van het Programma, en/of gebieden van de Lood bevatten.
* Toegevoegde krijg Beschikbaar eindpunt van de Gebieden van het Lid van het Programma van het Formulier om het gebruik van de Gebieden van de Douane van het Lid van het Programma als Gebieden van de Vorm te steunen. Hiermee wordt een lijst geretourneerd met alle aangepaste velden voor programmaleden die kunnen worden gebruikt in een Marketo-formulier.
* Toegevoegde [ krijgt E-mailMalplaatje dat door een ](/help/rest-api/email-templates.md) eindpunt wordt gebruikt dat een lijst van e-mailactiva terugkeert die van een bepaald e-mailmalplaatje afhangen. Hierdoor kunt u snel inzicht krijgen in de gevolgen van een mogelijke wijziging van een e-mailsjabloon en deze afhankelijkheden eenvoudiger aanpakken.

Gepost op _2020-01-17_ door _David_

## Elk aangepast object ophalen

Wij worden vaak gevraagd hoe te om Marketo API te gebruiken om een lijst van alle [ douanevoorwerpen ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) (COs) te krijgen. Het vragen voor COs vereist meer dan zijn naam: sommige _a priori_ kennis over elke CO wordt ook vereist. De methoden om die kennis op te halen zijn mogelijk niet duidelijk, aangezien de API geen methode biedt om rechtstreeks een query uit te voeren. Net als bij veel doelstellingen in Marketo Engage bieden slimme lijsten een antwoord voor CO&#39;s die zijn gekoppeld aan personen (Leads). De slimme Lijsten werken verschillend met Bedrijven en u zult omhoog met een lijst van alle Personen beëindigen de waarvan Bedrijven met het type van voorwerp voor de filter verbonden zijn zodat kunt u het noodzakelijk vinden om bedrijven afhankelijk van uw doelstellingen te dedupliceren. Telkens wanneer een nieuw Aangepast object wordt goedgekeurd, wordt een bijbehorend filter gemaakt. Het zal in het formaat &quot;**worden genoemd heeft de NAAM van CO**&quot;. In het voorbeeld hieronder, is de naam van het douanevoorwerp &quot;**Abonnement van het Spoor van de Conferentie&quot;** en zijn filter wordt genoemd &quot;**heeft het Abonnement van het Spoor van de Conferentie**&quot;. Zodra u de Slimme Lijst hebt gecreeerd, kunt u de informatie nodig terugwinnen om voor bijbehorende COs te vragen gebruikend het [ eindpunt van douanevoorwerpen ](/help/rest-api/custom-objects.md). Exporteer de lijst zodat het gekoppelde veld wordt opgenomen (id of e-mailadres). U kunt het gebruiken van het [ BulkLood uitvoeren API ](/help/rest-api/bulk-lead-extract.md) filtreren door **smartListName** of **smartListId** filter of [ uitvoer van UI ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/managing-people-in-smart-lists/export-people-to-excel-from-a-list-or-smart-list). In de volgende stap gebruikt u elke gekoppelde veldwaarde om gekoppelde aangepaste objecten afzonderlijk te controleren. De naam van het douanevoorwerp is **&quot;Abonnement van het Spoor van de Conferentie&quot;** in dit voorbeeld, en zijn API naam is **conferenceTrackSubscription_c**. U vindt de API naam zowel in UI als &quot;**API Naam**&quot;en via API als &quot;**naam**&quot;.  Beheerder | De Eigen Objecten van Marketo [/titel ] en hier is een fragment dat door het [ Eigen Objecten API van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/listCustomObjectsUsingGET) eindpunt is teruggekeerd:

```json
{
    "name": "conferenceTrackSubscription_c",
    "displayName": "Conference Track Subscription",
    "description": "Track subscription for conference attendees",
    "createdAt": "2020-01-13T19:50:06Z",
    "updatedAt": "2020-01-13T19:50:06Z",
    "idField": "marketoGUID",
    "dedupeFields": [
        "subscriptionID"
    ],
    "searchableFields": [
        [
            "subscriptionID"
        ],
        [
            "marketoGUID"
        ],
        [
            "leadID"
        ]
    ],
    "relationships": [
        {
            "field": "leadID",
            "type": "child",
            "relatedTo": {
                "name": "Lead",
                "field": "Id"
            }
        }
    ]
}
```

Als u de aangepaste objecten wilt ophalen die zijn gekoppeld aan een (1:1) of een aan vele (1:N) met de personen in uw slimme lijst, voert u een aanvraag als volgt uit:

`GET /rest/v1/customobjects/conferenceTrackSubscription_c.json?filterType=leadID&filterValues=1000302,1000303,1000304,1000306,1000307`

In dit voorbeeld, wordt dit douanevoorwerp verbonden met Personen door het **leadID** gebied zodat is het filtertype &quot;**leadID**&quot;. De parameter van filterwaarden is een komma-gescheiden lijst van IDs die van de Slimme uitvoer van de Lijst wordt genomen. De aanvraag kan zoveel filterwaarden bevatten als u in één aanvraag-URI kunt gebruiken: maximaal 8 kB tekens. Verzoeken waarbij deze lengte wordt overschreden, retourneren een foutcode op 414 HTTP-niveau. De reactie kan in meer dan één brok zijn teruggekeerd. Als zo, **moreResult** **&#x200B;**&#x200B;waar zal zijn en a **nextPageToken** zal inbegrepen zijn. U zult dan aan [ pagina door ](/help/rest-api/paging-tokens.md) de resultaten moeten **moreResult** **vals** zijn. Hier volgt een deel van het resultaat voor de bovenstaande API-aanvraag:

```json
"result": [
    {
        "seq": 0,
        "marketoGUID": "d6b3ed3d-4eb8-4066-a9cd-184c8d385cfe",
        "leadID": "1000302",
        "subscriptionID": "4ad59184-6bf1-4eeb-a583-d82aeee68210",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 1,
        "marketoGUID": "e5e0aba4-f27f-494d-93ed-9cb580989bf3",
        "leadID": "1000303",
        "subscriptionID": "fc5596d5-6fa2-4848-b4a2-89d96e245c59",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 2,
        "marketoGUID": "e65007cd-86b1-4c17-8d55-057c96e1788a",
        "leadID": "1000304",
        "subscriptionID": "7e54b8a0-2170-4d81-a809-4eac349508d0",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 3,
        "marketoGUID": "39d956b2-85e2-4c24-94e7-e9fa5a09d3d0",
        "leadID": "1000306",
        "subscriptionID": "644c8e5b-fc0c-4d4a-80f8-358a65ce0a68",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    },
    {
        "seq": 4,
        "marketoGUID": "a2151350-50c8-437f-bc71-7a054bb601f0",
        "leadID": "1000307",
        "subscriptionID": "bf14218c-ae6a-42b3-a14e-f7182903cbcd",
        "createdAt": "2020-01-13T21:24:04Z",
        "updatedAt": "2020-01-13T21:24:04Z"
    }
```

U hebt nu de waarden voor elk douanevoorwerp direct verbonden met de Personen in uw Slimme Lijst en voorbij het terugwinnen van de waarden, kunt u **marketoGUID** aan [ Update ](/help/rest-api/custom-objects.md) gebruiken of [ Schrapping ](/help/rest-api/custom-objects.md) deze voorwerpen. Voor douanevoorwerpen verbonden aan Personen in een vele aan vele verhouding (N:N), keert de bovengenoemde techniek het eerste niveauvoorwerp terug dat het intermediaire voorwerp is dat elke Persoon met veelvoudige tweede niveau COs verbindt.

Om die tweede niveau COs terug te winnen, begin een nieuwe reeks vragen voor het tweede niveau CO type door op het verbindingsgebied en de waarden te filtreren die uit het eerste niveau intermediair voorwerp worden gehaald. Bijvoorbeeld, kon het bovengenoemde &quot;**Abonnement van het Spoor van de Conferentie&quot;** voorwerp een ander niveau van voorwerpen hebben die zittingen roepen **&quot;Zitting&quot;** die waarschijnlijk door **subscriptionID** zouden worden verbonden. Het verzoek om zittingen terug te winnen verbonden aan de bovengenoemde Abonnementen van het Spoor van de Conferentie zou dan als dit kijken:

`GET /rest/v1/customobjects/session_c.json?filterType=subscriptionID&filterValues=4ad59184-6bf1-4eeb-a583-d82aeee68210,e5e0aba4-f27f-494d-93ed-9cb580989bf3,e65007cd-86b1-4c17-8d55-057c96e1788a,39d956b2-85e2-4c24-94e7-e9fa5a09d3d0,bf14218c-ae6a-42b3-a14e-f7182903cbcd`

_voetnoot_ _1)**smartListName**&#x200B;en **smartListId**&#x200B;filtertypes zijn niet beschikbaar voor sommige abonnementen. Als niet beschikbaar voor uw abonnement, ontvangt u een fout wanneer het roepen van het Create eindpunt van de Baan van de Uitvoer (**&quot;1035, Niet gestaafd filtertype voor doelabonnement&quot;**). Klanten kunnen contact opnemen met de ondersteuning van Marketo om deze functionaliteit in hun abonnement in te schakelen._

Gepast op _2020-01-14_ door _Tony_

## Hoe kan ik iedereen ophalen (lead)?

Er zijn veel vragen over de processen die vereist zijn om elke persoon (lead) op te halen uit een Marketo Engage-exemplaar. We hebben veel nuttige antwoorden gegeven, maar geen daarvan is zo volledig geweest als dit. Ik heb een aantal belangrijke concepten geïdentificeerd nodig om elke lead te extraheren met de Marketo Bulk Extract-API. Alle andere details kunnen van de demonstratiecode worden geleerd ik creeerde om met het te gaan. Na het lezen van dit bericht en het verkennen van de demo code, zult u alle informatie hebben u moet weten om elke lood van een instantie van Marketo Engage terug te winnen.

### Overzicht

De kerntechniek gebruikt het [ BulkLood Extraheren API ](/help/rest-api/bulk-lead-extract.md). U zou kunnen verwachten om een bulklood uitvoerbaan zonder filter tot stand te brengen, maar u kunt niet: **API vereist een filter**. De beschikbare filters zijn **createdAt**, **staticListName**, **staticListId,** **updatedAt**, **smartListName**, en **smartListId**. Filteren met een slimme lijst zonder filters lijkt ook aantrekkelijk. Probeer dat en u vindt dat het systeem slim genoeg is om een slimme lijst zonder filter te behandelen het zelfde: API vereist ook hier een filter. Aangezien wij een filter nodig hebben, is het betrouwbare en canonicale filter aan gebruik **createdAt**. Dit filtertype staat datetime tot 31 dagen toe, zodat moeten wij veelvoudige banen in werking stellen en de resultaten combineren. We beginnen met het zoeken naar de oudst mogelijke aanmaakdatum voor een lead in de doelinstantie. Vanaf die oudste mogelijke datum creëren wij banen die 31 dagen minus één seconde (meer op dat later) overspannen. Na het creëren van elke baan, zullen wij het vragen en wachten tot het voltooit. Vervolgens downloaden we het resulterende bestand en controleren we de integriteit ervan met een controlesom. En ten slotte, dedupliceer lood door identiteitskaart dan schrijven unieke leidt tot een outputCSV dossier.

### Je oudste lead zoeken

Ik gebruik een truc om de oudste mogelijke datum voor een lead in de doelinstantie te krijgen. Er is geen API eindpunt gewijd aan die taak, zodat hebben wij een beetje creativiteit nodig. Wat ik doe is vraag alle omslagen met a **maxDepth = 1** die ons een lijst van alle top-level omslagen in de instantie zal geven. Dan verzamel ik **createdAt** data, ontleed hen, en vind de oudste datum. Deze methode werkt omdat er standaard mappen op hoofdniveau worden gemaakt met de instantie en er vóór die tijd geen leads konden worden gemaakt.

### Vereiste velden selecteren

U moet bepalen welke velden u wilt extraheren. Vind de beschikbare gebieden voor uw doelinstantie gebruikend [ beschrijf lood 2 eindpunt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/describeUsingGET_2). Het antwoord op dat verzoek bevat een lijst met de naam &quot;velden&quot;. Hier volgt een fragment uit een voorbeeldreactie:

```json
  "fields": [
      {
          "name": "AccountSource",
          "displayName": "Account Source",
          "dataType": "string",
          "length": 40,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "acquisitionProgramId",
          "displayName": "Acquisition Program",
          "dataType": "reference",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Active_c",
          "displayName": "Active",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "address",
          "displayName": "Address",
          "dataType": "text",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "Address_lead",
          "displayName": "Address (L)",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "annualRevenue",
          "displayName": "Annual Revenue",
          "dataType": "currency",
          "updateable": true,
          "crmManaged": false
      },
      {
          "name": "anonymousIP",
          "displayName": "Anonymous IP",
          "dataType": "string",
          "length": 255,
          "updateable": true,
          "crmManaged": false
      }, ...
```

Dit eindpunt keert een limitatieve lijst met inbegrip van zowel standaard als douanegebieden terug. Hoe meer velden u aanvraagt, hoe langer het duurt voordat de exporttaak is voltooid en hoe groter het resulterende bestand is. Meestal moet u alleen de velden kiezen die u nodig hebt. Niets belet je om elk beschikbaar veld te vragen, dus dat is wat ik demoing. Het gebied herkenningsteken wordt vereist wanneer het creëren van een uitvoerbaan is de **naam** waarde die. Ik extraheer de naamwaarden naar een lijst met alle veldnamen. Dan zal ik het gebruiken om elk beschikbaar gebied te verzoeken wanneer ik elke uitvoerbaan creeer.

### Taakdatumbereiken exporteren: elk 31 dagen

Elke exporttaak kan maximaal 31 dagen duren. De demo-instantie die ik gebruik, is gemaakt in augustus 2016, dus moet ik vandaag iets meer dan 40 banen creëren. Dat is het aantal dagen sinds de eerste aanmaakdatum gedeeld door 31 naar boven afgerond. Met de API kunnen twee exporttaken tegelijk worden verwerkt, zodat u twee taken tegelijk kunt uitvoeren. Bulkextractietaken zijn een middel dat met elke andere integratie wordt gedeeld, dus ik zal aardig zijn. Ik laat de andere baan voor andere integraties beschikbaar en zal laten zien dat er één voor één werk is. De datums die voor de **worden gebruikt createdAt** filter worden geformatteerd gebruikend de [ specificatie van ISO 8601 ](https://www.w3.org/TR/NOTE-datetime). Ze bevinden zich altijd in GMT (Z+0000), zodat de tijdzone eenvoudig wordt weergegeven als &quot;Z&quot; of &quot;+00:00&quot;. Augustus 1st, is 2016 **2016-08-01T00 :00: 00+00:00** en 31 dagen later is 1 September, 2016 die **2016-09-01T00 :00: 0 is 0+00:00.** Zowel begin als eindtijden zijn inclusief, zodat ga ik 1 seconde van die beëindigende tijd aftrekken: **2016-09-01T00 :00: 00+00:00** wordt **2016-08-31T23 :59: 59+00:00**. Als u een seconde aftrekt, worden overlappende tijden voorkomen. Aangezien GMT het gebrek is, kunt u **Z** of **+ 00 ook verlaten:00** weg.

### Deduplicatie

Hoewel ik de moeite heb genomen overlappende tijden te vermijden, heb ik ook deduplicatie geïmplementeerd. Ik deed dat aangezien er sommige randgevallen zijn wanneer de tijden veranderen ([ de Tijd van de Besparing van het Daglicht ](https://en.wikipedia.org/wiki/Daylight_saving_time)) resulterend in dubbelzinnige waarden, en, dientengevolge, kan het Bulk van Marketo API anders onverwachte dubbele lood terugkeren. Het is zeldzaam dat dit gebeurt, maar moet in om het even welke integratie worden rekenschap gegeven gebruikend datetime filterwaaiers. Ik heb één seconde verwijderd om duidelijk te maken dat de tijden inclusief zijn. Ik zou u niet willen denken dat het creëren van een baan met **createdAt** en **endAt** keer van **2016-08-01T00 :00: 00Z** en **2016-09-01T00 :00: 00Z 9&rbrace; zal respectievelijk niet leiden omvatten die op** worden gecreeerd 2016-09-01T00 :00: 00Z **; het zal.**

### Een taak maken

De eerste stap moet een baan tot stand brengen gebruikend [ creeer het eindpunt van de Baan van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createExportLeadsUsingPOST). In deze demo ziet het verzoek om onze eerste exporttaak te maken er als volgt uit:

`POST /bulk/v1/leads/export/create.json`

```json
{ "filter": {"createdAt": {"startAt": "2016-08-01T00:00:00",
                           "endAt": "2016-09-01T00:00:00"}},
"fields":["AccountSource","acquisitionProgramId","Active_c","address","Address_lead","annualRevenue","anonymousIP","BillingAddress","billingCity","billingCountry","BillingGeocodeAccuracy","BillingLatitude","BillingLongitude","billingPostalCode","billingState","billingStreet","blackListed","blackListedCause","city","CleanStatus","CleanStatus_account","company","CompanyDunsNumber","contactCompany","cookies","country","createdAt","customfield","DandbCompanyId","DandbCompanyId_account","dateOfBirth","dddS","department","doNotCall","doNotCallReason","dS","dueDate","DunsNumber","email","EmailBouncedDate","EmailBouncedReason","emailInvalid","emailInvalidCause","emailSuspended","emailSuspendedAt","emailSuspendedCause","externalCompanyId","externalSalesPersonId","facebookDisplayName","facebookId","facebookPhotoURL","facebookProfileURL","facebookReach","facebookReferredEnrollments","facebookReferredVisits","fax","firstName","gender","GeocodeAccuracy","holll","id","industry","inferredCity","inferredCompany","inferredCountry","inferredMetropolitanArea","inferredPhoneAreaCode","inferredPostalCode","inferredStateRegion","interested","interestedIn","isAnonymous","IsEmailBounced","isLead","iSTRUE","Jigsaw","JigsawCompanyId_account","JigsawContactId_lead","Jigsaw_account","Languages_c","lastName","LastReferencedDate","LastReferencedDate_account","lastReferredEnrollment","lastReferredVisit","LastViewedDate","LastViewedDate_account","Latitude","leadPartitionId","leadPerson","leadRevenueCycleModelId","leadRevenueStageId","leadRole","leadScore","leadSource","leadStatus","linkedInDisplayName","linkedInId","linkedInPhotoURL","linkedInProfileURL","linkedInReach","linkedInReferredEnrollments","linkedInReferredVisits","links","Longitude","MailingAddress","MailingGeocodeAccuracy","MailingLatitude","MailingLongitude","mainPhone","marketingSuspended","marketingSuspendedCause","middleName","mktoAcquisitionDate","mktocomment1","mktocomments2","mktoCompanyNotes","mktoDoNotCallCause","mktoIsCustomer","mktoIsPartner","mktoName","mktoPersonNotes","mktosync","mktotest1","mobile","mobilePhone","NaicsCode","NaicsDesc","newcustom","numberOfEmployees","originalReferrer","originalSearchEngine","originalSearchPhrase","originalSourceInfo","originalSourceType","OtherAddress","OtherGeocodeAccuracy","OtherLatitude","OtherLongitude","personPrimaryLeadInterest","personTimeZone","personType","phone","PhotoUrl","PhotoUrl_account","postalCode","priority","ProductInterest_c","rating","referal","registrationSourceInfo","registrationSourceType","relativeScore","relativeUrgency","requiredNumberofCylinder","salutation","sfdcAccountId","sfdcContactId","sfdcId","sfdcLeadId","sfdcLeadOwnerId","sfdcType","ShippingAddress","ShippingGeocodeAccuracy","ShippingLatitude","ShippingLongitude","sicCode","SicDesc","site","state","surveyAnswers","syndicationId","testBooleanField","testscore","title","totalReferredEnrollments","totalReferredVisits","Tradestyle","twitterDisplayName","twitterId","twitterPhotoURL","twitterProfileURL","twitterReach","twitterReferredEnrollments","twitterReferredVisits","uNSUBSCIBE","unsubscribed","unsubscribedReason","updatedAt","urgency","url","website","YearStarted"]}
```

De reactie ziet er als volgt uit:

```json
{
  "requestId": "6902#16fb52118bf",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Created",
          "createdAt": "2020-01-17T20:10:43Z"
      }
  ],
  "success": true
}
```

### De taak in de wachtrij plaatsen

De baan wordt nu gecreëerd, maar daar zitten we gewoon niets te doen. Om de baan in werking te stellen, moeten wij het [ navraageindpunt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/enqueueExportLeadsUsingPOST) roepen gebruikend de **exportId** waarde om URI voor het verzoek te bouwen. Dat ziet er als volgt uit:

`POST /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/enqueue.json`

Er is geen lichaam voor deze POST, we gebruiken hier gewoon het POST HTTP-werkwoord. Dat verzoek zal een reactie als dit produceren:

```json
{
  "requestId": "1836a#16fb5238a48",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Queued",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z"
      }
  ],
  "success": true
}
```

Zoals ik al zei, bent u beperkt in het aantal banen dat tegelijk kan worden uitgevoerd. Er is ook een grens aan het aantal banen in één keer in de rij: 10. We hebben meer dan 40 nodig om te voorkomen dat we alle banen tegelijk creëren. Andere integraties kunnen ook banen opleveren, dus we moeten rekening houden met de mogelijkheid dat alle slots vol zijn. Het proberen om een nieuwe baan te onderzoeken wanneer er reeds 10 een rij gevormde banen zijn zal a [ 1029 ](/help/rest-api/error-codes.md) fout produceren. Wanneer u a **1029** krijgt, gebruik een exponentiële backoff tot de baan kan worden nagezocht. Ik wacht 1 minuut en verdubbelt die waarde telkens als ik a **1029** foutencode tot 4 minuten tussen verzoeken, maar nooit langer dan dat krijg. Deze techniek is gekend als [ Afgeknot Binaire Exponential Backoff ](https://devopedia.org/binary-exponential-backoff) en is beste praktijken voor terugwinnbare fouten en statuscontroles.

### Wacht tot taak is voltooid

Elke baan vergt wat tijd om te lopen zodat zullen wij het [ statuseindpunt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsStatusUsingGET) roepen om zijn vooruitgang te controleren. Opnieuw, zullen wij **exportId** in het verzoek URI als dit omvatten:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/status.json`

Voordat de taak is voltooid, krijgt u een reactie die er als volgt uitziet:

```json
{
    "requestId": "153cb#16fb525435d",
    "result": [
        {
            "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-17T20:10:43Z",
            "queuedAt": "2020-01-17T20:13:23Z",
            "startedAt": "2020-01-17T20:13:49Z"
        }
    ],
    "success": true
}
```

Ik voer dezelfde exponentiële backoff uit (1 minuut tot 4 minuten) terwijl de baanstatus &quot;in de rij&quot;is. De status is geen real-time, maar één keer per minuut bijgewerkt en er is weinig voordeel om sneller te stemmen. Ik stel de backoff opnieuw in wanneer de baanstatus in &quot;Verwerking&quot;verandert. We wachten op een status &quot;Voltooid&quot; die er als volgt uitziet:

```json
{
  "requestId": "10ad9#16fb5268f9b",
  "result": [
      {
          "exportId": "4f2b9115-c3f2-4e40-a87c-bf803bbfed99",
          "format": "CSV",
          "status": "Completed",
          "createdAt": "2020-01-17T20:10:43Z",
          "queuedAt": "2020-01-17T20:13:23Z",
          "startedAt": "2020-01-17T20:13:49Z",
          "finishedAt": "2020-01-17T20:15:28Z",
          "numberOfRecords": 59,
          "fileSize": 74436,
          "fileChecksum": "sha256:de553362e7ffad6556ae9ea749655c35010c7f0e944fc5a85782183130dca18d"
      }
  ],
  "success": true
}
```

De **numberOfRecords** waarde is nul wanneer het verzoek geen lood terugkeert. Ik controleer deze waarde en sla de volgende stappen over wanneer dat zinvol is. Wanneer de lood zijn teruggekeerd, haalt ik **fileChecksum** waarde. We gebruiken dit om de integriteit van het bestand te controleren wanneer het wordt gedownload.

### Je leads ophalen

Als **numberOfRecords** groter is dan nul, download het uitgevoerde dossier gebruikend [ krijg het Dossier van de Leiding van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportLeadsFileUsingGET) met een verzoek als dit:

`GET /bulk/v1/leads/export/4f2b9115-c3f2-4e40-a87c-bf803bbfed99/file.json`

Controle dat het dossier zonder fout wordt overgebracht: bereken de controlesom van het dossier en vergelijk met **fileChecksum** wij vroeger bewaarde. Bereken de controlesom gebruikend [ SHA-2 ](https://en.wikipedia.org/wiki/SHA-2) en specifiek de SHA256 knoeiboelfunctie. Als de berekende controlesommen niet overeenkomen, is er een fout opgetreden in de bestandsoverdracht en kunt u de overdracht opnieuw proberen of de overdracht handmatig afbreken.

### De gegevens samenvoegen

Na het doorlopen van elke 31 dagen van de eerste lood tot vandaag, zult u een volledige reeks hebben. U hebt ook één bestand voor elk bereik. De eenvoudigste manier om één enkel samengevoegd dossier met elke lood te bouwen zou zijn deze dossiers na het verwijderen van de kopbalrij voor allen behalve het eerste dossier aaneen. Als u dat doet, vergeet niet om potentiële duplicatie later in uw gegevensverwerkingspijplijn te verwachten en te plannen. In mijn demonstratie, proces ik de dossiers aangezien ik hen download. Voordat u elke gegevensrij aan het uitvoerbestand toevoegt, dedupliceer ik door te controleren of de lood-id van de rij al is geschreven.

Ik heb één of andere demonstratiecode ontwikkeld die [ hier ](https://github.com/Marketo/REST-Sample-Code/tree/master/python/LeadDatabase/Leads) wordt ontvangen die hopelijk de details van dit proces invult en als malplaatje voor uw eigen ontwikkeling kan dienen. De demo-code is bedoeld als een leerinstrument, zodat er verbeteringen zijn in de robuustheid die nodig zijn voor een productiesysteem. **de code wordt verstrekt AS-IS** onder de vergunning van het MIT, maar het is waarschijnlijk goed genoeg voor eenmalig gebruik met menselijk toezicht. Niets weerhoudt je nu! Wanneer u dit proces volgt, haalt u elke lead op met de Marketo Bulk Extract-API en mogelijk elk veld voor de Marketo Engage-doelinstantie. Om uw gegevens verder uit te breiden, krijg de activiteiten van elke lood gebruikend de technieken.

Gepast op _2020-03-05_ door _Tony_

## Updates voor februari 2020

In februari 2020 geven we nieuwe REST API&#39;s vrij. Zie de volledige lijst met updates hieronder.

### Aankondigingen

* Na September 2020, [&#128279;](/help/rest-api/assets.md) Eindpunten van activa API  zullen niet meer **_method** vraagparameter goedkeuren. Dit werd gebruikt om vraagparameters in een POST lichaam over te gaan om de lengtebeperkingen van URI te mijden. Om tegemoet te komen aan verzoeken die deze parameter vereisen, wordt de URI-limiet voor de bron-API&#39;s verhoogd van 6KiB naar 65KiB.
* Met betrekking tot onze positie op ITP, gelieve dit Marketo Communautaire post te zien: [ Browser de Updates van het Koekje: Hoe Marketo/Munchkin wordt beïnvloed ](https://nation.marketo.com:443/t5/knowledgebase/browser-cookie-updates-how-marketo-munchkin-is-affected/ta-p/251524)
* Er is een wijziging aangebracht in de activiteit &#39;Status wijzigen in progressie&#39;. Het kenmerk &quot;Id van programmalid&quot; is toegevoegd ter ondersteuning van de aanstaande functie &quot;Aangepaste velden programmalid&quot;.

Gepost op _2020-02-26_ door _David_

## Vervangen van de Munchkin Associate Lead-methode

Met de volgende versie van de Cliënt van Munchkin JavaScript, versie 159, zullen wij afleiding van de Munchkin [ Geassocieerde methode van de Lood ](/help/javascript-api/api-reference.md) beginnen. Vanaf deze versie wordt, wanneer de methode wordt aangeroepen, in de browserconsole een waarschuwing weergegeven die aangeeft dat de methode in een toekomstige versie wordt verwijderd. Wanneer de methode is verwijderd, resulteren pogingen om de methode te gebruiken in een fout. Marketo-klanten die deze methode onlangs hebben gebruikt, zullen individueel op de hoogte worden gesteld van hun gebruik. Voor meer informatie over Munchkin 159 versie, [ zie deze Post van de Natie van de Marketing ](https://nation.marketo.com:443/t5/product-documents/munchkin-javascript-version-159-amp-associate-lead-deprecation/ta-p/299687).

### Veelgestelde vragen

Hoe weet ik of ik beïnvloed ben?

Adobe zal klanten op de hoogte stellen van het gebruik van deze methode op hun abonnement. Dit zal meerdere keren gebeuren tijdens de periode van afschrijving.

Wanneer wordt de methode verwijderd?

Deze methode wordt verwijderd in Munchkin JS v161, die naast de Marketo-release van oktober 2021 voor algemene beschikbaarheid is gepland.

Waarom wordt deze methode verwijderd?

Sedert de invoering van deze methode zijn prestatievere manieren geïmplementeerd en vrijgegeven om aan dezelfde gebruiksgevallen te voldoen. Om de prestaties en de gezondheid van onze diensten te verbeteren, is het soms nodig om functies te verwijderen die niet aan aanvaardbare normen voldoen.

Wat moet ik gebruiken in plaats van deze methode?

Munchkin Associate Lead heeft twee hoofdgebruikstoepassingen: het verzenden van persoonlijke gegevens en het koppelen van een webvolgcookie van een browser aan de overeenkomstige persoonrecord in Marketo. Sinds de introductie van Munchkin Associate Lead zijn robuustere manieren geïmplementeerd om gegevensverzending en cookie-koppeling uit te voeren.

#### Serververzending

Als u geen browser-zijvoorlegging nodig hebt, REST API biedt vele methodes voor [ de voorlegging van persoongegevens ](/help/rest-api/leads.md) aan, en a [ doel-gebouwde methode om koekjes met persoonverslagen ](/help/rest-api/leads.md) te associëren.

Wanneer wordt versie 159 van Munchkin uitgerold?

Versie 159 bevindt zich sinds de Marketo-release van oktober 2020 in General Availability.

Gepast op _2020-05-06_ door _Kenny_

## Een Marketo-formulierverzending maken op de achtergrond

Als uw organisatie veel verschillende platforms heeft voor het hosten van webinhoud en klantgegevens, wordt het redelijk gebruikelijk om parallelle verzendingen van een formulier nodig te hebben, zodat de resulterende gegevens op verschillende platforms kunnen worden verzameld. Er zijn verschillende strategieën om dit te doen, maar de beste is vaak de eenvoudigste: de Forms 2 API gebruiken om een verborgen Marketo-formulier te verzenden. Dit werkt met elk nieuw Marketo-formulier, maar bij voorkeur moet u hiervoor een leeg formulier maken, dat geen velden heeft. Zo zorgt u ervoor dat het formulier niet meer gegevens laadt dan nodig is, omdat we niets hoeven te renderen. Behaal nu enkel [ bed code ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) van uw vorm in en voeg het aan het lichaam van uw gewenste pagina toe, makend een kleine wijziging. Uw insluitcode bevat een formulierelement zoals:

`<form id="mktoForm_1068"></form>`

U wilt &#39;style=&quot;display:none&quot; aan het element toevoegen zodat het niet zichtbaar is, zoals hier:

`<form id="mktoForm_1068" style="display:none"></form>`

Als het formulier eenmaal is ingesloten en verborgen, is de code voor het verzenden van het formulier heel eenvoudig:

```javascript
var myForm = MktoForms2.allForms()[0];
myForm.addHiddenFields({
 //These are the values which will be submitted to Marketo
 "Email":"<test@example.com>",
 "FirstName":"John",
 "LastName":"Doe"
 });
myForm.submit();
```

Forms heeft op deze manier een formulier ingediend dat precies hetzelfde gedrag vertoont als wanneer de lead was ingevuld en een zichtbaar formulier heeft ingediend. Het activeren van de verzending varieert per implementatie, aangezien elke toepassing een andere actie heeft om de verzending te vragen, maar u kunt deze actie in feite uitvoeren op elke actie. Het belangrijkste onderdeel is het correct instellen van uw velden en waarden. Gebruik de SOAP API-naam van de velden die u kunt vinden bij Veldnamen exporteren om ervoor te zorgen dat de waarden correct worden verzonden.

### Migreren van Munchkin Associate Lead

Het verzenden van achtergrondformulieren is een van de aanbevolen vervangingsmethoden voor Munchkin Associate Lead. De HTML-voorbeeldpagina hieronder laat zien hoe u op een hoog niveau kunt migreren door dezelfde waarden opnieuw te gebruiken als die welke zijn ingediend bij Associate Lead.

```html
<html>
<head>
    <!--
      Munchkin Code
      Replace with your own instance code
    -->
    <script type="text/javascript">
        (function() {
          var didInit = false;
          function initMunchkin() {
            if(didInit === false) {
              didInit = true;
              Munchkin.init('CHANGE ME');
            }
          }
          var s = document.createElement('script');
          s.type = 'text/javascript';
          s.async = true;
          s.src = '//munchkin.marketo.net/munchkin-beta.js';
          s.onreadystatechange = function() {
            if (this.readyState == 'complete' || this.readyState == 'loaded') {
              initMunchkin();
            }
          };
          s.onload = initMunchkin;
          document.getElementsByTagName['head'](0).appendChild(s);
        })();
        </script>
</head>

<body> 
  <!--
    Start Embed code.  
    Pasted from Form Actions -> Embed Code except for addition of 'style="display:none"' to the form tag in order to hide it, and instance-specific codes redacted
    Replace with your own code for testing
  -->
  <script src="CHANGE ME"></script>
  <form id="CHANGE ME" style="display:none"></form>
  <script>MktoForms2.loadForm("CHANGE ME", "CHANGE ME", "CHANGE ME TO AN INTEGER ID");</script>
  <!--End Embed Code-->

  <!--Demo code-->
    <script>
        //The same map which is assembled for the Munchkin submission can be reused for the form submission
        let values = {
            "Email": "email@example.com",
            "FirstName": "Test",
            "LastName": "Record"
        }
        //whenReady lets us apply a callback to all mkto forms on the page
        //the callback function fires whenever a form is completely loaded
        //for most use cases this will just be used to capture a reference to the form for later usage
        //submission is done in line here for demonstration only
        MktoForms2.whenReady(function(form){

            //the addHiddenFields methods lets us add arbitrary fields to the form as well as their values
            form.addHiddenFields(values);

            //pass the same set of values to associateLead
            //hashString: secret + email
            Munchkin.munchkinFunction('associateLead', values, "CHANGE ME");
            
            //submit the form
            form.submit();
            
            
        })
    </script>
</body>

</html>
```

   
Gepast op _2020-05-26_ door _Kenny_

## Updates voor juli 2020

In juli 2020 publiceren we nieuwe REST API&#39;s, verbeteren we bestaande API&#39;s en verhelpen we fouten. Zie de volledige lijst met updates hieronder.

* Er zijn twee eindpunten toegevoegd waarmee u gebruikers die hun uitnodiging nog niet hebben geaccepteerd, kunt opvragen en verwijderen (dit zijn gebruikers in behandeling). [ krijgt Uitgenodigde Gebruiker door Identiteitskaart ](/help/rest-api/user-management.md) eindpunt staat u toe om een hangende gebruiker te vragen. Het [ Schrapping Uitgenodigde Gebruiker ](/help/rest-api/user-management.md) eindpunt staat u toe om een hangende gebruiker te schrappen.
* Het [ Uitnodigde Gebruiker ](/help/rest-api/user-management.md) eindpunt is bijgewerkt om volgzame datetime reeksen van ISO 8601 voor de **te aanvaarden** parameter.
* Zowel krijgen [ Gebruiker door Identiteitskaart ](/help/rest-api/user-management.md) en [ eindpunten van het Attribuut van de Gebruiker van de Update ](/help/rest-api/user-management.md) is bijgewerkt om de laatste gebruiker login tijd in het **lastLoginAt** attribuut terug te keren.
* Vaste kwestie waar [ leidt tot Statische ](https://developer.adobe.com/marketo-apis/api/asset/#operation/createStaticListUsingPOST) eindpunt een fout &quot;611, de fout van het Systeem&quot;zou terugkeren wanneer u probeerde om een statische lijst tot stand te brengen die reeds bestond. Gewijzigd voor de geretourneerde fout &quot;709, statische lijst met dezelfde naam bestaat al&quot;. [ LM-135934 ]
* Vaste kwestie waar [ E-mail ](https://developer.adobe.com/marketo-apis/api/asset/#operation/createEmailUsingPOST) eindpunt creëren een fout &quot;611, de fout van het Systeem&quot;zou terugkeren wanneer u probeerde om e-mail tot stand te brengen die reeds bestond. Gewijzigd in retourfout &quot;709, E-mail met dezelfde naam bestaat al&quot;. [ LM-138648 ]
* De vaste kwestie waar het Landing de vraageindpunten van de Pagina onjuiste **createdAt** waarden teruggaven. De eindpunten waren het tijdstip waarop een landingspagina voor het laatst werd goedgekeurd. Ze keren nu terug naar de tijd dat er een bestemmingspagina werd gemaakt. [ LM-138648 ]
* Vaste kwestie waar [ Samenvoegen leads ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/mergeLeadsUsingPOST) eindpunt een fout &quot;611, de fout van het Systeem&quot;voor ongeldige fusieverrichting zou terugkeren. Voorkwam toen de fusie in een dubbel lood resulteerde en **mergeinCRM** werd geplaatst aan waar. Gewijzigd om fout &quot;712 terug te keren, creeert u een dubbel verslag. We raden u aan in plaats daarvan een bestaande record te gebruiken.&quot; [ LM-137463 ]

Gepast op _2020-08-01_ door _David_

## Gast na - Diep duiken: Aangepast object versus Aangepaste activiteiten vs Aangepast veld

**dit is een gastpost van Amit Jain, Marketo Champion 2020, de Specialist van IT MarTech** als de automatiseringsplatform van de Marketing op bedrijfsniveau, Marketo beheert uw gebruiker/klant/perspectiefinformatie die uit verscheidene bronnen wordt verworven en handhaaft dat in Marketo voor betere verpersoonlijking, segmentatie en rapportering. Deze bronnen kunnen variëren van uw websiteformulieren tot lijstbuild tot uw CRM-gegevens tot uw e-commercegegevens. Marketo biedt standaardobjecten, zoals Lead, Company, Opportunity en Activity, enz. Deze standaardobjecten zijn soms niet voldoende om te voldoen aan de opkomende marketingbehoeften van uitgebreide gegevenssets die beschikbaar moeten zijn in Marketo.

Bijvoorbeeld onderdelen die aan het winkelwagentje zijn toegevoegd, studenten hebben een aanvraag ingediend voor verschillende cursussen, producten die eigendom zijn van een bepaalde persoon, toestemmingen voor verschillende abonnementen enz. Deze informatie kan voor Marketers kritiek zijn om de klant/het vooruitzicht betrokken te houden door meer gepersonaliseerde inhoud en verenigde gebruikerservaring over platforms te verstrekken. Om aan deze bedrijfsbehoeften tegemoet te komen, biedt Marketo aangepaste manieren om dit type gegevens op te slaan in Aangepaste velden, Aangepaste activiteiten en Aangepaste objecten. Ik heb gezien dat mensen problemen hebben met het gebruik van de opties. In dit blogbericht, **zullen wij in detail begrijpen wat deze drie dingen eigenlijk zijn en wanneer om te gebruiken welke met sommige voorbeelden.**

**de Gebieden van de Douane**

Het object Lead in Marketo is het hoofdobject en alle andere objecten zijn direct of indirect met dit object verbonden. Met Marketo kunt u aangepaste velden maken voor het object &quot;Lead&quot;, het object &quot;Company&quot;, en onlangs hebben ze de ondersteuning aangekondigd van aangepaste velden &quot;Program member&quot;. Deze aangepaste velden voldoen aan uw behoefte om bepaalde soorten gegevens op te slaan. U moet bijvoorbeeld mogelijk de taakfunctie opslaan of andere toestemmingen van de gebruiker. Er zijn twee typen aangepaste velden in Marketo:

1. **Gesynchroniseerd van de Gebieden van CRM:** als deel van de autoMarketo ↔︎ de synchronisatie van SFDC, synchroniseert Marketo alle douanegebieden zichtbaar aan de gebruiker van Marketo in SFDC op Lood, Contact, Rekening en Voorwerp van de Opportunity.
1. **Alleen de Velden van Marketo:** U kunt een gebied in Marketo direct tot stand brengen. De gegevens van deze velden worden niet gesynchroniseerd met SFDC.

**Snelle Uiteinden**

* Hebt u een alleen voor Marketo bestemd veld en wilt u de gegevens nu synchroniseren in SFDC? Controle uit [ dit blog ](https://themarketingautomationblog.com/2019/12/06/field-management-merging-remapping-hiding-marketo-fields/) leren hoe u dat kunt doen.
* Leer meer over douanevelden [ hier ](https://themarketingautomationblog.com/2019/11/15/knowing-your-marketo-fields/).
* Marketo API&#39;s bieden momenteel geen ondersteuning voor het bijwerken/maken van aangepaste velden.

**de Voorwerpen van de Douane**

Naast standaardobjecten kunt u in Marketo ook uw eigen aangepaste objecten maken. _u kunt douanevoorwerpen tot stand brengen en op of Onderneming of het voorwerp van de Lood of een ander douanevoorwerp betrekking hebben._ Een aangepast object is een set aangepaste records die een aanvulling vormen op de standaardrecords voor leads en bedrijven. De voorwerpen van de douane staan u toe om extra gegevens op een scalable manier op te slaan en die gegevens te verbinden met een Lead of verslag van het Bedrijf. U kunt een douanevoorwerp met om het even welke combinatie standaard (de Gebieden van de Verbinding) en douanegebieden bouwen, die gebieden bevolken om douaneobjecten _verslagen_ tot stand te brengen, en dan die verslagen te verbinden met een Lood of een verslag van het Bedrijf. Krachtige en flexibele, verbonden verslagen verrijken uw Segmenten, Slimme Lijsten, en Campagnes door u die activa met informatie te laten bouwen die niet in de gebieden van het Lood en van het Bedrijf wordt gevonden. Een aangepast object kan een van de volgende typen relaties hebben:

* **Één-aan-één Verhouding**: waar elk douanevoorwerp één enkel Lead/het objecten van het Bedrijf verslag heeft.
* **Één-aan-Vele Verhouding**: waar elk douanevoorwerp veelvoudige douaneobjecten verslagen met betrekking tot een Lood/Bedrijf bevat.
* **Vele-aan-Vele Verhouding:** waar de veelvoudige verslagen van douaneobjecten met veelvoudige lood/bedrijfvoorwerpen kunnen verbinden. Bijvoorbeeld, worden de veelvoudige studenten ingeschreven in veelvoudige cursussen van een cursuscatalogus.

**Snelle Uiteinden**

* Leer meer aan opstellings douanevoorwerpen [ hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/home).
* U kunt het aangepaste Marketo-object gebruiken als een tussenliggend object en ook een aangepast object van een aangepast object bedoelen.

### Waarom aangepaste objecten?

Met aangepaste objecten kunt u unieke gegevens compileren en gebruiken die relevant zijn voor een bedrijf of lead, maar niet noodzakelijkerwijs statische informatie over het bedrijf zijn of zelf leiden. Terwijl de loodgebieden op de loodinformatie van een individu (_E-mailAdres_, _Code van het ZIP_, etc.), bedrijfsinformatie (_Titel van de Baan_, _Industrie_, etc.), of systeem-gedreven informatie (_identiteitskaart van Marketo_, een identiteitskaart van SFDC, of Create Datum enz.) betrekking hebben, zijn de gebieden van de douaneobjecten volledig klantgericht. Gebruik bijvoorbeeld aangepaste objecten voor het opslaan van gegevens zoals:

* De aankoopgeschiedenis van een gebruiker.
* Winkelgegevens.
* Of een klant al dan niet één van uw beperkte tijd, promotionele kortingscodes heeft gebruikt.
* Aanvullende informatie die is verkregen uit enquêteresultaten, interviews en aanwezigheid van gebeurtenissen.
* De proefgegevens van een gebruiker, waaronder de URL van de proefversie, de begindatum, de einddatum, het aantal gebruikers enz.

### Aangepaste objectbeperkingen van Marketo

1. Standaard kunt u in Marketo 10 aangepaste objecten maken. Je kunt de limiet verhogen met extra abonnementskosten.
1. Het standaardaantal velden per object is 50, maar u kunt desgewenst Marketo-ondersteuning voor extra velden aanvragen. Dank u [ Michael Florin ](https://www.linkedin.com/in/michaelflorin/) voor extra input hier.
1. Er geldt een limiet voor het aantal records dat u kunt hebben voor alle aangepaste objecten. Dit hangt af van je abonnement op Marketo. Deze limiet kan worden verhoogd met extra abonnementskosten.
1. Als een aangepast object is gemaakt met de API, staat Marketo het CO-schema niet toe vanuit de gebruikersinterface van Marketo.

**Snelle Uiteinden**

* Marketo API&#39;s ondersteunen de CRUD-bewerking (Maken, Lezen, Bijwerken en Verwijderen) voor aangepaste objecten.
* U kunt deze gegevens van douaneobjecten voor e-mailverpersoonlijking gebruiken gebruikend het Manuscript van de Snelheid.
* U kunt alle objecten van de Douane verwante eindpunten [ hier ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getExportProgramMembersStatusUsingGET) vinden.


### Terminologie voor aangepaste objecten

**Voorwerp van de Douane**: Een container die een groepering van alle verslagen van het douanevoorwerp houdt. Wordt formeel een gegevenskaartset of een aangepaste tabel genoemd. **Verslag van het Voorwerp van de Douane**: Het verslag van gegevens die extra gebiedsinformatie houden die aan een lood of een bedrijf kan worden gebonden. Een record kan bestaan uit standaardvelden voor leads of bedrijven en aangepaste recordvelden voor objecten. Formulier bekend als een gegevenskaart of rij van de gegevenslijst. **het Gebied van het Verslag van het Voorwerp van de Douane**: Volledig aanpasbare gebieden om unieke of tijdelijke informatie te verzamelen. Deze velden worden gemaakt en gehuisvest in het aangepaste object zelf. Formulier bekend als een gegevenskaartveld of databasetabelveld/-kolom. **Gebied van de Verbinding**: Speciaal type van het gebied van het douaneobjecten verslag om het verband tussen het verslag van het douanevoorwerp en het verbonden Lood/het objecten verslag van het Bedrijf te bepalen. Wanneer u aangepaste objecten maakt, moet u koppelingsvelden opgeven om de aangepaste objectreeks te koppelen aan de juiste bovenliggende record.

* Voor een één-op-veel of één-op-één douanestructuur, gebruik het verbindingsgebied in het douanevoorwerp om het met een persoon of een bedrijf te verbinden.
* Voor een vele-aan-vele structuur, gebruikt u twee verbindingsgebieden, die van een afzonderlijk gecreeerd intermediair voorwerp worden verbonden (dat een type van douanevoorwerp, ook is). Eén koppeling maakt verbinding met personen of bedrijven in uw database en de andere koppeling maakt verbinding met het aangepaste object. In dit geval bevindt het koppelingsveld zich niet in het aangepaste object zelf.

### Aangepaste activiteiten

**Er zijn verscheidene manieren iemand met onze organisatie kan in wisselwerking staan. Ze kunnen de website van uw bedrijf bezoeken, een van uw winkels bijwonen of op een koppeling klikken in een e-mailbericht dat u hebt verzonden. Deze acties zijn activiteiten**, en welke actie zij ook nemen, Marketo vangt het zodat kunnen uw Marketing en de Teams van de Verkoop beter het gedrag van de gebruiker voor gepersonaliseerde en verenigde betrokkenheid begrijpen. **_de Activiteiten van de Douane_** _kunnen u helpen een activiteit volgen die niet verwant met een vorm van Marketo, e-mail, of het landen pagina_ is. Als u bijvoorbeeld wilt bijhouden wanneer iemand een video op een website heeft bekeken of een enquête heeft gehouden, gebruikt u een aangepaste activiteit. Aangepaste activiteiten verschillen van aangepaste objecten. Gebruik aangepaste objecten wanneer de waarde kan worden gewijzigd (de kleur van de auto verandert van blauw in rood). Gebruik aangepaste activiteiten bij het bijhouden van momenten die zich hebben voorgedaan en de details ervan kunnen niet worden gewijzigd (d.w.z. &quot;gekochte auto&quot;). Standaard is de limiet van het maximale aantal aangepaste activiteiten dat kan worden gedefinieerd 10. Dit kan worden verhoogd met extra abonnementskosten. Zoals per het [ beleid van het de gegevensbehoud van Marketo ](https://nation.marketo.com/t5/knowledgebase/tkb-p/support_solutions-documents), zullen de douaneactiviteiten automatisch na 25 maanden worden geschrapt.

**Activiteit van de Douane:** niet-Marketo gebeurtenissen die u binnen Marketo zou willen volgen. **identiteitskaart van de Activiteit van de Douane:** Marketo wijst een numerieke identiteitskaart aan de douaneactiviteit toe die terwijl het proberen kan worden gebruikt om de activiteitengegevens te duwen/te trekken gebruikend Marketo API. **de Gebieden van de Activiteit van de Douane:** de meta- gegevens van de Activiteit kunnen op een activiteitengebied worden opgeslagen. Als u bijvoorbeeld de weergaven van video bijhoudt, kunt u de velden Pagina-URL, Video-titel enzovoort gebruiken. **Primair Gebied van de Activiteit van de Douane:** de activiteitengebieden van de Douane die als slimme criteria van de lijstfilter kunnen worden gebruikt.

**Snelle Uiteinden**

* De API eindpunten voor douaneactiviteiten zijn beschikbaar [ hier.](https://developer.adobe.com/marketo-apis/api/mapi/#operation/addCustomActivityUsingPOST)

## Aangepast object versus aangepaste activiteit

**Voorwerp van de Douane**

**Activiteit van de Douane**

1

Standaard worden maximaal 10 aangepaste objecten per instantie gebruikt.

Standaard zijn er maximaal 10 aangepaste activiteiten.

2

Maximaal 50 aangepaste objectvelden per aangepast object.

Elk type van douaneactiviteit kan tot 20 secundaire attributen hebben.

3

Max. 1 MM aangepaste objectreeks; mogelijk zeer gebaseerd op uw abonnement.

Na 25 maanden worden aangepaste activiteiten verwijderd volgens het beleid van Marketo voor het bewaren van gegevens.

4

Kan worden gebruikt als Filter en Trigger in Slimme Lijsten en Slimme Campagnes.

Kan worden gebruikt als Filter en Trigger in Slimme Lijsten en Slimme Campagnes.

5

Kan worden gebruikt om de e-mailinhoud aan te passen.

Kan niet worden gebruikt om de e-mailinhoud aan te passen.

6

Kan CRUD-bewerking uitvoeren op een aangepaste objectopname.

Alleen Maken en lezen is toegestaan in aangepaste activiteiten.

7

Gebruik aangepaste objecten wanneer de waarde kan worden gewijzigd (de kleur van de auto verandert van blauw in rood).

Gebruik aangepaste activiteiten bij het bijhouden van momenten die zich hebben voorgedaan en de details ervan kunnen niet worden gewijzigd (d.w.z. &quot;gekochte auto&quot;).

8

Aangepaste objecten vertellen je dat. d.w.z. contante waarde.

De Activiteiten van de douane vertellen u de gebeurtenissen die in het verleden zijn gebeurd.

 

Gepast op _2020-10-18_ door _Amit_

## Updates voor januari 2021

In januari 2021 brengen we nieuwe REST API&#39;s vrij en verhelpen we verschillende defecten. Zie de volledige lijst met updates hieronder.

* Toegevoegd [ legt 1&rbrace; eindpunt van de Vorm voor &lbrace;die u toestaat om programmatic vormvoorlegging uit te voeren. ](/help/rest-api/leads.md) Formulieren van derden kunnen nu worden geïntegreerd met Marketo-formulieren om te profiteren van bestaande marketingworkflows.
* Toegevoegd [ krijgt het Bestaan van de Pagina Volledige Inhoud ](/help/rest-api/landing-pages.md) eindpunt dat de in series vervaardigde versie van HTML van een het landen pagina terugkeert. Hiermee kunt u volledig persoonlijke voorvertoningen van bestemmingspagina&#39;s weergeven zonder u aan te melden bij Marketo Engage. Dit kan helpen bewerkings- en vertaalworkflows in geïntegreerde toepassingen stroomlijnen.
* U kunt het aantal douanevoorwerpen nu vormen beschikbaar voor toegang via het manuscript van de Snelheid. De instructies van de configuratie kunnen [ hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/email-setup/change-custom-object-retrieval-limits-in-velocity-scripting) worden gevonden.

### Resoluties beschermen

* Vaste kwestie waar het [ punt van de Gebruiker van de Schrapping ](/help/rest-api/user-management.md) u zou toestaan om een API-Enige Gebruiker te schrappen die door de Dienst van de Douane in gebruik was. Er wordt nu een fout &quot;611, u kunt een API-gebruiker die in API-service wordt gebruikt, niet verwijderen.&quot; [ LM-141893 ]
* Vaste kwestie waar [ krijgt Gebruikers ](/help/rest-api/user-management.md) eindpunt verwijderde gebruikers in sommige gevallen zou terugkeren. [ LM-141542 ]
* Vaste kwestie waar [ het eindpunt van het Programma van de Kloon ](https://developer.adobe.com/marketo-apis/api/asset/#operation/cloneProgramUsingPOST). Als u een programmanaam hebt opgegeven die langer is dan 255 tekens, wordt &quot;611. Kan programmafout niet klonen&quot; geretourneerd. Nu wordt &quot;701. De naam mag niet meer dan 255 tekens bevatten&quot;. [ LM-143436 ]
* Vaste kwestie met [ goedkeuren het Aanvoeren van het 1&rbrace; eindpunt van het Ontwerp van de Pagina ](https://developer.adobe.com/marketo-apis/api/asset/#operation/approveLandingPageUsingPOST). Wanneer u een openingspagina hebt goedgekeurd en de mobiele versie is geactiveerd, ziet u in bepaalde gevallen de inhoud van de mobiele versie in de desktopversie. [ LM-146867 ]
* Vaste kwestie met [ Ongoedgekeurd het LandingPagina ](https://developer.adobe.com/marketo-apis/api/asset/#operation/unapproveLandingPageByIdUsingPOST) eindpunt dat u toestond om een het landen pagina niet goed te keuren die als follow-up pagina door één of meerdere vormen in gebruik was. Er wordt nu een fout &quot;709, Unapproval landing page failed. De landende pagina is in gebruik door één of meerdere vormen als follow-up pagina met vorm IDs:[_formId1, formId2,..._]&quot;. [ LM-143326 ]

Gepost op _2021-01-15_ door _David_

## Munchkin 160 Beta en Beacon API

**jan 27th 2021:** Sommige gebruikers van Marketo die door de Verval van de Lood van de Vereniging zullen worden beïnvloed ontvingen een e-mailbericht in fout erop wijzend dat zij Munchkin Beta hebben die op één of meerdere van hun instanties wordt toegelaten. Deze release wordt gehouden totdat het juiste publiek op de hoogte is gesteld. Beginnend met versie 160 van Munchkin JavaScript, wordt [ baken API ](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API) de standaardmanier die Munchkin met de achtergrond van Marketo communiceert. Dit werd beschikbaar als optie in Zomer 2020 met de versie van versie 159 via de **configuratieparameter 0&rbrace; useBeaconAPI.** De API van het baken heeft verscheidene voordelen boven het gebruiken van de oude methode XMLHttpRequest, maar de belangrijkste verbetering is dat het een niet-blokkerende asynchrone API voor de mededeling van HTTP is die voor gebruik in alle moderne browsers van Internet beschikbaar is. Hoewel de meeste gebruikers van Munchkin geen verandering in websitegedrag zullen opmerken, zal deze update Munchkin verhinderen navigatie te blokkeren terwijl het wachten om een klikgebeurtenis aan het achtereind voor te leggen, of eenvoudigweg, dit alles maar elimineert de mogelijkheid van Munchkin dat browser &quot;hangt&quot;na het klikken van een verbinding aan een nieuwe pagina. Dit is een zeldzaam maar frustrerend voorval voor sommige Marketo-klanten.

Vanaf 27 januari 2021 is de uitrol van deze versie in afwachting van een herschikking in behandeling. Hoewel er geen problemen met betrekking tot deze wijziging worden verwacht en er geen problemen zijn vastgesteld tijdens het testen, is het voor Marketo onmogelijk om alle mogelijke implementatieconfiguraties van Munchkin te testen en u kunt deze wijzigingen vooraf testen of u wilt deze wijziging opgeven totdat deze versie algemeen beschikbaar is. Hieronder vindt u instructies voor verschillende scenario&#39;s.

### API voor testbaken

Als u wenst om bijgewerkte bakens API in afwachting van de aanstaande versie te testen, kunt u dit doen, door de **parameter 0&rbrace; useBeaconAPI aan uw configuratie van Munchkin op een externe testpagina toe te voegen.** Deze test werkt met de algemeen beschikbare of bètaversie van Munchkin. De configuratieparameter wordt hieronder weergegeven in het tweede argument van het aanroepen van de methode `Munchkin.init()` op regel 7: `{ 'useBeaconAPI': true}`

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827', {"useBeaconAPI":true});
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

### Munchkin Beta uitschakelen op Marketo-bestemmingspagina&#39;s

Om Munchkin Beta op Marketo landende pagina&#39;s onbruikbaar te maken, moet u tot uw [&#128279;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) menu van de Controle van de Treasure toegang hebben  in de Admin sectie van uw abonnement en Munchkin Beta veranderen op het Aanvoeren van Pagina&#39;s die aan gehandicapten plaatsen.

### Munchkin Beta uitschakelen op externe pagina&#39;s

Als u de Beta-versie van Munchkin JavaScript hebt geïmplementeerd op externe webpagina&#39;s en deze wijziging wilt opgeven totdat deze algemeen beschikbaar is, moet u het Munchkin JS-fragment wijzigen en de **munchkin als doel instellen.** **js** dossier in plaats van **munchkin-bèta.** **js** dossier. In het voorbeeld hieronder, is dit de waarde van **s.src** variabele op lijn 11. Uw fragment lijkt mogelijk niet veel op het voorbeeld, of wordt door een tagmanager op uw externe pagina&#39;s geïmplementeerd en u moet mogelijk uw IT-bronnen raadplegen of wie uw websites beheert met Munchkin tracking ingeschakeld.

```javascript
<script type="text/javascript">
(function() {
  var didInit = false;
  function initMunchkin() {
    if(didInit === false) {
      didInit = true;
      Munchkin.init('299-BYM-827');
    }
  }
  var s = document.createElement('script');
  s.type = 'text/javascript';
  s.async = true;
  s.src = '//munchkin.marketo.net/munchkin.js';//This line should have the munchkin.js file, not munchkin-beta.js
  s.onreadystatechange = function() {
    if (this.readyState == 'complete' || this.readyState == 'loaded') {
      initMunchkin();
    }
  };
  s.onload = initMunchkin;
  document.getElementsByTagName['head'](0).appendChild(s);
})();
</script>
```

Gepast op _2021-01-08_ door _Kenny_

## Definitieve API-afschrijving van e-mail V1

[ Verdringing van e-mail V1 begon bijna twee jaar geleden ](https://nation.marketo.com:443/t5/knowledgebase/email-editor-1-0-is-being-deprecated-june-18th/ta-p/250666) en beginnend met de de onderhoudsversie van Maart aan de abonnementen van Londen &amp; Nederland op 17 Maart, 2021 en alle andere abonnementen op 19 Maart, 2021, zal alle API steun voor V1 e-mail worden beëindigd. Na deze release zullen pogingen om via de API&#39;s voor middelen te communiceren met e-mails van V1 fouten en geen acties meer tot gevolg hebben. Alle bekende resterende gebruikers zijn sinds 24 februari 2021 op de hoogte gesteld, maar het is mogelijk dat er nog steeds sprake is van integratie die kan proberen met deze activa te communiceren. De meest voorkomende typen integratie die hiervan het slachtoffer zijn, zijn services die digitaal beheer, vertaling en lokalisatie van bedrijfsmiddelen bieden. Als u integratiemislukkingen als resultaat van deze verandering waarneemt, [ zult u nog problematische activa kunnen bevorderen door hen uit te geven en goed te keuren ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/transitioning-to-email-editor-2-0). Zodra een e-mailmiddel aan V2 wordt bevorderd, zou u het gebruiken met de geïntegreerde diensten moeten kunnen hervatten.

Gepast op _2021-03-17_ door _Kenny_

## Updates van mei 2021

In mei 2021 geven we nieuwe REST API&#39;s uit, verbeteren we de bestaande REST API&#39;s en verhelpen we verschillende defecten. Zie de volledige lijst met updates hieronder.

* Toegevoegde API&#39;s voor programmaleden waarmee u de lidmaatschapsrecords voor programma&#39;s kunt ophalen, bijwerken en verwijderen. Voor meer informatie zie [ REST API > het Gegevensbestand van het Lood > de Leden van het Programma ](/help/rest-api/program-members.md).
* Toegevoegde Bulk API&#39;s voor het uitpakken van aangepaste objecten die u toestaan Marketo Custom Object-records op het eerste niveau te exporteren die zijn gekoppeld aan leads in een een-op-een relatie. Voor meer informatie zie [ REST API > Bulk Extraheren > het Bulk Uittreksel van het Voorwerp van de Douane van de Douane ](/help/rest-api/bulk-custom-object-extract.md).
* Wij hebben zowel de [ Lood API ](/help/rest-api/leads.md) als [ Bulk Lood Extraheren API ](/help/rest-api/bulk-lead-extract.md) verbeterd om gebruikers toe te staan om identiteitskaart van Adobe Experience Cloud (ECID) terug te winnen. Dit staat gebruikers toe die [ Synchroniseren Soorten publiek van Adobe Experience Cloud ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/set-up-adobe-experience-cloud-audience-sharing.html?lang=nl-NL) om lood te identificeren die ECIDs hebben geassocieerd. Dit opent omhoog [ integratiemogelijkheden ](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360024277392-Adobe-Experience-Cloud-Using-the-ECID-for-integration) met andere producten van Adobe Experience Cloud.
* Wij hebben [ BulkLood de Invoer API ](/help/rest-api/bulk-lead-import.md) verbeterd om toe te voegen leidt tot bedrijfverslagen tijdens het de invoerproces te steunen. Dit wordt gedaan door het **externalCompanyId** gebied aan het de invoerdossier te omvatten.
* Wij hebben verscheidene eindpunten van het Programma verbeterd om pariteit van functionaliteit te voorzien die in Marketo Engage UI wordt gevonden. Wij hebben [ verbeterd creeer Programma&#39;s ](/help/rest-api/assets.md) en [ kloon Programma&#39;s ](https://developer.adobe.com/marketo-apis/api/asset/) eindpunten om, verrichtingen op gebeurtenisprogramma&#39;s tot stand te brengen, te klonen of te bewegen. Dit is voor gebruikers die gebeurtenisprogramma&#39;s organiseren door ze onder andere programmatypen te nesten. Wij hebben ook het [ punt van het Programma van de Schrapping ](https://developer.adobe.com/marketo-apis/api/asset/) verbeterd om schrapping van programma&#39;s toe te laten die de volgende activa bevatten: De Berichten van de duw, Berichten in-app, Rapporten, het Bestaan van Pagina&#39;s met Ingesloten Sociale Assets.
* Als Admin van Marketo, kunt u [ een specifiek gebied als &quot;gevoelig&quot;merken ](https://experienceleague.adobe.com/nl/docs/marketo/using/home) zodat worden zijn waarden [ nooit voorgevuld in vormen ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/forms/form-fields/disable-pre-fill-for-a-form-field), daardoor beschermend gebruikers&#39; gevoelige gegevens. We hebben verschillende eindpunten voor formuliervelden verbeterd zodat deze gelijk zijn aan deze functionaliteit die is gevonden in de gebruikersinterface van Marketo Engage.

### Resoluties beschermen

* Probleem opgelost met het eindpunt van het programma verwijderen. Als u een programma in een gedeelde omslag probeerde te schrappen, zou het &quot;611, de Fout van het Systeem&quot;terugkeren. Nu wordt &#39;&#39;Doelprogramma bevindt zich in een gedeelde map en kan het niet worden verwijderd.&#39;&#39; De map moet niet worden gedeeld voordat u de map kunt verwijderen.&quot;
* Probleem verholpen met eindpunt van kloonprogramma. Als u probeerde om een programma te klonen dat een DateTime in een stroomstap bevatte, zou het &quot;611, de Fout van het Systeem&quot;terugkeren. Nu klonen ze het programma.
* Probleem opgelost met het eindpunt Programma&#39;s maken dat u per ongeluk een programma onder een e-mailprogramma kon maken (wat niet is toegestaan).
* Probleem verholpen met eindpunt van kloonprogramma. Als u een programma hebt gekloond dat een bestemmingspagina bevatte, ontbrak de naam van de landingspagina in het doelprogramma een onderstrepingsteken tussen de naam van het programma en de naam van de landingspagina. bijv. `http://<_pod_\>.marketo.com/lp/<_munchkin_\>/<_program name_\>**_**<_LP name_\>.html`


Gepost op _2021-05-07_ door _David_

## Beperkt tijdaanbod voor deelname aan Adobe Exchange

De steun van de partnergemeenschap van Marketo Engage is één van de pijlers van het succes van onze klanten. Wij willen ervoor zorgen dat het ecosysteem van de Integratie van Marketo Engage goed vertegenwoordigd is in de Marketplace van de Uitwisseling, en een speciale aanbieding voor de partners van LaunchPoint hebben. Voor een zeer beperkte tijd die niet zal worden uitgebreid, bieden wij onze partners van LaunchPoint een vrij Innovate partnerschap in het programma van de Uitwisseling aan tegen eind 2022 (ongeveer $15k waarde). We hebben dit aanbod ontworpen om de partners van LaunchPoint aan te moedigen hun integratieaanbiedingen te maken in het Exchange Partner Portal, dat dan openbaar doorzoekbaar zal zijn in de Adobe Exchange Marketplace. Om een volledige lijst te zien van de voordelen van het Innovate Partnerschap die u tot december 2022 zonder kosten ontvangt.

1. Ga naar [ het Centrum van de Steun van de Partner van Adobe Exchange ](https://adobeexchangeec.zendesk.com/hc/en-us?mkt_tok=NjA4LURIVi05MTUAAAF-P5lIeVWOuBmKMS_uE_NpgFKtC0ukt7z_ksnq_Sbzb6mzXUuXpqpqQeujtPdZ24WcjMdptygQSR9XrYt_Cw)
1. Klik in de rechterbovenhoek op &quot;Een verzoek verzenden&quot;
1. In **te kiezen gelieve uw kwestie onder** dropdown kies &quot;Steun van Adobe Exchange&quot;
1. In **Uw e-mailadres** ga uw e-mailadres in
1. In het **Onderwerp** vakje gaat &quot;Aanbieding van het Punt van de Lancering&quot; in
1. In de **doos van de Beschrijving** gaat &quot;Aanbieding van het Punt van de Lancering&quot; in
1. In **Type van Steun** dropdown uitgezochte &quot;Steun van het Programma&quot;
1. In het **dropdown van het Product van 0&rbrace; Adobe Exchange uitgezochte &quot;Programma van Adobe Exchange&quot;**
1. Verzend het formulier. Ons team heeft binnenkort contact met u.

Gepost op _2021-07-22_ door _David_

## Updates voor augustus 2021

In augustus 2021 verbeteren we de bestaande REST API&#39;s en verhelpen we verschillende defecten. Zie de volledige lijst met updates hieronder.

* We hebben de API voor het uitpakken van bulkactiviteiten verbeterd zodat gebruikers kunnen filteren met primaire kenmerken voor 6 verschillende typen activiteiten. Voor meer informatie zie [ Uittreksel van de Activiteit van de Bulk ](/help/rest-api/bulk-activity-extract.md).
* Om Marketo Sales Connect-gebruikers meer toegang te geven tot hun gegevens over verkoopactiviteiten, hebben we extra kenmerken voor verkoopactiviteiten ingeschakeld. We hebben de volgende kenmerken toegevoegd voor het verzenden van e-mail, het openen van e-mails over verkopen en het klikken op e-mailactiviteiten over verkoop:


* Marketo Sales Person ID - Unieke ID voor persoonrecord in Sales Connect
* Naam verkoopcampagne - Naam verkoopcampagne, als e-mail deel uitmaakte van een verkoopcampagne
* Verkoopcampagne-URL - Verkoop Connect-URL voor verkoopcampagne als e-mail onderdeel was van een verkoopcampagne
* Naam verkoopsjabloon - Naam e-mailsjabloon in Sales Connect als een sjabloon is gebruikt
* Verkoopsjabloon-URL - Verkoop Connect-URL voor e-mailsjabloon als een sjabloon is gebruikt

### E-mails

* We hebben het eindpunt E-mails ophalen verbeterd door het filter `earliestUpdatedAt`/`latestUpdatedAt` toe te voegen. Op deze manier kunt u het veld `updatedAt` gebruiken om alleen te zoeken naar een subset e-mailberichten, waardoor incrementele synchronisatie mogelijk is.
* Wij hebben Get E-mail, krijgen E-mail door Naam verbeterd, krijgen E-mail door de eindpunten van identiteitskaart om terugwinning van [ Champion en Challenger ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/email-tests-champion-challenger/add-an-email-champion-challenger) type e-mailverslagen te steunen.

### Resoluties beschermen

* Het probleem met het eindpunt Gebruikers ophalen is opgelost. De gebruikers die a [ vergunning van de Kalender van de Marketing ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/marketing-calendar/understanding-the-calendar/issue-revoke-a-marketing-calendar-license) waren verleend werden niet teruggekeerd. Gebruikers van de marketingkalender worden nu correct geretourneerd.
* Probleem opgelost met het eindpunt Formulier verzenden. In het geval van dubbele lead records wordt Formulier verzenden gebruikt om de fout &#39;1007, Multiple lead match lookup criteria&#39; uit te geven. Verzend Vorm nu werkt het recentst bijgewerkte verslag op de zelfde manier bij dat [ Forms 2.0 API ](/help/javascript-api/forms-api-reference.md).
* Er zijn verschillende misleidende foutberichten verbeterd die zijn geretourneerd door het veld Regelafstand bijwerken en Eindpunten voor leidende velden maken. [ LM-151890, LM-151888, LM-151889 ]
* Probleem opgelost met het veld Ophalen op naam en de eindpunten van de velden Ophalen. Beide eindpunten kunnen mogelijk iets verouderde informatie retourneren. Ze retourneren nu altijd actuele informatie.
* Vaste kwestie met [ Bulk trekt API ](/help/rest-api/bulk-extract.md) wanneer het gebruiken van de &quot;waaier&quot;kopbal van HTTP voor gedeeltelijke herwinning waar de laatste byte in de waaier niet werd teruggekeerd.
* Correctie van probleem met het eindpunt van metagegevens van fragment bijwerken. Bij het bijwerken van de fragmentnaam of -beschrijving is de fragmentstatus gewijzigd in &quot;goedgekeurd met concept&quot;. De status van het uitgesneden fragment blijft nu ongewijzigd nadat de naam of beschrijving van het fragment is bijgewerkt.
* Probleem verholpen met eindpunt van e-mailmodule toevoegen. Wanneer het toevoegen van een module die een fragment bevatte, keerde het &quot;611, de Fout van het Systeem&quot; terug. De module wordt nu correct toegevoegd aan de e-mail.
* Probleem opgelost met het eindpunt van het programma verwijderen. Wanneer het schrappen van een programma dat een lokaal middel van het Bericht in-App bevatte, keerde het &quot;611, de Fout van het Systeem terug&quot;. Het programma wordt nu correct verwijderd.

Gepost op _2021-08-22_ door _David_

## Munchkin-versie 161-rollout

Op 7 september 2021 begint versie 161 van Munchkin met het uitrollen van 10% van de abonnementen met Munchkin Beta ingeschakeld, gevolgd door 50% op 16 september en 100% op 30 september. Deze wijziging heeft invloed op Marketo-bestemmingspagina&#39;s en op de versie van het bestand munchkin-beta.js die wordt gebruikt voor externe bestemmingspagina&#39;s die worden geladen van abonnementen waarop de nieuwe versie is geïmplementeerd. In deze versie wordt de Munchkin Associate Lead-methode volledig afgekeurd. Dit is een functie waarmee persoonlijke gegevens kunnen worden verzonden naar een Marketo-abonnement en de bijbehorende webbrowsergeschiedenis met een bekende persoonrecord. De geassocieerde Lood wordt verwijderd ten gunste van modernere en veiligere alternatieven, zoals [ Forms JS API ](/help/javascript-api/forms-api-reference.md), de Vorm legt API en [ associate Lood REST API ](/help/rest-api/leads.md) voor. Als u of uw organisatie deze methode gebruikt, zou u vanaf gebruik tegen 12 oktober 2021 moeten migreren wanneer de publicatie van Oktober gepland is te beginnen. Als u niet meer in de bèta van Munchkin wenst te kiezen, kunt u gebruik op Marketo landende pagina&#39;s onbruikbaar maken door de &quot;Beta van Munchkin op het Bestaan van Pagina&#39;s&quot;eigenschap aan `disabled` in het [ menu van de Chest van de Schat ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/settings/enable-or-disable-treasure-chest-features) van een knevel te voorzien. Als u de Munchkin Beta JavaScript hebt geïmplementeerd op externe webpagina&#39;s en wilt overschakelen naar het standaard Munchkin releasekanaal, moet u het codefragment bijwerken om Munchkin JavaScript te laden van munchkin.js in plaats van munchkin-beta.js.

Gepast op _2021-08-24_ door _Kenny_

## TLS 1.0 en TLS 1.1 End of Life for Munchkin Service

Vanaf 21 oktober 2021, zal munchkin.marketo.net, die wordt gebruikt om Munchkin JavaScript aan bezoekers te dienen, geen verbindingen meer goedkeuren gebruikend [ TLS 1.0 of TLS 1.1.](https://en.wikipedia.org/wiki/Transport_Layer_Security) Deze encryptiestandaarden worden niet meer aanvaard als deel van de beste praktijken van de Webveiligheid en niet meer gesteund door moderne browser versies. Er zijn geen verwachte negatieve effecten als gevolg van deze wijziging.

Gepast op _2021-10-04_ door _Kenny_

## Updates voor oktober 2021

In oktober 2021 verbeteren we de bestaande REST API&#39;s en verhelpen we verschillende defecten. Zie de volledige lijst met updates hieronder.

* Wij hebben het [ voorlegt 1&rbrace; eindpunt van de Vorm &lbrace;verbeterd om de gebieden van de programmalid als deel van de vormvoorlegging te steunen. ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/SubmitFormUsingPOST) Naar keuze, kan een programma als programma worden gespecificeerd om vorm aan toe te voegen, en/of het programma om de douanegebieden van het programmalid aan toe te voegen zoals [ hier ](/help/rest-api/leads.md) wordt beschreven.
Wij hebben het [&#128279;](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getProgramMembersUsingGET) eindpunt van de Leden van het Programma verbeterd om gebaseerde vragen van de datumwaaier te steunen die op de updatedAt attributen worden gebaseerd. Dit wordt gedaan door het beginnen en het beëindigen van datetime parameters over te gaan zoals [ hier ](/help/rest-api/program-members.md) wordt beschreven.
* Wij hebben [ Leads Gebieden ](/help/rest-api/leads.md) APIs verbeterd om [ Gevoelige Gebieden ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/field-management/mark-a-field-as-sensitive) te steunen. [ krijgt het Gebied van de Lood door Naam ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldByNameUsingGET), [ krijgt LeidingsGebieden ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getLeadFieldsUsingGET), [ leidt tot Gebieden van de Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST), en [ 2&rbrace; eindpunten van het Gebied van de Lood van de Update ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/updateLeadFieldUsingPOST) steunt nu de isSensitive attributen.

### Resoluties beschermen

* Vaste kwestie met [ het Beheer van de Gebruiker ](/help/rest-api/user-management.md) API. Bevat aan de gebruikers van Marketo die voor gebruik met [ Insight van de Verkoop ](https://business.adobe.com/products/marketo/sales-insight.html) worden gevormd. Deze gebruikers zijn nu teruggekeerd door [ krijgen Gebruikers ](https://developer.adobe.com/marketo-apis/api/user/#operation/getUsersUsingGET) eindpunt, en deze gebruikers kunnen nu worden geschrapt gebruikend het [ punt van de Gebruiker van de Schrapping ](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST). [ LM-155864 ]
* Vaste kwestie met toevoegen [&#128279;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Form-Fields/addRichTextFieldUsingPOST) eindpunt van het Gebied van de Tekst 0&rbrace; Rich.  Wanneer u een tekstveld met tekstopmaak van meer dan 65 k tekens toevoegt aan een e-mail, openingspagina, fragment of formulier, retourneert deze de waarde &quot;611, Systeemfout&quot;. Het keert nu fout &quot;701 terug, de Verrichting kan niet worden voltooid. &#39;content&#39; overschrijdt een maximumlengte van 65.535 bytes.&#39;

Gepost op _2021-10-25_ door _David_

## Updates voor januari 2022

In januari 2022 verbeteren we de bestaande REST API&#39;s en verhelpen we verschillende defecten. Zie de volledige lijst met updates hieronder.

* Wij hebben het [ Bulk Uittreksel van de Objecten van de Douane ](/help/rest-api/bulk-custom-object-extract.md) API verbeterd om gebruikers toe te staan om het gebruiken van een **updateAt** datumwaaier te filtreren.
* Toegevoegde API&#39;s voor metagegevens van het programmalid waarmee u metagegevens voor de velden Program Member kunt maken, bijwerken en ophalen. Voor meer informatie zie [ Leden van het Programma > Gebieden ](/help/rest-api/program-members.md).
* Toegevoegde het gebied van het Bedrijf meta-gegevens APIs die u toestaan om meta-gegevens voor de gebieden van het Bedrijf terug te winnen. Voor meer informatie zie [ Bedrijven > Gebieden ](/help/rest-api/companies.md).
* API&#39;s voor metagegevens van opportuniteitsvelden toegevoegd waarmee u metagegevens voor opportuniteitsvelden kunt ophalen. Voor meer informatie zie [ Kansen > Gebieden ](/help/rest-api/opportunities.md).
* Toegevoegde API&#39;s voor metagegevens van het veld Benoemd account waarmee u metagegevens voor velden van benoemde account kunt ophalen. Voor meer informatie zie [ Benoemde Rekeningen > Gebieden ](/help/rest-api/named-accounts.md).
* De bijgewerkte eindpunten van gebiedsmeta-gegevens om een nieuw booleaans bezit **isApiCreated** terug te keren om erop te wijzen of een gebied al dan niet door REST API werd gecreeerd.

### Resoluties beschermen

* Vaste latentiekwestie tussen tijd van vraag aan [ leidt tot de Gebieden van de Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) eindpunt en tijd toen het nieuwe gecreeerde loodgebied in slimme lijst beschikbaar was. [ LM-152838 ]
* Vaste kwestie met [ creeerde Loodgebieden ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/createLeadFieldUsingPOST) eindpunt waar de gecreeerde gebieden niet beschikbaar waren in de drop-down lijst van vormgebieden die wordt gebruikt om [ gebieden aan vorm ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/forms/creating-a-form/add-a-field-to-a-form) in Marketo Engage UI toe te voegen. [ LM-158243 ]
* Vaste kwestie met [ krijgt Campagnes ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/getCampaignsUsingGET) eindpunt waar de trekkerbare campagnes niet waren teruggekeerd wanneer de parameter isTriggerable=true werd gespecificeerd. [ LM-158283 ]
* Vaste kwestie waar [ leidt door Identiteitskaart van de Lijst ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteTokenByNameUsingPOST) eindpunt een fout &quot;611, de fout van het Systeem&quot;in bepaalde gevallen zou terugkeren. [ LM-157214 ]
* Opschonk verscheidene foutenmeldingen die door het [ verbindingspunt van het Lood van de Update ](/help/rest-api/leads.md) worden teruggekeerd. [ LM-151886, LM-151888, LM-151889 ]

Gepost op _2022-01-27_ door _David_

## Updates van maart 2022

In maart 2022 verbeteren we de bestaande REST API&#39;s en verhelpen we verschillende defecten. Zie de volledige lijst met updates hieronder.

* Wij hebben het **actionResult** gebied aan het de uitvoerdossier toegevoegd dat door het Bulk Uittreksel API van de Activiteit wordt geproduceerd. Dit veld kan worden gebruikt om een onderscheid te maken tussen geslaagde, overgeslagen en mislukte activiteiten.
* Wij hebben het **isOpenTrackingDisabled** gebied aan reacties van [ Emails API ](/help/rest-api/emails.md) toegevoegd. Dit gebied kan worden gebruikt om te bepalen of [ Open het Volgen ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-editor-v2-0-overview) eigenschap onbruikbaar maakt wordt toegelaten.
* Er zijn twee eindpunten toegevoegd waarmee u programmatags selectief kunt beheren. Het [ eindpunt van de Markeringen van het Programma van de Update ](/help/rest-api/programs.md) staat u toe om een programmalag selectief bij te werken. Het [ eindpunt van de Markeringen van het Programma van de Schrapping ](/help/rest-api/programs.md) staat u toe om een programmalag selectief te schrappen.
* Wij hebben de **isExecutable** parameter aan het [ Slimme 3&rbrace; eindpunt van de Campagne van de Kloon toegevoegd. ](/help/rest-api/smart-campaigns.md) Met deze parameter kunt u een programma klonen als een uitvoerbaar programma.
* Wij hebben het **headStart** gebied aan [ Programma&#39;s API ](/help/rest-api/programs.md) toegevoegd. Dit staat u toe om te creëren, bij te werken en terug te winnen [ HoofdBegin ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/head-start-for-email-programs) het plaatsen voor E-mailProgramma&#39;s.

### Resoluties beschermen

* Vaste kwestie met [ krijgt E-mail Dynamische Inhoud ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) eindpunt. Bij het ophalen van onderwerpregel met dynamische inhoud uit e-mails met verbroken sjabloonrelaties is een fout geretourneerd (709). De API staat alleen bewerkingen toe op e-mails met een sjabloon. Het eindpunt keert nu de dynamische inhoud terug. [ LM-152331 ]
* De Vaste kwestie met [ Synchronisatie leidt ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/syncLeadUsingPOST) eindpunt. Wanneer het gebruiken van externalSalesPersonId om Verkoper met een lood te associëren gebruikend externalSalesPersonId en het gebruiken van actie = createDuplicate, dan zou de vereniging van de Verkoper niet voorkomen. [ LM-158990 ]


### Adobe IMS-integratie

* Diegenen die aan [ IMS van Adobe ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview) zijn bezet kunnen niet alle [ het Beheer APIs van de Gebruiker van Marketo gebruiken ](/help/rest-api/user-management.md). De volgende eindpunten zullen een fout op wanneer geroepen op de Instanties van Marketo die met Adobe IMS zijn geïntegreerd terugkeren: [ nodigt Gebruiker ](https://developer.adobe.com/marketo-apis/api/user/#operation/inviteUserUsingPOST) uit, [ krijgt Uitgenodigde Gebruiker door Identiteitskaart ](https://developer.adobe.com/marketo-apis/api/user/#operation/getInvitedUserUsingGET), [ de Attributen van de Gebruiker van de Update ](https://developer.adobe.com/marketo-apis/api/user/#operation/updateUserAttributeUsingPOST), [ Schrapping Gebruiker ](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteUserUsingPOST), en [ Schrapping Uitgenodigde Gebruiker ](https://developer.adobe.com/marketo-apis/api/user/#operation/deleteInvitedUserUsingPOST). Als vervanging, zou het [ Beheer APIs van de Gebruiker van Adobe ](https://developer.adobe.com/umapi/) moeten worden gebruikt.

Gepost op _2022-03-14_ door _David_

## Updates van mei 2022-

In mei 2022 verbeteren we de bestaande REST API&#39;s en verhelpen we verschillende defecten. Zie de volledige lijst met updates hieronder.

* Wij hebben de capaciteit toegevoegd om [ Bedrijf ](/help/rest-api/companies.md) terug te winnen, [ Kans ](/help/rest-api/opportunities.md), en [ Verkopers ](/help/rest-api/sales-persons.md) verslagen wanneer of [ de Synchronisatie van SFDC ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/crm-sync/salesforce-sync/sfdc-sync-details/sfdc-sync-field-sync) of [ Synchronisatie van Microsoft Dynamics ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/microsoft-dynamics-sync-details/microsoft-dynamics-sync-user-sync) in uw instantie van Marketo Engage worden toegelaten.
* Wij hebben [ bijgewerkt krijgen E-mail Dynamische Inhoud ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getEmailDynamicContentUsingGET) eindpunt om u toe te staan om [ Dynamische Inhoud ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/functions-in-the-editor/using-dynamic-content-in-an-email) van een e-mailonderwerpregel terug te winnen. Dit werkt ongeacht of de opgegeven e-mail is gekoppeld aan een e-mailsjabloon.

`POST /rest/asset/v1/form/{id}/field/State.json?values=[{"label":"Alaska"},{"value":"AK"},{"label":"West Virginia","value":"WV"},{"label":"Wyoming","value":"WY"}]`

* Wij hebben [ bijgewerkt voeg de Regels van de Zichtbaarheid van het Gebied van de Vorm ](https://developer.adobe.com/marketo-apis/api/asset/#operation/getAllProgramMemberFieldsUsingGET) toe eindpunt om u toe te staan om veelvoudige vergelijkingswaarden voor **toe te voegen isNot** type [ Regels van het Onzichtbaar maken ](/help/rest-api/forms.md). Hier volgt een voorbeeld:

`POST /rest/asset/v1/form/{id}/field/LastName/visibility.json?visibilityRule={"ruleType":"show","rules":[{"subjectField":"LastName","operator":"isNot","values":["A","B","C"]}`

### Resoluties beschermen

* Vaste kwestie met het [ legt Vorm ](/help/rest-api/leads.md) eindpunt voor dat voorkwam wanneer het overgaan &quot;ongeldig&quot;voor een attribuut in de [ leadFormFields ](/help/rest-api/leads.md) parameter, werd een fout teruggekeerd, &quot;611, de Fout van het Systeem&quot;. Er wordt nu correct de fout &quot;1003, Form validation failed&quot; geretourneerd. [ LM-162213 ]

Gepost op _2022-05-09_ door _David_

## Updates voor augustus 2022

In augustus 2022 verbeteren we de bestaande REST API&#39;s. Zie de volledige lijst met updates hieronder.

LWe hebben verscheidene nieuwe filters toegevoegd die kunnen worden gebruikt wanneer het roepen van Create het eindpunt van de Taak van het Lid van het Programma van de Uitvoer. Veel van de filters kunnen in combinatie met elkaar worden gebruikt om de geëxtraheerde gegevensset te verfijnen.

* Het **programIds** filter kan worden gebruikt om tot 10 programmaherkenningstekens te specificeren die productie kunnen helpen verbeteren.
* **isExhausted** filter kan aan filterverslagen voor [ mensen worden gebruikt die uitgeputte inhoud ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content) hebben.
* Het **filter 0&rbrace; nurtureCadence kan worden gebruikt om verslagen te filtreren die op [ worden gebaseerd het programmacadence van de Overeenkomst ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/program-flow-actions/change-engagement-program-cadence).**
* De **statusNames** filter kan aan filterverslagen voor één of meerdere [ programmastatussen ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/programs/creating-programs/understanding-program-membership) worden gebruikt.
* Het **updatedAt** filter kan aan filterverslagen worden gebruikt die op een datumwaaier worden gebaseerd.

### Aankondigingen

* Het gedrag van het [ eindpunt van de Identiteit ](https://developer.adobe.com/marketo-apis/api/identity/#operation/identityUsingGET) is veranderd. Wanneer u het eindpunt roept en geen **access_token** parameter omvat, &quot;603, Ontkende Toegang&quot;fout is teruggekeerd. Eerder werd de fout &quot;600, Empty access token&quot; geretourneerd. Merk op dat de fout &quot;600, Leeg toegangstoken&quot;is verouderd.

Gepost op _2022-09-03_ door _David_

## Updates voor oktober 2022

In oktober 2022 verbeteren we de bestaande REST API&#39;s. Zie de volledige lijst met updates hieronder.

* Wij hebben [ BulkLood de Invoer API ](/help/rest-api/bulk-lead-import.md) verbeterd om het toevoegen van Lood aan de verslagen van de Personen van de Verkoop tijdens het de invoerproces te steunen. Dit wordt gedaan door het **externalSalesPersonId** gebied in het de invoerdossier te omvatten.
* Vaste kwestie met [ creeer Loodgebieden ](/help/rest-api/leads.md) eindpunt dat voorkwam toen het creëren van het type van Score gebieden. Deze gebieden waren niet beschikbaar voor gebruik in [ de stroomactie van de Score van de Verandering ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/core-marketo-concepts/smart-campaigns/flow-actions/change-score) in Marketo Engage UI. [ LM-166815 ]

### Aankondigingen

* Het kenmerk Program Membership `acquiredBy` is gewijzigd en kan worden bijgewerkt.

Gepost op _2022-10-18_ door _David_

## Aanstaande wijziging in Marketo Forms REST AP

Vanaf de release van 2022.R2, die momenteel gepland is voor de week van 24 maart 2023, zullen de Adobe Marketo Engage Forms Asset API&#39;s consequent alleen de naam van het formulier retourneren zonder een vooraf vastgestelde naam, ongeacht of het formulier een onderliggend formulier van een programma is of niet. Door deze wijziging wordt het gedrag van de Forms API met betrekking tot namen van elementen consistent met de rest van de Adobe Marketo Engage Asset API&#39;s. Om onderbreking van de dienst te vermijden, zou u om het even welke integraties moeten herzien die Marketo Engage Forms APIs gebruiken en met uw integrators werken om te zien of om het even welke veranderingen nodig zijn om deze aan te passen Voorafgaand aan deze aanstaande verandering, de namen die door Forms APIs worden teruggegeven een programmanaam voor vormen inconsistent prefixen die kinderen van programma&#39;s in de Afzetactiviteiten zijn. Een formulier met de naam &quot;Form 1&quot;, dat een onderliggend formulier was van &quot;Program 1&quot;, kan bijvoorbeeld de naam van het formulier krijgen dat door de API wordt geretourneerd als: Program 1.Form 1 or Form 1 Vanaf de release 2022.R2 wordt de naam van een formulier altijd geretourneerd zonder een vooraf ingestelde programmanaam. In hetzelfde voorbeeld is de naam altijd: Formulier 1

Gepast op _2022-11-04_ door _Kenny_

## Updates voor januari 2023

In januari 2023 hebben we een API-gerelateerde verbetering aangebracht in de interface van Admin en maken we twee aankondigingen. Zie de volledige lijst met updates hieronder.

Gebruikersinterface van beheerder

### Extraheren voor bulklood

* We hebben de gebruikersinterface van Marketo Engage Admin verbeterd zodat u de dagelijkse capaciteitstoewijzing van de API voor bulkextractie voor uw abonnement kunt bekijken. Bovendien kunt u het capaciteitsgebruik door API-Gebruiker in de afgelopen 7 dagen bekijken. Meer informatie kan [ hier ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/settings/bulk-export-api-information) worden gevonden.

### Resoluties beschermen

* Vaste kwestie met [ schrapte Kansen ](https://developer.adobe.com/marketo-apis/api/mapi/#operation/deleteOpportunitiesUsingPOST) eindpunt. In bepaalde gevallen werd de activiteit &quot;Verwijderen uit opportuniteit&quot; niet gegenereerd. [ LM-172208 ]

### Aankondigingen

* Gelieve te zien [ dit artikel ](https://nation.marketo.com/t5/product-documents/upcoming-change-to-marketo-rest-api/ta-p/331698) op de Gemeenschap van Marketo betreffende REST API en een verandering in de 2&rbrace; redenuitdrukking van het de reactiebericht van HTTP [&#128279;](https://www.rfc-editor.org/rfc/rfc7230#section-3.1.2).
* Het attribuut van het Lidmaatschap van het Programma **statusReason** is veranderd om updateable te zijn.

Gepost op _2023-01-21_ door _David_
