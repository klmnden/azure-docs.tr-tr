---
title: "Sunucu parametreleri Azure veritabanında MySQL için yapılandırma | Microsoft Docs"
description: "Bu makalede, Azure portalını kullanarak MySQL için Azure veritabanı'nda MySQL server parametreleri yapılandırmak açıklar."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/10/2017
ms.openlocfilehash: 06c7f9f6bd49ebfaf03b04cb6e30b963593bfb35
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Sunucu parametreleri Azure veritabanı'nda MySQL için Azure portalını kullanarak nasıl yapılandırılır

Azure veritabanı MySQL için bazı sunucu parametreleri yapılandırmasını destekler. Bu konuda, Azure portalını kullanarak bu parametreleri yapılandırmak açıklar. Tüm sunucu parametreleri ayarlanabilir. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure Portal'da sunucu parametreleri gidin
1. Azure portalında oturum açın ve ardından Azure veritabanınızı MySQL sunucusu için bulun.
2. Altında **ayarları** 'yi tıklatın **sunucu parametreleri** sunucu parametreleri sayfası Azure veritabanı için MySQL için açın.
3. Ayarlamanız gereken herhangi bir ayarı bulun. Gözden geçirme **açıklama** amacı ve izin verilen değerler anlamak için sütun. 
4. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için.

![Azure portal sunucusu parametreler dikey](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerin listesi

Desteklenen sunucu parametrelerin listesi sürekli olarak artmaktadır. Sunucu parametreleri sekmesi Azure portalında tanım almak ve sunucu parametreleri uygulama gereksinimlerinize göre yapılandırmak için kullanın. 

## <a name="nonconfigurable-server-parameters"></a>Nonconfigurable sunucu parametreleri

Aşağıdaki parametreleri yapılandırılabilir ve için bağlı değildir, [fiyatlandırma katmanı](concepts-service-tiers.md). 

| **Fiyatlandırma katmanı** | **InnoDB arabellek havuzu (MB)** | **En fazla bağlantı** |
| :------------------------ | :-------- | :----------- |
| Temel 50 | 1024 | 50 | 
| Temel 100  | 2560 | 100 | 
| Standart 100 | 2560 | 200 | 
| Standart 200 | 5120 | 400 | 
| Standart 400 | 10240 | 800 | 
| Standart 800 | 20480 | 1600 |

Diğer sunucu parametresi varsayılan değerlerini sürümü için [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) ve [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).
