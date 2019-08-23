---
title: Implementación del nivel de persistencia de la infraestructura con Entity Framework Core
description: Arquitectura de microservicios de .NET para aplicaciones .NET en contenedor | Información sobre la implementación del nivel de persistencia de la infraestructura con Entity Framework Core.
ms.date: 10/08/2018
ms.openlocfilehash: 7e3480999b115ac13f8d7ebcaed826b407aa7637
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674102"
---
# <a name="implement-the-infrastructure-persistence-layer-with-entity-framework-core"></a><span data-ttu-id="18b2b-103">Implementación del nivel de persistencia de infraestructura con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="18b2b-103">Implement the infrastructure persistence layer with Entity Framework Core</span></span>

<span data-ttu-id="18b2b-104">Al utilizar bases de datos relacionales, como SQL Server, Oracle o PostgreSQL, se recomienda implementar el nivel de persistencia basado en Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="18b2b-104">When you use relational databases such as SQL Server, Oracle, or PostgreSQL, a recommended approach is to implement the persistence layer based on Entity Framework (EF).</span></span> <span data-ttu-id="18b2b-105">EF es compatible con LINQ y proporciona objetos fuertemente tipados para el modelo, así como una persistencia simplificada en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-105">EF supports LINQ and provides strongly typed objects for your model, as well as simplified persistence into your database.</span></span>

<span data-ttu-id="18b2b-106">Entity Framework hace mucho tiempo que forma parte de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="18b2b-106">Entity Framework has a long history as part of the .NET Framework.</span></span> <span data-ttu-id="18b2b-107">Al utilizar .NET Core, también debe usar Entity Framework Core, que se ejecuta en Windows o Linux de la misma manera que .NET Core.</span><span class="sxs-lookup"><span data-stu-id="18b2b-107">When you use .NET Core, you should also use Entity Framework Core, which runs on Windows or Linux in the same way as .NET Core.</span></span> <span data-ttu-id="18b2b-108">EF Core es una reescritura completa de Entity Framework, que se implementa con una superficie mucho menor y con mejoras importantes en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="18b2b-108">EF Core is a complete rewrite of Entity Framework, implemented with a much smaller footprint and important improvements in performance.</span></span>

## <a name="introduction-to-entity-framework-core"></a><span data-ttu-id="18b2b-109">Introducción a Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="18b2b-109">Introduction to Entity Framework Core</span></span>

<span data-ttu-id="18b2b-110">Entity Framework (EF) Core es una versión ligera, extensible y multiplataforma de la popular tecnología de acceso a datos Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="18b2b-110">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span> <span data-ttu-id="18b2b-111">Se introdujo con .NET Core a mediados de 2016.</span><span class="sxs-lookup"><span data-stu-id="18b2b-111">It was introduced with .NET Core in mid-2016.</span></span>

<span data-ttu-id="18b2b-112">Puesto que en la documentación de Microsoft ya hay una introducción a EF Core, aquí nos limitaremos a proporcionar vínculos a dicha información.</span><span class="sxs-lookup"><span data-stu-id="18b2b-112">Since an introduction to EF Core is already available in Microsoft documentation, here we simply provide links to that information.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="18b2b-113">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="18b2b-113">Additional resources</span></span>

