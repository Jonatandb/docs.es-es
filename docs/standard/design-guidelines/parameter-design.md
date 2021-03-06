---
title: Diseño de parámetros
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- member design guidelines [.NET Framework], parameters
- members [.NET Framework], parameters
- names [.NET Framework], parameters
- parameters, design guidelines
- reserved parameters
ms.assetid: 3f33bf46-4a7b-43b3-bb78-1ffebe0dcfa6
ms.openlocfilehash: 78eb07503810e75d14bcd73740fe429e2f73475e
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743675"
---
# <a name="parameter-design"></a>Diseño de parámetros

En esta sección se proporcionan instrucciones generales sobre el diseño de parámetros, incluidas las secciones con instrucciones para comprobar los argumentos. Además, debe consultar las directrices descritas en nomenclatura de [parámetros](../../../docs/standard/design-guidelines/naming-parameters.md).

 ✔️ usar el tipo de parámetro menos derivado que proporciona la funcionalidad requerida por el miembro.

 Por ejemplo, suponga que desea diseñar un método que enumera una colección e imprime cada elemento en la consola. Este tipo de método debe tomar <xref:System.Collections.IEnumerable> como parámetro, no <xref:System.Collections.ArrayList> ni <xref:System.Collections.IList>, por ejemplo.

 ❌ no utilizan parámetros reservados.

 Si se necesita más entrada a un miembro en alguna versión futura, se puede Agregar una nueva sobrecarga.

 ❌ no tienen métodos expuestos públicamente que tomen punteros, matrices de punteros o matrices multidimensionales como parámetros.

 Los punteros y las matrices multidimensionales son relativamente difíciles de usar correctamente. En casi todos los casos, las API se pueden rediseñar para evitar que estos tipos sean parámetros.

 ✔️ se colocan todos los parámetros de `out` que siguen a todos los parámetros por valor y `ref` (excepto las matrices de parámetros), incluso si se produce una incoherencia en el orden de los parámetros entre sobrecargas (vea [sobrecarga de miembros](../../../docs/standard/design-guidelines/member-overloading.md)).

 Los parámetros `out` se pueden ver como valores devueltos adicionales y, al agruparlos, la firma del método resulta más fácil de entender.

 ✔️ son coherentes en los parámetros de nomenclatura cuando se invalidan miembros o se implementan miembros de interfaz.

 Esto comunica mejor la relación entre los métodos.

### <a name="choose-between-enum-and-boolean-parameters"></a>Elección entre parámetros booleanos y de enumeración
 ✔️ utilizar enumeraciones si, de lo contrario, un miembro tendría dos o más parámetros booleanos.

 ❌ no use valores booleanos a menos que esté absolutamente seguro de que nunca habrá una necesidad de más de dos valores.

 Las enumeraciones ofrecen algún espacio para la adición futura de valores, pero debe tener en cuenta todas las implicaciones de agregar valores a las enumeraciones, que se describen en el [diseño de enumeraciones](../../../docs/standard/design-guidelines/enum.md).

 ✔️ considere la posibilidad de usar valores booleanos para los parámetros de constructor que son realmente de dos Estados y que simplemente se utilizan para inicializar las propiedades booleanas.

### <a name="validate-arguments"></a>Validar argumentos
 ✔️ validar los argumentos pasados a miembros públicos, protegidos o implementados explícitamente. Produce <xref:System.ArgumentException?displayProperty=nameWithType>, o una de sus subclases, si se produce un error en la validación.

 Tenga en cuenta que no es necesario que la validación real se produzca en el propio miembro público o protegido. Podría producirse en un nivel inferior en algunas rutinas internas o privadas. El punto principal es que todo el área expuesta que se expone a los usuarios finales comprueba los argumentos.

 ✔️ iniciar una <xref:System.ArgumentNullException> si se pasa un argumento NULL y el miembro no admite argumentos null.

 ✔️ validar los parámetros de enumeración.

 No asuma que los argumentos de enumeración estarán en el intervalo definido por la enumeración. CLR permite convertir cualquier valor entero en un valor de enumeración incluso si el valor no está definido en la enumeración.

 ❌ no usan <xref:System.Enum.IsDefined%2A?displayProperty=nameWithType> para las comprobaciones de intervalo de enumeración.

 ✔️ Tenga en cuenta que es posible que los argumentos mutables hayan cambiado después de que se validaran.

 Si el miembro es sensible a la seguridad, se recomienda hacer una copia y, a continuación, validar y procesar el argumento.

### <a name="pass-parameters"></a>Pasar parámetros
 Desde la perspectiva de un diseñador de Marcos, hay tres grupos principales de parámetros: por valor, parámetros `ref` y parámetros de `out`.

 Cuando un argumento se pasa a través de un parámetro por valor, el miembro recibe una copia del argumento real pasado. Si el argumento es un tipo de valor, se coloca una copia del argumento en la pila. Si el argumento es un tipo de referencia, se coloca una copia de la referencia en la pila. Los lenguajes CLR más populares, C#como, Visual Basic y C++, de forma predeterminada pasan los parámetros por valor.

 Cuando un argumento se pasa a través de un parámetro `ref`, el miembro recibe una referencia al argumento real pasado. Si el argumento es un tipo de valor, se coloca una referencia al argumento en la pila. Si el argumento es un tipo de referencia, se coloca en la pila una referencia a la referencia. `Ref` parámetros se pueden usar para permitir que el miembro modifique los argumentos pasados por el autor de la llamada.

 `Out` parámetros son similares a los parámetros de `ref`, con algunas pequeñas diferencias. El parámetro se considera inicialmente sin asignar y no se puede leer en el cuerpo del miembro antes de que se le asigne algún valor. Además, el parámetro tiene que tener asignado algún valor antes de que el miembro devuelva.

 ❌ evitar el uso de parámetros de `out` o `ref`.

 El uso de los parámetros `out` o `ref` requiere experiencia con punteros, comprender cómo difieren los tipos de valor y de referencia, y cómo controlar métodos con varios valores devueltos. Además, la diferencia entre los parámetros `out` y `ref` no se comprende ampliamente. Los arquitectos de Framework que diseñan para una audiencia general no deben esperar que los usuarios dominen el trabajo con `out` o `ref` parámetros.

 ❌ no pasan tipos de referencia por referencia.

 Hay algunas excepciones limitadas a la regla, como un método que se puede utilizar para intercambiar referencias.

