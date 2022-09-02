---
title: Ejemplos del lenguaje
description: Ejemplos de Dart idiomático con enlaces a ejemplos más grandes.
---

Esta colección no es exhaustiva, es solo una breve introducción al lenguaje 
para las personas a las que les gusta aprender con ejemplos.
También puedes consultar los tours por el lenguaje y las bibliotecas, 
o el [codelab de la hoja de trucos de Dart](/codelabs/dart-cheatsheet).

<div class="card-grid no_toc_section">
  <div class="card">
    <h3><a href="/guides/language/language-tour">Tour del lenguaje</a></h3>
    <p>
      Un recorrido completo, con ejemplos, del lenguaje Dart.
      La mayoría de los enlaces <em>leer más</em> en esta página apuntan al tour del lenguaje.
    </p>
  </div>
  <div class="card">
    <h3><a href="/guides/libraries/library-tour">Tour de bibliotecas</a></h3>
    <p>
      Una introducción basada en ejemplos a las bibliotecas del core de Dart.
      Conoce cómo usar los tipos integrados, colecciones, 
      fechas y horas, streams y más.
    </p>
  </div>
</div>


## Hola mundo

Cada aplicación tiene una función `main()`.
Para mostrar texto en la consola, puedes usar la función `print()` de nivel superior:

<?code-excerpt "misc/test/samples_test.dart (hello-world)"?>
```dart
void main() {
  print('¡Hola mundo!');
}
```


## Variables

Incluso en el código Dart con seguridad de tipos, la mayoría de las variables 
no necesitan tipos explícitos, gracias a la inferencia de tipos:

<?code-excerpt "misc/test/samples_test.dart (var)"?>
```dart
var name = 'Voyager I';
var year = 1977;
var antennaDiameter = 3.7;
var flybyObjects = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
var image = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg'
};
```

