---
title: Azure Backup sunucusu ile SQL Server iş yükleri için Azure yedekleme
description: Azure Backup sunucusu ile SQL Server veritabanlarını yedeklemeye giriş
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 03/24/2017
ms.author: kasinh
ms.openlocfilehash: 72b2368979f0c9e546e1c7ef7fc462bf1d64c947
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60782081"
---
# <a name="back-up-sql-server-to-azure-with-azure-backup-server"></a>Azure ile Azure Backup sunucusu için SQL Server'ı yedekleme
Bu makalede, Microsoft Azure Backup sunucusu (MABS) kullanarak SQL Server veritabanlarını yedeklemesi için yapılandırma adımlarında yol gösterir.

SQL Server veritabanı yedeği Azure'a ve azure'dan kurtarma yönetimini üç adımdan oluşur:

1. SQL Server veritabanlarını azure'a korumak için bir yedekleme ilkesi oluşturun.
2. Azure için isteğe bağlı yedekleme kopyası oluşturun.
3. Veritabanını Azure'dan kurtarma.

## <a name="before-you-start"></a>Başlamadan önce
Başlamadan önce sahip olduğunuzdan emin olun [yüklü ve Azure Backup sunucusu hazırlanmış](backup-azure-microsoft-azure-backup.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>SQL Server veritabanlarını azure'a korumak için bir yedekleme ilkesi oluşturma
1. Azure Backup sunucusu kullanıcı Arabirimi üzerindeki, tıklayın **koruma** çalışma.
2. Araç şeridinde tıklayın **yeni** yeni bir koruma grubu oluşturma.

    ![Koruma grubu oluşturma](./media/backup-azure-backup-sql/protection-group.png)
3. MABS gösterir Kılavuzu başlangıç ekranı oluşturma ile ilgili bir **koruma grubu**. **İleri**’ye tıklayın.
4. Seçin **sunucuları**.

    ![Koruma grubu türü - 'Sunucuları' seçin](./media/backup-azure-backup-sql/pg-servers.png)
5. Yedeklenecek veritabanlarını mevcut olduğu SQL Server makinesinde genişletin. MABS, o sunucudan yedeklenebilir çeşitli veri kaynakları gösterir. Genişletin **tüm SQL paylaşımlar** ve yedeklenecek (Bu durumda seçtik ReportServer$ MSDPM2012 ve ReportServer$ MSDPM2012TempDB) veritabanlarını seçin. **İleri**’ye tıklayın.

    ![SQL DB seçin](./media/backup-azure-backup-sql/pg-databases.png)
6. Koruma grubu için bir ad girin ve seçin **çevrimiçi koruma istiyorum** onay kutusu.

    ![Veri koruma yöntemini - kısa süreli disk ve çevrimiçi Azure](./media/backup-azure-backup-sql/pg-name.png)
7. İçinde **kısa vadeli hedefleri belirtin** ekranında, diske yedekleme noktaları oluşturmak için gerekli girişleri içerir.

    Olduğunu görebiliriz burada **bekletme aralığı** ayarlanır *5 gün*, **eşitleme sıklığı** bir kez ayarlanır her *15 dakika* olduğu hangi yedekleme alınma sıklığı. **Hızlı tam yedekleme** ayarlanır *fiyatlara*.

    ![Kısa vadeli hedefleri](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > 8:00 PM (göre ekran giriş) bir yedekleme noktasının günde değiştirilmiş verileri önceki günün 8: 00'dan yedekleme noktasından aktarmak tarafından oluşturulur. Bu işlem çağrılırken **hızlı tam yedekleme**. Günlükleri eşzamanlı işlem sırasında her 15 dakikada gerekmiyorsa 9: 00'da – veritabanını kurtarmak için bir son günlükler yürüterek noktası oluşturulduktan sonra hızlı tam yedekleme noktası (Bu örnekte 8 pm).
   >
   >

8. **İleri**'ye tıklayın.

    MABS, kullanılabilir depolama alanı ve olası disk alanı kullanımını gösterir.

    ![Disk ayırma](./media/backup-azure-backup-sql/pg-storage.png)

    Varsayılan olarak, MABS için ilk yedek kopyanın kullanılan veri kaynağı (SQL Server veritabanı) başına tek bir birim oluşturur. Bu yaklaşımı kullanarak, MABS koruma 300 veri kaynakları (SQL Server veritabanları) için Mantıksal Disk Yöneticisi (LDM) sınırlar. Bu sınırlamaya geçici bir çözüm için seçin **DPM depolama Havuzu'ndaki verileri birlikte bulundur**, seçeneği. Bu seçeneği kullanırsanız, MABS tek bir birim için birden çok veri kaynağı, en çok 2.000 SQL veritabanlarını korumak MABS sağlayan kullanır.

    Varsa **birimleri otomatik olarak Büyüt** seçeneği seçildiğinde, üretim veri miktarı arttıkça, MABS için artan yedekleme birimi hesap. Varsa **birimleri otomatik olarak Büyüt** seçeneği seçili değilse, MABS koruma grubundaki veri kaynakları için kullanılan yedekleme alanının sınırlar.
9. Yöneticiler, bant genişliği Tıkanıklığı önlemek için bu ilk yedekleme el ile (Kapalı ağ) aktarılması veya ağ üzerinden seçeneği sunulur. Bunlar ayrıca, ilk aktarım oluşabilir zaman yapılandırabilirsiniz. **İleri**’ye tıklayın.

    ![İlk çoğaltma yöntemi](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedek kopyanın aktarımı (SQL Server veritabanı) tüm veri kaynağının üretim sunucusu (SQL Server makinesi) MABS gerektirir. Bu veri büyük olabilir ve ağ üzerinden veri aktarımı için bant genişliği aşabilir. Bu nedenle, ilk yedekleme aktarmak Yöneticiler birini seçebilirsiniz: **El ile** (çıkarılabilir medya kullanarak) bant genişliği Tıkanıklığı önlemek veya **ağ üzerinden otomatik olarak** (belirtilen saatte).

    İlk Yedekleme tamamlandıktan sonra yedekleri geri kalanını ilk yedek kopyanın üzerinde artımlı yedeklemeleri olursunuz. Artımlı yedekleme küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.
10. Tutarlılık denetimi çalıştırmak ve istediğiniz zaman seçin **sonraki**.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS, yedekleme noktası bütünlüğünü denetlemek için bir tutarlılık gerçekleştirebilirsiniz. Bu üretim sunucusunda (Bu senaryoda, SQL Server makinesi) ve yedeklenen veri yedekleme dosyasının MABS, söz konusu dosya için sağlama toplamını hesaplar. Bir çakışma olması durumunda, MABS, yedekleme dosyası bozuk olduğu varsayılır. MABS, yedeklenen veriler için sağlama toplamı eşleşmezliği karşılık gelen blokları göndererek rectifies. Tutarlılık denetimi performans kullanımı yoğun bir işlem olduğundan, yöneticiler tutarlılık denetimi zamanlaması veya otomatik olarak çalıştırarak seçeneğiniz vardır.
11. Veri kaynaklarının çevrimiçi korumasını belirtmek için tıklayın ve Azure ile korunacak veritabanlarını seçin **sonraki**.

    ![Veri kaynaklarını seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Yöneticiler, yedekleme zamanlamaları ve kuruluşun ilkelerini uygun bekletme ilkeleri seçebilirsiniz.

    ![Zamanlama ve bekletme](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeler günde bir kez 8 PM (ekranın alt bölümünde) ve 12: 00'dan alınmıştır

    > [!NOTE]
    > Diskte, Hızlı Kurtarma için birkaç kısa vadeli kurtarma noktaları için iyi bir uygulamadır. Bu kurtarma noktalarından "operasyonel kurtarma için" kullanılır. Azure, daha yüksek SLA'ları ile iyi şirket dışı konumu olarak hizmet veren ve kullanılabilirlik garantisi.
    >
    >

    **En iyi yöntem**: Azure yedekleme DPM kullanarak yerel disk yedeklemeleri tamamlanmasından sonra zamanlandığını emin olun. Bu, Azure'a kopyalanacak en son disk yedeklemesi sağlar.

13. Bekletme İlkesi zamanlamayı seçin. Bekletme İlkesi birlikte nasıl çalıştığı hakkında ayrıntılar, sağlanan [kullanımı Azure Backup'ın bant altyapısı makale değiştirin](backup-azure-backup-cloud-as-tape.md).

    ![Bekletme İlkesi](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedeklemeler günde bir kez 12: 00'a ve 8 PM (ekranın alt bölümünde) alınır ve 180 gün boyunca saklanır.
    * Cumartesi gece 12: 00'da yedeği 104 hafta boyunca korunur
    * Son 12: 00'da Cumartesi yedeği 60 ay boyunca saklanır
    * 12: 00'da Mart Son Cumartesi yedeği 10 yıl boyunca korunur
14. Tıklayın **sonraki** ve Azure'a ilk yedek kopyanın aktarılması uygun seçeneği belirleyin. Seçebileceğiniz **ağ üzerinden otomatik olarak** veya **Çevrimdışı Yedekleme**.

    * **Ağ üzerinden otomatik olarak** yedekleme için seçilen zamanlamaya göre yedekleme verileri Azure'a aktarır.
    * Nasıl **Çevrimdışı Yedekleme** çalışır açıklanmıştır [Çevrimdışı Yedekleme iş akışı Azure backup'taki](backup-azure-backup-import-export.md).

    ' A tıklayın ve Azure ile ilk yedek kopyanın göndermek için ilgili aktarım mekanizması seçme **sonraki**.
15. İlke ayrıntıları gözden geçirin, sonra **özeti** ekranında, tıklayarak **Grup Oluştur** iş akışının tamamlanması düğmesi. Tıklayabilirsiniz **Kapat** düğmesine tıklayın ve izleme çalışma alanı, işin ilerleme durumunu izleyin.

    ![Devam eden için koruma grubu oluşturma](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>İsteğe bağlı yedekleme SQL Server veritabanı
Önceki adımları bir yedekleme İlkesi oluşturuldu, ancak bir "kurtarma noktası" yalnızca ilk yedekleme gerçekleştiğinde oluşturulur. Zamanlayıcı etkisini göstermeye beklemek yerine, tetikleyici kurtarma oluşturulması aşağıdaki adımları el ile işaretleyin.

1. Koruma grubunun durumu gösterilene kadar bekleyin **Tamam** kurtarma noktası oluşturmadan önce veritabanı için.

    ![Koruma grubu üyeleri](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Sağ tıklatın ve veritabanı **kurtarma noktası oluştur**.

    ![Çevrimiçi kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Seçin **çevrimiçi koruma** öğelerine tıklayıp **Tamam**. Bu, Azure'da bir kurtarma noktası oluşturulmasını başlatır.

    ![Kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. İş ilerleme durumunu görüntüleyebileceğiniz **izleme** nerede bulabileceğiniz devam eden çalışma alanı, bir de gösterildiği gibi iş sonraki şekilde.

    ![İzleme konsolu](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>SQL Server veritabanını Azure'dan kurtarma
Aşağıdaki adımlar, bir Korunan varlık (SQL Server veritabanı) Azure'dan kurtarmak için gereklidir.

1. DPM sunucusu Yönetim Konsolu'nu açın. Gidin **kurtarma** sunucuları görebileceğiniz çalışma DPM tarafından yedeklenen. Gerekli veritabanında (Bu durum ReportServer$ MSDPM2012) göz atın. Seçin bir **kurtarma** ile biten zaman **çevrimiçi**.

    ![Kurtarma noktası seçin](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Veritabanı adını sağ tıklatıp **kurtarmak**.

    ![Azure'dan kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. DPM, kurtarma noktası ayrıntılarını gösterir. **İleri**’ye tıklayın. Veritabanının üzerine yazmak için kurtarma türünü seçin. **özgün SQL Server örneğine Kurtar**. **İleri**’ye tıklayın.

    ![Özgün konuma Kurtar](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte, başka bir SQL Server örneğine veya tek başına bir ağ klasörüne kurtarma veritabanı DPM sağlar.
4. İçinde **belirtin kurtarma seçeneklerini** ekran recovery tarafından kullanılan bant genişliğini azaltmak için ağ bant genişliği kullanımını azaltma gibi kurtarma seçeneklerini seçebilirsiniz. **İleri**’ye tıklayın.
5. İçinde **özeti** ekran, şu ana kadar verilen tüm kurtarma yapılandırmaları görürsünüz. Tıklayın **kurtarmak**.

    Kurtarılan veritabanı kurtarma durumunu gösterir. Tıklayabilirsiniz **kapatmak** sihirbazı kapatın ve ilerleme durumunu görüntülemek için **izleme** çalışma.

    ![Kurtarma işlemini başlatın](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma tamamlandıktan sonra geri yüklenen veritabanının tutarlı uygulamasıdır.

### <a name="next-steps"></a>Sonraki Adımlar:
• [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
