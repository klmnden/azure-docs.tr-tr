---
title: Azure Powershell'den IOT Central'ı yönetme | Microsoft Docs
description: IOT Central, Azure Powershell'den yönetin.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/14/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 04726249809656344c8f81164d5deed5af321e71
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355760"
---
# <a name="manage-iot-central-from-azure-powershell"></a>Azure Powershell'den IOT Central'ı yönetme

Oluşturma ve IOT Central'dan IOT Central uygulamaları yönetmek yerine [Uygulama Yöneticisi](https://aka.ms/iotcentral) kullanabileceğiniz sayfasında [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) uygulamalarınızı yönetmek için.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Azure PowerShell'i yerel makinenizde çalıştırmak isterseniz, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). Azure PowerShell'i yerel olarak çalıştırdığınızda kullanmak **Connect AzAccount** cmdlet'ini, bu makalede yer alan cmdlet'ler çalışmadan önce Azure'da oturum açın.

## <a name="install-the-iot-central-module"></a>IOT Central modülünü yükleme

Denetlemek için aşağıdaki komutu çalıştırın [IOT Central Modülü](https://docs.microsoft.com/powershell/module/az.iotcentral/) PowerShell ortamınızda yüklü:

```PowerShell
Get-InstalledModule -name Az.I*
```

Yüklü modülleri listesini içermiyorsa **Az.IotCentral**, aşağıdaki komutu çalıştırın:

```PowerShell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>Uygulama oluşturma

Kullanım [yeni AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/New-AzIotCentralApp) Azure aboneliğinizde bir IOT Central uygulaması oluşturmak için cmdlet'i. Örneğin:

```PowerShell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```PowerShell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "S1" -Template "iotc-demo@1.0.0" `
  -DisplayName "My Custom Display Name"
```

Betik, Doğu ABD bölgesinde uygulama için önce bir kaynak grubu oluşturur. Kullanılan parametreler aşağıdaki tabloda açıklanmıştır **yeni AzIotCentralApp** komutu:

|Parametre         |Açıklama |
|------------------|------------|
|ResourceGroupName |Uygulamayı içeren kaynak grubu. Bu kaynak grubunun aboneliğinizde zaten mevcut olmalıdır. |
|Konum |Varsayılan olarak, bu cmdlet, kaynak grubu konumu kullanır. Şu anda IOT Central uygulamada oluşturabilirsiniz **Doğu ABD**, **Batı ABD**, **Kuzey Avrupa**, veya **Batı Avrupa** bölgeleri. |
|Ad              |Azure portalında uygulama adı. |
|Al etki alanı         |Uygulama URL'sini alt etki alanı. Örnekte, uygulama URL'si olan https://mysubdomain.azureiotcentral.com. |
|Sku               |Şu anda yalnızca bir bölüm değerdir **S1** (standart katman). Bkz: [Azure IOT Central fiyatlandırma](https://azure.microsoft.com/pricing/details/iot-central/). |
|Şablon          | Kullanılacak uygulama şablonu. Daha fazla bilgi için aşağıdaki tabloya bakın: |
|DisplayName       |Uygulamanın kullanıcı Arabiriminde görüntülenen adı. |

**Uygulama Şablonları**

|Şablon adı  |Açıklama |
|---------------|------------|
|iotc-default@1.0.0 |Kendi cihaz şablonlarınız ve cihazlarınızla doldurabileceğiniz boş bir uygulama oluşturur. |
|iotc-demo@1.0.0    |Bir Soğutmalı Otomat için önceden oluşturulmuş cihaz şablonunu içeren bir uygulama oluşturur. Azure IoT Central'ı incelemeye başlamak için bu şablonu kullanın. |
|iotc-devkit-sample@1.0.0 |MXChip veya Raspberry Pi cihazını bağlamak amacıyla sizin için hazırlanmış cihaz şablonlarıyla bir uygulama oluşturur. Cihaz geliştiricisiyseniz tüm bu cihazların denemeler bu şablonu kullanın. |

## <a name="view-your-iot-central-applications"></a>IOT Central uygulamalarınızı görüntüleyin

Kullanım [Get-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Get-AzIotCentralApp) cmdlet'ini IOT Central uygulamalarınızın listelenmesi ve meta verileri görüntüleyin.

## <a name="modify-an-application"></a>Bir uygulamayı değiştirme

Kullanım [kümesi AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/set-aziotcentralapp) IOT Central uygulamasına meta verilerini güncelleştirmek için cmdlet. Örneğin, uygulamanızın görünen adını değiştirmek için şunu yazın:

```PowerShell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>Uygulamayı kaldırma

Kullanım [Remove-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Remove-AzIotCentralApp) IOT Central uygulamasına silmeye yönelik cmdlet'i. Örneğin:

```PowerShell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Powershell'den Azure IOT Central uygulamaları yönetmek öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)
