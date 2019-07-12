---
title: OPC İkizi modülü sıfırdan Azure'a dağıtma | Microsoft Docs
description: OPC İkizi sıfırdan dağıtma
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 798f087c260b6b0a1efc366b864fe2bb7bce732e
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603695"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>OPC İkizi modülü ve bağımlılıkları sıfırdan dağıtma

OPC İkizi modülü, IOT Edge üzerinde çalışır ve birkaç uç hizmetlerinin OPC cihaz ikizine ve kayıt defteri hizmetleri sağlar. 

Modüllerini dağıtmak için birkaç seçenek vardır, [Azure IOT Edge](https://azure.microsoft.com/services/iot-edge/) aralarında bir ağ geçidi

- [Azure portal'ın IOT Edge dikey penceresinden dağıtma](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [AZ CLI kullanarak dağıtma](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Dağıtım ayrıntıları ve yönergeleri hakkında daha fazla bilgi için bkz. GitHub [depo](https://github.com/Azure/azure-iiot-components).

## <a name="deployment-manifest"></a>Dağıtım bildirimi

Tüm modüller, bir dağıtım bildirimi kullanılarak dağıtılır.  Her ikisi de dağıtmak için bir örnek bildirim [OPC yayımcı](https://github.com/Azure/iot-edge-opc-publisher) ve [OPC İkizi](https://github.com/Azure/azure-iiot-opc-twin-module) aşağıda gösterilmiştir.

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
              "registryCredentials": {}
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": ""
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "opctwin": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-twin:latest",
                "createOptions": "{\"NetworkingConfig\": {\"EndpointsConfig\": {\"host\": {}}}, \"HostConfig\": {\"NetworkMode\": \"host\" }}"
              }
            },
            "opcpublisher": {
              "version": "2.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-publisher:latest",
                "createOptions": "{\"Hostname\":\"publisher\",\"Cmd\":[\"publisher\",\"--pf=./pn.json\",\"--di=60\",\"--to\",\"--aa\",\"--si=0\",\"--ms=0\"],\"ExposedPorts\":{\"62222/tcp\":{}},\"NetworkingConfig\":{\"EndpointsConfig\":{\"host\":{}}},\"HostConfig\":{\"NetworkMode\":\"host\",\"PortBindings\":{\"62222/tcp\":[{\"HostPort\":\"62222\"}]}}}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
            "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      }
    }
  }
}
```

## <a name="deploying-from-azure-portal"></a>Azure Portalı'ndan dağıtma

Azure portalı üzerinden modülleri Azure IOT Edge ağ geçidi cihazına dağıtmak için en kolay yoludur.  

### <a name="prerequisites"></a>Önkoşullar

1. OPC İkizi dağıtma [bağımlılıkları](howto-opc-twin-deploy-dependencies.md) ve bunun sonucunda elde edilen `.env` dosya. Dağıtılan Not `hub name` , `PCS_IOTHUBREACT_HUB_NAME` sonuç değişkeninde `.env` dosya.

2. Kaydolun ve başlayın bir [Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) veya [Windows](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-windows) IOT Edge ağ geçidi ve Not kendi `device id`.

### <a name="deploy-to-an-edge-device"></a>Bir edge cihazına dağıtma

1. Oturum [Azure portalında](https://portal.azure.com/) ve IOT hub'ınıza gidin.

2. Seçin **IOT Edge** sol taraftaki menüden.

3. Hedef cihazın cihazlar listesinden numarasını tıklayın.

4. **Modülleri Ayarlama**'yı seçin.

5. İçinde **dağıtım modülleri** sayfasında bölümünü **Ekle** ve **IOT Edge modülü.**

6. İçinde **IOT Edge özel Modülü** iletişim kullanım `opctwin` modül için ad kapsayıcı belirtmezseniz *URI görüntü* olarak

   ```bash
   mcr.microsoft.com/iotedge/opc-twin:latest
   ```

   Olarak *oluşturma seçenekleri* aşağıdaki JSON kullanın:

   ```json
   {"NetworkingConfig": {"EndpointsConfig": {"host": {}}}, "HostConfig": {"NetworkMode": "host" }}
   ```

   Gerekirse, isteğe bağlı alanları doldurun. Kapsayıcı hakkında daha fazla bilgi seçenekleri, yeniden başlatma ilkesi oluşturabilir ve istenen durumunu görmek için [EdgeAgent istenen özelliklerini](https://docs.microsoft.com/azure/iot-edge/module-edgeagent-edgehub#edgeagent-desired-properties). Modül ikizi hakkında daha fazla bilgi için bkz. [tanımlayın veya güncelleştirme istenen özelliklerini](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

7. Seçin **Kaydet** ve yineleyin **5**.  

8. IOT Edge özel modülü iletişim kutusunda, kullanmak `opcpublisher` modülü ve kapsayıcı adı olarak *URI görüntü* olarak 

   ```bash
   mcr.microsoft.com/iotedge/opc-publisher:latest
   ```

   Olarak *oluşturma seçenekleri* aşağıdaki JSON kullanın:

   ```json
   {"Hostname":"publisher","Cmd":["publisher","--pf=./pn.json","--di=60","--to","--aa","--si=0","--ms=0"],"ExposedPorts":{"62222/tcp":{}},"HostConfig":{"PortBindings":{"62222/tcp":[{"HostPort":"62222"}] }}}
   ```

9. Seçin **Kaydet** ardından **sonraki** yollar bölüme geçmek için.

10. Yollar sekmesinde, aşağıdaki yapıştırın 

    ```json
    {
      "routes": {
        "opctwinToIoTHub": "FROM /messages/modules/opctwin/outputs/* INTO $upstream",
        "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/outputs/* INTO $upstream"
      }
    }
    ```

    seçip **İleri**

11. Dağıtım bilgilerinizi gözden geçirin ve bildirim.  Bu, yukarıdaki dağıtım bildirimi gibi görünmelidir.  Seçin **gönderme**.

12. Cihazınıza modülleri dağıttıktan sonra bunların tümünün görüntüleyebilirsiniz **cihaz ayrıntıları** portal sayfası. Bu sayfa, her dağıtılan modülü yanı sıra dağıtım durumu ve çıkış kodu gibi yararlı bilgiler adını görüntüler.

## <a name="deploying-using-azure-cli"></a>Azure CLI kullanarak dağıtma

### <a name="prerequisites"></a>Önkoşullar

1. En son sürümünü yükleyin [Azure komut satırı arabirimi (AZ)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) gelen [burada](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="quickstart"></a>Hızlı Başlangıç

1. Yukarıdaki dağıtım bildirimine kaydetmek bir `deployment.json` dosya.  

2. IOT Edge cihazına yapılandırmayı uygulamak için aşağıdaki komutu kullanın:

   ```bash
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./deployment.json
   ```

   `device id` Parametre duyarlıdır. İçerik parametresi dağıtım noktalarına bildirim kaydettiğiniz dosyası. 
    ![az IOT Edge modülleri kümesini çıktı](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/set-modules.png)

3. Cihazınıza modülleri dağıttıktan sonra tüm bunları aşağıdaki komutla görebilirsiniz:

   ```bash
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

   Cihaz kimliği parametresi büyük/küçük harf duyarlıdır. ![az IOT hub kimlik modülü liste çıkışı](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Sonraki adımlar

Sıfırdan OPC İkizi dağıtmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Mevcut bir projeyi OPC İkizi dağıtma](howto-opc-twin-deploy-existing.md)
