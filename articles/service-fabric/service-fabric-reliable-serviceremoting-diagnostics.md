---
title: Azure ServiceFabric tanılama ve izleme | Microsoft Docs
description: Bu makale performans sayaçları tarafından gösterilen gibi Service Fabric güvenilir ServiceRemoting çalışma zamanında performans izleme özelliklerini açıklar.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: suchiagicha
ms.openlocfilehash: d462ba0955a362c27b786ee6a5670eec20c52a22
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Tanılama ve güvenilir hizmeti uzaktan iletişim için performans izleme
Güvenilir ServiceRemoting çalışma zamanı yayar [performans sayaçları](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Bunlar ServiceRemoting nasıl çalıştığını içine bilgiler ve sorun giderme ve performans izleme ile yardımcı.


## <a name="performance-counters"></a>Performans sayaçları
Güvenilir ServiceRemoting çalışma zamanı aşağıdaki performans sayacı kategorisi tanımlar:

| Kategori | Açıklama |
| --- | --- |
| Service Fabric Service |Azure Service Fabric hizmeti uzaktan iletişim için özel sayaçlar Örneğin, ortalama isteğinin işlenmesi için geçen süre |
| Service Fabric Service Metodu |Yöntemlere özel sayaçlar ne sıklıkta bir hizmet yöntemi çağrılır doku uzaktan iletişim hizmeti tarafından Örneğin, uygulanmadı. |

Önceki kategorilerden her biri bir veya daha fazla sayaca sahiptir.

[Windows Performans İzleyicisi'ni](https://technet.microsoft.com/library/cc749249.aspx) varsayılan olarak Windows işletim sisteminde kullanılabilir uygulama, toplama ve performans sayacı verilerini görüntülemek için kullanılabilir. [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) performans sayaç verileri toplayan ve Azure tablolara karşıya yükleme için başka bir seçenektir.

### <a name="performance-counter-instance-names"></a>Performans sayacı örneği adları
Çok sayıda ServiceRemoting Hizmetleri veya bölümleri olan bir küme, çok sayıda performans sayacı örnekleri vardır. Performans sayacı örneği adları belirli bir bölüm ve hizmet yöntemi (varsa) performans sayacı örneği ile ilişkili olduğunu belirlemenize yardımcı olabilir.

#### <a name="service-fabric-service-category"></a>Service Fabric hizmeti kategorisi
Kategori için `Service Fabric Service`, sayaç örneği adları aşağıdaki biçimdedir:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dize gösterimidir. Bölüm kimliği bir GUID olduğundan ve dize gösterimi aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) biçim belirticisi "D" yöntemiyle.

*ServiceReplicaOrInstanceId* performans sayacı örneği ile ilişkili Service Fabric çoğaltma/örnek kimliği dize gösterimidir.

*ServiceRuntimeInternalID* iç kullanımı için Fabric hizmeti çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı dize gösterimidir. Bu benzersizliğini sağlamak ve diğer performans sayacı örneği adları ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Kullanıcılar, bu performans sayacı örneği adı kısmını yorumlamaya deneyin.

Aşağıdaki bir sayaç örneği adı ait bir sayacın örneğidir `Service Fabric Service` kategorisi:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

Önceki örnekte `2740af29-78aa-44bc-a20b-7e60fb783264` Service Fabric bölüm kimliği dize gösterimidir `635650083799324046` çoğaltma/InstanceId dize gösterimidir ve `5008379932` çalışma zamanı iç için oluşturan 64-bit kimliği kullanın.

#### <a name="service-fabric-service-method-category"></a>Service Fabric hizmeti yöntemi kategorisi
Kategori için `Service Fabric Service Method`, sayaç örneği adları aşağıdaki biçimdedir:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* performans sayacı örneği ile ilişkili hizmet yöntemi adı. Yöntem adı biçimi, bazı performans sayacı örneği adları en fazla uzunluğu kısıtlamalar Windows adıyla okunabilirliğini dengeleyen Fabric hizmeti çalışma zamanı mantığında göre belirlenir.

*ServiceRuntimeMethodId* iç kullanımı için Fabric hizmeti çalışma zamanı tarafından oluşturulan bir 32 bit tamsayı dize gösterimidir. Bu benzersizliğini sağlamak ve diğer performans sayacı örneği adları ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Kullanıcılar, bu performans sayacı örneği adı kısmını yorumlamaya deneyin.

*ServiceFabricPartitionID* performans sayacı örneği ile ilişkili Service Fabric bölüm kimliği dize gösterimidir. Bölüm kimliği bir GUID olduğundan ve dize gösterimi aracılığıyla oluşturulan [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) biçim belirticisi "D" yöntemiyle.

*ServiceReplicaOrInstanceId* performans sayacı örneği ile ilişkili Service Fabric çoğaltma/örnek kimliği dize gösterimidir.

*ServiceRuntimeInternalID* iç kullanımı için Fabric hizmeti çalışma zamanı tarafından oluşturulan bir 64-bit tamsayı dize gösterimidir. Bu benzersizliğini sağlamak ve diğer performans sayacı örneği adları ile çakışmayı önlemek için performans sayacı örneği adını dahil edilir. Kullanıcılar, bu performans sayacı örneği adı kısmını yorumlamaya deneyin.

Aşağıdaki bir sayaç örneği adı ait bir sayacın örneğidir `Service Fabric Service Method` kategorisi:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

Önceki örnekte `ivoicemailboxservice.leavemessageasync` yöntem adı `2` zamanının iç kullanım için oluşturulan 32-bit kimliği `89383d32-e57e-4a9b-a6ad-57c6792aa521` Service Fabric bölüm kimliği dize gösterimidir`635650083804480486` Service Fabric çoğaltma/örnek kimliği dize gösterimi ise ve `5008380` çalışma zamanı iç için oluşturulan 64-bit Kimliğini kullanın.

## <a name="list-of-performance-counters"></a>Performans sayaçlarının listesi
### <a name="service-method-performance-counters"></a>Hizmet yöntemi performans sayaçları

Güvenilir hizmet çalışma zamanı hizmet yöntemleri yürütülmesi ile ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Service Metodu |Çağrı/Sn |Hizmet yöntemi, saniye başına çağrılma sayısı |
| Service Fabric Service Metodu |Çağrı başına ortalama süre (milisaniye) |Milisaniye cinsinden hizmet yöntemin yürütülmesi için geçen süre |
| Service Fabric Service Metodu |Saniye Başına Oluşturulan Özel Durum |Kaç kez hizmet yöntemi saniye başına özel durum oluşturdu |

### <a name="service-request-processing-performance-counters"></a>Hizmet isteği işleme performans sayaçları
Bir istemci bir hizmet proxy nesnesi yöntemiyle çalıştırdığında, ağ üzerinden uzaktan iletişim hizmeti için gönderilen bir istek iletisi sonuçlanır. Hizmet İsteği iletisini işler ve istemcisine geri yanıt gönderir. Güvenilir ServiceRemoting çalışma zamanı hizmet isteği işlemiyle ilgili aşağıdaki performans sayaçları yayımlar.

| Kategori adı | Sayaç adı | Açıklama |
| --- | --- | --- |
| Service Fabric Service |Beklemedeki istek sayısı |Hizmetinde işlenmekte olan istek sayısı |
| Service Fabric Service |İstek başına ortalama süre (milisaniye) |Bir isteği işlemek için (milisaniye cinsinden) hizmeti tarafından harcanan süre |
| Service Fabric Service |İsteğin seri durumdan çıkarılması için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmeti alındığında hizmet isteği iletisi serisini kaldırmak için harcanan süre |
| Service Fabric Service |Yanıtın serileştirilmesi için geçen ortalama süre (milisaniye) |(Milisaniye cinsinden) hizmetine hizmet yanıt iletisi yanıtı istemciye gönderilmeden önce serileştirmek için harcanan süre |

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek kod](https://github.com/Azure/servicefabric-samples)
* [PerfView EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
