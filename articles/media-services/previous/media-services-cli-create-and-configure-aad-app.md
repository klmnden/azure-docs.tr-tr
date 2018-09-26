---
title: Bir Azure AD uygulamanızı oluşturma ve Azure Media Services API'sine erişmek için yapılandırmak için Azure CLI kullanma | Microsoft Docs
description: Bu konuda, bir Azure AD uygulamanızı oluşturma ve bunu Azure Media Services API'sine erişmek için yapılandırmak üzere Azure CLI kullanma gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: fcd0ea10bd39f9e7252e114e8d6401a4fe0ecadb
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47096557"
---
# <a name="use-azure-cli-to-create-an-aad-app-and-configure-it-to-access-azure-media-services-api"></a>AAD uygulamasını oluşturmak ve bunu Azure Media Services API'sine erişmek için yapılandırmak için Azure CLI kullanma

Bu konu Azure Media Services kaynaklarına erişmek için bir Azure Active Directory (Azure AD) uygulama ve hizmet sorumlusu oluşturmak için Azure CLI'yı kullanmayı gösterir. 

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için [Azure portalını kullanarak bir Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).

## <a name="use-the-azure-cloud-shell"></a>Azure Cloud Shell kullanma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Portalın üst gezinti bölmesinden Cloud Shell'i başlatın.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Daha fazla bilgi için [Azure Cloud shell'e genel bakış](../../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-azure-cli"></a>Bir Azure AD uygulamanızı oluşturma ve Azure CLI ile medya hesabına erişimi yapılandırma
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create --assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Örneğin:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

Bu örnekte, **kapsam** tam kaynak medya hizmetleri hesabı yoludur. Ancak, **kapsam** herhangi bir düzeyde olabilir.

Örneğin, aşağıdaki düzeylerde biri olabilir:
 
* **Abonelik** düzeyi.
* **Kaynak grubu** düzeyi.
* **Kaynak** düzeyi (örneğin, bir medya hesabı).

Daha fazla bilgi için [Azure, Azure CLI ile hizmet sorumlusu oluşturma](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Ayrıca bkz: [Manage Role-Based erişim denetimini Azure komut satırı arabirimi ile](../../role-based-access-control/role-assignments-cli.md). 

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-portal-upload-files.md).
