---
title: Bir Azure AD uygulamanızı oluşturma ve Azure Media Services API'sine erişmek için yapılandırmak için Azure CLI kullanma | Microsoft Docs
description: Bu konuda, bir Azure AD uygulamanızı oluşturma ve bunu Azure Media Services API'sine erişmek için yapılandırmak üzere Azure CLI kullanma gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: e7ae5f83ff9dbb16733656a3bb4452ace750cf3f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64690112"
---
# <a name="use-azure-cli-to-create-an-azure-ad-app-and-configure-it-to-access-media-services-api"></a>Bir Azure AD uygulamanızı oluşturma ve Media Services API'sine erişmek için yapılandırmak için Azure CLI kullanma 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

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
