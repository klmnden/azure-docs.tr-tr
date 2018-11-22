---
title: Sanal ağ ve alt ağ tabanlı erişim Azure Cosmos hesabınız için yapılandırma
description: Bu belgede, Kurulum Azure Cosmos DB sanal ağ hizmet uç noktası için gereken adımlar açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: 16cd959a83850a3bc940803cd23e7542e34825c8
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52283221"
---
# <a name="how-to-access-azure-cosmos-db-resources-from-virtual-networks"></a>Azure Cosmos DB kaynaklarını sanal ağlardan erişme

Azure CosmosDB hesapları yalnızca Azure sanal ağının belirli alt ağından erişime izin verecek şekilde yapılandırılabilir. Bir sanal ağdaki (VNET) bir alt ağdan bağlantıları ile Azure Cosmos hesabına erişimi sınırlamak için gereken iki adımı vardır.
 
1. Azure Cosmos DB için alt ağ ve VNET kimlik göndermek alt ağı etkinleştirin. Hizmet uç noktası için Azure Cosmos DB belirli bir alt ağda sağlayarak bunu gerçekleştirebilirsiniz.

1. Azure Cosmos hesabında alt ağı olan, bir kaynak olarak belirten bir kural ekleyin. hesap erişilebilir.

> [!NOTE]
> Bir kez hesabı etkin bir alt ağ üzerinde Azure Cosmos için hizmet uç noktası, Azure Cosmos DB ulaşmasını trafik kaynağını genel IP ile VNET ve alt ağa geçer. Geçiş trafiği, bu alt ağından erişilen herhangi bir Azure Cosmos hesabı için geçerlidir. Azure Cosmos hesaplarınıza bu alt ağ izin vermek için IP tabanlı güvenlik duvarı varsa, alt ağ IP güvenlik duvarı kuralları artık eşleşir ve reddedilir ve etkin hizmetinden ister. Özetlenen adımları daha fazla bilgi için bkz [IP güvenlik duvarı kuralının VNET erişim denetim listesine geçirme](#migrate-from-firewall-to-vnet) bu makalenin. 

Aşağıdaki bölümlerde, sanal ağ hizmet uç noktası için bir Azure Cosmos hesabı yapılandırma açıklanmaktadır.

## <a id="configure-using-portal"></a>Azure portalını kullanarak hizmet uç noktasını yapılandırma

### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Bir mevcut Azure sanal ağı ve alt ağ için hizmet uç noktası yapılandırma

1. Gelen **tüm kaynakları** Azure Cosmos hesabına Bul dikey penceresinde istediğiniz güvenliğini sağlamak.

1. Seçin **güvenlik duvarları ve sanal ağlar** Ayarlar menüsünden ve erişime izin verecek **seçili ağlar**.

1. Bir var olan sanal ağın alt ağı, erişim altında vermek **sanal ağlar**seçin **var olan Azure sanal ağı Ekle**.

1. Seçin **abonelik** Azure sanal ağı eklemek istediğiniz. Azure'ı seçin **sanal ağlar** ve **alt ağlar** Azure Cosmos hesabınıza erişim sağlamak istediğiniz. Ardından **etkinleştirme** "Microsoft.AzureCosmosDB için" hizmet uç noktaları ile seçili ağlar'ı etkinleştirmek için. İşlem tamamlandığında seçin **Ekle**. 

   ![Sanal ağ ve alt ağ seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet.png)


1. Sanal ağ üzerinden erişmek için Azure Cosmos hesabı etkinleştirildikten sonra yalnızca bu seçilen alt ağa gelen trafiğe izin verir. Sanal ağ ve alt eklediğiniz aşağıdaki ekran görüntüsünde gösterildiği gibi görünmelidir:

   ![sanal ağ ve alt ağı başarıyla yapılandırıldı](./media/how-to-configure-vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

> [!NOTE]
> Sanal ağ hizmet uç noktalarını etkinleştirmek için aşağıdaki abonelik izinlere ihtiyacınız:
  * Sanal ağ ile abonelik: ağ Katılımcısı
  * Azure Cosmos hesabı abonelikle: DocumentDB hesabı Katılımcısı

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Yeni bir Azure sanal ağı ve alt ağ için hizmet uç noktasını yapılandırma

1. Gelen **tüm kaynakları** Azure Cosmos hesabına Bul dikey penceresinde istediğiniz güvenliğini sağlamak.

1. Seçin **güvenlik duvarları ve Azure sanal ağları** Ayarlar menüsünden ve erişime izin verecek **seçili ağlar**.  

1. Yeni bir Azure sanal ağı, sanal ağlar'ın altında erişim vermek için seçin **Ekle yeni sanal ağ**.  

1. Yeni bir sanal ağ oluşturmak için gereken ayrıntıları sağlayın ve Oluştur'u seçin. Alt ağı "için Microsoft.AzureCosmosDB etkin" olan bir hizmet uç noktası oluşturulur.

   ![Sanal ağ ve yeni sanal ağ için alt ağ seçin](./media/how-to-configure-vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

Azure Cosmos hesabınızı Azure Search gibi diğer Azure Hizmetleri tarafından kullanılan veya Stream analytics veya Power BI, kontrol ederek erişim izni **genel Azure veri merkezlerinin içinde gelen bağlantıları kabul etmesi**.

Erişim sahibi için Azure Cosmos DB ölçümleri portaldan emin olmak için etkinleştirmeniz gerekir **Azure portalından erişim izni** seçenekleri. Bu seçenekler hakkında daha fazla bilgi için Azure Portalı'ndan istekleri ve Azure PaaS Hizmetleri Yapılandırma bölümlerini istekten bkz [IP Güvenlik Duvarı](how-to-configure-firewall.md) makalesi. Erişim seçtikten sonra seçin **Kaydet** ayarları kaydetmek için.

## <a id="remove-vnet-or-subnet"></a>Bir sanal ağ veya alt ağı Kaldır 

1. Gelen **tüm kaynakları** dikey penceresinde, hizmet uç noktaları atadığınız Azure Cosmos hesabı bulunamadı.  

2. Seçin **güvenlik duvarları ve sanal ağlar** Ayarlar menüsünden.  

3. Bir sanal ağ veya alt ağ kuralı kaldırmak için seçin sanal ağ veya alt ağ ve Seç yanındaki "…" **Kaldır**.

   ![Bir sanal ağ'ı Kaldır](./media/how-to-configure-vnet-service-endpoint/remove-a-vnet.png)

4.  Tıklayın **Kaydet** yaptığınız değişiklikleri uygulamak için.

## <a id="configure-using-powershell"></a>Azure PowerShell kullanarak hizmet uç noktasını yapılandırma

> [!NOTE]
> PowerShell veya CLI kullanılırken parametrelerinde, parolaya eklenmesi gerekir olanları IP filtreleri ve sanal ağ ACL'leri tam listesi belirtmeyi unutmayın.

Azure PowerShell kullanarak bir Azure Cosmos hesap için hizmet uç noktasını yapılandırmak için aşağıdaki adımları kullanın:  

1. Son yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ve [oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  

1. Bir sanal ağın var olan bir alt ağ için hizmet uç noktasını girin.  

   ```powershell
   $rgname = "<Resource group name>"
   $vnName = "<Virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. VNET bilgileri edinin.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName `
     -ResourceGroupName $rgName
   ```

1. Azure Cosmos hesabının özellikleri, aşağıdaki cmdlet'i çalıştırarak alın:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
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

1. Azure Cosmos hesap özellikleri, aşağıdaki cmdlet'leri çalıştırarak yeni yapılandırmayla güncelleştirin: 

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName `
     -Properties $CosmosDBProperties
   ```

1. Azure Cosmos hesabınızı, önceki adımda yapılandırdığınız sanal ağ hizmet uç noktası ile güncelleştirildiğini doğrulamak için aşağıdaki komutu çalıştırın:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a id="configure-using-cli"></a>Azure CLI kullanarak hizmet uç noktasını yapılandırma 

1. Bir sanal ağ içindeki alt ağ için hizmet uç noktasını girin.

1. Var olan Azure Cosmos hesabı ACL'ler alt ağ ile güncelleştirme

   ```azurecli-interactive

   name="<Azure Cosmos account name>"
   resourceGroupName="<Resource group name>"

   az cosmosdb update \
    --name $name \
    --resource-group $resourceGroupName \
    --enable-virtual-network true \
    --virtual-network-rules "/subscriptions/testsub/resourceGroups/testRG/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/frontend"
   ```

1. ACL'ler alt ağ ile yeni bir Azure Cosmos hesabı oluşturma

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

## <a id="migrate-from-firewall-to-vnet"></a>IP güvenlik duvarı kuralının VNET ACL geçirme 

Aşağıdaki adımlar, yalnızca Azure Cosmos hesapları için bir alt ağ izin vererek var olan IP güvenlik duvarı kuralları ile gereklidir ve VNET ve alt ağ tabanlı ACL'ler yerine IP güvenlik duvarı kuralı kullanmak istiyorsunuz.

Bir alt ağ için Azure Cosmos hesabı için hizmet uç noktası etkinleştirildikten sonra istekleri yerine genel IP VNET ve alt ağ bilgilerini içeren kaynak ile gönderilir. Bu nedenle, bu tür istekleri IP Filtresi eşleşmiyor. Bu kaynak anahtarı, alt ağdan etkin hizmet bitiş noktası ile erişilen tüm Azure Cosmos hesaplar için gerçekleşir. Kapalı kalma süresini önlemek için aşağıdaki adımları kullanın:

1. Azure Cosmos hesabının özellikleri, aşağıdaki cmdlet'i çalıştırarak alın:

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Cosmos account name>"

   $cosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

1. Daha sonra kullanmak üzere değişkenlerini başlatın. Mevcut hesabı tanımından tüm değişkenleri ayarlayın. Tüm Azure cosmos VNET ACL hesapları alt ağ ile erişilen ekleme `ignoreMissingVNetServiceEndpoint` bayrağı.

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

1. Azure Cosmos hesap özellikleri, aşağıdaki cmdlet'leri çalıştırarak yeni yapılandırmayla güncelleştirin:

   ```powershell
   $cosmosDBProperties = @{
      databaseAccountOfferType      = $cosmosDBConfiguration.Properties.databaseAccountOfferType;
      consistencyPolicy             = $cosmosDBConfiguration.Properties.consistencyPolicy;
      ipRangeFilter                 = $cosmosDBConfiguration.Properties.ipRangeFilter;
      locations                     = $locations;
      virtualNetworkRules           = $virtualNetworkRules;
      isVirtualNetworkFilterEnabled = $True;
   }

   Set-AzureRmResource `
      -ResourceType "Microsoft.DocumentDB/databaseAccounts" `
      -ApiVersion $apiVersion `
      -ResourceGroupName $rgName `
      -Name $acctName `
      -Properties $CosmosDBProperties
   ```

1. Alt ağdan erişim tüm Azure Cosmos hesaplar için 1-3. adımları tekrarlayın.

1.  İçin 15 dakika bekleyin ve ardından alt ağ hizmet uç noktası etkinleştirmek için güncelleştirin.

1.  Bir sanal ağın var olan bir alt ağ için hizmet uç noktasını girin.

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
      -ResourceGroupName $rgname `
      -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
      -Name $sname `
      -AddressPrefix $subnetPrefix `
      -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

1. Alt ağ için IP güvenlik duvarı kuralı kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB için güvenlik duvarını yapılandırmak için bkz [güvenlik duvarı desteği](firewall-support.md) makalesi.
