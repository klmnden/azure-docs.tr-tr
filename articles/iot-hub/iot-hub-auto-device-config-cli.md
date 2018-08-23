---
title: Yapılandırma ve IOT cihazlarını uygun ölçekte Azure IOT hub'ı (CLI) ile izleme | Microsoft Docs
description: Bir yapılandırma birden fazla cihaza atamak için Azure IOT Hub otomatik cihaz yapılandırmaları kullanın
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: f81ef3c231874f314d6fe023ba247a0bcff61e90
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "42059210"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-cli"></a>Yapılandırma ve IOT cihazlarını uygun ölçekte Azure CLI kullanarak izleme

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Azure IOT hub otomatik cihaz Yönetimi döngülerini tamamına büyük cihaz filolarına yönetme yinelenen ve karmaşık görevlerinin birçoğunu otomatik hale getirir. İle otomatik cihaz yönetimi, bir dizi cihazda özelliklerine göre hedef, istenen yapılandırmasını tanımlamak ve IOT Hub'ın kapsama geldikleri her cihazları güncelleştirmek olanak tanır.  Bu işlem, ayrıca tamamlama ve uyumluluk, tanıtıcı birleştirme ve çakışmaları özetlemenize ve aşamalı bir yaklaşımla yapılandırmaları Dışarı Aktar sağlayacak bir otomatik cihaz Yapılandırması kullanılarak gerçekleştirilir.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Otomatik cihaz yapılandırmaları iş istediğiniz özelliklere sahip bir dizi cihaz ikizlerini güncelleştiriliyor ve özetini üzerinde cihaz ikizini raporlama tarafından bildirilen özellikler.  Yeni bir sınıf ve adlı bir JSON belge tanıtır bir *yapılandırma* üç bölümü vardır:

* **Hedef koşulu** güncelleştirilmesi için cihaz ikizlerini kapsamını tanımlar. Hedef koşul cihaz ikizi etiketler üzerinde bir sorgu olarak belirtildiğinden ve/veya bildirilen özellikler.

* **Hedef içerik** veya hedeflenen cihaz ikizlerini güncelleştirilemiyor için istenen özellikleri tanımlar. İçerik değiştirilmesi için istenen özellikler kısmında bir yolu içerir.

* **Ölçümleri** gibi çeşitli yapılandırma durumlarının Özet sayıları tanımlamak **başarı**, **sürüyor**, ve **hata**. Özel ölçümler olarak cihaz sorguları belirtilen çiftinin bildirilen özelliklerini.  Sistem ölçümlerini ikizi hedeflenen cihaz ikizlerini sayısı ve başarıyla güncelleştirildi ikizlerini sayısı gibi güncelleştirme durumunu ölçen varsayılan ölçümleridir. 

