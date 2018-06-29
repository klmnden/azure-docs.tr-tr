---
title: Dağıtma ve izleme modülleri Azure IOT kenar (CLI) | Microsoft Docs
description: Edge cihazlarda çalışan modülleri yönetme
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 705f7bfa62154bff62b2357bd8f33c01e97404d1
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036769"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Dağıtma ve IOT kenar modülleri Azure CLI kullanarak ölçekte izleme

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Azure IOT kenar kenarına analytics taşımanızı sağlar ve böylece yönetmek ve her biri fiziksel olarak erişmek zorunda kalmadan, IOT sınır cihazları izlemek bulut arabirimi sağlar. Cihazları uzaktan yönetmek için nesnelerin interneti çözümleri daha büyük ve daha karmaşık büyüyor giderek önemli bir özelliktir. Azure IOT kenar eklediğiniz kaç cihaz olsun, iş hedeflerinize destekleyecek şekilde tasarlanmıştır.

Tek bir cihazı yönetebilir ve modülleri dağıtmadan birer birer. Ancak, büyük ölçekli aygıtları değişiklik yapmak istiyorsanız, oluşturabileceğiniz bir **IOT kenar otomatik dağıtım**, IOT Hub'ındaki otomatik aygıt yönetiminin bir parçası olduğu. Dağıtımlar, birden fazla modülü aynı anda birden çok cihazlara dağıtma, durum ve modülleri sağlığını izlemek ve gerektiğinde değişiklikleri yapmanıza olanak sağlayan dinamik işlemlerdir. 

Bu makalede, Azure CLI 2.0 ve IoT uzantısını ayarlarsınız. Daha sonra modülleri IOT sınır cihazları kümesine dağıtmak ve CLI komutları kullanarak ilerleme durumunu izlemek nasıl öğrenin.

