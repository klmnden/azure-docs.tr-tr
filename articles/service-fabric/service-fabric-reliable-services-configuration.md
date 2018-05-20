---
title: Güvenilir Azure mikro yapılandırma | Microsoft Docs
description: Durum bilgisi olan güvenilir hizmetler Azure Service Fabric yapılandırma hakkında bilgi edinin.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: sumukhs
ms.openlocfilehash: c5aaf9869326f2de86d3bff33f36e8f967f3e6fa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="configure-stateful-reliable-services"></a>Durum bilgisi olan güvenilir hizmetler yapılandırın
Güvenilir hizmetler için yapılandırma ayarlarını iki kümesi vardır. Diğer küme belirli bir güvenilir hizmete özgü olsa da bir kümedeki tüm güvenilir hizmetler için genel kümesidir.

## <a name="global-configuration"></a>Genel yapılandırma
Genel güvenilir hizmet yapılandırmasını KtlLogger bölümünde küme için küme bildiriminde belirtilir. Paylaşılan günlük konumunu ve boyutunu ve Günlükçü tarafından kullanılan genel bellek sınırları yapılandırmanızı sağlar. Küme bildirimi ayarları ve tüm düğümleri ve küme Hizmetleri'nde geçerli yapılandırmaları tutan tek bir XML dosyasıdır. Dosya genellikle ClusterManifest.xml çağrılır. Küme görebilirsiniz Get-ServiceFabricClusterManifest powershell komutunu kullanarak, kümeniz için bildirim.

### <a name="configuration-names"></a>Yapılandırma adları
| name | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobayt |8388608 |Çekirdek modunda Günlükçü yazma arabelleği bellek havuzu için ayrılacak KB minimum sayısı. Bu bellek havuzundaki durum bilgilerini diske yazma önce önbelleğe almak için kullanılır. |
| WriteBufferMemoryPoolMaximumInKB |Kilobayt |Sınırsız |Günlükçü yazma arabelleği bellek havuzundaki en büyük boyutu büyüyebilir. |
| SharedLogId |GUID |"" |SharedLogId hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümlere tüm güvenilir hizmetler tarafından kullanılan varsayılan paylaşılan günlük dosyası belirlemek için kullanmak üzere benzersiz bir GUID belirtir. SharedLogId belirtilirse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam yol adı |"" |Paylaşılan günlük dosyası SharedLogPath hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümlere tüm güvenilir hizmetler tarafından kullanıldığı tam yolunu belirtir. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmesi gerekir. |
| SharedLogSizeInMB |Megabayt |8192 |Statik olarak paylaşılan günlüğü ayırmak için MB disk alanı sayısını belirtir. Değer, 2048 veya daha büyük olmalıdır. |

Aşağıdaki örnek, nasıl değiştirileceğini gösterir, Azure ARM veya şirket içi JSON şablonunu, durum bilgisi olan hizmetler için herhangi bir güvenilir koleksiyonu yedeklemek için oluşturulan paylaşılan işlem günlüğü.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>Örnek yerel geliştirici küme bildirim bölümü
Bu yerel geliştirme ortamınızı değiştirmek isterseniz, yerel clustermanifest.xml dosyasını düzenlemeniz gerekir.

```xml
   <Section Name="KtlLogger">
     <Parameter Name="SharedLogSizeInMB" Value="4096"/>
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
   </Section>
```

### <a name="remarks"></a>Açıklamalar
Günlükçü hizmetlerine ayrılmış günlüğe yazılan güvenilir hizmet çoğaltma ile ilişkili önce durumu verileri önbelleğe alma için tüm güvenilir bir düğüm üzerindeki kullanılabilir alınamayan çekirdek bellekten ayrılmış bellek genel havuzu vardır. Havuz boyutu WriteBufferMemoryPoolMinimumInKB ve WriteBufferMemoryPoolMaximumInKB ayarları tarafından denetlenir. Bu bellek havuzundaki ilk boyutu ve bellek havuzu Küçült en düşük boyut WriteBufferMemoryPoolMinimumInKB belirtir. WriteBufferMemoryPoolMaximumInKB, bellek havuzunda büyümesine en yüksek boyutudur. Açılan her güvenilir hizmet çoğaltma belirlenen sistem miktar WriteBufferMemoryPoolMaximumInKB kadar bellek havuzunun boyutunu artırabilir. Kullanılabilenden bellek havuzundaki bellek için daha fazla isteğe bağlı ise, bellek için istekleri bellek kullanılabilir oluncaya kadar geciktirilir. Yazma arabelleği bellek havuzu için belirli bir yapılandırma çok küçük ise, bu nedenle sonra performans düşebilir.

