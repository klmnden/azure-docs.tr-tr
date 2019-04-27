---
title: Azure VM'de yedeklenen SQL Server veritabanlarını geri yüklemek için Azure Yedekleme'yi | Microsoft Docs
description: Bu makalede, Azure Backup ile yedeklenir ve bir Azure sanal makinesinde çalışan SQL Server veritabanlarını geri yükleme.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: raynew
ms.openlocfilehash: 1712e46494796e563c26316b4f45d968872c304f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60781826"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Azure vm'lerde SQL Server veritabanlarını geri yükleme

Bu makalede bir Azure sanal makine (VM) üzerinde çalışan bir SQL Server veritabanını geri yükleme, [Azure Backup](backup-overview.md) hizmet yedeklenen bir Azure Backup kurtarma Hizmetleri kasasına.

Bu makalede, SQL Server veritabanlarını geri yüklemek açıklar. Daha fazla bilgi için [Azure vm'lerde SQL Server veritabanlarını yedekleme](backup-azure-sql-database.md).

## <a name="restore-to-a-time-or-a-recovery-point"></a>Bir saat veya bir kurtarma noktasından geri yükleme

Azure yedekleme gibi Azure Vm'lerinde çalışan SQL Server veritabanlarını geri yükleyebilirsiniz:

- Belirli bir tarih veya saat (saniye için) işlem günlüğü yedeklemeleri kullanarak geri yükleyin. Azure yedekleme, otomatik olarak uygun tam değişiklik yedeği belirler ve geri yüklemek için gerekli günlük yedekleme zincirini seçili zamana dayalı.
- Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya değişiklik yedeklemesinden geri yükleyin.


## <a name="prerequisites"></a>Önkoşullar

Bir veritabanını geri yüklemeden önce aşağıdakilere dikkat edin:

- Veritabanı, aynı Azure bölgesindeki bir SQL Server'ın bir örneğine geri yükleyebilirsiniz.
- Hedef sunucuda kaynak olarak aynı kasaya kayıtlı olması gerekir.
- TDE şifreli bir veritabanını başka bir SQL Server'a geri yüklemek için öncelikle gereken [sertifikayı hedef sunucuya geri](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- "Ana" veritabanını geri yüklemeden önce SQL Server örneği tek kullanıcı modunda başlatma seçeneğini kullanarak başlatın **-m AzureWorkloadBackup**.
    - Değeri **-m** istemci adıdır.
    - Yalnızca belirtilen istemci adını, bağlantı açabilirsiniz.
- Geri yüklemeyi tetikleyecek önce (modeli, master, msdb), tüm sistem veritabanları için SQL Server Agent hizmetini durdurun.
- Tüm bu veritabanlarını için bağlantı geçirmeye çalışan tüm uygulamaları kapatın.

## <a name="restore-a-database"></a>Veritabanını geri yükleme

Geri yüklemek için aşağıdaki izinler gerekir:

* **Yedekleme işleci** burada yaptığınız geri kasasındaki izinleri.
* **Katkıda bulunan (yazma)** yedeklenir VM kaynağına erişim.
* **Katkıda bulunan (yazma)** hedef sanal makine için erişim:
    - Aynı VM'yi geri yükleme, kaynak VM budur.
    - Alternatif bir konuma geri yükleme, bu yeni hedef sanal makine olacaktır.

Şu şekilde geri yükleyin:
1. Hangi SQL Server VM kayıtlı olduğu kasanın açın.
2. Kasa panosunda altında **kullanım**seçin **yedekleme öğeleri**.
3. İçinde **yedekleme öğeleri**altında **Yedekleme Yönetimi türü**seçin **Azure VM'deki SQL**.

    ![Azure VM'de SQL seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Geri yüklenecek veritabanını seçin.

    ![Geri yüklenecek veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Veritabanı menüsünde gözden geçirin. Veritabanı yedeklemesi hakkında bilgi sağlar dahil olmak üzere:

    * Eski ve en son geri yükleme noktaları.
    * Son 24 saat tam ve toplu günlük kurtarma modu ve veritabanları için günlük yedekleme durumu, işlem günlüğü yedeklemeleri için yapılandırılır.

6. Seçin **geri DB**.

    ![Geri yükleme Veritabanı Seç](./media/backup-azure-sql-database/restore-db-button.png)

7. İçinde **geri yükleme Yapılandırması**, verileri geri yüklemek istediğiniz yeri belirtin:
   - **Alternatif konuma**: Veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanı tutun.
   - **Üzerine yaz**: Verilerin, özgün kaynak aynı SQL Server örneğine geri yükleyin. Bu seçenek, özgün veritabanının üzerine yazar.

     > [!Important]
     > Seçilen veritabanı bir Always On kullanılabilirlik grubuna dahilse, SQL Server veritabanı üzerine yazılmasına izin vermez. Yalnızca **alternatif bir konuma** kullanılabilir.
     >

     ![Yapılandırma menü geri yükleme](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. İçinde **geri yükleme Yapılandırması** menüsü altında **nereye Geri Yüklensin**seçin **alternatif bir konuma**.
2. Veritabanını geri yüklemek istediğiniz SQL Server adı ve örneği seçin.
3. İçinde **geri DB adı** kutusunda, hedef veritabanı adı girin.
4. varsa **seçili SQL örneğinde aynı ada sahip bir veritabanı zaten varsa üzerine yaz**.
5. **Tamam**’ı seçin.

    ![Geri Yükleme Yapılandırması menüsünü değerleri sağlayın](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. İçinde **seçin, geri yükleme noktası**seçin kullanılıp kullanılmayacağını [zaman içinde belirli bir noktaya geri yükleme](#restore-to-a-specific-point-in-time) veya [belirli kurtarma noktasına](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > Belirli bir noktaya geri yükleme, yalnızca tam ve toplu günlük kurtarma modunda olan veritabanları için günlük yedeklemeler için kullanılabilir.

### <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

1. İçinde **geri yükleme Yapılandırması** menüsü altında **nereye Geri Yüklensin**seçin **üzerine DB** > **Tamam**.

    ![DB üzerine yazmayı seçin](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. İçinde **seçin, geri yükleme noktası**seçin **günlükleri (zaman içinde nokta)** için [zaman içinde belirli bir noktaya geri yükleme](#restore-to-a-specific-point-in-time). Veya **tam ve değişiklik** geri yüklemek için bir [belirli bir kurtarma noktası](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > Belirli bir noktaya geri yükleme, yalnızca tam ve toplu günlük kurtarma modunda olan veritabanları için günlük yedeklemeler için kullanılabilir.

### <a name="restore-to-a-specific-point-in-time"></a>Belirli bir noktaya geri yükleme

Seçtiyseniz, **günlükleri (zaman içinde nokta)** geri yükleme türü olarak aşağıdakileri yapın:

1.  Altında **geri tarih/saat**, takvimi açın. Takvim üzerinde kalın yazı tipinde kurtarma noktalarına sahip tarihler görüntülenir ve geçerli tarih vurgulanır.
1. Kurtarma noktaları olan bir tarih seçin. Kurtarma noktası olan tarih seçilemez.

    ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

1. Bir tarih seçtikten sonra zaman çizelgesi grafiği sürekli bir aralıktaki kullanılabilir kurtarma noktalarını görüntüler.
1. Kurtarma için zaman çizelgesi grafikte belirtin veya bir zaman seçin. Sonra **Tamam**’ı seçin.

    ![Geri yükleme saatini seçin](./media/backup-azure-sql-database/recovery-point-logs-graph.png)


1. Üzerinde **Gelişmiş Yapılandırma** veritabanını geri yüklemeden sonra işlemsel olmayan tutmak istiyorsanız menüsünü etkinleştir **kurtarma olmadan geri yükle**.
1. Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir hedef yol girin.
1. **Tamam**’ı seçin.

    ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı.
1. Geri yükleme ilerleme durumunu izlemek **bildirimleri** alanında veya seçerek izlemek **geri yükleme işleri** veritabanı menüsünde.

    ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-to-a-specific-restore-point"></a>Belirli bir geri yükleme noktasına geri yükleme

Seçtiyseniz, **tam ve değişiklik** geri yükleme türü olarak aşağıdakileri yapın:

1. Bir kurtarma noktası listeden seçip **Tamam** geri yükleme noktası yordamı tamamlamak için.

    ![Tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

1. Üzerinde **Gelişmiş Yapılandırma** veritabanını geri yüklemeden sonra işlemsel olmayan tutmak istiyorsanız menüsünü etkinleştir **kurtarma olmadan geri yükle**.
1. Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir hedef yol girin.
1. **Tamam**’ı seçin.

    ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı.
1. Geri yükleme ilerleme durumunu izlemek **bildirimleri** alanında veya seçerek izlemek **geri yükleme işleri** veritabanı menüsünde.

    ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

[Yönetme ve izleme](manage-monitor-sql-database-backup.md) Azure Backup tarafından yedeklenen SQL Server veritabanları.
