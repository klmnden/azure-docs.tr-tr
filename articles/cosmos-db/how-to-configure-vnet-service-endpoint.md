---
title: Sanal ağ ve alt ağ tabanlı erişim Azure Cosmos DB hesabınız için yapılandırma
description: Bu belgede, Azure Cosmos DB için sanal ağ hizmet uç noktası ayarlamak için gereken adımlar açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: govindk
ms.openlocfilehash: 375e79d2fe70e0988d8c58997a746f77b21d7f50
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241996"
---
# <a name="configure-access-from-virtual-networks-vnet"></a>Sanal ağ (VNet) erişimi yapılandırma

Bir Azure sanal ağı, yalnızca belirli bir alt ağından erişime izin vermek için Azure Cosmos DB hesabı yapılandırabilirsiniz. Bir sanal ağdaki bir alt ağdan bağlantısı olan bir Azure Cosmos DB hesabına erişimi sınırlamak için:
 
1. Alt ağ ve sanal ağ kimliği, Azure Cosmos DB'ye göndermek alt ağı etkinleştirin. Hizmet uç noktası için Azure Cosmos DB belirli bir alt ağda sağlayarak bunu gerçekleştirebilirsiniz.

1. Azure Cosmos DB hesabı alt hesabı erişilebilir bir kaynak olarak belirtmek için bir kural ekleyin.

