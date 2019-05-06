---
title: Azure Cosmos DB hesabınız için bir IP Güvenlik Duvarı yapılandırma
description: Azure Cosmos DB veritabanı hesaplarında güvenlik duvarı desteği için IP erişim denetim ilkeleri yapılandırmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: cdf2da745cc418190f6546fffc03e2ac2c330e0e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65068729"
---
# <a name="configure-ip-firewall-in-azure-cosmos-db"></a>Azure Cosmos DB'de IP Güvenlik Duvarı yapılandırma

IP Güvenlik Duvarı'nı kullanarak Azure Cosmos DB hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Azure Cosmos DB için gelen güvenlik duvarı desteği IP tabanlı erişim denetimlerini destekler. Aşağıdaki yöntemlerden birini kullanarak bir IP Güvenlik Duvarı Azure Cosmos DB hesabı ayarlayabilirsiniz:

* Azure portalından
* Bildirimli olarak bir Azure Resource Manager şablonu kullanarak
* Program aracılığıyla güncelleştirerek Azure PowerShell ve Azure CLI aracılığıyla **ipRangeFilter** özelliği

## <a id="configure-ip-policy"></a> Azure portalını kullanarak bir IP Güvenlik Duvarı yapılandırma

Azure portalında IP erişim denetim ilkesini ayarlamak için Azure Cosmos DB hesap sayfasına gidin ve seçin **güvenlik duvarı ve sanal ağlar** Gezinti menüsünde. Değişiklik **erişime izin verecek** değerini **seçili ağlar**ve ardından **Kaydet**. 

![Azure portalında güvenlik duvarı sayfasını açmak gösteren ekran görüntüsü](./media/how-to-configure-firewall/azure-portal-firewall.png)

Azure portalı, IP erişim denetimi etkinleştirildiğinde, IP adresleri, IP adresi aralıkları ve anahtarları belirtme olanağı sağlar. Anahtarlar, diğer Azure Hizmetleri ve Azure portalına erişim sağlar. Aşağıdaki bölümlerde bu anahtarları hakkında ayrıntılar verir.

> [!NOTE]
> Azure Cosmos DB hesabınız için bir IP erişim denetimi İlkesi etkinleştirdikten sonra izin verilen IP adresi aralıkları listesi dışındaki makinelerden Azure Cosmos DB hesabınız için tüm istekleri reddedilir. Portaldan Azure Cosmos DB kaynaklarını gözatma erişim denetimi bütünlüğünü sağlamak için de engellenir.

### <a name="allow-requests-from-the-azure-portal"></a>Azure portalından isteklere izin ver

Program aracılığıyla bir IP erişim denetimi İlkesi etkinleştirdiğinizde, Azure portalında IP adresi eklemeniz gereken **ipRangeFilter** erişimi sürdürmek için özellik. Portal IP adreslerini şunlardır:

|Bölge|IP adresi|
|------|----------|
|Almanya|51.4.229.218|
|Çin|139.217.8.252|
|US Gov|52.244.48.71|
|Diğer tüm bölgeler|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Azure portalına erişim'i seçerek etkinleştirebilirsiniz **Azure portalından erişim izni** , aşağıdaki ekran görüntüsünde gösterildiği gibi seçeneği: 