- <span data-ttu-id="18b2b-114">**Entity Framework Core** </span><span class="sxs-lookup"><span data-stu-id="18b2b-114">**Entity Framework Core** </span></span>\
  [https://docs.microsoft.com/ef/core/](/ef/core/)

- <span data-ttu-id="18b2b-115">**ASP.NET Core MVC con EF Core: serie de tutoriales** </span><span class="sxs-lookup"><span data-stu-id="18b2b-115">**Getting started with ASP.NET Core and Entity Framework Core using Visual Studio** </span></span>\
  [https://docs.microsoft.com/aspnet/core/data/ef-mvc/](/aspnet/core/data/ef-mvc/)

- <span data-ttu-id="18b2b-116">**DbContext Class** \ (Clase DbContext)</span><span class="sxs-lookup"><span data-stu-id="18b2b-116">**DbContext Class** \</span></span>
  [https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext](xref:Microsoft.EntityFrameworkCore.DbContext)

- <span data-ttu-id="18b2b-117">**Compare EF Core & EF6.x** \ (Comparación de EF Core y EF6)</span><span class="sxs-lookup"><span data-stu-id="18b2b-117">**Compare EF Core & EF6.x** \</span></span>
  [https://docs.microsoft.com/ef/efcore-and-ef6/index](/ef/efcore-and-ef6/index)

## <a name="infrastructure-in-entity-framework-core-from-a-ddd-perspective"></a><span data-ttu-id="18b2b-118">Infraestructura en Entity Framework Core desde una perspectiva DDD</span><span class="sxs-lookup"><span data-stu-id="18b2b-118">Infrastructure in Entity Framework Core from a DDD perspective</span></span>

<span data-ttu-id="18b2b-119">Desde un punto de vista DDD, una capacidad importante de EF es la de utilizar las entidades de dominio POCO, también conocidas en terminología de EF como *entidades Code First* de POCO.</span><span class="sxs-lookup"><span data-stu-id="18b2b-119">From a DDD point of view, an important capability of EF is the ability to use POCO domain entities, also known in EF terminology as POCO *code-first entities*.</span></span> <span data-ttu-id="18b2b-120">Si usa las entidades de dominio POCO, las clases de modelo de dominio ignoran la persistencia, siguiendo los principios de [omisión de persistencia](https://deviq.com/persistence-ignorance/) y [omisión de infraestructura](https://ayende.com/blog/3137/infrastructure-ignorance).</span><span class="sxs-lookup"><span data-stu-id="18b2b-120">If you use POCO domain entities, your domain model classes are persistence-ignorant, following the [Persistence Ignorance](https://deviq.com/persistence-ignorance/) and the [Infrastructure Ignorance](https://ayende.com/blog/3137/infrastructure-ignorance) principles.</span></span>

<span data-ttu-id="18b2b-121">Según los patrones DDD, debe encapsular las reglas y el comportamiento de dominio dentro de la misma clase de entidad, por lo que puede controlar las invariantes, las validaciones y las reglas al acceder a cualquier colección.</span><span class="sxs-lookup"><span data-stu-id="18b2b-121">Per DDD patterns, you should encapsulate domain behavior and rules within the entity class itself, so it can control invariants, validations, and rules when accessing any collection.</span></span> <span data-ttu-id="18b2b-122">Por lo tanto, en DDD no se recomienda permitir el acceso público a colecciones de entidades secundarias u objetos de valor.</span><span class="sxs-lookup"><span data-stu-id="18b2b-122">Therefore, it is not a good practice in DDD to allow public access to collections of child entities or value objects.</span></span> <span data-ttu-id="18b2b-123">En cambio, es interesante exponer métodos que controlen cómo y cuándo se pueden actualizar los campos y las colecciones de propiedades, y qué comportamiento y qué acciones se producirán cuando esto ocurra.</span><span class="sxs-lookup"><span data-stu-id="18b2b-123">Instead, you want to expose methods that control how and when your fields and property collections can be updated, and what behavior and actions should occur when that happens.</span></span>

<span data-ttu-id="18b2b-124">Desde la versión 1.1 de EF Core, para satisfacer estos requisitos de DDD, puede tener campos sin formato en las entidades en lugar de propiedades públicas.</span><span class="sxs-lookup"><span data-stu-id="18b2b-124">Since EF Core 1.1, to satisfy those DDD requirements, you can have plain fields in your entities instead of public properties.</span></span> <span data-ttu-id="18b2b-125">Si no quiere que se pueda acceder a un campo de entidad desde el exterior, solo puede crear un campo o un atributo en vez de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="18b2b-125">If you do not want an entity field to be externally accessible, you can just create the attribute or field instead of a property.</span></span> <span data-ttu-id="18b2b-126">También puede utilizar establecedores de propiedades privadas.</span><span class="sxs-lookup"><span data-stu-id="18b2b-126">You can also use private property setters.</span></span>

<span data-ttu-id="18b2b-127">De forma parecida, ahora puede tener acceso de solo lectura a las colecciones usando una propiedad pública del tipo `IReadOnlyCollection<T>`, que está respaldada por un miembro de campo privado para la colección (como `List<T>`) en la entidad que se basa en EF para la persistencia.</span><span class="sxs-lookup"><span data-stu-id="18b2b-127">In a similar way, you can now have read-only access to collections by using a public property typed as  `IReadOnlyCollection<T>`, which is backed by a private field member for the collection (like a `List<T>`) in your entity that relies on EF for persistence.</span></span> <span data-ttu-id="18b2b-128">En las versiones anteriores de Entity Framework, se requerían propiedades de colección para admitir `ICollection<T>`, lo que significaba que cualquier desarrollador que usara la clase de entidad primaria podía agregar o quitar elementos a través de sus colecciones de propiedades.</span><span class="sxs-lookup"><span data-stu-id="18b2b-128">Previous versions of Entity Framework required collection properties to support `ICollection<T>`, which meant that any developer using the parent entity class could add or remove items through its property collections.</span></span> <span data-ttu-id="18b2b-129">Esa posibilidad iría en contra de los patrones recomendados en DDD.</span><span class="sxs-lookup"><span data-stu-id="18b2b-129">That possibility would be against the recommended patterns in DDD.</span></span>

<span data-ttu-id="18b2b-130">Puede usar una colección privada al mismo tiempo que expone un objeto `IReadOnlyCollection<T>` de solo lectura, como se muestra en el ejemplo de código siguiente:</span><span class="sxs-lookup"><span data-stu-id="18b2b-130">You can use a private collection while exposing a read-only `IReadOnlyCollection<T>` object, as shown in the following code example:</span></span>

```csharp
public class Order : Entity
{
    // Using private fields, allowed since EF Core 1.1
    private DateTime _orderDate;
    // Other fields ...

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    protected Order() { }

    public Order(int buyerId, int paymentMethodId, Address address)
    {
        // Initializations ...
    }

    public void AddOrderItem(int productId, string productName,
                             decimal unitPrice, decimal discount,
                             string pictureUrl, int units = 1)
    {
        // Validation logic...

        var orderItem = new OrderItem(productId, productName,
                                      unitPrice, discount,
                                      pictureUrl, units);
        _orderItems.Add(orderItem);
    }
}
```

<span data-ttu-id="18b2b-131">Tenga en cuenta que solo se puede obtener acceso de solo lectura a la propiedad `OrderItems`, mediante `IReadOnlyCollection<OrderItem>`.</span><span class="sxs-lookup"><span data-stu-id="18b2b-131">Note that the `OrderItems` property can only be accessed as read-only using `IReadOnlyCollection<OrderItem>`.</span></span> <span data-ttu-id="18b2b-132">Este tipo es de solo lectura, por lo que está protegido frente a las actualizaciones externas normales.</span><span class="sxs-lookup"><span data-stu-id="18b2b-132">This type is read-only so it is protected against regular external updates.</span></span>

<span data-ttu-id="18b2b-133">EF Core proporciona una manera de asignar el modelo de dominio a la base de datos física sin que "contamine" el modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="18b2b-133">EF Core provides a way to map the domain model to the physical database without "contaminating" the domain model.</span></span> <span data-ttu-id="18b2b-134">Se trata de código POCO puro de .NET, puesto que la acción de asignación se implementa en el nivel de persistencia.</span><span class="sxs-lookup"><span data-stu-id="18b2b-134">It is pure .NET POCO code, because the mapping action is implemented in the persistence layer.</span></span> <span data-ttu-id="18b2b-135">En esa acción de asignación, debe configurar la asignación de campos a base de datos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-135">In that mapping action, you need to configure the fields-to-database mapping.</span></span> <span data-ttu-id="18b2b-136">En el siguiente ejemplo del método `OnModelCreating` de `OrderingContext` y la clase `OrderEntityTypeConfiguration`, la llamada a `SetPropertyAccessMode` indica a EF Core que debe acceder a la propiedad `OrderItems` a través de su campo.</span><span class="sxs-lookup"><span data-stu-id="18b2b-136">In the following example of the `OnModelCreating` method from `OrderingContext` and the `OrderEntityTypeConfiguration` class, the call to `SetPropertyAccessMode` tells EF Core to access the `OrderItems` property through its field.</span></span>

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities’ configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
        orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);
        // Other configuration

        var navigation =
              orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

        //EF access the OrderItem collection property through its backing field
        navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

        // Other configuration
    }
}
```

<span data-ttu-id="18b2b-137">Al usar campos en lugar de propiedades, se conserva la entidad `OrderItem` como si tuviera una propiedad `List<OrderItem>`.</span><span class="sxs-lookup"><span data-stu-id="18b2b-137">When you use fields instead of properties, the `OrderItem` entity is persisted just as if it had a `List<OrderItem>` property.</span></span> <span data-ttu-id="18b2b-138">Pero expone un descriptor de acceso único, el método `AddOrderItem`, para agregar nuevos elementos al pedido.</span><span class="sxs-lookup"><span data-stu-id="18b2b-138">However, it exposes a single accessor, the `AddOrderItem` method, for adding new items to the order.</span></span> <span data-ttu-id="18b2b-139">Como resultado, el comportamiento y los datos permanecen unidos y son coherentes a lo largo de cualquier código de aplicación que utilice el modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="18b2b-139">As a result, behavior and data are tied together and will be consistent throughout any application code that uses the domain model.</span></span>

## <a name="implement-custom-repositories-with-entity-framework-core"></a><span data-ttu-id="18b2b-140">Implementación de los repositorios personalizados con Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="18b2b-140">Implement custom repositories with Entity Framework Core</span></span>

<span data-ttu-id="18b2b-141">En el nivel de implementación, un repositorio no es más que una clase con código de persistencia de datos coordinada por una unidad de trabajo (DBContext en EF Core) al realizar actualizaciones, como se muestra en la clase siguiente:</span><span class="sxs-lookup"><span data-stu-id="18b2b-141">At the implementation level, a repository is simply a class with data persistence code coordinated by a unit of work (DBContext in EF Core) when performing updates, as shown in the following class:</span></span>

```csharp
// using statements...
namespace Microsoft.eShopOnContainers.Services.Ordering.Infrastructure.Repositories
{
    public class BuyerRepository : IBuyerRepository
    {
        private readonly OrderingContext _context;
        public IUnitOfWork UnitOfWork
        {
            get
            {
                return _context;
            }
        }

        public BuyerRepository(OrderingContext context)
        {
            _context = context ?? throw new ArgumentNullException(nameof(context));
        }

        public Buyer Add(Buyer buyer)
        {
            return _context.Buyers.Add(buyer).Entity;
        }

        public async Task<Buyer> FindAsync(string BuyerIdentityGuid)
        {
            var buyer = await _context.Buyers
                .Include(b => b.Payments)
                .Where(b => b.FullName == BuyerIdentityGuid)
                .SingleOrDefaultAsync();

            return buyer;
        }
    }
}
```

<span data-ttu-id="18b2b-142">Tenga en cuenta que la interfaz IBuyerRepository proviene del nivel de modelo de dominio como un contrato.</span><span class="sxs-lookup"><span data-stu-id="18b2b-142">Note that the IBuyerRepository interface comes from the domain model layer as a contract.</span></span> <span data-ttu-id="18b2b-143">Pero la implementación del repositorio se realiza en el nivel de persistencia e infraestructura.</span><span class="sxs-lookup"><span data-stu-id="18b2b-143">However, the repository implementation is done at the persistence and infrastructure layer.</span></span>

<span data-ttu-id="18b2b-144">DbContext de EF pasa mediante el constructor a través de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="18b2b-144">The EF DbContext comes through the constructor through Dependency Injection.</span></span> <span data-ttu-id="18b2b-145">Se comparte entre varios repositorios dentro del mismo ámbito de solicitud HTTP gracias a su duración predeterminada (`ServiceLifetime.Scoped`) en el contenedor de IoC (que también puede establecerse explícitamente con `services.AddDbContext<>`).</span><span class="sxs-lookup"><span data-stu-id="18b2b-145">It is shared between multiple repositories within the same HTTP request scope, thanks to its default lifetime (`ServiceLifetime.Scoped`) in the IoC container (which can also be explicitly set with `services.AddDbContext<>`).</span></span>

### <a name="methods-to-implement-in-a-repository-updates-or-transactions-versus-queries"></a><span data-ttu-id="18b2b-146">Métodos que se pueden implementar en un repositorio (actualizaciones o transacciones frente a consultas)</span><span class="sxs-lookup"><span data-stu-id="18b2b-146">Methods to implement in a repository (updates or transactions versus queries)</span></span>

<span data-ttu-id="18b2b-147">Dentro de cada clase de repositorio, debe colocar los métodos de persistencia que actualizan el estado de las entidades de forma que queden contenidos por su agregado relacionado.</span><span class="sxs-lookup"><span data-stu-id="18b2b-147">Within each repository class, you should put the persistence methods that update the state of entities contained by its related aggregate.</span></span> <span data-ttu-id="18b2b-148">Recuerde que hay una relación de uno a uno entre un agregado y su repositorio relacionado.</span><span class="sxs-lookup"><span data-stu-id="18b2b-148">Remember there is one-to-one relationship between an aggregate and its related repository.</span></span> <span data-ttu-id="18b2b-149">Tenga en cuenta que un objeto entidad de raíz agregada podría tener entidades secundarias insertadas en su gráfico de EF.</span><span class="sxs-lookup"><span data-stu-id="18b2b-149">Consider that an aggregate root entity object might have embedded child entities within its EF graph.</span></span> <span data-ttu-id="18b2b-150">Por ejemplo, un comprador puede tener varias formas de pago como entidades secundarias relacionadas.</span><span class="sxs-lookup"><span data-stu-id="18b2b-150">For example, a buyer might have multiple payment methods as related child entities.</span></span>

<span data-ttu-id="18b2b-151">Como el enfoque para el microservicio de ordenación en eShopOnContainers también se basa en CQS/CQRS, la mayoría de consultas no se implementa en repositorios personalizados.</span><span class="sxs-lookup"><span data-stu-id="18b2b-151">Since the approach for the ordering microservice in eShopOnContainers is also based on CQS/CQRS, most of the queries are not implemented in custom repositories.</span></span> <span data-ttu-id="18b2b-152">Los desarrolladores pueden crear libremente las consultas y combinaciones que necesiten para el nivel de presentación sin las restricciones impuestas por agregados, repositorios personalizados por agregado y DDD en general.</span><span class="sxs-lookup"><span data-stu-id="18b2b-152">Developers have the freedom to create the queries and joins they need for the presentation layer without the restrictions imposed by aggregates, custom repositories per aggregate, and DDD in general.</span></span> <span data-ttu-id="18b2b-153">La mayoría de repositorios personalizados sugeridos por esta guía tiene varios métodos de actualización o transacción, pero solo se actualizan los métodos de consulta necesarios para obtener los datos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-153">Most of the custom repositories suggested by this guide have several update or transactional methods but just the query methods needed to get data to be updated.</span></span> <span data-ttu-id="18b2b-154">Por ejemplo, el repositorio BuyerRepository implementa un método FindAsync, porque la aplicación necesita saber si existe un comprador determinado antes de crear un nuevo comprador relacionado con el pedido.</span><span class="sxs-lookup"><span data-stu-id="18b2b-154">For example, the BuyerRepository repository implements a FindAsync method, because the application needs to know whether a particular buyer exists before creating a new buyer related to the order.</span></span>

<span data-ttu-id="18b2b-155">Pero los métodos de consulta reales para obtener los datos que se van a enviar al nivel de presentación o a las aplicaciones cliente se implementan, como se ha mencionado, en las consultas CQRS basadas en consultas flexibles mediante Dapper.</span><span class="sxs-lookup"><span data-stu-id="18b2b-155">However, the real query methods to get data to send to the presentation layer or client apps are implemented, as mentioned, in the CQRS queries based on flexible queries using Dapper.</span></span>

### <a name="using-a-custom-repository-versus-using-ef-dbcontext-directly"></a><span data-ttu-id="18b2b-156">Uso de un repositorio personalizado frente al uso de DbContext EF directamente</span><span class="sxs-lookup"><span data-stu-id="18b2b-156">Using a custom repository versus using EF DbContext directly</span></span>

<span data-ttu-id="18b2b-157">La clase DbContext de Entity Framework se basa en los patrones de unidad de trabajo y repositorio, y puede utilizarse directamente desde el código, así como desde un controlador MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="18b2b-157">The Entity Framework DbContext class is based on the Unit of Work and Repository patterns, and can be used directly from your code, such as from an ASP.NET Core MVC controller.</span></span> <span data-ttu-id="18b2b-158">Esta es la forma de crear el código más sencillo, como en el microservicio de catálogo CRUD en eShopOnContainers.</span><span class="sxs-lookup"><span data-stu-id="18b2b-158">That is the way you can create the simplest code, as in the CRUD catalog microservice in eShopOnContainers.</span></span> <span data-ttu-id="18b2b-159">En los casos en los que quiera disponer del código más sencillo posible, puede utilizar directamente la clase DbContext, igual que muchos desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="18b2b-159">In cases where you want the simplest code possible, you might want to directly use the DbContext class, as many developers do.</span></span>

<span data-ttu-id="18b2b-160">Pero implementar repositorios personalizados ofrece varias ventajas al implementar aplicaciones o microservicios más complejos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-160">However, implementing custom repositories provides several benefits when implementing more complex microservices or applications.</span></span> <span data-ttu-id="18b2b-161">Los patrones de unidad de trabajo y repositorio están diseñados para encapsular el nivel de persistencia de infraestructura de tal modo que se separe de los niveles de aplicación y de modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="18b2b-161">The Unit of Work and Repository patterns are intended to encapsulate the infrastructure persistence layer so it is decoupled from the application and domain model layers.</span></span> <span data-ttu-id="18b2b-162">Implementar estos patrones puede facilitar el uso de repositorios ficticios que simulen el acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-162">Implementing these patterns can facilitate the use of mock repositories simulating access to the database.</span></span>

<span data-ttu-id="18b2b-163">En la Figura 7-18 puede ver las diferencias entre no usar repositorios (directamente mediante DbContext de EF) y usar repositorios que faciliten la simulación de los repositorios.</span><span class="sxs-lookup"><span data-stu-id="18b2b-163">In Figure 7-18 you can see the differences between not using repositories (directly using the EF DbContext) versus using repositories which make it easier to mock those repositories.</span></span>

![Comparación entre el uso de un repositorio personalizado y un elemento DbContext normal: el repositorio personalizado agrega una capa de abstracción que se puede usar para facilitar las pruebas mediante la simulación del repositorio.](./media/image19.png)

<span data-ttu-id="18b2b-165">**Figura 7-18**.</span><span class="sxs-lookup"><span data-stu-id="18b2b-165">**Figure 7-18**.</span></span> <span data-ttu-id="18b2b-166">Uso de repositorios personalizados frente a DbContext sin formato</span><span class="sxs-lookup"><span data-stu-id="18b2b-166">Using custom repositories versus a plain DbContext</span></span>

<span data-ttu-id="18b2b-167">Hay varias alternativas al plantear una simulación.</span><span class="sxs-lookup"><span data-stu-id="18b2b-167">There are multiple alternatives when mocking.</span></span> <span data-ttu-id="18b2b-168">Puede limitarse a simular repositorios o puede simular una unidad de trabajo completa.</span><span class="sxs-lookup"><span data-stu-id="18b2b-168">You could mock just repositories or you could mock a whole unit of work.</span></span> <span data-ttu-id="18b2b-169">Normalmente es suficiente con simular repositorios y no suele ser necesario pasar por la complejidad de tener que abstraer y simular una unidad de trabajo.</span><span class="sxs-lookup"><span data-stu-id="18b2b-169">Usually mocking just the repositories is enough, and the complexity to abstract and mock a whole unit of work is usually not needed.</span></span>

<span data-ttu-id="18b2b-170">Más adelante, cuando nos centremos en el nivel de aplicación, verá cómo funciona la inserción de dependencias en ASP.NET Core y cómo se implementa al utilizar repositorios.</span><span class="sxs-lookup"><span data-stu-id="18b2b-170">Later, when we focus on the application layer, you will see how Dependency Injection works in ASP.NET Core and how it is implemented when using repositories.</span></span>

<span data-ttu-id="18b2b-171">En resumen, los repositorios personalizados le permiten probar el código más fácilmente con pruebas unitarias que no se ven afectadas por el estado de la capa de datos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-171">In short, custom repositories allow you to test code more easily with unit tests that are not impacted by the data tier state.</span></span> <span data-ttu-id="18b2b-172">Si ejecuta pruebas que también tienen acceso a la base de datos real a través de Entity Framework, no se trata de pruebas unitarias sino de pruebas de integración, que son mucho más lentas.</span><span class="sxs-lookup"><span data-stu-id="18b2b-172">If you run tests that also access the actual database through the Entity Framework, they are not unit tests but integration tests, which are a lot slower.</span></span>

<span data-ttu-id="18b2b-173">Si estaba usando DbContext directamente, tendría que simularlo o ejecutar pruebas unitarias mediante el uso de SQL Server en la memoria con datos predecibles para pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="18b2b-173">If you were using DbContext directly, you would have to mock it or to run unit tests by using an in-memory SQL Server with predictable data for unit tests.</span></span> <span data-ttu-id="18b2b-174">Pero simular la clase DbContext o controlar datos falsos requiere más trabajo que la simulación en el nivel de repositorio.</span><span class="sxs-lookup"><span data-stu-id="18b2b-174">But mocking the DbContext or controlling fake data requires more work than mocking at the repository level.</span></span> <span data-ttu-id="18b2b-175">Por supuesto, siempre puede probar los controladores MVC.</span><span class="sxs-lookup"><span data-stu-id="18b2b-175">Of course, you could always test the MVC controllers.</span></span>

## <a name="ef-dbcontext-and-iunitofwork-instance-lifetime-in-your-ioc-container"></a><span data-ttu-id="18b2b-176">Duración de DbContext de EF y de la instancia IUnitOfWork en el contenedor de IoC</span><span class="sxs-lookup"><span data-stu-id="18b2b-176">EF DbContext and IUnitOfWork instance lifetime in your IoC container</span></span>

<span data-ttu-id="18b2b-177">El objeto `DbContext` (expuesto como un objeto `IUnitOfWork`) debería compartirse entre varios repositorios dentro del mismo ámbito de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="18b2b-177">The `DbContext` object (exposed as an `IUnitOfWork` object) should be shared among multiple repositories within the same HTTP request scope.</span></span> <span data-ttu-id="18b2b-178">Por ejemplo, esto sucede cuando la operación que se está ejecutando debe tratar con varios agregados o simplemente porque está usando varias instancias de repositorio.</span><span class="sxs-lookup"><span data-stu-id="18b2b-178">For example, this is true when the operation being executed must deal with multiple aggregates, or simply because you are using multiple repository instances.</span></span> <span data-ttu-id="18b2b-179">También es importante mencionar que la interfaz de `IUnitOfWork` forma parte del nivel de dominio, no es un tipo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="18b2b-179">It is also important to mention that the `IUnitOfWork` interface is part of your domain layer, not an EF Core type.</span></span>

<span data-ttu-id="18b2b-180">Para ello, hay que establecer la duración del servicio de la instancia del objeto `DbContext` en ServiceLifetime.Scoped.</span><span class="sxs-lookup"><span data-stu-id="18b2b-180">In order to do that, the instance of the `DbContext` object has to have its service lifetime set to ServiceLifetime.Scoped.</span></span> <span data-ttu-id="18b2b-181">Se trata de la duración predeterminada al registrar `DbContext` con `services.AddDbContext` en el contenedor de IoC desde el método ConfigureServices del archivo `Startup.cs` en el proyecto de ASP.NET Core Web API.</span><span class="sxs-lookup"><span data-stu-id="18b2b-181">This is the default lifetime when registering a `DbContext` with `services.AddDbContext` in your IoC container from the ConfigureServices method of the `Startup.cs` file in your ASP.NET Core Web API project.</span></span> <span data-ttu-id="18b2b-182">Esto se ilustra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="18b2b-182">The following code illustrates this.</span></span>

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddMvc(options =>
    {
        options.Filters.Add(typeof(HttpGlobalExceptionFilter));
    }).AddControllersAsServices();

    services.AddEntityFrameworkSqlServer()
      .AddDbContext<OrderingContext>(options =>
      {
          options.UseSqlServer(Configuration["ConnectionString"],
                               sqlOptions => sqlOptions.MigrationsAssembly(typeof(Startup).GetTypeInfo().
                                                                                    Assembly.GetName().Name));
      },
      ServiceLifetime.Scoped // Note that Scoped is the default choice
                             // in AddDbContext. It is shown here only for
                             // pedagogic purposes.
      );
}
```

<span data-ttu-id="18b2b-183">El modo de creación de instancias de DbContext no se debe configurar como ServiceLifetime.Transient o ServiceLifetime.Singleton.</span><span class="sxs-lookup"><span data-stu-id="18b2b-183">The DbContext instantiation mode should not be configured as ServiceLifetime.Transient or ServiceLifetime.Singleton.</span></span>

## <a name="the-repository-instance-lifetime-in-your-ioc-container"></a><span data-ttu-id="18b2b-184">Duración de la instancia de repositorio en su contenedor IoC</span><span class="sxs-lookup"><span data-stu-id="18b2b-184">The repository instance lifetime in your IoC container</span></span>

<span data-ttu-id="18b2b-185">De forma similar, la duración del repositorio normalmente se establece como determinada (InstancePerLifetimeScope en Autofac).</span><span class="sxs-lookup"><span data-stu-id="18b2b-185">In a similar way, repository’s lifetime should usually be set as scoped (InstancePerLifetimeScope in Autofac).</span></span> <span data-ttu-id="18b2b-186">También puede ser transitorio (InstancePerDependency en Autofac), pero el servicio será más eficaz en lo que respecta a la memoria si se usa la duración de ámbito.</span><span class="sxs-lookup"><span data-stu-id="18b2b-186">It could also be transient (InstancePerDependency in Autofac), but your service will be more efficient in regards memory when using the scoped lifetime.</span></span>

```csharp
// Registering a Repository in Autofac IoC container
builder.RegisterType<OrderRepository>()
    .As<IOrderRepository>()
    .InstancePerLifetimeScope();
```

<span data-ttu-id="18b2b-187">Tenga en cuenta que utilizar la duración de singleton para el repositorio puede causar problemas de simultaneidad graves al establecer DbContext en una duración determinada (InstancePerLifetimeScope) (duraciones predeterminadas para DBContext).</span><span class="sxs-lookup"><span data-stu-id="18b2b-187">Note that using the singleton lifetime for the repository could cause you serious concurrency problems when your DbContext is set to scoped (InstancePerLifetimeScope) lifetime (the default lifetimes for a DBContext).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="18b2b-188">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="18b2b-188">Additional resources</span></span>

- <span data-ttu-id="18b2b-189">**Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application** \ (Implementación de los patrones de repositorio y unidad de trabajo en una aplicación ASP.NET MVC)</span><span class="sxs-lookup"><span data-stu-id="18b2b-189">**Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application** \</span></span>
  <https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application>

- <span data-ttu-id="18b2b-190">**Jonathan Allen. Implementation Strategies for the Repository Pattern with Entity Framework, Dapper, and Chain** \ (Estrategias de implementación para el patrón de repositorio con Entity Framework, Dapper y Chain)</span><span class="sxs-lookup"><span data-stu-id="18b2b-190">**Jonathan Allen. Implementation Strategies for the Repository Pattern with Entity Framework, Dapper, and Chain** \</span></span>
  <https://www.infoq.com/articles/repository-implementation-strategies>

- <span data-ttu-id="18b2b-191">**Cesar de la Torre. Comparing ASP.NET Core IoC container service lifetimes with Autofac IoC container instance scopes** \ (Comparación de las duraciones de servicio del contenedor IoC de ASP-NET Core con ámbitos de instancia de contenedor Autofac IoC)</span><span class="sxs-lookup"><span data-stu-id="18b2b-191">**Cesar de la Torre. Comparing ASP.NET Core IoC container service lifetimes with Autofac IoC container instance scopes** \</span></span>
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="table-mapping"></a><span data-ttu-id="18b2b-192">Asignación de tabla</span><span class="sxs-lookup"><span data-stu-id="18b2b-192">Table mapping</span></span>

<span data-ttu-id="18b2b-193">La asignación de tabla identifica los datos de tabla que se van a consultar en la base de datos y que se guardarán en ella.</span><span class="sxs-lookup"><span data-stu-id="18b2b-193">Table mapping identifies the table data to be queried from and saved to the database.</span></span> <span data-ttu-id="18b2b-194">Anteriormente, vimos cómo las entidades de domino (por ejemplo, un dominio de producto o de pedido) se podían usar para generar un esquema de base de datos relacionado.</span><span class="sxs-lookup"><span data-stu-id="18b2b-194">Previously you saw how domain entities (for example, a product or order domain) can be used to generate a related database schema.</span></span> <span data-ttu-id="18b2b-195">EF está diseñado basándose en el concepto de *convenciones*.</span><span class="sxs-lookup"><span data-stu-id="18b2b-195">EF is strongly designed around the concept of *conventions*.</span></span> <span data-ttu-id="18b2b-196">Las convenciones generan preguntas como "¿Cuál será el nombre de la tabla?"</span><span class="sxs-lookup"><span data-stu-id="18b2b-196">Conventions address questions like “What will the name of a table be?”</span></span> <span data-ttu-id="18b2b-197">o "¿Qué propiedad es la clave principal?".</span><span class="sxs-lookup"><span data-stu-id="18b2b-197">or “What property is the primary key?”</span></span> <span data-ttu-id="18b2b-198">Normalmente las convenciones se basan en nombres convencionales, por ejemplo, es habitual que la clave principal sea una propiedad que termine con el identificador.</span><span class="sxs-lookup"><span data-stu-id="18b2b-198">Conventions are typically based on conventional names—for example, it is typical for the primary key to be a property that ends with Id.</span></span>

<span data-ttu-id="18b2b-199">Por convención, cada entidad se configurará para asignarse a una tabla que tenga el mismo nombre que la propiedad `DbSet<TEntity>` que expone la entidad en el contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="18b2b-199">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="18b2b-200">Si no se proporciona ningún valor `DbSet<TEntity>` a la entidad determinada, se utiliza el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="18b2b-200">If no `DbSet<TEntity>` value is provided for the given entity, the class name is used.</span></span>

### <a name="data-annotations-versus-fluent-api"></a><span data-ttu-id="18b2b-201">Anotaciones de datos frente a API fluida</span><span class="sxs-lookup"><span data-stu-id="18b2b-201">Data Annotations versus Fluent API</span></span>

<span data-ttu-id="18b2b-202">Hay muchas convenciones de EF Core adicionales, la mayoría de las cuales se puede cambiar mediante anotaciones de datos o la API fluida, que se implementan con el método OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="18b2b-202">There are many additional EF Core conventions, and most of them can be changed by using either data annotations or Fluent API, implemented within the OnModelCreating method.</span></span>

<span data-ttu-id="18b2b-203">Las anotaciones de datos se utilizan en las mismas clases del modelo de entidad, lo que supone un método más intrusivo desde el punto de vista de DDD.</span><span class="sxs-lookup"><span data-stu-id="18b2b-203">Data annotations must be used on the entity model classes themselves, which is a more intrusive way from a DDD point of view.</span></span> <span data-ttu-id="18b2b-204">Esto es así porque el modelo se contamina con anotaciones de datos relacionadas con la base de datos de la infraestructura.</span><span class="sxs-lookup"><span data-stu-id="18b2b-204">This is because you are contaminating your model with data annotations related to the infrastructure database.</span></span> <span data-ttu-id="18b2b-205">Por otro lado, la API fluida es una forma práctica de cambiar la mayoría de convenciones y asignaciones en el nivel de infraestructura de la persistencia de datos, por lo que el modelo de entidad estará limpio y desacoplado de la infraestructura de persistencia.</span><span class="sxs-lookup"><span data-stu-id="18b2b-205">On the other hand, Fluent API is a convenient way to change most conventions and mappings within your data persistence infrastructure layer, so the entity model will be clean and decoupled from the persistence infrastructure.</span></span>

### <a name="fluent-api-and-the-onmodelcreating-method"></a><span data-ttu-id="18b2b-206">API fluida y el método OnModelCreating</span><span class="sxs-lookup"><span data-stu-id="18b2b-206">Fluent API and the OnModelCreating method</span></span>

<span data-ttu-id="18b2b-207">Como se ha indicado, puede usar el método OnModelCreating en la clase DbContext con el fin de cambiar las convenciones y las asignaciones.</span><span class="sxs-lookup"><span data-stu-id="18b2b-207">As mentioned, in order to change conventions and mappings, you can use the OnModelCreating method in the DbContext class.</span></span>

<span data-ttu-id="18b2b-208">El microservicio de ordenación en eShopOnContainers implementa configuraciones y asignaciones explícitas, cuando es necesario, tal y como se muestra en el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="18b2b-208">The ordering microservice in eShopOnContainers implements explicit mapping and configuration, when needed, as shown in the following code.</span></span>

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities’ configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
            orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);

            orderConfiguration.HasKey(o => o.Id);

            orderConfiguration.Ignore(b => b.DomainEvents);

            orderConfiguration.Property(o => o.Id)
                .ForSqlServerUseSequenceHiLo("orderseq", OrderingContext.DEFAULT_SCHEMA);

            //Address Value Object persisted as owned entity type supported since EF Core 2.0
            orderConfiguration.OwnsOne(o => o.Address);

            orderConfiguration.Property<DateTime>("OrderDate").IsRequired();
            orderConfiguration.Property<int?>("BuyerId").IsRequired(false);
            orderConfiguration.Property<int>("OrderStatusId").IsRequired();
            orderConfiguration.Property<int?>("PaymentMethodId").IsRequired(false);
            orderConfiguration.Property<string>("Description").IsRequired(false);

            var navigation = orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

            // DDD Patterns comment:
            //Set as field (New since EF 1.1) to access the OrderItem collection property through its field
            navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

            orderConfiguration.HasOne<PaymentMethod>()
                .WithMany()
                .HasForeignKey("PaymentMethodId")
                .IsRequired(false)
                .OnDelete(DeleteBehavior.Restrict);

            orderConfiguration.HasOne<Buyer>()
                .WithMany()
                .IsRequired(false)
                .HasForeignKey("BuyerId");

            orderConfiguration.HasOne(o => o.OrderStatus)
                .WithMany()
                .HasForeignKey("OrderStatusId");
    }
}
```

<span data-ttu-id="18b2b-209">Puede establecer todas las asignaciones de la API fluida dentro del mismo método OnModelCreating, pero se aconseja crear particiones en el código y tener varias clases de configuración, una por cada entidad, tal como se muestra en el ejemplo.</span><span class="sxs-lookup"><span data-stu-id="18b2b-209">You could set all the Fluent API mappings within the same OnModelCreating method, but it is advisable to partition that code and have multiple configuration classes, one per entity, as shown in the example.</span></span> <span data-ttu-id="18b2b-210">Especialmente para modelos grandes, es aconsejable tener clases de configuración independientes para configurar diferentes tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="18b2b-210">Especially for particularly large models, it is advisable to have separate configuration classes for configuring different entity types.</span></span>

<span data-ttu-id="18b2b-211">En el código de ejemplo se muestran algunas asignaciones y declaraciones explícitas.</span><span class="sxs-lookup"><span data-stu-id="18b2b-211">The code in the example shows a few explicit declarations and mapping.</span></span> <span data-ttu-id="18b2b-212">Pero las convenciones de EF Core realizan muchas de esas asignaciones automáticamente, por lo que, en su caso, podría necesitar un código más pequeño.</span><span class="sxs-lookup"><span data-stu-id="18b2b-212">However, EF Core conventions do many of those mappings automatically, so the actual code you would need in your case might be smaller.</span></span>

### <a name="the-hilo-algorithm-in-ef-core"></a><span data-ttu-id="18b2b-213">Algoritmo Hi/Lo en EF Core</span><span class="sxs-lookup"><span data-stu-id="18b2b-213">The Hi/Lo algorithm in EF Core</span></span>

<span data-ttu-id="18b2b-214">Un aspecto interesante del código de ejemplo anterior es que utiliza el [algoritmo Hi/Lo](https://vladmihalcea.com/the-hilo-algorithm/) como estrategia de generación de claves.</span><span class="sxs-lookup"><span data-stu-id="18b2b-214">An interesting aspect of code in the preceding example is that it uses the [Hi/Lo algorithm](https://vladmihalcea.com/the-hilo-algorithm/) as the key generation strategy.</span></span>

<span data-ttu-id="18b2b-215">El algoritmo Hi-Lo es útil cuando se necesitan claves únicas antes de confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="18b2b-215">The Hi/Lo algorithm is useful when you need unique keys before committing changes.</span></span> <span data-ttu-id="18b2b-216">A modo de resumen, el algoritmo Hi-Lo asigna identificadores únicos a filas de la tabla, pero no depende del almacenaje inmediato de la fila en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-216">As a summary, the Hi-Lo algorithm assigns unique identifiers to table rows while not depending on storing the row in the database immediately.</span></span> <span data-ttu-id="18b2b-217">Esto le permite empezar a usar los identificadores de forma inmediata, como sucede con los identificadores de la base de datos secuencial normal.</span><span class="sxs-lookup"><span data-stu-id="18b2b-217">This lets you start using the identifiers right away, as happens with regular sequential database IDs.</span></span>

<span data-ttu-id="18b2b-218">El algoritmo Hi-Lo describe un mecanismo para obtener un lote de identificadores únicos de una secuencia de una base de datos relacionada.</span><span class="sxs-lookup"><span data-stu-id="18b2b-218">The Hi/Lo algorithm describes a mechanism for getting a batch of unique IDs from a related database sequence.</span></span> <span data-ttu-id="18b2b-219">Estos identificadores son seguros porque la base de datos garantiza que son únicos, por lo que no se producirán colisiones entre los usuarios.</span><span class="sxs-lookup"><span data-stu-id="18b2b-219">These IDs are safe to use because the database guarantees the uniqueness, so there will be no collisions between users.</span></span> <span data-ttu-id="18b2b-220">Este algoritmo es interesante por los siguientes motivos:</span><span class="sxs-lookup"><span data-stu-id="18b2b-220">This algorithm is interesting for these reasons:</span></span>

- <span data-ttu-id="18b2b-221">No interrumpe el patrón de la unidad de trabajo.</span><span class="sxs-lookup"><span data-stu-id="18b2b-221">It does not break the Unit of Work pattern.</span></span>

- <span data-ttu-id="18b2b-222">Obtiene la secuencia de id. por lotes, para minimizar los recorridos de ida y vuelta a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-222">It gets sequence IDs in batches, to minimize round trips to the database.</span></span>

- <span data-ttu-id="18b2b-223">Genera un identificador que pueden leer los humanos, a diferencia de las técnicas que utilizan los identificadores GUID.</span><span class="sxs-lookup"><span data-stu-id="18b2b-223">It generates a human readable identifier, unlike techniques that use GUIDs.</span></span>

<span data-ttu-id="18b2b-224">EF Core es compatible con [HiLo](https://stackoverflow.com/questions/282099/whats-the-hi-lo-algorithm) con el método ForSqlServerUseSequenceHiLo, tal como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="18b2b-224">EF Core supports [HiLo](https://stackoverflow.com/questions/282099/whats-the-hi-lo-algorithm) with the ForSqlServerUseSequenceHiLo method, as shown in the preceding example.</span></span>

### <a name="map-fields-instead-of-properties"></a><span data-ttu-id="18b2b-225">Asignación de campos en lugar de propiedades</span><span class="sxs-lookup"><span data-stu-id="18b2b-225">Map fields instead of properties</span></span>

<span data-ttu-id="18b2b-226">Con esta característica, disponible desde la versión 1.1 de EF Core, puede asignar directamente columnas a los campos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-226">With this feature, available since EF Core 1.1, you can directly map columns to fields.</span></span> <span data-ttu-id="18b2b-227">Es posible no utilizar propiedades en la clase de entidad y simplemente asignar columnas de una tabla a los campos.</span><span class="sxs-lookup"><span data-stu-id="18b2b-227">It is possible to not use properties in the entity class, and just to map columns from a table to fields.</span></span> <span data-ttu-id="18b2b-228">Un uso habitual de ello serían los campos privados para cualquier estado interno, al que no sea necesario acceder desde fuera de la entidad.</span><span class="sxs-lookup"><span data-stu-id="18b2b-228">A common use for that would be private fields for any internal state that do not need to be accessed from outside the entity.</span></span>

<span data-ttu-id="18b2b-229">Puede hacerlo con campos únicos o también con colecciones, como si se tratara de un campo `List<>`.</span><span class="sxs-lookup"><span data-stu-id="18b2b-229">You can do this with single fields or also with collections, like a `List<>` field.</span></span> <span data-ttu-id="18b2b-230">Este punto se mencionó anteriormente cuando analizamos el modelado de las clases de modelo de dominio, pero aquí puede ver cómo se realiza esta asignación con la configuración `PropertyAccessMode.Field` resaltada en el código anterior.</span><span class="sxs-lookup"><span data-stu-id="18b2b-230">This point was mentioned earlier when we discussed modeling the domain model classes, but here you can see how that mapping is performed with the `PropertyAccessMode.Field` configuration highlighted in the previous code.</span></span>

### <a name="use-shadow-properties-in-ef-core-hidden-at-the-infrastructure-level"></a><span data-ttu-id="18b2b-231">Uso de propiedades reemplazadas en EF Core y ocultas en el nivel de infraestructura</span><span class="sxs-lookup"><span data-stu-id="18b2b-231">Use shadow properties in EF Core, hidden at the infrastructure level</span></span>

<span data-ttu-id="18b2b-232">Las propiedades reemplazadas en EF Core son propiedades que no existen en su modelo de clase de entidad.</span><span class="sxs-lookup"><span data-stu-id="18b2b-232">Shadow properties in EF Core are properties that do not exist in your entity class model.</span></span> <span data-ttu-id="18b2b-233">Los valores y estados de estas propiedades se mantienen exclusivamente en la clase [ChangeTracker](https://docs.microsoft.com/ef/core/api/microsoft.entityframeworkcore.changetracking.changetracker), en el nivel de infraestructura.</span><span class="sxs-lookup"><span data-stu-id="18b2b-233">The values and states of these properties are maintained purely in the [ChangeTracker](https://docs.microsoft.com/ef/core/api/microsoft.entityframeworkcore.changetracking.changetracker) class at the infrastructure level.</span></span>

## <a name="implement-the-query-specification-pattern"></a><span data-ttu-id="18b2b-234">Implementación del patrón de especificación de consultas</span><span class="sxs-lookup"><span data-stu-id="18b2b-234">Implement the Query Specification pattern</span></span>

<span data-ttu-id="18b2b-235">Como se mencionó anteriormente en la sección de diseño, el patrón de especificación de consultas es un modelo de diseño controlado por dominios diseñado como el lugar donde se puede incluir la definición de una consulta con lógica opcional de ordenación y paginación.</span><span class="sxs-lookup"><span data-stu-id="18b2b-235">As introduced earlier in the design section, the Query Specification pattern is a Domain-Driven Design pattern designed as the place where you can put the definition of a query with optional sorting and paging logic.</span></span>

<span data-ttu-id="18b2b-236">El patrón de especificación de consultas define una consulta en un objeto.</span><span class="sxs-lookup"><span data-stu-id="18b2b-236">The Query Specification pattern defines a query in an object.</span></span> <span data-ttu-id="18b2b-237">Por ejemplo, para encapsular una consulta paginada que busque algunos productos, se puede crear una especificación PagedProduct que tome los parámetros de entrada necesarios (pageNumber, pageSize, filter, etc.).</span><span class="sxs-lookup"><span data-stu-id="18b2b-237">For example, in order to encapsulate a paged query that searches for some products you can create a PagedProduct specification that takes the necessary input parameters (pageNumber, pageSize, filter, etc.).</span></span> <span data-ttu-id="18b2b-238">Después, dentro de cualquier método del repositorio (normalmente una sobrecarga de List()) aceptaría una IQuerySpecification y ejecutaría la consulta esperada según esa especificación.</span><span class="sxs-lookup"><span data-stu-id="18b2b-238">Then, within any Repository method (usually a List() overload) it would accept an IQuerySpecification and run the expected query based on that specification.</span></span>

<span data-ttu-id="18b2b-239">Un ejemplo de una interfaz Specification genérica es el siguiente código de [eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb).</span><span class="sxs-lookup"><span data-stu-id="18b2b-239">An example of a generic Specification interface is the following code from [eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb).</span></span>

```csharp
// GENERIC SPECIFICATION INTERFACE
// https://github.com/dotnet-architecture/eShopOnWeb

