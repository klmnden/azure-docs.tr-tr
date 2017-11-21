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
ms.date: 10/12/2017
ms.author: ankshah
ms.openlocfilehash: 1ceaa834ff68d5dca4abce561f9185e89af582af
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="azure-cosmos-db-firewall-support"></a>Azure Cosmos DB güvenlik duvarı desteği
Bir Azure Cosmos DB veritabanı hesapta depolanan verilerin güvenliğini sağlamak için Azure Cosmos DB desteği dayalı bir gizlilik için sağladığınız [yetkilendirme modelini](https://msdn.microsoft.com/library/azure/dn783368.aspx) güçlü karma tabanlı ileti kimlik doğrulama kodu (HMAC) kullanır. Şimdi, gizli tabanlı yetkilendirme modeli yanı sıra Azure Cosmos DB gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini güdümlü İlkesi destekler. Bu model, geleneksel veritabanı sistem güvenlik duvarı kurallarına benzer ve ek bir Azure Cosmos DB veritabanı hesabı için güvenlik düzeyi sağlar. Bu modelde, artık yalnızca onaylanan bir makineler kümesinden erişilebilir olması ve/veya Bulut Hizmetleri için bir Azure Cosmos DB veritabanı hesabı yapılandırabilirsiniz. Bu onaylanmış kümelerinden makineleri ve Hizmetleri Azure Cosmos DB kaynaklarına erişimi hala geçerli bir yetkilendirme belirteci sunmak arayan gerektirir.

## <a name="ip-access-control-overview"></a>IP erişim denetimine genel bakış
Varsayılan olarak, bir Azure Cosmos DB veritabanı hesabı tarafından geçerli bir yetkilendirme belirteci isteği eşlik sürece, genel internet'ten erişilebilir. IP ilke tabanlı erişim denetimini yapılandırmak için kullanıcı IP adreslerini veya IP adresi aralıklarını istemci IP'leri verilen veritabanı hesabı için izin verilen listesi olarak dahil edilecek CIDR formunda kümesi sağlamanız gerekir. Bu yapılandırma uygulandıktan sonra bu izin verilenler dışında makinelerden kaynaklanan tüm istekler sunucu tarafından engellenir.  IP tabanlı erişim denetimi için akışı işleme bağlantısı aşağıdaki şemada tanımlanır:

![IP tabanlı erişim denetimi için bağlantı işlemini gösteren diyagram](./media/firewall-support/firewall-support-flow.png)

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

## <a name="connections-from-azure-paas-service"></a>Azure PaaS hizmetinden bağlantıları 
Azure, Azure akış analizi gibi PaaS hizmetlerini Azure işlevleri Azure Cosmos DB ile birlikte kullanılır. Bu tür, IP adresi kullanıma hazır değil servis Azure Cosmos DB veritabanı hesabına erişimi etkinleştirmek için IP adresi 0.0.0.0 tarafından AzureCosmosDBveritabanıhesabınızlailişkiliIPadreslerininizinverilenlistesineeklenmelidir[IP erişim denetimi ilkesini yapılandırma](#configure-ip-policy).  Bu, Azure PaaS Hizmetleri bu kural olan Azure Cosmos DB hesap erişebilmesini sağlar. 

 ## <a id="configure-ip-policy"></a>IP erişim denetimi ilkesini yapılandırma
IP erişim denetimi ilkesini Azure portalında veya program aracılığıyla aracılığıyla ayarlanabilir [Azure CLI](cli-samples.md), [Azure Powershell](powershell-samples.md), veya [REST API](/rest/api/documentdb/) güncelleştirerek `ipRangeFilter` özelliği. IP adresleri/aralıklarına virgülle ayrılmış ve boşluk içermemelidir olması gerekir. Örnek: "13.91.6.132,13.91.6.1/24". Veritabanı hesabınızı bu yöntemlerle güncelleştirirken varsayılan ayarlarına geri döndürülmesi önlemek için tüm özellikler doldurmak emin olun.

> [!NOTE]
> Azure Cosmos DB veritabanı hesabınız için bir IP erişim denetimi İlkesi etkinleştirerek, tüm Azure Cosmos DB veritabanı hesabınızı yapılandırılmış dışında makinelerden IP adresi aralıkları listesi engellendi erişebileceğini. Bu model, Portalı'ndan veri düzlemi işlemi tarama de erişim denetimi bütünlüğünü sağlamak için engellenir.

Makinenizde çalışan uygulamalar Azure Cosmos DB hesap erişebilmesi için geliştirme basitleştirmek için Azure portalında tanımlamak ve istemci makinenizde IP izin verilenler listesine ekleyin yardımcı olur. İstemci IP adresi portal tarafından görülen algılandı. Makinenizin istemci IP adresi olabilir, ancak ağ geçidinizin IP adresi olabilir. Üretime geçmeden önce kaldırmayı unutmayın.

Azure portalında IP erişim denetimi ilkesini ayarlamak için Azure Cosmos DB hesabı dikey penceresine gidin, **Güvenlik Duvarı** Gezinti menüsünde ardından **ON** 

![Azure portalında güvenlik duvarı dikey penceresini açmak nasıl gösteren ekran görüntüsü](./media/firewall-support/azure-portal-firewall.png)

Azure portalı hesap erişebilir ve diğer adresleri ve olarak uygun ve ardından aralıkları eklemek isteyip yeni bölmesinde belirtin **kaydetmek**.  

> [!NOTE]
> Bir IP erişim denetimi İlkesi etkinleştirdiğinizde, erişim Azure portalının IP adresi eklemeniz gerekir. Portal IP adreslerini şunlardır:
> |Bölge|IP adresi|
> |------|----------|
> |Tüm bölgeler aşağıda belirtilen hariç|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
> |Almanya|51.4.229.218|
> |Çin|139.217.8.252|
> |ABD Devleti|52.244.48.71|
>

![Bir nasıl gösteren ekran görüntüsü Azure portalında güvenlik duvarı ayarlarını yapılandırmak için](./media/firewall-support/azure-portal-firewall-configure.png)

## <a name="troubleshooting-the-ip-access-control-policy"></a>IP erişim denetimi İlkesi sorun giderme
### <a name="portal-operations"></a>Portal işlemleri
Azure Cosmos DB veritabanı hesabınız için bir IP erişim denetimi İlkesi etkinleştirerek, tüm Azure Cosmos DB veritabanı hesabınızı yapılandırılmış dışında makinelerden IP adresi aralıkları listesi engellendi erişebileceğini. Koleksiyonlar ve sorgu belgeleri gözatma gibi portal veri düzlemi işlemlerini etkinleştirmek istiyorsanız, bu nedenle, açıkça kullanarak Azure portal erişimini izin vermeniz gereken **Güvenlik Duvarı** portaldaki dikey pencere. 

![Bir nasıl gösteren ekran görüntüsü Azure portalına erişim etkinleştirmek için](./media/firewall-support/azure-portal-access-firewall.png)

### <a name="sdk--rest-api"></a>SDK & Rest API'si
Güvenlik nedeniyle, SDK veya REST API aracılığıyla erişime izin verilenler olmayan makinelere karşı genel 404 bulunamadı yanıtta başka ek ayrıntı döndürülecek için. Doğru ilke yapılandırması Azure Cosmos DB veritabanı hesabınıza uygulanan emin olmak için izin verilen listesine Azure Cosmos DB veritabanı hesabınız için yapılandırılan IP doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Ağ hakkında bilgi için ilgili performans ipuçları, bkz: [performans ipuçları](performance-tips.md).

