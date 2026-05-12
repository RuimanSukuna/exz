# Таймер обратного отсчёта

- **Фамилия Имя:** [Ильясов Тимур]
- **Вариант:** 9
- **Задание:** 
Поле ввода для количества секунд и кнопка «Старт». После нажатия запускается обратный отсчёт, текущее значение отображается крупным текстом. При достижении 0 выводится «Время вышло!». Использовать LaunchedEffect с delay. Кнопка «Старт» во время отсчёта неактивна.

## Описание
Приложение для обратного отсчёта времени на Jetpack Compose:
- Поле ввода секунд
- Кнопка «Старт»
- Крупное отображение текущего значения
- При достижении 0 выводится «Время вышло!»
- Кнопка «Старт» неактивна во время отсчёта

## Скриншоты

### Экран до запуска таймера
![До запуска](https://github.com/RuimanSukuna/exz/blob/main/kotlin/OuDlg-psQry3Pb_WVYUyno4HiMblxHGIyqEZE6W6ni2ycmaFkPTqyDThrJAmJUJk6HGgbvrHe3x6zY5f1QBXnGfH.jpg)

### Экран после завершения отсчёта
![Время вышло]()

## Код приложения

### MainActivity.kt
```kotlin
package com.example.exz

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import kotlinx.coroutines.delay

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                TimerScreen()
            }
        }
    }
}

@Composable
fun TimerScreen() {
    var timerValue by remember { mutableStateOf(0) }
    var isActive by remember { mutableStateOf(false) }
    var secondsInput by remember { mutableStateOf("") }
    var isFinished by remember { mutableStateOf(false) }

    LaunchedEffect(isActive, timerValue) {
        if (isActive && timerValue > 0) {
            delay(1000L)
            timerValue--
            if (timerValue == 0) {
                isActive = false
                isFinished = true
            }
        }
    }

    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Column(
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.spacedBy(24.dp)
        ) {
            // Только одно условие - никакого наложения!
            when {
                isFinished -> {
                    Text(
                        text = "Время вышло!",
                        fontSize = 48.sp,
                        fontWeight = FontWeight.Bold
                    )
                }
                else -> {
                    Text(
                        text = timerValue.toString(),
                        fontSize = 72.sp,
                        fontWeight = FontWeight.Bold
                    )
                }
            }

            OutlinedTextField(
                value = secondsInput,
                onValueChange = {
                    if (!isActive) {
                        secondsInput = it.filter { c -> c.isDigit() }
                    }
                },
                label = { Text("Секунды") },
                enabled = !isActive,
                modifier = Modifier.width(200.dp)
            )

            Button(
                onClick = {
                    val sec = secondsInput.toIntOrNull()
                    if (sec != null && sec > 0) {
                        timerValue = sec
                        isActive = true
                        isFinished = false
                        secondsInput = ""
                    }
                },
                enabled = !isActive && secondsInput.isNotBlank()
            ) {
                Text("Старт")
            }
        }
    }
}
