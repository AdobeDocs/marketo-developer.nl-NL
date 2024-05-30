---
title: "Forms API Reference"
description: "Forms API Reference"
feature: Forms, Javascript
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '1327'
ht-degree: 0%

---


# Forms API-naslag

Er zijn twee hoofdobjecten waarmee u werkt met de Forms 2.0-API. De `MktoForms2` en de `Form` object. De `MktoForms2` -object is de naamruimte op hoofdniveau die zichtbaar is voor Forms2-functionaliteit en bevat functies voor het maken, laden en ophalen van formulierobjecten.

## Methoden van MktoForms2

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Methode</strong></td>
      <td><strong>Beschrijving</strong></td>
      <td><strong>Parameters</strong></td>
      <td><strong>Retourneert</strong></td>
    </tr>
    <tr valign="top">
      <td>.loadForm(baseUrl, munchkinId, formId, callback)</td>
      <td>Hiermee wordt een formulierdescriptor geladen vanaf Marketo-servers en wordt een nieuw object Form gemaakt.</td>
      <td> baseUrl(String) - URL naar de Marketo-serverinstantie voor uw abonnement</td>
      <td>ongedefinieerd</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>munchkinId (String) -Munchkin-id van het abonnement</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>formId (String of Number) - De Vid (form version id) van het formulier dat moet worden geladen</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (optioneel) (Functie) - Een callback-functie waarmee het geconstrueerde object Form wordt doorgegeven nadat het is geladen en geïnitialiseerd.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.lightbox(form, opts)</td>
      <td>Hiermee wordt een modaal dialoogvenster met de stijl van de lichtbak gemaakt waarin het formulierobject zich bevindt.</td>
      <td>form (Form Object) - Een instantie van een object Form dat u in een lichtbak wilt weergeven.</td>
      <td>Een lichtbakobject met de methoden .show() en .hide().</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>opts (optioneel)(Object) - Een object met opties die worden doorgegeven aan het lichtbakobject</td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>onSuccess(Functie) - Een callback die wordt teweeggebracht wanneer de vorm wordt voorgelegd.</td>
      <td></td>
    </tr>
    <tr>
          <td></td>
      <td></td>
      <td>closeBtn (Boolean) standaard waar - Controles als een dichte knoop (X) op de lichtbakdialoog wordt getoond.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.newForm(formData, callback)</td>
      <td>Maakt een nieuw object Form van een JS-object Form Descriptor. Voegt een callback functie toe die wordt geroepen zodra alle stylesheets en bekende loodinformatie is opgehaald en het voorwerp van de Vorm is gecreeerd.</td>
      <td>formData (Object Form Descriptor) - Een formulierbeschrijvingsobject, zoals gemaakt door de Marketo Forms V2 Editor</td>
      <td>ongedefinieerd</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>callback (optional)(Function) - Deze callback wordt aangeroepen met één argument, een nieuw gemaakt exemplaar van het object Form.</td>
      <td></td>
    </tr>
    <tr valign="top">
      <td>.getForm(formId)</td>
      <td>Hiermee wordt een eerder gemaakt formulierobject opgehaald op basis van een formulierid</td>
      <td> formId (Number of String) - Vid-id van formulier.</td>
      <td>Formulierobject</td>
    </tr>
    <tr valign="top">
      <td>.allForms()</td>
      <td>Hiermee wordt een array opgehaald van alle formulierobjecten die eerder op de pagina zijn gemaakt.</td>
      <td>nvt</td>
      <td>Array van formulierobject</td>
    </tr>
    <tr valign="top">
      <td>.getPageFields()</td>
      <td>Haalt een JS-object op dat gegevens van de URL en de verwijzende persoon bevat die interessant kunnen zijn voor traceringsdoeleinden.</td>
      <td>nvt</td>
      <td>Object</td>
    </tr>
    <tr valign="top">
      <td>.whenReady(callback)</td>
      <td>Hiermee voegt u een callback toe die precies één keer wordt aangeroepen voor elk formulier op de pagina dat "gereed" wordt. Gereedheid betekent dat het formulier bestaat, in eerste instantie is gegenereerd en de initiële callbacks zijn opgeroepen. Als er al een formulier is dat gereed is op het moment dat deze functie wordt aangeroepen, wordt de doorgegeven callback onmiddellijk aangeroepen.</td>
      <td>callback(Function) - De callback wordt doorgegeven aan één argument, een formulierobject.</td>
      <td>Object MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.onFormRender(callback)</td>
      <td>Hiermee voegt u een callback toe die wordt aangeroepen wanneer een formulier op de pagina wordt weergegeven. Forms wordt weergegeven wanneer het wordt gemaakt en vervolgens telkens wanneer de zichtbaarheidsregels de structuur van het formulier wijzigen.</td>
      <td>callback (Function) - De callback wordt doorgegeven aan één argument, het formulierobject van het formulier dat is gegenereerd.</td>
      <td>Object MktoForms2</td>
    </tr>
    <tr valign="top">
      <td>.whenRendered(callback)</td>
      <td>Net als bij onFormRender voegt dit een callback toe die wordt aangeroepen telkens wanneer een formulier wordt gegenereerd. Daarnaast wordt de callback onmiddellijk opgeroepen voor alle formulieren die al zijn gegenereerd.</td>
      <td>callback(Function) - Aan de callback wordt één argument doorgegeven, het formulierobject van het gegenereerde formulier.</td>
      <td></td>
    </tr>
