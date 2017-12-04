---
title: "Microsoft Azure IoT seçenekleri | Microsoft Docs"
description: "Azure IoT Paketi, Microsoft IoT Central veya Azure IoT Hub'ı kullanarak IoT çözümünüzü nasıl uygulayacağınızı seçin."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.topic: get-started-article
ms.date: 11/10/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a23835f341bf02f8e662782984d95bfe194030b
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="compare-azure-iot-options"></a>Azure IoT seçeneklerini karşılaştırma

[Azure ve Nesnelerin İnterneti](iot-suite-what-is-azure-iot.md) makalesi aşağıdaki katmanlarla tipik bir IoT çözüm mimarisini açıklar:

* Cihaz bağlantısı ve yönetimi
* Veri işleme ve analizi
* Sunu ve iş tümleştirmesi

Bu mimariyi uygulamak için Azure IoT her biri farklı müşteri gereksinimlerine uygun olan birkaç seçenek sunar:

* [Azure IoT Paketi](index.md), özel IoT çözümlerinin geliştirilmesini hızlandırmanızı sağlayan Azure Hizmet Olarak Platform ile tümleşik, [önceden yapılandırılmış çözümlerden](iot-suite-what-are-preconfigured-solutions.md) oluşan bir kurumsal düzeyde koleksiyondur.

* [Microsoft IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions), bulut çözümü geliştirme uzmanlığı gerektiren kurumsal düzeyde IoT çözümleri oluşturmanıza olanak tanıyan model tabanlı bir yaklaşım kullanan SaaS çözümüdür.

## <a name="azure-iot-hub"></a>Azure IoT Hub'ı

Azure IoT Hub, hem Microsoft IoT Central hem de Azure IoT Paketi'nin yararlandığı temel Azure Hizmet Olarak Platformudur. IoT Hub, milyonlarca IoT cihazı ile bir bulut çözümü arasında güvenilir ve güvenli çift yönlü iletişimler sağlar. IoT Hub aşağıdaki gibi IoT uygulama sorunlarını gidermenize yardımcı olur:

* Yüksek hacimli cihaz bağlantısı ve yönetimi.
* Yüksek hacimli telemetri alımı.
* Cihazların komuta ve denetimi.
* Cihaz güvenliği uygulama.

## <a name="compare-azure-iot-suite-and-microsoft-iot-central"></a>Azure IoT Paketi ile Microsoft IoT Central'ı karşılaştırma

Azure IoT ürününüzün seçilmesi, IoT çözümünüzü planlamanın önemli bir parçasıdır. IoT Hub, kendi başına uçtan uca bir IoT çözümü sağlamayan bir Azure hizmetidir. IoT Hub herhangi bir IoT çözümü için başlangıç noktası olarak kullanılabilir ve onu kullanmak için Azure IoT Paketi veya Microsoft IoT Central kullanmanız gerekmez. Hem Azure IoT Paketi hem de Microsoft IoT Central, diğer Azure hizmetleriyle birlikte IoT Hub’ı kullanır. Aşağıdaki tabloda, gereksinimleriniz için doğru ürünü seçmenize yardımcı olmak üzere Azure IoT Paketi ile Microsoft IoT Central arasındaki temel farklılıklar özetlenmiştir:

|                        | Azure IoT Paketi | Microsoft IoT Central |
| ---------------------- | --------- | ----------- |
| Birincil kullanım | En üst düzeyde esneklik gerektiren özel bir IoT çözümünün geliştirilmesini hızlandırma. | Ayrıntılı hizmet özelleştirmesi gerektirmeyen basit IoT çözümleri için pazarlama süresini kısaltma. |
| Temel alınan PaaS hizmetlerine erişim          | Temel alınan Azure hizmetlerini yönetmek veya gerektiğinde değiştirmek için bu hizmetlere erişebilirsiniz. | SaaS. Tam olarak yönetilen çözüm, temel alınan hizmetler kullanıma sunulmaz. |
| Esneklik            | Yüksek. Mikro hizmet kodu açı kaynaktır ve uygun gördüğünüz herhangi bir şekilde değiştirebilirsiniz. Ayrıca, dağıtım altyapısını özelleştirebilirsiniz.| Orta. Yerleşik tarayıcı tabanlı kullanıcı deneyimini kullanarak çözüm modelini ve kullanıcı arabirimini farklı yönlerini özelleştirebilirsiniz. Farklı bileşenler kullanıma sunulmadığı için altyapı özelleştirilemez.|
| Beceri düzeyi                 | Orta-Yüksek. Çözüm arka ucunu özelleştirmek için Java veya .NET becerileri gerekir. Görselleştirmeyi özelleştirmek için JavaScript becerileri gerekir. | Düşük. Çözümü özelleştirmek için modelleme becerileri gerekir. Hiçbir kodlama becerisi gerekli değildir. |
| Başlangıç deneyimi | Önceden yapılandırılmış çözümler yaygın IOT senaryolarını uygular. Birkaç dakika içinde dağıtılabilir. | Uygulama şablonları ve cihaz şablonları önceden derlenmiş modeller sağlar. Birkaç dakika içinde dağıtılabilir. |
| Fiyatlandırma                | Maliyet denetlemek için hizmetlerde ince ayar yapabilirsiniz. | Basit, tahmin edilebilir fiyatlandırma yapısı. |

IoT çözümünüzü oluşturmak için kullanılacak ürün son olarak şununla belirlenir:

* İş gereksinimleriniz.
* Oluşturmak istediğiniz çözüm türü
* Kuruluşunuzun çözüm oluşturma ve uzun vadede sürdürme becerileri.

## <a name="next-steps"></a>Sonraki adımlar

Seçtiğiniz ürün ve yaklaşıma bağlı olarak, önerilen sonraki adımlar şunlardır:

* **Azure IoT Paketi**: [Önceden yapılandırılmış Azure IoT çözümleri nelerdir?](iot-suite-what-are-preconfigured-solutions.md)
* **Microsoft IoT Central**: [Microsoft IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions).
* **IoT Hub**: [Azure IoT Hub hizmetine genel bakış](../iot-hub/iot-hub-what-is-iot-hub.md).
