# Projet partogramme

Application de suivi du travail en salle de naissance (partogramme). Données fictives de TP.

## Fonctionnement du code

- Un onglet = un fichier JavaScript : `app.js` (patientes + navigation + utilitaires partagés),
  `grossesses.js`, `examens.js`, `enfants.js`, `praticiens.js`, `export.js`. Tout est en portée
  globale (pas de modules) : l'ordre des `<script>` dans `index.html` compte.
- Bibliothèques CDN : supabase-js, Chart.js (courbe de dilatation), ExcelJS (export).
- `config.js` ne contient que l'URL du projet et la clé publishable. Ne jamais y remettre la clé
  `service_role` ou `sb_secret_` : une version ancienne de ce fichier les contenait — elles doivent
  être réinitialisées dans le tableau de bord Supabase.

## Base Supabase (projet `fgjjnoaxaxwysmnsmxqq`)

- Tables : `patiente`, `grossesse`, `examen`, `enfant`, `praticien`, `orientation` (liste de
  valeurs : OIGA, OIDA, Siège… avec un nom de fichier pictogramme non affiché par le site).
- Colonnes à valeurs fixes = types « enum » Postgres : `groupe_sanguin` (patiente.gs_rh),
  `specialite` (praticien.specialite), `sexe` (enfant.sexe). Modifier l'enum dans Supabase si on
  veut ajouter une valeur, pas seulement le `<select>` du HTML.
- Clés primaires : `id_patiente`, `id_grossesse`, `id_examen`, `id_enfant`, `id_praticien`,
  `id_orientation` (numéros automatiques).
- RLS ouverte pour le TP : lecture, ajout et modification partout. **Suppression autorisée
  uniquement pour `examen`** — les autres tables n'ont pas de règle de suppression, ne pas ajouter
  de bouton « Supprimer » ailleurs sans créer la règle d'abord.
- ⚠️ Il existe une table vide `Praticien` (avec majuscule), vestige d'un essai : ne pas l'utiliser,
  la vraie table est `praticien`.

## Divers

- Pour tester en local : Python n'est pas installé, utiliser Node depuis le dossier du projet :
  `npx.cmd -y http-server -p 8000 -c-1` (le `.cmd` évite le blocage de scripts PowerShell).
- Heures enregistrées en `timestamptz` : l'export Excel écrit l'heure locale comme si c'était de
  l'UTC pour qu'Excel l'affiche correctement (voir `export.js`).
