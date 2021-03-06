---
title: Interfaz ICorDebugEval
ms.date: 03/30/2017
api_name:
- ICorDebugEval
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval
helpviewer_keywords:
- ICorDebugEval interface [.NET Framework debugging]
ms.assetid: 3a5c9815-832d-47e1-b7f7-bbba135d7cf1
topic_type:
- apiref
ms.openlocfilehash: c9e2dc95bf78d95e5d608a8ce6e9462345c20a07
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76788730"
---
# <a name="icordebugeval-interface"></a>Interfaz ICorDebugEval

Proporciona métodos que permiten al depurador ejecutar código en el contexto del código que se está depurando.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descripción|  
|------------|-----------------|  
|[Abort (método)](icordebugeval-abort-method.md)|Anula el cálculo que este objeto `ICorDebugEval` está realizando actualmente.|  
|[CallFunction (método)](icordebugeval-callfunction-method.md)|Configura una llamada a la función especificada. (Obsoleto en la .NET Framework versión 2,0; use [ICorDebugEval2:: CallParameterizedFunction (](icordebugeval2-callparameterizedfunction-method.md) en su lugar).|  
|[CreateValue (método)](icordebugeval-createvalue-method.md)|Obtiene un puntero de interfaz a un objeto "ICorDebugValue" del tipo especificado, con un valor inicial de cero o null. (Obsoleto en el .NET Framework 2,0; use [ICorDebugEval2:: createvaluefortype (](icordebugeval2-createvaluefortype-method.md) en su lugar).|  
|[GetResult (método)](icordebugeval-getresult-method.md)|Obtiene un puntero de interfaz a un `ICorDebugValue` que contiene los resultados de la evaluación.|  
|[GetThread (método)](icordebugeval-getthread-method.md)|Obtiene un puntero de interfaz a la expresión "ICorDebugThread" en la que se está ejecutando o se ejecutará esta evaluación.|  
|[IsActive (método)](icordebugeval-isactive-method.md)|Obtiene un valor que indica si este objeto `ICorDebugEval` se está ejecutando actualmente.|  
|[NewArray (método)](icordebugeval-newarray-method.md)|Asigna una nueva matriz del tipo de elemento y las dimensiones especificadas. (Obsoleto en el .NET Framework 2,0; use [ICorDebugEval2:: NewParameterizedArray (](icordebugeval2-newparameterizedarray-method.md) en su lugar).|  
|[NewObject (método)](icordebugeval-newobject-method.md)|Asigna una nueva instancia de objeto y llama al método de constructor especificado. (Obsoleto en el .NET Framework 2,0; use [ICorDebugEval2:: NewParameterizedObject (](icordebugeval2-newparameterizedobject-method.md) en su lugar).|  
|[NewObjectNoConstructor (método)](icordebugeval-newobjectnoconstructor-method.md)|Asigna una nueva instancia de objeto del tipo especificado, sin intentar llamar a un método de constructor. (Obsoleto en el .NET Framework 2,0; use [ICorDebugEval2:: newparameterizedobjectnoconstructor (](icordebugeval2-newparameterizedobjectnoconstructor-method.md) en su lugar).|  
|[NewString (método)](icordebugeval-newstring-method.md)|Asigna un nuevo objeto de cadena con el contenido especificado.|  
  
## <a name="remarks"></a>Notas  
 Se crea un objeto `ICorDebugEval` en el contexto de un subproceso específico que se utiliza para realizar las evaluaciones. Todos los objetos y tipos utilizados en una evaluación determinada deben residir en el mismo dominio de aplicación. Dicho dominio de aplicación no debe ser el mismo que el dominio de aplicación actual del subproceso. Las evaluaciones se pueden anidar.  
  
 Las operaciones de la evaluación no se completan hasta que el depurador llama a [ICorDebugController:: Continue](icordebugcontroller-continue-method.md)y, a continuación, recibe una devolución de llamada [ICorDebugManagedCallback:: evalcomplete (](icordebugmanagedcallback-evalcomplete-method.md) . Si necesita usar la funcionalidad de evaluación sin permitir que se ejecuten otros subprocesos, suspenda los subprocesos mediante [ICorDebugController:: setallthreadsdebugstate (](icordebugcontroller-setallthreadsdebugstate-method.md) o [ICorDebugController:: Stop](icordebugcontroller-stop-method.md) antes de llamar a [ICorDebugController:: Continue](icordebugcontroller-continue-method.md).  
  
 Dado que el código de usuario se está ejecutando cuando la evaluación está en curso, pueden producirse los eventos de depuración, incluidas las cargas de clases y los puntos de interrupción. El depurador recibirá devoluciones de llamada, como es habitual, para estos eventos. El estado de la evaluación se verá como parte de la inspección del estado normal del programa. La cadena de pila será una cadena de `CHAIN_FUNC_EVAL` (vea la enumeración "CorDebugStepReason (" y el método [ICorDebugChain:: GetReason (](icordebugchain-getreason-method.md) ). La API de depurador completa seguirá funcionando de la manera habitual.  
  
 Si se produce una situación de bucle indefinido o infinito, el código de usuario nunca puede completarse. En tal caso, debe llamar a [ICorDebugEval:: ABORT](icordebugeval-abort-method.md) antes de reanudar el programa.  
  
> [!NOTE]
> Esta interfaz no admite que se la llame de forma remota, ya sea entre procesos o entre equipos.  
  
## <a name="requirements"></a>Requisitos de  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versiones:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vea también

- [Interfaces de depuración](debugging-interfaces.md)
