# Suivi ménage OptiClean

Plateforme de suivi des ménages sur les locaux professionnels (bureaux, commerces).
Remplace le fichier `PLANNING 2025 OK.xlsx`, pensé pour l'ancienne activité Airbnb.

Fichier HTML autonome : aucune dépendance, aucun serveur, aucune installation.
Ouvrir `index.html` dans un navigateur (Chrome ou Edge).

## Fonctionnement

Trois écrans, accessibles par les onglets en haut :

- **Cette semaine** : l'écran d'accueil. Un agenda de la semaine en cours,
  aujourd'hui mis en avant, avec la liste des locaux à nettoyer chaque jour
  et une case à cocher. C'est l'écran à ouvrir chaque matin. Un passage
  prévu et jamais fait, plus de 30 jours en arrière, apparaît en rouge et
  compte dans un bandeau de retard en haut de l'écran, cliquable pour
  aller directement au plus ancien. Chaque passage peut aussi recevoir une
  petite remarque libre (par exemple porte fermée, agent absent).
- **Le mois** : un calendrier classique du mois, une pastille par jour
  (vert = tout est fait, rouge = un jour passé incomplet, gris = encore à
  venir). Cliquer sur un jour ouvre sa semaine dans l'écran précédent pour
  le détail.
- **Mes locaux** : la partie comptable, mois par mois. Une carte par local
  avec son forfait, sa conformité (passages prévus vs faits) et ses
  suppléments. C'est ici qu'on crée et modifie les locaux.

Autres points :

- **Les données sont en ligne, partagées entre tous les appareils.** Ouvrir
  le site depuis n'importe quel ordinateur ou téléphone affiche les mêmes
  informations, toujours à jour, sans rien sauvegarder ni charger. Le petit
  indicateur en haut à droite de l'écran (« Synchronisé », « Enregistrement… »
  ou « Hors ligne ») donne l'état de cette synchronisation. Techniquement :
  même mécanisme que la stratégie de contenu Opti'Clean (Supabase, table
  `strategies`, ligne `opticlean-suivi-menage`), voir `.claude/skills/strategie-contenu/SKILL.md`
  du dépôt principal pour le détail du fonctionnement et les points de
  vigilance (clé publique visible dans le fichier, projet gratuit mis en
  pause après 7 jours d'inactivité).
- Le bouton **Télécharger en PDF** imprime proprement l'écran affiché, pour
  garder une copie lisible. Ce n'est pas une sauvegarde technique : on ne
  peut pas recharger un PDF dans l'outil.
- Chaque local a une surface, un type d'engagement (12 mois / 6 mois / sans
  engagement) et un rythme de passage : le forfait mensuel se calcule tout
  seul à partir de la grille tarifaire officielle OptiClean (voir
  `Grilles_Tarifaires_OptiClean_3Formules.pdf`, hors de ce dossier). Un
  montant peut être saisi à la main si un cas a été négocié hors grille.
- L'agenda sert à vérifier que le contrat est respecté (jours prévus vs
  jours réellement faits, avec heure de début et de fin), pas à calculer
  le prix : le forfait reste fixe.
- Les suppléments (vitres, intervention ponctuelle...) se saisissent
  librement, local par local, mois par mois, dans l'onglet Mes locaux. Ce
  sont des montants, saisis après coup pour la facturation.
- Un local peut aussi avoir des **prestations complémentaires** (par
  exemple vitrerie ou consommables sanitaires), réglées dans sa fiche :
  un nom, une durée, et sur lequel des jours déjà prévus pour ce local
  elle s'ajoute. Contrairement aux suppléments, ce sont des minutes, pas
  des euros : elles s'ajoutent à la durée du ménage ce jour-là (visible
  dans l'agenda et dans l'heure de fin suggérée), sans changer le forfait.

## Hors périmètre (délibérément, voir le cahier des charges)

- Pas de comptes utilisateurs ni d'accès mobile pour les employés (ça
  demandera une vraie base de données en ligne, sujet à part).
- Pas d'export facture : suivi interne uniquement.
- Pas de lien avec `opticlean-devis/` (générateur de devis, resté séparé et
  manuel) ni avec les livrables de contenu OptiClean.

## Grille tarifaire (formule vérifiée)

```
forfait mensuel = temps estimé (h) × tarif horaire × passages dans le mois × (1 − remise)
```

- temps estimé : 1re tranche 0-75 m² = 1h, puis +0,5h tous les 75 m²
  supplémentaires (extrapolée au-delà de 975 m², plage max du PDF).
- tarif horaire : 36 € (12 mois), 40 € (6 mois), 43 € (sans engagement).
- passages dans le mois : nombre de passages/semaine × 52/12, sauf le
  rythme "2 passages/mois" qui reste fixe à 2.
- remise : 0 % (1/sem), 5 % (2/sem), 9 % (3/sem), 11 % (4/sem), 14 % (5/sem),
  17 % (6/sem), 20 % (7/sem).

Formule vérifiée automatiquement contre les 312 valeurs du PDF (0 écart).
