---
title: Información general de global.json
description: Obtenga información sobre cómo usar el archivo global.json para establecer la versión del SDK de .NET Core al ejecutar comandos de la CLI de .NET Core.
ms.date: 01/14/2020
ms.custom: updateeachrelease
ms.openlocfilehash: 70257566e1ff30f5c97212a5e0e3c308c27738b7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2020
ms.locfileid: "77626000"
---
# <a name="globaljson-overview"></a>Información general de global.json

**Este artículo se aplica a:** ✔️ SDK de .NET Core 2.0 y versiones posteriores

El archivo *global.json* permite definir qué versión del SDK de .NET Core se usa al ejecutar comandos de la CLI de .NET Core. La selección del SDK de .NET Core es independiente de especificar el runtime al que está destinado el proyecto. La versión del SDK de .NET Core indica qué versiones de la CLI de .NET Core se usan.

En general, se quiere usar la versión más reciente de las herramientas del SDK, por lo que no es necesario ningún archivo *global.json*. En algunos escenarios avanzados, es posible que quiera controlar la versión de las herramientas del SDK, así que en este artículo se explica cómo.

Para obtener más información sobre cómo especificar el runtime en su lugar, vea [Plataformas de destino](../../standard/frameworks.md).

El SDK de .NET Core busca un archivo *global.json* en el directorio de trabajo actual (que no es necesariamente el mismo que el directorio del proyecto) o en uno de sus directorios principales.

## <a name="globaljson-schema"></a>Esquema de global.JSON

### <a name="sdk"></a>sdk

Tipo: `object`

Especifica información sobre el SDK de .NET Core que se va a seleccionar.

#### <a name="version"></a>version

- Tipo: `string`

- Disponible a partir del SDK de .NET Core 1.0.

La versión del SDK de .NET Core que se va a usar.

Este campo:

- No admite caracteres comodín, es decir, se debe especificar el número de versión completo.
- No admite intervalos de versiones.

#### <a name="allowprerelease"></a>allowPrerelease

- Tipo: `boolean`

- Disponible a partir del SDK de .NET Core 3.0.

Indica si la resolución del SDK debe tener en cuenta las versiones preliminares a la hora de seleccionar la versión del SDK que se va a usar.

Si no se establece este valor de forma explícita, el valor predeterminado depende de si se está ejecutando desde Visual Studio:

- Si **no** se está en Visual Studio, el valor predeterminado es `true`.
- Si se está en Visual Studio, se usa el estado de versión preliminar solicitado. Es decir, si se usa una versión preliminar de Visual Studio o se establece la opción **Usar versiones preliminares del SDK de .NET Core** (en **Herramientas** > **Opciones** > **Entorno** > **Características en versión preliminar**), el valor predeterminado es `true`; de lo contrario, `false`.

#### <a name="rollforward"></a>rollForward

- Tipo: `string`

- Disponible a partir del SDK de .NET Core 3.0.

