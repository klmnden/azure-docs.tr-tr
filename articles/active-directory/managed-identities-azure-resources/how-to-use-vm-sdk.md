---
title: Azure SDK'ları ile Azure sanal makinesinde Azure kaynakları için yönetilen kimliklerini kullanma
description: Kod örnekleri, Azure SDK'ları Azure kaynakları için kimlikleri yönettiği bir Azure VM ile kullanmak için.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00c86562e0fdb4e6d62d44088b7aba08e45e22a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60293243"
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








