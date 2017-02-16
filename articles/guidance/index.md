---
title: "Azure Kılavuzu | Microsoft Docs"
description: "Azure için en iyi uygulamalar ve kılavuz"
services: 
documentationcenter: na
author: bennage
manager: marksou
editor: 
tags: 
ms.assetid: de94c74a-fea7-4815-8484-553e421a7490
ms.service: guidance
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: christb
translationtype: Human Translation
ms.sourcegitcommit: 5f3ced657cf3d6587a63789b3dd3ca41cd2856f0
ms.openlocfilehash: 0061e1ff2ae2d6b8ed7b7c3bb60405e76d4cc91b


---
# <a name="azure-guidance"></a>Azure Kılavuzu
[!INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Microsoft desenleri ve uygulamaları ekibi, Azure Müşteri Danışmanlık Ekibinin bir parçasıdır. Buradaki amacımız; geliştiricilerin, mimarların ve BT uzmanlarının Microsoft Azure platformunda başarılı olmalarına yardımcı olmaktır. Azure’da bulut uygulamaları oluşturmaya yönelik en iyi uygulamaları gösteren bir kılavuz geliştirdik.

## <a name="checklists"></a>Denetim listeleri
Bu listeler, kullanılabilirlik ve ölçeklenebilirliğin temel noktalarını gözden geçirmenizi sağlayan hızlı başvuru kaynaklarıdır. 

* [Kullanılabilirlik Denetim Listesi][AvailabilityChecklist] 
  
    Kullanılabilirlik sağlamak için önerilen uygulamaların bir özeti.
* [Ölçeklenebilirlik Denetim Listesi][ScalabilityChecklist]
  
    Ölçeklenebilir hizmetler tasarlama ve uygulama ile veri yönetimi işlemleri için önerilen uygulamaların bir özeti.

## <a name="best-practices-articles"></a>En iyi uygulamalar makaleleri
Bu makaleler, genelde bulut bilgi işlem ile birlikte ilişkilendirilen önemli kavramların kapsamlı bir incelemesini sunar. 

* [API Tasarımı][APIDesign] 
  
    Web API’si tasarlarken karşılaşılabilecek tasarım sorunlarıyla ilgili bir görüşme.
* [API Uygulaması][APIImplementation] 
  
    Bir web API’si uygulama ve yayımlamaya yönelik bir dizi önerilen uygulama.
* [API güvenlik kılavuzu](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 
  
    Kimlik doğrulama ve yetkilendirme ile ilgili konulardaki tartışmalar (ör. belirteç türleri, yetkilendirme protokolleri, yetkilendirme akışları ve tehdit risklerini azaltma).
* [Otomatik ölçeklendirme kılavuzu][AutoscalingGuidance] 
  
    El ile müdahaleye gerek kalmadan ölçeklendirme çözümleri ile ilgili dikkat edilecek konuların bir özeti.
* [Arka Plan İşleri kılavuzu][BackgroundJobsGuidance] 
  
    Ön planda çalışan ve etkileşimli işlerden bağımsız olarak arka planda çalışması gereken görevleri uygulanmasına yönelik kullanılabilir seçenekler ve önerilen uygulamaların bir açıklaması.
* [İçerik Teslim Ağı (CDN) kılavuzu][CDNGuidance] 
  
    Uygulamalarınızın yükünü en aza indirmek ve kullanılabilirlik ile performansı en üst düzeye çıkarmak için CDN’i kullanmaya yönelik genel kılavuz ve önerilen uygulama.
* [Önbelleğe alma kılavuzu][CachingGuidance] 
  
    Sistemin performansını ve ölçeklenebilirliğini artırmak için önbelleğe alma özelliğinin nasıl kullanılacağı ile ilgili özet.
* [Veri Bölümleme kılavuzu][DataPartitioningGuidance]
  
    Ölçeklenebilirliği geliştirmek, çekişmeyi azaltmak ve performansı iyileştirmek üzere veri bölümlemek için kullanabileceğiniz stratejiler.
* [İzleme ve Tanılama kılavuzu][MonitoringandDiagnosticsGuidance] 
  
    Kullanıcıların sisteminizi nasıl kullandığını izleme, kaynak kullanımını takip etme ve sisteminizin genel durumunu ve performansını izleme kılavuzu.
* [Önerilen adlandırma kuralları][naming-conventions] 
  
    Azure kaynakları için önerilen adlandırma kuralları.
* [Genel Yeniden Deneme kılavuzu][RetryGeneralGuidance] 
  
    Geçici hataları ele almaya yönelik genel kavramlarla ilgili tartışma.
* [Hizmete Özgü Yeniden Deneme kılavuzu][RetryServiceSpecificGuidance]
  
    Azure hizmetleri için yeniden deneme mekanizmasını kullanma, uyarlama ve genişletmeyle ilgili bilgiler dahil bu hizmetlere yönelik yeniden deneme özelliklerinin özeti.

## <a name="scenario-guides"></a>Senaryo kılavuzları
* [Azure’da Elasticsearch’ü çalıştırma][elasticsearch] 
  
    Elasticsearch yüksek oradan ölçeklenebilir açık kaynaklı bir arama motoru ve veritabanıdır. Hızlı çözümleme yapılmasını ve büyük veri kümelerinde bilgi bulunmasını gerektiren durumlar için kullanışlıdır. Bu kılavuzda, bir Elasticsearch kümesi tasarlanırken dikkat edilmesi gereken bazı önemli konular ele alınmaktadır.
* [Çok müşterili uygulamalar için kimlik yönetimi][identity-multitenant] 
  
    Çoklu müşteri mimarisi birden çok müşterinin bir uygulamayı paylaştığı, ancak birbirlerinden yalıtıldığı ortamlardır. Bu kılavuzda, kaydolma ve kimlik doğrulama ile ilgili işlemler için [Azure Active Directory][AzureAD]’yi kullanarak çok müşterili uygulamalarda kullanıcı kimliklerini nasıl yöneteceğiniz açıklanmaktadır.
* [Büyük veri çözümleri geliştirme](https://msdn.microsoft.com/library/dn749874.aspx)
  
    Bu kılavuzda HDInsight’ın yinelemeli araştırma senaryolarında, veri ambarı olarak, ETL işlemleri için ve mevcut BI sistemleriyle tümleştirilmek üzere nasıl kullanılacağı ele alınmaktadır. Ayrıca büyük veri kavramlarını anlama, büyük veri çözümleri planlama ve tasarlama ve bu çözümleri uygulama konusunda rehberlik sunulmaktadır.

## <a name="patterns"></a>Desenler
* [Bulut Tasarım Desenleri: Bulut Uygulamaları için Öngörücü Mimarlık Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx)
  
    Bulut Tasarım Desenleri, tasarım desenleri ile ilgili kılavuz konularından oluşan bir kitaplıktır. Her bir parçanın bulut uygulaması mimarileri ile nasıl uyum sağladığını göstererek desen uygulamanın avantajları belirtilmektedir.
* [Bulut Uygulamaları için Performansı İyileştirme](https://github.com/mspnp/performance-optimization)
  
    Bu kılavuzda, uygulamaların yük altında ölçeklendirilmesini engelleyen yaygın ters desenler açıklanmaktadır. Sekiz farklı ters deseni, [performans çözümleme ile ilgili temel bilgileri](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) ve temel [ölçümlere göre performansı değerlendirme](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md) konularını açıklayan örnekler içermektedir.

## <a name="reference-architectures"></a>Başvuru mimarileri
Başvuru mimarilerimizin düzenleme ölçütü senaryolardır.
Her bağımsız mimaride önerilen uygulamalar ve öngörücü adımların yanı sıra öneriler içeren bir yürütülebilir bileşen sunulmaktadır.

Başvuru mimarilerini içeren güncel kitaplığa [http://aka.ms/architecture](http://aka.ms/architecture) adresinden ulaşabilirsiniz.

## <a name="resiliency-guidance"></a>Dayanıklılık kılavuzu
Bu konu başlıklarında, dağıtılmış bir bulut ortamında hatalara karşı dayanıklı uygulamaların nasıl tasarlanacağı açıklanmaktadır.   

* [Dayanıklılığa genel bakış][ResiliencyOvervew]
  
     Azure platformunda hatalardan kurtarılarak çalışmaya devam edebilecek uygulamaların nasıl oluşturulacağı açıklanmaktadır. Tasarım aşamasından başlayıp uygulama, dağıtım ve işleme geçme aşamasına kadar dayanıklılık sağlayacak güçlü bir yaklaşım ele alınmaktadır.
* [Dayanıklılık denetim listesi][resiliency-checklist]
  
    Oluşabilecek birçok farklı hata moduna karşı hazırlıklı olmanıza yardımcı olacak öneriler içeren bir denetim listesi.
* [Hata modu analizi][resiliency-fma] 
  
    Hata modu çözümlemesi (FMA) muhtemelen hata noktalarını tanımlayarak sisteminizi dayanıklı hale getirme işlemidir. Bu makalede, FMA işlemine başlamanıza yardımcı olması için muhtemel hata modlarını ve bunların riskini nasıl azaltma yollarını içeren bir katalog bulunmaktadır. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md




<!--HONumber=Dec16_HO1-->


