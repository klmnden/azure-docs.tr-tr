---
title: Yapılandırma ve IOT cihazları ile Azure IOT hub'ı (CLI) ölçekte izleme | Microsoft Docs
description: Bir yapılandırma için birden çok aygıt atamak için Azure IOT hub'ı otomatik cihaz yapılandırmalarını kullanın
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: c12a07aabdecb070cfa99f8851f907499599a1fc
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036831"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak ölçekte IOT cihazları izleme

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Azure IOT hub'ı otomatik cihaz yönetiminde birçok büyük cihaz fleets kendi ömürleri tamamen yönetme yinelenen ve karmaşık görevleri otomatik hale getirir. Otomatik cihaz yönetimi ile cihazları özelliklerine göre bir dizi hedef, istenen yapılandırma tanımlayın ve IOT Hub'ın kapsam içine geldikleri her aygıtları güncelleştirmesi sağlayabilirsiniz.  Bu ayrıca tamamlama ve uyumluluk, tanıtıcı birleştirme ve çakışmaları özetlemek ve yapılandırmaları aşamalı bir yaklaşımla kullanıma alma sağlayacak bir otomatik cihaz Yapılandırması kullanılarak gerçekleştirilir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

İstenen özelliklere sahip bir cihaz çiftlerini kümesini güncelleştirme ve cihaz çifti dayalı bir özeti raporlama tarafından otomatik cihaz yapılandırmalarını iş özellikleri bildirdi.  Yeni bir sınıf ve JSON belgesi olarak adlandırılan tanıtır bir _yapılandırma_ üç bölümden vardır:

* **Hedef koşulu** güncelleştirilmesi cihaz çiftlerini kapsamını tanımlar. Hedef durumu cihaz çifti etiketleri sorgu olarak belirtildiğinden ve/veya özellikler bildirdi.

* **Hedef içerik** eklenemez veya hedeflenen cihaz çiftlerini güncelleştirilmiş için istenen özelliklerini tanımlar. İçerik değiştirilecek istenen özellikler kısmında yolunu içerir.

* **Ölçümleri** gibi çeşitli yapılandırma durumlarını Özet sayısını tanımlamak **başarı**, **sürüyor**, ve **hata**. Cihaz sorgulamaları olarak belirtilen özel ölçümleri twin özellikleri bildirdi.  Sistem, hedeflenen cihaz çiftlerini sayısı ve başarıyla güncelleştirildi çiftlerini sayısı gibi twin güncelleştirme durumunu ölçmek varsayılan ölçümleri ölçümleridir. 

