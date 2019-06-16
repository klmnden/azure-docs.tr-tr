---
title: Azure içeri/dışarı aktarma hizmeti REST API'sini kullanma | Microsoft Docs
description: Azure içeri/dışarı aktarma hizmeti REST API'si, nasıl yapılır hem başvuru malzemesi dahil olmak üzere kullanmak için kaynakları bulmak nereye öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 1e8b60f37cefb81fbbbbb7823be7752dd1188dc3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60320287"
---
# <a name="using-the-azure-importexport-service-rest-api"></a>Azure İçeri/Dışarı Aktarma hizmeti REST API’sini kullanma

Microsoft Azure içeri/dışarı aktarma hizmeti içeri/dışarı aktarma işleri programlı denetimini etkinleştirmek için bir REST API sunar. REST API tüm ile gerçekleştirebileceğiniz içeri/dışarı aktarma işlemlerini gerçekleştirmek için kullanabileceğiniz [Azure portalında](https://portal.azure.com/). Ayrıca, Azure portalında şu anda kullanılabilir değil bir işin tamamlanma sorgulama gibi ayrıntılı belirli işlemlerin gerçekleştirilmesi için REST API de kullanabilirsiniz.

Bkz: [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmetini kullanarak](../storage-import-export-service.md) genel bakış içeri/dışarı aktarma hizmeti ve portalı oluşturup yönetmek içeri aktarma ve dışarı aktarma işleri için nasıl kullanılacağını gösteren öğretici.

## <a name="service-endpoints"></a>Hizmet uç noktaları

Azure içeri/dışarı aktarma hizmeti, bir kaynak sağlayıcısı için Azure Resource Manager ve içeri/dışarı aktarma işleri yönetmek için bir dizi REST API'si aşağıdaki HTTPS uç noktasında sağlar:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Sürüm oluşturma

İçeri/dışarı aktarma hizmeti istekleri belirtmelisiniz `api-version` parametresi ve değeri ayarlamak `2016-11-01`.

## <a name="importexport-service-operations"></a>İçeri/dışarı aktarma hizmeti işlemleri

[İçeri aktarma işi oluşturma](../storage-import-export-creating-an-import-job.md)

[Dışarı aktarma işi oluşturma](../storage-import-export-creating-an-export-job.md)

[Bir iş için durum bilgilerini alma](storage-import-export-retrieving-state-info-for-a-job.md)

[İşleri numaralandırma](../storage-import-export-enumerating-jobs.md)

[İşleri iptal etme ve silme](storage-import-export-cancelling-and-deleting-jobs.md)

[Sürücü bildirimlerini yedekleme](../storage-import-export-backing-up-drive-manifests.md)

[İçeri/Dışarı Aktarma işleri için tanılama ve hata kurtarma](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama içeri/dışarı aktarma REST](/rest/api/storageimportexport)
