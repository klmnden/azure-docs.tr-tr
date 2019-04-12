---
title: Oluşturma ve PostgreSQL için Azure veritabanı'nda güvenlik duvarı kurallarını yönetme
description: Oluşturma ve Azure veritabanı Azure portalını kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/09/2019
ms.openlocfilehash: cb142e01009efbeaabd5d4e56dbedfe6384c5fc6
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495674"
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Oluşturma ve Azure veritabanı Azure portalını kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme
Sunucu düzeyinde güvenlik duvarı kuralları, bir Azure veritabanı'na PostgreSQL sunucusu için belirtilen bir IP adresi veya IP adresi aralığı erişimi yönetmek için kullanılabilir.

Sanal ağ (VNet) kuralları, sunucunuza erişim güvenliğini sağlamak için de kullanılabilir. Daha fazla bilgi edinin [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar](howto-manage-vnet-using-portal.md).

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- Bir sunucu [PostgreSQL için Azure veritabanı oluşturma](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma
1. PostgreSQL sunucusu sayfasında, ayarları altındaki başlığı tıklayın **bağlantı güvenliği** PostgreSQL için Azure veritabanı için bağlantı güvenliği sayfasına açmak.

   ![Azure portalı - bağlantı güvenliği](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Tıklayın **IP'mi Ekle** araç. Bu otomatik olarak bir güvenlik duvarı kuralı, bilgisayarınızın genel IP adresi ile Azure sistem tarafından algılandığı şekilde oluşturur.

   ![Azure portalı - IP'mi Ekle seçeneğine tıklayın](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portal tarafından gözlemlenen IP adresi, İnternet'e ve Azure sunucuları erişirken kullanılan IP adresi farklıdır. Bu nedenle, Başlangıç hem bitiş IP'si beklendiği gibi kuralı işlev yapmak için değiştirmeniz gerekebilir.
   Kendi IP adresinizi kontrol etmek için bir arama motoru veya çevrimiçi bir Aracı'nı kullanın. Örneğin, arama "IP Adresim nedir?"

   ![Bing arama IP Adresim nedir](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıkları ekleyin. PostgreSQL için Azure veritabanı güvenlik duvarı kurallarında, tek bir IP adresi veya adres aralığını belirtebilirsiniz. Tek bir IP adresi için kural sınırlandırmak istiyorsanız, Başlangıç hem bitiş IP'si için aynı adres alanına yazın. Güvenlik duvarını açmak, Yöneticiler, kullanıcılar ve geçerli kimlik bilgilerine sahip oldukları PostgreSQL sunucusu üzerinde herhangi bir veritabanına erişmek için uygulamaları sağlar.

   ![Azure portalı - güvenlik duvarı kuralları](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Tıklayın **Kaydet** bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda. Güvenlik duvarı kurallarının güncelleştirmenin başarılı olduğunu onaylanmasını bekleyin.

   ![Azure portalı - Kaydet'e tıklayın](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
PostgreSQL için Azure veritabanı sunucunuza bağlanmak Azure uygulamalarından izin vermek için Azure bağlantılarının etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure sanal Makinesinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi bağlanmak için. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** seçeneğini **ON** Portalı'nda **bağlantı güvenliği** bölmesi ve isabet **Kaydet**. Bağlantı denemesi izin verilmiyorsa, istek PostgreSQL sunucusu için Azure veritabanı ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetme adımlarını yineleyin.
* Kullanmakta olduğunuz bilgisayarı eklemek için düğmeyi tıklatın + **IP'mi Ekle**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için üç nokta […] ve **Sil** kuralı kaldırmak için. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [oluşturun ve Azure veritabanı Azure CLI kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-cli.md).
- Daha fazla güvenli sunucunuz tarafından erişim [oluşturma ve yönetme sanal ağ hizmet uç noktaları ve Azure portalını kullanarak kurallar](howto-manage-vnet-using-portal.md).
- PostgreSQL sunucusu için Azure veritabanı bağlanma konusunda yardım için bkz. [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
