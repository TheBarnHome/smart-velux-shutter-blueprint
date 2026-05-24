# Gestion intelligente Velux et Volet

Ce blueprint Home Assistant permet de gérer automatiquement l'ouverture et la fermeture d'une fenêtre Velux et de son volet roulant selon la température intérieure, une vraie température extérieure mesurée, les tendances de température et la consigne du thermostat.

## Fonctionnalités

- **Ouverture progressive** de la fenêtre Velux si la pièce est trop chaude et que l'extérieur est réellement plus frais.
- **Fermeture progressive** de la fenêtre si la pièce est assez fraîche ou si l'extérieur devient plus chaud que l'intérieur.
- **Fermeture automatique** du volet lorsque la pièce chauffe sans possibilité de ventilation efficace.
- **Position cible progressive** pour éviter les cycles complet ouvert/fermé liés à l'inertie thermique.
- **Anticipation optionnelle** de la tendance extérieure pour ouvrir plus prudemment si dehors chauffe, ou plus facilement si dehors refroidit.
- **Ouverture minimale de sécurité** la nuit selon `sun.sun` pour éviter de descendre sous 7% et déclencher le verrouillage du Velux.
- Déclenchement toutes les 10 minutes et aux changements des capteurs principaux.

## Entrées à renseigner

- `thermostat` : L’entité `climate` représentant le thermostat de la pièce (pour récupérer température actuelle et consigne).
- `outdoor_temp_sensor` : l’entité `sensor` de température extérieure réelle.
- `outdoor_temp_rising` : entité binaire optionnelle indiquant si la température extérieure monte (`on`) ou baisse (`off`).
- `velux` : L’entité `cover` correspondant à la fenêtre Velux.
- `volet` : L’entité `cover` correspondant au volet roulant.
- `temp_rising` : Entité binaire/sensor indiquant si la température intérieure est en train de monter (`on` ou `off`).
- `delta_temperature` : marge autour de la consigne pour éviter les ouvertures/fermetures trop fréquentes.
- `outdoor_cooling_delta` : écart minimum entre intérieur et extérieur avant d'ouvrir la fenêtre pour rafraîchir.
- `window_position_step` : pourcentage d'ajustement progressif appliqué à la fermeture et aux petites réouvertures.
- `min_window_position` : ouverture minimale conservée pour éviter le verrouillage, 7% par défaut.
- `window_position_tolerance` : écart ignoré entre la position actuelle et la position cible, 3% par défaut.

## Déclencheurs

- Toutes les 10 minutes
- À chaque changement stable pendant 5 minutes du thermostat, du capteur extérieur ou du booléen de température intérieure en hausse
- Aux changements de `sun.sun` pour appliquer la position de sécurité de nuit

## Logique principale

1. Le blueprint calcule d'abord une position cible de fenêtre au lieu de décider simplement ouvrir ou fermer.
2. La nuit selon `sun.sun`, en chauffage, ou quand l'extérieur est au moins aussi chaud que l'intérieur, la cible est l'ouverture minimale de sécurité, 7% par défaut.
3. Si la pièce est au-dessus de la consigne et que l'extérieur est nettement plus frais, la cible passe à 100% pour rafraîchir le plus vite possible.
4. Si l'extérieur est plus frais que l'intérieur mais pas assez pour justifier une ouverture plus grande, la fenêtre garde sa position au lieu de se refermer.
5. En forte demande de rafraîchissement, la fenêtre va directement à la cible. Près de la consigne, elle rouvre et ferme par pas configurables, 10% par défaut.
6. Si la tendance extérieure est renseignée, l'écart nécessaire pour ouvrir davantage augmente quand dehors chauffe et diminue quand dehors refroidit.
7. Le volet se ferme quand la pièce chauffe sans possibilité de ventilation efficace, et se rouvre seulement si la pièce n'est plus en surchauffe et que l'extérieur n'est pas au-dessus de la zone de consigne. La nuit ne ferme plus le volet.

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
