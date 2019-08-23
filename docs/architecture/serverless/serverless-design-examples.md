---
title: 'Ejemplos de diseño sin servidor: aplicaciones sin servidor'
description: Comprenda la variedad de escenarios admitidos por las arquitecturas sin servidor, desde la programación y el procesamiento basado en eventos hasta los desencadenadores de archivos y el proceso de flujo.
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 096dce6ef23bde5ef9c6ca65769f4dcc7e08a904
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "69577198"
---
# <a name="serverless-design-examples"></a><span data-ttu-id="609bc-103">Ejemplos de diseño sin servidor</span><span class="sxs-lookup"><span data-stu-id="609bc-103">Serverless design examples</span></span>

<span data-ttu-id="609bc-104">Existen muchos patrones de diseño que existen para sin servidor.</span><span class="sxs-lookup"><span data-stu-id="609bc-104">There are many design patterns that exist for serverless.</span></span> <span data-ttu-id="609bc-105">En esta sección se capturan algunos escenarios comunes en los que se usa sin servidor.</span><span class="sxs-lookup"><span data-stu-id="609bc-105">This section captures some common scenarios that use serverless.</span></span> <span data-ttu-id="609bc-106">Lo que todos los ejemplos tienen en común es la combinación fundamental de un desencadenador de eventos y una lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="609bc-106">What all of the examples have in common is the fundamental combination of an event trigger and business logic.</span></span>

## <a name="scheduling"></a><span data-ttu-id="609bc-107">Programación</span><span class="sxs-lookup"><span data-stu-id="609bc-107">Scheduling</span></span>

<span data-ttu-id="609bc-108">La programación de tareas es una función común.</span><span class="sxs-lookup"><span data-stu-id="609bc-108">Scheduling tasks is a common function.</span></span> <span data-ttu-id="609bc-109">En el diagrama siguiente se muestra una base de datos heredada que no tiene comprobaciones de integridad adecuadas.</span><span class="sxs-lookup"><span data-stu-id="609bc-109">The following diagram shows a legacy database that doesn't have appropriate integrity checks.</span></span> <span data-ttu-id="609bc-110">La base de datos debe borrarse periódicamente.</span><span class="sxs-lookup"><span data-stu-id="609bc-110">The database must be scrubbed periodically.</span></span> <span data-ttu-id="609bc-111">La función sin servidor busca datos no válidos y los limpia.</span><span class="sxs-lookup"><span data-stu-id="609bc-111">The serverless function finds invalid data and cleans it.</span></span> <span data-ttu-id="609bc-112">El desencadenador es un temporizador que ejecuta el código según una programación.</span><span class="sxs-lookup"><span data-stu-id="609bc-112">The trigger is a timer that runs the code on a schedule.</span></span>

![Programación sin servidor](./media/serverless-scheduling.png)

## <a name="command-and-query-responsibility-segregation-cqrs"></a><span data-ttu-id="609bc-114">Segregación de responsabilidades de comandos y consultas (CQRS)</span><span class="sxs-lookup"><span data-stu-id="609bc-114">Command and Query Responsibility Segregation (CQRS)</span></span>

<span data-ttu-id="609bc-115">Segregación de responsabilidades de comandos y consultas (CQRS) es un patrón que proporciona diferentes interfaces para leer (o consultar) los datos y las operaciones que modifican los datos.</span><span class="sxs-lookup"><span data-stu-id="609bc-115">Command and Query Responsibility Segregation (CQRS) is a pattern that provides different interfaces for reading (or querying) data and operations that modify data.</span></span> <span data-ttu-id="609bc-116">Soluciona varios problemas comunes.</span><span class="sxs-lookup"><span data-stu-id="609bc-116">It addresses several common problems.</span></span> <span data-ttu-id="609bc-117">En los sistemas basados en creación de eliminación de actualización de lectura (CRUD) tradicionales, pueden surgir conflictos del gran volumen de lecturas y escrituras en el mismo almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="609bc-117">In traditional Create Read Update Delete (CRUD) based systems, conflicts can arise from high volume of both reads and writes to the same data store.</span></span> <span data-ttu-id="609bc-118">El bloqueo puede producirse con frecuencia y ralentizar considerablemente las lecturas.</span><span class="sxs-lookup"><span data-stu-id="609bc-118">Locking may frequently occur and dramatically slow down reads.</span></span> <span data-ttu-id="609bc-119">A menudo, los datos se presentan como una composición de varios objetos de dominio y las operaciones de lectura deben combinar datos de distintas entidades.</span><span class="sxs-lookup"><span data-stu-id="609bc-119">Often, data is presented as a composite of several domain objects and read operations must combine data from different entities.</span></span>

