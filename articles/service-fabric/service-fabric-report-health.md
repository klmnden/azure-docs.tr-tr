---
title: Özel Service Fabric durum raporları ekleme | Microsoft Docs
description: Azure Service Fabric sistem durumu varlıkları için özel durum raporları göndermeyi açıklar. Tasarlama ve uygulama kalitesini sistem durumu raporu için öneriler sunar.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: chackdan
editor: ''
ms.assetid: 0a00a7d2-510e-47d0-8aa8-24c851ea847f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: 49ebf4ab95816a3da2f74a464b12b46de6228456
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723464"
---
# <a name="add-custom-service-fabric-health-reports"></a>Özel Service Fabric durum raporları ekleme
Azure Service Fabric tanıtır bir [sistem durumu modeli](service-fabric-health-introduction.md) sağlıksız küme ve belirli varlıkların uygulama koşullara bayrağı için tasarlanmıştır. Sistem durumu modeli kullanan **sistem durumu raporlayıcıları** (sistem bileşenleri ve watchdogs). , Kolay ve hızlı tanılama ve onarma hedeftir. Sistem durumu hakkında ön düşünmek beklemeleri gerekir. Özellikle bayrağı sorunların kök yakın yardımcı olabilir, sistem durumu etkileyebilecek herhangi bir koşul, bildirilmelidir. Sistem durumu bilgileri, hata ayıklama ve araştırma zaman ve çaba kaydedebilirsiniz. Hizmet bulutta ölçekte çalışır duruma geldikten sonra yararlılığı özellikle Temizle (özel veya Azure).

Service Fabric raporlayıcıları İzleyici ilgi koşullar belirledik. Bunlar, yerel bir görünümü temel alarak bu koşullara rapor. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) varlıkları genel olarak sağlıklı olup olmadığını belirlemek için tüm raporlayıcıları tarafından gönderilen sistem durumu verilerini toplar. Model, zengin, esnek ve kullanımı kolay olacak şekilde tasarlanmıştır. Sistem Durumu raporu Kalite küme sistem durumu görünümünü doğruluğunu belirler. Yanlış sağlıksız sorunları Göster hatalı pozitif sonuçları, yükseltme ya da sistem durumu verileri kullanan diğer hizmetlerde olumsuz yönde etkileyebilir. Onarım hizmetleri ve uyarı mekanizmaları gibi hizmetlere örnektir. Bu nedenle, bazı döndürülebileceği bakımından, olabilecek en iyi şekilde gösterdiğiniz ilgi koşullarını yakalama raporları sağlamak için gereklidir.

Tasarım ve durumu raporlama uygulamak için watchdogs ve sistem bileşenleri gerekir:

* Bunlar ilgilendiğiniz koşul izlenip biçimini ve etkisi kümede veya uygulamanın işlevselliğini tanımlayın. Bu bilgilere dayanarak, sistem durumu ve sistem durumu raporu özellik üzerinde karar verin.
* Belirlemek [varlık](service-fabric-health-introduction.md#health-entities-and-hierarchy) , rapor için geçerlidir.
* Raporlama olduğu belirlemek Bitti, gelen hizmetinde veya bir iç veya dış izleme.
* Muhabir tanımlamak için kullanılan bir kaynağı tanımlayın.
* Raporlama bir strateji, düzenli aralıklarla veya geçişi seçin. Daha basit bir kod gerekir ve hatalara daha az yatkındır önerilen yöntem düzenli olarak aynıdır.
* Ne kadar iyi durumda olmayan koşulları için rapor health store içinde da kalmalı ve nasıl temizlenmelidir belirler. Bu bilgileri kullanarak rapor yaşam süresi ve kaldırma üzerinde sona erme davranışını karar verin.

Belirtildiği gibi raporlama alanından yapılabilir:

* İzlenen Service Fabric hizmeti çoğaltma.
* İç watchdogs (koşullar izler ve raporlar sorunları gibi bir Service Fabric durum bilgisi olmayan hizmet) Service Fabric hizmet olarak dağıtılabilir. Watchdogs olabilir bir tüm düğümlerine dağıtılan veya izlenen hizmete affinitized.
* Service Fabric düğümleri üzerinde çalışmak, ancak olan iç watchdogs *değil* Service Fabric Hizmetleri uygulanır.
* Kaynak araştırma dış watchdogs *dışında* Service Fabric kümesine (örneğin, izleme hizmeti Gomez gibi).

> [!NOTE]
> Kullanıma hazır, küme sistem bileşenleri tarafından gönderilen sistem durumu raporlarının doldurulur. Adresinde daha fazla [sistem durumu kullanarak raporları sorun giderme için](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). Kullanıcı raporları gönderilmelidir [sistem durumu varlıklarını](service-fabric-health-introduction.md#health-entities-and-hierarchy) , zaten oluşturulmuşsa sistem tarafından.
> 
> 

Bir kez tasarım işaretlenmemiştir raporlama sistem, sistem durumu raporlarının kolayca gönderilebilir. Kullanabileceğiniz [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) küme değilse, rapor sağlığı [güvenli](service-fabric-cluster-security.md) veya doku istemci yönetim ayrıcalıkları varsa. Raporlama yapılabilir API tarafından aracılığıyla kullanarak [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth), PowerShell veya REST. Geliştirilmiş performans raporları yapılandırma düğmelerini toplu.

> [!NOTE]
> Rapor sistem zaman uyumludur ve yalnızca istemci tarafında doğrulama işi gösterir. Rapor sistem istemci tarafından kabul edildiğini olgu veya `Partition` veya `CodePackageActivationContext` nesneleri değil ortalama deposunda uygulanır. Zaman uyumsuz olarak gönderilen ve büyük olasılıkla diğer raporlarla toplu. Sunucuda işlenmesi başarısız olabilir: sıra numarası eski olabilir, raporu uygulanmalıdır varlık silindi, vb.
> 
> 

## <a name="health-client"></a>İstemci sistem durumu
Sistem durumu raporlarının sistem durumu Yöneticisi doku istemci içinde bulunduğu bir sistem durumu istemcisi üzerinden gönderilir. Sağlık Yöneticisi raporları health store içinde kaydeder. Sistem durumu istemci aşağıdaki ayarlarla yapılandırılabilir:

* **HealthReportSendInterval**: Rapor istemciye eklenen zaman ve saat arasındaki gecikme, sistem durumu Yöneticisi için gönderilir. Batch raporları için bir ileti gönderilmesi yerine tek bir ileti her rapor için kullanılır. Toplu işleme performansını geliştirir. Varsayılan: 30 saniye.
* **HealthReportRetrySendInterval**: Başlangıçtan birikmiş sağlık durumu istemci beşe aralığı için sistem durumu Yöneticisi bildirir. Varsayılan: 30 saniye, en düşük: 1 saniye.
* **HealthOperationTimeout**: Sağlık Yöneticisi için gönderilen bir raporu ileti için zaman aşımı süresi. Bir ileti zaman aşımına uğrarsa, rapor işlenen sistem durumu Yöneticisi onaylayana kadar durum istemci, yeniden dener. Varsayılan: iki dakika.

> [!NOTE]
> Raporları toplu işlendiğinde fabric istemci en az için Canlı tutulmalıdır HealthReportSendInterval bunlar gönderildiğinden emin olmak için. İleti kaybolur veya sistem durumu Yöneticisi geçici hatalar nedeniyle uygulanamıyor, doku istemci artık canlı tutulmalıdır yeniden denemek için bir fırsat vermek için.
> 
> 

İstemcide arabelleğe alma raporları benzersizliğini dikkate alır. Örneğin, belirli bir hatalı muhabir saniye başına 100 raporları aynı varlığın aynı özellikte yapıyorsa, raporlar en son sürümle değiştirilir. En fazla bir tür rapor istemci kuyrukta var. Toplu işleme yapılandırılmışsa, sistem durumu Yöneticisi için gönderilen raporların gönderme aralığı başına yalnızca bir tane sayısıdır. Bu rapor, varlığın en güncel durumu yansıtır son eklenen rapor eder.
Yapılandırma parametrelerini belirtin, `FabricClient` geçirerek oluşturulan [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) sistem durumu ile ilgili girdiler için istenen değerleri.

Aşağıdaki örnek, bir doku istemci oluşturur ve eklenen raporları gönderilmesi gerektiğini belirtir. Zaman aşımları ve yeniden denenebilir bir hata, yeniden denemeler 40 saniyede gerçekleşir.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Varsayılan yapı istemci ayarlanan ayarları tutma öneririz `HealthReportSendInterval` 30 saniyedir. Bu ayar, toplu işleme nedeniyle en iyi performans sağlar. Olabildiğince çabuk gönderilmelidir kritik raporlar için `HealthReportSendOptions` hemen ile `true` içinde [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API. Hemen raporları toplu aralığı atlama. Bu bayrak dikkatli kullanın; mümkün olduğunda toplu işleme durumu istemci yararlanmak istiyoruz. Hemen gönderme yararlıdır ayrıca fabric istemci kapatırken (örneğin, işlemi geçersiz bir durum belirledi ve yan etkileri önlemek için kapatmak gerekir). En yüksek çaba gönderme birikmiş raporlar sağlar. Bir rapor ile hemen bayrağı eklendiğinde, sistem durumu istemci birikmiş tüm raporları beri son gönderme toplu olarak işler.

PowerShell ile bir küme için bir bağlantı oluşturulduğunda, aynı parametreleri belirtilebilir. Aşağıdaki örnek, yerel bir küme için bir bağlantı başlatır:

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

Benzer şekilde, API'ye raporları kullanarak gönderilebilir `-Immediate` hemen gönderilen, bakılmaksızın olmasını anahtar `HealthReportSendInterval` değeri.

KALAN için bir iç yapı istemcisi olan Service Fabric gateway raporlar gönderilir. Varsayılan olarak, bu istemci raporları göndermek için yapılandırılmış her 30 saniyede bir toplu. Toplu iş aralığı ile küme yapılandırma ayarı değiştirebilirsiniz `HttpGatewayHealthReportSendInterval` üzerinde `HttpGateway`. Belirtildiği gibi raporlarıyla göndermek için daha iyi bir seçenek olan `Immediate` true. 

> [!NOTE]
> Yetkisiz Hizmetleri sistem durumu varlıkları kümedeki karşı raporlayamaz emin olmak için sunucu yalnızca güvenli istemcilerden gelen istekleri kabul edecek şekilde yapılandırın. `FabricClient` Gereken güvenlik seçeneğinin etkin olmasını kümeyle (örneğin, Kerberos veya sertifika kimlik doğrulaması) ile iletişim kurabilmesi için raporlama için kullanılır. Daha fazla bilgi edinin [küme güvenlik](service-fabric-cluster-security.md).
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Düşük ayrıcalıklı Hizmetleri içinde gelen rapor
Service Fabric Hizmetleri kümesi için yönetici erişimi yoksa, sistem durumu geçerli bağlam ile varlıkları üzerinde raporlamadan `Partition` veya `CodePackageActivationContext`.

* Durum bilgisi olmayan hizmetler için kullanın [IStatelessServicePartition.ReportInstanceHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) geçerli hizmet örneği üzerinde bildirmek için.
* Durum bilgisi olan hizmetler için kullanın [IStatefulServicePartition.ReportReplicaHealth](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) geçerli yineleme üzerinde bildirmek için.
* Kullanım [IServicePartition.ReportPartitionHealth](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) geçerli bölüm varlıkta bildirmek için.
* Kullanım [CodePackageActivationContext.ReportApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) geçerli uygulama bildirmek için.
* Kullanım [CodePackageActivationContext.ReportDeployedApplicationHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) geçerli düğümde dağıtılan geçerli uygulamayı bildirmek üzere.
* Kullanım [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) geçerli düğümde dağıtılmış uygulama için bir hizmet paketi hakkında rapor oluşturmak için.

> [!NOTE]
> Dahili olarak `Partition` ve `CodePackageActivationContext` varsayılan ayarlarla yapılandırılan bir sistem durumu istemcisi tutun. İçin açıklandığı gibi [istemci sistem durumu](service-fabric-report-health.md#health-client), raporları toplu ve bir zamanlayıcıyı temel gönderilir. Nesneleri raporu gönderme olanağı sağlamak canlı olarak tutulmalıdır.
> 
> 

Belirtebileceğiniz `HealthReportSendOptions` raporlarda gönderirken `Partition` ve `CodePackageActivationContext` durumu API'leri. Olabildiğince çabuk gönderilmelidir kritik raporlar varsa `HealthReportSendOptions` hemen ile `true`. Toplu işlem aralığı iç sistem durumu istemcinin anında raporları atlama. Daha önce belirtildiği gibi bu bayrak dikkatli kullanın; mümkün olduğunda toplu işleme durumu istemci yararlanmak istiyoruz.

## <a name="design-health-reporting"></a>Sistem durumu raporlama tasarlama
Yüksek kaliteli raporları üretme ilk adımı hizmet durumunu etkileyebilecek koşullar tanımlamak için kullanılır. Başlar--ya da daha da iyi bir sorun ortaya önce--potansiyel olarak denetleyebilir, bayrağı sorunları hizmetinde veya küme yardımcı olabilecek herhangi bir koşul milyarlarca dolarlık kaydedin. Araştırma ve sorunları ve daha yüksek müşteri memnuniyeti onarma daha az gece saat harcanan avantajları az kesinti içerir.

Koşullar tanımlandıktan sonra izleme yazıcılar ek yükü ve yararlılığını arasındaki dengeyi izlemek için en iyi yolu ekleyeceğimi gerekir. Örneğin, bazı geçici dosyaları paylaşımındaki kullanan karmaşık hesaplamalar yapan bir hizmeti kullanmayı düşünün. Bir izleme yeterli alanın kullanılabilir olmasını sağlamak için paylaşımı izleyebilir. Bu dosya veya dizin değişikliklerin bildirimleri için dinleme. Ön bir Eşiğe ulaşıldığında ve paylaşımı dolu ise, bir hata rapor, bir uyarı rapor edilemedi. Bir uyarı üzerinde onarım sistem eski dosyaların temizleme başlatabilir. Bir hatada, onarım sistem hizmeti çoğaltma başka bir düğüme taşıyabilirsiniz. Koşul durumlarını durumu bakımından ne açıklanan dikkat edin: (Tamam) sağlıklı kabul edilebilir bir koşulu veya sağlıksız (uyarı veya hata) durumu.

İzleme ayrıntılarını ayarladıktan sonra bir izleme yazıcısı izleme uygulamak nasıl gerekir. Koşulları hizmetinde belirlenebilir, izleme, izlenen hizmetin parçası olabilir. Örneğin, hizmet kod Paylaşımı'nın kullanımını denetleyin ve ardından bunu bir dosyaya yazmak her denediğinde rapor. Bu yaklaşımın avantajı raporlama basit olmasıdır. İzleme hataları hizmet işlevselliğinin etkilemesini önlemek için dikkatli olunması gerekir.

İçinde raporlama izlenen hizmeti her zaman bir seçenek değil. Bir izleme hizmeti dahilinde algılayan mümkün olmayabilir. Mantıksal veya belirlenmesi için veri olmayabilir. Koşullar izleme yükünü yüksek olabilir. Koşullar da belirli bir hizmete olmayabilir ancak bunun yerine Hizmetleri arasındaki etkileşimler etkiler. Ayrı işlemlerde kümedeki watchdogs sahip başka bir seçenektir. Watchdogs ana Hizmetleri herhangi bir şekilde etkilemeden rapor ve koşulları izleyin. Örneğin, bu watchdogs tüm düğümlere ya da hizmet olarak aynı düğümlerinde dağıttığınız aynı uygulamada durum bilgisi olmayan hizmetler olarak uygulanabilir.

Bazı durumlarda, kümede çalışan bir bekçi bir seçenek ya da değildir. Kullanıcılara sunulmadan gibi izlenen koşul kullanılabilirliğini veya işlevlerini ise, kullanıcı istemcileri olarak aynı yerde watchdogs sahip en iyisidir. Burada, bunlar operations çağırmaya kullanıcılar aynı şekilde test edebilirsiniz. Örneğin, kümenin dışında yer alan, istekleri hizmete sorunları ve gecikme süresi ve sonuç doğruluğunu denetler bir bekçi olabilir. (Hesaplayıcı hizmeti için örneğin, 2 + 2 4 makul bir sürede döndürür?)

İzleme Ayrıntıları sonlandırıldıktan sonra benzersiz olarak tanımlayan bir kaynak kimliği karar vermeniz gerekir. Farklı varlıklarda bildirilmesi gerekir kümedeki yaşayan aynı türde birden fazla watchdogs veya, bunlar aynı varlık üzerinde rapor gerekiyorsa, farklı bir kaynak kimliği veya özelliğini kullanın. Bu şekilde raporlarının bulunabilir. Sistem Durumu raporu, özellik, izlenen koşul yakalamalısınız. (Yukarıdaki örnekte özelliği olabilir **ShareSize**.) Birden çok rapor aynı durumuna uygularsanız, özelliği bulunabilmesi raporlar sağlayan dinamik bazı bilgileri içermelidir. Örneğin, birden çok paylaşımı izlenmesi gerekiyorsa, özellik adı olabilir **ShareSize sharename**.

> [!NOTE]
> Yapmak *değil* durum bilgilerini korumak için sistem durumu deposu kullanın. Bu bilgiler, bir varlığın sistem durumu değerlendirmesi etkiler gibi yalnızca sistem durumu ile ilgili bilgileri sistem durumu bildirilmelidir. Sistem durumu deposu, genel amaçlı bir deposu olarak tasarlanmamıştır. Tüm verileri sistem durumuna toplamak için sistem durumu değerlendirme mantığı kullanır. (Sistem durumu Tamam durumuyla raporlama gibi) sistem durumu için ilgisi olmayan bilgi gönderme toplanan sistem durumunu etkilemez, ancak sistem durumu deposu performansını olumsuz yönde etkileyebilir.
> 
> 

Sonraki karar noktası rapor varlığa açıktır. Çoğu zaman, koşul varlığa açıkça tanımlar. Varlık ile en iyi olası ayrıntı düzeyi seçin. Bir koşul, bir bölümdeki tüm çoğaltmaları etkiler, hizmet üzerinde bölüm rapor. Daha fazla döndürülebileceği bakımından, ancak gerekmesi halinde köşe durumlar vardır. Koşul, bir çoğaltma gibi varlık etkiler, ancak arzusu sağlamaktır birden çok çoğaltma yaşam süresi için koşul işaretlenmiş, sonra bölüme bildirilmesi. Aksi takdirde, çoğaltma silindiğinde, sistem durumu deposu tüm raporlarının temizler. İzleme yazarları, varlık ve rapor yaşam süresi hakkında düşünmeniz gerekir. (Örneğin, bir varlık üzerinde bildirilen hata artık uygulanmadığında) deposundan bir rapor temizlenmelidir olduğunda açık olmalıdır.

Birbirine ben açıklanan noktaları yerleştiren bir örneğe bakalım. Service Fabric uygulaması bir ana durum bilgisi olan kalıcı hizmeti ve tüm düğümleri (görevinin her türü için bir ikincil hizmet türü) dağıtılan ikincil durum bilgisi olmayan hizmetler oluşan göz önünde bulundurun. Asıl İkincil veritabanı tarafından yürütülecek komutları içeren bir işleme sırası vardır. İkincil gelen istekleri ve geri bildirim sinyal gönderin. İzlenen bir ana işleme sırası uzunluğunu durumdur. Ana kuyruk uzunluğu'ı bir eşiğe ulaşması halinde uyarı bildirilir. Uyarı, ikincil veritabanı yük işleyemiyor gösterir. Kuyruk uzunluğu en fazla ulaşır ve komutları bırakılır, hizmet kurtaramadığından bir hata bildirdi. Raporları özelliği olabilir **Kuyrukdurumu**. İzleme hizmeti içinde yer alan ve ana birincil Çoğaltmada düzenli aralıklarla gönderilir. Yaşam süresi iki dakika ve 30 saniyede bir düzenli aralıklarla gönderilir. Birincil kalırsa, raporun Mağazası'ndan otomatik olarak temizlenir. Hizmet çoğaltma çalışıyor, ancak bunu kilitlendiğini veya diğer sorun raporu health store içinde süresi dolar. Bu durumda, varlık hatası olarak değerlendirilir.

İzlenebilir başka bir koşul görev yürütme süresi kısadır. Ana görev türüne göre ikinciller görevleri dağıtır. Tasarım bağlı olarak, ana ikinciller görev durumu için yoklama. Ayrıca, aldıkları geri bildirim sinyaller göndermek ikinciller için de bekleyebilir. İkinci durumda, ikincil veritabanı zar veya iletileri kayıp olduğu durumlarda algılamak için dikkatli olunması gerekir. Durumu geri gönderen bir ping isteği aynı ikincil göndermek ana bir seçenektir. Durum yok alınmazsa, ana hata olarak kabul eder ve görev tarih değiştirdiğinde. Bu davranış, görevleri etkili olduğunu varsayar.

Görevi, belirli bir süre içinde yapılmazsa, bir uyarı olarak izlenen koşul çevrilebilir (**t1**, örneğin 10 dakika). Görev zaman tamamlanmadıysa (**t2**, örneğin 20 dakika), hata olarak izlenen koşul çevrilebilir. Bu raporlama birden çok yolla yapılabilir:

* Ana birincil çoğaltma kendisine düzenli aralıklarla bildirir. Kuyrukta bekleyen tüm görevler için bir özelliği olabilir. En az bir tane daha uzun sürer özelliği rapor durumu görev **PendingTasks** bir uyarı veya hata, uygun şekilde. Bekleyen görev yok veya tüm görevleri yürütme başlatıldı, rapor durumu uygun. Kalıcı görevlerdir. Birincil kalırsa, yeni yükseltilen birincil doğru rapor devam edebilirsiniz.
* Görevleri başka bir izleme işleminde (Bulut veya dış) denetler (gelen dışında istediğiniz görevin sonucuna bağlı olarak), tamamladığını görmek için. Yöneticiler eşikler uymuyorsa ana hizmette bir rapor gönderilir. Bir rapor gibi görev tanımlayıcısı içeren her görev için de gönderilir **PendingTask + Taskıd**. Raporlar yalnızca sağlıklı duruma göre gönderilmelidir. Birkaç dakika canlı ve temizleme emin olmak için bu süre dolduğunda kaldırılacak raporlar işaretlemek için zaman ayarlayın.
* Çalıştırmak için beklenenden daha uzun sürerse bir görevin yürütüldüğü ikincil bildirir. Hizmet örneği özellikte raporları **PendingTasks**. Rapor saptıyor sorunları olan hizmet örneği, ancak burada örnek sonlandıktan durumu yakalamaz. Raporları sonra temizlenir. İkincil hizmette raporu. İkincil görev tamamlanırsa ikincil örnek deposundan rapor temizler. Rapor nerede bildirim iletisi kaybolur ve Görev Yöneticisi'nin açısından bakıldığında bitmedi durumu yakalamaz.

Yukarıda açıklanan durumlarda raporlama gerçekleştirilir ancak sistem durumu değerlendirme raporları uygulama durumunu yakalanır.

## <a name="report-periodically-vs-on-transition"></a>Raporda düzenli aralıklarla ve geçiş
Sistem durumu raporlama modeli kullanarak watchdogs raporları düzenli aralıklarla veya geçişleri gönderebilirsiniz. Kod çok daha basit ve daha az olduğundan izleme raporlama için önerilen yöntem düzenli aralıklarla, hatalara açıktır. Watchdogs yanlış raporları tetikleme hataları önlemek mümkün olduğu kadar basit olmaya çabalar gerekir. Yanlış *sağlıksız* raporları, sistem durumu değerlendirme sürümleri ve yükseltme de dahil olmak üzere Durumu'na göre senaryoları etkiler. Yanlış *sağlıklı* raporları istenmiyorsa kümedeki sorunları gizle.

Dönemsel raporlama için izleme Zamanlayıcı ile uygulanabilir. Bir zamanlayıcı geri çağırma izleme durumunu denetleyin ve geçerli durumunu temel alan bir rapor gönderin. Rapor daha önce gönderilen bakın veya Mesajlaşma açısından iyileştirmelerin yapmak için gerek yoktur. Sistem durumu istemcinin, performansa yardımcı olmak için logic toplu işleme sahip. Sistem durumu istemci canlı olarak tutulur, ancak bunu dahili olarak rapor sistem durumu deposu tarafından onaylanır veya izleme varlık, özellik ve kaynak içeren yeni bir rapor oluşturur kadar yeniden dener.

Geçişleri üzerinde raporlama durum dikkatli işlenmesini gerektirir. İzleme, bazı koşullar izler ve yalnızca koşullar değiştiğinde bildirir. Bu yaklaşımın baş daha az raporları gereklidir ' dir. Olumsuz tarafı, izleme mantığını karmaşık olmasıdır. Böylece durumu değişiklikleri belirlemek için bunlarda izleme koşulu veya raporları sürdürmeniz gerekir. Yük devretmede, eklendi ancak henüz durum deposuna gönderilen raporlarla dikkatli olunması gerekir. Sürekli artan sıra numarası olmalıdır. Aksi durumda, raporlar, eski reddedilir. Ender durumlarda, veri kaybı burada tahakkuk ettirilen eşitleme muhabir durumunu ve sistem durumu deposu durumunu arasında gerekli olabilir.

Geçişleri üzerinde raporlama anlamlı aracılığıyla kendilerini üzerinde Raporlama Hizmetleri `Partition` veya `CodePackageActivationContext`. Yerel nesne (çoğaltma veya dağıtılmış hizmet paketi / uygulama dağıtılan) olan kaldırıldıysa, tüm raporlar da kaldırılır. Bu otomatik temizleme muhabir ve sistem durumu deposu arasındaki eşitleme gereği rahatlatır;. Raporun üst bölüm veya ana uygulama için ise, dikkatli health store içinde eski raporlar önlemek için yük devretme sırasında atılmalıdır. Doğru durumunu korumak ve raporun deposundan artık gerekmeyen temizlemek için mantıksal yeniden eklenmesi gerekir.

## <a name="implement-health-reporting"></a>Sistem durumu bildirimi uygulama
Varlık ve rapor ayrıntıları açık olduğunda, sistem durumu raporlarının gönderen API, PowerShell veya REST yapılabilir.

### <a name="api"></a>API
API aracılığıyla bildirmek için raporlamak istediğiniz varlık türü için belirli bir sistem durumu raporu oluşturmak gerekir. Sistem durumu istemciye raporu verin. Alternatif olarak, bir sistem durumu bilgisi oluşturmak ve raporlama yöntemleri üzerinde düzeltmek geçirin `Partition` veya `CodePackageActivationContext` geçerli varlıklarda bildirmek için.

Aşağıdaki örnek, küme içindeki bir bekçi kullanarak raporlama düzenli gösterir. İzleme, dış bir kaynak içinde bir düğüm erişilebilir olup olmadığını denetler. Kaynak uygulama içinde bir hizmet bildirimi tarafından gereklidir. Kaynak kullanılamıyorsa, uygulama içindeki diğer hizmetler yine de düzgün bir şekilde çalışabilir. Bu nedenle, raporu, her 30 saniyede dağıtılan hizmet paketi varlık üzerinde gönderilir.

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
Durum raporlarıyla göndermek **gönderme ServiceFabric*EntityType*HealthReport**.

Aşağıdaki örnek, bir düğüm üzerindeki CPU değerleri raporlama düzenli gösterir. Raporların her 30 saniyede gönderilmesi gerektiğini ve iki dakikalık bir süresi vardır. Bunlar zaman aşımına uğrarsa, düğüm hatası değerlendirilmemesi muhabir sorunları vardır. CPU bir eşiği aştığında uyarı sistem durumu raporu vardır. CPU, yapılandırılan süre değerinden daha fazla bilgi için bir eşiğin üstünde kaldığında, bir hata raporlanır. Aksi takdirde, sistem durumu Tamam muhabir gönderir.

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

Aşağıdaki örnek, bir çoğaltma üzerinde geçici bir uyarı bildirir. İlk bölüm kimliği ve çoğaltma kimliği, ilgileniyor hizmeti alır. Ardından bir rapordan gönderir **PowershellWatcher** özelliğindeki **ResourceDependency**. Raporu yalnızca iki dakika boyunca ilgi çekecektir ve Mağaza'dan otomatik olarak kaldırılır.

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
Sistem Durumu raporu, istenen varlığa gitmek ve sistem durumu raporu açıklaması gövdeye sahip POST istekleri ile REST kullanarak gönderin. Örneğin, REST gönderme konusunu [küme sistem durumu raporlarının](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) veya [hizmet sistem durumu raporlarının](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service). Tüm varlıkları desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
Sistem Durumu verilerini temel alarak hizmet Yazıcılar ve küme/uygulama yöneticileri bilgi edinme yollarını düşünebilirsiniz. Örneğin, bunlar sistem durumuna dayalı uyarılar bunlar kesintilerine yol açabilirsiniz önce ciddi sorunları yakalamak için ayarlayabilirsiniz. Yöneticiler, sorunları otomatik olarak düzeltmek için onarım sistemi de ayarlayabilirsiniz.

[Giriş Service Fabric sistem durumunu izleme](service-fabric-health-introduction.md)

[Service Fabric sistem durumu raporlarını görüntüleme](service-fabric-view-entities-aggregated-health.md)

[Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Sorunlarını gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md)

