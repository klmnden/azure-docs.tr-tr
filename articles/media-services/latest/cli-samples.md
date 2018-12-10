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
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: 9df9279a3922fa639385657660d6d0f4a55b5a4d
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53132781"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure Media Services için Azure CLI örnekleri

Aşağıdaki tabloda, Azure Media Services için Azure CLI örnekleri bağlantılarını içerir.

## <a name="examples"></a>Örnekler

|  |  |
|---|---|
|**Hesap**||
| [Bir Media Services hesabı oluşturma](./scripts/cli-create-account.md) | Azure Media Services hesabı oluşturur. Ayrıca, bir hizmet asıl hesabı programlı olarak yönetmek için API'lerine erişmek için kullanılabilir oluşturur. |
| [Hesap kimlik bilgilerini Sıfırla](./scripts/cli-reset-account-credentials.md)|Hesap kimlik bilgilerinizi sıfırlar ve app.config ayarlarını geri alır.|
|**Varlıklar**||
| [Varlıkları oluşturma](./scripts/cli-create-asset.md)|İçeriği karşıya yüklemek için bir medya Hizmetleri varlığı oluşturur.|
| [Bir dosyayı karşıya yükleyin](./scripts/cli-upload-file-asset.md)|Yerel bir dosyaya, bir depolama kapsayıcısına yükler.|
| [Bir varlığı yayımlayın](./scripts/cli-publish-asset.md)| Bir akış Bulucu oluşturur ve akış URL'lerini geri alır. |
| **Dönüşümler** ve **işleri**||
| [Dönüşümler oluşturmak](./scripts/cli-create-transform.md)|Dönüşümler oluşturma işlemi gösterilmektedir. Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır).<br/> Zaten istenen ada ve “tarife” sahip bir Dönüşümün olup olmadığını mutlaka kontrol etmelisiniz. Aşması durumunda yeniden kullanın. |
| [İş oluşturma](./scripts/cli-create-jobs.md)|HTTPs URL'sini kullanarak basit bir kodlama dönüştürme için bir iş gönderir.|
| [EventGrid oluşturma](./scripts/cli-create-event-grid.md)|İş durumu değişiklikleri için hesap düzeyinde Event Grid aboneliği oluşturur.|

## <a name="see-also"></a>Ayrıca bkz.

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
