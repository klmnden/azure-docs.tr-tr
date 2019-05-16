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
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 5dbcf446a609adcd0f1902fcca2ac19ad87f17b1
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65779664"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Azure CLI ile API erişim Azure medya Hizmetleri
 
Azure Media Services API'sine bağlanmak için Azure AD hizmet sorumlusu kimlik doğrulaması kullanmak için aşağıdaki parametrelere sahip bir Azure AD belirteç istemek uygulamanız gerekir:

* Azure AD Kiracı uç noktası
* Media Services kaynak URI'si
* Kaynak URI REST Media Services için
* Azure AD uygulama değerlerini: istemci Kimliğini ve istemci gizli anahtarı

Ayrıntılı bir açıklama için bkz. [erişme Media Services v3 API'ler](media-services-apis-overview.md#accessing-the-azure-media-services-api).

Bu makalede bir Azure AD uygulaması ve hizmet sorumlusu oluşturma ve Azure Media Services kaynaklarına erişmek için gerekli değerleri almak için Azure CLI'yı kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar 

[Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun.
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Ölçek medya ayrılmış birimleri - CLI](media-reserved-units-cli-how-to.md)
- [Bir Media Services hesabı oluşturma - CLI](create-account-cli-how-to.md) 
- [Sıfırlama hesabı kimlik bilgileri - CLI](cli-reset-account-credentials.md)
- [Varlıklar - CLI oluşturma](cli-create-asset.md)
- [Karşıya dosya - CLI](cli-upload-file-asset.md)
- [Oluşturma dönüşümleri - CLI](cli-create-transform.md)
- [-CLI gibi özel bir dönüşüm ile kodlayın](custom-preset-cli-howto.md)
- [Oluşturma işleri - CLI](cli-create-jobs.md)
- [CLI EventGrid - oluşturma](job-state-events-cli-how-to.md)
- [Bir varlığı - CLI yayımlayın](cli-publish-asset.md)
- [Filtre - CLI](filters-dynamic-manifest-cli-howto.md)
- [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>Sonraki adımlar

İçerik akışı yapmak istediğiniz akış uç çalışıyor durumda olması gerekir. Aşağıdaki CLI komutunu varsayılan akış uç noktasını başlatır:

`az ams streaming-endpoint start -n default -a <amsaccount> -g <amsResourceGroup>`

