# 📊 Projet BI : Analyse et Prédiction de l'Absentéisme au Travail

![Statut](https://img.shields.io/badge/Statut-En_Cours-orange)
![Domaine](https://img.shields.io/badge/Domaine-Business_Intelligence-blue)

## 🎯 Contexte et Objectifs
L'absentéisme représente un coût majeur et un défi organisationnel pour les entreprises. Ce projet vise à déployer une solution de Business Intelligence (BI) complète pour analyser les causes, la fréquence et la durée des absences des employés, afin d'aider les ressources humaines à prendre des décisions préventives.

**Objectifs spécifiques :**
- Identifier les principaux motifs d'absence (maladies, consultations, etc.) selon la classification CIM.
- Analyser les corrélations entre l'absentéisme et les profils des employés (âge, charge familiale, IMC, habitudes de vie).
- Détecter les tendances saisonnières et temporelles (mois, jours de la semaine).
- Mettre en place un pipeline BI complet : de l'extraction des données à la restitution visuelle.

## 🏗️ Architecture du Projet (Pipeline BI)

Notre solution repose sur l'architecture classique des systèmes d'aide à la décision :

1. **Sources de données :** Dataset `Absenteeism_at_work.csv` recensant les profils et les heures d'absence.
2. **ETL (Extraction, Transformation, Chargement) :** Nettoyage des données, traitement des catégories (traduction des codes CIM 1-28 en libellés clairs), et chargement.
3. **Entrepôt de Données (Modèle en Étoile) :** - **Table de Faits :** Enregistrements des absences (`Temps d'absentéisme en heures`, `Frais de transport`, `Charge de travail moyenne/jour`).
   - **Tables de Dimensions :** - *Dim_Employe* (Âge, IMC, Éducation, Enfants, Animaux, Buveur/Fumeur social)
     - *Dim_Temps* (Mois, Jour de la semaine, Saison)
     - *Dim_Motif* (Catégories CIM 1-28)
     - *Dim_Trajet* (Distance domicile-travail)
4. **Système OLAP :** Création d'un Cube multidimensionnel pour naviguer dans les données (Forage vers le bas par saison puis par mois, Tranchage par département).
5. **Restitution :** Tableau de bord RH interactif.

## 🗂️ Le Dataset
Le jeu de données comprend un registre détaillé des absences avec des variables de contexte :
- **Cible :** `Temps d'absentéisme en heures`
- **Motif :** 21 catégories de maladies (CIM) + 7 catégories de consultations/absences injustifiées.
- **Profil Employé :** `Âge`, `Éducation`, `IMC` (Indice de Masse Corporelle), `Enfants`, Habitudes (`Buveur social`, `Fumeur social`).
- **Facteurs externes :** `Saison`, `Mois de l'absence`, `Distance domicile-travail`.

## 💻 Exemples de Requêtes MDX Implémentées
Pour interroger notre cube OLAP, nous utilisons le langage MDX. Voici un exemple permettant de calculer le total des heures d'absence par motif, spécifiquement pendant la saison hivernale :

```mdx
SELECT 
   {[Measures].[Total Heures Absences]} ON COLUMNS,
   {[Motif].[Categorie].Members} ON ROWS
FROM [CubeAbsenteisme]
WHERE ([Temps].[Saison].[Hiver])# Dasbord-RH
