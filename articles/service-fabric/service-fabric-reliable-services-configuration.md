---
title: Azure Service Fabric Reliable Services özelliğini yapılandırma | Microsoft Docs
description: Azure Service Fabric'te durum bilgisi olan Reliable Services yapılandırma hakkında bilgi edinin.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: chackdan
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: sumukhs
ms.openlocfilehash: 8ddb5d0566c57dd1d507d543ac53c0975a83dd43
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60723583"
---
# <a name="configure-stateful-reliable-services"></a>Durum bilgisi olan reliable services özelliğini yapılandırma
Reliable services için yapılandırma ayarlarını iki kümesi vardır. Başka bir küme için belirli bir güvenilir hizmet belirli olsa bir kümesi, kümedeki tüm reliable services için geneldir.

## <a name="global-configuration"></a>Genel yapılandırma
Genel bir güvenilir hizmet yapılandırması KtlLogger bölümünün altında küme için küme bildiriminde belirtilir. Bu, paylaşılan Günlük konumu ve boyutu artı Günlükçü tarafından kullanılan genel bellek sınırlarını yapılandırmanızı sağlar. Küme bildiriminde ayarlar ve tüm düğümleri ve kümedeki hizmetler için geçerli yapılandırmaları tutan tek bir XML dosyasıdır. Dosya, genellikle ClusterManifest.xml çağrılır. Küme görebilirsiniz Get-ServiceFabricClusterManifest powershell komutunu kullanarak kümeniz için bildirim.

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobayt |8388608 |KB, çekirdek modunda Günlükçü yazma arabellek belleği havuzu için ayrılacak en küçük sayısı. Bu bellek havuzundaki önce diske yazma durumu bilgilerini önbelleğe almak için kullanılır. |
| WriteBufferMemoryPoolMaximumInKB |Kilobayt |Sınırsız |Günlükçü için arabellek belleği havuzu yazma en büyük boyutu büyür. |
| SharedLogId |GUID |"" |SharedLogId hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümleri üzerindeki tüm güvenilir Hizmetleri tarafından kullanılan varsayılan paylaşılan günlük dosyası tanımlamak için kullanılacak benzersiz bir GUID belirtir. SharedLogId belirtilmişse SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam yol adı |"" |Paylaşılan bir günlük dosyası SharedLogPath hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümleri üzerindeki tüm güvenilir Hizmetleri tarafından kullanıldığı tam yolunu belirtir. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmelidir. |
| SharedLogSizeInMB |Megabayt |8192 |MB disk alanı için paylaşılan günlüğe statik olarak ayrılacak sayısını belirtir. Değer, 2048 ya da daha büyük olmalıdır. |

Azure ARM veya şirket içi JSON şablon, aşağıdaki örnekte nasıl durum bilgisi olan hizmetler için herhangi bir güvenilir koleksiyonlar yedeklemek için oluşturulan paylaşılan işlem günlüğü değiştirileceğini gösterir.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>Örnek yerel geliştirme kümesi bildirim bölümü
Bu, yerel geliştirme ortamınıza değiştirmek istiyorsanız, yerel clustermanifest.xml dosyasını düzenlemeniz gerekir.

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
Günlükçü güvenilir hizmet çoğaltma ile ilişkili ayrılmış günlüğe yazılan önce durumu verileri önbelleğe alma için bir düğüm tüm güvenilir hizmetlerinde kullanılabilir disk belleksiz çekirdek bellek ayrılan belleğin küresel bir havuz sahiptir. Havuz boyutunu WriteBufferMemoryPoolMinimumInKB ve WriteBufferMemoryPoolMaximumInKB ayarlarınız tarafından kontrol edilir. En düşük boyutunun bellek havuzunda küçültme ve bu bellek havuzundaki ilk boyutu WriteBufferMemoryPoolMinimumInKB belirtir. WriteBufferMemoryPoolMaximumInKB, bellek havuzunda büyüme en yüksek boyutudur. Açılan her bir güvenilir hizmet çoğaltma WriteBufferMemoryPoolMaximumInKB kadar bir sistem belirlenen miktarda bellek havuzunun boyutunu artırabilirsiniz. Kullanılabilir alandan bellek havuzundaki bellek için daha fazla isteğe bağlı ise, bellek için istekleri bellek kullanılabilir oluncaya kadar geciktirilecek. Yazma arabelleğini bellek havuzundaki belirli bir yapılandırma için çok küçük olduğunda bu nedenle daha sonra performans düşebilir.

