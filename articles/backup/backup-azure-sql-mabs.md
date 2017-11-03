---
title: "Azure yedekleme sunucusu kullanarak SQL Server iş yükleri için Azure yedeklemeyi | Microsoft Docs"
description: "Azure yedekleme sunucusu kullanarak SQL Server veritabanlarını yedekleme için bir giriş"
services: backup
documentationcenter: 
author: pvrk
manager: Shivamg
editor: 
ms.assetid: c8b1f7ec-26b1-4ef0-a3f2-91aec959daea
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 2af9ebaa8f52690ed63406cbd85b77544d2d900d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-sql-server-to-azure-with-azure-backup-server"></a>Azure ile Azure yedekleme sunucusu için SQL Server'ı Yedekle
Bu makalede Microsoft Azure yedekleme sunucusu (MABS) kullanarak SQL Server veritabanlarını yedekleme için yapılandırma adımlarını size yol gösterir.

Azure ve Azure kurtarma için SQL Server Veritabanı yedeğinin yönetim üç adımdan oluşur:

1. Azure SQL Server veritabanlarını korumak için bir yedekleme ilkesi oluşturun.
2. İsteğe bağlı Azure yedek kopyalarını oluşturun.
3. Veritabanını Azure'dan kurtarma.

## <a name="before-you-start"></a>Başlamadan önce
Başlamadan önce şunları yapın [yüklü ve Azure yedekleme sunucusu hazırlanan](backup-azure-microsoft-azure-backup.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Azure SQL Server veritabanlarını korumak için bir yedekleme İlkesi Oluştur
1. Azure yedekleme sunucusu UI'ı tıklatın **koruma** çalışma.
2. Araç şeridinde tıklatın **yeni** yeni bir koruma grubu oluşturulamıyor.

    ![Koruma grubu oluşturma](./media/backup-azure-backup-sql/protection-group.png)
3. MABS gösterir yönlendirme ile başlangıç ekranı oluşturma ile ilgili bir **koruma grubu**. **İleri**’ye tıklayın.
4. Seçin **sunucuları**.

    ![Koruma grubu türü - 'Sunucuları' seçin](./media/backup-azure-backup-sql/pg-servers.png)
5. Yedeklenecek veritabanlarını mevcut olduğu SQL Server makinesinde genişletin. MABS o sunucudan yedeklenebilir çeşitli veri kaynakları gösterir. Genişletme **tüm SQL paylaşımları** ve yedeklenmesi için (Bu durumda biz seçili ReportServer$ MSDPM2012 ve ReportServer$ MSDPM2012TempDB) veritabanlarını seçin. **İleri**’ye tıklayın.

    ![SQL DB seçin](./media/backup-azure-backup-sql/pg-databases.png)
6. Koruma grubu için bir ad ve seçin **çevrimiçi koruma istiyorum** onay kutusu.

    ![Veri koruma yöntemini - kısa süreli disk ve çevrimiçi Azure](./media/backup-azure-backup-sql/pg-name.png)
7. İçinde **kısa vadeli hedefleri belirtin** ekranında, diske yedekleme noktaları oluşturmak için gerekli girişleri içerir.

    Burada, gösteriliyor **bekletme aralığı** ayarlanır *5 gün*, **eşitleme sıklığı** için bir kez ayarlanır her *15 dakika* yedekleme alınır sıklığı olduğu. **Hızlı tam yedekleme** ayarlanır *fiyatlara*.

    ![Kısa vadeli hedefleri](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > 8:00 PM (göre ekran giriş), bir yedekleme noktasının önceki günün 8:00 PM yedekleme noktasından değiştirilmiş verileri aktararak her gün oluşturulur. Bu işlem çağrılırken **hızlı tam yedekleme**. Günlükleri eşzamanlı işlem sırasında her 15 dakikada varsa 9: 00'da – veritabanını kurtarmak için gerekirse son günlüklerini tekrarlamak tarafından oluşturulan noktası sonra hızlı tam yedekleme noktası (Bu durumda 8 pm).
   >
   >

8. **İleri**’ye tıklayın

    MABS, genel depolama alanı ve olası disk alanı kullanımını gösterir.

    ![Disk ayırma](./media/backup-azure-backup-sql/pg-storage.png)

    Varsayılan olarak, MABS ilk yedek kopya için kullanılan veri kaynağı (SQL Server veritabanı) başına tek bir birim oluşturur. Bu yaklaşımı kullanarak, Mantıksal Disk Yöneticisi (LDM) MABS koruma 300 veri kaynakları (SQL Server veritabanları) için sınırlar. Bu sınırlamaya geçici bir çözüm için seçin **DPM depolama Havuzu'ndaki verileri birlikte bulundur**, seçeneği. Bu seçeneği kullanırsanız, MABS tek bir birimde birden çok veri kaynağı için en fazla 2000 SQL veritabanlarını korumak MABS sağlayan kullanır.

    Varsa **birimleri otomatik olarak büyütün** seçeneği seçildiğinde, üretim verileri büyüdükçe, MABS artan yedekleme birimi için hesap. Varsa **birimleri otomatik olarak büyütün** seçeneği seçili değilse, MABS koruma grubundaki veri kaynakları için kullanılan yedekleme depolama sınırlar.
9. Yöneticiler, bant genişliği tıkanıklık önlemek için bu ilk yedekleme el ile (Kapalı ağ) aktarılması veya ağ üzerinden seçeneği sunulur. Bunlar, ilk aktarımı oluşabilir zaman da yapılandırabilirsiniz. **İleri**’ye tıklayın.

    ![İlk çoğaltma yöntemi](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedekleme kopyasının aktarımını tüm veri kaynağı (SQL Server veritabanı) (SQL Server makinesinde) üretim sunucusundan MABS gerektirir. Bu veri büyük olabilir ve ağ üzerinden veri aktarırken bant genişliği aşabilir. Bu nedenle, ilk yedekleme aktarmak Yöneticiler seçebilirsiniz: **el ile** (çıkarılabilir medya kullanarak) bant genişliği tıkanıklık önlemek için veya **otomatik olarak ağ üzerinden** (bir belirtilen zamanda).

    İlk Yedekleme tamamlandıktan sonra yedekleri geri kalanı ilk yedek kopyanın artımlı yedeklemelerin olduğu. Artımlı yedeklemeler küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.
10. Tutarlılık denetimi çalıştırın ve tıklatın istediğinizde seçin **sonraki**.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS, yedekleme noktası bütünlüğünü denetlemek için bir tutarlılık gerçekleştirebilirsiniz. Yedekleme dosyasının (SQL Server makinesinde bu senaryoda) üretim sunucusunda ve yedeklenmiş verileri MABS, bu dosya için sağlama toplamı hesaplar. Bir çakışma olması durumunda MABS konumundaki yedeklenen dosya bozuk olduğu varsayılır. MABS sağlama toplamı eşleşmezliği karşılık gelen blokları göndererek yedeklenmiş verileri rectifies. Tutarlılık denetimi performansı yoğun bir işlem olduğundan, yöneticilerin tutarlılık denetimi zamanlaması veya otomatik olarak çalıştırarak seçeneğiniz vardır.
11. Veri kaynaklarının çevrimiçi korumasını belirtmek için Azure ve seçeneğini korunacak veritabanı seçin **sonraki**.

    ![Veri kaynakları seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Yöneticiler, yedekleme zamanlamaları ve kuruluşun ilkelerini uygun bekletme ilkeleri seçebilirsiniz.

    ![Zamanlama ve bekletme](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeleri günde bir kez 12: 00'dan ve 8 PM (ekranın alt bölümünün) alınır

    > [!NOTE]
    > Hızlı Kurtarma için diskte birkaç kısa vadeli kurtarma noktaları için iyi bir uygulamadır. Bu kurtarma noktaları "işletimsel kurtarma için" kullanılır. Azure yüksek SLA ile iyi site dışı konumu olarak hizmet verir ve kullanılabilirliğini garanti.
    >
    >

    **En iyi uygulaması**: DPM kullanarak yerel disk yedekleme tamamlandıktan sonra zamanlanmış Azure yedeklemeler emin olun. Bu, Azure'a kopyalanacak en son disk yedeklemesi sağlar.

13. Bekletme İlkesi zamanlamayı seçin. Bekletme İlkesi nasıl çalıştığı hakkında bilgi sırasında sağlanan [bant altyapısı Makalenizi değiştirmek için Azure Yedekleme'yi](backup-azure-backup-cloud-as-tape.md).

    ![Bekletme İlkesi](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedekleme günde bir kez 12: 00'dan ve 8 PM (ekranın alt bölümünün) alınır ve 180 gün için korunur.
    * Cumartesi 12: 00'da bir yedekleme 104 hafta boyunca tutulur
    * Son Cumartesi 12: 00'da bir yedekleme 60 ay korunur
    * Son Cumartesi 12: 00'da Mart yedekleme 10 yılı aşkın korunur
14. Tıklatın **sonraki** ve Azure'a ilk yedek kopyayı aktarmak için uygun seçeneği belirleyin. Seçebileceğiniz **otomatik olarak ağ üzerinden** veya **Çevrimdışı Yedekleme**.

    * **Ağ üzerinden otomatik olarak** yedekleme için seçilen zamanlamaya göre yedekleme verileri Azure'a aktarır.
    * Nasıl **Çevrimdışı Yedekleme** works açıklanmıştır adresindeki [Çevrimdışı Yedekleme iş akışı Azure Yedekleme'de](backup-azure-backup-import-export.md).

    İlk yedek kopyayı Azure ve seçeneğini göndermek için ilgili aktarım mekanizması seçin **sonraki**.
15. İlke ayrıntıları gözden geçirin sonra **Özet** ekranında, tıklayın **Grup Oluştur** iş akışını tamamlamak için düğmesini. Tıklayabilirsiniz **Kapat** düğmesini tıklatın ve çalışma alanı izleme işin ilerleme durumunu izleyin.

    ![Devam eden için koruma grubu oluşturma](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>İsteğe bağlı bir SQL Server veritabanının yedeğini
Bir yedekleme İlkesi önceki adımlarda oluşturduğunuz sırada "kurtarma noktası" yalnızca ilk yedek oluştuğunda oluşturulur. İlkenin etkisini gösterip için Zamanlayıcı için beklemek yerine, tetikleyici kurtarma oluşturulması aşağıdaki adımları el ile gelin.

1. Koruma grubu durumunu görüntüleyene kadar bekleyin **Tamam** kurtarma noktası oluşturmadan önce veritabanı için.

    ![Koruma grubu üyeleri](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Sağ tıklatın ve veritabanı **kurtarma noktası oluştur**.

    ![Çevrimiçi kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Seçin **çevrimiçi koruma** açılan menüsüne ve ardından içinde **Tamam**. Bu, Azure'da bir kurtarma noktası oluşturma başlatır.

    ![Kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. İçinde iş ilerleme durumunu görüntüleyebilirsiniz **izleme** burada bulabilirsiniz etmekte olan bir çalışma alanında bir içinde gösterilen gibi iş sonraki şekilde.

    ![İzleme konsolu](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>SQL Server veritabanını Azure'dan kurtarma
Aşağıdaki adımlar, bir Korunan varlık (SQL Server veritabanı) Azure yedeklemeden kurtarmak için gereklidir.

1. DPM sunucusu Yönetim Konsolu'nu açın. Gidin **kurtarma** burada görebilirsiniz sunucuları çalışma DPM tarafından yedeklenen. Gerekli veritabanında (Bu örnek ReportServer$ MSDPM2012) göz atın. Seçin bir **kurtarma** ile biten zaman **çevrimiçi**.

    ![Kurtarma noktası seçin](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Veritabanı adını sağ tıklatıp **kurtarmak**.

    ![Azure'dan kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. DPM, kurtarma noktası ayrıntılarını gösterir. **İleri**’ye tıklayın. Veritabanının üzerine yazmak için kurtarma türünü seçin **özgün SQL Server örneğine Kurtar**. **İleri**’ye tıklayın.

    ![Özgün konuma Kurtar](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte, DPM, başka bir SQL Server örneğine veya tek başına bir ağ klasörüne kurtarma veritabanının verir.
4. İçinde **belirtin Kurtarma Seçenekleri** ekran, Kurtarma tarafından kullanılan bant genişliğini azaltmak için ağ bant genişliği kullanımını azaltma gibi kurtarma seçenekleri seçebilirsiniz. **İleri**’ye tıklayın.
5. İçinde **Özet** ekran, o ana kadarki sağlanan tüm kurtarma yapılandırmaları bakın. Tıklatın **kurtarmak**.

    Kurtarılan veritabanı kurtarma durumunu gösterir. Tıklayabilirsiniz **kapatmak** sihirbazı kapatın ve devam eden görüntülemek için **izleme** çalışma.

    ![Kurtarma işlemini başlatın](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma tamamlandıktan sonra geri yüklenen veritabanı tutarlı uygulamasıdır.

### <a name="next-steps"></a>Sonraki Adımlar:
• [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
