---
title: "Modüller için Azure CLI 2.0 IOT uzantısını kullanarak IOT kenar cihazlara dağıtma | Microsoft Docs"
description: "Azure CLI 2.0 için IOT uzantısını kullanarak bir IOT sınır cihazı modülleri dağıtma"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 02/12/2018
ms.topic: article
ms.service: iot-edge
ms.custom: mvc
ms.reviewer: kgremban
ms.openlocfilehash: ce3e979428233af578d71dee5ed10103e105f4f4
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Azure CLI 2.0 için IOT uzantısını kullanarak bir IOT sınır cihazı modülleri dağıtma

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) çapraz platform komut satırı aracı IOT kenar gibi Azure kaynakları yönetmek için bir açık kaynak değil. Azure CLI 2.0, Windows, Linux ve MacOS kullanılabilir.

Azure CLI 2.0 Azure IOT Hub kaynakları, cihaz sağlama hizmet örneği ve kutunun dışında bağlantılı-hub yönetmenizi sağlar. Yeni IOT uzantısı aygıt yönetimi ve tam IOT kenar özelliği gibi özellikler ile Azure CLI 2.0 aşağıdakilere zenginleştirir.

Bu öğreticide, ilk Azure CLI 2.0 ve IOT uzantısı ayarlamak için adımları tamamlayın. Ardından, modüller CLI komutları kullanarak bir IOT sınır cihazı dağıtmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı. Henüz yoksa, şunları yapabilirsiniz [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/?v=17.39a) bugün. 

* [Python 2.7 x veya Python 3.x](https://www.python.org/downloads/).

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) ortamınızdaki. En azından, Azure CLI 2.0 sürümünüzün 2.0.24 olması gerekir veya üstü. Kullanım `az –-version` doğrulamak için. Bu sürüm az uzantısını komutları destekler ve Knack komut çerçevesi sunar. Windows yüklemek için bir basit yoludur indirmek ve yüklemek için [MSI](https://aka.ms/InstallAzureCliWindows).

* [Azure CLI 2.0 IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension):
   1. `az extension add --name azure-cli-iot-ext` öğesini çalıştırın. 
   2. Yükleme sonrasında kullanma `az extension list` yüklü uzantıları doğrulamak için veya `az extension show --name azure-cli-iot-ext` IOT uzantısı ayrıntılarını görmek için.
   3. Uzantıyı kaldırmak için kullanın `az extension remove --name azure-cli-iot-ext`.


## <a name="create-an-iot-edge-device"></a>Bir IOT sınır cihazı oluşturma
Bu makalede bir IOT kenar dağıtımı oluşturmak için yönergeler sağlar. Örnek Azure hesabınızda oturum açın, Azure kaynak grubu (Azure çözümünü ilgili kaynaklara tutan bir kapsayıcı) oluşturmak, IOT Hub oluşturma, üç IOT kenar cihaz kimliği oluşturma, kümesi etiketleri ve, bir IOT kenar dağıtımı oluşturmak nasıl gösterir Bu cihazlar hedefler. 

Azure hesabınızda oturum açın. Aşağıdaki oturum açma komutu girdikten sonra bir kerelik kodu kullanarak oturum için bir web tarayıcısı kullanın istenir: 

   ```cli
   az login
   ```

Adlı yeni bir kaynak grubu oluştur **IoTHubCLI** Doğu ABD bölgesinde: 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Kaynak grubu oluşturma][2]

Adlı IOT hub oluşturma **CLIDemoHub** yeni oluşturulmuş bir kaynak grubunda:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Her abonelik bir ücretsiz IOT hub ayrılan. CLI komutuyla boş bir hub oluşturmak için SKU değeriyle `--sku F1`. Boş bir hub'ı aboneliğinizde zaten varsa ikinci oluşturmaya çalışırken bir hata iletisi alırsınız. 

Bir IOT sınır cihazı oluşturun:

   ```cli
   az iot hub device-identity create --device-id edge001 -hub-name CLIDemoHub --edge-enabled
   ```

   ![IOT sınır cihazı oluşturma][4]

## <a name="configure-the-iot-edge-device"></a>IOT sınır cihazı yapılandırma

Bir dağıtım JSON şablonu oluşturun ve yerel olarak bir txt dosyası olarak kaydedin. Uygula yapılandırma komutunu çalıştırdığınızda, dosya yolunu gerekir.

Dağıtım JSON şablonları her zaman iki sistem modüller, edgeAgent ve edgeHub içermesi gerekir. Bu iki ek olarak IOT kenar ek modüller dağıtmak için bu dosyayı kullanabilirsiniz. IOT sınır cihazı bir tempSensor modülü ile yapılandırmak için aşağıdaki örneği kullanın:

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
               "loggingOptions": ""
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "edgepreview.azurecr.io/azureiotedge/edge-agent:1.0-preview",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "edgepreview.azurecr.io/azureiotedge/edge-hub:1.0-preview",
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
                 "image": "edgepreview.azurecr.io/azureiotedge/simulated-temperature-sensor:1.0-preview",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {},
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

Yapılandırma IOT kenar Cihazınızı Uygula:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

Modülleri IOT kenar aygıtınızda görüntüleyin:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Liste modülleri][6]

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [IOT sınır cihazı bir ağ geçidi olarak kullanma](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png

