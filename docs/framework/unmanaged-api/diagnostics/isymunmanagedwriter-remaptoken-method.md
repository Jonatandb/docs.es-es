---
title: ISymUnmanagedWriter::RemapToken (Método)
ms.date: 03/30/2017
api_name:
- ISymUnmanagedWriter.RemapToken
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- ISymUnmanagedWriter::RemapToken
helpviewer_keywords:
- ISymUnmanagedWriter::RemapToken method [.NET Framework debugging]
- RemapToken method [.NET Framework debugging]
ms.assetid: bca92682-ee1e-467f-8fb0-d8d4617f82fe
topic_type:
- apiref
ms.openlocfilehash: 9e441d4ff39632d9381e445ee99249d04539ad87
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74427888"
---
# <a name="isymunmanagedwriterremaptoken-method"></a>ISymUnmanagedWriter::RemapToken (Método)
Notifica al escritor de símbolos que se ha reasignado un token de metadatos cuando se emitieron los metadatos. Si el escritor de símbolos ha almacenado el token anterior en el almacén de símbolos, debe actualizar el token almacenado con el nuevo valor, o bien debe guardar el mapa del lector de símbolos correspondiente para reasignarlo durante la fase de lectura.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
HRESULT RemapToken(  
    [in] mdToken  oldToken,  
    [in] mdToken  newToken);  
```  
  
## <a name="parameters"></a>Parámetros  
 `oldToken`  
 de Token de metadatos que se reasignó.  
  
 `newToken`  
 de Nuevo token de metadatos al que se ha reasignado `oldToken`.  
  
## <a name="return-value"></a>Valor devuelto  
 S_OK si el método se ejecuta correctamente; de lo contrario, E_FAIL u otro código de error.  
  
## <a name="requirements"></a>Requisitos  
 **Encabezado:** CorSym. idl, CorSym. h  
  
## <a name="see-also"></a>Vea también

- [ISymUnmanagedWriter (interfaz)](../../../../docs/framework/unmanaged-api/diagnostics/isymunmanagedwriter-interface.md)
