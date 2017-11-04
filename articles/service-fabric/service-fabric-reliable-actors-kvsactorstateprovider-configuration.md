---
title: "Azure mikro KVSActorStateProvider ayarlarında değişiklik | Microsoft Docs"
description: "Durum bilgisi olan Azure Service Fabric aktör türü KVSActorStateProvider yapılandırma hakkında bilgi edinin."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/2/2017
ms.author: sumukhs
ms.openlocfilehash: d3424aa7a8e0f6011bbef4aa61274c1f598f5c86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Güvenilir aktörler--KVSActorStateProvider yapılandırma
Belirtilen aktör için Microsoft Visual Studio Paketi kök yapılandırma klasörü altında oluşturulan settings.xml dosyasını değiştirerek KVSActorStateProvider varsayılan yapılandırmasını değiştirebilirsiniz.

Azure Service Fabric çalışma zamanı settings.xml dosyasında tanımlanmış bölüm adları arar ve temeldeki çalışma zamanı bileşenleri oluşturulurken yapılandırma değerlerini kullanır.

> [!NOTE]
> Yapmak **değil** silebilir veya Visual Studio çözümünde oluşturulan settings.xml dosyasında aşağıdaki yapılandırmalardan birini bölüm adlarını değiştirebilirsiniz.
> 
> 

## <a name="replicator-security-configuration"></a>Çoğaltıcı güvenlik yapılandırması
Çoğaltıcı güvenlik yapılandırmalarını çoğaltma sırasında kullanılır ve iletişim kanalının güvenliğini sağlamak için kullanılır. Bu hizmetler yüksek oranda kullanılabilir hale getirileceğini verileri de güvenli olduğundan emin olmanın birbirlerinin çoğaltma trafiği, göremeyeceği anlamına gelir.
Varsayılan olarak, bir boş güvenlik yapılandırması bölümü çoğaltma güvenlik engeller.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Çoğaltıcı yapılandırma
Çoğaltıcı yapılandırmaları aktör durumu sağlayıcısı durumu yüksek oranda güvenilir yapmaktan sorumlu çoğaltıcı yapılandırın.
Varsayılan yapılandırma Visual Studio şablon tarafından oluşturulan ve yeterli olacaktır. Bu bölümde çoğaltıcı ayarlamak kullanılabilir olan ek yapılandırmalar hakkında alınmaktadır.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Saniye |0.015 |Kendisi için göndermeden önce bir işlem aldıktan sonra ikincil bekler adresindeki çoğaltıcı geri bir bildirim için birincil süre. Bu aralık dahilinde işlenen işlemleri için gönderilmek üzere başka bir onayları bir yanıt olarak gönderilir. |
| ReplicatorEndpoint |Yok |Varsayılan yok--gerekli parametre |IP adresi ve birincil/ikincil çoğaltıcı diğer çoğaltıcılar yineleme ile iletişim kurmak için kullanacağı bağlantı noktası olarak ayarlayın. Bu hizmet bildiriminde TCP kaynak uç noktası başvuruda bulunmalıdır. Başvurmak [Service manifest kaynakları](service-fabric-service-manifest-resources.md) daha fazla bilgi için uç nokta kaynakları hizmet bildiriminde tanımlama hakkında. |
| Retryınterval |Saniye |5 |Süre bir işlem için bir onay almazsa geçmesi çoğaltıcı bir ileti yeniden iletir. |
| MaxReplicationMessageSize |Bayt |50 MB |Tek bir iletiye iletilen çoğaltma verilerinin en büyük boyutu. |
| MaxPrimaryReplicationQueueSize |İşlem sayısı |1024 |Birincil kuyruk işlemlerinde maksimum sayısı. Birincil çoğaltma tüm ikincil çoğaltıcılar alındısı sonra bir işlem yukarı serbest bırakılır. Bu değer 64 ve 2'in büyük olmalıdır. |
| MaxSecondaryReplicationQueueSize |İşlem sayısı |2048 |İkincil kuyruk işlemlerinde maksimum sayısı. Bir işlem yukarı durumuna Kalıcılık üzerinden yüksek oranda kullanılabilir yaptıktan sonra serbest bırakılır. Bu değer 64 ve 2'in büyük olmalıdır. |

## <a name="store-configuration"></a>Depolama yapılandırması
Depolama yapılandırmaları çoğaltılmakta olan durumu sürdürmek için kullanılan yerel deposunu yapılandırmak için kullanılır.
Varsayılan yapılandırma Visual Studio şablon tarafından oluşturulan ve yeterli olacaktır. Bu bölümde yerel deposu ayarlamak kullanılabilir olan ek yapılandırmalar hakkında alınmaktadır.

### <a name="section-name"></a>Bölüm adı
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Yapılandırma adları
| Ad | Birim | Varsayılan değer | Açıklamalar |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |milisaniye |200 |Toplu işleme aralığı dayanıklı yerel depo yürütme için maksimum ayarlar. |
| MaxVerPages |Sayfa sayısı |16384 |En fazla yerel sürüm sayfa sayısını veritabanına depolar. Bekleyen işlemlerin sayısını belirler. |

## <a name="sample-configuration-file"></a>Örnek yapılandırma dosyası
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
BatchAcknowledgementInterval parametre çoğaltma gecikmesi denetler. (Daha fazla bildirim iletileri gerekir gönderilmesine ve işlenen her daha az onayları içeren gibi) '0' değeri, üretilen işi, en düşük olası gecikme sonuçlanır.
BatchAcknowledgementInterval için büyük değer, o kadar yüksektir genel çoğaltma üretilen işi, daha yüksek işlem gecikme artması pahasına olur. Bu işlem yürütme gecikmesi için doğrudan dönüşür.

