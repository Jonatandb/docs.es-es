---
title: Nombres de contratos de datos
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], naming
ms.assetid: 31f87e6c-247b-48f5-8e94-b9e1e33d8d09
ms.openlocfilehash: 16a42a2808104a77e56e93564a679dfc578e73f6
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61857296"
---
# <a name="data-contract-names"></a>Nombres de contratos de datos

A veces un cliente y un servicio no comparten los mismos tipos. Todavía pueden pasar datos entre sí porque los contratos de datos son equivalentes en ambos lados. [Equivalencia del contrato de datos](data-contract-equivalence.md) se basa en el contrato de datos y nombres de miembro de datos, y por lo tanto, se proporciona un mecanismo para asignar tipos y miembros a esos nombres. En este tema se explica las reglas para asignar nombres a los contratos de datos, así como el comportamiento predeterminado de la infraestructura de Windows Communication Foundation (WCF) al crear nombres.

## <a name="basic-rules"></a>Reglas básicas
Entre las reglas básicas con respecto a los nombres de los contratos de datos se incluyen:

- Un nombre de contrato de datos completo se compone de un espacio de nombres y de un nombre.

- Los miembros de datos solo tienen nombres, pero no espacios de nombres.

- Cuando se procesan los contratos de datos, la infraestructura de WCF distingue mayúsculas de minúsculas para los espacios de nombres y los nombres de contratos de datos y los miembros de datos.

## <a name="data-contract-namespaces"></a>Espacios de nombres de contratos de datos
Un espacio de nombres de contrato de datos toma la forma de un Identificador uniforme de recursos (URI). El URI puede ser absoluto o relativo. De forma predeterminada, se asigna un espacio de nombres que procede del espacio de nombres de Common Language Runtime (CLR) de ese tipo a los contratos de datos de un tipo determinado.

De forma predeterminada, cualquier espacio de nombres CLR (con el formato *Clr.Namespace*) se asigna al espacio de nombres `http://schemas.datacontract.org/2004/07/Clr.Namespace`. Para invalidar este valor predeterminado, aplique el atributo <xref:System.Runtime.Serialization.ContractNamespaceAttribute> al ensamblado o módulo completo. De manera alternativa, para controlar el espacio de nombres del contrato de datos de cada tipo, establezca la propiedad <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A> del <xref:System.Runtime.Serialization.DataContractAttribute>.

> [!NOTE]
> El `http://schemas.microsoft.com/2003/10/Serialization` espacio de nombres está reservado y no se puede usar como un espacio de nombres de contrato de datos.

> [!NOTE]
> No puede invalidar el espacio de nombres predeterminado en tipos de contratos de datos que contengan declaraciones `delegate`.

## <a name="data-contract-names"></a>Nombres de contratos de datos
El nombre predeterminado de un contrato de datos de un tipo determinado es el nombre de ese tipo. Para invalidar el valor predeterminado, establezca la propiedad <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> del <xref:System.Runtime.Serialization.DataContractAttribute> en un nombre alternativo. Más adelante, en este tema, se describen las reglas especiales de los tipos genéricos, en la sección "Nombres de contratos de datos para tipos genéricos".

## <a name="data-member-names"></a>Nombres de miembros de datos
El nombre predeterminado de un miembro de datos de un campo o propiedad determinado es el nombre de ese campo o propiedad. Para invalidar el valor predeterminado, establezca la propiedad <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A> del <xref:System.Runtime.Serialization.DataMemberAttribute> en un valor alternativo.

### <a name="examples"></a>Ejemplos
El siguiente ejemplo muestra cómo puede invalidar el comportamiento de denominación predeterminado de contratos de datos y miembros de datos.

[!code-csharp[C_DataContractNames#1](~/samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#1)]
[!code-vb[C_DataContractNames#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#1)]

## <a name="data-contract-names-for-generic-types"></a>Nombres de contratos de datos para tipos genéricos
Existen reglas especiales para determinar los nombres de contratos de datos para tipos genéricos. Estas reglas ayudan a evitar las coincidencias de nombres de contratos de datos entre dos genéricos cerrados del mismo tipo genérico.

De forma predeterminada, el contrato de datos nombre de un tipo genérico es el nombre del tipo, seguido por la cadena "De", seguido de los nombres de contrato de datos de los parámetros genéricos, seguidos por un *hash* calculada con los espacios de nombres de contrato de datos de los parámetros genéricos. Un hash es el resultado de una función matemática que actúa como una "huella" que identifica identifica de manera única una parte de datos. Cuando todos los parámetros genéricos son de tipo primitivo, se omite el hash.

Por ejemplo, vea los tipos del ejemplo siguiente.

[!code-csharp[C_DataContractNames#2](~/samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#2)]
[!code-vb[C_DataContractNames#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#2)]

En este ejemplo, el tipo `Drawing<Square,RegularRedBrush>` tiene el nombre de contrato de datos "DrawingOfSquareRedBrush5HWGAU6h", donde "5HWGAU6h" es un hash de los espacios de nombres "urn:default" y "urn:shapes". El tipo `Drawing<Square,SpecialRedBrush>` tiene el nombre de contrato de datos "DrawingOfSquareRedBrushjpB5LgQ_S", donde "jpB5LgQ_S" es un hash de los espacios de nombres "urn:shapes" y "urn:special". Tenga en cuenta que, si no se utiliza el hash, los dos nombres son idénticos y se produce un conflicto de nombres.

## <a name="customizing-data-contract-names-for-generic-types"></a>Personalizar nombres de contrato de datos para tipos genéricos

A veces, los nombres de contrato de datos generados para tipos genéricos, como se describió previamente, son inaceptables. Por ejemplo, puede conocer de antemano que no se encontrará con nombres coincidentes y puede que desee eliminar el hash. En este caso, puede usar el <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A?displayProperty=nameWithType> propiedad para especificar otra forma de generar nombres. Puede utilizar números dentro de llaves dentro de la propiedad `Name` para hacer referencia a los nombres de contratos de datos de los parámetros genéricos. (0 hace referencia al primer parámetro, 1 hace referencia al segundo, etc.). Puede utilizar una almohadilla (#) dentro de llaves para hacer referencia al hash. Puede utilizar cada una de estas referencias varias veces o ninguna.

Por ejemplo, el tipo genérico `Drawing` anterior podría haberse declarado como se muestra en el ejemplo siguiente.

[!code-csharp[c_DataContractNames#3](~/samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#3)]
[!code-vb[c_DataContractNames#3](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#3)]

En este caso, el tipo `Drawing<Square,RegularRedBrush>` tiene del el nombre de contrato de datos "Drawing_using_RedBrush_brush_and_Square_shape". Tenga en cuenta que porque hay un "{#}" en la propiedad <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A>, el hash no forma parte del nombre y por esto el tipo es susceptible a las coincidencias de nombres; por ejemplo, el tipo `Drawing<Square,SpecialRedBrush>` tendría exactamente el mismo nombre de contrato de datos.

## <a name="see-also"></a>Vea también

- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.ContractNamespaceAttribute>
- [Utilización de contratos de datos](using-data-contracts.md)
- [Equivalencia de contratos de datos](data-contract-equivalence.md)
- [Nombres de contratos de datos](data-contract-names.md)
- [Versiones de contratos de datos](data-contract-versioning.md)
