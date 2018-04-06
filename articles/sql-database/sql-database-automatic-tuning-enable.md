---
title: Azure SQL veritabanı için otomatik olarak ayarlamayı etkinleştirmek | Microsoft Docs
description: Otomatik Azure SQL veritabanınızda kolayca ayarlama etkinleştirebilirsiniz.
services: sql-database
author: veljko-msft
manager: drasumic
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 09/19/2016
ms.author: vvasic
ms.openlocfilehash: 2ea5c0e640ac7e635692dafac1f30f377aa79625
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure SQL veritabanı sürekli sorgularınızı izler ve İş yükünüzün performansını iyileştirmek için yapabileceğiniz eylemi tanımlayan bir otomatik olarak yönetilen veri hizmetidir. Önerileri gözden geçirin ve el ile uygulamadan veya için otomatik olarak düzeltme eylemleri uygulamak Azure SQL veritabanı sağlar - bu olarak bilinir **otomatik ayarlama modu**. Otomatik ayarlama sunucu veya veritabanı düzeyinde etkinleştirilebilir.

## <a name="enable-automatic-tuning-on-server"></a>Otomatik sunucu üzerinde ayarlama etkinleştir
Sunucu düzeyinde otomatik ayarlama yapılandırmadan "Azure varsayılan olarak" devralmak veya yapılandırma devralmayan seçebilirsiniz. Etkin FORCE_LAST_GOOD_PLAN Azure varsayılan değerler, CREATE_INDEX etkin ve DROP_INDEX devre dışı.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Otomatik ayarlama e-posta bildirimlerini yapılandırma

Bkz: [otomatik e-posta bildirimlerini ayarlama](sql-database-automatic-tuning-email-notifications.md)

### <a name="azure-portal"></a>Azure portalına
Azure SQL veritabanı sunucusuna otomatik olarak ayarlamayı etkinleştirmek için Azure portalında sunucusuna gidin ve ardından **otomatik ayarlama** menüde. İstediğiniz etkinleştirmek ve seçmek için otomatik ayarlama seçenekleri seçin **Uygula**:

![Sunucu](./media/sql-database-automatic-tuning-enable/server.png)

Sunucuda otomatik ayarlama seçenekleri, sunucudaki tüm veritabanları için uygulanır. Varsayılan olarak, tüm veritabanları, kendi üst sunucusundan yapılandırmasını devralmak, ancak bu kullanılabilir geçersiz kılındı ve tek tek her veritabanı için belirtilen.

### <a name="rest-api"></a>REST API
[Daha fazla bilgi için buraya tıklayın sunucu düzeyindeki REST API aracılığıyla otomatik ayarlama etkinleştirme hakkında](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

## <a name="enable-automatic-tuning-on-an-individual-database"></a>Otomatik tek bir veritabanının üzerine ayarlama etkinleştir

Azure SQL veritabanı ayrı ayrı her veritabanını ayarlama yapılandırmasını otomatik belirtmenize olanak sağlar. Veritabanı düzeyinde üst sunucusundan, "Azure varsayılan olarak" otomatik ayarlama yapılandırmasını devralmak için veya yapılandırma devralmayan seçebilirsiniz. Etkin FORCE_LAST_GOOD_PLAN Azure varsayılan değerler, CREATE_INDEX etkin ve DROP_INDEX devre dışı.

> [!NOTE]
> Aynı yapılandırma ayarları her veritabanı üzerinde otomatik olarak uygulanabilir şekilde otomatik ayarlama yapılandırma sunucusu düzeyinde yönetmek için genel yüklenmemesi önerilir. Veritabanı farklı ise tek bir veritabanının üzerine otomatik ayarlama başkalarının aynı sunucuda yapılandırın.
>

### <a name="azure-portal"></a>Azure portalına

Otomatik üzerinde tek bir veritabanı ayarlamayı etkinleştirmek için Azure portalında veritabanına gidin ve sonra hem de seçin **otomatik ayarlama**. Yapılandırma veritabanı için ayrı ayrı belirtin veya seçeneğini belirleyerek sunucudan ayarlarını devralmak için tek bir veritabanı yapılandırabilirsiniz.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Uygun yapılandırma seçtikten sonra tıklatın **Uygula**.

### <a name="rest-api"></a>REST API'si
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

## <a name="next-steps"></a>Sonraki adımlar
* Okuma [otomatik ayarlama makale](sql-database-automatic-tuning.md) nasıl performansınızı geliştirmenize yardımcı olabilir ve otomatik ayarlama hakkında daha fazla bilgi edinmek için.
* Bkz: [performans önerileri](sql-database-advisor.md) Azure SQL veritabanı performans önerileri genel bakış.
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) üst sorgularınızı performans etkisini görüntüleme hakkında bilgi edinmek için.
