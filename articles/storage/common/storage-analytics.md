---
title: Azure depolama analizi günlükleri ve ölçüm verilerini toplamak için kullan | Microsoft Docs
description: Depolama analizi, tüm depolama hizmetleri için ölçüm verileri izlemek amacıyla ve Blob, kuyruk ve tablo depolama için günlükleri toplamak için sağlar.
services: storage
author: normesta
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: b588ebe577e61014c6c2bbeaae751b2783dd6f80
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65153916"
---
# <a name="storage-analytics"></a>Depolama Analizi

Azure Depolama Analizi günlük kaydı yapar ve depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz.

Depolama analizi kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmeniz gerekir. Buradan etkinleştirebilirsiniz [Azure portalında](https://portal.azure.com). Ayrıntılar için bkz [Azure portalında depolama hesabı izleme](storage-monitor-storage-account.md). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Kullanım [Blob hizmeti özelliklerini ayarla](/rest/api/storageservices/set-blob-service-properties), [kuyruk hizmeti özelliklerini ayarla](/rest/api/storageservices/set-queue-service-properties), [tablo hizmeti özelliklerini ayarla](/rest/api/storageservices/set-table-service-properties), ve [ayarlamak dosya hizmeti özelliklerini](/rest/api/storageservices/Get-File-Service-Properties)her hizmet için depolama analizi etkinleştirme işlemleri.

Toplanan veriler (günlük için) iyi bilinen bir blob ve tablo hizmeti API'leri ve Blob hizmeti kullanılarak erişilebilecek (ölçüm) için iyi bilinen tablolara depolanır.

Depolama analizi, depolama hesabınız için toplam limiti bağımsızdır depolanan veri miktarına 20 TB limiti vardır. Depolama hesabı sınırları hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage-scalability-targets.md).

Kapsamlı bir kılavuz tanımlamak, tanılamak ve Azure depolama ile ilgili sorunları gidermek için depolama analizi ve diğer araçları kullanma hakkında bilgi için bkz: [izleme, tanılama ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="billing-for-storage-analytics"></a>Depolama analizi faturalandırması

Tüm ölçüm verileri bir depolama hesabının hizmetler tarafından yazılır. Sonuç olarak, depolama analizi tarafından gerçekleştirilen her yazma işlemi Faturalandırılabilir. Ayrıca, ölçüm verilerini tarafından kullanılan depolama miktarı da Faturalandırılabilir.

Depolama analizi tarafından gerçekleştirilen aşağıdaki eylemler Faturalanabilir şunlardır:

* Günlüğe kaydetme için BLOB'ları oluşturmak için istek sayısı.
* Ölçümler için tablo varlıkları oluşturmak için istek sayısı.

Veri bekletme ilkesi yapılandırdıysanız, depolama analizi, eski günlük ve ölçüm verileri sildiğinde silme işlemler için ücretlendirilmez. Ancak, bir istemciden silme işlemleri Faturalanabilir niteliktedir. Bekletme ilkeleri hakkında daha fazla bilgi için bkz: [depolama Analytics veri saklama ilkesini belirlemeden](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Faturalandırılabilir isteklerin anlama

Bir hesabın depolama hizmetine yapılan her isteği Faturalanabilir ya da Faturalanamayan değil. Depolama analizi yapılan bir hizmete istek nasıl işlendiğini belirten bir durum iletisi de dahil olmak üzere, tek tek her isteği günlüğe kaydeder. Benzer şekilde, Storage Analytics ölçümleri hem hizmet hem de hizmet yüzdeleri ve belirli durum iletilerinin sayısı dahil olmak üzere, API işlemleri için depolar. Birlikte, bu özellikler, Faturalanabilir isteklerinizi çözümleme, uygulamanız üzerinde geliştirmeler yapmak ve hizmetlerinizi istekleri ile ilgili sorunları tanılamanıza yardımcı olabilir. Faturalama hakkında daha fazla bilgi için bkz. [anlama Azure depolama Faturalaması - bant genişliği, işlemler ve kapasite](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Depolama analizi verilere baktığımızda, tablolardaki kullanabileceğiniz [depolama analizi günlüğe yazılan işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) hangi istekleri Faturalanabilir belirlemek için. Ardından, günlükleri ve ölçüm verileri belirli bir istek için ücret olmadığını görmek için durum iletilerine karşılaştırabilirsiniz. Depolama hizmeti veya tek tek API işlemi için kullanılabilirlik araştırmak için önceki konu başlığında tabloları da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure portalında depolama hesabı izleme](storage-monitor-storage-account.md)
* [Storage Analytics ölçümleri](storage-analytics-metrics.md)
* [Depolama analizi günlük kaydı](storage-analytics-logging.md)
