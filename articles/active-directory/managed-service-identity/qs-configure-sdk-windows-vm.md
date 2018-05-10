---
title: Bir MSI etkin Azure VM bir Azure SDK'sını kullanarak yapılandırma
description: Adım adım yönergeler yapılandırma ve Azure bir Azure SDK'yı kullanarak bir VM, bir yönetilen hizmet Kimliği'ni (MSI) kullanarak tarafından.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/28/2017
ms.author: daveba
ms.openlocfilehash: 781f332b2892d9af536bf9a6f81642842285927b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Bir VM yönetilen hizmet kimliği (bir Azure SDK'sını kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği de Azure Active Directory (AD) otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, etkinleştirme ve Azure bir Azure SDK'sını kullanarak VM için MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>Azure SDK'ları ile MSI desteği 

Azure destekleyen bir dizi programlama birden çok platform [Azure SDK'ları](https://azure.microsoft.com/downloads). Birkaç MSI desteklemek ve kullanım göstermek için karşılık gelen örnekleri sağlamak üzere güncelleştirilmiştir. Ek destek eklendikçe bu listeyi güncelleştirilmiştir:

| SDK | Örnek |
| --- | ------ | 
| .NET   | [MSI özellikli bir sanal makineden kaynak yönetme](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [MSI özellikli bir sanal makineden depolama yönetin](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [MSI etkin bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [MSI etkin bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Bir MSI ile Azure VM oluşturma](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

- "Yapılandırma MSI için bir Azure Azure portal, PowerShell'i, CLI ve kaynak şablonları da nasıl kullanabileceğinizi öğrenmek için VM" altında ilgili makalelerine bakın.

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
