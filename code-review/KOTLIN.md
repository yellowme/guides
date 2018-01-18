Kotlin
============

* [Unwrap justificado](#unwrap-justificado)
* [Código repetido](#evitar-repetirse)
* [Nombramientos](#nombramientos)
* [Validaciones null](#validaciones-null)
* [Objetos opcionales muy anidados](#accesando-objetos-opcionales-muy-anidados)
* [Uso de constantes](#usar-el-archivo-de-constantes)

Unwrap justificado
------------

Recordemos que Kotlin introduce el concepto de [Null Safety](https://kotlinlang.org/docs/reference/null-safety.html), por lo cual recuerda hacer un uso **justificado** del operador `!!`

En caso de no tener una razón justificada, las alternativas a usar el operador `!!` para forzar el `unwrap` de la variable son:

* [Safe Calls](https://kotlinlang.org/docs/reference/null-safety.html#safe-calls)
* [Elvis Operator](https://kotlinlang.org/docs/reference/null-safety.html#elvis-operator)
* [Safe Casts](https://kotlinlang.org/docs/reference/null-safety.html#safe-casts)

Evitar repetirse
------------

**Evita** situaciones como:

```kotlin
fun saveAddress(address: String, lat: String, lng: String) {
    val json = preferences.getPreferencesString(STORED_SERVICE)

    var mService = Service()
    if(json.isNotEmpty()) {
        mService = Service.fromJson(json).apply {
            this.address = address
            this.lat = lat
            this.lng = lng
        }
    } else {
        mService.apply {
            this.address = address
            this.lat = lat
            this.lng = lng
        }
    }
    preferences.registerPreferences(STORED_SERVICE, mService.toJson())
}

```

**Es preferible** delegar resposabilidades a otros métodos, en este caso al método `fromJson`:

```kotlin
fun saveAddress(address: String, lat: String, lng: String) {
    val json = preferences.getPreferencesString(STORED_SERVICE)
    val mService = Service.fromJson(json).apply {
        this.address = address
        this.lat = lat
        this.lng = lng
    }
    preferences.registerPreferences(STORED_SERVICE, mService.toJson())
}
```

Nombramientos
------------

### Archivos de tipo `Repository`

Suponiendo tenemos una clase llamada `ServicesRepository`

**Es preferible** nombrar a los métodos:

```kotlin
fun create(): LiveData<ApiResponse<Service>> { }
fun update(): LiveData<ApiResponse<Service>> { }
fun get(): LiveData<ApiResponse<Service>> { }
```

**Evita**:

```kotlin
fun createService(): LiveData<ApiResponse<Service>> { }
fun updateService(): LiveData<ApiResponse<Service>> { }
fun getService(): LiveData<ApiResponse<Service>> { }
```

Validaciones `null`
-----

### Multiples validaciones

Cuando te encuentres en una situación donde el método `.let` sobre dos variables opcionales pueda causar una anidación innecesaria, puedes agregar las siguientes [extensiones de código](https://gist.github.com/LuisBurgos/1510ad0bfbab23d65f4a14f30736b67f). Lo anterior te da acceso al método `safeLet`

**Es preferible**:

```kotlin
safeLet(startTime, endTime) { start, end ->
    //DO STUFF HERE
}
```

**Evita**:

```kotlin
if(startTime != null && endTime != null) { //DO STUFF HERE }
```

Accesando objetos opcionales muy anidados
-------

**Evita** situaciones como:

```kotlin
fun someMethod() {
    val startTime = service.dateInfo?.details?.startTime?.parsePrettyTimeFullDate()?.hours?.toFloat()

    val endTime = service.dateInfo?.details?.endTime?.parsePrettyTimeFullDate()?.hours?.toFloat()

    if(startTime != null && endTime != null) {
        dateRangeBar.setRangePinsByValue(
            startTime,
            endTime)
    }
}
```

Analizemos lo anterior podemos identificar:

* Un uso inadecuado del `code layout` al llamar a la función `setRangePinsByValue`
* Se accede dos veces a la variable `service.dateInfo?.details?`
* Se llamada dos veces al mismo método `.parsePrettyTimeFullDate()?.hours?.toFloat()`
* No se sigue el lineamiento de multiple validación de `null`.

En esta situación **es preferible**:

```kotlin
fun someMethod() {
    service.dateInfo?.details?.apply {
        safeLet(toFloat(startTime), toFloat(endTime), rangeBar) { start, end, bar ->
                bar.setRangePinsByValue(start, end)
        }
    }
}

// Método privado o extension.
private fun toFloat(string: String?): Float? {
    return string?.parsePrettyTimeFullDate()?.hoursFloat()
}
```

Usar el archivo de constantes
-------

Localiza el archivo `Constants.kt` o equivalente en el proyecto que te encuentres.
Recuerda que los `magic numbers` y `magic strings` son el principal enemigo de otros desarrolladores y del tiempo que pasará hasta que vuelves a retomar el código fuente que escribiste.
