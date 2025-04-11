# Analyzing-Electric-Vehicle-Charging-Habits
Analyze data about electronic vehicle charging behavior to better use shared charging stations.


# 🔌 EV Charging Behavior Analysis

As electric vehicles (EVs) become increasingly popular, apartment buildings are responding by adding shared EV charging stations in their parking garages. But with growing demand comes a challenge: **How do we ensure these shared stations meet tenants’ needs efficiently?**

This project aims to help building managers understand tenants’ EV charging behaviors using data analysis and SQL queries on a PostgreSQL database.

<img src="charging_station.jpg" alt="EV Charging" width="500">

---

## 🧠 Project Objective

Analyze EV charging habits in shared residential parking garages to:
- Understand usage patterns
- Identify high-demand times
- Recognize long-duration users
- Support better infrastructure planning

---

## 🗃️ Dataset Overview

The data is stored in a PostgreSQL table called `charging_sessions`, containing session logs for EV users in apartment garages.

### Table: `charging_sessions`

| Column               | Description                                                   | Data Type |
|----------------------|---------------------------------------------------------------|-----------|
| `garage_id`          | Unique identifier for the garage/building                    | `VARCHAR` |
| `user_id`            | Unique identifier for the user                               | `VARCHAR` |
| `user_type`          | Type of station (`Shared` or `Private`)                      | `VARCHAR` |
| `start_plugin`       | Date & time the charging session started                     | `DATETIME`|
| `start_plugin_hour`  | Hour (24h format) when the session started                   | `NUMERIC` |
| `end_plugout`        | Date & time the charging session ended                       | `DATETIME`|
| `end_plugout_hour`   | Hour (24h format) when the session ended                     | `NUMERIC` |
| `duration_hours`     | Duration of the session (in hours)                           | `NUMERIC` |
| `el_kwh`             | Energy used during session (in kWh)                          | `NUMERIC` |
| `month_plugin`       | Month the session started                                    | `VARCHAR` |
| `weekdays_plugin`    | Day of the week the session started                          | `VARCHAR` |

---

## 📊 Analysis & SQL Queries

### 1. 🔁 Unique Users per Garage (Shared Stations)

Find the number of unique users using shared stations in each garage:

```sql
SELECT garage_id, 
       COUNT(DISTINCT user_id) AS num_unique_users
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY garage_id
ORDER BY num_unique_users DESC;
```

---

### 2. ⏰ Most Popular Charging Start Times (Shared)

Identify the top 10 most frequent start times for shared charging sessions:

```sql
SELECT weekdays_plugin,
       start_plugin_hour, 
       COUNT(*) AS num_charging_sessions
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY weekdays_plugin, start_plugin_hour
ORDER BY num_charging_sessions DESC
LIMIT 10;
```

---

### 3. 🕒 Long Duration Shared Users

Highlight users whose average charging session lasts more than 10 hours:

```sql
SELECT user_id, 
       AVG(duration_hours) as avg_charging_duration
FROM charging_sessions 
WHERE user_type = 'Shared' 
GROUP BY user_id
HAVING AVG(duration_hours) > 10
ORDER BY avg_charging_duration DESC;
```

---

## 📚 Sources

- **Dataset**: [Kaggle - Residential EV Charging](https://www.kaggle.com/datasets/anshtanwar/residential-ev-chargingfrom-apartment-buildings) [CC BY 4.0](https://creativecommons.org/licenses/by/4.0)
- **Image**: Julian Herzog, [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Elektroauto_Lades%C3%A4ule_01.jpg) [CC BY 4.0](https://creativecommons.org/licenses/by/4.0)

---
