---
title: Azure sanal makine ölçek kümeleri ile otomatik ölçeklendirmeye genel bakış | Microsoft Docs
description: Bir Azure sanal makine ölçek bağlı olarak, performans veya sabit bir zamanlamaya göre otomatik olarak ölçeklendirebilirsiniz farklı yolları hakkında bilgi edinin
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 610f3073594f73f04a68865593be6bfb4188d4f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60883679"
---
# <a name="overview-of-autoscale-with-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek ile otomatik ölçeklendirmeye genel bakış ayarlar
Bir Azure sanal makine ölçek kümesini otomatik olarak artırabilir veya uygulamanızı çalıştıran VM örneği sayısını azaltabilirsiniz. Bu otomatik ve esnek davranışı izlemek ve uygulamanızın performansını en iyi duruma getirmek için yönetim yükünü azaltır. Pozitif bir müşteri deneyimi için kabul edilebilir performans tanımlayan kuralları oluşturun. Bu tanımlı eşikler karşılandığında, otomatik ölçeklendirme kurallarını ölçek kümenizin kapasitesinin ayarlamak için gerekeni yapın. Ayrıca, olayları otomatik olarak artırma veya azaltma ölçek kümenizin kapasitesinin kez sabit zamanlayabilirsiniz. Bu makalede performans ölçümleri kullanılabilir bir genel bakış ve hangi eylemleri otomatik ölçeklendirme gerçekleştirebilir sağlar.


## <a name="benefits-of-autoscale"></a>Otomatik ölçeklendirme avantajları
Uygulamanızın talebi artarsa, ölçek kümenizdeki sanal makine örneklerinde üzerindeki yük de artar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz.

Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. Hangi ölçümlerin izleneceğini, CPU veya bellek, uygulama yük belirli bir eşiği ne kadar süre karşılaması gerekir ve kümesi kaç VM örnekleri ölçek eklemek gibi denetim.

Bir akşam veya hafta sonu uygulama talebiniz azalabilir. Yük belirli bir süreye yayılarak tutarlı şekilde azalıyorsa, ölçek kümesindeki sanal makine örneği sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Mevcut talebi karşılamak için gerekli örnek sayısını yalnızca siz çalıştırdığınızdan, bu ölçeği daraltma eylemi ölçek kümenizi çalıştırma maliyetini azaltır.


## <a name="use-host-based-metrics"></a>Konak tabanlı ölçümleri kullanan
Bu yerleşik konak ölçümleri kullanılabilir VM örneklerinizi otomatik ölçeklendirme kuralları oluşturabilirsiniz. Ana ölçümler yüklemek veya ek aracılar ve veri koleksiyonları yapılandırmak zorunda kalmadan bir ölçek VM örnekleri performansını görünürlük sunar. Bu ölçümleri kullanan otomatik ölçeklendirme kuralları, kullanıma veya yanıt CPU kullanımı, bellek talebi ya da disk erişimi için sanal makine örneği sayısını ölçeklendirebilirsiniz.

Konak tabanlı ölçümleri kullanan otomatik ölçeklendirme kuralları aşağıdaki araçlarla oluşturulabilir:

- [Azure portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI](tutorial-autoscale-cli.md)
- [Azure şablonu](tutorial-autoscale-template.md)

