---
title: Şablon (PowerShell) kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: PowerShell ile bir IOT hub'ı oluşturmak için bir Azure Resource Manager şablonu kullanma
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: fcc9af9e614b0a1b7977ba18f3147fddab8b7b7d
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045065"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Azure Resource Manager şablonu (PowerShell) kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Azure Resource Manager'ı oluşturma ve Azure IOT hub'ları programlı olarak yönetmek için kullanabilirsiniz. Bu öğreticide PowerShell ile bir IOT hub'ı oluşturmak için bir Azure Resource Manager şablonu kullanmayı gösterir.

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Azure Resource Manager dağıtım modelini incelemektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. <br/>Hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap][lnk-free-trial] oluşturabilirsiniz.
* [Azure PowerShell 1.0] [ lnk-powershell-install] veya üzeri.

> [!TIP]
> Makaleyi [Azure PowerShell kullanarak Azure Resource Manager ile] [ lnk-powershell-arm] Azure kaynakları oluşturmak için PowerShell ve Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi sağlar.

## <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma

PowerShell komut isteminde, Azure aboneliğinizde oturum açmak için aşağıdaki komutu girin:

```powershell
Connect-AzAccount
```

Birden çok Azure aboneliğiniz varsa Azure'da oturum açma, kimlik bilgilerinizle ilişkili tüm Azure abonelikleri erişim verir. Azure aboneliklerini kullanmak için size sunulan listelemek için aşağıdaki komutu kullanın:

```powershell
Get-AzSubscription
```

IoT hub’ınızı oluşturmak için komutları çalıştırmak amacıyla kullanmak istediğiniz aboneliği seçmek üzere aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

```powershell
Select-AzSubscription `
    -SubscriptionName "{your subscription name}"
```

IOT hub'ı ve şu anda desteklenen API sürümlerinden dağıtabileceğiniz bulmak için aşağıdaki komutları kullanabilirsiniz:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

IOT hub'ın desteklenen konumlardan birinde aşağıdaki komutu kullanarak IOT hub'ınıza içerecek bir kaynak grubu oluşturun. Bu örnek adlı bir kaynak grubu oluşturur **MyIoTRG1**:

```powershell
New-AzResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a>IOT hub'ı oluşturmak için bir şablon gönderme

Kaynak grubunuzda bir IOT hub'ı oluşturmak için JSON şablonunu kullanın. Var olan bir IOT hub'ına değişiklik yapmak için bir Azure Resource Manager şablonunu da kullanabilirsiniz.

1. Adlı bir Azure Resource Manager şablonu oluşturmak için bir metin düzenleyicisi kullanın **template.json** yeni bir standart IOT hub'ı oluşturmak için aşağıdaki kaynak tanımına sahip. Bu örnek, IOT Hub'ında ekler **Doğu ABD** bölge, iki tüketici grupları oluşturur (**cg1** ve **cg2**) kullanır ve Event Hub ile uyumlu uç nokta  **2016-02-03** API sürümü. Bu şablon aynı zamanda IOT hub'ı adında bir parametre olarak geçirmenize bekliyor olarak adlandırılan **hubName**. IOT hub'ı destekleyen konumları güncel listesi için bkz. [Azure durumu][lnk-status].

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

2. Azure Resource Manager şablonu dosyasını yerel makinenize kaydedin. Bu örnek kaydetmeniz adlı bir klasöre varsayar **c:\templates**.

3. IOT hub'ınızın adını bir parametre olarak geçirerek, yeni IOT hub ' ınızı dağıtmak için aşağıdaki komutu çalıştırın. Bu örnekte, IOT hub'ı adıdır `abcmyiothub`. IOT hub'ınızın adını genel olarak benzersiz olması gerekir:

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. IOT hub'ının oluşturduğunuz anahtarları çıktıyı görüntüler.

5. Uygulamanızı yeni IOT hub'ı doğrulamak için ziyaret [Azure portalında] [ lnk-azure-portal] ve kaynakların listesini görüntüleyin. Alternatif olarak, **Get-AzResource** PowerShell cmdlet'i.

> [!NOTE]
> Bu örnek uygulama, bir S1 standart IOT için faturalandırılırsınız Hub ekler. IOT hub'ı aracılığıyla silebilirsiniz [Azure portalında] [ lnk-azure-portal] kullanarak veya **Remove-AzResource** işiniz bittiğinde PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell ile bir Azure Resource Manager şablonu kullanarak bir IOT hub'ı dağıttığınız artık daha iyi keşfedilebilmesi isteyebilirsiniz:

* Özellikleri hakkında okuyun [IOT hub'ı kaynak sağlayıcısı REST API'si][lnk-rest-api].
* Okuma [Azure Resource Manager'a genel bakış] [ lnk-azure-rm-overview] Azure Resource Manager'ın özellikleri hakkında daha fazla bilgi edinmek için.
* JSON söz dizimi ve özelliklerini şablonlarında kullanmak üzere bkz [Microsoft.Devices kaynak türleri](/azure/templates/microsoft.devices/iothub-allversions).

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [C SDK'ya giriş][lnk-c-sdk]
* [Azure IOT SDK'ları][lnk-sdks]

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/manage-resources-powershell.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
