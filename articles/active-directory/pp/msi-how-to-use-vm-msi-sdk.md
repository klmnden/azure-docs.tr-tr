---
title: "Bir VM üzerinde bir kullanıcı tarafından atanan yönetilen hizmet kimlikten Azure SDK'ları kullanma"
description: "Bir kullanıcı tarafından atanan MSI bir VM'de Azure SDK'ları kullanarak için kod örnekleri."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 59d65e42c9b32bd0acd98645342833b4d57ad7a4
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>Bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI ile) Azure SDK'ları kullanın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]
Bu makale, kullanıcı tarafından atanan MSI desteği, ilgili Azure SDK'ın kullanımını göstermek SDK örnekleri listesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

> [!IMPORTANT]
> - Tüm örnek kodu/komut dosyası bu makalede varsayar istemci MSI özellikli sanal makine üzerinde çalışan. VM "Bağlan" özelliği Azure portalında uzaktan VM'nize bağlanmak için kullanın. Bir VM'de MSI etkinleştirme hakkında daha fazla bilgi için bkz: [bir VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md), veya değişken makaleleri (PowerShell, Azure portal, bir şablonu veya bir Azure SDK'sını kullanarak). 

## <a name="sdk-code-samples"></a>SDK kod örnekleri

| SDK             | Kod örneği |
| --------------- | ----------- |
| Python          | [Yalnızca gelen bir VM içinde kimlik doğrulaması için MSI kullanın](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [MSI özellikli bir sanal makineden kaynaklarını yönetme](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure SDK'ları](https://azure.microsoft.com/downloads/) Azure SDK kaynaklarını tam listesi için kitaplık yüklemeleri, belgelerine ve benzeri.

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.








