---
title: Yeni bir Azure IOT Edge cihazı (CLI) kaydetme | Microsoft Docs
description: Yeni bir IOT Edge cihazı kaydetmek için Azure CLI 2.0 için IOT uzantısını kullanma
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 451f4df31cd1c520b14227829923f72fe80c38c3
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325505"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Azure CLI 2.0 ile yeni bir Azure IOT Edge cihazı kaydetme

IOT cihazlarınızı Azure IOT Edge ile kullanabilmeniz için önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, Edge iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alır. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), IoT Edge gibi Azure kaynaklarını yönetmeye yönelik açık kaynaklı bir platformlar arası komut satırı aracıdır. Azure IOT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub'ları hazır yönetmenizi sağlar. IoT uzantısı, Azure CLI 2.0’ı cihaz yönetimi ve tam IoT Edge kapasitesi gibi özelliklerle zenginleştirir.

Bu makalede, Azure CLI 2.0 kullanarak yeni bir IOT Edge cihazı kaydettirmek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizdeki. 
* Ortamınızdaki [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 
* [Azure CLI 2.0 için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Bir cihaz oluşturma

IOT hub'ına yeni bir cihaz kimliği oluşturmak için aşağıdaki komutu kullanın: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Bu komut üç parametreleri içerir:
* **cihaz kimliği**: IOT hub'ınıza benzersiz bir açıklayıcı ad girin.
* **hub adı**: IOT hub'ınızın adını sağlayın.
* **Edge özellikli**: Bu parametre cihazın IOT Edge ile kullanılmak üzere olduğunu bildirir.

   ![IoT Edge cihazı oluşturma](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Tüm cihazları görüntüle

IOT hub'ına tüm cihazları görüntülemek için aşağıdaki komutu kullanın:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

IOT Edge cihazı özelliğine sahip şekilde kayıtlı herhangi bir CİHAZDAN **capabilities.iotEdge** kümesine **true**. 

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır. Tek bir cihaz bağlantı dizesini geri dönmek için aşağıdaki komutu kullanın:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

Cihaz kimliği parametresi büyük/küçük harf duyarlıdır. Bağlantı dizesi etrafındaki tırnak işaretlerini bulundurmanıza gerek yoktur. 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [modülleri Azure CLI 2.0 ile bir cihaza dağıtın](how-to-deploy-modules-cli.md)