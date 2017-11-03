---
title: "Azure mikro ReliableDictionaryActorStateProvider ayarlarında değişiklik | Microsoft Docs"
description: "Durum bilgisi olan Azure Service Fabric aktör türü ReliableDictionaryActorStateProvider yapılandırma hakkında bilgi edinin."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: 79b48ffa-2474-4f1c-a857-3471f9590ded
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: 5dcd1b4f5a070e9a09b6f8338928d93d10227d38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Güvenilir aktörler--ReliableDictionaryActorStateProvider yapılandırma
Visual Studio Paketi kök yapılandırma klasörü altında belirtilen aktör için oluşturulan settings.xml dosyasını değiştirerek ReliableDictionaryActorStateProvider varsayılan yapılandırmasını değiştirebilirsiniz.

Azure Service Fabric çalışma zamanı settings.xml dosyasında tanımlanmış bölüm adları arar ve temeldeki çalışma zamanı bileşenleri oluşturulurken yapılandırma değerlerini kullanır.

> [!NOTE]
> Yapmak **değil** silebilir veya Visual Studio çözümünde oluşturulan settings.xml dosyasında aşağıdaki yapılandırmalardan birini bölüm adlarını değiştirebilirsiniz.
> 
> 

ReliableDictionaryActorStateProvider yapılandırmasını etkileyen genel ayarlar da vardır.

## <a name="global-configuration"></a>Genel yapılandırma
Genel yapılandırma KtlLogger bölümünde küme için küme bildiriminde belirtilir. Paylaşılan günlük konumunu ve boyutunu ve Günlükçü tarafından kullanılan genel bellek sınırları yapılandırmanızı sağlar. Küme bildiriminde değişiklik ReliableDictionaryActorStateProvider kullanan tüm hizmetleri ve durum bilgisi olan güvenilir hizmetler etkileyeceğini unutmayın.

Küme bildirimi ayarları ve tüm düğümleri ve küme Hizmetleri'nde geçerli yapılandırmaları tutan tek bir XML dosyasıdır. Dosya genellikle ClusterManifest.xml çağrılır. Küme görebilirsiniz Get-ServiceFabricClusterManifest powershell komutunu kullanarak, kümeniz için bildirim.

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobayt |8388608 |Çekirdek modunda Günlükçü yazma arabelleği bellek havuzu için ayrılacak KB minimum sayısı. Bu bellek havuzundaki durum bilgilerini diske yazma önce önbelleğe almak için kullanılır. |
| WriteBufferMemoryPoolMaximumInKB |Kilobayt |Sınırsız |Günlükçü yazma arabelleği bellek havuzundaki en büyük boyutu büyüyebilir. |
| SharedLogId |GUID |"" |SharedLogId hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümlere tüm güvenilir hizmetler tarafından kullanılan varsayılan paylaşılan günlük dosyası belirlemek için kullanmak üzere benzersiz bir GUID belirtir. SharedLogId belirtilirse, SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam yol adı |"" |Paylaşılan günlük dosyası SharedLogPath hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümlere tüm güvenilir hizmetler tarafından kullanıldığı tam yolunu belirtir. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmesi gerekir. |
| SharedLogSizeInMB |Megabayt |8192 |Statik olarak paylaşılan günlüğü ayırmak için MB disk alanı sayısını belirtir. Değer, 2048 veya daha büyük olmalıdır. |

### <a name="sample-cluster-manifest-section"></a>Örnek Küme bildirim bölümü
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Açıklamalar
Günlükçü hizmetlerine ayrılmış günlüğe yazılan güvenilir hizmet çoğaltma ile ilişkili önce durumu verileri önbelleğe alma için tüm güvenilir bir düğüm üzerindeki kullanılabilir alınamayan çekirdek bellekten ayrılmış bellek genel havuzu vardır. Havuz boyutu WriteBufferMemoryPoolMinimumInKB ve WriteBufferMemoryPoolMaximumInKB ayarları tarafından denetlenir. Bu bellek havuzundaki ilk boyutu ve bellek havuzu Küçült en düşük boyut WriteBufferMemoryPoolMinimumInKB belirtir. WriteBufferMemoryPoolMaximumInKB, bellek havuzunda büyümesine en yüksek boyutudur. Açılan her güvenilir hizmet çoğaltma belirlenen sistem miktar WriteBufferMemoryPoolMaximumInKB kadar bellek havuzunun boyutunu artırabilir. Kullanılabilenden bellek havuzundaki bellek için daha fazla isteğe bağlı ise, bellek için istekleri bellek kullanılabilir oluncaya kadar geciktirilir. Yazma arabelleği bellek havuzu için belirli bir yapılandırma çok küçük ise, bu nedenle sonra performans düşebilir.