</table>


## Formuliermethoden

<table>
  <tbody>
    <tr valign="top">
      <td><strong>Methode</strong></td>
      <td><strong>Beschrijving</strong></td>
      <td><strong>Parameters</strong></td>
      <td><strong>Retourneert</strong></td>
    </tr>
    <tr valign="top">
      <td>.render(formElem)</td>
      <td>Hiermee wordt een formulierobject gegenereerd, waarbij een jQuery-object wordt geretourneerd waarin een formulierelement met het formulier wordt ondergebracht. Als een formElem wordt doorgegeven, wordt dit gebruikt als het formulierelement. Als dit niet het geval is, wordt er een nieuw formulier gemaakt.</td>
      <td>formElem (optioneel) - Een formulierelement met een jQuery-object waarin moet worden gerenderd.</td>
      <td> Een formulierelement met een jQuery-object dat het weergegeven formulier bevat.</td>
    </tr>
    <tr valign="top">
      <td>.getId()</td>
      <td>Hiermee wordt de id van het formulier opgehaald.</td>
      <td>nvt</td>
      <td>Number - De id van het formulierobject dat dit formulier vertegenwoordigt</td>
    </tr>
    <tr valign="top">
      <td>.getFormElem()</td>
      <td>Haalt het formulierelement met jQuery-inhoud van een gegenereerd formulier op.</td>
      <td>nvt</td>
      <td>A jQuery object-wrapped form element or null if the form has not been rendered with the render() method yet.</td>
    </tr>
    <tr valign="top">
      <td>.validate()</td>
      <td>Forces the form to validate, highlight any errors that may exist and return the result. Hiermee verzendt u het formulier niet.</td>
      <td>nvt</td>
      <td>Boolean - Retourneert true als alle validators op het formulier zijn geslaagd, anders false.</td>
    </tr>
    <tr valign="top">
      <td>.onValidate(callback)</td>
      <td>Voegt een validatiecallback toe die op elk moment dat de validatie wordt geactiveerd, wordt aangeroepen.</td>
      <td>callback(Function) - Een callback die zal worden teweeggebracht wanneer de bevestiging voorkomt. Callback zal één parameter, een booleaanse verklaring worden overgegaan als de bevestiging was geslaagd.</td>
      <td>Formulierobject: hetzelfde formulierobject waarvoor de methode is aangeroepen, voor kettingdoeleinden.</td>
    </tr>
    <tr valign="top">
      <td>.submit()</td>
      <td>De verzendgebeurtenis van het formulier wordt geactiveerd. Hiermee start u het formulier via de verzendstroom, waarbij u validatie uitvoert, gebeurtenissen onSubmit activeert, het formulier verzendt en eventuele gebeurtenissen onSuccess afvaagt als het verzenden van het formulier is gelukt.</td>
      <td>nvt</td>
      <td>Formulierobject: hetzelfde formulierobject waarvoor de methode is aangeroepen, voor kettingdoeleinden.</td>
    </tr>
    <tr valign="top">
      <td>.onSubmit(callback)</td>
      <td>Hiermee voegt u een callback toe die wordt aangeroepen wanneer het formulier wordt verzonden. Dit wordt in werking gesteld wanneer de voorlegging begint, alvorens het succes/het mislukken van het verzoek wordt gekend.</td>
      <td>callback - Een functie die wordt aangeroepen wanneer het formulier wordt verzonden. Deze callback wordt doorgegeven aan één argument, dit object Form.</td>
      <td>Formulierobject: hetzelfde formulierobject waarvoor de methode is aangeroepen, voor kettingdoeleinden.</td>
    </tr>
    <tr valign="top">
      <td>.onSuccess(callback)</td>
      <td>Hiermee voegt u een callback toe die wordt aangeroepen wanneer het formulier is verzonden maar voordat de lead naar de vervolgpagina is doorgestuurd. Kan worden gebruikt om te voorkomen dat de lead na een geslaagde verzending naar de vervolgpagina wordt doorgestuurd.</td>
      <td>callback - Een functie die wordt aangeroepen wanneer het formulier is verzonden. Deze callback zal twee argumenten worden overgegaan. Een voorwerp JS die de waarden bevatten die werden voorgelegd en een URL van het Koord van de opvolgingspagina die de gebruiker aan, of ongeldig of leeg koord zal door:sturen als er geen gevormde opvolgingspagina is. Speciaal gedrag: Als deze callback "false"terugkeert (gemeten gebruikend ===) dan zal de bezoeker NIET aan de opvolgingspagina door:sturen en de pagina zal NIET worden opnieuw geladen. Hierdoor kan de implementator extra verwerkingen uitvoeren naar de follow-up-URL of actie ondernemen op de pagina met JavaScript in plaats van de pagina te verlaten.</td>
      <td>Formulierobject: hetzelfde formulierobject waarvoor de methode is aangeroepen, voor kettingdoeleinden.</td>
    </tr>
    <tr valign="top">
      <td>.submitTable(canSubmit) <em>ook beschikbaar als:</em> <em>.submitable(canSubmit)</em></td>
      <td>Hiermee wordt opgehaald of ingesteld of het formulier kan worden verzonden. Als deze functie zonder argumenten wordt aangeroepen, krijgt deze de waarde als deze met één argument wordt aangeroepen. Hiermee wordt voorkomen dat een formulier wordt verzonden, terwijl aan andere criteria buiten de normale vorm moet worden voldaan.</td>
      <td>canSubmit (optioneel)(Boolean) - Hiermee wordt ingesteld dat het formulier moet worden verzonden of niet kan worden verzonden.</td>
      <td>Booleaanse waarde of formulierobject - Bij aanroep zonder argumenten wordt een Booleaanse waarde geretourneerd die aangeeft of het formulier kan worden verzonden. Als deze optie met één argument wordt aangeroepen, wordt dit formulierobject geretourneerd voor kettingdoeleinden. </td>
    </tr>
    <tr valign="top">
      <td>.allFieldsFilled()</td>
      <td>Retourneert true als voor alle velden in het formulier waarden zijn ingesteld die niet leeg zijn.</td>
      <td>nvt</td>
      <td>Boolean - True als alle velden waarden hebben die niet leeg, leeg, niet ingesteld of null zijn, anders false.</td>
    </tr>
    <tr valign="top">
      <td>.setValues(vals)</td>
      <td>Hiermee stelt u waarden in voor een of meer velden in het formulier.</td>
      <td>vals - Een JS-object. Voor elk sleutel-/waardepaar in het object wordt het formulierveld met de naam key ingesteld op waarde.</td>
      <td>ongedefinieerd</td>
    </tr>
    <tr valign="top">
      <td>.getValues()</td>
      <td>Hiermee worden alle waarden van alle velden in het formulier opgehaald.</td>
      <td>nvt</td>
      <td>Object - Een JS-object dat sleutel/waardeparen bevat die de namen en waarden van de velden in het formulier vertegenwoordigen.</td>
    </tr>
    <tr valign="top">
      <td>.addHiddenFields(values)</td>
      <td>Hiermee voegt u invoerveld type=hidden toe aan het formulier.</td>
      <td>waarden - Een JS-object dat sleutel/waardeparen bevat die de namen en waarden vertegenwoordigen van de verborgen velden die aan het formulier moeten worden toegevoegd.</td>
      <td>ongedefinieerd</td>
    </tr>
    <tr valign="top">
      <td>.vals(waarden)</td>
      <td>jQuery-stijl .vals() setter/getter. Indien aangeroepen zonder argumenten, is dit gelijk aan het aanroepen van getValues(). Indien aangeroepen met één argument, is gelijk aan het aanroepen van setValues()</td>
      <td>waarden (optioneel) - Object</td>
      <td>ongedefinieerd</td>
    </tr>
    <tr valign="top">
      <td>.showErrorMessage(msg, elem)</td>
      <td>Toont een foutenmelding, richtend bij elem.</td>
      <td>msg (Koord van HTML) - een koord die de tekst van de fout bevatten u wilt tonen.</td>
            <td>Formulierobject - Dit formulierobject, voor ketting.</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td>elem (optioneel)(jQuery-object) - Het element waarnaar de fout moet verwijzen. Als de waarde niet is ingesteld, wordt de verzendknop van het formulier gebruikt.</td>
<td></td>
    </tr>
  </tbody>
</table>
