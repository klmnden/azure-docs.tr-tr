---
title: Azure ServiceFabric tanılama ve izleme | Microsoft Docs
description: Bu makalede, Service Fabric güvenilir ServiceRemoting çalışma zamanı tarafından yayınlanan performans sayaçları gibi performans izleme özelliklerini açıklar.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: chackdan
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: suchiagicha
ms.openlocfilehash: 01430c40ec9fcf1af3a463f8f86d646d15b6dd49
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925948"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Tanılama ve güvenilir hizmet uzaktan iletişim için performans izleme
Güvenilir ServiceRemoting çalışma zamanı yayan [performans sayaçları](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Bunlar ServiceRemoting nasıl çalıştığını içine ayrıntılı bilgiler sağlar ve sorun giderme ve performansı izleme ile Yardım.


## <a name="performance-counters"></a>Performans sayaçları
Güvenilir ServiceRemoting çalışma zamanı, aşağıdaki performans sayacı kategorileri tanımlar:

| Kategori | Açıklama |
| --- | --- |
| Service Fabric Service |Azure Service Fabric Service uzaktan iletişim için özel sayaçlar isteği işlemek için harcanan süre gibi ortalama |
| Service Fabric Service Metodu |Yöntemlere özel sayaçlar ne sıklıkta hizmet yöntemi çağrılır Service Fabric uzaktan iletişim hizmeti tarafından gibi uygulanır. |

Yukarıdaki kategorilerden her biri bir veya daha fazla sayaca sahiptir.

[Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) kullanılabilen Windows işletim sistemindeki varsayılan uygulama, toplama ve performans sayacı verilerini görüntülemek için kullanılabilir. [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayacı verilerini toplama ve Azure tablolara yüklemeye başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örneği adları
Çok sayıda ServiceRemoting Hizmetleri veya bölümler sahip bir küme, çok sayıda performans sayacı örnekler vardır. Performans sayacı örneği adları, belirli bir bölüm ve hizmet yöntemi (varsa) performans sayacı örneği ile ilişkili olduğunu tanımlanmasına yardımcı olabilir.

#### <a name="service-fabric-service-category"></a>Service Fabric hizmeti kategorisi
Kategori `Service Fabric Service`, şu biçimde sayaç örneği adları şunlardır:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dizesi gösterimidir. Bölüm kimliği bir GUID olduğundan dize gösterimine aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) "D" biçim belirteci ile yöntemi.

*ServiceReplicaOrInstanceId* performans sayacı örneği ile ilişkili Service Fabric çoğaltma/örnek kimliği dizesi gösterimidir.

*ServiceRuntimeInternalID* kendi iç kullanım için Service Fabric çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı, dize temsilidir. Bu, benzersiz olmasını sağlamak ve diğer performans sayacı örneği adlarla çakışma olmasını önlemek için performans sayacı örnek adı dahil edilir. Kullanıcılar bu performans sayacı örneği adı kısmı yorumlamak çalışmamanız gerekir.

Aşağıdaki örneğidir ait bir sayacın sayaç örneği adı `Service Fabric Service` kategorisi:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

Önceki örnekte `2740af29-78aa-44bc-a20b-7e60fb783264` Service Fabric bölüm kimliği dizesi gösterimidir `635650083799324046` çoğaltma/InstanceId dize gösterimini olduğu ve `5008379932` olan çalışma zamanı iç için oluşturulan 64-bit kimliği kullanın.

#### <a name="service-fabric-service-method-category"></a>Service Fabric Service metodu kategorisi
Kategori `Service Fabric Service Method`, şu biçimde sayaç örneği adları şunlardır:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* performans sayacı örneği ile ilişkili hizmet yöntemin adı. Yöntem adı biçimi, Windows performans sayacı örneği adları en fazla uzunluk kısıtlamaları adıyla okunabilirliğini dengeleyen Service Fabric çalışma zamanı mantığa göre belirlenir.

*ServiceRuntimeMethodId* kendi iç kullanım için Service Fabric çalışma zamanı tarafından oluşturulan bir 32 bit tamsayı, dize temsilidir. Bu, benzersiz olmasını sağlamak ve diğer performans sayacı örneği adlarla çakışma olmasını önlemek için performans sayacı örnek adı dahil edilir. Kullanıcılar bu performans sayacı örneği adı kısmı yorumlamak çalışmamanız gerekir.

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dizesi gösterimidir. Bölüm kimliği bir GUID olduğundan dize gösterimine aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) "D" biçim belirteci ile yöntemi.

*ServiceReplicaOrInstanceId* performans sayacı örneği ile ilişkili Service Fabric çoğaltma/örnek kimliği dizesi gösterimidir.

*ServiceRuntimeInternalID* kendi iç kullanım için Service Fabric çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı, dize temsilidir. Bu, benzersiz olmasını sağlamak ve diğer performans sayacı örneği adlarla çakışma olmasını önlemek için performans sayacı örnek adı dahil edilir. Kullanıcılar bu performans sayacı örneği adı kısmı yorumlamak çalışmamanız gerekir.

Aşağıdaki örneğidir ait bir sayacın sayaç örneği adı `Service Fabric Service Method` kategorisi:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

Önceki örnekte `ivoicemailboxservice.leavemessageasync` yöntem adı `2` çalışma zamanının iç kullanım için oluşturulan 32-bit kimliği `89383d32-e57e-4a9b-a6ad-57c6792aa521` Service Fabric bölüm kimliği dizesi gösterimidir`635650083804480486` olan dize gösterimi Service Fabric çoğaltma/örnek kimliği ve `5008380` çalışma zamanı iç için oluşturulan 64-bit Kimliğini kullanın.

## <a name="list-of-performance-counters"></a>Performans sayaçları listesi
### <a name="service-method-performance-counters"></a>Hizmeti yöntemi performans sayaçları

Güvenilir hizmet çalışma zamanı hizmet yöntemleri çalıştırmayla ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Service Metodu |Çağrı/Sn |Hizmet yöntemi, saniye başına çağrılma sayısı |
| Service Fabric Service Metodu |Çağrı başına ortalama süre (milisaniye) |Milisaniye cinsinden service metodunu yürütmek için harcanan süre |
| Service Fabric Service Metodu |Saniye Başına Oluşturulan Özel Durum |Kaç kez hizmet yöntemi saniye başına özel durum oluşturdu |

### <a name="service-request-processing-performance-counters"></a>Hizmet isteği işleme performans sayaçları
Bir istemci bir hizmeti proxy nesnesi aracılığıyla bir yöntemi çağırdığında, ağ üzerinden uzaktan iletişim hizmetine gönderilen bir istek iletisi ile sonuçlanır. Hizmet isteği iletiyi işler ve istemcisine geri yanıt gönderir. Güvenilir ServiceRemoting çalışma zamanı hizmet isteği işlemiyle ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Service |Beklemedeki istek sayısı |Hizmette işlenmekte olan istek sayısı |
| Service Fabric Service |İstek başına ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmeti tarafından bir isteği işlemek için harcanan süre |
| Service Fabric Service |İsteğin seri durumdan çıkarılması için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmeti alındığında hizmet istek iletisi seri durumdan çıkarmak için harcanan süre |
| Service Fabric Service |Yanıtın serileştirilmesi için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmetine hizmet yanıt iletisi yanıtı istemciye gönderilmeden önce seri hale getirmek için harcanan süre |

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek kod](https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0)
* [EventSource sağlayıcıları Perfview'de Aç](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
