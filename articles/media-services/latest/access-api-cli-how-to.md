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
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 8374e4c49012a2c49de41001be0fdb30f9151332
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617844"
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
- [Bir Media Services hesabı oluşturma - CLI](./scripts/cli-create-account.md) 
- [Sıfırlama hesabı kimlik bilgileri - CLI](./scripts/cli-reset-account-credentials.md)
- [Varlıklar - CLI oluşturma](./scripts/cli-create-asset.md)
- [Karşıya dosya - CLI](./scripts/cli-upload-file-asset.md)
- [Oluşturma dönüşümleri - CLI](./scripts/cli-create-transform.md)
- [Oluşturma işleri - CLI](./scripts/cli-create-jobs.md)
- [CLI EventGrid - oluşturma](./scripts/cli-create-event-grid.md)
- [Bir varlığı - CLI yayımlayın](./scripts/cli-publish-asset.md)
- [Filtre - CLI](filters-dynamic-manifest-cli-howto.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
