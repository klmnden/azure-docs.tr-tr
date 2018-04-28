---
title: Azure Service Fabric platformundan düzey izleme | Microsoft Docs
description: Platform düzeyi olayları ve günlükleri izlemek ve Azure Service Fabric kümeleri tanılamak için kullanılan hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 1ba02afa775343f496a2b5fec98699e593a330ba
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="monitoring-the-cluster-and-platform"></a>Küme ve platform izleme

Donanım ve küme beklendiği gibi davranmakta olup olmadığını belirlemek için platform düzeyinde izlemek önemlidir. Service Fabric bir donanım hatası sırasında çalışan uygulamaları kullanmaya devam edebilir, ancak yine de bir hata yaşamadığınızı gerekiyorsa rağmen bir uygulama veya altyapının oluşmalıdır. Daha iyi, kapasiteyi planlamak üzere kümelerinizi ekleme veya kaldırma donanım hakkında kararlar yardımcı ayrıca izlemeniz gerekir.

Service Fabric olarak çeşitli yapılandırılmış platform olaylarını gösterir "[Service Fabric olayları](service-fabric-diagnostics-events.md)," EventStore ve çeşitli kanalları out-of--box oturum açın. 

EventStore kümenizin olaylarına (küme, düğüm, uygulamaları, hizmetleri, bölümler, çoğaltmaları ve kapsayıcıları gibi varlıklar) varlık başına temelinde erişmenizi ve REST API'leri ve Service Fabric istemci kitaplığı aracılığıyla kullanıma sunar. Geliştirme ve test kümelerinizi izlemek için EventStore kullanın ve üretim kümelerinizi durumunu zaman içinde nokta anlayış alma. Şu anda hakkında daha fazla bilgiyi [EventStore genel bakış](service-fabric-diagnostics-eventstore.md).

Service Fabric ayrıca out-of--üretim kümelerinizi izlemek bir işlem hattı ayarlamak için box aşağıdaki günlük kanalları sağlar:

* [**İşletimsel**](service-fabric-diagnostics-event-generation-operational.md)  
Service Fabric ve yaklaşan bir düğüm, dağıtılmakta olan yeni bir uygulama veya bir yükseltme geri alma için olaylar dahil olmak üzere, küme tarafından gerçekleştirilen üst düzey işlemler vs.

* **İşlem - ayrıntılı**  
Sistem durumu raporları ve Yük Dengeleme kararları.

* **Veri & Mesajlaşma**  
Kritik günlükleri ve mesajlaşma (şu anda yalnızca ReverseProxy) oluşturulan olaylar ve veri yolu (güvenilir hizmetler modeller).

* **Veri & ayrıntılı ileti-**  
Veri ve mesajlaşma (Bu kanal olayları çok yüksek hacimli sahiptir) kümedeki tüm kritik olmayan günlükleri içeren ayrıntılı kanalı.

Bunlara ek olarak, var. iki sağlanan yapılandırılmış EventSource kanalları yanı sıra destek amaçlarıyla topladığımız günlükleri

* [Reliable Services olayları](service-fabric-reliable-services-diagnostics.md)  
Programlama modeli belirli olaylar.

* [Reliable Actors olayları](service-fabric-reliable-actors-diagnostics.md)  
Programlama modeli belirli olaylar ve performans sayaçları.

* Destek günlükleri  
Yalnızca tarafımızca desteği sağlama kullanılacak Service Fabric tarafından oluşturulan sistem günlükleri.

Bu çeşitli kanalları önerilir platform düzeyinde günlüğe kaydetme çoğunu kapsar. Platform düzeyinde günlüğe kaydetme artırmak için daha iyi sistem durumu modeli anlama ve özel durum raporları ekleme ve ekleme yatırım özel göz önünde bulundurun **performans sayaçları** etkisini gerçek zamanlı bir anlayış oluşturmak için Hizmetler ve uygulamalar küme üzerinde.

Bu günlükler yararlanmak için küme oluşturma sırasında "tanılama" etkinleştirildiğini kullanmamanız önerilir. Kümeyi dağıttığınızda tanılamayı açarak, Windows Azure Diagnostics içinde daha fazla anlatıldığı gibi veri depolamak ve işlemsel, Reliable Services ve Reliable actors kanalları kabul yapabiliyor [toplama Azure ile olayları Tanılama](service-fabric-diagnostics-event-aggregation-wad.md).

