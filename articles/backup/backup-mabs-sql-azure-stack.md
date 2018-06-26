---
title: SQL Server iş yüklerini Azure yığında yedekleyin
description: SQL Server iş yükü Azure yığında korumak için Azure yedekleme sunucusu kullanın.
services: backup
author: pvrk
manager: Shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/8/2018
ms.author: pullabhk
ms.openlocfilehash: ca7da7ab048b6f7bfdba81aac9bc7702b20ff967
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751806"
---
# <a name="back-up-sql-server-on-stack"></a>Yığın üzerinde SQL Server'ı Yedekle
Bu makalede Azure yığında SQL Server veritabanlarını korumak için Microsoft Azure yedekleme sunucusu (MABS) yapılandırmak için kullanın.

Azure ve Azure kurtarma için SQL Server Veritabanı yedeğinin yönetim üç adımdan oluşur:

1. SQL Server veritabanlarını korumak için bir yedekleme İlkesi Oluştur
2. İsteğe bağlı yedek kopyalarını oluşturun
3. Disk ve Azure veritabanını kurtarma

## <a name="before-you-start"></a>Başlamadan önce

[Yükleyin ve Azure yedekleme sunucusu hazırlayın](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Azure SQL Server veritabanlarını korumak için bir yedekleme İlkesi Oluştur
1. Azure yedekleme sunucusu UI'ı tıklatın **koruma** çalışma.

2. Araç şeridinde tıklatın **yeni** yeni bir koruma grubu oluşturulamıyor.

    ![Koruma grubu oluşturma](./media/backup-azure-backup-sql/protection-group.png)

    Azure yedekleme sunucusu oluşturmada size yol gösterir koruma grubu Sihirbazı başlar bir **koruma grubu**. **İleri**’ye tıklayın.

3. İçinde **koruma grubu türünü seçin** ekranında, seçin **sunucuları**.

    ![Koruma grubu türü - 'Sunucuları' seçin](./media/backup-azure-backup-sql/pg-servers.png)

4. İçinde **grup üyelerini seçin** ekran, kullanılabilir üyeler listesi çeşitli veri kaynakları görüntüler. Tıklatın **+** bir klasörünü genişletin ve alt klasörleri ortaya çıkarmak için. Öğeyi seçmek için onay kutusuna tıklayın.

    ![SQL DB seçin](./media/backup-azure-backup-sql/pg-databases.png)

    Seçili tüm öğeleri seçili üyeleri listede görüntülenir. Sunucuları veya korumak istediğiniz veritabanlarını seçtikten sonra **sonraki**.

5. İçinde **veri koruma yöntemini seçin** ekranında, koruma grubu için bir ad ve seçin **çevrimiçi koruma istiyorum** onay kutusu.

    ![Veri koruma yöntemini - kısa süreli disk ve çevrimiçi Azure](./media/backup-azure-backup-sql/pg-name.png)

6. İçinde **kısa vadeli hedefleri belirtin** tıklatın ve disk yedekleme noktaları oluşturmak için gerekli girdiler içeriyor, ekran **sonraki**.

    Örnekte, **bekletme aralığı** olan **5 gün**, **eşitleme sıklığı** kez her **15 dakika**, yedekleme olduğu sıklığı. **Hızlı tam yedekleme** ayarlanır **fiyatlara**.

    ![Kısa vadeli hedefleri](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Gösterilen örnekte, önceki günün 8:00 PM yedekleme noktasından değiştirilmiş verileri aktararak 8: 00'da her gün bir yedekleme noktası oluşturulur. Bu işlem çağrılırken **hızlı tam yedekleme**. İşlem günlükleri 15 dakikada bir eşitlenir. 9: 00'da veritabanını kurtarmak gerekirse, nokta son günlüklerinden oluşturulur hızlı tam yedekleme noktası (Bu durumda 8 PM).
   >
   >

7. Üzerinde **disk ayırmasını gözden** ekranında, genel depolama alanı ve olası disk alanının doğrulayın. **İleri**’ye tıklayın.

8. İçinde **çoğaltma oluşturma yöntemini seçin**, ilk kurtarma noktası oluşturma seçin. Bant genişliği tıkanıklık önlemek için el ile (Kapalı ağ) ilk yedekleme aktarabilir veya ağ üzerinden. İlk yedek aktarmak için beklenecek seçerseniz, zaman için ilk aktarım belirtebilirsiniz. **İleri**’ye tıklayın.

    ![İlk çoğaltma yöntemi](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedek kopyayı Azure yedekleme sunucusuna (SQL Server makinesinde) üretim sunucudan tüm veri kaynağı (SQL Server veritabanı) aktarma gerektirir. Bu veri büyük olabilir ve ağ üzerinden veri aktarırken bant genişliği aşabilir. Bu nedenle, ilk yedekleme aktarımı seçebilirsiniz: **el ile** (çıkarılabilir medya kullanarak) bant genişliği tıkanıklık önlemek için veya **otomatik olarak ağ üzerinden** (bir belirtilen zamanda).

    İlk Yedekleme tamamlandıktan sonra yedekleri geri kalanı ilk yedek kopyanın artımlı yedeklemelerin olduğu. Artımlı yedeklemeler küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.

9. Tutarlılık denetimi çalıştırın ve tıklatın istediğinizde seçin **sonraki**.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup sunucusu yedekleme noktası bütünlüğünü üzerinde tutarlılık denetimi gerçekleştirir. Azure Backup sunucusu yedekleme dosyasının (SQL Server makinesinde bu senaryoda) üretim sunucusunda ve yedeklenmiş verileri bu dosya için sağlama toplamı hesaplar. Bir çakışma varsa, Azure yedekleme sunucusu üzerindeki yedeklenen dosya bozuk varsayılır. Azure yedekleme sunucusu için sağlama toplamı eşleşmezliği karşılık gelen blokları göndererek yedeklenmiş verileri rectifies. Tutarlılık denetimleri performansı yoğun olduğundan, tutarlılık denetimi zamanlamanız ya da otomatik olarak çalıştırabilirsiniz.

10. Veri kaynaklarının çevrimiçi korumasını belirtmek için Azure ve seçeneğini korunacak veritabanı seçin **sonraki**.

    ![Veri kaynakları seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Yedekleme zamanlamaları ve kuruluşun ilkelerini uygun bekletme ilkeleri seçin.

    ![Zamanlama ve bekletme](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeleri günde bir kez 12: 00'dan ve 8 PM (ekranın alt bölümünün) alınır

    > [!NOTE]
    > Hızlı Kurtarma için diskte birkaç kısa vadeli kurtarma noktaları için iyi bir uygulamadır. Bu kurtarma noktaları işletimsel kurtarma için kullanılır. Azure yüksek SLA ile iyi site dışı konumu olarak hizmet verir ve kullanılabilirliğini garanti.
    >
    >

    **En iyi uygulaması**: yerel disk yedeklemeleri tamamladıktan sonra başlatmak için Azure yedekleme zamanlarsanız, en son disk yedeklemeleri her zaman için Azure kopyalanır.

12. Bekletme İlkesi zamanlamayı seçin. Bekletme İlkesi nasıl çalıştığı hakkında bilgi sırasında sağlanan [bant altyapısı Makalenizi değiştirmek için Azure Yedekleme'yi](backup-azure-backup-cloud-as-tape.md).

    ![Saklama İlkesi](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedekleme günde bir kez 12: 00'dan ve 8 PM (ekranın alt bölümünün) alınır ve 180 gün için korunur.
    * Cumartesi 12: 00'da bir yedekleme 104 hafta boyunca tutulur
    * Son Cumartesi 12: 00'da bir yedekleme 60 ay korunur
    * Son Cumartesi 12: 00'da Mart yedekleme 10 yılı aşkın korunur
13. Tıklatın **sonraki** ve Azure'a ilk yedek kopyayı aktarmak için uygun seçeneği belirleyin. Seçebileceğiniz **otomatik olarak ağ üzerinden**

14. İlke ayrıntıları gözden geçirin sonra **Özet** ekranında **Grup Oluştur** iş akışını tamamlamak için. Tıklayabilirsiniz **Kapat** ve izleme çalışma alanı, işin ilerleme durumunu izleyin.

    ![Devam eden için koruma grubu oluşturma](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>İsteğe bağlı bir SQL Server veritabanının yedeğini
Bir yedekleme İlkesi önceki adımlarda oluşturduğunuz sırada "kurtarma noktası" yalnızca ilk yedek oluştuğunda oluşturulur. İlkenin etkisini gösterip için Zamanlayıcı için beklemek yerine, tetikleyici kurtarma oluşturulması aşağıdaki adımları el ile gelin.

1. Koruma grubu durumunu görüntüleyene kadar bekleyin **Tamam** kurtarma noktası oluşturmadan önce veritabanı için.

    ![Koruma grubu üyeleri](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Sağ tıklatın ve veritabanı **kurtarma noktası oluştur**.

    ![Çevrimiçi kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Seçin **çevrimiçi koruma** açılan menüsüne ve ardından içinde **Tamam** Azure'da bir kurtarma noktası oluşturma başlatmak için.

    ![Kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. İş sürüyor görüntülemek **izleme** çalışma.

    ![İzleme konsolu](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>SQL Server veritabanını Azure'dan kurtarma
Aşağıdaki adımlar, bir Korunan varlık (SQL Server veritabanı) Azure yedeklemeden kurtarmak için gereklidir.

1. Azure Backup sunucusu Yönetim Konsolu'nu açın. Gidin **kurtarma** burada görebilirsiniz korumalı sunuculardaki çalışma. Gerekli veritabanında (Bu örnek ReportServer$ MSDPM2012) göz atın. Seçin bir **kurtarma** olarak belirtilen zaman bir **çevrimiçi** gelin.

    ![Kurtarma noktası seçin](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Veritabanı adını sağ tıklatıp **kurtarmak**.

    ![Azure'dan kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS kurtarma noktası ayrıntılarını gösterir. **İleri**’ye tıklayın. Veritabanının üzerine yazmak için kurtarma türünü seçin **özgün SQL Server örneğine Kurtar**. **İleri**’ye tıklayın.

    ![Özgün konuma Kurtar](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte, veritabanını başka bir SQL Server örneğine veya tek başına bir ağ klasörüne MABS kurtarır.

4. İçinde **belirtin Kurtarma Seçenekleri** ekran, Kurtarma tarafından kullanılan bant genişliğini azaltmak için ağ bant genişliği kullanımını azaltma gibi kurtarma seçenekleri seçebilirsiniz. **İleri**’ye tıklayın.

5. İçinde **Özet** ekran, o ana kadarki sağlanan tüm kurtarma yapılandırmaları bakın. Tıklatın **kurtarmak**.

    Kurtarılan veritabanı kurtarma durumunu gösterir. Tıklayabilirsiniz **kapatmak** sihirbazı kapatın ve devam eden görüntülemek için **izleme** çalışma.

    ![Kurtarma işlemini başlatın](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma tamamlandıktan sonra geri yüklenen veritabanı tutarlı uygulamasıdır.

## <a name="next-steps"></a>Sonraki Adımlar

Bkz: [yedekleme dosyaları ve uygulama](backup-mabs-files-applications-azure-stack.md) makalesi.
Bkz: [yedekleme SharePoint Azure yığında](backup-mabs-sharepoint-azure-stack.md) makalesi.
