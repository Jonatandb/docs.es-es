---
title: Procedimiento para implementar una aplicación de integración de COM+
ms.date: 03/30/2017
ms.assetid: 2e5a0510-db3c-4988-a09c-696285836650
ms.openlocfilehash: fcf525943e6e453253c6f4d3bcfa8a1a08df6909
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61778331"
---
# <a name="how-to-deploy-a-com-integration-application"></a>Procedimiento para implementar una aplicación de integración de COM+
Cuando ha escrito una aplicación de integración de COM+, puede que desee implementarla en otro equipo. En este tema se describe cómo mover una aplicación de integración de COM+ de un equipo a otro.  
  
### <a name="moving-a-com-hosted-integration-app"></a>Mover una aplicación de integración alojada por COM+  
  
1. Asegúrese de que está instalado WCF en ambos equipos.  
  
2. Exportar la aplicación desde el equipo A.  
  
3. Importar la aplicación en el equipo B.  
  
4. Establezca el directorio raíz de la aplicación. Por convención, éste es %PROGRAMFILES%/ComPlus Applications/{AppGUID}.  
  
5. Copie los archivos Application.config y Application.manifest del directorio raíz de la aplicación en el equipo A en el directorio raíz de la aplicación en el equipo B.  
  
6. Modifique las direcciones del punto de conexión de servicio en el archivo Application.config en el equipo B para identificar el equipo adecuado. Por ejemplo, cambie `http://machineA/MyService` a `http://machineB/MyService`.  
  
### <a name="moving-a-web-hosted-integration-application"></a>Mover una aplicación de integración alojada en Web  
  
1. Asegúrese de que está instalado WCF en ambos equipos.  
  
2. Exportar la aplicación desde el equipo A.  
  
3. Importar la aplicación en el equipo B.  
  
4. Cree un vroot de IIS en el equipo B.  
  
5. Copie el archivo .svc (componentName.svc) y el archivo Web.config de vroot en el equipo A al vroot recientemente creado en el equipo B.  
  
## <a name="see-also"></a>Vea también

- [Integración en la información general de las aplicaciones COM+](../../../../docs/framework/wcf/feature-details/integrating-with-com-plus-applications-overview.md)
- [Cómo: Configurar el servicio COM +](../../../../docs/framework/wcf/feature-details/how-to-configure-com-service-settings.md)
- [Cómo: Use la herramienta de configuración del modelo de servicio COM +](../../../../docs/framework/wcf/feature-details/how-to-use-the-com-service-model-configuration-tool.md)