## <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric sistem durumu ve yük raporlama

Service Fabric ayrıntılı bu makalelerde açıklanan kendi sistem durumu modeli vardır:

- [Service Fabric sistem durumu izlemeye giriş](service-fabric-health-introduction.md)
- [Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Özel Service Fabric sistem durumu rapor ekleme](service-fabric-report-health.md)
- [Service Fabric sistem durumu raporlarını görüntüle](service-fabric-view-entities-aggregated-health.md)

Sistem durumu izleme yapmakta bir hizmet birden çok yönlerini için kritik öneme sahiptir. Service Fabric adlandırılmış uygulama yükseltme yaparken, sistem durumu izleme özellikle önemlidir. Her bir yükseltme etki alanı hizmetinin yükseltilir ve müşterileriniz için kullanılabilir olduktan sonra dağıtım için bir sonraki yükseltme etki alanına gitmeden önce yükseltme etki alanı sistem durumu denetimlerinin geçmesi gerekir. İyi sistem durumu elde, uygulama bilinen, iyi bir durumda olmasını sağlamak dağıtım geri alınır. Müşterilerin çoğu, hizmetlerin geri önce bazı müşteriler etkilenebilir rağmen bir sorunla karşılaşırsınız olmaz. Ayrıca, bir çözüm oldukça hızlı bir şekilde ve İnsan olan bir operatörü eylemden beklemek zorunda kalmadan oluşur. Hizmetiniz için dağıtım sorunları olan, kodunuzu daha esnektir içine dahil daha fazla sistem durumu denetler.

Bir diğer unsuru hizmet sistem durumu hizmetinden ölçümleri bildiriyor. Kaynak kullanımı dengelemek için kullanıldığından ölçümleri Service Fabric önemlidir. Ölçümleri de sistem durumu bir göstergesi olabilir. Örneğin, birçok Hizmetleri olan bir uygulama olabilir ve ikinci (RPS) ölçüm başına bir isteği her örnek raporlar. Bir hizmet başka bir hizmete daha fazla kaynak kullanıyorsa, Service Fabric bile kaynak kullanımını korumak denemek için küme, geçici hizmet örnekleri taşır. Kaynak kullanımını nasıl çalıştığını daha ayrıntılı açıklaması için bkz: [kaynak tüketimini yönetmek ve ölçümler ile Service Fabric yük](service-fabric-cluster-resource-manager-metrics.md).

Ölçümleri hizmetinizin nasıl gerçekleştirmekte içine kavrayışı de yardımcı olabilir. Zamanla, hizmet içinde beklenen parametreler çalıştığını denetlemek için ölçümleri kullanabilirsiniz. Eğilimleri 9 sabah Pazartesi sabahı RPS ortalama 1.000 olduğunu gösterirse, örneğin, ardından RPS 500 altında veya üstünde 1.500 ise, sizi uyarır bir sistem durumu raporu ayarlamanız. Her şey mükemmel ince olabilir, ancak müşterilerinizin harika bir deneyim yaşıyor emin olmak için bir görünüm olabilir. Hizmet sistem durumu denetimi amacıyla bildirilebilir, ancak kaynak Dengeleme kümesini etkilemeyen ölçümleri kümesi tanımlayabilirsiniz. Bunu yapmak için ölçüm ağırlığı sıfır olarak ayarlayın. Tüm ölçümleri sıfır ağırlığını ile başlatın ve kaynak kümeniz için Dengeleme ölçüm ağırlığı nasıl etkilediğini anladığınızdan emin olana kadar ağırlık artırmak değil öneririz.

> [!TIP]
> Çok fazla ağırlıklı ölçümleri kullanmayın. Neden hizmet örnekleri Dengeleme için taşınan anlaşılması zor olabilir. Birkaç ölçümleri depoladıysanız gidebilirsiniz!

