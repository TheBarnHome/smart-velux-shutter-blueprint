# Gestion intelligente Velux et Volet

Ce blueprint Home Assistant permet de gérer automatiquement l'ouverture et la fermeture d'une fenêtre Velux et de son volet roulant selon la température intérieure, une vraie température extérieure mesurée et la consigne du thermostat.

## Fonctionnalités

- **Ouverture progressive** de la fenêtre Velux si la pièce est trop chaude et que l'extérieur est réellement plus frais.
- **Fermeture progressive** de la fenêtre si la pièce est assez fraîche ou si l'extérieur devient plus chaud que l'intérieur.
- **Fermeture automatique** du volet lorsque la pièce chauffe sans possibilité de ventilation efficace.
- **Position cible progressive** pour éviter les cycles complet ouvert/fermé liés à l'inertie thermique.
- **Ouverture minimale de sécurité** la nuit selon `sun.sun`, tout en autorisant la fermeture complète en journée.
- **Traces optionnelles** dans le Logbook pour comprendre les décisions de l'automatisation.
- Déclenchement toutes les 10 minutes et aux changements des capteurs principaux.

## Entrées à renseigner

- `thermostat` : L’entité `climate` représentant le thermostat de la pièce (pour récupérer température actuelle et consigne).
- `outdoor_temp_sensor` : l’entité `sensor` de température extérieure réelle.
- `velux` : L’entité `cover` correspondant à la fenêtre Velux.
- `volet` : L’entité `cover` correspondant au volet roulant.
- `delta_temperature` : marge autour de la consigne pour éviter les ouvertures/fermetures trop fréquentes.
- `outdoor_cooling_delta` : écart minimum entre intérieur et extérieur avant d'ouvrir la fenêtre pour rafraîchir.
- `window_position_step` : pourcentage d'ajustement progressif appliqué à la fermeture et aux petites réouvertures.
- `min_window_position` : ouverture minimale conservée la nuit pour éviter le verrouillage, 7% par défaut.
- `window_position_tolerance` : écart ignoré entre la position actuelle et la position cible, 3% par défaut.
- `debug_logging` : active les traces détaillées dans le Logbook sous le nom `Smart Velux Shutter`.

## Déclencheurs

- Toutes les 10 minutes
- À chaque changement stable pendant 5 minutes du thermostat ou du capteur extérieur
- Aux changements de `sun.sun` pour appliquer la position de sécurité de nuit

## Logique principale

1. Le blueprint calcule d'abord une position cible de fenêtre au lieu de décider simplement ouvrir ou fermer.
2. La nuit selon `sun.sun`, la cible minimale est l'ouverture de sécurité, 7% par défaut. En journée, la cible minimale est 0%, donc la fermeture complète reste possible. La nuit n'empêche pas le rafraîchissement si l'extérieur est plus frais.
3. Si la pièce est au-dessus de la consigne et que l'extérieur est nettement plus frais, la cible passe à 100% pour rafraîchir le plus vite possible.
4. Si l'extérieur est plus frais que l'intérieur mais pas assez pour justifier une ouverture plus grande, la fenêtre garde sa position au lieu de se refermer.
5. En forte demande de rafraîchissement, la fenêtre va directement à la cible. Près de la consigne, elle rouvre et ferme par pas configurables, 10% par défaut.
6. Le volet se ferme quand la pièce chauffe sans possibilité de ventilation efficace, et se rouvre seulement si la pièce n'est plus en surchauffe et que l'extérieur n'est pas au-dessus de la zone de consigne. La nuit ne ferme plus le volet, et le jour la fenêtre peut être fermée complètement avant le volet.

## Diagnostic

Activez `debug_logging` dans l'automatisation pour ajouter une trace à chaque exécution dans le Logbook Home Assistant. Le message indique notamment les températures, la position actuelle de la fenêtre, la position cible calculée, la prochaine position demandée et les décisions volet/fenêtre.

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
