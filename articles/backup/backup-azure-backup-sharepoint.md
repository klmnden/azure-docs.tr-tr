---
title: Bir SharePoint grubunu Azure'da DPM/Azure Backup sunucusu koruma
description: Bu makalede Azure'da DPM/Azure Backup sunucusu koruma SharePoint grubunun genel bir bakış sağlar.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: kasinh
ms.openlocfilehash: 7e8043badbc0accd38ad618a7d455729ab6606b2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60644344"
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a>Bir SharePoint grubunu Azure’a yedekleme
Microsoft Azure'a çok diğer veri kaynaklarını yedekleme aynı şekilde, System Center Data Protection Manager (DPM) kullanarak bir SharePoint grubunu yedekleme. Azure Backup, yedekleme zamanlaması günlük oluşturmak için esneklik sağlar, haftalık, aylık veya yıllık yedekleme işaret ve çeşitli yedekleme noktaları için bekletme ilkesi seçenekleri sunar. DPM, Hızlı Kurtarma süresi hedeflerini (RTO) için yerel disk kopyaları depolamak ve ekonomik, uzun süreli saklama için azure'a kopyaları depolamak için yeteneği sağlar.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint desteklenen sürümleri ve ilgili koruma senaryoları
Azure yedekleme DPM için aşağıdaki senaryoları destekler:

| İş yükü | Version | SharePoint dağıtımı | DPM dağıtım türü | DPM - System Center 2012 R2 | Koruma ve kurtarma |
| --- | --- | --- | --- | --- | --- |
| SharePoint |SharePoint 2013, SharePoint 2010, SharePoint 2007'de, SharePoint 3.0 |Fiziksel sunucu veya Hyper-V/VMware sanal makinesi olarak dağıtılan SharePoint <br> -------------- <br> SQL AlwaysOn |Fiziksel sunucu veya şirket içi Hyper-V sanal makinesi |Azure'da desteklediği yedekleme Güncelleştirme Paketi 5 |Kurtarma seçeneklerini SharePoint grubunu koruma: Kurtarma grubu, veritabanı ve disk kurtarma noktalarından dosya veya liste öğesi.  Azure kurtarma noktalarından kurtarma grubu ve veritabanı. |

## <a name="before-you-start"></a>Başlamadan önce
Bir SharePoint grubunu Azure'da yedekleme önce onaylamak için gereken birkaç nokta vardır.

### <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce tüm karşıladığınızdan emin olun [Microsoft Azure Backup'ı kullanma önkoşulları](backup-azure-dpm-introduction.md#prerequisites-and-limitations) iş yüklerini korumak için. Önkoşullar için bazı görevler aşağıdakileri içerir: bir yedekleme kasası oluşturun, kasa kimlik bilgilerini indirin, Azure Backup aracısını yükleyin ve DPM/Azure Backup sunucusu, kasa ile kaydeder.

