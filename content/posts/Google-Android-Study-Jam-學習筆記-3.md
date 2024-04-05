---
title: Google Android Study Jam 學習筆記(3)
date: 2022-06-13 21:57:59
tags: [Andriod, Kotlin, google]
---

## 學習範圍

單元 3 課程 1

## Kotlin 語法

1. ```Set```

   ```kotlin
    val tmp = <list>.toSet()
    val set1 = setOf(1,2,3)
    val set2 = mutableSetOf(3,2,1)
   ```

   可以用 ```contains()```、```intersect()```、```union()```

2. ```Map```

   ```kotlin
    fun main() {
        val peopleAges = mutableMapOf<String, Int>(
            "Fred" to 30,
            "Ann" to 23
        )
        peopleAges.put("Barbara", 42)
        peopleAges["Joe"] = 51
        val filteredNames = peopleAges.filter { it.key.length < 4 }
        println(peopleAges.map { "${it.key} is ${it.value}" }.joinToString(", ") )
        println(peopleAges)
        peopleAges.forEach { print("${it.key} is ${it.value}, ") }
    }
   ```

3. Lambda

    ```kotlin
    fun main() {
        val triple: (Int) -> Int = { a: Int -> a * 3 }
        println(triple(5))
        val peopleNames = listOf("Fred", "Ann", "Barbara", "Joe")
        println(peopleNames.sortedWith { str1: String, str2: String -> str1.length - str2.length })
        val words = listOf("about", "acute", "awesome", "balloon", "best", "brief", "class", "coffee", "creative")
        val filteredWords = words.filter { it.startsWith("b", ignoreCase = true) }.shuffled().take(2)
    }
    ```

## Android Studio

1. activity life cycle
    ![activity life cycle](https://i.imgur.com/R9tTkZS.png)
