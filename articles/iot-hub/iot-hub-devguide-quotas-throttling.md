---
title: "Azure IOT Hub kotaları anlamak ve azaltma | Microsoft Docs"
description: "Geliştirici Kılavuzu - IOT Hub ve beklenen azaltma davranış uygulamak kotaları açıklaması."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: dobett
ms.openlocfilehash: 8ffe25f1950f8535983c2c344b5c4331b7157869
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Başvuru - IOT hub'ı kotalar ve azaltma

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma
Her Azure aboneliğinin en fazla 10 IOT hub'ları ve en fazla 1 ücretsiz hub sahip olabilir.

Her IOT hub'ı belirli bir sayıda belirli bir SKU birimlerinde ile sağlanan (daha fazla bilgi için bkz: [Azure IOT Hub fiyatlandırma][lnk-pricing]). SKU ve birim sayısı maksimum günlük kotası gönderebileceğiniz iletilerin belirler.

SKU da IOT hub'ı tüm işlemler zorlar azaltma sınırları belirler.

## <a name="operation-throttles"></a>İşlem kısıtlamaları
İşlem kısıtlamaları dakika aralıklardaki uygulanır ve kötüye önlemek üzere tasarlanmıştır oranı sınırlamalardır. Mümkün olduğunda hataları döndürüyor önlemek IOT hub'ı çalışır, ancak kısıtlama çok uzun süre ihlal edilirse özel durumları döndüren başlatır.

Aşağıdaki tabloda zorlanan kısıtlamaları gösterir. Değerleri tek tek bir hub'ına bakın.

| Kısıtlama | Ücretsiz ve S1 hub'ları | S2 hub'ları | S3 hub'ları | 
| -------- | ------- | ------- | ------- |
| Kimlik kayıt defteri işlemleri (oluşturma, alma, liste, Güncelleştir, Sil) | 1.67/sec/Unit (min/100/birim) | 1.67/sec/Unit (min/100/birim) | 83.33/sec/Unit (min/5000/birim) |
| Cihaz bağlantıları | Saniye başına 100 ya da sn/12/birimi daha yüksek <br/> Örneğin, iki S1 2 birimleridir\*12 = 24/sn, ancak en az 100/sn, birim üzerinde. Saniye başına 108 sahip dokuz S1 birimleriyle (9\*12), birim üzerinde. | 120/sn/birim | 6000/sn/birim |
| Cihazdan buluta gönderim | Saniye başına 100 ya da sn/12/birimi daha yüksek <br/> Örneğin, iki S1 2 birimleridir\*12 = 24/sn, ancak en az 100/sn, birim üzerinde. Saniye başına 108 sahip dokuz S1 birimleriyle (9\*12), birim üzerinde. | 120/sn/birim | 6000/sn/birim |
| Buluttan cihaza gönderim | 1.67/sec/Unit (min/100/birim) | 1.67/sec/Unit (min/100/birim) | 83.33/sec/Unit (min/5000/birim) |
| Buluttan cihaza alım <br/> (yalnızca cihaz HTTPS kullandığında)| 16.67/sec/Unit (min/1000/birim) | 16.67/sec/Unit (min/1000/birim) | 833.33/sec/Unit (min/50000/birim) |
| Karşıya dosya yükleme | 1.67 dosya karşıya yükleme bildirimleri/sn/birim (min/100/birim) | 1.67 dosya karşıya yükleme bildirimleri/sn/birim (min/100/birim) | 83.33 dosya karşıya yükleme bildirimleri/sn/birim (min/5000/birim) |
| Doğrudan yöntemler | 20/sn/birim | 60/sn/birim | 3000/sn/birim | 
| Cihaz ikizi okumaları | 10/sn | Saniye başına 10 sn/1/birim veya daha yüksek | 50/sn/birim |
| Cihaz ikizi güncelleştirmeleri | 10/sn | Saniye başına 10 sn/1/birim veya daha yüksek | 50/sn/birim |
| İş işlemleri <br/> (oluşturma, güncelleştirme, listeleme, silme) | 1.67/sec/Unit (min/100/birim) | 1.67/sec/Unit (min/100/birim) | 83.33/sec/Unit (min/5000/birim) |
| İşler işlemleri için cihaz başına aktarım hızı | 10/sn | Saniye başına 10 sn/1/birim veya daha yüksek | 50/sn/birim |

