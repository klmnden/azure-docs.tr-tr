---
title: VS Code için Azure IOT Araç Seti kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: IOT hub'ı oluşturmak için VS Code için Azure IOT Toolkit kullanma
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: af31f9375d6a41e13a9122e9730ba9532d3d52c5
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003078"
---
# <a name="create-an-iot-hub-using-the-azure-iot-toolkit-for-visual-studio-code"></a>Visual Studio Code için Azure IOT Araç Seti kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Kullanabileceğiniz [Visual Studio Code için Azure IOT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) Azure IOT hub'ları oluşturmak için. Bu makale Azure IOT araç seti ile IOT hub oluşturma gösterilmektedir.

Bu makaleyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT Araç Seti](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. Visual Studio Code'da açmak **Gezgini** görünümü.

2. Explorer alt kısmında, Genişlet **Azure IOT Hub cihazları** bölümü. 

   ![Azure IOT Hub cihazları genişletin](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Tıklayarak **... ** içinde **Azure IOT Hub cihazları** bölüm başlığı. Üç nokta simgesini görmüyorsanız, üst bilgisinin üzerinde gezdirin. 

4. Seçin **IOT Hub oluşturma**.

5. Bir açılır pencere, Azure'da ilk kez oturum açarken izin vermek için sağ alt köşedeki gösterilir.

6. Azure aboneliğini seçin. 

7. Kaynak grubunu seçin.

8. Konum seçin.

9. Fiyatlandırma katmanını seçin.

10. IOT Hub'ınız için genel olarak benzersiz bir ad girin.

11. IOT hub'ı oluşturulana kadar birkaç dakika bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio Code için Azure IOT Araç Seti kullanarak IOT hub'ı dağıttığınız artık daha iyi keşfedilebilmesi isteyebilirsiniz:

* [Visual Studio Code için Azure IOT Toolkit uzantısını IOT Hub ve cihaz arasında iletileri gönderip kullanılacağını](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).
* [Azure IOT Toolkit uzantısını Visual Studio Code için Azure IOT Hub cihaz yönetimi için kullanın.](iot-hub-device-management-iot-toolkit.md)
* [Wiki sayfası](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki) için Azure IOT Araç Seti.
