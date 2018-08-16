Swift
==============

* [Forced Unwrapping](#forced-unwrapping)
* [Optional Binding](#optional-binding)
* [Nil-Coalescing Operator](#nil-coalescing-operator)
* [Código muy anidado](#código-muy-anidado)
* [Type inferred context](#type-inferred-context)
* [Argument labels](#argument-labels)


Forced Unwrapping
-------------

Generalmente es peligroso hacer uso de `forced unwrapping`, ya que la aplicación se puede detener en tiempo de ejecución si el valor en cuestión es nulo. 

Evitar casos como el siguiente, donde se hace uso del operador `!`.

```swift
print("optionalInt has an integer value of \(optionalInt!).")
```

Optional Binding
--------------

Para evitar el uso del `forced unwrapping`, podemos usar el `optional binding`, sobretodo si el valor que se le quiere hacer `unwrap` se va a utilizar despúes. Esto nos permite checar que el valor no sea nulo y extraerlo en una variable o constante.

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

Argument labels
-----------

En muchas ocasiones podemos usar `argument labels` al momento de declarar funciones. Cada parámetro en una función tiene un nombre del parámetro y puede tener una etiqueta a la izquierda, el nombre es el que se usa en el cuerpo de la función mientras que la etiqueta se usa en las llamadas a la función.

Veamos el siguiente ejemplo:

```swift
func string(from date: Date, using format: OwnDateFormat) -> String {
    let dateFormatter = DateFormatter()
    dateFormatter.dateFormat = format.rawValue
    dateFormatter.locale = Locale.init(identifier: localePreferred)
    return dateFormatter.string(from: date)
}

datePicker.text =  DateHelper.string(from: currentDate, using: .local)
```

El uso de etiquetas en los parámetros permite que se llame a las funciones de una forma más expresiva, que parezca que se esta leyendo una oración, mientras que se mantiene el uso del nombre del parámetro en el cuerpo de la función.

Documenting and commenting code
-----------

Para documentar una función, clase o struct:
* Utilizar ```///``` cuando la documentación sea de una línea.
* Utilizar ```/** */``` cuando la documentación sea mayor a una línea.

Por ejemplo: 

```swift

extension UIView {
    /// Exposes the x property of the frame.
    var x: CGFloat {
        get { return frame.origin.x }
        set { frame.origin.x = newValue }
    }
    
     /**
     Add all the views passed as argument as subviews.
     - Parameter subviews: All the UIViews, separated by ',' that should be added as subviews.
     */
    func add(_ subviews: UIView ...) {
        subviews.forEach(addSubview)
    }
}
```

Para escribir un comentario:
* Utilizar ```///``` cuando el comentario sea de una línea.
* Utilizar ```/** */``` cuando la documentación sea mayor a una línea.

Por ejemplo:

```swift

override func viewDidLoad() {
   super.viewDidLoad()
   
   // Set the background color of the controller.
   view.backgroundColor = .red
   
   /*
    Configuration of the following attributes for the label: 
    - Font: The font of the label.
    - textColor: The color of the label text.
    - text: The text of the label.
   */
   label.font = UIFont.systemFont(ofSize: 12)
   label.textColor = .black
   label.text = "Hola mundo"
}
```

