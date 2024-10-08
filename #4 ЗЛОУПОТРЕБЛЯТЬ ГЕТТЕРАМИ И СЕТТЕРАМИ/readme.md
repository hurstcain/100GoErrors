# ОШИБКА #4: ЗЛОУПОТРЕБЛЯТЬ ГЕТТЕРАМИ И СЕТТЕРАМИ

В некоторых языках геттеры и сеттеры являются обязательными. В Go это не так. Например, стандартная библиотека
реализует структуры, где некоторые поля доступны напрямую, как структура time.Timer:

```go
timer := time.NewTimer(time.Second)
<-timer.C // C — это поле <–chan Time
```

### Плюсы геттеров и сеттеров

* Они инкапсулируют поведение, связанное с получением полей структуры или присваиванием им значения. Это позволяет 
расширить методы новыми функциями. Например, добавить проверку поля или добавить мьютекс.
* Они скрывают внутреннее представление, давая больше гибкости в определении того, что мы раскрываем.

### Соглашения о наименованиях

Например, если использовать геттеры и сеттеры с полем Balance, нужно следовать следующим соглашениям:

* Метод геттера должен называться Balance (а не GetBalance).
* Метод сеттера должен называться SetBalance.

```go
currentBalance := customer.Balance() // Геттер
if currentBalance < 0 {
customer.SetBalance(0) // Сеттер
}
```

Не следует перегружать код геттерами и сеттерами в структурах, если они не приносят никакой пользы.
