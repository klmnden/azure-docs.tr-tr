---
title: Bir Azure AD uygulaması oluştur ve Azure Media Services API erişimini yapılandırmak için CLI 2.0 kullanın | Microsoft Docs
description: Bu konu, CLI 2.0 bir Azure AD uygulaması oluşturmak ve bunu Azure Media Services API erişmek için yapılandırmak için nasıl kullanılacağını gösterir.
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
ms.openlocfilehash: b640455b151d0e5d4701b8c076ee1a587b92f5b6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-cli-20-to-create-an-aad-app-and-configure-it-to-access-azure-media-services-api"></a>Bir AAD uygulaması oluşturma ve Azure Media Services API erişimini yapılandırmak için CLI 2.0 kullanın

Bu konu, CLI 2.0 Azure Media Services kaynaklara erişmek için bir Azure Active Directory (Azure AD) uygulama ve hizmet sorumlusu oluşturmak için nasıl kullanılacağını gösterir. 

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 
- Bir Media Services hesabı. Daha fazla bilgi için bkz: [Azure portalını kullanarak Azure Media Services hesabı oluşturma](media-services-portal-create-account.md).

## <a name="use-the-azure-cloud-shell"></a>Azure bulut kabuğunu kullanın

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Portalın üst gezinti bölmesinden bulut Kabuğu'nu başlatın.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Daha fazla bilgi için bkz: [Azure bulut Kabuk genel bakış](../cloud-shell/overview.md).

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-cli-20"></a>Bir Azure AD uygulaması oluşturma ve medya hesabına erişim izni CLI 2.0 ile yapılandırma
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> --password <strong password>
az role assignment create --assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Örneğin:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

Bu örnekte, **kapsam** tam kaynak media services hesabı yoludur. Ancak, **kapsam** herhangi bir düzeyde olabilir.

Örneğin, aşağıdaki düzeyleri biri olabilir:
 
* **Abonelik** düzeyi.
* **Kaynak grubu** düzeyi.
* **Kaynak** düzeyi (örneğin, bir ortam hesabı).

Daha fazla bilgi için bkz: [bir Azure hizmet sorumlusu Azure CLI 2.0 ile oluşturun.](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Ayrıca bkz. [Manage Role-Based erişim denetimini Azure komut satırı arabirimi ile](../role-based-access-control/role-assignments-cli.md). 

## <a name="next-steps"></a>Sonraki adımlar

Kullanmaya başlama [dosyaları hesabınıza Yükleniyor](media-services-portal-upload-files.md).
