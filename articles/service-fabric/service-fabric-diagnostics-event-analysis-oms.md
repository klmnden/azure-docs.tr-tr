---
title: "Azure Service Fabric olay çözümleme OMS ile | Microsoft Docs"
description: "Görselleştirme ve izleme ve tanılama Azure Service Fabric kümeleri için OMS kullanarak olayları analiz etme hakkında bilgi edinin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 977c5d64a32157b39aa6b618196dde20c4c3cc8e
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Olay çözümleme ve OMS Görselleştirme

Operations Management Suite (OMS), izleme ve tanılama uygulamaları için yardımcı Yönetim Hizmetleri ve bulutta barındırılan hizmetleri koleksiyonudur. OMS ve ne sunar daha ayrıntılı bir özetini almak için okuma [OMS nedir?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-the-oms-workspace"></a>Günlük analizi ve OMS çalışma alanı

Günlük analizi bir Azure depolama tablo veya bir aracı da dahil olmak üzere yönetilen kaynaklardan veri toplayan ve merkezi bir depoya tutar. Veriler, ardından analiz, uyarı ve görselleştirme için kullanılan ya da daha fazla verme olabilir. Günlük analizi olayları, performans verileri ya da herhangi bir özel veri destekler.

OMS yapılandırıldığında, belirli bir erişebilir *OMS çalışma*, burada veri yüklenebilir sorgulanan veya panolarında görselleştirilen kaynağı.

Günlük analizi tarafından alınan veri sonra OMS birkaç sahip *yönetim çözümleri* birkaç senaryo için özelleştirilmiş gelen verileri izlemek için paketlenmiş çözümleri bulunur. Bunlar bir *Service Fabric Analytics* çözüm ve *kapsayıcıları* iki en uygun olanlardır tanılama ve Service Fabric kümeleri kullanırken izleme çözümü. Birkaç diğerleri de incelenmesi yararlı olan vardır ve OMS de sağlar hakkında daha fazla bilgiyi özel çözümler oluşturma [burada](../operations-management-suite/operations-management-suite-solutions.md). Bir küme için kullanmayı seçtiğiniz her bir çözümü, aynı OMS çalışma alanında, günlük analizi yanında yapılandırılabilir. Çalışma alanları özel panolar ve veri ve değişiklikler, işlem, toplamak ve analiz etmek istediğiniz verileri görselleştirme izin verir.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-analytics-solution"></a>Service Fabric analiz çözümü ile bir OMS çalışma alanı ayarlama
Service Fabric çözüm OMS çalışma alanınızda dahil - platform ve uygulama düzeyinden çeşitli gelen günlük kanalları gösterir ve sorgu Service Fabric belirli günlüklerini yeteneği sağlayan bir Pano içerir önerilir. Görece basit bir Service Fabric çözüm nasıl, kümesi üzerinde dağıtılmış tek bir uygulama ile göründüğünü aşağıda verilmiştir:

![OMS BT çözümü](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Bkz: [OMS günlük analizi ayarlamak](service-fabric-diagnostics-oms-setup.md) bu kümeniz için başlamak için.

## <a name="using-the-oms-agent"></a>OMS Aracısı'nı kullanma

Tanılama daha modüler bir yaklaşım için izin vermek için EventFlow ve WAD toplama çözümler olarak kullanmak için önerilen ve izleme. Örneğin, EventFlow, çıkışları değiştirmek istiyorsanız, hiçbir değişiklik, gerçek araçları yalnızca basit bir değişiklikle config dosyasına gerektirir. Ancak, OMS günlük analizi kullanarak yatırım karar verirseniz, ayarladığınız [OMS Aracısı](../log-analytics/log-analytics-windows-agent.md). Ayrıca OMS Aracısı kapsayıcıları, kümeniz için dağıtırken aşağıda açıklandığı gibi kullanmanız gerekir. 

Üzerinden baş [OMS Aracısı bir kümeye ekleme](service-fabric-diagnostics-oms-agent.md) bu adımları için.

Bu avantajları şunlardır:

* Performans sayaçları ve ölçümleri tarafında daha zengin veri
* Kolayca kümeden ve kümenizin yapılandırmasını güncelleştirmek zorunda kalmadan toplanmakta ölçümleri yapılandırılabilir. OMS Portalı'ndan aracısının ayarlarında yapılan değişiklikler yapılabilir ve gerekli yapılandırma ile eşleşmesi için aracıyı otomatik olarak yeniden başlatır. Belirli performans sayaçlarını seçmek için OMS aracısının yapılandırmak için çalışma alanına gidin **Giriş > ayarları > veri > Windows performans sayaçlarını** ve gibi görmek için toplanan veri çekme
* Verileri görüntülenir, OMS tarafından çekilen önce depolanması gerek kalmadan daha hızlı / günlük analizi
* Docker günlükleri (stdout, stderr) ve istatistikleri (kapsayıcı ve düğüm düzeyde performans ölçümleri) seçebilirsiniz kapsayıcıları izleme çok daha kolay olduğu

Ana burada aracı kümenizdeki tüm uygulamalarınızın yanı sıra dağıtılmış olduğundan, olabileceğini uygulamalarınızı kümede performansını artırmak için bazı etkisi noktadır.

## <a name="monitoring-containers"></a>Kapsayıcı izleme

Kapsayıcıları için Service Fabric kümesi dağıtırken, OMS Aracısı ile küme ayarlanmıştır ve kapsayıcıları çözüm tanılama ve izlemeyi etkinleştirmek için OMS çalışma eklendiğini önerilir. Kapsayıcıları çözümü nasıl bir çalışma alanında göründüğünü aşağıda verilmiştir:

![Temel OMS Panosu](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Aracı OMS sorgulanan veya görselleştirilmiş performans göstergeleri için kullanılan birkaç kapsayıcı özgü günlükleri koleksiyonunu sağlar. Toplanan günlük türleri şunlardır:

* ContainerInventory: kapsayıcı konumunu, adı ve görüntüleri hakkında bilgi gösterir
* ContainerImageInventory: bilgi kimlikleri veya boyutları dahil olmak üzere, dağıtılan görüntüler hakkında
* ContainerLog: belirli hata günlüklerini, docker günlükleri (stdout, vb.) ve diğer girişleri
* ContainerServiceLog: çalıştırılmış docker arka plan programı komutları
* Perf: kapsayıcı dahil olmak üzere performans sayaçlarını cpu, bellek, ağ trafiğini, disk g/ç ve ana bilgisayar makinelerden özel ölçümleri

[İzleme OMS günlük analizi ile kapsayıcıları](service-fabric-diagnostics-oms-containers.md) kapsayıcı kümeniz için izlemeyi ayarlamak için gerekli adımlar kapsanmaktadır. OMS'ın kapsayıcıları çözüm hakkında daha fazla bilgi için kullanıma kendi [belgelerine](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki OMS araçları ve bir çalışma alanı gereksinimlerinize özelleştirmek için seçenekleri inceleyin:

* Şirket içi kümeleri için OMS için OMS veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla bilgi [Internet erişimi olmayan bilgisayarlar için OMS OMS ağ geçidini kullanarak bağlanma](../log-analytics/log-analytics-oms-gateway.md)
* Ayarlamak için OMS yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler