---
title: Azure Service Fabric aktör ReliableDictionaryActorStateProvider ayarları | Microsoft Docs
description: Azure Service Fabric durum bilgisi olan aktör türü reliabledictionaryactorstateprovider'ı yapılandırma hakkında bilgi edinin.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: chackdan
editor: ''
ms.assetid: 79b48ffa-2474-4f1c-a857-3471f9590ded
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: 4e39357a765ec85aa64055b1aa422d8d7a01c116
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669410"
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Reliable Actors--reliabledictionaryactorstateprovider'ı yapılandırma
Visual Studio Paket kökünde Config klasörü altında belirtilen aktör için oluşturulan settings.xml dosyasının değiştirerek varsayılan reliabledictionaryactorstateprovider yapılandırmasını değiştirebilirsiniz.

Azure Service Fabric çalışma zamanı settings.xml dosyasında önceden tanımlı bölüm adlarını arar ve temel alınan çalışma zamanı bileşenleri oluşturma sırasında yapılandırma değerlerini kullanır.

> [!NOTE]
> Yapmak **değil** silebilir veya Visual Studio çözümü içinde oluşturulan settings.xml dosyasında aşağıdaki yapılandırmaları bölüm adlarını değiştirebilirsiniz.
> 
> 

Reliabledictionaryactorstateprovider yapılandırmasını etkileyen genel ayarlar da vardır.

## <a name="global-configuration"></a>Genel yapılandırma
Genel yapılandırma KtlLogger bölümünün altında küme için küme bildiriminde belirtilir. Bu, paylaşılan Günlük konumu ve boyutu artı Günlükçü tarafından kullanılan genel bellek sınırlarını yapılandırmanızı sağlar. Küme bildiriminde değişiklikler reliabledictionaryactorstateprovider'ı kullanan tüm hizmetler ve güvenilir durum bilgisi olan hizmetler etkileyeceğini unutmayın.

Küme bildiriminde ayarlar ve tüm düğümleri ve kümedeki hizmetler için geçerli yapılandırmaları tutan tek bir XML dosyasıdır. Dosya, genellikle ClusterManifest.xml çağrılır. Küme görebilirsiniz Get-ServiceFabricClusterManifest powershell komutunu kullanarak kümeniz için bildirim.

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobayt |8388608 |KB, çekirdek modunda Günlükçü yazma arabellek belleği havuzu için ayrılacak en küçük sayısı. Bu bellek havuzundaki önce diske yazma durumu bilgilerini önbelleğe almak için kullanılır. |
| WriteBufferMemoryPoolMaximumInKB |Kilobayt |Sınırsız |Günlükçü için arabellek belleği havuzu yazma en büyük boyutu büyür. |
| SharedLogId |GUID |"" |SharedLogId hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümleri üzerindeki tüm güvenilir Hizmetleri tarafından kullanılan varsayılan paylaşılan günlük dosyası tanımlamak için kullanılacak benzersiz bir GUID belirtir. SharedLogId belirtilmişse SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam yol adı |"" |Paylaşılan bir günlük dosyası SharedLogPath hizmet belirli yapılandırmalarında belirtmeyin kümedeki tüm düğümleri üzerindeki tüm güvenilir Hizmetleri tarafından kullanıldığı tam yolunu belirtir. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmelidir. |
| SharedLogSizeInMB |Megabayt |8192 |MB disk alanı için paylaşılan günlüğe statik olarak ayrılacak sayısını belirtir. Değer, 2048 ya da daha büyük olmalıdır. |

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
Günlükçü güvenilir hizmet çoğaltma ile ilişkili ayrılmış günlüğe yazılan önce durumu verileri önbelleğe alma için bir düğüm tüm güvenilir hizmetlerinde kullanılabilir disk belleksiz çekirdek bellek ayrılan belleğin küresel bir havuz sahiptir. Havuz boyutunu WriteBufferMemoryPoolMinimumInKB ve WriteBufferMemoryPoolMaximumInKB ayarlarınız tarafından kontrol edilir. En düşük boyutunun bellek havuzunda küçültme ve bu bellek havuzundaki ilk boyutu WriteBufferMemoryPoolMinimumInKB belirtir. WriteBufferMemoryPoolMaximumInKB, bellek havuzunda büyüme en yüksek boyutudur. Açılan her bir güvenilir hizmet çoğaltma WriteBufferMemoryPoolMaximumInKB kadar bir sistem belirlenen miktarda bellek havuzunun boyutunu artırabilirsiniz. Kullanılabilir alandan bellek havuzundaki bellek için daha fazla isteğe bağlı ise, bellek için istekleri bellek kullanılabilir oluncaya kadar geciktirilecek. Yazma arabelleğini bellek havuzundaki belirli bir yapılandırma için çok küçük olduğunda bu nedenle daha sonra performans düşebilir.