## <a name="cli-prerequisites"></a>CLI önkoşulları

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizde. 
* [IOT sınır cihazları](how-to-register-device-cli.md) yüklü IOT kenar çalışma zamanı.
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [IOT uzantısı için Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimini yapılandırın

Bir dağıtım bildirimi modüllerine dağıtmak için modüller ve modül çiftlerini istenen özelliklerini arasında veri akışını açıklayan bir JSON belgedir. Dağıtım iş nasıl bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [nasıl IOT kenar modülleri, yapılandırılmış, yeniden ve kullanılabilecek anlamak](module-composition.md).

Azure CLI 2.0 kullanan modülleri dağıtmak için dağıtım bildirimi yerel olarak .txt dosyası olarak kaydedin. Cihazınız için yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolunu kullanır. 

Örnek olarak bir modülü ile temel dağıtım bildirimi şöyledir:

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
                     "image": "microsoft/azureiotedge-agent:1.0-preview",
                     "createOptions": "{}"
                   }
                 },
                 "edgeHub": {
                   "type": "docker",
                   "status": "running",
                   "restartPolicy": "always",
                   "settings": {
                     "image": "microsoft/azureiotedge-hub:1.0-preview",
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


## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirlemek

Bir dağıtımı oluşturmadan önce hangi cihazların etkilenmesini istediğiniz belirtmek kullanabilmek için gerekir. Azure IOT kenar tanımlayan kullanarak cihazları **etiketleri** cihaz çiftine. Her cihazı birden fazla etikete sahip olabilir ve çözümünüz için anlamlı herhangi bir şekilde tanımlayabilirsiniz. Örneğin, akıllı bina kampüs yönetiyorsanız, bir cihaza aşağıdaki etiketlerin ekleyebilirsiniz:

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

Cihaz çiftlerini ve etiketleri hakkında daha fazla bilgi için bkz: [IOT hub'ı Anlama ve Kullan cihaz çiftlerini][lnk-device-twin].

## <a name="create-a-deployment"></a>Bir dağıtımı oluşturma

Modüller, diğer parametreler yanı sıra dağıtım bildirimi oluşan bir dağıtım oluşturarak, hedef cihazlara dağıtın. 

Bir dağıtım oluşturmak için aşağıdaki komutu kullanın:

   ```cli
   az iot edge deployment create --deployment-id [deployment id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int]
   ```

* **--Dağıtım kimliği** -IOT hub'ı oluşturulacak dağıtım adı. Dağıtımınızı en fazla 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
* **--etiketleri** -dağıtımlarınızı izlemenize yardımcı olması için etiketler ekleyin. Ad, dağıtımınızı tanımlamak değer çiftleri etiketleridir. Örneğin, `HostPlatform, Linux` veya `Version, 3.0.1`
* **--İçerik** -dağıtım için Filepath bildirim JSON. 
* **--hub adı** -dağıtım oluşturulacağı IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`
* **--Hedef durumu** -hangi aygıtların bu dağıtım ile hedeflenir belirlemek için bir hedef koşulu girin. Koşul cihaz çifti etiketlere göre veya cihaz çifti özellikleri istenen ve ifade biçimi eşleşmelidir. Örneğin, `tags.environment='test'` veya `properties.desired.devicemodel='4000x'`. 
* **--öncelik** -pozitif bir tamsayı. İki veya daha fazla dağıtım aynı aygıtta hedeflenen gelmesi durumunda, en yüksek sayısal değer ile dağıtım önceliği geçerli olur.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Bir dağıtım içeriğini görüntülemek için aşağıdaki komutu kullanın:

   ```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
   ```
* **--Dağıtım kimliği** -IOT hub ' mevcut dağıtım adı.
* **--hub adı** -dağıtım bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`

Komut penceresinde dağıtım inceleyin. **Ölçümleri** özelliği her hub tarafından değerlendirilen her ölçüm için bir sayı listeler:
* **targetedCount** -hedefleme koşuluyla eşleşen IOT hub'da cihaz çiftlerini sayısını belirten bir sistem ölçümü.
* **appliedCount** -sistem ölçüm kendi modülü çiftlerini IOT Hub'ındaki uygulanan dağıtım içerik beklendiğinden cihaz sayısını belirtir.
* **reportedSuccessfulCount** -IOT kenar istemci çalışma zamanı başarı raporlama dağıtım sınır cihazları sayısını belirtir bir aygıt ölçümü.
* **reportedFailedCount** -IOT kenar istemci çalışma zamanı hatasından raporlama dağıtım sınır cihazları sayısını belirtir bir aygıt ölçümü.

Size aygıt kimlikleri listesini veya nesneler her ölçümler için aşağıdaki komutu kullanarak gösterebilir:

   ```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
   ```

* **--Dağıtım kimliği** -IOT hub ' mevcut dağıtım adı.
* **--Ölçüm kimliği** - örneğin aygıt kimlikleri, listesini görmek istediğiniz ölçüm adı `reportedFailedCount`
* **--hub adı** -dağıtım bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değişiklik yaptığınızda, değişiklikler hemen tüm hedeflenen cihazlar için çoğaltılır. 

Hedef durumu güncelleştirirseniz, aşağıdaki güncelleştirmeleri oluşur:
* Bir aygıt, eski hedef durumu karşılamadı, ancak yeni hedef koşulunu ve en yüksek öncelikli cihaz için bu dağıtım ise, bu dağıtım cihaza uygulanır. 
* Şu anda bu dağıtım artık çalıştıran bir cihazda hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelik dağıtımı alır. 
* Şu anda bu dağıtım artık çalıştıran bir cihazda hedef koşulunu ve diğer dağıtımlar hedef koşulu karşılamıyor, hiçbir değişiklik cihazda oluşur. Aygıt geçerli modülleri kendi geçerli durumunda çalışmaya devam eder, ancak artık bu dağıtımın bir parçası olarak yönetilmez. Diğer dağıtım hedef koşulunu sonra bu dağıtım kaldırır ve yeni alır. 

Bir dağıtımı güncelleştirmek için aşağıdaki komutu kullanın:

   ```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **--Dağıtım kimliği** -IOT hub ' mevcut dağıtım adı.
* **--hub adı** -dağıtım bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`
* **--ayarlamak** -dağıtım özelliğini güncelleştirin. Aşağıdaki özellikler güncelleştirebilirsiniz:
    * targetCondition - Örneğin `targetCondition=tags.location.state='Oregon'`
    * etiketleri 
    * öncelik


## <a name="delete-a-deployment"></a>Bir dağıtımı silin

Bir dağıtım sildiğinizde, tüm cihazlar üzerinde kendi sonraki en yüksek öncelikli dağıtım alın. Ardından aygıtlarınızı başka bir dağıtım hedef koşulu karşılamıyorsa, dağıtım silindiğinde modülleri kaldırılmaz. 

Bir dağıtımı silmek için aşağıdaki komutu kullanın:

   ```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
   ```
* **--Dağıtım kimliği** -IOT hub ' mevcut dağıtım adı.
* **--hub adı** -dağıtım bulunduğu IOT hub'ın adı. Hub geçerli abonelikte olması gerekir. İstenen abonelik komutu ile geçiş `az account set -s [subscription name]`

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek [modülleri kenar cihazlara dağıtma][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
