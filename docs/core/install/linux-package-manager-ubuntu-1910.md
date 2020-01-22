---
title: 'Instalación de .NET Core en el administrador de paquetes de Ubuntu 19.10: .NET Core'
description: Use un administrador de paquetes para instalar el SDK y el runtime de .NET Core en Ubuntu 19.10.
author: thraka
ms.author: adegeo
ms.date: 01/16/2020
ms.openlocfilehash: afba761e2237ed84528157841e538a9b44d9a966
ms.sourcegitcommit: 5d769956a04b6d68484dd717077fabc191c21da5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76164073"
---
# <a name="ubuntu-1910-package-manager---install-net-core"></a><span data-ttu-id="81d72-103">Administrador de paquetes de Ubuntu 19.10: instalación de .NET Core</span><span class="sxs-lookup"><span data-stu-id="81d72-103">Ubuntu 19.10 Package Manager - Install .NET Core</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

<span data-ttu-id="81d72-104">En este artículo se explica cómo usar un administrador de paquetes para instalar .NET Core en Ubuntu 19.10.</span><span class="sxs-lookup"><span data-stu-id="81d72-104">This article describes how to use a package manager to install .NET Core on Ubuntu 19.10.</span></span> <span data-ttu-id="81d72-105">Si va a instalar el entorno de ejecución, le recomendamos que instale el [entorno de ejecución de ASP.NET Core](#install-the-aspnet-core-runtime), ya que incluye los de .NET Core y ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81d72-105">If you're installing the runtime, we suggest you install the [ASP.NET Core runtime](#install-the-aspnet-core-runtime), as it includes both .NET Core and ASP.NET Core runtimes.</span></span>

## <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="81d72-106">Registro de la clave y la fuente de Microsoft</span><span class="sxs-lookup"><span data-stu-id="81d72-106">Register Microsoft key and feed</span></span>

<span data-ttu-id="81d72-107">Antes de instalar .NET, deberá realizar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="81d72-107">Before installing .NET, you'll need to:</span></span>

- <span data-ttu-id="81d72-108">Registrar la clave de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="81d72-108">Register the Microsoft key.</span></span>
- <span data-ttu-id="81d72-109">Registrar el repositorio del producto.</span><span class="sxs-lookup"><span data-stu-id="81d72-109">Register the product repository.</span></span>
- <span data-ttu-id="81d72-110">Instalar las dependencias necesarias.</span><span class="sxs-lookup"><span data-stu-id="81d72-110">Install required dependencies.</span></span>

<span data-ttu-id="81d72-111">Esto solo se debe hacer una vez por máquina.</span><span class="sxs-lookup"><span data-stu-id="81d72-111">This only needs to be done once per machine.</span></span>

<span data-ttu-id="81d72-112">Abra un terminal y ejecute los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="81d72-112">Open a terminal and run the following commands.</span></span>

```bash
wget -q https://packages.microsoft.com/config/ubuntu/19.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-core-sdk"></a><span data-ttu-id="81d72-113">Instalación del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="81d72-113">Install the .NET Core SDK</span></span>

<span data-ttu-id="81d72-114">Actualice los productos disponibles para la instalación y, después, instale el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="81d72-114">Update the products available for installation, then install the .NET Core SDK.</span></span> <span data-ttu-id="81d72-115">En el terminal, ejecute los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="81d72-115">In your terminal, run the following commands.</span></span>

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-3.1
```

> [!IMPORTANT]
> <span data-ttu-id="81d72-116">Si recibe un mensaje de error similar a **No se puede encontrar el paquete dotnet-sdk-3.1**, vea la sección [Solución de problemas del administrador de paquetes](#troubleshoot-the-package-manager).</span><span class="sxs-lookup"><span data-stu-id="81d72-116">If you receive an error message similar to **Unable to locate package dotnet-sdk-3.1**, see the [Troubleshoot the package manager](#troubleshoot-the-package-manager) section.</span></span>

## <a name="install-the-aspnet-core-runtime"></a><span data-ttu-id="81d72-117">Instalación del entorno de ejecución de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81d72-117">Install the ASP.NET Core runtime</span></span>

<span data-ttu-id="81d72-118">Actualice los productos disponibles para la instalación y, después, instale el entorno de ejecución de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81d72-118">Update the products available for installation, then install the ASP.NET Core runtime.</span></span> <span data-ttu-id="81d72-119">En el terminal, ejecute los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="81d72-119">In your terminal, run the following commands.</span></span>

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install aspnetcore-runtime-3.1
```

> [!IMPORTANT]
> <span data-ttu-id="81d72-120">Si recibe un mensaje de error similar a **No se puede encontrar el paquete aspnetcore-runtime-3.1**, vea la sección [Solución de problemas del administrador de paquetes](#troubleshoot-the-package-manager).</span><span class="sxs-lookup"><span data-stu-id="81d72-120">If you receive an error message similar to **Unable to locate package aspnetcore-runtime-3.1**, see the [Troubleshoot the package manager](#troubleshoot-the-package-manager) section.</span></span>

## <a name="install-the-net-core-runtime"></a><span data-ttu-id="81d72-121">Instalación del entorno de ejecución de .NET Core</span><span class="sxs-lookup"><span data-stu-id="81d72-121">Install the .NET Core runtime</span></span>

<span data-ttu-id="81d72-122">Actualice los productos disponibles para la instalación y, después, instale el entorno de ejecución de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="81d72-122">Update the products available for installation, then install the .NET Core runtime.</span></span> <span data-ttu-id="81d72-123">En el terminal, ejecute los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="81d72-123">In your terminal, run the following commands.</span></span>

```bash
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-runtime-3.1
```

> [!IMPORTANT]
> <span data-ttu-id="81d72-124">Si recibe un mensaje de error similar a **No se puede encontrar el paquete dotnet-runtime-3.1**, vea la sección [Solución de problemas del administrador de paquetes](#troubleshoot-the-package-manager).</span><span class="sxs-lookup"><span data-stu-id="81d72-124">If you receive an error message similar to **Unable to locate package dotnet-runtime-3.1**, see the [Troubleshoot the package manager](#troubleshoot-the-package-manager) section.</span></span>

## <a name="how-to-install-other-versions"></a><span data-ttu-id="81d72-125">Procedimiento para instalar otras versiones</span><span class="sxs-lookup"><span data-stu-id="81d72-125">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="81d72-126">Solución de problemas del administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="81d72-126">Troubleshoot the package manager</span></span>

<span data-ttu-id="81d72-127">Si recibe un mensaje de error similar a **No se puede encontrar el paquete (el paquete .NET Core)** , ejecute los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="81d72-127">If you receive an error message similar to **Unable to locate package {the .NET Core package}**, run the following commands.</span></span>

```bash
sudo dpkg --purge packages-microsoft-prod && sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
sudo apt-get install {the .NET Core package}
```

<span data-ttu-id="81d72-128">Si eso no funciona, puede ejecutar una instalación manual con los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="81d72-128">If that doesn't work, you can run a manual install with the following commands.</span></span>

```bash
sudo apt-get install -y gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget -q https://packages.microsoft.com/config/ubuntu/19.10/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get install -y apt-transport-https
sudo apt-get update
sudo apt-get install {the .NET Core package}
```