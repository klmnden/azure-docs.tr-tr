---
title: Azure sanal ağ hizmet uç noktası'nı kullanarak bir Azure Cosmos DB hesabına erişim güvenliğinin | Microsoft Docs
description: Bu belgede, Kurulum Azure Cosmos DB sanal ağ hizmet uç noktası için gereken adımlar açıklanmaktadır.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: govindk
ms.openlocfilehash: a4758e5597876112fa7a85850786491e22af8c83
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037151"
---
# <a name="secure-access-to-an-azure-cosmos-db-account-by-using-azure-virtual-network-service-endpoint"></a>Azure sanal ağ hizmet uç noktası'nı kullanarak bir Azure Cosmos DB hesabı güvenli erişim

Azure CosmosDB hesapları yalnızca Azure sanal ağının belirli alt ağından erişime izin verecek şekilde yapılandırılabilir. Sağlayarak bir [hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) bir sanal ağ ve onun alt Azure CosmosDB için en uygun ve güvenli bir yol Azure Cosmos DB için trafiği güvence altına.  

Azure Cosmos DB global olarak dağıtılmış, çok modelli bir veritabanıdır. Azure Cosmos DB hesabını mevcut verileri birden çok bölgesine çoğaltabilirsiniz. Azure Cosmos DB ile bir sanal ağ hizmet uç noktası olarak yapılandırıldığında, her bir sanal ağ specicifc alt ağına ait ıp'lerden erişim sağlar. Aşağıdaki görüntüde bir gösterimi olan sanal ağ hizmet uç noktası etkin bir Azure Cosmos DB gösterilmektedir:

![sanal ağ hizmet uç noktası mimarisi](./media/vnet-service-endpoint/vnet-service-endpoint-architecture.png)

Azure Cosmos DB hesabı olan bir sanal ağ hizmet uç noktası yapılandırıldıktan sonra yalnızca belirtilen alt ağdan erişilebilir, tüm genel/Internet erişimi kaldırılır. Azure hizmet uç noktaları hakkında ayrıntılı bilgi, sorun [sanal ağ hizmet uç noktalarına genel bakış](../virtual-network/virtual-network-service-endpoints-overview.md) makalesi.

## <a name="configure-service-endpoint-by-using-azure-portal"></a>Azure portalını kullanarak hizmet uç noktasını yapılandırma
### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Bir mevcut Azure sanal ağı ve alt ağ için hizmet uç noktası yapılandırma

1. Gelen **tüm kaynakları** Bul sanal ağ dikey penceresinde istediğiniz Azure Cosmos DB için hizmet uç noktasını yapılandırmak. Gidin **hizmet uç noktalarını** dikey penceresi ve sanal ağın alt ağında "Azure.CosmosDB" hizmet uç noktası için etkinleştirilmiş olduğundan emin olun.  

   ![Etkin hizmet bitiş noktası onaylayın](./media/vnet-service-endpoint/confirm-service-endpoint-enabled.png)

2. Gelen **tüm kaynakları** Bul Azure Cosmos DB hesabı dikey penceresinde istediğiniz güvenliğini sağlamak.  

3. Sanal ağ hizmet uç noktası etkinleştirmeden önce Azure Cosmos DB hesabınız gelecekteki kullanımlarınız için ile ilişkili IP Güvenlik Duvarı bilgileri kopyalayın. IP Güvenlik Duvarı Hizmeti uç noktası yapılandırıldıktan sonra yeniden etkinleştirebilirsiniz.  

4. Seçin **güvenlik duvarları ve sanal ağlar** Ayarlar menüsünden ve erişime izin verecek **seçili ağlar**.  

3. Bir var olan sanal ağın alt ağı, sanal ağlar'ın altında erişim vermek için seçin **var olan Azure sanal ağı Ekle**.  

