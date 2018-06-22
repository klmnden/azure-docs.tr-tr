---
title: Azure sanal ağ hizmeti uç noktası kullanarak bir Azure Cosmos DB hesabına erişimi güvenli hale getirme | Microsoft Docs
description: Bu belgede Kurulum Azure Cosmos DB sanal ağ hizmeti uç noktası için gerekli adımlar açıklanmaktadır.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: govindk
ms.openlocfilehash: 76387733b1511593280f4a9439f5ddbf12d60975
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302011"
---
# <a name="secure-access-to-an-azure-cosmos-db-account-by-using-azure-virtual-network-service-endpoint"></a>Azure sanal ağ hizmeti uç noktası kullanarak bir Azure Cosmos DB hesabı güvenli erişim

Azure CosmosDB hesapları yalnızca Azure sanal ağının belirli alt erişime izin verecek şekilde yapılandırılabilir. Etkinleştirerek bir [Hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) CosmosDB Azure için sanal ağ ve onun alt ağ trafiği Azure Cosmos DB en iyi ve güvenli bir rotaya sağlamış.  

Azure Cosmos DB global olarak dağıtılmış, çok modelli bir veritabanıdır. Azure Cosmos DB hesabında mevcut verileri birden çok bölgeye çoğaltabilirsiniz. Azure Cosmos DB sanal ağ hizmeti uç noktası ile yapılandırıldığında, her sanal ağ erişim specicifc alt ağa ait IP'leri izin verir. Aşağıdaki resimde bir sanal ağ hizmeti uç noktası etkin olan bir Azure Cosmos DB çizimi gösterilmektedir:

![sanal ağ hizmet uç noktası mimarisi](./media/vnet-service-endpoint/vnet-service-endpoint-architecture.png)

Sanal Ağ Hizmeti uç noktası ile bir Azure Cosmos DB hesap yapılandırıldıktan sonra yalnızca belirtilen alt ağ üzerinden erişilebilir, tüm ortak/Internet erişimi kaldırılır. İçinde hizmet uç noktaları hakkında ayrıntılı bilgi edinmek için Azure başvuran [sanal ağ hizmet uç noktaları genel bakış](../virtual-network/virtual-network-service-endpoints-overview.md) makalesi.

> [!NOTE]
> Şu anda sanal ağ hizmet uç noktaları Azure Cosmos DB SQL API veya Mongo API hesapları için yapılandırılabilir. Diğer API'leri ve Azure Almanya veya Azure kamu gibi sovereign Bulutları için hizmet uç noktaları yapılandırmanıza olanak, yakında kullanıma sunulacaktır. Azure Cosmos DB hesabınız için yapılandırılan mevcut bir IP güvenlik duvarınız varsa, lütfen güvenlik duvarı yapılandırması unutmayın, IP Güvenlik Duvarı'nı kaldırın ve Hizmeti uç noktası ACL yapılandırın. Hizmet uç noktası yapılandırdıktan sonra IP Güvenlik Duvarı'nı gerekirse yeniden etkinleştirebilirsiniz.

## <a name="configure-service-endpoint-by-using-azure-portal"></a>Azure portalını kullanarak hizmet uç noktasını yapılandırma
### <a name="configure-service-endpoint-for-an-existing-azure-virtual-network-and-subnet"></a>Hizmet uç noktası bir var olan Azure sanal ağ ve alt ağ için yapılandırma

1. Gelen **tüm kaynakları** dikey penceresinde, sanal ağ bulma hizmeti uç noktası için Azure Cosmos DB yapılandırmak istediğiniz. Gidin **hizmet uç noktaları** dikey ve sanal ağ alt "Azure.CosmosDB" hizmet uç noktası için etkinleştirilmiş olduğundan emin olun.  

   ![Hizmet uç noktası etkin onaylayın](./media/vnet-service-endpoint/confirm-service-endpoint-enabled.png)

2. Gelen **tüm kaynakları** Bul Azure Cosmos DB hesap dikey penceresinde istediğiniz güvenli hale getirmek.  

