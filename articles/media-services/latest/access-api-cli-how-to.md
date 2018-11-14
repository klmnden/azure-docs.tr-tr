---
title: Azure Media Services API'sine - Azure CLI erişim | Microsoft Docs
description: Bu yöntem, Azure Media Services API'sine erişmek için adımları izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 54a92e56df21b59430ed12f191a9cf7a918e14c9
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612908"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Azure CLI ile API erişim Azure medya Hizmetleri
 
Azure Media Services API'sine bağlanmak için Azure AD hizmet sorumlusu kimlik doğrulaması kullanmanız gerekir. Aşağıdaki parametrelere sahip bir Azure AD belirteç istemek uygulamanız gerekir:

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si
* Kaynak URI REST Media Services için
* Azure AD uygulama değerlerini: istemci Kimliğini ve istemci gizli anahtarı

Bu makalede bir Azure AD uygulaması ve hizmet sorumlusu oluşturma ve Azure Media Services kaynaklarına erişmek için gerekli değerleri almak için Azure CLI'yı kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar 

- Yükleyin ve bu makalede Azure CLI 2.0 veya sonraki bir sürüm gerektirir, CLI'yı yerel olarak kullanın. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

    Şu anda tüm [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) komutlar Azure Cloud Shell içinde çalışır. CLI'yi yerel olarak kullanmak için önerilir.

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

    Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun.

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
