---
title: Habilitar la navegación en un proveedor de fragmentos de UI Automation
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UI Automation, enabling navigation in provider
- navigation, enabling in UI Automation provider
ms.assetid: 3cb6092a-58c9-4ca0-84a5-0e54d5d00a0d
ms.openlocfilehash: 264a24646f7a3c3b5b20e94fa0ed98a1341f8273
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74435763"
---
# <a name="enable-navigation-in-a-ui-automation-fragment-provider"></a>Habilitar la navegación en un proveedor de fragmentos de UI Automation
> [!NOTE]
> Esta documentación está dirigida a los desarrolladores de .NET Framework que quieran usar las clases [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] administradas definidas en el espacio de nombres <xref:System.Windows.Automation>. Para ver la información más reciente acerca de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: automatización de la interfaz de usuario](/windows/win32/winauto/entry-uiauto-win32).  
  
 Este tema contiene código de ejemplo que muestra cómo habilitar la navegación en un proveedor de automatización de interfaz de usuario para un elemento que está dentro de un fragmento.  
  
## <a name="example"></a>Ejemplo  
 El siguiente código de ejemplo implementa <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A> para un elemento de lista dentro de una lista. El elemento primario es el elemento de cuadro de lista y los elementos del mismo nivel son otros elementos de la colección de lista. El método devuelve `null` (`Nothing` en Visual Basic) para las direcciones que no son válidas; en este caso, <xref:System.Windows.Automation.Provider.NavigateDirection.FirstChild> y <xref:System.Windows.Automation.Provider.NavigateDirection.LastChild>, ya que el elemento no tiene elementos secundarios.  
  
 [!code-csharp[UIAFragmentProvider_snip#103](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAFragmentProvider_snip/CSharp/ListItemFragment.cs#103)]
 [!code-vb[UIAFragmentProvider_snip#103](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAFragmentProvider_snip/VisualBasic/ListItemFragment.vb#103)]  
  
## <a name="see-also"></a>Vea también

- [Información general sobre proveedores de la Automatización de la interfaz de usuario](ui-automation-providers-overview.md)
- [Implementación del proveedor de automatización de la interfaz de usuario en el servidor](server-side-ui-automation-provider-implementation.md)
