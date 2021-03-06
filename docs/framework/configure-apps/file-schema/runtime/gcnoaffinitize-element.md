---
title: Elemento GCNoAffinitize
ms.date: 11/08/2019
helpviewer_keywords:
- gcNoAffinitize element
- <gcNoAffinitize> element
ms.openlocfilehash: 4031ff19131c905072696837d1622dbb6e54ae61
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73978418"
---
# <a name="gcnoaffinitize-element"></a>\<elemento > GCNoAffinitize

Especifica si establecer afinidad entre o no los subprocesos de GC del servidor con CPU.

\<Configuración > \
&nbsp;&nbsp;\<Runtime > \
&nbsp;&nbsp;&nbsp;&nbsp;\<GCNoAffinitize >

## <a name="syntax"></a>Sintaxis

```xml
<GCNoAffinitize
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Atributos y elementos

En las siguientes secciones se describen los atributos, los elementos secundarios y los elementos primarios.

### <a name="attributes"></a>Atributos

|Atributo|Descripción|
|---------------|-----------------|
|`enabled`|Atributo necesario.<br /><br />Especifica si los subprocesos o montones de GC del servidor se afinidad con con los procesadores disponibles en la máquina.|

#### <a name="enabled-attribute"></a>atributo Enabled

|Valor|Descripción|
|-----------|-----------------|
|`false`|Subprocesos de GC del servidor establece con CPU. Este es el valor predeterminado.|
|`true`|No establecer afinidad entre los subprocesos de GC de servidor con CPU.|

### <a name="child-elements"></a>Elementos secundarios

Ninguno.

### <a name="parent-elements"></a>Elementos primarios

|Elemento|Descripción|
|-------------|-----------------|
|`configuration`|Elemento raíz de cada archivo de configuración usado por las aplicaciones de Common Language Runtime y .NET Framework.|
|`runtime`|Contiene información del enlace del ensamblado y de la recolección de elementos no utilizados.|

## <a name="remarks"></a>Comentarios

De forma predeterminada, los subprocesos de GC de servidor son afinidad cons con sus respectivas CPU. Cada uno de los procesadores disponibles del sistema tiene su propio subproceso y montón de GC. Esta suele ser la configuración preferida, ya que optimiza el uso de la memoria caché. A partir de .NET Framework 4.6.2, al establecer el atributo de `enabled` del elemento **GCNoAffinitize** en `false`, puede especificar que los subprocesos de GC del servidor y las CPU no estén estrechamente acoplados.

Puede especificar el elemento de configuración **GCNoAffinitize** solo para los subprocesos de GC del servidor de establecer afinidad entre con CPU. También puede utilizarlo junto con el elemento [GCHeapCount](gcheapcount-element.md) para controlar el número de subprocesos y montones de GC utilizados por una aplicación.

Si el `enabled` atributo del elemento **GCNoAffinitize** es `false` (su valor predeterminado), también puede usar el elemento [GCHeapCount](gcheapcount-element.md) para especificar el número de subprocesos y montones de GC, junto con el elemento [GCHeapAffinitizeMask](gcheapaffinitizemask-element.md) para especificar los procesadores en los que se afinidad con los montones y subprocesos de GC.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente no se establecer afinidad entre los subprocesos de GC del servidor:

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

En el ejemplo siguiente no se establecer afinidad entre los subprocesos de GC del servidor y se limita el número de subprocesos o montones de GC a 10:

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCNoAffinitize enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Vea también

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [Elemento GCHeapAffinitizeMask](gcheapaffinitizemask-element.md)
- [Elemento GCHeapCount](gcheapcount-element.md)
- [Fundamentos de la recolección de elementos no utilizados](../../../../standard/garbage-collection/fundamentals.md)
- [Esquema de la configuración de Common Language Runtime](index.md)
- [Esquema de los archivos de configuración](../index.md)
