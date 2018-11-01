---
title: Herkese açık yük dengeleyici oluşturma - Azure şablonu | Microsoft Docs
description: Şablon kullanarak Resource Manager’da herkese açık bir yük dengeleyici oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 0ca988fba91cd1d4a97ca376a6f3ff45fae214ac
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418985"
---
# <a name="creating-a-public-load-balancer-using-a-template"></a>Şablon CLI kullanarak herkese açık bir yük dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)



[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Tıklayarak dağıtma kullanarak şablonu dağıtma

Genel depoda yer alan örnek şablonda, yukarıdaki senaryoyu oluşturmak için kullanılan varsayılan değerleri içeren parametre dosyası kullanılmaktadır. Tıklayarak dağıtma kullanarak bu şablonu dağıtmak için [bu bağlantıya](http://go.microsoft.com/fwlink/?LinkId=544801) gidin, **Azure’a dağıt**’a tıklayın, gerekirse varsayılan parametreleri değiştirin ve portaldaki talimatları uygulayın.

## <a name="deploy-the-template-by-using-powershell"></a>PowerShell kullanarak şablonu dağıtma

PowerShell kullanarak yüklediğiniz şablonu dağıtmak için aşağıdaki adımları izleyin.

1. Daha önce Azure PowerShell kullanmadıysanız, [Azure PowerShell’i Yükleme ve Yapılandırma](/powershell/azure/overview) sayfasına gidin ve Azure’da oturum açıp aboneliğinizi seçmek için talimatları sonuna kadar uygulayın.
2. Şablonu kullanarak kaynak grubu oluşturmak için **New-AzureRmResourceGroupDeployment** cmdlet’ini çalıştırın.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
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

3. Tarayıcınızdan [Hızlı Başlangıç Şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)’na gidin, json dosyasının içeriğini kopyalayın ve bilgisayarınızda yeni bir dosyaya yapıştırın. Bu senaryo için değerleri **c:\lb\azuredeploy.parameters.json** adlı bir dosyaya kopyalayacaksınız.
4. Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni yük dengeleyiciyi dağıtmak için, **azure group deployment create** cmdlet’ini çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
