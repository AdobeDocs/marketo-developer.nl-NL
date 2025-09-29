---
title: Geavanceerde toegangsmodus voor beveiliging
feature: Mobile Marketing
description: Leer de Geavanceerde Wijze van de Toegang van de Veiligheid voor Marketo Mobile SDK, met HMAC handtekeninggeneratie, server eindpuntopstelling, het gebruik van apparaatidentiteitskaart en de voorbeelden van iOS en Android
exl-id: bd4730ff-708b-465e-b494-485a4dbf67ff
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---

# Geavanceerde toegangsmodus voor beveiliging

De Marketo SDK stelt methoden beschikbaar om de beveiligingshandtekening in te stellen en te verwijderen. Er is ook een hulpprogrammamethode om apparaat-id op te halen. De apparaat-id moet samen met de e-mail bij aanmelding worden doorgegeven aan de klantenserver voor gebruik bij het berekenen van de beveiligingshandtekening. De SDK moet het nieuwe eindpunt kiezen, waarbij naar het hierboven vermelde algoritme wordt verwezen, om de velden op te halen die nodig zijn om het handtekeningobject te instantiëren. Het instellen van deze handtekening in de SDK is een noodzakelijke stap als de beveiligingstoegangsmodus is ingeschakeld in Marketo Mobile Admin.

## Beveiligde toegangsmodus instellen

Deze instelling moet worden geïmplementeerd voordat de modus voor beveiligde toegang is ingeschakeld via Marketo Admin > de pagina Mobiele toepassingen en apparaten. De volgende verdere stappen beschrijven het proces dat wordt vereist om het proces van de veiligheidsbevestiging te voltooien:

De veilige wijze van de Toegang vereist het uitvoeren van het handtekeningalgoritme op de klant server-kant die een eindpunt zal verstrekken om de toegangssleutel, berekende handtekening, verlooptimestamp, en e-mail terug te winnen. Dit algoritme vereist de toegangssleutel van de gebruiker, toegangsgeheim, e-mail, timestamp, en apparaat identiteitskaart om de berekening uit te voeren. De klant is verantwoordelijk voor het instellen van een eindpunt, het implementeren van het algoritme voor het uitvoeren van handtekeningberekeningen en het vernieuwen van de tijdstempel.

```python
import argparse
import datetime
import hashlib
import hmac


ACCESS_KEY = 'Your Access Key'
ACCESS_SECRET = 'Your access secret'

# Key should not be unicode
def get_signing_key(timestamp):
    return 'MKTO' + ACCESS_SECRET + str(timestamp)

def get_string_to_sign(email, uuid):
    return email + uuid

def get_hmac(key, string_to_sign):
    return hmac.new(key, string_to_sign.encode('utf-8'), hashlib.sha256).hexdigest()

def get_epoch_plus_day():
    epoch = datetime.datetime.utcfromtimestamp(0)
    valid_until_dt = datetime.datetime.utcnow() + datetime.timedelta(days=1)
    return long((valid_until_dt - epoch).total_seconds())

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument("-e", "--email", required=True, help="email address")
    parser.add_argument("-u", "--uuid", required=True, help="Device install id")
    parser.add_argument("-t", "--timestamp", type=int, help="Valid until timestamp")
    args = parser.parse_args()
    string_to_sign = get_string_to_sign(args.email, args.uuid)
    if not args.timestamp:
        valid_until = get_epoch_plus_day()
    else:
        valid_until = args.timestamp
    signing_key = get_signing_key(valid_until)
    hmac_string = get_hmac(signing_key, string_to_sign)
    print 'HMAC is ', hmac_string
```

De Marketo SDK stelt nieuwe methoden beschikbaar om de beveiligingshandtekening in te stellen en te verwijderen. Er is ook een hulpprogrammamethode om apparaat-id op te halen. De apparaat-id moet samen met de e-mail bij aanmelding worden doorgegeven aan de klantenserver voor gebruik bij het berekenen van de beveiligingshandtekening. De SDK moet het nieuwe eindpunt kiezen, waarbij naar het hierboven vermelde algoritme wordt verwezen, om de velden op te halen die nodig zijn om het handtekeningobject te instantiëren. Het instellen van deze handtekening in de SDK is een noodzakelijke stap als de beveiligingstoegangsmodus is ingeschakeld in Marketo Mobile Admin.

### iOS

```
Marketo * sharedInstance =[Marketo sharedInstance];

// set secure signature
MKTSecuritySignature *signature =
[[MKTSecuritySignature alloc] initWithAccessKey:<ACCESS_KEY> signature:<SIGNATURE_TOKEN> timestamp:<EXPIRY_TIMESTAMP> email:<EMAIL>];
[sharedInstance setSecureSignature:signature];

// remove signature
[sharedInstance removeSecureSignature];

// get device id
[sharedInstance getDeviceId];
```

```
let sharedInstance = Marketo.sharedInstance()

 // set secure signature
let signature = MKTSecuritySignature(accessKey: <ACCESS_KEY>, signature: <SIGNATURE_TOKEN> , timestamp: <EXPIRY_TIMESTAMP>, email: <EMAIL>)
sharedInstance.setSecureSignature(signature)

// remove signature
[sharedInstance removeSecureSignature];

// get device id
sharedInstance.getDeviceId()
```

### Android

```
Marketo sdk = Marketo.getInstance(getApplicationContext());

// set signature
MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
secureMode.setAccessKey(<ACCESS_KEY>);
secureMode.setEmail(<EMAIL_ADDRESS>);
secureMode.setSignature(<SIGNATURE_TOKEN>);
secureMode.setTimestamp(<EXPIRY_DATE>);
if (secureMode.isValid()) {
  sdk.setSecureSignature(secureMode);
}

// remove signature
sdk.removeSecureSignature();

// get device id
sdk.getDeviceId();
```
