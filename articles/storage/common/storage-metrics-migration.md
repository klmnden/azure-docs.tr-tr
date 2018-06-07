---
title: Azure depolama ölçümlerini geçiş | Microsoft Docs
description: Eski ölçümleri Azure İzleyici tarafından yönetilen yeni ölçümleri geçiş yapmayı öğrenin.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: fhryo-msft
ms.assetid: ''
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 03/30/2018
ms.author: fryu
ms.openlocfilehash: c64061aee94e8c08a3f6bcae78cffca0b4172d97
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650681"
---
# <a name="azure-storage-metrics-migration"></a>Azure depolama ölçümlerini geçişi

Azure'da izleme deneyimi birleştirin stratejisini ile hizalanmış olarak Azure Storage ölçümleri Azure İzleyici platformu tümleştirir. Gelecekte, hizmet eski ölçümleri Azure ilkesini temel alarak erken bir bildirim ile sona erer. Eski depolama ölçümleri kullanırsanız, ölçüm bilgilerinizi korumak için hizmet bitiş tarihinden önce geçirmek gerekir.

Bu makalede yeni ölçümlere eski ölçümleri geçirmek nasıl gösterir.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Azure Depolama tarafından yönetilen eski ölçümleri anlama

Azure depolama eski ölçüm değerleri toplar ve toplar ve bunları aynı depolama hesabındaki $Metric tablolardaki depolar. İzleme grafikte ayarlamak için Azure portalını kullanabilirsiniz. Azure depolama SDK'ları, şemasını temel alan $Metric tablolardaki verileri okumak için de kullanabilirsiniz. Daha fazla bilgi için bkz: [depolama çözümlemeleri](./storage-analytics.md).

Eski ölçümleri yalnızca Azure Blob Depolama kapasite ölçümleri sağlar. Eski ölçümleri Blob storage, Table storage, Azure dosyaları ve kuyruk depolama üzerinde işlem ölçümlerini sağlar. 

Eski ölçümleri düz şemada tasarlanmıştır. Ölçüm tetikleme trafik düzenlerini olmadığında ölçüm sıfır değeri tasarım sonuçlanır. Örneğin, **ServerTimeoutError** değeri ayarlanmış 0 $Metric tablolardaki bile sunucu zaman aşımı hatalarını Canlı trafiği bir depolama hesabına almadığınız olduğunda.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Azure İzleyici tarafından yönetilen yeni ölçümleri anlama

Yeni depolama ölçümler Azure İzleyici arka uç ölçüm verileri Azure Storage yayar. Azure İzleyici Portalı'ndan verileri dahil olmak üzere bir birleşik bir izleme deneyimi sağlar veri alımı yanı sıra. Daha fazla ayrıntı için bu başvurabilirsiniz [makale](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Yeni ölçümleri kapasite ölçümleri ve işlem ölçümlerini Blob, tablo, dosya, kuyruk ve premium depolama sağlar.

Birden çok boyut Azure İzleyicisi'nin sağladığı özelliklerden biridir. Azure depolama yeni ölçüm şeması tanımlamakta tasarım devralır. Ölçümleri desteklenen boyutlarında için ayrıntılarda bulabilirsiniz [Azure Storage ölçümlerini Azure İzleyicisi'nde](./storage-metrics-in-azure-monitor.md). Birden çok boyut tasarım alım gelen iki bant genişliği ve kapasite ölçümleri depolama alanından maliyet verimliliği sağlar. Sonuç olarak, trafiğinizin ilgili ölçümleri tetiklenen değil, ilgili ölçüm verileri üretilmez. Trafiğinizi sunucu zaman aşımı hatalarını tetiklenen değil, ölçüm değerini sorguladığınızda Örneğin, Azure İzleyici herhangi bir veri döndürmez **işlemleri** boyutla **ResponseType** eşit **ServerTimeoutError**.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Eski Ölçümler ve yeni ölçümler arasında ölçümleri eşleme

Ölçüm verileri programlı olarak okuma, programlarınızı yeni ölçüm şemada benimsemeyi gerekir. Değişiklikleri daha iyi anlamak için aşağıdaki tabloda listelenen eşleme başvurabilirsiniz:

**Kapasite ölçümleri**

| Eski ölçümü | Yeni ölçümü |
| ------------------- | ----------------- |
| **Kapasite**            | **BlobCapacity** boyutuyla **BlobType** eşit **BlockBlob** veya **PageBlob** |
| **ObjectCount**        | **BLOB sayısı** boyutuyla **BlobType** eşit **BlockBlob** veya **PageBlob** |
| **ContainerCount**      | **ContainerCount** |

