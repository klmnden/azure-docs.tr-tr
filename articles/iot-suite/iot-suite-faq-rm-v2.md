---
title: Azure IOT paketi Uzaktan izleme ile ilgili SSS | Microsoft Docs
description: "IOT paketi Uzaktan izleme çözümü için sık sorulan sorular"
services: iot-suite
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 3885e40eb4ddbf61b03a467a71d4dd97d00a9042
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="frequently-asked-questions-for-iot-suite"></a>IoT Paketi için sık sorulan sorular

Ayrıca bkz.: Genel [SSS](iot-suite-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Nasıl, yeni Uzaktan izleme çözümü sağlamak için maliyeti nedir?

Yeni önceden yapılandırılmış çözüm iki dağıtım seçenekleri sunar:

* A *temel* alt geliştirme maliyet veya bir tanıtım veya kavram kanıtı oluşturmak için arayan müşteriler arayan geliştiricileri için tasarlanmış seçeneği.
* A *standart* seçeneği üretime hazır bir altyapı dağıtmayı isteyen kuruluşlar için tasarlanmıştır.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>My çözümü geliştirirken ı my maliyetleri düşük tutun nasıl sağlayabilirsiniz?

İki farklı dağıtım sağlamaya ek olarak, yeni Uzaktan izleme çözümü etkinleştirmek veya isteğe bağlı tüm sanal cihazların devre dışı bırakmak için bir ayar vardır. Benzetim devre dışı bırakma, çözüm ve genel maliyeti, bu nedenle, alınan veri azaltır.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Temel ve standart dağıtım seçenekleri arasındaki fark nedir? İki dağıtım seçenekleri arasında nasıl karar?

Her dağıtım seçeneği için farklı gereksinimleri yanıt verir. Temel dağıtımı başlamak ve PoC ve küçük pilotlar geliştirmek üzere tasarlanmıştır. Minimum gerekli kaynakları ve daha düşük maliyetli bir kolaylaştırılmış bir mimari sağlar. Standart dağıtım oluşturmak ve bir üretime hazır çözümü özelleştirmek için tasarlanmıştır ve bir dağıtım, hayata geçirmek için gerekli öğeleri sağlar. Güvenilirlik ve ölçek, uygulama mikro Docker kapsayıcılar olarak oluşturulur ve bir orchestrator (varsayılan olarak Kubernetes) kullanılarak dağıtılabilir. Orchestrator, dağıtım, ölçeklendirme ve uygulama yönetimi için sorumludur. Geçerli gereksinimlerinize göre bir seçenek seçmeniz gerekir. Biri, diğer veya proje aşaması bağlı olarak her ikisinin birleşimini kullanabilirsiniz.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Panoda nasıl dinamik bir harita yapılandırırım?

Daha fazla bilgi için bkz: [dinamik bir haritada aygıtları görmek için yükseltme eşleme anahtarını](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir Azure aboneliğinde Enterprise planları için yalnızca iki adet İç İşlemler Düzey 1 Bing Haritası oluşturabilirsiniz. Uzaktan izleme çözümü, varsayılan olarak bir İç İşlemler Düzey 1 planı ile hazırlanır. Sonuç olarak, herhangi bir değişiklik yapılmadıysa bir abonelikte yalnızca en fazla iki tane uzaktan izleme çözümü sağlayabilirsiniz.

### <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [Önceden yapılandırılmış Uzaktan izleme çözümü özelliklerini keşfedin](iot-suite-remote-monitoring-explore.md)
* [Önceden yapılandırılmış Tahmine dayalı bakım çözümüne genel bakış](iot-suite-predictive-overview.md)
* [Önceden yapılandırılmış bağlı Fabrika çözümüne genel bakış](iot-suite-connected-factory-overview.md)
* [IOT güvenlik sıfırdan](securing-iot-ground-up.md)