SharedLogId ve SharedLogPath ayarları her zaman GUID ve tüm düğümler için varsayılan paylaşılan Günlük konumu kümedeki tanımlamak için birlikte kullanılır. Varsayılan paylaşılan günlük ayarları belirli hizmet settings.xml belirtmeyen tüm reliable services için kullanılır. En iyi performans için paylaşılan günlük dosyaları, çekişmeyi azaltmak için yalnızca paylaşılan bir günlük dosyası için kullanılan disklerde yerleştirilmelidir.

SharedLogSizeInMB için tüm düğümlerde varsayılan paylaşılan günlük erişinceye için disk alanı miktarını belirtir.  SharedLogId ve SharedLogPath SharedLogSizeInMB belirtilmesi için sırayla belirtilmesi gerekmez.

## <a name="replicator-security-configuration"></a>Çoğaltıcı güvenliği yapılandırma
Çoğaltıcı güvenlik yapılandırmalarını, çoğaltma sırasında kullanılan iletişim kanalını güvenli hale getirmek için kullanılır. Bu hizmetler yüksek oranda kullanılabilir hale getirileceğini verileri de güvenli olduğunu birbirlerinin çoğaltma trafiğini göremez anlamına gelir.
Varsayılan olarak, bir boş güvenlik yapılandırma bölümü çoğaltma güvenlik engeller.

