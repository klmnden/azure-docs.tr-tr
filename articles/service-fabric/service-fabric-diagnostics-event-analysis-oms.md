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
ms.openlocfilehash: f0cefab15a115719ea9c378546a7e6004bd06187
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
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

Sağlamak ve Resource Manager şablonu üzerinden veya doğrudan Azure Marketi'nden bir OMS çalışma yapılandırmak için iki yolu vardır. Eski bir küme dağıtımı ve Tanılama ile dağıtılan bir küme zaten varsa ikinci etkin olduğunda kullanın.

### <a name="deploying-oms-using-a-resource-management-template"></a>Kaynak Yönetimi şablonunu kullanarak OMS dağıtma

Resource Manager şablonu kullanarak bir küme dağıtımı sırasında şablonun yeni bir OMS çalışma alanı oluşturabilirsiniz hizmeti yapı çözümü için ekleyip uygun depolama tablolarından verileri okumak için yapılandırabilirsiniz.

>[!NOTE]
>Bunun çalışması için tanılama olması için OMS mevcut için Azure depolama tabloları için etkinleştirilmesi gerekir / günlük analizi'nın gelen bilgileri okuyun.

[Burada](https://azure.microsoft.com/resources/templates/service-fabric-oms/) eylemleri gerçekleştirir gereksinime uygun şekilde değiştirin ve kullanan bir örnek şablonudur. Daha fazla isteğe bağlı olma istediğinizden, burada işleminde bir OMS çalışma ayarını olabilir bağlı olarak farklı seçenekler size birkaç daha fazla şablon vardır - konumunda bulunabilir durumda [Service Fabric ve OMS şablonları](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Azure Marketi'nde OMS kullanarak dağıtma

Bir küme dağıttıktan sonra bir OMS çalışma eklemek isterseniz, Azure Marketi head üzerinden ve Ara *"Service Fabric Analytics"*.

![OMS BT analizi pazarında](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

* Tıklayın **oluşturma**
* Service Fabric Analytics oluşturma penceresinde **bir çalışma alanı seçin** için *OMS çalışma* alan ve ardından **yeni bir çalışma alanı oluşturma**. Gerekli girişleri doldurun - burada tek gereksinim abonelik Service Fabric kümesi ve OMS çalışma alanı için aynı olmalıdır. Girişlerinizi doğruladıktan sonra OMS çalışma dağıtmaya başlar. Bu, yalnızca birkaç dakika sürer.
* Tamamlandığında tıklatarak **oluşturma** Service Fabric Analytics oluşturma penceresinin altındaki yeniden. Yeni bir çalışma alanı altında gösterildiğini doğrulayın *OMS çalışma*. Bu çözüm, yeni oluşturduğunuz çalışma alanına ekleyin.


Bu çözüm için çalışma alanına ekler de, çalışma alanı hala kümenizden gelen tanılama verilerini bağlı olması gerekir. Service Fabric Analytics çözümde oluşturduğunuz kaynak grubuna gidin. Görmeniz gerekir bir *ServiceFabric (\<nameOfOMSWorkspace\>)*.

* Burada çözüm ayarları, çalışma alanı ayarları değiştirin ve OMS Portalı'na gidin, genel bakış sayfasına gitmek için çözüm tıklayın.
* Sol gezinti menüsünde tıklatın **depolama hesapları günlükleri**altında *çalışma veri kaynakları*.

    ![Service Fabric analiz çözümü portalında](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-portal.png)

* Üzerinde *depolama hesabı günlüklerine* sayfasında, **Ekle** kümenizin günlükleri için çalışma alanına eklemek için üst.
* İçine tıklatın **depolama hesabı** kümenizdeki oluşturulan uygun hesap eklemek için. Varsayılan ad kullandıysanız, depolama hesabı adı *sfdg\<resourceGroupName\>*. Kümeniz için kullanılan değer denetleyerek dağıtmak için kullanılan Azure Resource Manager şablonu denetleyerek bu de onaylayabilirsiniz `applicationDiagnosticsStorageAccountName`. Aşağı kaydırın ve da sahip olabilirsiniz **daha fazla yük** hesap adı görünmüyor durumunda. Seçmek için gösteriliyorsa doğru depolama hesabı adına tıklayın.
* Ardından, belirtmeniz gerekecektir *veri türü*, gereken **Service Fabric olayları**.
* *Kaynak* otomatik olarak ayarlanması gerektiğini *WADServiceFabric\*EventTable*.
* Tıklatın **Tamam** çalışma alanınızı kümenizin günlüklerine bağlanmak için.

    ![Depolama hesabı günlükleri için OMS ekleme](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

* Hesap artık bir parçası olarak gösterilmesi gerekir, *depolama hesabı günlüklerine* alanınıza ait veri kaynaklarında.

Bu, artık Service Fabric analiz çözümü artık doğru kümenizin platform ve uygulama günlüğü tablosu bağlanmış bir OMS günlük analizi çalışma alanındaki eklediniz. Ek kaynaklar aynı şekilde çalışma alanına ekleyebilirsiniz.

## <a name="using-the-oms-agent"></a>OMS Aracısı'nı kullanma

Tanılama daha modüler bir yaklaşım için izin vermek için EventFlow ve WAD toplama çözümler olarak kullanmak için önerilen ve izleme. Örneğin, EventFlow, çıkışları değiştirmek istiyorsanız, hiçbir değişiklik, gerçek araçları yalnızca basit bir değişiklikle config dosyasına gerektirir. Ancak, OMS kullanarak yatırım karar ve olay çözümleme için kullanmaya devam etmeye hazır olduğunuz, (yalnızca olmak zorunda değildir platform kullanın, ancak bu BT platformları en az biri yerine görüntülenir), yedekleme ayarı keşfedin öneririz [OMS ag ent](../log-analytics/log-analytics-windows-agents.md). Ayrıca OMS Aracısı kapsayıcıları, kümeniz için dağıtırken aşağıda açıklandığı gibi kullanmanız gerekir.

Yalnızca bir sanal makine ölçek uzantısı, Resource Manager şablonu için ayarladığınız gibi aracısı eklemek, her düğümleriniz yüklenmesini sağlayarak sahip olduğundan işlem bunu yapmak için oldukça kolaydır. Service Fabric çözümü (yukarıdaki gibi) ile OMS çalışma dağıtan ve aracı, düğüm ekler örnek Resource Manager şablonu çalıştıran kümeler için bulunabilir [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) veya [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

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

Bu makalede, kümeniz için kapsayıcı izleme ayarlamak için gerekli adımlar kapsanmaktadır. OMS'ın kapsayıcıları çözüm hakkında daha fazla bilgi için kullanıma kendi [belgelerine](../log-analytics/log-analytics-containers.md).

Çalışma alanınızda kapsayıcı çözümünü kurmak için yukarıdaki adımları izleyerek, küme düğümlerinde dağıtılan OMS Aracısı sahip emin olun. Küme hazır olduktan sonra bir kapsayıcı dağıtma. Bir küme için bir kapsayıcı görüntüsü dağıtılan ilk kez, görüntünün boyutuna bağlı olarak yüklemek için birkaç dakika sürer aklınızda size aittir.

Azure Marketi'nde aramak *kapsayıcı izlemesi çözümü* ve oluşturma **kapsayıcı izlemesi çözümü** , alınması gereken izleme + Yönetimi altında sonuç kategorisi.

![Kapsayıcıları çözüm ekleme](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

Oluşturma adımda bir OMS çalışma ister. Yukarıdaki dağıtımı ile oluşturulmuş bir tanesini seçin. Bu adım, OMS çalışma kapsayıcıları çözümünüzde ekler ve şablon tarafından dağıtılan OMS aracısı tarafından otomatik olarak algılanır. Aracı, küme ve 15 dakikadan kısa kapsayıcıları işlemler üzerinde veri toplamayı başlayacaktır veya bu nedenle, yukarıdaki Pano görüntüsü olduğu gibi verilerle yukarı hafif çözüm görmeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki OMS araçları ve bir çalışma alanı gereksinimlerinize özelleştirmek için seçenekleri inceleyin:

* Şirket içi kümeleri için OMS için OMS veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla bilgi [Internet erişimi olmayan bilgisayarlar için OMS OMS ağ geçidini kullanarak bağlanma](../log-analytics/log-analytics-oms-gateway.md)
* Ayarlamak için OMS yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler