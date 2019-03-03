---
title: Simular dependencias usando Mockito
short-title: Mocking
prev:
  title: Introducción a las pruebas unitarias
  path: /docs/cookbook/testing/unit/introduction
next:
  title: Introducción a las pruebas de Widgets
  path: /docs/cookbook/testing/widget/introduction
---

En ciertos casos, las pruebas unitarias pueden depender de las clases que obtienen datos 
de servicios web en vivo o bases de datos. Esto es inconveniente por algunas razones:

  * Llamar a servicios en vivo o bases de datos ralentizará la ejecución de la prueba.
  * Una prueba de aprobación puede comenzar a fallar si un servicio web o una base de 
  datos arroja resultados inesperados. Esto se conoce como una "prueba escamosa".
  * Es difícil probar todos los posibles escenarios de éxito y falla utilizando un 
  servicio web en vivo o una base de datos.
  
Por lo tanto, en lugar de confiar en un servicio web o base de datos en linea, puedes 
"simular" esas dependencias. Los Mocks nos permiten emular un servicio web en vivo o 
una base de datos y devolver resultados específicos según la situación.

En términos generales, puedes simular dependencias creando una implementación 
alternativa de una clase. Puedes escribir estas implementaciones alternativas a 
mano o hacer uso del paquete 
[Mockito]({{site.pub-pkg}}/mockito) como atajo.

Esta receta demuestra los conceptos básicos de simular utilizando el paquete Mockito. 
Para obtener más información, por favor consulta la documentación del paquete 
[mockito]({{site.pub-pkg}}/mockito).

## Instrucciones

  1. Agrega las dependencias `mockito` y `test` 
  2. Crea una función para probar 
  3. Crea un archivo de prueba con un `http.Client` simulado
  4. Escribe una prueba para cada condición
  5. Ejecuta las pruebas

## 1. Agrega la dependencia de `mockito` 

Para utilizar el paquete `mockito` , primero debes agregarlo al archivo  
`pubspec.yaml` junto con la dependencia `flutter_test` en la sección 
`dev_dependencies`.

También usarás el paquete `http` en este ejemplo y definiremos esa 
dependencia en la sección `dependencies`.

```yaml
dependencies:
  http: <newest_version>
dev_dependencies:
  test: <newest_version>
  mockito: <newest_version>
```

## 2. Crea una función para probar

En este ejemplo, quieres probar de forma unitaria la función `fetchPost` de la receta
[Obtener datos desde internet](/docs/cookbook/networking/fetch-data/). 
Para probar esta función, necesitas hacer dos cambios:

  1. Proporciona un `http.Client` a la función. Esto te permitirá proporcionar el `http.Client` 
  correcto según la situación. Para proyectos de Flutter y del lado del servidor, puedes 
  proporcionar un `http.IOClient`. Para las aplicaciones del navegador, puedes proporcionar un 
  `http.BrowserClient`. 
  Para las pruebas, proporciona un `http.Client` simulado.
  2. Utiliza el `client` proporcionado para buscar datos de Internet, en lugar del método
  estático `http.get`, que es difícil de simular.

La función debería tener el siguiente aspecto:

<!-- skip -->
```dart
Future<Post> fetchPost(http.Client client) async {
  final response =
      await client.get('https://jsonplaceholder.typicode.com/posts/1');

  if (response.statusCode == 200) {
    // Si la llamada al servidor fue exitosa, analice el JSON
    return Post.fromJson(json.decode(response.body));
  } else {
    // Si esa llamada no fue exitosa, lance un error.
    throw Exception('Error al cargar post');
  }
}
```

## 3. Crea un archivo de prueba con un `http.Client` falso

A continuación, crea un archivo de prueba junto con una clase `MockClient` .
Siguiendo los consejos de la receta 
[Introducción a la prueba unitaria](/docs/cookbook/testing/unit/) ,
crea un archivo denominado `fetch_post_test.dart` en la carpeta raíz `test` . 

La clase `MockClient` implementa la clase `http.Client` . Esto nos permite 
pasar el `MockClient` a la función `fetchPost` , y nos permite devolver 
diferentes respuestas http en cada prueba.

<!-- skip -->
```dart
// Crea un MockClient usando la clase Mock proporcionada por el paquete Mockito
// Crea nuevas instancias de esta clase en cada prueba. 
class MockClient extends Mock implements http.Client {}

main() {
  // Las pruebas van aquí
}
``` 

## 4. Escribe una prueba para cada condición

Si piensas en la función `fetchPost` hará una de estas dos cosas:

  1. Devolver un `Post` si la llamada http tiene éxito
  2. Lanzar una `Exception` si falla la llamada http 

Por lo tanto, quieres probar estas dos condiciones. Podemos usar la clase `MockClient`
para devolver una respuesta "Ok" para la prueba de éxito y una respuesta de 
error para la prueba fallida. 

Para lograr esto, usa la función `when` proporcionada por Mockito.

<!-- skip -->
```dart
// Crea un MockClient usando la clase Mock proporcionada por el paquete Mockito.
// Crea nuevas instancias de esta clase en cada prueba.
class MockClient extends Mock implements http.Client {}

main() {
  group('fetchPost', () {
    test('returns a Post if the http call completes successfully', () async {
      final client = MockClient();

      // Usa Mockito para devolver una respuesta exitosa cuando llama al 
      // http.Client proporcionado.
      when(client.get('https://jsonplaceholder.typicode.com/posts/1'))
          .thenAnswer((_) async => http.Response('{"title": "Test"}', 200));

      expect(await fetchPost(client), isInstanceOf<Post>());
    });

    test('throws an exception if the http call completes with an error', () {
      final client = MockClient();

      // Usa Mockito para devolver una respuesta fallida cuando llama al 
      // http.Client proporcionado.
      when(client.get('https://jsonplaceholder.typicode.com/posts/1'))
          .thenAnswer((_) async => http.Response('Not Found', 404));

      expect(fetchPost(client), throwsException);
    });
  });
}
```

### 5. Ejecuta las pruebas

Ahora que tienes una función `fetchPost` con pruebas en su lugar, 
ejecuta las pruebas. 

```terminal
$ dart test/fetch_post_test.dart
```

También puedes ejecutar pruebas dentro de tu editor favorito siguiendo 
las instrucciones en 
la receta 
[Introducción a las pruebas unitarias](/docs/cookbook/testing/unit#run-tests-using-intellij-or-vscode). 

### Resumen

En este ejemplo, has aprendido a usar Mockito para probar funciones o clases que 
dependen de servicios web o bases de datos. Esta es solo una breve introducción a 
la biblioteca de Mockito y al concepto de simulación. Para obtener más información, 
consulte la documentación provista por el 
[paquete Mockito]({{site.pub-pkg}}/mockito).  
