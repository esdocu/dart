---
title: Dart overview
description: Una breve introducción a Dart
js: [{url: 'https://dartpad.dev/inject_embed.dart.js', defer: true}]
---

<img 
  style="padding: 30px; float: right; width: 300px" 
  src="/assets/img/logo_lockup_dart_horizontal.png" 
  alt="Dart product logo">

Dart es un lenguaje optimizado para el cliente para desarrollar aplicaciones rápidas en cualquier plataforma.
Su objetivo es ofrecer el lenguaje de programación más productivo para el desarrollo multiplataforma, junto con una [plataforma de tiempo de ejecución de ejecución flexible](#platform) para frameworks de aplicaciones.

Los lenguajes se definen por su _envoltura técnica_: las elecciones realizadas durante el desarrollo que dan forma a las capacidades y fortalezas de un lenguaje.
Dart está diseñado para un entorno técnico que es particularmente adecuado para el desarrollo de clientes, priorizando tanto el desarrollo (hot reload) como las experiencias de producción de alta calidad en una amplia variedad de objetivos de compilación (web, móvil y escritorio).

Dart también forma la base de [Flutter]({{site.flutter}}).
Dart proporciona el lenguaje y los tiempos de ejecución que impulsan las aplicaciones de Flutter, pero Dart también es compatible con muchas tareas básicas del desarrollador, como formatear, analizar y testing del código.

## Dart: El lenguaje {#language}

El lenguaje Dart es de tipo seguro; utiliza la verificación de tipo estático para garantizar que el valor de una variable _siempre_ coincida con el tipo estático de la variable.
Aunque los tipos son obligatorios, las anotaciones de tipos son opcionales debido a la inferencia de tipos.
El sistema de escritura Dart también es flexible, lo que permite el uso de un tipo `dynamic` combinado con comprobaciones en tiempo de ejecución, lo que puede ser útil durante la experimentación o para el código que necesita ser especialmente dinámico.

Dart ofrece [null safety](/null-safety), lo que significa que los valores no pueden ser nulos a menos que digas que pueden serlo.
Con una sólida seguridad de valores nulos, Dart puede protegerte de excepciones nulas en tiempo de ejecución a través del análisis de código estático.
A diferencia de muchos otros lenguajes seguros para nulos, cuando Dart determina que una variable no admite nulos, esa variable _siempre_ no admite nulos.
Si inspeccionas tu código en ejecución en el depurador, verás que la no nulabilidad se conserva en el tiempo de ejecución (es decir, null safety).

El siguiente ejemplo de código muestra varias características del lenguaje Dart, incluidas libraries, llamadas asíncronas, tipos que aceptan y que no aceptan valores null, sintaxis de flecha, generadores, streams y getters.
Para encontrar ejemplos del uso de funciones adicionales de Dart, consulta la [página de muestras](/samples).
Para obtener más información sobre el lenguaje, realiza el [tour de Dart](/guides/language/language-tour).

<?code-excerpt "misc/lib/overview_pi.dart"?>
```dart:run-dartpad:ga_id-overview
import 'dart:math' show Random;

void main() async {
  print('Calcular π utilizando el método de Monte Carlo.');
  await for (final estimate in computePi().take(100)) {
    print('π ≅ $estimate');
  }
}

/// Genera un stream de estimaciones cada vez más precisas de π.
Stream<double> computePi({int batch = 100000}) async* {
  var total = 0; // Inferido para ser de tipo int
  var count = 0;
  while (true) {
    final points = generateRandom().take(batch);
    final inside = points.where((p) => p.isInsideUnitCircle);

    total += batch;
    count += inside.length;
    final ratio = count / total;

    // El área de un círculo es A = π⋅r², por lo tanto, π = A/r².
    // Entonces, cuando se dan puntos aleatorios con x ∈ <0,1>,
    // y ∈ <0,1>, la proporción de aquellos dentro de un círculo unitario
    // debe aproximarse a π / 4. Por lo tanto, el valor de π
    // debiera ser:
    yield ratio * 4;
  }
}

Iterable<Point> generateRandom([int? seed]) sync* {
  final random = Random(seed);
  while (true) {
    yield Point(random.nextDouble(), random.nextDouble());
  }
}

class Point {
  final double x;
  final double y;

  const Point(this.x, this.y);

  bool get isInsideUnitCircle => x * x + y * y <= 1;
}
```

{{site.alert.info}}
  Este ejemplo se ejecuta en un [DartPad](/tools/dartpad) incrustado.
  También puedes <a href="{{site.dartpad}}/bc63d212c3252e44058ff76f34ef5730" target="_blank" rel="noopener">
  abrir este ejemplo en tu propia ventana</a>.
{{site.alert.end}}


## Dart: Las bibliotecas {#libraries}

Dart tiene [un amplio conjunto de bibliotecas de core](/guides/libraries),
proporcionando elementos esenciales para muchas tareas diarias de programación:

* Tipos incorporados, colecciones y otras funcionalidades básicas 
  para cada programa Dart 
  (`dart:core`)
* Tipos de colección más ricos, como colas, listas enlazadas, hashmaps y
  árboles binarios 
  (`dart:collection`)
* Codificadores y decodificadores para convertir entre diferentes representaciones 
  de datos, incluidos JSON y UTF-8
  (`dart:convert`)
* Constantes y funciones matemáticas, y generación de números aleatorios 
  (`dart:math`)
* Soporte de archivo, socket, HTTP y otras E/S para aplicaciones no web 
  (`dart:io`)
* Soporte para programación asíncrona, con clases como `Future` y `Stream` 
  (`dart:async`)
* Listas que manejan eficientemente datos de tamaño fijo (por ejemplo, 
  enteros de 8 bytes sin signo) y tipos numéricos SIMD 
  (`dart:typed_data`)
* Interfaces de funciones externas para la interoperabilidad con otro código 
  que presenta una interfaz de estilo C 
  (`dart:ffi`)
* Programación concurrente usando workers _aislados_ independientes
  que son similares a hilos pero no comparten memoria, comunicándose 
  solo a través de mensajes 
  (`dart:isolate`)
* Elementos HTML y otros recursos para aplicaciones basadas en web que 
  necesitan interactuar con el navegador y el Modelo de Objetos de Documento (DOM) 
  (`dart:html`)

Más allá de las bibliotecas del core, muchas API se proporcionan 
a través de un conjunto completo de paquetes.
El equipo de Dart publica muchos paquetes complementarios útiles, 
como estos:

* [characters]({{site.pub-pkg}}/characters)
* [intl]({{site.pub-pkg}}/intl) 
* [http]({{site.pub-pkg}}/http)
* [crypto]({{site.pub-pkg}}/crypto)
* [markdown]({{site.pub-pkg}}/markdown)

Además, los editores externos y la comunidad en general publican miles de 
paquetes, con soporte para características como estas:

* [XML]({{site.pub-pkg}}/xml) 
* [Windows integration]({{site.pub-pkg}}/win32)
* [SQLite]({{site.pub-pkg}}/sqflite_common)
* [compression]({{site.pub-pkg}}/archive)

Para ver una serie de ejemplos de trabajo con las bibliotecas del core de Dart, 
realiza el [tour por la biblioteca](/guides/libraries/library-tour).
Para encontrar API adicionales, consulta la 
[página de paquetes de uso común](/guides/libraries/useful-libraries).

## Dart: Las plataformas {#platform}

La tecnología de compilación de Dart te permite ejecutar código de diferentes maneras:

* **Plataforma nativa**: para aplicaciones destinadas a dispositivos móviles y de escritorio,
   Dart incluye una máquina virtual Dart con compilación just-in-time (JIT) y
   un compilador ahead-of-time (AOT) para producir código de máquina.

* **Plataforma web**: para aplicaciones dirigidas a la web, Dart puede compilar para
   fines de desarrollo o producción. Su compilador web traduce Dart
   en JavaScript.

<img 
  src="/assets/img/Dart-platforms.svg" 
  width="800" 
  alt="An illustration of the targets supported by Dart">

El [Flutter framework]({{site.flutter}}) es un popular conjunto de herramientas de interfaz de usuario 
multiplataforma que funciona con la plataforma Dart y que proporciona herramientas y bibliotecas 
de interfaz de usuario para crear experiencias de interfaz de usuario que se ejecutan en iOS, Android, 
macOS, Windows, Linux y la web.

#### Dart Nativo (código de máquina JIT y AOT) {#native-platform}

Durante el desarrollo, un ciclo de desarrollo rápido es fundamental para la iteración.
Dart VM ofrece un compilador just-in-time (JIT) con recompilación incremental 
(que permite la recarga en caliente o hot reload), recopilaciones de métricas en vivo 
(alimentando [DevTools](/tools/dart-devtools)) y soporte de depuración enriquecido.

Cuando las aplicaciones están listas para implementarse en producción, ya sea que estés 
publicando en una tienda de aplicaciones o implementando en un backend de producción, el
compilador Dart Ahead-of-Time (AOT) puede compilar a ARM nativo o código de máquina x64. 
Tu aplicación compilada por AOT se inicia con un tiempo de inicio corto y constante.

El código compilado por AOT se ejecuta dentro de un tiempo de ejecución de Dart eficiente 
que aplica el sistema de tipo de Dart y administra la memoria mediante la asignación 
rápida de objetos y un [recolector de basura generacional](https://medium.com/flutter-io/flutter-dont-fear-the-garbage-collector-d69b3ff1ca30).

Más información:
* [Comenzar: línea de comandos y aplicaciones de servidor](/tutorials/server/get-started)
* [Herramienta `dart` para ejecutar con JIT o AOT compilando código de máquina](/tools/dart-tool)
* [Escribir aplicaciones de línea de comandos](/tutorials/server/cmdline)
* [Escribir servidores HTTP](/tutorials/server/httpserver)

#### Dart Web (JavaScript dev & prod) {#web-platform}

Dart Web permite ejecutar código Dart en plataformas web con tecnología de JavaScript. 
Con Dart Web, compila el código Dart en código JavaScript, que a su vez se ejecuta en 
un navegador, por ejemplo, [V8](https://v8.dev/) dentro de [Chrome](https://www.google.com/chrome/).

Dart web contiene dos modos de compilación:

* Un compilador de desarrollo incremental que permite un ciclo de desarrollo rápido
* Un compilador de producción optimizado que compila el código Dart en JavaScript rápido, 
  compacto e implementable. Estas eficiencias provienen de técnicas como la eliminación 
  de códigos muertos.

Más información:
* [Comenzar: aplicaciones web](/tutorials/web/get-started)
* [`dart compile js`](/tools/dart-compile#js)
* [herramienta `webdev`](/tools/webdev)
* [Consejos de implementación web](/web/deployment)

#### El runtime de Dart {#runtime}

Independientemente de la plataforma que uses o cómo compiles tu código, ejecutar 
el código requiere un tiempo de ejecución de Dart.
Este tiempo de ejecución es responsable de las siguientes tareas críticas:

* Gestión de la memoria:
  Dart utiliza un modelo de memoria administrada, en el que un recolector de 
  basura (GC) reclama la memoria no utilizada.

* Hacer cumplir el sistema de tipo Dart:
  Aunque la mayoría de las verificaciones de tipo en Dart son estáticas (tiempo de compilación), 
  algunas verificaciones de tipo son dinámicas (tiempo de ejecución).
  Por ejemplo, el tiempo de ejecución de Dart impone comprobaciones dinámicas mediante 
  [comprobación de tipo y operadores de conversión](/guides/language/language-tour#type-test-operators).

* Administrar [isolates](/guides/language/language-tour#isolates):
  El tiempo de ejecución de Dart controla el isolate principal 
  (donde normalmente se ejecuta el código) y cualquier otro isolate que cree la aplicación.

En las plataformas nativas, el tiempo de ejecución de Dart se incluye automáticamente dentro de los 
ejecutables autónomos y es parte de la VM de Dart proporcionada por
el comando [`dart run`](/tools/dart-run).

## Aprender Dart {#learning-dart}

Tienes muchas opciones para aprender Dart. Aquí hay algunos que recomendamos:

* [Explora Dart en el navegador]({{site.dartpad}}/) a través de DartPad,
  un entorno de ejecución basado en web para código Dart.
* [Haz un recorrido por el lenguaje Dart](/guides/language/language-tour),
  que te muestra cómo usar cada característica importante de Dart.
* [Completa un tutorial de Dart](/tutorials/server/cmdline) que
  cubre los conceptos básicos del uso de Dart para compilar para la línea de comandos.
* [Trabaja a través de una amplia capacitación en línea][udemy]
  de expertos en Dart.
* [Explora la documentación de la API]({{site.dart-api}}) que
  describe las bibliotecas principales de Dart.
* [Lee un libro sobre programación en Dart](/resources/books).

[udemy]: https://www.udemy.com/course/complete-dart-guide/?couponCode=NOV-20
