---
title: Uzaktan izleme Çözüm Hızlandırıcısı SSS | Microsoft Docs
description: Uzaktan izleme Çözüm Hızlandırıcısı için sık sorulan sorular
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: a65f2a90aedf5aa50016a6c5e5019094f202bf55
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="frequently-asked-questions-for-remote-monitoring-solution-accelerator"></a>Uzaktan izleme Çözüm Hızlandırıcısı için sık sorulan sorular

Ayrıca bkz.: Genel [SSS](iot-accelerators-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Nasıl, yeni Uzaktan izleme çözümü sağlamak için maliyeti nedir?

Yeni Çözüm Hızlandırıcısı iki dağıtım seçenekleri sunar:

* A *temel* alt geliştirme maliyet veya bir tanıtım veya kavram kanıtı oluşturmak için arayan müşteriler arayan geliştiricileri için tasarlanmış seçeneği.
* A *standart* seçeneği üretime hazır bir altyapı dağıtmayı isteyen kuruluşlar için tasarlanmıştır.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>My çözümü geliştirirken ı my maliyetleri düşük tutun nasıl sağlayabilirsiniz?

İki farklı dağıtım sağlamaya ek olarak, yeni Uzaktan izleme çözümü etkinleştirmek veya isteğe bağlı tüm sanal cihazların devre dışı bırakmak için bir ayar vardır. Benzetim devre dışı bırakma, çözüm ve genel maliyeti, bu nedenle, alınan veri azaltır.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Temel ve standart dağıtım seçenekleri arasındaki fark nedir? İki dağıtım seçenekleri arasında nasıl karar?

Her dağıtım seçeneği için farklı gereksinimleri yanıt verir. Temel dağıtımı başlamak ve PoC ve küçük pilotlar geliştirmek üzere tasarlanmıştır. Minimum gerekli kaynakları ve daha düşük maliyetli bir kolaylaştırılmış bir mimari sağlar. Standart dağıtım oluşturmak ve bir üretime hazır çözümü özelleştirmek için tasarlanmıştır ve bir dağıtım, hayata geçirmek için gerekli öğeleri sağlar. Güvenilirlik ve ölçek, uygulama mikro Docker kapsayıcılar olarak oluşturulur ve bir orchestrator (varsayılan olarak Kubernetes) kullanılarak dağıtılabilir. Orchestrator, dağıtım, ölçeklendirme ve uygulama yönetimi için sorumludur. Geçerli gereksinimlerinize göre bir seçenek seçmeniz gerekir. Biri, diğer veya proje aşaması bağlı olarak her ikisinin birleşimini kullanabilirsiniz.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Panoda nasıl dinamik bir harita yapılandırırım?

Daha fazla bilgi için bkz: [dinamik bir haritada aygıtları görmek için yükseltme eşleme anahtarını](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Uzaktan izleme Çözüm Hızlandırıcısı özelliklerini keşfedin](iot-accelerators-remote-monitoring-explore.md)
* [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](../iot-suite/iot-suite-predictive-overview.md)
* [Bağlı Fabrika Çözüm Hızlandırıcısı genel bakış](iot-accelerators-connected-factory-overview.md)
* [IOT güvenlik sıfırdan](../iot-suite/securing-iot-ground-up.md)