## <a name="cli-prerequisites"></a>CLI önkoşulları

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizdeki. 
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [Azure CLI 2.0 için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="implement-device-twins-to-configure-devices"></a>Cihazları yapılandırmak için cihaz ikizlerini uygulayın

Otomatik cihaz yapılandırmaları, cihazlar ve bulut arasında durumunu eşitlemek için cihaz ikizlerini kullanılmasını gerektirir.  Başvurmak [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](iot-hub-devguide-device-twins.md) için cihaz ikizlerini kullanma yönergeleri.

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

## <a name="define-the-target-content-and-metrics"></a>Hedef içerik ve ölçümlerinizi tanımlayın

Hedef içerik ve ölçüm sorguları, aygıtı tanımlayan JSON belgeleri olarak ayarlandığında için istenen özellikleri ve ölçülecek bildirilen özellikleri ikizi olarak belirtilir.  Azure CLI 2.0 kullanarak bir otomatik cihaz yapılandırmasını oluşturmak için Ölçümler ve hedef içerik yerel olarak .txt dosyaları olarak kaydedin. Cihazınıza yapılandırmayı uygulamak için komutu çalıştırdığınızda, daha sonra bir sonraki bölümde dosya yolları kullanın. 

Temel hedef içerik örneği aşağıdadır:

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

Ölçüm sorguları örnekleri aşağıda verilmiştir:

```json
{
  "queries": {
    "Compliant": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='current'",
    "Error": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='error'",
    "Pending": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='pending'"
  }
}
```

## <a name="create-a-configuration"></a>Bir yapılandırma oluşturun

Hedef cihazlar, hedef içerik ve ölçümleri oluşan bir yapılandırma oluşturarak yapılandırın. 

Bir yapılandırma oluşturmak için aşağıdaki komutu kullanın:

```cli
   az iot hub configuration create --config-id [configuration id] \
     --labels [labels] --content [file path] --hub-name [hub name] \
     --target-condition [target query] --priority [int] \
     --metrics [metric queries]
```

* --**Yapılandırma Kimliği** -IOT hub'ında oluşturulacak yapılandırmasının adı. Yapılandırmanızı 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.

* --**etiketleri** -yapılandırmanızı izlenmesine yardımcı olması için etiketler ekleyin. Etiket adı, dağıtımınızı tanımlayan değer çiftleri olan. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`

* --**İçerik** -istenen özellikler ikizi ayarlanması için hedef içeriği satır içi JSON veya dosya yolu. 

* --**hub adı** -yapılandırma oluşturulacağı IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`

* --**Hedef koşul** -bu yapılandırma ile hangi cihazların hedeflenecek belirlemek için bir hedef koşulu girin. Koşul, cihaz ikizi etiketlere göre veya cihaz ikizi istenen özellikleri ve ifade biçim ile eşleşmesi. Örneğin, `tags.environment='test'` veya `properties.desired.devicemodel='4000x'`. 

* --**öncelik** -pozitif bir tamsayı. İki veya daha fazla yapılandırması aynı cihazda hedeflenen durumunda durumunda, sayısal değeri en yüksek öncelikli yapılandırmasını uygular.

* --**Ölçümleri** -ölçüm sorguları için dosya yolu. Bir cihaz, uygulama yapılandırma içeriği sonucunda geri bildirebilir çeşitli durumları özeti sayıları ölçümleri sağlar. Örneğin, ayarları değişiklikleri, hatalar için bir ölçüm ve bir ölçüm başarılı ayarları değişiklikleri için bekleyen bir ölçüm için oluşturabilir. 

## <a name="monitor-a-configuration"></a>İzleyici yapılandırma

Bir yapılandırma içeriğini görüntülemek için aşağıdaki komutu kullanın:

```cli
az iot hub configuration show --config-id [configuration id] \
  --hub-name [hub name]
```

* --**Yapılandırma Kimliği** -IOT hub'ında bulunan yapılandırmasının adı.

* --**hub adı** -yapılandırma bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`

Komut penceresinde yapılandırmasını inceleyin. **Ölçümleri** özelliği her hub tarafından değerlendirilen her bir ölçüm sayısını listeler:

* **targetedCount** -hedefleme koşulu IOT hub'da cihaz ikizlerini sayısını belirten bir sistem ölçümü.

* **appliedCount** -bir sistem ölçüm hedef içerik uygulanmış olan cihaz sayısını belirtir.

* **Özel ölçümünüzün** -tanımladığınız tüm ölçümleri kullanıcı ölçümlerini kabul edilir.

Cihaz kimlikleri listesini veya nesnelerin her biri ölçümler için aşağıdaki komutu kullanarak gösterebilirsiniz:

```cli
az iot hub configuration show-metric --config-id [configuration id] \
   --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
```

* --**Yapılandırma Kimliği** -IOT hub'ında bulunan dağıtım adı.

* --**Ölçüm kimliği** - örneğin cihaz kimliklerinin listesini görmek istediğiniz ölçüm adı `appliedCount`.

* --**hub adı** -dağıtım bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`.

* --**Ölçüm Türü** -ölçüm türü olabilir `system` veya `user`.  Sistem ölçümleri `targetedCount` ve `appliedCount`. Diğer tüm ölçümler kullanıcı ölçümleridir.

## <a name="modify-a-configuration"></a>Bir yapılandırmasını Değiştir

Yapılandırma değiştirdiğinizde değişiklikler hemen hedeflenen tüm cihazlara çoğaltın. 

Hedef koşul güncelleştirme aşağıdaki güncelleştirmeleri oluşur:

* Cihaz ikizi eski hedef koşul karşılanmadıysa, ancak yeni hedef koşulunu ve bu yapılandırma, cihaz ikizi için en yüksek öncelikli ise, bu yapılandırma için cihaz ikizi'e uygulanır. 

* Cihaz ikizi artık hedef koşulu karşılıyorsa, yapılandırma ayarları kaldırılır ve cihaz ikizinde sonraki en yüksek öncelikli bir yapılandırma ile değiştirilecek. 

* Artık bu yapılandırma şu anda çalışan bir cihaz ikizi hedef koşulu karşılayan ve diğer tüm yapılandırmaları hedef koşulu karşılamıyor ise yapılandırma ayarlarından kaldırılacak ve ikizinin diğer herhangi bir değişiklik yapılmaz. 

Bir yapılandırmasını güncelleştirmek için aşağıdaki komutu kullanın:

```cli
az iot hub configuration update --config-id [configuration id] \
   --hub-name [hub name] --set [property1.property2='value']
```

* --**Yapılandırma Kimliği** -IOT hub'ında bulunan yapılandırmasının adı.

* --**hub adı** -yapılandırma bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`.

* --**ayarlama** -yapılandırmasında bir özelliğini güncelleştirin. Aşağıdaki özellikleri güncelleştirebilirsiniz:

    * Örneğin - targetCondition `targetCondition=tags.location.state='Oregon'`

    * etiketleri 

    * öncelik

## <a name="delete-a-configuration"></a>Bir yapılandırmayı silmek

Bir yapılandırmayı silmek, tüm cihaz ikizlerini kendi sonraki en yüksek öncelikli yapılandırmasına yararlanın. Cihaz ikizlerini herhangi bir yapılandırma hedef koşulu karşılamıyorsanız, sonra başka bir ayarları uygulanır. 

Bir yapılandırmayı silmek için aşağıdaki komutu kullanın:

```cli
az iot hub configuration delete --config-id [configuration id] \
   --hub-name [hub name] 
```
* --**Yapılandırma Kimliği** -IOT hub'ında bulunan yapılandırmasının adı.

* --**hub adı** -yapılandırma bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`.

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
