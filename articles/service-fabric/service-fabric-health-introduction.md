---
title: Service Fabric'te durum izleme | Microsoft Docs
description: Azure Service Fabric sistem durumu modeli, küme ve uygulamalar ve hizmetler izleme sağlar izleme giriş.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: chackdan
editor: ''
ms.assetid: 1d979210-b1eb-4022-be24-799fd9d8e003
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: d0ef9f34d6b657a063e50b0f144197c41905e809
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60949190"
---
# <a name="introduction-to-service-fabric-health-monitoring"></a>Service Fabric sistem durumu izlemeye giriş
Azure Service Fabric, zengin, esnek ve Genişletilebilir bir sistem durumu değerlendirme ve raporlama sağlar bir sistem durumu modeli sunar. Model, küme ve içinde çalışan hizmetleri durumunu neredeyse gerçek zamanlı izlenmesini sağlar. Kolayca sağlık bilgilerini almak ve düzeltmek yayılmadığını ve devasa kesintilerine neden önce olası sorunlar. Tipik modelinde, hizmetleri kendi yerel görünümleri temel alan raporlar gönderin ve genel bir sağlamak için bu bilgileri de toplanır küme görünümü düzeyi.

Service Fabric bileşenleri bu zengin sistem durumu modeli geçerli durumlarını bildirmek için kullanın. Uygulamalarınızı için rapor sistem durumu, mekanizma kullanabilirsiniz. Özel koşullarınızı yakalayan yüksek kaliteli sistem durumu raporlama yatırım yaparsanız, algılayın ve çalışan uygulamanız için çok daha kolay sorunları giderin.

> [!NOTE]
> Sistem durumu alt sistemini yükselten için bir gereksinimi ele Başladık. Service Fabric tam kullanılabilirlik, kapalı kalma süresi olmadan izlenen uygulama ve Küme yükseltme sağlar ve kullanıcı müdahalesi olmadan için en az. Bu hedeflere ulaşmak için yapılandırılmış yükseltme ilkelere dayalı sistem durumu yükseltme denetler. Yalnızca sistem durumu istediğiniz eşikleri uyar, yükseltmeye devam edebilirsiniz. Aksi halde, yükseltme otomatik olarak geri veya Yöneticiler sorunları gidermek için bir fırsat vermek için duraklatıldı. Uygulama yükseltmeleri hakkında daha fazla bilgi için bkz: [bu makalede](service-fabric-application-upgrade.md).
> 
> 

## <a name="health-store"></a>Sistem durumu deposu
Sistem durumu deposu kolay alma ve değerlendirme için kümedeki varlıklarla ilgili sistem durumu ile ilgili bilgileri tutar. Service Fabric durum bilgisi olan hizmet, yüksek kullanılabilirlik ve ölçeklenebilirlik sağlamak için kalıcı olarak uygulanır. Sistem durumu deposu parçasıdır **fabric: / Sistem** uygulama ve küme yukarı olduğunda kullanılabilir ve çalışır.

## <a name="health-entities-and-hierarchy"></a>Sistem durumu varlıklarını ve hiyerarşi
Sistem durumu varlıklarını etkileşimleri ve farklı varlıklar arasındaki bağımlılıkları yakalayan bir mantıksal hiyerarşideki düzenlenir. Sistem durumu deposu, sistem durumu varlıklarını ve hiyerarşi Service Fabric bileşenleri alınan raporlar göre otomatik olarak oluşturur.

Sistem durumu varlıkları, Service Fabric varlıklarıyla yansıtır. (Örneğin, **uygulama varlık sistem durumu** eşleşen bir uygulama örneği kümeye dağıtılan, while **sistem durumu düğümünde varlık** bir Service Fabric küme düğümü eşleşir.) Sistem durumu hiyerarşi sistem varlıkları etkileşimlerinin yakalar ve Gelişmiş Sistem durumu değerlendirmesi temelidir. Service Fabric temel kavramları hakkında bilgi edinebilirsiniz [Service Fabric teknik genel bakış](service-fabric-technical-overview.md). Uygulama hakkında daha fazla bilgi için bkz. [Service Fabric uygulama modelini](service-fabric-application-model.md).

Sistem durumu varlıklarını ve hiyerarşi küme ve etkili bir şekilde bildirilen, hata ayıklama, izlenen ve uygulamaları izin verir. Sistem durumu modeli bir doğru sağlar *ayrıntılı* kümedeki birçok hareketli parça durumunu temsili.

