---
title: 'Operador new: referencia de C#'
ms.custom: seodec18
ms.date: 06/25/2019
helpviewer_keywords:
- new operator keyword [C#]
ms.assetid: a212b697-a79b-4105-9923-1f7b108036e8
ms.openlocfilehash: 3d64c4805abe38c80301748ffa6b35fc87563b60
ms.sourcegitcommit: bab17fd81bab7886449217356084bf4881d6e7c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2019
ms.locfileid: "67403973"
---
# <a name="new-operator-c-reference"></a><span data-ttu-id="773d5-102">Operador new (referencia de C#)</span><span class="sxs-lookup"><span data-stu-id="773d5-102">new operator (C# reference)</span></span>

<span data-ttu-id="773d5-103">El operador `new` crea una nueva instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="773d5-103">The `new` operator creates a new instance of a type.</span></span>

<span data-ttu-id="773d5-104">También puede usar la palabra clave `new` como un [modificador de declaración de miembro](../keywords/new-modifier.md) o una [restricción de tipo genérico](../keywords/new-constraint.md).</span><span class="sxs-lookup"><span data-stu-id="773d5-104">You can also use the `new` keyword as a [member declaration modifier](../keywords/new-modifier.md) or a [generic type constraint](../keywords/new-constraint.md).</span></span>

## <a name="constructor-invocation"></a><span data-ttu-id="773d5-105">Invocación del constructor</span><span class="sxs-lookup"><span data-stu-id="773d5-105">Constructor invocation</span></span>

<span data-ttu-id="773d5-106">Para crear una nueva instancia de un tipo, normalmente se invoca uno de los [constructores](../../programming-guide/classes-and-structs/constructors.md) de ese tipo mediante el operador `new`:</span><span class="sxs-lookup"><span data-stu-id="773d5-106">To create a new instance of a type, you typically invoke one of the [constructors](../../programming-guide/classes-and-structs/constructors.md) of that type using the `new` operator:</span></span>

[!code-csharp-interactive[invoke constructor](~/samples/csharp/language-reference/operators/NewOperator.cs#Constructor)]

<span data-ttu-id="773d5-107">Puede usar un [inicializador de colección u objeto](../../programming-guide/classes-and-structs/object-and-collection-initializers.md) con el `new` operador para crear una instancia e inicializar un objeto en una sola instrucción, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="773d5-107">You can use an [object or collection initializer](../../programming-guide/classes-and-structs/object-and-collection-initializers.md) with the `new` operator to instantiate and initialize an object in one statement, as the following example shows:</span></span>

[!code-csharp-interactive[constructor with initializer](~/samples/csharp/language-reference/operators/NewOperator.cs#ConstructorWithInitializer)]

## <a name="array-creation"></a><span data-ttu-id="773d5-108">creación de matriz</span><span class="sxs-lookup"><span data-stu-id="773d5-108">Array creation</span></span>

<span data-ttu-id="773d5-109">También se usa el operador `new` para crear una instancia de matriz, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="773d5-109">You also use the `new` operator to create an array instance, as the following example shows:</span></span>

[!code-csharp-interactive[create array](~/samples/csharp/language-reference/operators/NewOperator.cs#Array)]

<span data-ttu-id="773d5-110">Utilice la sintaxis de inicialización de matriz para crear una instancia de matriz y rellenarla con los elementos en una sola instrucción.</span><span class="sxs-lookup"><span data-stu-id="773d5-110">Use array initialization syntax to create an array instance and populate it with elements in one statement.</span></span> <span data-ttu-id="773d5-111">En el ejemplo siguiente se muestran varias maneras de hacerlo:</span><span class="sxs-lookup"><span data-stu-id="773d5-111">The following example shows various ways how you can do that:</span></span>

[!code-csharp-interactive[initialize array](~/samples/csharp/language-reference/operators/NewOperator.cs#ArrayInitialization)]

<span data-ttu-id="773d5-112">Para obtener más información sobre las matrices, consulte [Matrices](../../programming-guide/arrays/index.md).</span><span class="sxs-lookup"><span data-stu-id="773d5-112">For more information about arrays, see [Arrays](../../programming-guide/arrays/index.md).</span></span>

## <a name="instantiation-of-anonymous-types"></a><span data-ttu-id="773d5-113">Creación de instancias de tipos anónimos</span><span class="sxs-lookup"><span data-stu-id="773d5-113">Instantiation of anonymous types</span></span>

<span data-ttu-id="773d5-114">Para crear una instancia de un [tipo anónimo](../../programming-guide/classes-and-structs/anonymous-types.md), utilice el operador `new` y la sintaxis de inicializador de objeto:</span><span class="sxs-lookup"><span data-stu-id="773d5-114">To create an instance of an [anonymous type](../../programming-guide/classes-and-structs/anonymous-types.md), use the `new` operator and object initializer syntax:</span></span>

[!code-csharp-interactive[anonymous type](~/samples/csharp/language-reference/operators/NewOperator.cs#AnonymousType)]

## <a name="destruction-of-type-instances"></a><span data-ttu-id="773d5-115">Destrucción de instancias de tipo</span><span class="sxs-lookup"><span data-stu-id="773d5-115">Destruction of type instances</span></span>

<span data-ttu-id="773d5-116">No tiene que destruir instancias de tipo creadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="773d5-116">You don't have to destroy earlier created type instances.</span></span> <span data-ttu-id="773d5-117">Las instancias de tipos de valor y referencia se destruyen automáticamente.</span><span class="sxs-lookup"><span data-stu-id="773d5-117">Instances of both reference and value types are destroyed automatically.</span></span> <span data-ttu-id="773d5-118">Las instancias de tipos de valor se destruyen en cuanto se destruye el contexto que los contiene.</span><span class="sxs-lookup"><span data-stu-id="773d5-118">Instances of value types are destroyed as soon as the context that contains them is destroyed.</span></span> <span data-ttu-id="773d5-119">Las instancias de tipos de referencia los destruye el [recolector de elementos no utilizados](../../../standard/garbage-collection/index.md) en un momento no especificado después de eliminarse la última referencia a ellos.</span><span class="sxs-lookup"><span data-stu-id="773d5-119">Instances of reference types are destroyed by the [garbage collector](../../../standard/garbage-collection/index.md) at unspecified time after the last reference to them is removed.</span></span>

<span data-ttu-id="773d5-120">Para los tipos que contienen recursos no administrados, como un identificador de archivo, lo recomendable es llevar a cabo una limpieza determinista para asegurarse de que los recursos que contienen se liberan tan pronto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="773d5-120">For types that contain unmanaged resources, for example, a file handle, it's recommended to employ deterministic clean-up to ensure that the resources they contain are released as soon as possible.</span></span> <span data-ttu-id="773d5-121">Para más información, vea la referencia de la API <xref:System.IDisposable?displayProperty=nameWithType> y el artículo sobre la [instrucción using](../keywords/using-statement.md).</span><span class="sxs-lookup"><span data-stu-id="773d5-121">For more information, see the <xref:System.IDisposable?displayProperty=nameWithType> API reference and the [using statement](../keywords/using-statement.md) article.</span></span>

## <a name="operator-overloadability"></a><span data-ttu-id="773d5-122">Posibilidad de sobrecarga del operador</span><span class="sxs-lookup"><span data-stu-id="773d5-122">Operator overloadability</span></span>

<span data-ttu-id="773d5-123">Un tipo definido por el usuario no puede sobrecargar el operador `new`.</span><span class="sxs-lookup"><span data-stu-id="773d5-123">A user-defined type cannot overload the `new` operator.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="773d5-124">Especificación del lenguaje C#</span><span class="sxs-lookup"><span data-stu-id="773d5-124">C# language specification</span></span>

<span data-ttu-id="773d5-125">Para más información, vea la sección [El operador new](~/_csharplang/spec/expressions.md#the-new-operator) de la [Especificación del lenguaje C#](~/_csharplang/spec/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="773d5-125">For more information, see [The new operator](~/_csharplang/spec/expressions.md#the-new-operator) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="773d5-126">Vea también</span><span class="sxs-lookup"><span data-stu-id="773d5-126">See also</span></span>

- [<span data-ttu-id="773d5-127">Referencia de C#</span><span class="sxs-lookup"><span data-stu-id="773d5-127">C# reference</span></span>](../index.md)
- [<span data-ttu-id="773d5-128">Operadores de C#</span><span class="sxs-lookup"><span data-stu-id="773d5-128">C# operators</span></span>](index.md)
- [<span data-ttu-id="773d5-129">Inicializadores de objeto y colección</span><span class="sxs-lookup"><span data-stu-id="773d5-129">Object and collection initializers</span></span>](../../programming-guide/classes-and-structs/object-and-collection-initializers.md)