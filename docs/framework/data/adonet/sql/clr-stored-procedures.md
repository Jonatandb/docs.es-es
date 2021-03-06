---
title: Procedimientos almacenados de CLR
ms.date: 03/30/2017
ms.assetid: fd7eea9b-218a-4988-8c9a-8abcc6031c66
ms.openlocfilehash: ca0fd2d33f0ad56c96fb63530e6ba3f7f7890f81
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77450367"
---
# <a name="clr-stored-procedures"></a>Procedimientos almacenados de CLR
Los procedimientos almacenados son rutinas que no pueden usarse en expresiones escalares. Pueden devolver resultados en tabla y mensajes al cliente, invocar instrucciones de lenguaje de definición de datos (DDL) y lenguaje de manipulación de datos (DML) y devolver parámetros de salida.  
  
> [!NOTE]
> Microsoft Visual Basic no admite parámetros de salida de la misma forma que Microsoft Visual C#. Debe especificar para pasar el parámetro por referencia y aplicar el atributo \<out () > para representar un parámetro de salida, como en el siguiente procedimiento:  
  
```vb
Public Shared Sub ExecuteToClient( <Out()> ByRef number As Integer)  
```
  
Para obtener información más detallada, consulte la versión de [SQL Server documentación](/sql) de la versión de SQL Server que esté usando.
  
 **Documentación de SQL Server**

1. [Procedimientos almacenados de CLR](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/ms131094(v=sql.100))  
  
## <a name="see-also"></a>Consulte también

- [Crear SQL Server objetos 2005 en código administrado](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/6s0s2at1(v=vs.90))
- [Información general sobre ADO.NET](../ado-net-overview.md)
