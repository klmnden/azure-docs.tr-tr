---
title: Azure IOT kenar modülleri (CLI) dağıtma | Microsoft Docs
description: Azure CLI 2.0 IOT uzantısı modülleri IOT kenar cihazına dağıtmak için kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/08/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 02a7533cf1c06142f564ed9fd6008a25be9fc603
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036362"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli-20"></a>Azure CLI 2.0 ile Azure IOT kenar modülleri dağıtma

IOT kenar modülleri, iş mantığı ile oluşturduktan sonra bunları sınırında çalışmaya aygıtlarınıza dağıtmak istiyorsanız. Toplamak ve verileri işlemek için birlikte çalışan birden fazla modülü varsa, bunları aynı anda dağıtması ve bunları bağlayan yönlendirme kurallarını bildirin. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure IOT Hub kaynakları, cihaz sağlama hizmet örneği ve kutunun dışında bağlantılı-hub yönetmenizi sağlar. IoT uzantısı, Azure CLI 2.0’ı cihaz yönetimi ve tam IoT Edge kapasitesi gibi özelliklerle zenginleştirir.

Bu makalede, bir JSON dağıtım bildirimi oluşturun, sonra dağıtım IOT kenar cihazına göndermek için bu dosyayı kullanmanız gösterilmektedir. Kullanıcıların paylaşılan etiketlere göre birden çok aygıt hedefleyen bir dağıtımı oluşturma hakkında daha fazla bilgi için bkz [dağıtma ve izleme ölçekte IOT kenar modülleri](how-to-deploy-monitor-cli.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizde. 
* Bir [IOT sınır cihazı](how-to-register-device-cli.md) yüklü IOT kenar çalışma zamanı.
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [IOT uzantısı için Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimini yapılandırın

Bir dağıtım bildirimi modüllerine dağıtmak için modüller ve modül çiftlerini istenen özelliklerini arasında veri akışını açıklayan bir JSON belgedir. Dağıtım iş nasıl bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [nasıl IOT kenar modülleri, yapılandırılmış, yeniden ve kullanılabilecek anlamak](module-composition.md).

Azure CLI 2.0 kullanan modülleri dağıtmak için dağıtım bildirimi yerel olarak .txt dosyası olarak kaydedin. Cihazınız için yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolunu kullanır. 

Örnek olarak bir modülü ile temel dağıtım bildirimi şöyledir:

   ```json
   {
     "moduleContent": {
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
                 "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
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

## <a name="deploy-to-your-device"></a>Cihazınızı için dağıtma

Modülleri modül bilgileriyle yapılandırılmış dağıtım bildirimi uygulayarak aygıtınıza dağıtın. 

Bir IOT sınır cihazı yapılandırmayı uygulamak için aşağıdaki komutu kullanın:

   ```cli
   az iot hub apply-configuration --device-id [device id] --hub-name [hub name] --content [file path]
   ```

Aygıt kimliği parametresi büyük/küçük harf duyarlıdır. Dağıtım için içerik parametresi noktaları kaydettiğiniz dosya bildirimi. 

## <a name="view-modules-on-your-device"></a>Aygıtınızdaki modül görüntüle

Aygıtınıza modülleri dağıttıktan sonra sonra aşağıdaki komut ile bunların tümünün görüntüleyebilirsiniz: 

IoT Edge cihazınızda modülleri görüntüleme:
    
   ```cli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

Aygıt kimliği parametresi büyük/küçük harf duyarlıdır.

   ![Modülleri listeleme](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme ölçekte IOT kenar modülleri](how-to-deploy-monitor.md)