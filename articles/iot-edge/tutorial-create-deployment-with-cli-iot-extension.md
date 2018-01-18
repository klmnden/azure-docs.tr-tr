---
title: "Modüller için Azure CLI 2.0 IOT uzantısını kullanarak IOT kenar cihazlara dağıtma | Microsoft Docs"
description: "Azure CLI 2.0 için IOT uzantısını kullanarak bir IOT sınır cihazı modülleri dağıtma"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 26067187864f9a2a4c85c953ae8aca888458d245
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Azure CLI 2.0 için IOT uzantısını kullanarak bir IOT sınır cihazı modülleri dağıtma

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) çapraz platform komut satırı aracı IOT kenar gibi Azure kaynakları yönetmek için açık bir kaynaktır. Azure CLI 2.0, Windows, Linux ve MacOS kullanılabilir.

Azure CLI 2.0 Azure IOT Hub kaynakları, cihaz sağlama hizmet örneği ve kutunun dışında bağlantılı-hub yönetmenizi sağlar. Yeni IOT uzantısı aygıt yönetimi ve tam IOT kenar özelliği gibi özellikler ile Azure CLI 2.0 aşağıdakilere zenginleştirir.

Bu öğreticide, ilk Azure CLI 2.0 ve IOT uzantısı ayarlamak için adımları tamamlayın. Ardından, modüller CLI komutları kullanarak bir IOT sınır cihazı dağıtmayı öğrenin.

## <a name="installation"></a>Yükleme 

### <a name="step-1---install-python"></a>1. adım - yükleme Python

[Python 2.7 x veya Python 3.x](https://www.python.org/downloads/) gereklidir.

### <a name="step-2---install-azure-cli-20"></a>Adım 2 - Azure CLI 2.0 yükleyin

İzleyin [yükleme yönergesindeki](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) Azure CLI 2.0 ortamınızda kurulumu. En azından, Azure CLI 2.0 sürümünüzün 2.0.24 olması gerekir veya üstü. Kullanım `az –version` doğrulamak için. Bu sürüm az uzantısını komutları destekler ve Knack komut çerçevesi sunar. Windows yüklemek için bir basit yoludur indirmek ve yüklemek için [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>3. adım - yükleme IOT uzantısı

[IOT uzantısı Benioku](https://github.com/Azure/azure-iot-cli-extension) uzantıyı yüklemek için çeşitli yollar açıklanmaktadır. En basit yolu çalıştırmaktır `az extension add --name azure-cli-iot-ext`. Yükleme sonrasında, kullandığınız `az extension list` yüklü uzantıları doğrulamak için veya `az extension show --name azure-cli-iot-ext` IOT uzantısı ayrıntılarını görmek için. Uzantıyı kaldırmak için kullanabileceğiniz `az extension remove --name azure-cli-iot-ext`.


## <a name="deploy-modules-to-an-iot-edge-device"></a>Modülleri IOT kenar cihazına dağıtın
Bu öğreticide, bir IOT kenar dağıtımın nasıl oluşturulacağı öğreneceksiniz. Örnek Azure hesabınızda oturum açmak, Azure kaynak grubu (Azure çözümünü ilgili kaynaklara tutan bir kapsayıcı) oluşturmak, IOT Hub oluşturma, üç IOT kenar cihaz kimliği oluşturma, kümesi etiketleri ve, bir IOT kenar dağıtımı oluşturmak nasıl gösterir Bu cihazlar hedefler. Başlamadan önce daha önce açıklanan yükleme adımlarını tamamlayın. Bir Azure hesabınız yoksa henüz yapabilecekleriniz, [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/?v=17.39a) bugün. 


### <a name="1-login-to-the-azure-account"></a>1. Azure hesabı için oturum açma
  
    az login

![oturum açma][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Bir kaynak grubu IoTHubBlogDemo içinde eastus oluşturun

    az group create -l eastus -n IoTHubBlogDemo

![Kaynak grubu oluşturma][2]


### <a name="3-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>3. Yeni oluşturulmuş bir kaynak grubu altında bir IOT hub'ı blogDemoHub oluşturma

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![IOT Hub oluşturma][3]


### <a name="4-create-an-iot-edge-device"></a>4. Bir IOT sınır cihazı oluşturma

    az iot hub device-identity create -d edge001 -n blogDemoHub --edge-enabled

![IOT sınır cihazı oluşturma][4]

### <a name="5-apply-configuration-to-the-iot-edge-device"></a>5. IOT sınır cihazı yapılandırmasını Uygula

Dağıtım JSON şablonunuzu yerel olarak bir txt dosyası olarak kaydedin. Uygula yapılandırma komutunu çalıştırdığınızda, dosya yolunu gerekir.

Bir tempSensor modülü içeren bir örnek dağıtım JSON şablonu aşağıdadır:

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

    az iot hub apply-configuration --device-id edge001 --hub-name blogDemoHub --content C:\<yourLocation>\edgeconfig.txt

![Yapılandırmasını Uygula][5]

### <a name="6-list-modules"></a>6. Liste modülleri
    
    az iot hub module-identity list --device-id edge001 --hub-name blogDemoHub

![Liste modülleri][6]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, IOT kenar cihazınız tarafından oluşturulan ham verileri filtrelemek için kodu içeren bir Azure işlevi oluşturuldu. Azure IOT kenar keşfetme tutmak için bir IOT sınır cihazı bir ağ geçidi olarak kullanmayı öğrenin. 

> [!div class="nextstepaction"]
> [Bir IOT sınır ağ geçidi cihazı oluşturma](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-create-deployment-with-cli-iot-extension/login.jpg
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.jpg
[3]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-hub.jpg
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[5]: ./media/tutorial-create-deployment-with-cli-iot-extension/apply-configuration.PNG
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.PNG