## <a name="cli-prerequisites"></a>CLI önkoşulları

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizde. 
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [IOT uzantısı için Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

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
## <a name="define-the-target-content-and-metrics"></a>İçerik hedef ve ölçümleri tanımlayın

Ölçüm ve hedef içerik sorguları aygıtı tanımlayan JSON belgeleri olarak ayarlandığında için istenen özellikleri ve ölçülecek bildirilen özellikleri çifti olarak belirtilir.  Azure CLI 2.0 kullanan bir otomatik cihaz yapılandırması oluşturmak için Ölçümler ve hedef içeriği yerel olarak .txt dosyaları olarak kaydedin. Cihazınız için yapılandırmayı uygulamak için komutu çalıştırdığınızda, daha sonra bir sonraki bölümde dosya yolları kullanır. 

Temel hedef içerik örnek aşağıda verilmiştir:

```json
{
  "content": {
    "deviceContent": {
      "properties.desired.chillerWaterSettings": {
        "temperature": 38,
        "pressure": 78
      }
    }
}
```

Ölçüm sorguları örnekleri şunlardır:

```json
{
  "queries": {
    "Compliant": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='current'",
    "Error": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='error'",
    "Pending": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='pending'"
  }
}
```

## <a name="create-a-configuration"></a>Bir yapılandırma oluşturmak

Hedef aygıtlara hedef içerik ve ölçümleri oluşan bir yapılandırma oluşturularak yapılandırın. 

Bir yapılandırma oluşturmak için aşağıdaki komutu kullanın:

   ```cli
   az iot hub configuration create --config-id [configuration id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int] --metrics [metric queries]
   ```

* **--config kimliği** -IOT hub'ı oluşturulacak yapılandırmasının adı. Yapılandırmanızı en fazla 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
* **--etiketleri** -yapılandırmanızı izlemenize yardımcı olması için etiketler ekleyin. Ad, dağıtımınızı tanımlamak değer çiftleri etiketleridir. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`
* **--İçerik** -twin ayarlanacak hedef içerik satır içi JSON ya da dosya yolunu istenen özellikleri. 
* **--hub adı** -yapılandırma oluşturulacağı IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`
* **--Hedef durumu** -hangi aygıtların bu yapılandırma ile hedeflenir belirlemek için bir hedef koşulu girin. Koşul cihaz çifti etiketlere göre veya cihaz çifti özellikleri istenen ve ifade biçimi eşleşmelidir. Örneğin, `tags.environment='test'` veya `properties.desired.devicemodel='4000x'`. 
* **--öncelik** -pozitif bir tamsayı. İki veya daha fazla yapılandırmaları aynı aygıtta hedeflenen gelmesi durumunda, en yüksek sayısal değer yapılandırmayla önceliği geçerli olur.
* **--Ölçümleri** -ölçüm sorgulara Filepath. Bir aygıt yapılandırma içeriği uygulama sonucunda geri bildirebilir çeşitli durumlarını Özet sayısı ölçümleri sağlar. Örneğin, bekleyen ayar değişiklikleri, hatalar için bir ölçüm ve başarılı ayar değişiklikleri yönelik ölçüm için bir ölçüm oluşturabilir. 

## <a name="monitor-a-configuration"></a>İzleyici bir yapılandırma

Bir yapılandırma içeriğini görüntülemek için aşağıdaki komutu kullanın:

   ```cli
az iot hub configuration show --config-id [configuration id] --hub-name [hub name]
   ```
* **--config kimliği** -IOT hub ' mevcut yapılandırmasının adı.
* **--hub adı** -yapılandırma bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`

Komut penceresinde yapılandırmasını inceleyin. **Ölçümleri** özelliği her hub tarafından değerlendirilen her ölçüm için bir sayı listeler:
* **targetedCount** -hedefleme koşuluyla eşleşen IOT hub'da cihaz çiftlerini sayısını belirten bir sistem ölçümü.
* **appliedCount** -sistem ölçüm uygulanan hedef içerik beklendiğinden cihaz sayısını belirtir.
* **Özel ölçüm** -tanımladığınız ölçümleri kullanıcı ölçümleri olarak kabul edilir.

Size aygıt kimlikleri listesini veya nesneler her ölçümler için aşağıdaki komutu kullanarak gösterebilir:

   ```cli
az iot hub configuration show-metric --config-id [configuration id] --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
   ```

* **--config kimliği** -IOT hub ' mevcut dağıtım adı.
* **--Ölçüm kimliği** - örneğin aygıt kimlikleri, listesini görmek istediğiniz ölçüm adı `appliedCount`
* **--hub adı** -dağıtım bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`
* **--Ölçüm Türü** -ölçüm türü olabilir `system` veya `user`.  Sistem ölçümleridir `targetedCount` ve `appliedCount`. Tüm diğer kullanıcı ölçümleri ölçümleridir.

## <a name="modify-a-configuration"></a>Bir yapılandırmasını Değiştir

Bir yapılandırma değiştirdiğinizde, değişikliklerin hemen tüm hedeflenen cihazlar için çoğaltılır. 

Hedef durumu güncelleştirirseniz, aşağıdaki güncelleştirmeleri oluşur:
* Cihaz çifti eski hedef durumu karşılamadı, ancak yeni hedef koşulunu ve bu yapılandırma, cihaz çifti için en yüksek öncelikli ise, bu yapılandırma için cihaz çifti uygulanır. 
* Cihaz çifti artık hedef koşulu karşılıyorsa, yapılandırma ayarları kaldırılır ve sonraki en yüksek öncelikli bir yapılandırma cihaz çiftine değiştirilecek. 
* Artık bu yapılandırma şu anda çalışan bir cihaz çifti hedef koşulunu ve diğer tüm yapılandırmaları hedef koşulu karşılamıyor, yapılandırma ayarlarından kaldırılacak ve başka bir değişiklik üzerinde twin yapılmayacak. 

Bir yapılandırma güncelleştirmek için aşağıdaki komutu kullanın:

   ```cli
az iot hub configuration update --config-id [configuration id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **--config kimliği** -IOT hub ' mevcut yapılandırmasının adı.
* **--hub adı** -yapılandırma bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`
* **--ayarlamak** -yapılandırmasında bir özelliğini güncelleştirin. Aşağıdaki özellikler güncelleştirebilirsiniz:
    * targetCondition - Örneğin `targetCondition=tags.location.state='Oregon'`
    * etiketleri 
    * öncelik

## <a name="delete-a-configuration"></a>Bir yapılandırma Sil

Bir yapılandırma sildiğinizde, tüm cihaz çiftlerini kendi sonraki en yüksek öncelikli bir yapılandırma üzerinde alın. Cihaz çiftlerini hedef durumu ve herhangi bir yapılandırma karşılamıyorsa, diğer bir ayarları uygulanır. 

Bir yapılandırma silmek için aşağıdaki komutu kullanın:

   ```cli
az iot hub configuration delete --config-id [configuration id] --hub-name [hub name] 
   ```
* **--config kimliği** -IOT hub ' mevcut yapılandırmasının adı.
* **--hub adı** -yapılandırma bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`

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
