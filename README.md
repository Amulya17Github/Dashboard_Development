Social Media Addiction Dashboard

Overview

This repository contains the Tableau dashboard "Social Media Addiction Dashboard" — an interactive visualization that explores patterns of social media usage, addiction scores, academic impact, and mental health across users and countries. The dashboard helps identify which platforms contribute most to addiction, average daily usage, and correlations between usage and mental health.

Quick demo

Open images/screenshot.png to see a static preview of the dashboard layout (KPI cards, platform bars, hours vs mental health scatter, and a country map).

Prerequisites

Tableau Desktop (recommended) or Tableau Public / Tableau Reader to open .twbx or .twb files.

Dataset (suggested schema)

The dashboard assumes a flat CSV with at least the following columns:

user_id (unique identifier) age or age_group gender country (Tableau geo role = Country) most_used_platform (e.g., Instagram, YouTube, Snapchat) avg_daily_hours (numeric) addicted_score (numeric score, 0-100) mental_health_score (numeric or ordinal) affects_academic (Yes / No) or academic_impact_percent If your raw CSV uses different names, rename columns or map fields while importing to Tableau.

Step-by-step: Build the dashboard in Tableau

These steps assume you are familiar with basic Tableau operations. Use data/social_media_addiction_clean.csv as the data source.

Connect to Data

In Tableau, connect to the CSV file (data/...csv). Verify geographic role for country (set to Country).

Create Calculated Fields (examples)

Academic Impact %:AVG(IIF([affects_academic] = 'Yes', 1, 0)) * 100 High Addiction %:SUM(IIF([addiction_level] = 'High', 1, 0)) / COUNT([user_id]) * 100 Addiction Level (if not precomputed): IF [addicted_score] >= 60 THEN 'High' ELSEIF [addicted_score] >= 40 THEN 'Moderate' ELSE 'Low'

Create Sheets

KPI Cards: Create four separate single-number sheets for Academic Impact %, Avg Daily Hours (AVG([avg_daily_hours])), Avg Mental Health (AVG([mental_health_score])), and High Addiction %. Bar — Addicted Score by Platform: Rows = most_used_platform, Columns = AVG(addicted_score), sort descending, show labels. Bar — Academic Impact % by Platform: Rows = most_used_platform, Columns = AVG([Academic Impact %]) or AVG(IIF([affects_academic]='Yes',1,0)) * 100. Scatter — Hours vs Mental Health: Columns = AVG([avg_daily_hours]), Rows = AVG([mental_health_score]), Size = AVG([avg_daily_hours]) or COUNT(user_id), Color = addiction_level, mark type = Circle. Enable tooltips to show user_id, platform, country, addicted_score. Map — Daily Usage: Country on Detail, Size = AVG([avg_daily_hours]), Color = most_used_platform or addiction_level. Turn on map background and set zoom. END

Assemble Dashboard Layout

Create a new Dashboard. Set size to a desktop-friendly resolution (e.g. 1366 x 768). Use a 3-column layout: left column for platform bars & academic impact bars, center for KPI cards and scatter, right for map and filters. Add a dashboard title (large font), place KPI cards across the top (floating containers if needed). Add legends and global filters (Platform, Country, Addiction Level, Affects Academic). Apply filters to all sheets where appropriate.

Interactivity

Add Hover tooltips for each chart with contextual metrics. Create Filter Actions or Highlight Actions: clicking a platform bar filters/scopes the scatter and map. Configure parameter (optional) to let users adjust threshold used in Addiction Level buckets.

Formatting

Use consistent color palette (e.g., purples/pinks used in the screenshot). Format KPI numbers (e.g., round, append % where necessary). Add gridlines, axis labels and titles for clarity.

Export/Save

Save as packaged workbook: File -> Save As... -> .twbx and store it under tableau/social_media_addiction.twbx. Export a static preview: Dashboard -> Export Image -> images/screenshot.png.
