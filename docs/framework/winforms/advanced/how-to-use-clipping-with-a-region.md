---
title: Procedimiento para usar el recorte en una región
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regions [Windows Forms], clipping
- regions [Windows Forms], restricting drawing surface
ms.assetid: 43d121b4-e14c-4901-b25c-2d6c25ba4e29
ms.openlocfilehash: e62be137b36a2f369c02151466154f6b3bab090b
ms.sourcegitcommit: ca2ca60e6f5ea327f164be7ce26d9599e0f85fe4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65063836"
---
# <a name="how-to-use-clipping-with-a-region"></a>Procedimiento para usar el recorte en una región
Una de las propiedades de la <xref:System.Drawing.Graphics> clase es la región de recorte. Todos los dibujos realizados por un determinado <xref:System.Drawing.Graphics> está restringido a la región de recorte de dicho objeto <xref:System.Drawing.Graphics> objeto. Puede establecer la región de recorte mediante una llamada a la <xref:System.Drawing.Graphics.SetClip%2A> método.  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente crea una ruta de acceso que consta de un polígono. A continuación, el código construye una región, en función de esa ruta de acceso. La región se pasa a la <xref:System.Drawing.Graphics.SetClip%2A> método de un <xref:System.Drawing.Graphics> se dibujan los objetos y, a continuación, dos cadenas.  
  
 La siguiente ilustración muestra las cadenas recortadas:  
  
 ![Captura de pantalla que muestra cadenas recortadas.](./media/how-to-use-clipping-with-a-region/clipped-strings-polygon.png)  
  
 [!code-csharp[System.Drawing.MiscLegacyTopics#41](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.MiscLegacyTopics/CS/Class1.cs#41)]
 [!code-vb[System.Drawing.MiscLegacyTopics#41](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.MiscLegacyTopics/VB/Class1.vb#41)]  
  
## <a name="compiling-the-code"></a>Compilar el código  
 El ejemplo anterior está diseñado para su uso con Windows Forms y requiere <xref:System.Windows.Forms.PaintEventArgs> `e`, que es un parámetro de <xref:System.Windows.Forms.PaintEventHandler>.  
  
## <a name="see-also"></a>Vea también

- [Regiones de GDI+](regions-in-gdi.md)
- [Utilizar regiones](using-regions.md)