### <a name="dpm-agent"></a>DPM Aracısı
DPM Aracısı, SharePoint, SQL Server çalıştıran sunuculara ve SharePoint grubunun parçası olan diğer tüm sunucular çalıştıran sunucuya yüklenmelidir. Koruma aracısını ayarlama hakkında daha fazla bilgi için bkz: [Kurulum koruma aracısını](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  Yalnızca bir tek bir web ön uç (WFE) sunucusuna aracı yükleme istisnadır. DPM, aracı yalnızca, koruma için giriş noktası olarak hizmet verecek bir WFE sunucusunda gerektirir.

### <a name="sharepoint-farm"></a>SharePoint grubu
Gruptaki her 10 milyon öğe için DPM klasörünün bulunduğu birimde alan en az 2 GB olmalıdır. Bu boşluk katalog oluşturma için gereklidir. (Site koleksiyonları, siteler, listeler, belge kitaplıkları, klasörler, tek tek belgeler ve liste öğeleri) belirli öğeleri kurtarmak DPM için katalog oluşturma, her bir içerik veritabanında bulunan URL'lerin bir listesini oluşturur. URL'lerin listesini kurtarılabilir öğe bölmesinde görüntüleyebilirsiniz **kurtarma** görev alanındaki DPM Yönetici Konsolu'nun.

### <a name="sql-server"></a>SQL Server
DPM, bir LocalSystem hesabı olarak çalışır. SQL Server veritabanlarını yedeklemek için DPM, SQL Server'ı çalıştıran sunucu için o hesapta sysadmin ayrıcalıkları gerektirir. Kümesine NT AUTHORITY\SYSTEM *sysadmin* önce SQL Server çalıştıran sunucuda yedekleyin.

SharePoint grubu SQL Server diğer adlarıyla yapılandırılmış SQL Server veritabanlarını varsa, DPM tarafından korunacak ön uç Web sunucusunda SQL Server istemci bileşenlerini yükleyin.

### <a name="sharepoint-server"></a>SharePoint Server
Performans, SharePoint grubu boyutu gibi birçok faktöre bağlıdır, ancak genel rehberlik sağlaması 25 TB SharePoint grubunu bir DPM sunucusu koruyabilir.

### <a name="dpm-update-rollup-5"></a>DPM Güncelleştirme Paketi 5
Bir SharePoint grubunu Azure'da koruma başlamak için DPM Güncelleştirme Paketi 5 veya sonraki bir sürümü yüklemeniz gerekir. Güncelleştirme Paketi 5 SQL Alwayson'u kullanarak Grup yapılandırılmışsa, bir SharePoint grubunu Azure'da koruma olanağı sağlar.
Daha fazla bilgi için bkz. blog tanıtan post [DPM Güncelleştirme Paketi 5](https://blogs.technet.com/b/dpm/archive/2015/02/11/update-rollup-5-for-system-center-2012-r2-data-protection-manager-is-now-available.aspx)

### <a name="whats-not-supported"></a>Desteklenmeyen durumlar
* Arama dizinleri veya uygulama hizmeti veritabanları bir SharePoint çiftliğini koruyan DPM korumaz. Bu veritabanlarının korunmasını ayrı yapılandırmanız gerekir.
* DPM, genişleme dosya sunucusu (SOFS) paylaşımları üzerinde barındırılan SharePoint SQL Server veritabanlarının yedekleme sağlamaz.

## <a name="configure-sharepoint-protection"></a>SharePoint korumasını yapılandırma
SharePoint korumak için DPM'yi kullanabilmeniz için önce kullanarak SharePoint VSS Yazıcı hizmetini (WSS yazıcı hizmeti) yapılandırmanız gerekir **ConfigureSharePoint.exe**.

Bulabilirsiniz **ConfigureSharePoint.exe** [DPM yükleme yolu] \bin klasöründe ön uç web sunucusunda. Bu araç SharePoint grubu için kimlik bilgileriyle koruma aracısını sağlar. Bunu tek bir WFE sunucusunda çalıştırın. Birden çok WFE sunucunuz varsa, bir koruma grubu yapılandırırken yalnızca bir tanesini seçin.

### <a name="to-configure-the-sharepoint-vss-writer-service"></a>SharePoint VSS Yazıcı hizmetini yapılandırmak için
1. WFE sunucusundaki bir komut isteminde [DPM yükleme konumu] \bin\ konumuna gidin.
2. ConfigureSharePoint - EnableSharePointProtection girin.
3. Grup Yöneticisi kimlik bilgilerini girin. Bu hesap, WFE sunucusunda yerel yönetici grubunun bir üyesi olmalıdır. Grup Yöneticisi yerel bir değilse Yöneticisi WFE sunucusunda aşağıdaki izinleri verin:
   * WSS_Admin_WPG grubu tam denetimini DPM klasörüne (% Program Files%\Microsoft Data Protection Manager\DPM) verme.
   * WSS_Admin_WPG grubu okuma erişimini DPM kayıt defteri anahtarı (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager) verme.

> [!NOTE]
> SharePoint grubu yönetici kimlik bilgilerinde bir değişiklik olduğunda, ConfigureSharePoint.exe yeniden çalıştırmanız gerekir.
>
>

## <a name="back-up-a-sharepoint-farm-by-using-dpm"></a>DPM kullanarak bir SharePoint grubunu yedekleme
DPM ve daha önce açıklandığı gibi SharePoint grubu yapılandırdıktan sonra SharePoint, DPM tarafından korunabilir.

### <a name="to-protect-a-sharepoint-farm"></a>Bir SharePoint çiftliğini korumaya
1. Gelen **koruma** sekmesinde DPM Yönetici Konsolu'nun tıklayın **yeni**.
    ![Yeni koruma sekmesi](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. Üzerinde **koruma grubu türünü seçin** sayfasının **yeni koruma grubu oluşturma** seçin **sunucuları**ve ardından **sonraki**.

    ![Koruma grubu seç türü](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. Üzerinde **grup üyelerini seçin** ekranında, SharePoint Server'ı tıklatın ve korumak istediğiniz onay kutusunu işaretleyin **sonraki**.

    ![Grup üyelerini seçin](./media/backup-azure-backup-sharepoint/select-group-members2.png)

   > [!NOTE]
   > DPM aracısının yüklü olduğu sunucunun sihirbazdaki görebilirsiniz. DPM, aynı zamanda yapısını gösterir. ConfigureSharePoint.exe çalıştığından, DPM SharePoint VSS Yazıcı hizmetini ve ilgili SQL Server veritabanlarını ile iletişim kurar ve SharePoint grup yapısı, ilişkili içerik veritabanları ve karşılık gelen öğeleri algılar.
   >
   >
4. Üzerinde **veri koruma yöntemini seçin** sayfasını, adını **koruma grubu**ve tercih ettiğiniz *koruma yöntemleri*. **İleri**’ye tıklayın.

    ![Veri koruma yöntemini seçin](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)

   > [!NOTE]
   > Disk koruması yöntemi kısa kurtarma süresi hedeflerini karşılamak için yardımcı olur. Azure için bantları karşılaştırıldığında, ekonomik, uzun vadeli koruma bir hedeftir. Daha fazla bilgi için [kullanımı Azure Backup kullanarak bant altyapınızı değiştirme](https://azure.microsoft.com/documentation/articles/backup-azure-backup-cloud-as-tape/)
   >
   >
5. Üzerinde **kısa vadeli hedefleri belirtin** sayfasında, tercih ettiğiniz **bekletme aralığı** ve gerçekleşmesini istediğinizde tanımlayın.

    ![Kısa vadeli hedefleri belirtin](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)

   > [!NOTE]
   > Kurtarma genellikle beş günden eski olan veriler için gerekli olduğundan, biz diskte beş günlük bir bekletme aralığı seçili ve yedekleme Bu örnek için üretim dışı saatlerde yapıldığından emin oldunuz.
   >
   >
6. Depolama havuzuna disk alanı koruma grubu için ayrılan ve ardından gözden geçirme **sonraki**.
7. Her koruma grubu için DPM çoğaltmaları yönetmek için disk alanı ayırır. Bu noktada, DPM, seçilen verilerin bir kopyasını oluşturmanız gerekir. Nasıl ve ne zaman, istediğiniz oluşturulan çoğaltma, seçin ve ardından **sonraki**.

    ![Çoğaltma oluşturma yöntemini seçin](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)

   > [!NOTE]
   > Ağ trafiğini etkilenmez emin olmak için üretim saatleri dışında bir saat seçin.
   >
   >
8. DPM, çoğaltma üzerinde tutarlılık denetimleri uygulayarak veri bütünlüğü sağlar. Kullanılabilir iki seçenek vardır. Tutarlılık denetimleri çalıştırmak için bir zamanlama tanımlayabilir veya tutarsız hale duyduğunuzda DPM tutarlılık denetimleri otomatik olarak çoğaltma gerçekleştirebilirsiniz. Tercih ettiğiniz seçeneği seçin ve ardından **sonraki**.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. Üzerinde **çevrimiçi koruma verilerini belirtin** sayfasında, korumak ve ardından istediğiniz SharePoint grubu **sonraki**.

    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfasında tercih ettiğiniz zamanlamayı seçin ve ardından **sonraki**.

    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)

    > [!NOTE]
    > DPM, farklı zamanlarda Azure'a en fazla iki günlük yedek sağlar. Azure Backup ayrıca kullanılabilecek yedeklere yoğun olan ve yoğun olmayan saatler için kullanarak WAN bant genişliği denetimini [Azure yedekleme, ağ kapasitesi azaltma](https://azure.microsoft.com/documentation/articles/backup-configure-vault/#enable-network-throttling).
    >
    >
11. Üzerinde seçili yedekleme zamanlamasına bağlı olarak **çevrimiçi saklama ilkesini belirtin** sayfasında, günlük, haftalık, aylık ve yıllık yedekleme noktası bekletme ilkesini seçin.

    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)

    > [!NOTE]
    > DPM, farklı bir yedekleme noktaları için farklı bir bekletme ilkesi seçilebilir bir dedenizin bırak son bekletme düzeni kullanır.
    >
    >
12. Disk benzeyen bir ilk başvuru noktası çoğaltması Azure'da oluşturulması gerekir. Azure'a ilk bir yedek kopya oluşturmak ve ardından tercih ettiğiniz seçeneği seçin **sonraki**.

    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Seçili ayarlarınızı gözden **özeti** sayfasında ve ardından **Grup Oluştur**. Koruma grubu oluşturulduktan sonra bir başarı iletisi görürsünüz.

    ![Özet](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-dpm"></a>DPM kullanarak bir SharePoint öğesi diskten geri yükleme
Aşağıdaki örnekte, *SharePoint kurtarma öğesi* yanlışlıkla silinmişse ve geri yüklenmesi gerekiyor.
![DPM SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Açık **DPM Yönetici Konsolu**. DPM tarafından korunan tüm SharePoint grupları gösterilen **koruma** sekmesi.

    ![DPM SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. Öğeyi kurtarmak başlamak için seçin **kurtarma** sekmesi.

    ![DPM SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. SharePoint için arama yapabilirsiniz *SharePoint kurtarma öğesi* kurtarma içinde bir joker karakter tabanlı arama kullanarak noktası aralığı.

    ![DPM SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Arama sonuçlarından uygun Kurtarma noktasını seçin, öğeye sağ tıklayın ve ardından **kurtarmak**.
5. Ayrıca, çeşitli kurtarma noktalarına göz atın ve bir veritabanı veya kurtarmak için öğeyi seçin. Seçin **tarih > Kurtarma zamanı**ve ardından doğru **veritabanı > SharePoint grubu > kurtarma noktası > öğesi**.

    ![DPM SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Öğeye sağ tıklayın ve ardından **kurtarmak** açmak için **Kurtarma Sihirbazı'nı**. **İleri**’ye tıklayın.

    ![Kurtarma seçimini inceleyin](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Gerçekleştirin ve ardından istediğiniz kurtarma türünü seçin **sonraki**.

    ![Kurtarma türü](./media/backup-azure-backup-sharepoint/select-recovery-type.png)

   > [!NOTE]
   > Seçimi **özgün kurtarmak** örnekte öğe özgün SharePoint sitesine kurtarır.
   >
   >
8. Seçin **kurtarma işlemi** kullanmak istediğiniz.

   * Seçin **kurtarma grubu kullanmadan kurtarmak** SharePoint grubu değişmedi ve geri yüklenen bir kurtarma noktası ile aynıdır.
   * Seçin **kurtarma grubu kullanarak kurtarma** SharePoint grubu kurtarma noktasının oluşturulmasından bu yana değişmişse.

     ![Kurtarma işlemi](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Geçici olarak veritabanını kurtarmak için bir hazırlama SQL Server örneği konumu belirtin ve DPM sunucusu ve öğeyi kurtarmak için SharePoint çalıştıran sunucu üzerinde hazırlama bir dosya paylaşımı sağlayın.

    ![Hazırlama Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)

    DPM içerik veritabanını geçici SQL Server örneği için SharePoint öğesini barındıran ekler. İçerik veritabanından DPM sunucusuna öğesi kurtarır ve DPM sunucusundaki hazırlama dosya konumunda koyar. DPM sunucusunun hazırlama konumuna sunulmuştur Kurtarılan bir öğeye SharePoint grubu hazırlama konumuna verilmesi gerekir.

    ![Hazırlama Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Seçin **kurtarma seçeneklerini belirtin**, SharePoint grubu için güvenlik ayarlarını Uygula ve kurtarma noktasının güvenlik ayarlarını uygula. **İleri**’ye tıklayın.

    ![Kurtarma Seçenekleri](./media/backup-azure-backup-sharepoint/recovery-options.png)

    > [!NOTE]
    > Ağ bant genişliği kullanımını azaltma seçebilirsiniz. Bu, üretim saatleri içinde üretim sunucusunu bir etki azaltır.
    >
    >
11. Özet bilgilerini gözden geçirin ve ardından **kurtarmak** dosyanın kurtarmayı başlatmak için.

    ![Kurtarma özeti](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Şimdi seçtiğiniz **izleme** sekmesinde **DPM Yönetici Konsolu'nu** görüntülemek için **durumu** kurtarma.

    ![Kurtarma durumu](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)

    > [!NOTE]
    > Dosya geri yüklenir. Geri yüklenen dosya denetlemek için SharePoint sitesi yenileyebilirsiniz.
    >
    >

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>DPM kullanarak bir SharePoint veritabanını Azure'dan geri yükleme
1. Bir SharePoint içerik veritabanını kurtarmak için (daha önce gösterildiği gibi) çeşitli kurtarma noktalarına göz ve geri yüklemek istediğiniz kurtarma noktasını seçin.

    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. SharePoint kurtarma noktasının kullanılabilir SharePoint katalog bilgileri görüntülemek için çift tıklayın.

   > [!NOTE]
   > SharePoint grubunu azure'da uzun süreli saklama için korumalı olduğundan, hiçbir katalog bilgilerini (meta veriler) DPM sunucusunda kullanılabilir. Sonuç olarak, zaman içinde nokta SharePoint içerik veritabanını kurtarılması gereken zaman, SharePoint grubu yeniden katalog gerekir.
   >
   >
3. Tıklayın **yeniden kataloglama**.

    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    **Bulut yeniden kataloglama** durum penceresi açılır.

    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Kataloglama tamamlandıktan sonra durumu değişerek *başarı*. **Kapat**’a tıklayın.

    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. DPM'de SharePoint nesnelerden tıklayın **kurtarma** içerik veritabanı yapısı almak için sekmesinde. Öğeye sağ tıklayın ve ardından **kurtarmak**.

    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Bu noktada, bir SharePoint içerik veritabanını diskten kurtarmak için bu makalenin önceki bölümlerinde kurtarma adımları izleyin.

## <a name="faqs"></a>SSS

### <a name="which-versions-of-dpm-support-sql-server-2014-and-sql-2012-sp2"></a>Hangi sürümlerinin DPM, SQL Server 2014 ve SQL 2012 (SP2) destekliyor?
DPM 2012 R2 güncelleştirme paketi 4 ile her ikisini de destekler.

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>SharePoint, SQL AlwaysOn (disk koruması) kullanılarak yapılandırılmışsa, bir SharePoint öğesi için özgün konuma kurtarma gerçekleştirebilir miyim?
Evet, öğe özgün SharePoint sitesine kurtarılabilir.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>SharePoint, SQL Alwayson'u kullanarak yapılandırılmışsa, bir SharePoint veritabanını özgün konumuna kurtarma gerçekleştirebilir miyim?
SharePoint veritabanlarını SQL AlwaysOn yapılandırıldığından, bunlar kullanılabilirlik grubu kaldırılmadığı sürece değiştirilemez. Sonuç olarak, DPM veritabanını özgün konumuna geri yükleyemezsiniz. Bir SQL Server veritabanını başka bir SQL Server örneğine kurtarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* SharePoint DPM koruma hakkında daha fazla bilgi - bkz [Video serisi - SharePoint DPM koruma](https://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
* Gözden geçirme [sürüm notları System Center 2012 - Data Protection Manager](https://technet.microsoft.com/library/jj860415.aspx)
* Gözden geçirme [System Center 2012 SP1 Data Protection Manager için sürüm notları](https://technet.microsoft.com/library/jj860394.aspx)