<span data-ttu-id="609bc-120">Con CQRS, una lectura puede implicar una entidad especial "plana" que modela los datos de la forma en que se consumen.</span><span class="sxs-lookup"><span data-stu-id="609bc-120">Using CQRS, a read might involve a special "flattened" entity that models data the way it's consumed.</span></span> <span data-ttu-id="609bc-121">La lectura se trata de forma diferente a como se almacena.</span><span class="sxs-lookup"><span data-stu-id="609bc-121">The read is handled differently than how it's stored.</span></span> <span data-ttu-id="609bc-122">Por ejemplo, aunque la base de datos puede almacenar un contacto como un registro de encabezado con un registro de dirección secundario, la lectura puede implicar una entidad con propiedades de encabezado y dirección.</span><span class="sxs-lookup"><span data-stu-id="609bc-122">For example, although the database may store a contact as a header record with a child address record, the read could involve an entity with both header and address properties.</span></span> <span data-ttu-id="609bc-123">Existen infinidad de métodos para crear el modelo de lectura.</span><span class="sxs-lookup"><span data-stu-id="609bc-123">There are myriad approaches to creating the read model.</span></span> <span data-ttu-id="609bc-124">Podría materializarse a partir de vistas.</span><span class="sxs-lookup"><span data-stu-id="609bc-124">It might be materialized from views.</span></span> <span data-ttu-id="609bc-125">Las operaciones de actualización se pueden encapsular como eventos aislados que luego desencadenan actualizaciones en dos modelos diferentes.</span><span class="sxs-lookup"><span data-stu-id="609bc-125">Update operations could be encapsulated as isolated events that then trigger updates to two different models.</span></span> <span data-ttu-id="609bc-126">Existen modelos independientes para lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="609bc-126">Separate models exist for reading and writing.</span></span>

![Ejemplo de CQRS](./media/cqrs-example.png)

