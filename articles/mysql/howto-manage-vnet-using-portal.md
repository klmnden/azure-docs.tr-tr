---
title: Oluşturma ve MySQL sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar için Azure veritabanı'nı yönetme | Microsoft Docs
description: Oluşturma ve MySQL sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar için Azure veritabanı'nı yönetme
services: mysql
author: mbolz
ms.author: mbolz
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/22/2018
ms.openlocfilehash: 73655cf55a93a758ecb29b3408cdd0d803060397
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956651"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak MySQL sanal ağ hizmet uç noktaları ve sanal ağ kuralları için Azure veritabanı yönetme
Sanal ağ (VNet) Hizmetleri uç noktaları ve kuralları MySQL için Azure veritabanı sunucunuza sanal ağ özel adres alanını genişletin. Sınırlamalar da dahil olmak üzere MySQL sanal ağ hizmet uç noktaları için Azure veritabanı'nın genel bir bakış için bkz. [MySQL Server sanal ağ hizmet uç noktaları için Azure veritabanı](concepts-data-access-and-security-vnet.md). Sanal ağ hizmet uç noktaları, MySQL için Azure veritabanı için desteklenen tüm bölgelerde kullanılabilir.

> [!NOTE]
> Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Bir sanal ağ kuralı oluşturun ve Azure portalında hizmet uç noktalarını etkinleştirin

1. MySQL sunucusu sayfasında, ayarları altındaki başlığı tıklayın **bağlantı güvenliği** için MySQL için Azure veritabanı bağlantı güvenliği bölmesinde açmak. Ardından, tıklayarak **+ var olan sanal ağı ekleme**. Mevcut bir Vnet'i yoksa tıklayabilirsiniz **+ yeni sanal ağ oluştur** oluşturmak için. Bkz: [hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portalı - bağlantı güvenliği](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Bir sanal ağ kural adını girin, abonelik, sanal ağ ve alt ağ adı'nı seçin ve ardından **etkinleştirme**. Bu sanal ağ hizmet uç noktaları kullanarak alt ağ üzerinde otomatik olarak etkinleştirdiğini **Microsoft.SQL** hizmet etiketi.

   ![Azure portalı - sanal ağ yapılandırma](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   Hesabın, bir sanal ağ ve hizmet uç noktası oluşturmak için gerekli izinleri olmalıdır.

   Hizmet uç noktaları sanal ağlarda birbirinden bağımsız olarak, sanal ağda yazma erişimine sahip bir kullanıcı tarafından yapılandırılabilir.
    
   Azure hizmet kaynaklarını bir sanal ağ güvenliğini sağlamak için kullanıcının eklenen alt ağlarda "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" için izni olmalıdır. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.
    
   [Yerleşik roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) ve [özel rollere](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
    
   Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynaklarının farklı Aboneliklerde olması halinde, kaynakların aynı Active Directory (AD) kiracısı altında olması.

   > [!IMPORTANT]
   > Hizmet uç noktaları yapılandırmadan önce bu makalede hizmet uç noktası yapılandırması ve konuları hakkında okunacak önemle tavsiye edilir. **Sanal ağ hizmet uç noktası:** A [sanal ağ hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri içeren bir veya daha fazla biçimsel Azure hizmet türü adları bir alt ağ. Sanal ağ hizmet uç noktalarını kullanan hizmet türü adı **Microsoft.Sql**, adlandırılmış SQL veritabanı, Azure hizmetini ifade eder. Bu hizmet etiketi, hizmetleri, PostgreSQL ve MySQL için Azure veritabanı Azure SQL veritabanı için de geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi isteğe bağlı olarak bir sanal ağ hizmet uç noktası için Azure SQL veritabanı ve PostgreSQL için Azure veritabanı dahil olmak üzere tüm Azure veritabanı hizmetleri için hizmet uç noktası trafiğini yapılandırır ve Alt ağdaki MySQL Server için Azure veritabanı. 
   > 

3. Etkinleştirildikten sonra tıklayın **Tamam** ve sanal ağ hizmet uç noktaları bir sanal ağ kuralı birlikte etkinleştirildiğini görürsünüz.

   ![Sanal ağ hizmet uç noktaları etkinleştirilmiş ve oluşturulan sanal ağ kuralı](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [etkinleştirme sanal ağ hizmet uç noktalarını ve bir sanal ağ kuralı için Azure CLI kullanarak MySQL için Azure veritabanı oluşturma](howto-manage-vnet-using-cli.md).
- MySQL sunucusu için Azure veritabanı bağlanma konusunda yardım için bkz [MySQL için Azure veritabanı için bağlantı kitaplıkları](./concepts-connection-libraries.md)
