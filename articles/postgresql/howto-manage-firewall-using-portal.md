---
title: "Oluşturma ve güvenlik duvarı kuralları Azure veritabanındaki PostgreSQL için yönetme"
description: "Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure Portalı'nı kullanarak yönetme"
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: bef927cff49d957728a2a12362786d48d60e61b7
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure Portalı'nı kullanarak yönetme
Belirtilen IP adresi veya IP adresi aralığı PostgreSQL sunucusu için bir Azure veritabanına erişmek yöneticiler sunucu düzeyinde güvenlik duvarı kuralları etkinleştirin. 

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- Bir sunucu [PostgreSQL için bir Azure veritabanı oluşturma](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma
1. PostgreSQL sunucusu sayfasında, ayarları altında başlığını tıklatın **bağlantı güvenliği** PostgreSQL için Azure veritabanı için bağlantı güvenliği sayfası açın.

  ![Azure portal - bağlantı güvenliği](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Tıklatın **eklemek IP** araç çubuğunda. Bu otomatik olarak bir güvenlik duvarı kuralı, bilgisayarınızın ortak IP adresiyle Azure sistem tarafından algılanan olarak oluşturur.

  ![Azure portal - My IP Ekle'yi tıklatın](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portal tarafından gözlenen IP adresi internet ve Azure sunucuları erişirken kullanılan IP adresinden farklıdır. Bu nedenle, başlangıç IP ve bitiş IP beklendiği gibi kural işlevi yapmak için değiştirmeniz gerekebilir.
Kendi IP adresini denetlemek için bir arama motoru veya diğer çevrimiçi aracı kullanın. Örneğin, "Benim IP nedir."

  ![Bing arama my IP nedir](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıklarını ekleyin. Azure veritabanı PostgreSQL için güvenlik duvarı kurallarında tek bir IP adresi veya adres aralığı belirtebilirsiniz. Kural tek bir IP adresi için sınırlamak istiyorsanız, IP başlangıç ve bitiş IP için aynı adres alanına yazın. Güvenlik Duvarı'nı açarak, Yöneticiler, kullanıcılar ve uygulamalar oturum açmak için PostgreSQL sunucusunda herhangi bir veritabanı için geçerli kimlik bilgilerine sahip oldukları sağlar.

  ![Azure portal - güvenlik duvarı kuralları ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Tıklatın **kaydetmek** bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda. Güvenlik duvarı kurallarının Güncelleştirme başarılı onaylanmasını bekleyin.

  ![Azure portal - Kaydet](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure uygulamalardan PostgreSQL server için Azure veritabanına bağlanmak izin vermek için Azure bağlantıları etkinleştirilmesi gerekir. Örneğin, bir Azure Web Apps uygulama veya bir Azure VM içinde çalışan bir uygulamayı barındırmak için veya bir Azure Data Factory veri yönetimi ağ geçidi'nden bağlanma. Kaynakların bu bağlantıları etkinleştirmek için aynı sanal ağ (VNet) veya kaynak grubu için güvenlik duvarı kuralı olması gerekmez. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Birkaç bu tür bağlantıları etkinleştirmek için yöntem vardır. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Alternatif olarak, ayarlayabileceğiniz **Azure hizmetlerine erişime izin ver** için seçenek **ON** Portalı'nda **bağlantı güvenliği** bölmesinde ve isabet **Kaydet**. Bağlantı denemesi izin verilmiyorsa, istek PostgreSQL sunucu için Azure veritabanı ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetmek için gereken adımları yineleyin.
* Geçerli bilgisayar eklemek için düğmeyi tıklatın + **eklemek IP**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için [...] üç nokta düğmesine tıklayın ve **silmek** kuralı kaldırmak için. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Benzer şekilde, yazabilirsiniz [oluşturma ve Azure veritabanı PostgreSQL güvenlik duvarı kuralları için Azure CLI kullanarak yönetme](howto-manage-firewall-using-cli.md).
- PostgreSQL sunucu için bir Azure veritabanına bağlanma konusunda yardım için bkz: [PostgreSQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
