---
title: Oluşturma ve MariaDB sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar için Azure veritabanı'nı yönetme | Microsoft Docs
description: Oluşturma ve MariaDB sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar için Azure veritabanı'nı yönetme
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: a1d92f324c30c498b1a42f6155a478cf131ecc96
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961967"
---
# <a name="create-and-manage-azure-database-for-mariadb-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak MariaDB sanal ağ hizmet uç noktaları ve sanal ağ kuralları için Azure veritabanı yönetme

Sanal ağ (VNet) Hizmetleri uç noktaları ve kuralları MariaDB için Azure veritabanı sunucunuza sanal ağ özel adres alanını genişletin. Sınırlamalar da dahil olmak üzere MariaDB sanal ağ hizmet uç noktaları için Azure veritabanı'nın genel bir bakış için bkz. [MariaDB sunucusu sanal ağ hizmet uç noktaları için Azure veritabanı](concepts-data-access-security-vnet.md). Sanal ağ hizmet uç noktaları, MariaDB için Azure veritabanı için desteklenen tüm bölgelerde kullanılabilir.

> [!NOTE]
> Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

## <a name="create-a-vnet-rule-and-enable-service-endpoints"></a>Bir sanal ağ kuralı oluşturun ve hizmet uç noktalarını etkinleştirin

1. MariaDB sunucusu sayfasında, ayarları altındaki başlığı tıklayın **bağlantı güvenliği** için MariaDB için Azure veritabanı bağlantı güvenliği bölmesinde açmak. Ardından, tıklayarak **+ var olan sanal ağı ekleme**. Mevcut bir Vnet'i yoksa tıklayabilirsiniz **+ yeni sanal ağ oluştur** oluşturmak için. Bkz: [hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portalı - bağlantı güvenliği](./media/howto-manage-vnet-portal/1-connection-security.png)

2. Bir sanal ağ kural adını girin, abonelik, sanal ağ ve alt ağ adı'nı seçin ve ardından **etkinleştirme**. Bu sanal ağ hizmet uç noktaları kullanarak alt ağ üzerinde otomatik olarak etkinleştirdiğini **Microsoft.SQL** hizmet etiketi.

   ![Azure portalı - sanal ağ yapılandırma](./media/howto-manage-vnet-portal/2-configure-vnet.png)

   Hesabın, bir sanal ağ ve hizmet uç noktası oluşturmak için gerekli izinleri olmalıdır.

   Hizmet uç noktaları sanal ağlarda birbirinden bağımsız olarak, sanal ağda yazma erişimine sahip bir kullanıcı tarafından yapılandırılabilir.
    
   Azure hizmet kaynaklarını bir sanal ağ güvenliğini sağlamak için kullanıcının eklenen alt ağlarda "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" için izni olmalıdır. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.
    
   [Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
    
   Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynaklarının farklı Aboneliklerde olması halinde, kaynakların aynı Active Directory (AD) kiracısı altında olması.

   > [!IMPORTANT]
   > Hizmet uç noktaları yapılandırmadan önce bu makalede hizmet uç noktası yapılandırması ve konuları hakkında okunacak önemle tavsiye edilir. **Sanal ağ hizmet uç noktası:** A [sanal ağ hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri içeren bir veya daha fazla biçimsel Azure hizmet türü adları bir alt ağ. Sanal ağ hizmet uç noktalarını kullanan hizmet türü adı **Microsoft.Sql**, adlandırılmış SQL veritabanı, Azure hizmetini ifade eder. Bu hizmet etiketi Hizmetleri MariaDB, PostgreSQL ve MySQL için Azure veritabanı Azure SQL veritabanı için de geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi, PostgreSQL için Azure veritabanı olan Azure SQL veritabanı dahil olmak üzere tüm Azure veritabanı hizmetleri için hizmet uç noktası trafiğini sanal ağ hizmet uç noktası yapılandırır MariaDB ve alt ağ üzerinde MySQL Server için Azure veritabanı için Azure veritabanı.
   > 

3. Etkinleştirildikten sonra tıklayın **Tamam** ve sanal ağ hizmet uç noktaları bir sanal ağ kuralı birlikte etkinleştirildiğini görürsünüz.

   ![Sanal ağ hizmet uç noktaları etkinleştirilmiş ve oluşturulan sanal ağ kuralı](./media/howto-manage-vnet-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [SSL MariaDB için Azure veritabanı yapılandırılıyor](howto-configure-ssl.md)
- Benzer şekilde, yazabilirsiniz [etkinleştirme sanal ağ hizmet uç noktalarını ve bir sanal ağ kuralı için Azure CLI kullanarak MariaDB için Azure veritabanı oluşturma](howto-manage-vnet-cli.md).
