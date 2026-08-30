# 🗑️ Afvalmelding blueprint voor Home Assistant

Deze afvalherinnering met interactieve meldingen stuurt 's avonds een melding als er de volgende dag afval opgehaald wordt. Later op de avond krijg je nog een herinneringsvraag met Ja/Nee-knoppen of de bak al aan de weg staat. Antwoord je niet, of nog niet, dan volgt er één laatste herinnering.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FRuud-St%2Fha-afvalmelding-blueprint%2Fblob%2Fmain%2Fafvalmelding.yaml)

## Hoe het werkt

| Wanneer | Melding |
|---|---|
| 19:00 (instelbaar) | 🗑️ Afvalmelding — *Morgen wordt het gft-afval opgehaald. Vergeet niet om de bak aan de weg te zetten.* |
| 22:00 (instelbaar) | 🗑️ Afvalmelding herinnering — *Staat de gft-bak al aan de weg?* met de knoppen **Ja** en **Nog niet** |
| na X minuten | 🗑️ Afvalmelding laatste herinnering! — *Dit is je laatste herinnering: vergeet de gft-bak niet!* |

De laatste herinnering komt alleen als je op **Nog niet** drukt of helemaal niet reageert. Druk je op **Ja**, dan wordt de melding van je telefoon gehaald en gebeurt er verder niets meer.

Teksten passen zich aan het aantal bakken aan:

- één bak: *Morgen wordt het gft-afval opgehaald. Vergeet niet om de bak aan de weg te zetten.*
- meerdere: *Morgen worden het restafval en het oud papier opgehaald. Vergeet niet om de bakken aan de weg te zetten.*
- kerstboom: *Morgen wordt de kerstboom opgehaald. Vergeet niet om deze aan de weg te zetten.*

## Vereisten

- Home Assistant 2024.10 of nieuwer
- De Home Assistant Companion-app op de telefoons die de melding moeten ontvangen
- Afvalsensoren met het attribuut `days_until_collection_date`

## Instellen

| Instelling | Toelichting |
|---|---|
| Sensor restafval | Optioneel |
| Sensor plastic (PBD) | Optioneel |
| Sensor GFT | Optioneel |
| Sensor oud papier | Optioneel |
| Sensor kerstboom | Optioneel |
| Telefoons / tablets | Eén of meer apparaten met de Companion-app |
| Tijd eerste melding | Standaard 19:00 |
| Tijd vraag met knoppen | Standaard 22:00 |
| Minuten tot herinnering | Standaard 15 |

Alle afvalsoorten zijn optioneel: vul alleen in wat er in jouw gemeente wordt opgehaald.

## Testen

Vuur het event `afvalmelding_test` af via **Instellingen → Tools → Gebeurtenissen**. Je krijgt dan beide meldingen achter elkaar, met 15 seconden ertussen.

In Home Assistant ouder dan 2026.8 heet dit **Ontwikkelhulpmiddelen** in plaats van Tools.

Wil je een knop op je dashboard, maak dan een script aan:

```yaml
script:
  test_afvalmelding:
    alias: Test afvalmelding
    sequence:
      - event: afvalmelding_test
```

## Blueprint Updaten

Home Assistant werkt blueprints niet automatisch bij. Nieuwe versies kondig ik aan bij [Releases](https://github.com/Ruud-St/ha-afvalmelding-blueprint/releases). Bijwerken doe je zo:

1. Ga naar **Instellingen → Automatiseringen & scènes → Blueprints**
2. Zoek `Ruud-St/afvalmelding.yaml` in de lijst
3. Klik op de drie puntjes rechts en kies **Blueprint opnieuw laden**

Je bestaande automations blijven bestaan en houden hun instellingen. Nieuwe instellingen komen er leeg bij.

## Goed om te weten

- **Waarom `notify.mobile_app_*` en niet `notify.send_message`?** Die laatste ondersteunt geen actieknoppen, dus meldingen met Ja/Nee moeten via de klassieke notify-service.
- **Apple Watch** werkt zonder extra instellingen: iOS spiegelt de melding inclusief knoppen. Selecteer gewoon de iPhone.
- **Fitbit** kan de melding wel tonen, maar de knoppen niet gebruiken.
- Alle meldingen delen de tag `afvalmelding`, zodat een nieuwe melding de vorige vervangt in plaats van ernaast te komen staan.

## Versies

**v5.2** — Alle afvalsoorten zijn vanaf nu optioneel en er is een oudpapierbak toegevoegd.

**v5.1** — Nieuwe meldingsteksten met correcte enkelvoud/meervoud, titels gelijkgetrokken, pauze tussen testmeldingen.

**v5** — "Nog niet"-knop werkt nu daadwerkelijk, unieke action-ID's per run, device-selector in plaats van notify-entiteiten, moderne triggersyntax, ongebruikte helpers verwijderd.
