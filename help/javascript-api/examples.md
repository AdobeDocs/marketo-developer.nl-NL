---
title: Voorbeelden
description: Marketo Forms 2.0 JavaScript-voorbeelden die u kunt verbergen of omleiden bij het verzenden, instellen en lezen van velden, valideren met aangepaste fouten, lichtbak en externe triggers.
feature: Javascript
exl-id: dc5f0cc5-ff5a-48b0-be36-52c10e56f798
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---

# Voorbeelden

Hieronder vindt u een aantal voorbeelden van demonstratieve Forms 2.0-webformulieren.

## Formulier verbergen na verzending

In dit voorbeeld gaat de bezoeker niet naar de vervolgpagina of wordt de huidige pagina opnieuw geladen.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Get the form's jQuery element and hide it
        form.getFormElem().hide();
        // Return false to prevent the submission handler from taking the lead to the follow up url
        return false;
    });
});
```

## Bezoeker doorsturen naar door gebruiker gedefinieerde URL

In dit voorbeeld gaat de bezoeker naar een URL die door JavaScript wordt bepaald na een geslaagde verzending in plaats van naar de geconfigureerde pagina voor bedankt.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    //Add an onSuccess handler
    form.onSuccess(function(values, followUpUrl) {
        // Take the lead to a different page on successful submit, ignoring the form's configured followUpUrl
        location.href = "https://google.com/?q=marketo+forms+v2+examples";
        // Return false to prevent the submission handler continuing with its own processing
        return false;
    });
});
```

## Waarden voor formuliervelden instellen

In dit voorbeeld worden formuliervelden ingesteld.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Set the value of the Phone and Country fields
    form.vals({ "Phone":"555-555-1234", "Country":"USA"});
});
```

## Formulierveldwaarden lezen op Formulier verzenden

In dit voorbeeld worden formuliervelden gelezen bij het verzenden van een formulier.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function(form) {
    // Add an onSubmit handler
    form.onSubmit(function(){
        // Get the form field values
        var vals = form.vals();
        // You may wish to call other function calls here, for example to fire google analytics tracking or the like
        // callSomeFunction(vals);
        // We'll just alert them to show the principle
        alert("Submitted values: " + JSON.stringify(vals));
    });
});
```

## Formulier verzenden bij klikgebeurtenis anders dan formulier

In dit voorbeeld wordt een formulier verzonden dat is gebaseerd op een klikgebeurtenis op een ander element of een gebeurtenis die geen deel uitmaakt van het formulier.

```javascript
// Load the form normally
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057);

// Find the button element that you want to attach the event to
var btn = document.getElementById("MyAlternativeSubmitButtonId");
btn.onclick = function() {
    // When the button is clicked, get the form object and submit it
    MktoForms2.whenReady(function (form) {
        form.submit();
    });
};
```

## Voorkomen dat een gebruiker een formulier verzendt

In dit voorbeeld moet u ten minste drie keer klikken op de tellerknop voordat de verzendknop op het formulier werkt.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Check if the form is submittable
    if (form.submittable()) {
        // Set it to be non submittable
        form.submittable(false);
    }
});

var clickCount = 0;
// Wire up the click counter button
var clickCounterBtn = document.getElementById("ClickCounter");
clickCounterBtn.onclick = function() {
    clickCount++;
    // Update the buttons's text
    clickCounterBtn.innerHTML = "Click Counter Button ("+clickCount+")";

    if (clickCount >= 3) {
        // Get the form and set it to be submittable
        MktoForms2.whenReady(function (form) {
            form.submittable(true);
        });
    }
};
```

## Waarden instellen voor verborgen velden op het formulier

In dit voorbeeld worden waarden voor verborgen velden ingesteld.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    // Set values for the hidden fields, "userIsAwesome" and "enrollDate"
    // Note that these fields were configured in the form editor as hidden fields already
    form.vals({"userIsAwesome":"true", "enrollDate":"2014-01-01"});
});
```

## Formulier tonen in LightBox

In dit voorbeeld wordt het formulier weergegeven in een dialoogvenster met een lichtbakstijl als de URL een parameter `lightboxForm=true` bevat.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    if (location.href.indexOf("lightboxForm=true") != -1) {
        MktoForms2.lightbox(form).show();
    }
});
```

## Aangepast foutbericht weergeven

In dit voorbeeld wordt een aangepast foutbericht weergegeven bij verzenden op basis van aangepaste bedrijfslogica.

```javascript
MktoForms2.loadForm("//app-ab00.marketo.com", "785-UHP-775", 1057, function (form) {
    //listen for the validate event
    form.onValidate(function() {
        // Get the values
        var vals = form.vals();
        //Check your condition
        if (vals.Country == "USA" && vals.vehicleSize != "Massive") {
            // Prevent form submission
            form.submittable(false);
            // Show error message, pointed at VehicleSize element
            var vehicleSizeElem = form.getFormElem().find("#vehicleSize");
            form.showErrorMessage("All Americans must have a massive vehicle", vehicleSizeElem);
        }
        else {
            // Enable submission for those who met the criteria
            form.submittable(true);
        }
  });
});
```
