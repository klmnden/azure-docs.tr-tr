---
title: VS Code için Azure IOT araçları kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: IOT hub'ı oluşturmak için VS Code için Azure IOT araçları kullanma
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: junhan
ms.openlocfilehash: c37eeec6429e8367ade12b58bb4e20022423edf6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66166228"
---
# <a name="create-an-iot-hub-using-the-azure-iot-tools-for-visual-studio-code"></a>Visual Studio Code için Azure IOT araçları kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede nasıl kullanılacağını gösterir [Visual Studio Code için Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Azure IOT hub'ı oluşturmak için. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makaleyi tamamlamak için aşağıdakiler gerekir:

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Visual Studio Code için.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. Visual Studio Code'da açmak **Gezgini** görünümü.

2. Explorer alt kısmında, Genişlet **Azure IOT Hub cihazları** bölümü. 

   ![Azure IOT Hub cihazları genişletin](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Tıklayarak **...**  içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta simgesini görmüyorsanız, üst bilgisinin üzerinde gezdirin. 

4. Seçin **IOT Hub oluşturma**.

5. Bir açılır pencere, Azure'da ilk kez oturum açarken izin vermek için sağ alt köşedeki gösterilir.

6. Azure aboneliğini seçin. 

7. Kaynak grubunu seçin.

8. Konum seçin.

9. Fiyatlandırma katmanını seçin.

10. IOT Hub'ınız için genel olarak benzersiz bir ad girin.

11. IOT hub'ı oluşturulana kadar birkaç dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Artık Visual Studio Code için Azure IOT araçları kullanarak bir IOT hub'a dağıttım. Daha fazla araştırmak için aşağıdaki makalelere göz denetleyin:

* [Cihazınızı IOT hub'ı arasındaki iletiler gönderip almak için Visual Studio Code için Azure IOT araçları kullanın](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

* [Azure IOT araçları Visual Studio Code için Azure IOT Hub cihaz yönetimi için kullanın.](iot-hub-device-management-iot-toolkit.md)

* [Azure IOT hub'ı Araç Seti wiki sayfasına bakın](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki).