public interface ISpecification<T>
{
    Expression<Func<T, bool>> Criteria { get; }
    List<Expression<Func<T, object>>> Includes { get; }
    List<string> IncludeStrings { get; }
}
```

<span data-ttu-id="18b2b-240">La siguiente es la implementación de una clase base de especificación genérica.</span><span class="sxs-lookup"><span data-stu-id="18b2b-240">Then, the implementation of a generic specification base class is the following.</span></span>

```csharp
// GENERIC SPECIFICATION IMPLEMENTATION (BASE CLASS)
// https://github.com/dotnet-architecture/eShopOnWeb

public abstract class BaseSpecification<T> : ISpecification<T>
{
    public BaseSpecification(Expression<Func<T, bool>> criteria)
    {
        Criteria = criteria;
    }
    public Expression<Func<T, bool>> Criteria { get; }

    public List<Expression<Func<T, object>>> Includes { get; } =
                                           new List<Expression<Func<T, object>>>();

    public List<string> IncludeStrings { get; } = new List<string>();

    protected virtual void AddInclude(Expression<Func<T, object>> includeExpression)
    {
        Includes.Add(includeExpression);
    }

    // string-based includes allow for including children of children
    // e.g. Basket.Items.Product
    protected virtual void AddInclude(string includeString)
    {
        IncludeStrings.Add(includeString);
    }
}
```

<span data-ttu-id="18b2b-241">La siguiente especificación carga una entidad de cesta única a partir del id. o del id. de comprador al que pertenece la cesta y</span><span class="sxs-lookup"><span data-stu-id="18b2b-241">The following specification loads a single basket entity given either the basket’s ID or the ID of the buyer to whom the basket belongs.</span></span> <span data-ttu-id="18b2b-242">realiza una [carga diligente](https://docs.microsoft.com/ef/core/querying/related-data) de la colección de artículos de la cesta.</span><span class="sxs-lookup"><span data-stu-id="18b2b-242">It will [eagerly load](https://docs.microsoft.com/ef/core/querying/related-data) the basket’s Items collection.</span></span>

```csharp
// SAMPLE QUERY SPECIFICATION IMPLEMENTATION

