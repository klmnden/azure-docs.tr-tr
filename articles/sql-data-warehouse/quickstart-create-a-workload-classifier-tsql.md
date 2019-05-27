---
title: 'Hızlı Başlangıç: Bir iş yükü sınıflandırıcı - T-SQL oluşturma | Microsoft Docs'
description: Bir iş yükü sınıflandırıcı yüksek önem derecesiyle oluşturmak için T-SQL kullanma
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: workload management
ms.date: 05/01/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 1c84bf84f8ba28a98937b02a463003a900aefaa0
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002905"
---
# <a name="quickstart-create-a-workload-classifier-using-t-sql"></a>Hızlı Başlangıç: T-SQL kullanarak bir iş yükü sınıflandırıcı oluşturma

Bu hızlı başlangıçta, CEO, kuruluşunuz için yüksek önem düzeyine sahip bir iş yükü sınıflandırıcı hızla oluşturacaksınız. Bu iş yükü sınıflandırıcı CEO sorguların diğer sorguları ile daha düşük önem sırasına göre önceliklidir izin verir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!NOTE]
> Bir SQL Veri Ambarı'nın oluşturulması ek hizmet ücretlerinin alınmasına neden olabilir.  Ayrıntılı bilgi için bkz. [SQL Veri Ambarı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>
>

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, zaten sahip olduğunuz bir SQL veri ambarı ve Denetim veritabanı izinlere sahip olduğunuzu varsayar. Gerekiyorsa **mySampleDataWarehouse** adlı bir veri ambarı oluşturmak için [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanabilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-login-for-theceo"></a>Oturum açma için TheCEO oluşturma

SQL Server kimlik doğrulaması oturum açma bilgisi oluşturma `master` kullanarak veritabanı [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql) 'TheCEO' için.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'TheCEO')
BEGIN
CREATE LOGIN [TheCEO] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Kullanıcı oluştur

[Kullanıcı oluşturma](/sql/t-sql/statements/create-user-transact-sql?view=azure-sqldw-latest), "TheCEO" mySampleDataWarehouse içinde

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'THECEO')
BEGIN
CREATE USER [TheCEO] FOR LOGIN [TheCEO]
END
;
```

## <a name="create-a-workload-classifier"></a>Bir iş yükü sınıflandırıcı oluşturma

Oluşturma bir [iş yükü sınıflandırıcı](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) yüksek önem düzeyine sahip "TheCEO" için.

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO];
CREATE WORKLOAD CLASSIFIER [wgcTheCEO]
WITH (WORKLOAD_GROUP = 'xlargerc'
      ,MEMBERNAME = 'TheCEO'
      ,IMPORTANCE = HIGH);
```

## <a name="view-existing-classifiers"></a>Mevcut sınıflandırıcılar görüntüleyin

```sql
SELECT * FROM sys.workload_management_workload_classifiers
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO]
DROP USER [TheCEO]
;
```

Veri ambarı birimleri ve veri Ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depoda tutmak istiyorsanız, veri ambarını kullanmadığınız zamanlarda işlemi duraklatabilirsiniz. Duraklatarak işlem, sizin yalnızca veri depolama için ücretlendirilirsiniz. Verilerle çalışmak hazır olduğunuzda, sürdürebilirsiniz.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

Kaynakları temizlemek için aşağıdaki adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com)üzerinde veri ambarı'nızı seçin.

    ![Kaynakları temizleme](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. İşlemi duraklatmak için işaretleyin **duraklatmak** düğmesi. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlemi sürdürmek için seçin **Başlat**.

3. Veri ambarı, işlem ve depolama için ücret ödemezsiniz üzere kaldırmak için işaretleyin **Sil**.

4. Oluşturduğunuz SQL sunucusunu kaldırmak için işaretleyin **mynewserver-20180430.database.windows.net** seçin ve önceki görüntüde **Sil**.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için işaretleyin **myResourceGroup**ve ardından **kaynak grubunu Sil**.

## <a name="next-steps"></a>Sonraki adımlar

- Bir iş yükü sınıflandırıcı oluşturdunuz. Birkaç sorgu gösterdikleri için TheCEO çalıştırın. Bkz: [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) sorgular ve atanan önem görüntülemek için.
- Azure SQL veri ambarı iş yükü yönetimi hakkında daha fazla bilgi için bkz: [iş yükü önem](sql-data-warehouse-workload-importance.md) ve [iş yükü sınıflandırma](sql-data-warehouse-workload-classification.md).
- Nasıl yapılır makalelerine bakın [iş yükü önem yapılandırma](sql-data-warehouse-how-to-configure-workload-importance.md) ve nasıl [yönetme ve izleme iş yükü yönetimi](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
