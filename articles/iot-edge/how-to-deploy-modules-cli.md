---
title: Komut satırından - Azure IOT Edge modüllerini dağıtmak | Microsoft Docs
description: Modüller IOT Edge cihazına dağıtmak için Azure CLI için IOT uzantısı kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/09/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 766b51f208e7e8f4a49109e32864f2726b8ccd63
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62126401"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli"></a>Azure CLI ile Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin.

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) çapraz platform komut satırı aracı IOT Edge gibi Azure kaynaklarını yönetmek için bir açık kaynak. Azure IOT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub'ları hazır yönetmenizi sağlar. Yeni IOT uzantısı, Azure CLI cihaz yönetimi ve tam IOT Edge özelliği gibi özelliklerle zenginleştirir.

Bu makalede, bir JSON dağıtım bildirimi oluşturun, sonra IOT Edge cihazına dağıtım göndermek için bu dosyayı kullanma gösterilmektedir. Birden fazla cihazda kendi paylaşılan etiketlere göre hedefleyen bir dağıtım oluşturma hakkında daha fazla bilgi için bkz. [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor-cli.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizdeki.
* Bir [IOT Edge cihazı](how-to-register-device-cli.md) yüklü olan bir IOT Edge çalışma zamanı ile.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ortamınızdaki. En az 2.0.24 Azure CLI sürümünüzü olmalıdır veya üzeri. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

Azure CLI kullanarak modüllerini dağıtmak için dağıtım bildirimi yerel olarak bir .json dosyası kaydedin. Cihazınıza yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolu kullanır.

Örnek olarak bir modülü ile temel bir dağıtım bildirimi şöyledir:

   ```json
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
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
   ```

## <a name="deploy-to-your-device"></a>Cihazınıza dağıtma

Modül bilgileri yapılandırdığınız dağıtım bildirimini uygulayarak modülleri cihazınıza dağıtın.

Dağıtım bildiriminin kaydedildiği klasöre dizinleri değiştirin. VS Code IOT Edge şablonlardan birini kullandıysanız, `deployment.json` dosyası **config** çözüm dizininizin klasörünü ve `deployment.template.json` dosya.

IOT Edge cihazına yapılandırmayı uygulamak için aşağıdaki komutu kullanın:

   ```cli
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content [file path]
   ```

Cihaz kimliği parametresi büyük/küçük harf duyarlıdır. İçerik parametresi dağıtım noktalarına bildirim kaydettiğiniz dosyası.

   ![az IOT edge modülleri kümesini çıktı](./media/how-to-deploy-cli/set-modules.png)

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Cihazınıza modülleri dağıttıktan sonra tüm bunları aşağıdaki komutla görebilirsiniz:

IoT Edge cihazınızda modülleri görüntüleme:

   ```cli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

Cihaz kimliği parametresi büyük/küçük harf duyarlıdır.

   ![az IOT hub kimlik modülü liste çıkışı](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)
