---
title: Oluşturma ve PostgreSQL - Azure portalını kullanarak tek bir sunucu için sanal ağ hizmet uç noktaları ve Azure veritabanı'nda kurallarını yönetme
description: Oluşturma ve sanal ağ hizmet uç noktaları ve Azure veritabanı kuralları, PostgreSQL - Azure portalını kullanarak tek bir sunucu yönetme
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 9da46ae905457f6f6b1786a2161e224d397d0507
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65073168"
---
# <a name="create-and-manage-vnet-service-endpoints-and-vnet-rules-in-azure-database-for-postgresql---single-server-by-using-the-azure-portal"></a>Oluşturma ve PostgreSQL - Azure portalını kullanarak tek sunucu için sanal ağ hizmet uç noktaları ve Azure veritabanı'nda sanal ağ kuralları'nı yönetme
Sanal ağ (VNet) Hizmetleri uç noktaları ve kuralları PostgreSQL için Azure veritabanı sunucunuza sanal ağ özel adres alanını genişletin. Sınırlamalar da dahil olmak üzere PostgreSQL sanal ağ hizmet uç noktaları için Azure veritabanı'nın genel bir bakış için bkz. [PostgreSQL sunucusu sanal ağ hizmet uç noktaları için Azure veritabanı](concepts-data-access-and-security-vnet.md). Sanal ağ hizmet uç noktaları, PostgreSQL için Azure veritabanı için desteklenen tüm bölgelerde kullanılabilir.

> [!NOTE]
> Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.
> Trafiği ortak bir VNet ağ geçidi hizmet uç noktaları ile üzerinden akan ve eşler arası akışı beklenir, VNet eşlemesi olması durumunda, Azure sanal makineler PostgreSQL sunucusu için Azure veritabanına erişmek için ağ geçidi sanal ağda izin vermek için bir ACL/sanal ağ kuralı Lütfen oluşturun.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Bir sanal ağ kuralı oluşturun ve Azure portalında hizmet uç noktalarını etkinleştirin

1. PostgreSQL sunucusu sayfasında, ayarları altındaki başlığı tıklayın **bağlantı güvenliği** bağlantı güvenliği bölmesi için PostgreSQL için Azure veritabanı açmak için. 

2. Azure Hizmetleri denetlemek için erişim izni ver emin olun **OFF**.

> [!Important]
> AÇIK olarak ayarlanmış denetimi değiştirmeden bırakırsanız, Azure PostgreSQL veritabanı sunucunuza tüm alt ağından gelen iletişimi kabul eder. AÇIK olarak ayarlanmış denetimi bırakarak bir güvenlik açısından aşırı erişimi olabilir. Koordinasyon halinde sanal ağ kuralı özelliğiyle Azure veritabanı'nın, PostgreSQL için Microsoft Azure sanal ağ hizmet uç noktası özelliği birlikte, güvenlik yüzey alanını azaltabilirsiniz.

3. Ardından, tıklayarak **+ var olan sanal ağı ekleme**. Mevcut bir Vnet'i yoksa tıklayabilirsiniz **+ yeni sanal ağ oluştur** oluşturmak için. Bkz: [hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portalı - bağlantı güvenliği](./media/howto-manage-vnet-using-portal/1-connection-security.png)

4. Bir sanal ağ kural adını girin, abonelik, sanal ağ ve alt ağ adı'nı seçin ve ardından **etkinleştirme**. Bu sanal ağ hizmet uç noktaları kullanarak alt ağ üzerinde otomatik olarak etkinleştirdiğini **Microsoft.SQL** hizmet etiketi.

   ![Azure portalı - sanal ağ yapılandırma](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

    Hesabın, bir sanal ağ ve hizmet uç noktası oluşturmak için gerekli izinleri olmalıdır.

    Hizmet uç noktaları sanal ağlarda birbirinden bağımsız olarak, sanal ağda yazma erişimine sahip bir kullanıcı tarafından yapılandırılabilir.
    
    Azure hizmet kaynaklarını bir sanal ağ güvenliğini sağlamak için kullanıcının eklenen alt ağlarda "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" için izni olmalıdır. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.
    
    [Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
    
    Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynaklarının farklı Aboneliklerde olması halinde, kaynakların aynı Active Directory (AD) kiracısı altında olması.

   > [!IMPORTANT]
   > Hizmet uç noktaları yapılandırmadan önce bu makalede hizmet uç noktası yapılandırması ve konuları hakkında okunacak önemle tavsiye edilir. **Sanal ağ hizmet uç noktası:** A [sanal ağ hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri içeren bir veya daha fazla biçimsel Azure hizmet türü adları bir alt ağ. Sanal ağ hizmet uç noktalarını kullanan hizmet türü adı **Microsoft.Sql**, adlandırılmış SQL veritabanı, Azure hizmetini ifade eder. Bu hizmet etiketi, hizmetleri, PostgreSQL ve MySQL için Azure veritabanı Azure SQL veritabanı için de geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi isteğe bağlı olarak bir sanal ağ hizmet uç noktası için Azure SQL veritabanı ve PostgreSQL için Azure veritabanı dahil olmak üzere tüm Azure veritabanı hizmetleri için hizmet uç noktası trafiğini yapılandırır ve Alt ağdaki MySQL Server için Azure veritabanı. 
   > 

5. Etkinleştirildikten sonra tıklayın **Tamam** ve sanal ağ hizmet uç noktaları bir sanal ağ kuralı birlikte etkinleştirildiğini görürsünüz.

   ![Sanal ağ hizmet uç noktaları etkinleştirilmiş ve oluşturulan sanal ağ kuralı](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [etkinleştirme sanal ağ hizmet uç noktalarını ve bir sanal ağ kuralı için Azure CLI kullanarak PostgreSQL için Azure veritabanı oluşturma](howto-manage-vnet-using-cli.md).
- PostgreSQL sunucusu için Azure veritabanı bağlanma konusunda yardım için bkz. [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](./concepts-connection-libraries.md)
