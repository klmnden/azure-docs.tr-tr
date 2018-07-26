---
title: Dağıtma ve izleme modülleri Azure IOT Edge (CLI) | Microsoft Docs
description: Uç cihazlarında çalışan modülleri yönetme
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/25/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b90c26eaa36c906dda904106b104c3dbf04a55ce
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257989"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Dağıtma ve izleme uygun ölçekte Azure CLI kullanarak IOT Edge modülleri

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IOT Edge bulut arabirimi sağlar, böylece yönetebilir ve her bir fiziksel olarak erişmek zorunda kalmadan IOT Edge cihazlarınıza izleme ve analiz uç cihazlara taşıyın olanak sağlar. Cihazları uzaktan yönetme olanağı, nesnelerin interneti çözümleri, daha büyük ve daha karmaşık büyüyor gibi giderek önemlidir. Azure IOT Edge, eklediğiniz kaç cihaz ne olursa olsun, iş hedeflerinizi desteklemek için tasarlanmıştır.

Tek tek cihazları yönetmek ve modülleri dağıtabilmek teker teker. Ancak, cihazların büyük ölçekli değişiklikler yapmak istiyorsanız, oluşturabileceğiniz bir **IOT Edge otomatik dağıtım**, IOT Hub otomatik cihaz yönetiminin bir parçası değildir. Birden çok modülle aynı anda birden çok cihazlara dağıtın, sistem modüllerinin izlemek ve gerektiğinde değişiklik olanak sağlayan dinamik işlemler dağıtımlarıdır. 

Bu makalede, Azure CLI 2.0 ve IoT uzantısını ayarlarsınız. Ardından bir dizi cihaza IOT Edge modüllerini dağıtmak ve CLI komutları kullanarak ilerleme durumunu izlemek hakkında bilgi edinin.