![Azure portal erişimi etkinleştirme gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Küresel Azure veri merkezleri veya azure'daki diğer kaynakları gelen isteklere izin ver

Azure Cosmos DB hesabınızın (örneğin, Azure Stream Analytics ve Azure işlevleri) statik bir IP sağlamayan hizmetlerinden erişirseniz, IP Güvenlik Duvarı erişimi sınırlamak için kullanmaya devam edebilirsiniz. Bu hizmetlerden Azure Cosmos DB hesabına erişmesine izin verilen IP adreslerinin listesi için IP adresi 0.0.0.0 ekleyin. 0.0.0.0 adresi Azure veri merkezi IP aralığından Azure Cosmos DB hesabınıza istekleri kısıtlar. Bu ayar, Azure Cosmos DB hesabınıza başka bir IP aralıkları için erişim izin vermez.

> [!NOTE]
> Bu seçenek, Azure, Azure'da dağıtılan diğer müşterilerin aboneliklerinden gelen istekleri dahil olmak üzere tüm isteklere izin vermek için güvenlik duvarını yapılandırır. Bu seçeneği tarafından izin verilen IP'lerin genelinde olduğundan bir güvenlik duvarı ilkesi etkisini sınırlar. Bu seçenek yalnızca isteklerinizi statik IP ya da sanal ağlardaki alt ağlara kaynaklanan yoksa kullanın. Azure portalı, Azure'a dağıtılmış olduğundan otomatik olarak bu seçeneğin belirlenmesi, Azure portalından erişime izin verir.

Azure portalına erişim'i seçerek etkinleştirebilirsiniz **genel Azure veri merkezlerinin içinde gelen bağlantıları kabul etmesi** , aşağıdaki ekran görüntüsünde gösterildiği gibi seçeneği: 

![Azure portalında güvenlik duvarı sayfasını açmak gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Geçerli IP gelen istekleri

Geliştirmeyi basitleştirmek için Azure portalında tanımlamanıza ve istemci makinenizin IP izin verilenler listesine ekleme yardımcı olur. Makinenizde çalışan uygulamalar, daha sonra Azure Cosmos DB hesabınıza erişebilirsiniz. 

Portal, istemci IP adresini otomatik olarak algılar. Makinenizin istemci IP adresini veya ağ geçidinizin IP adresi olabilir. Üretim iş yüklerinize olabilmesi bu IP adresi kaldırdığınızdan emin olun. 

Geçerli IP IP'ler listesine eklemek için seçin **geçerli IP'mi Ekle**. Daha sonra **Kaydet**’e tıklayın.

![Bir gösteren ekran görüntüsü geçerli IP Güvenlik Duvarı ayarlarını yapılandırmak için](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Bulut hizmetleri gelen istekleri

Azure'da, bulut Hizmetleri için Azure Cosmos DB kullanarak orta katman hizmet mantığını barındıran ortak yoludur. Seçeneğinden bulut hizmetini Azure Cosmos DB hesabınıza erişimi etkinleştirmek için bulut hizmeti genel IP adresini Azure Cosmos DB hesabınız ile ilişkili IP adreslerine izin verilen listesine eklemelisiniz [IPerişimdenetimiilkesiyapılandırma](#configure-ip-policy). Bu, bulut hizmetlerinin tüm rol örneklerine Azure Cosmos DB hesabınıza erişimi olmasını sağlar. 

Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında bulut hizmetleriniz için IP adresi alabilirsiniz:

![Azure Portalı'nda görüntülenen bir bulut hizmeti için genel IP adresini gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses.png)

Rol örnekleri ekleyerek bulut hizmetinizi ölçeklendirin, bunlar aynı bulut hizmetine bir parçası olduğunuz için bu yeni örnekleri otomatik olarak Azure Cosmos DB hesabına erişebilir.

### <a name="requests-from-virtual-machines"></a>Sanal makinelerden istekleri

Ayrıca [sanal makineler](https://azure.microsoft.com/services/virtual-machines/) veya [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Azure Cosmos DB kullanarak orta katman hizmetlerini barındırmak için. Sanal makinelerden erişim sağlar, Cosmos DB hesabınızın yapılandırmak için genel IP adresini, sanal makine ve/veya sanal makine ölçek kümesi Azure Cosmos DB hesabınız için izin verilen IP adreslerini biri olarak yapılandırın [ IP erişim denetimi ilkesini yapılandırma](#configure-ip-policy). 

Azure portalında, sanal makineler için IP adresleri aşağıdaki ekran görüntüsünde gösterildiği gibi alabilirsiniz:

![Azure Portalı'nda görüntülenen bir sanal makine için genel bir IP adresi gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Gruba birini eklediğinizde, bu sanal makine örnekleri otomatik olarak Azure Cosmos DB hesabınıza erişimi alırlar.

### <a name="requests-from-the-internet"></a>İnternet'ten gelen istekleri

Azure Cosmos DB hesabınızın internetteki bir bilgisayar eriştiğinizde, istemci IP adresini veya IP adresi aralığı makinenin IP adreslerini hesabınız için izin verilen listesine eklenmelidir.

## <a id="configure-ip-firewall-arm"></a>Resource Manager şablonu kullanarak bir IP Güvenlik Duvarı yapılandırma

Azure Cosmos DB hesabınıza erişim denetimini yapılandırmak için Resource Manager şablonu belirttiğinden emin olun **ipRangeFilter** özniteliği izin verilen IP aralıkları listesi ile. Örneğin, şablonunuza aşağıdaki JSON kodunu ekleyin:

```json
   {
     "apiVersion": "2015-04-08",
     "type": "Microsoft.DocumentDB/databaseAccounts",
     "kind": "GlobalDocumentDB",
     "name": "[parameters('databaseAccountName')]",
     "location": "[resourceGroup().location]",
     "properties": {
       "databaseAccountOfferType": "Standard",
       "name": "[parameters('databaseAccountName')]",
       "ipRangeFilter":"183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
     }
   }
```

## <a id="configure-ip-firewall-cli"></a>Azure CLI kullanarak bir IP erişim denetimi ilkesini yapılandırma

Aşağıdaki komut, IP erişim denetimi ile bir Azure Cosmos DB hesabı oluşturma işlemini gösterir: 

```azurecli-interactive

name="<Azure Cosmos DB account name>"
resourceGroupName="<Resource group name>"

az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
```

Hesabınız için Güvenlik Duvarı ayarlarını güncelleştirmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255,104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="configure-ip-firewall-ps"></a>PowerShell kullanarak bir IP erişim denetimi ilkesini yapılandırma

Aşağıdaki betiği, IP erişim denetimi ile bir Azure Cosmos DB hesabı oluşturma işlemi gösterilmektedir:

```azurepowershell-interactive

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

# Add local machine's IP address to firewall, InterfaceAlias is your Network Adapter's name
$ipRangeFilter = Get-NetIPConfiguration | Where-Object InterfaceAlias -eq "Ethernet 2" | Select-Object IPv4Address

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "ipRangeFilter"=$ipRangeFilter
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

## <a id="troubleshoot-ip-firewall"></a>Bir IP erişim denetimi İlkesi ile ilgili sorunları giderme

Aşağıdaki seçenekleri kullanarak bir IP erişim denetimi İlkesi ile ilgili sorunları giderebilirsiniz: 

### <a name="azure-portal"></a>Azure portal 
Azure Cosmos DB hesabınız için bir IP erişim denetimi İlkesi etkinleştirerek, tüm istekleri hesabınıza izin verilen IP adresi aralıkları listesi dışındaki makinelerden engelleyin. Kapsayıcıları göz atma ve belgelerin sorgulanmasını gibi portal veri düzlemi işlemlerini etkinleştirmek için kullanarak Azure portala erişim açıkça izin vermeniz gerekir **Güvenlik Duvarı** portalındaki bölmesinde.

### <a name="sdks"></a>SDK’lar 
İzin verilenler listesinde genel olmayan makineler SDK'larını kullanarak Azure Cosmos DB kaynaklarını eriştiğinizde **403 Yasak** ek ayrıntı yok yanıt döndürülür. Hesabınız için izin verilen IP listesi doğrulayın ve doğru ilke yapılandırmasının Azure Cosmos DB hesabınıza uygulandığından emin olun. 

### <a name="source-ips-in-blocked-requests"></a>Engellenen istekleri kaynak IP'leri
Azure Cosmos DB hesabınızdaki tanılama günlük kaydını etkinleştirin. Bu günlükler her istek ve yanıt gösterir. Güvenlik Duvarı-ilgili iletiler 403 bir dönüş koduyla günlüğe kaydedilir. Bu iletiler filtreleyerek, Engellenen istekler için kaynak IP'leri görebilirsiniz. Bkz: [Azure Cosmos DB tanılama günlüğüne kaydetme](logging.md).

### <a name="requests-from-a-subnet-with-a-service-endpoint-for-azure-cosmos-db-enabled"></a>Azure Cosmos DB için hizmet uç noktası ile bir alt ağdan istekleri etkin
Azure Cosmos DB hesaplarına sanal ağ ve alt ağ kimliği etkin Azure Cosmos DB için hizmet uç noktası olan bir sanal ağa bir alt ağdan gelen istekleri gönderir. IP filtreleri bunları reddetmek için bu istekleri genel IP kaynağı yok. Belirli alt ağlara erişimi sanal ağlarda izin vermek için bir erişim denetim listesi açıklandığı gibi ekleyin [sanal ağ ve alt ağ tabanlı erişim Azure Cosmos DB hesabınız için nasıl yapılandırılacağı](how-to-configure-vnet-service-endpoint.md). Bu güvenlik duvarı kurallarını uygulamak 15 dakika kadar sürebilir.


## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hesabınız için bir sanal ağ hizmet uç noktası yapılandırmak için aşağıdaki makalelere bakın:

* [Azure Cosmos DB hesabınız için sanal ağ ve alt ağ erişim denetimi](vnet-service-endpoint.md)
* [Sanal ağ ve alt ağ tabanlı erişim Azure Cosmos DB hesabınız için yapılandırma](how-to-configure-vnet-service-endpoint.md)