![Sistem durumu varlıkları.][1]
Sistem durumu varlıkları düzenlenmiş bir hiyerarşide üst-alt ilişkileri üzerinde temel.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Sistem durumu varlıklarını şunlardır:

* **Küme**. Service Fabric kümesi sağlığını temsil eder. Küme sistem durumu raporlarının sayısı, tüm küme etkileyen koşullar açıklanmaktadır. Bu koşullar, küme veya küme içinde birden çok varlık etkiler. Koşula bağlı olarak, bir veya daha fazla sağlıksız alt aşağı sorunu muhabir daraltamazsınız. Ağ iletişimi bölümleme veya sorunlarından bölme küme beyin örneklerindendir.
* **Düğüm**. Bir Service Fabric düğüm durumunu temsil eder. Düğüm sistem durumu raporlarının düğüm işlevselliğini etkileyen koşullar açıklanmaktadır. Bunlar genellikle üzerinde çalıştırılan dağıtılan tüm varlıkları etkiler. Örnekler düğüm disk alanı yetersiz (veya bellek, bağlantıları gibi diğer makine genelindeki özellikleri) ve bir düğüm kapalı olduğunda. Düğümün varlık, düğüm adı (dize) tarafından tanımlanır.
* **Uygulama**. Kümede çalışan bir uygulama örneği sağlığını temsil eder. Uygulama sistem durumu raporları, uygulamanın genel durumunu etkileyen koşullar açıklanmaktadır. Bunlar tek alt öğelere (hizmet veya dağıtılan uygulamalar) daraltabilir olamaz. Örnek uygulamada farklı hizmetler arasında uçtan uca etkileşim verilebilir. Uygulama varlığı, uygulama adı (URI) tarafından tanımlanır.
* **Hizmet**. Kümede çalıştırılan bir hizmeti temsil eder. Hizmet sistem durumu raporlarının genel hizmet durumunu etkileyen koşullar açıklanmaktadır. Muhabir daraltmak iyi durumda olmayan bölüm ya da çoğaltma sorunu olamaz. Tüm bölümler için sorun yaratan bir hizmet yapılandırması (örneğin, bağlantı noktası veya dış dosya paylaşımı) verilebilir. Hizmet varlığı hizmet adı (URI) tarafından tanımlanır.
* **Bölüm**. Hizmet bölüm sağlığını temsil eder. Bölüm sistem durumu raporlarının kopya kümesinin tamamı etkileyen koşullar açıklanmaktadır. Yineleme sayısı hedef sayısı olduğunda ve bir bölüm çekirdek kaybında verilebilir. Bölüm varlığın bölüm Kimliğini (GUID) tarafından tanımlanır.
* **Çoğaltma**. Bir durum bilgisi olan hizmet yineleme veya bir durum bilgisi olmayan hizmet örneği sağlığını temsil eder. Çoğaltma watchdogs ve sistem bileşenleri üzerinde bir uygulamanın bildirebileceği en küçük birimdir. Durum bilgisi olan hizmetler için işlemleri ikincil veritabanı ve yavaş çoğaltma çoğaltamazsınız bir birincil çoğaltmaya örneklerindendir. Kaynaklar yetersiz çalışıyor veya bağlantı sorunları, ayrıca, durum bilgisi olmayan bir örnek rapor edebilirsiniz. Çoğaltma varlığı bölüm Kimliğini (GUID) ve çoğaltma veya örnek kimliği (uzun) tarafından tanımlanır.
* **DeployedApplication**. Sistem durumunu gösteren bir *bir düğüm üzerinde çalışan uygulama*. Dağıtılmış uygulama sistem durumu raporlarının aynı düğümde dağıtılan hizmet paketlerini daraltabilir olamaz düğüme uygulamaya belirli koşullar açıklanmaktadır. Hatalar uygulama paketini o düğümde indirilemediğinde ve düğüm üzerindeki uygulama güvenlik ilkeleri ayarlama sorunları verilebilir. Dağıtılan bir uygulama, uygulama adı (URI) ve düğüm adı (dize) tarafından tanımlanır.
* **DeployedServicePackage**. Kümedeki bir düğüm üzerinde çalışan bir hizmet paketi sağlığını temsil eder. Bu, diğer hizmet paketleri aynı uygulama için aynı düğümde etkilemez bir hizmet paketine belirli koşullar açıklanmaktadır. Kod paketi başlatılamadı hizmet paketi ve okunamaz bir yapılandırma paketini örneklerindendir. Dağıtılan hizmet paketi, uygulama adı (URI), düğüm adı (dize), hizmet bildirim adı (dize) ve hizmet paketi etkinleştirme kimliği (dizesi) tarafından tanımlanır.

