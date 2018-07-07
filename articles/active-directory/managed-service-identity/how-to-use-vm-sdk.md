---
title: Azure SDK'ları ile bir Azure VM yönetilen hizmet kimliği kullanma
description: Kod örnekleri, Azure SDK'ları ile Azure VM MSI kullanma.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 1c7a0d10f8ff005d90bb77f33bf40a00f97e078f
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901765"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-with-azure-sdks"></a>Azure SDK'ları ile bir Azure VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede, MSI desteği, ilgili Azure SDK'ın kullanımını gösteren SDK örnekleri, bir listesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Tüm bu makaledeki örnek kodun/betik varsayar istemci MSI etkin bir sanal makinede çalışıyor. VM "Bağlan" özelliği Azure Portalı'nda uzaktan VM'nize bağlanmak için kullanın. Bir VM'deki MSI etkinleştirme hakkında daha fazla bilgi için bkz [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md), veya değişken makaleleri (PowerShell, CLI, bir şablon veya bir Azure SDK'sını kullanarak). 

## <a name="sdk-code-samples"></a>SDK kod örnekleri

| SDK             | Kod örneği |
| --------------- | ----------- |
| .NET            | [Bir Azure Resource Manager şablonundan VM yönetilen hizmet kimliği kullanarak bir Windows VM dağıtma](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure Hizmetleri, yönetilen hizmet kimliği kullanarak bir Linux sanal makinesinden çağırın](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Yönetilen hizmet kimliği kullanarak kaynakları yönetme](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Yalnızca bir sanal makine içinde kimlik doğrulaması için MSI kullanma](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [MSI etkin bir VM'den kaynaklarını yönetme](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="related-content"></a>İlgili içerik

- Bkz: [Azure SDK'ları](https://azure.microsoft.com/downloads/) Azure SDK'sı kaynakların tam listesi için kitaplık dosyalara, belgelere ve daha fazlası dahil olmak üzere.
- Azure VM'deki MSI etkinleştirmek için bkz: [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.








