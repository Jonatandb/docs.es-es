---
title: COR_ARRAY_LAYOUT (Estructura)
ms.date: 03/30/2017
api_name:
- COR_ARRAY_LAYOUT
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_ARRAY_LAYOUT
helpviewer_keywords:
- COR_DEBUG_IL_TO_NATIVE_MAP structure [.NET Framework debugging]
ms.assetid: aa20ac3d-6f60-4aa2-91c5-f3a86f82eba8
topic_type:
- apiref
ms.openlocfilehash: ca2d00611a7530dfb0d1c2a27123947bdf69820d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2020
ms.locfileid: "79179353"
---
# <a name="cor_array_layout-structure"></a>COR_ARRAY_LAYOUT (Estructura)
Proporciona información sobre la distribución de un objeto de matriz en la memoria.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
typedef struct COR_ARRAY_LAYOUT {  
    COR_TYPEID componentID;  
    CorElementType componentType;  
    ULONG32 firstElementOffset;  
    ULONG32 elementSize;  
    ULONG32 countOffset;
    ULONG32 rankSize;
    ULONG32 numRanks;
    ULONG32 rankOffset;
} COR_ARRAY_LAYOUT;  
```  
  
## <a name="members"></a>Members  
  
|Member|Descripción|  
|------------|-----------------|  
|`componentID`|Identificador del tipo de objetos que contiene la matriz.|  
|`componentType`|Un corElementType valor de enumeración que indica si el componente es una referencia de recolección de elementos no utilizados, una clase de valor o una primitiva.|  
|`firstElementOffset`|Desplazamiento al primer elemento de la matriz.|  
|`elementSize`|El tamaño de cada elemento.|  
|`countOffset`|Desplazamiento al número de elementos de la matriz.|  
|`rankSize`|El tamaño del rango, en bytes.|  
|`numRanks`|El número de rangos en la matriz.|  
|`rankOffset`|Desplazamiento en el que comienzan los rangos.|  
  
## <a name="remarks"></a>Observaciones  
 El `rankSize` campo especifica el tamaño de un rango en una matriz multidimensional. También es preciso para matrices unidimensionales.  
  
 El valor `numRanks` de es 1 para `N` una matriz unidimensional `N` y para una matriz multidimensional de dimensiones.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Vea [Requisitos de sistema](../../get-started/system-requirements.md).  
  
 **Encabezado:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **Versiones de .NET Framework:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>Consulte también

- [Estructuras de depuración](debugging-structures.md)
- [Depuración](index.md)
