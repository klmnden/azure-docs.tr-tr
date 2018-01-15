---
title: "Bir Azure VM yönetilen hizmet kimliği Azure SDK'ları ile kullanma"
description: "Azure SDK'ları ile Azure VM MSI kullanmak için kod örnekleri."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 6c9bd57080c95e4aad45515a0814471194261286
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-with-azure-sdks"></a>Azure SDK'ları ile bir Azure VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]  
Bu makale, kendi ilgili Azure SDK'ın destek MSI kullanımını göstermek SDK örnekleri listesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Tüm örnek kodu/komut dosyası bu makalede varsayar istemci MSI özellikli sanal makine üzerinde çalışan. VM "Bağlan" özelliği Azure portalında uzaktan VM'nize bağlanmak için kullanın. Bir VM'de MSI etkinleştirme hakkında daha fazla bilgi için bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md), veya değişken makaleleri (PowerShell, CLI, şablon veya bir Azure SDK'sını kullanarak). 

## <a name="sdk-code-samples"></a>SDK kod örnekleri

| SDK             | Kod örneği |
| --------------- | ----------- |
| .NET            | [Yönetilen hizmet kimliği kullanarak bir Windows VM bir Azure Resource Manager şablonu dağıtma](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure Hizmetleri yönetilen hizmet kimliği kullanarak bir Linux VM çağırın](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Yönetilen hizmet kimliği kullanarak kaynakları yönetme](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Yalnızca gelen bir VM içinde kimlik doğrulaması için MSI kullanın](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [MSI özellikli bir sanal makineden kaynaklarını yönetme](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="related-content"></a>İlgili içerik

- Bkz: [Azure SDK'ları](https://azure.microsoft.com/downloads/) Azure SDK kaynaklarını tam listesi için kitaplık yüklemeleri, belgelerine ve benzeri.
- Azure VM'de MSI etkinleştirmek için bkz: [bir VM yönetilen hizmet kimliği (Azure Portalı'nı kullanarak MSI) yapılandırma](msi-qs-configure-portal-windows-vm.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.








