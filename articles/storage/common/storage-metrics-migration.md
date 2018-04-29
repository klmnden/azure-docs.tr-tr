---
title: Azure depolama ölçümlerini geçiş | Microsoft Docs
description: Eski ölçümleri Azure İzleyici tarafından yönetilen yeni ölçümleri geçirme hakkında bilgi edinin.
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
ms.openlocfilehash: c4dc9b231668315af16c625314c737fee99d672d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-storage-metrics-migration"></a>Azure depolama ölçümlerini geçişi

Azure Birleştirici İzleyici deneyimi stratejisi ile hizalı, Azure depolama üzerinde-Azure İzleyici platformu ölçümleri panoları. Gelecekte, hizmet eski ölçümleri Azure ilkesini temel alarak erken uyarı ile sonlandırılır. Eski depolama ölçümleri kullanırsanız, ölçüm bilgilerinizi korumak için hizmet bitiş tarihinden önce geçirmek gerekir.

Bu makalede, eski ölçümleri yeni ölçümler geçirme size yol gösterir.

## <a name="understand-legacy-metrics-managed-by-azure-storage"></a>Azure Depolama tarafından yönetilen eski ölçümleri anlama

Azure depolama eski ölçüm değerleri, toplamalar, toplar ve bunları aynı depolama hesabındaki $Metric tablolardaki depolar. Grafik izleme ayarlamak için Azure portalını kullanabilirsiniz ve Azure depolama SDK'ları şemasını temel alan $Metric tablolardaki verileri okumak için de kullanabilirsiniz. Daha fazla ayrıntı için bu başvurabilirsiniz [depolama çözümlemeleri](./storage-analytics.md).

Eski ölçümleri Blob, tablo, dosya ve kuyruk Blob kapasite ölçümleri yalnızca ve işlem ölçümlerini sağlar.

Eski ölçümleri düz şemada tasarlanmıştır. Ölçüm tetikleme trafik düzenlerini olmadığında ölçüm sıfır değeri tasarım sonuçlanır. Örneğin, **ServerTimeoutError** bile sunucu zaman aşımı hatalarını Canlı trafiği depolama hesabına almadığınız 0 $Metric tablolar olarak ayarlanır.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Azure İzleyici tarafından yönetilen yeni ölçümleri anlama

Yeni depolama ölçümler Azure İzleyici arka uç ölçüm verileri Azure Storage yayar. Azure İzleyici Portalı'ndan verileri dahil olmak üzere bir birleşik bir izleme deneyimi sağlar veri alımı yanı sıra. Daha fazla ayrıntı için bu başvurabilirsiniz [makale](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Yeni ölçümleri kapasite ölçümleri ve işlem ölçümlerini Blob, tablo, dosya, kuyruk ve premium depolama sağlar.