SharedLogId ve SharedLogPath ayarları her zaman GUID ve tüm düğümler için varsayılan paylaşılan Günlük konumu kümedeki tanımlamak için birlikte kullanılır. Varsayılan paylaşılan günlük ayarları belirli hizmet settings.xml belirtmeyen tüm reliable services için kullanılır. En iyi performans için paylaşılan günlük dosyaları, çekişmeyi azaltmak için yalnızca paylaşılan bir günlük dosyası için kullanılan disklerde yerleştirilmelidir.

SharedLogSizeInMB için tüm düğümlerde varsayılan paylaşılan günlük erişinceye için disk alanı miktarını belirtir.  SharedLogId ve SharedLogPath SharedLogSizeInMB belirtilmesi için sırayla belirtilmesi gerekmez.

## <a name="service-specific-configuration"></a>Belirli hizmet yapılandırması
Yapılandırma paketi (yapılandırma) veya hizmet uygulaması (kod) kullanarak durum bilgisi olan Reliable Services varsayılan yapılandırmaları değiştirebilirsiniz.

* **Config** -config paketi aracılığıyla yapılandırma, Microsoft Visual Studio Paket kökünde uygulamadaki her bir hizmet için yapılandırma klasörü altında oluşturulan Settings.xml dosyasının değiştirerek gerçekleştirilir.
* **Kod** -kod aracılığıyla yapılandırma, uygun seçenekleri kümesiyle ReliableStateManagerConfiguration nesnesi kullanarak bir ReliableStateManager oluşturularak gerçekleştirilir.

Varsayılan olarak, Azure Service Fabric çalışma zamanı Settings.xml dosyasında önceden tanımlı bölüm adlarını arar ve temel alınan çalışma zamanı bileşenleri oluşturma sırasında yapılandırma değerlerini kullanır.

> [!NOTE]
> Yapmak **değil** bölüm adları kod aracılığıyla hizmetinizi yapılandırmak planlamıyorsanız, Visual Studio çözümü içinde oluşturulan Settings.xml dosyasında aşağıdaki yapılandırmalardan birini silin.
> Yapılandırma bölümü veya paket adları yeniden adlandırma, bir kod değişikliği ReliableStateManager yapılandırırken gerekir.
> 
> 

### <a name="replicator-security-configuration"></a>Çoğaltıcı güvenliği yapılandırma
Çoğaltıcı güvenlik yapılandırmalarını, çoğaltma sırasında kullanılan iletişim kanalını güvenli hale getirmek için kullanılır. Başka bir deyişle, hizmetler, yüksek oranda kullanılabilir hale getirileceğini verileri de güvenli olduğundan emin olmanın birbirlerinin çoğaltma trafiği görmek mümkün olmayacaktır. Varsayılan olarak, bir boş güvenlik yapılandırma bölümü çoğaltma güvenlik engeller.

