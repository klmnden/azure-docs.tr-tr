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
ms.openlocfilehash: 929a9b74e5ef6eb492f50051b8d9c64bf698eee0
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39523810"
---
# <a name="enable-automatic-tuning"></a>Otomatik ayarlamayı etkinleştirme

Azure SQL veritabanı, sürekli olarak izler, sorgularınızı ve İş yükünüzün performansını gerçekleştirdiğiniz eylemi tanımlayan bir otomatik olarak yönetilen veri hizmetidir. Yapabilir önerileri gözden geçirin ve el ile uygulayabilirsiniz veya Azure SQL veritabanı, otomatik düzeltici eylemler uygulayın sağlar - bu olarak bilinir **otomatik ayarlama modu**. Otomatik ayarlama sunucu veya veritabanı düzeyinde etkinleştirilebilir.

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
[Daha fazla bilgi için buraya tıklayın nasıl etkinleştirileceği REST API aracılığıyla sunucu düzeyinde otomatik ayarlama](https://docs.microsoft.com/rest/api/sql/serverautomatictuning)

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
[Daha fazla bilgi için buraya tıklayın nasıl etkinleştirileceği REST API aracılığıyla tek bir veritabanı otomatik ayarlama](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning)

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

## <a name="disabled-by-the-system"></a>Sistem tarafından devre dışı bırakıldı
Otomatik ayarlama, veritabanı üzerinde gereken tüm eylemleri izleme ve bazı durumlarda, otomatik ayarlama düzgün veritabanı üzerinde çalışamaz olduğunu belirleyebilirsiniz. Bu durumda ayarlama seçeneği sistem tarafından devre dışı bırakılır. Çoğu durumda, Query Store etkin değil veya belirli bir veritabanı salt okunur durumda olduğundan bu gerçekleşir.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Otomatik ayarlama e-posta bildirimlerini yapılandırma

Bkz: [otomatik e-posta bildirimlerini ayarlama](sql-database-automatic-tuning-email-notifications.md)

## <a name="next-steps"></a>Sonraki adımlar
* Okuma [otomatik ayarlama makale](sql-database-automatic-tuning.md) nasıl performansınızı geliştirmenize yardımcı olabileceğini ve otomatik ayarlama hakkında daha fazla bilgi edinmek için.
* Bkz: [performans önerileri](sql-database-advisor.md) genel bakış, Azure SQL veritabanı performans için.
* Bkz: [sorgu performansı öngörüleri](sql-database-query-performance.md) performans etkisini en sık kullandığınız sorguların görüntüleme hakkında bilgi edinmek için.
