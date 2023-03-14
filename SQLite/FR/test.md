

# Déclarations CASE

### 5.1 Catégorisation de la vitesse du vent

Vous pouvez utiliser une instruction `CASE` pour transformer une valeur de colonne en une autre valeur basée sur des conditions. Par exemple, nous pouvons transformer différentes plages de `wind_speed` en catégories `HIGH`, `MODERATE` et `LOW`.

```SQL
SELECT code_rapport, année, mois, jour, vitesse_vent,

CAS
   QUAND wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   QUAND wind_speed >= 0 THEN 'LOW'
   AUTREMENT 'N/A'
END as wind_severity

DE données_station
ORDRE par vent_vitesse DESC ;
```

### 5.2 Un moyen plus efficace de catégoriser la vitesse du vent

Nous pouvons en fait omettre `AND wind_speed < 40` de l'exemple précédent car chaque `WHEN`/`THEN` est évalué de haut en bas. Le premier qu'il trouve vrai est celui avec lequel il ira et cessera d'évaluer les conditions suivantes.

```SQL
SELECT code_rapport, année, mois, jour, vitesse_vent,

CAS
   QUAND wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   AUTREMENT 'BAS'
END as wind_severity

DE données_station
```

### 5.3 Utilisation de CASE avec GROUP BY

Nous pouvons utiliser `GROUP BY` en conjonction avec une instruction `CASE` pour découper les données de plusieurs manières, telles que l'obtention du nombre d'enregistrements par `wind_severity`.

```SQL
SÉLECTIONNER

CAS
   QUAND wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   QUAND wind_speed >= 0 THEN 'LOW'
   AUTREMENT 'N/A'
END AS wind_severity,
COUNT(*) AS record_count
DE données_station
GROUP BY wind_severity ;
```

De plus, certaines valeurs de wind_speed sont NULL, donc sans `ELSE`, tous les enregistrements qui ne remplissent pas une condition se révéleront NULL.

```SQL
SÉLECTIONNER

CAS
   QUAND wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   QUAND wind_speed >= 0 THEN 'LOW'
 
END AS wind_severity,
COUNT(*) AS record_count
DE données_station
GROUP BY wind_severity ;
```

### 5.4 Astuce "Zéro/Nul"

Il n'y a vraiment aucun moyen de créer plusieurs agrégations avec des conditions différentes à moins que vous ne connaissiez une astuce avec l'instruction `CASE`. Si vous souhaitez trouver deux précipitations totales, avec et sans précipitations de tornade, pour chaque année et mois, vous devez effectuer des requêtes distinctes.

**Précipitations de tornade**
```SQL
SÉLECTIONNEZ l'année, le mois,
SUM(précipitation) as tornado_precipitation
DE données_station
OÙ tornade = 1
ET année >= 1990
GROUPER PAR année, mois
```

**Précipitations sans tornade**
```SQL
SÉLECTIONNEZ l'année, le mois,
SOMME(précipitation) as non_tornado_precipitation
DE données_station
OÙ tornade =0
ET année >= 1990
GROUPER PAR année, mois
```

Mais vous pouvez utiliser une seule requête à l'aide d'une instruction `CASE` qui définit une valeur sur 0 si la condition n'est pas remplie. De cette façon, cela n'aura pas d'incidence sur la somme.

```SQL
SÉLECTIONNEZ l'année, le mois,
SUM(CASE WHEN tornado = 1 THEN précipitation ELSE 0 END) as tornado_precipitation,
SUM(CASE WHEN tornado = 0 THEN précipitation ELSE 0 END) as non_tornado_precipitation

DE données_station
OÙ année >= 1990

GROUPER PAR année, mois
```

Beaucoup de gens qui ne sont pas conscients de l'astuce du cas zéro/null auront recours à des tables dérivées (non couvertes dans cette classe mais couvertes dans _Advanced SQL for Data Analysis_), ce qui ajoute une quantité inutile d'efforts et de désordre.

```SQL
SELECT t.année,
t.mois,
t.tornado_precipitation,
non_t.non_tornado_precipitation

DEPUIS (
    SÉLECTIONNEZ l'année, le mois,
    SUM(précipitation) as tornado_precipitation
    DE données_station
    OÙ tornade =1
    ET année >= 1990
    GROUPER PAR année, mois
) t

JOINTURE INTERNE

( SÉLECTIONNEZ l'année, le mois,
    SOMME(précipitation) as non_tornado_precipitation
    DE données_station
    OÙ tornade =0
    ET année >= 1990
    GROUPER PAR année, mois
) non_t
```

### 5.5 Utiliser Null dans un CASE pour conditionner MIN/MAX

Étant donné que `NULL` est ignoré dans SUM, MIN, MAX et d'autres fonctions d'agrégation, vous pouvez l'utiliser dans une instruction `CASE` pour contrôler conditionnellement si une valeur doit être incluse ou non dans cette agrégation.

Par exemple, nous pouvons séparer les précipitations maximales lorsqu'une tornade était présente ou non.

```SQL
SÉLECTIONNER l'année,
MAX(CASE WHEN tornado = 0 THEN précipitation ELSE NULL END) as max_non_tornado_precipitation,
MAX(CASE WHEN tornado = 1 THEN précipitation ELSE NULL END) as max_tornado_precipitation
DE données_station
OÙ année >= 1990
GROUPER PAR année
```

* Passez aux diapositives pour faire de l'exercice *


### Exercice 5.1

SÉLECTIONNEZ le report_code, l'année, le trimestre et la température, où un "trimestre" est "Q1", "Q2", "Q3" ou "Q4" reflétant les mois 1-3, 4-6, 7-9 et 10- 12 respectivement.

**RÉPONDRE:**

```SQL
SÉLECTIONNER

rapport_code,
année,

CAS
    QUAND mois ENTRE 1 et 3 ALORS 'Q1'
    QUAND mois ENTRE 4 et 6 PUIS 'Q2'
    QUAND mois ENTRE 7 et 9 ALORS 'Q3'
    QUAND mois ENTRE 10 et 12 ALORS 'Q4'
FIN comme trimestre,

température

DEPUIS STATION_DATA
```

### Exercice 5.2

Obtenez la température moyenne par trimestre et par mois, où un "trimestre" est "Q1", "Q2", "Q3" ou "Q4" reflétant les mois 1-3, 4-6, 7-9 et 10-12 respectivement .

**RÉPONDRE**

```SQL
SÉLECTIONNER
année,

CAS
    QUAND mois ENTRE 1 et 3 ALORS 'Q1'
    QUAND mois ENTRE 4 et 6 PUIS 'Q2'
    QUAND mois ENTRE 7 et 9 ALORS 'Q3'
    QUAND mois ENTRE 10 et 12 ALORS 'Q4'
FIN comme trimestre,

AVG (température) comme avg_temp

DEPUIS STATION_DATA
GROUPER PAR 1,2
```

