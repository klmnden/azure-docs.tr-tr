---
title: Nasıl yapılır, Azure Cosmos hesabınız için IP Güvenlik Duvarı yapılandırma
description: Azure Cosmos DB veritabanı hesaplarında güvenlik duvarı desteği için IP erişim denetim ilkeleri yapılandırmayı öğrenin.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: e1253d4a43e45fc7df7efbb6d5f9dd4176957ca2
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629251"
---
# <a name="how-to-configure-ip-firewall-for-your-azure-cosmos-account"></a>Nasıl yapılır, Azure Cosmos hesabınız için IP Güvenlik Duvarı yapılandırma

IP Güvenlik Duvarı'nı kullanarak Azure Cosmos hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Azure Cosmos DB için gelen güvenlik duvarı desteği IP tabanlı erişim denetimlerini destekler. Aşağıdaki yöntemlerden birini kullanarak Azure Cosmos hesabı üzerinde bir IP Güvenlik Duvarı ayarlayabilirsiniz:

1. Azure portalından
2. Bildirimli olarak bir Azure Resource Manager şablonu kullanarak
3. Program aracılığıyla Azure CLI veya güncelleştirerek Azure Powershell aracılığıyla **ipRangeFilter** özelliği.

## <a id="configure-ip-policy"></a> Azure portalını kullanarak IP Güvenlik Duvarı yapılandırma

Azure portalında IP erişim denetim ilkesini ayarlamak için Azure Cosmos DB hesap sayfasına gidin, **güvenlik duvarı ve sanal ağlar** Gezinti menüsünde, ardından değiştirme **erişime izin verecek** değeri için **seçili ağlar**ve ardından **Kaydet**. 

![Azure portalında güvenlik duvarı sayfasını açmak gösteren ekran görüntüsü](./media/how-to-configure-firewall/azure-portal-firewall.png)

IP erişim denetimi etkinleştirildikten sonra Azure portalında IP adresleri, IP adresi aralıklarını yanı sıra diğer Azure Hizmetleri ve Azure portalına erişimi etkinleştirmek için anahtarlar belirtme olanağı sağlar. Bu anahtarları hakkında ayrıntılar aşağıdaki bölümlerde verilmiştir.

> [!NOTE]
> IP erişim denetim ilkesi, Azure Cosmos hesabınız için etkinleştirdikten sonra Azure Cosmos hesabınıza tüm istekler izin verilen IP adresi aralıkları listesi dışındaki makinelerden reddedilir. Portaldan Azure Cosmos DB kaynaklarını gözatma ayrıca olması engellendi erişim denetimi bütünlüğünü sağlamak için.

### <a name="allow-requests-from-the-azure-portal"></a>Azure portalından isteklere izin ver 

Program aracılığıyla bir IP erişim denetimi İlkesi etkinleştirdiğinizde, Azure portalında IP adresi eklemeniz gereken **ipRangeFilter** erişimi sürdürmek için özellik. Portal IP adreslerini şunlardır:

|Bölge|IP adresi|
|------|----------|
|Almanya|51.4.229.218|
|Çin|139.217.8.252|
|US Gov|52.244.48.71|
|Yukarıdaki üç dışındaki tüm bölgeler|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|

Azure portalına erişim'i seçerek etkinleştirebilirsiniz **Azure portalından erişim izni** seçenek aşağıdaki ekran görüntüsünde gösterildiği gibi: 

![Azure portal erişimi etkinleştirme gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-portal.png)

### <a name="allow-requests-from-global-azure-datacenters-or-other-sources-within-azure"></a>Küresel Azure veri merkezleri veya azure'daki diğer kaynakları gelen isteklere izin ver

Örneğin Azure Stream analytics, bir statik IP sağlamayan hizmetlerinden Azure Cosmos hesap erişim Azure işlevleri yine de vb. IP Güvenlik Duvarı erişimi sınırlamak için kullanın. Erişim gibi hizmetlerden Azure Cosmos hesabına izin vermek için bir güvenlik duvarı ayarı etkinleştirilmelidir. Bu güvenlik duvarı ayarını etkinleştirmek için izin verilen IP adreslerinin listesi için IP adresi 0.0.0.0 ekleyin. 0.0.0.0, Azure veri merkezi IP aralığından Azure Cosmos hesabınıza istekleri kısıtlar. Bu ayar, Azure Cosmos hesabınıza başka bir IP aralıkları için erişim izin vermez.

