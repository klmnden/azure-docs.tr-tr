---
title: Azure Service Fabric Platform düzeyi izleme | Microsoft Docs
description: Platform düzeyinde etkinlikler ve izleme ve tanılama Azure Service Fabric kümeleri için kullanılan günlükleri hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 96bbb221f5fa133ee88a09d489627e3d2f9b0713
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409195"
---
# <a name="monitoring-the-cluster-and-platform"></a>Platform ve küme izleme

Donanım ve küme beklendiği gibi davrandığından olup olmadığını belirlemek için platform düzeyinde izlemek önemlidir. Service Fabric, bir donanım hatası sırasında çalışan uygulamalar tutabilirsiniz, ancak bir hata olup olmadığını tanılamak yine de uygulamanın veya altyapının yapılıyor. Kümelerinize daha iyi, kapasiteyi planlamak için ekleme veya kaldırma donanım hakkında kararlarında yardımcı ayrıca izlemeniz gerekir.

Service Fabric olarak birkaç yapılandırılmış platform olaylarını sunan "[Service Fabric olayları](service-fabric-diagnostics-events.md)," Eventstore'a ve çeşitli Kanallar,-hazır oturum. 

Eventstore'a kümenizin olayları (varlıklar, küme, düğümler, uygulamaları, hizmetleri, bölümler, çoğaltmalar ve kapsayıcılar dahil) başına varlık olarak erişmenizi sağlar ve REST API'leri ve Service Fabric istemci kitaplığı kullanıma sunar. Eventstore'a geliştirme/test kümelerinizi izlemek için kullanın ve üretim kümeleri durumu zaman içinde nokta anlayış alma. Şu anda hakkında daha fazla bilgiyi [Eventstore'a genel bakış](service-fabric-diagnostics-eventstore.md).

Service Fabric,-üretim kümelerinizi izlemek bir işlem hattı ayarlamak için hazır aşağıdaki günlük kanalları da sağlar:

* [**İşletimsel**](service-fabric-diagnostics-event-generation-operational.md)  
Service Fabric ve olaylar, yaklaşan bir düğüm, dağıtılmakta olan yeni bir uygulama veya yükseltme bir geri alma dahil olmak üzere, küme tarafından gerçekleştirilen üst düzey işlemleri vs.

* **Ayrıntılı işlemsel-**  
Sistem durumu raporlarının ve Yük Dengeleme kararları.

* **Veri & Mesajlaşma**  
Kritik günlükleri ve ileti gönderme (şu anda yalnızca ReverseProxy) oluşturulan olayları ve veri yolu (reliable services modellerine).

* **Veri ve ayrıntılı bir ileti sistemi-**  
Veri ve mesajlaşma (Bu kanal olayların çok yüksek hacimli sahiptir) kümedeki tüm kritik olmayan günlükleri içeren ayrıntılı kanal.

Bunlara ek olarak, var. iki sağlanan yapılandırılmış EventSource kanalları yanı sıra destek amacıyla topladığımız günlükleri

* [Reliable Services olayları](service-fabric-reliable-services-diagnostics.md)  
Programlama modeli belirli olayları.

* [Reliable Actors olayları](service-fabric-reliable-actors-diagnostics.md)  
Programlama modeli belirli olayları ve performans sayaçları.

* Destek günlükleri  
Sistem günlükleri yalnızca bizim tarafımızdan desteği sağlamak amacıyla Service Fabric tarafından oluşturulur.

Bu çeşitli kanallar önerilir platform düzeyinde günlüğe kaydetme çoğunu kapsar. Platform düzeyinde günlüğe kaydetme iyileştirmek için daha iyi sistem durumu modeli anlama ve özel durum raporları ekleme ve ekleme yatırım özel göz önünde bulundurun **performans sayaçları** etkisini gerçek zamanlı bir anlayış oluşturmak için Hizmetler ve uygulamalar küme üzerinde.

Bu günlükler yararlanmak için küme oluşturma sırasında "tanılama" etkinleştirildiğini önerilir. Küme dağıtıldığında tanılamayı etkinleştirerek Windows Azure Diagnostics de daha fazla anlatıldığı gibi veri depolamak ve işlemsel, Reliable Services ve Reliable actors kanalları onaylamak bulabildiği [Azure ile olayları toplama Tanılama](service-fabric-diagnostics-event-aggregation-wad.md).

