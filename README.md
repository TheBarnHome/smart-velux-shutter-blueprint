# Gestion intelligente Velux

Ce blueprint Home Assistant permet de gérer automatiquement l'ouverture et la fermeture d'une fenêtre Velux selon la température intérieure, une vraie température extérieure mesurée, la tendance intérieure et une consigne de température.

## Fonctionnalités

- **Ouverture proportionnelle** de la fenêtre Velux selon l'écart entre la pièce et la consigne.
- **Fermeture progressive** de la fenêtre si la pièce est assez fraîche ou si l'extérieur devient plus chaud que l'intérieur.
- **Position cible PID-like stateless** pour éviter les cycles complet ouvert/fermé liés à l'inertie thermique.
- **Ouverture minimale de sécurité** la nuit selon `sun.sun`, tout en autorisant la fermeture complète en journée.
- **Traces optionnelles** dans le Logbook pour comprendre les décisions de l'automatisation.
- Déclenchement toutes les 10 minutes et aux changements des capteurs principaux.

## Entrées à renseigner

- `thermostat` : entité `climate` représentant le thermostat de la pièce, ou simple `sensor` de température intérieure.
- `target_temperature` : température cible utilisée quand `thermostat` est un simple thermomètre, ou si le thermostat n'expose pas de consigne.
- `outdoor_temp_sensor` : l’entité `sensor` de température extérieure réelle.
- `velux` : L’entité `cover` correspondant à la fenêtre Velux.
- `indoor_temp_rising` : entité binaire indiquant si la température intérieure est en hausse.
- `delta_temperature` : marge autour de la consigne pour éviter les ouvertures/fermetures trop fréquentes.
- `outdoor_cooling_delta` : écart minimum entre intérieur et extérieur avant d'ouvrir la fenêtre pour rafraîchir.
- `proportional_band` : écart à la consigne qui correspond à 100% d'ouverture, 3°C par défaut.
- `window_position_step` : pourcentage d'ajustement progressif appliqué à la fermeture et aux petites réouvertures.
- `min_window_position` : ouverture minimale conservée la nuit pour éviter le verrouillage, 7% par défaut.
- `window_position_tolerance` : écart ignoré entre la position actuelle et la position cible, 3% par défaut.
- `debug_logging` : active les traces détaillées dans le Logbook sous le nom `Smart Velux Shutter`.

## Déclencheurs

- Toutes les 10 minutes
- À chaque changement stable pendant 5 minutes du thermostat, du capteur extérieur ou de la tendance intérieure
- Aux changements de `sun.sun` pour appliquer la position de sécurité de nuit

## Logique principale

1. Le blueprint calcule d'abord une position cible de fenêtre au lieu de décider simplement ouvrir ou fermer.
2. Si l'entité intérieure est un thermostat, la consigne vient du thermostat. Si c'est un simple thermomètre, la consigne vient du slider `target_temperature`.
3. La nuit selon `sun.sun`, la cible minimale est l'ouverture de sécurité, 7% par défaut. En journée, la cible minimale est 0%, donc la fermeture complète reste possible. La nuit n'empêche pas le rafraîchissement si l'extérieur est plus frais.
4. Si la pièce est au-dessus de la consigne et que l'extérieur est nettement plus frais, la cible est calculée proportionnellement : 0% d'écart vise la cible minimale, `proportional_band` ou plus vise 100%.
5. En journée, si la température intérieure recommence à monter, la fenêtre se referme vers la cible minimale seulement quand le refroidissement extérieur est insuffisant.
6. Si l'extérieur est plus frais que l'intérieur mais pas assez pour justifier une ouverture plus grande, la fenêtre garde sa position au lieu de se refermer.
7. La fenêtre se déplace vers la cible par pas configurables, 10% par défaut, en ouverture comme en fermeture.

## Diagnostic

Activez `debug_logging` dans l'automatisation pour ajouter une trace à chaque exécution dans le Logbook Home Assistant. Le message indique notamment les températures, l'erreur à la consigne, le ratio proportionnel, la marge de refroidissement, la tendance intérieure, la position actuelle de la fenêtre, la position cible calculée, la prochaine position demandée et les décisions d'ouverture/fermeture.

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
