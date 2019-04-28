---
title: Mevcut Azure App Service, MySQL için Azure veritabanı'na bağlanma
description: Düzgün mevcut bir Azure App Service'in MySQL için Azure veritabanı'na bağlanma yönergeleri
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: eb2fee7c76bcf29aee2dcd70d7975d7631bb23f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459309"
---
# <a name="connect-an-existing-azure-app-service-to-azure-database-for-mysql-server"></a>Mevcut bir Azure App Service, MySQL sunucusu için Azure veritabanı'na bağlanma
Bu konu, mevcut bir Azure App Service'in MySQL için Azure veritabanı sunucunuza bağlanmak nasıl açıklar.

## <a name="before-you-begin"></a>Başlamadan önce
[Azure Portal](https://portal.azure.com)’da oturum açın. MySQL için Azure veritabanı oluşturma. Ayrıntılar için başvurmak [portalından MySQL için Azure veritabanı oluşturma işlemini](quickstart-create-mysql-server-database-using-azure-portal.md) veya [CLI kullanarak MySQL sunucusu için Azure veritabanı oluşturma işlemini](quickstart-create-mysql-server-database-using-azure-cli.md).

Şu anda MySQL için Azure veritabanı, bir Azure App Service'in erişimi etkinleştirmek için iki çözümü vardır. Her iki çözüm de, sunucu düzeyinde güvenlik duvarı kurallarını ayarlama içerir.

## <a name="solution-1---allow-azure-services"></a>Çözüm 1 - Azure hizmetlerine izin ver
MySQL için Azure veritabanı, verilerinizi korumak için bir Güvenlik Duvarı'nı kullanarak erişim güvenliğini sağlar. Bir Azure App Service'ten MySQL server için Azure veritabanına bağlanırken, giden IP'ler, App Service dinamik nitelikte olduğunu aklınızda bulundurun. "Azure hizmetlerine erişime izin ver" seçeneği, MySQL sunucusuna bağlanmak app service izin verir.

1. MySQL sunucusu dikey penceresinde ayarlar altındaki başlığı tıklayın **bağlantı güvenliği** bağlantı güvenlik dikey penceresi için MySQL için Azure veritabanı açmak için.

   ![Azure portalı - bağlantı güvenliği](./media/howto-connect-webapp/1-connection-security.png)

2. Seçin **ON** içinde **Azure hizmetlerine erişime izin ver**, ardından **Kaydet**.
   ![Azure portal - Azure izin erişim](./media/howto-connect-webapp/allow-azure.png)

## <a name="solution-2---create-a-firewall-rule-to-explicitly-allow-outbound-ips"></a>2 - çözüm giden IP'ler açıkça izin vermek için bir güvenlik duvarı kuralı oluşturma
Azure App Service, tüm giden IP'ler açıkça ekleyebilirsiniz.

1. App Service özellikleri dikey penceresinde görüntülemek, **giden IP adresi**.

   ![Azure portalı - görünüm giden IP'ler](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. MySQL bağlantı güvenlik dikey penceresinde, giden IP'ler tek tek ekleyin.

   ![Azure portalı - açık IP'ler Ekle](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Unutmayın **Kaydet** , güvenlik duvarı kuralları.

Azure App service, zaman içinde IP adresleri değişmemesi dener ancak burada IP adresleri değişebilir durumlar vardır. Örneğin, bu durum ortaya çıkabilir ne zaman uygulama geri dönüşümlerine genel ya da bir ölçeklendirme işlemi gerçekleşir veya kapasitesini artırmak için Azure bölgesel veri yeni bilgisayarlar zaman eklenir ortalar. IP adresleri, artık MySQL sunucusuna bağlanabilir durumunda uygulama kapalı kalma süresiyle karşılaşabilir. Bu durum, önceki çözümlerden birini seçerken göz önünde bulundurun.

## <a name="ssl-configuration"></a>SSL yapılandırması
MySQL için Azure veritabanı, varsayılan olarak etkin SSL sahiptir. Uygulamanız veritabanına bağlanmak için SSL kullanmıyorsa, MySQL sunucunuzda SSL'yi devre dışı bırakmak gerekir. SSL yapılandırma hakkında daha fazla ayrıntı için bkz. [SSL kullanarak MySQL için Azure veritabanı ile](howto-configure-ssl.md).

## <a name="next-steps"></a>Sonraki adımlar
Bağlantı dizeleri hakkında daha fazla bilgi için [bağlantı dizeleri](howto-connection-string.md).
