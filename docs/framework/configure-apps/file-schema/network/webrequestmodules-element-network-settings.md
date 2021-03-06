---
title: Elemento <webRequestModules> (configuración de red)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/webRequestModules
- http://schemas.microsoft.com/.NetConfiguration/v2.0#webRequestModules
helpviewer_keywords:
- webRequestModules element
- <webRequestModules> element
ms.assetid: 1263de11-3e0a-4f94-97c9-710b2ae53817
ms.openlocfilehash: 7f2805283f89e6165d336b3e593d34054e02115d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2020
ms.locfileid: "79154548"
---
# <a name="webrequestmodules-element-network-settings"></a>\<Elemento webRequestModules> (configuración de red)
Especifica los módulos que se utilizarán para solicitar información de los hosts de red.  
  
[**\<configuración>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<system.net>**](system-net-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;\<webRequestModules>  
  
## <a name="syntax"></a>Sintaxis  
  
```xml  
<webRequestModules>
</webRequestModules>  
```  
  
## <a name="attributes-and-elements"></a>Atributos y elementos  
 En las siguientes secciones se describen los atributos, los elementos secundarios y los elementos primarios.  
  
### <a name="attributes"></a>Atributos  
 Ninguno.  
  
### <a name="child-elements"></a>Elementos secundarios  
  
|**Elemento**|**Descripción**|  
|-----------------|---------------------|  
|[agregar](add-element-for-webrequestmodules-network-settings.md)|Agrega un módulo de solicitud web personalizado a la aplicación.|  
|[Claro](clear-element-for-webrequestmodules-network-settings.md)|Quita todos los módulos de solicitud web registrados de la aplicación.|  
|[quitar](remove-element-for-webrequestmodules-network-settings.md)|Quita un módulo de solicitud web personalizado de la aplicación.|  
  
### <a name="parent-elements"></a>Elementos primarios  
  
|**Elemento**|**Descripción**|  
|-----------------|---------------------|  
|[system.net](system-net-element-network-settings.md)|Contiene valores que especifican cómo se conecta .NET Framework a la red.|  
  
## <a name="remarks"></a>Observaciones  
 El `webRequestModules` elemento registra los <xref:System.Net.WebRequest> descendientes de la clase para controlar las solicitudes de información a los hosts de red. Los módulos de <xref:System.Net.IWebRequestCreate> solicitud web deben implementar la interfaz.  
  
 .NET Framework incluye módulos de solicitudweb `http://` `https://`para `file://`URI que comienzan por , , y . Puede invalidar los módulos predeterminados solo registrando un módulo personalizado en el archivo de configuración.  
  
## <a name="configuration-files"></a>Archivos de configuración  
 Este elemento se puede usar en el archivo de configuración de la aplicación o en el archivo de configuración del equipo (Machine.config).  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se registra el módulo HTTP predeterminado. Debe reemplazar los valores de Version y PublicKeyToken por los valores correctos para el módulo especificado.  
  
```xml  
<configuration>  
  <system.net>  
    <webRequestModules>  
      <add prefix="http"  
           type="System.Net.HttpRequestCreator, System, Version=2.0.3600.0,  
           Culture=neutral, PublicKeyToken=b77a5c561934e089"  
      />  
    </webRequestModules>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte también

- <xref:System.Net.WebRequest>
- <xref:System.Net.IWebRequestCreate>
- [Esquema de configuración de red](index.md)