SharedLogId ve SharedLogPath ayarlarını her zaman kümedeki tüm düğümler için varsayılan paylaşılan günlük için konum ve GUID tanımlamak için birlikte kullanılır. Varsayılan paylaşılan günlük ayarları belirli hizmet settings.xml belirtmediği tüm güvenilir hizmetler için kullanılır. En iyi performans için yalnızca paylaşılan günlük dosyası için Çekişme azaltmak için kullanılan diskler üzerinde paylaşılan günlük dosyalarını yerleştirilmelidir.

SharedLogSizeInMB varsayılan paylaşılan günlüğü tüm düğümlerde erişinceye için disk alanı miktarını belirtir.  SharedLogId ve SharedLogPath belirtilmesini SharedLogSizeInMB sırada belirtilmesi gerekmez.

## <a name="service-specific-configuration"></a>Hizmet belirli yapılandırma
Yapılandırma paketi (yapılandırma) veya hizmet uygulaması (kod) kullanarak, durum bilgisi olan güvenilir hizmetler varsayılan yapılandırmaları değiştirebilirsiniz.

* **Config** -yoluyla yapılandırma yapılandırma paketi Microsoft Visual Studio Paketi kök uygulamadaki her hizmet için yapılandırma klasörü altında oluşturulan Settings.xml dosyasını değiştirerek gerçekleştirilir.
* **Kod** -kod aracılığıyla yapılandırmaya, uygun seçenekleri kümesiyle ReliableStateManagerConfiguration nesnesi kullanılarak bir ReliableStateManager oluşturarak gerçekleştirilir.

Varsayılan olarak, Azure Service Fabric çalışma zamanı Settings.xml dosyasında tanımlanmış bölüm adları arar ve temeldeki çalışma zamanı bileşenleri oluşturulurken yapılandırma değerlerini kullanır.

> [!NOTE]
> Yapmak **değil** bölüm adlarını hizmetinizde kod yapılandırmak planlamıyorsanız, Visual Studio çözümünde oluşturulan Settings.xml dosyasında aşağıdaki yapılandırmalardan birini silin.
> Yapılandırma paketi veya bölüm adları yeniden adlandırma ReliableStateManager yapılandırırken bir kod değişikliği gerekir.
> 
> 

### <a name="replicator-security-configuration"></a>Çoğaltıcı güvenlik yapılandırması
Çoğaltıcı güvenlik yapılandırmalarını çoğaltma sırasında kullanılır ve iletişim kanalının güvenliğini sağlamak için kullanılır. Bu hizmetler yüksek oranda kullanılabilir hale getirileceğini verileri de güvenli olduğundan emin olmanın birbirlerinin çoğaltma trafiği, görmeye olmayacak anlamına gelir. Varsayılan olarak, bir boş güvenlik yapılandırması bölümü çoğaltma güvenlik engeller.

### <a name="default-section-name"></a>Varsayılan bölüm adı
ReplicatorSecurityConfig

> [!NOTE]
> Bu bölüm adı değiştirmek için bu hizmet için ReliableStateManager oluştururken ReliableStateManagerConfiguration Oluşturucusu replicatorSecuritySectionName parametresi geçersiz.
> 
> 

