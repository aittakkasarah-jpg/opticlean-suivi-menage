# Suivi ménage OptiClean

Plateforme de suivi des ménages sur les locaux professionnels (bureaux, commerces).
Remplace le fichier `PLANNING 2025 OK.xlsx`, pensé pour l'ancienne activité Airbnb.

Fichier HTML autonome : aucune dépendance, aucun serveur, aucune installation.
Ouvrir `index.html` dans un navigateur (Chrome ou Edge).

## Fonctionnement

Deux écrans, accessibles par les deux onglets en haut :

- **Cette semaine** : l'écran d'accueil. Un agenda de la semaine en cours,
  aujourd'hui mis en avant, avec la liste des locaux à nettoyer chaque jour
  et une case à cocher. C'est l'écran à ouvrir chaque matin.
- **Mes locaux** : la partie comptable, mois par mois. Une carte par local
  avec son forfait, sa conformité (passages prévus vs faits) et ses
  suppléments. C'est ici qu'on crée et modifie les locaux.

Autres points :

- **Un seul utilisateur, sur un seul ordinateur.** Les données restent dans le
  navigateur (stockage local). Cliquer régulièrement sur **Sauvegarder** pour
  télécharger un fichier de secours, et sur **Charger** pour le remettre.
  Si le navigateur est vidé sans sauvegarde, les données de cette page sont
  perdues : c'est le compromis d'un outil sans base de données.
- Chaque local a une surface, un type d'engagement (12 mois / 6 mois / sans
  engagement) et un rythme de passage : le forfait mensuel se calcule tout
  seul à partir de la grille tarifaire officielle OptiClean (voir
  `Grilles_Tarifaires_OptiClean_3Formules.pdf`, hors de ce dossier). Un
  montant peut être saisi à la main si un cas a été négocié hors grille.
- L'agenda sert à vérifier que le contrat est respecté (jours prévus vs
  jours réellement faits, avec heure de début et de fin), pas à calculer
  le prix : le forfait reste fixe.
- Les suppléments (vitres, intervention ponctuelle...) se saisissent
  librement, local par local, mois par mois, dans l'onglet Mes locaux.

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
