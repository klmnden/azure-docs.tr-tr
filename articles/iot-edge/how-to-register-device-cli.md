---
title: Komut satırından - Azure IOT Edge yeni bir cihaz kaydetme | Microsoft Docs
description: Yeni bir IOT Edge cihazı kaydedin ve bağlantı dizesini almak için Azure CLI için IOT uzantısı kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: de00c317483da9bcd93bb2e2505350d787385925
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66495384"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli"></a>Azure CLI ile yeni bir Azure IOT Edge cihazı kaydetme

IOT cihazlarınızı Azure IOT Edge ile kullanabilmeniz için önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihaz kaydettiğinizde, IOT Edge iş yükleri için Cihazınızı ayarlamak için kullanılan bir bağlantı dizesi alır.

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) çapraz platform komut satırı aracı IOT Edge gibi Azure kaynaklarını yönetmek için bir açık kaynak. Azure IOT Hub kaynaklarını, cihaz sağlama hizmeti örneklerini ve bağlı hub'ları hazır yönetmenizi sağlar. Yeni IOT uzantısı, Azure CLI cihaz yönetimi ve tam IOT Edge özelliği gibi özelliklerle zenginleştirir.

Bu makalede, Azure CLI kullanarak yeni bir IOT Edge cihazı kaydettirmek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Bir [IOT hub'ı](../iot-hub/iot-hub-create-using-cli.md) Azure aboneliğinizdeki.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ortamınızdaki. En az 2.0.24 Azure CLI sürümünüzü olmalıdır veya üzeri. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar.
* [Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Cihaz oluşturma

Kullanım [az IOT hub cihaz kimliği oluşturma](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) IOT hub'ına yeni bir cihaz kimliği oluşturmak için komutu. Örneğin:

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Bu komut üç parametreleri içerir:

* **cihaz kimliği**: IOT hub'ınıza benzersiz bir açıklayıcı ad girin.

* **hub adı**: IOT hub'ınızın adını sağlayın.

* **Edge özellikli**: Bu parametre, cihazın IOT Edge ile kullanılmak üzere olduğunu bildirir.

   ![Çıkış az IOT hub cihaz kimliği oluşturma](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Tüm cihazları görüntüle

Kullanım [az IOT hub cihaz kimliği listesi](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-list) tüm cihazlar IOT hub'ına görmek için komutu. Örneğin:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

IOT Edge cihazı özelliğine sahip şekilde kayıtlı herhangi bir CİHAZDAN **capabilities.iotEdge** kümesine **true**.

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır. Kullanım [az IOT hub cihaz kimliği show-connection-string](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string) tek bir cihaz bağlantı dizesini geri dönmek için komut:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

Değeri `device-id` parametresi duyarlıdır. Bağlantı dizesi etrafındaki tırnak işaretlerini bulundurmanıza gerek yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [modülleri, Azure CLI ile bir cihaz dağıtmak](how-to-deploy-modules-cli.md).
