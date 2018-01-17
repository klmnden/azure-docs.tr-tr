---
title: "Azure IoT Edge’de Modbus dağıtma | Microsoft Docs"
description: "IoT Edge ağ geçidi cihazı oluşturarak Modbus TCP kullanan cihazların Azure IoT Hub ile iletişim kurmasına imkan sağlama"
services: iot-Edge
documentationcenter: 
author: kgremban
manager: timlt
editor: chrisgmsft
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: kgremban
ms.custom: 
ms.openlocfilehash: e239bde48c3da0d899e3c78bdd39f520c4128b95
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway---preview"></a>IoT Edge ağ geçidi cihazı aracılığıyla Modbus TCP cihazlarını bağlama - önizleme

Modbus TCP veya RTU protokollerini kullanan IoT cihazlarını bir Azure IoT hub’ına bağlamak istiyorsanız ağ geçidi olarak bir IoT Edge cihazı kullanın. Ağ geçidi cihazı Modbus cihazlarınızdan verileri okur, sonra desteklenen bir protokolü kullanarak bu verileri buluta iletir. 

![Modbus cihazları IoT Hub’a Edge ağ geçidi üzerinden bağlanır](./media/deploy-modbus-gateway/diagram.png)

Bu makalede, bir Modbus modülü için kendi kapsayıcı görüntünüzü oluşturma (dilerseniz önceden oluşturulmuş bir örneği de kullanabilirsiniz) ve bu görüntüyü ağ geçidi olarak kullanacağınız IoT Edge cihazına dağıtma işlemleri açıklanmaktadır. 

Bu makalede Modbus TCP protokolünü kullandığınız varsayılır. Modülün Modbus RTU’yu destekleyecek şekilde yapılandırılması hakkında daha fazla bilgi edinmek için Github’daki [Azure IoT Edge Modbus modülü](https://github.com/Azure/iot-edge-modbus) projesine başvurun. 

## <a name="prerequisites"></a>Önkoşullar
* Bir Azure IoT Edge cihazı. Edge cihazı ayarlama konusunda adım adım bir kılavuz için bkz. [Linux](tutorial-simulate-device-linux.md) veya [Windows’da bir sanal cihaza Azure IoT Edge dağıtma](tutorial-simulate-device-windows.md). 
* IoT Edge cihazı için birincil anahtar bağlantı dizesi.
* Modbus TCP’yi destekleyen fiziksel veya sanal bir cihaz.

## <a name="prepare-a-modbus-container"></a>Modbus kapsayıcısı hazırlama

Modbus ağ geçidinin işlevselliğini test etmek istiyorsanız Microsoft tarafından sağlanan örnek modülü kullanabilirsiniz. Örnek modülü kullanmak için [Çözümü çalıştırın](#run-the-solution) bölümüne gidip aşağıdaki Resim URI’sini girin: 

```URL
microsoft/azureiotedge-modbus-tcp:1.0-preview
```

Kendi modülünüzü oluşturmak ve ortamınız için özelleştirmek istiyorsanız, Github’daki açık kaynak [Azure IOT Edge Modbus modülü](https://github.com/Azure/iot-edge-modbus) projesine göz atın. Kendi kapsayıcı görüntünüzü oluşturmak için bu projedeki yönergeleri izleyin. Kendi kapsayıcı görüntünüzü oluşturursanız, kapsayıcı görüntülerini bir kayıt defterinde yayımlama ve cihazınıza özel modül dağıtma işlemleriyle ilgili yönergeler için bkz. [C# IoT Edge modülü geliştirme ve dağıtma](tutorial-csharp-module.md). 


## <a name="run-the-solution"></a>Çözümü çalıştırın
1. [Azure portalında](https://portal.azure.com/) IoT hub'ınıza gidin.
2. **IoT Edge (önizleme)** sayfasına gidip IoT Edge cihazınızı seçin.
3. **Modül ayarla**’yı seçin.
4. Modbus modülünü ekleyin:
   1. **IoT Edge modülü ekle**’yi seçin.
   2. **Ad** alanına "modbus" yazın.
   3. **Resim** alanına örnek kapsayıcının resim URI’sini girin: `microsoft/azureiotedge-modbus-tcp:1.0-preview`.
   4. Modül ikizinin istenen özelliklerini güncelleştirmek için **Etkinleştir** kutusunu işaretleyin.
   5. Aşağıdaki JSON’u metin kutusuna kopyalayın. **SlaveConnection** değerini Modbus cihazınızın IPv4 adresi olarak değiştirin.

      ```JSON
      {  
        "properties.desired":{  
          "PublishInterval":"2000",
          "SlaveConfigs":{  
            "Slave01":{  
              "SlaveConnection":"<IPV4 address>",
              "HwId":"PowerMeter-0a:01:01:01:01:01",
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
7. **Yolları Belirtin** adımında, aşağıdaki JSON’u metin kutusuna kopyalayın. Bu yol, Modbus modülü tarafından toplanan tüm iletileri IoT Hub’a gönderir. Bu yoldaki ''modbusOutput'', Modbus modülünün veri çıktısı gerçekleştirmek için kullandığı uç noktadır ve ''upstream'', Edge Hub’a iletileri IoT Hub’a göndermesini söyleyen özel bir hedeftir. 
   ```JSON
   {
    "routes": {
      "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
    }
   }
   ```

8. **İleri**’yi seçin. 
9. **Şablonu Gözden Geçirin** adımında **Gönder**’i seçin. 
10. Cihaz ayrıntıları sayfasına dönüp **Yenile**’yi seçin. Yeni **modbus**’un IoT Edge çalışma zamanıyla birlikte çalıştığını görmelisiniz.

## <a name="view-data"></a>Verileri görüntüleme
Modbus modülü üzerinden gelen verileri görüntüleyin:
```cmd/sh
docker logs -f modbus
```

Ayrıca, [IoT Hub Gezgini aracını](https://github.com/azure/iothub-explorer) kullanarak cihazın gönderdiği telemetriyi görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- IoT Edge cihazlarının nasıl ağ geçidi olarak kullanılabildiği hakkında daha fazla bilgi edinmek için bkz. [Saydam bir ağ geçidi olarak kullanılabilen bir IoT Edge cihazı oluşturma](how-to-create-transparent-gateway.md)
- IoT Edge modüllerinin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Edge modüllerini anlama](iot-edge-modules.md)