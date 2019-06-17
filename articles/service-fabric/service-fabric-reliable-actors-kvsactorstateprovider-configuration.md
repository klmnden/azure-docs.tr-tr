---
title: Azure Service Fabric actors kvsactorstateprovider'ı ayarlarını değiştirin | Microsoft Docs
description: Azure Service Fabric durum bilgisi olan aktör türü kvsactorstateprovider'ı yapılandırma hakkında bilgi edinin.
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: chackdan
editor: ''
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: 8b10ef18fd389179a4f5422783606c45fa2e0d32
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60728058"
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Reliable Actors--kvsactorstateprovider'ı yapılandırma
Kvsactorstateprovider'ı varsayılan yapılandırması için belirtilen aktör Config klasörü altında Microsoft Visual Studio Paket kökünde oluşturulan settings.xml dosyasının değiştirerek değiştirebilirsiniz.

Azure Service Fabric çalışma zamanı settings.xml dosyasında önceden tanımlı bölüm adlarını arar ve temel alınan çalışma zamanı bileşenleri oluşturma sırasında yapılandırma değerlerini kullanır.

> [!NOTE]
> Yapmak **değil** silebilir veya Visual Studio çözümü içinde oluşturulan settings.xml dosyasında aşağıdaki yapılandırmaları bölüm adlarını değiştirebilirsiniz.
> 
> 

## <a name="replicator-security-configuration"></a>Çoğaltıcı güvenliği yapılandırma
Çoğaltıcı güvenlik yapılandırmalarını, çoğaltma sırasında kullanılan iletişim kanalını güvenli hale getirmek için kullanılır. Bu hizmetler yüksek oranda kullanılabilir hale getirileceğini verileri de güvenli olduğundan emin olmanın birbirlerini çoğaltma trafiğini göremez anlamına gelir.
Varsayılan olarak, bir boş güvenlik yapılandırma bölümü çoğaltma güvenlik engeller.

> [!IMPORTANT]
> Linux düğümlerinde sertifikaların PEM biçimli olması gerekir. Daha fazla bulma ve sertifikalar için Linux yapılandırması hakkında bilgi edinmek için [Linux'ta sertifikaları yapılandırma](./service-fabric-configure-certificates-linux.md). 
> 

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırma
Çoğaltma, yüksek oranda güvenilir aktör durumu sağlayıcısı durumu yapmaktan sorumlu çoğaltıcı yapılandırmaları.
Varsayılan yapılandırma, Visual Studio şablon tarafından oluşturulur ve yeterli olacaktır. Bu bölümde, yineleyici ayarlamak kullanılabilir olan ek yapılandırmalar hakkında konuşuyor.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Kendisi için göndermeden önce bir işlem aldıktan sonra ikincil bekler, yineleyici geri bir bildirim birincil siteye süre. Bu aralıkta işlenen işlemleri için gönderilecek diğer bir onayları bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan--gerekli parametre |IP adresi ve birincil/ikincil çoğaltma çoğaltmasındaki diğer çoğaltıcılar ile iletişim kurmak için kullanacağı bağlantı noktası ayarlayın. Bu hizmet bildirimindeki bir TCP kaynak uç noktası başvurmalıdır. Başvurmak [hizmet bildirimi kaynakları](service-fabric-service-manifest-resources.md) daha fazla bilgi için hizmet bildiriminde uç nokta kaynakları tanımlama hakkında. |
| Retryınterval |Saniye |5 |Zaman aralığı bir işlem için bir bildirim almazsa sonra çoğaltıcı bir ileti yeniden iletir. |
| (Maxreplicationmessagesize) |Bayt |50 MB |Tek bir iletiye iletilen çoğaltma verilerinin en büyük boyutu. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |1024 |Birincil sırasındaki işlemlerinin maksimum sayısı. Birincil çoğaltıcı tüm ikincil çoğaltıcılar alındısı sonra bir işlem yukarı serbest bırakılır. Bu değer, 64 ve 2'in kuvveti büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |2048 |İkincil sırasındaki işlemlerinin maksimum sayısı. Bir işlem yukarı durumuna Kalıcılık aracılığıyla yüksek oranda kullanılabilir yaptıktan sonra serbest bırakılır. Bu değer, 64 ve 2'in kuvveti büyük olmalıdır. |

## <a name="store-configuration"></a>Store yapılandırma
Store yapılandırmaları çoğaltılmakta olan durumu kalıcı hale getirmek için kullanılan yerel deposunu yapılandırmak için kullanılır.
Varsayılan yapılandırma, Visual Studio şablon tarafından oluşturulur ve yeterli olacaktır. Bu bölümde, yerel depo ayarlamak kullanılabilir olan ek yapılandırmalar hakkında konuşuyor.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |Milisaniye |200 |Toplu işleme aralığı dayanıklı yerel depo işlemeler için en fazla ayarlar. |
| MaxVerPages |Sayfa sayısı |16384 |Yerel sürüm sayfa sayısının veritabanı depolayın. Bekleyen işlemlerin sayısını belirler. |

## <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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

