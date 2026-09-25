# Cahier des charges — Suivi ménage OptiClean

Dernière mise à jour : 25 septembre 2026. Ce document décrit ce qui est réellement construit aujourd'hui, pas le projet initial : plusieurs choix ont changé en cours de route, ce fichier reflète l'état actuel.

## Contexte

Opti'Clean est déjà client de l'agence pour du contenu (stratégie Instagram/TikTok et bibliothèque interne LinkedIn, deux livrables distincts et sans lien avec cet outil). Ce projet est un troisième chantier, opérationnel : une plateforme web pour remplacer le fichier Excel `PLANNING 2025 OK.xlsx`, qui servait au suivi ménage.

Le client a changé d'activité en cours de route : il passait du ménage sur appartements type Airbnb (ménage déclenché par l'arrivée/le départ d'un locataire, suppléments type canapé lit ou check in) à du ménage sur des locaux professionnels (bureaux, commerces), avec des contrats à passages fixes et récurrents, facturés au forfait mensuel. L'outil a été pensé pour ce nouveau modèle, pas pour numériser l'ancien Excel tel quel.

## Le modèle métier

- Chaque local a un contrat : une surface, un type d'engagement (12 mois, 6 mois, ou sans engagement) et un rythme de passage (de 1 passage par semaine à 7, ou 2 passages par mois).
- Le forfait mensuel se calcule automatiquement à partir de la grille tarifaire officielle OptiClean (voir `Grilles_Tarifaires_OptiClean_3Formules.pdf`, hors de ce dossier). La formule a été vérifiée contre les 312 valeurs du PDF, sans écart.
- Le forfait reste fixe quel que soit le nombre réel de passages faits dans le mois : le rôle de l'outil est de vérifier que le contrat est respecté, pas de recalculer un prix.
- Un montant peut être saisi à la main si un local a été négocié en dehors de la grille.

## Ce que l'outil propose

Trois écrans, accessibles par des onglets :

1. **Cette semaine** : l'écran d'accueil. Agenda de la semaine en cours, aujourd'hui mis en avant. Pour chaque local prévu ce jour, une case à cocher (fait ou pas fait), avec l'heure de début et de fin du passage (une heure de fin est suggérée automatiquement à partir du temps estimé de la grille, à ajuster si besoin). Un passage exceptionnel, non prévu au contrat, peut être ajouté ponctuellement à n'importe quel jour.
2. **Le mois** : calendrier classique, une pastille de couleur par jour (tout est fait, encore à venir, ou jour passé incomplet). Cliquer sur un jour ouvre sa semaine dans l'écran précédent pour le détail.
3. **Mes locaux** : la partie comptable, mois par mois. Une fiche par local avec son forfait, son indicateur de conformité (passages prévus contre passages faits), ses suppléments (saisie libre, nom et montant, par exemple vitres ou intervention ponctuelle) et son total. C'est aussi ici qu'on crée, modifie ou supprime un local.

## Fonctionnement technique

- Site web autonome (une seule page HTML/CSS/JavaScript), sans installation, à ouvrir dans un navigateur (Chrome ou Edge recommandés).
- **Les données sont en ligne et partagées entre tous les appareils.** Ouvrir le site depuis n'importe quel ordinateur ou téléphone affiche les mêmes informations, toujours à jour, sans sauvegarde ni chargement manuel. Un indicateur en haut de l'écran donne l'état de cette synchronisation (Synchronisé, Enregistrement en cours, ou Hors ligne).
- Techniquement, cette synchronisation réutilise l'infrastructure déjà en place pour la stratégie de contenu Opti'Clean (un compte Supabase gratuit, une table générique `strategies`, une ligne dédiée à cet outil). Voir `.claude/skills/strategie-contenu/SKILL.md` dans le dépôt principal de l'agence pour le détail.
- Un bouton **Télécharger en PDF** imprime proprement l'écran affiché, pour garder une copie lisible ou l'envoyer par mail. Ce n'est pas une sauvegarde technique : on ne peut pas recharger un PDF dans l'outil.

## Ce que l'outil ne fait pas (délibérément, pour l'instant)

- **Pas de comptes utilisateurs individuels.** Toute personne qui a le lien du site peut voir et modifier toutes les données : il n'y a ni connexion, ni mot de passe, ni accès différencié par intervenant. C'est un poste de pilotage unique, pas un outil multi-utilisateur.
- **Pas d'export facture.** Suivi interne uniquement, les totaux s'affichent à l'écran ou s'impriment, mais l'outil ne génère pas de facture.
- **Pas de reprise de l'historique Excel.** L'outil est reparti de zéro : l'ancien fichier correspond de toute façon à une autre activité (l'Airbnb).
- **Aucun lien avec `opticlean-devis/`**, le générateur de devis et de contrat existant, qui reste un outil séparé et manuel.

## Évolution possible, hors périmètre actuel

Si le besoin apparaît d'ouvrir l'outil à plusieurs intervenants (par exemple un agent de ménage qui ne voit que ses propres locaux, avec son propre calendrier et ses propres droits), c'est un fonctionnement différent : une vraie base de données avec comptes personnalisés et gestion des permissions, pas juste des données partagées comme aujourd'hui. C'est un chantier technique distinct, à chiffrer séparément le moment venu.

## Points de vigilance à connaître

- La clé de connexion à la base en ligne est visible dans le fichier du site (c'est normal pour ce type d'outil), et n'importe qui avec le lien peut lire ou modifier les données. Pas de sujet pour un usage interne à l'entreprise, mais le lien ne doit pas être diffusé publiquement.
- Le compte gratuit qui héberge les données se met en pause après 7 jours sans utilisation et doit être relancé depuis son tableau de bord. Il faut savoir qui, à l'agence ou chez le client, a accès à ce compte.
- L'hébergement définitif du site (pour lui donner une adresse publique stable) reste à définir, probablement Netlify comme les autres sites de l'agence.
