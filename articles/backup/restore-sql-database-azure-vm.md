---
title: Geri yükleme, Azure Backup ile bir Azure VM'de SQL Server veritabanlarını yedeklenen | Microsoft Docs
description: Bu makalede Azure Backup ile desteklenen bir Azure sanal makinesinde çalışan SQL Server veritabanlarını geri yükleme
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 3571eb2471f9b3f06eb509937fd11866b4e0caa8
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430811"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Azure vm'lerde SQL Server veritabanlarını geri yükleme 


Bu makalede bir Azure Backup kurtarma Hizmetleri kasası ile yedeklenen öğeleri bir Azure sanal makinesinde çalışan SQL Server veritabanını geri yükleme [Azure Backup](backup-overview.md) hizmeti.


> [!NOTE]
> Azure Backup ile bir Azure sanal makinesinde çalışan SQl Server veritabanlarının yedekleme şu anda genel Önizleme aşamasındadır.


Bu makalede, SQL Server veritabanlarını geri yüklemek açıklar. Veritabanları için yedekleme yapılandırmadıysanız bölümündeki yönergeleri [bu makalede](backup-azure-sql-database.md)



## <a name="about-restoring-databases"></a>Veritabanlarını geri yükleme hakkında

Azure yedekleme gibi Azure Vm'lerinde çalışan SQL Server veritabanlarını geri yükleyebilirsiniz:

- Belirli bir tarih ya da işlem günlüğü yedeklemeleri kullanarak süresi (saniye) için geri yükleyin. Azure yedekleme, otomatik olarak uygun tam değişiklik yedeği belirler ve geri yüklemek için gerekli olan günlük yedekleme zincirini seçili zamana dayalı.
- Geri yükleme bir belirli bir zaman yerine belirli bir kurtarma noktasını geri yüklemek için belirli bir tam veya değişiklik yedeklemesinden.


### <a name="prerequisites"></a>Önkoşullar

Bir veritabanını geri yüklemeden önce aşağıdakilere dikkat edin:

- Veritabanı, aynı Azure bölgesindeki bir SQL Server'ın bir örneğine geri yükleyebilirsiniz.
- Hedef sunucuda kaynak olarak aynı kasaya kayıtlı olması gerekir.
- TDE şifrelenmiş veritabanı başka bir SQL Server'a geri yüklemek için öncelikle gereken [sertifikayı hedef sunucuya geri](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- "Ana" veritabanı geri yükleme tetiklemeden önce SQL Server örneği tek kullanıcı modunda başlatma seçeneği ile başlatın **-m AzureWorkloadBackup**.
    - Değeri **-m** istemci adıdır.
    - Bağlantıyı açmak için yalnızca belirtilen istemci adına izin.
- Geri yüklemeyi tetikleyecek önce (modeli, master, msdb), tüm sistem veritabanları için SQL Aracı hizmeti durdurun.
- Bu veritabanlarının herhangi bir bağlantı çalabilir deneyebilir tüm uygulamaları kapatın.

## <a name="restore-a-database"></a>Veritabanını geri yükleme

Geri yüklemek için aşağıdaki izinlere ihtiyacınız olur:

* **Yedekleme işleci** izinleri kasaya hangi geri yapıyor.
* **Katkıda bulunan (yazma)** yedeklenir VM kaynağına erişim.
* **Katkıda bulunan (yazma)** hedef sanal makine için erişim:
    - Geri yükleme, aynı sanal makine için bu kaynak VM olacaktır.
    - Geri yükleme, alternatif bir konuma bu yeni hedef sanal makine olacaktır. 