public class BasketWithItemsSpecification : BaseSpecification<Basket>
{
    public BasketWithItemsSpecification(int basketId)
        : base(b => b.Id == basketId)
    {
        AddInclude(b => b.Items);
    }
    public BasketWithItemsSpecification(string buyerId)
        : base(b => b.BuyerId == buyerId)
    {
        AddInclude(b => b.Items);
    }
}
```

<span data-ttu-id="18b2b-243">Por último, puede ver a continuación cómo un repositorio de EF genérico puede usar una especificación de este tipo para filtrar y cargar de forma diligente los datos relacionados con un determinado tipo de entidad T.</span><span class="sxs-lookup"><span data-stu-id="18b2b-243">And finally, you can see below how a generic EF Repository can use such a specification to filter and eager-load data related to a given entity type T.</span></span>

```csharp
// GENERIC EF REPOSITORY WITH SPECIFICATION
// https://github.com/dotnet-architecture/eShopOnWeb

public IEnumerable<T> List(ISpecification<T> spec)
{
    // fetch a Queryable that includes all expression-based includes
    var queryableResultWithIncludes = spec.Includes
        .Aggregate(_dbContext.Set<T>().AsQueryable(),
            (current, include) => current.Include(include));

    // modify the IQueryable to include any string-based include statements
    var secondaryResult = spec.IncludeStrings
        .Aggregate(queryableResultWithIncludes,
            (current, include) => current.Include(include));

    // return the result of the query using the specification's criteria expression
    return secondaryResult
                    .Where(spec.Criteria)
                    .AsEnumerable();
}
```

<span data-ttu-id="18b2b-244">Además de encapsular la lógica de filtro, puede especificar la forma de los datos que se van a devolver, incluidas las propiedades que se van a rellenar.</span><span class="sxs-lookup"><span data-stu-id="18b2b-244">In addition to encapsulating filtering logic, the specification can specify the shape of the data to be returned, including which properties to populate.</span></span>

<span data-ttu-id="18b2b-245">Aunque no se recomienda devolver IQueryable desde un repositorio, se puede usar perfectamente dentro del repositorio para crear un conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="18b2b-245">Although we don’t recommend to return IQueryable from a repository, it’s perfectly fine to use them within the repository to build up a set of results.</span></span> <span data-ttu-id="18b2b-246">Puede ver cómo se usa este enfoque en el método List anterior, en que se utilizan expresiones IQueryable intermedias para generar la lista de consultas de inclusión antes de ejecutar la consulta con los criterios de especificación de la última línea.</span><span class="sxs-lookup"><span data-stu-id="18b2b-246">You can see this approach used in the List method above, which uses intermediate IQueryable expressions to build up the query’s list of includes before executing the query with the specification’s criteria on the last line.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="18b2b-247">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="18b2b-247">Additional resources</span></span>

- <span data-ttu-id="18b2b-248">**Table Mapping** \ (Asignación de tabla)</span><span class="sxs-lookup"><span data-stu-id="18b2b-248">**Table Mapping** \</span></span>
  [https://docs.microsoft.com/ef/core/modeling/relational/tables](/ef/core/modeling/relational/tables)

- <span data-ttu-id="18b2b-249">**Use HiLo to generate keys with Entity Framework Core** \ (Uso de Hi-Lo para generar claves con Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="18b2b-249">**Use HiLo to generate keys with Entity Framework Core** \</span></span>
  <https://www.talkingdotnet.com/use-hilo-to-generate-keys-with-entity-framework-core/>

- <span data-ttu-id="18b2b-250">**Backing Fields** \ (Campos de respaldo)</span><span class="sxs-lookup"><span data-stu-id="18b2b-250">**Backing Fields** \</span></span>
  [https://docs.microsoft.com/ef/core/modeling/backing-field](/ef/core/modeling/backing-field)

- <span data-ttu-id="18b2b-251">**Steve Smith. Encapsulated Collections in Entity Framework Core** \ (Colecciones encapsuladas en Entity Framework Core)</span><span class="sxs-lookup"><span data-stu-id="18b2b-251">**Steve Smith. Encapsulated Collections in Entity Framework Core** \</span></span>
  <https://ardalis.com/encapsulated-collections-in-entity-framework-core>

- <span data-ttu-id="18b2b-252">**Propiedades reemplazadas** </span><span class="sxs-lookup"><span data-stu-id="18b2b-252">**Shadow Properties** </span></span>\
  [https://docs.microsoft.com/ef/core/modeling/shadow-properties](/ef/core/modeling/shadow-properties)

- <span data-ttu-id="18b2b-253">**The Specification pattern** \ (Patrón de especificación)</span><span class="sxs-lookup"><span data-stu-id="18b2b-253">**The Specification pattern** \</span></span>
  <https://deviq.com/specification-pattern/>

> [!div class="step-by-step"]
> <span data-ttu-id="18b2b-254">[Anterior](infrastructure-persistence-layer-design.md)
> [Siguiente](nosql-database-persistence-infrastructure.md)</span><span class="sxs-lookup"><span data-stu-id="18b2b-254">[Previous](infrastructure-persistence-layer-design.md)
[Next](nosql-database-persistence-infrastructure.md)</span></span>