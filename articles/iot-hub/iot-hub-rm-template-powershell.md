---
title: Şablon (PowerShell) kullanarak Azure IOT Hub oluşturma | Microsoft Docs
description: Azure PowerShell ile bir IOT hub'ı oluşturmak için bir Azure Resource Manager şablonu kullanma
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: d23d3824c477d3bba4e4900bee355376f1317f92
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59609191"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Azure Resource Manager şablonu (PowerShell) kullanarak IOT hub oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bir Azure Resource Manager şablonu kullanarak IOT Hub ile bir tüketici grubu oluşturmak için kullanmayı öğrenin. Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Resource Manager şablonları geliştirme hakkında daha fazla bilgi için bkz. [Azure Resource Manager belgelerini](https://docs.microsoft.com/azure/azure-resource-manager/).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Bu hızlı başlangıçta kullanılan Resource Manager şablonu dandır [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/101-iothub-with-consumergroup-create/). Şablonun bir kopyasını şu şekildedir:

[!code-json[iothub-creation](~/quickstart-templates/101-iothub-with-consumergroup-create/azuredeploy.json)]

Şablon, bir Azure IOT hub üç uç noktaları (eventhub, bulut-cihaz ve mesajlaşma) ve bir tüketici grubu oluşturur. Daha fazla şablon örnekleri için bkz [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Devices&pageNumber=1&sort=Popular). IOT hub'ı şablon şeması bulunabilir [burada](https://docs.microsoft.com/azure/templates/microsoft.devices/iothub-allversions).

Bir şablonu dağıtmak için çeşitli yöntemler vardır.  Bu öğreticide Azure PowerShell kullanırsınız.

PowerShell betiğini çalıştırmak için seçin **deneyin** Azure Cloud Shell'i açmak için. Betik yapıştırmak için kabuk sağ tıklayın ve ardından Yapıştır seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$iotHubName = Read-Host -Prompt "Enter the IoT Hub name"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-iothub-with-consumergroup-create/azuredeploy.json" `
    -iotHubName $iotHubName
```

PowerShell betiğini görebileceğiniz gibi Azure hızlı başlangıç şablonlarından kullanılan şablon budur. Kendi kullanmak için ilk şablon dosyasını Cloud shell'e yüklemeniz ve sonra kullanmak gereken `-TemplateFile` geçme dosya adını belirtin.  Bir örnek için bkz. [şablonu dağıtmak](../azure-resource-manager/resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell#deploy-the-template).

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure Resource Manager şablonu kullanarak bir IOT hub'ı dağıttıktan sonra daha iyi keşfedilebilmesi isteyebilirsiniz:

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
