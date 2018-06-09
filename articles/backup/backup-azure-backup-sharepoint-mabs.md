---
title: SharePoint grubunun kurulumu Azure'a yedeklemek için Azure yedekleme sunucusu kullan
description: Azure yedekleme sunucusu ve SharePoint verilerini geri yükleme için kullanın. Bu makalede, böylece istenen verileri Azure'da depolanabilir, SharePoint grubunu yapılandırmak için bilgi sağlar. Korumalı SharePoint verileri diskten veya Azure geri yükleyebilirsiniz.
services: backup
author: pvrk
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/8/2018
ms.author: pullabhk
ms.openlocfilehash: 4dff27d8ef7357e5af3635cc39fb52963689e7bb
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247974"
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a>Bir SharePoint grubunu Azure’a yedekleme
SharePoint grubunun kurulumu çok diğer veri kaynaklarını geri aynı şekilde Microsoft Azure yedekleme sunucusu (MABS) kullanarak Microsoft Azure'a yedekleyin. Azure yedekleme günlük oluşturmak için yedekleme zamanlamasını esneklik sağlar, haftalık, aylık veya yıllık yedekleme noktaları ve çeşitli yedekleme noktaları için bekletme ilkesi seçenekler sunar. Ayrıca, Hızlı Kurtarma süresi hedefi (RTO) için yerel disk kopyaları depolamak ve ekonomik, uzun vadeli bekletme için Azure kopyaları depolamak için yeteneği sağlar.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint desteklenen sürümleri ve ilgili koruma senaryoları
Azure yedekleme DPM için aşağıdaki senaryoları destekler:

| İş yükü | Sürüm | SharePoint dağıtımı | Koruma ve kurtarma |
| --- | --- | --- | --- | --- | --- |
| SharePoint |SharePoint 2013, SharePoint 2010, SharePoint 2007'de, SharePoint 3.0 |Bir fiziksel sunucu veya Hyper-V/VMware sanal makinesi dağıtılan SharePoint <br> -------------- <br> SQL AlwaysOn | SharePoint grubu kurtarma seçeneklerini koruma: Kurtarma grubu, veritabanı ve disk kurtarma noktalarından dosya veya liste öğesi.  Azure kurtarma noktalarından grubu ve veritabanı kurtarma. |

## <a name="before-you-start"></a>Başlamadan önce
Bir SharePoint grubuna Azure yedekleme önce onaylamak için gereken birkaç nokta vardır.

### <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce bilgisayarınızda yüklü olduğundan emin olun [yüklü ve Azure yedekleme sunucusu hazırlanan](backup-azure-microsoft-azure-backup.md) iş yüklerini korumak için.

