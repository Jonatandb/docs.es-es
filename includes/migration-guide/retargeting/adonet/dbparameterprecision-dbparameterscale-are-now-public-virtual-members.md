---
ms.openlocfilehash: 1721d32f8cdc9b6ea4b4732e38afa56a8a532600
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59234819"
---
### <a name="dbparameterprecision-and-dbparameterscale-are-now-public-virtual-members"></a>DbParameter.Precision y DbParameter.Scale son ahora miembros virtuales públicos

|   |   |
|---|---|
|Detalles|<xref:System.Data.Common.DbParameter.Precision> y <xref:System.Data.Common.DbParameter.Scale> se implementan como propiedades públicas virtuales. Reemplazan las implementaciones de interfaz explícitas correspondientes, <xref:System.Data.Common.DbParameter.System%23Data%23IDbDataParameter%23Precision> y <xref:System.Data.Common.DbParameter.System%23Data%23IDbDataParameter%23Scale>.|
|Sugerencia|Al volver a compilar un proveedor de base de datos de ADO.NET, estas diferencias necesitarán que se aplique la palabra clave "override" a las propiedades Scale y Precision. Esto solo es necesario al volver a compilar los componentes; los archivos binarios existentes continuarán funcionando.|
|Ámbito|Secundaria|
|Versión|4.5.1|
|Tipo|Redestinación|
|API afectadas|<ul><li><xref:System.Data.Common.DbParameter.Precision?displayProperty=nameWithType></li><li><xref:System.Data.Common.DbParameter.Scale?displayProperty=nameWithType></li></ul>|
