---
title: Oluşturma ve Azure veritabanı PostgreSQL VNet hizmet uç noktaları ve Azure portalını kullanarak kurallar için yönetme | Microsoft Docs
description: Oluşturma ve Azure veritabanı PostgreSQL VNet hizmet uç noktaları ve Azure portalını kullanarak kurallar için yönetme
services: postgresql
author: mbolz
ms.author: mbolz
manager: kfile
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 8199bd98b9ae091ff2b27efa8334e8e763879359
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757374"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak Azure veritabanı PostgreSQL VNet hizmet uç noktaları ve sanal ağ kuralları için yönetme
Sanal ağ (VNet) Hizmetleri uç noktaları ve kurallar için Azure veritabanınızda PostgreSQL sunucu özel adres alanı sanal ağ genişletir. Azure veritabanı sınırlamalar da dahil olmak üzere PostgreSQL VNet hizmet uç noktaları için bir genel bakış için bkz: [Azure veritabanı PostgreSQL sunucu VNet hizmet uç noktaları için](concepts-data-access-and-security-vnet.md). VNet Hizmeti uç noktalarını genel Önizleme'de tüm desteklenen bölgeler Azure veritabanı için PostgreSQL için kullanılabilir.

> [!NOTE]
> Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için VNet hizmet uç noktaları desteğidir.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Bir sanal ağ kuralı oluşturun ve Azure portalında hizmet uç noktaları etkinleştirin

1. PostgreSQL sunucusu sayfasında, ayarları altında başlığını tıklatın **bağlantı güvenliği** PostgreSQL için Azure veritabanı için bağlantı güvenliği bölmesini açmak için. Ardından, tıklayın **+ varolan sanal ağ ekleyerek**. Var olan bir VNet yoksa tıklayabilirsiniz **+ yeni sanal ağ oluştur** oluşturmak için. Bkz: [hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portal - bağlantı güvenliği](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Bir sanal ağ kuralı adı girin, abonelik, sanal ağ ve alt ağ adı seçin ve ardından **etkinleştirmek**. Bu alt ağ kullanarak VNet Hizmeti uç noktalarını otomatik olarak etkinleştirir **Microsoft.SQL** hizmet etiketi.

   ![Azure portal - VNet yapılandırın](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   > [!IMPORTANT]
   > Hizmet uç noktaları yapılandırmadan önce bu makalede hizmet uç noktası yapılandırması ve konuları hakkında okumak için önerilir. **Sanal Ağ Hizmeti uç noktası:** A [sanal ağ hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri içeren bir veya daha fazla resmi Azure hizmeti tür adları bir alt ağ. Sanal Ağ Hizmetleri uç noktaları kullanma hizmet türü adı **Microsoft.Sql**, adlandırılmış SQL Database, Azure hizmetine başvurduğu. Bu hizmet etiketi de Azure SQL Database, Azure veritabanı PostgreSQL ve MySQL Hizmetleri için geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi VNet Hizmeti uç noktası için hizmet uç noktası trafiği Azure SQL Database, Azure veritabanı PostgreSQL için de dahil olmak üzere tüm Azure veritabanı hizmetleri için yapılandırır ve Azure veritabanı alt ağdaki MySQL sunucuları için. 
   > 

3. Bir kez etkinleştirildikten sonra tıklatın **Tamam** ve VNet hizmet uç noktaları ile birlikte bir sanal ağ kuralı etkin olduğunu görürsünüz.

   ![VNet Hizmeti uç noktaları etkin ve oluşturulan sanal ağ kuralı](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [etkinleştirmek VNet hizmet uç noktaları ve Azure CLI kullanarak PostgreSQL için Azure veritabanı için bir sanal ağ kuralı oluşturma](howto-manage-vnet-using-cli.md).
- PostgreSQL sunucu için bir Azure veritabanına bağlanma konusunda yardım için bkz: [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](./concepts-connection-libraries.md)
