---
title: Compatibilidad con DateTime y DateTimeOffset en System.Text.Json
description: Información general sobre cómo se admiten los tipos DateTime y DateTimeOffset en la biblioteca System. Text. JSON.
ms.technology: dotnet-standard
author: layomia
ms.author: laakinri
ms.date: 07/22/2019
helpviewer_keywords:
- JSON, Serializer, Utf8
- JSON DateTime, JSON DateTimeOffset
- DateTime, DateTimeOffset
- JsonSerializer, Utf8JsonReader, Utf8JsonWriter, JsonElement, JsonDocument
- JSON Serializer, JSON Reader, JSON Writer
- Converter, JSON Converter, DateTime Converter
- ISO, ISO 8601, ISO 8601-1:2019
ms.openlocfilehash: 5bff01b10b2bdea4fdcfee86e348c47f44d50103
ms.sourcegitcommit: c70542d02736e082e8dac67dad922c19249a8893
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70374474"
---
# <a name="datetime-and-datetimeoffset-support-in-systemtextjson"></a><span data-ttu-id="6cce1-103">Compatibilidad con DateTime y DateTimeOffset en System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="6cce1-103">DateTime and DateTimeOffset support in System.Text.Json</span></span>

<span data-ttu-id="6cce1-104">La biblioteca System. Text. JSON analiza y escribe <xref:System.DateTime> <xref:System.DateTimeOffset> los valores de acuerdo con el perfil extendido ISO 8601:-2019.</span><span class="sxs-lookup"><span data-stu-id="6cce1-104">The System.Text.Json library parses and writes <xref:System.DateTime> and <xref:System.DateTimeOffset> values according to the ISO 8601:-2019 extended profile.</span></span>
<span data-ttu-id="6cce1-105">Los [convertidores](https://docs.microsoft.com/dotnet/api/system.text.json.serialization.jsonconverter-1?view=netcore-3.0) proporcionan compatibilidad personalizada para la serialización y deserialización con <xref:System.Text.Json.JsonSerializer>.</span><span class="sxs-lookup"><span data-stu-id="6cce1-105">[Converters](https://docs.microsoft.com/dotnet/api/system.text.json.serialization.jsonconverter-1?view=netcore-3.0) provide custom support for serializing and deserializing with <xref:System.Text.Json.JsonSerializer>.</span></span>

## <a name="support-for-the-iso-8601-12019-format"></a><span data-ttu-id="6cce1-106">Compatibilidad con el formato ISO 8601-1:2019</span><span class="sxs-lookup"><span data-stu-id="6cce1-106">Support for the ISO 8601-1:2019 format</span></span>

<span data-ttu-id="6cce1-107">Los <xref:System.Text.Json.JsonSerializer>tipos <xref:System.Text.Json.Utf8JsonReader>, ,<xref:System.Text.Json.Utf8JsonWriter>y <xref:System.DateTime> <xref:System.DateTimeOffset> analizan y escriben representaciones de texto y de acuerdo con el perfil extendido del formato ISO 8601-1:2019; por ejemplo, 2019-07-26T16 <xref:System.Text.Json.JsonElement> : 59:57-05:00.</span><span class="sxs-lookup"><span data-stu-id="6cce1-107">The <xref:System.Text.Json.JsonSerializer>, <xref:System.Text.Json.Utf8JsonReader>, <xref:System.Text.Json.Utf8JsonWriter>, and <xref:System.Text.Json.JsonElement> types parse and write <xref:System.DateTime> and <xref:System.DateTimeOffset> text representations according to the extended profile of the ISO 8601-1:2019 format; for example, 2019-07-26T16:59:57-05:00.</span></span>

<span data-ttu-id="6cce1-108"><xref:System.DateTime>los <xref:System.DateTimeOffset> datos y se pueden serializar <xref:System.Text.Json.JsonSerializer>con:</span><span class="sxs-lookup"><span data-stu-id="6cce1-108"><xref:System.DateTime> and <xref:System.DateTimeOffset> data can be serialized with <xref:System.Text.Json.JsonSerializer>:</span></span>

[!code-csharp[example-serializing-with-jsonserializer](~/samples/snippets/standard/datetime/json/serializing-with-jsonserializer.cs)]

<span data-ttu-id="6cce1-109">También se pueden deserializar con <xref:System.Text.Json.JsonSerializer>:</span><span class="sxs-lookup"><span data-stu-id="6cce1-109">They can also be deserialized with <xref:System.Text.Json.JsonSerializer>:</span></span>

[!code-csharp[example-deserializing-with-jsonserializer-valid](~/samples/snippets/standard/datetime/json/deserializing-with-jsonserializer-valid.cs)]

<span data-ttu-id="6cce1-110">Con las opciones predeterminadas <xref:System.DateTimeOffset> , las representaciones de entrada <xref:System.DateTime> y de texto deben ajustarse al perfil ISO 8601-1:2019 extendido.</span><span class="sxs-lookup"><span data-stu-id="6cce1-110">With default options, input <xref:System.DateTime> and <xref:System.DateTimeOffset> text representations must conform to the extended ISO 8601-1:2019 profile.</span></span>
<span data-ttu-id="6cce1-111">Al intentar deserializar las representaciones que no se ajustan al perfil, <xref:System.Text.Json.JsonSerializer> se producirá <xref:System.Text.Json.JsonException>una excepción:</span><span class="sxs-lookup"><span data-stu-id="6cce1-111">Attempting to deserialize representations that don't conform to the profile will cause <xref:System.Text.Json.JsonSerializer> to throw a <xref:System.Text.Json.JsonException>:</span></span>

[!code-csharp[example-deserializing-with-jsonserializer-error](~/samples/snippets/standard/datetime/json/deserializing-with-jsonserializer-error.cs)]

<span data-ttu-id="6cce1-112">Proporciona acceso estructurado al contenido de una carga JSON, incluidas <xref:System.DateTime> las representaciones y <xref:System.DateTimeOffset>. <xref:System.Text.Json.JsonDocument></span><span class="sxs-lookup"><span data-stu-id="6cce1-112">The <xref:System.Text.Json.JsonDocument> provides structured access to the contents of a JSON payload, including <xref:System.DateTime> and <xref:System.DateTimeOffset> representations.</span></span> <span data-ttu-id="6cce1-113">En el ejemplo siguiente se muestra cómo, cuando se proporciona una colección de temperaturas, se puede calcular la temperatura media de los lunes:</span><span class="sxs-lookup"><span data-stu-id="6cce1-113">The example below shows how, when given a collection of temperatures, the average temperature on Mondays can be calculated:</span></span>

[!code-csharp[example-computing-with-jsondocument-valid](~/samples/snippets/standard/datetime/json/computing-with-jsondocument-valid.cs)]

<span data-ttu-id="6cce1-114">Al intentar calcular la temperatura media dada una carga con representaciones no compatibles <xref:System.DateTime> <xref:System.Text.Json.JsonDocument> , se producirá una <xref:System.FormatException>excepción:</span><span class="sxs-lookup"><span data-stu-id="6cce1-114">Attempting to compute the average temperature given a payload with non-compliant <xref:System.DateTime> representations will cause <xref:System.Text.Json.JsonDocument> to throw a <xref:System.FormatException>:</span></span>

[!code-csharp[example-computing-with-jsondocument-error](~/samples/snippets/standard/datetime/json/computing-with-jsondocument-error.cs)]

<span data-ttu-id="6cce1-115">Los datos y <xref:System.Text.Json.Utf8JsonWriter> <xref:System.DateTime> escrituras<xref:System.DateTimeOffset> de nivel inferior:</span><span class="sxs-lookup"><span data-stu-id="6cce1-115">The lower level <xref:System.Text.Json.Utf8JsonWriter> writes <xref:System.DateTime> and <xref:System.DateTimeOffset> data:</span></span>

[!code-csharp[example-writing-with-utf8jsonwriter](~/samples/snippets/standard/datetime/json/writing-with-utf8jsonwriter.cs)]

<span data-ttu-id="6cce1-116"><xref:System.Text.Json.Utf8JsonReader><xref:System.DateTime> análisis y<xref:System.DateTimeOffset> datos:</span><span class="sxs-lookup"><span data-stu-id="6cce1-116"><xref:System.Text.Json.Utf8JsonReader> parses <xref:System.DateTime> and <xref:System.DateTimeOffset> data:</span></span>

[!code-csharp[example-reading-with-utf8jsonreader-valid](~/samples/snippets/standard/datetime/json/reading-with-utf8jsonreader-valid.cs)]

<span data-ttu-id="6cce1-117">Si intenta leer formatos no compatibles con <xref:System.Text.Json.Utf8JsonReader> , se producirá una <xref:System.FormatException>excepción:</span><span class="sxs-lookup"><span data-stu-id="6cce1-117">Attempting to read non-compliant formats with <xref:System.Text.Json.Utf8JsonReader> will cause it to throw a <xref:System.FormatException>:</span></span>

[!code-csharp[example-reading-with-utf8jsonreader-error](~/samples/snippets/standard/datetime/json/reading-with-utf8jsonreader-error.cs)]

## <a name="custom-support-for-xrefsystemdatetime-and-xrefsystemdatetimeoffset-in-xrefsystemtextjsonjsonserializer"></a><span data-ttu-id="6cce1-118">Compatibilidad personalizada para <xref:System.DateTime> y <xref:System.DateTimeOffset> en<xref:System.Text.Json.JsonSerializer></span><span class="sxs-lookup"><span data-stu-id="6cce1-118">Custom support for <xref:System.DateTime> and <xref:System.DateTimeOffset> in <xref:System.Text.Json.JsonSerializer></span></span>

<span data-ttu-id="6cce1-119">Si desea que el serializador realice el análisis o el formato personalizados, puede implementar [convertidores personalizados](https://docs.microsoft.com/dotnet/api/system.text.json.serialization.jsonconverter-1?view=netcore-3.0).</span><span class="sxs-lookup"><span data-stu-id="6cce1-119">If you want the serializer to perform custom parsing or formatting, you can implement [custom converters](https://docs.microsoft.com/dotnet/api/system.text.json.serialization.jsonconverter-1?view=netcore-3.0).</span></span>
<span data-ttu-id="6cce1-120">Estos son algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="6cce1-120">Here are a few examples:</span></span>

### <a name="using-datetimeoffsetparse-and-datetimeoffsettostring"></a><span data-ttu-id="6cce1-121">Usar `DateTime(Offset).Parse` y`DateTime(Offset).ToString`</span><span class="sxs-lookup"><span data-stu-id="6cce1-121">Using `DateTime(Offset).Parse` and `DateTime(Offset).ToString`</span></span>

<span data-ttu-id="6cce1-122">Si no puede determinar los formatos de las representaciones <xref:System.DateTime> de <xref:System.DateTimeOffset> entrada o texto, puede usar el `DateTime(Offset).Parse` método en la lógica de lectura del convertidor.</span><span class="sxs-lookup"><span data-stu-id="6cce1-122">If you can't determine the formats of your input <xref:System.DateTime> or <xref:System.DateTimeOffset> text representations, you can use the `DateTime(Offset).Parse` method in your converter read logic.</span></span> <span data-ttu-id="6cce1-123">Esto le permite usar. La amplia compatibilidad de la red con <xref:System.DateTime> el <xref:System.DateTimeOffset> análisis de varios formatos de texto y, como cadenas que no son ISO 8601 y formatos ISO 8601 que no se ajustan al perfil ISO 8601-1:2019 extendido.</span><span class="sxs-lookup"><span data-stu-id="6cce1-123">This allows you to use .NET's extensive support for parsing various <xref:System.DateTime> and <xref:System.DateTimeOffset> text formats, including non-ISO 8601 strings and ISO 8601 formats that don't conform to the extended ISO 8601-1:2019 profile.</span></span> <span data-ttu-id="6cce1-124">Este enfoque es significativamente menos eficaz que el uso de la implementación nativa del serializador.</span><span class="sxs-lookup"><span data-stu-id="6cce1-124">This approach is significantly less performant than using the serializer's native implementation.</span></span>

<span data-ttu-id="6cce1-125">Para la serialización, puede usar el `DateTime(Offset).ToString` método en la lógica de escritura del convertidor.</span><span class="sxs-lookup"><span data-stu-id="6cce1-125">For serializing, you can use the `DateTime(Offset).ToString` method in your converter write logic.</span></span> <span data-ttu-id="6cce1-126">Esto <xref:System.DateTime> le permite escribir valores y <xref:System.DateTimeOffset> con cualquiera de los formatos de [fecha y hora estándar](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings)y los formatos de [fecha y hora personalizados](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings).</span><span class="sxs-lookup"><span data-stu-id="6cce1-126">This allows you to write <xref:System.DateTime> and <xref:System.DateTimeOffset> values using any of the [standard date and time formats](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), and the [custom date and time formats](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings).</span></span>
<span data-ttu-id="6cce1-127">Esto también es significativamente menos eficaz que el uso de la implementación nativa del serializador.</span><span class="sxs-lookup"><span data-stu-id="6cce1-127">This is also significantly less performant than using the serializer's native implementation.</span></span>

[!code-csharp[example-showing-datetime-parse](~/samples/snippets/standard/datetime/json/datetime-converter-examples/example1/Program.cs)]

> [!NOTE]
> <span data-ttu-id="6cce1-128">Al implementar <xref:System.Text.Json.Serialization.JsonConverter%601> `T` y es <xref:System.DateTime>, el`typeToConvert` parámetro siempre`typeof(DateTime)`será.</span><span class="sxs-lookup"><span data-stu-id="6cce1-128">When implementing <xref:System.Text.Json.Serialization.JsonConverter%601>, and `T` is <xref:System.DateTime>, the `typeToConvert` parameter will always be `typeof(DateTime)`.</span></span>
<span data-ttu-id="6cce1-129">El parámetro es útil para controlar los casos polimórficos y cuando se usan genéricos para obtener `typeof(T)` una manera eficaz.</span><span class="sxs-lookup"><span data-stu-id="6cce1-129">The parameter is useful for handling polymorphic cases and when using generics to get `typeof(T)` in a performant way.</span></span>

### <a name="using-xrefsystembufferstextutf8parser-and-xrefsystembufferstextutf8formatter"></a><span data-ttu-id="6cce1-130">Usar <xref:System.Buffers.Text.Utf8Parser> y<xref:System.Buffers.Text.Utf8Formatter></span><span class="sxs-lookup"><span data-stu-id="6cce1-130">Using <xref:System.Buffers.Text.Utf8Parser> and <xref:System.Buffers.Text.Utf8Formatter></span></span>

<span data-ttu-id="6cce1-131">Puede usar los métodos de formato y análisis basados en UTF-8 rápidos en la lógica del convertidor si <xref:System.DateTime> las <xref:System.DateTimeOffset> representaciones de entrada o de texto son compatibles con una de las [cadenas de formato de fecha y hora estándar](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings)"R", "l", "o" o "G", o bien desea Escriba según uno de estos formatos.</span><span class="sxs-lookup"><span data-stu-id="6cce1-131">You can use fast UTF-8-based parsing and formatting methods in your converter logic if your input <xref:System.DateTime> or <xref:System.DateTimeOffset> text representations are compliant with one of the "R", "l", "O", or "G" [standard date and time format Strings](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings), or you want to write according to one of these formats.</span></span> <span data-ttu-id="6cce1-132">Esto es mucho más rápido que `DateTime(Offset).Parse` el `DateTime(Offset).ToString`uso de y.</span><span class="sxs-lookup"><span data-stu-id="6cce1-132">This is much faster than using `DateTime(Offset).Parse` and `DateTime(Offset).ToString`.</span></span>

<span data-ttu-id="6cce1-133">En este ejemplo se muestra un convertidor personalizado que serializa y deserializa <xref:System.DateTime> valores según [el formato estándar "R"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings#the-rfc1123-r-r-format-specifier):</span><span class="sxs-lookup"><span data-stu-id="6cce1-133">This example shows a custom converter that serializes and deserializes <xref:System.DateTime> values according to [the "R" standard format](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings#the-rfc1123-r-r-format-specifier):</span></span>

[!code-csharp[example-showing-utf8-parser-and-formatter](~/samples/snippets/standard/datetime/json/datetime-converter-examples/example2/Program.cs)]

> [!NOTE]
> <span data-ttu-id="6cce1-134">El formato estándar "R" siempre tendrá una longitud de 29 caracteres.</span><span class="sxs-lookup"><span data-stu-id="6cce1-134">The "R" standard format will always be 29 characters long.</span></span>

### <a name="using-datetimeoffsetparse-as-a-fallback-to-the-serializers-native-parsing"></a><span data-ttu-id="6cce1-135">Usar `DateTime(Offset).Parse` como reserva para el análisis nativo del serializador</span><span class="sxs-lookup"><span data-stu-id="6cce1-135">Using `DateTime(Offset).Parse` as a fallback to the serializer's native parsing</span></span>

<span data-ttu-id="6cce1-136">Si normalmente espera que la entrada <xref:System.DateTime> o <xref:System.DateTimeOffset> los datos se ajusten al perfil extendido ISO 8601-1:2019, puede usar la lógica de análisis nativo del serializador.</span><span class="sxs-lookup"><span data-stu-id="6cce1-136">If you generally expect your input <xref:System.DateTime> or <xref:System.DateTimeOffset> data to conform to the extended ISO 8601-1:2019 profile, you can use the serializer's native parsing logic.</span></span> <span data-ttu-id="6cce1-137">También puede implementar un mecanismo de reserva en caso de que se trate.</span><span class="sxs-lookup"><span data-stu-id="6cce1-137">You can also implement a fallback mechanism just in case.</span></span>
<span data-ttu-id="6cce1-138">En este ejemplo se muestra que, después de no analizar <xref:System.DateTime> una representación de <xref:System.Text.Json.Utf8JsonReader.TryGetDateTime(System.DateTime@)>texto mediante, el convertidor analiza correctamente los datos <xref:System.DateTime.Parse(System.String)>mediante.</span><span class="sxs-lookup"><span data-stu-id="6cce1-138">This example shows that, after failing to parse a <xref:System.DateTime> text representation using <xref:System.Text.Json.Utf8JsonReader.TryGetDateTime(System.DateTime@)>, the converter successfully parses the data using <xref:System.DateTime.Parse(System.String)>.</span></span>

[!code-csharp[example-showing-datetime-parse-as-fallback](~/samples/snippets/standard/datetime/json/datetime-converter-examples/example3/Program.cs)]

## <a name="the-extended-iso-8601-12019-profile-in-systemtextjson"></a><span data-ttu-id="6cce1-139">El perfil ISO 8601-1:2019 extendido en System. Text. JSON</span><span class="sxs-lookup"><span data-stu-id="6cce1-139">The extended ISO 8601-1:2019 profile in System.Text.Json</span></span>

### <a name="date-and-time-components"></a><span data-ttu-id="6cce1-140">Componentes de fecha y hora</span><span class="sxs-lookup"><span data-stu-id="6cce1-140">Date and time components</span></span>

<span data-ttu-id="6cce1-141">El perfil extendido ISO 8601-1:2019 implementado <xref:System.Text.Json> en define los siguientes componentes para las representaciones de fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="6cce1-141">The extended ISO 8601-1:2019 profile implemented in <xref:System.Text.Json> defines the following components for date and time representations.</span></span> <span data-ttu-id="6cce1-142">Estos componentes se usan para definir diversos niveles de granularidad admitidos al analizar y dar <xref:System.DateTime> formato <xref:System.DateTimeOffset> y representaciones.</span><span class="sxs-lookup"><span data-stu-id="6cce1-142">These components are used to define various levels of granularity supported when parsing and formatting <xref:System.DateTime> and <xref:System.DateTimeOffset> representations.</span></span>

| <span data-ttu-id="6cce1-143">Componente</span><span class="sxs-lookup"><span data-stu-id="6cce1-143">Component</span></span>       | <span data-ttu-id="6cce1-144">Formato</span><span class="sxs-lookup"><span data-stu-id="6cce1-144">Format</span></span>                      | <span data-ttu-id="6cce1-145">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="6cce1-145">Description</span></span>                                                                     |
|-----------------|-----------------------------|---------------------------------------------------------------------------------|
| <span data-ttu-id="6cce1-146">Year</span><span class="sxs-lookup"><span data-stu-id="6cce1-146">Year</span></span>            | <span data-ttu-id="6cce1-147">"yyyy"</span><span class="sxs-lookup"><span data-stu-id="6cce1-147">"yyyy"</span></span>                      | <span data-ttu-id="6cce1-148">0001-9999</span><span class="sxs-lookup"><span data-stu-id="6cce1-148">0001-9999</span></span>                                                                       |
| <span data-ttu-id="6cce1-149">Mes</span><span class="sxs-lookup"><span data-stu-id="6cce1-149">Month</span></span>           | <span data-ttu-id="6cce1-150">"MM"</span><span class="sxs-lookup"><span data-stu-id="6cce1-150">"MM"</span></span>                        | <span data-ttu-id="6cce1-151">01-12</span><span class="sxs-lookup"><span data-stu-id="6cce1-151">01-12</span></span>                                                                           |
| <span data-ttu-id="6cce1-152">Día</span><span class="sxs-lookup"><span data-stu-id="6cce1-152">Day</span></span>             | <span data-ttu-id="6cce1-153">"dd"</span><span class="sxs-lookup"><span data-stu-id="6cce1-153">"dd"</span></span>                        | <span data-ttu-id="6cce1-154">01-28, 01-29, 01-30, 01-31 basado en mes/año</span><span class="sxs-lookup"><span data-stu-id="6cce1-154">01-28, 01-29, 01-30, 01-31 based on month/year</span></span>                                  |
| <span data-ttu-id="6cce1-155">Hour</span><span class="sxs-lookup"><span data-stu-id="6cce1-155">Hour</span></span>            | <span data-ttu-id="6cce1-156">"HH"</span><span class="sxs-lookup"><span data-stu-id="6cce1-156">"HH"</span></span>                        | <span data-ttu-id="6cce1-157">00-23</span><span class="sxs-lookup"><span data-stu-id="6cce1-157">00-23</span></span>                                                                           |
| <span data-ttu-id="6cce1-158">Minute</span><span class="sxs-lookup"><span data-stu-id="6cce1-158">Minute</span></span>          | <span data-ttu-id="6cce1-159">"mm"</span><span class="sxs-lookup"><span data-stu-id="6cce1-159">"mm"</span></span>                        | <span data-ttu-id="6cce1-160">00-59</span><span class="sxs-lookup"><span data-stu-id="6cce1-160">00-59</span></span>                                                                           |
| <span data-ttu-id="6cce1-161">Second</span><span class="sxs-lookup"><span data-stu-id="6cce1-161">Second</span></span>          | <span data-ttu-id="6cce1-162">"ss"</span><span class="sxs-lookup"><span data-stu-id="6cce1-162">"ss"</span></span>                        | <span data-ttu-id="6cce1-163">00-59</span><span class="sxs-lookup"><span data-stu-id="6cce1-163">00-59</span></span>                                                                           |
| <span data-ttu-id="6cce1-164">Segunda fracción</span><span class="sxs-lookup"><span data-stu-id="6cce1-164">Second fraction</span></span> | <span data-ttu-id="6cce1-165">"FFFFFFF"</span><span class="sxs-lookup"><span data-stu-id="6cce1-165">"FFFFFFF"</span></span>                   | <span data-ttu-id="6cce1-166">Mínimo de un dígito, máximo de 16 dígitos</span><span class="sxs-lookup"><span data-stu-id="6cce1-166">Minimum of one digit, maximum of 16 digits</span></span>                                      |
| <span data-ttu-id="6cce1-167">Desplazamiento de tiempo</span><span class="sxs-lookup"><span data-stu-id="6cce1-167">Time offset</span></span>     | <span data-ttu-id="6cce1-168">"K"</span><span class="sxs-lookup"><span data-stu-id="6cce1-168">"K"</span></span>                         | <span data-ttu-id="6cce1-169">"Z" o "(' + '/'-') HH ': ' mm '</span><span class="sxs-lookup"><span data-stu-id="6cce1-169">Either "Z" or "('+'/'-')HH':'mm"</span></span>                                                |
| <span data-ttu-id="6cce1-170">Tiempo parcial</span><span class="sxs-lookup"><span data-stu-id="6cce1-170">Partial time</span></span>    | <span data-ttu-id="6cce1-171">"HH": "mm": "SS [FFFFFFF]"</span><span class="sxs-lookup"><span data-stu-id="6cce1-171">"HH':'mm':'ss[FFFFFFF]"</span></span>     | <span data-ttu-id="6cce1-172">Hora sin información de desplazamiento de UTC</span><span class="sxs-lookup"><span data-stu-id="6cce1-172">Time without UTC offset information</span></span>                                             |
| <span data-ttu-id="6cce1-173">Fecha completa</span><span class="sxs-lookup"><span data-stu-id="6cce1-173">Full date</span></span>       | <span data-ttu-id="6cce1-174">"YYYY-'-DD"</span><span class="sxs-lookup"><span data-stu-id="6cce1-174">"yyyy'-'MM'-'dd"</span></span>            | <span data-ttu-id="6cce1-175">Fecha del calendario</span><span class="sxs-lookup"><span data-stu-id="6cce1-175">Calendar date</span></span>                                                                   |
| <span data-ttu-id="6cce1-176">Tiempo completo</span><span class="sxs-lookup"><span data-stu-id="6cce1-176">Full time</span></span>       | <span data-ttu-id="6cce1-177">"Time'K Partial"</span><span class="sxs-lookup"><span data-stu-id="6cce1-177">"'Partial time'K"</span></span>           | <span data-ttu-id="6cce1-178">Hora UTC del día o hora local del día con el desplazamiento de tiempo entre la hora local y la hora UTC</span><span class="sxs-lookup"><span data-stu-id="6cce1-178">UTC of day or Local time of day with the time offset between local time and UTC</span></span> |
| <span data-ttu-id="6cce1-179">Fecha y hora</span><span class="sxs-lookup"><span data-stu-id="6cce1-179">Date time</span></span>       | <span data-ttu-id="6cce1-180">"' ' Fecha completa ' ' t ' ' ' '</span><span class="sxs-lookup"><span data-stu-id="6cce1-180">"'Full date''T''Full time'"</span></span> | <span data-ttu-id="6cce1-181">Fecha y hora del calendario del día, por ejemplo, 2019-07-26T16:59:57-05:00</span><span class="sxs-lookup"><span data-stu-id="6cce1-181">Calendar date and time of day, e.g. 2019-07-26T16:59:57-05:00</span></span>                   |

### <a name="support-for-parsing"></a><span data-ttu-id="6cce1-182">Compatibilidad con el análisis</span><span class="sxs-lookup"><span data-stu-id="6cce1-182">Support for parsing</span></span>

<span data-ttu-id="6cce1-183">Se definen los siguientes niveles de granularidad para el análisis:</span><span class="sxs-lookup"><span data-stu-id="6cce1-183">The following levels of granularity are defined for parsing:</span></span>

1. <span data-ttu-id="6cce1-184">' Fecha completa '</span><span class="sxs-lookup"><span data-stu-id="6cce1-184">'Full date'</span></span>
    1. <span data-ttu-id="6cce1-185">"YYYY-'-DD"</span><span class="sxs-lookup"><span data-stu-id="6cce1-185">"yyyy'-'MM'-'dd"</span></span>

2. <span data-ttu-id="6cce1-186">"Fecha completa ' t ' ' hora ' ': ' ' minuto '"</span><span class="sxs-lookup"><span data-stu-id="6cce1-186">"'Full date''T''Hour'':''Minute'"</span></span>
    1. <span data-ttu-id="6cce1-187">"YYYY-'-Dd't": "mm"</span><span class="sxs-lookup"><span data-stu-id="6cce1-187">"yyyy'-'MM'-'dd'T'HH':'mm"</span></span>

3. <span data-ttu-id="6cce1-188">"' Fecha completa ' ' t ' ' de hora parcial ' '</span><span class="sxs-lookup"><span data-stu-id="6cce1-188">"'Full date''T''Partial time'"</span></span>
    1. <span data-ttu-id="6cce1-189">"YYYY-'-Dd't": "mm": "SS" ([el especificador de formato que se va a ordenar ("s")](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings#the-sortable-s-format-specifier))</span><span class="sxs-lookup"><span data-stu-id="6cce1-189">"yyyy'-'MM'-'dd'T'HH':'mm':'ss" ([The Sortable ("s") Format Specifier](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings#the-sortable-s-format-specifier))</span></span>
    2. <span data-ttu-id="6cce1-190">"YYYY-'-Dd't ': ' mm ': ' SS '. ' FFFFFFF</span><span class="sxs-lookup"><span data-stu-id="6cce1-190">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF"</span></span>

4. <span data-ttu-id="6cce1-191">"Fecha completa ' t ' ' hora de la hora ' ': ' ' minuto ' ' desplazamiento de tiempo ' '</span><span class="sxs-lookup"><span data-stu-id="6cce1-191">"'Full date''T''Time hour'':''Minute''Time offset'"</span></span>
    1. <span data-ttu-id="6cce1-192">"YYYY-'-Dd't": "mmZ"</span><span class="sxs-lookup"><span data-stu-id="6cce1-192">"yyyy'-'MM'-'dd'T'HH':'mmZ"</span></span>
    2. <span data-ttu-id="6cce1-193">"YYYY-'-Dd't ': ' mm (' + '/'-') HH ': ' mm '</span><span class="sxs-lookup"><span data-stu-id="6cce1-193">"yyyy'-'MM'-'dd'T'HH':'mm('+'/'-')HH':'mm"</span></span>

5. <span data-ttu-id="6cce1-194">' Fecha y hora '</span><span class="sxs-lookup"><span data-stu-id="6cce1-194">'Date time'</span></span>
    1. <span data-ttu-id="6cce1-195">"YYYY-'-Dd't ': ' mm ': ' ssZ '</span><span class="sxs-lookup"><span data-stu-id="6cce1-195">"yyyy'-'MM'-'dd'T'HH':'mm':'ssZ"</span></span>
    2. <span data-ttu-id="6cce1-196">"YYYY-'-Dd't ': ' mm ': ' SS '. ' FFFFFFFZ"</span><span class="sxs-lookup"><span data-stu-id="6cce1-196">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFFZ"</span></span>
    3. <span data-ttu-id="6cce1-197">"YYYY-'-Dd't ': ' mm ': ' SS (' + '/'-') HH ': ' mm '</span><span class="sxs-lookup"><span data-stu-id="6cce1-197">"yyyy'-'MM'-'dd'T'HH':'mm':'ss('+'/'-')HH':'mm"</span></span>
    4. <span data-ttu-id="6cce1-198">"YYYY-'-Dd't ': ' mm ': ' SS '. ' FFFFFFF (' + '/'-') HH ': ' mm '</span><span class="sxs-lookup"><span data-stu-id="6cce1-198">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF('+'/'-')HH':'mm"</span></span>

<span data-ttu-id="6cce1-199">Si hay fracciones decimales para segundos, debe haber al menos un dígito. `2019-07-26T00:00:00.` no está permitido.</span><span class="sxs-lookup"><span data-stu-id="6cce1-199">If there are decimal fractions for seconds, there must be at least one digit; `2019-07-26T00:00:00.` is not allowed.</span></span>
<span data-ttu-id="6cce1-200">Aunque se permiten hasta 16 dígitos fraccionarios, solo se analizan los siete primeros.</span><span class="sxs-lookup"><span data-stu-id="6cce1-200">While up to 16 fractional digits are allowed, only the first seven are parsed.</span></span> <span data-ttu-id="6cce1-201">Cualquier cosa más allá se considera un cero.</span><span class="sxs-lookup"><span data-stu-id="6cce1-201">Anything beyond that is considered a zero.</span></span>
<span data-ttu-id="6cce1-202">Por ejemplo, `2019-07-26T00:00:00.1234567890` se analizará como si se tratase `2019-07-26T00:00:00.1234567`de.</span><span class="sxs-lookup"><span data-stu-id="6cce1-202">For example, `2019-07-26T00:00:00.1234567890` will be parsed as if it is `2019-07-26T00:00:00.1234567`.</span></span>
<span data-ttu-id="6cce1-203">Esto es para mantener la compatibilidad con <xref:System.DateTime> la implementación, que se limita a esta resolución.</span><span class="sxs-lookup"><span data-stu-id="6cce1-203">This is to stay compatible with the <xref:System.DateTime> implementation, which is limited to this resolution.</span></span>

<span data-ttu-id="6cce1-204">No se admiten los segundos de LEAP.</span><span class="sxs-lookup"><span data-stu-id="6cce1-204">Leap seconds are not supported.</span></span>

### <a name="support-for-formatting"></a><span data-ttu-id="6cce1-205">Compatibilidad con el formato</span><span class="sxs-lookup"><span data-stu-id="6cce1-205">Support for formatting</span></span>

<span data-ttu-id="6cce1-206">Se definen los siguientes niveles de granularidad para el formato:</span><span class="sxs-lookup"><span data-stu-id="6cce1-206">The following levels of granularity are defined for formatting:</span></span>

1. <span data-ttu-id="6cce1-207">"' Fecha completa ' ' t ' ' de hora parcial ' '</span><span class="sxs-lookup"><span data-stu-id="6cce1-207">"'Full date''T''Partial time'"</span></span>
    1. <span data-ttu-id="6cce1-208">"YYYY-'-Dd't": "mm": "SS" ([el especificador de formato que se va a ordenar ("s")](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings#the-sortable-s-format-specifier))</span><span class="sxs-lookup"><span data-stu-id="6cce1-208">"yyyy'-'MM'-'dd'T'HH':'mm':'ss"  ([The Sortable ("s") Format Specifier](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings#the-sortable-s-format-specifier))</span></span>

        <span data-ttu-id="6cce1-209">Se usa para dar <xref:System.DateTime> formato a un sin fracciones de segundo y sin información de desplazamiento.</span><span class="sxs-lookup"><span data-stu-id="6cce1-209">Used to format a <xref:System.DateTime> without fractional seconds and without offset information.</span></span>

    2. <span data-ttu-id="6cce1-210">"YYYY-'-Dd't ': ' mm ': ' SS '. ' FFFFFFF</span><span class="sxs-lookup"><span data-stu-id="6cce1-210">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF"</span></span>

        <span data-ttu-id="6cce1-211">Se usa para dar <xref:System.DateTime> formato a un con fracciones de segundo pero sin información de desplazamiento.</span><span class="sxs-lookup"><span data-stu-id="6cce1-211">Used to format a <xref:System.DateTime> with fractional seconds but without offset information.</span></span>

2. <span data-ttu-id="6cce1-212">' Fecha y hora '</span><span class="sxs-lookup"><span data-stu-id="6cce1-212">'Date time'</span></span>
    1. <span data-ttu-id="6cce1-213">"YYYY-'-Dd't ': ' mm ': ' ssZ '</span><span class="sxs-lookup"><span data-stu-id="6cce1-213">"yyyy'-'MM'-'dd'T'HH':'mm':'ssZ"</span></span>

        <span data-ttu-id="6cce1-214">Se usa para dar <xref:System.DateTime> formato <xref:System.DateTimeOffset> a un o sin fracciones de segundo, pero con un desplazamiento UTC.</span><span class="sxs-lookup"><span data-stu-id="6cce1-214">Used to format a <xref:System.DateTime> or <xref:System.DateTimeOffset> without fractional seconds but with a UTC offset.</span></span>

    2. <span data-ttu-id="6cce1-215">"YYYY-'-Dd't ': ' mm ': ' SS '. ' FFFFFFFZ"</span><span class="sxs-lookup"><span data-stu-id="6cce1-215">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFFZ"</span></span>

        <span data-ttu-id="6cce1-216">Se usa para dar <xref:System.DateTime> formato <xref:System.DateTimeOffset> a un o con fracciones de segundo y con un desplazamiento de UTC.</span><span class="sxs-lookup"><span data-stu-id="6cce1-216">Used to format a <xref:System.DateTime> or <xref:System.DateTimeOffset> with fractional seconds and with a UTC offset.</span></span>

    3. <span data-ttu-id="6cce1-217">"YYYY-'-Dd't ': ' mm ': ' SS (' + '/'-') HH ': ' mm '</span><span class="sxs-lookup"><span data-stu-id="6cce1-217">"yyyy'-'MM'-'dd'T'HH':'mm':'ss('+'/'-')HH':'mm"</span></span>

        <span data-ttu-id="6cce1-218">Se usa para dar <xref:System.DateTime> formato <xref:System.DateTimeOffset> a o sin fracciones de segundo, pero con un desplazamiento local.</span><span class="sxs-lookup"><span data-stu-id="6cce1-218">Used to format a <xref:System.DateTime> or <xref:System.DateTimeOffset> without fractional seconds but with a local offset.</span></span>

    4. <span data-ttu-id="6cce1-219">"YYYY-'-Dd't ': ' mm ': ' SS '. ' FFFFFFF (' + '/'-') HH ': ' mm '</span><span class="sxs-lookup"><span data-stu-id="6cce1-219">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF('+'/'-')HH':'mm"</span></span>

        <span data-ttu-id="6cce1-220">Se usa para dar <xref:System.DateTime> formato <xref:System.DateTimeOffset> a un o con fracciones de segundo y con un desplazamiento local.</span><span class="sxs-lookup"><span data-stu-id="6cce1-220">Used to format a <xref:System.DateTime> or <xref:System.DateTimeOffset> with fractional seconds and with a local offset.</span></span>