### <a name="members-with-variable-number-of-parameters"></a>Miembros con número variable de parámetros
 Los miembros que pueden tomar un número variable de argumentos se expresan proporcionando un parámetro de matriz. Por ejemplo, <xref:System.String> proporciona el método siguiente:

```csharp
public class String {
    public static string Format(string format, object[] parameters);
}
```

 Después, un usuario puede llamar al método <xref:System.String.Format%2A?displayProperty=nameWithType>, como se indica a continuación:

 `String.Format("File {0} not found in {1}",new object[]{filename,directory});`

 Al agregar C# la palabra clave params a un parámetro de matriz, se cambia el parámetro a un parámetro de matriz denominado params y se proporciona un acceso directo a la creación de una matriz temporal.

```csharp
public class String {
    public static string Format(string format, params object[] parameters);
}
```

 De este modo, el usuario puede llamar al método pasando los elementos de la matriz directamente en la lista de argumentos.

 `String.Format("File {0} not found in {1}",filename,directory);`

 Tenga en cuenta que la palabra clave params solo se puede Agregar al último parámetro de la lista de parámetros.

 ✔️ considere la posibilidad de agregar la palabra clave params a los parámetros de matriz si espera que los usuarios finales pasen matrices con un número pequeño de elementos. Si se espera que se pasen muchos elementos en escenarios comunes, es probable que los usuarios no pasen estos elementos en línea de todas formas, por lo que no es necesario usar la palabra clave params.

 ❌ evitar el uso de matrices params si el llamador casi siempre tendría la entrada en una matriz.

 Por ejemplo, nunca se llamaría a los miembros con parámetros de matriz de bytes pasando bytes individuales. Por esta razón, los parámetros de matriz de bytes del .NET Framework no utilizan la palabra clave params.

 ❌ no utilice matrices params si la matriz la modifica el miembro que toma el parámetro de matriz params.

 Debido al hecho de que muchos compiladores convierten los argumentos en el miembro en una matriz temporal en el sitio de llamada, la matriz puede ser un objeto temporal y, por tanto, se perderán todas las modificaciones de la matriz.

 ✔️ considere la posibilidad de usar la palabra clave params en una sobrecarga simple, incluso si una sobrecarga más compleja no pudiera utilizarla.

 Pregúntese si los usuarios van a tener la matriz params en una sobrecarga aunque no estuvieran en todas las sobrecargas.

 ✔️ intente ordenar los parámetros para que sea posible usar la palabra clave params.

 ✔️ considere la posibilidad de proporcionar sobrecargas especiales y rutas de acceso de código para las llamadas con un número pequeño de argumentos en las API con un rendimiento muy importante.

 Esto permite evitar la creación de objetos de matriz cuando se llama a la API con un pequeño número de argumentos. Formar los nombres de los parámetros tomando una forma singular del parámetro de la matriz y agregando un sufijo numérico.

 Solo debe hacerlo si va a utilizar la ruta de acceso a todo el código, no solo crea una matriz y llama al método más general.

 ✔️ Tenga en cuenta que null podría pasarse como un argumento de matriz params.

 Debe validar que la matriz no sea NULL antes del procesamiento.

 ❌ no utilizan los métodos de `varargs`, también conocidos como puntos suspensivos.

 Algunos lenguajes CLR, como C++, admiten una Convención alternativa para pasar listas de parámetros de variables llamadas `varargs` métodos. La Convención no debe usarse en marcos de trabajo, ya que no es conforme a CLS.

### <a name="pointer-parameters"></a>Parámetros de puntero
 En general, los punteros no deberían aparecer en el área expuesta pública de un marco de código administrado bien diseñado. La mayoría de las veces, se deben encapsular los punteros. Sin embargo, en algunos casos, se requieren punteros por motivos de interoperabilidad y el uso de punteros en estos casos es adecuado.

 ✔️ proporcionan una alternativa para cualquier miembro que toma un argumento de puntero, ya que los punteros no son conformes a CLS.

 ❌ evitar realizar una comprobación costosa de los argumentos de puntero.

 ✔️ seguir las convenciones comunes relacionadas con los punteros al diseñar miembros con punteros.

 Por ejemplo, no es necesario pasar el índice de inicio, ya que se puede usar una aritmética de puntero simple para lograr el mismo resultado.

 *Partes © 2005, 2009 Microsoft Corporation. Todos los derechos reservados.*

 *Material reimpreso con el consentimiento de Pearson Education, Inc. y extraído de [Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) (Instrucciones de diseño de .NET Framework: convenciones, expresiones y patrones para bibliotecas .NET reutilizables, 2.ª edición), de Krzysztof Cwalina y Brad Abrams, publicado el 22 de octubre de 2008 por Addison-Wesley Professional como parte de la serie Microsoft Windows Development.*

## <a name="see-also"></a>Consulte también

- [Instrucciones de diseño de miembros](../../../docs/standard/design-guidelines/member.md)
- [Instrucciones de diseño de .NET Framework](../../../docs/standard/design-guidelines/index.md)