SharedLogId ve SharedLogPath ayarlarını her zaman kümedeki tüm düğümler için varsayılan paylaşılan günlük için konum ve GUID tanımlamak için birlikte kullanılır. Varsayılan paylaşılan günlük ayarları belirli hizmet settings.xml belirtmediği tüm güvenilir hizmetler için kullanılır. En iyi performans için yalnızca paylaşılan günlük dosyası için Çekişme azaltmak için kullanılan diskler üzerinde paylaşılan günlük dosyalarını yerleştirilmelidir.

SharedLogSizeInMB varsayılan paylaşılan günlüğü tüm düğümlerde erişinceye için disk alanı miktarını belirtir.  SharedLogId ve SharedLogPath belirtilmesini SharedLogSizeInMB sırada belirtilmesi gerekmez.

## <a name="replicator-security-configuration"></a>Çoğaltıcı güvenlik yapılandırması
Çoğaltıcı güvenlik yapılandırmalarını çoğaltma sırasında kullanılır ve iletişim kanalının güvenliğini sağlamak için kullanılır. Bu hizmetler yüksek oranda kullanılabilir hale getirileceğini verileri de güvenlidir sağlama birbirlerinin çoğaltma trafiği, göremeyeceği anlamına gelir.
Varsayılan olarak, bir boş güvenlik yapılandırması bölümü çoğaltma güvenlik engeller.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırma
Çoğaltıcı yapılandırmaları aktör durumu sağlayıcısı durumu yüksek oranda güvenilir çoğaltmak ve durumu yerel olarak kalıcı yapmaktan sorumlu çoğaltıcı yapılandırmak için kullanılır.
Varsayılan yapılandırma Visual Studio şablon tarafından oluşturulan ve yeterli olacaktır. Bu bölümde çoğaltıcı ayarlamak kullanılabilir olan ek yapılandırmalar hakkında alınmaktadır.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Kendisi için göndermeden önce bir işlem aldıktan sonra ikincil bekler adresindeki çoğaltıcı geri bir bildirim için birincil süre. Bu aralık dahilinde işlenen işlemleri için gönderilmek üzere başka bir onayları bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan yok--gerekli parametre |IP adresi ve birincil/ikincil çoğaltıcı diğer çoğaltıcılar yineleme ile iletişim kurmak için kullanacağı bağlantı noktası olarak ayarlayın. Bu hizmet bildiriminde TCP kaynak uç noktası başvuruda bulunmalıdır. Başvurmak [Service manifest kaynakları](service-fabric-service-manifest-resources.md) daha fazla bilgi için uç nokta kaynakları hizmet bildiriminde tanımlama hakkında. |
| MaxReplicationMessageSize |Bayt |50 MB |Tek bir iletiye iletilen çoğaltma verilerinin en büyük boyutu. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |8192 |Birincil kuyruk işlemlerinde maksimum sayısı. Birincil çoğaltma tüm ikincil çoğaltıcılar alındısı sonra bir işlem yukarı serbest bırakılır. Bu değer 64 ve 2'in büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |16384 |İkincil kuyruk işlemlerinde maksimum sayısı. Bir işlem yukarı durumuna Kalıcılık üzerinden yüksek oranda kullanılabilir yaptıktan sonra serbest bırakılır. Bu değer 64 ve 2'in büyük olmalıdır. |
| CheckpointThresholdInMB |MB |200 |Günlük dosyası alanının geçmesi belirttiğinizde durumda tutar. |
| MaxRecordSizeInKB |KB |1024 |Çoğaltıcı günlüğüne yazabilir en büyük kayıt boyutu. Bu değer birden fazla 4 ve 16'den büyük olmalıdır. |
| OptimizeLogForLowerDiskUsage |Boole değeri |TRUE |Doğru olduğunda, günlük bir NTFS seyrek dosya kullanarak yinelemenin ayrılmış günlük dosyası oluşturulur şekilde yapılandırılır. Bu dosya için gerçek disk alanı kullanımını azaltır. Yanlış olduğunda, dosya yazma en iyi performansı sağlamak sabit ayırma ile oluşturulur. |
| SharedLogId |GUID |"" |Bu çoğaltma ile kullanılan paylaşılan günlük dosyası belirlemek için kullanmak üzere benzersiz bir GUID belirtir. Genellikle, hizmetleri, bu ayar kullanmamalısınız. SharedLogId belirtilirse, ancak ardından SharedLogPath de belirtilmesi gerekir. |
| SharedLogPath |Tam yol adı |"" |Bu çoğaltma için paylaşılan günlük dosyası oluşturulacağı tam yolunu belirtir. Genellikle, hizmetleri, bu ayar kullanmamalısınız. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmesi gerekir. |

