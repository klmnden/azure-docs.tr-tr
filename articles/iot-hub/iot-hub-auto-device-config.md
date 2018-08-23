---
title: Yapılandırma ve IOT cihazlarını uygun ölçekte Azure IOT Hub ile izleme | Microsoft Docs
description: Bir yapılandırma birden fazla cihaza atamak için Azure IOT Hub otomatik cihaz yapılandırmaları kullanın
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: 2edde122b109779794bb86752d69a5318edb9235
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "42060884"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-portal"></a>Yapılandırma ve IOT cihazlarını uygun ölçekte Azure portalını kullanarak izleme

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Azure IOT hub otomatik cihaz Yönetimi döngülerini tamamına büyük cihaz filolarına yönetme yinelenen ve karmaşık görevlerinin birçoğunu otomatik hale getirir. İle otomatik cihaz yönetimi, bir dizi cihazda özelliklerine göre hedef, istenen yapılandırmasını tanımlamak ve IOT Hub'ın kapsama geldikleri her cihazları güncelleştirmek olanak tanır.  Bu işlem, ayrıca tamamlama ve uyumluluk, tanıtıcı birleştirme ve çakışmaları özetlemenize ve aşamalı bir yaklaşımla yapılandırmaları Dışarı Aktar sağlayacak bir otomatik cihaz Yapılandırması kullanılarak gerçekleştirilir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Otomatik cihaz yapılandırmaları iş istediğiniz özelliklere sahip bir dizi cihaz ikizlerini güncelleştiriliyor ve özetini üzerinde cihaz ikizini raporlama tarafından bildirilen özellikler.  Yeni bir sınıf ve adlı bir JSON belge tanıtır bir *yapılandırma* üç bölümü vardır:

* **Hedef koşulu** güncelleştirilmesi için cihaz ikizlerini kapsamını tanımlar. Hedef koşul cihaz ikizi etiketler üzerinde bir sorgu olarak belirtildiğinden ve/veya bildirilen özellikler.

* **Hedef içerik** veya hedeflenen cihaz ikizlerini güncelleştirilemiyor için istenen özellikleri tanımlar. İçerik değiştirilmesi için istenen özellikler kısmında bir yolu içerir.

* **Ölçümleri** gibi çeşitli yapılandırma durumlarının Özet sayıları tanımlamak **başarı**, **sürüyor**, ve **hata**. Özel ölçümler olarak cihaz sorguları belirtilen çiftinin bildirilen özelliklerini.  Sistem ölçümlerini ikizi hedeflenen cihaz ikizlerini sayısı ve başarıyla güncelleştirildi ikizlerini sayısı gibi güncelleştirme durumunu ölçen varsayılan ölçümleridir. 

## <a name="implement-device-twins-to-configure-devices"></a>Cihazları yapılandırmak için cihaz ikizlerini uygulayın

Otomatik cihaz yapılandırmaları, cihazlar ve bulut arasında durumunu eşitlemek için cihaz ikizlerini kullanılmasını gerektirir. Başvurmak [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](iot-hub-devguide-device-twins.md) için cihaz ikizlerini kullanma yönergeleri.

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirleyin

Bir yapılandırma oluşturmadan önce değiştirmek istediğiniz hangi cihazların belirtmeniz gerekir. Azure IOT Hub cihaz ikizinde etiketleri kullanarak cihazları tanımlar. Her cihazı birden fazla etikete sahip olabilir ve çözümünüz için mantıklı olan herhangi bir şekilde tanımlayabilirsiniz. Örneğin, farklı konumlardaki cihazlarını yönetiyorsanız, bir cihaz çifti için aşağıdaki etiketlerin ekleyebilirsiniz:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="create-a-configuration"></a>Bir yapılandırma oluşturun

