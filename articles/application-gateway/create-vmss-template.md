---
title: Azure Application Gateway - şablonları oluşturun | Microsoft Docs
description: Bu sayfa, Azure Resource Manager şablonunu kullanarak, Azure uygulama ağ geçidi oluşturma yönergelerini verir.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: victorh
ms.openlocfilehash: 7ff6db5acb150207f975931155386a308c48888b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66134088"
---
# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a>Azure Resource Manager şablonunu kullanarak uygulama ağ geçidi oluşturma

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ve performans yönlendirmeli HTTP istekleri sağlar. Application Gateway; HTTP yük dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi, Güvenli Yuva Katmanı (SSL) boşaltma, özel sistem durumu araştırmaları, çoklu site desteği gibi birçok uygulama teslim denetleyicisi (ADC) özelliği sunar. Desteklenen özelliklerin tam bir listesi için bkz [Application Gateway'e genel bakış](overview.md)

Bu makalede indiriliyor ve var olan bir değiştirme için size [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md) GitHub ve şablonu GitHub, PowerShell ve Azure CLI'yı dağıtma.

Yalnızca şablon herhangi bir değişiklik yapmadan doğrudan github'dan dağıtıyorsanız, github'dan şablon dağıtma bölümüne atlayın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario"></a>Senaryo

Bu senaryoda:

* Application gateway web uygulaması güvenlik duvarıyla oluşturma.
* Ayrılmış 10.0.0.0/16 CIDR bloğu olan, VirtualNetwork1 adlı bir sanal ağ oluşturacaksınız.
* Appgatewaysubnet adlı, CIDR bloğu olarak 10.0.0.0/28 kullanan bir alt ağ oluşturacaksınız.
* Trafik yük dengelemesi yapmak istediğiniz web sunucuları için, önceden yapılandırılmış iki arka uç IP’si ayarlayacaksınız. Bu şablon örneğinde arka uç IP’leri 10.0.1.10 ve 10.0.1.11’dir.

> [!NOTE]
> Bu ayarlar, bu şablonun parametreleridir. Şablonu özelleştirmek için kuralları, dinleyiciyi, SSL ve diğer seçenekleri azuredeploy.json dosyasındaki değiştirebilirsiniz.

![Senaryo](./media/create-vmss-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Azure Resource Manager şablonu indirme ve anlama

GitHub’dan sanal ağ ve iki adet alt ağ oluşturmak için, mevcut Azure Resource Manager şablonunu indirebilir, istediğiniz değişikliği yapabilir ve yeniden kullanabilirsiniz. Bunu yapmak için aşağıdaki adımları kullanın:

1. Gidin [oluşturma Application Gateway web uygulaması Güvenlik Duvarı etkin](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).
1. **azuredeploy.json** ve **RAW** öğelerine sırayla tıklayın.
1. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
1. Eğer Azure Resource Manager şablonları hakkında bilginiz varsa, 7. adıma atlayın.
1. Kaydettiğiniz dosyayı açın ve altındaki içeriğe bakın **parametreleri** satırında
1. Azure Resource Manager şablonu parametreleri, dağıtım sırasında doldurulabilecek değerler için yer tutucu sağlar.

   | Parametre | Açıklama |
   | --- | --- |
   | **subnetPrefix** |Uygulama ağ geçidi alt ağının CIDR bloğu. |
   | **applicationGatewaySize** | Application gateway boyutu.  WAF yalnızca orta ve büyük izin verir. |
   | **backendIpaddress1** |İlk web sunucusunun IP adresi. |
   | **backendIpaddress2** |İkinci web sunucusunun IP adresi. |
   | **wafEnabled** | WAF etkin olup olmadığını belirlemek için ayarlanıyor.|
   | **wafMode** | Web uygulaması güvenlik duvarı modu.  Kullanılabilir seçenekler **önleme** veya **algılama**.|
   | **wafRuleSetType** | WAF kural kümesi türü.  Şu anda desteklenen tek seçenek OWASP aşamasındadır. |
   | **wafRuleSetVersion** |Kural kümesi sürümü. Şu anda desteklenen seçenekler OWASP CRS 2.2.9 ve 3.0 belirtilmiştir. |

1. Altındaki içeriği denetleyin **kaynakları** ve aşağıdaki özelliklere dikkat edin:

   * **type**. Şablon tarafından oluşturulan kaynak türü. Bu durumda, türü, `Microsoft.Network/applicationGateways`, bir uygulama ağ geçidini temsil eder.
   * **name**. Kaynağın adı. Kullanımına dikkat edin `[parameters('applicationGatewayName')]`, adı giriş olarak bir parametre dosyası tarafınızdan girilerek veya dağıtım sırasında sağlanan anlamına gelir.
   * **properties**. Kaynak özelliklerinin listesi. Bu şablon, uygulama ağ geçidi oluştururken sanal ağı ve genel IP adresini kullanır. JSON söz dizimi ve bir uygulama ağ geçidi şablondaki özellikleri için bkz [Microsoft.Network/applicationGateways](/azure/templates/microsoft.network/applicationgateways).

1. Geri gidin [ https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/ ](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).
1. Tıklayın **azuredeploy-parameters.json**ve ardından **ham**.
1. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
1. Kaydettiğiniz dosyayı açın ve parametre değerlerini düzenleyin. Senaryomuzda açıklanan uygulama ağ geçidini dağıtmak için aşağıdaki değerleri kullanın.

     ```json
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
             "addressPrefix": {
             "value": "10.0.0.0/16"
             },
             "subnetPrefix": {
             "value": "10.0.0.0/28"
             },
             "applicationGatewaySize": {
             "value": "WAF_Medium"
             },
             "capacity": {
             "value": 2
             },
             "backendIpAddress1": {
             "value": "10.0.1.10"
             },
             "backendIpAddress2": {
             "value": "10.0.1.11"
             },
             "wafEnabled": {
             "value": true
             },
             "wafMode": {
             "value": "Detection"
             },
             "wafRuleSetType": {
             "value": "OWASP"
             },
             "wafRuleSetVersion": {
             "value": "3.0"
             }
         }
     }
     ```

1. Dosyayı kaydedin. JSON şablonunu ve parametre şablonunu, [JSlint.com](https://www.jslint.com/) gibi çevrimiçi JSON doğrulama araçlarını kullanarak test edebilirsiniz.

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a>PowerShell kullanarak Azure Resource Manager şablonu dağıtma

Azure PowerShell'i hiç kullanmadıysanız, ziyaret edin: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview) ve Azure'da oturum açıp aboneliğinizi seçmek için yönergeleri izleyin.

1. PowerShell oturum açın

    ```powershell
    Login-AzAccount
    ```

1. Hesapla ilişkili abonelikleri kontrol edin.

    ```powershell
    Get-AzSubscription
    ```

    Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.

1. Hangi Azure aboneliğinizin kullanılacağını seçin.

    ```powershell
    Select-AzSubscription -Subscriptionid "GUID of subscription"
    ```

1. Gerekirse, **New-AzureResourceGroup** cmdlet’ini kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnekte Doğu ABD konumunda AppgatewayRG adlı yeni bir kaynak grubu oluşturacaksınız.

    ```powershell
    New-AzResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. Çalıştırma **yeni AzResourceGroupDeployment** indirdiğiniz ve değiştirdiğiniz şablonu ve parametre kullanarak yeni sanal ağı dağıtmak için cmdlet dosyaları.
    
    ```powershell
    New-AzResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a>Azure CLI kullanarak Azure Resource Manager şablonu dağıtma

Azure CLI kullanarak indirdiğiniz Azure Resource Manager şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Daha önce Azure CLI kullanmadıysanız, [Azure CLI yükleme ve yapılandırma](/cli/azure/install-azure-cli) sayfasına gidin ve Azure hesabınızı ve aboneliğinizi seçene kadar talimatları uygulayın.

1. Gerekirse, çalıştırma `az group create` komutunu aşağıdaki kod parçacığında gösterildiği gibi bir kaynak grubu oluşturun. Komutun çıktısına dikkat edin. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md) sayfasını ziyaret edin.

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    **-n (veya --name)** . Yeni kaynak grubunun adı. Senaryomuz için bu ad, *appgatewayRG*.
    
    **-l (veya --location)** . Yeni kaynak grubunun oluşturulduğu Azure bölgesi. Bizim senaryomuz için sahip *westus*.

1. Çalıştırma `az group deployment create` şablonu ve parametre kullanarak yeni sanal ağı dağıtmak için cmdlet indirdiğiniz ve değiştirdiğiniz önceki adımda dosyaları. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a>Dağıtmak için tıkla özelliğini kullanarak Azure Resource Manager şablonu dağıtma

Dağıtmak için tıkla, Azure Resource Manager şablonlarını kullanmanın başka bir yoludur. Kolay bir Azure portalıyla şablonları kullanma yoludur.

1. Git [application gateway web uygulaması güvenlik duvarıyla oluşturma](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).

1. **Azure’a dağıt**’a tıklayın.

    ![Azure’a dağıtma](./media/create-vmss-template/deploytoazure.png)
    
1. Portalda, dağıtım şablonu parametrelerini doldurun ve **Tamam**’a tıklayın.

    ![Parametreler](./media/create-vmss-template/ibiza1.png)
    
1. Seçin **hüküm ve koşulları yukarıda belirtilen kabul ediyorum** tıklatıp **satın alma**.

1. Özel dağıtım dikey penceresinde **Oluştur**’a tıklayın.

## <a name="providing-certificate-data-to-resource-manager-templates"></a>Resource Manager şablonlarının sağlayan sertifika verileri

SSL ile bir şablonu kullanılırken, sertifikayı karşıya yüklenen yerine bir base64 dizesi sağlanması gerekir. Bir .pfx veya base64 dizesi için .cer dönüştürerek aşağıdaki komutlardan birini. Aşağıdaki komutları sertifika şablonu için sağlanan bir base64 dizesine dönüştürün. Beklenen çıktıyı bir değişkende depolanan ve şablonda yapıştırılan bir dizedir.

### <a name="macos"></a>Mac OS
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a>Windows
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a>Tüm kaynakları silme

Bu makalede oluşturulan tüm kaynakları silmek için aşağıdaki adımlardan birini tamamlayın:

### <a name="powershell"></a>PowerShell

```powershell
Remove-AzResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a>Sonraki adımlar

SSL yük boşaltmayı yapılandırmak istiyorsanız, ziyaret edin: [SSL yük boşaltımı için bir uygulama ağ geçidi](tutorial-ssl-cli.md).

Bir iç yük dengeleyiciyle kullanacağınız uygulama ağ geçidi yapılandırmak istiyorsanız, ziyaret edin: [İç yük dengeleyici (ILB) ile bir uygulama ağ geçidi oluşturma](redirect-internal-site-cli.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

