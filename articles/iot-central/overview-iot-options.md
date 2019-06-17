---
title: Microsoft Azure IoT seçenekleri | Microsoft Docs
description: Azure IOT Central, IOT Çözüm Hızlandırıcıları veya IOT hub'ı kullanarak Azure IOT çözümünüzü uygulamak nasıl seçin.
author: dominicbetts
ms.author: dobett
ms.date: 06/09/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 19ec7afeb71f0e9d5602f1c4ba1a2456162cdfae
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077396"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Azure IoT Central ile Azure IoT seçeneklerini karşılaştırma

Microsoft Azure IOT Central'ı ve Azure IOT, IOT çözümü oluşturmaya yönelik çeşitli seçenekler sunar. Bu seçenekler, farklı müşteri gereksinimlerini kümesi için uygundur:

* [Azure IOT Central](overview-iot-central.md) bulut çözümü geliştirme uzmanlığı gerektiren Kurumsal düzeyde IOT çözümleri oluşturmanıza yardımcı olacak bir model tabanlı bir yaklaşım kullanan bir hizmet (SaaS) çözümü olarak bir yazılımdır.

* [Azure IOT Çözüm Hızlandırıcıları](https://docs.microsoft.com/azure/iot-accelerators/) , kurumsal düzeyde koleksiyondur [Çözüm Hızlandırıcıları](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md) yardımcı olan bir hizmet (PaaS) Azure platformunu temel alan özel IOT çözümlerinin geliştirilmesini hızlandırmanızı.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub, hem Azure IoT Central hem de Azure IoT çözüm hızlandırıcılarının kullandığı temel Azure PaaS çözümüdür. IOT Hub, milyonlarca IOT cihazı ile bir bulut çözümü arasında güvenilir ve güvenli çift yönlü iletişimler destekler. IOT Hub gibi IOT uygulama sorunlarını karşılamanıza yardımcı olur:

* Yüksek hacimli cihaz bağlantısı ve Yönetimi
* Yüksek hacimli telemetri alımı
* Komut ve denetim cihaz
* Cihaz güvenliği uygulama

## <a name="compare-azure-iot-central-and-azure-iot-solution-accelerators"></a>Azure IoT Central ve Azure IoT çözüm hızlandırıcılarını karşılaştırma

Azure IoT ürününüzün seçilmesi, IoT çözümünüzü planlamanın önemli bir parçasıdır. IoT Hub, kendi başına uçtan uca bir IoT çözümü sağlamayan bir Azure hizmetidir. Herhangi bir IOT çözümü için başlangıç noktası olarak IOT hub'ı kullanabilirsiniz. IOT hub'ını kullanmak için Azure IOT Çözüm Hızlandırıcıları veya Azure IOT Central kullanmanız gerekmez. IoT çözüm hızlandırıcıları ve Azure IoT Central’ın her ikisi de, diğer Azure hizmetleriyle birlikte IoT Hub’ı kullanır.

Aşağıdaki tabloda, gereksinimleriniz için doğru ürünü seçmenize yardımcı olmak üzere IoT çözüm hızlandırıcıları ile Azure IoT Central arasındaki temel farklılıklar özetlenmiştir:

|     | Azure IoT Central | Azure IoT çözüm hızlandırıcıları |
| --- | ----------- | --------- |
| Birincil kullanım                      | Ayrıntılı hizmet özelleştirmesi gerektirmeyen basit IoT çözümleri için pazarlama süresini kısaltma.                                                    | En üst düzeyde esneklik gerektiren özel bir IoT çözümünün geliştirilmesini hızlandırma.                                                                                                                             |
| Temel alınan PaaS hizmetlerine erişim | SaaS. Tam olarak yönetilen bir çözümü olduğundan, temel alınan hizmetler kullanıma sunulmaz.                                                                                            | Temel alınan Azure hizmetlerini yönetmek veya gerektiğinde değiştirmek için bu hizmetlere erişebilirsiniz.                                                                                                                    |
| Esneklik                        | Orta. Yerleşik tarayıcı tabanlı kullanıcı deneyimini kullanarak çözüm modelini ve kullanıcı arabirimini farklı yönlerini özelleştirebilirsiniz. Altyapı özelleştirilebilir değildir farklı bileşenler kullanıma sunulmaz. | Yüksek. Mikro hizmet kodu açık kaynaktır ve uygun gördüğünüz herhangi bir yolla değiştirin. Ayrıca, dağıtım altyapısını özelleştirebilirsiniz.                                               |
| Beceri düzeyi                        | Düşük. Çözümü özelleştirmek için modelleme becerileri gerekir. Hiçbir kodlama becerisi gerekli değildir.                                                                          | Orta Yüksek. Bir çözümün arka ucu özelleştirmek için Java veya .NET becerileri gerekir. Görselleştirmeyi özelleştirmek için JavaScript becerileri gerekir.                                                                       |
| Başlarken deneyimi alınıyor             | Uygulama şablonları ve cihaz şablonları önceden oluşturulmuş modeller sağlar. Birkaç dakika içinde dağıtılabilir.                                                                                                  | Önceden yapılandırılmış çözümler yaygın IOT senaryolarını uygular. Birkaç dakika içinde dağıtılabilir.                                                                                                                            |
| Fiyatlandırma                            | Basit, tahmin edilebilir fiyatlandırma yapısı.                                                                                                                           | Maliyet denetlemek için hizmetlerde ince ayar yapabilirsiniz.                                                                                                                                                            |

IOT çözümünüzü oluşturmak için kullanılacak ürün bağlıdır:

* İş gereksinimleriniz.
* Derlemek istediğiniz çözüm türü.
* Kuruluşunuzun çözüm oluşturma ve uzun vadede sürdürme becerileri.

## <a name="next-steps"></a>Sonraki adımlar

Seçtiğiniz ürün ve yaklaşıma bağlı olarak, önerilen sonraki adımlar şunlardır:

* **Azure IOT Central**: [Azure IoT Central nedir?](overview-iot-central.md)
* **IOT Çözüm Hızlandırıcıları**: [Azure IoT çözüm hızlandırıcıları nedir?](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md)
* **IOT hub'ı**: [Azure IoT Hub nedir?](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub)