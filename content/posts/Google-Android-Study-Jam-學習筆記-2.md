---
title: Google Android Study Jam 學習筆記(2)
date: 2022-06-11 12:30:51
tags: [Andriod, Kotlin, google]
---

## 學習範圍

單元 2 課程 3

## Kotlin 語法

1. list
   - list
       - 宣告

        ```kotlin
        val numbers: List<Int> = listOf(1, 2, 3, 4, 5, 6)
        ```

       - 其他

        ```kotlin
        println("List: $numbers") // = println("List: " + numbers)
        println("Size: ${numbers.size}")
        println("First element: ${numbers[0]}")
        println("First: ${numbers.first()}")
        println("Last: ${numbers.last()}")
        println("Contains 4? ${numbers.contains(4)}") // true
        println("Contains 7? ${numbers.contains(7)}") // false
        println("Reversed list: ${colors.reversed()}")
        println("Sorted list: ${colors.sorted()}")
        ```

   - mutablelist
       - 宣告

        ```kotlin
        val entrees = mutableListOf<String>() // val entrees: MutableList<String> = mutableListOf()
        ```

       - 其他

        ```kotlin
        println("Add noodles: ${entrees.add("noodles")}")
        val moreItems = listOf("ravioli", "lasagna", "fettuccine")
        println("Add list: ${entrees.addAll(moreItems)}") // "Add list: true"
        println("Remove spaghetti: ${entrees.remove("spaghetti")}")
        println("Remove first element: ${entrees.removeAt(0)}")
        entrees.clear()
        println("Empty? ${entrees.isEmpty()}")
        ```

2. ```while```

    ```kotlin
    val guestsPerFamily = listOf(2, 4, 1, 3)
    var totalGuests = 0
    var index = 0
    while (index < guestsPerFamily.size) {
        totalGuests += guestsPerFamily[index]
        index++
    }
    println("Total Guest Count: $totalGuests")
    ```

3. ```for```

    ```kotlin
    for (name in names) println(name)
    for (item in 5 downTo 1) print(item)
    for (item in 3..6 step 2) print(item)
    ```

4. ```vararg```
    將類型相同、數量可變的引數傳遞給函式或建構函式

    ```kotlin
    // 只能將一個參數標示為 vararg，且通常是清單中的最後一個參數。
    class Vegetables(vararg val toppings: String) : Item("Vegetables", 5) {
    ```

## Android Studio

1. RecycleView
    ![pieces involved in creating and using a RecyclerView](https://i.imgur.com/eKHLae3.png)

2. MaterialCardView

## 其他

- [front end roadmap](https://roadmap.sh/frontend)
