---
title: -quiet
ms.date: 07/20/2015
f1_keywords:
- -quiet
- quiet
helpviewer_keywords:
- -quiet compiler option [Visual Basic]
- /quiet compiler option [Visual Basic]
- quiet compiler option [Visual Basic]
ms.assetid: 5d77fa23-4c50-4708-8535-649912b098e8
ms.openlocfilehash: 6e773c60469e8426956c92a5aa377741ba5af4d3
ms.sourcegitcommit: eff6adb61852369ab690f3f047818c90580e7eb1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005285"
---
# <a name="-quiet"></a>-quiet

Evita que el compilador muestre código de errores y advertencias relacionados con la sintaxis.

## <a name="syntax"></a>Sintaxis

```console
-quiet
```

## <a name="remarks"></a>Comentarios

De forma predeterminada, `-quiet` no está en vigor. Cuando el compilador informa de un error o una advertencia relacionados con la sintaxis, también genera la línea del código fuente. En el caso de las aplicaciones que analizan los resultados del compilador, puede ser más conveniente que el compilador solo genere el texto del diagnóstico.

En el ejemplo siguiente, `Module1` genera un error que incluye el código fuente cuando se compila sin `-quiet`.

```vb
Module Module1
    Sub Main()
        x()
    End Sub
End Module
```

Resultado:

```console
C:\projects\vb2.vb(3) : error BC30451: 'x' is not declared. It may be inaccessible due to its protection level.

        x()
        ~
```

Compilada con `-quiet`, el compilador solo genera lo siguiente:

```console
E:\test\t2.vb(3) : error BC30451: Name 'x' is not declared.
```

> [!NOTE]
> La opción `-quiet` no está disponible en el entorno de desarrollo de Visual Studio; solo está disponible al compilar desde la línea de comandos.

## <a name="example"></a>Ejemplo

En el código siguiente se compila `T2.vb` y no se muestra el código para el diagnóstico del compilador relacionado con la sintaxis:

```console
vbc -quiet t2.vb
```

## <a name="see-also"></a>Vea también

- [Compilador de línea de comandos de Visual Basic](../../../visual-basic/reference/command-line-compiler/index.md)
- [Líneas de comandos de compilación de ejemplo](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