Açıklamak önemlidir *cihaz bağlantılarını* kısıtlama yeni cihaz bağlantılarını kurulabileceği bir IOT hub ile oranı yönetir. *Cihaz bağlantılarını* kısıtlama en fazla eş zamanlı cihaz sayısını belirleyen değil. IOT hub için sağlanan birim sayısını azaltma bağlıdır.

Örneğin, tek bir S1 birim satın alırsanız, bir kısıtlama saniyede 100 bağlantısı alın. Bu nedenle, 100.000 cihaz bağlanmak için en az 1000 saniye (yaklaşık 16 dakika) alır. Ancak, kimlik kayıt defterinizde kayıtlı cihazlara sahip sayıda eş zamanlı cihazı olabilir.

IOT hub'ı kapsamlı bir açıklama için blog gönderisine bakın davranışı, azaltma [IOT Hub'ın azaltma ve][lnk-throttle-blog].

> [!NOTE]
> Belirli bir zamanda, bir IOT hub'ındaki sağlanan birim sayısını artırarak kotaları veya azaltma sınırları artırmak mümkündür.
> 
> [!IMPORTANT]
> Kimlik kayıt defteri işlemlerini aygıt yönetimi ve senaryoları sağlama çalışma zamanı kullanmak için tasarlanmıştır. Okunurken ya da çok sayıda cihaz kimliklerini güncelleştirilirken desteklenen aracılığıyla [içeri ve dışarı aktarma işleri][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Diğer sınırları

IOT hub'ı diğer işlemsel sınırları uygular:

| İşlem | Sınır |
| --------- | ----- |
| Karşıya dosya yükleme URI'ler | 10000 SAS URI'ler çıkışı için bir depolama hesabı aynı anda olabilir. <br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| İşler | İş Geçmişi yukarı 30 gün olarak tutulur <br/> En fazla eşzamanlı iş, 1 (ücretsiz) ve S1, S2) (için 5, 10 (için S3). |
| Ek uç noktaları | SKU Ücretli hub 10 ek uç noktaları olabilir. Ücretsiz SKU hub'ları bir ek uç noktası olabilir. |
| İleti yönlendirme kuralları | SKU Ücretli hub 100 yönlendirme kuralları olabilir. Ücretsiz SKU hub beş yönlendirme kuralları olabilir. |
| Cihaz bulut Mesajlaşma | Maksimum ileti boyutu 256 KB |
| Bulut cihaz Mesajlaşma | Maksimum ileti boyutu 64 KB |
| Bulut cihaz Mesajlaşma | İletiler teslim için bekleyen maksimum 50'dir |

> [!NOTE]
> Şu anda en fazla tek bir IOT hub'ına bağlanabileceğiniz aygıtlar 500.000 sayısıdır. Bu sınırı artırmak istiyorsanız, ilgili kişi [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Gecikme süresi
Tüm işlemler için düşük gecikmeli sağlamak için IOT hub'ı çalışır. Ancak, ağ koşulları ve diğer öngörülemeyen Etkenler nedeniyle, bir maksimum gecikme garanti edemez. Çözümünüzü tasarlarken, aşağıdakileri yapmalısınız:

* Tüm özelliklerinin en büyük gecikme hakkında herhangi bir IOT hub'ı işlem yapmaktan kaçının.
* IOT hub'ınıza aygıtlarınıza en yakın Azure bölgesi sağlayın.
* Cihazda veya bir ağ geçidi aygıtı yakın gecikme süresine duyarlı işlemleri gerçekleştirmek için Azure IOT kenar kullanmayı düşünün.

Birden çok IOT hub'ı birimleri, daha önce açıklandığı gibi azaltma etkiler, ancak herhangi bir ek gecikme avantajları veya garanti sağlamaz.
Beklenmeyen artar işlemi gecikme görürseniz, başvurun [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Sonraki adımlar
Bu IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları][lnk-devguide-endpoints]
* [Cihaz çiftlerini, işler ve ileti yönlendirme için IOT hub'ı sorgulama dili][lnk-devguide-query]
* [IOT Hub MQTT desteği][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
