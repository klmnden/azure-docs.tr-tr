---
title: Ağ geçidi - Azure IOT Edge modbus protokollerini çevirme | Microsoft Docs
description: IoT Edge ağ geçidi cihazı oluşturarak Modbus TCP kullanan cihazların Azure IoT Hub ile iletişim kurmasına imkan sağlama
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 1c9855f982b888e8e1d68bfe5233983db8c826ad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61248044"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Bir IOT Edge ağ geçidi cihazı aracılığıyla Modbus TCP cihazlarını bağlama

Modbus TCP veya RTU protokollerini kullanan IoT cihazlarını bir Azure IoT hub’ına bağlamak istiyorsanız ağ geçidi olarak bir IoT Edge cihazı kullanın. Ağ geçidi cihazı Modbus cihazlarınızdan verileri okur, sonra desteklenen bir protokolü kullanarak bu verileri buluta iletir.

![Modbus cihazları IOT Edge ağ geçidi üzerinden IOT hub'a bağlama](./media/deploy-modbus-gateway/diagram.png)

Bu makalede, bir Modbus modülü için kendi kapsayıcı görüntünüzü oluşturma (dilerseniz önceden oluşturulmuş bir örneği de kullanabilirsiniz) ve bu görüntüyü ağ geçidi olarak kullanacağınız IoT Edge cihazına dağıtma işlemleri açıklanmaktadır.

Bu makalede Modbus TCP protokolünü kullandığınız varsayılır. Modülün Modbus RTU'yu destekleyecek şekilde yapılandırma hakkında daha fazla bilgi için bkz. [Azure IOT Edge Modbus Modülü](https://github.com/Azure/iot-edge-modbus) github'daki proje.

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure IoT Edge cihazı. Ayarlama konusunda bir kılavuz için bkz. [Windows üzerinde Azure IOT Edge'e dağıtma](quickstart.md) veya [Linux](quickstart-linux.md).
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.
* Modbus TCP’yi destekleyen fiziksel veya sanal bir cihaz.

## <a name="prepare-a-modbus-container"></a>Modbus kapsayıcısı hazırlama

Modbus ağ geçidinin işlevselliğini test etmek istiyorsanız Microsoft tarafından sağlanan örnek modülü kullanabilirsiniz. Modül Azure Market'ten erişebileceğiniz [Modbus](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview), veya URI görüntüyle **mcr.microsoft.com/azureiotedge/modbus:1.0**.

Kendi modülünüzü oluşturmak ve ortamınız için özelleştirmek istiyorsanız, açık kaynaklı yoktur [Azure IOT Edge Modbus Modülü](https://github.com/Azure/iot-edge-modbus) github'daki proje. Kendi kapsayıcı görüntünüzü oluşturmak için bu projedeki yönergeleri izleyin. Kendi kapsayıcı görüntünüzü oluşturursanız, başvurmak [geliştirme C# Visual Studio'daki modüller](how-to-visual-studio-develop-csharp-module.md) veya [Visual Studio code'da modülleri geliştirme](how-to-vs-code-develop-module.md). Bu makaleler, yeni modülleri oluşturma ve kapsayıcı görüntülerini bir kayıt defterine yayımlama yönergeleri sağlar.

## <a name="try-the-solution"></a>Çözümü deneyin

Bu bölümde, Microsoft'un örnek Modbus modülü IOT Edge cihazınıza dağıtma işlemi gösterilir.

1. [Azure portalında](https://portal.azure.com/) IoT hub'ınıza gidin.

2. Git **IOT Edge** ve IOT Edge Cihazınızda'ı tıklatın.

3. **Modül ayarla**’yı seçin.

4. Modbus modülünü ekleyin:

   1. Tıklayın **Ekle** seçip **IOT Edge Modülü**.

   2. **Ad** alanına "modbus" yazın.

   3. **Resim** alanına örnek kapsayıcının resim URI’sini girin: `mcr.microsoft.com/azureiotedge/modbus:1.0`.

   4. Modül ikizinin istenen özelliklerini güncelleştirmek için **Etkinleştir** kutusunu işaretleyin.

   5. Aşağıdaki JSON’u metin kutusuna kopyalayın. **SlaveConnection** değerini Modbus cihazınızın IPv4 adresi olarak değiştirin.

      ```JSON
      {
        "properties.desired":{
          "PublishInterval":"2000",
          "SlaveConfigs":{
            "Slave01":{
              "SlaveConnection":"<IPV4 address>","HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{
                "Op01":{
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"400001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. **Kaydet**’i seçin.

5. **Modül Ekle** adımına dönüp **İleri**’yi seçin.

7. **Yolları Belirtin** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Bu yol, Modbus modülü tarafından toplanan tüm iletileri IoT Hub’a gönderir. Bu yoldaki '' modbusOutput'', Modbus modülünün veri çıktısı gerçekleştirmek uç noktadır ve IOT Edge hub'ı iletileri IOT hub'a göndermesini söyleyen özel bir hedef '' upstream''.
   ```JSON
   {
    "routes": {
      "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
    }
   }
   ```

8. **İleri**’yi seçin.

9. **Dağıtımı Gözden Geçirin** adımında **Gönder**'i seçin.

10. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Yeni görmelisiniz **modbus** IOT Edge çalışma zamanıyla birlikte çalıştığını modülü.

## <a name="view-data"></a>Verileri görüntüleme
Modbus modülü üzerinden gelen verileri görüntüleyin:
```cmd/sh
iotedge logs modbus
```

Ayrıca cihaz kullanarak gönderdiği telemetriyi görüntüleyebilirsiniz [Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eski adıyla Azure IOT Toolkit uzantısını).

## <a name="next-steps"></a>Sonraki adımlar

- IOT Edge cihazları nasıl ağ geçidi olarak görev yapabilir hakkında daha fazla bilgi için bkz: [saydam bir ağ geçidi olarak davranır bir IOT Edge cihazı oluşturma](./how-to-create-transparent-gateway.md).
- IOT Edge modüllerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [anlamak Azure IOT Edge modülleri](iot-edge-modules.md).
