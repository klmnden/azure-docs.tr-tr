---
title: "PowerShell cmdlet'ini kullanarak Azure IOT Hub oluşturma | Microsoft Docs"
description: "Nasıl bir IOT hub'ı oluşturmak için bir PowerShell cmdlet'ini kullanın."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 02227adeb8a9a7463506efa44ddc2977f8aae65a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-iot-hub-using-the-new-azurermiothub-cmdlet"></a>Yeni AzureRmIotHub cmdlet'ini kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Giriş

Oluşturma ve Azure IOT hub'ları yönetmek için Azure PowerShell cmdlet'lerini kullanabilirsiniz. Bu öğreticide PowerShell ile bir IOT hub oluşturulacağını gösterir.

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Azure Resource Manager dağıtım modeli kullanılarak yer almaktadır.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. <br/>Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure PowerShell cmdlet'leri][lnk-powershell-install].

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma
Bir PowerShell komut isteminde, Azure aboneliğinizde oturum açmak için aşağıdaki komutu girin:

```powershell
Login-AzureRmAccount
```

Birden çok Azure aboneliğiniz varsa, Azure'da oturum açma kimlik bilgileriyle ilişkili tüm Azure abonelikleri için size erişim verir. Azure aboneliklerini kullanabilmeniz için kullanılabilir listelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzureRMSubscription
```

IOT hub'ınızı oluşturması için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komut çıktısı abonelik adı veya kimliği kullanabilirsiniz:

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

IOT hub'ı dağıtmak için bir kaynak grubu gerekir. Varolan bir kaynak grubunu kullanın veya yeni bir tane oluşturun.

Burada, IOT hub'ı dağıtabilirsiniz konumları bulmak için aşağıdaki komutu kullanın:

```powershell
((Get-AzureRmResourceProvider `
  -ProviderNamespace Microsoft.Devices).ResourceTypes `
  | Where-Object ResourceTypeName -eq IoTHubs).Locations
```

IOT hub'ın desteklenen konumlardan birinde IOT hub'ınız için bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın. Bu örnek adlı bir kaynak grubu oluşturur **MyIoTRG1** içinde **Doğu ABD** bölge:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

IOT hub'ı önceki adımda oluşturduğunuz kaynak grubu oluşturmak için aşağıdaki komutu kullanın. Bu örnekte bir **S1** adlı hub **MyTestIoTHub** içinde **Doğu ABD** bölge:

```powershell
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

IOT hub'ın adı benzersiz olmalıdır.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


Tüm IOT hub'ları aşağıdaki komutu kullanarak, aboneliğinizde listeleyebilirsiniz:

```powershell
Get-AzureRmIotHub
```

Önceki örnekte S1 standart IOT için faturalandırılır hub'ı ekler. IOT hub'ı aşağıdaki komutu kullanarak silebilirsiniz:

```powershell
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternatif olarak, bir kaynak grubu ve aşağıdaki komutu kullanarak içerdiği tüm kaynaklar kaldırabilirsiniz:

```powershell
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Sonraki adımlar

PowerShell cmdlet'ini kullanarak bir IOT hub dağıttığınız artık daha ayrıntılı incelemek isteyebilirsiniz:

* Diğer Bul [IOT hub ile çalışmak için PowerShell cmdlet'leri][lnk-iothub-cmdlets].
* Özelliklerinin tamamı hakkında okuyun [IOT hub'ı kaynak sağlayıcısı REST API][lnk-rest-api].

IOT Hub için geliştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [C SDK Giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [Bir cihaz IOT Edge benzetimini yapma][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-iothub-cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.iothub/
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
