---
title: Azure Cosmos DB güvenlik duvarı & IP erişim denetimi | Microsoft Docs
description: Azure Cosmos DB veritabanı hesaplarında güvenlik duvarı desteği için IP erişim denetimi ilkeleri kullanmayı öğrenin.
keywords: IP erişim denetimi, güvenlik duvarı desteği
services: cosmos-db
author: SnehaGunda
manager: kfile
tags: azure-resource-manager
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: sngun
ms.openlocfilehash: 0407d3c58fa63a11c8391f069039f7c35a15ceb7
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294746"
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure Cosmos DB güvenlik duvarı desteği
Bir Azure Cosmos DB veritabanı hesapta depolanan verilerin güvenliğini sağlamak için Azure Cosmos DB desteği dayalı bir gizlilik için sağladığınız [yetkilendirme modelini](https://msdn.microsoft.com/library/azure/dn783368.aspx) güçlü karma tabanlı ileti kimlik doğrulama kodu (HMAC) kullanır. Şimdi, gizli tabanlı yetkilendirme modeli yanı sıra Azure Cosmos DB gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini güdümlü İlkesi destekler. Bu model, geleneksel veritabanı sistem güvenlik duvarı kurallarına benzer ve ek bir Azure Cosmos DB veritabanı hesabı için güvenlik düzeyi sağlar. Bu modelde, artık yalnızca onaylanan bir makineler kümesinden erişilebilir olması ve/veya Bulut Hizmetleri için bir Azure Cosmos DB veritabanı hesabı yapılandırabilirsiniz. Bu onaylanmış kümelerinden makineleri ve Hizmetleri Azure Cosmos DB kaynaklarına erişimi hala geçerli bir yetkilendirme belirteci sunmak arayan gerektirir.

> [!NOTE]
> Şu anda güvenlik duvarı Azure Cosmos DB SQL API ve Mongo API hesapları için kullanılabilir. Diğer API'leri ve Azure Almanya veya Azure kamu gibi sovereign Bulutlar için güvenlik duvarlarını yapılandırma olanağı yakında kullanıma sunulacaktır. Yapılandırılmış var olan bir IP güvenlik duvarına sahip Azure Cosmos DB hesabınız için hizmet uç noktası ACL yapılandırmak planlıyorsanız, lütfen güvenlik duvarı yapılandırması unutmayın, IP Güvenlik Duvarı'nı kaldırın ve Hizmeti uç noktası ACL yapılandırın. Hizmet uç noktası yapılandırdıktan sonra IP Güvenlik Duvarı'nı gerekirse yeniden etkinleştirebilirsiniz.

## <a name="ip-access-control-overview"></a>IP erişim denetimine genel bakış
İstek, geçerli bir yetkilendirme belirteciyle birlikte sunulduğu sürece, varsayılan olarak bir Azure Cosmos DB veritabanı hesabına genel İnternet’ten erişilebilir. IP ilke tabanlı erişim denetimini yapılandırmak için kullanıcı, CIDR formunda bir grup IP adresi veya IP adresi aralığı sağlamalıdır. Bunlar, belirli bir veritabanı hesabı için izin verilen IP listesi olarak dahil edilecektir. Bu yapılandırma uygulandıktan sonra, bu izin verilen liste dışındaki makinelerden gelen tüm istekler, sunucu tarafından engellenir.  IP tabanlı erişim denetimi için akışı işleme bağlantısı aşağıdaki şemada tanımlanır:

![IP tabanlı erişim denetimi için bağlantı işlemini gösteren diyagram](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a> IP erişim denetimi ilkesini yapılandırma
IP erişim denetimi ilkesini Azure portalında veya program aracılığıyla aracılığıyla ayarlanabilir [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), veya [REST API](/rest/api/cosmos-db/) güncelleştirerek**ipRangeFilter** özelliği. 

Azure portalında IP erişim denetimi ilkesini ayarlamak için Azure Cosmos DB hesap sayfasına gidin, **güvenlik duvarı ve sanal ağlar** Gezinti menüsünde sonra değiştirmek **izni** değeri için **seçili ağları**ve ardından **kaydetmek**. 

![Azure portalında güvenlik duvarı sayfasını açmak nasıl gösteren ekran görüntüsü](./media/firewall-support/azure-portal-firewall.png)

Portal, üzerinde IP erişim denetimi olduktan sonra IP adresleri ve aralıkları yanı sıra diğer Azure hizmetleriyle ve Azure portalına erişim sağlamak için anahtarları belirtme olanağı sağlar. Aşağıdaki bölümlerde bu anahtarları hakkında ek bilgi sağlanmıştır.

> [!NOTE]
> Azure Cosmos DB veritabanı hesabınız için bir IP erişim denetimi İlkesi etkinleştirerek, tüm Azure Cosmos DB veritabanı hesabınızı yapılandırılmış dışında makinelerden IP adresi aralıkları listesi engellendi erişebileceğini. Bu model, Portalı'ndan veri düzlemi işlemi tarama de erişim denetimi bütünlüğünü sağlamak için engellenir.

## <a name="connections-from-the-azure-portal"></a>Azure portalından bağlantıları 

Program aracılığıyla bir IP erişim denetimi İlkesi etkinleştirdiğinizde, Azure portalında IP adresi eklemek gereken **ipRangeFilter** erişim için özellik. Portal IP adreslerini şunlardır:

|Bölge|IP adresi|
|------|----------|
|Tüm bölgeler aşağıda belirtilen hariç|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
|Almanya|51.4.229.218|
|Çin|139.217.8.252|
|ABD Devleti|52.244.48.71|

Azure portalına erişim, varsayılan olarak etkindir, güvenlik duvarı ayarı değiştirdiğinizde **seçili ağları** Azure portalında. 

![Azure portal erişiminin nasıl etkinleştirileceği gösteren ekran görüntüsü](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-public-azure-datacenters-or-azure-paas-services"></a>Ortak Azure veri merkezleri ya da Azure PaaS hizmetlerinden bağlantıları
Azure'da, PaaS Hizmetleri Azure Stream analytics, Azure işlevleri ve Azure App Service gibi Azure Cosmos DB ile birlikte kullanılır. Veritabanı hesabı, IP adres kullanıma hazır olmayan hizmetlerin Azure Cosmos DB erişimi etkinleştirmek için program aracılığıyla Azure Cosmos DB veritabanı hesabınızla ilişkili IP adreslerinin izin verilenler listesine IP adresi 0.0.0.0 ekleyin. 

Ortak Azure veri merkezleri içinde bağlantılarından erişimi, varsayılan olarak etkindir, güvenlik duvarı ayarı değiştirdiğinizde **seçili ağları** Azure portalında. 

![Azure portalında güvenlik duvarı sayfasını açmak nasıl gösteren ekran görüntüsü](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Geçerli IP gelen bağlantıları

Makinenizde çalışan uygulamalar Azure Cosmos DB hesap erişebilmesi için geliştirme basitleştirmek için Azure portalında tanımlamak ve istemci makinenizde IP izin verilenler listesine ekleyin yardımcı olur. İstemci IP adresi portal tarafından görülen algılandı. Makinenizin istemci IP adresi olabilir, ancak ağ geçidinizin IP adresi olabilir. Üretime geçmeden önce kaldırmayı unutmayın.

Geçerli IP etkinleştirmek için seçin **my geçerli IP eklemek**, hangi geçerli IP IP'leri listesine ekler ve ardından **kaydetmek**.

![Bir nasıl gösteren ekran görüntüsü geçerli IP Güvenlik Duvarı ayarlarını yapılandırmak için](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Bulut hizmetleri gelen bağlantıları
Azure'da, bulut Hizmetleri Azure Cosmos DB kullanan Orta katman hizmet mantığı barındırmak için ortak bir yol sağlar. Bir bulut hizmetinden bir Azure Cosmos DB veritabanı hesabına erişimi etkinleştirmek için bulut hizmetini genel IP adresi izin verilenler tarafından Azure Cosmos DB veritabanı hesabınızla ilişkili IP adreslerinin eklenmeli [IP erişim denetimi ilkesini yapılandırma](#configure-ip-policy). Bu, bulut Hizmetleri tüm rol örneklerini Azure Cosmos DB veritabanı hesabınıza erişimi olmasını sağlar. Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında bulut hizmetleriniz için IP adreslerini alabilirsiniz:

![Azure portalında görüntülenen bir bulut hizmeti için genel IP adresi gösteren ekran görüntüsü](./media/firewall-support/public-ip-addresses.png)

Ek rol örnekleri ekleyerek, bulut hizmetinin ölçeklendirdiğinizde aynı bulut hizmetinin bir parçası olduğundan bu yeni örnekleri otomatik olarak Azure Cosmos DB veritabanı hesabı erişebilir.

## <a name="connections-from-virtual-machines"></a>Sanal makinelerden bağlantıları
[Sanal makineler](https://azure.microsoft.com/services/virtual-machines/) veya [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Azure Cosmos DB kullanarak orta katman hizmetlerini barındırmak için de kullanılabilir.  Sanal makineler erişime izin verecek şekilde Azure Cosmos DB veritabanı hesabı yapılandırmak için sanal makine ve/veya sanal makine ölçek kümesi ortak IP adreslerini Azure Cosmos DB veritabanı hesabınızı tarafından izin verilen IP adreslerini biri olarak yapılandırılmalıdır [IP erişim denetimi ilkesini yapılandırma](#configure-ip-policy). Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında sanal makineler için IP adresleri alabilirsiniz.

![Azure portalında görüntülenen bir sanal makine için genel bir IP adresi gösteren ekran görüntüsü](./media/firewall-support/public-ip-addresses-dns.png)

Ek sanal makine örnekleri gruba eklediğinizde, bunlar otomatik olarak Azure Cosmos DB veritabanı hesabınıza erişimi sağlanır.

## <a name="connections-from-the-internet"></a>Internet'ten gelen bağlantıları
İnternet üzerindeki bir bilgisayardan bir Azure Cosmos DB veritabanı hesabı eriştiğinde, istemci IP adresi veya IP adresi aralığı makinenin IP adresi Azure Cosmos DB veritabanı hesabı için izin verilen listesine eklenmesi gerekir. 

## <a name="troubleshooting-the-ip-access-control-policy"></a>IP erişim denetimi İlkesi sorun giderme
### <a name="portal-operations"></a>Portal işlemleri
Azure Cosmos DB veritabanı hesabınız için bir IP erişim denetimi İlkesi etkinleştirerek, tüm Azure Cosmos DB veritabanı hesabınızı yapılandırılmış dışında makinelerden IP adresi aralıkları listesi engellendi erişebileceğini. Koleksiyonlar ve sorgu belgeleri gözatma gibi portal veri düzlemi işlemlerini etkinleştirmek istiyorsanız, bu nedenle, açıkça kullanarak Azure portal erişimini izin vermeniz gereken **Güvenlik Duvarı** portalında sayfası. 

### <a name="sdk--rest-api"></a>SDK & Rest API'si
Güvenlik nedeniyle, SDK veya REST API aracılığıyla erişime izin verilenler olmayan makinelere karşı genel 404 bulunamadı yanıtta başka ek ayrıntı döndürülecek için. Doğru ilke yapılandırması Azure Cosmos DB veritabanı hesabınıza uygulanan emin olmak için izin verilen listesine Azure Cosmos DB veritabanı hesabınız için yapılandırılan IP doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Ağla ilgili performans ipuçları hakkında daha fazla bilgi için bkz: [performans ipuçları](performance-tips.md).

