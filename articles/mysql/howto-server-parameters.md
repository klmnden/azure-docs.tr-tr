---
title: Sunucu parametreleri Azure veritabanında MySQL için yapılandırma
description: Bu makalede, Azure portalını kullanarak MySQL için Azure veritabanı'nda MySQL server parametreleri yapılandırmak açıklar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6865663bebc84df288f4c7e2564ddb4870667c6f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Sunucu parametreleri Azure veritabanı'nda MySQL için Azure portalını kullanarak nasıl yapılandırılır

Azure veritabanı MySQL için bazı sunucu parametreleri yapılandırmasını destekler. Bu makalede, Azure portalını kullanarak bu parametreleri yapılandırmak açıklar. Tüm sunucu parametreleri ayarlanabilir. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure Portal'da sunucu parametreleri gidin
1. Azure portalında oturum açın ve ardından Azure veritabanınızı MySQL sunucusu için bulun.
2. Altında **ayarları** 'yi tıklatın **sunucu parametreleri** sunucu parametreleri sayfası Azure veritabanı için MySQL için açın.
![Azure portal sunucusu parametreleri sayfası](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Ayarlamanız gereken herhangi bir ayarı bulun. Gözden geçirme **açıklama** amacı ve izin verilen değerler anlamak için sütun. 
![Aşağı açılan listeleme](./media/howto-server-parameters/3-toggle_parameter.png)
4. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için.
![Kaydet veya değişiklikleri at](./media/howto-server-parameters/4-save_parameters.png)
5. Yeni parametre değerlerini kaydettiyseniz, her zaman varsayılan değerleri dön her şeyi seçerek geri dönebilirsiniz **tümünü Varsayılana Sıfırla**.
![Tüm Varsayılana sıfırla](./media/howto-server-parameters/5-reset_parameters.png)


## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerin listesi

Desteklenen sunucu parametrelerin listesi sürekli olarak artmaktadır. Sunucu parametreleri sekmesi Azure portalında tanım almak ve sunucu parametreleri uygulama gereksinimlerinize göre yapılandırmak için kullanın. 

## <a name="nonconfigurable-server-parameters"></a>Nonconfigurable sunucu parametreleri
InnoDB arabellek havuzu ve en fazla bağlantı olmayan yapılandırılabilir ve bağlı için [fiyatlandırma katmanı](concepts-service-tiers.md). 

|**Fiyatlandırma Katmanı**| **İşlem oluşturma**|**vCore(s)**|**InnoDB arabellek havuzu (MB)**| **En fazla bağlantı**|
|---|---|---|---|--|
|Temel| 4. Nesil| 1| 1024| 50|
|Temel| 4. Nesil| 2| 2560| 100|
|Temel| 5. Nesil| 1| 1024| 50|
|Temel| 5. Nesil| 2| 2560| 100|
|Genel Amaçlı| 4. Nesil| 2| 3584| 300|
|Genel Amaçlı| 4. Nesil| 4| 7680| 625|
|Genel Amaçlı| 4. Nesil| 8| 15360| 1250|
|Genel Amaçlı| 4. Nesil| 16| 31232| 2500|
|Genel Amaçlı| 4. Nesil| 32| 62976| 5000|
|Genel Amaçlı| 5. Nesil| 2| 3584| 300|
|Genel Amaçlı| 5. Nesil| 4| 7680| 625|
|Genel Amaçlı| 5. Nesil| 8| 15360| 1250|
|Genel Amaçlı| 5. Nesil| 16| 31232| 2500|
|Genel Amaçlı| 5. Nesil| 32| 62976| 5000|
|Bellek için İyileştirilmiş| 5. Nesil| 2| 7168| 600|
|Bellek için İyileştirilmiş| 5. Nesil| 4| 15360| 1250|
|Bellek için İyileştirilmiş| 5. Nesil| 8| 30720| 2500|
|Bellek için İyileştirilmiş| 5. Nesil| 16| 62464| 5000|

Bu ek sunucu parametreler sistemde yapılandırılabilir değildir:

|**Parametre**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanındaki innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Burada listelenmeyen diğer sunucu sürümleri için MySQL out-of-box varsayılan değerlerine parametrelerinin [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) ve [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
