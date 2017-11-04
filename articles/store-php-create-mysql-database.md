---
title: "Azure’da MySQL veritabanı oluşturma ve ona bağlanma"
description: "Bir MySQL veritabanı oluşturun ve bir PHP web uygulamasını Azure buna bağlanmak için Azure portalını kullanmayı öğrenin."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: c072cb3a7d376d1e3c2b9f741f5410106e701256
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Azure’da MySQL veritabanı oluşturma ve ona bağlanma
Bu öğretici bir MySQL veritabanının oluşturulacağı gösterilmektedir [Azure portal](https://portal.azure.com) (sağlayıcısı [ClearDB](http://www.cleardb.com/)) ve çalışır durumda bir PHP web uygulamasından bağlanmak nasıl [Azure App Service](app-service/app-service-web-overview.md).

> [!NOTE]
> Ayrıca bir MySQL veritabanı parçası olarak oluşturabileceğiniz bir <a href="https://portal.azure.com/#create/WordPress.WordPress" target="_blank">Market uygulaması şablonu</a>.
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Azure portalında bir MySQL veritabanı oluşturma
Azure portalında bir MySQL veritabanı oluşturmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Sol menüden **yeni** > **veri + depolama** > **MySQL veritabanı**.

    ![Azure - başlangıç MySQL veritabanı oluşturma](./media/store-php-create-mysql-database/create-db-1-start.png)
3. Yeni MySQL veritabanı [dikey](azure-portal-overview.md), yeni MySQL veritabanınız şu şekilde yapılandırın (*dikey*: yatay açılır bir portal sayfası):

   * **Veritabanı adı**: benzersiz olarak tanımlanabilen bir ad yazın
   * **Abonelik**: kullanılacak aboneliği seçin
   * **Veritabanı türü**: seçin **paylaşılan** için düşük maliyetli veya ücretsiz katman veya **adanmış** özel kaynakları alınamadı.
   * **Kaynak grubu**: mevcut bir MySQL veritabanı Ekle [kaynak grubu](azure-resource-manager/resource-group-overview.md) veya yeni bir yerleştirin. Aynı gruptaki kaynakların birlikte kolayca yönetilebilir.
   * **Konum**: yakın bir konum seçin. Varolan bir kaynak grubuna eklerken, kaynak grubunun konuma kilitli.
   * **Fiyatlandırma katmanı**: tıklatın **fiyatlandırma katmanı**, ardından fiyatlandırma seçeneğini belirleyin (**Mercury** katman ücretsiz) ve ardından **seçin**.
   * **Yasal koşullar**: tıklatın **yasal koşulları**, satın alma ayrıntılarını gözden geçirin ve'ı tıklatın **satın**.
   * **Panoya Sabitle**: doğrudan panodan erişim isteyip istemediğinizi seçin. Henüz portal gezinme alışık değilseniz bu özellikle yararlıdır.

     Aşağıdaki ekran görüntüsünde, MySQL veritabanınızı nasıl yapılandırabileceğiniz yalnızca bir örnektir.  
     ![Azure üzerinde MySQL veritabanı oluşturma - yapılandırma](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Bitirdiğinizde yapılandırma, tıklatın **oluşturma**.

    ![Azure üzerinde MySQL veritabanı oluşturma - oluşturma](./media/store-php-create-mysql-database/create-db-3-create.png)

    Dağıtım başladı bildiğiniz bir açılır bildirim veren görürsünüz.

    ![Bir MySQL veritabanı - ediyor oluşturma](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Dağıtım başarılı olduktan sonra başka bir açılır alırsınız. Portal, MySQL veritabanı dikey aynı zamanda otomatik olarak açar.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>MySQL veritabanına bağlan
Yeni MySQL veritabanınız için bağlantı bilgilerini görmek için tıklatmanız **özellikleri** web uygulamanızın dikey penceresinde.

![Azure - MySQL veritabanı dikey bir MySQL veritabanı oluşturma](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Bu gibi durumlarda, bu bağlantı bilgisini artık herhangi bir web uygulamasına kullanabilirsiniz. Basit bir PHP uygulaması'ndan bağlantı bilgisinin nasıl kullanılacağını gösteren bir örnek kullanılabilir [burada](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [PHP Geliştirici Merkezi](/develop/php/).
