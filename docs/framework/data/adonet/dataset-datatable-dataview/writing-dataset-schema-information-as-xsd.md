---
title: Escribir información del esquema de un conjunto de datos como XSD
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 4e530831-695e-49ff-8f0b-e5b0c526b8eb
ms.openlocfilehash: f86e9100489ddf35d8ef5f98e386306a7dbfd4ed
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2019
ms.locfileid: "70784181"
---
# <a name="writing-dataset-schema-information-as-xsd"></a>Escribir información del esquema de un conjunto de datos como XSD
Puede escribir el esquema de un <xref:System.Data.DataSet> como un esquema de lenguaje de definición de esquemas XML (XSD), de forma que pueda transportarlo, con o sin datos relacionados, a un documento XML. El esquema XML se puede escribir en un archivo, una secuencia, <xref:System.Xml.XmlWriter>un o una cadena; resulta útil para generar un conjunto de un **DataSet**fuertemente tipado. Para obtener más información sobre los objetos de **conjunto** de datos fuertemente tipados, vea [conjuntos de datos con tipo](typed-datasets.md).  
  
 Puede especificar cómo se representa una columna de una tabla en el esquema XML mediante la propiedad **ColumnMapping** del <xref:System.Data.DataColumn> objeto. Para obtener más información, vea "asignar columnas a elementos, atributos y texto XML" al [escribir el contenido del conjunto de datos como datos XML](writing-dataset-contents-as-xml-data.md).  
  
 Para escribir el esquema de un **DataSet** como esquema XML, en un archivo, una secuencia o **XmlWriter**, utilice el método **WriteXmlSchema** del **conjunto**de archivos. **WriteXmlSchema** toma un parámetro que especifica el destino del esquema XML resultante. En los siguientes ejemplos de código se muestra cómo escribir el esquema XML de un **conjunto** de objetos en un archivo pasando una cadena que contiene un <xref:System.IO.StreamWriter> nombre de archivo y un objeto.  
  
```vb  
dataSet.WriteXmlSchema("Customers.xsd")  
```  
  
```csharp  
dataSet.WriteXmlSchema("Customers.xsd");  
```  
  
```vb  
Dim writer As System.IO.StreamWriter = New System.IO.StreamWriter("Customers.xsd")  
dataSet.WriteXmlSchema(writer)  
writer.Close()  
```  
  
```csharp  
System.IO.StreamWriter writer = new System.IO.StreamWriter("Customers.xsd");  
dataSet.WriteXmlSchema(writer);  
writer.Close();  
```  
  
 Para obtener el esquema de un **DataSet** y escribirlo como una cadena de esquema XML, use el método **GetXmlSchema** , como se muestra en el ejemplo siguiente.  
  
```vb  
Dim schemaString As String = dataSet.GetXmlSchema()  
```  
  
```csharp  
string schemaString = dataSet.GetXmlSchema();  
```  
  
## <a name="see-also"></a>Vea también

- [Usar XML en un conjunto de datos](using-xml-in-a-dataset.md)
- [Escritura de contenido de un conjunto de datos como datos XML](writing-dataset-contents-as-xml-data.md)
- [Objetos DataSet con tipo](typed-datasets.md)
- [Objetos DataSet, DataTable y DataView](index.md)
- [Información general sobre ADO.NET](../ado-net-overview.md)
