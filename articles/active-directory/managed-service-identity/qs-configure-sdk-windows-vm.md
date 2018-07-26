---
title: Bir yönetilen hizmet kimliğini yapılandırmak nasıl bir Azure SDK'sını kullanarak Azure VM etkin
description: Yapılandırma ve bir Azure SDK'sını kullanarak Azure sanal makinesinde, bir yönetilen hizmet kimliği kullanarak yönergeleri adım adım.
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
ms.date: 09/28/2017
ms.author: daveba
ms.openlocfilehash: 2763c78d309f5a90d68429caa46581e50f8b4303
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257669"
---
# <a name="configure-a-vm-managed-service-identity-using-an-azure-sdk"></a>Bir Azure SDK'sını kullanarak bir VM-Managed hizmet kimliği yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği otomatik olarak yönetilen bir kimlik Azure Active Directory (AD) ile Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve bir Azure SDK'sını kullanarak Azure VM için Yönetilen hizmet kimliği kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-service-identity-support"></a>Azure SDK'ları ile yönetilen hizmet kimliği desteği 

Azure, bir dizi birden çok programlama platformları destekler [Azure SDK'ları](https://azure.microsoft.com/downloads). Birkaç tanesinin yönetilen hizmet kimliği desteklemek ve kullanımını göstermek için ilgili örnekleri sağlamak üzere güncelleştirilmiştir. Bu liste, ek destek eklendikçe güncelleştirilir:

| SDK | Örnek |
| --- | ------ | 
| .NET   | [MSI etkin bir VM'den kaynak yönetme](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [MSI etkin bir VM'den depolama alanını Yönet](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [MSI etkin bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [MSI etkin bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Bir MSI ile Azure VM oluşturma](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

- "Yapılandırma yönetilen hizmet kimliği için bir Azure Azure portalı, PowerShell, CLI ve kaynak şablonları da nasıl kullanabileceğinizi öğrenmek için VM" altında ilgili makalelerine bakın.

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
