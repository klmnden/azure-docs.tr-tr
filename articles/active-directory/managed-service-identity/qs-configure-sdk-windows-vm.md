---
title: Bir MSI etkin bir Azure SDK'sını kullanarak Azure VM yapılandırma
description: Yapılandırma ve bir Azure SDK'sını kullanarak Azure sanal makinesinde, bir yönetilen hizmet kimliği (MSI) kullanma yönergeleri adım adım.
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
ms.openlocfilehash: dee4a3e27623150ce3fa648d73542db0cbb23e93
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901451"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Bir VM-Managed hizmet kimliği (bir Azure SDK'sını kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği otomatik olarak yönetilen bir kimlik Azure Active Directory (AD) ile Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve bir Azure SDK'sını kullanarak Azure VM için MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>MSI desteğiyle Azure SDK'ları 

Azure, bir dizi birden çok programlama platformları destekler [Azure SDK'ları](https://azure.microsoft.com/downloads). Birkaç tanesinin MSI desteklemek ve kullanımını göstermek için ilgili örnekleri sağlamak üzere güncelleştirilmiştir. Bu liste, ek destek eklendikçe güncelleştirilir:

| SDK | Örnek |
| --- | ------ | 
| .NET   | [MSI etkin bir VM'den kaynak yönetme](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [MSI etkin bir VM'den depolama alanını Yönet](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [MSI etkin bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [MSI etkin bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Bir MSI ile Azure VM oluşturma](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

- "Yapılandırma MSI için bir Azure portalı, PowerShell, CLI ve kaynak şablonları da nasıl kullanabileceğinizi öğrenmek için Azure sanal makinesi", altında ilgili makalelerine bakın.

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
