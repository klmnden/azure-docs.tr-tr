---
title: Azure IOT kenar modülleri (VS Code) dağıtma | Microsoft Docs
description: Visual Studio Code modülleri IOT kenar cihazına dağıtmak için kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7e75f2ff5e2df3189683d084a315ad6c8730be84
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036356"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Visual Studio Code Azure IOT kenar modüllerden dağıtma

IOT kenar modülleri, iş mantığı ile oluşturduktan sonra bunları sınırında çalışmaya aygıtlarınıza dağıtmak istiyorsanız. Toplamak ve verileri işlemek için birlikte çalışan birden fazla modülü varsa, bunları aynı anda dağıtması ve bunları bağlayan yönlendirme kurallarını bildirin. 

Bu makalede, bir JSON dağıtım bildirimi oluşturun, sonra dağıtım IOT kenar cihazına göndermek için bu dosyayı kullanmanız gösterilmektedir. Kullanıcıların paylaşılan etiketlere göre birden çok aygıt hedefleyen bir dağıtımı oluşturma hakkında daha fazla bilgi için bkz [dağıtma ve izleme ölçekte IOT kenar modülleri](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizde. 
* Bir [IOT sınır cihazı](how-to-register-device-portal.md) yüklü IOT kenar çalışma zamanı. 
* [Visual Studio Code](https://code.visualstudio.com/).
* [Azure IOT kenar uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) Visual Studio Code için. 

## <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimini yapılandırın

Bir dağıtım bildirimi modüllerine dağıtmak için modüller ve modül çiftlerini istenen özelliklerini arasında veri akışını açıklayan bir JSON belgedir. Dağıtım iş nasıl bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [nasıl IOT kenar modülleri, yapılandırılmış, yeniden ve kullanılabilecek anlamak](module-composition.md).

Visual Studio Code kullanarak modülleri dağıtmak için dağıtım bildirimi olarak yerel olarak Kaydet bir. JSON dosyası. Cihazınız için yapılandırmayı uygulamak için komutu çalıştırdığınızda, sonraki bölümde dosya yolunu kullanır.

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
                 "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
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

## <a name="sign-in-to-access-your-iot-hub"></a>IOT hub'ınızı erişmek için oturum açın

Visual Studio Code için Azure IOT uzantıları, IOT hub'ınıza işlemlerini gerçekleştirmek için kullanabilirsiniz. Bu işlemler çalışmak Azure hesabınızda oturum açın ve üzerinde çalıştığınız IOT hub'ını seçin gerekir.

1. Visual Studio Code'da açın **Explorer** görünümü.

2. Explorer alt kısmındaki genişletin **Azure IOT Hub cihazları** bölümü. 

   ![Azure IOT Hub cihazları genişletin](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

3. Tıklayın **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta görmüyorsanız, üstbilgi gelin. 

4. Seçin **IOT hub'ını seçin**.

5. Azure hesabınızda açmadınız varsa, bunu yapmak için yönergeleri izleyin. 

6. Azure aboneliğinizi seçin. 

7. IOT hub'ı seçin. 


## <a name="deploy-to-your-device"></a>Cihazınızı için dağıtma

Modülleri modül bilgileriyle yapılandırılmış dağıtım bildirimi uygulayarak aygıtınıza dağıtın. 

1. Visual Studio Code Gezgini görünümünde genişletin **Azure IOT Hub cihazları** bölümü. 

2. İle dağıtım bildirimini yapılandırmak istediğiniz cihaza sağ tıklayın. 

3. Seçin **IOT sınır cihazı için dağıtımı oluşturma**. 

4. Kullanmak istediğiniz dağıtım bildirim JSON dosyasına gidin ve'ı tıklatın **seçin kenar dağıtım bildirimi**. 

   ![Edge dağıtım bildirimi seçin](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)


Dağıtımınızı sonuçlarını VS Code çıktısında yazdırılır. Başarılı dağıtımları hedef aygıt çalışıyorsa, birkaç dakika içinde uygulanan ve internet'e bağlı. 

## <a name="view-modules-on-your-device"></a>Aygıtınızdaki modül görüntüle

Aygıtınıza modülleri dağıttıktan sonra sonra bunların tümünün görüntüleyebilirsiniz **Azure IOT Hub cihazları** bölümü. Genişletmek için IOT kenar Cihazınızı yanındaki oka seçin. Çalışmakta olan tüm modülleri görüntülenir. 

Yakın zamanda yeni modüller bir aygıta dağıtılmışsa, üzerine gelerek **Azure IOT Hub cihazları** bölüm başlığı ve Görünümü güncelleştirmek için yenileme simgesini seçin. 

Görüntülemek ve modül twin düzenlemek için bir modül adını sağ tıklatın. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [dağıtma ve izleme ölçekte IOT kenar modülleri](how-to-deploy-monitor.md)