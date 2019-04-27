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
ms.date: 03/11/2019
ms.author: juliako
ms.openlocfilehash: dee7f831562dc1f4b2478d13b204aab1d8455e1e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733190"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure Media Services için Azure CLI örnekleri

Aşağıdaki tabloda, Azure Media Services için Azure CLI örnekleri bağlantılarını içerir.

## <a name="examples"></a>Örnekler

|  |  |
|---|---|
|**Ölçeklendirme**||
| [Ölçek medya ayrılmış birimleri](media-reserved-units-cli-how-to.md)|Media Services v3 veya Video Indexer ile tetiklenen Ses Analizi ve Video Analizi İşleri için hesabınıza 10 S3 MRU sağlamanız önerilir. <br/>Komut, medya ayrılmış birimi (MRU) ölçeklendirmek için CLI kullanmayı gösterir.|
|**Hesap**||
| [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md) | Betik bir Azure Media Services hesabı oluşturur. |
| [Hesap kimlik bilgilerini Sıfırla](./scripts/cli-reset-account-credentials.md)|Hesap kimlik bilgilerinizi sıfırlar ve app.config ayarlarını geri alır.|
|**Varlıklar**||
| [Varlıkları oluşturma](./scripts/cli-create-asset.md)|İçeriği karşıya yüklemek için bir medya Hizmetleri varlığı oluşturur.|
| [Bir dosyayı karşıya yükleyin](./scripts/cli-upload-file-asset.md)|Yerel bir dosyaya, bir depolama kapsayıcısına yükler.|
| **Dönüşümler** ve **işleri**||
| [Dönüşümler oluşturmak](./scripts/cli-create-transform.md)|Dönüşümler oluşturma işlemi gösterilmektedir. Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır).<br/> Her zaman bir dönüştürme, istediğiniz bir adla denetlemeniz gerekir ve "yemek" zaten mevcut. Aşması durumunda yeniden kullanın. |
| [Özel bir dönüşüm ile kodlayın](custom-preset-cli-howto.md) | Senaryonuz ya da cihaz belirli gereksinimlerinizi hedeflemek için önceden belirlenmiş bir özel yapı işlemi gösterilmektedir.|
| [İş oluşturma](./scripts/cli-create-jobs.md)|HTTPs URL'sini kullanarak basit bir kodlama dönüştürme için bir iş gönderir.|
| [EventGrid oluşturma](./scripts/cli-create-event-grid.md)|İş durumu değişiklikleri için hesap düzeyinde Event Grid aboneliği oluşturur.|
| **Teslim edin**||
| [Bir varlığı yayımlayın](./scripts/cli-publish-asset.md)| Bir akış Bulucu oluşturur ve akış URL'lerini geri alır. |
| [Filtre](filters-dynamic-manifest-cli-howto.md)| Video isteğe bağlı varlık için bir filtre yapılandırır ve oluşturmak için CLI kullanma işlemini gösterir [hesap filtreleri](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) ve [varlık filtreleri](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="see-also"></a>Ayrıca bkz.

- [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
- [Hızlı Başlangıç: Video dosyalarını akışla aktarma - CLI](stream-files-cli-quickstart.md)
