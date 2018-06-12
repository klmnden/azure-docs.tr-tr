---
title: Oluşturma ve MySQL VNet hizmet uç noktaları ve Azure portalını kullanarak kurallar için Azure veritabanı yönetme | Microsoft Docs
description: Oluşturma ve Azure veritabanı MySQL VNet hizmet uç noktaları ve Azure portalını kullanarak kurallar için yönetme
services: mysql
author: mbolz
ms.author: mbolz
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 7520868fd6bd349043ad2c53e62de5db978db8b1
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267229"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak Azure veritabanı MySQL VNet hizmet uç noktaları ve sanal ağ kuralları için yönetme
Sanal ağ (VNet) Hizmetleri uç noktaları ve kuralları Azure veritabanınıza MySQL sunucusu için bir sanal ağ özel adres alanında genişletir. Azure veritabanı sınırlamalar da dahil olmak üzere MySQL VNet hizmet uç noktaları için bir genel bakış için bkz: [Azure veritabanı MySQL Server VNet hizmet uç noktaları için](concepts-data-access-and-security-vnet.md). VNet Hizmeti uç noktalarını genel Önizleme'de tüm desteklenen bölgeler Azure veritabanı için MySQL için kullanılabilir.

> [!NOTE]
> Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için VNet hizmet uç noktaları desteğidir.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Bir sanal ağ kuralı oluşturun ve Azure portalında hizmet uç noktaları etkinleştirin

1. MySQL sunucusu sayfasında, ayarları altında başlığını tıklatın **bağlantı güvenliği** bağlantı güvenliği bölmesinde Azure veritabanı için MySQL için açın. Ardından, tıklayın **+ varolan sanal ağ ekleyerek**. Var olan bir VNet yoksa tıklayabilirsiniz **+ yeni sanal ağ oluştur** oluşturmak için. Bkz: [hızlı başlangıç: Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

   ![Azure portal - bağlantı güvenliği](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Bir sanal ağ kuralı adı girin, abonelik, sanal ağ ve alt ağ adı seçin ve ardından **etkinleştirmek**. Bu alt ağ kullanarak VNet Hizmeti uç noktalarını otomatik olarak etkinleştirir **Microsoft.SQL** hizmet etiketi.

   ![Azure portal - VNet yapılandırın](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   > [!IMPORTANT]
   > Hizmet uç noktaları yapılandırmadan önce bu makalede hizmet uç noktası yapılandırması ve konuları hakkında okumak için önerilir. **Sanal Ağ Hizmeti uç noktası:** A [sanal ağ hizmeti uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) özellik değerleri içeren bir veya daha fazla resmi Azure hizmeti tür adları bir alt ağ. Sanal Ağ Hizmetleri uç noktaları kullanma hizmet türü adı **Microsoft.Sql**, adlandırılmış SQL Database, Azure hizmetine başvurduğu. Bu hizmet etiketi de Azure SQL Database, Azure veritabanı PostgreSQL ve MySQL Hizmetleri için geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi VNet Hizmeti uç noktası için hizmet uç noktası trafiği Azure SQL Database, Azure veritabanı PostgreSQL için de dahil olmak üzere tüm Azure veritabanı hizmetleri için yapılandırır ve Azure veritabanı alt ağdaki MySQL sunucuları için. 
   > 

3. Bir kez etkinleştirildikten sonra tıklatın **Tamam** ve VNet hizmet uç noktaları ile birlikte bir sanal ağ kuralı etkin olduğunu görürsünüz.

   ![VNet Hizmeti uç noktaları etkin ve oluşturulan sanal ağ kuralı](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [etkinleştirmek VNet hizmet uç noktaları ve Azure CLI kullanarak MySQL için Azure veritabanı için bir sanal ağ kuralı oluşturma](howto-manage-vnet-using-cli.md).
- MySQL sunucusu için bir Azure veritabanına bağlanmada daha fazla yardım için bkz: [Azure veritabanı için MySQL için bağlantı kitaplıkları](./concepts-connection-libraries.md)
