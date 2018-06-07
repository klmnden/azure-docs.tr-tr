---
title: Azure SQL Database güvenliği olağanüstü durum kurtarma için yapılandırma | Microsoft Docs
description: Yapılandırma ve veritabanı geri yükleme veya ikincil bir sunucu için bir yük devretme sonrasında güvenlik yönetmeye yönelik güvenlik konuları hakkında bilgi edinin.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sashan
ms.openlocfilehash: 0796e5900bc67d93e51a0ce377ef5d1144346e2c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646873"
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Yapılandırma ve coğrafi geri yükleme veya yük devretme için Azure SQL veritabanı güvenlik yönetme 

Bu konu, yapılandırmak ve denetlemek için kimlik doğrulama gereksinimleri açıklanır. [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md) ve kullanıcı erişimini ikincil veritabanını ayarlamak için gerekli adımlar. Ayrıca kullandıktan sonra kurtarılmış veritabanını erişimi sağlamak nasıl açıklanır [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore). Kurtarma seçenekleri hakkında daha fazla bilgi için bkz: [iş Sürekliliğine genel bakış](sql-database-business-continuity.md).

> [!NOTE]
> [Aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md) tüm hizmet katmanlarında tüm veritabanları için kullanıma sunulmuştur.
>  

## <a name="disaster-recovery-with-contained-users"></a>Kapsanan kullanıcılar ile olağanüstü durum kurtarma
Oturum açma bilgileri ana veritabanında eşlenmelidir, geleneksel kullanıcılar içerilen kullanıcı tamamen veritabanı tarafından yönetilir. Bu iki faydası vardır. Olağanüstü durum kurtarma senaryosunda, kullanıcıların yeni birincil veritabanına bağlanmak devam edebilir veya veritabanı kullanıcıları yönetir olduğundan veritabanı coğrafi geri yükleme herhangi bir ek yapılandırma olmadan kullanılarak kurtarılır. Ayrıca vardır olası ölçeklenebilirlik ve performans avantajı oturum açma açısından bu yapılandırmasından. Daha fazla bilgi için bkz. [Bağımsız Veritabanı Kullanıcıları - Veritabanınızı Taşınabilir Hale Getirme](https://msdn.microsoft.com/library/ff929188.aspx). 

Ana dengelemeyi ölçekte olağanüstü durum kurtarma işlemini yönetme daha zor olmasıdır. Aynı oturum açma kullanan birden çok veritabanı varsa, birden çok veritabanlarında kapsanan kullanıcılar kullanarak kimlik bilgilerini koruma kapsanan kullanıcılar yararları negate. Örneğin, parola döndürme İlkesi değişiklikleri birden çok veritabanlarında tutarlı bir şekilde yapılması gerektirir ve ana veritabanındaki bir kez oturum açma parolasını değiştirme yerine. Aynı kullanıcı adı ve parolası kullan birden çok veritabanı varsa, bu nedenle, kapsanan kullanıcılar kullanarak önerilmez. 

## <a name="how-to-configure-logins-and-users"></a>Oturum açmalar ve kullanıcıları nasıl yapılandırılır?
Oturumlar ve kullanıcılar kullanıyorsanız (kapsanan kullanıcılar yerine), aynı giriş ana veritabanında mevcut olduğunu güvence altına almaya ek adımlar uygulamanız gerekir. Aşağıdaki bölümlerde adımları söz konusu ve ek ilgili önemli noktalar verilmiştir.

### <a name="set-up-user-access-to-a-secondary-or-recovered-database"></a>Bir ikincil veya kurtarılan veritabanı için kullanıcı erişimi ayarlama
İkincil veritabanı için salt okunur bir ikincil veritabanı olarak kullanılabilmesi ve yeni birincil veritabanı veya coğrafi geri yükleme kullanarak kurtarılan veritabanı için uygun erişim sağlamak için hedef sunucunun ana veritabanı uygun güvenlik yapılandırması kurtarma önce yerinde olması gerekir.

Özel izinler her adımı için bu konunun ilerleyen bölümlerinde açıklanmıştır.

Coğrafi çoğaltma İkincil kullanıcı erişimini hazırlama coğrafi çoğaltma yapılandırma bir parçası olarak gerçekleştirilmelidir. Kullanıcı veritabanlarına erişim izni coğrafi geri hazırlama özgün sunucunun olduğunda çevrimiçi herhangi bir zamanda gerçekleştirilmelidir (örneğin bir parçası olarak DR detayı).

> [!NOTE]
> Yük devretme veya coğrafi geri yükleme düzgün yapılandırılmış oturum açmalar, erişimi olmayan bir sunucuya server yönetici hesabı'na sınırlı olur.
> 
> 

Hedef sunucudaki oturumları ayarlama aşağıda açıklanan üç adımdan oluşur:

#### <a name="1-determine-logins-with-access-to-the-primary-database"></a>1. Oturum açma bilgileri birincil veritabanına erişimi olan belirleyin:
İşleminin ilk adımı hedef sunucuda hangi oturumların çoğaltılmalıdır belirlemektir. Bu, bir SELECT deyimi, kaynak sunucu üzerindeki mantıksal asıl veritabanı birinde ve birincil veritabanında bir çiftinden ile gerçekleştirilir.

Yalnızca sunucu yöneticisi veya bir üyesi **LoginManager** sunucu rolü, aşağıdaki SELECT deyimiyle kaynak sunucuda oturum açma bilgileri belirleyebilir. 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

Yalnızca bir üye db_owner veritabanı rolü, dbo kullanıcısı veya Sunucu Yöneticisi, tüm birincil veritabanında veritabanı kullanıcısı Sorumlular belirleyebilirsiniz.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-the-sid-for-the-logins-identified-in-step-1"></a>2. 1. adımda tanımlanan oturumlar için SID bulun:
Önceki bölümdeki sorguların çıktısını karşılaştırma ve SID'ler eşleşen sunucu oturum açma veritabanı kullanıcısı ile eşleyebilirsiniz. Eşleşen SID'e sahip bir veritabanı kullanıcısı sahip oturumların asıl veritabanı kullanıcı kullanıcı erişimi o veritabanına sahip. 

Aşağıdaki sorgu tüm kullanıcı ilkeleri ve bir veritabanında SID'leri görmek için kullanılabilir. Yalnızca db_owner veritabanı rol veya Sunucu Yöneticisi bir üyesi bu sorguyu çalıştırabilir.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> **INFORMATION_SCHEMA** ve **sys** kullanıcınız *NULL* SID'leri ve **Konuk** SID **0x00**. **Dbo** SID ile başlayabilir *0x01060000000001648000000000048454*, veritabanı oluşturucusu üyesi yerine sunucu yöneticisi ise **DbManager**.
> 
> 

#### <a name="3-create-the-logins-on-the-target-server"></a>3. Hedef sunucuda oturum oluşturma:
Hedef sunucu veya sunucuları gidin ve uygun SID'lerle oturumları oluşturmak için son adım olacaktır. Temel söz dizimi aşağıdaki gibidir.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> İkincil, ancak birincil kullanıcı erişimi vermek istiyorsanız, aşağıdaki sözdizimini kullanarak birincil sunucu üzerindeki kullanıcı oturum açma değiştirerek bunu yapabilirsiniz.
> 
> ALTER OTURUM AÇMA <login name> DEVRE DIŞI BIRAK
> 
> Her zaman gerekirse etkinleştirebilirsiniz şekilde devre dışı bırakma parola değişmez.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Veritabanı erişimi ve oturumları yönetme ile ilgili daha fazla bilgi için bkz: [SQL veritabanı güvenlik: veritabanı erişimi ve oturum açma güvenlik yönetmek](sql-database-manage-logins.md).
* Kapsanan veritabanı kullanıcıları hakkında daha fazla bilgi için bkz: [bulunan veritabanı kullanıcıları - yapmadan bilgisayarınızı veritabanı taşınabilir](https://msdn.microsoft.com/library/ff929188.aspx).
* Aktif coğrafi çoğaltma yapılandırma ve kullanma hakkında daha fazla bilgi için bkz: [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md)
* Coğrafi geri yükleme hakkında daha fazla bilgi için bkz: [coğrafi geri yükleme](sql-database-recovery-using-backups.md#geo-restore)

