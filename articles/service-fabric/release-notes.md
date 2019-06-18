---
title: Azure Service Fabric sürümü
description: En son özellikler ve Service Fabric geliştirmeler için sürüm notları.
author: athinanthny
manager: chackdan
ms.author: atsenthi
ms.date: 6/10/2019
ms.topic: conceptual
ms.service: service-fabric
hide_comments: true
hideEdit: true
ms.openlocfilehash: 5610c6d31732144086812bb02b65cfaffa067eae
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063004"
---
# <a name="service-fabric-releases"></a>Service Fabric güncelleştirmeleri

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Sorun giderme kılavuzlarının</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">sorun izlemeyi</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">destek seçenekleri</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">desteklenen sürümleri</a>  
|  <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Kod örnekleri</a>

Bu makalede, en son sürümlerine ve Service Fabric çalışma zamanını ve SDK'ları için güncelleştirmeler daha fazla bilgi sağlar.

## <a name="whats-new-in-service-fabric"></a>**Service Fabric'te yenilikler nelerdir?**

### <a name="service-fabric-65"></a>Service Fabric 6.5

Son Service Fabric sürümüne desteklenebilirlik, güvenilirlik ve performans geliştirmeleri, yeni özellikler, hata düzeltmeleri ve küme ve uygulama yaşam döngüsü yönetimini kolaylaştırmak için geliştirmeler içerir.

> [!IMPORTANT]
> Service Fabric 6.5, Service Fabric araçları, Visual Studio 2015'te desteği ile son sürümdür. Müşterilerin taşımak için önerilir [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) ileride.

Service Fabric 6.5 yenilikler aşağıda verilmiştir:

- Service Fabric Explorer'ı içeren bir [resim görüntüleyici Store](service-fabric-visualizing-your-cluster.md#image-store-viewer) uygulamaları İnceleme için görüntü deposuna yüklediğiniz.

- [Düzeltme eki düzenleme uygulaması (POA)](service-fabric-patch-orchestration-application.md) sürüm [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) self-diagnostic geliştirmeleri içerir. POA müşterileri bu sürümüne taşımak için önerilir.

- [Eventstore'a hizmet, varsayılan olarak etkindir](service-fabric-visualizing-your-cluster.md#event-store) vazgeçti değilse, Service Fabric 6.5 kümeleri için.

- Eklenen [çoğaltma yaşam döngüsü olaylarını](service-fabric-diagnostics-event-generation-operational.md#replica-events) durum bilgisi olan hizmetler için.

- [Daha iyi bir çekirdek değer düğümü durumu görünürlüğünü](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status), bir çekirdek değer düğümü sağlıksız ise küme düzeyinde uyarılar dahil olmak üzere (*aşağı*, *kaldırıldı* veya *bilinmeyen*).

- [Service Fabric uygulama olağanüstü durum kurtarma aracı](https://github.com/Microsoft/Service-Fabric-AppDRTool) hızlı bir şekilde birincil küme karşılaştığında bir olağanüstü durum kurtarma Service Fabric durum bilgisi olan hizmetler sağlar. Birincil küme verilerini düzenli yedekleme ve geri yükleme kullanarak ikincil bekleme uygulama üzerinde sürekli olarak eşitlenir.

- Visual Studio desteği [Linux tabanlı kümeler için .NET Core uygulamaları yayımlama](service-fabric-how-to-publish-linux-app-vs.md).

- [Azure Service Fabric CLI (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) Service Fabric 6.5 (ve sonraki sürümler) otomatik olarak yüklenecek yükseltirken veya Azure'da yeni bir Linux kümesi oluşturma.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) MacOS/Linux OneBox kümelerde varsayılan olarak yüklenir.

Daha fazla ayrıntı için bkz. [Service Fabric 6.5 sürüm notları](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf).

## <a name="previous-versions"></a>Önceki sürümler

### <a name="service-fabric-64-releases"></a>Service Fabric 6.4 yayınlar

| Sürüm tarihi | Yayınla | Daha fazla bilgi |
|---|---|---|
| 30 Kasım 2018'e | [Azure Service Fabric'e 6.4 ](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12 Aralık 2018'e | [Azure Service Fabric 6.4 Yenile yayın için Windows kümeleri](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4 Şubat 2019 | [Azure Service Fabric 6.4 yenileme sürüm](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4 Mart 2019 | [Azure Service Fabric 6.4 yenileme sürüm](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8 Nisan 2019 | [Azure Service Fabric 6.4 yenileme sürüm](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2 Mayıs 2019 | [Azure Service Fabric 6.4 yenileme sürüm](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28 Mayıs 2019 | [Azure Service Fabric 6.4 yenileme sürüm](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
