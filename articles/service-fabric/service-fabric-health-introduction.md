---
title: Service Fabric sistem durumu izleme | Microsoft Docs
description: Azure Service Fabric sistem durumu kümeyi ve uygulamaları ve Hizmetleri izlemeyi sağlayan modeli izleme giriş.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: ''
ms.assetid: 1d979210-b1eb-4022-be24-799fd9d8e003
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: fc0bb56e85c2a9cf7a458b0f6d97887d392ee65f
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114325"
---
# <a name="introduction-to-service-fabric-health-monitoring"></a>Service Fabric sistem durumu izlemeye giriş
Azure Service Fabric zengin, esnek ve Genişletilebilir sistem durumu değerlendirmesi ve raporlama sağlar bir sistem durumu modeli sunar. Model durumu kümeyi ve içinde çalışan hizmetleri yakın gerçek zamanlı izlenmesini sağlar. Kolayca sistem durumu bilgilerini almak ve düzeltmek olası sorunlar basamaklı ve yoğun kesintileri neden önce. Tipik modelinde Hizmetleri kendi yerel görünümleri temel alan raporları göndermek ve genel bir sağlamak için bu bilgileri de toplanır küme görünüm düzeyi.

Service Fabric bileşenleri, bunların geçerli durumu raporlamak için bu zengin sistem durumu modeli kullanır. Rapor sağlık aynı mekanizmayı uygulamalarınızdan kullanabilirsiniz. Yüksek kaliteli sistem durumu, özel koşullarınızı yakalayan raporlamada yatırım algılamak ve çalışan uygulamanız için daha kolay düzeltin.

Aşağıdaki Microsoft Virtual Academy video de Service Fabric sistem durumu modeli ve nasıl kullanıldığı açıklanmaktadır: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tevZw56yC_1906218965">
<img src="./media/service-fabric-health-introduction/HealthIntroVid.png" WIDTH="360" HEIGHT="244">
</a></center>

> [!NOTE]
> Biz izlenen yükseltmeler gereksinimini gidermek için sistem durumu alt sistemi başlatıldı. Service Fabric tam, kapalı kalma süresi kullanılabilirliğini izlenen uygulama ve küme yükseltmeleri sağlar ve kullanıcı müdahalesi için en az. Bu hedeflere ulaşmak için yapılandırılmış yükseltme ilkelerine bağlı olarak durumu yükseltme denetler. Yalnızca sistem durumu istediğiniz eşikleri dikkate alır, bir yükseltme devam edebilirsiniz. Aksi halde, yükseltme otomatik olarak geri veya Yöneticiler sorunları gidermek için bir şansı vermek duraklatıldı. Uygulama yükseltme hakkında daha fazla bilgi için bkz: [bu makalede](service-fabric-application-upgrade.md).
> 
> 

## <a name="health-store"></a>Sistem durumu deposu
Sistem sağlığı deposunu kolay alma ve değerlendirme için kümedeki varlıkları hakkındaki durumuyla ilgili bilgileri tutar. Service Fabric yüksek kullanılabilirlik ve ölçeklenebilirlik sağlamak için durum bilgisi olan hizmet kalıcı olarak uygulanır. Sistem sağlığı deposunu parçası olan **fabric: / Sistem** uygulama ve kümenin çalışır durumda olduğunda kullanılabilir ve çalışır.

## <a name="health-entities-and-hierarchy"></a>Sistem durumu varlıkları ve hiyerarşisi
Sistem durumu varlıkları etkileşimleri ve farklı varlıklar arasında bağımlılıklar yakalar mantıksal bir hiyerarşide düzenlenir. Sistem sağlığı deposunu durumu varlıklarını ve hiyerarşi Service Fabric bileşenleri alınan raporları göre otomatik olarak oluşturur.

Sistem durumu varlıkları Service Fabric varlıklar yansıtır. (Örneğin, **sistem durumu uygulama varlığı** uygulama örneğini eşleşen kümede dağıtıldı, ancak **sistem durumu düğümünde varlık** Service Fabric küme düğümü eşleşir.) Sistem durumu hiyerarşi sistem varlıkları etkileşimlerinin yakalar ve Gelişmiş Sistem durumu değerlendirmesi temelidir. Anahtar Service Fabric kavramlar hakkında bilgi alabilirsiniz [Service Fabric teknik genel bakış](service-fabric-technical-overview.md). Uygulama hakkında daha fazla bilgi için bkz: [Service Fabric uygulama modeli](service-fabric-application-model.md).