### <a name="replicator-configuration"></a>Çoğaltıcı yapılandırma
Çoğaltıcı yapılandırmaları durum bilgisi olan güvenilir hizmetinin durumunu yüksek oranda güvenilir çoğaltmak ve durumu yerel olarak kalıcı yapmaktan sorumlu çoğaltıcı yapılandırın.
Varsayılan yapılandırma Visual Studio şablon tarafından oluşturulan ve yeterli olacaktır. Bu bölümde çoğaltıcı ayarlamak kullanılabilir olan ek yapılandırmalar hakkında alınmaktadır.

### <a name="default-section-name"></a>Varsayılan bölüm adı
ReplicatorConfig

> [!NOTE]
> Bu bölüm adı değiştirmek için bu hizmet için ReliableStateManager oluştururken ReliableStateManagerConfiguration Oluşturucusu replicatorSettingsSectionName parametresi geçersiz.
> 
> 

### <a name="configuration-names"></a>Yapılandırma adları
| name | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Kendisi için göndermeden önce bir işlem aldıktan sonra ikincil bekler adresindeki çoğaltıcı geri bir bildirim için birincil süre. Bu aralık dahilinde işlenen işlemleri için gönderilmek üzere başka bir onayları bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan yok--gerekli parametre |IP adresi ve birincil/ikincil çoğaltıcı diğer çoğaltıcılar yineleme ile iletişim kurmak için kullanacağı bağlantı noktası olarak ayarlayın. Bu hizmet bildiriminde TCP kaynak uç noktası başvuruda bulunmalıdır. Başvurmak [Service manifest kaynakları](service-fabric-service-manifest-resources.md) daha fazla bilgi için bir hizmet bildirimi uç noktası kaynakları tanımlama hakkında. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |8192 |Birincil kuyruk işlemlerinde maksimum sayısı. Birincil çoğaltma tüm ikincil çoğaltıcılar alındısı sonra bir işlem yukarı serbest bırakılır. Bu değer 64 ve 2'in büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |16384 |İkincil kuyruk işlemlerinde maksimum sayısı. Bir işlem yukarı durumuna Kalıcılık üzerinden yüksek oranda kullanılabilir yaptıktan sonra serbest bırakılır. Bu değer 64 ve 2'in büyük olmalıdır. |
| CheckpointThresholdInMB |MB |50 |Günlük dosyası alanının geçmesi belirttiğinizde durumda tutar. |
| MaxRecordSizeInKB |KB |1024 |Çoğaltıcı günlüğüne yazabilir en büyük kayıt boyutu. Bu değer birden fazla 4 ve 16'den büyük olmalıdır. |
| MinLogSizeInMB |MB |0 (belirlenen sistem) |İşlem günlüğü minimum boyutu. Günlük bu ayarı aşağıda boyuta bir kesecek şekilde izin verilmiyor. Çoğaltıcı en küçük günlük boyutu belirleyecek 0 gösterir. Bu değer artırıldığında kısmi kopyalar ve artımlı yedeklemeler kesilmesine azaltıldığı ilgili günlük kayıtları olasılığını itibaren yapmanın olasılığı artar. |
| TruncationThresholdFactor |Faktörü |2 |Hangi günlük boyutta kesilmesi tetiklenecek belirler. Kesme eşiği tarafından TruncationThresholdFactor çarpılan MinLogSizeInMB tarafından belirlenir. TruncationThresholdFactor 1'den büyük olmalıdır. MinLogSizeInMB * TruncationThresholdFactor MaxStreamSizeInMB daha az olmalıdır. |
| ThrottlingThresholdFactor |Faktörü |4 |Hangi günlük boyutta karşılaşıldığı çoğaltma başlar belirler. Azaltma eşiği (MB) cinsinden en büyük tarafından belirlenir ((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)). Azaltma eşiği (MB) cinsinden (MB) cinsinden kesilmesi eşik değerinden büyük olmalıdır. Kesme eşiği (MB) cinsinden MaxStreamSizeInMB daha az olmalıdır. |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |En büyük boyutunu (MB) verilen yedek günlüğü zinciri yedekleme günlüklerinde toplanan. Artımlı yedekleme birikmiş yedekleme günlüklerini bu boyuttan büyük olacak şekilde ilgili tam yedekleme sonrasındaki neden olacak bir yedekleme günlüğü oluşturur bir artımlı yedekleme istekleri başarısız olur. Böyle durumlarda, kullanıcı, tam yedek almak için gereklidir. |
| SharedLogId |GUID |"" |Bu çoğaltma ile kullanılan paylaşılan günlük dosyası belirlemek için kullanmak üzere benzersiz bir GUID belirtir. Genellikle, hizmetleri, bu ayar kullanmamalısınız. SharedLogId belirtilirse, ancak ardından SharedLogPath de belirtilmesi gerekir. |
| SharedLogPath |Tam yol adı |"" |Bu çoğaltma için paylaşılan günlük dosyası oluşturulacağı tam yolunu belirtir. Genellikle, hizmetleri, bu ayar kullanmamalısınız. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmesi gerekir. |
| SlowApiMonitoringDuration |Saniye |300 |Yönetilen API çağrıları için izleme aralığı ayarlar. Örnek: kullanıcı tarafından sağlanan yedekleme geri çağırma işlevi. Aralığı geçtikten sonra bir uyarı sistem durumu raporu sistem durumu Yöneticisi gönderilir. |

### <a name="sample-configuration-via-code"></a>Kod aracılığıyla örnek yapılandırma
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### <a name="remarks"></a>Açıklamalar
Çoğaltma gecikmesi BatchAcknowledgementInterval denetler. (Daha fazla bildirim iletileri gerekir gönderilmesine ve işlenen her daha az onayları içeren gibi) '0' değeri, üretilen işi, en düşük olası gecikme sonuçlanır.
BatchAcknowledgementInterval için büyük değer, o kadar yüksektir genel çoğaltma üretilen işi, daha yüksek işlem gecikme artması pahasına olur. Bu işlem yürütme gecikmesi için doğrudan dönüşür.

CheckpointThresholdInMB değeri çoğaltıcı yinelemenin ayrılmış günlük dosyasında durum bilgilerini depolamak için kullanabileceğiniz disk alanı miktarını denetler. Bu varsayılan değerinden daha yüksek bir değere artırma yeni bir çoğaltma kümesine eklendiğinde daha hızlı yeniden yapılandırma sürelerine neden olabilir. Bu işlem günlüğünde daha fazla geçmiş kullanılabilirliği nedeniyle gerçekleşir kısmi durum transfer kaynaklanır. Bu olası kilitlenme sonrasında bir çoğaltmanın kurtarma süresini artırabilir.

MaxRecordSizeInKB ayarı çoğaltıcı tarafından günlük dosyasına yazılabilir bir kayıt en büyük boyutunu tanımlar. Çoğu durumda, varsayılan 1024-KB kayıt boyutu idealdir. Hizmet durumu bilgileri parçası olarak daha büyük veri öğeleri neden olup olmadığını ancak, daha sonra bu değeri artırılacak gerekebilir. Daha küçük kayıtları yalnızca küçük kayıt için ihtiyaç duyulan alanı kullanırken, 1024'ten MaxRecordSizeInKB küçülterek az avantajı vardır. Bu değer yalnızca nadir durumlarda değiştirilmesi gerekir bekliyoruz.

SharedLogId ve SharedLogPath ayarlarını her zaman ayrı bir paylaşılan günlük varsayılan paylaşılan günlüğünden düğümü için kullanmak hizmet olmak için birlikte kullanılır. En iyi verimlilik için mümkün olduğunca çok Hizmetleri aynı paylaşılan günlük belirtmeniz gerekir. Paylaşılan günlük dosyaları, yalnızca paylaşılan günlük dosyası için baş taşıma Çekişme azaltmak için kullanılan disklerde yerleştirilmelidir. Bu değer yalnızca nadir durumlarda değiştirilmesi gerekir bekliyoruz.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulamanızı Visual Studio'da hata ayıklama](service-fabric-debugging-your-application.md)
* [Güvenilir hizmetler için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/dn706529.aspx)

