---
title: Bir şablon (PowerShell) kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: PowerShell ile bir IOT hub'ı oluşturmak için bir Azure Resource Manager şablonu kullanma
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 7eade855-c289-4ffb-b5ef-02be8c5f670f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 0e5f95d98f772b226e162f601939bc94bf8fb78b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Azure Resource Manager şablonu (PowerShell) kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager oluşturmak ve Azure IOT hub'ları programlı olarak yönetmek için kullanabilirsiniz. Bu öğretici bir Azure Resource Manager şablonu PowerShell ile bir IOT hub'ı oluşturmak için nasıl kullanılacağını gösterir.

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Azure Resource Manager dağıtım modeli kullanılarak yer almaktadır.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. <br/>Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure PowerShell 1.0] [ lnk-powershell-install] veya sonraki bir sürümü.

> [!TIP]
> Makaleyi [Azure PowerShell kullanarak Azure Resource Manager ile] [ lnk-powershell-arm] Azure kaynaklarını oluşturmak için PowerShell ve Azure Resource Manager şablonları kullanma hakkında daha fazla bilgi sağlar.

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

Bir PowerShell komut isteminde, Azure aboneliğinizde oturum açmak için aşağıdaki komutu girin:

```powershell
Connect-AzureRmAccount
```

Birden çok Azure aboneliğiniz varsa, Azure'da oturum açma kimlik bilgileriyle ilişkili tüm Azure abonelikleri için size erişim verir. Azure aboneliklerini kullanabilmeniz için kullanılabilir listelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzureRMSubscription
```

IoT hub’ınızı oluşturmak için komutları çalıştırmak amacıyla kullanmak istediğiniz aboneliği seçmek üzere aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

```powershell
Select-AzureRMSubscription `
    -SubscriptionName "{your subscription name}"
```

Burada bir IOT hub ve şu anda desteklenen API sürümleri dağıtabilirsiniz bulmak için aşağıdaki komutları kullanın:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

IOT hub'ın desteklenen konumlardan birinde aşağıdaki komutu kullanarak IOT hub'ınızı içerecek şekilde bir kaynak grubu oluşturun. Bu örnek adlı bir kaynak grubu oluşturur **MyIoTRG1**:

```powershell
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a>IOT hub'ı oluşturmak için bir şablon gönderin

IOT hub'ı, kaynak grubu oluşturmak için JSON şablonunu kullanın. Bir Azure Resource Manager şablonu, var olan bir IOT hub'ına değişiklik yapmak için de kullanabilirsiniz.

1. Adlı bir Azure Resource Manager şablonu oluşturmak için bir metin düzenleyicisi kullanın **template.json** yeni bir standart IOT hub oluşturmak için aşağıdaki kaynak tanımı'yla. Bu örnek, IOT hub'ı ekler **Doğu ABD** bölge, iki tüketici grupları oluşturur (**cg1** ve **cg2**) kullanır ve Event Hub ile uyumlu uç nokta **2016-02-03** API sürümü. Bu şablon, IOT hub'adında bir parametre olarak geçirmek için aynı zamanda bekliyor olarak adlandırılan **hubName**. IOT hub'ı destekleyen konumları geçerli listesi için bkz [Azure durumu][lnk-status].

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Yerel makinenizde Azure Resource Manager şablonu dosyasını kaydedin. Bu örnek kaydettiğiniz bu adlı bir klasöre varsayar **c:\templates**.

3. IOT hub'ınızın adını parametre olarak geçirme, yeni IOT hub ' ınızı dağıtmak için aşağıdaki komutu çalıştırın. Bu örnekte, IOT hub'ı adıdır `abcmyiothub`. IOT hub'ınızı adı genel olarak benzersiz olması gerekir:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
  [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. Çıktıyı tuşları oluşturduğunuz IOT hub'ı görüntüler.

5. Uygulamanızı eklenen yeni IOT hub'ı doğrulamak için ziyaret edin [Azure portal] [ lnk-azure-portal] ve kaynakları listesini görüntüleyin. Alternatif olarak, kullanın **Get-AzureRmResource** PowerShell cmdlet'i.

> [!NOTE]
> Bu örnek uygulama S1 standart IOT için faturalandırılır hub'ı ekler. IOT hub'ı aracılığıyla silebilirsiniz [Azure portal] [ lnk-azure-portal] veya kullanarak **Kaldır AzureRmResource** bitirdiğinizde PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell ile bir Azure Resource Manager şablonu kullanarak bir IOT hub dağıttığınız artık daha ayrıntılı incelemek isteyebilirsiniz:

* Özelliklerinin tamamı hakkında okuyun [IOT hub'ı kaynak sağlayıcısı REST API][lnk-rest-api].
* Okuma [Azure Resource Manager'a genel bakış] [ lnk-azure-rm-overview] Azure Kaynak Yöneticisi'nin özellikleri hakkında daha fazla bilgi edinmek için.

IOT Hub için geliştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [C SDK Giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-azurerm-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
