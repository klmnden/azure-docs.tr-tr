---
title: Azure CLI örnekleri - Azure Media Services | Microsoft Docs
description: Azure Media Services hizmeti için Azure CLI örnekleri
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 02/21/2019
ms.author: juliako
ms.openlocfilehash: bbd57933993e22dd32b84f1d44175bb3b3d749c9
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672435"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure Media Services için Azure CLI örnekleri

Aşağıdaki tabloda, Azure Media Services için Azure CLI örnekleri bağlantılarını içerir.

## <a name="examples"></a>Örnekler

|  |  |
|---|---|
|**Ölçeklendirme**||
| [Ölçek medya ayrılmış birimleri](media-reserved-units-cli-how-to.md)|Ses analizi ve Video analizi işleri, Media Services v3 tarafından tetiklenen veya Video Indexer için 10 S3 MRU hesabınızla sağlama önemle tavsiye edilir. <br/>Komut, medya ayrılmış birimi (MRU) ölçeklendirmek için CLI kullanmayı gösterir.|
|**Hesap**||
| [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md) | Azure Media Services hesabı oluşturur. Ayrıca, bir hizmet asıl hesabı programlı olarak yönetmek için API'lerine erişmek için kullanılabilir oluşturur. |
| [Hesap kimlik bilgilerini Sıfırla](./scripts/cli-reset-account-credentials.md)|Hesap kimlik bilgilerinizi sıfırlar ve app.config ayarlarını geri alır.|
|**Varlıklar**||
| [Varlıkları oluşturma](./scripts/cli-create-asset.md)|İçeriği karşıya yüklemek için bir medya Hizmetleri varlığı oluşturur.|
| [Bir dosyayı karşıya yükleyin](./scripts/cli-upload-file-asset.md)|Yerel bir dosyaya, bir depolama kapsayıcısına yükler.|
| **Dönüşümler** ve **işleri**||
| [Dönüşümler oluşturmak](./scripts/cli-create-transform.md)|Dönüşümler oluşturma işlemi gösterilmektedir. Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır).<br/> Zaten istenen ada ve “tarife” sahip bir Dönüşümün olup olmadığını mutlaka kontrol etmelisiniz. Aşması durumunda yeniden kullanın. |
| [İş oluşturma](./scripts/cli-create-jobs.md)|HTTPs URL'sini kullanarak basit bir kodlama dönüştürme için bir iş gönderir.|
| [EventGrid oluşturma](./scripts/cli-create-event-grid.md)|İş durumu değişiklikleri için hesap düzeyinde Event Grid aboneliği oluşturur.|
| **Teslim edin**||
| [Bir varlığı yayımlayın](./scripts/cli-publish-asset.md)| Bir akış Bulucu oluşturur ve akış URL'lerini geri alır. |
| [Filtre](filters-dynamic-manifest-cli-howto.md)| Video isteğe bağlı varlık için bir filtre yapılandırır ve oluşturmak için CLI kullanma işlemini gösterir [hesap filtreleri](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) ve [varlık filtreleri](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="see-also"></a>Ayrıca bkz.

- [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
- [Hızlı Başlangıç: Stream video dosyaları - CLI](stream-files-cli-quickstart.md)