> [!IMPORTANT]
> Linux düğümlerinde sertifikaların PEM biçimli olması gerekir. Daha fazla bulma ve sertifikalar için Linux yapılandırması hakkında bilgi edinmek için [Linux'ta sertifikaları yapılandırma](./service-fabric-configure-certificates-linux.md). 
> 

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırma
Çoğaltıcı yapılandırmaları, aktör durumu sağlayıcısı durumu yüksek oranda güvenilir yineleme ve durumu yerel olarak kalıcı hale yapmaktan sorumlu çoğaltıcı yapılandırmak için kullanılır.
Varsayılan yapılandırma, Visual Studio şablon tarafından oluşturulur ve yeterli olacaktır. Bu bölümde, yineleyici ayarlamak kullanılabilir olan ek yapılandırmalar hakkında konuşuyor.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Kendisi için göndermeden önce bir işlem aldıktan sonra ikincil bekler, yineleyici geri bir bildirim birincil siteye süre. Bu aralıkta işlenen işlemleri için gönderilecek diğer bir onayları bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |YOK |Varsayılan--gerekli parametre |IP adresi ve birincil/ikincil çoğaltma çoğaltmasındaki diğer çoğaltıcılar ile iletişim kurmak için kullanacağı bağlantı noktası ayarlayın. Bu hizmet bildirimindeki bir TCP kaynak uç noktası başvurmalıdır. Başvurmak [hizmet bildirimi kaynakları](service-fabric-service-manifest-resources.md) daha fazla bilgi için hizmet bildiriminde uç nokta kaynakları tanımlama hakkında. |
| (Maxreplicationmessagesize) |Bayt |50 MB |Tek bir iletiye iletilen çoğaltma verilerinin en büyük boyutu. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |8192 |Birincil sırasındaki işlemlerinin maksimum sayısı. Birincil çoğaltıcı tüm ikincil çoğaltıcılar alındısı sonra bir işlem yukarı serbest bırakılır. Bu değer, 64 ve 2'in kuvveti büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |16384 |İkincil sırasındaki işlemlerinin maksimum sayısı. Bir işlem yukarı durumuna Kalıcılık aracılığıyla yüksek oranda kullanılabilir yaptıktan sonra serbest bırakılır. Bu değer, 64 ve 2'in kuvveti büyük olmalıdır. |
| CheckpointThresholdInMB |MB |200 |Günlük dosyası alanının sonra durumu belirttiğinizde miktarı. |
| MaxRecordSizeInKB |KB |1024 |Çoğaltıcı günlüğünde yazabilir en büyük kayıt boyutu. Bu değerin katlarından biri 4 ile 16'dan büyük olması gerekir. |
| OptimizeLogForLowerDiskUsage |Boolean |gerçek |True olduğunda, günlük bir NTFS seyrek dosyası kullanarak yinelemenin ayrılmış günlük dosyası oluşturulur şekilde yapılandırılır. Bu dosya için gerçek disk alanı kullanımını azaltır. Yanlış olduğunda, dosya yazma en iyi performansı sağlamak sabit ayırmaları ile oluşturulur. |
| SharedLogId |GUID |"" |Bu çoğaltma ile kullanılan paylaşılan günlük dosyası tanımlamak için kullanılacak benzersiz bir GUID belirtir. Genellikle, hizmetleri bu ayarı kullanmanız gerekir. SharedLogId belirtilirse, ancak ardından SharedLogPath de belirtilmelidir. |
| SharedLogPath |Tam yol adı |"" |Bu yineleme için paylaşılan bir günlük dosyasına oluşturulacağı tam yolunu belirtir. Genellikle, hizmetleri bu ayarı kullanmanız gerekir. SharedLogPath belirtilirse, ancak ardından SharedLogId de belirtilmelidir. |

## <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
Çoğaltma gecikmesi BatchAcknowledgementInterval parametre denetler. (Daha fazla bildirim iletileri gönderilen ve gereken işlenen her daha az bir onayları içeren gibi) '0' değeri en düşük gecikmeyi, aktarım hızı artırılabilir sonuçlanır.
BatchAcknowledgementInterval için büyük değer, yüksek genel çoğaltma aktarım hızı, daha yüksek işlem gecikme süresi artırılabilir. Bu işlem yürütme gecikme süresi için doğrudan dönüşür.

CheckpointThresholdInMB parametre çoğaltıcı yinelemenin ayrılmış günlük dosyasında durum bilgilerini depolamak için kullanabileceğiniz disk alanı miktarını denetler. Bu varsayılan değerinden daha yüksek bir değere artırma kümesine yeni bir yineleme eklendiğinde daha hızlı yeniden yapılandırma süresi neden olabilir. Bu, günlük işlemlerinde daha fazla geçmiş kullanılabilirliği nedeniyle gerçekleşen kısmi durum transferi kaynaklanır. Bu olası kilitlenme sonrasında bir çoğaltmanın kurtarma süresini artırabilir.

OptimizeForLowerDiskUsage true olarak ayarlarsanız, etkin olmayan çoğaltmaların daha az disk alanı kullanırken daha fazla durum bilgilerini günlük dosyalarına etkin çoğaltmaları depolayabileceğiniz günlük dosyası alanının aşırı sağlanmış olur. Daha fazla düğüm üzerindeki barındıracak mümkün kılar. Durum bilgisi OptimizeForLowerDiskUsage false olarak ayarlarsanız, günlük dosyalarına daha hızlı yazılır.

MaxRecordSizeInKB ayarı günlük dosyasına çoğaltıcı tarafından yazılabilir bir kaydın en büyük boyutunu tanımlar. Çoğu durumda, varsayılan 1024-KB kayıt boyutu idealdir. Hizmet durumu bilgilerini bir parçası olarak daha büyük veri öğeleri neden oluyorsa, ancak daha sonra bu değer artırılması gerekebilir. Daha küçük kayıtları yalnızca küçük bir kayıt için ihtiyaç duyulan alanı kullanırken, 1024'ten MaxRecordSizeInKB küçülterek içinde küçük bir avantajı yoktur. Bu değer yalnızca nadir durumlarda değiştirilmesi gerekecektir bekliyoruz.

SharedLogId ve SharedLogPath ayarları her zaman varsayılan paylaşılan günlüğünden ayrı bir paylaşılan günlük düğümü için kullanmak bir hizmet sağlamak için birlikte kullanılır. En iyi verimlilik için mümkün olduğunca çok Hizmetleri aynı paylaşılan günlüğe belirtmeniz gerekir. Paylaşılan günlük dosyaları, yalnızca paylaşılan bir günlük dosyası için baş taşıma çekişmeyi azaltmak için kullanılan disklerde yerleştirilmelidir. Bu değerler yalnızca nadir durumlarda değiştirilmesi gerekecektir bekliyoruz.