Sistem durumu varlıkları ve hiyerarşi küme ve etkili bir şekilde bildirilen, hata ayıklaması, izlenen ve uygulamaları izin verir. Sistem durumu modeli doğru bir, sağlar *ayrıntılı* kümedeki birçok taşıma parçaları durumunu gösterimi.

![Sistem durumu varlıkları.][1]
Sistem durumu varlıkları organize üst-alt ilişkilere dayanan bir hiyerarşideki.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Sistem durumu varlıkları şunlardır:

* **Küme**. Service Fabric kümesi durumunu temsil eder. Küme sistem durumu raporlarının tüm küme etkileyen koşullar açıklanmaktadır. Bu koşullar kümenin veya küme içindeki birden çok varlık etkiler. Koşula göre bir veya daha fazla sağlıksız alt aşağıya doğru sorunu Raporlayıcı daraltamazsınız. Küme ağ bölümleme ya da iletişim sorunları nedeniyle bölme beyin örnekler.
* **Düğüm**. Service Fabric düğümü durumunu temsil eder. Düğüm durumu raporları düğümü işlevselliğini etkileyen koşullar açıklanmaktadır. Bunlar genellikle üzerinde çalışan tüm dağıtılan varlıklar etkiler. Örnekler düğüm disk alanı yetersiz (veya diğer makine genelinde özellikleri, bellek, bağlantıları gibi) ve bir düğüm olduğunda kapalı. Düğümün varlık düğüm adı (dize) tarafından tanımlanır.
* **Uygulama**. Kümede çalışan bir uygulama örneği durumunu temsil eder. Uygulama sistem durumu raporları uygulamanın genel sağlığını etkileyen koşullar açıklanmaktadır. Bunlar ayrı alt (Hizmetleri veya dağıtılan uygulamalar) daraltabilir olamaz. Farklı hizmetler arasında uçtan uca etkileşim uygulamada örnekler. Uygulama varlığı uygulama adı (URI) tarafından tanımlanır.
* **Hizmet**. Kümede çalışan bir hizmetin durumunu temsil eder. Hizmet sistem durumu raporları genel sistem durumu hizmetinin etkileyen koşullar açıklanmaktadır. Sağlıksız bölüm ya da çoğaltma sorunu kapalı Bildirici daraltamazsınız. Örnek tüm bölümler için sorunlara neden bir hizmet yapılandırması (örneğin, bağlantı noktası veya dış dosya paylaşımı) verilebilir. Hizmet varlığı hizmet adı (URI) tarafından tanımlanır.
* **Bölüm**. Bir hizmet bölüm durumunu temsil eder. Bölüm sistem durumu raporlarının tüm çoğaltma kümesi etkileyen koşullar açıklanmaktadır. Örnek çoğaltmaların sayısı hedef sayısı altında olduğunda ve bir bölüm çekirdek kaybında olduğunda verilebilir. Bölüm varlık bölüm kimlik (GUID) tarafından tanımlanır.
* **Çoğaltma**. Bir durum bilgisi olan hizmet çoğaltmayı veya bir durum bilgisiz hizmet örneği durumunu temsil eder. Çoğaltma watchdogs ve sistem bileşenleri üzerinde bir uygulama için bildirebileceği en küçük bir birimdir. Durum bilgisi olan hizmetler için işlemleri ikincil kopya ve yavaş çoğaltma için çoğaltma yapamaz bir birincil çoğaltma örnek verilebilir. Ayrıca, durum bilgisi olmayan bir örnek kaynaklar yetersiz çalışıyor veya bağlantı sorunları bildirebilirsiniz. Çoğaltma varlığı, bölüm kimliği (GUID) ve çoğaltma veya örnek kimliği (uzun) tarafından tanımlanır.
* **DeployedApplication**. Sistem durumunu temsil eden bir *bir düğüm üzerinde çalışan uygulama*. Dağıtılmış uygulama sistem durumu raporlarının koşullar uygulamaya özgü aynı düğümde dağıtılan hizmet paketlerini daraltabilir olamaz düğümünde açıklanmaktadır. Örnek uygulama paketi bu düğümde yüklendiğinde ve uygulama güvenlik sorumluları düğümde ayarlarken sorun hataları verilebilir. Dağıtılan bir uygulama, uygulama adı (URI) ve düğüm adı (dize) tarafından tanımlanır.
* **DeployedServicePackage**. Kümedeki bir düğüm üzerinde çalışan bir hizmet paketi durumunu temsil eder. Aynı uygulama için aynı düğümdeki diğer hizmet paketlerinin etkilemez bir hizmet paketi için belirli koşullar açıklanmaktadır. Örnek kod paketi başlatılamıyor, hizmet paketi ve okunamıyor bir yapılandırma paketi verilebilir. Dağıtılan hizmet paketi, uygulama adı (URI), düğüm adı (dize), hizmet bildirim adı (dize) ve hizmet paketi etkinleştirme kimliği (dizesi) tarafından tanımlanır.

