# 🎯 Projet de Transformation de Données - Power Query

Bienvenue dans ce projet d’analyse de données où nous appliquons des transformations intelligentes pour mieux comprendre les comportements d'achats des clients à travers différentes sources.

## 🌱 Objectif du Projet

L'objectif principal est de **transformer des données brutes** provenant de la table "Subscriptions and churns" qui ont été divisées en deux bases, **All-time subscription** et **Channel_Subscription** et de les analyser pour **catégoriser les achats** selon deux sources :
1. **SMS** - Achat lié à un SMS spécifique.
2. **Habitude** - Achat répété habituellement par le client.

## 🧑‍💻 La Méthodologie Utilisée

Nous avons utilisé **Power Query** disponible dans **Excel (2016)** pour transformer et analyser les données. Voici les étapes clés de notre processus :

### 1. **Vérification et Modification de la base**

Avant de commencer toute transformation, nous avons pris soin de faire les jointures(externe) afin d'avoir uniquement les numéros de téléphone correspondants, de garantir que toutes les **dates** étaient au bon format dans chaque table. Cela a permis d'éviter des erreurs lors des étapes suivantes.

- **Format "date courte"** appliqué sur toutes les colonnes de dates.

### 2. **Ajout de la Logique de Catégorisation**

Une fois les dates alignées, nous avons calculé le nombre de mois qui sépare la date de la première souscription de la date de la dernière souscription. En associant ce chiffre au nombre d'achat du client, nous pouvons savoir si ce dernier est actif ou pas, et trouver une stratégie pour le relancer.
A côté de cela, nous avons vérifier si la date "Date Last Billed" correspond à l'une des dates de campagne SMS.

//Calucler le nombre de mois qui sépare la date de la première souscription de la date de la dernière souscription
```m
= (Date.Year([Date Last Billed]) - Date.Year([Date Joined])) * 12 + (Date.Month([Date Last Billed]) - Date.Month([Date Joined]))

// Vérification si la date "Date Last Billed" correspond à l'une des dates spécifiées:
let
    Resultat = if List.Contains({#date(2024, 11, 1), #date(2024, 11, 2), #date(2024, 11, 3), #date(2024, 11, 4), #date(2024, 11, 5), #date(2024, 11, 6), #date(2024, 11, 7), #date(2024, 11, 8), #date(2024, 11, 9), #date(2024, 11, 10), #date(2024, 11, 11), #date(2024, 11, 12), #date(2024, 11, 13), #date(2024, 11, 14), #date(2024, 11, 15), #date(2024, 11, 16), #date(2024, 11, 17), #date(2024, 11, 18), #date(2024, 11, 19), #date(2024, 11, 20), #date(2024, 11, 21), #date(2024, 11, 22), #date(2024, 11, 23), #date(2024, 11, 24), #date(2024, 11, 25)}, [Date Last Billed]) then "SMS" else "Habitude"
in
    Resultat


appliqué une logique conditionnelle pour vérifier si un achat était **lié à un SMS spécifique** ou s'il était **répétitif** dans les habitudes du client.

Exemple de code utilisé pour cela :

```m
let
    Source = [Nom de votre source de données],
    Resultat = Table.AddColumn(Source, "Source", each if List.Contains({#date(2024, 11, 1), #date(2024, 11, 2), #date(2024, 11, 3)}, [Date Last Billed]) then "SMS" else "Habitude")
in
    Resultat
```

👉 **En résumé** : Ce code analyse chaque date et catégorise l'achat en fonction de la date associée.

### 3. **Application à Toutes les Tables**

Une fois la logique validée pour une table, il est simple de **l'appliquer à toutes les autres tables** avec Power Query. Le processus devient donc scalable et adaptable à de nombreux scénarios.

---

## 🛠️ Outils et Technologies Utilisées

- **Power Query** dans **Microsoft Excel** pour transformer les données.
- **Code M** : le langage utilisé pour appliquer des règles et manipuler les données.
  
## 📥 Télécharger le Projet

Pour télécharger le projet, rien de plus simple ! Vous pouvez le **cloner directement depuis GitHub** ou le **télécharger sous forme de fichier ZIP**. 

### 📂 Étapes pour télécharger :

1. **Cloner** le projet en utilisant Git :  
   ```bash
   git clone https://github.com/username/nom-du-projet.git
   ```

2. **Télécharger** en ZIP :  
   - Allez sur la page du projet GitHub et cliquez sur "Code" puis "Download ZIP".

Une fois téléchargé, ouvrez-le dans **Excel**, et accédez à l'éditeur **Power Query** pour commencer à explorer.

---

## ✨ Pourquoi ce projet est fun ?

C'est plus qu'un simple projet de transformation de données, c'est une véritable **aventure** dans l'analyse des comportements clients ! Vous verrez que, grâce à Power Query, les étapes de transformation sont simples et interactives, et le résultat final vous donnera une belle vue d'ensemble sur les achats des clients.

### Bonus : Si vous aimez manipuler des **dates** et découvrir des **patterns cachés**, ce projet est fait pour vous ! 🔍

---

## 🚀 Contribuer

Vous pouvez participer à ce projet ! **Forkez**, **modifiez** et envoyez vos **pull requests** pour ajouter des améliorations, des transformations supplémentaires ou de nouvelles fonctionnalités.

---

## 📌 Liens Utiles

- [GitHub du projet](https://github.com/username/nom-du-projet)
- [Notion](https://votre-lien-notion.com)

---

## 📝 Conclusion

Si vous cherchez un projet simple mais efficace pour comprendre comment transformer des données en informations exploitables, vous êtes au bon endroit. **Power Query** transforme un fichier Excel en véritable tableau de bord interactif !

Alors, prêt à transformer vos données ? 🚀

---

Ce format est à la fois ludique, simple et engageant, avec des touches de convivialité pour donner envie de s'impliquer dans le projet !
