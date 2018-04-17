---
title: Günlük analizi ile Azure Service Fabric olay çözümleme | Microsoft Docs
description: Görselleştirme ve izleme ve tanılama Azure Service Fabric kümeleri için günlük analizi kullanarak olayları analiz etme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 290b1d594cc1f874bcfdd0cef728fc78af96f702
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Olay çözümleme ve görselleştirme günlük analizi

Azure yönetim, izleme ve tanılama uygulamaları için yardımcı Yönetim Hizmetleri ve bulutta barındırılan hizmetleri koleksiyonunu çözümdür. Günlük analizi ve ne sunar daha ayrıntılı bir özetini almak için okuma [günlük analizi nedir?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı

Günlük analizi bir Azure depolama tablo veya bir aracı da dahil olmak üzere yönetilen kaynaklardan veri toplayan ve merkezi bir depoya tutar. Veriler, ardından analiz, uyarı ve görselleştirme için kullanılan ya da daha fazla verme olabilir. Günlük analizi olayları, performans verileri ya da herhangi bir özel veri destekler.

Günlük analizi yapılandırıldığında, belirli bir erişebilir *günlük analizi çalışma alanı*, burada veri yüklenebilir sorgulanan veya panolarında görselleştirilen kaynağı.

Günlük analizi tarafından alınan veri sonra Azure birkaç sahip *yönetim çözümleri* birkaç senaryo için özelleştirilmiş gelen verileri izlemek için paketlenmiş çözümleri bulunur. Bunlar bir *Service Fabric Analytics* çözüm ve *kapsayıcıları* iki en uygun olanlardır tanılama ve Service Fabric kümeleri kullanırken izleme çözümü. Birkaç diğerleri de incelenmesi yararlı olan vardır ve günlük analizi de sağlar hakkında daha fazla bilgiyi özel çözümler oluşturma [burada](../operations-management-suite/operations-management-suite-solutions.md). Bir küme için kullanmayı seçtiğiniz her bir çözümü, aynı günlük analizi çalışma alanında, günlük analizi yanında yapılandırılabilir. Çalışma alanları özel panolar ve veri ve değişiklikler, işlem, toplamak ve analiz etmek istediğiniz verileri görselleştirme izin verir.

## <a name="setting-up-a-log-analytics-workspace-with-the-service-fabric-analytics-solution"></a>Service Fabric analiz çözümü ile günlük analizi çalışma alanı ayarlama
Önerilen günlük analizi çalışma alanınızda hizmeti yapı çözümü dahil - platform ve uygulama düzeyinden çeşitli gelen günlük kanalları gösterir ve sorgulama Service Fabric belirli sağlayan bir Pano içerir günlüğe kaydeder. Görece basit bir Service Fabric çözüm nasıl, kümesi üzerinde dağıtılmış tek bir uygulama ile göründüğünü aşağıda verilmiştir:

![OMS BT çözümü](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Bkz: [günlük analizi ayarlamak](service-fabric-diagnostics-oms-setup.md) bu kümeniz için başlamak için.

## <a name="using-the-oms-agent"></a>OMS Aracısı'nı kullanma

Tanılama daha modüler bir yaklaşım için izin vermek için EventFlow ve WAD toplama çözümler olarak kullanmak için önerilen ve izleme. Örneğin, EventFlow, çıkışları değiştirmek istiyorsanız, hiçbir değişiklik, gerçek araçları yalnızca basit bir değişiklikle config dosyasına gerektirir. Ancak, günlük analizi kullanarak yatırım karar verirseniz, ayarladığınız [OMS Aracısı](../log-analytics/log-analytics-windows-agent.md). Ayrıca OMS Aracısı kapsayıcıları, kümeniz için dağıtırken aşağıda açıklandığı gibi kullanmanız gerekir. 

Üzerinden baş [OMS Aracısı bir kümeye ekleme](service-fabric-diagnostics-oms-agent.md) bu adımları için.

Bu avantajları şunlardır:

* Performans sayaçları ve ölçümleri tarafında daha zengin veri
* Kolayca kümeden ve kümenizin yapılandırmasını güncelleştirmek zorunda kalmadan toplanmakta ölçümleri yapılandırılabilir. OMS Portalı'ndan aracısının ayarlarında yapılan değişiklikler yapılabilir ve gerekli yapılandırma ile eşleşmesi için aracıyı otomatik olarak yeniden başlatır. Belirli performans sayaçlarını seçmek için OMS aracısının yapılandırmak için çalışma alanına gidin **Giriş > ayarları > veri > Windows performans sayaçlarını** ve gibi görmek için toplanan veri çekme
* Verileri, günlük analizi tarafından çekilen önce depolanması gerek kalmadan daha hızlı görüntülenir
* Docker günlükleri (stdout, stderr) ve istatistikleri (kapsayıcı ve düğüm düzeyde performans ölçümleri) seçebilirsiniz kapsayıcıları izleme çok daha kolay olduğu

Ana burada aracı kümenizdeki tüm uygulamalarınızın yanı sıra dağıtılmış olduğundan, olabileceğini uygulamalarınızı kümede performansını artırmak için bazı etkisi noktadır.

## <a name="monitoring-containers"></a>Kapsayıcı izleme

Kapsayıcıları için Service Fabric kümesi dağıtırken, OMS Aracısı ile küme ayarlanmıştır ve kapsayıcıları çözüm tanılama ve izlemeyi etkinleştirmek için günlük analizi çalışma alanı eklendiğini önerilir. Kapsayıcıları çözümü nasıl bir çalışma alanında göründüğünü aşağıda verilmiştir:

![Temel OMS Panosu](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Aracı, günlük analizi sorgulanan veya görselleştirilmiş performans göstergeleri için kullanılan birkaç kapsayıcı özgü günlükleri koleksiyonunu sağlar. Toplanan günlük türleri şunlardır:

* ContainerInventory: kapsayıcı konumunu, adı ve görüntüleri hakkında bilgi gösterir
* ContainerImageInventory: bilgi kimlikleri veya boyutları dahil olmak üzere, dağıtılan görüntüler hakkında
* ContainerLog: belirli hata günlüklerini, docker günlükleri (stdout, vb.) ve diğer girişleri
* ContainerServiceLog: çalıştırılmış docker arka plan programı komutları
* Perf: kapsayıcı dahil olmak üzere performans sayaçlarını cpu, bellek, ağ trafiğini, disk g/ç ve ana bilgisayar makinelerden özel ölçümleri

[İzleme günlük analizi ile kapsayıcıları](service-fabric-diagnostics-oms-containers.md) kapsayıcı kümeniz için izlemeyi ayarlamak için gerekli adımlar kapsanmaktadır. Günlük analizi'nın kapsayıcıları çözüm hakkında daha fazla bilgi için kullanıma kendi [belgelerine](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki günlük analizi araçları ve bir çalışma alanı gereksinimlerinize özelleştirmek için seçenekleri inceleyin:

* Şirket içi kümeleri için günlük analizi için günlük analizi veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla bilgi [günlük OMS ağ geçidini kullanma analizi için Internet erişimi olmayan bilgisayarları bağlama](../log-analytics/log-analytics-oms-gateway.md)
* Ayarlamak için günlük analizi yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler