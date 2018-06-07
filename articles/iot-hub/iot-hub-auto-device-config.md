---
title: Yapılandırma ve Azure IOT Hub ile ölçekte IOT cihazları izleme | Microsoft Docs
description: Bir yapılandırma için birden çok aygıt atamak için Azure IOT hub'ı otomatik cihaz yapılandırmalarını kullanın
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: fe5ce960663f39d4f2c87a7bbffa091d327e9559
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632457"
---
# <a name="configure-and-monitor-iot-devices-at-scale---preview"></a>Yapılandırma ve IOT cihazları ölçekte izleme - Önizleme

Azure IOT hub'ı otomatik cihaz yönetiminde birçok büyük cihaz fleets kendi ömürleri tamamen yönetme yinelenen ve karmaşık görevleri otomatik hale getirir. Otomatik cihaz yönetimi ile cihazları özelliklerine göre bir dizi hedef, istenen yapılandırma tanımlayın ve IOT Hub'ın kapsam içine geldikleri her aygıtları güncelleştirmesi sağlayabilirsiniz.  Bu ayrıca tamamlama ve uyumluluk, tanıtıcı birleştirme ve çakışmaları özetlemek ve yapılandırmaları aşamalı bir yaklaşımla kullanıma alma sağlayacak bir otomatik cihaz Yapılandırması kullanılarak gerçekleştirilir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

İstenen özelliklere sahip bir cihaz çiftlerini kümesini güncelleştirme ve cihaz çifti dayalı bir özeti raporlama tarafından otomatik cihaz yapılandırmalarını iş özellikleri bildirdi.  Yeni bir sınıf ve JSON belgesi olarak adlandırılan tanıtır bir _yapılandırma_ üç bölümden vardır:

* **Hedef koşulu** güncelleştirilmesi cihaz çiftlerini kapsamını tanımlar. Hedef durumu cihaz çifti etiketleri sorgu olarak belirtildiğinden ve/veya özellikler bildirdi.

* **Hedef içerik** eklenemez veya hedeflenen cihaz çiftlerini güncelleştirilmiş için istenen özelliklerini tanımlar. İçerik değiştirilecek istenen özellikler kısmında yolunu içerir.

* **Ölçümleri** gibi çeşitli yapılandırma durumlarını Özet sayısını tanımlamak **başarı**, **sürüyor**, ve **hata**. Cihaz sorgulamaları olarak belirtilen özel ölçümleri twin özellikleri bildirdi.  Sistem, hedeflenen cihaz çiftlerini sayısı ve başarıyla güncelleştirildi çiftlerini sayısı gibi twin güncelleştirme durumunu ölçmek varsayılan ölçümleri ölçümleridir. 

> [!Note]
> Önizleme sırasında bu özellik IOT hub'ları Doğu ABD, Batı ABD, Kuzey Avrupa ve Batı Avrupa bölgelerde kullanılamaz.

## <a name="implement-device-twins-to-configure-devices"></a>Cihazları yapılandırmak için cihaz çiftlerini uygulama

