---
title: Yeni bir Azure IOT kenar cihaz (CLI) kaydetme | Microsoft Docs
description: Yeni bir IOT sınır cihazı kaydetmek için Azure CLI 2.0 IOT uzantısı kullanın
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/30/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a6935dfe5b32364e402017cd3005ab17969ce1ce
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036382"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Azure CLI 2.0 ile yeni bir Azure IOT kenar cihaz kaydetme

IOT cihazlarınızı Azure IOT Edge kullanmadan önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, kenar iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alırsınız. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure IOT Hub kaynakları, cihaz sağlama hizmet örneği ve kutunun dışında bağlantılı-hub yönetmenizi sağlar. IoT uzantısı, Azure CLI 2.0’ı cihaz yönetimi ve tam IoT Edge kapasitesi gibi özelliklerle zenginleştirir.

Bu makalede, Azure CLI 2.0 kullanan yeni bir IOT sınır cihazı kaydetmek gösterilmiştir.

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizde. 
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [IOT uzantısı için Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Bir cihaz oluşturma

IOT hub'ınıza yeni bir cihaz kimliği oluşturmak için aşağıdaki komutu kullanın: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Bu komut üç parametreleri içerir:
* **cihaz kimliği**: IOT hub'ınıza benzersizdir açıklayıcı bir ad sağlayın.
* **hub adı**: IOT hub'ınızın adını sağlayın.
* **Edge etkin**: Bu parametre cihazın IOT Edge ile kullanılmak üzere olduğunu bildirir.

   ![IoT Edge cihazı oluşturma](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Tüm cihazları görüntüleme

IOT hub'ınıza tüm cihazları görüntülemek için aşağıdaki komutu kullanın:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Bir IOT sınır cihazı özelliği olacağından kayıtlı herhangi bir aygıt **capabilities.iotEdge** kümesine **doğru**. 

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel kimliğini IOT hub'ında cihazınızla bağlanan bağlantı dizesi gerekiyor. Tek bir cihaz için bağlantı dizesini döndürmek için aşağıdaki komutu kullanın:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

Aygıt kimliği parametresi büyük/küçük harf duyarlıdır. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [modülleri Azure CLI 2.0 bir cihaza dağıtın](how-to-deploy-modules-cli.md)