Birden çok boyut Azure İzleyici tarafından sağlanan özelliklerden biridir. Azure depolama yeni ölçüm şeması tanımlamakta tasarım devralır. Ölçümleri desteklenen boyutlarında için Ayrıntılar bu konuda bulabilirsiniz [Azure Storage ölçümlerini Azure İzleyicisi'nde](./storage-metrics-in-azure-monitor.md). Birden çok boyut tasarım alım gelen iki bant genişliği ve kapasite ölçümleri depolama alanından maliyet verimliliği sağlar. Sonuç olarak, trafiğinizin ilgili ölçümleri tetiklenen değil, ilgili ölçüm verileri üretilmez. Trafiğinizi sunucu zaman aşımı hatalarını tetiklenen değil, ölçüm değerini sorguladığınızda Örneğin, Azure monitör hiçbir veri döndürür **işlemleri** boyutla **ResponseType** eşit**ServerTimeoutError**.

## <a name="metrics-mapping-between-legacy-metrics-and-new-metrics"></a>Eski Ölçümler ve yeni ölçümler arasında ölçümleri eşleme

Ölçüm verileri programlı olarak okuma, programlarınızı yeni ölçüm şemada benimsemeyi gerekir. Değişiklikleri daha iyi anlamak için başvurabilirsiniz eşleme aşağıdaki tabloda listelenen:

**Kapasite ölçümleri**

| Eski ölçümü | Yeni ölçümü |
| ------------------- | ----------------- |
| Kapasite            | BlobCapacity BlobType BlockBlob veya PageBlob eşit boyutla |
| ObjectCount         | BlobType BlockBlob veya PageBlob eşit boyutla BLOB sayısı |
| ContainerCount      | ContainerCount |

Aşağıdaki ölçümleri eski ölçümleri desteklemeyen yeni teklifleri şunlardır:
* TableCapacity
* TableCount
* TableEntityCount
* QueueCapacity
* QueueCount
* QueueMessageCount
* FileCapacity
* FileCount
* FileShareCount
* UsedCapacity

**İşlem ölçümlerini**

| Eski ölçümü | Yeni ölçümü |
| ------------------- | ----------------- |
| AnonymousAuthorizationError | Boyut için AuthorizationError eşit ResponseType işlemleri |
| AnonymousClientOtherError | Boyut için ClientOtherError eşit ResponseType işlemleri |
| AnonymousClientTimeoutError | Boyut için ClientTimeoutError eşit ResponseType işlemleri |
| AnonymousNetworkError | Boyut için NetworkError eşit ResponseType işlemleri |
| AnonymousServerOtherError | Boyut için ServerOtherError eşit ResponseType işlemleri |
| AnonymousServerTimeoutError | Boyut için ServerTimeoutError eşit ResponseType işlemleri |
| AnonymousSuccess | Boyut ResponseType başarılı durumuna eşit işlemleri |
| AnonymousThrottlingError | Boyut ResponseType ClientThrottlingError veya ServerBusyError eşit işlemleri |
| AuthorizationError | Boyut için AuthorizationError eşit ResponseType işlemleri |
| Kullanılabilirlik | Kullanılabilirlik |
| AverageE2ELatency | SuccessE2ELatency |
| AverageServerLatency | SuccessServerLatency |
| ClientOtherError | Boyut için ClientOtherError eşit ResponseType işlemleri |
| ClientTimeoutError | Boyut için ClientTimeoutError eşit ResponseType işlemleri |
| NetworkError | Boyut için NetworkError eşit ResponseType işlemleri |
| PercentAuthorizationError | Boyut için AuthorizationError eşit ResponseType işlemleri |
| PercentClientOtherError | Boyut için ClientOtherError eşit ResponseType işlemleri |
| PercentNetworkError | Boyut için NetworkError eşit ResponseType işlemleri |
| PercentServerOtherError | Boyut için ServerOtherError eşit ResponseType işlemleri |
| PercentSuccess | Boyut ResponseType başarılı durumuna eşit işlemleri |
| PercentThrottlingError | Boyut ResponseType ClientThrottlingError veya ServerBusyError eşit işlemleri |
| PercentTimeoutError | Boyut ResponseType eşit ServerTimeoutError veya ResponseType hareketlerle ClientTimeoutError için eşit|
| SASAuthorizationError | Boyut için AuthorizationError eşit ResponseType işlemleri |
| SASClientOtherError | Boyut için ClientOtherError eşit ResponseType işlemleri |
| SASClientTimeoutError | Boyut için ClientTimeoutError eşit ResponseType işlemleri |
| SASNetworkError | Boyut için NetworkError eşit ResponseType işlemleri |
| SASServerOtherError | Boyut için ServerOtherError eşit ResponseType işlemleri |
| SASServerTimeoutError | Boyut için ServerTimeoutError eşit ResponseType işlemleri |
| SASSuccess | Boyut ResponseType başarılı durumuna eşit işlemleri |
| SASThrottlingError | Boyut ResponseType ClientThrottlingError veya ServerBusyError eşit işlemleri |
| ServerOtherError | Boyut için ServerOtherError eşit ResponseType işlemleri |
| ServerTimeoutError | Boyut için ServerTimeoutError eşit ResponseType işlemleri |
| Başarılı | Boyut ResponseType başarılı durumuna eşit işlemleri |
| ThrottlingError | Boyut ResponseType ClientThrottlingError veya ServerBusyError eşit işlemleri |
| TotalBillableRequests | İşlemler |
| TotalEgress | Çıkış |
| Totalıngress | Giriş |
| TotalRequests | İşlemler |

## <a name="faq"></a>SSS

* Var olan uyarı kuralları nasıl geçirmeniz gerekir?

A: Klasik uyarı kuralları eski depolama ölçümleri temel oluşturduysanız, kurallar yeni ölçüm şemasını temel alan yeni uyarı oluşturmanız gerekir.

* Varsayılan olarak aynı depolama hesabında depolanan yeni ölçüm verileri mı?

Y: No Depolama hesabı ölçüm verileri arşivlemek gerekiyorsa, kullanabileceğiniz [tanılama ayarında Azure İzleyicisi](https://azure.microsoft.com/blog/azure-monitor-multiple-diagnostic-settings/)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Depolama ölçümleri Azure İzleyicisi](./storage-metrics-in-azure-monitor.md)