> [!NOTE]
> Azure Cosmos DB hesabınız için bir hizmet uç noktası bir alt ağ üzerinde etkinleştirildiğinde, bir sanal ağ ve alt ağ Azure Cosmos DB ulaştığında trafik kaynağını bir genel IP adresinden geçer. Geçiş trafiği, bu alt ağdan erişilebilen herhangi bir Azure Cosmos DB hesabı için geçerlidir. Bu alt ağ bir IP tabanlı Güvenlik Duvarı'nı, Azure Cosmos DB hesapları varsa hizmeti etkin alt ağından gelen istekleri IP güvenlik duvarı kuralları artık eşleşen ve bunlar reddetti. 
>
> Özetlenen adımları daha fazla bilgi için bkz [bir IP güvenlik duvarı kuralı bir sanal ağ erişim denetim listesine geçirme](#migrate-from-firewall-to-vnet) bu makalenin. 

Aşağıdaki bölümlerde, bir Azure Cosmos DB hesabı için bir sanal ağ hizmet uç noktası yapılandırma açıklanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a id="configure-using-portal"></a>Azure portalını kullanarak bir hizmet uç noktasını yapılandırın

### <a name="configure-a-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Bir mevcut Azure sanal ağı ve alt ağ için hizmet uç noktası yapılandırma

1. Gelen **tüm kaynakları** dikey penceresinde Azure Cosmos DB hesabını, bulma, istediğiniz güvenliğini sağlamak.

1. Seçin **güvenlik duvarları ve sanal ağlar** Ayarlar menüsünden ve erişime izin verecek şekilde seçin **seçili ağlar**.

1. Bir var olan sanal ağın alt ağı, erişim altında vermek **sanal ağlar**seçin **var olan Azure sanal ağı Ekle**.

1. Seçin **abonelik** bir Azure sanal ağı eklemek istediğiniz. Azure'ı seçin **sanal ağlar** ve **alt ağlar** Azure Cosmos DB hesabınızın erişim sağlamak istediğiniz. Ardından, **etkinleştirme** "Microsoft.AzureCosmosDB için" hizmet uç noktaları ile seçili ağlar'ı etkinleştirmek için. İşlem tamamlandığında seçin **Ekle**. 

   ![Sanal ağ ve alt ağ seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)


1. Azure Cosmos DB hesabı, bir sanal ağdan erişim için etkinleştirildikten sonra yalnızca bu alt ağ seçilen gelen trafiğe izin verir. Eklenen alt ağı ve sanal ağ, aşağıdaki ekran görüntüsünde gösterildiği gibi görünmelidir:

   ![sanal ağ ve alt ağı başarıyla yapılandırıldı](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Sanal ağ hizmet uç noktalarını etkinleştirmek için aşağıdaki abonelik izinler gerekir:
>   * Abonelik sanal ağ ile: Ağ Katılımcısı
>   * Azure Cosmos DB hesabını içeren aboneliği: DocumentDB hesabı Katılımcısı
>   * Sanal ağınız ile Azure Cosmos DB hesabını, farklı Aboneliklerde olması halinde, sanal ağ olan aboneliği de olduğundan emin olun `Microsoft.DocumentDB` kaynak sağlayıcısı kayıtlı. Bir kaynak sağlayıcısını kaydetmek için bkz: [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md) makalesi. 

Abonelik, kaynak sağlayıcısı ile kaydetmek için yönergeleri aşağıda verilmiştir.

### <a name="configure-a-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Yeni bir Azure sanal ağı ve alt ağ için hizmet uç noktası yapılandırma

1. Gelen **tüm kaynakları** dikey penceresinde Azure Cosmos DB hesabını, bulma, istediğiniz güvenliğini sağlamak.  

1. Seçin **güvenlik duvarları ve Azure sanal ağları** Ayarlar menüsünden ve erişime izin verecek şekilde seçin **seçili ağlar**.  

1. Yeni bir Azure sanal ağ, erişim altında vermek **sanal ağlar**seçin **Ekle yeni sanal ağ**.  

1. Yeni bir sanal ağ oluşturun ve ardından seçmek için gerekli ayrıntıları **Oluştur**. Alt ağı "için Microsoft.AzureCosmosDB etkin" olan bir hizmet uç noktası oluşturulur.

   ![Bir sanal ağ ve yeni bir sanal ağ için alt ağ seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Azure Cosmos DB hesabınızın Azure Search gibi diğer Azure Hizmetleri tarafından kullanılan ya da Power BI ve Stream analytics depolamadan erişilen seçerek erişim izni **genel Azure veri merkezlerinin içinde gelen bağlantıları kabul etmesi**.

Portaldan ölçümlerine Azure Cosmos DB erişimi olduğundan emin olun için etkinleştirmeniz gerekir **Azure portalından erişim izni** seçenekleri. Bu seçenekler hakkında daha fazla bilgi için bkz. [bir IP Güvenlik Duvarı Yapılandırma](how-to-configure-firewall.md) makalesi. Erişimi etkinleştirdikten sonra Seç **Kaydet** ayarları kaydetmek için.

## <a id="remove-vnet-or-subnet"></a>Bir sanal ağ veya alt ağı Kaldır 

1. Gelen **tüm kaynakları** dikey penceresinde, hizmet uç noktaları atadığınız Bul Azure Cosmos DB hesabı.  

2. Seçin **güvenlik duvarları ve sanal ağlar** Ayarlar menüsünden.  

3. Bir sanal ağ veya alt ağ kuralı kaldırmak için işaretleyin **...**  sanal ağ veya alt ağ ve select yanındaki **Kaldır**.

   ![Bir sanal ağ'ı Kaldır](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

4.  Seçin **Kaydet** yaptığınız değişiklikleri uygulamak için.

## <a id="configure-using-powershell"></a>Azure PowerShell kullanarak bir hizmet uç noktasını yapılandırma

> [!NOTE]
> PowerShell veya Azure CLI'yı kullanırken, parametreleri, parolaya eklenmesi gerekir olanları IP filtreleri ve sanal ağ ACL'leri tam listesi belirtmeyi unutmayın.

Azure PowerShell kullanarak Azure Cosmos DB hesabı için bir hizmet uç noktasını yapılandırmak için aşağıdaki adımları kullanın:  

1. Yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps) ve [oturum](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Bir sanal ağın var olan bir alt ağ için hizmet uç noktasını girin.  

   ```powershell
   $rgname = "<Resource group name>"
   $vnName = "<Virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzVirtualNetwork
   ```

1. Sanal ağ bilgi alın.

   ```powershell
   $vnProp = Get-AzVirtualNetwork `
     -Name $vnName `
     -ResourceGroupName $rgName
   ```

1. Azure Cosmos DB hesabının özellikleri, aşağıdaki cmdlet'i çalıştırarak alın:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Daha sonra kullanmak için değişkenlerini başlatın. Mevcut hesabı tanımından tüm değişkenleri ayarlayın.

   ```powershell
   $locations = @()

   foreach ($readLocation in $cosmosDBConfiguration.Properties.readLocations) {
      $locations += , @{
         locationName     = $readLocation.locationName;
         failoverPriority = $readLocation.failoverPriority;
      }
   }

   $virtualNetworkRules = @(@{
      id = "$($vnProp.Id)/subnets/$sname";
   })

   if ($cosmosDBConfiguration.Properties.isVirtualNetworkFilterEnabled) {
      $virtualNetworkRules = $cosmosDBConfiguration.Properties.virtualNetworkRules + $virtualNetworkRules
   }
   ```

1. Azure Cosmos DB hesabı özellikleri, aşağıdaki cmdlet'leri çalıştırarak yeni yapılandırmayla güncelleştir: 

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName `
     -Properties $CosmosDBProperties
   ```

1. Azure Cosmos DB hesabınızın, önceki adımda yapılandırdığınız sanal ağ hizmet uç noktası ile güncelleştirildiğini doğrulamak için aşağıdaki komutu çalıştırın:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a id="configure-using-cli"></a>Azure CLI kullanarak bir hizmet uç noktasını yapılandırma 

1. Bir sanal ağ içindeki alt ağ için hizmet uç noktasını girin.

1. Var olan Azure Cosmos DB hesabı, alt ağ erişim denetim listeleri (ACL) ile güncelleştirin.

   ```azurecli-interactive

   name="<Azure Cosmos DB account name>"
   resourceGroupName="<Resource group name>"

   az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/frontend"
   ```

1. ACL'ler alt ağ ile yeni bir Azure Cosmos DB hesabı oluşturun.

   ```azurecli-interactive
   az cosmosdb create \
    --name $name \
    --kind GlobalDocumentDB \
    --resource-group $resourceGroupName \
    --max-interval 10 \
    --max-staleness-prefix 200 \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/default"
   ```

## <a id="migrate-from-firewall-to-vnet"></a>Bir IP güvenlik duvarı kuralı bir sanal ağa ACL geçirme 

Yalnızca bir alt ağ, sanal ağ ve alt ağ tabanlı ACL'ler bir IP güvenlik duvarı kuralı yerine kullanmak istediğinizde sağlayan mevcut IP güvenlik duvarı kuralları ile Azure Cosmos DB hesapları için aşağıdaki adımları kullanın.

Bir alt ağ için bir Azure Cosmos DB hesabı için bir hizmet uç noktası etkinleştirildikten sonra istekleri bir genel IP yerine sanal ağ ve alt ağ bilgilerini içeren bir kaynak ile gönderilir. Bu istekleri bir IP filtresinin eşleşmiyor. Bu kaynak anahtarı, alt ağdan etkin hizmet bitiş noktası ile erişilen tüm Azure Cosmos DB hesapları için gerçekleşir. Kapalı kalma süresini önlemek için aşağıdaki adımları kullanın:

1. Azure Cosmos DB hesabının özellikleri, aşağıdaki cmdlet'i çalıştırarak alın:

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Daha sonra kullanmak üzere değişkenlerini başlatın. Mevcut hesabı tanımından tüm değişkenleri ayarlayın. Sanal ağ ACL'si alt ağ ile erişilen tüm Azure Cosmos DB hesapları ekleyin `ignoreMissingVNetServiceEndpoint` bayrağı.

   ```powershell
   $locations = @()

   foreach ($readLocation in $cosmosDBConfiguration.Properties.readLocations) {
      $locations += , @{
         locationName     = $readLocation.locationName;
         failoverPriority = $readLocation.failoverPriority;
      }
   }

   $subnetID = "Subnet ARM URL" e.g "/subscriptions/f7ddba26-ab7b-4a36-a2fa-7d01778da30b/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/subnet1"

   $virtualNetworkRules = @(@{
      id = $subnetID;
      ignoreMissingVNetServiceEndpoint = "True";
   })

   if ($cosmosDBConfiguration.Properties.isVirtualNetworkFilterEnabled) {
      $virtualNetworkRules = $cosmosDBConfiguration.Properties.virtualNetworkRules + $virtualNetworkRules
   }
   ```

1. Azure Cosmos DB hesabı özellikleri, aşağıdaki cmdlet'leri çalıştırarak yeni yapılandırmayla güncelleştir:

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzResource `
      -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
      -ApiVersion $apiVersion `
      -ResourceGroupName $rgName `
      -Name $acctName `
      -Properties $CosmosDBProperties
   ```

1. Alt ağdan erişim tüm Azure Cosmos DB hesapları için 1-3. adımları tekrarlayın.

1.  15 dakika bekleyin ve ardından hizmet uç noktasını etkinleştirmek için alt güncelleştirin.

1.  Bir sanal ağın var olan bir alt ağ için hizmet uç noktasını girin.

    ```powershell
    $rgname= "<Resource group name>"
    $vnName = "<virtual network name>"
    $sname = "<Subnet name>"
    $subnetPrefix = "<Subnet address range>"

    Get-AzVirtualNetwork `
       -ResourceGroupName $rgname `
       -Name $vnName | Set-AzVirtualNetworkSubnetConfig `
       -Name $sname `
       -AddressPrefix $subnetPrefix `
       -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzVirtualNetwork
    ```

1. Alt ağ için IP güvenlik duvarı kuralını kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB için güvenlik duvarını yapılandırmak için bkz [güvenlik duvarı desteği](firewall-support.md) makalesi.
