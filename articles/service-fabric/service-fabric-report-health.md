---
title: "Özel Service Fabric sistem durumu rapor ekleme | Microsoft Docs"
description: "Azure Service Fabric sistem durumu varlıkları için özel durum raporu göndermeyi açıklar. Tasarlama ve kalite sistem durumu raporlarının uygulamak için öneriler sağlar."
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: 
ms.assetid: 0a00a7d2-510e-47d0-8aa8-24c851ea847f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: 1cd429ed8252573f8e8c3ed11d6c841cba855b52
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="add-custom-service-fabric-health-reports"></a>Özel Service Fabric durum raporları ekleme
Azure Service Fabric tanıtır bir [sistem durumu modeli](service-fabric-health-introduction.md) sağlıksız küme ve belirli varlıkları uygulama koşullara bayrak için tasarlanmıştır. Sistem durumu modeli kullanan **sistem durumu reporters** (sistem bileşenleri ve watchdogs). , Kolay ve hızlı tanı ve onarım hedeftir. Hizmeti yazıcıları sistem durumu hakkında ön düşünmeniz gerekir. Özellikle, kök yakın bayrağı sorunları yardımcı olur, sistem durumu etkileyebilir herhangi bir koşul, bildirilmelidir. Sistem durumu bilgileri, hata ayıklama ve araştırma zaman ve çaba kaydedebilirsiniz. Hizmet bulutta ölçekte da çalışır durumda bir kez yararlılığı özellikle Temizle (özel veya Azure).

Service Fabric reporters İzleyicisi'ni faiz koşullarını tanımlanır. Kendi yerel bir görünümü temel alarak bu koşullara bildirin. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) varlıklar genel sağlıklı olup olmadığını belirlemek için tüm reporters tarafından gönderilen sistem durumu verileri toplar. Model, zengin, esnek ve kullanımı kolay olacak şekilde tasarlanmıştır. Sistem durumu raporlarının kalitesini küme sistem durumu görünümünü doğruluğunu belirler. Yanlış sağlıksız sorunları göstermek hatalı pozitif sonuç, yükseltme veya sistem durumu verileri kullanan diğer hizmetler olumsuz yönde etkileyebilir. Bu tür hizmetlerin onarım hizmetleri ve uyarı mekanizmaları gösterilebilir. Bu nedenle, bazı düşünce olabilecek en iyi şekilde ilgi koşullarını yakalama raporları sağlamak için gereklidir.

Tasarlama ve sistem durumu raporlama, uygulama watchdogs ve sistem bileşenleri gerekir:

* İlginizi koşulu, izlenip yolu ve etkisi küme veya uygulamanın işlevselliğini tanımlayın. Bu bilgilere dayanarak, sistem durumu ve sistem durumu raporu özelliği üzerinde karar verin.
* Belirlemek [varlık](service-fabric-health-introduction.md#health-entities-and-hierarchy) rapor için geçerli.
* Raporlama olduğu belirlemek Bitti'yi, gelen hizmet içinde veya bir iç veya dış izleme.
* Raporlayıcı tanımlamak için kullanılan bir kaynak tanımlayın.
* Bir raporlama stratejisi geçişleri veya düzenli aralıklarla seçin. Daha basit kod gerektirir ve daha az hatalara önerilen yöntem, düzenli aralıklarla aynıdır.
* Ne kadar rapor sağlıksız koşullar için sistem durumu deposunda da kalmalı ve nasıl temizlenmelidir belirler. Bu bilgileri kullanarak, raporun süresi ve kaldırma üzerinde sona erme davranışını karar verin.

Belirtildiği gibi raporlama den yapılabilir:

* İzlenen Service Fabric hizmeti çoğaltma.
* İç watchdogs Service Fabric hizmeti (koşullar izler ve raporları sorunları Örneğin, bir Service Fabric durum bilgisiz hizmet) dağıtılmış. Watchdogs olabilir tüm düğümlere dağıtılan veya izlenen hizmete affinitized.
* Service Fabric düğümlerinde çalıştırın, ancak olan iç watchdogs *değil* Service Fabric Hizmetleri olarak uygulanır.
* Kaynak araştırma dış watchdogs *dışında* Service Fabric kümesi (örneğin, izleme hizmeti Gomez gibi).

> [!NOTE]
> Kutudan çıktığında, küme sistem bileşenleri tarafından gönderilen durum raporları ile doldurulur. Ayrıntılı bilgi için [kullanarak sistem durumu raporları sorun giderme için](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). Kullanıcı raporları gönderilmelidir [durumu varlıklarını](service-fabric-health-introduction.md#health-entities-and-hierarchy) , önceden oluşturulmuş sistem tarafından.
> 
> 

Bir kez tasarım işaretlenmemiştir raporlama sistem, sistem durumu raporları kolayca gönderilebilir. Kullanabileceğiniz [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) küme değilse rapor sağlığı [güvenli](service-fabric-cluster-security.md) veya doku istemci yönetim ayrıcalıkları varsa. Raporlama yapılabilir tarafından API aracılığıyla kullanarak [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth), PowerShell veya REST aracılığıyla. Yapılandırma düğmelerini performansı için raporları toplu.

> [!NOTE]
> Rapor durumu zaman uyumlu ve yalnızca istemci tarafında doğrulama işi temsil eder. Raporun sistem istemci tarafından kabul edilen olgu veya `Partition` veya `CodePackageActivationContext` nesneleri değil anlamına deposunda uygulanır. Zaman uyumsuz olarak gönderilir ve büyük olasılıkla diğer raporlarla toplu. Sunucuda işlenmesi başarısız olabilir: sıra numarası eski olabilir, raporu uygulanmalıdır varlık silindi, vb.
> 
> 

## <a name="health-client"></a>İstemci sistem durumu
Sistem durumu raporlarının doku istemci içinde bulunduğu bir sistem durumu istemci sistem durumu mağazayla gönderilir. Sistem durumu istemcinin aşağıdaki ayarlara sahip yapılandırılabilir:

* **HealthReportSendInterval**: Raporun istemciye eklenen süre sistem durumu depoya gönderilir zaman arasında gecikme. Toplu raporları için bir ileti gönderirken yerine tek bir ileti her rapor için kullanılan. Toplu işleme performansı artırır. Varsayılan: 30 saniyedir.
* **HealthReportRetrySendInterval**: hangi sistem istemci yeniden gönderir birikmiş sistem durumu aralığı sistem durumu depoya raporlar. Varsayılan: 30 saniyedir.
* **HealthOperationTimeout**: sistem durumu mağazaya gönderilen bir raporu iletisi için zaman aşımı süresi. Bir ileti zaman aşımına uğrarsa, rapor işlenmiş olan sistem durumu deposu onaylayıncaya kadar sistem durumu istemci bunu yeniden dener. Varsayılan: iki dakika.

> [!NOTE]
> Raporları toplu hale, doku istemci en az canlı için tutulmalıdır HealthReportSendInterval gönderildiğinden emin olun. İleti kaybolur veya sistem durumu deposu geçici hataları nedeniyle uygulanamıyor, doku istemci artık canlı tutulmalıdır yeniden denemek için bir fırsat vermek için.
> 
> 

İstemcide arabelleğe alma raporları benzersizlik dikkate alır. Örneğin, belirli bir hatalı Raporlayıcı raporlama aynı varlık aynı özellikte saniyede 100 raporları, rapor en son sürümü ile değiştirilir. En çok bir tür rapor istemci sıraya yok. Toplu işleme yapılandırdıysanız sistem durumu depoya gönderilen rapor sayısı yalnızca gönderme aralığı başına biridir. Bu rapor, varlık en geçerli durumunu yansıtır son eklenen rapor eder.
Yapılandırma parametrelerini belirtin zaman `FabricClient` geçirerek oluşturulan [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) sistem durumu ile ilgili girdileri istenen değerlere sahip.

Aşağıdaki örnek, bir doku istemci oluşturur ve eklendiklerinde raporları gönderilmesi gerektiğini belirtir. Zaman aşımları ve yeniden denenebilir hata, yeniden deneme 40 saniyede gerçekleşir.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Varsayılan doku istemci ayarlamak ayarları tutma öneririz `HealthReportSendInterval` 30 saniye. Bu ayar, toplu işleme nedeniyle en iyi performans sağlar. Mümkün olan en kısa sürede gönderilmesi gereken kritik raporlar için `HealthReportSendOptions` hemen ile `true` içinde [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API. Hemen raporları toplu aralığı atlama. Bu bayrak dikkatli kullanın; mümkün olduğunda toplu işleme sistem istemci yararlanmak istiyoruz. Hemen gönder yararlıdır de doku istemci kapatırken (örneğin, işlem geçersiz durum belirledi ve yan etkileri önlemek için kapanması gerekiyor). En yüksek çaba gönderme birikmiş raporlar sağlar. Bir rapor anlık bayrağı ile eklendiğinde, sistem durumu istemci birikmiş tüm raporları itibaren son gönderme toplu işlemleri.

PowerShell aracılığıyla bir kümeye bir bağlantı oluşturulduğunda, aynı parametreleri belirtilebilir. Aşağıdaki örnek, yerel bir küme için bir bağlantı başlatır:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

Benzer şekilde API için raporları kullanma gönderilebilir `-Immediate` hemen gönderilen, ne olursa olsun, olmasını anahtar `HealthReportSendInterval` değeri.

KALAN için bir iç doku istemci sahip Service Fabric geçidine raporlar gönderilir. Varsayılan olarak, bu istemci raporları göndermek için yapılandırılmış her 30 saniyede toplu. Küme yapılandırmasını Ayarla toplu aralığını değiştirebilirsiniz `HttpGatewayHealthReportSendInterval` üzerinde `HttpGateway`. Belirtildiği gibi daha iyi bir seçenek raporlarla göndermektir `Immediate` true. 

> [!NOTE]
> Yetkisiz Hizmetleri sistem durumu kümeyi varlıklarda karşı raporlayamaz emin olmak için sunucuyu yalnızca güvenli istemcilerinden gelen istekleri kabul edecek şekilde yapılandırın. `FabricClient` Gerekir sahip güvenlik etkin küme (örneğin, Kerberos veya sertifika kimlik doğrulaması ile) ile iletişim kurabilmesi için raporlama için kullanılır. Daha fazla bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Gelen düşük ayrıcalıklı Hizmetleri içinde rapor
Service Fabric hizmetleri kümesine yönetici erişimi yoksa, geçerli bağlamdan varlıklar üzerinde sistem durumu bildirebilirsiniz `Partition` veya `CodePackageActivationContext`.

* Durum bilgisi olmayan hizmetler için kullanmak [IStatelessServicePartition.ReportInstanceHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) geçerli hizmet örneği üzerinde raporlamak için.
* Durum bilgisi olan hizmetler için kullanmak [IStatefulServicePartition.ReportReplicaHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) geçerli kopyada bildirilecek.
* Kullanım [IServicePartition.ReportPartitionHealth](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) geçerli bölüm varlıkta bildirilecek.
* Kullanım [CodePackageActivationContext.ReportApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) geçerli uygulamasında bildirilecek.
* Kullanım [CodePackageActivationContext.ReportDeployedApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) geçerli düğümde dağıtılan geçerli uygulama hakkında rapor oluşturmak için.
* Kullanım [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) geçerli düğüme dağıtılmış uygulama için bir hizmet paketi hakkında rapor oluşturmak için.

> [!NOTE]
> Dahili olarak, `Partition` ve `CodePackageActivationContext` varsayılan ayarlarla yapılandırılan bir sistem durumu istemcisi tutun. İçin açıklandığı gibi [sistem istemci](service-fabric-report-health.md#health-client), raporları toplu hale ve bir süreölçer gönderilir. Nesneleri raporu gönderme olanağı sağlamak Canlı tutulmalıdır.
> 
> 

Belirleyebileceğiniz `HealthReportSendOptions` raporlarda gönderirken `Partition` ve `CodePackageActivationContext` sistem durumu API'leri. Mümkün olan en kısa sürede gönderilmesi gereken kritik raporlarınız varsa, kullanmak `HealthReportSendOptions` hemen ile `true`. Hemen raporları iç sistem istemci toplu aralığını atlama. Önceden belirtildiği gibi bu bayrak dikkatli kullanın; mümkün olduğunda toplu işleme sistem istemci yararlanmak istiyoruz.

## <a name="design-health-reporting"></a>Sistem durumu raporlama tasarlama
Yüksek kaliteli raporları üretme ilk adımı hizmeti etkileyebilir koşulları tanımlamak için kullanılır. Başlatıldığında--veya hatta daha iyi bir sorun gerçekleşmeden önce--olası olabilir, hizmet veya küme bayrağı sorunlarını yardımcı olabilecek herhangi bir koşul ABD Doları milyarlarca kaydedin. Kesinti daha az yararları, daha az gece saat araştırma ve sorunları ve daha yüksek müşteri memnuniyetini onarma harcanan.

Koşullar tanımlandıktan sonra bunları ek yükü ve yararlılığını arasında bir denge izlemek için en iyi yolu ekleneceğini izleme yazıcılarının gerekir. Örneğin, bazı geçici dosyaları paylaşımındaki kullanmak karmaşık hesaplamalar yapan bir hizmet göz önünde bulundurun. Bir izleme yeterli boş alan kullanılabilir olduğundan emin olmak için paylaşımı izleyebilir. Dosya veya dizin değişiklik bildirimleri için dinleme. Bir ön eşik ulaşıldığında ve Paylaşım doluysa bir hata raporu bir uyarı rapor edilemedi. Bir uyarı üzerinde bir onarım sistemi eski dosya paylaşımındaki temizleniyor başlayamadı. Bir hata üzerinde onarım sistem hizmeti çoğaltma başka bir düğüme taşıyabilirsiniz. Koşul durumlarını bakımından sistem durumu nasıl açıklanan dikkat edin: (Tamam) sağlıklı kabul edilebilir koşul veya sağlıksız (uyarı veya hata) durumu.

İzleme Ayrıntıları ayarladıktan sonra izleme uygulamak nasıl bulmak izleme yazıcısı gerekir. Koşullar hizmet içinde belirlenebilir, izleme izlenen hizmetin parçası olabilir. Örneğin, hizmet kod paylaşım kullanımını denetleyin ve sonra dosya yazmaya çalıştığı her zaman rapor. Bu yaklaşımın avantajı raporlama kolay olmasıdır. Hizmeti işlevselliğini etkileyen izleme hataları önlemek için dikkatli olunması gerekir.

İçinden raporlama izlenen hizmetin her zaman bir seçenek değil. İzleme hizmeti içinde koşulları algılamak mümkün olmayabilir. Mantığı ya da belirlenmesi için veri olmayabilir. Koşullar izleme yükünü yüksek olabilir. Koşullar ayrıca bir hizmet için belirli olmayabilir ancak bunun yerine Hizmetleri arasındaki etkileşimler etkiler. Watchdogs kümedeki ayrı işlemleri olarak sahip başka bir seçenektir. Watchdogs koşullar ve rapor, herhangi bir şekilde ana Hizmetleri etkilemeden izleyin. Örneğin, bu watchdogs aynı uygulamadaki tüm düğümlerde veya hizmet ile aynı düğümlerde dağıtılan durum bilgisi olmayan hizmetler olarak uygulanabilir.

Bazı durumlarda, kümede çalışan bir izleme seçeneği ya da değil. Kullanıcıların gördüğünüz izlenen koşul kullanılabilirlik veya hizmetin işlevlerini ise, kullanıcı istemcileri ile aynı yerde watchdogs sahip en iyisidir. Burada, bunlar operations kullanıcıların onları çağıran aynı şekilde test edebilirsiniz. Örneğin, küme dışında yaşar, istekleri için hizmet veren ve gecikme süresi ve sonucu doğruluğunu denetler bir izleme olabilir. (Hesaplayıcı hizmeti için örneğin, 2 + 2 4 makul bir sürede return mu?)

İzleme Ayrıntıları sonlandırıldıktan sonra benzersiz olarak tanımlayan bir kaynak kimliği karar vermeniz gerekir. Farklı varlıklar üzerinde rapor gerekir kümedeki yaşayan aynı türde birden fazla watchdogs veya aynı varlık üzerinde rapor, farklı bir kaynak kimliği veya özelliğini kullanın. Bu şekilde raporlarının bulunabilir. Sistem Durumu raporu özellik izlenen durum yakalama. (Yukarıdaki örnekte özelliği olabilir **ShareSize**.) Birden çok rapor aynı durumuna uygularsanız, özelliği bir arada raporları sağlayan bazı dinamik bilgileri içermelidir. Örneğin, birden çok paylaşımı izlenmesi gereken, özellik adı olabilir **ShareSize sharename**.

> [!NOTE]
> Yapmak *değil* durum bilgilerini tutmak için sistem sağlığı deposunu kullanın. Bu bilgiler bir varlık sistem durumu değerlendirmesi etkiler olarak yalnızca sistem durumu ile ilgili bilgileri sistem durumu olarak bildirilmesi. Sistem sağlığı deposunu genel amaçlı bir depolama alanı olarak tasarlanmamıştır. Sistem durumu tüm verileri toplamak için sistem durumu değerlendirme mantığını kullanır. Toplanan sistem durumu bilgilerini (örneğin, bir sistem durumu Tamam durumuyla raporlama) sistem durumu ilişkisiz gönderme etkilemez, ancak sistem durumu depolama performansını olumsuz yönde etkileyebilir.
> 
> 

Sonraki karar noktası rapor varlığa açıktır. Çoğu zaman, koşul açıkça idetifies varlık. Varlık ile olası en iyi ayrıntı düzeyi seçin. Bir koşul bir bölümdeki tüm çoğaltmaları etkiliyorsa, hizmet üzerinde bölüm raporlama. Burada daha fazla düşünce, ancak gerekli köşe durumlar vardır. Bir çoğaltma gibi bir varlık koşul etkiler, ancak isteğini sağlamaktır birden çok çoğaltma yaşam süresini koşul bayrağı, sonra bölüme bildirilmesi. Aksi takdirde, çoğaltma silindiğinde, sistem sağlığı deposunu, tüm raporları siler. İzleme yazıcılarının varlık ve rapor ömürleri hakkında düşünmeniz gerekir. Mağaza (örneğin, bir varlık üzerinde bildirdiği bir hata artık uygulanmadığında) bir rapor temizlenmelidir zaman açık olması gerekir.

Birlikte ı açıklanan noktaları koyan bir örneğe bakalım. Bir durum bilgisi olan kalıcı hizmet ana ve tüm düğümleri (görevinin her türü için bir ikincil hizmet türü) dağıtılan ikincil durum bilgisi olmayan hizmetler Service Fabric uygulaması oluşan göz önünde bulundurun. Ana ikincil öğe tarafından yürütülecek komutları içeren bir işleme sırası vardır. İkincil kopya gelen isteklerin yürütün ve geri bildirim sinyalleri gönderin. İzlenen bir ana işleme sırası uzunluğu durumdur. Ana kuyruk uzunluğu eşiği ulaşırsa, bir uyarı bildirilir. Uyarı ikinciller yük işleyemiyor gösterir. Sıranın en fazla uzunluk ulaşana ve komutları gösterilmez, hizmet kurtaramazsınız gibi bir hata bildirdi. Raporları, özellikte olabilir **QueueStatus**. İçinde hizmet izleme olaylarını yaşar ve ana birincil Çoğaltmada düzenli aralıklarla gönderilir. Etkin kalma süresi iki dakikadır ve her 30 saniyede düzenli aralıklarla gönderilir. Birincil devre dışı kalırsa, raporun Mağazası'ndan otomatik olarak temizlenir. Hizmet çoğaltma yukarı ise, ancak, karşılıklı veya diğer konusunda sorun, raporun health store içinde süresi dolar. Bu durumda, varlık hatasında değerlendirilir.

İzlenebilir başka bir koşul görev yürütme süresi ' dir. Asıl görev türüne göre ikinciller görevlere dağıtır. Tasarım bağlı olarak, ana ikinciller görev durumu için yoklama. Aynı zamanda bittiğinde, bunlar geri bildirim sinyalleri göndermek ikinciller için bekleyin. İkinci durumda, ikincil kopya dökme veya iletileri kayıp olduğu durumlarda algılamak için dikkatli olunması gerekir. Durumu geri gönderen bir ping isteği aynı ikincil göndermek asıl için bir seçenektir. Durum yok alınmazsa, asıl hata göz önünde bulundurur ve görev reschedules. Bu davranış, görevleri ıdempotent olduğunu varsayar.

Görev belirli bir süre içinde yapmadıysanız izlenen durum uyarı olarak çevrilebilir (**t1**, örneğin 10 dakika). Görev zaman içinde tamamlanmazsa (**t2**, örneğin 20 dakika), izlenen koşul hata olarak çevrilebilir. Bu raporlama birden çok yolla yapılabilir:

* Ana birincil çoğaltma kendisini düzenli aralıklarla bildirir. Sırada bekleyen tüm görevler için bir özellik olabilir. En az bir daha uzun sürer özelliği hakkında rapor durumu görev **PendingTasks** bir uyarı veya hata uygun olarak. Bekleyen görev yok veya tüm görevler yürütme başladı, rapor durum normaldir. Kalıcı görevlerdir. Birincil kullanılamaz hale gelirse, yeni yükseltilen birincil düzgün bildirmeye devam edebilirsiniz.
* Başka bir izleme işlemde (Bulut veya dış) görevler denetler (gelen dışında istediğiniz görevin sonucuna bağlı olarak) bunlar tamamladığını görmek için. Eşikleri uymaz, bir rapor ana hizmette gönderilir. Rapor ayrıca görev tanımlayıcısı gibi içeren her görev için gönderilen **PendingTask + Taskıd**. Raporları yalnızca sağlıksız durumlarına gönderilmelidir. Birkaç dakika canlı ve temizleme emin olmak için dolduğunda kaldırılacak raporlar işaretlemek için zaman ayarlayın.
* Bir görevi Yürütülüyor ikincil çalıştırmak için beklenenden uzun sürüyor bildirir. Özellikte hizmet örneği üzerinde raporları **PendingTasks**. Raporun sorunlarına neden olan hizmet örneği pinpoints ancak burada örnek sonlandıktan durumu yakalamaz. Raporları sonra temizlenir. İkincil hizmette rapor edilemedi. İkincil görev tamamlarsa, ikincil örneği Mağaza'dan rapor temizler. Raporun burada bildirim iletisi kaybolur ve Görev Yöneticisi'nin açısından bakıldığında bitmedi durumu yakalamaz.

Yukarıda açıklanan durumlarda raporlama yapılır ancak sistem durumu değerlendirmesinde raporları uygulama sistem yakalanır.

## <a name="report-periodically-vs-on-transition"></a>Raporda düzenli aralıklarla ve geçiş
Sistem durumu raporlama modeli kullanarak watchdogs raporları düzenli aralıklarla veya geçişleri gönderebilirsiniz. Kod çok daha basit ve daha az olduğundan izleme raporlama için önerilen yol, düzenli aralıklarla hatalara açıktır. Watchdogs yanlış raporları tetiklemek hatalardan kaçınmak olabildiğince basit olmasını çaba gerekir. Yanlış *sağlıksız* raporları, sistem durumu değerlendirmesini ve yükseltmeleri dahil olmak üzere sistem durumu hakkında temel senaryoları etkileyen. Yanlış *sağlıklı* raporları istenmediği küme sorunları gizle.

Dönemsel raporlama için izleme olaylarını bir süreölçerli uygulanabilir. Bir zamanlayıcı geri izleme durumunu denetleyin ve geçerli durumunu temel alan bir rapor gönderin. Hangi raporun daha önce gönderilen bakın veya Mesajlaşma bakımından herhangi iyileştirmeler yapmak için gerek yoktur. Sistem durumu istemci performansı yardımcı olmak için mantığı toplu işleme sahiptir. Sistem durumu istemci Canlı tutulur, ancak bu dahili olarak rapor sistem durumu mağaza tarafından onaylanan veya izleme olaylarını varlık, özellik ve kaynak ile daha yeni bir rapor oluşturur kadar yeniden dener.

Geçişleri üzerinde raporlama durumunun dikkatli işleme gerektirir. İzleme bazı koşullar izler ve koşullar değiştiğinde yalnızca raporlar. Bu yaklaşımın baş daha az raporları gerekli olan ' dir. Dezavantajı izleme mantığını karmaşık olmasıdır. Böylece bunlarda durum değişiklikleri belirlemek için denetimi izleme olaylarını koşullar veya raporları bulundurmanız gerekir. Yük devretme, eklediğiniz, ancak henüz sistem durumu mağazaya gönderilen raporlarla dikkatli olunması gerekir. Sıra numarası, gitgide artan olması gerekir. Aksi durumda, raporları eski reddedilir. Veri kaybı burada ücrete nadir durumlarda eşitleme Raporlayıcı durumunu ve sistem durumu deposu durumunu arasında gerekli olabilir.

Geçişleri üzerinde raporlama anlamlı kendilerini üzerinde aracılığıyla reporting services için `Partition` veya `CodePackageActivationContext`. Zaman yerel nesnesini (çoğaltma veya dağıtılmış hizmet paketi / dağıtılan uygulama) olan kaldırıldı, tüm raporları da kaldırılır. Bu otomatik temizleme Raporlayıcı ve sistem durumu deposu arasındaki eşitleme gereksinimini rahatlatır. Rapor için üst bölüm veya üst uygulama ise dikkatli health store içinde eski raporları önlemek için yük devretme izlenmelidir. Doğru durumunu korumak ve artık gerekmeyen zaman deposundan rapor temizlemek için mantığı yeniden eklenmesi gerekir.

## <a name="implement-health-reporting"></a>Sistem durumu raporlamasını Uygula
Varlık ve rapor ayrıntıları Temizle olduktan sonra sistem durumu raporları gönderme API'si, PowerShell veya REST yapılabilir.

### <a name="api"></a>API
API aracılığıyla bildirmek için bir sistem durumu raporu raporlamak istediğiniz varlık türüne özgü oluşturmanız gerekir. Rapor bir sistem durumu istemciye verin. Alternatif olarak, bir sistem durumu bilgisi oluşturun ve bu üzerinde raporlama yöntemleri düzeltmek için geçirin `Partition` veya `CodePackageActivationContext` geçerli varlıklarını bildirilecek.

Aşağıdaki örnekte küme içindeki izleme olaylarını kullanarak raporlama düzenli gösterir. İzleme, dış kaynak gelen bir düğümde erişilip erişilemeyeceğini denetler. Kaynak, uygulama içinde bir hizmet bildirimi gerekli değildir. Kaynak kullanılamıyorsa, uygulama içinde diğer hizmetlerin hala düzgün bir şekilde çalışabilir. Bu nedenle, rapor her 30 saniyede dağıtılan hizmet paketi varlıkta gönderilir.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShell
Sistem durumu raporları ile Gönder **gönderme ServiceFabric*EntityType*HealthReport**.

Aşağıdaki örnek, bir düğüm üzerindeki CPU değerleri raporlama düzenli gösterir. Raporların her 30 saniyede gönderilmesi gerektiğini ve iki dakikalık bir süresi vardır. Bunlar zaman aşımına uğrarsa Raporlayıcı sorunları sahiptir, böylece düğüm hatası değerlendirilir. CPU bir eşiğin olduğunda uyarı sistem durumu raporu vardır. CPU yapılandırılmış saatten daha fazla bilgi için bir eşiğin üstünde kaldığında, hata olarak bildirilir. Aksi takdirde Raporlayıcı Tamam sistem durumunu gönderir.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Aşağıdaki örnek, bir çoğaltma üzerinde geçici bir uyarı bildirir. Bu ilk bölüm kimliği ve çoğaltma kimliği, ilgilendiği hizmet alır. Ardından bir rapordan gönderir **PowershellWatcher** özellikte **ResourceDependency**. Rapor yalnızca iki dakika ilginizi çekecektir ve Mağazası'ndan otomatik olarak kaldırılır.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>REST
İstenen varlık gidin ve sistem durumu raporu açıklaması gövdesine sahip POST istekleri ile REST kullanarak sistem durumu raporları gönderin. Örneğin, REST göndermek bkz. [küme sistem durumu raporlarının](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) veya [hizmet sistem durumu raporlarının](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service). Tüm varlıklar desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
Sistem Durumu verilerini temel alarak hizmeti yazıcılarını ve küme/uygulama yöneticileri bilgi tüketmek için farklı şekilde düşünebilirsiniz. Örneğin, bunların sistem durumuna göre uyarıları bunlar kesintilere yol açabilirsiniz önce ciddi sorunlar yakalamak için ayarlayabilirsiniz. Yöneticiler, sorunları otomatik olarak düzeltmek için onarım sistemi de ayarlayabilirsiniz.

[Giriş Service Fabric sistem durumu izleme](service-fabric-health-introduction.md)

[Service Fabric sistem durumu raporlarını görüntüle](service-fabric-view-entities-aggregated-health.md)

[Nasıl rapor ve hizmetin sistem durumunu denetle](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Sorunlarını gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[İzleme ve Hizmetleri yerel olarak tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md)

