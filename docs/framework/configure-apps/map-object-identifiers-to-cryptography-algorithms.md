---
title: Asignar identificadores de objeto a algoritmos de criptografía
ms.date: 03/30/2017
helpviewer_keywords:
- digital signatures
- identifiers, mapping object identifiers
- cryptographic algorithms
- mapping object identifiers
- cryptography, mapping object identifiers
ms.assetid: c9673f81-bf9e-47fd-bc6f-6bc1c1c4c15e
ms.openlocfilehash: a5aebac2d392d4540581dfe7c7afff0819968ac0
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69912541"
---
# <a name="mapping-object-identifiers-to-cryptography-algorithms"></a>Asignar identificadores de objeto a algoritmos de criptografía
Las firmas digitales garantizan que no se manipulen los datos cuando se envían de un programa a otro. Normalmente, la firma digital se calcula aplicando una función matemática al hash de los datos que se van a firmar. Al dar formato a un valor hash que se va a firmar, algunos algoritmos de firma digital anexan un identificador de objeto (OID) ASN. 1 como parte de la operación de formato. El OID identifica el algoritmo que se usó para calcular el hash. Puede asignar algoritmos a identificadores de objeto para extender el mecanismo de criptografía para usar algoritmos personalizados. En el ejemplo siguiente se muestra cómo asignar un identificador de objeto a un nuevo algoritmo hash.  
  
```xml  
<configuration>  
   <mscorlib>  
      <cryptographySettings>  
         <cryptoNameMapping>  
            <cryptoClasses>  
               <cryptoClass MyNewHash="MyNewHashClass, MyAssembly  
                  Culture='en', PublicKeyToken=a5d015c7d5a0b012,  
                  Version=1.0.0.0"/>  
            </cryptoClasses>  
            <nameEntry name="NewHash" class="MyNewHash"/>  
         </cryptoNameMapping>  
         <oidMap>  
            <oidEntry OID="1.3.14.33.42.46"  name="NewHash"/>  
         </oidMap>  
      </cryptographySettings>  
   </mscorlib>  
</configuration>  
```  
  
 [ El\<elemento > oidEntry](./file-schema/cryptography/oidentry-element.md) contiene dos atributos. El atributo **OID** es el número del identificador de objeto. El atributo **Name** es el valor del [ \<](./file-schema/cryptography/nameentry-element.md)atributo name del elemento > de elemento nameEntry. Debe haber una asignación desde un nombre de algoritmo a una clase antes de que un identificador de objeto pueda asignarse a un nombre simple.  
  
## <a name="see-also"></a>Vea también

- [Configurar clases de criptografía](configure-cryptography-classes.md)
- [Cryptographic Services](../../standard/security/cryptographic-services.md)