Ayrıntı düzeyini sistem durumu modeli algılayabilir ve sorunları gidermek daha kolay hale getirir. Örneğin, bir hizmeti yanıt vermiyor, Uygulama örneğinin sağlıksız olduğunu rapor mantıklı olur. Sorun bu uygulamadaki tüm hizmetleri etkileyen değil çünkü en düzeyi ancak ideal, olmadığını raporlama. Daha fazla bilgi için bu bölümü işaret ediyorsa raporun sağlıksız hizmetine veya bir özel alt bölümü uygulanmalıdır. Verileri otomatik olarak yüzeyleri hiyerarşi ve iyi durumda olmayan bir bölüm aracılığıyla yapılan görünür hizmet ve uygulama düzeylerinde. Bu toplama belirlemenize ve sorunun kök nedenini daha hızlı bir şekilde çözmek için yardımcı olur.

Sistem durumu hiyerarşinin üst-alt ilişkilerini oluşur. Küme düğümleri ve uygulamaları oluşur. Uygulama hizmetleri ve dağıtılan uygulamalar. Dağıtılan uygulamaları, hizmet paketleri dağıttım. Hizmetleri bölümleri ve her bölüm, bir veya daha fazla çoğaltma gerekir. Düğümleri ve dağıtılan varlıklar arasında özel bir ilişki yoktur. İyi durumda olmayan bir düğüm kendi yetkilisi sistem bileşeni tarafından Yük Devretme Yöneticisi hizmeti raporlandığı şekilde dağıtılan uygulamaları, hizmet paketleri ve üzerinde dağıtılan çoğaltmaları etkiler.

Sistem durumu hiyerarşi son neredeyse gerçek zamanlı bilgi en son sistem durumu raporlarının temel sistem durumunu temsil eder.
İç ve dış watchdogs uygulamaya özgü mantığı veya özel izlenen koşullara göre aynı varlıklar üzerinde rapor edebilirsiniz. Kullanıcı raporları, sistem raporlarıyla arada.

Rapor ve sağlık verilerinin büyük bir bulut hizmeti tasarım sırasında yanıt yatırım yapmaya planlayın. Bu önceden yatırım hizmet hata ayıklama, izleme ve işletin kolaylaştırır.

## <a name="health-states"></a>Sistem sağlığı durumları
Service Fabric, bir varlığın sağlıklı olup olmadığını açıklamak için üç sağlık durumundan kullanır: Tamam, uyarı ve hata. Durum deposuna gönderilen herhangi bir raporu bu durumlardan biriyle belirtmeniz gerekir. Sistem durumu değerlendirme sonucu bu durumlardan biridir.

Olası [sistem durumlarının](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) şunlardır:

* **TAMAM**. Varlık iyi durumda. Bunu veya onun alt öğelerini (uygun olduğunda) üzerinde bildirilen bilinen bir sorun vardır.
* **Uyarı**. Varlığın bazı sorunlar vardır, ancak hala onu doğru bir şekilde çalışabilir. Örneğin, gecikmeler vardır, ancak bunlar işlev sorunları henüz neden olmaz. Bazı durumlarda, uyarı koşulunu kendisini dış müdahalesi olmadan giderebilir. Bu gibi durumlarda, sistem durumu raporlarının farkındalığı artırmak ve görünürlük neler olup bittiğini sağlayın. Diğer durumlarda, uyarı koşulunu kullanıcı müdahalesi olmadan ciddi bir sorun olarak düşebilir.
* **Hata**. Varlık durumu iyi değil. Düzgün çalışamıyor çünkü eylemi varlığın durumu düzeltmek için gerçekleştirilmelidir.
* **Bilinmeyen**. Health store içinde varlık yok. Bu sonuç, birden çok bileşen sonuçları birleştirme dağıtılmış sorgularından alınabilir. Get düğüm liste sorgusu gibi gider **FailoverManager**, **ClusterManager**, ve **HealthManager**; uygulaması alma liste sorgusu gider  **ClusterManager** ve **HealthManager**. Bu sorgular, birden çok sistem bileşenlerinden sonuçları birleştirin. Health store içinde var olmayan bir varlık başka bir sistem bileşeni döndürürse, bilinmeyen birleştirme sonucu olan sistem durumu. Bir varlık deposunda çünkü sistem durumu raporlarının henüz işlenmemiştir veya silindikten sonra varlık temizlendi değil.

