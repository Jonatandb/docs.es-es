---
title: <namespaceTable>
ms.date: 03/30/2017
ms.assetid: 64801766-01b7-4c65-9ce6-70ad5af67689
ms.openlocfilehash: aefe7beec7335d80341e670961800907c2bd0200
ms.sourcegitcommit: 205b9a204742e9c77256d43ac9d94c3f82909808
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70855102"
---
# <a name="namespacetable"></a>\<namespaceTable>

Representa una sección de configuración para definir un conjunto de elementos que contienen el espacio de nombres para prefijar asignaciones que se pueden utilizar a continuación en filtros XPath para el enrutamiento.

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<> System. serviceModel**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<> de enrutamiento**](routing.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<> namespaceTable**  
  
## <a name="syntax"></a>Sintaxis  
  
```xml  
<system.serviceModel>
  <routing>
    <namespaceTable>
      <add namespace="String"
           prefix="String" />
    </namespaceTable>
  </routing>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos y elementos

En las siguientes secciones se describen los atributos, los elementos secundarios y los elementos primarios.

### <a name="attributes"></a>Atributos

None

### <a name="child-elements"></a>Elementos secundarios

|     | DESCRIPCIÓN |
| --- | ----------- |
| [ **\<filter>** ](filter.md) | Define una asignación de prefijo de espacio de nombres usado para expresiones XPath. |

### <a name="parent-elements"></a>Elementos primarios

|     | DESCRIPCIÓN |
| --- | ----------- |
| [ **\<> de enrutamiento**](routing.md) | Representa una sección de configuración para definir un conjunto de filtros de enrutamiento, que determinan el tipo de<xref:System.ServiceModel.Dispatcher.MessageFilter> Windows Communication Foundation (WCF) que se va a usar al evaluar los mensajes entrantes, así como las tablas de enrutamiento que definen los extremos de destino. enviar mensajes a cuando coincida un filtro. |

## <a name="see-also"></a>Vea también

- <xref:System.ServiceModel.Routing.Configuration.NamespaceElementCollection?displayProperty=nameWithType>
