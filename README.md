# Devoir-1-CSI2532

## Partie A [60 points]: Modèles E-R

### A1 [15 points]: Relations, cardinalité et participation

a) [5 points] Un étudiant peut être supervisé par plusieurs professeurs, et un professeur peut
superviser plusieurs étudiants. Un étudiant a un numéro d'étudiant, un nom et est inscrit dans
un programme spécifique. Un professeur a un numéro d'employé, nom et domaine d'expertise

![Partie A_a](Images/question_A1_a.png)

b) [5 points] Toutes les sections de cours doivent être affectées à un cours. Un cours a un code
de département (par exemple CSI ou SEG) et un numéro de cours (par exemple 2532 ou 4105).
Une section de cours est défini uniquement par le nom de la section (par exemple A ou B ou C),
semestre (par exemple hiver), l'année et le cours lui-même. Une section de cours dépend sur le
cours.

![Partie A_b](Images/question_A1_b.png)

c) [5 points] Une entreprise a un nom et un quartier général (spécifié par pays). Un conseil d'administration a
un président, un vice-président, et secrétaire (juste leurs noms). Une entreprise peut avoir au
plus un conseil administrateurs (mais il n'en a pas besoin). Tout les conseils d'administration
doivent avoir une et une seule entreprise à gérer.

![Partie_A_c](Images/question_A1_c.png)

### A2 [30 points]: Conception du système

On vous demande de concevoir une base de données (à l'aide d'un diagramme relationnel)
CorporateBnB, une entreprise qui permet à toutes les personnes ayant un bureau à domicile de
fournir des espaces de réunion à d'autres. Tous les utilisateurs peut s'inscrire à ce service en
fournissant leur nom, email et numéro de téléphone. Un utilisateur peut gérer un espace de
bureau, ou peut louer un espace de bureau. Un bureau a un nom, une adresse et une superficie
totale (en pieds carrés). Chaque bureau a la disponibilité pour quand il peut être loué. La
disponibilité stocke la date et le coût quotidien pour l'utilisation, les heures d'arrivée et de départ
et si cette date est toujours disponible. Lorsqu'un utilisateur loue une propriété, on stock la date
d'arrivée et la date de départ.
Modélisez le système ci-dessus à l'aide d'un diagramme Entité-Relation. Vous devez inclure les
entités, les relations, la cardinalité, la participation et les attributs. Vous n'avez pas besoin de
spécifier les types d'attributs (c'est-à-dire le domaine).

![Partie_A_2](Images/question_A2.png)


### A3 [15 points]: Algèbre relationnelle

a) [7 points] Trouvez tous les espaces de bureau à Ottawa qui sont disponibles le 2 mars 2020


b) [8 points] Trouvez tous les utilisateurs (nom et email) et les détails de la propriété (nom et
ville) et les informations de location (date et coût quotidien) de toutes les bureaux loués du mois
de janvier 2020.


## Partie B [60 points]: SQL

### B1. [15 points] Lecture de requêtes SQL
a) [4 points]
```sql
SELECT
 name,
 EXTRACT(year from age(users.join_date)) AS experience
FROM users
JOIN licenses ON licenses.user_id = users.id
WHERE licenses.software_name = 'MS Word'
ORDER BY users.name;
```

Le code sql ci-dessus fonctionnait comme prévu. Il permettait d'extraire l'année de la table des utilisateurs et permettait également de joindre les licenses des utilisateurs avec le logiciel pour lequel le nom était MS Word. Finalement, tous les résultats sont triés (ordonnés) en fonction des noms des utilisateurs. Voici le résultat obtenu:

![Partie_B_1](Images/question_B1_a.png)

b) [4 points]
```sql
SELECT name,
 released_date
FROM softwares
WHERE released_date < '2018-01-01'
ORDER BY released_date ASC;
```
Pour cette partie, le code sql ci-dessus sélectionner deux colonnes (name et released_date) de la table _softwares_ avec une condition particulière: Il fallait que la date de distribution du logiciel ce soit faite avant le 01 janvier 2018. De plus, tous les résultats étaient ordonnés par date de distribution en order croissant(ascendant). Voici le résultat obtenu:

![Partie_B_2](Images/question_B1_b.png)

c) [7 points]
```sql
WITH users_2019 (id, name) AS
 (SELECT *
 FROM users
 WHERE join_date BETWEEN '2019-01-01' AND '2019-12-31')
SELECT id,
 name,
 count(licenses.access_code) AS num
FROM users_2019
LEFT JOIN licenses ON licenses.user_id = id
GROUP BY name
ORDER BY num DESC;
```
Après avoir fait plusieurs recherches sur internet, j’ai découvert que la raison pour laquelle cette requête ne fonctionnait pas était dû au fait que la ligne à laquelle ‘’GROUP BY NAME’’ était écrit devait également contenir le user_2019.id afin de pouvoir s’assurer que la requête s’exécute comme prévu. Voici donc le nouveau code modifié:
```sql
WITH users_2019 (id, name) AS
 (SELECT *
 FROM users
 WHERE join_date BETWEEN '2019-01-01' AND '2019-12-31')
SELECT id,
 name,
 count(licenses.access_code) AS num
FROM users_2019
LEFT JOIN licenses ON licenses.user_id = id
GROUP BY name, users_2019.id
ORDER BY num DESC;
```
De plus, voici la sortie que j'ai obtenu ensuite:

![Partie_B_3](Images/question_B1_c.png)
