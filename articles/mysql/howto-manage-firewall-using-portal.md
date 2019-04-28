---
title: Oluşturma ve MySQL için Azure veritabanı'nda MySQL güvenlik duvarı kurallarını yönetme
description: Oluşturma ve Azure veritabanı Azure portalını kullanarak MySQL için güvenlik duvarı kurallarını yönetme
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: 017266fd28fb31b4509957560a042abf74314453
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61458878"
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak Azure veritabanı için MySQL güvenlik duvarı kurallarını yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, bir Azure veritabanı'na MySQL sunucusu için belirtilen bir IP adresi veya bir IP adresi aralığı erişimi yönetmek için kullanılabilir. 

Sanal ağ (VNet) kuralları, sunucunuza erişim güvenliğini sağlamak için de kullanılabilir. Daha fazla bilgi edinin [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar](howto-manage-vnet-using-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

1. MySQL sunucusu sayfasında, ayarları altındaki başlığı tıklayın **bağlantı güvenliği** için MySQL için Azure veritabanı bağlantı güvenliği sayfasını açmak.

   ![Azure portalı - bağlantı güvenliği](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Tıklayın **IP'mi Ekle** araç. Bu otomatik olarak bir güvenlik duvarı kuralı, bilgisayarınızın genel IP adresi ile Azure sistem tarafından algılandığı şekilde oluşturur.

   ![Azure portalı - IP'mi Ekle seçeneğine tıklayın](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portal tarafından gözlemlenen IP adresi, İnternet'e ve Azure sunucuları erişirken kullanılan IP adresi farklıdır. Bu nedenle, Başlangıç hem bitiş IP'si beklendiği gibi kuralı işlev yapmak için değiştirmeniz gerekebilir.

   Kendi IP adresinizi kontrol etmek için bir arama motoru veya çevrimiçi bir Aracı'nı kullanın. Örneğin, "IP Adresim nedir" aratın. 

   ![IP Adresim nedir için Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıkları ekleyin. MySQL için Azure veritabanı için güvenlik duvarı kurallarında, tek bir IP adresi veya adres aralığını belirtebilirsiniz. Tek bir IP adresi için kural sınırlandırmak istiyorsanız, Başlangıç hem bitiş IP'si alanları aynı adresini yazın. Güvenlik duvarını açmak, Yöneticiler, kullanıcılar ve MySQL sunucusundaki geçerli kimlik bilgilerine sahip oldukları herhangi bir veritabanına erişmek için uygulamayı etkinleştirir.

   ![Azure portalı - güvenlik duvarı kuralları](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Tıklayın **Kaydet** bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda. Güvenlik duvarı kurallarının güncelleştirmenin başarılı olduğunu onaylanmasını bekleyin.

   ![Azure portalı - Kaydet'e tıklayın](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
MySQL için Azure veritabanı sunucunuza bağlanmak Azure uygulamalarından izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure sanal Makinesinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi bağlanmak için. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** seçeneğini **ON** Portalı'nda **bağlantı güvenliği** bölmesi ve isabet **Kaydet**. İstek, bağlantı denemesi izin verilmiyorsa, MySQL için Azure veritabanı ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Azure portalını kullanarak mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetme adımlarını yineleyin.
* Kullanmakta olduğunuz bilgisayarı eklemek için tıklayın **+ IP'mi Ekle**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Ek IP adresi eklemek için yazın **kural adı**, **başlangıç IP**, ve **bitiş IP**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan tıklayın ve ardından değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için […] üç noktaya tıklayın ve ardından **Sil**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [oluşturun ve Azure veritabanı Azure CLI kullanarak MySQL için güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-cli.md).
- Daha fazla güvenli sunucunuz tarafından erişim [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar](howto-manage-vnet-using-portal.md).
- MySQL sunucusu için Azure veritabanı bağlanma konusunda yardım için bkz [MySQL için Azure veritabanı için bağlantı kitaplıkları](./concepts-connection-libraries.md).