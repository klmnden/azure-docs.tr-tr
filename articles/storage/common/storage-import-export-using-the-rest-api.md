---
title: "Azure içeri/dışarı aktarma hizmeti REST API kullanarak | Microsoft Docs"
description: "Azure içeri/dışarı aktarma hizmeti REST API'si, nasıl yapılır ve başvuru malzeme dahil olmak üzere kullanmak için kaynakların nerede bulacağını öğrenin."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 233f80e9-2e7f-48e0-9639-5c7785e7d743
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: b780385ad0af34bcb15639683d1aa5d689b38b50
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-importexport-service-rest-api"></a>Azure İçeri/Dışarı Aktarma hizmeti REST API’sini kullanma

Microsoft Azure içeri/dışarı aktarma hizmetini içeri/dışarı aktarma işleri programsal denetimi etkinleştirmek için bir REST API gösterir. REST API tüm ile gerçekleştirebileceğiniz içeri/dışarı aktarma işlemlerini gerçekleştirmek için kullanabileceğiniz [Azure portal](https://portal.azure.com/). Ayrıca, REST API Klasik Azure portalında şu anda kullanılabilir değil bir işin tamamlanma sorgulama gibi belirli ayrıntılı işlemler gerçekleştirmek için kullanabilirsiniz.

Bkz: [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanılarak](../storage-import-export-service.md) içeri/dışarı aktarma hizmeti ve klasik Portalı'nı oluşturmak ve içeri aktarma yönetmek ve işleri dışarı aktarmak için nasıl kullanılacağını gösteren bir eğitim genel bakış.

## <a name="service-endpoints"></a>Hizmet uç noktaları

Azure içeri/dışarı aktarma hizmeti kaynak sağlayıcısı için Azure Resource Manager ve içeri/dışarı aktarma işleri yönetmek için bir REST API kümesi aşağıdaki HTTPS uç noktada sağlar:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Sürüm oluşturma

İçeri/dışarı aktarma hizmeti isteklerine belirtmelisiniz `api-version` parametresi ve değeri ayarlayın `2016-11-01`.

## <a name="importexport-service-operations"></a>İçeri/dışarı aktarma hizmeti işlemleri

[İçeri aktarma işi oluşturma](../storage-import-export-creating-an-import-job.md)

[Dışarı aktarma işi oluşturma](../storage-import-export-creating-an-export-job.md)

[Bir iş için durum bilgilerini alma](storage-import-export-retrieving-state-info-for-a-job.md)

[İşleri numaralandırma](../storage-import-export-enumerating-jobs.md)

[İşleri iptal etme ve silme](storage-import-export-cancelling-and-deleting-jobs.md)

[Yedekleme sürücü bildirimleri](../storage-import-export-backing-up-drive-manifests.md)

[İçeri/Dışarı Aktarma işleri için tanılama ve hata kurtarma](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama içeri/dışarı aktarma REST](/rest/api/storageimportexport)
