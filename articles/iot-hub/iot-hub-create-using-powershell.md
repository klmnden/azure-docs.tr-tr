---
title: Bir PowerShell cmdlet'ini kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: Nasıl bir IOT hub'ı oluşturmak için PowerShell cmdlet'ini kullanın.
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/24/2018
ms.author: robinsh
ms.openlocfilehash: f9903781a998db8192e3958ae386b7420f56fd31
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045635"
---
# <a name="create-an-iot-hub-using-the-new-azurermiothub-cmdlet"></a>New-AzureRmIotHub cmdlet'ini kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Giriş

Oluşturma ve Azure IOT hub'ları yönetmek için Azure PowerShell cmdlet'lerini kullanabilirsiniz. Bu öğretici, IOT hub'ı PowerShell ile oluşturma işlemini göstermektedir.

Bu nasıl yapılır tamamlamak için bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

Cloud Shell kullanıyorsanız, zaten aboneliğinize oturum açtınız. PowerShell'i yerel olarak bunun yerine çalıştırıyorsanız, Azure aboneliğinizde oturum açmak için aşağıdaki komutu girin:

```powershell
# Log into Azure account.
Login-AzureRMAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir IOT hub'ı dağıtmak için bir kaynak grubu gerekir. Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun.

IOT hub'ınız için bir kaynak grubu oluşturmak için kullanın [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) komutu. Bu örnek adlı bir kaynak grubu oluşturur **MyIoTRG1** içinde **Doğu ABD** bölgesi:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki adımda oluşturduğunuz kaynak grubunda bir IOT hub'ı oluşturmak için kullanın [yeni AzureRmIotHub](https://docs.microsoft.com/powershell/module/AzureRM.IotHub/New-AzureRmIotHub) komutu. Bu örnekte bir **S1** adlı merkez **MyTestIoTHub** içinde **Doğu ABD** bölgesi:

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

IOT hub'ı adı genel olarak benzersiz olmalıdır.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Tüm IOT hub'larını kullanarak abonelik listeleyebilirsiniz [Get-AzureRmIotHub](https://docs.microsoft.com/powershell/module/AzureRM.IotHub/Get-AzureRmIotHub) komutu:

```powershell
Get-AzureRmIotHub
```

Bu örnek, önceki adımda oluşturduğunuz S1 standart IOT Hub gösterir. 

IOT hub'ı kullanarak silebilirsiniz [Remove-AzureRmIotHub](https://docs.microsoft.com/powershell/module/azurerm.iothub/remove-azurermiothub) komutu:

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternatif olarak, bir kaynak grubu kaldırabilirsiniz ve tüm kaynakları kullanarak içeren [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/AzureRM.Resources/Remove-AzureRmResourceGroup) komutu:

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi daha fazlasını keşfetmek istiyorsanız aşağıdaki makaleye göz atın, bir PowerShell cmdlet'ini kullanarak bir IOT hub'a dağıtmış:

* [IOT hub ile çalışmaya yönelik PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.iothub/).

* [IOT hub'ı kaynak sağlayıcısı REST API'si](https://docs.microsoft.com/rest/api/iothub/iothubresource).

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)