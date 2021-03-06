---
title: Buscar un elemento de UI Automation basándose en una condición de propiedad
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- elements, finding by property conditions
- UI Automation, finding elements by property conditions
ms.assetid: 3acaee5a-6ce8-4c3e-81c8-67e59eb74477
ms.openlocfilehash: 3ca609551955b32ad8b63296975ce10f097d9c30
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74433590"
---
# <a name="find-a-ui-automation-element-based-on-a-property-condition"></a>Buscar un elemento de UI Automation basándose en una condición de propiedad
> [!NOTE]
> Esta documentación está dirigida a los desarrolladores de .NET Framework que quieran usar las clases [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] administradas definidas en el espacio de nombres <xref:System.Windows.Automation>. Para ver la información más reciente acerca de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: automatización de la interfaz de usuario](/windows/win32/winauto/entry-uiauto-win32).  
  
 Este tema contiene código de ejemplo que muestra cómo buscar un elemento dentro del árbol de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] basado en una propiedad o propiedades específicas.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente, se especifica un conjunto de condiciones de propiedad que identifican un determinado elemento (o elementos) de interés en el árbol de <xref:System.Windows.Automation.AutomationElement>. Después, se realiza una búsqueda de todos los elementos coincidentes con el método <xref:System.Windows.Automation.AutomationElement.FindAll%2A> que incorpora una serie de operaciones booleanas <xref:System.Windows.Automation.AndCondition> para limitar el número de elementos coincidentes.  
  
> [!NOTE]
> Al buscar desde el <xref:System.Windows.Automation.AutomationElement.RootElement%2A>, debe intentar obtener solo elementos secundarios directos. Una búsqueda de descendientes puede iterar a través de cientos o incluso miles de elementos, lo que podría producir un desbordamiento de pila. Si intenta obtener un elemento concreto en un nivel inferior, debe iniciar la búsqueda desde la ventana de aplicación o desde un contenedor en un nivel inferior.  
  
 [!code-csharp[InvokePatternApp#1100](../../../samples/snippets/csharp/VS_Snippets_Wpf/InvokePatternApp/CSharp/InvokePatternApp.cs#1100)]
 [!code-vb[InvokePatternApp#1100](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InvokePatternApp/VisualBasic/Client.vb#1100)]  
  
## <a name="see-also"></a>Vea también

- [Ejemplo de elemento de menú InvokePattern y ExpandCollapsePattern](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771636(v=vs.90))
- [Obtaining UI Automation Elements](obtaining-ui-automation-elements.md)
- [Uso de la propiedad AutomationID](use-the-automationid-property.md)
