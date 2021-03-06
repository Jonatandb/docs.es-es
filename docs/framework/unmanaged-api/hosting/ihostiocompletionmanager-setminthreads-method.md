---
title: IHostIoCompletionManager::SetMinThreads (Método)
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager.SetMinThreads
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager::SetMinThreads
helpviewer_keywords:
- SetMinThreads method, IHostIoCompletionManager interface [.NET Framework hosting]
- IHostIoCompletionManager::SetMinThreads method [.NET Framework hosting]
ms.assetid: dea34b81-8d2b-4cc3-8696-0ad4291d8a92
topic_type:
- apiref
ms.openlocfilehash: 8b1fc936b601d327ce6306f22dbec2c1078718da
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73133733"
---
# <a name="ihostiocompletionmanagersetminthreads-method"></a>IHostIoCompletionManager::SetMinThreads (Método)
Establece el número mínimo de subprocesos que el host debe asignar a la finalización de e/s.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
HRESULT SetMinThreads (  
    [in] DWORD dwMinIoCompletionThreads  
);  
```  
  
## <a name="parameters"></a>Parámetros  
 `dwMinIoCompletionThreads`  
 de Número mínimo de subprocesos de finalización de e/s que debe crear el host.  
  
## <a name="return-value"></a>Valor devuelto  
  
|HRESULT|Descripción|  
|-------------|-----------------|  
|S_OK|`SetMinThreads` devolvió correctamente.|  
|HOST_E_CLRNOTAVAILABLE|El Common Language Runtime (CLR) no se ha cargado en un proceso o el CLR se encuentra en un estado en el que no puede ejecutar código administrado ni procesar la llamada correctamente.|  
|HOST_E_TIMEOUT|Se agotó el tiempo de espera de la llamada.|  
|HOST_E_NOT_OWNER|El autor de la llamada no posee el bloqueo.|  
|HOST_E_ABANDONED|Se canceló un evento mientras un subproceso o fibra bloqueados estaba esperando en él.|  
|E_FAIL|Se produjo un error grave desconocido. Cuando un método devuelve E_FAIL, el CLR ya no se puede usar en el proceso. Las llamadas subsiguientes a métodos de hospedaje devuelven HOST_E_CLRNOTAVAILABLE.|  
|E_NOTIMPL|El host no proporciona una implementación de `SetMinThreads`.|  
  
## <a name="remarks"></a>Comentarios  
 Un host podría querer controlar de forma exclusiva el número de subprocesos que se pueden asignar para procesar solicitudes de e/s, por motivos como la implementación, el rendimiento o la escalabilidad. Por esta razón, no es necesario que el host implemente `SetMinThreads`. En este caso, el host debe devolver E_NOTIMPL desde este método.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** MSCorEE. h  
  
 **Biblioteca:** Se incluye como recurso en MSCorEE. dll  
  
 **Versiones de .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Vea también

- [ICLRIoCompletionManager (interfaz)](../../../../docs/framework/unmanaged-api/hosting/iclriocompletionmanager-interface.md)
- [SetMaxThreads (método)](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-setmaxthreads-method.md)
- [IHostIoCompletionManager (interfaz)](../../../../docs/framework/unmanaged-api/hosting/ihostiocompletionmanager-interface.md)
