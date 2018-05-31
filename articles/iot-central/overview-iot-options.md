---
title: Microsoft Azure IoT seçenekleri | Microsoft Docs
description: Azure IoT Central, IoT Paketi veya IoT Hub kullanarak Azure IoT çözümünüzü nasıl uygulayacağınızı seçin.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: overview
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 8ed524e87f820f6c1e2e05da0bcbc7241bdd1ec1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201467"
---
# <a name="compare-azure-iot-central-and-azure-iot-options"></a>Azure IoT Central ile Azure IoT seçeneklerini karşılaştırma

Bir IoT çözümünü uygulamak için Microsoft Azure IoT Central ve Azure IoT, her biri farklı bir müşteri gereksinimleri kümesine uygun olan birkaç seçenek sunar:

* [Azure IoT Central](overview-iot-central.md), bulut çözümü geliştirme uzmanlığı gerektiren kurumsal düzeyde IoT çözümleri oluşturmanıza olanak tanıyan model tabanlı bir yaklaşım kullanan bir Hizmet Olarak Yazılım (SaaS) çözümüdür.

* [Azure IoT Paketi](https://docs.microsoft.com/azure/iot-suite/), özel IoT çözümlerinin geliştirilmesini hızlandırmanızı sağlayan Azure Hizmet Olarak Platform (PaaS) ile tümleşik, [önceden yapılandırılmış çözümlerden](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions) oluşan, kurumsal düzeyde bir koleksiyondur.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub, hem Azure IoT Central hem de Azure IoT Paketi'nin kullandığı temel Azure PaaS çözümüdür. IoT Hub, milyonlarca IoT cihazı ile bir bulut çözümü arasında güvenilir ve güvenli çift yönlü iletişimler sağlar. IoT Hub aşağıdaki gibi IoT uygulama sorunlarını gidermenize yardımcı olur:

* Yüksek hacimli cihaz bağlantısı ve yönetimi.
* Yüksek hacimli telemetri alımı.
* Cihazların komuta ve denetimi.
* Cihaz güvenliği uygulama.

## <a name="compare-azure-iot-central-and-azure-iot-suite"></a>Azure IoT Central ile Azure IoT Paketi'ni karşılaştırma

Azure IoT ürününüzün seçilmesi, IoT çözümünüzü planlamanın önemli bir parçasıdır. IoT Hub, kendi başına uçtan uca bir IoT çözümü sağlamayan bir Azure hizmetidir. IoT Hub herhangi bir IoT çözümü için başlangıç noktası olarak kullanılabilir ve onu kullanmak için Azure IoT Paketi veya Azure IoT Central kullanmanız gerekmez. Hem IoT Paketi hem de Azure IoT Central, diğer Azure hizmetleriyle birlikte IoT Hub’ı kullanır. Aşağıdaki tabloda, gereksinimleriniz için doğru ürünü seçmenize yardımcı olmak üzere IoT Paketi ile Azure IoT Central arasındaki temel farklılıklar özetlenmiştir:

|     | Azure IoT Central | Azure IoT Paketi |
| --- | ----------- | --------- |
| Birincil kullanım                      | Ayrıntılı hizmet özelleştirmesi gerektirmeyen basit IoT çözümleri için pazarlama süresini kısaltma.                                                    | En üst düzeyde esneklik gerektiren özel bir IoT çözümünün geliştirilmesini hızlandırma.                                                                                                                             |
| Temel alınan PaaS hizmetlerine erişim | SaaS. Tam olarak yönetilen çözüm, temel alınan hizmetler kullanıma sunulmaz.                                                                                            | Temel alınan Azure hizmetlerini yönetmek veya gerektiğinde değiştirmek için bu hizmetlere erişebilirsiniz.                                                                                                                    |
| Esneklik                        | Orta. Yerleşik tarayıcı tabanlı kullanıcı deneyimini kullanarak çözüm modelini ve kullanıcı arabirimini farklı yönlerini özelleştirebilirsiniz. Farklı bileşenler kullanıma sunulmadığı için altyapı özelleştirilemez. | Yüksek. Mikro hizmet kodu açı kaynaktır ve uygun gördüğünüz herhangi bir şekilde değiştirebilirsiniz. Ayrıca, dağıtım altyapısını özelleştirebilirsiniz.                                               |
| Beceri düzeyi                        | Düşük. Çözümü özelleştirmek için modelleme becerileri gerekir. Hiçbir kodlama becerisi gerekli değildir.                                                                          | Orta-Yüksek. Çözüm arka ucunu özelleştirmek için Java veya .NET becerileri gerekir. Görselleştirmeyi özelleştirmek için JavaScript becerileri gerekir.                                                                       |
| Başlangıç deneyimi             | Uygulama şablonları ve cihaz şablonları önceden derlenmiş modeller sağlar. Birkaç dakika içinde dağıtılabilir.                                                                                                  | Önceden yapılandırılmış çözümler yaygın IOT senaryolarını uygular. Birkaç dakika içinde dağıtılabilir.                                                                                                                            |
| Fiyatlandırma                            | Basit, tahmin edilebilir fiyatlandırma yapısı.                                                                                                                           | Maliyet denetlemek için hizmetlerde ince ayar yapabilirsiniz.                                                                                                                                                            |

IoT çözümünüzü oluşturmak için kullanılacak ürün son olarak şununla belirlenir:

* İş gereksinimleriniz.
* Oluşturmak istediğiniz çözüm türü
* Kuruluşunuzun çözüm oluşturma ve uzun vadede sürdürme becerileri.

## <a name="next-steps"></a>Sonraki adımlar

Seçtiğiniz ürün ve yaklaşıma bağlı olarak, önerilen sonraki adımlar şunlardır:

* **Azure IoT Central**: [Azure IoT Central](overview-iot-central.md).
* **IoT Paketi**: [Önceden yapılandırılmış Azure IoT çözümleri nelerdir?](https://docs.microsoft.com/azure/iot-suite/iot-suite-what-are-preconfigured-solutions).
* **IoT Hub**: [Azure IoT Hub hizmetine genel bakış](https://docs.microsoft.com/azure/iot-hub/iot-hub-what-is-iot-hub).