## <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric sistem durumunu ve yük raporlama

Service Fabric, bu makalelerinde ayrıntılı açıklanan kendi sistem durumu modeli vardır:

- [Service Fabric sistem durumu izlemeye giriş](service-fabric-health-introduction.md)
- [Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Özel Service Fabric durum raporları ekleme](service-fabric-report-health.md)
- [Service Fabric sistem durumu raporlarını görüntüleme](service-fabric-view-entities-aggregated-health.md)

Sistem durumu izleme, bir hizmeti birden çok yönleri için kritik öneme sahiptir. Service Fabric adlandırılmış uygulama yükseltme yaparken, sistem durumu izleme özellikle önemlidir. Her bir yükseltme etki alanı hizmeti yükseltilir ve müşterilerinizin kullanımına sonra dağıtım için bir sonraki yükseltme etki alanına taşınmadan önce yükseltme etki alanı sistem durumu denetimleri geçmesi gerekir. İyi sistem durumu karşılanamıyorsa, böylece uygulamanın bilinen bir iyi durumda dağıtım geri alınır. Çoğu müşteri, bazı müşteriler Hizmetleri geri alınacak önce etkilenebilecek olsa da, bir sorunla karşılaşırsınız olmaz. Ayrıca, bir çözüm oldukça hızlı bir şekilde ve İnsan eylemden beklemek zorunda kalmadan gerçekleşir. Dağıtım sorunları için hizmetinizin olduğundan kodunuz daha dayanıklı dahil daha fazla sistem durumu denetimleri.

Bir diğer unsuru hizmet sistem durumu hizmetinden alınan ölçümleri bildiriyor. Kaynak kullanımı dengelemek için kullanıldığından ölçümleri Service Fabric'te önemlidir. Ölçümleri de sistem durumu göstergesi olabilir. Örneğin, birçok hizmet olan bir uygulama olabilir ve her örneği bir istek ikinci (RP'ler) ölçüm raporlar. Bir hizmetin başka bir hizmete daha fazla kaynak kullanıyorsa, Service Fabric küme bile kaynak kullanımını sağlamak için geçici hizmet örnekleri taşır. Kaynak kullanımını nasıl çalıştığına ilişkin daha ayrıntılı açıklaması için bkz. [kaynak tüketimini yönetmek ve ölçümler ile Service Fabric'de yük](service-fabric-cluster-resource-manager-metrics.md).

Ölçümler, ayrıca hizmet uygulamanızın performansı hakkında bilgiler size yardımcı olabilir. Zamanla, hizmet içinde beklenen parametrelerin çalıştığını denetlemek için ölçümleri kullanabilirsiniz. Eğilimleri Pazartesi sabahı 9'da RPS ortalama 1000 olduğunu gösterirse, örneğin, ardından RPS 500'altına veya üstüne 1.500 ise sizi uyaran bir sistem durumu raporu ayarlayabilirsiniz. Her şey mükemmel bir şekilde hassas olabilir, ancak müşterilerinize harika bir deneyim yaşadığınız emin olmak için bir görünüm olabilir. Hizmetiniz bir dizi sistem durumu denetimi amacıyla bildirilebilir, ancak kaynak Dengeleme kümesinin etkilemeyen ölçümleri tanımlayabilirsiniz. Bunu yapmak için ölçüm ağırlık sıfır olarak ayarlayın. Tüm ölçümleri sıfır ağırlığı ile başlayın ve kümeniz için Dengeleme kaynak ölçüm ağırlığı nasıl etkilediğini anladığınızdan emin olana kadar ağırlığı artırmak değil öneririz.

> [!TIP]
> Çok fazla ağırlıklı ölçümleri kullanmayın. Neden hizmet örnekleri Dengeleme için Taşınmakta anlamak zor olabilir. Bazı ölçümler uzun yol kat etmiştir!

Uygulamanızın performansını ve sistem durumu belirtebilir herhangi bir bilgi, ölçümleri ve sistem durumu raporlarını bir adaydır. Bir CPU performans sayacı düğümünüzü nasıl kullanılır söyleyebilirsiniz, ancak bu, birden çok hizmet tek bir düğümde çalışmıyor olabilir çünkü belirli bir hizmet olup iyi ve sunmayacaktır. Ancak, RPS, işlenen, öğeler ölçümleri ister ve tüm istek gecikme süresi, belirli bir hizmet durumunu gösterebilir.

## <a name="service-fabric-support-logs"></a>Service Fabric destek günlükleri

Destek günlükleri, Azure Service Fabric kümenizi ile ilgili Yardım için Microsoft desteğine başvurun. Gerekirse, neredeyse her zaman gereklidir. Kümenizi Azure'da barındırılıyorsa destek günlükleri otomatik olarak yapılandırılır ve küme oluşturulmasının bir parçası toplanır. Günlükler, kümenin kaynak grubundaki bir adanmış depolama hesabında depolanır. Depolama hesabı, sabit bir adı yok, ancak hesabında blob kapsayıcıları ve tabloları ile başlayan adlarla görürsünüz *fabric*. Günlük koleksiyonlar için tek başına küme ayarlama hakkında daha fazla bilgi için bkz: [oluştur ve tek başına Azure Service Fabric kümesini yönetme](service-fabric-cluster-creation-for-windows-server.md) ve [KümeyapılandırmaayarlarıtekbaşınaWindows](service-fabric-cluster-manifest.md). Tek başına Service Fabric örnekleri için bir yerel dosya paylaşımına günlükleri gönderilmelidir. İşiniz **gerekli** için bu günlükleri için destek, ancak bunlar amaçlanmayan kullanılabilir olması için Microsoft müşteri destek ekibinin dışındaki hiç kimse tarafından.

## <a name="measuring-performance"></a>Performansı ölçme

Kümenizin performansını anlamanıza yardımcı olur nasıl kümenizi ölçeklendirme etrafında yükleme ve sürücü kararları işleyebilir (bir küme ölçeklendirme hakkında daha fazla bilgi bkz [azure'da](service-fabric-cluster-scale-up-down.md), veya [şirket içi](service-fabric-cluster-windows-server-add-remove-nodes.md)). Performans verilerini de siz veya uygulama ve hizmetlerinizin, gelecekte günlüklerini analiz ederken sürmüş olabileceğini eylemleri karşılaştırıldığında yararlıdır. 

Service Fabric kullanırken toplamak için performans sayaçları listesi için bkz. [Service fabric'te performans sayaçları](service-fabric-diagnostics-event-generation-perf.md)

Kümeniz için performans verilerini toplama yukarı ayarlayabilirsiniz iki genel yolu vardır:

* **Bir aracı kullanarak**  
Bu, bir makineden performans toplama aracıları genellikle toplanabilir olası performans ölçümlerinin listesi olduğundan, tercih edilen yoludur ve ölçümleri toplamak veya değiştirmek istediğiniz seçmek için görece kolay bir işlemdir. Hakkında bilgi edinin [Service Fabric için Log Analytics aracısını nasıl yapılandıracağınızı öğrenmek](service-fabric-diagnostics-event-analysis-oms.md) ve [Log Analytics aracısını ayarlama ayarlama](../log-analytics/log-analytics-windows-agent.md) olan bir tür İzleme Aracısı Log Analytics aracısını hakkında daha fazla bilgi için makaleleri performans veri kümesi Vm'lerini ve dağıtılan kapsayıcılar için çekme yapabileceksiniz.

* **Performans sayaçları için bir tablo yazmak için tanılamayı yapılandırma**  
Azure'da kümeler için bu, kümenizdeki VM'lerin uygun performans sayaçlarını alacak şekilde Azure Tanılama yapılandırmasını değiştirme ve tüm kapsayıcı dağıtımı, docker istatistiği ' seçmek etkinleştirmeden anlamına gelir. Yapılandırma hakkında okuyun [WAD performans sayaçları](service-fabric-diagnostics-event-aggregation-wad.md) performans sayacı toplama ayarlamanız, Service fabric'te.

## <a name="next-steps"></a>Sonraki adımlar

Günlükler ve olaylar için herhangi bir analiz platform gönderilmeden önce toplanması gerekir. Hakkında bilgi edinin [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) ve [WAD](service-fabric-diagnostics-event-aggregation-wad.md) bazı önerilen seçenekleri daha iyi anlamak için.
