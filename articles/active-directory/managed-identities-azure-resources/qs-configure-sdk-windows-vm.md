---
title: Azure kaynakları için yönetilen kimliklerle bir VM'yi yapılandırmak için bir Azure SDK'sını kullanma
description: Adım adım yapılandırma ve kullanma yönergeleri için Azure SDK'sını kullanarak Azure VM, Azure kaynaklarında kimlik yönetilen.
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
ms.date: 09/28/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 29446f6f0a86a00cc92fe46211caddd22aaa4859
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66112566"
---
# <a name="configure-a-vm-with-managed-identities-for-azure-resources-using-an-azure-sdk"></a>Bir Azure SDK'sını kullanarak Azure kaynakları için yönetilen kimliklerle bir VM yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri otomatik olarak yönetilen bir kimlik Azure Active Directory (AD) ile Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve bir Azure SDK'sını kullanarak Azure VM için Azure kaynakları için yönetilen kimlikleri kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-identities-for-azure-resources-support"></a>Azure SDK'ları Azure kaynakları için yönetilen kimliklerle desteği 

Azure, bir dizi birden çok programlama platformları destekler [Azure SDK'ları](https://azure.microsoft.com/downloads). Birkaç Azure kaynakları için yönetilen kimliklerini desteklemek ve kullanımını göstermek için ilgili örnekleri sağlamak için güncelleştirildi. Bu liste, ek destek eklendikçe güncelleştirilir:

| SDK | Örnek |
| --- | ------ | 
| .NET   | [Etkin Azure kaynakları için yönetilen kimliklerle etkin bir VM'den kaynak yönetme](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Azure kaynakları için yönetilen kimliklerle etkin bir VM'den depolama alanını Yönet](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Etkin sistem tarafından atanan yönetilen kimlikle bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Etkin sistem tarafından atanan yönetilen kimlikle bir VM oluşturma](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Sistem tarafından atanan kimliği etkin bir Azure VM oluşturma](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Sonraki adımlar

- Altında ilgili makaleleri görmek **Azure VM için kimlik yapılandırma**, Azure portalı, PowerShell, CLI ve kaynak şablonları da nasıl kullanabileceğinizi öğrenin.