Otomatik cihaz yapılandırmalarını cihazlar ve bulut arasında durumunu eşitlemek için cihaz çiftlerini kullanılmasını gerektirir.  Başvurmak [IOT hub'ı Anlama ve Kullan cihaz çiftlerini] [ lnk-device-twin] cihaz çiftlerini kullanma konusunda yönergeler için.

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirlemek

Bir yapılandırma oluşturmadan önce hangi cihazların etkilenmesini istediğiniz belirtmeniz gerekir. Azure IOT Hub cihaz çiftine etiketleri kullanarak cihazları tanımlar. Her cihazı birden fazla etikete sahip olabilir ve çözümünüz için anlamlı herhangi bir şekilde tanımlayabilirsiniz. Örneğin, farklı konumlarda aygıtları yönetiyorsanız, bir cihaz çifti aşağıdaki etiketlerin ekleyebilirsiniz:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="create-a-configuration"></a>Bir yapılandırma oluşturmak

1. İçinde [Azure portal][lnk-portal], IOT hub'ınızı gidin. 
1. Seçin **IOT cihaz yapılandırması (Önizleme)**.
1. Seçin **yapılandırması eklemek**.

Bir yapılandırma oluşturmak için beş adım vardır. Aşağıdaki bölümlerde her birini yol. 

### <a name="step-1-name-and-label"></a>1. adım: Ad ve etiket

1. Yapılandırmanızı en fazla 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Yapılandırmalarınızı izlemenize yardımcı olması için etiketler ekleyin. Etiketler **adı**, **değeri** yapılandırmanızı açıklamak çiftleri. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`.
1. Seçin **sonraki** iki adımına geçmek için. 

### <a name="step-2-specify-settings"></a>2. adım: Ayarlarını belirtin

Bu bölümde, hedeflenen cihaz çiftlerini ayarlanacak hedef içeriği belirtir. Her ayar için iki girdi vardır. İlk ayarlanacak istenen twin özellikleri JSON bölümde yolu cihaz çifti yoludur.  Bu bölümde eklenecek JSON içeriği saniyedir. Örneğin, aygıt çifti yolu ve içerik şunu ayarlayın:

![Cihaz çifti yolu ve içerik ayarlayın](./media/iot-hub-auto-device-config/create-configuration-full-browser.png)

Cihaz çifti yolunu ve hiçbir köşeli içerikle değerinde yolun tamamını belirterek tek tek ayarların de ayarlayabilirsiniz. Örneğin, aygıt çifti yolu kümesine `properties.desired.chiller-water.temperature` ve içeriği ayarlamak: `66`

İki veya daha fazla yapılandırmaları aynı aygıt çifti yolu hedefliyorsanız, yüksek öncelikli bir yapılandırma içerikten uygulanır (4. adımda öncelik tanımlanmış).

Bir özelliği kaldırmak isterseniz, özellik değeri belirtin `null`.

Seçerek ek ayarlar ekleyebilirsiniz **cihaz çifti ayar Ekle**

### <a name="step-3-specify-metrics-optional"></a>3. adım: Ölçümler (isteğe bağlı) belirtin

Bir aygıt yapılandırma içeriği uygulama sonucunda geri bildirebilir çeşitli durumlarını Özet sayısı ölçümleri sağlar. Örneğin, bekleyen ayar değişiklikleri, hatalar için bir ölçüm ve başarılı ayar değişiklikleri yönelik ölçüm için bir ölçüm oluşturabilir.

1. İçin bir ad girin **ölçüm adı**
1. İçin bir sorgu girin **ölçüm ölçütleri**.  Sorgu aygıtı temel alarak twin özellikleri bildirdi.  Ölçüm sorgu tarafından döndürülen satır sayısını temsil eder.

Örneğin, `SELECT deviceId FROM devices WHERE properties.reported.chillerWaterSettings.status='pending'`

Yapılandırma, örneğin uygulanmış olduğunu yan tümcesi içerebilir: `SELECT deviceId FROM devices WHERE configurations.[[yourconfigname]].status='Applied'` çift köşeli ayraçlar dahil olmak üzere.


### <a name="step-4-target-devices"></a>Adım 4: Hedef cihazlar

Bu yapılandırmayı alması gereken belirli cihazları hedeflemek için cihaz çiftlerini etiketleri özelliğinden kullanın.  Cihazlar cihaz tarafından aynı zamanda hedefleyebilirsiniz twin özellikleri bildirdi.

Birden çok yapılandırmayı aynı aygıt hedefleyebilir olduğundan, her yapılandırma bir öncelik numarası vermesi gerekir. Şimdiye kadar bir çakışma varsa, en yüksek öncelikli yapılandırma kazanır. 

1. Yapılandırma için pozitif bir tamsayı girin **öncelik**. En yüksek sayısal değer, en yüksek öncelikli olarak kabul edilir. Aynı öncelik numarasını iki yapılandırmaları varsa, çoğu oluşturulmuş bir son kazanır. 
1. Girin bir **hedef koşulu** hangi aygıtların bu yapılandırma ile hedeflenir belirlemek için. Koşul cihaz çifti etiketlere göre veya cihaz çifti özellikleri bildirilen ve ifade biçimi eşleşmelidir. Örneğin, `tags.environment='test'` veya `properties.reported.chillerProperties.model='4000x'`. 
1. Seçin **sonraki** son adımına geçmek için.

### <a name="step-5-review-configuration"></a>5. adım: Yapılandırmayı gözden geçir

Yapılandırma bilgilerini gözden geçirin ve ardından **gönderme**.

## <a name="monitor-a-configuration"></a>İzleyici bir yapılandırma

Bir yapılandırma ayrıntılarını görüntülemek ve onu çalıştıran aygıtları izlemek için aşağıdaki adımları kullanın:

1. İçinde [Azure portal][lnk-portal], IOT hub'ınızı gidin. 
1. Seçin **IOT cihaz yapılandırması (Önizleme)**.
1. Yapılandırma listesini inceleyin. Her yapılandırma için aşağıdaki ayrıntıları görüntüleyebilirsiniz:
   * **Kimliği** -yapılandırmasının adı.
   * **Hedef koşulu** -hedeflenen cihazlar tanımlamak için kullanılan sorgu.
   * **Öncelik** -yapılandırmaya atanan öncelik numarası.
   * **Oluşturulma zamanı** -yapılandırma oluşturulduğu gelen zaman damgası. Bu zaman damgası iki yapılandırmaları aynı önceliğe sahip olduğunuzda TIES ayırmak için kullanılır. 
   * **Sistem ölçümleri** -IOT Hub tarafından hesaplanır ve geliştiriciler tarafından özelleştirilemez ölçümleri. Hedeflenen hedef koşuluyla eşleşen cihaz çiftlerini sayısını belirtir. Geçerli belirtilen kısmi değişiklikleri ayrı, daha yüksek öncelikli bir yapılandırma aynı zamanda yapılan değişiklikler gerektiğinde, içerebilen yapılandırması tarafından değiştirilmiş cihaz çiftlerini sayısı. 
   * **Özel ölçümleri** -cihaz çifti sorguları olarak geliştirici tarafından belirtilen ölçümleri özellikleri bildirdi.  En fazla beş özel ölçümleri başına yapılandırma tanımlanabilir. 
   
1. İzlemek istediğiniz yapılandırmayı seçin.  
1. Yapılandırma ayrıntıları inceleyin. Alınan yapılandırmayı aygıtları belirli ayrıntılarını görüntülemek için sekmeleri kullanabilirsiniz: 
   * **Hedef koşulu** -hedef koşuluyla eşleşen aygıtlar. 
   * **Ölçümleri** -sistem ölçümleri ve özel ölçümleri listesi.  Açılan ölçüm ve sonra seçerek sayılır cihazların her ölçüm için bir listesini görüntüleyebileceğiniz **aygıtlarını görüntüle**.
   * **Cihaz çifti ayarları** -yapılandırma tarafından ayarlanan cihaz çifti ayarları. 
   * **Yapılandırma etiketleri** -yapılandırma tanımlamak için kullanılan anahtar-değer çiftleri.  Etiketleri işlevselliğini etkisi yok. 

## <a name="modify-a-configuration"></a>Bir yapılandırmasını Değiştir

Bir yapılandırma değiştirdiğinizde, değişikliklerin hemen tüm hedeflenen cihazlar için çoğaltılır. 

Hedef durumu güncelleştirirseniz, aşağıdaki güncelleştirmeleri oluşur:
* Cihaz çifti eski hedef durumu karşılamadı, ancak yeni hedef koşulunu ve bu yapılandırma, cihaz çifti için en yüksek öncelikli ise, bu yapılandırma için cihaz çifti uygulanır. 
* Cihaz çifti artık hedef koşulu karşılıyorsa, yapılandırma ayarları kaldırılır ve sonraki en yüksek öncelikli bir yapılandırma cihaz çiftine değiştirilecek. 
* Artık bu yapılandırma şu anda çalışan bir cihaz çifti hedef koşulunu ve diğer tüm yapılandırmaları hedef koşulu karşılamıyor, yapılandırma ayarlarından kaldırılacak ve başka bir değişiklik üzerinde twin yapılmayacak. 

Yapılandırmasını değiştirmek için aşağıdaki adımları kullanın: 

1. İçinde [Azure portal][lnk-portal], IOT hub'ınızı gidin. 
1. Seçin **IOT cihaz yapılandırması (Önizleme)**. 
1. Değiştirmek istediğiniz yapılandırmayı seçin. 
1. Güncelleştirmeleri aşağıdaki alanları olun: 
   * Hedef durumu 
   * Etiketler 
   * Öncelik 
   * Ölçümler
1. **Kaydet**’i seçin.
1. [İzleyici yapılandırma] adımları [bağlantı dağıtmadan değişiklikleri izlemek için monitör]. 

## <a name="delete-a-configuration"></a>Bir yapılandırma Sil

Bir yapılandırma sildiğinizde, tüm cihaz çiftlerini kendi sonraki en yüksek öncelikli bir yapılandırma üzerinde alın. Cihaz çiftlerini hedef durumu ve herhangi bir yapılandırma karşılamıyorsa, diğer bir ayarları uygulanır. 

1. İçinde [Azure portal][lnk-portal], IOT hub'ınızı gidin. 
1. Seçin **IOT cihaz yapılandırması (Önizleme)**. 
1. Silmek istediğiniz yapılandırma seçmek için onay kutusunu kullanın. 
1. **Sil**’i seçin.
1. Bir istem onaylamanızı ister.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, öğrenilen nasıl yapılandırmak ve IOT cihazları ölçekte izleme. Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IOT Hub cihaz kimliklerinizi toplu yönetme][lnk-bulkIDs]
* [IOT hub'ı ölçümleri][lnk-metrics]
* [İzleme işlemleri][lnk-monitor]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

IOT Hub cihaz sağlama hizmeti kullanarak zero touch, yalnızca zaman sağlama etkinleştirmek için bkz: keşfetmek için: 

* [Azure IOT Hub cihaz hizmet sağlama][lnk-dps]

[lnk-device-twin]: iot-hub-devguide-device-twins.md
[lnk-bulkIDs]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
[lnk-portal]: https://portal.azure.com
