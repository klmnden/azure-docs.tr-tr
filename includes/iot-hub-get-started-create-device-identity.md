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
ms.openlocfilehash: b2bce9788006a564def9bd8c1375a85dc4184b67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814906"
---
## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma

Bu bölümde, Bu öğretici için bir cihaz kimliği oluşturmak için Azure CLI'yı kullanın. Azure CLI içinde önceden [Azure Cloud Shell](~/articles/cloud-shell/overview.md), veya [Azure CLI'yi yerel olarak yükleme](/cli/azure/install-azure-cli). Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

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