[Leer más](/guides/language/language-tour#variables) sobre las variables en Dart, incluidos 
los valores predeterminados, las palabras clave `final` y `const`, y los tipos estáticos.


## Declaraciones de flujo de control

Dart admite las declaraciones de control de flujo habituales:

<?code-excerpt "misc/test/samples_test.dart (control-flow)"?>
```dart
if (year >= 2001) {
  print('Siglo 21');
} else if (year >= 1901) {
  print('Siglo 20');
}

for (final object in flybyObjects) {
  print(object);
}

for (int month = 1; month <= 12; month++) {
  print(month);
}

while (year < 2016) {
  year += 1;
}
```

[Leer más](/guides/language/language-tour#control-flow-statements) sobre las declaraciones 
de flujo de control en Dart, incluidas `break` y `continue`, `switch` y `case`, y `assert`.


## Funciones

[Recomendamos](/guides/language/effective-dart/design#types) especifica los tipos de 
argumentos de cada función y el valor de retorno:

<?code-excerpt "misc/test/samples_test.dart (functions)"?>
```dart
int fibonacci(int n) {
  if (n == 0 || n == 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

var result = fibonacci(20);
```

Una sintaxis abreviada `=>` (_flecha_) es útil para las funciones que 
contienen una sola instrucción.
Esta sintaxis es especialmente útil cuando se pasan funciones anónimas como argumentos:

<?code-excerpt "misc/test/samples_test.dart (arrow)"?>
```dart
flybyObjects.where((name) => name.contains('turn')).forEach(print);
```

Además de mostrar una función anónima (el argumento de `where()`), 
este código muestra que puedes usar una función como argumento: 
la función `print()` de nivel superior es un argumento de `forEach()`.

[Leer más](/guides/language/language-tour#functions) sobre funciones en Dart, 
incluidos parámetros opcionales, valores predeterminados de parámetros y alcance léxico.


## Comentarios

Los comentarios de Dart generalmente comienzan con `//`.

```dart
// Este es un comentario normal de una línea.

/// Este es un comentario de documentación, usado para documentar bibliotecas,
/// clases y sus miembros. Herramientas como IDE y dartdoc tratan 
/// los comentarios de documentos de forma especial.

/* También se admiten comentarios como estos. */

```

[Leer más](/guides/language/language-tour#comments) sobre los comentarios en Dart, 
incluido cómo funcionan las herramientas de documentación.


## Importaciones

Para acceder a las API definidas en otras bibliotecas, use `import`.

<?code-excerpt "misc/test/samples_test.dart (import)" plaster="none"?>
```
// Importa bibliotecas del core
import 'dart:math';

// Importa bibliotecas de paquetes externos
import 'package:test/test.dart';

// Importa archivos
import 'path/to/my_other_file.dart';
```

[Leer más](/guides/language/language-tour#libraries-and-visibility)
sobre bibliotecas y visibilidad en Dart, incluidos los prefijos de biblioteca, 
`show` y `hide`, y la carga diferida a través de la palabra clave `deferred`.


## Clases

Aquí hay un ejemplo de una clase con tres propiedades, dos constructores y un método. 
Una de las propiedades no se puede establecer directamente, por lo que se define 
mediante un método getter (en lugar de una variable).

<?code-excerpt "misc/lib/samples/spacecraft.dart (class)"?>
```dart
class Spacecraft {
  String name;
  DateTime? launchDate;

  // Propiedad no final de solo lectura
  int? get launchYear => launchDate?.year;

  // Constructor, con azúcar sintáctico para asignación a miembros.
  Spacecraft(this.name, this.launchDate) {
    // El código de inicialización va aquí.
  }

  // Constructor con nombre que reenvía al predeterminado.
  Spacecraft.unlaunched(String name) : this(name, null);

  // Método.
  void describe() {
    print('Spacecraft: $name');
    // La promoción de tipos no funciona con getters.
    var launchDate = this.launchDate;
    if (launchDate != null) {
      int years = DateTime.now().difference(launchDate).inDays ~/ 365;
      print('Lanzado: $launchYear (hace $years años)');
    } else {
      print('No lanzado');
    }
  }
}
```

Podrías usar la clase `Spacecraft` así:

<?code-excerpt "misc/test/samples_test.dart (use class)" plaster="none"?>
```dart
var voyager = Spacecraft('Voyager I', DateTime(1977, 9, 5));
voyager.describe();

var voyager3 = Spacecraft.unlaunched('Voyager III');
voyager3.describe();
```

[Leer más](/guides/language/language-tour#classes) sobre las clases en Dart, 
incluidas las listas de inicializadores, `new` y `const` opcionales, 
constructores de redireccionamiento, constructores `factory`, getters, 
setters y mucho más.


## Enums

Las enumeraciones son una forma de enumerar un conjunto predefinido de valores 
o instancias de una manera que garantiza que no puede haber otras 
instancias de ese tipo.

Aquí hay un ejemplo de un `enum` simple que define una lista simple de 
tipos de planetas predefinidos:

<?code-excerpt "misc/lib/samples/spacecraft.dart (simple-enum)"?>
```dart
enum PlanetType { terrestrial, gas, ice }
```

Aquí hay un ejemplo de una declaración de enumeración mejorada de una clase que 
describe planetas, con un conjunto definido de instancias constantes, 
a saber, los planetas de nuestro propio sistema solar.

<?code-excerpt "misc/lib/samples/spacecraft.dart (enhanced-enum)"?>
```dart
/// Enum que enumera los diferentes planetas de nuestro sistema solar 
/// y algunas de sus propiedades.
enum Planet {
  mercury(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  venus(planetType: PlanetType.terrestrial, moons: 0, hasRings: false),
  // ···
  uranus(planetType: PlanetType.ice, moons: 27, hasRings: true),
  neptune(planetType: PlanetType.ice, moons: 14, hasRings: true);

  /// Un constructor de generación constante
  const Planet(
      {required this.planetType, required this.moons, required this.hasRings});

  /// Todas las variables de instancia son finales
  final PlanetType planetType;
  final int moons;
  final bool hasRings;

  /// Las enumeraciones mejoradas admiten getters y otros métodos
  bool get isGiant =>
      planetType == PlanetType.gas || planetType == PlanetType.ice;
}
```

Podrías usar la enumeración `Planet` así:

<?code-excerpt "misc/test/samples_test.dart (use enum)" plaster="none"?>
```dart
final yourPlanet = Planet.earth;

if (!yourPlanet.isGiant) {
  print('Tu planeta no es un "planeta gigante".');
}
```

[Leer más](/guides/language/language-tour#enums) sobre las enumeraciones en Dart, 
incluidos los requisitos de enumeración mejorados, las propiedades introducidas 
automáticamente, el acceso a los nombres de valores enumerados, 
la compatibilidad con declaraciones switch y mucho más.


## Herencia

Dart tiene herencia única.

<?code-excerpt "misc/lib/samples/spacecraft.dart (extends)"?>
```dart
class Orbiter extends Spacecraft {
  double altitude;

  Orbiter(super.name, DateTime super.launchDate, this.altitude);
}
```

[Leer más](/guides/language/language-tour#extending-a-class) sobre 
la extensión de clases, la anotación opcional `@override` y más.


## Mixins

Los mixins son una forma de reutilizar el código en varias jerarquías 
de clases. La siguiente es una declaración de mixin:

<?code-excerpt "misc/lib/samples/spacecraft.dart (mixin)"?>
```dart
mixin Piloted {
  int astronauts = 1;

  void describeCrew() {
    print('Número de astronautas: $astronauts');
  }
}
```

Para agregar las capacidades de un mixin a una clase, simplemente extiende la clase con el mixin.

<?code-excerpt "misc/lib/samples/spacecraft.dart (mixin-use)" replace="/with/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
class PilotedCraft extends Spacecraft [!with!] Piloted {
  // ···
}
{% endprettify %}

`PilotedCraft` ahora tiene el campo `astronauts` así como el método `describeCrew()`.

[Leer más](/guides/language/language-tour#adding-features-to-a-class-mixins) sobre mixins.


## Interfaces y clases abstractas

Dart no tiene la palabra clave `interface`.
En cambio, todas las clases definen implícitamente una interfaz.
Por lo tanto, puedes implementar cualquier clase.

<?code-excerpt "misc/lib/samples/spacecraft.dart (implements)"?>
```dart
class MockSpaceship implements Spacecraft {
  // ···
}
```

[Leer más](/guides/language/language-tour#implicit-interfaces) sobre las interfaces implícitas.

Puedes crear una clase abstracta para que una clase concreta la amplíe (o la implemente).
Las clases abstractas pueden contener métodos abstractos (con cuerpos vacíos).

<?code-excerpt "misc/lib/samples/spacecraft.dart (abstract)" replace="/abstract/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
[!abstract!] class Describable {
  void describe();

  void describeWithEmphasis() {
    print('=========');
    describe();
    print('=========');
  }
}
{% endprettify %}

Cualquier clase que extienda `Describable` tiene el método `describeWithEmphasis()`, 
que llama a la implementación del extensor de `describe()`.

[Leer más](/guides/language/language-tour#abstract-classes) 
sobre clases y métodos abstractos.


## Async

Evita el infierno de devolución de llamada y haz que tu código sea 
mucho más legible usando `async` y `await`.

<?code-excerpt "misc/test/samples_test.dart (async)" replace="/async/[!$&!]/g"?>
{% prettify dart tag=pre+code %}
const oneSecond = Duration(seconds: 1);
// ···
Future<void> printWithDelay(String message) [!async!] {
  await Future.delayed(oneSecond);
  print(message);
}
{% endprettify %}

El método anterior es equivalente a:

<?code-excerpt "misc/test/samples_test.dart (Future.then)"?>
```dart
Future<void> printWithDelay(String message) {
  return Future.delayed(oneSecond).then((_) {
    print(message);
  });
}
```

Como muestra el siguiente ejemplo, `async` y `await` ayudan a que 
el código asíncrono sea fácil de leer.

<?code-excerpt "misc/test/samples_test.dart (await)"?>
```dart
Future<void> createDescriptions(Iterable<String> objects) async {
  for (final object in objects) {
    try {
      var file = File('$object.txt');
      if (await file.exists()) {
        var modified = await file.lastModified();
        print(
            'File for $object already exists. It was modified on $modified.');
        continue;
      }
      await file.create();
      await file.writeAsString('Start describing $object in this file.');
    } on IOException catch (e) {
      print('Cannot create description for $object: $e');
    }
  }
}
```

También puedes usar `async*`, que te brinda una forma agradable y legible de crear streams.

<?code-excerpt "misc/test/samples_test.dart (async*)"?>
```dart
Stream<String> report(Spacecraft craft, Iterable<String> objects) async* {
  for (final object in objects) {
    await Future.delayed(oneSecond);
    yield '${craft.name} flies by $object';
  }
}
```

[Leer más](/guides/language/language-tour#asynchrony-support) sobre 
la compatibilidad con la asincronía, incluidos `Future`, `Stream`, 
las funciones `async`, y el bucle asíncrono (`await for`).

## Excepciones

Para generar una excepción, usa `throw`:

<?code-excerpt "misc/test/samples_test.dart (throw)"?>
```dart
if (astronauts == 0) {
  throw StateError('No astronauts.');
}
```

Para capturar una excepción, usa una instrucción `try` con `on` o `catch` (o ambos):

<?code-excerpt "misc/test/samples_test.dart (try)"?>
```
try {
  for (final object in flybyObjects) {
    var description = await File('$object.txt').readAsString();
    print(description);
  }
} on IOException catch (e) {
  print('No se pudo describir el objeto: $e');
} finally {
  flybyObjects.clear();
}
```

Ten en cuenta que el código anterior es asíncrono; 
`try` funciona tanto para el código síncrono como para el código en una función `async`.

[Leer más](/guides/language/language-tour#exceptions) sobre las excepciones, incluidos 
los seguimientos de pila (stack traces), `rethrow` y la diferencia 
entre `Error` y `Exception`.

## Otros temas

Hay muchas más muestras de código en el 
[tour del lenguaje](/guides/language/language-tour) y el 
[tour por las bibliotecas](/guides/libraries/library-tour).
Consulta también la [referencia de API de Dart]({{site.dart-api}}), 
que a menudo contiene ejemplos.