Sistem durumunu ve uygulamanızın performansını belirtebilir herhangi bir bilgi ölçümleri ve sistem durumu raporları için bir adaydır. CPU performans sayacı düğümünüz nasıl kullandığı anlayabilirsiniz, ancak bu, tek bir düğümünde birden fazla hizmet çalışmıyor olabilir çünkü belirli bir hizmet sistem durumunun iyi olup söyler. Ancak, RPS, işlenen, öğeler ölçümleri ister ve tüm istek gecikme süresi belirli bir hizmet durumunu gösterebilir.

## <a name="service-fabric-support-logs"></a>Service Fabric destek günlükleri

Azure Service Fabric kümesi ile ilgili Yardım için Microsoft Destek'e başvurun gerekiyorsa, destek günlükleri neredeyse her zaman gereklidir. Kümenizi Azure üzerinde barındırılıyorsa, destek günlükleri otomatik olarak yapılandırılır ve küme oluşturma bir parçası olarak toplanır. Günlükler, küme kaynak grubu ayrılmış bir depolama hesabında depolanır. Depolama hesabı bir sabit adına sahip değil, ancak hesabında blob kapsayıcıları ve tabloları ile başlayan adlarla gördüğünüz *doku*. Tek başına küme için günlük koleksiyonları ayarlama hakkında daha fazla bilgi için bkz: [Oluştur bir tek başına Azure Service Fabric kümesi ve yönetebileceğiniz](service-fabric-cluster-creation-for-windows-server.md) ve [tek başına Windows için yapılandırma ayarları küme](service-fabric-cluster-manifest.md). Tek başına Service Fabric örnekleri için bir yerel dosya paylaşımına günlükleri gönderilmelidir. Olduğunuz **gerekli** için bu günlükleri için destek, ancak bunlar amaçlanmayan kullanılabilir Microsoft müşteri destek ekibi dışında herhangi bir kişi tarafından.

## <a name="measuring-performance"></a>Performansı ölçme

Kümenizi ölçü performansını nasıl kümenizi ölçeklendirme geçici yük ve sürücü kararları işleyebilen anlamanıza yardımcı olur (küme ölçeklendirme hakkında daha fazla [azure'da](service-fabric-cluster-scale-up-down.md), veya [şirket içi](service-fabric-cluster-windows-server-add-remove-nodes.md)). Performans verileri de, veya, uygulamalar ve hizmetler, gelecekte günlüklerini analiz ederken almış olabilir eylemlerinde karşılaştırıldığında yararlıdır. 

Service Fabric kullanırken toplamak için performans sayaçları listesi için bkz: [Service Fabric performans sayaçları](service-fabric-diagnostics-event-generation-perf.md)

Kümeniz için performans verilerini toplama yukarı ayarlayabilirsiniz iki genel yolu şunlardır:

* **Bir aracı kullanma**  
Bu, aracıları toplanabilir olası performans ölçümleri listesi genellikle sahip olduğu bir makineden performans toplama, tercih edilen yoludur ve toplama veya bunları değiştirmek istediğiniz ölçümler seçmek için görece olarak daha kolay bir işlemdir. Hakkında bilgi edinin [için Service Fabric OMS yapılandırma](service-fabric-diagnostics-event-analysis-oms.md) ve [OMS Windows aracınızı ayarlama](../log-analytics/log-analytics-windows-agent.md) performansını çekme mümkün olan bir tür İzleme Aracısı OMS Aracısı hakkında daha fazla bilgi makaleleri küme sanal makineleri ve dağıtılan kapsayıcıları için verileri.

* **Performans sayaçları için bir tablo yazmak için tanılama yapılandırılıyor**  
Azure ile ilgili daha fazla kümeler için bu kümenizdeki vm'lerden uygun performans sayaçları kuruluyor almak için Azure Tanılama yapılandırmasını değiştirme ve herhangi bir kapsayıcıdaki dağıtacaksanız docker istatistikleri almak etkinleştirmeden anlamına gelir. Yapılandırma hakkında okuyun [WAD performans sayaçları](service-fabric-diagnostics-event-aggregation-wad.md) performans sayacı toplamayı ayarlama için Service Fabric içinde.

## <a name="next-steps"></a>Sonraki adımlar

Günlüklerini ve olayları analiz platformdan gönderilmeden önce toplanması gerekir. Hakkında bilgi edinin [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) ve [WAD](service-fabric-diagnostics-event-aggregation-wad.md) önerilen seçeneklerden bazıları daha iyi anlamak için.
