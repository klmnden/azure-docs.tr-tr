---
title: Azure SQL veritabanı için otomatik olarak ayarlamayı etkinleştirmek | Microsoft Docs
description: Otomatik Azure SQL veritabanınızda kolayca ayarlama etkinleştirebilirsiniz.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: vvasic
ms.openlocfilehash: d4d3b7f54c7393b57339ea149e8a79f97891dc20
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646040"
---
# <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure SQL veritabanı sürekli sorgularınızı izler ve İş yükünüzün performansını iyileştirmek için yapabileceğiniz eylemi tanımlayan bir otomatik olarak yönetilen veri hizmetidir. Önerileri gözden geçirin ve el ile uygulamadan veya için otomatik olarak düzeltme eylemleri uygulamak Azure SQL veritabanı sağlar - bu olarak bilinir **otomatik ayarlama modu**. Otomatik ayarlama sunucu veya veritabanı düzeyinde etkinleştirilebilir.

## <a name="enable-automatic-tuning-on-server"></a>Otomatik sunucu üzerinde ayarlama etkinleştir
Sunucu düzeyinde otomatik ayarlama yapılandırmadan "Azure varsayılan olarak" devralmak veya yapılandırma devralmayan seçebilirsiniz. Azure varsayılan FORCE_LAST_GOOD_PLAN etkin, CREATE_INDEX etkin ve DROP_INDEX devre dışı olur.

### <a name="azure-portal"></a>Azure portalına
Azure SQL Database mantıksal otomatik ayarlamayı etkinleştirmek için **server**, Azure portalında sunucusuna gidin ve ardından **otomatik ayarlama** menüde.

![Sunucu](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Lütfen unutmayın **DROP_INDEX** seçenek şu anda bölüm değiştirme ve dizin ipuçlarını kullanarak uygulamaları ile uyumlu değildir ve bu gibi durumlarda etkinleştirilmemelidir.
>

İstediğiniz etkinleştirmek ve seçmek için otomatik ayarlama seçenekleri seçin **Uygula**.

Bir sunucuda otomatik ayarlama seçenekleri, bu sunucudaki tüm veritabanları için uygulanır. Varsayılan olarak, tüm veritabanları, kendi üst sunucusundan yapılandırmasını devralmak, ancak bu kullanılabilir geçersiz kılındı ve tek tek her veritabanı için belirtilen.

### <a name="rest-api"></a>REST API
[Daha fazla bilgi için buraya tıklayın sunucu düzeyindeki REST API aracılığıyla otomatik ayarlama etkinleştirme hakkında](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

## <a name="enable-automatic-tuning-on-an-individual-database"></a>Otomatik tek bir veritabanının üzerine ayarlama etkinleştir

Azure SQL veritabanı ayrı ayrı her veritabanı için otomatik ayarlama yapılandırması belirtmenize olanak sağlar. Veritabanı düzeyinde üst sunucusundan, "Azure varsayılan olarak" otomatik ayarlama yapılandırmasını devralmak için veya yapılandırma devralmayan seçebilirsiniz. Azure varsayılan ayarlandığında FORCE_LAST_GOOD_PLAN etkinleştirilmişse, CREATE_INDEX etkindir ve DROP_INDEX devre dışıdır.

> [!NOTE]
> Otomatik ayarlama yapılandırmasını yönetmek için genel önerilir **sunucu düzeyinde** şekilde aynı yapılandırma ayarları her veritabanı üzerinde otomatik olarak uygulanabilir. Yalnızca bu veritabanını diğerlerinden farklı ayarlara sahip gerekiyorsa tek bir veritabanının üzerine otomatik ayarlama yapılandırma ayarları aynı sunucudan devralma.
>

### <a name="azure-portal"></a>Azure portalına

Üzerinde otomatik ayarlamayı etkinleştirmek için bir **tek veritabanı**, Azure portalında veritabanına gidin ve seçin **otomatik ayarlama**.

Otomatik ayarlama ayarlardan ayrı olarak her veritabanı için yapılandırılabilir. El ile tek bir otomatik ayarlama seçeneği yapılandırmak veya bir seçenek sunucudan ayarlarını devralır belirtin.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Şu anda DROP_INDEX seçeneği bölüm değiştirme ve dizin ipuçlarını kullanarak uygulamaları ile uyumlu değil ve bu gibi durumlarda etkinleştirilmemelidir unutmayın.

İstenen yapılandırma seçtikten sonra tıklatın **Uygula**.

### <a name="rest-api"></a>REST API
[Daha fazla bilgi için burayı tıklatın REST API aracılığıyla tek bir veritabanı üzerinde otomatik ayarlama etkinleştirme hakkında](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning)

### <a name="t-sql"></a>T-SQL

T-SQL aracılığıyla tek bir veritabanı ayarlamayı otomatik etkinleştirmek için veritabanına bağlanmak ve aşağıdaki sorguyu çalıştırın:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
Otomatik otomatik olarak ayarlamayı ayarlama, Azure varsayılan olarak uygulanır. İçin DEVRAL ayarlama, üst sunucusundan otomatik ayarlama yapılandırma devralınır. ÖZEL seçme, otomatik ayar el ile yapılandırmanız gerekir.

T-SQL aracılığıyla tek tek otomatik ayarlama seçeneklerini yapılandırmak için veritabanına bağlanmak ve bu gibi sorgu yürütün:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
Tek tek ayarlama seçeneği ON olarak ayarlanamadı, devralınan veritabanı herhangi ayarını geçersiz kılın ve ayarlama seçeneğini etkinleştirin. OFF olarak ayarlama, ayrıca devralınan veritabanı herhangi ayarını geçersiz kılmak ve ayarlama seçeneğini devre dışı bırakın. Kendisi için varsayılan belirtilirse, otomatik ayarlama seçeneği yapılandırma ayarı ayarlama otomatik veritabanı düzeyden devralır.  

## <a name="disabled-by-the-system"></a>Sistem tarafından devre dışı bırakıldı
Otomatik ayarlama veritabanında gereken tüm eylemleri izleme ve bazı durumlarda bu otomatik ayarlama düzgün veritabanı üzerinde çalışamaz olduğunu belirleyebilirsiniz. Bu durumda ayarlama seçeneği sistem tarafından devre dışı bırakılır. Çoğu durumda, Query Store etkin değil veya belirli bir veritabanı salt okunur durumda olduğundan bu gerçekleşir.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Otomatik ayarlama e-posta bildirimlerini yapılandırma

Bkz: [otomatik e-posta bildirimlerini ayarlama](sql-database-automatic-tuning-email-notifications.md)

## <a name="next-steps"></a>Sonraki adımlar
* Okuma [otomatik ayarlama makale](sql-database-automatic-tuning.md) nasıl performansınızı geliştirmenize yardımcı olabilir ve otomatik ayarlama hakkında daha fazla bilgi edinmek için.
* Bkz: [performans önerileri](sql-database-advisor.md) Azure SQL veritabanı performans önerileri genel bakış.
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) üst sorgularınızı performans etkisini görüntüleme hakkında bilgi edinmek için.
