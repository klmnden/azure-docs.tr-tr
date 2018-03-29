---
title: İç yük dengeleyicisi oluşturma - Azure şablonu | Microsoft Docs
description: Resource Manager’da şablon kullanarak iç yük dengeleyici oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: 64150862-6ced-42de-85dc-89d323257d7c
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: f92430cad52edc60a0ab69caec61cc6f20515f61
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="create-an-internal-load-balancer-using-a-template"></a>Şablon kullanarak iç yük dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)


[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Tıklayarak dağıtma kullanarak şablonu dağıtma

Genel depoda yer alan örnek şablonda, yukarıdaki senaryoyu oluşturmak için kullanılan varsayılan değerleri içeren parametre dosyası kullanılmaktadır. Tıklayarak dağıtma kullanarak bu şablonu dağıtmak için [bu bağlantıya](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer) gidin, **Azure’a dağıt**’a tıklayın, gerekirse varsayılan parametreleri değiştirin ve portaldaki talimatları uygulayın.

## <a name="deploy-the-template-by-using-powershell"></a>PowerShell kullanarak şablonu dağıtma

PowerShell kullanarak yüklediğiniz şablonu dağıtmak için aşağıdaki adımları izleyin.

1. Azure PowerShell’i hiç kullanmadıysanız bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview) ve Azure'a giriş yapıp aboneliğinizi seçene kadar da tüm bu süreç boyunca tüm talimatları uygulayın.
2. Parametre dosyasını yerel diskinize indirin.
3. Dosyayı düzenleyin ve kaydedin.
4. Şablonu kullanarak kaynak grubu oluşturmak için **New-AzureRmResourceGroupDeployment** cmdlet’ini çalıştırın.

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Azure CLI kullanarak şablonu dağıtma

Azure CLI’yi kullanarak şablonu dağıtmak için aşağıdaki adımları uygulayın.

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Resource Manager moduna geçmek için **azure config mode** komutunu aşağıda gösterildiği gibi çalıştırın.

    ```azurecli
    azure config mode arm
    ```

    Yukarıdaki komut için beklenen çıkış buradaki gibidir:

        info:    New mode is arm

3. Parametre dosyasını açın, içeriğini seçin ve bilgisayarınızdaki bir dosyaya kaydedin. Bu örnekte parametre dosyasını *parameters.json* dosyasına kaydettik.
4. Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni iç yük dengeleyiciyi dağıtmak için, **azure group deployment create** komutunu çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Kaynak IP benzeşimi kullanarak yük dengeleyici dağıtım modunu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)