Daha ayrıntılı performans ölçümleri kullanan otomatik ölçeklendirme kuralları oluşturmak için [yüklemek ve Azure tanılama uzantısını yapılandırmak](#in-guest-vm-metrics-with-the-azure-diagnostics-extension) VM örneklerinde veya [uygulama kullanımınızı App Insights yapılandırma](#application-level-metrics-with-app-insights).

Konak tabanlı ölçümleri kullanan otomatik ölçeklendirme kuralları, aşağıdaki yapılandırma ayarları Azure tanılama uzantısı ve App Insights ile Konuk VM ölçümlerini kullanabilirsiniz.

### <a name="metric-sources"></a>Ölçüm kaynağı
Otomatik ölçeklendirme kuralları ölçümleri aşağıdaki kaynaklardan birini kullanabilirsiniz:

| Ölçüm kaynağı        | Kullanım örneği                                                                                                                     |
|----------------------|------------------------------------------------------------------------------------------------------------------------------|
| Geçerli bir ölçek kümesi    | İçin ana bilgisayar tabanlı ölçümler ek aracı yüklenmedi veya Yapılandırılmadı, gerektirmez.                                  |
| Depolama hesabı      | Azure tanılama uzantısı performans ölçümlerini sonra otomatik ölçeklendirme kurallarını tetiklemek için kullanılan Azure depolamaya yazar. |
| Service Bus Kuyruğu    | Uygulamanızı veya diğer bileşenleri Tetikleyici kuralları için bir Azure Service Bus kuyruğundaki iletileri iletebilir.                   |
| Application Insights | Uygulamanızdaki doğrudan uygulama üzerinden ölçüm akışları yüklü bir izleme paketi.                         |


### <a name="autoscale-rule-criteria"></a>Otomatik ölçeklendirme kuralını ölçütü
Otomatik ölçeklendirme kuralları oluşturduğunuzda aşağıdaki ana bilgisayar tabanlı ölçümler kullanılabilir duruma gelir. App Insights ve Azure tanılama uzantısı kullanırsanız, izleme ve otomatik ölçeklendirme kuralları ile kullanmak için hangi ölçümler tanımlayın.

| Ölçüm adı               |
|---------------------------|
| CPU yüzdesi            |
| Ağ Girişi                |
| Ağ Çıkışı               |
| Diskten Okunan Bayt           |
| Diske Yazılan Bayt          |
| Disk Okuma İşlemi/Sn  |
| Disk Yazma İşlemi/Sn |
| Kalan CPU Kredisi     |
| Tüketilen CPU Kredisi      |

Belirli bir metrik izlemek için otomatik ölçeklendirme kuralları oluşturduğunuzda, kuralları aşağıdaki ölçümleri toplama eylemlerden birini arayın:

| Toplama türü |
|------------------|
| Ortalama          |
| Minimum          |
| Maksimum          |
| Toplam            |
| Son             |
| Sayı            |

Otomatik ölçeklendirme kuralları ölçümleri tanımlanmış yönelik eşiğiniz karşı aşağıdaki işleçleri biri ile karşılaştırıldığında daha sonra tetiklenen:

| İşleç                 |
|--------------------------|
| Büyüktür             |
| Büyük veya eşit |
| Şu değerden az:                |
| Küçük veya eşit    |
| Eşittir                 |
| Eşit değildir             |


### <a name="actions-when-rules-trigger"></a>Kurallarını tetikleme sırasındaki Eylemler
Bir otomatik ölçeklendirme kural tetiklendiğinde, Ölçek kümeniz otomatik olarak aşağıdaki yollardan biriyle ölçeklendirebilirsiniz:

| Ölçeklendirme işlemi     | Kullanım örneği                                                                                                                               |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Sayıyı şu kadar artır:   | VM örnekleri oluşturmak için sabit bir sayı. Ölçek kümeleri ile Vm'leri daha az sayıda yararlıdır.                                           |
| Yüzdeyi şu kadar artır: | Yüzde tabanlı bir artış VM örnekleri. Daha büyük ölçekli iyi burada sabit bir artış fark edilir derecede performansını iyileştirebilir değil ayarlar. |
| Sayıyı şuna artır:   | İstenen bir maksimum miktar ulaşmak için birçok VM örnekleri gerektiği gibi oluşturun.                                                            |
| Sayıyı şu kadar azalt:   | VM örnekleri kaldırmak için sabit bir sayı. Ölçek kümeleri ile Vm'leri daha az sayıda yararlıdır.                                           |
| Yüzdeyi şu kadar azalt: | Yüzde tabanlı bir düşüş VM örnekleri. Daha büyük ölçekli iyi burada sabit bir artış fark edilir derecede kaynak tüketimine ve maliyetlere azaltabilir değil ayarlar. |
| Sayıyı şuna düşür:   | Birçok VM örnekleri istediğiniz en düşük düzeyde erişmek için gerekli olan kaldırın.                                                            |


## <a name="in-guest-vm-metrics-with-the-azure-diagnostics-extension"></a>Konuk VM ölçümlerini Azure tanılama uzantısı
Azure tanılama uzantısını içinde bir sanal makine örneği çalıştıran bir aracıdır. Aracı izler ve performans ölçümlerini Azure Depolama'ya kaydeder. Bu performans ölçümlerini gibi VM durumu hakkında daha ayrıntılı bilgi içeren *AverageReadTime* diskler için veya *PercentIdleTime* CPU. Daha ayrıntılı bir farkındalık yalnızca CPU kullanımı veya bellek tüketimi yüzdesi VM performansı üzerinde dayalı otomatik ölçeklendirme kuralları oluşturabilirsiniz.

Azure tanılama uzantısını kullanmak için sanal makine örnekleriniz için Azure depolama hesapları oluşturun, Azure tanılama aracısını yükleyin ve ardından depolama hesabına stream belirli performans sayaçları için Vm'leri yapılandırma.

Daha fazla bilgi için Azure tanılama uzantısını [Linux VM](../virtual-machines/extensions/diagnostics-linux.md) veya [Windows VM](../virtual-machines/extensions/diagnostics-windows.md) için etkinleştirme makalelerini inceleyebilirsiniz.


## <a name="application-level-metrics-with-app-insights"></a>App Insights ile uygulama düzeyinde ölçümler
Uygulamalarınızın performansını artırmak için daha fazla görünürlük elde etmek için Application ınsights'ı kullanabilirsiniz. Uygulama izler ve Azure'a telemetri gönderen uygulamanızda küçük bir izleme paketi yükleyin. Sayfa yükleme performansı, uygulamanızın yanıt süreleri gibi ölçümleri izleyebilir ve oturum sayılarını. Bu uygulama ölçümleri, müşteri deneyimini etkileyebilecek eyleme dönüştürülebilir öngörüleri temel alan kurallarını tetikleme ayrıntılı ve katıştırılmış düzeyinde otomatik ölçeklendirme kuralları oluşturmak için kullanılabilir.

App Insights hakkında daha fazla bilgi için bkz. [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md).


## <a name="scheduled-autoscale"></a>Zamanlanan otomatik ölçeklendirme
Zamanlamalara göre temel otomatik ölçeklendirme kuralları oluşturabilirsiniz. Zamanlama tabanlı kurallar otomatik olarak sanal makine örneği sayısını kez sabit ölçek sağlar. Kurallar, performans tabanlı otomatik ölçeklendirme kurallarını tetikleme önce uygulamada performans etkisi olabilir ve yeni VM örnekleri sağlanır. Böyle bir talep tahmin, ek sanal makine örnekleri sağlanmış ve ek müşteri kullanımı ve uygulama talep için hazır.

Aşağıdaki örnekler, zamanlama tabanlı otomatik ölçeklendirme kuralları kullanımını faydalanabilir senaryolar şunlardır:

- Ne zaman müşteri talebini artırır iş gününün başlangıcında VM örneklerinin sayısını otomatik olarak ölçeği. İş günü sonunda, uygulama kullanımı düşük olduğunda gece kaynak maliyetleri azaltmak için sanal makine örneği sayısını otomatik olarak ölçeklendirin.
- Bir bölüm bir uygulamanın bazı kısımlarını ayın veya mali döngüsü sırasında yoğun olarak kullanıyorsa, ek talepleri karşılamak için sanal makine örneği sayısını otomatik olarak ölçeklendirin.
- Bir pazarlama olay, yükseltme veya tatil satış olduğunda, beklenen müşteri talebini önceden sanal makine örneği sayısını otomatik olarak ölçeklendirebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki araçlardan biriyle konak tabanlı ölçümleri kullanan otomatik ölçeklendirme kuralları oluşturabilirsiniz:

- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI](tutorial-autoscale-cli.md)
- [Azure şablonu](tutorial-autoscale-template.md)

Bu genel bakışta yatay olarak genişletmek ve artırmak veya azaltmak için otomatik ölçeklendirme kurallarını kullanma konusunda ayrıntılı bilgi *numarası* ölçek VM örnekleri kümesi. Ayrıca dikey olarak artırabilir veya azaltabilirsiniz VM örneğine ölçeklendirebilirsiniz *boyutu*. Daha fazla bilgi için [sanal makine ölçek kümeleri ile dikey otomatik ölçeklendirme](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Sanal makine örnekleriniz yönetme hakkında daha fazla bilgi için bkz: [Yönet sanal makine ölçek kümeleri Azure PowerShell ile](virtual-machine-scale-sets-windows-manage.md).

Tetikleyici, otomatik ölçeklendirme kuralları taktirde uyarı oluşturma konusunda bilgi almak için bkz: [e-posta ve Web kancası, Azure İzleyici'de uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın](../azure-monitor/platform/autoscale-webhook-email.md). Ayrıca [e-posta ve Web kancası, Azure İzleyici'de uyarı bildirimleri göndermek için Denetim günlükleri kullanın](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
