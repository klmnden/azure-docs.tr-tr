---
title: Uzaktan izleme çözüm Hızlandırıcısını SSS - Azure | Microsoft Docs
description: Uzaktan izleme çözüm hızlandırıcısının için sık sorulan sorular
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 746d21c7ff4d5b939eea7690193ac07425b4001c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447922"
---
# <a name="frequently-asked-questions-for-remote-monitoring-solution-accelerator"></a>Uzaktan izleme çözüm hızlandırıcısının için sık sorulan sorular

Ayrıca bkz. genel [SSS](iot-accelerators-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Bu yeni Uzaktan izleme çözümü sağlamak için nin ücreti ne kadardır?

Yeni çözüm Hızlandırıcısını iki dağıtım seçeneği sunar:

* A *temel* daha düşük geliştirme maliyeti veya bir tanıtım veya kavram kanıtı oluşturmak isteyen müşteriler için isteyen geliştiriciler için tasarlanmış seçeneği.
* A *standart* seçeneği üretime hazır bir altyapı dağıtmayı isteyen kuruluşlar için tasarlanmıştır.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Ben çözümüm geliştirmeye devam ederken miyim my maliyetleri düşük tutun nasıl sağlayabilirsiniz?

İki farklı dağıtım sağlamanın yanı sıra yeni bir uzaktan izleme çözümü etkinleştirme veya devre dışı isteğe bağlı tüm sanal cihazlardan bir ayarı vardır. Benzetim devre dışı bırakma, çözüm ve genel maliyeti, bu nedenle, alınan veriler azaltır.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Temel ve standart dağıtım seçenekleri arasındaki fark nedir? İki dağıtım seçeneğinden nasıl karar verebilirim?

Her dağıtım seçeneğinin, farklı ihtiyaçlarına yanıt verir. Temel dağıtımın, PT ve küçük pilotlar geliştirmesine başlamak için tasarlanmıştır. En düşük gerekli kaynakları ve daha düşük bir maliyet basitleştirilmiş bir mimari sağlar. Standart dağıtım oluşturun ve üretime hazır bir çözümü özelleştirmek için tasarlanmıştır ve bir dağıtım, hayata geçirmek için gereken öğeleri sağlar. Güvenilirlik ve ölçek için uygulama mikro hizmetler, Docker kapsayıcıları olarak oluşturulur ve bir düzenleyici (varsayılan olarak Kubernetes) aracılığıyla dağıtılır. Düzenleyici, uygulamanın dağıtımından, ölçeklenmesinden ve yönetiminden sorumludur. Geçerli gereksinimlerinize göre bir seçenek seçmeniz gerekir. Biri, diğer veya, proje aşama bağlı olarak her ikisinin bir birleşimini kullanabilirsiniz.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Dinamik bir harita Panoda nasıl yapılandırabilirim?

Daha fazla bilgi için [dinamik bir haritada cihazları görmek için yükseltme harita anahtarı](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Uzaktan izleme çözümünün önceki sürümü hakkında bilgileri nerede bulabilirim?

Uzaktan izleme çözüm Hızlandırıcısını önceki sürümünü IOT paketi uzaktan önceden yapılandırılmış izleme çözümü olarak bilinir. Arşivlenen belgeleri bulabilirsiniz [ https://docs.microsoft.com/previous-versions/azure/iot-suite/ ](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Uzaktan izleme çözüm Hızlandırıcısını özelliklerini keşfedin](quickstart-remote-monitoring-deploy.md)
* [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](iot-accelerators-predictive-overview.md)
* [Bağlı Fabrika çözüm Hızlandırıcısını dağıtma](quickstart-connected-factory-deploy.md)
* [Baştan sona IOT güvenliği](/azure/iot-fundamentals/iot-security-ground-up)
