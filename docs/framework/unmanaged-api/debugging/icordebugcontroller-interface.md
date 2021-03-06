---
title: Interfaz ICorDebugController
ms.date: 03/30/2017
api_name:
- ICorDebugController
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController
helpviewer_keywords:
- ICorDebugController interface [.NET Framework debugging]
ms.assetid: dbb1c4dc-269a-459b-ab1d-6c70788782ce
topic_type:
- apiref
ms.openlocfilehash: d6c923f03309da3ad8092ea6119e7d850120ee2c
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76783805"
---
# <a name="icordebugcontroller-interface"></a>Interfaz ICorDebugController

Representa un ámbito, <xref:System.Diagnostics.Process> o <xref:System.AppDomain>, en el que se puede controlar el contexto de ejecución de código.  
  
## <a name="methods"></a>Métodos  
  
|Método|Descripción|  
|------------|-----------------|  
|`ICorDebugController::CanCommitChanges`|Este método está obsoleto.|  
|`ICorDebugController::CommitChanges`|Este método está obsoleto.|  
|[Continue (método)](icordebugcontroller-continue-method.md)|Reanuda la ejecución de subprocesos administrados después de una llamada a [ICorDebugController:: Stop](icordebugcontroller-stop-method.md).|  
|[Detach (método)](icordebugcontroller-detach-method.md)|Desasocia el depurador del dominio del proceso o de la aplicación.|  
|[EnumerateThreads (método)](icordebugcontroller-enumeratethreads-method.md)|Obtiene un enumerador para los subprocesos administrados activos en el proceso.|  
|[HasQueuedCallbacks (método)](icordebugcontroller-hasqueuedcallbacks-method.md)|Obtiene un valor que indica si las devoluciones de llamada administradas están actualmente en la cola del subproceso especificado.|  
|[IsRunning (método)](icordebugcontroller-isrunning-method.md)|Obtiene un valor que indica si los subprocesos del proceso se están ejecutando libremente.|  
|[SetAllThreadsDebugState (método)](icordebugcontroller-setallthreadsdebugstate-method.md)|Establece el estado de depuración de todos los subprocesos administrados en el proceso.|  
|[Stop (método)](icordebugcontroller-stop-method.md)|Realiza una detención cooperativa en todos los subprocesos que ejecutan código administrado en el proceso.|  
|[Terminate (método)](icordebugcontroller-terminate-method.md)|Finaliza el proceso con el código de salida especificado.|  
  
## <a name="remarks"></a>Notas  
 Si `ICorDebugController` controla un proceso, el ámbito incluye todos los subprocesos del proceso. Si `ICorDebugController` está controlando un dominio de aplicación, el ámbito solo incluye los subprocesos de ese dominio de aplicación concreto.  
  
> [!NOTE]
> Esta interfaz no admite que se la llame de forma remota, ya sea entre procesos o entre equipos.  
  
## <a name="requirements"></a>Requisitos de  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versiones:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vea también

- [Interfaces de depuración](debugging-interfaces.md)
