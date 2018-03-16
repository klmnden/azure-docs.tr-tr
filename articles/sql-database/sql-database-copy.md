---
title: "Bir Azure SQL veritabanını kopyalama | Microsoft Docs"
description: "Aynı sunucu veya farklı bir sunucu üzerinde var olan bir Azure SQL veritabanı işlemsel olarak tutarlı kopyasını oluşturun."
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: load & move data
ms.date: 06/15/2017
ms.author: carlrab
ms.topic: article
ms.openlocfilehash: c4a3836cfd0bbbb8d26a42af2980deab5f9d7681
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="copy-an-azure-sql-database"></a>Bir Azure SQL veritabanını kopyalama

Azure SQL veritabanını aynı sunucuya veya farklı bir sunucu üzerinde var olan bir Azure SQL veritabanı işlemsel olarak tutarlı bir kopyasını oluşturmak için çeşitli yöntemler sağlar. Azure portal, PowerShell veya T-SQL kullanarak bir SQL veritabanı kopyalayabilirsiniz. 

## <a name="overview"></a>Genel Bakış

Bir veritabanı kopyası kopya isteğini zaman itibariyle kaynak veritabanı anlık görüntüsüdür. Aynı sunucuya veya farklı bir sunucu, kendi hizmet katmanını ve performans düzeyi ya da aynı hizmet Katmanı (edition) içindeki farklı performans düzeyi seçebilirsiniz. Kopyalama tamamlandıktan sonra tam olarak işlevsel, bağımsız bir veritabanı haline gelir. Bu noktada, yükseltme veya herhangi bir sürümünü düşürmek. Oturumlar, kullanıcılar ve izinler bağımsız olarak yönetilebilir.  

## <a name="logins-in-the-database-copy"></a>Veritabanı kopyasını oturumlar

