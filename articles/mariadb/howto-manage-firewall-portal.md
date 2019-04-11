---
title: Oluşturma ve MariaDB için Azure veritabanı'nda MariaDB güvenlik duvarı kurallarını yönetme
description: Oluşturma ve Azure veritabanı Azure portalını kullanarak MariaDB için güvenlik duvarı kurallarını yönetme
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 04/09/2019
ms.openlocfilehash: e9ab243692f5a4a1ec7de25774f5bad867698fc3
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59470007"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak Azure veritabanı MariaDB için güvenlik duvarı kurallarını yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, bir Azure veritabanı'na MariaDB sunucusu için belirtilen bir IP adresi veya bir IP adresi aralığı erişimi yönetmek için kullanılabilir.

Sanal ağ (VNet) kuralları, sunucunuza erişim güvenliğini sağlamak için de kullanılabilir. Daha fazla bilgi edinin [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar](howto-manage-vnet-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

1. MariaDB sunucusu sayfasında, ayarları altındaki başlığı tıklayın **bağlantı güvenliği** için MariaDB için Azure veritabanı bağlantı güvenliği sayfasını açmak.

   ![Azure portalı - bağlantı güvenliği](./media/howto-manage-firewall-portal/1-connection-security.png)

2. Tıklayın **IP'mi Ekle** araç. Bu otomatik olarak bir güvenlik duvarı kuralı, bilgisayarınızın genel IP adresi ile Azure sistem tarafından algılandığı şekilde oluşturur.

   ![Azure portalı - IP'mi Ekle seçeneğine tıklayın](./media/howto-manage-firewall-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portal tarafından gözlemlenen IP adresi, İnternet'e ve Azure sunucuları erişirken kullanılan IP adresi farklıdır. Bu nedenle, Başlangıç hem bitiş IP'si beklendiği gibi kuralı işlev yapmak için değiştirmeniz gerekebilir.

   Kendi IP adresinizi kontrol etmek için bir arama motoru veya çevrimiçi bir Aracı'nı kullanın. Örneğin, "IP Adresim nedir" aratın.

4. Ek adres aralıkları ekleyin. MariaDB için Azure veritabanı için güvenlik duvarı kurallarında, tek bir IP adresi veya adres aralığını belirtebilirsiniz. Tek bir IP adresi için kural sınırlandırmak istiyorsanız, Başlangıç hem bitiş IP'si alanları aynı adresini yazın. Güvenlik duvarını açmak, Yöneticiler, kullanıcılar ve geçerli kimlik bilgilerine sahip oldukları MariaDB sunucu üzerindeki herhangi bir veritabanına erişmek için uygulamayı etkinleştirir.

   ![Azure portalı - güvenlik duvarı kuralları](./media/howto-manage-firewall-portal/4-specify-addresses.png)

5. Tıklayın **Kaydet** bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda. Güvenlik duvarı kurallarının güncelleştirmenin başarılı olduğunu onaylanmasını bekleyin.

   ![Azure portalı - Kaydet'e tıklayın](./media/howto-manage-firewall-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
MariaDB için Azure veritabanı sunucunuza bağlanmak Azure uygulamalarından izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure sanal Makinesinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi bağlanmak için. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** seçeneğini **ON** Portalı'nda **bağlantı güvenliği** bölmesi ve tıklayın **Kaydet**. Bağlantı denemesi izin verilmiyorsa, istek MariaDB için Azure veritabanı ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-firewall-rules-in-the-azure-portal"></a>Azure portalında mevcut güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetme adımlarını yineleyin.
* Kullanmakta olduğunuz bilgisayarı eklemek için tıklayın **+ IP'mi Ekle**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Ek IP adresi eklemek için yazın **kural adı**, **başlangıç IP**, ve **bitiş IP**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan tıklayın ve ardından değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için […] üç noktaya tıklayın ve ardından **Sil**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
 - Benzer şekilde, yazabilirsiniz [oluşturun ve Azure veritabanı Azure CLI kullanarak MariaDB için güvenlik duvarı kurallarını yönetme](howto-manage-firewall-cli.md).
 - Daha fazla güvenli sunucunuz tarafından erişim [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar](howto-manage-vnet-portal.md).