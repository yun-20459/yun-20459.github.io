---
title: Google Android Study Jam 學習筆記(6)
date: 2022-06-17 00:16:03
tags: [Andriod, Kotlin, google]
---

## 學習範圍

單元 5、單元 6、Jetpack

## SQL 語法

1. statement

   ```sql
    SELECT name FROM park
    WHERE type != "recreation_area"
    AND area_acres > 100000
   ```

2. function

    ```sql
    SELECT COUNT(*) FROM park
    SELECT SUM(park_visitors) FROM park
    SELECT MAX(area_acres) FROM park
    SELECT COUNT(DISTINCT type) FROM park
    ```

3. ordering & grouping

    ```sql
    /* sort by descending order*/
    SELECT name FROM park
    ORDER BY name DESC
    /* see how many parks of each type are present, and get a separate count for each. */
    SELECT type, COUNT(*) FROM park
    GROUP BY type
    ORDER BY type
    ```

4. inserting & deleting

    ```sql
    INSERT INTO table_name
    VALUES (column1, column2, ...)

    UPDATE table_name
    SET column1 = ...,
    column2 = ...,
    ...
    WHERE column_name = ...
    ...

    DELETE FROM table_name
    WHERE column_name = ...
    ```

## Kotlin 語法

1. companion object: class 中靜態的成員或方法，可以讓 class 不是整個都靜態的

## Android Studio

1. DAO
2. Flow
3. datastore
4. WorkManager
   - Worker
   - WorkRequest
   - WorkManager
5. Jetpack