Sistem durumu modeli kesinliği algılayabilir ve sorunları gidermek daha kolay hale getirir. Örneğin, bir hizmeti yanıt vermiyor, uygulama örneği sağlıksız olduğunu bildirmek için uygun olur. Sorun bu uygulamadaki tüm hizmetleri etkileyen değil çünkü adresindeki düzeyi ancak ideal, olmadığını raporlama. Daha fazla bilgi için bu bölümü gösteriyorsa raporun sağlıksız hizmet veya belirli alt bölüm uygulanmalıdır. Verileri otomatik olarak hiyerarşi ve sağlıksız bir bölümü aracılığıyla yüzeyleri yapıldığında görünür hizmet ve uygulama düzeylerinde. Bu toplama sabitleme ve sorunun kök nedenini daha hızlı bir şekilde çözmek için yardımcı olur.

Sistem durumu hiyerarşinin üst-alt ilişkilerini oluşur. Bir küme düğümleri ve uygulamaların oluşur. Uygulamaları, hizmetleri ve uygulamaları dağıtılır. Dağıtılmış uygulamalar, hizmet paketleri dağıttım. Hizmetleri bölümleri varsa ve her bölüm bir veya daha fazla çoğaltmalarına sahip. Düğümleri ve dağıtılan varlıklar arasında özel bir ilişki yoktur. Sağlıksız düğüm kendi yetkilisi sistem bileşeni tarafından Yük Devretme Yöneticisi hizmeti bildirilen dağıtılan uygulamaları, hizmet paketleri ve çoğaltmalar, üzerinde dağıtılan etkiler.

Sistem durumu hiyerarşi neredeyse gerçek zamanlı bilgiler son sistem durumu raporları, temel sistem en son durumunu temsil eder.
İç ve dış watchdogs, uygulamaya özgü mantığı veya özel izlenen koşullara göre aynı varlıklar üzerinde bildirebilirsiniz. Kullanıcı raporları sistem raporlarla aradadır.

Rapor ve sistem durumu için büyük bulut hizmeti tasarım sırasında yanıt vermek nasıl kullanacağınızı planlayın. Bu önceden investement hizmet hata ayıklama, izlemek ve çalıştırmak kolaylaştırır.

## <a name="health-states"></a>Sistem sağlığı durumları
Service Fabric bir varlık sağlıklı olup olmadığını açıklamak için üç sistem durumlarını kullanır: Tamam, uyarı ve hata. Sistem durumu mağazaya gönderilen herhangi bir raporu şu durumlardan birini belirtmeniz gerekir. Sistem durumu değerlendirme sonucu bu durumlardan biri.

Olası [sistem durumlarını](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) şunlardır:

* **TAMAM**. Varlık sağlıklı değil. Bilinen, veya alt öğelerinden (uygunsa) bildirilen herhangi bir sorun vardır.
* **Uyarı**. Varlığın bazı sorunlar vardır, ancak bunu düzgün çalışmaya devam. Örneğin, gecikmeler vardır, ancak bunlar işlevsel sorunları henüz neden olmaz. Bazı durumlarda, Uyarı koşulu kendisini dış müdahalesi olmadan düzeltebilir. Bu durumda, sistem durumu raporlarının farkındalığı artırmak ve görünürlük ne olacağına sağlayın. Diğer durumlarda, kullanıcı müdahalesi olmadan ciddi bir sorun içine Uyarı koşulu düşürebilir.
* **Hata**. Varlık sağlam değil. Düzgün çalışamaz çünkü varlık durumu düzeltmek için yapılması gerekir.
* **Bilinmeyen**. Varlık health store içinde yok. Bu sonucu birden çok bileşen sonuçlarından birleştirme dağıtılmış sorgular öğesinden elde edilebilir. Get düğüm liste sorgusu Örneğin, gider **FailoverManager**, **ClusterManager**, ve **HealthManager**; uygulama alma liste sorgusu gider  **ClusterManager** ve **HealthManager**. Bu sorguları birden çok sistem bileşenleri sonuçlarından birleştirin. Başka bir sistem bileşeni health store içinde mevcut olmayan bir varlık döndürürse, birleştirilmiş sonucu bilinmiyor sahip sistem durumu. Bir varlık deposunda sistem durumu raporlarının henüz işlenmemiş veya varlık silindikten sonra temizlendi olduğundan değil.

