# Gestion intelligente Velux et Volet

Ce blueprint Home Assistant permet de gérer automatiquement l'ouverture et la fermeture d'une fenêtre Velux et de son volet roulant selon la température intérieure, la température extérieure, la tendance de la température (montée ou descente), l'heure et les conditions météo via l'intégration Météo-France.

## Fonctionnalités

- **Ouverture automatique** de la fenêtre Velux le matin si la température intérieure est supérieure à la consigne et que la température extérieure est plus fraîche.
- **Fermeture automatique** de la fenêtre et du volet si la température intérieure est trop élevée et que la température monte.
- **Fermeture automatique** du volet la nuit ou lors de fortes chaleurs, en s'assurant que la fenêtre est bien fermée.
- Gestion intelligente de la fenêtre et du volet avec des délais pour sécuriser la fermeture et réouverture de la fenêtre si nécessaire.

## Entrées à renseigner

- `thermostat` : L’entité `climate` représentant le thermostat de la pièce (pour récupérer température actuelle et consigne).
- `weather` : L’entité `weather` (ex: Météo-France) pour récupérer la température extérieure.
- `velux` : L’entité `cover` correspondant à la fenêtre Velux.
- `volet` : L’entité `cover` correspondant au volet roulant.
- `sunset_time` : L’heure à partir de laquelle le volet doit se fermer (heure du soir).
- `sunrise_time` : L’heure à laquelle la fenêtre doit s’ouvrir automatiquement le matin.
- `temp_rising` : Entité binaire/sensor indiquant si la température intérieure est en train de monter (`on` ou `off`).
- `temp_falling` : Entité binaire/sensor indiquant si la température intérieure est en train de descendre (`on` ou `off`).

## Déclencheurs

- Toutes les 5 minutes
- À l’heure d’ouverture automatique le matin (`sunrise_time`)

## Logique principale

1. Si la température intérieure est au-dessus de la consigne, que la température monte et que la fenêtre est ouverte, alors on ferme la fenêtre puis le volet (avec délai de 10 secondes entre les actions).
2. Si la température intérieure est au-dessus de la consigne, que la température extérieure est plus fraîche et que ce n’est pas la nuit, la fenêtre s’ouvre pour rafraîchir la pièce.
3. Si la température intérieure est inférieure ou égale à la consigne et que la fenêtre est ouverte, on ferme la fenêtre.
4. Si c’est la nuit ou qu’il fait très chaud (temp ext > consigne), on ferme le volet. Si la fenêtre est ouverte, on la ferme avant de fermer le volet, puis on réouvre la fenêtre après 30 secondes.
5. À l’heure d’ouverture automatique le matin, si la température intérieure est au-dessus de la consigne et la température extérieure plus fraîche, on ouvre la fenêtre.

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
