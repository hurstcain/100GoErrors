# ОШИБКА #1: НЕПРЕДНАМЕРЕННО ЗАТЕНЯТЬ ПЕРЕМЕННЫЕ

_Область видимости переменной_ — это места в коде, в которых можно ссылаться на эту переменную. 
В Go имя переменной, уже объявленное во внешней области видимости, может быть повторно объявлено во внутренней области видимости.
Такая ситуация называется затенением переменной и может приводить к распространенным ошибкам.

```go
var client *http.Client // Объявляем переменную client
if tracing {
	// Создается HTTP-клиент со включенной трассировкой (при этом премеменная client объявляется снова, то есть затемняется)
	// Когда используем данный оператор присваивания для двух переменных, они обе объявляются автоматически 
    client, err := createClientWithTracing()
    if err != nil {
        return err
    }
} else {
	// Создается HTTP-клиент по умолчанию (переменная client также затемняется)
    client, err := createDefaultClient()
    if err != nil {
        return err
    }
}
// Использование переменной client. В данном случае client будет nil
```

Данный код можно поправить таким образом:
```go
var client *http.Client // Объявляем переменную client
var err error // Объявляем переменную err
if tracing {
	// Обе переменные не пересоздаются заново, им просто присваиваются новые значения
    client, err = createClientWithTracing()
    if err != nil {
        return err
    }
} else {
	// То же самое
}
```