Aşağıdaki ölçümleri eski ölçümleri desteklemeyen yeni teklifleri şunlardır:
* **TableCapacity**
* **TableCount**
* **TableEntityCount**
* **QueueCapacity**
* **QueueCount**
* **QueueMessageCount**
* **FileCapacity**
* **FileCount**
* **FileShareCount**
* **UsedCapacity**

**İşlem ölçümlerini**

| Eski ölçümü | Yeni ölçümü |
| ------------------- | ----------------- |
| **AnonymousAuthorizationError** | Boyut hareketlerle **ResponseType** eşit **AuthorizationError** |
| **AnonymousClientOtherError** | Boyut hareketlerle **ResponseType** eşit **ClientOtherError** |
| **AnonymousClientTimeoutError** | Boyut hareketlerle **ResponseType** eşit **ClientTimeoutError** |
| **AnonymousNetworkError** | Boyut hareketlerle **ResponseType** eşit **NetworkError** |
| **AnonymousServerOtherError** | Boyut hareketlerle **ResponseType** eşit **ServerOtherError** |
| **AnonymousServerTimeoutError** | Boyut hareketlerle **ResponseType** eşit **ServerTimeoutError** |
| **AnonymousSuccess** | Boyut hareketlerle **ResponseType** eşit **başarılı** |
| **AnonymousThrottlingError** | Boyut hareketlerle **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError** |
| **AuthorizationError** | Boyut hareketlerle **ResponseType** eşit **AuthorizationError** |
| **Kullanılabilirlik** | **Kullanılabilirlik** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **ClientOtherError** | Boyut hareketlerle **ResponseType** eşit **ClientOtherError** |
| **ClientTimeoutError** | Boyut hareketlerle **ResponseType** eşit **ClientTimeoutError** |
| **NetworkError** | Boyut hareketlerle **ResponseType** eşit **NetworkError** |
| **PercentAuthorizationError** | Boyut hareketlerle **ResponseType** eşit **AuthorizationError** |
| **PercentClientOtherError** | Boyut hareketlerle **ResponseType** eşit **ClientOtherError** |
| **PercentNetworkError** | Boyut hareketlerle **ResponseType** eşit **NetworkError** |
| **PercentServerOtherError** | Boyut hareketlerle **ResponseType** eşit **ServerOtherError** |
| **PercentSuccess** | Boyut hareketlerle **ResponseType** eşit **başarılı** |
| **PercentThrottlingError** | Boyut hareketlerle **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError** |
| **PercentTimeoutError** | Boyut hareketlerle **ResponseType** eşit **ServerTimeoutError** veya **ResponseType** eşit **ClientTimeoutError** |
| **SASAuthorizationError** | Boyut hareketlerle **ResponseType** eşit **AuthorizationError** |
| **SASClientOtherError** | Boyut hareketlerle **ResponseType** eşit **ClientOtherError** |
| **SASClientTimeoutError** | Boyut hareketlerle **ResponseType** eşit **ClientTimeoutError** |
| **SASNetworkError** | Boyut hareketlerle **ResponseType** eşit **NetworkError** |
| **SASServerOtherError** | Boyut hareketlerle **ResponseType** eşit **ServerOtherError** |
| **SASServerTimeoutError** | Boyut hareketlerle **ResponseType** eşit **ServerTimeoutError** |
| **SASSuccess** | Boyut hareketlerle **ResponseType** eşit **başarılı** |
| **SASThrottlingError** | Boyut hareketlerle **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError** |
| **ServerOtherError** | Boyut hareketlerle **ResponseType** eşit **ServerOtherError** |
| **ServerTimeoutError** | Boyut hareketlerle **ResponseType** eşit **ServerTimeoutError** |
| **Başarılı** | Boyut hareketlerle **ResponseType** eşit **başarılı** |
| **ThrottlingError** | **İşlemler** boyutuyla **ResponseType** eşit **ClientThrottlingError** veya **ServerBusyError**|
| **TotalBillableRequests** | **İşlemler** |
| **TotalEgress** | **Çıkış** |
| **Totalıngress** | **Giriş** |
| **TotalRequests** | **İşlemler** |

## <a name="faq"></a>SSS

### <a name="how-should-i-migrate-existing-alert-rules"></a>Var olan uyarı kuralları nasıl geçirmeniz gerekir?

Eski depolama ölçümleri temel Klasik uyarı kuralları oluşturduysanız, kurallar yeni ölçüm şemasını temel alan yeni uyarı oluşturmanız gerekir.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>Yeni ölçüm verileri varsayılan olarak aynı depolama hesabında depolanır?

Hayır. Bir depolama hesabına ölçüm verileri arşivlemek için kullanmanız [Azure İzleyici tanılama ayarını API](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings/createorupdate).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Depolama ölçümleri Azure İzleyicisi](./storage-metrics-in-azure-monitor.md)
