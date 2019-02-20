---
title: Yönetme ve izleme Azure Backup tarafından yedeklenen bir Azure VM'de SQL Server veritabanlarını | Microsoft Docs
description: Bu makalede Azure Backup ile desteklenen bir Azure sanal makinesinde çalışan SQL Server veritabanlarını geri yükleme
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 1c2ce0ba42f0bc3efd1dcc951113b05ab6941b98
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430804"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Yönetme ve İzleme SQL Server veritabanlarını desteklenir 


Tarafından yönetmek ve izlemek için bir Azure Backup kurtarma Hizmetleri yedeklenen bir Azure sanal makinesinde çalışan SQL Server veritabanlarını kasa için bu makalede, ortak görevler açıklanmaktadır. [Azure Backup](backup-overview.md) hizmeti. Görevler, işlerini ve Uyarıları izleme, durdurma ve veritabanı koruma sürdürülüyor, yedekleme işleri çalıştırma ve yedekleme VM'den kaydını da dahil olmak üzere.


> [!NOTE]
> Azure Backup ile bir Azure sanal makinesinde çalışan SQl Server veritabanlarının yedekleme şu anda genel Önizleme aşamasındadır.


' Ndaki yönergeleri izleyin, SQL Server veritabanları için yapılandırılmış bir yedekleme henüz yapmadıysanız TIF [bu makalede](backup-azure-sql-database.md)

## <a name="monitor-backup-jobs"></a>Yedekleme işleri İzle

###  <a name="monitor-ad-hoc-jobs-in-the-portal"></a>Portalda geçici işlerini izleme

Azure yedekleme el ile tetiklenen tüm işleri gösteren **yedekleme işleri** Portal'da, bulma ve veritabanları ve yedekleme ve geri yükleme işlemleri kaydediliyor.

