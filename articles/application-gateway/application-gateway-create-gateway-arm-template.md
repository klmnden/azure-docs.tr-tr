
<properties
   pageTitle="Azure Resource Manager şablonlarını kullanarak uygulama ağ geçidi oluşturma| Microsoft Azure"
   description="Bu sayfa, Azure Resource Manager şablonunu kullanarak, Azure uygulama ağ geçidi oluşturma yönergelerini verir."
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="joaoma"/>


# Azure Resource Manager şablonunu kullanarak uygulama ağ geçidi oluşturma

Azure Application Gateway, bir katman 7 yük dengeleyicidir. Bulutta veya şirket içinde olmalarından bağımsız olarak, farklı sunucular arasında yük devretme ile HTTP istekleri için performans amaçlı yönlendirme sağlar. Application Gateway şu uygulama teslim özelliklerine sahiptir: HTTP yük dengeleme, tanımlama bilgisi tabanlı oturum benzeşimi ve Güvenli Yuva Katmanı (SSL) yük boşaltımı.

> [AZURE.SELECTOR]
- [Azure Klasik PowerShell](application-gateway-create-gateway.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure Resource Manager şablonu](application-gateway-create-gateway-arm-template.md)

<BR>

GitHub’dan mevcut bir Azure Resource Manager şablonu indirip değiştirmeyi ve şablonu GitHub, PowerShell ve Azure CLI’dan dağıtmayı öğreneceksiniz.

Azure Resource Manager şablonunu hiçbir değişiklik yapmadan doğrudan GitHub'dan dağıtıyorsanız, GitHub'dan şablon dağıtma bölümüne atlayın.


## Senaryo

Bu senaryoda:

- İki örnekli bir uygulama ağ geçidi oluşturacaksınız.
- Ayrılmış 10.0.0.0/16 CIDR bloğu olan, VirtualNetwork1 adlı bir sanal ağ oluşturacaksınız.
- Appgatewaysubnet adlı, CIDR bloğu olarak 10.0.0.0/28 kullanan bir alt ağ oluşturacaksınız.
- Trafik yük dengelemesi yapmak istediğiniz web sunucuları için, önceden yapılandırılmış iki arka uç IP’si ayarlayacaksınız. Bu şablon örneğinde arka uç IP’leri 10.0.1.10 ve 10.0.1.11.olacak.

>[AZURE.NOTE] Bunlar, bu şablonun parametreleridir. Şablonu özelleştirmek için kuralları, dinleyiciyi ve azuredeploy.json’u açan SSL’yi değiştirebilirsiniz.



![Senaryo](./media/application-gateway-create-gateway-arm-template/scenario-arm.png)



## Azure Resource Manager şablonu indirme ve anlama

GitHub’dan sanal ağ ve iki adet alt ağ oluşturmak için, mevcut Azure Resource Manager şablonunu indirebilir, istediğiniz değişikliği yapabilir ve yeniden kullanabilirsiniz. Bunun için aşağıdaki adımları uygulayın:

1. [Application Gateway Oluştur](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-create) bağlantısına gidin.
2. Önce **azuredeploy.json**, sonra da **RAW**’a tıklayın.
3. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
4. Eğer Azure Resource Manager şablonları hakkında bilginiz varsa, 7. adıma atlayın.
5. Kaydettiğiniz dosyayı açın ve 5. satırdaki **parametreler** altındaki içeriğe bakın. Azure Resource Manager şablonu parametreleri, dağıtım sırasında doldurulabilecek değerler için yer tutucu sağlar.

  	| Parametre | Açıklama |
  	|---|---|
  	| **location** | Uygulama ağ geçidinin oluşturulacağı Azure bölgesi |
  	| **VirtualNetwork1** | Yeni sanal ağın adı |
  	| **addressPrefix** | Sanal ağ için CIDR biçiminde adres alanı |
  	| **ApplicationGatewaysubnet** | Uygulama ağ geçidi alt ağının adı |
  	| **subnetPrefix** | Uygulama ağ geçidi alt ağının CIDR bloğu |
  	| **skuname** | SKU örnek boyutu |
  	| **capacity** | Örnek sayısı |
  	| **backendaddress1** | İlk web sunucusunun IP adresi |
  	| **backendaddress2** | İkinci web sunucusunun IP adresi |


