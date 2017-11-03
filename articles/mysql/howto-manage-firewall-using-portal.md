---
title: "Oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure Portalı'nı kullanarak yönetme | Microsoft Docs"
description: "Oluşturma ve Azure veritabanı için MySQL güvenlik duvarı kuralları Azure Portalı'nı kullanarak yönetme"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 0604b29fcd9849545886a783ae5bbb2cbb72f2ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Oluşturma ve Azure portalını kullanarak Azure veritabanı için MySQL güvenlik duvarı kurallarını yönetme
Belirtilen IP adresi veya bir IP adresi aralığı MySQL sunucusu için bir Azure veritabanına erişmek yöneticiler sunucu düzeyinde güvenlik duvarı kuralları etkinleştirin. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

1. MySQL server dikey penceresinde, ayarlar altında başlığını tıklatın **bağlantı güvenliği** MySQL için Azure veritabanı için bağlantı güvenliği dikey penceresini açmak için.

   ![Azure portal - bağlantı güvenliği](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Tıklatın **eklemek IP** Azure sistem tarafından algılanan gibi bilgisayarınızın IP adresiyle bir kural oluşturmak için araç çubuğunda.

   ![Azure portal - My IP Ekle'yi tıklatın](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda, Azure portal tarafından gözlenen IP adresi internet ve Azure sunucuları erişirken kullanılan IP adresinden farklıdır. Bu nedenle, başlangıç IP ve bitiş IP beklendiği gibi kural işlevi yapmak için değiştirmeniz gerekebilir.

   Kendi IP adresini denetlemek için bir arama motoru veya diğer çevrimiçi aracını kullanın (örneğin, "IP adresimi nedir" için arama yapın).

   ![My IP nedir için Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Ek adres aralıklarını ekleyin. Azure veritabanı için MySQL güvenlik duvarı kuralları, tek bir IP adresi veya adres aralığı belirtebilirsiniz. Kural tek bir IP adresi için sınırlamak istiyorsanız, IP başlangıç ve bitiş IP alanları aynı adresini yazın. Güvenlik Duvarı'nı açarak, Yöneticiler ve kullanıcılar geçerli kimlik bilgilerine sahip oldukları MySQL server üzerinde herhangi bir veritabanına erişmek etkinleştirir.

   ![Azure portal - güvenlik duvarı kuralları ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. Tıklatın **kaydetmek** bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda. Güvenlik duvarı kurallarının güncelleştirmenin başarılı olduğunu onaylanmasını bekleyin.

   ![Azure portal - Kaydet](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Azure portalını kullanarak var olan sunucu düzeyinde güvenlik duvarı kurallarını yönet
Güvenlik duvarı kurallarını yönetmek için gereken adımları yineleyin.
* Geçerli bilgisayar eklemek için tıklatın **+ IP eklemek**.
* Ek IP adresleri eklemek için yazın **kural adı**, **başlangıç IP**, ve **bitiş IP**.
* Mevcut bir kuralı değiştirmek için kuralı alanlarında birini tıklatın ve ardından değiştirin.
* Mevcut bir kuralı silmek için üç nokta [...]'ı tıklatın ve ardından **silmek**.
* Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- MySQL sunucusu için bir Azure veritabanına bağlanmada daha fazla yardım için bkz: [Azure veritabanı için MySQL için bağlantı kitaplıkları](./concepts-connection-libraries.md)