Şu şekilde geri yükleyin:
1. Hangi SQL Server VM kayıtlı olduğu kasanın açın.
2. Kasa panosunda altında **kullanım**seçin **yedekleme öğeleri**.

    ![Yedekleme öğeleri menüsünü açın](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. İçinde **yedekleme öğeleri**altında **Yedekleme Yönetimi türü**seçin **Azure VM'deki SQL**.

    ![Azure VM'de SQL seçin](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Geri yüklenecek veritabanını seçin.

    ![Geri yüklenecek veritabanını seçin](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Veritabanı menüsünde gözden geçirin. Veritabanı yedeklemesi hakkında bilgi sağlar dahil olmak üzere: 

    * Eski ve en son geri yükleme noktaları.
    * Yedekleme durumu tam ve toplu günlük kurtarma modunda, veritabanları için son 24 saat için işlem günlüğü yedeklemeleri için yapılandırılmışsa oturum.

6. Tıklayın **geri DB**. 

    ![Geri yükleme Veritabanı Seç](./media/backup-azure-sql-database/restore-db-button.png)

7. İçinde **geri yükleme Yapılandırması**, verileri geri yüklemek istediğiniz yeri belirtin:
    - **Alternatif konuma**: Veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını korur.
    - **Üzerine yaz**: Verilerin, özgün kaynak aynı SQL Server örneğine geri yükleyin. Bu seçenek, özgün veritabanının üzerine yazmak için etkisidir.

    > [!Important]
    > Seçilen veritabanı bir her zaman üzerinde kullanılabilirlik grubuna dahilse, SQL Server veritabanı üzerine yazılmasına izin vermez. Yalnızca **alternatif bir konuma** kullanılabilir.
    >

    ![Yapılandırma menü geri yükleme](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Alternatif bir konuma geri yükleme

1. İçinde **geri yükleme Yapılandırması** menüsü altında **nereye Geri Yüklensin**seçin **alternatif bir konuma**.
2. Veritabanını geri yüklemek istediğiniz SQL Server adı ve örneği seçin.
3. İçinde **geri DB adı** kutusunda, hedef veritabanı adı girin.
4. varsa **seçili SQL örneğinde aynı ada sahip bir veritabanı zaten varsa üzerine yaz**.
5. **Tamam** düğmesine tıklayın.

    ![Geri Yükleme Yapılandırması menüsünü değerleri sağlayın](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. İçinde **seçin, geri yükleme noktası**seçin kullanılıp kullanılmayacağını [zaman içinde belirli bir noktaya geri yükleme](#restore-to-a-specific-point-in-time), ya da geri yüklemek için bir [belirli bir kurtarma noktası](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > Belirli bir noktaya geri yükleme, yalnızca tam ve toplu günlük kurtarma moduna sahip veritabanları için günlük yedeklemeler için kullanılabilir. 


### <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

1. İçinde **geri yükleme Yapılandırması** menüsü altında **nereye Geri Yüklensin**seçin **üzerine DB** > **Tamam**.

    ![DB üzerine yazmayı seçin](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. İçinde **seçin, geri yükleme noktası**seçin ** günlükleri (zaman içinde nokta) [zaman içinde belirli bir noktaya geri yükleme](#restore-to-a-specific-point-in-time), veya **tam ve değişiklik** geri yüklemek için bir [belirli kurtarma noktası](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > Belirli bir noktaya geri yükleme, yalnızca tam ve toplu günlük kurtarma modeli veritabanları için günlük yedeklemeler için kullanılabilir. 




    
### <a name="restore-to-a-specific-point-in-time"></a>Belirli bir noktaya geri yükleme

Seçtiyseniz, **günlükleri (zaman içinde nokta)** geri yükleme türü olarak aşağıdakileri yapın:

1.  Altında **geri tarih/saat**, mini takvim seçin. Üzerinde **Takvim**Kalın tarihleri kurtarma noktalarına sahip ve geçerli tarih vurgulanır.
2. Kurtarma noktaları olan bir tarih seçin. Kurtarma noktaları olmadan tarih seçilemez.

    ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

3. Bir tarih seçtikten sonra zaman çizelgesi grafiği sürekli bir aralıktaki kullanılabilir kurtarma noktalarını görüntüler.
4. Zaman Çizelgesi grafiği kullanarak kurtarma için bir saat belirtin veya bir zaman seçin. Daha sonra, **Tamam**'a tıklayın.

    ![Takvimi Aç](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

 
4. Üzerinde **Gelişmiş Yapılandırma** veritabanı geri yüklendikten sonra çalışmaz durumda tutmak istiyorsanız menüsünü etkinleştir **kurtarma olmadan geri yükle**.
5. Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir hedef yol girin.
6. **Tamam** düğmesine tıklayın.

    ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)


7. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı.
8. Geri yükleme ilerleme durumunu izlemek **bildirimleri** seçin veya alan **geri yükleme işleri** veritabanı menüsünde.

    ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)



### <a name="restore-to-a-specific-restore-point"></a>Belirli bir geri yükleme noktasına geri yükleme

Seçtiyseniz, **tam ve değişiklik** geri yükleme türü olarak aşağıdakileri yapın:


1. Listeden bir kurtarma noktası seçin ve tıklayın **Tamam** geri yükleme noktası yordamı tamamlamak için.

    ![Tam kurtarma noktası seçin](./media/backup-azure-sql-database/choose-fd-recovery-point.png)
        
2. Üzerinde **Gelişmiş Yapılandırma** veritabanı geri yüklendikten sonra çalışmaz durumda tutmak istiyorsanız menüsünü etkinleştir **kurtarma olmadan geri yükle**.
3. Hedef sunucuda geri yükleme konumunu değiştirmek isterseniz, yeni bir hedef yol girin. 
4. **Tamam** düğmesine tıklayın.

    ![Gelişmiş Yapılandırma menüsü](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

7. Üzerinde **geri** menüsünde **geri** geri yükleme işi başlatılamadı.
8. Geri yükleme ilerleme durumunu izlemek **bildirimleri** seçin veya alan **geri yükleme işleri** veritabanı menüsünde.

    ![Geri yükleme işi ilerleme durumu](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

[Yönetme ve izleme](manage-monitor-sql-database-backup.md) SQL Server veritabanlarını Azure Backup tarafından yedeklendi.