![Yedekleme işleri portalı](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Zamanlanan yedekleme işlerinin olmayan görünen **yedekleme işleri** portalı. Sonraki bölümde açıklandığı gibi zamanlanmış yedekleme işleri izlemek için SQL Server Management Studio'yu kullanın.
>

### <a name="monitor-backup-jobs-with-sql-server-management-studio"></a>SQL Server Management Studio ile yedekleme işleri İzle 

Azure Backup, SQL yerel API'lerin tüm yedekleme işlemleri için kullanır.

Tüm iş bilgileri getirmek için yerel API kullanan [SQL yedek kümesi tablo](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) msdb veritabanında.

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

15 dakikada bir günlüğü yedeklerini meydana geldiği için yedekleme işlerini izleme can sıkıcı olabilir. Azure yedekleme ile e-posta uyarıları izleme kolaylaştırır.

- Tüm yedekleme hataları için uyarıları tetiklenir.
- Uyarılar veritabanı düzeyinde hata koduna göre birleştirilir.
- Yalnızca ilk yedekleme hatası için bir veritabanı için bir e-posta uyarı gönderilir. 

Yedekleme uyarıları izlemek için:

1. Azure aboneliğinizde oturum açın [Azure portalında](https://portal.azure.com) veritabanı uyarıları izlemek için.

2. Kasa panosunda seçin **uyarıları ve olayları**.

   ![Uyarıları ve olayları seçin](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. İçinde **uyarıları ve olayları**seçin **yedekleme uyarıları**.

   ![Yedekleme uyarıları seçin](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>SQL Server veritabanı için korumayı Durdur

Çeşitli şekillerde SQL Server veritabanında yedekleme durdurabilirsiniz:

* Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silin.
* Gelecek tarihli tüm yedekleme işlerini durdurma ama kurtarma noktaları olduğu gibi bırakır.

Şunlara dikkat edin:

Kurtarma noktalarını değiştirmeden bırakırsanız, noktaları yedekleme ilkesine uygun olarak temizlenir. Tüm kurtarma noktalarını temizlenir kadar korumalı örnek ve tüketilen depolama alanı için ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/backup/) fiyatlandırma hakkında daha fazla.
- Süreleri dolduğunda bekletme ilkesi uyarınca olsa da, Kurtarma noktaları olduğu gibi bırakın, açıkça yedekleme verilerini silene kadar Azure yedekleme her zaman bir son kurtarma noktasını korur.
- Durdurma yedekleme olmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olmaya başlar. Yine, eski kurtarma noktalarını ilkesine göre dolacak, ancak yedeklemeyi Durdur veriler silinene kadar her zaman bir son kurtarma noktası korunur.
- Otomatik korumayı devre dışı kadar otomatik koruma için etkin bir veritabanı için yedekleme durdurulamıyor.

Bir veritabanı için korumayı durdurmak için:

1. Kasa panosunda altında **kullanım**seçin **yedekleme öğeleri**.

    ![Yedekleme öğeleri menüsünü açın](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

2. İçinde **Yedekleme Yönetimi türü**seçin **Azure VM'deki SQL**.

    ![Azure VM'de SQL seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)


3. Korumayı durdurmak istediğiniz veritabanını seçin.

    ![Korumayı durdurmak için veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)


5. Veritabanı menüsünde **yedeklemeyi Durdur**.

    ![Yedeklemeyi Durdur seçin](./media/backup-azure-sql-database/stop-db-button.png)


6. İçinde **yedeklemeyi Durdur** menüsünde korumak veya veri silmeyi seçin. İsteğe bağlı olarak, nedeni ve açıklama belirtin.

    ![Yedekleme menüsündeki Durdur](./media/backup-azure-sql-database/stop-backup-button.png)

7. Tıklayın **yedeklemeyi Durdur** .

  

### <a name="resume-protection-for-a-sql-database"></a>SQL veritabanı korumasını sürdürme

Varsa **yedekleme verilerini koru** seçeneği seçildiğinde SQL veritabanı için korumayı durdurduğunuzda, koruma devam edebilir. Koruma, yedekleme verileri korunur değildi, sürdüremezsiniz.

1. SQL veritabanı için korumayı sürdürmek için yedekleme öğesi açın ve seçin **yedeklemeyi Sürdür**.

    ![Veritabanı korumayı sürdürmek için yedeklemeyi Sürdür eylemi seçin](./media/backup-azure-sql-database/resume-backup-button.png)

2. Üzerinde **yedekleme İlkesi** menüsünde, bir ilkeyi seçin ve ardından **Kaydet**.

## <a name="run-an-on-demand-backup"></a>Bir talep üzerine yedekleme gerçekleştirin

Farklı türde isteğe bağlı yedeklemeler çalıştırabilirsiniz:

* Tam yedekleme
* Yalnızca kopya tam yedekleme
* Değişiklik yedeği
* Günlük yedekleme

[Daha fazla bilgi edinin](backup-architecture.md#sql-server-backup-types) SQL Server Yedekleme türleri.

## <a name="unregister-a-sql-server-instance"></a>SQL Server örneği kaydı

Bir SQL Server örneği, sonra koruma devre dışı bırakıldı, ancak kasayı silmeden önce kaydı:

1. Kasa panosunda altında **Yönet**seçin **Yedekleme Altyapısı**.  

   ![Yedekleme Altyapısı'nı seçin](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. Altında **yönetim sunucuları**seçin **korumalı sunucuların**.

   ![Korumalı sunucuları seçin](./media/backup-azure-sql-database/protected-servers.png)


3. İçinde **korumalı sunucuların**, kaydını kaldırmak için sunucuyu seçin. Kasayı silmek için tüm sunucuların kaydını silmeniz gerekir.

4. Korumalı sunucuya sağ tıklayın > **Sil**.

   ![Sil'i seçin](./media/backup-azure-sql-database/delete-protected-server.png)


## <a name="next-steps"></a>Sonraki adımlar

[Gözden geçirme](backup-sql-server-azure-troubleshoot.md) sorun giderme bilgileri için SQL Server veritabanı yedeği.
