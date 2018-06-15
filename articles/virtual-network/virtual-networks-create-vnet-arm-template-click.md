---
title: Bir sanal ağ oluşturma | Azure Resource Manager şablonu | Microsoft Docs
description: Bir Azure Resource Manager şablonu kullanarak bir sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bb096f64a6bc41ad2e75c058c7a9f00bbe480207
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
ms.locfileid: "29880552"
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir sanal ağ oluşturma

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure'un iki dağıtım modeli vardır: Azure Resource Manager ve klasik. Microsoft, kaynakların Resource Manager dağıtım modeliyle oluşturulmasını önerir. İki model arasındaki farkları öğrenmek için [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md) makalesini okuyun.
 
Bu makalede Azure Resource Manager şablonu kullanarak Resource Manager dağıtım modeli üzerinden bir VNet oluşturma açıklanmaktadır. Resource Manager’da farklı araçlar kullanarak da sanal ağ kurabilir veya aşağıdaki listeden farklı bir seçenek belirleyerek klasik dağıtım modeliyle sanal ağ oluşturabilirsiniz:

> [!div class="op_single_selector"]
- [Portal](quick-create-portal.md)
- [PowerShell](quick-create-powershell.md)
- [CLI](quick-create-cli.md)
- [Şablon](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (Klasik)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (Klasik)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (Klasik)](virtual-networks-create-vnet-classic-cli.md)

İndirip değiştirmeyi ve mevcut Azure Resource Manager şablonunu github'dan öğrenin ve şablonu GitHub, PowerShell ve Azure CLI dağıtın.

