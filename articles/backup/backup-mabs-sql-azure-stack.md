---
title: Azure Stack üzerinde SQL Server iş yüklerini yedekleme
description: Azure Stack'te SQL Server iş yükünü korumak için Azure Backup sunucusu kullanma.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/8/2018
ms.author: adigan
ms.openlocfilehash: fb064c39fa014515fb2a3f4ccc96ce216f2f7b2e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60254261"
---
# <a name="back-up-sql-server-on-stack"></a>Yığın üzerinde SQL Server'ı yedekleme
Azure Stack'te SQL Server veritabanlarını korumak için Microsoft Azure Backup sunucusu (MABS) yapılandırmak için bu makaleyi kullanın.

SQL Server veritabanı yedeği Azure'a ve azure'dan kurtarma yönetimini üç adımdan oluşur:

1. SQL Server veritabanlarını korumak için bir yedekleme ilkesi oluşturma
2. İsteğe bağlı yedekleme kopyası oluşturma
3. Veritabanını azure'dan ve disklerden kurtarma

## <a name="before-you-start"></a>Başlamadan önce

[Yükleme ve Azure Backup sunucusu hazırlama](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>SQL Server veritabanlarını azure'a korumak için bir yedekleme ilkesi oluşturma
1. Azure Backup sunucusu kullanıcı Arabirimi üzerindeki, tıklayın **koruma** çalışma.

2. Araç şeridinde tıklayın **yeni** yeni bir koruma grubu oluşturma.

    ![Koruma grubu oluşturma](./media/backup-azure-backup-sql/protection-group.png)

    Azure Backup sunucusu başlatır, oluşturmada size yol gösterir. koruma grubu Sihirbazı bir **koruma grubu**. **İleri**’ye tıklayın.

3. İçinde **koruma grubu türünü seçin** ekranında, seçin **sunucuları**.

    ![Koruma grubu türü - 'Sunucuları' seçin](./media/backup-azure-backup-sql/pg-servers.png)

4. İçinde **grup üyelerini seçin** kullanılabilir üyeler listesi ekranında, çeşitli veri kaynaklarını görüntüler. Tıklayın **+** bir klasörü genişletin ve alt klasörleri ortaya çıkarmak için. Öğeyi seçmek için onay kutusuna tıklayın.

    ![SQL DB seçin](./media/backup-azure-backup-sql/pg-databases.png)

    Tüm seçili öğeler seçilen üye listesinde görünür. Korumak istediğiniz veritabanlarını ve sunucuları seçtikten sonra **sonraki**.

5. İçinde **veri koruma yöntemini seçin** ekranında, koruma grubu için bir ad girin ve seçin **çevrimiçi koruma istiyorum** onay kutusu.

    ![Veri koruma yöntemini - kısa süreli disk ve çevrimiçi Azure](./media/backup-azure-backup-sql/pg-name.png)

6. İçinde **kısa vadeli hedefleri belirtin** ekranında, disk ve yedekleme noktaları oluşturmak için gerekli girişleri dahil **sonraki**.

    Örnekte, **bekletme aralığı** olduğu **5 gün**, **eşitleme sıklığı** kez her **15 dakika**, yedekleme olduğu sıklığı. **Hızlı tam yedekleme** ayarlanır **fiyatlara**.

    ![Kısa vadeli hedefleri](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Gösterilen örnekte, önceki günün 8: 00'dan yedekleme noktasından değiştirilen veri aktararak 8: 00'da her gün bir yedekleme noktasının oluşturulur. Bu işlem çağrılırken **hızlı tam yedekleme**. İşlem günlüklerini 15 dakikada bir eşitlenir. 9: 00'da veritabanını kurtarmak gerekiyorsa noktası son günlüklerinden oluşturulduktan hızlı tam yedekleme noktası (Bu örnekte 8 PM).
   >
   >

7. Üzerinde **disk ayırmasını gözden geçirin** ekranında, kullanılabilir depolama alanı ve olası disk alanının doğrulayın. **İleri**’ye tıklayın.

8. İçinde **çoğaltma oluşturma yöntemini seçin**, oluşturma, ilk kurtarma noktasını seçin. Bant genişliği Tıkanıklığı önlemek için el ile (Kapalı ağ) ilk yedeklemeyi aktarabilir veya ağ üzerinden. İlk yedekleme aktarmak için beklenecek seçerseniz, ilk aktarım için süreyi belirtebilirsiniz. **İleri**’ye tıklayın.

    ![İlk çoğaltma yöntemi](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedek kopyanın (SQL Server veritabanı) tüm veri kaynağı için Azure Backup sunucusu (SQL Server makinesi) üretim sunucudan aktarılması gerekir. Bu veri büyük olabilir ve ağ üzerinden veri aktarımı için bant genişliği aşabilir. Bu nedenle, ilk yedekleme aktarmayı seçebilirsiniz: **El ile** (çıkarılabilir medya kullanarak) bant genişliği Tıkanıklığı önlemek veya **ağ üzerinden otomatik olarak** (belirtilen saatte).

    İlk Yedekleme tamamlandıktan sonra yedekleri geri kalanını ilk yedek kopyanın üzerinde artımlı yedeklemeleri olursunuz. Artımlı yedekleme küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.

9. Tutarlılık denetimi çalıştırmak ve istediğiniz zaman seçin **sonraki**.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup sunucusu yedekleme noktası bütünlüğünü üzerinde bir tutarlılık denetimi gerçekleştirir. Azure Backup sunucusu, söz konusu dosya için üretim sunucusunda (Bu senaryoda, SQL Server makinesi) ve yedeklenen veri yedekleme dosyası, sağlama toplamını hesaplar. Bir çakışma varsa, Azure Backup sunucusu üzerinde yedekleme dosyası bozuk olduğu varsayılır. Azure Backup sunucusu, yedeklenen veriler için sağlama toplamı eşleşmezliği karşılık gelen blokları göndererek rectifies. Tutarlılık denetimlerinin yoğun performans olduğundan, tutarlılık denetimi zamanlayabilir veya otomatik olarak çalıştırın.

10. Veri kaynaklarının çevrimiçi korumasını belirtmek için tıklayın ve Azure ile korunacak veritabanlarını seçin **sonraki**.

    ![Veri kaynaklarını seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Yedekleme zamanlamaları ve kuruluşun ilkelerini uygun bekletme ilkelerini seçin.

    ![Zamanlama ve bekletme](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeler günde bir kez 8 PM (ekranın alt bölümünde) ve 12: 00'dan alınmıştır

    > [!NOTE]
    > Diskte, Hızlı Kurtarma için birkaç kısa vadeli kurtarma noktaları için iyi bir uygulamadır. Bu kurtarma noktaları, operasyonel kurtarma için kullanılır. Azure, daha yüksek SLA'ları ile iyi şirket dışı konumu olarak hizmet veren ve kullanılabilirlik garantisi.
    >
    >

    **En iyi yöntem**: Yerel disk yedekleme tamamlandıktan sonra başlatmak için azure'a yedekleme zamanlarsanız, en son disk yedeklemeleri her zaman Azure'a kopyalanır.

12. Bekletme İlkesi zamanlamayı seçin. Bekletme İlkesi birlikte nasıl çalıştığı hakkında ayrıntılar, sağlanan [kullanımı Azure Backup'ın bant altyapısı makale değiştirin](backup-azure-backup-cloud-as-tape.md).

    ![Bekletme İlkesi](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedeklemeler günde bir kez 12: 00'a ve 8 PM (ekranın alt bölümünde) alınır ve 180 gün boyunca saklanır.
    * Cumartesi gece 12: 00'da yedeği 104 hafta boyunca korunur
    * Son 12: 00'da Cumartesi yedeği 60 ay boyunca saklanır
    * 12: 00'da Mart Son Cumartesi yedeği 10 yıl boyunca korunur
13. Tıklayın **sonraki** ve Azure'a ilk yedek kopyanın aktarılması uygun seçeneği belirleyin. Seçebileceğiniz **ağ üzerinden otomatik olarak**

14. İlke ayrıntıları gözden geçirin, sonra **özeti** ekranında **Grup Oluştur** iş akışının tamamlanması. Tıklayabilirsiniz **Kapat** ve izleme çalışma alanı, işin ilerleme durumunu izleyin.

    ![Devam eden için koruma grubu oluşturma](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>İsteğe bağlı yedekleme SQL Server veritabanı
Önceki adımları bir yedekleme İlkesi oluşturuldu, ancak bir "kurtarma noktası" yalnızca ilk yedekleme gerçekleştiğinde oluşturulur. Zamanlayıcı etkisini göstermeye beklemek yerine, tetikleyici kurtarma oluşturulması aşağıdaki adımları el ile işaretleyin.

1. Koruma grubunun durumu gösterilene kadar bekleyin **Tamam** kurtarma noktası oluşturmadan önce veritabanı için.

    ![Koruma grubu üyeleri](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Sağ tıklatın ve veritabanı **kurtarma noktası oluştur**.

    ![Çevrimiçi kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Seçin **çevrimiçi koruma** öğelerine tıklayıp **Tamam** Azure'da kurtarma noktası oluşturma işlemini başlatmak için.

    ![Kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. İş ilerleme durumunu görüntüleme **izleme** çalışma.

    ![İzleme konsolu](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>SQL Server veritabanını Azure'dan kurtarma
Aşağıdaki adımlar, bir Korunan varlık (SQL Server veritabanı) Azure'dan kurtarmak için gereklidir.

1. Azure Backup sunucusu Yönetim Konsolu'nu açın. Gidin **kurtarma** korumalı sunuculardaki görebileceğiniz çalışma. Gerekli veritabanında (Bu durum ReportServer$ MSDPM2012) göz atın. Seçin bir **kurtarma** olarak belirtilen zaman bir **çevrimiçi** gelin.

    ![Kurtarma noktası seçin](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Veritabanı adını sağ tıklatıp **kurtarmak**.

    ![Azure'dan kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS, kurtarma noktası ayrıntılarını gösterir. **İleri**’ye tıklayın. Veritabanının üzerine yazmak için kurtarma türünü seçin. **özgün SQL Server örneğine Kurtar**. **İleri**’ye tıklayın.

    ![Özgün konuma Kurtar](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte, başka bir SQL Server örneğine veya tek başına bir ağ klasörüne veritabanı MABS kurtarır.

4. İçinde **belirtin kurtarma seçeneklerini** ekran recovery tarafından kullanılan bant genişliğini azaltmak için ağ bant genişliği kullanımını azaltma gibi kurtarma seçeneklerini seçebilirsiniz. **İleri**’ye tıklayın.

5. İçinde **özeti** ekran, şu ana kadar verilen tüm kurtarma yapılandırmaları görürsünüz. Tıklayın **kurtarmak**.

    Kurtarılan veritabanı kurtarma durumunu gösterir. Tıklayabilirsiniz **kapatmak** sihirbazı kapatın ve ilerleme durumunu görüntülemek için **izleme** çalışma.

    ![Kurtarma işlemini başlatın](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma tamamlandıktan sonra geri yüklenen veritabanının tutarlı uygulamasıdır.

## <a name="next-steps"></a>Sonraki Adımlar

Bkz: [yedekleme dosyaları ve uygulama](backup-mabs-files-applications-azure-stack.md) makalesi.
Bkz: [Azure Stack yedekleme SharePoint'te](backup-mabs-sharepoint-azure-stack.md) makalesi.
