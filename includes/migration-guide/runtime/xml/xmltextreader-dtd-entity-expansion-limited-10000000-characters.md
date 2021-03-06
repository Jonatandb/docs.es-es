---
ms.openlocfilehash: 1f5bc43dcba15c28c179b23352558411f511c82f
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59235872"
---
### <a name="xmltextreader-dtd-entity-expansion-is-limited-to-10000000-characters"></a>La expansión de entidades DTD de XmlTextReader está limitada a 10 000 000 caracteres

|   |   |
|---|---|
|Detalles|La expansión de entidades DTD ahora está limitada a 10 000 000 caracteres. La carga de archivos XML sin expansión de entidades DTD o con expansión de entidades DTD limitada no se verá afectada. Los archivos con entidades de DTD que se expanden a más de 10.000.000 caracteres no se podrán cargar y ahora iniciarán una excepción.|
|Sugerencia|Si el límite de expansión de entidades DTD de 10 000 000 es demasiado bajo, el valor se puede anular con la propiedad <xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities>. Se puede pasar un <xref:System.Xml.XmlReaderSettings?displayProperty=name> con el valor <xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities?displayProperty=name> adecuado a <code>XmlReader.Create</code> que toma <xref:System.Xml.XmlReaderSettings?displayProperty=name> (por ejemplo, <xref:System.Xml.XmlReader.Create(System.String,System.Xml.XmlReaderSettings)>).|
|Ámbito|Borde|
|Versión|4.5|
|Tipo|Tiempo de ejecución|
|API afectadas|<ul><li><xref:System.Xml.XmlTextReader?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.Stream)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.Stream,System.Xml.XmlNameTable)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.Stream,System.Xml.XmlNodeType,System.Xml.XmlParserContext)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.TextReader)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.IO.TextReader,System.Xml.XmlNameTable)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.Stream)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.Stream,System.Xml.XmlNameTable)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.TextReader)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.IO.TextReader,System.Xml.XmlNameTable)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.Xml.XmlNameTable)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.String,System.Xml.XmlNodeType,System.Xml.XmlParserContext)?displayProperty=nameWithType></li><li><xref:System.Xml.XmlTextReader.%23ctor(System.Xml.XmlNameTable)?displayProperty=nameWithType></li></ul>|