Azure Resource Manager şablonunu doğrudan github'dan, herhangi bir değişiklik yapılmadan dağıtıyorsanız geçin [github'dan şablon dağıtma](#deploy-the-arm-template-by-using-click-to-deploy).

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Azure Resource Manager şablonu indirme ve anlama
Github'dan VNet ve iki alt ağ oluşturmak için varolan şablonunu indirebilir, istediğiniz ve yeniden tüm değişiklikleri yapın. Bunu yapmak için aşağıdaki adımları tamamlayın:

1. [Örnek şablon sayfasına](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets) gidin.
2. **azuredeploy.json** ve **RAW** öğelerine sırayla tıklayın.
3. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
4. Şablonları hakkında bilginiz varsa 7. adıma geçin.
5. Kaydettiğiniz dosyayı açın ve altındaki içeriğe bakın **parametreleri** satırında 5. Azure Resource Manager şablonu parametreleri, dağıtım sırasında doldurulabilecek değerler için yer tutucu sağlar.
   
   | Parametre | Açıklama |
   | --- | --- |
   | **vnetName** |Yeni VNet'in adı |
   | **addressPrefix** |CIDR biçiminde VNet adres alanı |
   | **subnet1Name** |İlk VNet adı |
   | **subnet1Prefix** |İlk alt ağ için CIDR bloğu |
   | **subnet2Name** |İkinci VNet adı |
   | **subnet2Prefix** |İkinci alt ağ için CIDR bloğu |
   
   > [!IMPORTANT]
   > GitHub’da tutulan Azure Resource Manager şablonları zaman içinde değişebilir. Kullanmadan önce şablonu denetlediğinizden emin olun.
   > 
   > 
6. **Kaynaklar** altındaki içeriği denetleyin ve aşağıdakilere dikkat edin:
   
   * **type**. Şablon tarafından oluşturulan kaynak türü. Bu durumda, bir VNet’i temsil eden **Microsoft.Network/virtualNetworks**.
   * **name**. Kaynağın adı. Kullanımına dikkat edin **[parameters('vnetName')]**, girdi olarak dağıtımı sırasında kullanıcı ya da bir parametre dosyası tarafından sağlanan adı anlamına gelir.
   * **properties**. Kaynak özelliklerinin listesi. Bu şablon, VNet oluşturulduğu sırada adres alanını ve alt ağ özelliklerini kullanır.
7. [Örnek şablon sayfasına](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets) geri gidin.
8. **azuredeploy-parameters.json** ve **RAW** öğelerine sırayla tıklayın.
9. Dosyayı bilgisayarınızdaki yerel bir klasöre kaydedin.
10. Yeni kaydettiğiniz dosyayı açın ve parametre değerlerini düzenleyin. Senaryoda açıklanan Vnet'i dağıtmak için aşağıdaki aşağıdaki değerleri kullanın:

    ```json
        {
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. Dosyayı kaydedin.


## <a name="deploy-the-template-using-powershell"></a>PowerShell kullanarak şablonu dağıtmak

PowerShell kullanarak yüklediğiniz şablonunu dağıtmak için aşağıdaki adımları tamamlayın:

1. PowerShell'i yükleme ve Azure içindeki adımları tamamlayarak yapılandırma [yükleme ve yapılandırma Azure PowerShell](/powershell/azure/overview) makalesi.
2. Yeni bir kaynak grubu oluşturmak için şu komutu çalıştırın:

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    Komut bir kaynak grubu oluşturur *TestRG* içinde *Orta ABD* azure bölgesi. Kaynak grupları hakkında daha fazla bilgi için [Azure Resource Manager’a Genel Bakış](../azure-resource-manager/resource-group-overview.md)’ı ziyaret edin.

    Beklenen çıktı:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni Vnet'i dağıtmak için aşağıdaki komutu çalıştırın:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    Beklenen çıktı:
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. Yeni Vnet'in özelliklerini görüntülemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    Beklenen çıktı:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-the-template-using-click-to-deploy"></a>Dağıtmak için kullanarak şablonu dağıtmak

Microsoft tarafından yönetilen bir GitHub deposuna karşıya önceden tanımlanmış Azure Resource Manager şablonları yeniden kullanmak ve topluluğa açık. Bu şablonlar Github'dan sınırsız dağıtılabilir veya indirdiğiniz ve gereksinimlerinizi karşılayacak biçimde değiştirilebilir. İki alt ağa sahip VNet oluşturan bir şablonu dağıtmak için aşağıdaki adımları tamamlayın:

1. Bir tarayıcıdan [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) adresine gidin.
2. Şablonları listesini kaydırın ve **101-vnet-two-subnets** öğesine tıklayın. Aşağıda gösterilen gibi **README.md** dosyası gözden geçirin.

    ![Github’da READEME.md dosyası](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. **Azure’a dağıt**’a tıklayın. Gerekiyorsa, Azure oturum açma kimlik bilgilerinizi girin. 
4. **Parametreler** dikey penceresinde, yeni VNet oluşturmak için kullanmak istediğiniz değerleri girip **Tamam**’a tıklayın. Aşağıdaki şekil senaryo için değerleri gösterir:
   
    ![ARM şablonu parametreleri](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. **Kaynak grubu**’na tıklayıp VNet'e eklenecek kaynak grubunu seçin veya VNet’i yeni bir kaynak grubuna eklemek için **Yeni oluştur**’a tıklayın. Kaynak adı verilen yeni bir kaynak grubu için Grup ayarları aşağıdaki şekilde gösterilmiştir **TestRG**:

    ![Kaynak grubu](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. Gerekiyorsa, VNet’inizle ilgili **Abonelik** ve **Konum** ayarlarını değiştirin.
7. VNet’i bir kutucuk olarak görmek istemiyorsanız **Başlangıç panosu**’nda **Başlangıç panosuna sabitlemek** öğesini devre dışı bırakın.
8. Tıklatın **yasal koşulları**koşullarını okuyun ve tıklatın **satın** kabul edin. 
9. VNet oluşturmak için **Oluştur**’a tıklayın.
   
    ![Önizleme portalında dağıtım kutucuğu gönderiliyor](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. Azure portal tıklatın, dağıtım tamamlandıktan sonra **tüm hizmetleri**, türü *sanal ağlar* görüntülenen filtre kutusunda sanal ağlar dikey penceresinde görmek için Sanal Ağları'i tıklatın. Dikey penceresinde tıklayın *TestVNet*. İçinde *TestVNet* dikey penceresinde tıklatın **alt ağlar** oluşturulan alt ağlar, aşağıdaki resimde gösterildiği gibi görmek için:
    
     ![Önizleme portalında VNet oluşturma](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a>Sonraki adımlar

Nasıl bağlayacağınızı öğrenin:

- Sanal ağ ile sanal makine (VM) bağlantısı için [Windows VM oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md) veya [Linux VM oluşturma](../virtual-machines/linux/quick-create-portal.md) makalelerini okuyun. Makalelerde yer alan adımlarda sanal ağ ve alt ağ oluşturmak yerine var olan sanal ağı ve alt ağı seçerek VM bağlantısı yapabilirsiniz.
- Bir sanal ağı diğer sanal ağlara bağlamak için [Sanal ağları bağlama](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) makalesini okuyun.
- Bir sanal ağı şirket içi ağa bağlamak için siteden siteye sanal özel ağ (VPN) veya ExpressRoute devresi kullanın. Nasıl yapacağınızı öğrenmek için [Siteden siteye VPN kullanarak sanal ağ ile şirket içi ağ arasında bağlantı kurma](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) ve [Bir sanal ağı ExpressRoute devresine bağlama](../expressroute/expressroute-howto-linkvnet-arm.md) makalelerini okuyun.
