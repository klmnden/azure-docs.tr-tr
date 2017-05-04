---
title: "İnternet’e yönelik yük dengeleyicisi oluşturma - Azure şablonu | Microsoft Docs"
description: "Şablon kullanarak Resource Manager’da İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: b24f4729-4559-4458-8527-71009d242647
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: f30a450c7d3aa73bb2fb196ca7de94436916bed0
ms.lasthandoff: 04/27/2017

---

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Şablon kullanarak İnternet’e yönelik yük dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. [Klasik dağıtım modeli kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-classic-portal.md) sayfasını da inceleyebilirsiniz.

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

    Yukarıdaki komut için beklenen çıktı şu şekildedir:

        info:    New mode is arm

3. Tarayıcınızdan [Hızlı Başlangıç Şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)’na gidin, json dosyasının içeriğini kopyalayın ve bilgisayarınızda yeni bir dosyaya yapıştırın. Bu senaryo için değerleri **c:\lb\azuredeploy.parameters.json** adlı bir dosyaya kopyalayacaksınız.
4. Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni yük dengeleyiciyi dağıtmak için, **azure group deployment create** cmdlet’ini çalıştırın. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır.

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)

