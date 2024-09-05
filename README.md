Глобальні продажі електричного транспорту.
Завантаження данних
Проєкт досліджує закономірності у світових продажах електротранспорту. Використано дані Global Electric Vehicle Sales Data (2010-2024), 
завантажені з сайту Kaggle - https://www.kaggle.com Автор MUHAMMAD EHSAN 

Мета проєкту: дослідження закономірностей у світових продажах електротранспорту.
Огляд різних типів електротранспорту, доступних сьогодні, аналіз світових тенденцій демонструє стабільне зростання продажів та їх потенціалу у транспортній галузі.
Візуалізація дозволяє краще зрозуміти динаміку ринку та сьогочасну популяризацію електричного транспорту.
Сучасний період відродження та розвитку електро транспорту зумовлений світовим прогресом, потребами та проблемами екології та державними стимулами.

Використані у дослідженні технології:
SQL для написання запитів та отримання данних із датасету
Tableau - для візуалізації результатів аналізу та створення дашборду

Скрипт, написаний на SQL у BigQuery для отримання даних

WITH mode_count AS (
  SELECT mode, COUNT(*) AS count_mode
  FROM `inspired-parsec-433116-a1.Electrocars.e-cars`
  GROUP BY mode),
total_counts AS (
  SELECT SUM(count_mode) AS total_count
  FROM mode_count),
powertrain_counts AS (
  SELECT powertrain, COUNT(*) AS count_powertrain,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 1) AS percentage_powertrain
  FROM `inspired-parsec-433116-a1.Electrocars.e-cars`
  GROUP BY powertrain
),
total AS (
  SELECT region, year, ROUND(SUM(value), 2) AS total_size
  FROM `inspired-parsec-433116-a1.Electrocars.e-cars`
  GROUP BY region, year)
SELECT
  mc.mode,
  ROUND(mc.count_mode / tc.total_count * 100, 1) AS percentage_mode,
  pc.powertrain,
  pc.percentage_powertrain,
  t.region,
  t.year,
  t.total_size
FROM
  mode_count mc
CROSS JOIN
  total_counts tc
CROSS JOIN
  powertrain_counts pc
CROSS JOIN
  total t
ORDER BY
  mc.count_mode;


Основний дашборд розміщено https://public.tableau.com/app/profile/.70531054/viz/E-vehicleglobalsales/Dashboard_project