## <a name="health-policies"></a>Sistem durumu ilkeleri
Sistem sağlığı deposunu, raporlar ve alt öğelerini göre bir varlık sağlıklı olup olmadığını belirlemek için sistem durumu ilkeleri uygulanır.

> [!NOTE]
> Sistem durumu ilkeleri (için sistem durumu değerlendirmesi küme ve düğüm) Küme bildirimi veya uygulama bildirimi (için uygulama değerlendirmesi ve öğelerinden herhangi biri) belirtilebilir. Sistem durumu değerlendirme istekleri, yalnızca bu değerlendirme için kullanılan özel sistem durumu değerlendirme İlkeleri'nde de geçirebilirsiniz.
> 
> 

Varsayılan olarak, Service Fabric katı kurallar üst-alt hiyerarşik ilişkiyi (her şeyi sağlıklı olmalıdır) uygulanır. Üst, alt öğelerini biri bile bir sağlıksız olay varsa, kötü olarak değerlendirilir.

### <a name="cluster-health-policy"></a>Küme sistem durumu ilkesi
[Küme sistem durumu ilkesi](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) düğümü sistem durumlarını ve küme sistem durumunu değerlendirmek için kullanılır. İlke küme bildiriminde tanımlanabilir. Mevcut değilse, varsayılan ilke (toleranslı hataları sıfır) kullanılır.
Küme sistem durumu ilkesi içerir:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Uyarı sistem durumu işlemek için hata olarak sistem durumu değerlendirmesi sırasında raporlar olup olmadığını belirtir. Varsayılan: false.
* [MaxPercentUnhealthyApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Küme hata olarak kabul edilmeden önce sağlıksız uygulamaları toleranslı yüzdesinin üst sınırını belirtir.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Küme hata olarak kabul edilmeden önce sağlıksız düğümleri toleranslı yüzdesinin üst sınırını belirtir. Bu yüzde, tolerans şekilde yapılandırılmalıdır büyük kümelerinde, bazı düğümler her zaman aşağı veya çıkışı onarımı için; dolayısıyla.
* [ApplicationTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). Uygulama türü sistem durumu ilkesi eşlem küme sistem durumu değerlendirmesi sırasında özel uygulama türlerini tanımlamak için kullanılabilir. Varsayılan olarak, tüm uygulamaları bir havuza yerleştirin ve MaxPercentUnhealthyApplications ile değerlendirilir. Bazı uygulama türleri farklı şekilde ele alınması gerektiğini, genel havuzunun dışına alınabilir. Bunun yerine, bunlar harita kendi uygulama türü adı ile ilişkili yüzdeleri karşı değerlendirilir. Örneğin, bir küme var. uygulamaları farklı türlerde binlerce ve bir özel uygulama türünün birkaç denetim uygulama örnekleri Denetim uygulamalarını hiç hata olmalıdır. % 20 bazı hatalar tolerans, ancak "ControlApplicationType" MaxPercentUnhealthyApplications 0 olarak ayarlayın. uygulama türü için genel MaxPercentUnhealthyApplications belirtebilirsiniz. Bu şekilde, birçok uygulama sağlıksız bazıları, ancak genel sağlıksız yüzdesi Aşağıda, küme uyarı olarak değerlendirilmesi. Sistem Durumu Uyarısı Küme yükseltme etkilemez veya diğer izleme tarafından hata durumu tetiklendi. Ancak hata bile bir denetim uygulamada geri tetikler veya yükseltme yapılandırmasına bağlı olarak Küme yükseltme duraklatır küme sağlıksız, hale getirir.
  Eşleme içinde tanımlanan uygulama türleri için tüm uygulama örnekleri uygulamaları genel havuzu dışında alınır. Bunlar, belirli MaxPercentUnhealthyApplications eşlemesinden kullanarak uygulama türünde uygulamalar toplam sayısına dayalı olarak değerlendirilir. Uygulamaların tüm rest genel havuzda ve MaxPercentUnhealthyApplications ile değerlendirilir.

Aşağıdaki örnek, bir küme bildirimindeki bir alıntı aynıdır. Uygulama türü eşlemesinde girdileri tanımlamak için "ApplicationTypeMaxPercentUnhealthyApplications ardından uygulama türü adı-" ile parametre adı öneki.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Uygulama durumu ilkesi
[Uygulama durumu ilkesi](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) olayları ve alt durumları toplama değerlendirmesi uygulamaları ve bunların alt öğeleri için nasıl yapılacağı açıklanır. Uygulama bildiriminde tanımlanabilir **ApplicationManifest.xml**, uygulama paketi. İlke belirtilirse, Service Fabric varlık uyarı veya hata durumu bir sistem durumu raporu ya da bir alt sahipse sağlıksız olduğunu varsayar.
Yapılandırılabilir ilkeler şunlardır:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Uyarı sistem durumu işlemek için hata olarak sistem durumu değerlendirmesi sırasında raporlar olup olmadığını belirtir. Varsayılan: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Uygulama hata olarak kabul edilmeden önce sağlıksız dağıtılmış uygulamalar toleranslı yüzdesinin üst sınırını belirtir. Bu yüzde, uygulamaları şu anda kümede dağıtılan düğüm sayısı üzerinden sağlıksız dağıtılan uygulama sayısı bölünmesiyle hesaplanır. Küçük sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan yüzdesi: sıfır.
* [DefaultServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Uygulamadaki tüm hizmet türleri için varsayılan sistem durumu ilkesi değiştirir varsayılan service type durum ilkesi belirtir.
* [ServiceTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Hizmet türü başına hizmet sistem durumu ilkeleri haritasını sağlar. Bu ilkelerin her belirtilen hizmet türü için varsayılan hizmet türü sistem durumu ilkeleri değiştirin. Örneğin, bir uygulama bir durum bilgisi olmayan ağ geçidi hizmet türü ve bir durum bilgisi olan altyapısı hizmet türü varsa, bunların değerlendirme için sistem durumu ilkeleri farklı şekilde yapılandırabilirsiniz. Hizmet türü bazında ilke belirttiğinizde, hizmeti konusunda daha ayrıntılı denetim kazanmadan.

### <a name="service-type-health-policy"></a>Service type durum ilkesi
[Service type durum ilkesi](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) nasıl değerlendirmek ve Hizmetleri ve Hizmetleri alt toplama belirtir. İlke içerir:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Bir hizmet sağlıksız kabul edilmeden önce toleranslı sağlıksız bölümleri yüzdesinin üst sınırını belirtir. Varsayılan yüzdesi: sıfır.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Bir bölüm sağlıksız kabul edilmeden önce toleranslı sağlıksız çoğaltmaları yüzdesinin üst sınırını belirtir. Varsayılan yüzdesi: sıfır.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Uygulama sağlıksız kabul edilmeden önce toleranslı sağlıksız Hizmetleri yüzdesinin üst sınırını belirtir. Varsayılan yüzdesi: sıfır.

Aşağıdaki örnek bir uygulama bildirimi yapılan bir alıntı verilmiştir:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Sistem durumu değerlendirmesi
Kullanıcılar ve otomatik hizmetler için herhangi bir varlık health herhangi bir zamanda değerlendirebilirsiniz. Bir varlığın sistem durumunu değerlendirmek için sistem durumu deposu toplamalar tüm sistem durumu varlık üzerinde raporlar ve tüm alt öğelerini (uygunsa) değerlendirir. Sistem durumu raporlarının değerlendirmek ve alt sistem durumlarını (uygunsa) toplama belirtin sistem durumu ilkeleri sistem durumu toplama algoritmasını kullanır.

### <a name="health-report-aggregation"></a>Sistem durumu rapor toplama
Bir varlığın farklı özellikleri farklı reporters (sistem bileşenleri veya watchdogs) tarafından gönderilen birden fazla sistem durumu raporlarının olabilir. Toplama ilişkili sistem durumu ilkeleri, uygulama veya küme sistem durumu ilkesi ConsiderWarningAsError üyesi özellikle kullanır. ConsiderWarningAsError nasıl uyarıları değerlendirileceğini belirtir.

Toplanan sistem durumu tarafından tetiklenen *kötü* durumu raporları varlık üzerinde. En az bir hata sistem durumu raporu ise, toplanan sistem durumu bir hatadır.

![Hata raporu sistem durumu rapor toplama.][2]

Bir veya daha fazla hata durumu raporların bulunduğu bir sistem durumu varlık hata olarak değerlendirilir. Aynı sistem durumu bakılmaksızın bir süresi dolmuş durum raporu için geçerlidir.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Hiçbir hata raporlarını ve bir veya daha fazla uyarı olduğunda toplanan sistem durumu uyarı veya hata ConsiderWarningAsError İlkesi bayrağı bağlı olur.

![Sistem durumu rapor toplama uyarı raporu ve ConsiderWarningAsError false.][3]

Sistem durumu rapor toplama uyarı raporu ve ConsiderWarningAsError (varsayılan) false olarak ayarlayın.

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Alt sistem durumu toplama
Bir varlık toplanan sistem durumunu (uygunsa) alt sistem durumlarını yansıtır. Algoritma alt sistem durumlarını toplamak için varlık türüne göre sistem durumu ilkeleri uygulanabilir kullanır.

![Alt varlık sistem durumu toplama.][4]

Sistem durumu ilkeleri temel alan bir alt toplama.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Sistem sağlığı deposunu tüm alt değerlendirildi sonra sistem sağlığı durumlarına sağlıksız alt maksimum yapılandırılmış yüzdesini toplar. Bu yüzde varlık ve alt türüne göre ilke alınır.

* Tüm alt öğeleri Tamam durumlar varsa, toplanan alt sistem durumu normaldir.
* Alt öğe Tamam ve uyarı durumları varsa, toplanan alt sistem durumu uyarı konumunda.
* Sağlıksız alt yüzdesi izin verilen maksimum uymaz hata durumları ile alt öğe varsa, toplanan sistem durumu bir hatadır.
* Hata durumları ile alt sağlıksız alt izin verilen en yüksek yüzdesi varsa saygı, toplanan sistem durumu uyarı konumunda.

## <a name="health-reporting"></a>Sistem durumu raporlama
Sistem bileşenleri, sistem Fabric uygulamaları ve iç/dış watchdogs Service Fabric varlıklar karşı bildirebilirsiniz. Reporters olun *yerel* belirlemeleri izleme koşullara göre izlenen varlık sağlık durumu. Herhangi bir genel durum veya veri toplama sırasında Ara gerekmez. İstenen davranışı basit reporters ve göndermek için hangi bilgilerin anlamak için pek çok şeyi aramak için gereken karmaşık organizmalar olmalıdır.

Sistem Durumu verileri sistem durumu depoya göndermek için etkilenen varlık tanımlamak ve bir sistem durumu raporu oluşturmak bir Raporlayıcı gerekir. Raporu göndermeyi kullanmak [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API, rapor durumu sergilenen API'ları `Partition` veya `CodePackageActivationContext` nesneleri, PowerShell cmdlet'leri ve REST.

### <a name="health-reports"></a>Sistem durumu raporları
[Sistem durumu raporlarının](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) her küme içindeki varlıklar için aşağıdaki bilgileri içerir:

* **SourceId**. Sistem durumu olayı Raporlayıcı benzersiz olarak tanımlayan bir dize.
* **Varlık tanımlayıcısı**. Raporu nerede uygulanan varlık tanımlar. Temel alınarak farklı [varlık türü](service-fabric-health-introduction.md#health-entities-and-hierarchy):
  
  * Küme. Yok.
  * düğüm. Düğüm adı (dize).
  * Uygulama. Uygulama adı (URI). Kümede dağıtılmış uygulama örneğinin adını temsil eder.
  * Hizmet. Hizmet adı (URI). Kümede dağıtılan hizmet örneğinin adını temsil eder.
  * Bölüm. Bölüm kimliği (GUID). Bölüm benzersiz tanımlayıcıyı temsil eder.
  * Çoğaltma. Durum bilgisi olan hizmet çoğaltma kimliği veya durum bilgisiz hizmet örneği kimliği (Int64).
  * DeployedApplication. Uygulama adı (URI) ve düğüm adı (dize).
  * DeployedServicePackage. Uygulama adı (URI), düğüm adı (dize) ve hizmet adı (dize) bildirim.
* **Özellik**. A *dize* (sabit numaralandırma değil), böylece Raporlayıcı varlığın belirli bir özellik için sistem durumu olayı kategorilere ayırmak için. Örneğin, bir Raporlayıcı Node01 durumunu bildirebilirsiniz "Depo" özelliği ve Raporlayıcı B Node01 sistem durumu raporu "Bağlantı" özelliği. Health store içinde bu raporları Node01 varlık için ayrı sistem durumu olayları olarak kabul edilir.
* **Açıklama**. Sistem durumu olayı hakkında ayrıntılı bilgi sağlamak bir Raporlayıcı sağlayan bir dize. **SourceId**, **özelliği**, ve **HealthState** rapor tam olarak açıklamalıdır. Açıklama raporu hakkında okunabilir bilgi ekler. Metin, Yöneticiler ve kullanıcılar sistem durumu raporu anlamak kolaylaştırır.
* **HealthState**. Bir [numaralandırma](service-fabric-health-introduction.md#health-states) rapor sistem durumunu açıklar. Kabul edilen Tamam, uyarı ve hata değerlerdir.
* **TimeToLive**. Ne kadar süreyle sistem durumu raporu geçerli olacağını belirten bir timespan. İle birlikte **RemoveWhenExpired**, süresi dolan olayları değerlendirmek biliyorsunuz sistem durumu deposu olanak sağlar. Varsayılan değer sonsuzdur ve rapor sonsuza kadar geçerlidir.
* **RemoveWhenExpired**. Bir Boole değeri. TRUE olarak süresi dolmuş durum raporunu otomatik olarak durum deposu ve rapor kaldırılırsa varlık sistem durumu değerlendirmesi etkilemez. Raporun süresi yalnızca belirli bir süre boyunca geçerli olduğunda ve Raporlayıcı açıkça temizleyin gerekmez kullanılır. Raporları sistem durumu deposundan silmek için de kullanılır (örneğin, bir izleme değiştirilir ve raporları önceki kaynak ve özelliği ile gönderme durdurur). Sistem durumu Mağaza'dan önceki herhangi bir durum temizlemek için RemoveWhenExpired yanı sıra kısa TimeToLive sahip bir rapor gönderebilirsiniz. Değeri false olarak ayarlarsanız, süresi dolan rapor sistem durumu değerlendirmesi üzerinde hata olarak kabul edilir. False değeri, kaynak düzenli aralıklarla bu özellikte raporu olan sistem durumu Mağazası'na işaret eder. Seçili değilse, ardından olmalıdır izleme ile bir sorun. İzleme'nin sistem durumu olayı bir hata olarak dikkate alarak yakalanır.
* **SequenceNumber**. Gitgide artan olması gereken bir pozitif tamsayı raporları sırasını temsil eder. Ağ gecikmesi veya diğer sorunlar nedeniyle geç alınan eski raporları algılamak için sistem durumu mağaza tarafından kullanılır. Sıra numarası numarası aynı varlık, kaynak ve özellik için en küçük veya eşit en son uygulanan ise bir rapor reddedilir. Belirtilmezse, sıra numarası otomatik olarak oluşturulur. Durumu geçişleri bildirilirken sıra numarasına koymak gereklidir. Bu durumda, bunu gönderen hangi raporların unutmayın ve kurtarma için yük devretme hakkında bilgi korumak kaynak gerekir.

Bu dört parça bilgi--SourceId, varlık tanımlayıcısı, özellik ve HealthState--her sistem durumu raporu için gereklidir. SourceId dize öneki ile başlatmak için izin verilmiyor "**sistem**", sistem raporlar için ayrılmış. Aynı varlık için aynı kaynak ve özelliği için yalnızca bir rapor yoktur. Aynı kaynak ve özellik için birden çok rapor birbirlerini geçersiz, sistem durumu veya (bunlar toplu olarak gönderilir) sistem durumu istemci tarafında üzerindeki yan depolamak. Değiştirme sıra numaraları temel alır; Yeni raporlarla (yüksek sıra numaraları) eski raporları değiştirin.

### <a name="health-events"></a>Sistem durumu olayları
Dahili olarak, sistem sağlığı deposunu tutar [sistem durumu olayları](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent), raporlar ve ek meta veri tüm bilgileri içerir. Rapor durumu istemcinin belirtilen süre ve sunucu tarafında değiştirildiği zamanı meta verileri içerir. Sistem durumu olayları tarafından döndürülen [sistem durumu sorgularının](service-fabric-view-entities-aggregated-health.md#health-queries).

Ek meta veriler içeriyor:

* **SourceUtcTimestamp**. Saat raporun (Eşgüdümlü Evrensel Saat) sistem durumu istemciye verildi.
* **LastModifiedUtcTimestamp**. Rapor sunucu tarafında (Eşgüdümlü Evrensel Saat) son değiştirildiği zamanı.
* **IsExpired**. Sorgu durumu mağaza tarafından çalıştırıldığında rapor süresi olup olmadığını belirten bir bayrak. Yalnızca RemoveWhenExpired false ise bir olay süresi dolmuş. Aksi takdirde, olay sorgu tarafından döndürülen değil ve Mağazası'ndan kaldırıldı.
* **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. Tamam/uyarı/hata geçişleri son kez. Bu alanların durumu geçişleri olayı için sistem durumu geçmişini verin.

Durum geçişi alanları akıllı uyarıları veya "geçmiş" sistem durumu olay bilgileri için kullanılabilir. Bunlar senaryoları aşağıdaki gibi etkinleştirin:

* Bir özellik uyarı/hata için birden fazla X dakika sırasında bırakıldı olduğunda uyarır. Bir süre için koşul denetimi uyarılar geçici koşullara önler. Örneğin, sistem durumu beş dakikadan fazla uyarı, bir uyarı içine çevrilebilir (HealthState uyarı ve şimdi - LastWarningTransitionTime == > 5 dakika).
* Son değiştirilen koşullara uyarı X dakika. Bir rapor zaten hatada belirtilen süreden önce varsa, onu zaten daha önce sinyal çünkü yoksayılabilir.
* Bir özellik uyarı ve hata arasında geçiş, ne kadar süreyle sağlıksız edildikten belirleyin (diğer bir deyişle, Tamam değil). Örneğin, bir uyarı özelliği beş dakikadan daha iyi bırakılmamışsa içine çevrilebilir (HealthState! Tamam ve şimdi - LastOkTransitionTime = > 5 dakika).

## <a name="example-report-and-evaluate-application-health"></a>Örnek: Rapor etme ve uygulama sağlığını değerlendir
Aşağıdaki örnek, uygulama üzerinde PowerShell aracılığıyla bir sistem durumu raporu gönderir **fabric: / WordCount** kaynağından **MyWatchdog**. Sistem Durumu raporu sonsuz TimeToLive ile bir hata sistem durumu "kullanılabilirlik" sistem durumu özelliği hakkında bilgiler içerir. Ardından döndüren sağlık durumu hataları ve sistem durumu olayları listesinde bildirilen durum olayları toplanmış uygulama durumunu sorgular.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Sistem durumu modeli kullanımı
İzleme ve sistem durumu belirlemeleri küme içindeki farklı izleyiciler arasında dağıtıldığı için sistem durumu modeli bulut Hizmetleri ve ölçeklendirmek için temel alınan Service Fabric platformu sağlar.
Diğer sistemler, merkezi bir hizmettir tüm ayrıştırır küme düzeyinde sahip *olası* Hizmetleri tarafından gösterilen yararlı bilgiler. Bu yaklaşım, ölçeklenebilirlik yavaşlattığını. Bu da onları ve kök nedeni mümkün olduğunca yakın olarak olası sorunlarını tanımlamaya yardımcı olmak üzere belirli bilgiler toplamak izin vermez.

Sistem durumu modeli, izleme ve Tanılama, küme ve uygulama durumunu değerlendirme ve izlenen yükseltmeler için yoğun olarak kullanılır. Diğer hizmetler sistem durumu verileri belirli koşullar uyarılar vermek, küme sistem durumu geçmişini yapı ve otomatik onarım gerçekleştirmek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric sistem durumu raporlarını görüntüle](service-fabric-view-entities-aggregated-health.md)

[Sorunlarını gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Nasıl rapor ve hizmetin sistem durumunu denetle](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Özel Service Fabric sistem durumu rapor ekleme](service-fabric-report-health.md)

[İzleme ve Hizmetleri yerel olarak tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md)