## <a name="cli-prerequisites"></a>CLI önkoşulları

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizdeki. 
* [IOT Edge cihazları](how-to-register-device-cli.md) yüklü olan bir IOT Edge çalışma zamanı ile.
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [Azure CLI 2.0 için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

Azure CLI 2.0 kullanarak modüllerini dağıtmak için dağıtım bildirimi yerel olarak bir .txt dosyası olarak kaydedin. Cihazınıza yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolu kullanır. 

Örnek olarak bir modülü ile temel bir dağıtım bildirimi şöyledir:

   ```json
   {
        "content": {
         "modulesContent": {
           "$edgeAgent": {
             "properties.desired": {
               "schemaVersion": "1.0",
               "runtime": {
                 "type": "docker",
                 "settings": {
                   "minDockerVersion": "v1.25",
                   "loggingOptions": "",
                   "registryCredentials": {
                     "registryName": {
                       "username": "",
                       "password": "",
                       "address": ""
                     }
                   }
               },
               "systemModules": {
                 "edgeAgent": {
                   "type": "docker",
                   "settings": {
                     "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                     "createOptions": "{}"
                   }
                 },
                 "edgeHub": {
                   "type": "docker",
                   "status": "running",
                   "restartPolicy": "always",
                   "settings": {
                     "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                     "createOptions": "{}"
                   }
                 }
               },
               "modules": {
                 "tempSensor": {
                   "version": "1.0",
                   "type": "docker",
                   "status": "running",
                   "restartPolicy": "always",
                   "settings": {
                     "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                     "createOptions": "{}"
                   }
                 }
               }
             }
           },
           "$edgeHub": {
             "properties.desired": {
               "schemaVersion": "1.0",
               "routes": {
                   "route": "FROM /* INTO $upstream"
               },
               "storeAndForwardConfiguration": {
                 "timeToLiveSecs": 7200
               }
             }
           },
           "tempSensor": {
             "properties.desired": {}
           }
         }
       }
   }
   ```


## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirleyin

Bir dağıtımı oluşturmadan önce değiştirmek istediğiniz hangi cihazların belirtebilmek sahip. Azure IOT Edge kullanarak cihazları tanımlar **etiketleri** cihaz ikizinde. Her cihazı birden fazla etikete sahip olabilir ve çözümünüz için mantıklı olan herhangi bir şekilde tanımlayabilirsiniz. Örneğin, bir akıllı binalar, kampüs yönetiyorsanız, bir cihaza aşağıdaki etiketler ekleyebilirsiniz:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Cihaz ikizleri ve etiketleri hakkında daha fazla bilgi için bkz: [IOT hub'daki cihaz ikizlerini kavrama ve kullanma][lnk-device-twin].

## <a name="create-a-deployment"></a>Bir dağıtım oluşturun

Dağıtım bildiriminin yanı sıra diğer parametreler içeren bir dağıtım oluşturarak modülleri, hedef cihazlara dağıtın. 

Bir dağıtımı oluşturmak için aşağıdaki komutu kullanın:

   ```cli
   az iot edge deployment create --deployment-id [deployment id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int]
   ```

* **--Dağıtım kimliği** -IOT hub'ında oluşturulacak dağıtım adı. Dağıtımınızı en çok 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
* **--Etiket** -dağıtımlarınızı izlenmesine yardımcı olması için etiketler ekleyin. Etiket adı, dağıtımınızı tanımlayan değer çiftleri olan. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`
* **--İçerik** -dağıtım yolu JSON bildirim. 
* **--hub adı** -dağıtım oluşturulacağı IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`
* **--Hedef koşulu** -hangi cihazların bu dağıtım ile hedeflenecek belirlemek için bir hedef koşulu girin. Koşul, cihaz ikizi etiketlere göre veya cihaz çiftinin bildirilen özellikler ve ifade biçim ile eşleşmesi. Örneğin, `tags.environment='test'` veya `properties.reported.devicemodel='4000x'`. 
* **--öncelik** -pozitif bir tamsayı. İki veya daha fazla dağıtım aynı cihazda hedeflenen durumunda durumunda, sayısal değeri en yüksek dağıtım önceliği geçerli olur.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Bir dağıtım içeriğini görüntülemek için aşağıdaki komutu kullanın:

   ```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
   ```
* **--Dağıtım kimliği** -IOT hub'ında bulunan dağıtım adı.
* **--hub adı** -dağıtım bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`

Komut penceresinde dağıtım inceleyin. **Ölçümleri** özelliği her hub tarafından değerlendirilen her bir ölçüm sayısını listeler:
* **targetedCount** -hedefleme koşulu IOT hub'da cihaz ikizlerini sayısını belirten bir sistem ölçümü.
* **appliedCount** -bir sistem ölçüm kendi modül ikizlerini IOT hub'ında uygulanan dağıtım içeriğine kalmışlardır cihaz sayısını belirtir.
* **reportedSuccessfulCount** -IOT Edge istemci çalışma zamanı başarı raporlama dağıtım Edge cihaz sayısını belirten bir cihaz ölçümü.
* **reportedFailedCount** -IOT Edge istemci çalışma zamanı hatasından raporlama dağıtımdaki uç cihazlarına sayısını belirten bir cihaz ölçümü.

Cihaz kimlikleri listesini veya nesnelerin her biri ölçümler için aşağıdaki komutu kullanarak gösterebilirsiniz:

   ```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
   ```

* **--Dağıtım kimliği** -IOT hub'ında bulunan dağıtım adı.
* **--Ölçüm kimliği** - örneğin cihaz kimliklerinin listesini görmek istediğiniz ölçüm adı `reportedFailedCount`
* **--hub adı** -dağıtım bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değiştirdiğinizde değişiklikler hemen hedeflenen tüm cihazlara çoğaltın. 

Hedef koşul güncelleştirme aşağıdaki güncelleştirmeleri oluşur:
* Ardından, bir cihaz, eski hedef koşul karşılanmadıysa, ancak yeni hedef koşulunu ve bu dağıtım bu cihaz için en yüksek öncelikli ise, bu dağıtım cihaza uygulanır. 
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelikli dağıtımı alır. 
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılayan ve diğer tüm dağıtımları, hedef koşulu yerine getirmeyen, hiçbir değişiklik cihazda gerçekleşir. Cihaz, geçerli alt modüller kendi geçerli durumunda çalışmaya devam eder ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtım hedef koşulu karşılayan sonra bu dağıtım kaldırır ve yeni alır. 

Bir dağıtımı güncelleştirmek için aşağıdaki komutu kullanın:

   ```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **--Dağıtım kimliği** -IOT hub'ında bulunan dağıtım adı.
* **--hub adı** -dağıtım bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`
* **--ayarlamak** -dağıtımdaki bir özelliğini güncelleştirin. Aşağıdaki özellikleri güncelleştirebilirsiniz:
    * Örneğin - targetCondition `targetCondition=tags.location.state='Oregon'`
    * etiketleri 
    * öncelik


## <a name="delete-a-deployment"></a>Dağıtımı Sil

Bir dağıtım sildiğinizde, sonraki en yüksek öncelikli dağıtım üzerinde herhangi bir cihaza yararlanın. Daha sonra başka bir dağıtım hedef koşulu cihazlarınızı karşılamıyorsa, dağıtım silindiğinde modülleri kaldırılmaz. 

Bir dağıtımı silmek için aşağıdaki komutu kullanın:

   ```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
   ```
* **--Dağıtım kimliği** -IOT hub'ında bulunan dağıtım adı.
* **--hub adı** -dağıtım bulunduğu IOT hub'ının adı. Hub'ın geçerli abonelikte olmalıdır. Komutu istediğiniz aboneliğe geçin `az account set -s [subscription name]`

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [modülleri uç cihazlarına dağıtma][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
