---
title: Olağanüstü durum kurtarma için Azure SQL veritabanı güvenliğini yapılandırma | Microsoft Docs
description: Yapılandırma ve veritabanı geri yükleme veya ikincil sunucuya Yük devretme sonrasında güvenlik yönetmeye yönelik güvenlik konuları hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 8a2a2fffa9ed3a4dae3c0768291b7585be4bfc6d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64690845"
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Yapılandırma ve Azure SQL veritabanı güvenliğini coğrafi geri yükleme ya da yük devretme için yönetme

Bu makalede yapılandırmak ve denetlemek için kimlik doğrulama gereksinimleri [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve [otomatik yük devretme grupları](sql-database-auto-failover-group.md). Ayrıca, ikincil veritabanı için kullanıcı erişimi ayarlamak için gerekli adımları sağlar. Son olarak, kullandıktan sonra kurtarılan veritabanına erişimi etkinleştirme konusunda da açıklar [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore). Kurtarma seçenekleri hakkında daha fazla bilgi için bkz. [iş Sürekliliğine genel bakış](sql-database-business-continuity.md).

## <a name="disaster-recovery-with-contained-users"></a>Bağımsız kullanıcılar ile olağanüstü durum kurtarma

Asıl veritabanında oturumlar eşlenmelidir, geleneksel kullanıcıları farklı olarak içerilen kullanıcı veritabanı tarafından tamamen yönetilir. Bunun iki avantajı vardır. Olağanüstü durum kurtarma senaryosunda kullanıcıları yeni birincil veritabanına bağlanmaya devam edebilir veya herhangi bir ek yapılandırma olmadan coğrafi geri yükleme kullanarak veritabanı veritabanı kullanıcıları yönettiğinden kurtarıldı. Var. Ayrıca olası ölçeklenebilirlik ve performans avantajlarından oturum açma açısından bu yapılandırma Daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](https://msdn.microsoft.com/library/ff929188.aspx).

Ana dengelemeyi uygun ölçekte olağanüstü durum kurtarma işlemini yönetmeyi daha zor olmasıdır. Aynı oturum açma bilgilerini kullanan birden çok veritabanına sahip olduğunda, bağımsız kullanıcılar birden çok veritabanını kullanarak kimlik bilgilerini koruma kapsanan kullanıcılar avantajlarını negate. Örneğin, parola dönüşü İlkesi değişiklikleri birden çok veritabanını tutarlı bir şekilde yapılmasını gerektirir. yerine ana veritabanındaki bir kez oturum açma parolası değiştiriliyor. Aynı kullanıcı adı ve parola birden çok veritabanı varsa, bu nedenle, kapsanan kullanıcılar kullanarak önerilmez.

## <a name="how-to-configure-logins-and-users"></a>Oturum açma bilgileri ve kullanıcılar nasıl yapılandırılır?

Oturumlar ve kullanıcılar kullanıyorsanız (bağımsız kullanıcılar yerine), aynı oturum açma bilgileri ana veritabanında mevcut emin olmak için ek adımlar uygulaması gerekir. Aşağıdaki bölümlerde adımları dahil olan ve ek konuları özetler.

  >[!NOTE]
  > Veritabanlarınızı yönetmek için Azure Active Directory (AAD) oturum açma bilgileri kullanmak da mümkündür. Daha fazla bilgi için [Azure SQL oturumları ve kullanıcıları](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a>İkincil ya kurtarılan veritabanına kullanıcı erişimini ayarlama

İkincil veritabanı salt okunur bir ikincil veritabanı olarak kullanılabilir ve yeni birincil veya coğrafi geri yükleme kullanarak kurtarılan veritabanının uygun erişim sağlamak için hedef sunucunun ana veritabanına uygun güvenlik olması gerekir Kurtarma önce yerinde yapılandırması.

Özel izinler her adım için bu konunun ilerleyen bölümlerinde açıklanmıştır.

Coğrafi çoğaltma İkincil kullanıcı erişimini hazırlama coğrafi çoğaltmayı yapılandırma kapsamında gerçekleştirilmesi gerekir. Coğrafi geri veritabanlarına erişim kullanıcı hazırlama, özgün sunucunun çevrimiçi herhangi bir zamanda gerçekleştirilmelidir (örneğin bir parçası olarak DR tatbikatı).

> [!NOTE]
> Yük devretme veya coğrafi geri yükleme düzgün bir şekilde yapılandırılmış oturum açma bilgileri, erişiminiz olmayan bir sunucuya sunucu yönetici hesabı için sınırlı olur.

Hedef sunucuda oturum açma bilgileri ayarlama, aşağıda açıklanan üç adımdan oluşur:

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a>1. Oturum açma bilgileri birincil veritabanına erişimi olan belirleme

İşlemin ilk adımı, hedef sunucuda hangi oturum açma bilgileri çoğaltılmalıdır belirlemektir. Bu, SELECT deyimleri, bir mantıksal asıl veritabanı kaynak sunucuda dosya ve biri birincil veritabanında bir çifti ile gerçekleştirilir.

Yalnızca sunucu yöneticisi veya üyesi **LoginManager** sunucu rolü, aşağıdaki SELECT deyimi ile kaynak sunucuda oturum açma bilgileri belirleyebilir.

    SELECT [name], [sid]
    FROM [sys].[sql_logins]
    WHERE [type_desc] = 'SQL_Login'

Yalnızca bir üye db_owner veritabanı rolü, dbo kullanıcısı veya Sunucu Yöneticisi, tüm birincil veritabanında veritabanı kullanıcı ilkeleri belirleyebilirsiniz.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a>2. 1. adımda tanımladığınız oturumlar için SID değerini bulma

Önceki bölümde sorgularından çıktısını karşılaştırma ve SID eşleşen veritabanı kullanıcısı için sunucu oturumu eşleyebilirsiniz. Bir veritabanı kullanıcısı eşleşen bir SID'ye sahip bir oturum açma bilgileri o veritabanına erişim kullanıcı asıl veritabanı kullanıcı sahip.

Aşağıdaki sorgu tüm kullanıcı asıl adları ve veritabanındaki SID'leri görmek için kullanılabilir. Db_owner veritabanı rol veya Sunucu Yöneticisi yalnızca bir üyesi bu sorguyu çalıştırabilir.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> **INFORMATION_SCHEMA** ve **sys** kullanıcınız *NULL* SID ve **Konuk** SID'si **0x00**. **Dbo** SID ile başlayabilir *0x01060000000001648000000000048454*, veritabanı oluşturan üyesi yerine sunucu yöneticisi olduysa **DbManager**.

#### <a name="3-create-the-logins-on-the-target-server"></a>3. Hedef sunucuda oturum oluşturma

Son adım, hedef sunucuya veya sunuculara gidin ve uygun SID ile oturum açma bilgileri oluşturmak sağlamaktır. Temel sözdizimi aşağıdaki gibidir.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> İkincil, ancak birincil kullanıcı erişimi vermek istiyorsanız, aşağıdaki sözdizimini kullanarak, kullanıcı oturum açma birincil sunucudaki değiştirerek bunu yapabilirsiniz.
>
> ```sql
> ALTER LOGIN <login name> DISABLE
> ```
>
> Her zaman, gerekirse verebilmeniz için devre dışı bırakma parola değiştirmez.

## <a name="next-steps"></a>Sonraki adımlar

* Veritabanı erişimi ve oturum açma bilgilerini yönetme ile ilgili daha fazla bilgi için bkz: [SQL veritabanı güvenliği: Veritabanı erişimi ve oturum açma güvenliğini yönetme](sql-database-manage-logins.md).
* Kapsanan veritabanı kullanıcıları hakkında daha fazla bilgi için bkz. [bağımsız veritabanı kullanıcıları - taşınabilir hale getirme veritabanı](https://msdn.microsoft.com/library/ff929188.aspx).
* Etkin coğrafi çoğaltma hakkında bilgi edinmek için [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md).
* Otomatik Yük devretme grupları hakkında daha fazla bilgi için bkz: [otomatik yük devretme grupları](sql-database-auto-failover-group.md).
* Coğrafi geri yükleme hakkında daha fazla bilgi için bkz: [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore)
