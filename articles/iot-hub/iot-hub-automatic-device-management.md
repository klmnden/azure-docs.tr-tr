---
title: Uygun ölçekte Azure IOT Hub ile otomatik cihaz Yönetimi | Microsoft Docs
description: Bir yapılandırma birden çok IOT cihazlarına atamak için Azure IOT Hub otomatik cihaz Yönetimi'ni kullanın
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: 598bf82e375f472b2f723c3462ba7ba7b4d25fbe
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012958"
---
# <a name="automatic-iot-device-management-at-scale-using-the-azure-portal"></a>Azure portalını kullanarak ölçekte otomatik IOT cihaz Yönetimi

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Azure IOT hub otomatik cihaz Yönetimi büyük cihaz filolarına yönetme yinelenen ve karmaşık görevlerinin birçoğunu otomatik hale getirir. İle otomatik cihaz yönetimi, bir dizi cihazda özelliklerine göre hedef, istenen yapılandırmasını tanımlamak ve ardından IOT Hub'ın kapsama geldikleri cihazları güncelleştirmesine olanak sağlar. Bu güncelleştirme bitti kullanarak bir _otomatik cihaz yapılandırma_, tamamlama ve uyumluluk, tanıtıcı birleştirme ve çakışmaları özetlemek ve aşamalı bir yaklaşımla yapılandırmaları Dışarı Aktar seçmenizi sağlar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Otomatik cihaz Yönetimi works istediğiniz özelliklere sahip bir dizi cihaz ikizlerini güncelleştiriliyor ve üzerinde cihaz ikizini tabanlı bir Özet raporlama tarafından bildirilen özellikler.  Yeni bir sınıf ve adlı bir JSON belge tanıtır bir *yapılandırma* , üç bölümden oluşur:

* **Hedef koşulu** güncelleştirilmesi için cihaz ikizlerini kapsamını tanımlar. Hedef koşul cihaz ikizi etiketler üzerinde bir sorgu olarak belirtildiğinden ve/veya bildirilen özellikler.

* **Hedef içerik** veya hedeflenen cihaz ikizlerini güncelleştirilemiyor için istenen özellikleri tanımlar. İçerik değiştirilmesi için istenen özellikler kısmında bir yolu içerir.

* **Ölçümleri** gibi çeşitli yapılandırma durumlarının Özet sayıları tanımlamak **başarı**, **sürüyor**, ve **hata**. Özel ölçümler olarak cihaz sorguları belirtilen çiftinin bildirilen özelliklerini.  Sistem, hedeflenen cihaz ikizlerini sayısı ve başarıyla güncelleştirildi ikizlerini sayısı gibi ikizi güncelleştirme durumunu ölçen varsayılan ölçümler ölçümleridir. 

## <a name="implement-device-twins-to-configure-devices"></a>Cihazları yapılandırmak için cihaz ikizlerini uygulayın

Otomatik cihaz yapılandırmaları, cihazlar ve bulut arasında durumunu eşitlemek için cihaz ikizlerini kullanılmasını gerektirir.  Başvurmak [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](iot-hub-devguide-device-twins.md) için cihaz ikizlerini kullanma yönergeleri.

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirleyin

Bir yapılandırma oluşturmadan önce değiştirmek istediğiniz hangi cihazların belirtmeniz gerekir. Azure IOT Hub cihaz ikizinde etiketleri kullanarak cihazları tanımlar. Her cihazı birden fazla etikete sahip olabilir ve çözümünüz için mantıklı olan herhangi bir şekilde tanımlayabilirsiniz. Örneğin, farklı konumlardaki cihazlarını yönetiyorsanız, aşağıdaki etiketleri için cihaz ikizi ekleyin:

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

Ölçümleri yapılandırma içeriği uyguladıktan sonra bir cihazı geri bildirebilir çeşitli durumları özeti sayısını sağlar. Örneğin, ayarları değişiklikleri, hatalar için bir ölçüm ve bir ölçüm başarılı ayarları değişiklikleri için bekleyen bir ölçüm için oluşturabilir.

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

1. İçinde [Azure portalında](https://portal.azure.com), IOT hub'ınıza gidin. 

2. Seçin **IOT cihaz Yapılandırması**. 

3. Silmek istediğiniz yapılandırmayı seçmek için onay kutusunu kullanın. 

4. **Sil**’i seçin.

5. Bir komut istemi onaylamanızı ister.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, yapılandırma ve izleme IOT cihazlarını uygun ölçekte öğrendiniz. Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [Toplu, IOT Hub cihaz kimliklerini yönetme](iot-hub-bulk-identity-mgmt.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)

IOT Hub cihazı sağlama hizmeti kullanarak müdahalesi gerektirmeyen, tam zamanında sağlama etkinleştireceğinizi öğrenmek için keşfetmek için: 

* [Azure IoT Hub Cihazı Sağlama Hizmeti](/azure/iot-dps)
