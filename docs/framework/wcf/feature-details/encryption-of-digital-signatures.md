---
title: Cifrado de firmas digitales
ms.date: 03/30/2017
helpviewer_keywords:
- encryption of digital signatures [WCF]
- digital signatures [WCF], encryption
- digital signatures [WCF]
ms.assetid: 0868866d-40b4-4341-8e42-eee3b7f15b69
ms.openlocfilehash: ea53a575802f1e7903a66c2eda466c8937fb02f9
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61856420"
---
# <a name="encryption-of-digital-signatures"></a>Cifrado de firmas digitales
De forma predeterminada, un mensaje se cifra y se firma y la firma se cifra digitalmente. Puede controlar esto creando un enlace personalizado con una instancia de <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> o <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> y estableciendo la propiedad `MessageProtectionOrder` de cualquier clase en un valor de enumeración <xref:System.ServiceModel.Security.MessageProtectionOrder>. De manera predeterminada, es <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncryptAndEncryptSignature>. Este proceso tarda entre un 10 y un 40 por ciento más que la simple firma y cifrado. Deshabilitar el cifrado de la firma, sin embargo, puede permitir a un atacante adivinar el contenido del mensaje. Esto es posible porque el elemento de firma contiene el código hash del texto sin formato de cada parte del mensaje firmada. Por ejemplo, aunque se cifra el cuerpo del mensaje de forma predeterminada, la firma no cifrada contiene el código hash del cuerpo del mensaje. Si el mensaje es pequeño, un atacante podría ser capaz de deducir el contenido. Cifrar la firma reduce o elimina esta posibilidad.  
  
 Por consiguiente, solo deshabilite el cifrado de la firma cuando el valor del contenido sea bajo y el aumento de rendimiento significativo, por ejemplo, al enviar archivos binarios de gran tamaño que no tienen implicaciones de seguridad.  
  
### <a name="to-disable-digital-signing"></a>Para deshabilitar la firma digital  
  
1. Creará un control <xref:System.ServiceModel.Channels.CustomBinding>. Para obtener más información, vea [Cómo: Crear un enlace personalizado mediante SecurityBindingElement](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md).  
  
2. Agregue <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> o <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> a la colección de enlaces.  
  
3. Establezca la propiedad <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> en <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt> o establezca la propiedad <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A?displayProperty=nameWithType> en <xref:System.ServiceModel.Security.MessageProtectionOrder.SignBeforeEncrypt>.  
  
 Para obtener más información acerca de cómo crear enlaces personalizados, consulte [crear enlaces](../../../../docs/framework/wcf/extending/creating-user-defined-bindings.md). Para obtener más información acerca de cómo crear un enlace personalizado para un modo de autenticación específicos, vea [Cómo: Crear un SecurityBindingElement para un modo de autenticación especificado](../../../../docs/framework/wcf/feature-details/how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md).  
  
## <a name="see-also"></a>Vea también

- <xref:System.ServiceModel.Security.MessageProtectionOrder>
- <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>
- [Cómo: Crear un enlace personalizado mediante SecurityBindingElement](../../../../docs/framework/wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Creación de enlaces definidos por el usuario](../../../../docs/framework/wcf/extending/creating-user-defined-bindings.md)
- [Cómo: Crear un SecurityBindingElement para un modo de autenticación especificado](../../../../docs/framework/wcf/feature-details/how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
