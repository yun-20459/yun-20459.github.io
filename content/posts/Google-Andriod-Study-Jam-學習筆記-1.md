---
title: Google Andriod Study Jam 學習筆記(1)
date: 2022-06-10 21:10:46
tags: [Andriod, Kotlin, google]
---

## 學習範圍

單元 1 ~ 單元 2 課程 2

## Kotlin 語法

1. 函式

    ```kotlin
    fun <function name> : <return type> {

    }
    ```

2. 輸出

    ```kotlin
    println("Hi") // 有換行
    print("Hi") // 沒換行
    ```

3. 宣告變數

    ```kotlin
    val <variable name> = <variable value>
    val <variable name> = <class name>(parameter)
    ```

4. 迴圈

    ```kotlin
    repeat (<times>) {
        
    }
    ```

5. ```class```

    ```kotlin
    class <class name> (val <variable name>: <variable type>, ...) {
        val <variable name> = <variable type>
        fun <function name> {
        
        }
    }
    ```

    - inherit
  
        ```kotlin
        // abstract: 未完整實作，無法執行個體化
        abstract class Dwelling(private var residents: Int) {
            abstract val buildingMaterial: String
            abstract val capacity: Int

            abstract fun floorArea(): Double

            fun hasRoom(): Boolean {
                return residents < capacity
            }

            fun getRoom() {
                if (capacity > residents) {
                    residents++
                    println("You got a room!")
                } else {
                    println("Sorry, at capacity and no rooms left.")
                }
            }

        }


        class SquareCabin(
            residents: Int, val length: Double) : Dwelling(residents) {
            override val buildingMaterial = "Wood"
            override val capacity = 6

            override fun floorArea(): Double {
                return length * length
            }

        }

        // 只有 open 跟 abstract 可以被繼承
        open class RoundHut(
            val residents: Int, val radius: Double) : Dwelling(residents) {

            override val buildingMaterial = "Straw"
            override val capacity = 4

            override fun floorArea(): Double {
                return PI * radius * radius
            }

            fun calculateMaxCarpetSize(): Double {
                val diameter = 2 * radius
                return sqrt(diameter * diameter / 2)
            }

        }

        class RoundTower(
            residents: Int,
            radius: Double,
            val floors: Int = 2) : RoundHut(residents, radius) {

            override val buildingMaterial = "Stone"

            override val capacity = floors * 4

            override fun floorArea(): Double {
                return super.floorArea() * floors
            }
        }
        ```

6. IntRange

    ```kotlin
    val tmp = 1..6 // tmp 是一個 type IntRange 的 variable, 範圍是 [1, 6]
    ```

7. ```when```

    ```kotlin
    when (<variable>) {
        <value> -> <execute content>
    }
    ```

    其中 ```<value>```可以是給定 variable

8. ```with```

    ```kotlin
    with (instanceName) {
        // all operations to do with instanceName
    }
    ```

    搭配```class```的例子

    ```kotlin
    with(squareCabin) {
        println("\nSquare Cabin\n============")
        println("Capacity: ${capacity}")
        println("Material: ${buildingMaterial}")
        println("Has room? ${hasRoom()}")
    }
    ```

## Android Studio

1. UI
   - ```TextView```、```ImageView```、```Button```
   - ![parent and child](https://i.imgur.com/dSMHnvo.png)
   - [material design tool1](https://material.io/resources/color/#!/?view.left=0&view.right=0)
   - [material design tool2](https://material.io/design/color/the-color-system.html#color-theme-creation)
   - [component](https://material.io/components)

2. hardcode
    不要 hardcode，把 text 的名稱弄到 string.xml
    ex.

    ```xml
    <resources>
        <string name="app_name">Happy Birthday</string>
        <string name="happy_birthday_text">Happy Birthday, Sam!</string>
    </resources>
    ```

    text 那格就變成填 ```@string/happy_birthday_text```

3. object reference (其實我不知道應該把它歸在寫 app 還是語法)

   ```kotlin
   val rollButton: Button = findViewById(R.id.button)
   val resultTextView: TextView = findViewById(R.id.textView)
   resultTextView.text = "6"
   ```

   ```findViewById```會在 layout 中找到 ```Button```，然後把```Button```的 object reference 存到 rollButton 裡

4. 顯示訊息給使用者

    ```kotlin
    val toast = Toast.makeText(this, "Dice Rolled!", Toast.LENGTH_SHORT)
    toast.show()
    ```

5. 設定貨幣（數字）規格

   ```kotlin
   val formattedTip = NumberFormat.getCurrencyInstance().format(tip)
   ```

6. binding

    activitity_main.xml 中

    ```xml
    binding.tipResult.text = getString(R.string.tip_amount, formattedTip)
    ```

    string.xml 中

    ```xml
    <string name="tip_amount">Tip Amount: %s</string>
    ```

7. 打完字收起小鍵盤

   ```kotlin
    private fun handleKeyEvent(view: View, keyCode: Int): Boolean {
        if (keyCode == KeyEvent.KEYCODE_ENTER) {
            // Hide the keyboard
            val inputMethodManager =
                getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
            inputMethodManager.hideSoftInputFromWindow(view.windowToken, 0)
            return true
        }
        return false
    }
   ```

8. 螢幕轉向、不同尺寸

   ```xml
    <ScrollView
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_height="match_parent"
        android:layout_width="match_parent">

        <androidx.constraintlayout.widget.ConstraintLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="16dp"
            tools:context=".MainActivity">

            ...
        </ConstraintLayout>

    </ScrollView>
   ```

9. unittest

    ```kotlin
    @RunWith(AndroidJUnit4::class)
    class CalculatorTests {

        @get:Rule()
        val activity = ActivityScenarioRule(MainActivity::class.java)

        @Test
        fun calculate_20_percent_tip() {
            onView(withId(R.id.cost_of_service_edit_text))
                .perform(typeText("50.00"))

            onView(withId(R.id.calculate_button)).perform(click())

            onView(withId(R.id.tip_result))
                .check(matches(withText(containsString("$10.00"))))
        }
    }
    ```