<span data-ttu-id="609bc-128">Sin servidor puede alojar el patrón CQRS proporcionando los puntos de conexión segregados.</span><span class="sxs-lookup"><span data-stu-id="609bc-128">Serverless can accommodate the CQRS pattern by providing the segregated endpoints.</span></span> <span data-ttu-id="609bc-129">Una función sin servidor admite consultas o lecturas, y una función o un conjunto de funciones sin servidor diferente controla las operaciones de actualización.</span><span class="sxs-lookup"><span data-stu-id="609bc-129">One serverless function accommodates queries or reads, and a different serverless function or set of functions handles update operations.</span></span> <span data-ttu-id="609bc-130">Una función sin servidor también puede ser responsable de mantener actualizado el modelo de lectura y se puede desencadenar mediante la [fuente de cambios](https://docs.microsoft.com/azure/cosmos-db/change-feed)de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="609bc-130">A serverless function may also be responsible for keeping the read model up-to-date, and can be triggered by the database's [change feed](https://docs.microsoft.com/azure/cosmos-db/change-feed).</span></span> <span data-ttu-id="609bc-131">El desarrollo de front-end se ha simplificado para conectarse a los puntos de conexión necesarios.</span><span class="sxs-lookup"><span data-stu-id="609bc-131">Front-end development is simplified to connecting to the necessary endpoints.</span></span> <span data-ttu-id="609bc-132">El procesamiento de eventos se controla en el back-end.</span><span class="sxs-lookup"><span data-stu-id="609bc-132">Processing of events is handled on the back end.</span></span> <span data-ttu-id="609bc-133">Este modelo también se escala bien para los proyectos de gran tamaño porque diferentes equipos pueden trabajar en diferentes operaciones.</span><span class="sxs-lookup"><span data-stu-id="609bc-133">This model also scales well for large projects because different teams may work on different operations.</span></span>

## <a name="event-based-processing"></a><span data-ttu-id="609bc-134">Procesamiento basado en eventos</span><span class="sxs-lookup"><span data-stu-id="609bc-134">Event-based processing</span></span>

<span data-ttu-id="609bc-135">En los sistemas basados en mensajes, los eventos se suelen recopilar en las colas o en los temas del publicador y del suscriptor sobre los que se va a actuar.</span><span class="sxs-lookup"><span data-stu-id="609bc-135">In message-based systems, events are often collected in queues or publisher/subscriber topics to be acted upon.</span></span> <span data-ttu-id="609bc-136">Estos eventos pueden desencadenar funciones sin servidor para ejecutar una lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="609bc-136">These events can trigger serverless functions to execute a piece of business logic.</span></span> <span data-ttu-id="609bc-137">Un ejemplo de procesamiento basado en eventos son los sistemas de origen de eventos.</span><span class="sxs-lookup"><span data-stu-id="609bc-137">An example of event-based processing is event-sourced systems.</span></span> <span data-ttu-id="609bc-138">Se genera un "evento" para marcar una tarea como completada.</span><span class="sxs-lookup"><span data-stu-id="609bc-138">An "event" is raised to mark a task as complete.</span></span> <span data-ttu-id="609bc-139">Una función sin servidor desencadenada por el evento actualiza el documento de base de datos adecuado.</span><span class="sxs-lookup"><span data-stu-id="609bc-139">A serverless function triggered by the event updates the appropriate database document.</span></span> <span data-ttu-id="609bc-140">Una segunda función sin servidor puede usar el evento para actualizar el modelo de lectura para el sistema.</span><span class="sxs-lookup"><span data-stu-id="609bc-140">A second serverless function may use the event to update the read model for the system.</span></span> <span data-ttu-id="609bc-141">[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) proporciona una manera de integrar eventos con funciones como suscriptores.</span><span class="sxs-lookup"><span data-stu-id="609bc-141">[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) provides a way to integrate events with functions as subscribers.</span></span>

> <span data-ttu-id="609bc-142">Los eventos son mensajes informativos.</span><span class="sxs-lookup"><span data-stu-id="609bc-142">Events are informational messages.</span></span> <span data-ttu-id="609bc-143">Para obtener más información, vea [patrón Event Sourcing](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing).</span><span class="sxs-lookup"><span data-stu-id="609bc-143">For more information, see [Event Sourcing pattern](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing).</span></span>

## <a name="file-triggers-and-transformations"></a><span data-ttu-id="609bc-144">Desencadenadores y transformaciones de archivos</span><span class="sxs-lookup"><span data-stu-id="609bc-144">File triggers and transformations</span></span>

<span data-ttu-id="609bc-145">Extraer, transformar y cargar (ETL) es una función empresarial común.</span><span class="sxs-lookup"><span data-stu-id="609bc-145">Extract, Transform, and Load (ETL) is a common business function.</span></span> <span data-ttu-id="609bc-146">Sin servidor es una excelente solución para ETL porque permite que el código se desencadene como parte de una canalización.</span><span class="sxs-lookup"><span data-stu-id="609bc-146">Serverless is a great solution for ETL because it allows code to be triggered as part of a pipeline.</span></span> <span data-ttu-id="609bc-147">Los componentes de código individuales pueden abordar diversos aspectos.</span><span class="sxs-lookup"><span data-stu-id="609bc-147">Individual code components can address various aspects.</span></span> <span data-ttu-id="609bc-148">Una función sin servidor puede descargar el archivo, otro aplica la transformación y otro carga los datos.</span><span class="sxs-lookup"><span data-stu-id="609bc-148">One serverless function may download the file, another applies the transformation, and another loads the data.</span></span> <span data-ttu-id="609bc-149">El código se puede probar e implementar de forma independiente, lo que facilita el mantenimiento y el escalado cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="609bc-149">The code can be tested and deployed independently, making it easier to maintain and scale where needed.</span></span>

![Desencadenadores y transformaciones de archivos sin servidor](./media/serverless-file-triggers.png)

<span data-ttu-id="609bc-151">En el diagrama, "almacenamiento de acceso esporádico" proporciona los datos que se analizan en [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="609bc-151">In the diagram, "cool storage" provides data that is parsed in [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics).</span></span> <span data-ttu-id="609bc-152">Cualquier problema encontrado en el flujo de datos desencadena una función de Azure para resolver la anomalía.</span><span class="sxs-lookup"><span data-stu-id="609bc-152">Any issues encountered in the data stream trigger an Azure Function to address the anomaly.</span></span>

## <a name="asynchronous-background-processing-and-messaging"></a><span data-ttu-id="609bc-153">Procesamiento y mensajería en segundo plano asíncronos</span><span class="sxs-lookup"><span data-stu-id="609bc-153">Asynchronous background processing and messaging</span></span>

<span data-ttu-id="609bc-154">La mensajería asincrónica y el procesamiento en segundo plano permiten a las aplicaciones iniciar procesos sin tener que esperar.</span><span class="sxs-lookup"><span data-stu-id="609bc-154">Asynchronous messaging and background processing allow applications to kick off processes without having to wait.</span></span> <span data-ttu-id="609bc-155">Un ejemplo de procesamiento asincrónico es una aplicación de OCR.</span><span class="sxs-lookup"><span data-stu-id="609bc-155">An example of asynchronous processing is an OCR app.</span></span> <span data-ttu-id="609bc-156">Se envía una imagen y se pone en cola para su procesamiento.</span><span class="sxs-lookup"><span data-stu-id="609bc-156">An image is submitted and queued for processing.</span></span> <span data-ttu-id="609bc-157">El análisis de la imagen para extraer texto puede tardar tiempo y, una vez finalizada, se envía una notificación.</span><span class="sxs-lookup"><span data-stu-id="609bc-157">Scanning the image to extract text may take time, and once it's finished a notification is sent.</span></span> <span data-ttu-id="609bc-158">Sin servidor puede controlar tanto la invocación como el resultado en este escenario.</span><span class="sxs-lookup"><span data-stu-id="609bc-158">Serverless can handle both the invocation and the result in this scenario.</span></span>

## <a name="web-apps-and-apis"></a><span data-ttu-id="609bc-159">Web Apps y API</span><span class="sxs-lookup"><span data-stu-id="609bc-159">Web apps and APIs</span></span>

<span data-ttu-id="609bc-160">Un escenario popular de sin servidor son las aplicaciones de N niveles, que suelen ser donde la capa de la interfaz de usuario es una aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="609bc-160">A popular scenario for serverless is N-tier applications, most commonly ones where the UI layer is a web app.</span></span> <span data-ttu-id="609bc-161">La popularidad de las aplicaciones de una sola página (SPA) ha sobrecargado recientemente.</span><span class="sxs-lookup"><span data-stu-id="609bc-161">The popularity of Single Page Applications (SPA) has surged recently.</span></span> <span data-ttu-id="609bc-162">Las aplicaciones de SPA representan una sola página y luego confían en las llamadas API y los datos devueltos para representar dinámicamente la nueva interfaz de usuario sin volver a cargar una página completa.</span><span class="sxs-lookup"><span data-stu-id="609bc-162">SPA apps render a single page, then rely on API calls and the returned data to dynamically render new UI without reloading a full page.</span></span> <span data-ttu-id="609bc-163">La representación del lado cliente proporciona una aplicación mucho más rápida y con mayor capacidad de respuesta para el usuario final.</span><span class="sxs-lookup"><span data-stu-id="609bc-163">Client-side rendering provides a much faster, more responsive application to the end user.</span></span>

<span data-ttu-id="609bc-164">Los puntos de conexión sin servidor desencadenados por llamadas HTTP se pueden usar para controlar las solicitudes de API.</span><span class="sxs-lookup"><span data-stu-id="609bc-164">Serverless endpoints triggered by HTTP calls can be used to handle the API requests.</span></span> <span data-ttu-id="609bc-165">Por ejemplo, una empresa de servicios de ad puede llamar a una función sin servidor con información de Perfil de usuario para solicitar publicidad personalizada.</span><span class="sxs-lookup"><span data-stu-id="609bc-165">For example, an ad services company may call a serverless function with user profile information to request custom advertising.</span></span> <span data-ttu-id="609bc-166">La función sin servidor devuelve el anuncio personalizado y la página web lo representa.</span><span class="sxs-lookup"><span data-stu-id="609bc-166">The serverless function returns the custom ad and the web page renders it.</span></span>

![API Web sin servidor](./media/serverless-web-api.png)

## <a name="data-pipeline"></a><span data-ttu-id="609bc-168">Canalización de datos</span><span class="sxs-lookup"><span data-stu-id="609bc-168">Data pipeline</span></span>

<span data-ttu-id="609bc-169">Las funciones sin servidor se pueden usar para facilitar una canalización de datos.</span><span class="sxs-lookup"><span data-stu-id="609bc-169">Serverless functions can be used to facilitate a data pipeline.</span></span> <span data-ttu-id="609bc-170">En este ejemplo, un archivo desencadena una función para convertir los datos de un archivo CSV en filas de datos de una tabla.</span><span class="sxs-lookup"><span data-stu-id="609bc-170">In this example, a file triggers a function to translate data in a CSV file to data rows in a table.</span></span> <span data-ttu-id="609bc-171">La tabla organizada permite que un panel de Power BI presente el análisis al usuario final.</span><span class="sxs-lookup"><span data-stu-id="609bc-171">The organized table allows a Power BI dashboard to present analytics to the end user.</span></span>

![Canalización de datos sin servidor](./media/serverless-data-pipeline.png)

## <a name="stream-processing"></a><span data-ttu-id="609bc-173">Procesamiento de flujos</span><span class="sxs-lookup"><span data-stu-id="609bc-173">Stream processing</span></span>

<span data-ttu-id="609bc-174">Los dispositivos y sensores suelen generar flujos de datos que se deben procesar en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="609bc-174">Devices and sensors often generate streams of data that must be processed in real time.</span></span> <span data-ttu-id="609bc-175">Hay una serie de tecnologías que pueden capturar mensajes y secuencias de [Event hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs) y [IOT Hub](https://docs.microsoft.com/azure/iot-hub) a [Service Bus](https://docs.microsoft.com/azure/service-bus).</span><span class="sxs-lookup"><span data-stu-id="609bc-175">There are a number of technologies that can capture messages and streams from [Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs) and [IoT Hub](https://docs.microsoft.com/azure/iot-hub) to [Service Bus](https://docs.microsoft.com/azure/service-bus).</span></span> <span data-ttu-id="609bc-176">Independientemente del transporte, el modo sin servidor es un mecanismo ideal para procesar los mensajes y los flujos de datos a medida que llegan.</span><span class="sxs-lookup"><span data-stu-id="609bc-176">Regardless of transport, serverless is an ideal mechanism for processing the messages and streams of data as they come in.</span></span> <span data-ttu-id="609bc-177">Sin servidor se puede escalar rápidamente para satisfacer la demanda de grandes volúmenes de datos.</span><span class="sxs-lookup"><span data-stu-id="609bc-177">Serverless can scale quickly to meet the demand of large volumes of data.</span></span> <span data-ttu-id="609bc-178">El código sin servidor puede aplicar la lógica de negocios para analizar los datos y la salida en un formato estructurado para la acción y el análisis.</span><span class="sxs-lookup"><span data-stu-id="609bc-178">The serverless code can apply business logic to parse the data and output in a structured format for action and analytics.</span></span>

![Procesamiento de flujos sin servidor](./media/serverless-stream-processing.png)

## <a name="api-gateway"></a><span data-ttu-id="609bc-180">Puerta de enlace de API</span><span class="sxs-lookup"><span data-stu-id="609bc-180">API gateway</span></span>

<span data-ttu-id="609bc-181">Una puerta de enlace de API proporciona un único punto de entrada para los clientes y, a continuación, enruta de forma inteligente las solicitudes a los servicios back-end.</span><span class="sxs-lookup"><span data-stu-id="609bc-181">An API gateway provides a single point of entry for clients and then intelligently routes requests to back-end services.</span></span> <span data-ttu-id="609bc-182">Resulta útil para administrar grandes conjuntos de servicios.</span><span class="sxs-lookup"><span data-stu-id="609bc-182">It's useful to manage large sets of services.</span></span> <span data-ttu-id="609bc-183">También puede controlar el control de versiones y simplificar el desarrollo mediante la conexión sencilla de clientes a entornos dispares.</span><span class="sxs-lookup"><span data-stu-id="609bc-183">It can also handle versioning and simplify development by easily connecting clients to disparate environments.</span></span> <span data-ttu-id="609bc-184">Sin servidor puede controlar el escalado de back-end de microservicios individuales, a la vez que presenta un único front-end a través de una puerta de enlace de API.</span><span class="sxs-lookup"><span data-stu-id="609bc-184">Serverless can handle back-end scaling of individual microservices while presenting a single front end via an API gateway.</span></span>

![Puerta de enlace de API sin servidor](./media/serverless-api-gateway.png)

## <a name="recommended-resources"></a><span data-ttu-id="609bc-186">Recursos recomendados</span><span class="sxs-lookup"><span data-stu-id="609bc-186">Recommended resources</span></span>

* [<span data-ttu-id="609bc-187">Azure Event Grid</span><span class="sxs-lookup"><span data-stu-id="609bc-187">Azure Event Grid</span></span>](https://docs.microsoft.com/azure/event-grid/overview)
* [<span data-ttu-id="609bc-188">Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="609bc-188">Azure IoT Hub</span></span>](https://docs.microsoft.com/azure/iot-hub)
* [<span data-ttu-id="609bc-189">Desafíos y soluciones de la administración de datos distribuidos</span><span class="sxs-lookup"><span data-stu-id="609bc-189">Challenges and solutions for distributed data management</span></span>](../microservices/architect-microservice-container-applications/distributed-data-management.md)
* [<span data-ttu-id="609bc-190">Diseño de microservicios: identificación de los límites de microservicios</span><span class="sxs-lookup"><span data-stu-id="609bc-190">Designing microservices: identifying microservice boundaries</span></span>](https://docs.microsoft.com/azure/architecture/microservices/microservice-boundaries)
* [<span data-ttu-id="609bc-191">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="609bc-191">Event Hubs</span></span>](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs)
* [<span data-ttu-id="609bc-192">Patrón Event Sourcing</span><span class="sxs-lookup"><span data-stu-id="609bc-192">Event Sourcing pattern</span></span>](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)
* [<span data-ttu-id="609bc-193">Implementar el patrón de interruptor</span><span class="sxs-lookup"><span data-stu-id="609bc-193">Implementing the Circuit Breaker pattern</span></span>](../microservices/implement-resilient-applications/implement-circuit-breaker-pattern.md)
* [<span data-ttu-id="609bc-194">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="609bc-194">IoT Hub</span></span>](https://docs.microsoft.com/azure/iot-hub)
* [<span data-ttu-id="609bc-195">Service Bus</span><span class="sxs-lookup"><span data-stu-id="609bc-195">Service Bus</span></span>](https://docs.microsoft.com/azure/service-bus)
* [<span data-ttu-id="609bc-196">Trabajar con la compatibilidad con la fuente de cambios en Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="609bc-196">Working with the change feed support in Azure Cosmos DB</span></span>](https://docs.microsoft.com/azure/cosmos-db/change-feed)

>[!div class="step-by-step"]
><span data-ttu-id="609bc-197">[Anterior](serverless-architecture-considerations.md)
>[Siguiente](azure-serverless-platform.md)</span><span class="sxs-lookup"><span data-stu-id="609bc-197">[Previous](serverless-architecture-considerations.md)
[Next](azure-serverless-platform.md)</span></span>