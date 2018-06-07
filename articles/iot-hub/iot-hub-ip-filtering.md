---
title: Azure IOT Hub IP bağlantı filtrelerini | Microsoft Docs
description: Belirli IP adreslerinden için Azure IOT hub'ınıza bloğu bağlantıları filtreleme IP kullanmayı. Tek tek bağlantılarından veya IP adres aralıklarını engelleyebilirsiniz.
author: BeatriceOltean
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: fa44fd21eadb910ce90523b46332505c7303751e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635976"
---
# <a name="use-ip-filters"></a>IP filtreleri kullanın

Güvenlik Azure IOT hub'ına bağlı herhangi bir IOT çözümü önemli bir yönüdür. Bazen açıkça cihazları bağlamak IP adreslerini Güvenlik yapılandırmanızın bir parçası belirtmeniz gerekir. _IP Filtresi_ özelliği reddetme veya belirli IPv4 adresleri gelen trafiği kabul edilmesi için kuralları yapılandırmanıza olanak sağlar.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Belirli IP adresleri için IOT Hub uç noktaları engellemek yararlı olduğunda iki belirli kullanım örnekleri şunlardır:

- IOT hub'ınızı trafiği yalnızca belirtilen IP adreslerinden almak ve şey Reddet gerekir. Örneğin, IOT hub'ınıza kullanarak [Azure hızlı rota] bir IOT hub ile şirket içi altyapınızı arasında özel bağlantılar oluşturmak için.
- Kuşkulu olarak IOT hub'ı yönetici tarafından belirlenen IP adreslerinden gelen trafiği reddetmeye gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır

IP filtre kuralları IOT hub'ı hizmet düzeyinde uygulanır. Bu nedenle IP Filtresi kuralları, cihazları ve herhangi bir desteklenen protokolünü kullanarak arka uç uygulamalarının tüm bağlantılara uygulanır.

IOT hub'ınızdaki rejecting IP kuralıyla eşleşen bir IP adresinden gelen bağlantı girişimleri yetkisiz 401 durum kodu ve açıklama alır. Yanıt iletisi IP kural Bahsediyor değil.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, **IP Filtresi** IOT hub'ı portal kılavuzunda boştur. Bu varsayılan ayarı hub'ınızı herhangi bir IP adresi bağlantılarını kabul anlamına gelir. Bu varsayılan ayarı 0.0.0.0/0 IP adresi aralığı kabul eden bir kural eşdeğerdir.

![IOT hub'ı varsayılan IP Filtresi Ayarları][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a>IP filtre kuralı Ekle veya Düzenle

IP filtre kuralı eklediğinizde, için aşağıdaki değerleri istenir:

- Bir **IP filtre kuralı adı** benzersiz, büyük küçük harf duyarsız, alfasayısal bir dize en çok 128 karakter uzunluğunda olmalıdır. Yalnızca ASCII 7 bit alfasayısal karakterler artı `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` kabul edilir.
- Seçin bir **Reddet** veya **kabul** olarak **eylem** IP filtre kuralı için.
- Tek bir IPv4 adresi veya IP adreslerinin CIDR gösteriminde sağlar. Örneğin, içinde CIDR gösterimi 192.168.100.0/22 1024 IPv4 adresleri 192.168.100.0 için 192.168.103.255 temsil eder.

![Bir IOT hub'ına bir IP filtre kuralı ekleyin][img-ip-filter-add-rule]

Kural kaydettikten sonra güncelleştirme devam ediyor bildiren bir uyarı görürsünüz.

![IP filtre kuralı kaydetme hakkında bildirim][img-ip-filter-save-new-rule]

**Ekle** en fazla 10 IP filtresi kurallarını ulaştığında seçeneği devre dışıdır.

Mevcut bir kuralı, kuralı içeren satırı çift tıklayarak düzenleyebilirsiniz.

> [!NOTE]
> Reddetme IP adreslerini IOT hub ile etkileşim diğer Azure Hizmetleri (örneğin, Azure akış analizi, Azure sanal makineleri veya portal aygıt Explorer'da) engelleyebilirsiniz.

> [!WARNING]
> IP Filtresi etkinleştirilmiş bir IOT hub'ından iletileri okumak için Azure Stream Analytics (ASA) kullanıyorsanız, Event Hub ile uyumlu ada ve IOT hub'ınızın uç ASA bağlantı dizesinde kullanın.

## <a name="delete-an-ip-filter-rule"></a>IP filtre kuralını Sil

IP Filtresi kuralını silmek için bir veya daha fazla kural kılavuz tıklatın seçin ve **silmek**.

![Bir IOT Hub IP Filtresi kuralını silmek][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a>IP filtre kural değerlendirmesi

IP filtre kuralları sırayla uygulanır ve IP adresi ile eşleşen ilk kural kabul etme veya reddetme eylemi belirler.

Örneğin, aralığı 192.168.100.0/22 adresleri kabul edin ve başka her şeyi Reddet istiyorsanız, ilk kural kılavuzunda adres aralığı 192.168.100.0/22 kabul etmelidir. Sonraki kural aralığı 0.0.0.0/0 kullanarak tüm adresleri reddedecek.

Bir satırın başındaki üç dikey noktalar tıklayarak ve sürükleme kullanarak IP Filtresi kurallarınızı kılavuzunda sırasını değiştirmek ve bırakın.

Yeni IP filtre kuralı siparişinizi kaydetmek için tıklatın **kaydetmek**.

![IOT Hub IP Filtresi kurallarınızı sırasını değiştirme][img-ip-filter-rule-order]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

- [İzleme işlemleri][lnk-monitor]
- [IOT hub'ı ölçümleri][lnk-metrics]

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure hızlı rota]:  https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md
