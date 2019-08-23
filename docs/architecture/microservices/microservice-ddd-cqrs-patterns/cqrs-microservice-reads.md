---
title: Implementación de lecturas/consultas en un microservicio CQRS
description: Arquitectura de microservicios de .NET para aplicaciones .NET en contenedor | Información sobre la implementación del lado de consultas de CQRS en el microservicio Ordering en eShopOnContainers mediante Dapper.
ms.date: 10/08/2018
ms.openlocfilehash: f791546e2fc00e276ab55302802a5534465ace58
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674342"
---
# <a name="implement-readsqueries-in-a-cqrs-microservice"></a><span data-ttu-id="2b789-103">Implementación de lecturas/consultas en un microservicio CQRS</span><span class="sxs-lookup"><span data-stu-id="2b789-103">Implement reads/queries in a CQRS microservice</span></span>

<span data-ttu-id="2b789-104">Para lecturas/consultas, el microservicio de pedidos (Ordering) de la aplicación de referencia eShopOnContainers implementa las consultas de manera independiente del modelo DDD y el área transaccional.</span><span class="sxs-lookup"><span data-stu-id="2b789-104">For reads/queries, the ordering microservice from the eShopOnContainers reference application implements the queries independently from the DDD model and transactional area.</span></span> <span data-ttu-id="2b789-105">Esto se hacía principalmente porque las demandas de consultas y transacciones son muy diferentes.</span><span class="sxs-lookup"><span data-stu-id="2b789-105">This was done primarily because the demands for queries and for transactions are drastically different.</span></span> <span data-ttu-id="2b789-106">Las escrituras ejecutan transacciones que deben ser compatibles con la lógica del dominio.</span><span class="sxs-lookup"><span data-stu-id="2b789-106">Writes execute transactions that must be compliant with the domain logic.</span></span> <span data-ttu-id="2b789-107">Por otro lado, las consultas son idempotentes y se pueden segregar de las reglas de dominio.</span><span class="sxs-lookup"><span data-stu-id="2b789-107">Queries, on the other hand, are idempotent and can be segregated from the domain rules.</span></span>

<span data-ttu-id="2b789-108">El enfoque es sencillo, como se muestra en la figura 7-3.</span><span class="sxs-lookup"><span data-stu-id="2b789-108">The approach is simple, as shown in Figure 7-3.</span></span> <span data-ttu-id="2b789-109">La interfaz API se implementa mediante los controladores de API Web con cualquier infraestructura, como un microasignador objeto-relacional (ORM) como Dapper, y devolviendo ViewModel dinámicos según las necesidades de las aplicaciones de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="2b789-109">The API interface is implemented by the Web API controllers using any infrastructure, such as a micro Object Relational Mapper (ORM) like Dapper, and returning dynamic ViewModels depending on the needs of the UI applications.</span></span>

![El enfoque más sencillo para el lado de las consultas en un enfoque CQRS simplificado se puede implementar simplemente consultando la base de datos con un Micro-ORM como Dapper, devolviendo ViewModel dinámicos.](./media/image3.png)

<span data-ttu-id="2b789-111">**Figura 7-3**.</span><span class="sxs-lookup"><span data-stu-id="2b789-111">**Figure 7-3**.</span></span> <span data-ttu-id="2b789-112">El enfoque más sencillo para las consultas en un microservicio CQRS</span><span class="sxs-lookup"><span data-stu-id="2b789-112">The simplest approach for queries in a CQRS microservice</span></span>

<span data-ttu-id="2b789-113">Este es el enfoque más sencillo posible para las consultas.</span><span class="sxs-lookup"><span data-stu-id="2b789-113">This is the simplest possible approach for queries.</span></span> <span data-ttu-id="2b789-114">Las definiciones de consulta realizan una consulta a la base de datos y devuelven un ViewModel dinámico creado sobre la marcha para cada consulta.</span><span class="sxs-lookup"><span data-stu-id="2b789-114">The query definitions query the database and return a dynamic ViewModel built on the fly for each query.</span></span> <span data-ttu-id="2b789-115">Puesto que las consultas son idempotentes, no cambian los datos por muchas veces que ejecute una consulta.</span><span class="sxs-lookup"><span data-stu-id="2b789-115">Since the queries are idempotent, they won't change the data no matter how many times you run a query.</span></span> <span data-ttu-id="2b789-116">Por lo tanto, no es necesario estar restringido por un patrón DDD usado en el lado transaccional, como agregados y otros patrones, y por eso las consultas se separan del área transaccional.</span><span class="sxs-lookup"><span data-stu-id="2b789-116">Therefore, you don't need to be restricted by any DDD pattern used in the transactional side, like aggregates and other patterns, and that is why queries are separated from the transactional area.</span></span> <span data-ttu-id="2b789-117">Basta con consultar la base de datos para obtener los datos que necesita la interfaz de usuario y devolver un ViewModel dinámico que no tiene que estar definido estáticamente en ningún lugar (no hay clases para los ViewModel), excepto en las propias instrucciones SQL.</span><span class="sxs-lookup"><span data-stu-id="2b789-117">You simply query the database for the data that the UI needs and return a dynamic ViewModel that does not need to be statically defined anywhere (no classes for the ViewModels) except in the SQL statements themselves.</span></span>

