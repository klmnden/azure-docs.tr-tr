---
title: "Azure Cosmos DB güvenlik duvarı & IP erişim denetimi | Microsoft Docs"
description: "Azure Cosmos DB veritabanı hesaplarında güvenlik duvarı desteği için IP erişim denetimi ilkeleri kullanmayı öğrenin."
keywords: "IP erişim denetimi, güvenlik duvarı desteği"
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: ankshah
ms.openlocfilehash: 85f5e0e076f92afc79ed8f4ce652bb0f31923113
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure Cosmos DB güvenlik duvarı desteği
Bir Azure Cosmos DB veritabanı hesapta depolanan verilerin güvenliğini sağlamak için Azure Cosmos DB desteği dayalı bir gizlilik için sağladığınız [yetkilendirme modelini](https://msdn.microsoft.com/library/azure/dn783368.aspx) güçlü karma tabanlı ileti kimlik doğrulama kodu (HMAC) kullanır. Şimdi, gizli tabanlı yetkilendirme modeli yanı sıra Azure Cosmos DB gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini güdümlü İlkesi destekler. Bu model, geleneksel veritabanı sistem güvenlik duvarı kurallarına benzer ve ek bir Azure Cosmos DB veritabanı hesabı için güvenlik düzeyi sağlar. Bu modelde, artık yalnızca onaylanan bir makineler kümesinden erişilebilir olması ve/veya Bulut Hizmetleri için bir Azure Cosmos DB veritabanı hesabı yapılandırabilirsiniz. Bu onaylanmış kümelerinden makineleri ve Hizmetleri Azure Cosmos DB kaynaklarına erişimi hala geçerli bir yetkilendirme belirteci sunmak arayan gerektirir.

## <a name="ip-access-control-overview"></a>IP erişim denetimine genel bakış
Varsayılan olarak, bir Azure Cosmos DB veritabanı hesabı tarafından geçerli bir yetkilendirme belirteci isteği eşlik sürece, genel internet'ten erişilebilir. IP ilke tabanlı erişim denetimini yapılandırmak için kullanıcı IP adreslerini veya IP adresi aralıklarını istemci IP'leri verilen veritabanı hesabı için izin verilen listesi olarak dahil edilecek CIDR formunda kümesi sağlamanız gerekir. Bu yapılandırma uygulandıktan sonra bu izin verilenler dışında makinelerden kaynaklanan tüm istekler sunucu tarafından engellendi.  IP tabanlı erişim denetimi için akışı işleme bağlantısı aşağıdaki şemada tanımlanır:

![IP tabanlı erişim denetimi için bağlantı işlemini gösteren diyagram](./media/firewall-support/firewall-support-flow.png)

## <a id="configure-ip-policy"></a>IP erişim denetimi ilkesini yapılandırma
IP erişim denetimi ilkesini Azure portalında veya program aracılığıyla aracılığıyla ayarlanabilir [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), veya [REST API](/rest/api/documentdb/) güncelleştirerek**ipRangeFilter** özelliği. 

Azure portalında IP erişim denetimi ilkesini ayarlamak için Azure Cosmos DB hesap sayfasına gidin, **Güvenlik Duvarı** Gezinti menüsünde sonra değiştirmek **IP erişim denetimi etkinleştir** değerine**ON**. 

![Azure portalında güvenlik duvarı sayfasını açmak nasıl gösteren ekran görüntüsü](./media/firewall-support/azure-portal-firewall.png)

Üzerinde IP erişim denetimi olduktan sonra portal, Azure portalı, diğer Azure hizmetleriyle ve geçerli IP erişimi etkinleştirmek için anahtarlar sağlar. Aşağıdaki bölümlerde bu anahtarları hakkında ek bilgi sağlanmıştır.

![Bir nasıl gösteren ekran görüntüsü Azure portalında güvenlik duvarı ayarlarını yapılandırmak için](./media/firewall-support/azure-portal-firewall-configure.png)

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

Azure Portalı aracılığıyla Azure portalına erişim etkinleştirmek iççin **Azure portalına erişime izin ver** değeri **ON** Azure portalında ( **IP erişim denetimi etkinleştir** değeri ayarlanmalıdır **ON** görüntülemek ve değiştirmek için **Azure portalına erişim izin** değeri).

![Azure portal erişiminin nasıl etkinleştirileceği gösteren ekran görüntüsü](./media/firewall-support/enable-azure-portal.png)

## <a name="connections-from-other-azure-paas-services"></a>Diğer Azure PaaS hizmetlerinden bağlantıları 
Azure'da, PaaS Hizmetleri Azure Stream analytics, Azure işlevleri ve Azure App Service gibi Azure Cosmos DB ile birlikte kullanılır. Azure Cosmos DB erişimi etkinleştirmek için IP adres kullanıma hazır olmayan bu Hizmetleri veritabanı hesabından IP adresi 0.0.0.0 program aracılığıyla Azure Cosmos DB veritabanı hesabınızla ilişkili IP adreslerinin izin verilenler listesine ekleyin veya ayarlama **Azure hizmetlerine erişime izin ver** açık bir değer Azure Portalı'ndaki ( **IP erişim denetimi etkinleştir** değeri ayarlanmalıdır **ON** görüntülemek ve değiştirmek için **izin ver Azure hizmetlerine erişim** değeri). Bu, Azure PaaS Hizmetleri Azure Cosmos DB hesap erişebilmesini sağlar. 

![Azure portalında güvenlik duvarı sayfasını açmak nasıl gösteren ekran görüntüsü](./media/firewall-support/enable-azure-services.png)

## <a name="connections-from-your-current-ip"></a>Geçerli IP gelen bağlantıları

Makinenizde çalışan uygulamalar Azure Cosmos DB hesap erişebilmesi için geliştirme basitleştirmek için Azure portalında tanımlamak ve istemci makinenizde IP izin verilenler listesine ekleyin yardımcı olur. İstemci IP adresi portal tarafından görülen algılandı. Makinenizin istemci IP adresi olabilir, ancak ağ geçidinizin IP adresi olabilir. Üretime geçmeden önce kaldırmayı unutmayın.

![Bir nasıl gösteren ekran görüntüsü geçerli IP Güvenlik Duvarı ayarlarını yapılandırmak için](./media/firewall-support/enable-current-ip.png)

## <a name="connections-from-cloud-services"></a>Bulut hizmetleri gelen bağlantıları
Azure'da, bulut Hizmetleri Azure Cosmos DB kullanan Orta katman hizmet mantığı barındırmak için ortak bir yol sağlar. Bir bulut hizmetinden bir Azure Cosmos DB veritabanı hesabına erişimi etkinleştirmek için bulut hizmetini genel IP adresi izin verilenler tarafından Azure Cosmos DB veritabanı hesabınızla ilişkili IP adreslerinin eklenmeli [IP erişim denetimi ilkesini yapılandırma](#configure-ip-policy).  Bu, bulut Hizmetleri tüm rol örneklerini Azure Cosmos DB veritabanı hesabınıza erişimi olmasını sağlar. Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında bulut hizmetleriniz için IP adreslerini alabilirsiniz:

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

![Bir nasıl gösteren ekran görüntüsü Azure portalına erişim etkinleştirmek için](./media/firewall-support/enable-azure-portal.png)

### <a name="sdk--rest-api"></a>SDK & Rest API'si
Güvenlik nedeniyle, SDK veya REST API aracılığıyla erişime izin verilenler olmayan makinelere karşı genel 404 bulunamadı yanıtta başka ek ayrıntı döndürülecek için. Doğru ilke yapılandırması Azure Cosmos DB veritabanı hesabınıza uygulanan emin olmak için izin verilen listesine Azure Cosmos DB veritabanı hesabınız için yapılandırılan IP doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Ağla ilgili performans ipuçları hakkında daha fazla bilgi için bkz: [performans ipuçları](performance-tips.md).

