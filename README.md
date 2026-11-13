# Canvasly

Canvasly est un MVP open source de présentations vivantes : chaque slide est un canvas d’édition et devient un tableau annotable pendant la présentation. Il fonctionne localement, sans compte ni service distant.

## Lancer l’application

Il suffit d’ouvrir `index.html` dans un navigateur moderne. Pour le développement, servez ce dossier avec tout serveur statique.

## MVP livré

- Écran d’accueil enrichi : reprise du dernier projet local, nouveau projet, import PowerPoint et huit modèles (cours, pitch, atelier, cyberpunk, éditorial, nordique, coucher de soleil et minimal mono).
- Slides multiples, ajout, duplication, suppression et réorganisation par sélection.
- Navigateur de diapositives façon PowerPoint/Impress : miniatures numérotées, aperçu du contenu et réorganisation par glisser-déposer.
- Texte, rectangles, ellipses, et import d’images PNG/JPEG/WebP/SVG.
- Sélection, déplacement, redimensionnement, modification de texte, couleurs et fond.
- Sauvegarde locale automatique, export et ouverture de fichiers `.craftslides` (JSON portable).
- Nouveau projet et import local de fichiers `.pptx` : texte, images et positionnement de base des éléments sont récupérés sans connexion Internet.
- Mode présentation plein écran : navigation clavier, stylo, surligneur, gomme au trait avec largeur réglable, couleur, épaisseur, pointeur laser, écran noir/blanc.
- Gestes d’encre : rature d’un trait fraîchement écrit pour le retirer et maintien immobile pour lisser un tracé ou aligner automatiquement un trait presque droit.
- Annotations isolées par slide. Elles sont temporaires par défaut et peuvent être conservées explicitement.
- Entrées Pointer Events : souris, tactile et stylet. La pression du stylet module la largeur du trait quand le navigateur l’expose.

## Architecture

Le MVP emploie une architecture entièrement locale, sans dépendance :

```text
Modèle de présentation (slides / objets / annotations persistées)
        │
        ├─ Éditeur DOM : objets librement placés en coordonnées relatives
        ├─ Présentateur DOM : rendu de la slide en lecture seule
        └─ Calque Canvas 2D : traits temporaires et persistants par slide
```

Le contenu permanent (`objects`) et les annotations (`annotations`) sont deux collections indépendantes. Les traits temporaires vivent uniquement dans la session de présentation et sont réinitialisés à chaque changement de slide. L’action « conserver » les copie dans le calque persistant de la slide.

## Décisions techniques

Canvas 2D et Pointer Events constituent le meilleur compromis MVP : faible latence, pression de stylet native lorsque disponible et prise en charge tactile simple. Les coordonnées normalisées rendent le contenu et les traits indépendants de la résolution. Une prochaine étape naturelle est un emballage Tauri + Rust pour les dialogues de fichiers natifs, l’export PDF et une distribution Windows/macOS/Linux.

## Limites conscientes de cette version

L’import PPTX privilégie une conversion robuste du contenu essentiel : textes, images et placement de base. Les animations, graphiques, SmartArt, tableaux complexes, polices non disponibles et effets PowerPoint ne sont pas reproduits à l’identique. L’export PPTX et PDF, le rognage d’image, les connecteurs, l’effacement par portion et la reconnaissance de formes/écriture sont prévus pour des itérations ultérieures.

## Licence

MIT — voir [LICENSE](LICENSE).