Directiva de puesta al día que se va a usar al seleccionar una versión del SDK, ya sea como reserva si falta una versión específica del SDK o como una directiva para usar una versión superior. Se debe especificar una [versión](#version) con un valor `rollForward`, a menos que se esté estableciendo en `latestMajor`.

Para comprender las directivas disponibles y su comportamiento, tenga en cuenta las siguientes definiciones de una versión del SDK en el formato `x.y.znn`:

- `x` es la versión principal.
- `y` es la versión secundaria.
- `z` es la banda de características.
- `nn` es la versión de la revisión.

En la siguiente tabla se muestran los posibles valores de la clave `rollForward`:

| Valor         | Comportamiento |
| ------------- | ---------- |
| `patch`       | Usa la versión especificada. <br> Si no se encuentra, se pone al día hasta el nivel de revisión más reciente. <br> Si no se encuentra, se produce un error. <br><br> Este valor es el comportamiento heredado de las versiones anteriores del SDK. |
| `feature`     | Usa el nivel de revisión más reciente para las versiones principal y secundaria y la banda de características especificadas. <br> Si no se encuentra, se pone al día hasta la siguiente banda de características superior dentro de la misma versión principal/secundaria y usa el nivel de revisión más reciente para esa banda de características. <br> Si no se encuentra, se produce un error. |
| `minor`       | Usa el nivel de revisión más reciente para las versiones principal y secundaria y la banda de características especificadas. <br> Si no se encuentra, se pone al día hasta la siguiente banda de características superior dentro de la misma versión principal/secundaria y usa el nivel de revisión más reciente para esa banda de características. <br> Si no se encuentra, se pone al día hasta la siguiente versión secundaria y banda de características superiores dentro de la misma versión principal y usa el nivel de revisión más reciente para esa banda de características. <br> Si no se encuentra, se produce un error. |
| `major`       | Usa el nivel de revisión más reciente para las versiones principal y secundaria y la banda de características especificadas. <br> Si no se encuentra, se pone al día hasta la siguiente banda de características superior dentro de la misma versión principal/secundaria y usa el nivel de revisión más reciente para esa banda de características. <br> Si no se encuentra, se pone al día hasta la siguiente versión secundaria y banda de características superiores dentro de la misma versión principal y usa el nivel de revisión más reciente para esa banda de características. <br> Si no se encuentra, se pone al día hasta la siguiente versión principal, secundaria y banda de características superiores y usa el nivel de revisión más reciente para esa banda de características. <br> Si no se encuentra, se produce un error. |
| `latestPatch` | Usa el nivel de revisión instalado más reciente que coincide con la versión principal, secundaria y banda de características solicitadas con un nivel de revisión y que es mayor o igual que el valor especificado. <br> Si no se encuentra, se produce un error. |
| `latestFeature` | Usa la banda de características instalada superior y el nivel de revisión que coincide con la versión principal y secundaria solicitadas con una banda de características que es mayor o igual que el valor especificado. <br> Si no se encuentra, se produce un error. |
| `latestMinor` | Usa la versión secundaria y la banda de características instaladas superiores y el nivel de revisión que coincide con la versión principal solicitada con una versión secundaria que es mayor o igual que el valor especificado. <br> Si no se encuentra, se produce un error. |
| `latestMajor` | Usa el SDK de .NET Core instalado superior con una versión principal que es mayor o igual que el valor especificado. <br> Si no se encuentra, se produce un error. |
| `disable`     | No se pone al día. Se requiere una coincidencia exacta. |

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo no usar versiones preliminares:

```json
{
  "sdk": {
    "allowPrerelease": false
  }
}
```

En el ejemplo siguiente se muestra cómo usar la versión superior instalada que es mayor o igual que la versión especificada:

```json
{
  "sdk": {
    "version": "3.1.100",
    "rollForward": "latestMajor"
  }
}
```

En el ejemplo siguiente se muestra cómo usar la versión exacta especificada:

```json
{
  "sdk": {
    "version": "3.1.100",
    "rollForward": "disable"
  }
}
```

En el ejemplo siguiente se muestra cómo usar la versión de revisión superior instalada de una versión específica (con el formato 3.1.1xx):

```json
{
  "sdk": {
    "version": "3.1.100",
    "rollForward": "latestPatch"
  }
}
```

## <a name="globaljson-and-the-net-core-cli"></a>global.json y la CLI de .NET Core

Resulta útil saber qué versiones del SDK están instaladas en el equipo con el fin de establecer una en el archivo *global.json*. Para obtener más información sobre cómo hacerlo, vea [Cómo comprobar que .NET Core ya está instalado](../install/how-to-detect-installed-versions.md#check-sdk-versions).

Para instalar otras versiones del SDK de .NET Core en el equipo, visite la página de [descargas de .NET Core](https://dotnet.microsoft.com/download/dotnet-core).

Puede crear un archivo *global.json* en el directorio actual mediante la ejecución del comando [dotnet new](dotnet-new.md), similar al ejemplo siguiente:

```dotnetcli
dotnet new globaljson --sdk-version 3.0.100
```

## <a name="matching-rules"></a>Reglas de coincidencia

> [!NOTE]
> Las reglas de coincidencia se rigen por el punto de entrada de `dotnet.exe`, que es común en todos los runtime instalados de .NET Core. Las reglas de coincidencia de la última versión instalada del runtime de .NET Core se usan cuando se tienen varios runtime instalados en paralelo.

## <a name="net-core-3x"></a>[.NET Core 3.x](#tab/netcore3x)

A partir de .NET Core 3.0, se aplican las reglas siguientes al determinar qué versión del SDK se va a usar:

- Si no se encuentra ningún archivo *global.json* o en *global.json* no se especifica una versión del SDK ni un valor `allowPrerelease`, se usa la versión del SDK instalada más reciente (lo que equivale a establecer `rollForward` en `latestMajor`). El que se consideren o no las versiones preliminares del SDK depende de cómo se invoque a `dotnet`.
  - Si **no** se está en Visual Studio, se tienen en cuenta las versiones preliminares.
  - Si se está en Visual Studio, se usa el estado de versión preliminar solicitado. Es decir, si se usa una versión preliminar de Visual Studio o se establece la opción **Usar versiones preliminares del SDK de .NET Core** (en **Herramientas** > **Opciones** > **Entorno** > **Características en versión preliminar**), se tienen en cuenta las versiones preliminares; de lo contrario, solo se consideran las versiones publicadas.
- Si se encuentra un archivo *global.json* que no especifica una versión del SDK pero sí un valor `allowPrerelease`, se usa la versión del SDK instalada superior (lo que equivale a establecer `rollForward` en `latestMajor`). El que la versión más reciente del SDK pueda ser publicada o preliminar, depende del valor de `allowPrerelease`. `true` indica que se tienen en cuenta las versiones preliminares; `false` indica que solo se tienen en cuenta las versiones publicadas.
- Si se encuentra un archivo *global.json* y especifica una versión del SDK:

  - Si no hay ningún valor `rollFoward` establecido, se usa `latestPatch` como directiva de `rollForward` predeterminada. De lo contrario, vea cada valor y su comportamiento en la sección [rollForward](#rollforward).
  - En la sección [allowPrerelease](#allowprerelease) se explica si se tienen en cuenta las versiones preliminares y cuál es el comportamiento predeterminado cuando `allowPrerelease` no está establecido.

## <a name="net-core-2x"></a>[.NET Core 2.x](#tab/netcore2x)

En el SDK de .NET Core 2.x, se aplican las reglas siguientes a la hora de determinar qué versión del SDK se va a usar:

- Si no se encuentra ningún archivo *global.json* o en *global.json* no se especifica una versión del SDK, se usa la versión instalada del SDK más reciente. La versión del SDK más reciente puede ser la publicada o la preliminar: prevalece el número de versión más alto.
- Si *global.json* especifica una versión del SDK:
  - Si la versión del SDK especificada se encuentra en el equipo, se usa esa versión exacta.
  - Si la versión del SDK especificada no se encuentra en el equipo, se usa la **versión de revisión** del SDK instalada más reciente de esa versión. La **versión de revisión** del SDK instalada más reciente puede ser la publicada o la preliminar: prevalece el número de versión más alto. En .NET Core 2.1 y versiones posteriores, las **versiones de revisión** inferiores a la **versión de revisión** especificada se omiten en la selección del SDK.
  - Si no se encuentra la versión del SDK especificada y una **versión de revisión** adecuada del SDK, se produce un error.

La versión del SDK se compone de los elementos siguientes:

`[.NET Core major version].[.NET Core minor version].[xyz][-optional preview name]`

La **versión de características** del SDK de .NET Core se representa por medio del primer dígito (`x`) de la última parte del número (`xyz`) para las versiones 2.1.100 del SDK y posteriores. En general, el SDK de .NET Core tiene un ciclo de versiones más rápido que .NET Core.

La **versión de revisión** se define mediante los dos últimos dígitos (`yz`) de la última parte del número (`xyz`) para las versiones 2.1.100 del SDK y posteriores. Por ejemplo, si especifica `2.1.300` como la versión del SDK, la selección del SDK busca hasta `2.1.399` pero `2.1.400` no se considera una versión de revisión para `2.1.300`.

Las versiones del SDK de .NET Core de `2.1.100` a `2.1.201` se publicaron durante la transición entre las combinaciones de número de versión y no procesan correctamente la notación `xyz`. Si estas versiones se especifican en el archivo *global.json*, se recomienda encarecidamente asegurarse de que las versiones especificadas están en los equipos de destino.

---

## <a name="troubleshoot-build-warnings"></a>Solución de problemas de advertencias de compilación

* La advertencia siguiente indica que el proyecto se ha compilado con una versión preliminar del SDK de .NET Core:

  > Trabaja con una versión preliminar del SDK de .NET Core. Puede definir la versión del SDK a través de un archivo global.json en el proyecto actual. Más información en <https://go.microsoft.com/fwlink/?linkid=869452>.

  Las versiones del SDK de .NET Core tienen un historial y el compromiso de ser de alta calidad. Pero si no quiere usar una versión preliminar, vea las distintas estrategias que puede aplicar con el SDK de .NET Core 3.0 o una versión posterior en la sección [allowPrerelease](#allowprerelease). En el caso de los equipos que nunca han tenido instalado un SDK o un runtime de .NET Core 3.0 o superior, debe crear un archivo *global.json* y especificar la versión exacta que quiere usar.

* La advertencia siguiente indica que el proyecto tiene como destino EF Core 1.0 ó 1.1, que no es compatible con el SDK de .NET Core 2.1 y versiones posteriores:

  > El proyecto de inicio "{proyectoDeInicio}" está destinado a la versión "{versiónPlataformaDeDestino}" de ".NETCoreApp". Esta versión de las herramientas de línea de comandos de Entity Framework Core .NET solo admite la versión 2.0 o superior. Para obtener información sobre el uso de versiones anteriores de las herramientas, vea <https://go.microsoft.com/fwlink/?linkid=871254>.

  A partir del SDK de .NET Core 2.1 (versión 2.1.300), el comando `dotnet ef` se incluye en el SDK. Para compilar el proyecto, instale el SDK de .NET Core 2.0 (versión 2.1.201) o versiones anteriores en el equipo y defina la versión del SDK deseada mediante el archivo *global.json*. Para obtener más información sobre el comando `dotnet ef`, vea [EF Core .NET Command-line Tools](/ef/core/miscellaneous/cli/dotnet) (Herramientas de línea de comandos de EF Core .NET).

## <a name="see-also"></a>Vea también

- [Resolución de los SDK de proyecto](/visualstudio/msbuild/how-to-use-project-sdk#how-project-sdks-are-resolved)
