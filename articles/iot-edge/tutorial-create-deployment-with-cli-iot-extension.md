---
title: Azure CLI 2.0 için IoT uzantısını kullanarak IoT Edge cihazlarına modüller dağıtma | Microsoft Docs
description: Azure CLI 2.0 için IoT uzantısını kullanarak IoT Edge cihazlarına modüller dağıtma
author: chrissie926
manager: ''
ms.author: menchi
ms.date: 03/02/2018
ms.topic: tutorial
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
md.custom: mvc
ms.openlocfilehash: deee54fe5d11d6d1cf5485357f853b1cb078f96d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631590"
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Azure CLI 2.0 için IoT uzantısını kullanarak IoT Edge cihazlarına modüller dağıtma

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure CLI 2.0 Windows, Linux ve Mac OS ile kullanılabilir.

Azure CLI 2.0’ı kullanarak Azure IoT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub’ları hemen yönetmeye başlayabilirsiniz. IoT uzantısı, Azure CLI 2.0’ı cihaz yönetimi ve tam IoT Edge kapasitesi gibi özelliklerle zenginleştirir.

Bu makalede, Azure CLI 2.0 ve IoT uzantısını ayarlarsınız. Daha sonra, kullanılabilir CLI komutlarını kullanarak bir IoT Edge cihazına modüllerin nasıl dağıtılacağını öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabı. Henüz bir hesabınız yoksa bugün [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?v=17.39a). 

* [Python 2.7x veya Python 3.x](https://www.python.org/downloads/).

* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. Windows’a yüklemenin kolay bir yolu, [MSI](https://aka.ms/InstallAzureCliWindows) indirip yüklemektir.

* [Azure CLI 2.0 için loT uzantısı](https://github.com/Azure/azure-iot-cli-extension):
   1. `az extension add --name azure-cli-iot-ext` öğesini çalıştırın. 
   2. Yükleme sonrasında `az extension list` kullanarak o anda yüklü uzantıları doğrulayın veya `az extension show --name azure-cli-iot-ext` kullanarak IoT uzantısına ilişkin ayrıntıları görüntüleyin.
   3. Uzantıyı kaldırmak için `az extension remove --name azure-cli-iot-ext` komutunu kullanın.


## <a name="create-an-iot-edge-device"></a>IoT Edge cihazı oluşturma
Bu makalede, IoT Edge dağıtımı oluşturmaya yönelik yönergeler sağlanmaktadır. Örnekte, Azure hesabınızda oturum açma, bir Azure Kaynak Grubu (bir Azure çözümü için ilgili kaynakları tutan kapsayıcı) oluşturma, bir IoT Hub oluşturma, üç IoT Edge cihaz kimliği oluşturma, etiketler ayarlama ve sonra bu cihazları hedefleyen bir IoT Edge dağıtımı oluşturma işlemleri gösterilmiştir. 

Azure hesabınızda oturum açın. Aşağıdaki oturum açma komutunu girdikten sonra, bir defalık kod kullanarak web tarayıcısından oturum açmanız istenir: 

   ```cli
   az login
   ```

Doğu ABD bölgesinde **IoTHubCLI** adlı yeni bir kaynak grubu oluşturun: 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Kaynak grubu oluşturma][2]

Yeni oluşturulan kaynak grubunda **CLIDemoHub** adlı bir IoT hub oluşturun:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Her aboneliğe tek bir ücretsiz IoT hub tahsis edilir. CLI komutuyla boş bir hub oluşturmak için SKU değerini `--sku F1` ile değiştirin. Aboneliğinizde zaten boş bir hub varsa, ikincisini oluşturmayı denediğinizde bir hata iletisi alırsınız. 

IoT Edge cihazı oluşturma:

   ```cli
   az iot hub device-identity create --device-id edge001 --hub-name CLIDemoHub --edge-enabled
   ```

   ![IoT Edge cihazı oluşturma][4]

## <a name="configure-the-iot-edge-device"></a>IoT Edge cihazını yapılandırma

Bir dağıtım JSON şablonu oluşturun ve bunu yerel ortamda txt dosyası olarak kaydedin. apply-configuration komutunu çalıştırdığınızda dosyanın yolu gerekir.

Dağıtım JSON şablonları her zaman iki sistem modülünü içermelidir: edgeAgent ve edgeHub. Bu ikisine ek olarak, IoT Edge cihazına ek modüller dağıtmak için bu dosyayı kullanabilirsiniz. Bir tempSensor modülü ile IoT Edge cihazınızı yapılandırmak için aşağıdaki örneği kullanın:

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

IoT Edge cihazınıza yapılandırmayı uygulama:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

IoT Edge cihazınızda modülleri görüntüleme:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Modülleri listeleme][6]

## <a name="next-steps"></a>Sonraki adımlar

* [Bir IoT Edge cihazını ağ geçidi olarak kullanmayı](how-to-create-transparent-gateway.md) öğrenme

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png