### <a name="protection-agent"></a>Koruma Aracısı
Azure Backup aracısını, SharePoint, SQL Server çalıştıran sunucular ve SharePoint grubunun parçası olan diğer tüm sunucuları çalıştıran sunucuda yüklenmesi gerekir. Koruma aracısını ayarlama hakkında daha fazla bilgi için bkz: [Kurulum koruma Aracısı](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  Aracı yalnızca bir tek web front end (WFE) sunucusuna yükleyin istisnadır. Azure yedekleme sunucusu yalnızca koruma için giriş noktası olarak hizmet verecek bir WFE sunucusunda aracının gerekir.

### <a name="sharepoint-farm"></a>SharePoint grubu
Gruptaki her 10 milyon öğe için en az 2 GB alan MABS klasörünün bulunduğu birimde olmalıdır. Bu alanı katalog oluşturma için gereklidir. Belirli öğeleri (site koleksiyonları, siteler, listeler, belge kitaplıkları, klasörler, tek tek belgeler ve liste öğeleri) kurtarma MABS için katalog oluşturma her içerik veritabanında bulunan URL'lerin bir listesini oluşturur. Kurtarılabilir öğe bölmesinde URL'lerin listesini görüntüleyebilirsiniz **kurtarma** MABS Yönetici Konsolu'nun alanı görev.

### <a name="sql-server"></a>SQL Server
Azure yedekleme sunucusu LocalSystem hesabı olarak çalışır. SQL Server veritabanlarını yedeklemek için SQL Server çalıştıran sunucu için bu hesabı üzerinde Sistem Yöneticisi ayrıcalıklarına MABS gerekir. Kümesine NT AUTHORITY\SYSTEM *sysadmin* önce SQL Server çalıştıran sunucuda yedekleyin.

SharePoint grubu SQL Server diğer adlarıyla yapılandırılmış SQL Server veritabanlarını ise MABS korunacak ön uç Web sunucusunda SQL Server istemci bileşenlerini yükleyin.

### <a name="sharepoint-server"></a>SharePoint Server
Performans SharePoint grubu boyutu gibi birçok faktöre bağlıdır, ancak genel bir yönerge olarak 25 TB SharePoint grubunu bir MABS koruyabilirsiniz.

### <a name="whats-not-supported"></a>Desteklenmeyen durumlar
* Bir SharePoint grubu korur MABS, arama dizinlerini veya uygulama hizmeti veritabanlarını koruma sağlamaz. Bu veritabanlarını koruma ayrı olarak yapılandırmanız gerekir.
* MABS, genişleme dosya sunucusu (SOFS) paylaşımları üzerinde barındırılan SharePoint SQL Server veritabanlarını yedekleme sağlamaz.

## <a name="configure-sharepoint-protection"></a>SharePoint korumasını yapılandırma
SharePoint'i koruma için MABS kullanmadan önce kullanarak SharePoint VSS yazıcısı hizmetini (WSS yazıcısı hizmeti) yapılandırmanız gerekir **ConfigureSharePoint.exe**.

Bulabileceğiniz **ConfigureSharePoint.exe** [MABS yükleme yolu] \bin klasöründe ön uç web sunucusunda. Bu araç SharePoint grubu için kimlik bilgileriyle koruma aracısını sağlar. Bu tek bir WFE sunucusunda çalıştırın. Birden çok WFE sunucunuz varsa, bir koruma grubu yapılandırırken yalnızca bir tanesini seçin.

### <a name="to-configure-the-sharepoint-vss-writer-service"></a>SharePoint VSS yazıcısı hizmetini yapılandırmak için
1. WFE sunucusundaki bir komut isteminde [MABS yükleme konumu] \bin\ konumuna gidin.
2. ConfigureSharePoint - EnableSharePointProtection girin.
3. Grup Yöneticisi kimlik bilgilerini girin. Bu hesap, WFE sunucusunda yerel yönetici grubunun bir üyesi olmalıdır. Grup Yöneticisi yerel değilse yönetici WFE sunucusunda aşağıdaki izinleri verin:
   * WSS_Admin_WPG grubu tam denetimini DPM klasörünü (% Program Files%\Microsoft Azure Backup\DPM) verin.
   * WSS_Admin_WPG grubu okuma erişimini (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager) DPM kayıt defteri anahtarına verin.

> [!NOTE]
> SharePoint grubu yönetici kimlik bilgilerinde bir değişiklik olduğunda ConfigureSharePoint.exe yeniden çalıştırmanız gerekir.
>
>

## <a name="back-up-a-sharepoint-farm-by-using-mabs"></a>SharePoint grubunun kurulumu MABS kullanarak yedekleyin
MABS ve daha önce açıklandığı gibi SharePoint grubu yapılandırdıktan sonra bu SharePoint MABS tarafından korunur.

### <a name="to-protect-a-sharepoint-farm"></a>Bir SharePoint grubunu korumak için
1. Gelen **koruma** sekmesini MABS Yönetici Konsolu **yeni**.
    ![Yeni koruma sekmesi](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. Üzerinde **koruma grubu türünü seçin** sayfasında **yeni koruma grubu oluşturma** seçin **sunucuları**ve ardından **sonraki**.

    ![Koruma grubu seç türü](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. Üzerinde **grup üyelerini seçin** ekranında, önce korumak istediğiniz SharePoint server için onay kutusunu işaretleyin **sonraki**.

    ![Grup üyelerini seçin](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > Koruma aracısı yüklü Sunucu Sihirbazı'nda görebilirsiniz. MABS da yapısını gösterir. ConfigureSharePoint.exe çalıştırdığınız için MABS karşılık gelen SQL Server veritabanlarını ve SharePoint VSS Yazıcı hizmeti ile iletişim kurar ve SharePoint grup yapısı, ilişkili içerik veritabanları ve karşılık gelen tüm öğeleri tanır.
   >
   >
4. Üzerinde **veri koruma yöntemini seçin** sayfasında, adını girin **koruma grubu**ve tercih ettiğiniz seçin *koruma yöntemleri*. **İleri**’ye tıklayın.

    ![Veri koruma yöntemini seçin](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > Disk koruması yöntemi kısa kurtarma zamanı hedeflerine yardımcı olur.
   >
   >
5. Üzerinde **kısa vadeli hedefleri belirtin** sayfasında, tercih ettiğiniz **bekletme aralığı** ve gerçekleşmesini istediğinizde belirleyin.

    ![Kısa vadeli hedefleri belirtin](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > Kurtarma genellikle beş günden eski olan verileri için gerekli olduğundan, biz diskteki beş günlük bir bekletme aralığı seçili ve yedekleme Bu örnek için üretim dışı saatlerde olur güvence altına.
   >
   >
6. Depolama havuzuna disk alanı koruma grubu için ayrılmış ve ardından gözden geçirme **sonraki**.
7. Her koruma grubu için MABS depolamak ve çoğaltmaları yönetmek için disk alanı ayırır. Bu noktada, MABS seçili verilerin bir kopyasını oluşturmanız gerekir. Nasıl ve ne zaman, oluşturulan çoğaltma istediğiniz'ı seçin ve **sonraki**.

    ![Çoğaltma oluşturma yöntemini seçin](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > Ağ trafiği değil parametreden emin olmak için üretim saatleri dışında bir saat seçin.
   >
   >
8. MABS, çoğaltma üzerinde tutarlılık denetimleri gerçekleştirerek veri bütünlüğü sağlar. Kullanılabilir iki seçenek vardır. Tutarlılık denetimleri çalıştırmak için bir zamanlama tanımlayın veya tutarsız hale her DPM otomatik olarak çoğaltma üzerinde tutarlılık denetimleri çalıştırabilirsiniz. Tercih ettiğiniz seçeneği seçin ve ardından **sonraki**.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. Üzerinde **çevrimiçi koruma verilerini belirtin** sayfasında, koruma ve ardından istediğiniz SharePoint grubu seçin **sonraki**.

    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, tercih ettiğiniz zamanlamayı seçin ve ardından **sonraki**.

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > MABS en fazla iki günlük yedeklemeler için Azure sonra kullanılabilir en son disk yedekleme noktasından sağlar. Azure yedekleme de en yüksek ve saatlere yedeklemelerin kullanarak kullanılabilir WAN bant genişliği miktarını kontrol [Azure yedekleme ağ azaltma](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).
    >
    >
11. Üzerinde seçili yedekleme zamanlaması bağlı olarak **çevrimiçi bekletme ilkesini belirtin** sayfasında, günlük, haftalık, aylık ve yıllık yedekleme noktası bekletme ilkesi seçin.

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > MABS farklı yedekleme noktaları için farklı bir bekletme ilkesi seçilebilir bir üst öğe son bekletme düzeni kullanır.
    >
    >
12. Disk benzeyen, bir ilk başvuru noktası çoğaltmasını Azure içinde oluşturulması gerekir. Azure bir ilk yedek kopyasını oluşturun ve ardından tercih ettiğiniz seçeneği seçin **sonraki**.

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Seçtiğiniz ayarları gözden **Özet** sayfasında ve ardından **Grup Oluştur**. Koruma grubu oluşturulduktan sonra bir başarı iletisi görürsünüz.

    ![Özet](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-mabs"></a>Bir SharePoint öğesi MABS kullanarak diskten geri yükleme
Aşağıdaki örnekte, *SharePoint kurtarma öğesi* yanlışlıkla silinmişse ve geri yüklenmesi gerekiyor.
![MABS SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Açık **DPM Yönetici Konsolu**. DPM tarafından korunan tüm SharePoint grupları gösterilen **koruma** sekmesi.

    ![MABS SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. Öğeyi kurtarmak başlamak için seçin **kurtarma** sekmesi.

    ![MABS SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. SharePoint için arama yapabilirsiniz *SharePoint kurtarma öğesi* kurtarma içinde bir joker karakter tabanlı arama kullanarak aralığı gösterin.

    ![MABS SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Arama sonuçlarından uygun Kurtarma noktasını seçin, öğeyi sağ tıklatın ve ardından **kurtarmak**.
5. Ayrıca, çeşitli kurtarma noktalarına göz ve bir veritabanı veya kurtarmak için öğeyi seçin. Seçin **tarihi > kurtarma süresini**ve ardından doğru seçin **veritabanı > SharePoint grubu > kurtarma noktası > öğesi**.

    ![MABS SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Öğeyi sağ tıklatın ve ardından **kurtarmak** açmak için **Kurtarma Sihirbazı'nı**. **İleri**’ye tıklayın.

    ![Kurtarma seçimini inceleyin](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Gerçekleştirin ve ardından istediğiniz kurtarma türünü seçin **sonraki**.

    ![Kurtarma türü](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > Seçimi **özgün kurtarmak** örnekte özgün SharePoint sitesi öğesine kurtarır.
   >
   >
8. Seçin **kurtarma işlemi** kullanmak istediğiniz.

   * Seçin **kurtarma grubu kullanmadan kurtarmak** SharePoint grubu değişmedi ve geri yükleniyor kurtarma noktası ile aynıdır.
   * Seçin **kurtarma grubu kullanarak kurtarma** kurtarma noktası oluşturulduktan sonra SharePoint grubu değiştirilmesi durumunda.

     ![Kurtarma işlemi](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Veritabanı geçici olarak kurtarmak için hazırlama bir SQL Server örneğinin konumu sağlayın ve MABS ve öğeyi kurtarmak için SharePoint çalıştıran sunucu hazırlama bir dosya paylaşımı sağlayın.

    ![Hazırlama Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    Geçici SQL Server örneği için SharePoint öğe barındırma içerik veritabanını MABS ekler. İçerik veritabanından öğe kurtarır ve MABS hazırlama dosyası konumunda bulunan koyar. Hazırlama konumuna sunulmuştur Kurtarılan bir öğeye SharePoint grubu üzerinde hazırlama konumu için aktarılması gerekir.

    ![Hazırlama Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Seçin **kurtarma seçeneklerini belirtin**, SharePoint grubu için güvenlik ayarlarını Uygula ve kurtarma noktası güvenlik ayarlarını uygula. **İleri**’ye tıklayın.

    ![Kurtarma Seçenekleri](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > Ağ bant genişliği kullanımını azaltma seçebilirsiniz. Bu, üretim saatleri içinde üretim sunucusu üzerinde etkisi azaltır.
    >
    >
11. Özet bilgilerini inceleyin ve ardından **kurtarmak** dosyasının kurtarmayı başlatmak için.

    ![Kurtarma özeti](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Şimdi seçin **izleme** sekmesinde **MABS Yönetici Konsolu** görüntülemek için **durum** kurtarma.

    ![Kurtarma durumu](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > Dosya artık geri yüklenir. Geri yüklenen dosya denetlemek için SharePoint sitesi yenileyebilirsiniz.
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>DPM kullanarak bir SharePoint veritabanı Azure'dan geri yükleme
1. Bir SharePoint içerik veritabanını kurtarmak için (daha önce gösterildiği gibi) çeşitli kurtarma noktalarına göz ve geri yüklemek istediğiniz kurtarma noktasını seçin.

    ![MABS SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. SharePoint kurtarma noktası kullanılabilir SharePoint katalog bilgileri görüntülemek için çift tıklayın.

   > [!NOTE]
   > SharePoint grubu Azure uzun vadeli bekletme için korumalı olduğundan, hiçbir Kataloğu bilgi (meta veriler) MABS üzerinde kullanılabilir. Sonuç olarak, bir zaman içinde nokta SharePoint içerik veritabanı kurtarılması gereken her SharePoint grubu yeniden katalog gerekir.
   >
   >
3. Tıklatın **yeniden kataloglama**.

    ![MABS SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    **Bulut yeniden katalogla** durum penceresi açar.

    ![MABS SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Katalog tamamlandıktan sonra durum değişikliklerini *başarı*. **Kapat**’a tıklayın.

    ![MABS SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. MABS gösterilen SharePoint nesnesine tıklayın **kurtarma** içerik veritabanı yapısı almak için sekmesini. Öğeyi sağ tıklatın ve ardından **kurtarmak**.

    ![MABS SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Bu noktada, izleyin [kurtarma adımları bu makalenin önceki bölümlerinde](#restore-a-sharepoint-item-from-disk-using-dpm) diskten bir SharePoint içerik veritabanını kurtarmak için.

## <a name="faqs"></a>SSS
S: özgün konuma SharePoint öğesi, SharePoint SQL AlwaysOn (disk koruması) kullanılarak yapılandırılmışsa kurtarma?<br>
A: Evet öğenin özgün SharePoint sitesine kurtarılabilir.

S: bir SharePoint veritabanını özgün konumuna, SharePoint SQL AlwaysOn kullanarak yapılandırılmışsa kurtarma?<br>
A: SharePoint veritabanlarını SQL AlwaysOn yapılandırıldığından, kullanılabilirlik grubu kaldırılmadığı sürece bunlar değiştirilemez. Sonuç olarak, MABS bir veritabanını özgün konumuna geri yükleyemezsiniz. SQL Server veritabanını başka bir SQL Server örneğine kurtarabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Bkz: [Exchange sunucusu yedekleme](backup-azure-exchange-mabs.md) makalesi.
Bkz: [SQL Server Yedekleme](backup-azure-sql-mabs.md) makalesi.