> [!NOTE]
> Bu seçenek, Azure'da dağıtılan diğer müşterilerin aboneliklerinden gelen Azure dahil olmak üzere istekleri gelen tüm isteklerin izin verecek şekilde yapılandırır. Güvenlik Duvarı İlkesi etkisini sınırlar için bu seçeneği tarafından izin verilen IP'lerin geniştir. Yalnızca isteklerinizi statik IP veya alt ağlardaki kaynaklanan yoksa bu seçeneği kullanılmalıdır. Azure portalı, Azure üzerinde dağıtıldığı otomatik olarak bu seçeneğin belirlenmesi, Azure portalından erişim sağlar.

Azure portalına erişim'i seçerek etkinleştirebilirsiniz **genel Azure veri merkezleri gelen bağlantıları kabul etmesi** seçenek aşağıdaki ekran görüntüsünde gösterildiği gibi: 

![Azure portalında güvenlik duvarı sayfasını açmak gösteren ekran görüntüsü](./media/how-to-configure-firewall/enable-azure-services.png)

### <a name="requests-from-your-current-ip"></a>Geçerli IP gelen istekleri

Makinenizde çalışan uygulamaları Azure Cosmos hesabınıza erişebilmesi için geliştirmeyi basitleştirmek için Azure portalında tanımlamanıza ve istemci makinenizin IP izin verilenler listesine ekleme yardımcı olur. İstemci IP adresi, portal tarafından otomatik olarak algılanır. Makinenizin istemci IP adresini veya ağ geçidinizin IP adresi olabilir. Üretim iş yüklerinize almadan önce bu IP adresi kaldırdığınızdan emin olun. 

Geçerli IP etkinleştirmek için seçin **geçerli IP'mi Ekle**, geçerli IP IP'ler listesine ekler ve ardından **Kaydet**.

![Bir gösteren ekran görüntüsü geçerli IP Güvenlik Duvarı ayarlarını yapılandırmak için](./media/how-to-configure-firewall/enable-current-ip.png)

### <a name="requests-from-cloud-services"></a>Bulut hizmetleri gelen istekleri

Azure'da, bulut Hizmetleri Azure Cosmos DB kullanarak orta katman hizmet mantığı barındırmak için genel bir yoludur. Bir bulut hizmetinden Azure Cosmos hesabınıza erişimi etkinleştirmek için bulut hizmeti genel IP adresini Azure Cosmos hesabınız ile ilişkili IP adreslerine izin verilen listesi eklenmelidir [IPerişimdenetimiilkesiyapılandırma](#configure-ip-policy). Bu, tüm rol örneklerine bulut Hizmetleri, Azure Cosmos hesabınıza erişim için sahip olmasını sağlar. Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında bulut hizmetleriniz için IP adresi alabilirsiniz:

![Azure Portalı'nda görüntülenen bir bulut hizmeti için genel IP adresini gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses.png)

Ek rol örnekleri ekleyerek bulut hizmetinizi ölçeklendirin, aynı bulut hizmetine bir parçası olduğundan bu yeni örnekleri otomatik olarak Azure Cosmos hesabına erişebilir.

### <a name="requests-from-virtual-machines"></a>Sanal makinelerden istekleri

[Sanal makineler](https://azure.microsoft.com/services/virtual-machines/) veya [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Azure Cosmos DB kullanarak bir orta katman hizmetlerini barındırmak için de kullanılabilir. Cosmos hesabınızı sanal makinelerden erişime izin verecek şekilde yapılandırmak için genel IP adresleri, sanal makine ve/veya sanal makine ölçek kümesi olarak Azure Cosmos hesabınız için izin verilen IP adreslerinden birini yapılandırılmalıdır [IP yapılandırma erişim denetimi İlkesi](#configure-ip-policy). Azure portalında, sanal makineler için IP adresleri aşağıdaki ekran görüntüsünde gösterildiği gibi alabilirsiniz:

![Azure Portalı'nda görüntülenen bir sanal makine için genel bir IP adresi gösteren ekran görüntüsü](./media/how-to-configure-firewall/public-ip-addresses-dns.png)

Gruba birini eklediğinizde, bu ek sanal makine örnekleri, Azure Cosmos hesabınıza erişimi'ne otomatik olarak sağlanır.

### <a name="requests-from-the-internet"></a>İnternet'ten gelen istekleri

Azure Cosmos hesabınızı internetteki bir bilgisayar eriştiğinizde, istemci IP adresini veya IP adresi aralığı makinenin IP adresi hesabınız için izin verilen listesine eklenmelidir.

## <a id="configure-ip-firewall-arm"></a>Resource Manager şablonu kullanarak IP Güvenlik Duvarı yapılandırma

Azure Cosmos hesabınıza erişim denetimini yapılandırmak için Resource Manager şablonu belirtmelidir **ipRangeFilter** özniteliğiyle beyaz listeye alınması gereken IP aralıklarının listesi. Örneğin, şablonunuza aşağıdaki JSON kodunu ekleyin:

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
     "ipRangeFilter":"183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
   }
```

## <a id="configure-ip-firewall-cli"></a>Azure CLI kullanarak IP erişim denetimi ilkesini yapılandırma

Aşağıdaki komut, IP erişim denetimi ile bir Azure Cosmos hesabı oluşturma işlemini gösterir: 

```azurecli-interactive
az cosmosdb create \
  --name $name \
  --kind GlobalDocumentDB \
  --resource-group $resourceGroupName \
  --max-interval 10 \
  --max-staleness-prefix 200 \
  --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

Hesabınız için Güvenlik Duvarı ayarlarını güncelleştirmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb update \
      --name $name \
      --resource-group $resourceGroupName \
      --ip-range-filter "183.240.196.255, 104.42.195.92,40.76.54.131, 52.176.6.30,52.169.50.45,52.187.184.26"
```

## <a id="troubleshoot-ip-firewall"></a>IP erişim denetim ilkesi sorunlarını giderme

Aşağıdaki seçenekleri kullanarak IP erişim denetim ilkesi sorunlarını giderebilirsiniz: 

### <a name="azure-portal"></a>Azure portal 
Azure Cosmos hesabınız için bir IP erişim denetimi İlkesi etkinleştirerek, tüm istekleri hesabınıza izin verilen IP adresi aralıkları listesi dışındaki makinelerden engellenir. Kapsayıcılar ve sorgu belgelerini gözatma gibi portal veri düzlemi işlemleri etkinleştirmek istiyorsanız, bu nedenle açıkça Azure portalı kullanarak izin vermeniz gerekir **Güvenlik Duvarı** portalındaki bölmesinde.

### <a name="sdks"></a>SDK’lar 
İzin verilenler listesinde olmayan makineler SDK'larını kullanarak Azure Cosmos kaynaklara erişirken **ek ayrıntı yok genel 404 bulunamadı yanıt döndürülür**. Hesabınız için izin verilen IP listesi doğrulayın ve doğru ilke yapılandırması, Cosmos hesabınıza uygulandığından emin olun. 

### <a name="check-source-ips-in-blocked-requests"></a>Engellenen istekleri onay kaynak IP'leri
Etkinleştirme Tanılama, Azure Cosmos hesabınızda oturum, bu günlükler her istek ve yanıt gösterebilir. **Güvenlik Duvarı-ilgili iletiler dahili olarak bir 403 dönüş koduyla oturum**. Bu iletiler filtreleyerek, Engellenen istekler için kaynak IP'leri görebilirsiniz. Bkz: [Azure Cosmos DB tanılama günlüğüne kaydetme](logging.md).

### <a name="requests-from-subnet-with-service-endpoint-for-azure-cosmos-database-enabled"></a>Etkin bir Azure Cosmos veritabanı için hizmet uç noktası ile alt ağından gelen istekleri
Azure Cosmos DB için hizmet uç noktası olan bir sanal ağ içindeki alt ağ gelen istekleri gönderir VNET ve alt ağ kimliğini Azure Cosmos hesapları için etkinleştirildi. IP filtreleri tarafından reddedilir ve bu nedenle bu istekler genel IP kaynağı yok. Sanal ağ erişim denetimi listesi ana hatlarıyla belirtilen belirli alt ağlara erişimi sanal ağlarda izin vermek için ekleme [sanal ağ ve alt ağ tabanlı erişim Azure Cosmos hesabınız için nasıl yapılandırılacağı](how-to-configure-vnet-service-endpoint.md). Bu güvenlik duvarı kurallarını uygulamak 15 dakika kadar sürebilir.


## <a name="next-steps"></a>Sonraki adımlar

Sanal ağ hizmet uç noktaları Azure Cosmos DB hesabınız için yapılandırmak için aşağıdaki makalelere bakın:

* [Azure Cosmos hesabınız için VNET ve alt ağ erişim denetimi](vnet-service-endpoint.md)
* [Sanal ağ ve Azure Cosmos hesabınız için alt ağ erişimi yapılandırma](how-to-configure-vnet-service-endpoint.md)