> [!IMPORTANT]
> Linux düğümlerinde sertifikaların PEM biçimli olması gerekir. Daha fazla bulma ve sertifikalar için Linux yapılandırması hakkında bilgi edinmek için [Linux'ta sertifikaları yapılandırma](./service-fabric-configure-certificates-linux.md). 
> 
> 

### <a name="default-section-name"></a>Varsayılan bölüm adı
ReplicatorSecurityConfig

> [!NOTE]
> Bu bölüm adını değiştirmek için bu hizmet için ReliableStateManager oluştururken ReliableStateManagerConfiguration Oluşturucusu replicatorSecuritySectionName parametresi geçersiz kılar.
> 
> 

### <a name="replicator-configuration"></a>Çoğaltıcı yapılandırma
Durum bilgisi olan güvenilir hizmet durumu yüksek oranda güvenilir yineleme ve durumu yerel olarak kalıcı hale yapmaktan sorumlu çoğaltıcı çoğaltıcı yapılandırmaları yapılandırın.
Varsayılan yapılandırma, Visual Studio şablon tarafından oluşturulur ve yeterli olacaktır. Bu bölümde, yineleyici ayarlamak kullanılabilir olan ek yapılandırmalar hakkında konuşuyor.

### <a name="default-section-name"></a>Varsayılan bölüm adı
ReplicatorConfig

> [!NOTE]
> Bu bölüm adını değiştirmek için bu hizmet için ReliableStateManager oluştururken ReliableStateManagerConfiguration Oluşturucusu replicatorSettingsSectionName parametresi geçersiz kılar.
> 
> 

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Kendisi için göndermeden önce bir işlem aldıktan sonra ikincil bekler, yineleyici geri bir bildirim birincil siteye süre. Bu aralıkta işlenen işlemleri için gönderilecek diğer bir onayları bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan--gerekli parametre |IP adresi ve birincil/ikincil çoğaltma çoğaltmasındaki diğer çoğaltıcılar ile iletişim kurmak için kullanacağı bağlantı noktası ayarlayın. Bu hizmet bildirimindeki bir TCP kaynak uç noktası başvurmalıdır. Başvurmak [hizmet bildirimi kaynakları](service-fabric-service-manifest-resources.md) daha fazla bilgi için bir hizmet bildiriminde uç nokta kaynakları tanımlama hakkında. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |8192 |Birincil sırasındaki işlemlerinin maksimum sayısı. Birincil çoğaltıcı tüm ikincil çoğaltıcılar alındısı sonra bir işlem yukarı serbest bırakılır. Bu değer, 64 ve 2'in kuvveti büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |16384 |İkincil sırasındaki işlemlerinin maksimum sayısı. Bir işlem yukarı durumuna Kalıcılık aracılığıyla yüksek oranda kullanılabilir yaptıktan sonra serbest bırakılır. Bu değer, 64 ve 2'in kuvveti büyük olmalıdır. |
| CheckpointThresholdInMB |MB |50 |Günlük dosyası alanının sonra durumu belirttiğinizde miktarı. |
| MaxRecordSizeInKB |KB |1024 |Çoğaltıcı günlüğünde yazabilir en büyük kayıt boyutu. Bu değerin katlarından biri 4 ile 16'dan büyük olması gerekir. |
| MinLogSizeInMB |MB |0 (belirlenen sistem) |İşlem günlüğü en küçük boyutu. Günlük bir boyuta Bu ayar aşağıdaki kesmek için izin verilmiyor. 0 çoğaltıcı en düşük günlük boyutunu belirler gösterir. Bu değeri artırmak kısmi kopyalarını ve artımlı yedeklemeler, ilgili günlük kayıtları kırpılırken azaltıldığı olasılığını beri yapmanın olasılığı artar. |
| TruncationThresholdFactor |faktörü |2 |Günlüğün hangi boyutta kesilmesi tetiklenir belirler. Kesme eşiği TruncationThresholdFactor ile çarpılmış MinLogSizeInMB tarafından belirlenir. TruncationThresholdFactor 1'den büyük olmalıdır. MinLogSizeInMB * TruncationThresholdFactor MaxStreamSizeInMB değerinden küçük olması gerekir. |
| ThrottlingThresholdFactor |faktörü |4 |Günlüğün hangi boyutta aşarak çoğaltma başlar belirler. Azaltma eşiği (MB cinsinden) en fazla tarafından belirlenir ((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)). (MB cinsinden) azaltma eşiği (MB) cinsinden kesilme eşik değerinden büyük olmalıdır. Kesme eşiği (MB cinsinden) MaxStreamSizeInMB değerinden küçük olması gerekir. |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |En fazla boyutu (MB cinsinden) belirtilen yedek günlüğü zinciri yedekleme günlükleri toplanır. Artımlı yedekleme birikmiş yedekleme günlükleri bu boyuttan daha büyük olabilir ilgili tam yedeklemeden bu yana neden olan bir yedekleme günlüğü üretir bir artımlı yedekleme istekleri başarısız olur. Böyle durumlarda, kullanıcı, tam yedek almak için gereklidir. |
| SharedLogId |GUID |"" |Bu çoğaltma ile kullanılan paylaşılan günlük dosyası tanımlamak için kullanılacak benzersiz bir GUID belirtir. Genellikle, hizmetleri bu ayarı kullanmanız gerekir. SharedLogId belirtilirse, ancak ardından SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam yol adı |"" |Bu yineleme için paylaşılan bir günlük dosyasına oluşturulacağı tam yolunu belirtir. Genellikle, hizmetleri bu ayarı kullanmanız gerekir. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmelidir. |
| SlowApiMonitoringDuration |Saniye |300 |Yönetilen API çağrıları için izleme aralığı ayarlar. Örnek: kullanıcı tarafından sağlanan yedekleme geri çağırma işlevi. Aralığı geçtikten sonra bir uyarı sistem durumu raporu için sistem durumu Yöneticisi gönderilir. |
| LogTruncationIntervalSeconds |Saniye |0 |Yapılandırılabilir aralığı günlük kesilmesi her çoğaltma üzerinde başlatılacak. Günlük, ayrıca günlük boyutu yerine süreye dayalı kesilmiş emin olmak için kullanılır. Bu ayar, ayrıca güvenilir bir sözlükte silinen girişleri temizleme zorlar. Bu nedenle zamanında silinmiş öğeler temizlenir emin olmak için kullanılabilir. |

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
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
Çoğaltma gecikmesi BatchAcknowledgementInterval denetler. (Daha fazla bildirim iletileri gönderilen ve gereken işlenen her daha az bir onayları içeren gibi) '0' değeri en düşük gecikmeyi, aktarım hızı artırılabilir sonuçlanır.
BatchAcknowledgementInterval için büyük değer, yüksek genel çoğaltma aktarım hızı, daha yüksek işlem gecikme süresi artırılabilir. Bu işlem yürütme gecikme süresi için doğrudan dönüşür.

CheckpointThresholdInMB değeri çoğaltıcı yinelemenin ayrılmış günlük dosyasında durum bilgilerini depolamak için kullanabileceğiniz disk alanı miktarını denetler. Bu varsayılan değerinden daha yüksek bir değere artırma kümesine yeni bir yineleme eklendiğinde daha hızlı yeniden yapılandırma süresi neden olabilir. Bu, günlük işlemlerinde daha fazla geçmiş kullanılabilirliği nedeniyle gerçekleşen kısmi durum transferi kaynaklanır. Bu olası kilitlenme sonrasında bir çoğaltmanın kurtarma süresini artırabilir.

MaxRecordSizeInKB ayarı günlük dosyasına çoğaltıcı tarafından yazılabilir bir kaydın en büyük boyutunu tanımlar. Çoğu durumda, varsayılan 1024-KB kayıt boyutu idealdir. Hizmet durumu bilgilerini bir parçası olarak daha büyük veri öğeleri neden oluyorsa, ancak daha sonra bu değer artırılması gerekebilir. Daha küçük kayıtları yalnızca küçük bir kayıt için ihtiyaç duyulan alanı kullanırken, 1024'ten MaxRecordSizeInKB küçülterek içinde küçük bir avantajı yoktur. Bu değer yalnızca nadir durumlarda değiştirilmesi gerekecektir bekliyoruz.

SharedLogId ve SharedLogPath ayarları her zaman varsayılan paylaşılan günlüğünden ayrı bir paylaşılan günlük düğümü için kullanmak bir hizmet sağlamak için birlikte kullanılır. En iyi verimlilik için mümkün olduğunca çok Hizmetleri aynı paylaşılan günlüğe belirtmeniz gerekir. Paylaşılan günlük dosyaları için yalnızca paylaşılan bir günlük dosyasına baş taşıma çekişmeyi azaltmak için kullanılan disklerde yerleştirilmelidir. Bu değer yalnızca nadir durumlarda değiştirilmesi gerekecektir bekliyoruz.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulamanızı Visual Studio'da hata ayıklama](service-fabric-debugging-your-application.md)
* [Reliable Services için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/dn706529.aspx)

