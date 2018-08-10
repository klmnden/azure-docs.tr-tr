---
title: Azure SQL veritabanı için otomatik ayarlamayı etkinleştirme | Microsoft Docs
description: Otomatik Azure SQL veritabanınızda kolayca ayarlama etkinleştirebilirsiniz.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 9ebc3a8cb01d93fc6cec5d208c5a10020413cec2
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631104"
---
# <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure SQL veritabanı, sürekli olarak izler, sorgularınızı ve İş yükünüzün performansını gerçekleştirdiğiniz eylemi tanımlayan bir otomatik olarak yönetilen veri hizmetidir. Yapabilir önerileri gözden geçirin ve el ile uygulayabilirsiniz veya Azure SQL veritabanı, otomatik düzeltici eylemler uygulayın sağlar - bu olarak bilinir **otomatik ayarlama modu**.

Sunucu veya veritabanı düzeyinde aracılığıyla otomatik ayarlama etkinleştirilebilir [Azure portalında](sql-database-automatic-tuning-enable.md#azure-portal), [REST API](sql-database-automatic-tuning-enable.md#rest-api) çağrıları ve [T-SQL](sql-database-automatic-tuning-enable.md#t-sql) komutları.

## <a name="enable-automatic-tuning-on-server"></a>Sunucu üzerinde otomatik ayarlamayı etkinleştirme
Sunucu düzeyinde "Azure varsayılan olarak" den otomatik ayarlama yapılandırması devralan veya yapılandırma devralmayan seçebilirsiniz. Azure varsayılan FORCE_LAST_GOOD_PLAN etkin CREATE_INDEX etkinleştirilir ve DROP_INDEX devre dışı olur.

### <a name="azure-portal"></a>Azure portal
Otomatik ayarlama Azure SQL veritabanı mantıksal etkinleştirmek için **sunucu**, Azure portalındaki sunucuya gidin ve ardından **otomatik ayarlama** menüsünde.

![Sunucu](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Lütfen unutmayın **DROP_INDEX** seçeneği şu anda bölüm değiştirme ve dizin ipuçlarını kullanarak uygulamaları ile uyumlu değildir ve bu gibi durumlarda etkinleştirilmemelidir.
>

Otomatik ayarlama seçeneklerini seçin ve etkinleştirmek istediğiniz seçin **Uygula**.

Bir sunucudaki otomatik ayarlama seçeneklerini, bu sunucudaki tüm veritabanları için uygulanır. Varsayılan olarak, tüm veritabanları yapılandırma kendi üst sunucudan devralır, ancak bunun geçersiz kılınacak ve tek tek her veritabanı için belirtilen.

### <a name="rest-api"></a>REST API

REST API kullanarak bir sunucu üzerinde otomatik ayarlamayı etkinleştirme, bkz: hakkında daha fazla bilgi edinin [SQL Server güncelleştirme hem de GET HTTP yöntemleri otomatik ayarlama](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).


## <a name="enable-automatic-tuning-on-an-individual-database"></a>Tek bir veritabanı üzerinde otomatik ayarlamayı etkinleştirme

Azure SQL veritabanı otomatik ayarlama yapılandırmasını her veritabanı için ayrı ayrı belirtmenize olanak sağlar. Veritabanı düzeyinde üst sunucudan, "Azure varsayılan olarak" otomatik ayarlama yapılandırması devralan veya yapılandırma devralmayan seçebilirsiniz. Azure Varsayılanları ayarlandığında FORCE_LAST_GOOD_PLAN etkin CREATE_INDEX etkin ve DROP_INDEX devre dışı bırakıldı.

> [!NOTE]
> Otomatik ayarlama yapılandırmasını yönetmek için genel önerilir **sunucu düzeyinde** şekilde her veritabanında aynı yapılandırma ayarlarını otomatik olarak uygulanabilir. Yalnızca bu veritabanı diğerlerinden farklı ayarlara sahip gerekiyorsa üzerinde tek bir veritabanı otomatik ayarlama yapılandırma aynı sunucudan ayarları devralıyor.
>

### <a name="azure-portal"></a>Azure portal

Üzerinde otomatik ayarlama etkinleştirmek için bir **tek veritabanı**, Azure portalında veritabanı bulun ve seçin **otomatik ayarlama**.

Otomatik ayarlama ayarları tek tek her veritabanı için ayrı olarak yapılandırılabilir. El ile tek bir otomatik ayarlama seçeneğini yapılandırabilir veya bir seçenek ayarlarını sunucudan devralıyor belirtin.

![Database](./media/sql-database-automatic-tuning-enable/database.png)

Şu anda DROP_INDEX seçeneği bölüm değiştirme ve dizin ipuçlarını kullanarak uygulamaları ile uyumlu değil ve bu gibi durumlarda etkinleştirilmemelidir lütfen unutmayın.

İstediğiniz yapılandırma seçtikten sonra tıklayın **Uygula**.

### <a name="rest-api"></a>REST API

Tek bir veritabanında otomatik ayarlama için REST API kullanma hakkında daha fazla bilgi için bkz: [SQL veritabanı otomatik ayarlama güncelleştirme hem de GET HTTP yöntemleri](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning).

### <a name="t-sql"></a>T-SQL

Otomatik ayarlama T-SQL aracılığıyla tek bir veritabanı üzerinde etkinleştirmek için veritabanına bağlanmak ve aşağıdaki sorguyu yürütün:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
   ```
   
Otomatik ayarlama otomatik ayarlama, Azure varsayılan olarak uygulanır. DEVRALMA için ayarlamak, ana sunucudan otomatik ayarlama yapılandırmasını devralınır. ÖZEL seçme, otomatik ayarlama el ile yapılandırmanız gerekecektir.

T-SQL aracılığıyla tek tek otomatik ayarlama seçeneklerini yapılandırmak için veritabanına bağlanmak ve bunun gibi bir sorguyu yürütün:

   ```T-SQL
   ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
   ```
   
Tek tek ayarlama seçeneği ON olarak ayarlanamadı, devralınan veritabanı tüm ayarını geçersiz kılın ve ayarlama seçeneğini etkinleştirin. OFF olarak ayarlamak, ayrıca devralınan veritabanı tüm ayarını geçersiz kılın ve ayarlama seçeneği devre dışı bırakın. Kendisi için varsayılan belirtilirse, otomatik ayarlama seçeneği yapılandırma veritabanı düzeyinden otomatik ayarlama ayarını devralır.  

Otomatik ayarlama yapılandırmak için bkz: T-SQL seçenekleri daha sonlandıran Bul [ALTER DATABASE SET seçenekleri (Transact-SQL) SQL veritabanı mantıksal sunucusu için](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-database-transact-sql-set-options?view=sql-server-2017&tabs=sqldbls#arguments-1).

## <a name="disabled-by-the-system"></a>Sistem tarafından devre dışı bırakıldı
Otomatik ayarlama, veritabanı üzerinde gereken tüm eylemleri izleme ve bazı durumlarda, otomatik ayarlama düzgün veritabanı üzerinde çalışamaz olduğunu belirleyebilirsiniz. Bu durumda ayarlama seçeneği sistem tarafından devre dışı bırakılır. Çoğu durumda, Query Store etkin değil veya belirli bir veritabanı salt okunur durumda olduğundan bu gerçekleşir.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Otomatik ayarlama e-posta bildirimlerini yapılandırma

Bkz: [otomatik ayarlama e-posta bildirimleri](sql-database-automatic-tuning-email-notifications.md) Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar
* Okuma [otomatik ayarlama makale](sql-database-automatic-tuning.md) nasıl performansınızı geliştirmenize yardımcı olabileceğini ve otomatik ayarlama hakkında daha fazla bilgi edinmek için.
* Bkz: [performans önerileri](sql-database-advisor.md) genel bakış, Azure SQL veritabanı performans için.
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) performans etkisini en sık kullandığınız sorguların görüntüleme hakkında bilgi edinmek için.
