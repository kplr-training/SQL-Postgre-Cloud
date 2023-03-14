Déclarations CASE
5.1 Catégorisation de la vitesse du vent

Vous pouvez utiliser une CASEinstruction pour transformer une valeur de colonne en une autre valeur basée sur des conditions. Par exemple, nous pouvons transformer différentes wind_speedplages en catégories HIGH, MODERATEet .LOW

SELECT report_code, year, month, day, wind_speed,

CASE
   WHEN wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   WHEN wind_speed >= 0 THEN 'LOW'
   ELSE 'N/A'
END as wind_severity

FROM station_data
ORDER by wind_speed DESC;

5.2 Un moyen plus efficace de catégoriser la vitesse du vent

Nous pouvons en fait omettre AND wind_speed < 40l'exemple précédent car chaque WHEN/ THENest évalué de haut en bas. Le premier qu'il trouve vrai est celui avec lequel il ira et cessera d'évaluer les conditions suivantes.

SELECT report_code, year, month, day, wind_speed,

CASE
   WHEN wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   ELSE 'LOW'
END as wind_severity

FROM station_data

5.3 Utiliser CASE avec GROUP BY

Nous pouvons utiliser GROUP BYconjointement avec une CASEinstruction pour découper les données de plusieurs manières, telles que l'obtention du nombre d'enregistrements par wind_severity.

SELECT 

CASE
   WHEN wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   WHEN wind_speed >= 0 THEN 'LOW'
   ELSE 'N/A'
END AS wind_severity,
COUNT(*)  AS record_count
FROM station_data
GROUP BY wind_severity;

De plus, certaines valeurs de wind_speed sont NULL, donc sans aucun ELSEenregistrement qui ne remplit pas une condition se révélera être NULL.

SELECT 

CASE
   WHEN wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   WHEN wind_speed >= 0 THEN 'LOW'
 
END AS wind_severity,
COUNT(*)  AS record_count
FROM station_data
GROUP BY wind_severity;

5.4 Astuce du cas "zéro/nul"

Il n'y a vraiment aucun moyen de créer plusieurs agrégations avec des conditions différentes à moins que vous ne connaissiez une astuce avec l' CASEinstruction. Si vous souhaitez trouver deux précipitations totales, avec et sans précipitations de tornade, pour chaque année et mois, vous devez effectuer des requêtes distinctes.

Précipitations de tornade

SELECT year, month,
SUM(precipitation) as tornado_precipitation
FROM station_data
WHERE tornado = 1
AND year >= 1990
GROUP BY year, month

Précipitations sans tornade

SELECT year, month,
SUM(precipitation) as non_tornado_precipitation
FROM station_data
WHERE tornado =0
AND year >= 1990
GROUP BY year, month

Mais vous pouvez utiliser une seule requête à l'aide d'une CASEinstruction qui définit une valeur sur 0 si la condition n'est pas remplie. De cette façon, cela n'aura pas d'incidence sur la somme.

SELECT year, month,
SUM(CASE WHEN tornado = 1 THEN precipitation ELSE 0 END) as tornado_precipitation,
SUM(CASE WHEN tornado = 0 THEN precipitation ELSE 0 END) as non_tornado_precipitation

FROM station_data
WHERE year >= 1990

GROUP BY year, month

Beaucoup de gens qui ne sont pas au courant de l'astuce de cas zéro/null auront recours à des tables dérivées (non couvertes dans cette classe mais couvertes dans Advanced SQL for Data Analysis ), ce qui ajoute une quantité inutile d'efforts et de désordre.

SELECT t.year,
t.month,
t.tornado_precipitation,
non_t.non_tornado_precipitation

FROM (
    SELECT year, month,
    SUM(precipitation) as tornado_precipitation
    FROM station_data
    WHERE tornado =1
    AND year >= 1990
    GROUP BY year, month
) t

INNER JOIN

(   SELECT year, month,
    SUM(precipitation) as non_tornado_precipitation
    FROM station_data
    WHERE tornado =0
    AND year >= 1990
    GROUP BY year, month
) non_t

5.5 Utiliser Null dans un CASE pour conditionner MIN/MAX

Comme NULLest ignoré dans SUM, MIN, MAX et d'autres fonctions d'agrégation, vous pouvez l'utiliser dans une CASEinstruction pour contrôler conditionnellement si une valeur doit être incluse ou non dans cette agrégation.

Par exemple, nous pouvons séparer les précipitations maximales lorsqu'une tornade était présente ou non.

SELECT year,
MAX(CASE WHEN tornado = 0 THEN precipitation ELSE NULL END) as max_non_tornado_precipitation,
MAX(CASE WHEN tornado = 1 THEN precipitation ELSE NULL END) as max_tornado_precipitation
FROM station_data
WHERE year >= 1990
GROUP BY year

Passer aux diapositives pour faire de l'exercice
Exercice 5.1

SÉLECTIONNEZ le report_code, l'année, le trimestre et la température, où un "trimestre" est "Q1", "Q2", "Q3" ou "Q4" reflétant les mois 1-3, 4-6, 7-9 et 10- 12 respectivement.

RÉPONDRE:

SELECT

report_code,
year,

CASE
    WHEN month BETWEEN 1 and 3 THEN 'Q1'
    WHEN month BETWEEN 4 and 6 THEN 'Q2'
    WHEN month BETWEEN 7 and 9 THEN 'Q3'
    WHEN month BETWEEN 10 and 12 THEN 'Q4'
END as quarter,

temperature

FROM STATION_DATA

Exercice 5.2

Obtenez la température moyenne par trimestre et par mois, où un "trimestre" est "Q1", "Q2", "Q3" ou "Q4" reflétant les mois 1-3, 4-6, 7-9 et 10-12 respectivement .

RÉPONDRE

SELECT
year,

CASE
    WHEN month BETWEEN 1 and 3 THEN 'Q1'
    WHEN month BETWEEN 4 and 6 THEN 'Q2'
    WHEN month BETWEEN 7 and 9 THEN 'Q3'
    WHEN month BETWEEN 10 and 12 THEN 'Q4'
END as quarter,

AVG(temperature) as avg_temp

FROM STATION_DATA
GROUP BY 1,2
