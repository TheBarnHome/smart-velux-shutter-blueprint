# Gestion intelligente Velux et Volet

Ce blueprint Home Assistant permet de gérer automatiquement l'ouverture et la fermeture d'une fenêtre Velux et de son volet roulant selon la température intérieure, une vraie température extérieure mesurée, la tendance de la température intérieure, l'heure et la consigne du thermostat.

## Fonctionnalités

- **Ouverture progressive** de la fenêtre Velux si la pièce est trop chaude et que l'extérieur est réellement plus frais.
- **Fermeture progressive** de la fenêtre si la pièce est assez fraîche ou si l'extérieur devient plus chaud que l'intérieur.
- **Fermeture automatique** du volet la nuit ou lorsque la pièce chauffe sans possibilité de ventilation efficace.
- **Correction par petits pas** pour éviter les cycles complet ouvert/fermé liés à l'inertie thermique.
- Déclenchement toutes les 10 minutes, aux changements des capteurs principaux et aux heures de début/fin de nuit.

## Entrées à renseigner

- `thermostat` : L’entité `climate` représentant le thermostat de la pièce (pour récupérer température actuelle et consigne).
- `outdoor_temp_sensor` : l’entité `sensor` de température extérieure réelle.
- `velux` : L’entité `cover` correspondant à la fenêtre Velux.
- `volet` : L’entité `cover` correspondant au volet roulant.
- `night_start` : l’heure à partir de laquelle le volet doit se fermer.
- `night_end` : l’heure à laquelle la logique de nuit s'arrête.
- `temp_rising` : Entité binaire/sensor indiquant si la température intérieure est en train de monter (`on` ou `off`).
- `delta_temperature` : marge autour de la consigne pour éviter les ouvertures/fermetures trop fréquentes.
- `outdoor_cooling_delta` : écart minimum entre intérieur et extérieur avant d'ouvrir la fenêtre pour rafraîchir.
- `window_position_step` : pourcentage d'ouverture/fermeture appliqué à chaque correction de la fenêtre.

## Déclencheurs

- Toutes les 10 minutes
- À chaque changement stable pendant 5 minutes du thermostat, du capteur extérieur ou du booléen de température intérieure en hausse
- Aux heures `night_start` et `night_end`

## Logique principale

1. Si la pièce est franchement au-dessus de la consigne, ou au-dessus et en train de remonter, et que l'extérieur est suffisamment plus frais, la fenêtre s'ouvre d'un pas configurable, 10% par défaut.
2. Si la consigne est atteinte, ou si l'extérieur devient aussi chaud/plus chaud que l'intérieur, la fenêtre se ferme d'un pas configurable.
3. Si c'est la nuit, le volet se ferme. La fenêtre est temporairement fermée si nécessaire pour laisser le volet descendre.
4. Si la pièce chauffe et que l'extérieur n'est pas assez frais pour ventiler, le volet se ferme pour limiter l'apport de chaleur.
5. En journée, le volet se rouvre seulement si la pièce n'est plus en surchauffe et que l'extérieur n'est pas au-dessus de la zone de consigne.

---

## Installation

1. Copier le fichier YAML du blueprint dans le dossier `config/blueprints/automation/` de Home Assistant.
2. Importer le blueprint depuis l’interface Home Assistant.
3. Créer une nouvelle automatisation basée sur ce blueprint et renseigner les entités et heures demandées.

---

## Contribution

N’hésitez pas à proposer des améliorations via des issues ou pull requests sur le dépôt GitHub.

---

## Licence

MIT License

---

*Développé avec ❤️ par [TonNom]*