4. Seçin **abonelik** Azure sanal ağı eklemek istediğiniz. Azure'ı seçin **sanal ağlar** ve **alt ağlar** Azure Cosmos DB hesabınızın erişim sağlamak istediğiniz. Ardından **etkinleştirme** "Microsoft.AzureCosmosDB için" hizmet uç noktaları ile seçili ağlar'ı etkinleştirmek için. İşlem tamamlandığında seçin **Ekle**.  

   ![Sanal ağ ve alt ağ seçin](./media/vnet-service-endpoint/choose-subnet-and-vnet.png)

   > [!NOTE]
   > Azure Cosmos DB için hizmet uç noktası daha önce seçili Azure sanal ağları ve alt ağlar için yapılandırılmamışsa, bu işlemin bir parçası yapılandırılabilir. Erişimi etkinleştirme tamamlanması 15 dakika sürer. IP Güvenlik Duvarı'nı Not renabling güvenlik duvarı ACL içeriğini bunları daha sonra aldığınızdan sonra devre dışı bırakmak çok önemlidir. 

   ![sanal ağ ve alt ağı başarıyla yapılandırıldı](./media/vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

Artık Azure Cosmos DB hesabınız yalnızca bu seçilen alt ağa gelen trafiğe izin verir. Daha önce IP Güvenlik Duvarı etkin, Lütfen bunları önceki bilgileri kullanarak yeniden etkinleştirin.

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Yeni bir Azure sanal ağı ve alt ağ için hizmet uç noktasını yapılandırma

1. Gelen **tüm kaynakları** Bul Azure Cosmos DB hesabı dikey penceresinde istediğiniz güvenliğini sağlamak.  

> [!NOTE]
> Azure Cosmos DB hesabınız için yapılandırılan mevcut bir IP güvenlik duvarı varsa, lütfen güvenlik duvarı yapılandırması unutmayın, IP Güvenlik Duvarı'nı kaldırın ve ardından hizmet uç noktasını girin. Hizmet uç noktası olmadığında disbling güvenlik duvarını etkinleştirirseniz, bu IP aralığı gelen trafik sanal IP kimlik kaybedeceksiniz ve bir IP Filtresi hata iletisiyle bırakılır. Bu hatayı önlemek için her zaman güvenlik duvarı kurallarını devre dışı bırakmanız gerekir, böylece bunları kopyalayabilir, alt ağ ve son olarak ACL Cosmos DB'den bir alt ağ hizmet uç noktasını girin. Hizmet uç noktası yapılandırın ve ACL ekleyin, IP Güvenlik Duvarı'nı yeniden gerekirse yeniden etkinleştirebilirsiniz.

2. Sanal ağ hizmet uç noktası etkinleştirmeden önce Azure Cosmos DB hesabınız gelecekteki kullanımlarınız için ile ilişkili IP Güvenlik Duvarı bilgileri kopyalayın. IP Güvenlik Duvarı Hizmeti uç noktası yapılandırıldıktan sonra yeniden etkinleştirebilirsiniz.  

3. Seçin **güvenlik duvarları ve Azure sanal ağları** Ayarlar menüsünden ve erişime izin verecek **seçili ağlar**.  

4. Yeni bir Azure sanal ağı, sanal ağlar'ın altında erişim vermek için seçin **Ekle yeni sanal ağ**.  

5. Yeni bir sanal ağ oluşturmak için gereken ayrıntıları sağlayın ve Oluştur'u seçin. Alt ağı "için Microsoft.AzureCosmosDB etkin" olan bir hizmet uç noktası oluşturulur.

   ![Sanal ağ ve yeni sanal ağ için alt ağ seçin](./media/vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

## <a name="allow-access-from-azure-portal"></a>Azure portalından erişime izin ver

Azure sanal ağ hizmet uç noktaları, Azure Cosmos DB veritabanı hesabınız için etkinleştirildikten sonra portal veya diğer Azure hizmetlerinden gelen erişim varsayılan olarak devre dışıdır. Erişim Portalı'ndan dahil olmak üzere yapılandırılmış alt dışındaki makinelerden Azure Cosmos DB veritabanı hesabınız için erişim engellenir.

![Portaldan erişime izin ver](./media/vnet-service-endpoint/allow-access-from-portal.png)

Azure Cosmos DB hesabınızın Azure Search gibi diğer Azure Hizmetleri tarafından kullanılan veya Stream analytics veya Power BI, kontrol ederek erişim izni **Azure hizmetlerine erişime izin ver**.

Erişim sahibi için Azure Cosmos DB ölçümleri portaldan emin olmak için etkinleştirmeniz gerekir **Azure portalına erişimine izin** seçenekleri. Bu seçenekler hakkında daha fazla bilgi için bkz. [bağlantıları Azure portalından](firewall-support.md#connections-from-the-azure-portal) ve [Azure PaaS hizmetlerinden gelen bağlantıları](firewall-support.md#connections-from-global-azure-datacenters-or-azure-paas-services) bölümler. Erişim seçtikten sonra seçin **Kaydet** ayarları kaydetmek için.

## <a name="remove-a-virtual-network-or-subnet"></a>Bir sanal ağ veya alt ağı Kaldır 

1. Gelen **tüm kaynakları** dikey penceresinde, hizmet uç noktaları atadığınız Bul Azure Cosmos DB hesabı.  

2. Seçin **güvenlik duvarları ve sanal ağlar** Ayarlar menüsünden.  

3. Bir sanal ağ veya alt ağ kuralı kaldırmak için seçin sanal ağ veya alt ağ ve Seç yanındaki "…" **Kaldır**.

   ![Bir sanal ağ'ı Kaldır](./media/vnet-service-endpoint/remove-a-vnet.png)

4.  Tıklayın **Kaydet** yaptığınız değişiklikleri uygulamak için.

## <a name="configure-service-endpoint-by-using-azure-powershell"></a>Azure PowerShell kullanarak hizmet uç noktasını yapılandırma 

Azure PowerShell kullanarak Azure Cosmos DB hesabı için hizmet uç noktasını yapılandırmak için aşağıdaki adımları kullanın:  

1. Son yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ve [oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  IP Güvenlik Duvarı ayarlarını not edin ve hesap için hizmet uç noktasını etkinleştirmeden önce IP Güvenlik Duvarı tamamen silmek emin olun.


> [!NOTE]
> Azure Cosmos DB hesabınız için yapılandırılan mevcut bir IP güvenlik duvarı varsa, lütfen güvenlik duvarı yapılandırması unutmayın, IP Güvenlik Duvarı'nı kaldırın ve ardından hizmet uç noktasını girin. Hizmet uç noktası olmadığında disbling güvenlik duvarını etkinleştirirseniz, bu IP aralığı gelen trafik sanal IP kimlik kaybedeceksiniz ve bir IP Filtresi hata iletisiyle bırakılır. Bu hatayı önlemek için her zaman güvenlik duvarı kurallarını devre dışı bırakmanız gerekir, böylece bunları kopyalayabilir, alt ağ ve son olarak ACL Cosmos DB'den bir alt ağ hizmet uç noktasını girin. Hizmet uç noktası yapılandırın ve ACL ekleyin, IP Güvenlik Duvarı'nı yeniden gerekirse yeniden etkinleştirebilirsiniz.

2. Sanal ağ hizmet uç noktası etkinleştirmeden önce Azure Cosmos DB hesabınız gelecekteki kullanımlarınız için ile ilişkili IP Güvenlik Duvarı bilgileri kopyalayın. Hizmet uç noktası yapılandırıldıktan sonra IP Güvenlik Duvarı'nı yeniden etkinleştirmeniz.  

3. Bir sanal ağın var olan bir alt ağ için hizmet uç noktasını girin.  

   ```powershell
   $rgname= "<Resource group name>"
   $vnName = "<virtual network name>"
   $sname = "<Subnet name>"
   $subnetPrefix = "<Subnet address range>"

   Get-AzureRmVirtualNetwork `
    -ResourceGroupName $rgname `
    -Name $vnName | Set-AzureRmVirtualNetworkSubnetConfig `
    -Name $sname  `
    -AddressPrefix $subnetPrefix `
    -ServiceEndpoint "Microsoft.AzureCosmosDB" | Set-AzureRmVirtualNetwork
   ```

4. CosmosDB hesabı, sanal ağ ve alt ağ için Azure Cosmos DB etkin hizmet bitiş noktası olmasını sağlayarak ACL doğrulanabilmesi için hazır olun.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

5. Azure Cosmos DB hesabının özellikleri, aşağıdaki cmdlet'i çalıştırarak alın:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

6. Daha sonra kullanmak için değişkenlerini başlatın. Birden fazla konuma varsa tüm değişkenlerini mevcut hesabı tanımından ayarlamak, dizi bir parçası olarak eklemeniz gerekir. Bu adımda, "accountVNETFilterEnabled" değişkeni "True" ayarını belirleyerek sanal ağ hizmet uç noktası da yapılandırın. Bu değer daha sonra "isVirtualNetworkFilterEnabled" parametresi olarak atanır.  

   ```powershell
   $locations = @(@{})

   <# If you have read regions in addition to a write region, use the following code to set the $locations variable instead.

   $locations = @(@{"locationName"="<Write location>"; 
                 "failoverPriority"=0}, 
               @{"locationName"="<Read location>"; 
                  "failoverPriority"=1}) #>

   $consistencyPolicy = @{}
   $cosmosDBProperties = @{}

   $locations[0]['failoverPriority'] = $cosmosDBConfiguration.Properties.failoverPolicies.failoverPriority
   $locations[0]['locationName'] = $cosmosDBConfiguration.Properties.failoverPolicies.locationName

   $consistencyPolicy = $cosmosDBConfiguration.Properties.consistencyPolicy

   $accountVNETFilterEnabled = $True
   $subnetID = $vnProp.Id+"/subnets/" + $sname  
   $virtualNetworkRules = @(@{"id"=$subnetID})
   $databaseAccountOfferType = $cosmosDBConfiguration.Properties.databaseAccountOfferType
   ```

7. Azure Cosmos DB hesabı özellikleri, aşağıdaki cmdlet'leri çalıştırarak yeni yapılandırmayla güncelleştir: 

   ```powershell
   $cosmosDBProperties['databaseAccountOfferType'] = $databaseAccountOfferType
   $cosmosDBProperties['locations'] = $locations
   $cosmosDBProperties['consistencyPolicy'] = $consistencyPolicy
   $cosmosDBProperties['virtualNetworkRules'] = $virtualNetworkRules
   $cosmosDBProperties['isVirtualNetworkFilterEnabled'] = $accountVNETFilterEnabled

   Set-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName -Properties $CosmosDBProperties
   ```

8. Azure Cosmos DB hesabınızın, önceki adımda yapılandırdığınız sanal ağ hizmet uç noktası ile güncelleştirildiğini doğrulamak için aşağıdaki komutu çalıştırın:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a name="add-virtual-network-service-endpoint-for-an-azure-cosmos-db-account-that-has-ip-firewall-enabled"></a>Sanal ağ hizmet uç noktası IP Güvenlik Duvarı etkin olan bir Azure Cosmos DB hesabı için ekleyin

1. İlk Azure Cosmos DB hesabı IP Güvenlik Duvarı erişimi devre dışı bırakın.  

2. Azure Cosmos DB hesabı için sanal ağ uç noktası, önceki bölümlerde açıklanan yöntemlerden birini kullanarak etkinleştirin.  

3. IP Güvenlik Duvarı erişimi yeniden etkinleştirin. 

## <a name="test-the-access"></a>Erişimi test etme

Azure Cosmos DB için hizmet uç noktalarının beklenen şekilde yapılandırılıp yapılandırılmadığını denetlemek için aşağıdaki adımları kullanın:

* Ön uç ve arka uç olarak Cosmos DB hizmet uç noktası için arka uç alt ağı iki alt ağa bir sanal ağdaki ayarlayın.  

* Arka uç alt ağının Cosmos DB hesabına erişimi etkinleştirin.  

* İki sanal makine bir arka uç alt ağı ve başka bir ön uç alt ağını oluşturun.  

* Her iki sanal makinelerden Azure Cosmos DB hesabı erişmeyi deneyin. Arka uç alt ağda oluşturulan sanal makineden ancak ön uç alt ağı içinde oluşturulan bir bağlantı kurmanız. İstek, sanal ağ hizmet uç noktasını kullanarak Azure Cosmos DB erişimi sağlandığını doğrulayan frontend alt ağından bağlanmayı denediğinizde 404 ile kullanıma alma hatası olur. Arka uç alt ağı makinede düzgün çalışması mümkün olacaktır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-happens-when-you-access-an-azure-cosmos-db-account-that-has-virtual-network-access-control-list-acl-enabled"></a>Sanal ağ erişim denetimi listesi (ACL) etkin olan bir Azure Cosmos DB hesap eriştiğinde ne olur?  

HTTP 404 hatası döndürülür.  

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-cosmos-db-account-in-another-region-for-example-if-azure-cosmos-db-account-is-in-west-us-or-east-us-and-virtual-networks-are-in-multiple-regions-can-the-virtual-network-access-azure-cosmos-db"></a>Başka bir bölgede bir Azure Cosmos DB hesabına erişmesi için izin verilen farklı bölgelerde oluşturulmuş sanal ağ alt ağları misiniz? Örneğin, Batı ABD veya Doğu ABD ve sanal ağın Azure Cosmos DB hesabı ise, birden çok bölgede sanal ağ, Azure Cosmos DB erişebilirsiniz misiniz?  

Evet, farklı bölgelerde oluşturulmuş sanal ağlar ile yeni özellik erişebilirsiniz. 

### <a name="can-an-azure-cosmos-db-account-have-both-virtual-network-service-endpoint-and-a-firewall"></a>Azure Cosmos DB hesabı, sanal ağ hizmet uç noktası hem de bir güvenlik duvarı olabilir mi?  

Evet, sanal ağ hizmet uç noktası ve bir güvenlik duvarı birlikte bulunabilir. Genel olarak, portalına erişim her zaman bir sanal ağ hizmet uç noktası yapılandırmadan önce kapsayıcı ile ilişkili ölçümleri görüntülemek sağlamak için etkinleştirildiğinden emin olmanız gerekir.

### <a name="can-i-allow-access-to-other-azure-services-from-a-given-azure-region-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Ben "erişim diğer Azure Hizmetleri için belirli bir Azure bölgesinden izin verebilirsiniz" Azure Cosmos DB için hizmet uç noktası erişimi etkinleştirildiğinde?  

Azure Data factory, Azure Search veya tüm hizmet gibi diğer Azure birinci taraf tarafından erişilecek, Azure Cosmos DB hesabı istediğinizde Hizmetleri yalnızca bu gereklidir belirli bir Azure bölgesine dağıtılır.

### <a name="how-many-virtual-network-service-endpoints-are-allowed-for-azure-cosmos-db"></a>Kaç tane sanal ağ hizmet uç noktaları, Azure Cosmos DB için izin verilir?  

64 sanal ağ hizmet uç noktaları, bir Azure Cosmos DB hesabı için izin verilir.

### <a name="what-is-the-relationship-between-service-endpoint-and-network-security-group-nsg-rules"></a>Hizmet uç noktası ve ağ güvenlik grubu (NSG) kuralları arasındaki ilişki nedir?  

Azure Cosmos DB'de NSG kuralları, belirli Azure Cosmos DB IP adresi aralığına erişimini kısıtlamak üzere izin verin. Belirli bir var olan bir Azure Cosmos DB örneğine erişmesine izin vermek istiyorsanız [bölge](https://azure.microsoft.com/global-infrastructure/regions/), bölgeyi şu biçimde belirtebilirsiniz: 

    AzureCosmosDB.<region name>

Bkz: etiketler NSG hakkında daha fazla bilgi edinmek için [sanal ağ hizmet etiketleri](../virtual-network/security-overview.md#service-tags) makalesi. 
  
### <a name="what-is-relationship-between-an-ip-firewall-and-virtual-network-service-endpoint-capability"></a>Bir IP Güvenlik Duvarı ve sanal ağ hizmet uç noktası özelliğini arasındaki ilişki nedir?  

Bu iki özellik, diğer Azure Cosmos DB varlıklarını yalıtımının emin olmak ve bunları güvenli tamamlar. IP Güvenlik Duvarı statik IP Azure Cosmos DB hesabına erişebildiğinizden emin sağlar.  

### <a name="can-an-on-premises-devices-ip-address-that-is-connected-through-azure-virtual-network-gatewayvpn-or-express-route-gateway-access-azure-cosmos-db-account"></a>Azure sanal ağı gateway(VPN) veya Express route ağ geçidi bağlanan bir şirket içi cihazın IP adresi, Azure Cosmos DB hesabına erişebilir miyim?  

Şirket içi cihazın IP adresi veya IP adresi aralığı Azure Cosmos DB hesabınıza erişmek için statik IP listesine eklenmesi gerekir.  

### <a name="what-happens-if-you-delete-a-virtual-network-that-has-service-endpoint-setup-for-azure-cosmos-db"></a>Azure Cosmos DB için hizmet uç noktası Kurulum olan bir sanal ağa sildiğinizde ne olur?  

Bir sanal ağ silindiğinde, ACL bilgileri Azure Cosmos DB silinir ve sanal ağ ve Azure Cosmos DB hesabı arasındaki etkileşimi kaldırır ilişkili. 

### <a name="what-happens-if-an-azure-cosmos-db-account-that-has-virtual-network-service-endpoint-enabled-is-deleted"></a>Sanal ağ hizmet uç noktaları etkinleştirilmiş olan bir Azure Cosmos DB hesabı, ne silindi mi?

Alt ağından belirli bir Azure Cosmos DB hesabı ile ilişkili meta verileri silinir. Ve bunu kullanılan sanal ağ ve alt ağı silmek için son kullanıcının sorumluluğundadır.

### <a name="can-i-use-a-peered-virtual-network-to-create-service-endpoint-for-azure-cosmos-db"></a>Azure Cosmos DB için hizmet uç noktası oluşturmak için eşlenmiş bir sanal ağ kullanabilir miyim?  

Hayır, yalnızca Azure Cosmos DB hizmet uç noktaları, doğrudan sanal ağ ve alt ağlarını oluşturabilir.

### <a name="what-happens-to-the-source-ip-address-of-resource-like-virtual-machine-in-the-subnet-that-has-azure-cosmos-db-service-endpoint-enabled"></a>Azure Cosmos DB hizmet uç noktası etkin olan bir alt ağa sanal makine gibi kaynak kaynak IP adresine ne olur?

Sanal ağ hizmet uç noktaları etkin olduğunda, sanal ağınızın alt ağdaki kaynaklara kaynak IP adresleri genel IPv4 adresleri Azure sanal ağ özel adreslerini kullanarak trafiği Azure Cosmos DB için geçiş yapar.

### <a name="does-azure-cosmos-db-reside-in-the-azure-virtual-network--provided-by-the-customer"></a>Azure Cosmos DB, müşteri tarafından sağlanan Azure sanal ağı içinde yer almıyor?  

Azure Cosmos DB, genel bir IP adresi ile çok kiracılı bir hizmettir. Hizmet uç noktası özelliğini kullanarak bir Azure sanal ağ alt ağı için erişimi kısıtlamak, Azure Cosmos DB hesabınıza verilen Azure sanal ağ ve onun alt aracılığıyla erişimi sınırlıdır.  Azure Cosmos DB hesabı, Azure sanal ağında bulunmuyor. 

### <a name="what-if-anything-will-be-logged-in-log-analyticsoms-if-it-is-enabled"></a>Ne herhangi bir şey etkinleştirilirse, Log Analytics/OMS kaydedilir?  

Azure Cosmos DB, ACL tarafından engellendi durumu 403 isteği için IP adresi (olmadan son sekizli) günlükleriyle gönderir.  

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB için bkz. bir güvenlik duvarını [güvenlik duvarı desteği](firewall-support.md) makalesi.