1. İçinde [Azure portalında](https://portal.azure.com), IOT hub'ınıza gidin. 

2. Seçin **IOT cihaz Yapılandırması**.

3. Seçin **Yapılandırması Ekle**.

Bir yapılandırma oluşturmak için beş adım vardır. Aşağıdaki bölümlerde, her birini yol. 

### <a name="name-and-label"></a>Ad ve Etiket

1. Yapılandırmanızı 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.

2. Yapılandırmalarınızı izlenmesine yardımcı olması için etiketler ekleyin. Etiketler **adı**, **değer** yapılandırmanızı açıklayan çiftleri. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`.

3. Seçin **sonraki** sonraki adıma geçmek için. 

### <a name="specify-settings"></a>Ayarları belirtin

Bu bölümde, hedeflenen cihaz ikizlerini ayarlamak için hedef içeriği belirtir. Her dizi ayarları için iki giriş vardır. İlk ayarlanacak çiftinin istenen özelliklerini JSON bölümündeki yolu cihaz ikizi yoludur.  Bu bölümde eklenecek JSON içeriği saniyedir. Örneğin, cihaz İkizi yolunu ve içerik şunu ayarlayın:

![İçerik ve cihaz İkizi yolunu ayarlayın.](./media/iot-hub-auto-device-config/create-configuration-full-browser.png)

Tek tek ayarlar yolun tamamını cihaz İkizi yolunu ve hiçbir köşeli ayraçlar içerikle değer belirterek de ayarlayabilirsiniz. Örneğin, cihaz İkizi yolunu ayarlamak `properties.desired.chiller-water.temperature` ve içerik kümesine `66`.

İki veya daha fazla yapılandırması aynı cihaz İkizi yolunu hedefliyorsanız, en yüksek öncelikli bir yapılandırma içerikten uygulanır (öncelik adım 4'te tanımlanır).

Bir özelliği kaldırmak istiyorsanız, özellik değerini belirtin `null`.

Ek ayarlar seçerek ekleyebilirsiniz **cihaz İkizi ayar Ekle**.

### <a name="specify-metrics-optional"></a>Ölçümler (isteğe bağlı) belirtin

Bir cihaz, uygulama yapılandırma içeriği sonucunda geri bildirebilir çeşitli durumları özeti sayıları ölçümleri sağlar. Örneğin, ayarları değişiklikleri, hatalar için bir ölçüm ve bir ölçüm başarılı ayarları değişiklikleri için bekleyen bir ölçüm için oluşturabilir.

1. İçin bir ad girin **ölçüm adı**.

2. İçin bir sorgu girin **ölçüm ölçütleri**.  Sorguyu cihazda alan çiftinin bildirilen özelliklerini.  Ölçüm, sorgu tarafından döndürülen satır sayısını temsil eder.

Örneğin:

```sql
SELECT deviceId FROM devices 
  WHERE properties.reported.chillerWaterSettings.status='pending'
```

Yapılandırma, örneğin uygulanmış olduğunu bir yan tümce ekleyebilirsiniz: 

```sql
/* Include the double brackets. */
SELECT deviceId FROM devices 
  WHERE configurations.[[yourconfigname]].status='Applied'
```

### <a name="target-devices"></a>Hedef Cihazlar

Bu yapılandırmayı alması gereken belirli cihazları hedeflemek için etiketler özelliği, cihaz ikizlerini kullanın.  Ayrıca cihaz cihazları hedefleyebilirsiniz çiftinin bildirilen özelliklerini.

Aynı cihazda birden fazla yapılandırması hedefleyebilir olduğundan, her yapılandırma bir öncelik numarası vermesi gerekir. Şimdiye kadar bir çakışma varsa, yapılandırmanın en yüksek öncelikli kazanır. 

1. Yapılandırma için pozitif bir tamsayı girin **öncelik**. En yüksek sayısal değeri en yüksek öncelikli olarak kabul edilir. İki yapılandırma aynı öncelik numarasına sahip, çoğu oluşturulmuş bir son kazanır. 

2. Girin bir **hedef koşulu** hangi cihazların bu yapılandırma ile hedeflenecek belirlemek için. Koşul, cihaz ikizi etiketlere göre veya cihaz çiftinin bildirilen özellikler ve ifade biçim ile eşleşmesi. Örneğin, `tags.environment='test'` veya `properties.reported.chillerProperties.model='4000x'`. Belirtebileceğiniz `*` tüm cihazları hedeflemek için.

3. Seçin **sonraki** son adıma geçmek için.

### <a name="review-configuration"></a>Yapılandırmayı Gözden Geçir

Yapılandırma bilgilerinizi gözden geçirin ve ardından **Gönder**.

## <a name="monitor-a-configuration"></a>İzleyici yapılandırma

Bir yapılandırma ayrıntılarını görüntülemek ve onu çalıştıran cihazları izlemek için aşağıdaki adımları kullanın:

1. İçinde [Azure portalında](https://portal.azure.com), IOT hub'ınıza gidin. 

2. Seçin **IOT cihaz Yapılandırması**.

3. Yapılandırma listesini inceleyin. Her yapılandırma için aşağıdaki ayrıntıları görüntüleyebilirsiniz:

   * **Kimliği** -yapılandırmasının adı.

   * **Hedef koşul** -hedeflenen cihazları tanımlamak için kullanılan sorgu.

   * **Öncelik** -yapılandırmaya atanan öncelik numarası.

   * **Oluşturma zamanı** -yapılandırma oluşturulduğu zaman damgası. Bu zaman damgasından iki yapılandırması aynı önceliğe sahip olduğunuzda TIES ayırmak için kullanılır. 

   * **Sistem ölçümlerini** -IOT Hub tarafından hesaplanır ve geliştiriciler tarafından özelleştirilemez ölçümleri. Hedeflenen hedef koşulu sağlayan cihaz ikizlerini sayısını belirtir. Geçerli belirlenen ayrı, daha yüksek öncelikli bir yapılandırma değişiklikleri de yaptığınız olay, kısmi değişiklikler içerebilen yapılandırması tarafından değiştirilmiş olan cihaz ikizlerini sayısı. 

   * **Özel ölçümler** -cihaz çifti sorguları olarak geliştirici tarafından belirtilen ölçüm bildirilen özellikler.  Yapılandırma en fazla beş özel ölçümler tanımlanabilir. 
   
4. İzlemek istediğiniz yapılandırmayı seçin.  

5. Yapılandırma ayrıntılarını inceleyin. Alınan yapılandırmayı ayrıştıramadığını cihazlar hakkındaki belirli ayrıntılarını görüntülemek için sekmeleri kullanabilirsiniz.

   * **Hedef koşul** -hedef koşulu cihazlar. 

   * **Ölçümleri** -sistem ölçümlerini ve özel ölçümler listesi.  Açılan menü ölçüm ve ardından seçerek sayılan her ölçüm için cihazların listesini görüntüleyebilirsiniz **cihazlarını görüntülemek**.

   * **Cihaz İkizi ayarları** -yapılandırma tarafından ayarlanan cihaz ikizi ayarları. 

   * **Yapılandırma etiketleri** -yapılandırma açıklamak için kullanılan anahtar-değer çiftleri.  Etiketleri işlevselliğini etkisi yok. 

## <a name="modify-a-configuration"></a>Bir yapılandırmasını Değiştir

Yapılandırma değiştirdiğinizde değişiklikler hemen hedeflenen tüm cihazlara çoğaltın. 

Hedef koşul güncelleştirme aşağıdaki güncelleştirmeleri oluşur:

* Cihaz ikizi eski hedef koşul karşılanmadıysa, ancak yeni hedef koşulunu ve bu yapılandırma, cihaz ikizi için en yüksek öncelikli ise, bu yapılandırma için cihaz ikizi'e uygulanır. 

* Cihaz ikizi artık hedef koşulu karşılıyorsa, yapılandırma ayarları kaldırılır ve cihaz ikizinde sonraki en yüksek öncelikli bir yapılandırma ile değiştirilecek. 

* Artık bu yapılandırma şu anda çalışan bir cihaz ikizi hedef koşulu karşılayan ve diğer tüm yapılandırmaları hedef koşulu karşılamıyor ise yapılandırma ayarlarından kaldırılacak ve ikizinin diğer herhangi bir değişiklik yapılmaz. 

Bir yapılandırmasını değiştirmek için aşağıdaki adımları kullanın: 

1. İçinde [Azure portalında](https://portal.azure.com), IOT hub'ınıza gidin. 

2. Seçin **IOT cihaz Yapılandırması**. 

3. Değiştirmek istediğiniz yapılandırmayı seçin. 

4. Aşağıdaki alanları güncelleştirmeleri yapın: 

   * Hedef koşul 
   * Etiketler 
   * Öncelik 
   * Ölçümler

4. **Kaydet**’i seçin.

5. Bağlantısındaki [yapılandırma izleme](#monitor-a-configuration) dağıtmadan değişiklikleri izlemek için. 

## <a name="delete-a-configuration"></a>Bir yapılandırmayı silmek

Bir yapılandırmayı silmek, tüm cihaz ikizlerini kendi sonraki en yüksek öncelikli yapılandırmasına yararlanın. Cihaz ikizlerini herhangi bir yapılandırma hedef koşulu karşılamıyorsanız, sonra başka bir ayarları uygulanır. 

1. İçinde [Azure portalında](https://portal.azure.com) IOT hub'ınıza gidin. 

2. Seçin **IOT cihaz Yapılandırması**. 

3. Silmek istediğiniz yapılandırmayı seçmek için onay kutusunu kullanın. 

4. **Sil**’i seçin.

5. Bir komut istemi onaylamanızı ister.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılandırmak ve IOT cihazlarını uygun ölçekte izleyin. Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [Toplu, IOT Hub cihaz kimliklerini yönetme](iot-hub-bulk-identity-mgmt.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)

IOT Hub cihazı sağlama hizmeti kullanarak müdahalesi gerektirmeyen, tam zamanında sağlama etkinleştireceğinizi öğrenmek için keşfetmek için: 

* [Azure IoT Hub Cihazı Sağlama Hizmeti](/azure/iot-dps)
