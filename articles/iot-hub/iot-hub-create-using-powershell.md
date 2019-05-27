---
title: Bir PowerShell cmdlet'ini kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: Nasıl bir IOT hub'ı oluşturmak için PowerShell cmdlet'ini kullanın.
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: robinsh
ms.openlocfilehash: fd376728a1ebdf769c7f2ae76d61a60703e13711
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66146593"
---
# <a name="create-an-iot-hub-using-the-new-aziothub-cmdlet"></a>New-AzIotHub cmdlet'ini kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Tanıtım

Oluşturma ve Azure IOT hub'ları yönetmek için Azure PowerShell cmdlet'lerini kullanabilirsiniz. Bu öğretici, IOT hub'ı PowerShell ile oluşturma işlemini göstermektedir.

Bu nasıl yapılır tamamlamak için bir Azure aboneliği gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

Cloud Shell kullanıyorsanız, zaten aboneliğinize oturum açtınız. PowerShell'i yerel olarak bunun yerine çalıştırıyorsanız, Azure aboneliğinizde oturum açmak için aşağıdaki komutu girin:

```powershell
# Log into Azure account.
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Bir IOT hub'ı dağıtmak için bir kaynak grubu gerekir. Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun.

IOT hub'ınız için bir kaynak grubu oluşturmak için kullanın [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.Resources/New-azResourceGroup) komutu. Bu örnek adlı bir kaynak grubu oluşturur **MyIoTRG1** içinde **Doğu ABD** bölgesi:

```azurepowershell-interactive
New-AzResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Önceki adımda oluşturduğunuz kaynak grubunda bir IOT hub'ı oluşturmak için kullanın [yeni AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/New-azIotHub) komutu. Bu örnekte bir **S1** adlı merkez **MyTestIoTHub** içinde **Doğu ABD** bölgesi:

```azurepowershell-interactive
New-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

IOT hub'ı adı genel olarak benzersiz olmalıdır.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Tüm IOT hub'larını kullanarak abonelik listeleyebilirsiniz [Get-AzIotHub](https://docs.microsoft.com/powershell/module/az.IotHub/Get-azIotHub) komutu:

```azurepowershell-interactive
Get-AzIotHub
```

Bu örnek, önceki adımda oluşturduğunuz S1 standart IOT Hub gösterir.

IOT hub'ı kullanarak silebilirsiniz [Remove-AzIotHub](https://docs.microsoft.com/powershell/module/az.iothub/remove-aziothub) komutu:

```azurepowershell-interactive
Remove-AzIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternatif olarak, bir kaynak grubu kaldırabilirsiniz ve tüm kaynakları kullanarak içeren [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.Resources/Remove-azResourceGroup) komutu:

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi daha fazlasını keşfetmek istiyorsanız aşağıdaki makaleye göz atın, bir PowerShell cmdlet'ini kullanarak bir IOT hub'a dağıtmış:

* [IOT hub ile çalışmaya yönelik PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/az.iothub/).

* [IOT hub'ı kaynak sağlayıcısı REST API'si](https://docs.microsoft.com/rest/api/iothub/iothubresource).

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya giriş](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDK’ları](iot-hub-devguide-sdks.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
