---
title: Estructura DacpReJitData
ms.date: 02/01/2019
api.name:
- DacpReJitData Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- DacpReJitData Structure
helpviewer.keywords:
- DacpReJitData Structure [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: 46031f29da6916eeaeea863ebef6924a720d7155
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793812"
---
# <a name="dacprejitdata-structure"></a>Estructura DacpReJitData

Define la información básica sobre un método instrumentado del generador de perfiles determinado.

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>Sintaxis

```cpp
struct MSLAYOUT DacpReJitData
{
    enum Flags
    {
        kUnknown,
        kRequested,
        kActive,
        kReverted,
    };

    CLRDATA_ADDRESS                 rejitID;
    Flags                           flags;
    CLRDATA_ADDRESS                 NativeCodeAddr;
};
```

## <a name="members"></a>Miembros

| Miembro           | Descripción                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| `rejitID`        | El número de revisión de ReJit para un método.                                                          |
| `flags`          | Marca que indica el estado actual de la instrumentación ReJit del método para la versión especificada. |
| `NativeCodeAddr` | Dirección base de la implementación de rejitted del método.                                         |

## <a name="remarks"></a>Notas

Esta estructura reside dentro del tiempo de ejecución y no se expone a través de los encabezados o archivos de biblioteca. Para usarlo, defina la estructura tal y como se especificó anteriormente. La estructura también se debe definir utilizando `ms_struct` Packing si no se usan los compiladores de Microsoft.

## <a name="requirements"></a>Requisitos de
**Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
**Encabezado:** Ninguna  
**Biblioteca:** Ninguna  
**.NET Framework versiones:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>Vea también

- [Depuración](index.md)
- [Estructuras de depuración](debugging-structures.md)