3. Sanal Ağ Hizmeti uç noktası etkinleştirmeden önce gelecekteki kullanım için Azure Cosmos DB hesabınızla ilişkili IP Güvenlik Duvarı bilgileri kopyalayın. IP Güvenlik Duvarı Hizmeti uç noktası yapılandırdıktan sonra yeniden etkinleştirebilirsiniz.  

4. Seçin **güvenlik duvarları ve sanal ağlar** ayarlar menüsünde ve erişime izin verecek **seçili ağları**.  

3. Sanal ağlar altında bir varolan sanal ağın alt ağ erişimi vermek için seçin **mevcut Azure sanal ağı eklemek**.  

4. Seçin **abonelik** Azure sanal ağı eklemek istediğiniz. Azure seçin **sanal ağlar** ve **alt ağlar** Azure Cosmos DB hesabınıza erişim sağlamak istiyor. Ardından **etkinleştirmek** "Microsoft.AzureCosmosDB" için seçilen ağ hizmet uç noktaları ile etkinleştirmek için. Bu tamamlandığında seçin **Ekle**.  

   ![Sanal ağ ve alt ağ seçin](./media/vnet-service-endpoint/choose-subnet-and-vnet.png)

   > [!NOTE]
   > Hizmet uç noktası Azure Cosmos DB için daha önce seçilen Azure sanal ağları ve alt ağlar için yapılandırılmamışsa, bu işlemin bir parçası olarak yapılandırılabilir. Erişimi etkinleştirme tamamlanması 15 dakika sürer. IP Güvenlik Duvarı'nı renabling güvenlik duvarı ACL içeriğini aşağı bunları daha sonra belirtmeye sonra devre dışı bırakmak çok önemlidir. 

   ![sanal ağ ve alt ağ başarıyla yapılandırıldı](./media/vnet-service-endpoint/vnet-and-subnet-configured-successfully.png)

Şimdi Azure Cosmos DB hesabınızı yalnızca bu seçilen alt ağ trafiğine izin verir. Daha önce IP Güvenlik Duvarı etkin, Lütfen bunları önceki bilgileri kullanarak yeniden etkinleştirin.

### <a name="configure-service-endpoint-for-a-new-azure-virtual-network-and-subnet"></a>Hizmet uç noktası yeni Azure sanal ağ ve alt ağ için yapılandırma

1. Gelen **tüm kaynakları** Bul Azure Cosmos DB hesap dikey penceresinde istediğiniz güvenli hale getirmek.  

> [!NOTE]
> Azure Cosmos DB hesabınız için yapılandırılan mevcut bir IP güvenlik duvarınız varsa, lütfen güvenlik duvarı yapılandırması unutmayın, IP Güvenlik Duvarı'nı kaldırın ve ardından hizmet uç noktası etkinleştirin. Hizmet uç noktası olmadan disbling Güvenlik Duvarı'nı etkinleştirirseniz, bu IP aralığı trafiği sanal IP kimlik kaybedeceksiniz ve bir IP Filtresi hata iletisiyle bırakıldı. Bu hatayı önlemek için her zaman güvenlik duvarı kuralları devre dışı bırakmanız gerekir böylece bunları kopyalayabilir, alt ağ ve son olarak ACL alt ağdan Cosmos DB Hizmeti uç noktası etkinleştirin. Hizmet uç noktası yapılandırmak ve ACL ekleyin sonra IP Güvenlik Duvarı'nı yeniden gerektiğinde yeniden etkinleştirebilirsiniz.

2. Sanal Ağ Hizmeti uç noktası etkinleştirmeden önce gelecekteki kullanım için Azure Cosmos DB hesabınızla ilişkili IP Güvenlik Duvarı bilgileri kopyalayın. IP Güvenlik Duvarı Hizmeti uç noktası yapılandırdıktan sonra yeniden etkinleştirebilirsiniz.  

3. Seçin **güvenlik duvarları ve Azure sanal ağlar** ayarlar menüsünde ve erişime izin verecek **seçili ağları**.  