>[AZURE.IMPORTANT] GitHub’da tutulan Azure Resource Manager şablonları zaman içinde değişebilir. Kullanmadan önce şablonu denetlediğinizden emin olun.

6. **Kaynaklar** altındaki içeriği denetleyin ve aşağıdakilere dikkat edin:

    - **type**. Şablon tarafından oluşturulan kaynak türü. Burada tür, uygulama ağ geçidini temsil eden **Microsoft.Network/applicationGateways**.
    - **name**. Kaynağın adı. **[parameters('applicationGatewayName')]** kullanıldığına dikkat edin. Bu, adın kullanıcı tarafından girilerek veya dağıtım sırasında bir parametre dosyasıyla belirtileceği anlamına gelir.
    - **properties**. Kaynak özelliklerinin listesi. Bu şablon, uygulama ağ geçidi oluştururken sanal ağı ve genel IP adresini kullanır.

7. https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create/ bağlantısına geri gidin.
8. Önce **azuredeploy-paremeters.json**, sonra da**RAW**’a tıklayın.
9. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
10. Kaydettiğiniz dosyayı açın ve parametre değerlerini düzenleyin. Senaryomuzda açıklanan uygulama ağ geçidini dağıtmak için aşağıdaki değerleri kullanın.

        {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        {
        "location" : {
        "value" : "West US"
        },
        "addressPrefix": {
        "value": "10.0.0.0/16"
        },
        "subnetPrefix": {
        "value": "10.0.0.0/24"
        },
        "skuName": {
        "value": "Standard_Small"
        },
        "capacity": {
        "value": 2
        },
        "backendIpAddress1": {
        "value": "10.0.1.10"
        },
        "backendIpAddress2": {
        "value": "10.0.1.11"
        }
        }

11. Dosyayı kaydedin. JSON şablonunu ve parametre şablonunu, [JSlint.com](http://www.jslint.com/) gibi çevrimiçi JSON doğrulama araçlarını kullanarak test edebilirsiniz.

## PowerShell kullanarak Azure Resource Manager şablonu dağıtma

Daha önce Azure PowerShell kullanmadıysanız, [Azure PowerShell’i yükleme ve yapılandırma](../powershell-install-configure.md) sayfasına gidin ve Azure’da oturum açıp aboneliğinizi seçmek için talimatları sonuna kadar uygulayın.

### 1. Adım

        Login-AzureRmAccount



### 2. Adım

Hesapla ilişkili abonelikleri kontrol edin.

        get-AzureRmSubscription

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.<BR>

### 3. Adım

Hangi Azure aboneliğinizin kullanılacağını seçin. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### 4. Adım


Gerekirse, **New-AzureResourceGroup** cmdlet’ini kullanarak yeni bir kaynak grubu oluşturun. Aşağıdaki örnekte, Doğu ABD konumunda AppgatewayRG adlı yeni bir kaynak grubu oluşturacaksınız.

     New-AzureRmResourceGroup -Name AppgatewayRG -Location "East US"
        VERBOSE: 5:38:49 PM - Created resource group 'AppgatewayRG' in location 'eastus'


        ResourceGroupName : AppgatewayRG
        Location          : eastus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                     Actions  NotActions
                     =======  ==========
                      *

        ResourceId        : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/AppgatewayRG

Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni sanal ağı dağıtmak için, **New-AzureRmResourceGroupDeployment** cmdlet’ini çalıştırın.

        New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
           -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json

Komut satırı tarafından oluşturulan çıktı şu şekildedir:

        DeploymentName    : testappgatewaydeployment
        ResourceGroupName : appgatewayRG
        ProvisioningState : Succeeded
        Timestamp         : 9/19/2015 1:49:41 AM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                   Name             Type                       Value
                   ===============  =========================  ==========
                   location         String                     East US
                   addressPrefix    String                     10.0.0.0/16
                   subnetPrefix     String                     10.0.0.0/24
                   skuName          String                     Standard_Small
                   capacity         Int                        2
                   backendIpAddress1  String                     10.0.1.10
                   backendIpAddress2  String                     10.0.1.11

        Outputs           :


## Azure CLI kullanarak Azure Resource Manager şablonu dağıtma

Azure CLI kullanarak indirdiğiniz Azure Resource Manager şablonunu dağıtmak için aşağıdaki adımları uygulayın:

1. Daha önce Azure CLI kullanmadıysanız, [Azure CLI yükleme ve yapılandırma](../xplat-cli-install.md) sayfasına gidin ve Azure hesabınızı ve aboneliğinizi seçene kadar talimatları uygulayın.
2. Resource Manager moduna geçmek için **azure config mode** komutunu aşağıda gösterildiği gibi çalıştırın.

        azure config mode arm

Yukarıdaki komut için beklenen çıktı şu şekildedir:

        info:   New mode is arm

3. Gerekirse, yeni bir kaynak grubu oluşturmak için **azure group create** komutunu aşağıda gösterildiği gibi çalıştırın. Komutun çıktısına dikkat edin. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a genel bakış](../resource-group-overview.md) sayfasını ziyaret edin.

        azure group create -n appgatewayRG -l eastus

**-n (veya --name)**. Yeni kaynak grubunun adı. Senaryomuz için bu ad, *appgatewayRG*.

**-l (veya --location)**. Yeni kaynak grubunun oluşturulacağı Azure bölgesi. Senaryomuz için bu bölge, *eastus*.

4. Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni sanal ağı dağıtmak için, **azure group deployment create** cmdlet’ini çalıştırın. Çıktıdan sonra gösterilen listede, kullanılan parametreler açıklanmaktadır.

        azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

Yukarıdaki komut için beklenen çıktı şu şekildedir:

        azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestAppgatewayDeployment"
        + Waiting for deployment to complete
        data:    DeploymentName     : TestAppgatewayDeployment
        data:    ResourceGroupName  : appgatewayRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-09-21T20:50:27.5129912Z
        data:    Mode               : Incremental
        data:    Name               Type    Value
        data:    -----------------  ------  --------------
        data:    location           String  East US
        data:    addressPrefix      String  10.0.0.0/16
        data:    subnetPrefix       String  10.0.0.0/24
        data:    skuName            String  Standard_Small
        data:    capacity           Int     2
        data:    backendIpAddress1  String  10.0.1.10
        data:    backendIpAddress2  String  10.0.1.11
        info:    group deployment create command OK

**-g (veya --resource-group)**. Yeni sanal ağın oluşturulacağı kaynak grubunun adı.

**-f (veya --template-file)**. Azure Resource Manager şablonu dosyanızın yolu.

**-e (veya --parameters-file)**. Azure Resource Manager parametreleri dosyanızın yolu.

## Dağıtmak için tıkla özelliğini kullanarak Azure Resource Manager şablonu dağıtma

Dağıtmak için tıkla, Azure Resource Manager şablonlarını kullanmanın başka bir yoludur. Kolay bir Azure portalıyla şablonları kullanma yoludur.


### 1. Adım
[Genel IP ile uygulama ağ geçidi oluşturma](https://azure.microsoft.com/documentation/templates/101-application-gateway-public-ip/) sayfasına gidin.


### 2. Adım

**Azure’a Dağıt**’a tıklayın.

![Azure’a Dağıt](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)

### 3. Adım

Portalda, dağıtım şablonu parametrelerini doldurun ve **Tamam**’a tıklayın.

![Parametreler](./media/application-gateway-create-gateway-arm-template/ibiza1.png)

### 4. Adım

**Yasal koşullar**’ı seçin ve **Satın Al**’a tıklayın.

### 5. Adım

Özel dağıtım dikey penceresinde **Oluştur**’a tıklayın.



## Sonraki adımlar

SSL yük boşaltımı yapılandırmak istiyorsanız, bkz. [SSL yük boşaltımı için uygulama ağ geçidi yapılandırma ](application-gateway-ssl.md).

İç yük dengeleyiciyle kullanacağınız bir uygulama ağ geçidi yapılandırmak istiyorsanız, bkz. [İç yük dengeleyici (ILB) ile uygulama ağ geçidi oluşturma](application-gateway-ilb.md).

Yük dengeleme seçenekleri hakkında daha fazla genel bilgi edinmek istiyorsanız, bkz.

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!----HONumber=Jun16_HO2-->


