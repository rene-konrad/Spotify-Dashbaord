# 🎧 Spotify Listening Behavior Dashboard  

Ein datengetriebenes Dashboard-Projekt zur **Analyse des persönlichen Spotify-Hörverhaltens**.  
Ziel war es, Muster im Musikverhalten zu erkennen — etwa bevorzugte Künstler, zeitliche Hörgewohnheiten und Orte, an denen besonders viel Musik gehört wurde.  

---

## 📊 Projektüberblick  

Das Projekt kombiniert **Python**, **SQL** und **Power BI**, um Streamingdaten aus Spotify-Exports zu bereinigen, zu analysieren und visuell aufzubereiten.  

**Hauptziele:**
- Identifikation der meistgehörten **Künstler, Songs und Alben**  
- Analyse des Hörverhaltens nach **Tageszeit, Monat und Jahr**  
- Geografische Analyse von **Orten**, an denen Musik gehört wurde  
- Vergleich von Hörmustern in verschiedenen **Lebensphasen** (Schule, Studium, Beruf)  

---

## 🧠 Tools & Technologien  

| Tool | Verwendung |
|------|-------------|
| **Python** | Datenvorbereitung & JSON-Merging |
| **SQL** | Datenmodellierung, Explorative Analysen, Erstellung von Dimensionstabellen |
| **Power BI** | Dashboard-Erstellung & Visualisierung |

---

## 🗂️ Datenbasis  

Die Daten stammen aus dem **Spotify Data Export** (JSON-Dateien), die jährlich bereitgestellt werden.  
In Python wurden alle JSONs **zusammengeführt** und als **CSV** exportiert.

Dashbaords

Startsteite des Dashboards mit Darstellung den KPI's der gehörten Minuten und den Streams mit der Darstellung der beliebtesten Alben, Songs und Künstlern.

![Aufzeichnung 2025-10-23 215256](https://github.com/user-attachments/assets/6591c513-19a4-4501-b52c-3a0adadd76ae)

Zeitanalyse nach Tageszeit und Monaten zur Ermittlung von Hörgewohnheiten. Interessante Einblicke in Tagesrythmus in verschiedenen Lebensphasen: Schule, Studieren, Vollzeit-Job.

![Aufzeichnung 2025-10-23 215536](https://github.com/user-attachments/assets/e3f29569-ae7c-4689-9127-49af16faf91e)

Ortsanalyse, mit Anzeige wo am meisten Musik über das Jahr gehört wurde, mit interssanten Einblicken über das Hörverheltnis bei verschiedenen Reisen

![Aufzeichnung 2025-10-23 215633](https://github.com/user-attachments/assets/4a67b20d-5614-49bf-bc28-367acf426e11)

🧩 Datenmodell & SQL-Analysen

Die Daten wurden in einer relationalen Struktur aufbereitet und in Dimensionstabellen überführt (z. B. dim_date, dim_time, dim_album, dim_artist, dim_location).

Beispiel: Minutenberechnung
ALTER TABLE spotify_dataset_combined ADD COLUMN minutes_played DECIMAL(10,2);
UPDATE spotify_dataset_combined 
SET minutes_played = ROUND(ms_played / 60000.0, 2);

Beispielabfragen

Top 10 Artists

WITH artist_plays AS (
  SELECT master_metadata_album_artist_name AS artist,
         SUM(ms_played / 60000.0) AS minutes_played
  FROM spotify_dataset_combined
  WHERE master_metadata_album_artist_name IS NOT NULL
  GROUP BY master_metadata_album_artist_name
)
SELECT artist, minutes_played, RANK() OVER (ORDER BY minutes_played DESC) AS rank
FROM artist_plays
WHERE rank <= 10;


Jährliche Hörzeit

SELECT SUBSTRING(ts,1,4) AS year,
       SUM(minutes_played) AS total_minutes
FROM spotify_dataset_combined
GROUP BY year
ORDER BY year;

🧾 Erkenntnisse

Deutliche Unterschiede zwischen Lebensphasen:

Schülerzeit → längere Abendaktivität

Studium → unregelmäßiges Muster

Vollzeitjob → stärker strukturierter Rhythmus

Ortsabhängigkeit: Mehr Streaming in Urlaubsländern als auf Abentteuertrips


 
