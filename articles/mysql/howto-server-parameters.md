---
title: "Sunucu parametreleri Azure veritabanında MySQL için yapılandırma"
description: "Bu makalede, Azure portalını kullanarak MySQL için Azure veritabanı'nda MySQL server parametreleri yapılandırmak açıklar."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: b3510c616d2a9ba66cb83cb998c42e03fdbb0f2b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
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
|Temel| Gen 4| 1| 1024| 50 |
|Temel| Gen 4| 2| 2560| 100 |
|Temel| Gen 5| 1| 1024| 50 |
|Temel| Gen 5| 2| 2560| 100 |
|Genel Amaçlı| Gen 4| 2| 2560| 200|
|Genel Amaçlı| Gen 4| 4| 5120| 400|
|Genel Amaçlı| Gen 4| 8| 10240| 800|
|Genel Amaçlı| Gen 4| 16| 20480| 1600|
|Genel Amaçlı| Gen 4| 32| 40960| 3200|
|Genel Amaçlı| Gen 5| 2| 2560| 200|
|Genel Amaçlı| Gen 5| 4| 5120| 400|
|Genel Amaçlı| Gen 5| 8| 10240| 800|
|Genel Amaçlı| Gen 5| 16| 20480| 1600|
|Genel Amaçlı| Gen 5| 32| 40960| 3200|
|Bellek için İyileştirilmiş| Gen 5| 2| 7168| 600|
|Bellek için İyileştirilmiş| Gen 5| 4| 15360| 1250|
|Bellek için İyileştirilmiş| Gen 5| 8| 30720| 2500|
|Bellek için İyileştirilmiş| Gen 5| 16| 62464| 5000|
|Bellek için İyileştirilmiş| Gen 5| 32| 125952| 10000| 

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
