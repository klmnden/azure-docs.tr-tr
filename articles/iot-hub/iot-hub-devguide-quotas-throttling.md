---
title: Azure IOT Hub kotaları anlama ve azaltma | Microsoft Docs
description: Geliştirici Kılavuzu - IOT hub'ı ve beklenen azaltma davranış için geçerli kotalar açıklaması.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/11/2019
ms.openlocfilehash: ea9bea8b314d00db87ad7addacc49a976e0da08e
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550481"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Başvuru - IOT Hub kotaları ve azaltma

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma

Her Azure aboneliği, en fazla 50 IOT hub ve en fazla 1 ücretsiz hub sahip olabilir.

Her IoT hub'ı belirli bir katmanda belirli bir birim sayısıyla sağlanır. Katman ve birim sayısı en fazla günlük kota gönderebileceğiniz iletilerinin belirleyin. İleti boyutu 0,5 KB'lık ücretsiz katmanı hub için ve diğer katmanlar için 4 KB Günlük kotayı hesaplamak için kullanılır. Daha fazla bilgi için [Azure IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/).

Katman da üzerindeki tüm işlemler IOT hub'ı uygulayan azaltma sınırları belirler.

## <a name="operation-throttles"></a>İşlem kısıtlamalar

İşlem kısıtlamalar dakikalık aralıklar uygulanır ve kötüye kullanımı önlemek için kullanılmaya oranı sınırlamalardır. Bunlar tabi olduğunuz [trafik şekillendirme](#traffic-shaping).

Aşağıdaki tabloda zorlanan kısıtlamalar gösterilmektedir. Değerleri tek tek bir hub'ına bakın.

| Kısıtlama | Ücretsiz, S1 ve B1 | B2 ve S2 | B3 ve S3 | 
| -------- | ------- | ------- | ------- |
| [Kimlik kayıt defteri işlemlerini](#identity-registry-operations-throttle) (oluşturma, Al, Listele, güncelleştirme ve silme) | 1.67/sec/Unit (100/dk/birim) | 1.67/sec/Unit (100/dk/birim) | 83.33/sec/Unit (5000/dk/birim) |
| [Yeni cihaz bağlantılarını](#device-connections-throttle) (hızı için bu sınır geçerlidir _yeni bağlantıları_, bağlantıları toplam sayısını değil) | Daha yüksek 100/sn veya 12/sn/birim <br/> Örneğin, iki adet S1 birimi 2 olan\*12 = 24 yeni bağlantıları/sn, ancak sahip en az 100 yeni bağlantıları/sn, birimleri. Dokuz adet S1 birimi ile kullandığınız 108 yeni bağlantıları/sn (9\*12), birimleri arasında. | 120 yeni bağlantıları/sn/birim | 6000 yeni bağlantıları/sn/birim |
| Cihazdan buluta gönderim | Daha yüksek 100/sn veya 12/sn/birim <br/> Örneğin, iki adet S1 birimi 2 olan\*12 = 24/sn, ancak en az 100/sn, birimleri arasında. Dokuz adet S1 birimi ile kullandığınız 108/sn (9\*12), birimleri arasında. | 120/sn/birim | 6.000/sn/birim |
| Bulut-cihaz gönderen<sup>1</sup> | 1.67/sec/Unit (100/dk/birim) | 1.67/sec/Unit (100/dk/birim) | 83.33/sec/Unit (5000/dk/birim) |
| Bulut-cihaz alır<sup>1</sup> <br/> (yalnızca cihaz HTTPS kullandığında)| 16.67/sec/Unit (1000/dk/birim) | 16.67/sec/Unit (1000/dk/birim) | 833.33/sec/Unit (50.000/dk/birim) |
| Karşıya dosya yükleme | 1.67 dosya karşıya yükleme bildirimi/sn/birim (100/dk/birim) | 1.67 dosya karşıya yükleme bildirimi/sn/birim (100/dk/birim) | 83.33 dosya karşıya yükleme bildirimi/sn/birim (5000/dk/birim) |
| Doğrudan yöntemler<sup>1</sup> | 160KB/sn/birim<sup>2</sup> | 480KB/sn/birim<sup>2</sup> | 24MB/sn/birim<sup>2</sup> | 
| Sorgular | 20/dk/birim | 20/dk/birim | 1000/dk/birim |
| İkiz (cihaz ve modül) okuma<sup>1</sup> | 100/sn | Daha yüksek 100/sn veya 10/sn/birim | 500/sn/birim |
| İkiz Güncelleştirmesi (cihaz ve modül)<sup>1</sup> | 50/sn | Daha yüksek 50/sn veya 5/sn/birim | 250/sn/birim |
| İşler işlemleri<sup>1</sup> <br/> (oluşturma, güncelleştirme, listeleme, silme) | 1.67/sec/Unit (100/dk/birim) | 1.67/sec/Unit (100/dk/birim) | 83.33/sec/Unit (5000/dk/birim) |
| Cihaz işlemleri işleri<sup>1</sup> <br/> (ikiz güncelleştirmesi, doğrudan yöntem çağırma) | 10/sn | Daha yüksek 10/sn veya 1/sn/birim | 50/sn/birim |
| Yapılandırmalar ve edge dağıtımlarını<sup>1</sup> <br/> (oluşturma, güncelleştirme, listeleme, silme) | 0.33/sec/Unit (20/dk/birim) | 0.33/sec/Unit (20/dk/birim) | 0.33/sec/Unit (20/dk/birim) |
| Cihaz akış başlatma hızı<sup>1</sup> | 5 yeni akışlar/sn | 5 yeni akışlar/sn | 5 yeni akışlar/sn |
| En fazla eşzamanlı olarak bağlı cihaza akış sayısı<sup>1</sup> | 50 | 50 | 50 |
| En fazla cihaz akış veri aktarımı<sup>1</sup> (toplam günlük birim) | 300 MB | 300 MB | 300 MB |

<sup>1</sup>bu özellik, IOT Hub'ın temel katmanda kullanılabilir değil. Daha fazla bilgi için [doğru IOT hub'a seçme](iot-hub-scaling.md). <br/><sup>2</sup>4 KB olan ölçüm boyutunu azaltma.

### <a name="traffic-shaping"></a>Trafik şekillendirme

Veri bloğu trafiği uyum sağlamak için IOT hub'ı sınırlı bir süre için yukarıdaki kısıtlama isteklerini kabul eder. Bu isteklerin ilk birkaç hemen işlenir. Ancak, istek sayısı devam ederse azaltma, isteklerin bir kuyrukta yerleştirme ve sınırı oranı işlenen IOT hub'ı başlatır ihlal ediyor. Bu etkiyi adlı *trafik şekillendirme*. Ayrıca, bu kuyruk boyutu sınırlıdır. Kısıtlama ihlali devam ederse, sonunda sıranın dolarsa ve IOT hub'ı başlatır istekleri reddetme `429 ThrottlingException`. 

Örneğin, S1 IOT (100/sn D2C gönderir sınırı olan) Hub'ınıza saniye başına 200 CİHAZDAN buluta iletileri göndermek için sanal bir cihaz kullanın. İlk veya iki dakika için iletileri hemen işlenir. Ancak, cihaz kısıtlama sınırından daha fazla ileti göndermeye devam ettiğinden, IOT hub'ı yalnızca işlem 100 iletileri saniyede başlar ve rest sıraya koyar. Daha yüksek gecikme süresiyle fark etmeye başlarsınız. Sonuç olarak, başlama başlamadan `429 ThrottlingException` kuyruk dolgular oluşturan ve "sayısında azaltma hataları" olarak [IOT Hub'ın ölçümleri](iot-hub-metrics.md) artırma başlatır.

### <a name="identity-registry-operations-throttle"></a>Kimlik kayıt defteri işlemlerini kısıtlama

Cihaz kimlik kayıt defteri işlemleri, cihaz yönetimi ve sağlama senaryolarında çalışma zamanı kullanmak için tasarlanmıştır. Okuma veya çok sayıda cihaz kimliklerini güncelleştirme aracılığıyla desteklenir [içeri ve dışarı aktarma işleri](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

### <a name="device-connections-throttle"></a>Cihaz bağlantılarını azaltma

*Cihaz bağlantılarını* kısıtlama oranı, yeni cihaz bağlantılarını kurulabileceği ile IOT hub'ı yönetir. *Cihaz bağlantılarını* azaltma, en fazla eşzamanlı olarak bağlanan cihaz sayısını belirleyen değil. *Cihaz bağlantılarını* hızı azaltma, IOT hub için sağlanan birim sayısını bağlıdır.

Örneğin, tek bir S1 birimi satın alırsanız, saniye başına 100 bağlantılarının bir kısıtlama alın. Bu nedenle, 100 bağlanmak için 000 cihazları, en az 1.000 saniye (yaklaşık 16 dakika) alır. Ancak, kimlik kayıt defterinde kayıtlı cihazları olan sayıda eşzamanlı olarak bağlanan cihazlara sahip olabilir.


## <a name="other-limits"></a>Diğer sınırlamaları

IOT hub'ı diğer kullanım sınırlamaları uygular:

| İşlem | Sınır |
| --------- | ----- |
| Cihazlar | Tek bir IOT hub'ına bağlanabilir cihazların sayısı 1.000.000 ' dir. Bu sınırı artırmak için tek yolu başvurmaktır [Microsoft Support](https://azure.microsoft.com/support/options/).| 
| Karşıya dosya yükleme URI'ler | 10.000 SAS URI'leri bir depolama hesabı için tek seferde çıkarılabilir. <br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| İşleri<sup>1</sup> | En fazla eşzamanlı iş, 1 (ücretsiz ve S1 için), 5 (S2 için) ve 10 (S3 için). Ancak, en fazla eşzamanlı [cihaz içeri/dışarı aktarma işleri](iot-hub-bulk-identity-mgmt.md) tüm katmanlarda 1'dir. <br/>30 gün için iş geçmişi korunur. |
| Ek uç noktalar | SKU Ücretli hubs 10 ek uç noktalar olabilir. Ücretsiz SKU hub'ları, bir ek uç nokta olabilir. |
| İleti yönlendirme kuralları | SKU Ücretli hubs 100 yönlendirme kuralları olabilir. Ücretsiz SKU hubs beş yönlendirme kuralları olabilir. |
| CİHAZDAN buluta ileti gönderme | En büyük ileti boyutu 256 KB |
| Bulut-cihaz Mesajlaşma<sup>1</sup> | En büyük ileti boyutu 64 KB. Bekleyen iletileri teslim için en fazla 50'dir. |
| Doğrudan yöntem<sup>1</sup> | En fazla bir doğrudan yöntem yükünün boyutu 128 KB ' dir. |
| Otomatik cihaz yapılandırmaları<sup>1</sup> | Ücretli SKU hub başına 100 yapılandırmalar. Ücretsiz SKU hub başına 20 yapılandırmalar. |
| Otomatik Edge dağıtımlarını<sup>1</sup> | Dağıtım başına 20 modüller. Ücretli SKU hub başına 100 dağıtımları. Ücretsiz SKU hub başına 20 dağıtımları. |
| İkizlerini<sup>1</sup> | 8 KB'lık ikizi bölümü (etiketler, istenen özellikler, bildirilen özellikleri) başına en büyük boyut: |

<sup>1</sup>bu özellik, IOT Hub'ın temel katmanda kullanılabilir değil. Daha fazla bilgi için [doğru IOT hub'a seçme](iot-hub-scaling.md).

## <a name="increasing-the-quota-or-throttle-limit"></a>Kota veya kısıtlama sınırı artırma

Herhangi bir zamanda kotaları artırma veya azaltma sınırları [bir IOT hub'ındaki sağlanan birimi sayısını artırabilir](iot-hub-upgrade.md).

## <a name="latency"></a>Gecikme

Tüm işlemler için düşük gecikme süresi sağlamak IOT hub'ı içindedir. Ancak, ağ koşulları ve öngörülemeyen diğer faktörler nedeniyle, belirli bir gecikme süresi garanti edemez. Çözümünüzü tasarlarken, aşağıdakileri yapmalısınız:

* En yüksek gecikme süresi hakkında varsayımlar her IOT Hub işleminin yapmaktan kaçının.
* IOT hub'ınıza cihazlarınıza en yakın Azure bölgesine sağlayın.
* Azure IOT Edge cihazı veya bir ağ geçidi cihazı yakın gecikmeye duyarlı işlemleri gerçekleştirmek için kullanmayı düşünün.

Birden çok IOT Hub birimi, daha önce açıklandığı gibi azaltma etkiler, ancak herhangi bir ek gecikme avantajları veya garanti sağlamaz.

İşlem gecikme süresi içinde beklenmeyen artışları görürseniz, başvurun [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub'ın ayrıntılı incelemesi için azaltma davranışı, blog gönderisine bakın [IOT Hub'ın azaltma ve](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/).

Bu IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md)
