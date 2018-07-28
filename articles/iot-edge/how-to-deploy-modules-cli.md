---
title: Azure IOT Edge modüllerini (CLI) dağıtma | Microsoft Docs
description: Modüller IOT Edge cihazına dağıtmak için Azure CLI 2.0 için IOT uzantısını kullanma
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 29c11139a2c773db2d26bf44984ad4dc72f2d870
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324614"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli-20"></a>Azure CLI 2.0 ile Azure IOT Edge modüllerini dağıtmak

IOT Edge modülleri, iş mantığı ile oluşturduktan sonra bunları ucuna çalışılacak cihazlarınıza dağıtmak istiyorsanız. Toplamak ve veri işlemek için birlikte çalışan birden çok modül varsa, bunları tamamını aynı anda dağıtabilir ve bunları bağlayan yönlendirme kurallarını bildirin. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure IOT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub'ları hazır yönetmenizi sağlar. IoT uzantısı, Azure CLI 2.0’ı cihaz yönetimi ve tam IoT Edge kapasitesi gibi özelliklerle zenginleştirir.

Bu makalede, bir JSON dağıtım bildirimi oluşturun, sonra IOT Edge cihazına dağıtım göndermek için bu dosyayı kullanma gösterilmektedir. Birden fazla cihazda kendi paylaşılan etiketlere göre hedefleyen bir dağıtım oluşturma hakkında daha fazla bilgi için bkz. [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor-cli.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizdeki. 
* Bir [IOT Edge cihazı](how-to-register-device-cli.md) yüklü olan bir IOT Edge çalışma zamanı ile.
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [Azure CLI 2.0 için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).

Azure CLI 2.0 kullanarak modüllerini dağıtmak için dağıtım bildirimi yerel olarak bir .json dosyası kaydedin. Cihazınıza yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolu kullanır. 

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

Dağıtım bildiriminin kaydedildiği klasöre dizinleri değiştirin. VS Code IOT Edge şablonlardan birini kullandıysanız, `deployment.json` dosyası **config** çözüm dizininizin klasör. Kullanmayın `deployment.template.json` dosya. 

IOT Edge cihazına yapılandırmayı uygulamak için aşağıdaki komutu kullanın:

   ```cli
   az iot hub apply-configuration --device-id [device id] --hub-name [hub name] --content [file path]
   ```

Cihaz kimliği parametresi büyük/küçük harf duyarlıdır. İçerik parametresi dağıtım noktalarına bildirim kaydettiğiniz dosyası. 

   ![Modülleri ayarlama](./media/how-to-deploy-cli/set-modules.png)

## <a name="view-modules-on-your-device"></a>Cihazınızda modülleri görüntüleme

Cihazınıza modülleri dağıttıktan sonra tüm bunları aşağıdaki komutla görebilirsiniz: 

IoT Edge cihazınızda modülleri görüntüleme:
    
   ```cli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

Cihaz kimliği parametresi büyük/küçük harf duyarlıdır.

   ![Modülleri listeleme](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md)