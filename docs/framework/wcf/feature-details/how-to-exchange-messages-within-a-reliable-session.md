---
title: Intercambio de mensajes dentro de una sesión confiable
ms.date: 03/30/2017
ms.assetid: 87cd0e75-dd2c-44c1-8da0-7b494bbdeaea
ms.openlocfilehash: 58a392fc6295e82f41e08c80a3343b4059afad7e
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141679"
---
# <a name="how-to-exchange-messages-within-a-reliable-session"></a>Intercambio de mensajes dentro de una sesión confiable

En este tema se describen los pasos necesarios para habilitar una sesión confiable utilizando uno de los enlaces proporcionados por el sistema que admiten este tipo de sesión, pero no de forma predeterminada. Habilita una sesión confiable de manera imperativa mediante código o mediante declaración en el archivo de configuración. Este procedimiento utiliza los archivos de configuración de servicio y cliente para habilitar la sesión confiable y estipular que los mensajes lleguen en el mismo orden en el que se enviaron.

La parte clave de este procedimiento es que el elemento de configuración de extremo contiene un `bindingConfiguration` atributo que hace referencia a una configuración de enlace denominada `Binding1`. El elemento de configuración de [**enlace de\<** ](../../configure-apps/file-schema/wcf/bindings.md) hace referencia a este nombre para habilitar sesiones confiables estableciendo el atributo `enabled` del elemento [ **\<reliableSession >** ](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731302(v=vs.100)) en `true`. Especifica las garantías de entrega ordenada de la sesión confiable estableciendo el atributo `ordered` en `true`.

Para la copia de origen de este ejemplo, consulte [WS Reliable Session](../../../../docs/framework/wcf/samples/ws-reliable-session.md).

### <a name="configure-the-service-with-a-wshttpbinding-to-use-a-reliable-session"></a>Configurar el servicio con un WSHttpBinding para utilizar una sesión confiable

1. Defina un contrato de servicios para el tipo de servicio.

   [!code-csharp[c_HowTo_UseReliableSession#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/service.cs#1121)]

1. Implemente el contrato de servicios en una clase de servicio. Tenga en cuenta que la información de dirección o enlace no se especifica dentro de la implementación del servicio. No es necesario escribir código para recuperar la dirección o la información de enlace del archivo de configuración.

   [!code-csharp[c_HowTo_UseReliableSession#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/service.cs#1122)]

1. Cree un archivo *Web. config* para configurar un extremo para el `CalculatorService` que utiliza el <xref:System.ServiceModel.WSHttpBinding> con sesión confiable habilitada y entrega ordenada de los mensajes necesarios.

   [!code-xml[c_HowTo_UseReliableSession#2111](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/common/web.config#2111)]

1. Cree un archivo *Service. SVC* que contenga la línea:

   ```
   <%@ServiceHost language=c# Service="CalculatorService" %>
   ```

1. Coloque el archivo *Service. SVC* en el directorio virtual de Internet Information Services (IIS).

### <a name="configure-the-client-with-a-wshttpbinding-to-use-a-reliable-session"></a>Configurar el cliente con un WSHttpBinding para utilizar una sesión confiable

1. Use la [herramienta de utilidad de metadatos de ServiceModel (*SvcUtil. exe*)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) desde la línea de comandos para generar código a partir de los metadatos de servicio:

   ```console
   Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
   ```

1. El cliente generado contiene la interfaz `ICalculator` que define el contrato de servicio que la implementación del cliente debe cumplir.

   [!code-csharp[C_HowTo_UseReliableSession#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1221)]

1. La aplicación de cliente generada también contiene la implementación de `ClientCalculator`. Tenga en cuenta que la dirección y la información de enlace no se especifican en ningún lugar dentro de la implementación del servicio. No es necesario escribir código para recuperar la dirección o la información de enlace del archivo de configuración.

   [!code-csharp[C_HowTo_UseReliableSession#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1222)]

1. *SvcUtil. exe* también genera la configuración para el cliente que utiliza la clase <xref:System.ServiceModel.WSHttpBinding>. Asigne un nombre al archivo de configuración *app. config* al usar Visual Studio.

   [!code-xml[C_HowTo_UseReliableSession#2211](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/common/app.config#2211)]

1. Cree una instancia de la `ClientCalculator` en una aplicación y llame a las operaciones del servicio.

   [!code-csharp[C_HowTo_UseReliableSession#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1223)]

1. Compile y ejecute el cliente.

## <a name="example"></a>Ejemplo

Algunos de los enlaces proporcionados por el sistema admiten de forma predeterminada las sesiones confiables. Se incluyen los siguientes:

- <xref:System.ServiceModel.WSDualHttpBinding>

- <xref:System.ServiceModel.NetNamedPipeBinding>

- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>

Para obtener un ejemplo de cómo crear un enlace personalizado que admita sesiones confiables, consulte [Cómo: crear un enlace de sesión confiable personalizado con https](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-reliable-session-binding-with-https.md).

## <a name="see-also"></a>Vea también

- [Sesiones de confianza](../../../../docs/framework/wcf/feature-details/reliable-sessions.md)
