---
title: puntero a la función LPOVERLAPPED_COMPLETION_ROUTINE
ms.date: 03/30/2017
api_name:
- LPOVERLAPPED_COMPLETION_ROUTINE
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- LPOVERLAPPED_COMPLETION_ROUTINE
helpviewer_keywords:
- LPOVERLAPPED_COMPLETION_ROUTINE function pointer [.NET Framework hosting]
ms.assetid: 5fb645d9-b818-401c-8c2c-c30d86de58ba
topic_type:
- apiref
ms.openlocfilehash: 103ac75e7c3eaf9739c3a448ff1c052c158621db
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73090904"
---
# <a name="lpoverlapped_completion_routine-function-pointer"></a>puntero a la función LPOVERLAPPED_COMPLETION_ROUTINE
Apunta a una función que notifica al host cuando se ha completado una e/s superpuesta (es decir, asincrónica) a un dispositivo.  
  
 Este puntero de función ha quedado en desuso en el .NET Framework 4.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
typedef VOID (*LPOVERLAPPED_COMPLETION_ROUTINE) (  
    [in] DWORD  dwErrorCode,  
    [in] DWORD  dwNumberOfBytesTransfered,  
    [in] LPVOID lpOverlapped  
);  
```  
  
## <a name="parameters"></a>Parámetros  
 `dwErrorCode`  
 de Un valor que es un código de error si se ha cerrado el dispositivo; de lo contrario, este valor es cero.  
  
 Al cerrar un dispositivo, todas las e/s pendientes en el dispositivo se completarán inmediatamente.  
  
 `dwNumberOfBytesTransfered`  
 de Número de bytes transferidos por la operación de e/s.  
  
 `lpOverlapped`  
 de Puntero a una estructura que contiene información que se va a usar para completar la solicitud de e/s.  
  
## <a name="remarks"></a>Comentarios  
 Función a la que `LPOVERLAPPED_COMPLETION_ROUTINE` puntos es una función de devolución de llamada y debe ser implementada por el escritor de la aplicación de hospedaje. La función de devolución de llamada permite al host procesar la solicitud de e/s completada.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** MSCorEE. h  
  
 **Biblioteca:** MSCorWks. dll  
  
 **Versiones de .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vea también

- [Funciones de hospedaje de CLR en desuso](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
