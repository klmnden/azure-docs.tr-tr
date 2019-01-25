---
title: Azure SDK'ları ile Azure sanal makinesinde Azure kaynakları için yönetilen kimliklerini kullanma
description: Kod örnekleri, Azure SDK'ları Azure kaynakları için kimlikleri yönettiği bir Azure VM ile kullanmak için.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: priyamo
ms.openlocfilehash: 0d6d122e7f05a14344292fe84608c384c5dcc306
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882064"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Azure SDK'ları ile Azure sanal makinesinde Azure kaynakları için yönetilen kimliklerini kullanma 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede, Azure kaynakları için yönetilen kimlikleri için destek, ilgili Azure SDK'ın kullanımını gösteren SDK örnekleri bir listesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Tüm bu makaledeki örnek kodun/betik etkin Azure kaynakları için yönetilen kimliklerle bir VM'de çalıştıran istemci varsayar. VM "Bağlan" özelliği Azure Portalı'nda uzaktan VM'nize bağlanmak için kullanın. Bir VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirme hakkında daha fazla bilgi için bkz [yapılandırma kimlikleri Azure portalını kullanarak bir VM üzerindeki Azure kaynakları için yönetilen](qs-configure-portal-windows-vm.md), ya da (PowerShell, CLI, bir şablon veya bir Azure kullanarak değişken makalelerden birine SDK'SI). 

## <a name="sdk-code-samples"></a>SDK kod örnekleri

| SDK             | Kod örneği |
| --------------- | ----------- |
| .NET            | [Windows Azure kaynakları için yönetilen kimliklerle bir VM'den bir Azure Resource Manager şablonu dağıtma](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure kaynakları için yönetilen kimlik kullanarak bir Linux VM Azure Hizmetleri çağırmanıza](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Azure kaynakları için yönetilen kimlik kullanarak kaynakları yönetme](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Yalnızca bir sanal makine içinde kimlik doğrulaması için Azure kaynakları için yönetilen kimlikleri kullanın](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Etkin Azure kaynakları için yönetilen kimliklerle bir VM'den kaynaklarını yönetme](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure SDK'ları](https://azure.microsoft.com/downloads/) Azure SDK'sı kaynakların tam listesi için kitaplık dosyalara, belgelere ve daha fazlası dahil olmak üzere.
- Azure sanal makinesinde Azure kaynakları için yönetilen kimlikleri etkinleştirmek için bkz: [yapılandırma kimlikleri Azure portalını kullanarak bir VM üzerindeki Azure kaynakları için yönetilen](qs-configure-portal-windows-vm.md).








