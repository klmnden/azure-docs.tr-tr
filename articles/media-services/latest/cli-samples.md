---
title: Azure CLI örnekler - Azure Media Services | Microsoft Docs
description: Azure Media Services hizmeti için Azure CLI örnekleri
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 04/15/2018
ms.author: juliako
ms.openlocfilehash: bbf69bdcc92316642f6b37d267cdea2aad920316
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure Media Services için Azure CLI örnekleri

Aşağıdaki tabloda, Azure Media Services için Azure CLI örnekler bağlantılarını içerir.

|  |  |
|---|---|
|**Hesap**||
| [Bir Media Services hesabı oluşturma](./scripts/cli-create-account.md) | Azure Media Services hesabı oluşturur. Ayrıca, bir hizmet asıl hesabı programlı olarak yönetmek için API'ler erişmek için kullanılabilir oluşturur. |
| [Hesap kimlik bilgilerini sıfırlama](./scripts/cli-reset-account-credentials.md)|Hesap kimlik bilgilerinizi sıfırlar ve app.config ayarlarını geri alır.|
|**Varlıklar**||
| [Varlıklar oluşturma](./scripts/cli-create-asset.md)|İçeriği yüklemek için bir medya Hizmetleri varlığı oluşturur.|
| [Bir dosyayı karşıya yüklemek](./scripts/cli-upload-file-asset.md)|Yerel bir dosya için bir depolama kapsayıcısı yükler.|
| [Bir varlığı yayımlayın](./scripts/cli-publish-asset.md)| Akış Bulucu oluşturur ve akış URL'lerini geri alır. |
| **Dönüşümleri** ve **işleri**||
| [Dönüşümler oluşturma](./scripts/cli-create-transform.md)|Dönüşümler oluşturulacağını gösterir. Dönüşümler (genellikle "tarif" adlandırılır), video ve ses dosyalarını işlemek için görevlerin basit bir iş akışını açıklar.<br/> Her zaman bir dönüşüm, istenen adda denetlemelisiniz ve "tarif" zaten mevcut. Bulursa, onu yeniden kullanabilirsiniz. |
| [İşleri oluşturma](./scripts/cli-create-jobs.md)|HTTPs URL'sini kullanarak basit bir kodlama dönüştürme için bir iş gönderir.|
| [EventGrid oluşturma](./scripts/cli-create-event-grid.md)|Bir hesap düzeyi olay kılavuz abonelik için iş durumu değişiklikleri oluşturur.|

