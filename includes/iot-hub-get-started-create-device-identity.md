---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 09/07/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: e80033d696de1b83da43fc27e5be9eca3b3f8757
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44168547"
---
## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma

Bu bölümde, Bu öğretici için bir cihaz kimliği oluşturmak için Azure CLI'yı kullanın. Azure CLI içinde önceden [Azure Cloud Shell](https://docs.microsoft.com/zure/cloud-shell/overview), veya [yerel olarak yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

1. Aşağıdaki komut, IOT uzantısını yüklemek için Azure CLI'yı burada kullandığınız komut satırı ortamında çalıştırın:

    ```cmd/sh
    az extension add --name azure-cli-iot-ext
    ```

1. Azure CLI'yi yerel olarak çalıştırıyorsanız, Azure hesabınızda (Cloud Shell kullanıyorsanız, otomatik olarak imzalanmış ve bu komutu çalıştırmanız gerekmez varsa) oturum açmak için aşağıdaki komutu kullanın:

    ```cmd/sh
    az login
    ```

1. Son olarak, adlı yeni bir cihaz kimliği oluşturma `myDeviceId` ve cihaz bağlantı dizesi şu komutlarla alın:

    ```cmd/sh
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Sonuç cihaz bağlantı dizesini not edin. Bu cihaz bağlantı dizesi, bir cihaz olarak IOT hub'ınıza bağlanmak için cihaz uygulama tarafından kullanılır.

<!-- images and links -->