## <a name="health-policies"></a>Sistem durumu ilkeleri
Sistem durumu deposu raporlarının ve alt öğeleri temel alan bir varlık durumunun iyi olup olmadığını belirlemek için sistem durumu ilkeleri uygular.

> [!NOTE]
> Sistem durumu ilkeleri (için sistem durumu değerlendirmesi kümesi ve düğüm) Küme bildiriminde veya (uygulama değerlendirmesi ve tüm alt öğelerini için) uygulama bildiriminde belirtilebilir. Sistem durumu değerlendirme istekleri, yalnızca bu değerlendirme için kullanılan özel sistem durumu değerlendirme ilkelerinde de geçirebilirsiniz.
> 
> 

Varsayılan olarak, Service Fabric (her şey iyi durumda olması gerekir) katı kuralları üst-alt hiyerarşi ilişkisi için geçerlidir. Üst, alt biri bile bir sağlıksız olay varsa, kötü olarak değerlendirilir.

### <a name="cluster-health-policy"></a>Küme sistem durumu ilkesi
[Küme sistem durumu ilkesi](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) düğüm sistem sağlığı durumları ve küme sistem durumunu değerlendirmek için kullanılır. İlke küme bildiriminde tanımlanabilir. Mevcut değilse varsayılan ilkeyi (toleranslı hataları sıfır) kullanılır.
Küme sistem durumu ilkesi içerir:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Uyarı sistem durumu değerlendirilecek hata olarak değerlendirme sırasında sistem durumu raporu oluşturup oluşturmayacağını belirtir. Varsayılan: false.
* [MaxPercentUnhealthyApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Küme hata olarak kabul edilmeden önce iyi durumda olmayan uygulamalar maksimum toleranslı yüzdesini belirtir.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Küme hata olarak kabul edilmeden önce iyi durumda olmayan düğümlerin en yüksek toleranslı yüzdesini belirtir. Bu yüzdesi, tolerans yapılandırılmalıdır büyük kümelerde bazı düğümleri her zaman aşağı veya çıkış onarımı için olduğundan.
* [ApplicationTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). Uygulama türü sistem durumu ilkesi eşlem küme sistem durumu değerlendirmesi sırasında özel uygulama türlerini tanımlamak için kullanılabilir. Varsayılan olarak, tüm uygulamaların bir havuza yerleştirin ve MaxPercentUnhealthyApplications ile değerlendirilir. Bazı uygulama türleri farklı şekilde değerlendirilmesi gerektiğini, genel havuz dışına alınabilir. Bunun yerine, bunlar kendi uygulama türü adı haritadaki ilişkili yüzdeleri karşı değerlendirilir. Örneğin, bir kümede var. binlerce uygulama farklı türlerde ve özel uygulama türünün birkaç denetim uygulama örnekleri Denetim uygulamalar hiçbir zaman hata olmamalıdır. Genel MaxPercentUnhealthyApplications bazı hatalar tolerans %20, ancak "ControlApplicationType" MaxPercentUnhealthyApplications 0 olarak ayarlayın. uygulama türü için belirtebilirsiniz. Bazı çoğu uygulama iyi durumda olmayan, ancak genel sağlıksız yüzdenin altında kümeye uyarı değerlendirileceği bu şekilde. Sistem Durumu Uyarısı küme yükseltmesi etkilemez ya da hata sistem durumuna göre diğer izleme tetiklendi. Ancak, tek denetim uygulamada hata geri Tetikleyiciler veya yükseltme yapılandırmasına bağlı olarak bir küme yükseltmesi duraklatır kümenin iyi durumda olmayan, hale getirir.
  Uygulama türleri için haritada tanımlı tüm uygulama örnekleri uygulamaların Genel Havuz dışına alınır. Bunlar, uygulamaları belirli MaxPercentUnhealthyApplications eşlemesinden kullanarak uygulama türünün toplam sayısına bağlı olarak değerlendirilir. Uygulamaların tüm rest genel havuzda ve MaxPercentUnhealthyApplications ile değerlendirilir.

Aşağıdaki örnek, bir küme bildirimindeki bir alıntı. Uygulama türü haritasında girişleri tanımlamak için parametre adıyla "ApplicationTypeMaxPercentUnhealthyApplications ardından uygulama türü adı-" öneki.

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
[Uygulama durumu ilkesi](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) olayları ve durumlarını alt toplama değerlendirme'de uygulamalar ve bunların alt öğeleri için nasıl yapılacağı açıklanır. Uygulama bildiriminde tanımlanan **ApplicationManifest.xml**, uygulama paketindeki. İlke belirtilirse, Service Fabric varlık uyarı veya hata durumu bir sistem durumu raporu ya da bir alt varsa sağlıksız olduğunu varsayar.
Yapılandırılabilir İlkesi şunlardır:

* [ConsiderWarningAsError](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror). Uyarı sistem durumu değerlendirilecek hata olarak değerlendirme sırasında sistem durumu raporu oluşturup oluşturmayacağını belirtir. Varsayılan: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Uygulama hata olarak kabul edilmeden önce iyi durumda olmayan dağıtılmış uygulamalar maksimum toleranslı yüzdesini belirtir. Bu yüzdesi, iyi durumda olmayan dağıtılmış uygulamalar uygulamaları şu anda kümede dağıtılan sanal düğüm sayısı üzerinden bölünmesiyle hesaplanır. Az sayıda düğüm üzerinde bir hatasını tolere için hesaplama yukarı yuvarlar. Varsayılan yüzde: sıfır.
* [DefaultServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Uygulamadaki tüm hizmet türleri için varsayılan sistem durumu ilkesi değiştirir varsayılan hizmet türü sistem durumu ilkesi belirtir.
* [ServiceTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Hizmet türü başına hizmet sistem durumu ilkeleri Haritası sağlar. Bu ilkeler, her belirtilen hizmet türü için varsayılan hizmet türü sistem durumu ilkeleri değiştirin. Örneğin, bir uygulamanın durum bilgisi olmayan bir ağ geçidi hizmet türü ve durum bilgisi olan altyapısı hizmet türü varsa, sistem durumu ilkeleri, değerlendirme için farklı şekilde yapılandırabilirsiniz. İlke başına hizmet türü belirttiğinizde, hizmetinin sistem durumunu daha ayrıntılı denetim elde edebilirsiniz.

### <a name="service-type-health-policy"></a>Hizmet türü sistem durumu ilkesi
[Hizmet türü sistem durumu ilkesi](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) değerlendirmek ve Hizmetleri ve Hizmetleri alt toplamak nasıl belirtir. İlke içerir:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Bir hizmet sistem durumu kötü olarak kabul edilmeden önce iyi durumda olmayan bölümler toleranslı maksimum yüzdesini belirtir. Varsayılan yüzde: sıfır.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Bir bölüm sağlıksız olarak kabul edilmeden önce iyi durumda olmayan çoğaltmalar toleranslı maksimum yüzdesini belirtir. Varsayılan yüzde: sıfır.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Uygulama sistem durumu kötü olarak kabul edilmeden önce iyi durumda olmayan hizmetler toleranslı maksimum yüzdesini belirtir. Varsayılan yüzde: sıfır.

Aşağıdaki örnek bir uygulama bildirimindeki bir alıntı:

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
Kullanıcılar ve otomatik hizmetler için herhangi bir varlık health herhangi bir zamanda değerlendirebilirsiniz. Bir varlığın sistem durumunu değerlendirmek için sistem durumu deposu toplamalar tüm sistem durumu raporları varlık üzerinde ve tüm alt öğelerini (uygun olduğunda) değerlendirir. Sistem durumu toplama algoritması, sistem durumu raporlarının değerlendirmek ve (varsa) alt sistem durumlarını Topla belirtin sistem durumu ilkeleri kullanır.

### <a name="health-report-aggregation"></a>Sistem durumu rapor toplama
Bir varlığın birden çok sistem durumu raporlarının farklı özellikleri farklı raporlayıcıları (sistem bileşenleri veya watchdogs) tarafından gönderilen olabilir. Toplama ilişkili sistem durumu ilkeleri, uygulama veya küme sistem durumu ilkesi ConsiderWarningAsError üyesi özellikle kullanır. ConsiderWarningAsError uyarıları değerlendirmek nasıl belirtir.

Toplanmış sistem durumu tarafından tetiklenen *kötü* durumu raporları varlık üzerinde. En az bir hata sistem durumu raporu varsa, toplanan sistem durumu bir hatadır.

![Hata raporu rapor toplama sistem durumu.][2]

Bir veya daha fazla hata sistem durumu raporlarının olan bir sistem durumu varlık hata olarak değerlendirilir. Sistem sağlığı durumuna bakılmaksızın bir süresi dolan sistem durumu raporu aynı geçerlidir.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Hiçbir hata raporlarını ve bir veya daha fazla uyarı olduğunda toplanan sistem durumu uyarı ya da ConsiderWarningAsError ilke bayrağı bağlı olarak bir hata olur.

![Sistem durumu rapor toplama uyarı rapor ve ConsiderWarningAsError false.][3]

Sistem durumu rapor toplama uyarı rapor ve ConsiderWarningAsError (varsayılan) false olarak ayarlayın.

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Alt sistem durumu toplama
Toplanmış sistem durumu, bir varlığın alt sistem durumlarını (uygun olduğunda) yansıtır. Alt sistem durumlarının toplama algoritması varlık türüne göre sistem durumu geçerli olan ilkeler kullanır.

![Alt varlıklar sistem durumu toplama.][4]

Sistem durumu ilkelerine bağlı alt toplama.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Tüm alt sistem durumu deposu değerlendirdikten sonra sistem durumlarını sağlıksız alt maksimum yapılandırılmış yüzdesini toplar. Bu yüzdesi, varlık ve alt türüne göre ilkeden alınır.

* Tüm alt Tamam durumları varsa, toplanmış alt sistem durumu uygun.
* Alt öğeleri Tamam hem uyarı durumları varsa, toplanmış alt sistem durumu uyarı konumunda.
* Alt sağlıksız alt yüzdesi izin verilen maksimum uymaz hata durumları ile varsa, toplanan üst sistem durumu bir hatadır.
* Alt hata durumları ile sağlıksız alt izin verilen en yüksek yüzdesi, saygı, toplanan üst sistem durumu uyarı konumunda.

## <a name="health-reporting"></a>Sistem durumu raporlama
Service Fabric varlıklarının sistem bileşenleri, sistem yapı uygulamaları ve dahili/harici watchdogs bildirebilirsiniz. Raporlayıcıları olun *yerel* belirlemeleri izlemekte olduğunuz koşullara göre izlenen varlık sistem durumu. Herhangi bir genel durumu veya veri toplama sırasında Ara gerekmez. İstenen davranışı basit raporlayıcıları ve göndermek için hangi bilgilerin çıkarsamak için birçok şeyi aramak için gereken karmaşık organizma sağlamaktır.

Sistem Durumu verileri sistem durumu depoya göndermek için etkilenen varlık sistem durumu raporu oluşturmak üzere bir Raporlayıcı gerekir. Raporu göndermeyi kullanmak [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API, API üzerinde sunulan rapor durumu `Partition` veya `CodePackageActivationContext` nesneleri, PowerShell cmdlet'leri ve REST.

### <a name="health-reports"></a>Sistem durumu raporlarının sayısı
[Sistem durumu raporlarının](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) kümesindeki varlıkların her biri için aşağıdaki bilgileri içerir:

* **SourceId**. Sistem durumu olayının muhabir benzersiz olarak tanımlayan bir dize.
* **Varlık tanımlayıcısı**. Rapor uygulandığı varlığı tanımlar. Bağlı olarak farklı [varlık türü](service-fabric-health-introduction.md#health-entities-and-hierarchy):
  
  * Küme. Yok.
  * Düğüm. Düğüm adı (dize).
  * uygulama. Uygulama adı (URI). Kümeye dağıtılan uygulama örneğinin adını temsil eder.
  * Hizmeti. Hizmet adı (URI). Kümede dağıtılan hizmet örneğinin adını temsil eder.
  * Bölüm. Bölüm kimliği (GUID). Bölüm benzersiz tanımlayıcısını temsil eder.
  * Çoğaltma. Durum bilgisi olan hizmet çoğaltma kimliği ya da durum bilgisi olmayan hizmet örneği kimliği (Int64).
  * DeployedApplication. Uygulama adı (URI) ve düğüm adı (dize).
  * DeployedServicePackage. Uygulama adı (URI), düğüm adı (dize) ve hizmet bildirim adı (dize).
* **Özellik**. A *dize* (sabit bir numaralandırma değil) olanak tanıyan muhabir varlığın belirli bir özellik için sistem durumu olayı kategorilere ayırmak için. Örneğin, bir Raporlayıcı Node01 durumunu raporlayabilirsiniz "Alanı" özelliği ve muhabir B Node01 durumunu rapor edebilir "Bağlantı" özelliği. Health store içinde bu raporları Node01 varlık için ayrı bir sistem durumu olayları olarak kabul edilir.
* **Açıklama**. Sistem durumu olayı hakkında ayrıntılı bilgi sağlamak bir Raporlayıcı izin veren bir dize. **SourceId**, **özelliği**, ve **HealthState** raporun tam olarak açıklamalıdır. Açıklama raporu okunabilir bilgileri ekler. Metin, sistem durumu raporu anlamak, Yöneticiler ve kullanıcılar için kolaylaştırır.
* **HealthState**. Bir [numaralandırma](service-fabric-health-introduction.md#health-states) , raporun sistem durumunu açıklar. Kabul edilen değerler Tamam, uyarı ve hata var.
* **TimeToLive**. Ne kadar süreyle sistem durumu raporu geçerli olacağını belirten bir timespan. İle birlikte **RemoveWhenExpired**, süresi dolan olaylar değerlendirilecek biliyorsunuz sistem durumu deposu sağlar. Varsayılan olarak, sonsuz bir değerdir ve rapor sonsuza kadar geçerlidir.
* **RemoveWhenExpired**. Bir Boole değeri. Süresi dolan sistem durumu raporu true olarak sistem durumu deposu ve rapor otomatik olarak kaldırılması durumunda, varlık sistem durumu değerlendirmesi etkisi yoktur. Rapor zaman yalnızca belirli bir dönem için geçerlidir ve muhabir açıkça Temizle gerekmez olduğunda kullanılır. Ayrıca health Store'dan raporları silmek için kullanılır (örneğin, bir izleme değiştirilir ve önceki kaynağı ve özelliği içeren raporlar göndermeyi durdurur). Bu kısa bir TimeToLive health Store'dan herhangi bir önceki durumu sıfırlamaya RemoveWhenExpired birlikte içeren bir rapor gönderebilirsiniz. Değer false olarak ayarlanırsa, raporun süresi dolan sistem durumu değerlendirme üzerinde hata olarak kabul edilir. False değeri sistem durumu deposu için kaynak tuto vlastnost nelze upravovat düzenli olarak raporlamalıdır bildirir. Seçili değilse, ardından olmalıdır bir bekçi ile sorun. İzleme'nın sistem durumu olayı hata olarak dikkate alarak yakalanır.
* **SequenceNumber**. Sürekli artan olması gereken bir pozitif tamsayı raporları sırasını temsil eder. Sistem durumu deposu tarafından ağ gecikmeleri veya diğer sorunlar nedeniyle geç alınan eski raporlar algılamak için kullanılır. Sıra numarası aynı varlık, kaynak ve özelliği için sayı en küçük veya eşit en son uygulanan ise bir rapor reddedilir. Belirtilmezse, sıra numarası otomatik olarak oluşturulur. Durum geçişlerini üzerinde bildirirken sıra numarasına koymak gereklidir. Bu durumda, kaynak gönderilen hangi raporların unutmayın ve yük devretme kurtarma bilgileri tutmak gerekir.

Bu dört parçaları bilgi--SourceId, varlığı tanımlayıcısı, özellik ve HealthState--her sistem durumu raporu için gereklidir. SourceId dize ön ekine sahip izin verilmiyor "**sistem**", sistem raporlar için ayrılmıştır. Aynı varlık için aynı kaynak ve özelliği için yalnızca bir rapor yoktur. Aynı kaynak ve özellik için birden çok rapor birbirini geçersiz kılma, (bunlar toplu varsa) sistem durumu istemci tarafında veya sistem tarafı depolayın. Değişiklik, sıra numaraları üzerinde temel alır. eski raporlar yeni raporlar (yüksek sıra numaraları ile) değiştirin.

### <a name="health-events"></a>Sistem durumu olayları
Dahili olarak, sistem durumu deposu tutar [sistem durumu olaylarını](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent), raporlar ve ek meta veri gelen tüm bilgileri içerir. Rapor sistem durumu istemciye verilen zaman ve sunucu tarafında değiştirildiği zamanı meta verileri içerir. Sistem durumu olayları tarafından döndürülen [sistem durumu sorgularının](service-fabric-view-entities-aggregated-health.md#health-queries).

Ek meta verileri içerir:

* **SourceUtcTimestamp**. Saat raporun durumu istemciye (Eşgüdümlü Evrensel Saat) verildi.
* **LastModifiedUtcTimestamp**. Rapor sunucu tarafında (Eşgüdümlü Evrensel Saat) son değiştirildiği zamanı.
* **IsExpired**. Sorgu sistem durumu deposu tarafından yürütüldüğünde raporun süresi olup olmadığını belirten bir bayrak. Yalnızca RemoveWhenExpired false ise bir olay süresi dolmuş. Aksi takdirde, olay, sorgu tarafından döndürülen değil ve Mağazası'ndan kaldırılır.
* **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. Tamam/uyarı/hata geçişleri son kez. Bu alanlar, olayın durumu geçişleri sistem durumu geçmişini verin.

Durum geçişi alanlarının daha akıllı uyarıları veya "geçmiş" sistem durumu olay bilgilerini için kullanılabilir. Bunlar gibi senaryoları etkinleştir:

* En fazla dakika X uyarı/hata için bir özellik olduğunda uyarır. Bir süre için koşulu kontrol etmeyi geçici koşullar ile ilgili uyarılar önler. Örneğin, beş dakikadan fazla uyarı sistem durumu, bir uyarı içine çevrilebilir (HealthState uyarı ve LastWarningTransitionTime'artık - > 5 dakika ==).
* Uyarı son değişen koşullara X dakika. Bir rapor zaten belirtilen süreden önce hata oluştu, bunu zaten daha önce sinyal çünkü yoksayılabilir.
* Bir özellik, hata ve uyarı arasında geçiş, ne kadar süreyle kötü geçtiğine belirleyin (diğer bir deyişle, Tamam değil). Örneğin, bir uyarı özelliği beş dakikadan fazla sağlıklı bırakılmamışsa içine çevrilebilir (HealthState! Tamam ve şimdi - LastOkTransitionTime = > 5 dakika).

## <a name="example-report-and-evaluate-application-health"></a>Örnek: Rapor ve uygulama sağlığını değerlendir
Aşağıdaki örnek, PowerShell aracılığıyla uygulama üzerinde bir sistem durumu raporu gönderir **fabric: / WordCount** kaynağından **MyWatchdog**. Sistem Durumu raporu, sistem durumu özelliği "kullanılabilirlik" sonsuz TimeToLive ile bir hata sistem durumu hakkında bilgi içerir. Ardından uygulama durumunun sistem durumu hataları ve sistem durumu olaylarını listesinde bildirilen sistem durumu olaylarını döndüren toplu sorgular.

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

## <a name="health-model-usage"></a>Sistem durumu modeli kullanım
İzleme ve sistem durumu belirlemeleri kümedeki farklı izleyiciler arasında dağıtıldığı için sistem durumu modeli, bulut Hizmetleri ve ölçeklendirmek için temel alınan Service Fabric platformu sağlar.
Diğer sistemlerimiz tüm ayrıştırır küme düzeyinde tek, merkezi bir hizmet *potansiyel olarak* Hizmetleri tarafından yayılan yararlı bilgiler. Bu yaklaşım, ölçeklenebilirlik yavaşlattığını. Bu da onları sorunları ve kök nedenin mümkün olduğunca yakın olarak olası sorunları belirlemenize yardımcı olması için belirli bilgiler toplamak izin vermez.

Sistem durumu modeli, izleme ve Tanılama, küme ve uygulama durumunu değerlendirmek için ve yükselten yoğun olarak kullanılır. Diğer hizmetleri, otomatik onarımlar yapmak için küme sistem durumu geçmişi oluşturup belirli koşullar ile ilgili uyarılar verecek sistem durumu verileri kullanın.

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric sistem durumu raporlarını görüntüleme](service-fabric-view-entities-aggregated-health.md)

[Sorunlarını gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Özel Service Fabric durum raporları ekleme](service-fabric-report-health.md)

[Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md)

