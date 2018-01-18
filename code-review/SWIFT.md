Swift
==============

* [Forced Unwrapping](#forced-unwrapping)
* [Optional Binding](#optional-binding)
* [Nil-Coalescing Operator](#nil-coalescing-operator)
* [Código muy anidado](#código-muy-anidado)
* [Type inferred context](#type-inferred-context)


Forced Unwrapping
-------------

Generalmente es peligroso hacer uso de `forced unwrapping`, ya que la aplicación se puede detener en tiempo de ejecución si el valor en cuestión es nulo. Es preferible checar que el valor no sea nulo antes de hacer el `unwrapping`.

```swift
let optionalInt: Int? = 5

if optionalInt != nil {
    print("optionalInt has an integer value of \(optionalInt!).")
}
```

Optional Binding
--------------

Mejor aún es el uso del `optional binding` si el valor que se le quiere hacer `unwrap` se va a utilizar despúes. Esto nos permite checar que el valor no sea nulo y extraerlo en una variable o constante.

En lugar de realizar:
```swift
if dateDetails.startTime != nil {
    setStartTime(dateDetails.startTime!)
}
```
Es preferible:

```swift
if let startTime = dateDetails.startTime {
    setStartTime(startTime)
} else {
    // do something else
}
```

Nótese que esta vez no fue necesario el `!` porque el valor de `startTime` ya está `unwrapped` y es seguro poder usarlo las veces que queramos dentro del bloque del `if`.

 Nil-Coalescing Operator
----------------

Esto sirve cuando queremos usar un valor por defecto cuando la variable opcional es `nil`.

Si se tiene algo como: 

```swift
let dateString = currentDateDetails.serviceStart != nil ? currentDateDetails.serviceStart : currentDateDetails.startTime
```

Se puede refacotirzar usando este concepto, de la siguiente forma:

```swift
let dateString = currentDateDetails.serviceStart ?? currentDateDetails.startTime
```

Esto significa, "Si `currentDateDetails.serviceStart` es nulo entonces usa el valor de la derecha, si no, usa su valor desenvuelto"

Código muy anidado
----------

Veamos el siguiente código:

```swift
let dateType = preferenceManager.get(key: .dateType)
let startTime = preferenceManager.get(key: .startTime)
let endTime = preferenceManager.get(key: .endTime)
 
if dateType != nil {
    var details: Details? = nil
    if startTime != nil, endTime != nil {
        details = Details(
            date: nil,
            startTime: DateHelper.setTomorrowHour(string: startTime!),
            endTime: DateHelper.setTomorrowHour(string: endTime!)
       )
    }
    let dateInfo = DateInfo(dateType: DateType(rawValue: dateType!)!, details: details)
    store.dispatch(ServiceActionChangeDateInfo(dateInfo: dateInfo))
}
```

Vemos que si `dateType` es nulo, al final no se hace nada más, podemos aprovechar el uso de `guard` para checar si se cumple la condición que queremos y en caso contrario se salga de la función. Véase el siguiente código con correcciones:

```swift
let dateType = preferenceManager.get(key: .dateType)
let startTime = preferenceManager.get(key: .startTime)
let endTime = preferenceManager.get(key: .endTime)

guard let rawType = dateType, let type = DateType(rawValue: rawType) else {
    return
}

var details: Details? = nil
if let startTime = startTime, let endTime = endTime {
    details = Details(
        date: nil,
        startTime: DateHelper.setTomorrowHour(from: startTime),
        endTime: DateHelper.setTomorrowHour(from: endTime)
    )
}
let dateInfo = DateInfo(dateType: type, details: details)
store.dispatch(ServiceActionChangeDateInfo(dateInfo: dateInfo))
```

De esta forma se eliminó un nivel de anidación y además nótese que además se utilizó el `optional binding` para desenvolver algunos valores antes de usarlos.

Type inferred context
---------------

Opta por el uso de `contextos inferidos` que nos brinda el compilador para escribir código más limpio.

Por ejemplo, es preferible:

```swift
view.textColor = .red
let carSize = preferenceManager.get(key: .carSize)
```

En lugar de:

```swift
view.textColor = UIColor.red
let carSize = preferenceManager.get(key: Preference.carSize)
```

