---
title: Yönetme ve izleme Azure Backup tarafından yedeklenen bir Azure VM'de SQL Server veritabanlarını | Microsoft Docs
description: Bu makalede, Azure Backup tarafından yedeklenir ve bir Azure sanal makinesinde çalışan SQL Server veritabanlarını geri yükleme.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: da4264047830b21b3ac4dae723dd1fd2f9d7a8f4
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57432864"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Yönetme ve İzleme SQL Server veritabanlarını desteklenir 


Tarafından kasası yönetme ve izleme, yedeklenir bir Azure Backup kurtarma Hizmetleri ve bir Azure sanal makine (VM) üzerinde çalışan SQL Server veritabanları için bu makalede, ortak görevler açıklanmaktadır. [Azure Backup](backup-overview.md) hizmeti. İşlerini ve Uyarıları izleme, durdurun ve veritabanını korumayı sürdürmek, yedekleme işlerinin çalıştırılması ve bir sanal makine yedeklemelerinden kaydını öğreneceksiniz.


> [!NOTE]
> Azure Backup ile bir Azure sanal makinesinde çalışan SQL Server veritabanlarını yedekleme şu anda genel Önizleme aşamasındadır.


SQL Server veritabanlarınız için yedeklemeler henüz yapılandırmadıysanız, bkz. [Azure vm'lerde SQL Server veritabanlarını yedekleme](backup-azure-sql-database.md)

##  <a name="monitor-manual-backup-jobs-in-the-portal"></a>Portalında el ile yedekleme işlerini izleme

Azure yedekleme el ile tetiklenen tüm işleri gösteren **yedekleme işleri** portalı. Bkz. Bu portal INCLUDE veritabanı bulma ve kaydetme ve yedekleme ve geri yükleme işlemlerini işler.

![Yedekleme işleri portalı](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> **Yedekleme işleri** portalı zamanlanan yedekleme işlerinin Göster değil. Sonraki bölümde açıklandığı gibi zamanlanmış yedekleme işleri izlemek için SQL Server Management Studio'yu kullanın.
>

## <a name="monitor-scheduled-backup-jobs-in-sql-server-management-studio"></a>SQL Server Management Studio'da zamanlanmış yedekleme işlerini izleme 

Azure Backup, SQL yerel API'lerin tüm yedekleme işlemleri için kullanır. Tüm iş bilgileri getirmek için yerel API kullanan [SQL yedek kümesi tablo](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) msdb veritabanında.

Aşağıdaki örnekte adlı bir veritabanı için tüm yedekleme işleri getiren bir sorgudur **DB1**. Gelişmiş izleme sorgu özelleştirin.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

## <a name="view-backup-alerts"></a>Yedekleme Uyarıları görüntüle

15 dakikada bir günlüğü yedeklerini meydana geldiği için yedekleme işlerini izleme can sıkıcı olabilir. Azure Backup e-posta uyarıları göndererek izleme kolaylaştırır. E-posta uyarıları şunlardır:

- Tüm yedekleme hataları için tetiklenir.
- Veritabanı düzeyinde hata koduna göre birleştirilir.
- Yalnızca bir veritabanının ilk yedekleme hatası için gönderilir. 

Veritabanı yedekleme uyarıları izlemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Kasa panosunda seçin **uyarıları ve olayları**.

   ![Uyarıları ve olayları seçin](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

1. İçinde **uyarıları ve olayları**seçin **yedekleme uyarıları**.

   ![Yedekleme uyarıları seçin](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>SQL Server veritabanı için korumayı Durdur

Çeşitli şekillerde SQL Server veritabanında yedekleme durdurabilirsiniz:

* Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silin.
* Gelecek tarihli tüm yedekleme işlerini durdurma ve kurtarma noktalarını olduğu gibi bırakır.

Kurtarma noktalarını bırakmak isterseniz bu ayrıntıları göz önünde bulundurun:

* Yedekleme ilkesine göre tüm kurtarma noktalarını çıkmadan temizlenir. 
* Tüm kurtarma noktalarını temizlenir kadar korumalı örnek ve tüketilen depolama alanı için ücret ödersiniz. Daha fazla bilgi için [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).
* Yedekleme verilerini silene kadar azure yedekleme, bir son kurtarma noktası her zaman korur. 
* Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur. 
* Veritabanınız için autoprotection etkinleştirilirse, autoprotection devre dışı bırakılmadığı sürece yedeklemeleri durdurulamıyor.

Bir veritabanı için korumayı durdurmak için:

1. Kasa panosunda altında **kullanım**seçin **yedekleme öğeleri**.

1. Altında **Yedekleme Yönetimi türü**seçin **Azure VM'deki SQL**.

    ![Azure VM'de SQL seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)


1. Korumayı durdurmak istediğiniz veritabanını seçin.

    ![Korumayı durdurmak için veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)


1. Veritabanı menüsünde **yedeklemeyi Durdur**.

    ![Yedeklemeyi Durdur seçin](./media/backup-azure-sql-database/stop-db-button.png)


1. Üzerinde **yedeklemeyi Durdur** menüsünde korumak veya veri silmeyi seçin. İsterseniz, nedeni ve açıklama sağlayın.

    ![Tutmak mı yedeklemeyi Durdur menüsünde verilerini sil](./media/backup-azure-sql-database/stop-backup-button.png)

1. Seçin **yedeklemeyi Durdur**.

  

## <a name="resume-protection-for-a-sql-database"></a>SQL veritabanı korumasını sürdürme

Ne zaman durdurmanız SQL veritabanı için korumayı seçerseniz **yedekleme verilerini koru** seçeneği, daha sonra korumayı devam edebilirsiniz. Yedekleme verileri korumaz, koruma sürdüremezsiniz.

Bir SQL veritabanı için korumayı sürdürmek için:

1. Yedekleme öğesi açın ve seçin **yedeklemeyi Sürdür**.

    ![Veritabanı korumayı sürdürmek için yedeklemeyi Sürdür eylemi seçin](./media/backup-azure-sql-database/resume-backup-button.png)

2. Üzerinde **yedekleme İlkesi** menüsünde, bir ilkeyi seçin ve ardından **Kaydet**.

## <a name="run-an-on-demand-backup"></a>Bir talep üzerine yedekleme gerçekleştirin

Farklı türde isteğe bağlı yedeklemeler çalıştırabilirsiniz:

* Tam yedekleme
* Yalnızca kopya tam yedekleme
* Değişiklik yedeği
* Günlük yedekleme

Daha fazla bilgi için [SQL Server Yedekleme türleri](backup-architecture.md#sql-server-backup-types).

## <a name="unregister-a-sql-server-instance"></a>SQL Server örneği kaydı

Bir SQL Server örneği koruma devre dışı bıraktıktan sonra ancak kasayı silmeden önce kaydı:

1. Kasa panosunda altında **Yönet**seçin **Yedekleme Altyapısı**.  

   ![Yedekleme Altyapısı'nı seçin](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. Altında **yönetim sunucuları**seçin **korumalı sunucuların**.

   ![Korumalı sunucuları seçin](./media/backup-azure-sql-database/protected-servers.png)


3. İçinde **korumalı sunucuların**, kaydını kaldırmak için sunucuyu seçin. Kasayı silmek için tüm sunucuların kaydını silmeniz gerekir.

4. Korumalı sunucuya sağ tıklayın ve seçin **Sil**.

   ![Sil'i seçin](./media/backup-azure-sql-database/delete-protected-server.png)


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [bir SQL Server veritabanında yedekleme sorunlarını giderme](backup-sql-server-azure-troubleshoot.md).