<span data-ttu-id="2b789-118">Puesto que se trata de un método sencillo, el código necesario para el lado de las consultas (como código que usa un micro ORM como [Dapper](https://github.com/StackExchange/Dapper)) pueden implementarse [dentro del mismo proyecto de API Web](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Queries/OrderQueries.cs).</span><span class="sxs-lookup"><span data-stu-id="2b789-118">Since this is a simple approach, the code required for the queries side (such as code using a micro ORM like [Dapper](https://github.com/StackExchange/Dapper)) can be implemented [within the same Web API project](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Queries/OrderQueries.cs).</span></span> <span data-ttu-id="2b789-119">Esto se muestra en la Figura 7-4.</span><span class="sxs-lookup"><span data-stu-id="2b789-119">Figure 7-4 shows this.</span></span> <span data-ttu-id="2b789-120">Las consultas se definen en el proyecto de microservicio **Ordering.API** dentro de la solución eShopOnContainers.</span><span class="sxs-lookup"><span data-stu-id="2b789-120">The queries are defined in the **Ordering.API** microservice project within the eShopOnContainers solution.</span></span>

![Vista del Explorador de soluciones del proyecto Ordering.API, en la que se muestra la carpeta Aplicación > Consultas.](./media/image4.png)

<span data-ttu-id="2b789-122">**Figura 7-4**.</span><span class="sxs-lookup"><span data-stu-id="2b789-122">**Figure 7-4**.</span></span> <span data-ttu-id="2b789-123">Consultas (Queries) en el microservicio de pedidos (Ordering) en eShopOnContainers</span><span class="sxs-lookup"><span data-stu-id="2b789-123">Queries in the Ordering microservice in eShopOnContainers</span></span>

## <a name="use-viewmodels-specifically-made-for-client-apps-independent-from-domain-model-constraints"></a><span data-ttu-id="2b789-124">Uso de ViewModel específicos para aplicaciones de cliente, sin las restricciones del modelo de dominio</span><span class="sxs-lookup"><span data-stu-id="2b789-124">Use ViewModels specifically made for client apps, independent from domain model constraints</span></span>

<span data-ttu-id="2b789-125">Dado que las consultas se realizan para obtener los datos que necesitan para las aplicaciones cliente, el tipo de valor devuelto puede estar hecho específicamente para los clientes, en función de los datos devueltos por las consultas.</span><span class="sxs-lookup"><span data-stu-id="2b789-125">Since the queries are performed to obtain the data needed by the client applications, the returned type can be specifically made for the clients, based on the data returned by the queries.</span></span> <span data-ttu-id="2b789-126">Estos modelos, u objetos de transferencia de datos (DTO), se denominan ViewModel.</span><span class="sxs-lookup"><span data-stu-id="2b789-126">These models, or Data Transfer Objects (DTOs), are called ViewModels.</span></span>

<span data-ttu-id="2b789-127">Los datos devueltos (ViewModel) pueden ser el resultado de combinar datos de varias entidades o tablas de la base de datos, o incluso de varios agregados definidos en el modelo de dominio para el área transaccional.</span><span class="sxs-lookup"><span data-stu-id="2b789-127">The returned data (ViewModel) can be the result of joining data from multiple entities or tables in the database, or even across multiple aggregates defined in the domain model for the transactional area.</span></span> <span data-ttu-id="2b789-128">En este caso, dado que va a crear consultas independientes del modelo de dominio, se ignoran completamente las restricciones y los límites de agregados, y se pueden consultar cualquier tabla y columna que necesite.</span><span class="sxs-lookup"><span data-stu-id="2b789-128">In this case, because you are creating queries independent of the domain model, the aggregates boundaries and constraints are completely ignored and you're free to query any table and column you might need.</span></span> <span data-ttu-id="2b789-129">Este enfoque proporciona gran flexibilidad y productividad a los desarrolladores que crean o actualizan las consultas.</span><span class="sxs-lookup"><span data-stu-id="2b789-129">This approach provides great flexibility and productivity for the developers creating or updating the queries.</span></span>

<span data-ttu-id="2b789-130">Los ViewModel pueden ser tipos estáticos definidos en las clases.</span><span class="sxs-lookup"><span data-stu-id="2b789-130">The ViewModels can be static types defined in classes.</span></span> <span data-ttu-id="2b789-131">O bien, se pueden crear dinámicamente en función de las consultas realizadas (tal y como se implementa en el microservicio de pedidos), lo que resulta muy ágil para los desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="2b789-131">Or they can be created dynamically based on the queries performed (as is implemented in the ordering microservice), which is very agile for developers.</span></span>

## <a name="use-dapper-as-a-micro-orm-to-perform-queries"></a><span data-ttu-id="2b789-132">Uso de Dapper como micro ORM para realizar consultas</span><span class="sxs-lookup"><span data-stu-id="2b789-132">Use Dapper as a micro ORM to perform queries</span></span> 

<span data-ttu-id="2b789-133">Para la consulta puede usar cualquier micro ORM, Entity Framework Core o incluso ADO.NET estándar.</span><span class="sxs-lookup"><span data-stu-id="2b789-133">You can use any micro ORM, Entity Framework Core, or even plain ADO.NET for querying.</span></span> <span data-ttu-id="2b789-134">En la aplicación de ejemplo, se seleccionó Dapper para el microservicio de pedidos en eShopOnContainers como un buen ejemplo de un micro ORM popular.</span><span class="sxs-lookup"><span data-stu-id="2b789-134">In the sample application, Dapper was selected for the ordering microservice in eShopOnContainers as a good example of a popular micro ORM.</span></span> <span data-ttu-id="2b789-135">Dapper puede ejecutar consultas SQL estándar con un gran rendimiento, porque es un marco de trabajo muy ligero.</span><span class="sxs-lookup"><span data-stu-id="2b789-135">It can run plain SQL queries with great performance, because it's a very light framework.</span></span> <span data-ttu-id="2b789-136">Con Dapper, se puede escribir una consulta SQL que puede acceder a varias tablas y combinarlas.</span><span class="sxs-lookup"><span data-stu-id="2b789-136">Using Dapper, you can write a SQL query that can access and join multiple tables.</span></span>

<span data-ttu-id="2b789-137">Dapper es un proyecto de código abierto (creado originalmente por Sam Saffron) y forma parte de los bloques de creación que se usan en [Stack Overflow](https://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2b789-137">Dapper is an open-source project (original created by Sam Saffron), and is part of the building blocks used in [Stack Overflow](https://stackoverflow.com/).</span></span> <span data-ttu-id="2b789-138">Para usar Dapper, solo hay que instalarlo a través del [paquete Dapper de NuGet](https://www.nuget.org/packages/Dapper), tal y como se muestra en la ilustración siguiente:</span><span class="sxs-lookup"><span data-stu-id="2b789-138">To use Dapper, you just need to install it through the [Dapper NuGet package](https://www.nuget.org/packages/Dapper), as shown in the following figure:</span></span>

![El paquete Dapper como se ve en la vista de paquetes NuGet administrados en VS.](./media/image4.1.png)

<span data-ttu-id="2b789-140">También debe agregar una instrucción using para que el código tenga acceso a los métodos de extensión de Dapper.</span><span class="sxs-lookup"><span data-stu-id="2b789-140">You also need to add a using statement so your code has access to the Dapper extension methods.</span></span>

<span data-ttu-id="2b789-141">Cuando se utiliza Dapper en el código, se usa directamente la clase <xref:System.Data.SqlClient.SqlConnection> disponible en el espacio de nombres <xref:System.Data.SqlClient>.</span><span class="sxs-lookup"><span data-stu-id="2b789-141">When you use Dapper in your code, you directly use the <xref:System.Data.SqlClient.SqlConnection> class available in the <xref:System.Data.SqlClient> namespace.</span></span> <span data-ttu-id="2b789-142">Mediante el método QueryAsync y otros métodos de extensión que extienden la clase <xref:System.Data.SqlClient.SqlConnection>, simplemente se ejecutan las consultas de una manera sencilla y eficaz.</span><span class="sxs-lookup"><span data-stu-id="2b789-142">Through the QueryAsync method and other extension methods that extend the <xref:System.Data.SqlClient.SqlConnection> class, you can simply run queries in a straightforward and performant way.</span></span>

## <a name="dynamic-versus-static-viewmodels"></a><span data-ttu-id="2b789-143">ViewModel dinámicos frente a estáticos</span><span class="sxs-lookup"><span data-stu-id="2b789-143">Dynamic versus static ViewModels</span></span>

<span data-ttu-id="2b789-144">Cuando se devuelven ViewModel desde el servidor a las aplicaciones cliente, se puede pensar en esos ViewModel como DTO (Objetos de transferencia de datos) que pueden ser diferentes a las entidades de dominio interno de su modelo de entidad, ya que los ViewModel contienen los datos de la forma en que la aplicación cliente necesita.</span><span class="sxs-lookup"><span data-stu-id="2b789-144">When returning ViewModels from the server-side to client apps, you can think about those ViewModels as DTOs (Data Transfer Objects) that can be different to the internal domain entities of your entity model because the ViewModels hold the data the way the client app needs.</span></span> <span data-ttu-id="2b789-145">Por lo tanto, en muchos casos, se pueden agregar datos procedentes de varias entidades de dominio y crear los ViewModel exactamente según la forma en que la aplicación cliente necesita los datos.</span><span class="sxs-lookup"><span data-stu-id="2b789-145">Therefore, in many cases, you can aggregate data coming from multiple domain entities and compose the ViewModels precisely according to how the client app needs that data.</span></span>

<span data-ttu-id="2b789-146">Esos ViewModel o DTO pueden definirse explícitamente (como clases de contenedor de datos) como la clase `OrderSummary` que se muestra en un fragmento de código más adelante, o simplemente se podrían devolver ViewModel o DTO dinámicos únicamente en función de los atributos devueltos por las consultas, como un tipo dinámico.</span><span class="sxs-lookup"><span data-stu-id="2b789-146">Those ViewModels or DTOs can be defined explicitly (as data holder classes) like the `OrderSummary` class shown in a later code snippet, or you could just return dynamic ViewModels or dynamic DTOs simply based on the attributes returned by your queries, as a dynamic type.</span></span>

### <a name="viewmodel-as-dynamic-type"></a><span data-ttu-id="2b789-147">ViewModel como tipo dinámico</span><span class="sxs-lookup"><span data-stu-id="2b789-147">ViewModel as dynamic type</span></span>

<span data-ttu-id="2b789-148">Como se muestra en el siguiente código, las consultas pueden devolver un `ViewModel` directamente al devolver un tipo *dinámico* que internamente se basa en los atributos devueltos por una consulta.</span><span class="sxs-lookup"><span data-stu-id="2b789-148">As shown in the following code, a `ViewModel` can be directly returned by the queries by just returning a *dynamic* type that internally is based on the attributes returned by a query.</span></span> <span data-ttu-id="2b789-149">Esto significa que el subconjunto de atributos que se devuelve se basa en la propia consulta.</span><span class="sxs-lookup"><span data-stu-id="2b789-149">That means that the subset of attributes to be returned is based on the query itself.</span></span> <span data-ttu-id="2b789-150">Por tanto, si se agrega una nueva columna a la consulta o combinación, esos datos se agregan dinámicamente al `ViewModel` devuelto.</span><span class="sxs-lookup"><span data-stu-id="2b789-150">Therefore, if you add a new column to the query or join, that data is dynamically added to the returned `ViewModel`.</span></span>

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
    public async Task<IEnumerable<dynamic>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            return await connection.QueryAsync<dynamic>(
                @"SELECT o.[Id] as ordernumber,
                o.[OrderDate] as [date],os.[Name] as [status],
                SUM(oi.units*oi.unitprice) as total
                FROM [ordering].[Orders] o
                LEFT JOIN[ordering].[orderitems] oi ON o.Id = oi.orderid
                LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                GROUP BY o.[Id], o.[OrderDate], os.[Name]");
        }
    }
}
```

<span data-ttu-id="2b789-151">Lo importante es que, mediante el uso de un tipo dinámico, la colección de datos devuelta dinámicamente se ensambla como un ViewModel.</span><span class="sxs-lookup"><span data-stu-id="2b789-151">The important point is that by using a dynamic type, the returned collection of data is dynamically assembled as the ViewModel.</span></span>

<span data-ttu-id="2b789-152">**Ventajas**: este enfoque reduce la necesidad de modificar las clases estáticas de ViewModel cada vez que se actualice la frase SQL de una consulta, lo que hace que este enfoque de diseño sea bastante ágil a la hora de codificar, sencillo y rápido de evolucionar con respecto a los cambios en el futuro.</span><span class="sxs-lookup"><span data-stu-id="2b789-152">**Pros:** This approach reduces the need to modify static ViewModel classes whenever you update the SQL sentence of a query, making this design approach pretty agile when coding, straightforward, and quick to evolve in regard to future changes.</span></span>

<span data-ttu-id="2b789-153">**Inconvenientes**: a largo plazo, los tipos dinámicos pueden perjudicar a la claridad y afectar a la compatibilidad de un servicio con las aplicaciones cliente.</span><span class="sxs-lookup"><span data-stu-id="2b789-153">**Cons:** In the long term, dynamic types can negatively impact the clarity and the compatibility of a service with client apps.</span></span> <span data-ttu-id="2b789-154">Además, el software middleware como Swashbuckle no puede proporcionar el mismo nivel de documentación en tipos devueltos si se utilizan tipos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="2b789-154">In addition, middleware software like Swashbuckle cannot provide the same level of documentation on returned types if using dynamic types.</span></span>

### <a name="viewmodel-as-predefined-dto-classes"></a><span data-ttu-id="2b789-155">ViewModel como clases DTO predefinidas</span><span class="sxs-lookup"><span data-stu-id="2b789-155">ViewModel as predefined DTO classes</span></span>

<span data-ttu-id="2b789-156">**Ventajas**: disponer de clases ViewModel predefinidas estáticas, como "contratos" basados en clases DTO explícitas, es definitivamente mejor para las API públicas, pero también para los microservicios a largo plazo, incluso si solo los utiliza la misma aplicación.</span><span class="sxs-lookup"><span data-stu-id="2b789-156">**Pros**: Having static predefined ViewModel classes, like “contracts” based on explicit DTO classes, is definitely better for public APIs but also for long term microservices, even if they are only used by the same application.</span></span>

<span data-ttu-id="2b789-157">Si quiere especificar los tipos de respuesta de Swagger, debe utilizar clases DTO explícitas como tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="2b789-157">If you want to specify response types for Swagger, you need to use explicit DTO classes as the return type.</span></span> <span data-ttu-id="2b789-158">Por lo tanto, las clases DTO predefinidas permiten ofrecer información más completa de Swagger.</span><span class="sxs-lookup"><span data-stu-id="2b789-158">Therefore, predefined DTO classes allow you to offer richer information from Swagger.</span></span> <span data-ttu-id="2b789-159">Eso mejora la documentación y la compatibilidad de la API al utilizar una API.</span><span class="sxs-lookup"><span data-stu-id="2b789-159">That improves the API documentation and compatibility when consuming an API.</span></span>

<span data-ttu-id="2b789-160">**Inconvenientes**: tal y como se mencionó anteriormente, al actualizar el código se requieren algunos pasos adicionales para actualizar las clases DTO.</span><span class="sxs-lookup"><span data-stu-id="2b789-160">**Cons**: As mentioned earlier, when updating the code, it takes some more steps to update the DTO classes.</span></span>

<span data-ttu-id="2b789-161">*Sugerencia basada en nuestra experiencia*: en las consultas que se implementan en el microservicio de pedidos en eShopOnContainers, iniciamos el desarrollo con ViewModel dinámicos porque resultaba muy sencillo y ágil en las primeras fases de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2b789-161">*Tip based on our experience*: In the queries implemented at the Ordering microservice in eShopOnContainers, we started developing by using dynamic ViewModels as it was very straightforward and agile on the early development stages.</span></span> <span data-ttu-id="2b789-162">Pero, una vez que se estabilizó el desarrollo, optamos por refactorizar las API y usar DTO estático o predefinido para los ViewModel, porque es más fácil para los consumidores del microservicio conocer los tipos DTO explícitos, utilizados como "contratos".</span><span class="sxs-lookup"><span data-stu-id="2b789-162">But, once the development was stabilized, we chose to refactor the APIs and use static or pre-defined DTOs for the ViewModels, because it is clearer for the microservice’s consumers to know explicit DTO types, used as "contracts".</span></span>

<span data-ttu-id="2b789-163">En el ejemplo siguiente, puede ver cómo la consulta devuelve datos mediante una clase ViewModel DTO explícita: la clase OrderSummary.</span><span class="sxs-lookup"><span data-stu-id="2b789-163">In the following example, you can see how the query is returning data by using an explicit ViewModel DTO class: the OrderSummary class.</span></span>

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
  public async Task<IEnumerable<OrderSummary>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            var result = await connection.QueryAsync<OrderSummary>(
                  @"SELECT o.[Id] as ordernumber, 
                  o.[OrderDate] as [date],os.[Name] as [status], 
                  SUM(oi.units*oi.unitprice) as total
                  FROM [ordering].[Orders] o
                  LEFT JOIN[ordering].[orderitems] oi ON  o.Id = oi.orderid 
                  LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                  GROUP BY o.[Id], o.[OrderDate], os.[Name]
                  ORDER BY o.[Id]");
        }
    } 
}
```

#### <a name="describe-response-types-of-web-apis"></a><span data-ttu-id="2b789-164">Descripción de los tipos de respuesta de las API Web</span><span class="sxs-lookup"><span data-stu-id="2b789-164">Describe response types of Web APIs</span></span>

<span data-ttu-id="2b789-165">Lo que más preocupa a los desarrolladores que utilizan API Web y microservicios es lo que se devuelve, sobre todo los tipos de respuesta y los códigos de error (si no son los habituales).</span><span class="sxs-lookup"><span data-stu-id="2b789-165">Developers consuming web APIs and microservices are most concerned with what is returned — specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="2b789-166">Estos se administran en las anotaciones de datos y en los comentarios XML.</span><span class="sxs-lookup"><span data-stu-id="2b789-166">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="2b789-167">Sin una documentación correcta en la interfaz de usuario de Swagger, el consumidor desconoce los tipos que se devuelven o los códigos HTTP que se pueden devolver.</span><span class="sxs-lookup"><span data-stu-id="2b789-167">Without proper documentation in the Swagger UI, the consumer lacks knowledge of what types are being returned or what HTTP codes can be returned.</span></span> <span data-ttu-id="2b789-168">Este problema se corrige agregando <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute?displayProperty=nameWithType>, para que Swashbuckle pueda generar información completa sobre el modelo de devolución y los valores de API, como se muestra en el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="2b789-168">That problem is fixed by adding the <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute?displayProperty=nameWithType>, so Swashbuckle can generate richer information about the API return model and values, as shown in the following code:</span></span>

```csharp
namespace Microsoft.eShopOnContainers.Services.Ordering.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class OrdersController : Controller
    {
        //Additional code...
        [Route("")]
        [HttpGet]
        [ProducesResponseType(typeof(IEnumerable<OrderSummary>),
            (int)HttpStatusCode.OK)]
        public async Task<IActionResult> GetOrders()
        {
            var userid = _identityService.GetUserIdentity();
            var orders = await _orderQueries
                .GetOrdersFromUserAsync(Guid.Parse(userid));
            return Ok(orders);
        }
    }
}
```

<span data-ttu-id="2b789-169">Pero el atributo `ProducesResponseType` no puede utilizar un tipo dinámico, sino que requiere utilizar tipos explícitos, como ViewModel DTO `OrderSummary`, se mostrado en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2b789-169">However, the `ProducesResponseType` attribute cannot use dynamic as a type but requires to use explicit types, like the `OrderSummary` ViewModel DTO, shown in the following example:</span></span>

```csharp
public class OrderSummary
{
    public int ordernumber { get; set; }
    public DateTime date { get; set; }
    public string status { get; set; }
    public double total { get; set; }
}
```

<span data-ttu-id="2b789-170">Este es otro de los motivos por los que, a largo plazo, los tipos explícitos son mejores que los tipos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="2b789-170">This is another reason why explicit returned types are better than dynamic types, in the long term.</span></span> <span data-ttu-id="2b789-171">Cuando se usa el atributo `ProducesResponseType`, también se puede especificar cuál es el resultado esperado en lo que respecta a posibles errores/códigos HTTP, como 200, 400, etc.</span><span class="sxs-lookup"><span data-stu-id="2b789-171">When using the `ProducesResponseType` attribute, you can also specify what is the expected outcome in regards possible HTTP errors/codes, like 200, 400, etc.</span></span>

<span data-ttu-id="2b789-172">En la siguiente imagen, se puede ver cómo la interfaz de usuario de Swagger de interfaz de usuario muestra la información de ResponseType.</span><span class="sxs-lookup"><span data-stu-id="2b789-172">In the following image, you can see how Swagger UI shows the ResponseType information.</span></span>

![Vista del explorador de la página de IU de Swagger para la API de Ordering.](./media/image5.png)

<span data-ttu-id="2b789-174">**Figura 7-5**.</span><span class="sxs-lookup"><span data-stu-id="2b789-174">**Figure 7-5**.</span></span> <span data-ttu-id="2b789-175">Interfaz de usuario de Swagger que muestra los tipos de respuesta y los posibles códigos de estado HTTP de una API Web</span><span class="sxs-lookup"><span data-stu-id="2b789-175">Swagger UI showing response types and possible HTTP status codes from a Web API</span></span>

<span data-ttu-id="2b789-176">En la ilustración anterior se pueden ver algunos valores de ejemplo basados en los tipos ViewModel, además de los posibles códigos de estado HTTP que se pueden devolver.</span><span class="sxs-lookup"><span data-stu-id="2b789-176">You can see in the image above some example values based on the ViewModel types plus the possible HTTP status codes that can be returned.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b789-177">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2b789-177">Additional resources</span></span>

- <span data-ttu-id="2b789-178">**Dapper** </span><span class="sxs-lookup"><span data-stu-id="2b789-178">**Dapper** </span></span>\
 <https://github.com/StackExchange/dapper-dot-net>

- <span data-ttu-id="2b789-179">**Julie Lerman. Puntos de datos: Dapper, Entity Framework y aplicaciones híbridas** </span><span class="sxs-lookup"><span data-stu-id="2b789-179">**Julie Lerman. Data Points - Dapper, Entity Framework and Hybrid Apps (MSDN Mag. article)** </span></span>\
  <https://msdn.microsoft.com/magazine/mt703432.aspx>

- <span data-ttu-id="2b789-180">**Páginas de ayuda de ASP.NET Core Web API con Swagger/Open API** </span><span class="sxs-lookup"><span data-stu-id="2b789-180">**ASP.NET Core Web API Help Pages using Swagger** </span></span>\
  <https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger?tabs=visual-studio>

>[!div class="step-by-step"]
><span data-ttu-id="2b789-181">[Anterior](eshoponcontainers-cqrs-ddd-microservice.md)
>[Siguiente](ddd-oriented-microservice.md)</span><span class="sxs-lookup"><span data-stu-id="2b789-181">[Previous](eshoponcontainers-cqrs-ddd-microservice.md)
[Next](ddd-oriented-microservice.md)</span></span>