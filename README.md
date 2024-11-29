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
A côté de cela, nous avons vérifier si un achat était **lié à un SMS spécifique** ou s'il était **répétitif** dans les habitudes du client en se basant sur [Date last Billed] = Dernière date de facturation et la date de recrutement (qui corespond à la date de campagne SMS).

```m
//Calucler le nombre de mois qui sépare la date de la première souscription de la date de la dernière souscription
= (Date.Year([Date Last Billed]) - Date.Year([Date Joined])) * 12 + (Date.Month([Date Last Billed]) - Date.Month([Date Joined]))
```

```m
// Vérifier si la date "Date Last Billed" correspond à l'une des dates de campagne SMS afin de savoir la source d'achat:
let
    Resultat = if List.Contains({#date(2024, 11, 1), #date(2024, 11, 2), #date(2024, 11, 3), #date(2024, 11, 4), #date(2024, 11, 5), #date(2024, 11, 6), #date(2024, 11, 7), #date(2024, 11, 8), #date(2024, 11, 9), #date(2024, 11, 10), #date(2024, 11, 11), #date(2024, 11, 12), #date(2024, 11, 13), #date(2024, 11, 14), #date(2024, 11, 15), #date(2024, 11, 16), #date(2024, 11, 17), #date(2024, 11, 18), #date(2024, 11, 19), #date(2024, 11, 20), #date(2024, 11, 21), #date(2024, 11, 22), #date(2024, 11, 23), #date(2024, 11, 24), #date(2024, 11, 25)}, [Date Last Billed]) then "SMS" else "Habitude"
in
    Resultat
```


Pour pouvoir l'appliquer à toutes les tables, nous avons automatisé cette formule en créant une fonction qu'on pourra appelé à n'importe quel moment :
```m
let
    // Fonction qui applique la logique sur chaque ligne
    AppliquerLogique = (table as table) as table =>
        Table.AddColumn(table, "Resultat", each 
            if List.Contains({#date(2024, 11, 1), #date(2024, 11, 2), #date(2024, 11, 3), #date(2024, 11, 4), 
                             #date(2024, 11, 5), #date(2024, 11, 6), #date(2024, 11, 7), #date(2024, 11, 8), 
                             #date(2024, 11, 9), #date(2024, 11, 10), #date(2024, 11, 11), #date(2024, 11, 12), 
                             #date(2024, 11, 13), #date(2024, 11, 14), #date(2024, 11, 15), #date(2024, 11, 16), 
                             #date(2024, 11, 17), #date(2024, 11, 18), #date(2024, 11, 19), #date(2024, 11, 20), 
                             #date(2024, 11, 21), #date(2024, 11, 22), #date(2024, 11, 23), #date(2024, 11, 24), 
                             #date(2024, 11, 25)}, [Date Last Billed]) then "SMS" else "Habitude"
        )
in
    AppliquerLogique
```

👉 **En résumé** : Ce code analyse chaque date et catégorise l'achat en fonction de la date associée.


### 3. **Application à Toutes les Tables**

Une fois la logique validée pour une table, il est simple de **l'appliquer à toutes les autres tables** avec Power Query. Le processus devient donc scalable et adaptable à de nombreux scénarios.

Exemple de code utilisé :

// Répétez cette procédure pour toutes les autres tables, en remplaçant FOOT 01 par le nom de chaque feuille
```m
let
    Source = FOOT 01,
    Resultat = AppliquerLogique(Source)
in
    Resultat
```


---

## 🛠️ Outils et Technologies Utilisées

- **Power Query** dans **Microsoft Excel** pour transformer les données.
- **Code M** : le langage utilisé pour appliquer des règles et manipuler les données.

 
---

## ✨ Pourquoi ce projet est fun ?

C'est plus qu'un simple projet de transformation de données, c'est une véritable **aventure** dans l'analyse des comportements clients ! Grâce à ce projet, nous avons pu extraire des informations sur les comportements d'achat des clients, comme la fréquence des achats et les tendances mensuelles.


## Résultats visuels : Tableau Croisé Dynamique (TCD)

Pour visualiser les tendances des achats des abonnés, un Tableau Croisé Dynamique a été créé. Ce TCD permet de distinguer les abonnés ayant acheté par habitude de ceux ayant acheté grâce à un SMS. Voici la structure du TCD :

- **Lignes :** Les dates de dernières facturation
- **Colonnes :** Source ("Habitude" ou "SMS")
- **Valeurs :** Nombre d'achats (msisdn)

### Capture d'écran du TCD :
Voici une vue du TCD montrant la répartition des achats par type :

![TCD Abonnés](C:\Users\DELL\Documents\Visual Studio 2017\Projects\Transformation de données - Power Query\Projet-de-Transformation-de-Donnees--Power-Query\Assets\TCD.png)

### Analyse du TCD
Le TCD a permis d'identifier les comportements des abonnés :
- Une majorité des achats proviennent des utilisateurs recevant un SMS (indiquant des actions marketing efficaces)
- Les achats par habitude ont également montré une petite tendance, ce qui pourrait indiquer une absence de fidélisation


---

## 📝 Conclusion

Ce projet a permis de mieux comprendre les tendances des abonnés à nos services et d'identifier les moments d'achat clés.
