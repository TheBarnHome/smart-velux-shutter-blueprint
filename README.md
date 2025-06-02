# HA Smart Velux Shutter

Blueprint Home Assistant pour la gestion intelligente d’une fenêtre Velux exposée plein sud et de son volet roulant.  
Optimise naturellement la température intérieure en combinant température intérieure/extérieure, thermostat, heure de la journée, et conditions météo via l’intégration **Météo-France**.

---

## 🧠 Fonctionnalités

- ✅ Ouvre la fenêtre Velux si l’air extérieur est plus frais que l’intérieur.
- ✅ Ferme la fenêtre si la température est confortable.
- ✅ Ferme automatiquement le volet la nuit.
- ✅ Ferme le volet si ensoleillement fort et chaleur excessive.
- ✅ Prend en compte l’état du Velux pour fermer correctement le volet (séquence sécurisée).
- ✅ Compatible avec l’intégration [Météo-France](https://www.home-assistant.io/integrations/meteo_france/).
- ✅ Entièrement paramétrable via l’interface graphique de Home Assistant (Blueprint).

---

## 📦 Installation

1. Télécharge ou copie le fichier [`velux_shutter_blueprint.yaml`](velux_shutter_blueprint.yaml) dans :
