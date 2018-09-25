---
title: Azure Cosmos DB güvenlik duvarı desteği ve IP erişim denetimi | Microsoft Docs
description: IP erişim denetim ilkeleri, Azure Cosmos DB veritabanı hesaplarında güvenlik duvarı desteği için kullanmayı öğrenin.
keywords: IP erişim denetimi, güvenlik duvarı desteği
services: cosmos-db
author: kanshiG
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: govindk
ms.openlocfilehash: ebfba4d54b4d4158a2dc0bc2aed09699012ac157
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038060"
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure Cosmos DB güvenlik duvarı desteği
Bir Azure Cosmos DB veritabanı hesabına depolanan verilerinizin güvenliğini sağlamak için Azure Cosmos DB bağlı bir gizli dizi için destek sağlanan [yetkilendirme modelini](https://msdn.microsoft.com/library/azure/dn783368.aspx) , güçlü bir karma tabanlı ileti kimlik doğrulama kodu (HMAC) kullanır. Şimdi, gizli tabanlı yetkilendirme modeli ek olarak, ilke temelli IP tabanlı erişim denetimlerini gelen güvenlik duvarı desteği için Azure Cosmos DB destekler. Bu model, geleneksel veritabanı sistemi güvenlik duvarı kurallarına benzer ve ek bir Azure Cosmos DB veritabanı hesabı için güvenlik düzeyi sağlar. Bu modelde, artık erişilebilir yalnızca onaylanmış bir makine kümesinden ve/veya Bulut Hizmetleri için bir Azure Cosmos DB veritabanı hesabı yapılandırabilirsiniz. Bu onaylı kümelerinden makineleri ve Hizmetleri Azure Cosmos DB kaynaklarına erişimi hala geçerli bir yetkilendirme belirteciyle sunmak çağıranın gerektirir.

## <a name="ip-access-control-overview"></a>IP erişim denetimine genel bakış
İstek, geçerli bir yetkilendirme belirteciyle birlikte sunulduğu sürece, varsayılan olarak bir Azure Cosmos DB veritabanı hesabına genel İnternet’ten erişilebilir. IP ilke tabanlı erişim denetimini yapılandırmak için kullanıcı, CIDR formunda bir grup IP adresi veya IP adresi aralığı sağlamalıdır. Bunlar, belirli bir veritabanı hesabı için izin verilen IP listesi olarak dahil edilecektir. Bu yapılandırma uygulandıktan sonra, bu izin verilen liste dışındaki makinelerden gelen tüm istekler, sunucu tarafından engellenir.  IP tabanlı erişim denetimi için akış işleme bağlantı, aşağıdaki diyagramda açıklanmıştır:

![IP tabanlı erişim denetimi bağlantı işlemini gösteren diyagram](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> IP erişim denetimi ilkesini yapılandırma
IP erişim denetimi ilkesini Azure portalında veya program aracılığıyla aracılığıyla ayarlanabilir [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), veya [REST API](/rest/api/cosmos-db/) güncelleştirerek**ipRangeFilter** özelliği. 

Azure portalında IP erişim denetim ilkesini ayarlamak için Azure Cosmos DB hesap sayfasına gidin, **güvenlik duvarı ve sanal ağlar** Gezinti menüsünde, ardından değiştirme **erişime izin verecek** değeri için **seçili ağlar**ve ardından **Kaydet**. 

![Azure portalında güvenlik duvarı sayfasını açmak gösteren ekran görüntüsü](./media/firewall-support/azure-portal-firewall.png)

Portal, IP erişim denetim üzerinde olduğunda, IP adresleri ve aralıkları yanı sıra diğer Azure Hizmetleri ve Azure portalına erişimi etkinleştirmek için anahtarlar belirtme olanağı sağlar. Aşağıdaki bölümlerde bu anahtarları hakkında ek bilgi sağlanır.

> [!NOTE]
> Azure Cosmos DB veritabanı hesabınız için bir IP erişim denetim ilkesi etkinleştirerek, IP adresi aralıkları listesi engellenir yapılandırılmış dışındaki makinelerden Azure Cosmos DB veritabanı hesabınız için tüm erişim izin. Bu model da Portalı'ndan veri düzlemi işlemi gözatma de erişim denetimi bütünlüğünü sağlamak için engellenir.

## <a name="connections-from-the-azure-portal"></a>Azure portalından bağlantıları 

Program aracılığıyla bir IP erişim denetimi İlkesi etkinleştirdiğinizde, Azure portalında IP adresi eklemeniz gereken **ipRangeFilter** erişimi sürdürmek için özellik. Portal IP adreslerini şunlardır:

|Bölge|IP adresi|
|------|----------|
|Tüm bölgeler aşağıda belirtilen hariç|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Almanya|51.4.229.218|
|Çin|139.217.8.252|
|US Gov|52.244.48.71|

Azure portalına erişim için güvenlik duvarı ayarını değiştirdiğinizde, varsayılan olarak etkin **seçili ağlar** Azure portalında. 

![Azure portal erişimi etkinleştirme gösteren ekran görüntüsü](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-global-azure-datacenters-or-azure-paas-services"></a>Küresel Azure veri merkezleri ya da Azure PaaS hizmetlerine bağlantılar

Azure Stream analytics gibi Azure PaaS Hizmetleri, Azure işlevleri vb. Azure Cosmos DB ile birlikte kullanılır. Azure Cosmos DB kaynaklarınıza bağlanmak diğer Azure PaaS Hizmetleri uygulamalardan izin vermek için bir güvenlik duvarı ayarı etkinleştirilmelidir. Bu güvenlik duvarı ayarını etkinleştirmek için izin verilen IP adreslerinin listesi için IP adresi 0.0.0.0 ekleyin. 0.0.0.0 bağlantılar, Azure Cosmos DB hesabı için Azure veri merkezi IP aralığından kısıtlar. Bu ayar, herhangi bir IP aralıkları için Azure Cosmos DB hesabı için erişim izin vermez.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

Küresel Azure veri merkezleri içinde bağlantılara erişim için güvenlik duvarı ayarını değiştirdiğinizde, varsayılan olarak etkindir **seçili ağlar** Azure portalında. 

![Azure portalında güvenlik duvarı sayfasını açmak gösteren ekran görüntüsü](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Geçerli IP gelen bağlantıları

Makinenizde çalışan uygulamaları Azure Cosmos DB hesabına erişebilmesi için geliştirmeyi basitleştirmek için Azure portalında tanımlamanıza ve istemci makinenizin IP izin verilenler listesine ekleme yardımcı olur. Portal tarafından görülen istemci IP adresi algılandı. İstemci IP adresi, makinenizin olabilir, ancak ağ geçidinizin IP adresi olabilir. Üretime geçmeden önce kaldırmayı unutmayın.

Geçerli IP etkinleştirmek için seçin **geçerli IP'mi Ekle**, geçerli IP IP'ler listesine ekler ve ardından **Kaydet**.

![Bir gösteren ekran görüntüsü geçerli IP Güvenlik Duvarı ayarlarını yapılandırmak için](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Bulut hizmetlerinden gelen bağlantıları
Azure'da, bulut Hizmetleri Azure Cosmos DB kullanarak orta katman hizmet mantığı barındırmak için genel bir yoludur. Bir bulut hizmetindeki bir Azure Cosmos DB veritabanı hesabına erişimi etkinleştirmek için bulut hizmeti genel IP adresini, Azure Cosmos DB veritabanı hesabıyla ilişkili IP adreslerinin izin verilenler eklenmelidir [IP erişimi yapılandırma Denetim İlkesi](#configure-ip-policy). Bu, bulut hizmetlerinin tüm rol örneklerine Azure Cosmos DB veritabanı hesabınızı erişimi olmasını sağlar. Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında bulut hizmetleriniz için IP adresi alabilirsiniz:

![Azure Portalı'nda görüntülenen bir bulut hizmeti için genel IP adresini gösteren ekran görüntüsü](./media/firewall-support/public-ip-addresses.png)

Ek rol örnekleri ekleyerek bulut hizmetinizi ölçeklendirin, aynı bulut hizmetine bir parçası olduğundan bu yeni örnekleri otomatik olarak Azure Cosmos DB veritabanı hesabına erişebilir.

## <a name="connections-from-virtual-machines"></a>Sanal makinelerden bağlantıları
[Sanal makineler](https://azure.microsoft.com/services/virtual-machines/) veya [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Azure Cosmos DB kullanarak bir orta katman hizmetlerini barındırmak için de kullanılabilir.  Azure Cosmos DB veritabanı hesabı, sanal makineler, sanal makine ve/veya sanal makinenin genel IP adreslerini erişime izin verecek şekilde yapılandırmak için ölçek kümesi, Azure Cosmos DB veritabanı hesabınız için izin verilen IP adreslerini biri olarak tarafındanyapılandırılmasıgerekir[IP erişim denetimi ilkesini yapılandırma](#configure-ip-policy). Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında, sanal makineler için IP adresi alabilirsiniz.

![Azure Portalı'nda görüntülenen bir sanal makine için genel bir IP adresi gösteren ekran görüntüsü](./media/firewall-support/public-ip-addresses-dns.png)

Gruba birini eklediğinizde, bu ek sanal makine örnekleri, erişim için Azure Cosmos DB veritabanı hesabınızı otomatik olarak sağlanır.

## <a name="connections-from-the-internet"></a>Internet'ten gelen bağlantıları
Bir Azure Cosmos DB veritabanı hesabı internetteki bir bilgisayar eriştiğinizde, istemci IP adresini veya IP adresi aralığı makinenin IP adresini Azure Cosmos DB veritabanı hesabı için izin verilen listesine eklenmelidir. 

## <a name="using-azure-resource-manager-template-to-set-up-the-ip-access-control"></a>IP erişim denetimini ayarlamak için Azure Resource Manager şablonu kullanma

Şablonunuzu IP erişim denetimini ayarlamak için aşağıdaki JSON ekleyin. Resource Manager şablonu bir hesap için beyaz listeye alınması gereken IP aralıklarının listesi ipRangeFilter özniteliği olacaktır.

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
     "ipRangeFilter":"10.0.0.1,10.0.0.2,183.240.196.255"
   }
   }
```

## <a name="troubleshooting-the-ip-access-control-policy"></a>IP erişim denetimi İlkesi sorun giderme
### <a name="portal-operations"></a>Portal işlemleri
Azure Cosmos DB veritabanı hesabınız için bir IP erişim denetim ilkesi etkinleştirerek, IP adresi aralıkları listesi engellenir yapılandırılmış dışındaki makinelerden Azure Cosmos DB veritabanı hesabınız için tüm erişim izin. Kapsayıcılar ve sorgu belgelerini gözatma gibi portal veri düzlemi işlemleri etkinleştirmek istiyorsanız, bu nedenle açıkça Azure portalı kullanarak izin vermeniz gerekir **Güvenlik Duvarı** portalında sayfası. 

### <a name="sdk--rest-api"></a>SDK'sını ve Rest API
Güvenlik nedeniyle, SDK veya REST API aracılığıyla erişime izin verilen listede olmayan makinelerden genel 404 bulunamadı ile bir yanıt ek ayrıntı yok döndüreceği için. Doğru ilke yapılandırmasının Azure Cosmos DB veritabanı hesabınızı uygulanmasını sağlamak için izin verilen listesi Azure Cosmos DB veritabanı hesabınız için yapılandırılan IP doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Ağla ilgili performans ipuçları hakkında daha fazla bilgi için bkz. [performans ipuçları](performance-tips.md).