4. Yeni bir Azure sanal ağı, sanal ağlar altında erişim vermek için seçin **Ekle yeni bir sanal ağ**.  

5. Yeni bir sanal ağ oluşturmak için gereken ayrıntıları sağlayın ve Oluştur'u seçin. Alt Ağ Hizmeti uç noktası ile "etkin Microsoft.AzureCosmosDB" için oluşturulur.

   ![Yeni sanal ağ için sanal ağ ve alt seçin](./media/vnet-service-endpoint/choose-subnet-and-vnet-new-vnet.png)

## <a name="allow-access-from-azure-portal"></a>Azure Portalı'ndan erişime izin ver

Azure Virtual Network hizmet uç noktaları Azure Cosmos DB veritabanı hesabınız için etkinleştirildikten sonra portal veya diğer Azure hizmetlerine erişim varsayılan olarak devre dışıdır. Portalından erişim dahil olmak üzere yapılandırılmış alt ağı dışından makinelerden Azure Cosmos DB veritabanı hesabınıza erişim engellenir.

![Portal'dan erişime izin ver](./media/vnet-service-endpoint/allow-access-from-portal.png)

Azure Cosmos DB hesabınızı Azure Search gibi diğer Azure Hizmetleri tarafından kullanılan veya akış analizi veya Power BI erişilen, denetleyerek erişim izni **Azure hizmetlerine erişime izin ver**.

Erişiminiz Azure Cosmos DB ölçümleri portaldan emin olmak için etkinleştirmeniz gerekir **Azure portalına erişim izin** seçenekleri. Bu seçenekler hakkında daha fazla bilgi için bkz: [Azure portalından bağlantıları](firewall-support.md#connections-from-the-azure-portal) ve [Azure PaaS Hizmetleri bağlantılarından](firewall-support.md#connections-from-public-azure-datacenters-or-azure-paas-services) bölümler. Erişim seçtikten sonra seçin **kaydetmek** ayarları kaydetmek için.

## <a name="remove-a-virtual-network-or-subnet"></a>Bir sanal ağ veya alt ağı Kaldır 

1. Gelen **tüm kaynakları** dikey penceresinde, hizmet uç noktaları atadığınız Bul Azure Cosmos DB hesabı.  

2. Seçin **güvenlik duvarları ve sanal ağlar** ayarları menüsünden.  

3. Bir sanal ağ veya alt ağ kuralı kaldırmak için seçin sanal ağ veya alt ağ ve select yanındaki "..." **kaldırmak**.

   ![Bir sanal ağ Kaldır](./media/vnet-service-endpoint/remove-a-vnet.png)

4.  Tıklatın **kaydetmek** değişikliklerinizi uygulamak için.

## <a name="configure-service-endpoint-by-using-azure-powershell"></a>Azure PowerShell kullanarak hizmet uç noktası yapılandırma 

Azure PowerShell kullanarak Azure Cosmos DB hesabına hizmet uç noktası yapılandırmak için aşağıdaki adımları kullanın:  

1. En son yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) ve [oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).  IP Güvenlik Duvarı ayarlarını not alın ve hesabın hizmet uç noktası etkinleştirmeden önce IP Güvenlik Duvarı'nı tamamen silmek emin olun.


> [!NOTE]
> Azure Cosmos DB hesabınız için yapılandırılan mevcut bir IP güvenlik duvarınız varsa, lütfen güvenlik duvarı yapılandırması unutmayın, IP Güvenlik Duvarı'nı kaldırın ve ardından hizmet uç noktası etkinleştirin. Hizmet uç noktası olmadan disbling Güvenlik Duvarı'nı etkinleştirirseniz, bu IP aralığı trafiği sanal IP kimlik kaybedeceksiniz ve bir IP Filtresi hata iletisiyle bırakıldı. Bu hatayı önlemek için her zaman güvenlik duvarı kuralları devre dışı bırakmanız gerekir böylece bunları kopyalayabilir, alt ağ ve son olarak ACL alt ağdan Cosmos DB Hizmeti uç noktası etkinleştirin. Hizmet uç noktası yapılandırmak ve ACL ekleyin sonra IP Güvenlik Duvarı'nı yeniden gerektiğinde yeniden etkinleştirebilirsiniz.

2. Sanal Ağ Hizmeti uç noktası etkinleştirmeden önce gelecekteki kullanım için Azure Cosmos DB hesabınızla ilişkili IP Güvenlik Duvarı bilgileri kopyalayın. Hizmet uç noktası yapılandırdıktan sonra IP Güvenlik Duvarı'nı yeniden olanak sağlar.  

3. Hizmet uç noktası mevcut bir alt sanal ağ için etkinleştirin.  

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

4. CosmosDB hesabında sanal ağ ve alt ağ hizmeti uç noktası için Azure Cosmos DB etkin olmasını sağlayarak ACL etkinleştirme için hazırlanın.

   ```powershell
   $vnProp = Get-AzureRmVirtualNetwork `
     -Name $vnName  -ResourceGroupName $rgName
   ```

5. Aşağıdaki cmdlet'i çalıştırarak Azure Cosmos DB hesabının özelliklerini alın:  

   ```powershell
   $apiVersion = "2015-04-08"
   $acctName = "<Azure Cosmos DB account name>"

   $cosmosDBConfiguration = Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName
   ```

6. Daha sonra kullanmak için değişkenler başlatır. Birden çok konumda varsa, mevcut hesabı tanımından tüm değişkenlerini ayarlamak, dizi bir parçası olarak eklemeniz gerekir. Bu adımda, "True" "accountVNETFilterEnabled" değişkeni ayarlayarak sanal ağ hizmeti uç noktası da yapılandırın. Bu değer daha sonra "isVirtualNetworkFilterEnabled" parametresi atanır.  

   ```powershell
   $locations = @(@{})
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

7. Azure Cosmos DB hesap özellikleri, aşağıdaki cmdlet'leri çalıştırarak yeni yapılandırma ile güncelleştir: 

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

8. Önceki adımda yapılandırdığınız sanal ağ hizmeti uç noktası ile Azure Cosmos DB hesabınızı güncelleştirilmiş olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

   ```powershell
   $UpdatedcosmosDBConfiguration = Get-AzureRmResource `
     -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
     -ApiVersion $apiVersion `
     -ResourceGroupName $rgName `
     -Name $acctName

   $UpdatedcosmosDBConfiguration.Properties
   ```

## <a name="add-virtual-network-service-endpoint-for-an-azure-cosmos-db-account-that-has-ip-firewall-enabled"></a>IP Güvenlik Duvarı etkin olan bir Azure Cosmos DB hesabı için sanal ağ hizmeti uç noktası ekleme

1. İlk Azure Cosmos DB hesabına IP Güvenlik Duvarı erişimi devre dışı bırakın.  

2. Sanal ağ uç noktası Azure Cosmos DB hesabına önceki bölümlerde açıklanan yöntemlerden birini kullanarak etkinleştirin.  

3. IP güvenlik duvarı erişiminin yeniden etkinleştirin. 

## <a name="test-the-access"></a>Erişimi test edin

Hizmet uç noktaları için Azure Cosmos DB beklendiği şekilde yapılandırılıp yapılandırılmadığını kontrol etmek için aşağıdaki adımları kullanın:

* Ön uç ve arka uç, etkinleştirme gibi Cosmos DB Hizmeti uç noktası arka uç alt ağ için bir sanal ağda iki alt ayarlayın.  

* Arka uç alt ağ için Cosmos DB hesabındaki erişimi etkinleştirin.  

* İki sanal makineleri bir arka uç alt ağı ve başka bir ön uç alt ağ oluşturun.  

* Her iki sanal makinelerden Azure Cosmos DB hesap erişmeyi deneyin. Arka uç alt ağda oluşturulan sanal makineden ancak ön uç alt ağda oluşturulan bir bağlantı olması gerekir. İstek sanal ağ hizmeti uç noktası kullanarak Azure Cosmos DB erişimi güvenli onaylar ön uç alt ağından bağlanmayı denediğinizde 404 ile kullanıma alma hatası olur. Arka uç alt makine uygundur mümkün olacaktır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-happens-when-you-access-an-azure-cosmos-db-account-that-has-virtual-network-access-control-list-acl-enabled"></a>Sanal ağ erişim denetimi listesi (ACL) etkin olan bir Azure Cosmos DB hesap erişim ne olur?  

HTTP 404 hata döndürülür.  

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-cosmos-db-account-in-another-region-for-example-if-azure-cosmos-db-account-is-in-west-us-or-east-us-and-virtual-networks-are-in-multiple-regions-can-the-virtual-network-access-azure-cosmos-db"></a>Başka bir bölgede bir Azure Cosmos DB hesaba erişmek için izin verilen farklı bölgelerdeki oluşturulan bir sanal ağ alt ağların misiniz? Örneğin, Batı ABD veya Doğu ABD ve sanal ağın Azure Cosmos DB hesabı ise, birden çok bölgede sanal ağı Azure Cosmos DB erişebilir misiniz?  

Evet, sanal ağlar farklı bölgelerde oluşturulan yeni yeteneği erişebilir. 

### <a name="can-an-azure-cosmos-db-account-have-both-virtual-network-service-endpoint-and-a-firewall"></a>Bir Azure Cosmos DB hesap sanal ağ hizmeti uç noktası ve bir güvenlik duvarı olabilir?  

Evet, sanal ağ hizmeti uç noktası ve bir Güvenlik Duvarı'nı birlikte bulunabilir. Genel olarak, portalına erişim her zaman bir sanal ağ hizmeti uç noktası yapılandırmadan önce kapsayıcı ile ilişkili ölçümleri görüntülemek üzere etkinleştirmek için etkin olduğundan emin olmanız gerekir.

### <a name="can-i-allow-access-to-other-azure-services-from-a-given-azure-region-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>"Access diğer Azure hizmetlerine belirli bir Azure bölgesinden İzin verdiklerim" hizmet uç noktası erişimi için Azure Cosmos DB etkinleştirildiğinde?  

Azure Data factory, Azure Search veya herhangi bir hizmet gibi diğer Azure birinci taraf tarafından erişilecek Azure Cosmos DB hesabınızı istediğinizde Hizmetleri yalnızca bu gereklidir Azure bölgesi dağıtılır.

### <a name="how-many-virtual-network-service-endpoints-are-allowed-for-azure-cosmos-db"></a>Kaç tane sanal ağ hizmet uç noktaları için Azure Cosmos DB izin verilir?  

64 sanal ağ hizmet uç noktaları için bir Azure Cosmos DB hesap izin verilir.

### <a name="what-is-the-relationship-between-service-endpoint-and-network-security-group-nsg-rules"></a>Hizmet uç noktası ve ağ güvenlik grubu (NSG) kurallarını arasındaki ilişki nedir?  

NSG kuralları Azure Cosmos veritabanı, belirli Azure Cosmos DB IP adresi aralığına erişimini kısıtlamak izin verir. Belirli bir mevcut olan bir Azure Cosmos DB örneği erişmesine izin vermek istiyorsanız, [bölge](https://azure.microsoft.com/global-infrastructure/regions/), bölge şu biçimde belirtin: 

    AzureCosmosDB.<region name>

Bkz: etiketler NSG hakkında daha fazla bilgi edinmek için [sanal ağ hizmeti etiketleri](../virtual-network/security-overview.md#service-tags) makalesi. 
  
### <a name="what-is-relationship-between-an-ip-firewall-and-virtual-network-service-endpoint-capability"></a>IP Güvenlik Duvarı'nı ve sanal ağ hizmeti uç noktası özelliğini arasındaki ilişki nedir?  

Bu iki özellik diğer Azure Cosmos DB varlıklar yalıtımı sağlamak ve bunları güvenli tamamlar. IP kullanan, güvenlik duvarı statik IP Azure Cosmos DB hesap erişebilmesini sağlar.  

### <a name="can-an-on-premise-devices-ip-address-that-is-connected-through-azure-virtual-network-gatewayvpn-or-express-route-gateway-access-azure-cosmos-db-account"></a>Azure Virtual Network gateway(VPN) veya hızlı rota ağ geçidi bağlı bir şirket içi aygıtın IP adresi, Azure Cosmos DB hesap erişebilir mi?  

Şirket içi aygıtın IP adresi veya IP adresi aralığı Azure Cosmos DB hesabınıza erişmeniz için statik IP listesine eklenmesi gerekir.  

### <a name="what-happens-if-you-delete-a-virtual-network-that-has-service-endpoint-setup-for-azure-cosmos-db"></a>Hizmet uç noktası kurulumu için Azure Cosmos DB olan bir sanal ağ silersem ne olur?  

Bir sanal ağ silindiğinde, ACL bilgilerini Azure Cosmos DB silinir ve sanal ağ ve Azure Cosmos DB hesap arasındaki etkileşimi kaldırır ilişkili. 

### <a name="what-happens-if-an-azure-cosmos-db-account-that-has-virtual-network-service-endpoint-enabled-is-deleted"></a>Sanal Ağ Hizmeti uç noktası etkin olan bir Azure Cosmos DB hesap varsa olanlar silinir mi?

Belirli Azure Cosmos DB hesabıyla ilişkili meta veri alt ağdan silinir. Ve bu alt ağ ve kullanılan sanal ağı silmek için son kullanıcının sorumluluğundadır.

### <a name="can-i-use-a-peered-virtual-network-to-create-service-endpoint-for-azure-cosmos-db"></a>Hizmet uç noktası için Azure Cosmos DB oluşturmak için eşlenmiş bir sanal ağ kullanabilir miyim?  

Hayır, yalnızca doğrudan sanal ağ ve alt ağlarını Azure Cosmos DB hizmet uç noktaları oluşturabilir.

### <a name="what-happens-to-the-source-ip-address-of-resource-like-virtual-machine-in-the-subnet-that-has-azure-cosmos-db-service-endpoint-enabled"></a>Kaynak sanal makine Azure Cosmos DB Hizmeti uç noktası etkin olan bir alt ağ gibi kaynak IP adresine ne olur?

Sanal ağ hizmet uç noktaları etkinleştirildiğinde, sanal ağınızın alt ağ kaynaklarında kaynak IP adreslerini Azure Cosmos DB trafik için Azure sanal ağın özel adresleri bir ortak IPv4 adresi kullanarak geçiş yapar.

### <a name="does-azure-cosmos-db-reside-in-the-azure-virtual-network--provided-by-the-customer"></a>Azure Cosmos DB müşteri tarafından sağlanan Azure sanal ağı bulunur mu?  

Azure Cosmos DB, bir ortak IP adresine sahip bir çok kiracılı hizmetidir. Hizmet uç noktası özelliğini kullanarak bir Azure sanal ağ alt ağa erişimi kısıtladığınızda, belirtilen Azure sanal ağı ve kendi alt ağı aracılığıyla Azure Cosmos DB hesabınız için erişim sınırlıdır.  Azure Cosmos DB hesabı, Azure sanal ağında bulunmuyor. 

### <a name="what-if-anything-will-be-logged-in-log-analyticsoms-if-it-is-enabled"></a>Ne herhangi bir şey etkinleştirilirse, günlük analizi/OMS kaydedilir?  

Azure Cosmos DB durumundaki ACL tarafından engellenen 403 isteği için IP adresi (olmadan son sekizli) günlükleriyle gönderir.  

## <a name="next-steps"></a>Sonraki adımlar
Bir Güvenlik Duvarı'nı Azure Cosmos DB bakın yapılandırmak için [güvenlik duvarı desteği](firewall-support.md) makalesi.