Bir veritabanı aynı mantıksal sunucusuna kopyaladığınızda, aynı oturum açma bilgileri hem veritabanlarında kullanılabilir. Güvenlik sorumlusu veritabanının kopyalanacak kullandığınız yeni bir veritabanı üzerinde veritabanı sahibi olur. Tüm veritabanı kullanıcıları, izinlerini ve bunların güvenlik tanımlayıcılarını (SID'ler) veritabanı kopyası kopyalanır.  

Farklı bir mantıksal sunucu için bir veritabanı kopyaladığınızda, güvenlik sorumlusu yeni sunucuda yeni bir veritabanı üzerinde veritabanı sahibi olur. Kullanırsanız [bulunan veritabanı kullanıcıları](sql-database-manage-logins.md) kopyalama tamamlandıktan sonra hemen, aynı kimlik bilgileriyle erişebilmesi için veri erişimi için birincil ve ikincil veritabanları her zaman aynı kullanıcı kimlik bilgilerini olduğundan emin olun . 

Kullanırsanız [Azure Active Directory](../active-directory/active-directory-whatis.md), tamamen kopyalama kimlik bilgilerini yönetme ihtiyacını ortadan kaldırabilirsiniz. Yeni bir sunucuya veritabanı kopyaladığınızda, oturum açma bilgileri yeni sunucuda bulunmadığı için ancak, oturum açma tabanlı erişim, çalışmayabilir. Farklı bir mantıksal sunucu için bir veritabanı kopyaladığınızda oturumları yönetme hakkında bilgi edinmek için [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenlik yönetme](sql-database-geo-replication-security-config.md). 

Kopyalama başarılı olduktan sonra ve diğer kullanıcıların eşleştirilir önce veritabanı sahibi kopyalama, başlatılan oturum açma için yeni veritabanı oturum açabilir. Kopyalama işlemi tamamlandıktan sonra oturum açmalar çözümlemek için bkz: [gidermek oturumları](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Azure portalı kullanarak bir veritabanını kopyalama

Azure portalını kullanarak bir veritabanı kopyalamak için veritabanınızın sayfasını açın ve ardından **kopya**. 

   ![Veritabanı kopyalama](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>PowerShell kullanarak bir veritabanını kopyalama

PowerShell kullanarak bir veritabanı kopyalamak için kullanın [yeni AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet'i. 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Bir tam örnek betik için bkz: [bir veritabanını yeni bir sunucuya kopyalama](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Transact-SQL kullanarak bir veritabanını kopyalama

Ana veritabanı sunucu düzeyinde asıl oturum açma veya kopyalamak istediğiniz veritabanı oluşturulan oturum açma ile oturum açın. Veritabanı başarılı kopyalamak için sunucu düzeyinde sorumlu olmayan oturumlar dbmanager rolünün üyeleri olmalıdır. Oturum açmalar ve sunucuya bağlanma hakkında daha fazla bilgi için bkz: [oturumları yönetme](sql-database-manage-logins.md).

Kaynak veritabanı ile kopyalama işlemini başlatmak [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) deyimi. Bu deyimi yürütme işlemi kopyalama veritabanı başlatır. Veritabanı kopyalama zaman uyumsuz bir işlem olduğundan, veritabanı kopyalama tamamlanmadan önce CREATE DATABASE deyimi döndürür.

### <a name="copy-a-sql-database-to-the-same-server"></a>Aynı sunucuya bir SQL veritabanını kopyalama
Ana veritabanı sunucu düzeyinde asıl oturum açma veya kopyalamak istediğiniz veritabanı oluşturulan oturum açma ile oturum açın. Veritabanı başarılı kopyalamak için sunucu düzeyinde sorumlu olmayan oturumlar dbmanager rolünün üyeleri olmalıdır.

Bu komut Database1 aynı sunucu üzerinde Database2 adlı yeni bir veritabanına kopyalar. Veritabanı boyutuna bağlı olarak, kopyalama işleminin tamamlanması biraz zaman alabilir.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Farklı bir sunucuya bir SQL veritabanını kopyalama

Oluşturulacak yeni veritabanının bulunduğu SQL veritabanı sunucusu hedef sunucunun ana veritabanı için oturum açın. Kaynak veritabanı kaynak SQL veritabanı sunucusunda veritabanı sahibi olarak aynı adı ve parolanın sahip bir oturum açma kullanın. Hedef sunucuda oturum açma de dbmanager rolünün bir üyesi olmanız veya sunucu düzeyinde asıl oturum açma olması gerekir.

Bu komut Database1 Sunucu1 ' Database2 Sunucu2'adlı yeni bir veritabanı kopyalar. Veritabanı boyutuna bağlı olarak, kopyalama işleminin tamamlanması biraz zaman alabilir.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-the-progress-of-the-copying-operation"></a>Kopyalama işleminin ilerleyişini izleyin

Kopyalama işlemi sys.databases ve sys.dm_database_copies görünümleri sorgulayarak izleyin. Kopyalama, sürerken **state_desc** yeni veritabanı için sys.databases görünümünün sütunu olarak ayarlanmış **kopyalama**.

* Kopyalama başarısız olursa, **state_desc** yeni veritabanı için sys.databases görünümünün sütunu olarak ayarlanmış **ŞÜPHELENİYORSANIZ**. Yeni veritabanında DROP deyimi yürütün ve daha sonra yeniden deneyin.
* Kopyalama işlemi başarılı olursa, **state_desc** yeni veritabanı için sys.databases görünümünün sütunu olarak ayarlanmış **çevrimiçi**. Kopyalama tamamlandıktan ve yeni veritabanı kaynak veritabanı bağımsız olarak değiştirilebilir normal bir veritabanıdır.

> [!NOTE]
> İşlemi devam ederken kopyalama iptal etmeye karar verirseniz, yürütme [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) deyimi yeni bir veritabanı üzerinde. Alternatif olarak, kaynak veritabanında DROP DATABASE deyimi yürütme da kopyalama işlemini iptal eder.
> 

## <a name="resolve-logins"></a>Oturum açma bilgileri çözümleyin

Yeni veritabanı hedef sunucuda çevrimiçi olduktan sonra kullanmak [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) kullanıcılara yeni veritabanından hedef sunucuda oturum açmalar yeniden eşlemek için deyimi. Sahipsiz kullanıcıların çözümlemek için bkz: [sahipsiz kullanıcıların sorun giderme](https://msdn.microsoft.com/library/ms175475.aspx). Ayrıca bkz. [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenlik yönetme](sql-database-geo-replication-security-config.md).

Yeni veritabanındaki tüm kullanıcıların kaynak veritabanında sahip oldukları izinleri korur. Veritabanı kopyasını başlatan kullanıcının yeni veritabanının veritabanı sahibi olur ve yeni bir güvenlik tanımlayıcısı (SID) atanır. Kopyalama başarılı olduktan sonra ve diğer kullanıcıların eşleştirilir önce veritabanı sahibi kopyalama, başlatılan oturum açma için yeni veritabanı oturum açabilir.

Bir veritabanı farklı bir mantıksal sunucusuna kopyaladığınızda, kullanıcılar ve oturumları yönetme hakkında bilgi edinmek için [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenlik yönetme](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Sonraki adımlar

* Oturum açma bilgileri hakkında daha fazla bilgi için bkz: [oturumları yönetme](sql-database-manage-logins.md) ve [olağanüstü durum kurtarma işleminden sonra Azure SQL veritabanı güvenlik yönetme](sql-database-geo-replication-security-config.md).
* Bir veritabanı dışarı aktarmak için bkz: [veritabanı için bir BACPAC dışarı](sql-database-export.md).
