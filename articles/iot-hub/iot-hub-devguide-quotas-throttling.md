---
title: Azure IOT Hub kotaları anlamak ve azaltma | Microsoft Docs
description: Geliştirici Kılavuzu - IOT Hub ve beklenen azaltma davranış uygulamak kotaları açıklaması.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: ef86af61284bb208cc8c469e3fe75bd4f4bdc5bf
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Başvuru - IOT hub'ı kotalar ve azaltma

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma
Her Azure aboneliğinin en fazla 10 IOT hub'ları ve en fazla 1 ücretsiz hub sahip olabilir.

Her IOT hub'ı belirli bir sayıda belirli bir katman birimleri ile birlikte sağlanır. Daha fazla bilgi için bkz: [Azure IOT Hub fiyatlandırma][lnk-pricing]. Katman ve birim sayısı maksimum günlük kotası gönderebileceğiniz iletilerin belirler.

Katmanı da IOT hub'ı tüm işlemler zorlar azaltma sınırları belirler.

## <a name="operation-throttles"></a>İşlem kısıtlamaları
İşlem kısıtlamaları dakika aralıklardaki uygulanır ve kötüye önlemek üzere tasarlanmıştır oranı sınırlamalardır. IOT hub'ı mümkün olduğunca hataları döndürüyor önlemek çalışır, ancak kısıtlama çok uzun süre ihlal edilirse özel durumları döndüren başlatır.

Belirli bir zamanda, bir IOT hub'ındaki sağlanan birim sayısını artırarak kotaları veya azaltma sınırları artırabilir.

Aşağıdaki tabloda zorlanan kısıtlamaları gösterir. Değerleri tek tek bir hub'ına bakın.

| Kısıtlama | Ücretsiz, B1 ve S1 | B2 ve S2 | B3 ve S3 | 
| -------- | ------- | ------- | ------- |
| Kimlik kayıt defteri işlemleri (oluşturma, alma, liste, Güncelleştir, Sil) | 1.67/sec/Unit (min/100/birim) | 1.67/sec/Unit (min/100/birim) | 83.33/sec/Unit (min/5000/birim) |
| Cihaz bağlantıları | Saniye başına 100 ya da sn/12/birimi daha yüksek <br/> Örneğin, iki S1 2 birimleridir\*12 = 24/sn, ancak en az 100/sn, birim üzerinde. Saniye başına 108 sahip dokuz S1 birimleriyle (9\*12), birim üzerinde. | 120/sn/birim | 6000/sn/birim |
| Cihazdan buluta gönderim | Saniye başına 100 ya da sn/12/birimi daha yüksek <br/> Örneğin, iki S1 2 birimleridir\*12 = 24/sn, ancak en az 100/sn, birim üzerinde. Saniye başına 108 sahip dokuz S1 birimleriyle (9\*12), birim üzerinde. | 120/sn/birim | 6000/sn/birim |
| Bulut cihaz gönderir<sup>1</sup> | 1.67/sec/Unit (min/100/birim) | 1.67/sec/Unit (min/100/birim) | 83.33/sec/Unit (min/5000/birim) |
| Bulut cihaz alır<sup>1</sup> <br/> (yalnızca cihaz HTTPS kullandığında)| 16.67/sec/Unit (min/1000/birim) | 16.67/sec/Unit (min/1000/birim) | 833.33/sec/Unit (min/50000/birim) |
| Karşıya dosya yükleme | 1.67 dosya karşıya yükleme bildirimleri/sn/birim (min/100/birim) | 1.67 dosya karşıya yükleme bildirimleri/sn/birim (min/100/birim) | 83.33 dosya karşıya yükleme bildirimleri/sn/birim (min/5000/birim) |
| Doğrudan yöntemleri<sup>1</sup> | 160KB/sn/birim<sup>2</sup> | 480KB/sec/unit<sup>2</sup> | 24MB/sn/birim<sup>2</sup> | 
| Cihaz çifti okur<sup>1</sup> | 10/sn | Saniye başına 10 sn/1/birim veya daha yüksek | 50/sn/birim |
| Cihaz çifti güncelleştirmeleri<sup>1</sup> | 10/sn | Saniye başına 10 sn/1/birim veya daha yüksek | 50/sn/birim |
| İşlerini işlemleri<sup>1</sup> <br/> (oluşturma, güncelleştirme, listeleme, silme) | 1.67/sec/Unit (min/100/birim) | 1.67/sec/Unit (min/100/birim) | 83.33/sec/Unit (min/5000/birim) |
| İşlerini aygıt başına işlemi işleme<sup>1</sup> | 10/sn | Saniye başına 10 sn/1/birim veya daha yüksek | 50/sn/birim |

<sup>1</sup>bu özellik IOT Hub'ın temel katmanında kullanılabilir değil. Daha fazla bilgi için bkz: [sağ IOT hub'ı seçme](iot-hub-scaling.md). <br/><sup>2</sup>olan 8 KB ölçer boyutunu azaltma.

*Cihaz bağlantılarını* kısıtlama yeni cihaz bağlantılarını kurulabileceği bir IOT hub ile oranı yönetir. *Cihaz bağlantılarını* kısıtlama en fazla eş zamanlı cihaz sayısını belirleyen değil. IOT hub için sağlanan birim sayısını azaltma bağlıdır.

Örneğin, tek bir S1 birim satın alırsanız, bir kısıtlama saniyede 100 bağlantısı alın. Bu nedenle, 100.000 cihaz bağlanmak için en az 1000 saniye (yaklaşık 16 dakika) alır. Ancak, kimlik kayıt defterinizde kayıtlı cihazlara sahip sayıda eş zamanlı cihazı olabilir.

IOT hub'ı kapsamlı bir açıklama için blog gönderisine bakın davranışı, azaltma [IOT Hub'ın azaltma ve][lnk-throttle-blog].

> [!IMPORTANT]
> Kimlik kayıt defteri işlemlerini aygıt yönetimi ve senaryoları sağlama çalışma zamanı kullanmak için tasarlanmıştır. Okunurken ya da çok sayıda cihaz kimliklerini güncelleştirilirken desteklenen aracılığıyla [içeri ve dışarı aktarma işleri][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Diğer sınırları

IOT hub'ı diğer işlemsel sınırları uygular:

| İşlem | Sınır |
| --------- | ----- |
| Karşıya dosya yükleme URI'ler | 10000 SAS URI'ler çıkışı için bir depolama hesabı aynı anda olabilir. <br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| İşlerini<sup>1</sup> | İş Geçmişi yukarı 30 gün olarak tutulur <br/> En fazla eşzamanlı iş, 1 (ücretsiz) ve S1, S2) (için 5, 10 (için S3). |
| Ek uç noktaları | SKU Ücretli hub 10 ek uç noktaları olabilir. Ücretsiz SKU hub'ları bir ek uç noktası olabilir. |
| İleti yönlendirme kuralları | SKU Ücretli hub 100 yönlendirme kuralları olabilir. Ücretsiz SKU hub beş yönlendirme kuralları olabilir. |
| Cihaz bulut Mesajlaşma | Maksimum ileti boyutu 256 KB |
| Bulut cihaz Mesajlaşma<sup>1</sup> | Maksimum ileti boyutu 64 KB. İletiler teslim için bekleyen maksimum 50'dir. |
| Doğrudan yöntemi<sup>1</sup> | En fazla doğrudan yöntemi yükü boyutu 128 KB'tır. |

<sup>1</sup>bu özellik IOT Hub'ın temel katmanında kullanılabilir değil. Daha fazla bilgi için bkz: [sağ IOT hub'ı seçme](iot-hub-scaling.md).

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