## <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
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

## <a name="remarks"></a>Açıklamalar
BatchAcknowledgementInterval parametre çoğaltma gecikmesi denetler. (Daha fazla bildirim iletileri gerekir gönderilmesine ve işlenen her daha az onayları içeren gibi) '0' değeri, üretilen işi, en düşük olası gecikme sonuçlanır.
BatchAcknowledgementInterval için büyük değer, o kadar yüksektir genel çoğaltma üretilen işi, daha yüksek işlem gecikme artması pahasına olur. Bu işlem yürütme gecikmesi için doğrudan dönüşür.

CheckpointThresholdInMB parametre çoğaltıcı yinelemenin ayrılmış günlük dosyasında durum bilgilerini depolamak için kullanabileceğiniz disk alanı miktarını denetler. Bu varsayılan değerinden daha yüksek bir değere artırma yeni bir çoğaltma kümesine eklendiğinde daha hızlı yeniden yapılandırma sürelerine neden olabilir. Bu işlem günlüğünde daha fazla geçmiş kullanılabilirliği nedeniyle gerçekleşir kısmi durum transfer kaynaklanır. Bu olası kilitlenme sonrasında bir çoğaltmanın kurtarma süresini artırabilir.

OptimizeForLowerDiskUsage true olarak ayarlarsanız, etkin olmayan çoğaltmalar daha az disk alanı kullanırken etkin çoğaltmaları günlük dosyalarına daha fazla durum bilgilerini depolayabileceğiniz günlük dosyası alanının aşırı sağlanan olacaktır. Bu, bir düğüm üzerinde daha fazla çoğaltmaları barındıran mümkün kılar. Durum bilgisi OptimizeForLowerDiskUsage false olarak ayarlarsanız, günlük dosyalarına daha hızlı yazılır.

MaxRecordSizeInKB ayarı çoğaltıcı tarafından günlük dosyasına yazılabilir bir kayıt en büyük boyutunu tanımlar. Çoğu durumda, varsayılan 1024-KB kayıt boyutu idealdir. Hizmet durumu bilgileri parçası olarak daha büyük veri öğeleri neden olup olmadığını ancak, daha sonra bu değeri artırılacak gerekebilir. Daha küçük kayıtları yalnızca küçük kayıt için ihtiyaç duyulan alanı kullanırken, 1024'ten MaxRecordSizeInKB küçülterek az avantajı vardır. Bu değer yalnızca nadir durumlarda değiştirilmesi gerekir bekliyoruz.

SharedLogId ve SharedLogPath ayarlarını her zaman ayrı bir paylaşılan günlük varsayılan paylaşılan günlüğünden düğümü için kullanmak hizmet olmak için birlikte kullanılır. En iyi verimlilik için mümkün olduğunca çok Hizmetleri aynı paylaşılan günlük belirtmeniz gerekir. Paylaşılan günlük dosyaları, yalnızca paylaşılan günlük dosyası için baş taşıma Çekişme azaltmak için kullanılan disklerde yerleştirilmelidir. Bu değerleri yalnızca nadir durumlarda değiştirilmesi gerekir bekliyoruz.

