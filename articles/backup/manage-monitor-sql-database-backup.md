---
title: Yönetme ve izleme Azure Backup tarafından yedeklenen bir Azure VM'de SQL Server veritabanlarını | Microsoft Docs
description: Bu makalede, bir Azure sanal makinesinde çalışan SQL Server veritabanlarını izleme ve yönetme açıklar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: raynew
ms.openlocfilehash: ea5495867d5f453db014e000e01d533d049dc628
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59787740"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Yönetme ve İzleme SQL Server veritabanlarını desteklenir

Tarafından kasası yönetme ve izleme, yedeklenir bir Azure Backup kurtarma Hizmetleri ve bir Azure sanal makine (VM) üzerinde çalışan SQL Server veritabanları için bu makalede, ortak görevler açıklanmaktadır. [Azure Backup](backup-overview.md) hizmeti. İşlerini ve Uyarıları izleme, durdurun ve veritabanını korumayı sürdürmek, yedekleme işlerinin çalıştırılması ve bir sanal makine yedeklemelerinden kaydını öğreneceksiniz.

SQL Server veritabanlarınız için yedeklemeler henüz yapılandırmadıysanız, bkz. [Azure vm'lerde SQL Server veritabanlarını yedekleme](backup-azure-sql-database.md)

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Portalında el ile yedekleme işlerini izleme

Azure yedekleme el ile tetiklenen tüm işleri gösteren **yedekleme işleri** portalı. Bkz. Bu portal INCLUDE veritabanı bulma ve kaydetme ve yedekleme ve geri yükleme işlemlerini işler.

![Yedekleme işleri portalı](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> **Yedekleme işleri** portalı zamanlanan yedekleme işlerinin Göster değil. Sonraki bölümde açıklandığı gibi zamanlanmış yedekleme işleri izlemek için SQL Server Management Studio'yu kullanın.
>

İzleme senaryoları hakkında daha fazla bilgi için Git [Azure Portalı'nda izlemeyi](backup-azure-monitoring-built-in-monitor.md) ve [Azure İzleyicisi'ni kullanarak izleme](backup-azure-monitoring-use-azuremonitor.md).  


## <a name="view-backup-alerts"></a>Yedekleme Uyarıları görüntüle

15 dakikada bir günlüğü yedeklerini meydana geldiği için yedekleme işlerini izleme can sıkıcı olabilir. Azure Backup e-posta uyarıları göndererek izleme kolaylaştırır. E-posta uyarıları şunlardır:

- Tüm yedekleme hataları için tetiklenir.
- Veritabanı düzeyinde hata koduna göre birleştirilir.
- Yalnızca bir veritabanının ilk yedekleme hatası için gönderilir.

Veritabanı yedekleme uyarıları izlemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Kasa panosunda seçin **uyarıları ve olayları**.

   ![Uyarıları ve olayları seçin](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

3. İçinde **uyarıları ve olayları**seçin **yedekleme uyarıları**.

   ![Yedekleme uyarıları seçin](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>SQL Server veritabanı için korumayı Durdur

Çeşitli şekillerde SQL Server veritabanında yedekleme durdurabilirsiniz:

* Gelecek tarihli tüm yedekleme işlerini durdurma ve tüm kurtarma noktalarını silin.
* Gelecek tarihli tüm yedekleme işlerini durdurma ve kurtarma noktalarını olduğu gibi bırakır.

Kurtarma noktalarını bırakmak isterseniz bu ayrıntıları göz önünde bulundurun:

* Tüm kurtarma noktalarını sonsuza kadar değişmeden kalır, tüm ayıklama durağında durdurma verileri bekleterek korumayı.
* Korumalı örnek ve tüketilen depolama alanı için ücretlendirilirsiniz. Daha fazla bilgi için [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).
* Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur.

Bir veritabanı için korumayı durdurmak için:

1. Kasa panosunda seçin **yedekleme öğeleri**.

2. Altında **Yedekleme Yönetimi türü**seçin **Azure VM'deki SQL**.

    ![Azure VM'de SQL seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)

3. Korumayı durdurmak istediğiniz veritabanını seçin.

    ![Korumayı durdurmak için veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

4. Veritabanı menüsünde **yedeklemeyi Durdur**.

    ![Yedeklemeyi Durdur seçin](./media/backup-azure-sql-database/stop-db-button.png)


5. Üzerinde **yedeklemeyi Durdur** menüsünde korumak veya veri silmeyi seçin. İsterseniz, nedeni ve açıklama sağlayın.

    ![Tutmak mı yedeklemeyi Durdur menüsünde verilerini sil](./media/backup-azure-sql-database/stop-backup-button.png)

6. Seçin **yedeklemeyi Durdur**.


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

Yalnızca kopya tam yedekleme bekletme süresini belirtmek düzeltmeniz gerekirken, diğer yedekleme türleri için bekletme aralığı geçerli saatten 30 gün için otomatik olarak ayarlanır. <br/>
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

## <a name="re-register-extension-on-the-sql-server-vm"></a>SQL Server VM uzantısını yeniden kaydedin

Bazı durumlarda, iş yükü uzantısı VM'de nedenlerinden biri veya diğeri için etkilenen. Bu gibi durumlarda, sanal makinede tetikleyen tüm işlemler başarısız başlar. Ardından, VM uzantısını yeniden kaydetmeniz gerekebilir. **Yeniden kaydolmak** işlemi devam etmek işlemleri için VM üzerinde iş yükü yedekleme uzantısını yükler.  <br>

Bu seçeneği dikkatli önerilir; bir VM'de zaten sağlıklı bir uzantı ile tetiklendiğinde, bu işlem yeniden uzantı neden olur. Bu, başarısız tüm devam eden işler neden olabilir. Lütfen onay için bir veya daha fazla [belirtileri](backup-sql-server-azure-troubleshoot.md#symptoms) yeniden kayıt işlemini tetiklemeden önce.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [bir SQL Server veritabanında yedekleme sorunlarını giderme](backup-sql-server-azure-troubleshoot.md).
