---
title: 'Mitigación: Método X509CertificateClaimSet.FindClaims'
ms.date: 03/30/2017
ms.assetid: ee356e3b-f932-48f5-875a-5e42340bee63
ms.openlocfilehash: f94a5f685a5aa94332bf2e15e5d5eb0def02d7ef
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398668"
---
# <a name="mitigation-x509certificateclaimsetfindclaims-method"></a>Mitigación: Método X509CertificateClaimSet.FindClaims
A partir de las aplicaciones que tienen como destino .NET Framework 4.6.1, el método <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=nameWithType> intentará hacer coincidir el argumento `claimType` con todas las entradas DNS de su campo de SAN.  
  
## <a name="impact"></a>Impacto  
 Este cambio solo afecta a aplicaciones que tienen como destino versiones de .NET Framework a partir de .NET Framework 4.6.1.  
  
 Para aquellas aplicaciones destinadas a versiones anteriores de .NET Framework, el método <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=nameWithType> intenta hacer coincidir el argumento `claimType` solo con la última entrada DNS.  
  
## <a name="mitigation"></a>Mitigación  
 Si no quiere este cambio, las aplicaciones que tienen como destino versiones de .NET Framework a partir de .NET Framework 4.6.1 pueden optar por no recibir agregando la siguiente opción de configuración en la sección [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) del archivo de configuración de la aplicación:  
  
```xml  
<runtime>  
   <AppContextSwitchOverrides value="Switch.System.IdentityModel.DisableMultipleDNSEntriesInSANCertificate=true" />
</runtime>  
```  
  
 Además, las aplicaciones que tienen como destino versiones anteriores de .NET Framework, pero que se ejecutan en .NET Framework 4.6.1 y versiones posteriores, pueden optar por recibir en este comportamiento agregando el siguiente ajuste de configuración a la sección [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) del archivo de configuración de la aplicación:  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IdentityModel.DisableMultipleDNSEntriesInSANCertificate=false" />
</runtime>  
```  
  
## <a name="see-also"></a>Vea también

- [Compatibilidad de aplicaciones](application-compatibility.md)
