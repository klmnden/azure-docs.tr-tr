---
title: SharePoint grubunun Azure DPM/Azure yedekleme sunucusu koruma
description: Bu makale, Azure için SharePoint grubunun DPM/Azure yedekleme sunucusu koruma genel bir bakış sağlar
services: backup
author: adigan
manager: Nkolli1
ms.service: backup
ms.topic: conceptual
ms.date: 09/29/2016
ms.author: adigan
ms.openlocfilehash: 728850fe70fb3f9e64b0fa25b4ceebb1a1b51cd4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606662"
---
# <a name="back-up-a-sharepoint-farm-to-azure"></a>Bir SharePoint grubunu Azure’a yedekleme
SharePoint grubunun kurulumu diğer veri kaynaklarını yedekleyen benzer şekilde, System Center Data Protection Manager (DPM) kullanarak Microsoft Azure'a yedekleyin. Azure yedekleme günlük oluşturmak için yedekleme zamanlamasını esneklik sağlar, haftalık, aylık veya yıllık yedekleme noktaları ve çeşitli yedekleme noktaları için bekletme ilkesi seçenekler sunar. DPM, Hızlı Kurtarma süresi hedefi (RTO) için yerel disk kopyaları depolamak ve ekonomik, uzun vadeli bekletme için Azure kopyaları depolamak için yeteneği sağlar.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint desteklenen sürümleri ve ilgili koruma senaryoları
Azure yedekleme DPM için aşağıdaki senaryoları destekler:

| İş yükü | Sürüm | SharePoint dağıtımı | DPM dağıtım türü | DPM - System Center 2012 R2 | Koruma ve kurtarma |
| --- | --- | --- | --- | --- | --- |
| SharePoint |SharePoint 2013, SharePoint 2010, SharePoint 2007'de, SharePoint 3.0 |Bir fiziksel sunucu veya Hyper-V/VMware sanal makinesi dağıtılan SharePoint <br> -------------- <br> SQL AlwaysOn |Fiziksel sunucu veya şirket içi Hyper-V sanal makine |Destekler yedekten Azure için Güncelleştirme Paketi 5 |SharePoint grubu kurtarma seçeneklerini koruma: Kurtarma grubu, veritabanı ve disk kurtarma noktalarından dosya veya liste öğesi.  Azure kurtarma noktalarından grubu ve veritabanı kurtarma. |

## <a name="before-you-start"></a>Başlamadan önce
Bir SharePoint grubuna Azure yedekleme önce onaylamak için gereken birkaç nokta vardır.

### <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce tüm karşıladığınızdan emin olun [Microsoft Azure Yedekleme kullanılarak önkoşulları](backup-azure-dpm-introduction.md#prerequisites) iş yüklerini korumak için. Önkoşullar için bazı görevler aşağıdakileri içerir: bir yedekleme kasası oluşturun, kasa kimlik bilgilerini indirme, Azure Yedekleme aracısı yükleyin ve DPM/Azure yedekleme sunucusu kasayla kaydedin.

### <a name="dpm-agent"></a>DPM Aracısı
DPM Aracısı, SharePoint, SQL Server çalıştıran sunucular ve SharePoint grubunun parçası olan diğer tüm sunucuları çalıştıran sunucuda yüklenmesi gerekir. Koruma aracısını ayarlama hakkında daha fazla bilgi için bkz: [Kurulum koruma Aracısı](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  Aracı yalnızca bir tek web front end (WFE) sunucusuna yükleyin istisnadır. DPM yalnızca koruma için giriş noktası olarak hizmet verecek bir WFE sunucusunda aracının gerekir.

### <a name="sharepoint-farm"></a>SharePoint grubu
Gruptaki her 10 milyon öğe için en az 2 GB alan DPM klasörünün bulunduğu birimde olmalıdır. Bu alanı katalog oluşturma için gereklidir. Belirli öğeleri (site koleksiyonları, siteler, listeler, belge kitaplıkları, klasörler, tek tek belgeler ve liste öğeleri) kurtarmak DPM için katalog oluşturma her içerik veritabanında bulunan URL'lerin bir listesini oluşturur. Kurtarılabilir öğe bölmesinde URL'lerin listesini görüntüleyebilirsiniz **kurtarma** görev alanındaki DPM Yönetici Konsolu'nun.

### <a name="sql-server"></a>SQL Server
DPM LocalSystem hesabı olarak çalışır. SQL Server veritabanlarını yedeklemek için DPM SQL Server çalıştıran sunucu için bu hesabı üzerinde sysadmin ayrıcalıkları gerekir. Kümesine NT AUTHORITY\SYSTEM *sysadmin* önce SQL Server çalıştıran sunucuda yedekleyin.

SharePoint grubu SQL Server diğer adlarıyla yapılandırılmış SQL Server veritabanlarını ise, DPM korunacak ön uç Web sunucusunda SQL Server istemci bileşenlerini yükleyin.

### <a name="sharepoint-server"></a>SharePoint Server
Performans SharePoint grubu boyutu gibi birçok faktöre bağlıdır, ancak genel bir yönerge olarak 25 TB SharePoint grubunu bir DPM sunucusu koruyabilir.

### <a name="dpm-update-rollup-5"></a>DPM Güncelleştirme Paketi 5
SharePoint grubunun azure'a korumayı başlatmak için DPM Güncelleştirme Paketi 5 veya sonraki sürümünü yüklemeniz gerekir. Güncelleştirme Paketi 5 grubu SQL AlwaysOn kullanarak yapılandırılmışsa Azure bir SharePoint grubuna koruma olanağı sağlar.
Daha fazla bilgi için bkz: blog tanıtır post [DPM Güncelleştirme Paketi 5](http://blogs.technet.com/b/dpm/archive/2015/02/11/update-rollup-5-for-system-center-2012-r2-data-protection-manager-is-now-available.aspx)

### <a name="whats-not-supported"></a>Desteklenmeyen durumlar
* Bir SharePoint grubu koruyan DPM arama dizinlerini veya uygulama hizmeti veritabanlarını koruma sağlamaz. Bu veritabanlarını koruma ayrı olarak yapılandırmanız gerekir.
* DPM, genişleme dosya sunucusu (SOFS) paylaşımları üzerinde barındırılan SharePoint SQL Server veritabanlarını yedekleme sağlamaz.

## <a name="configure-sharepoint-protection"></a>SharePoint korumasını yapılandırma
SharePoint'i koruma için DPM kullanmadan önce kullanarak SharePoint VSS yazıcısı hizmetini (WSS yazıcısı hizmeti) yapılandırmanız gerekir **ConfigureSharePoint.exe**.

Bulabileceğiniz **ConfigureSharePoint.exe** [DPM yükleme yolu] \bin klasöründe ön uç web sunucusunda. Bu araç SharePoint grubu için kimlik bilgileriyle koruma aracısını sağlar. Bu tek bir WFE sunucusunda çalıştırın. Birden çok WFE sunucunuz varsa, bir koruma grubu yapılandırırken yalnızca bir tanesini seçin.

### <a name="to-configure-the-sharepoint-vss-writer-service"></a>SharePoint VSS yazıcısı hizmetini yapılandırmak için
1. WFE sunucusundaki bir komut isteminde [DPM yükleme konumuna] \bin\ konumuna gidin.
2. ConfigureSharePoint - EnableSharePointProtection girin.
3. Grup Yöneticisi kimlik bilgilerini girin. Bu hesap, WFE sunucusunda yerel yönetici grubunun bir üyesi olmalıdır. Grup Yöneticisi yerel değilse yönetici WFE sunucusunda aşağıdaki izinleri verin:
   * WSS_Admin_WPG grubu tam denetimini DPM klasörünü (% Program Files%\Microsoft Data Protection Manager\DPM) verin.
   * WSS_Admin_WPG grubu okuma erişimini (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager) DPM kayıt defteri anahtarına verin.

> [!NOTE]
> SharePoint grubu yönetici kimlik bilgilerinde bir değişiklik olduğunda ConfigureSharePoint.exe yeniden çalıştırmanız gerekir.
> 
> 

## <a name="back-up-a-sharepoint-farm-by-using-dpm"></a>DPM kullanarak bir SharePoint grubunun kurulumu yedekleyin
DPM ve daha önce açıklandığı gibi SharePoint grubu yapılandırdıktan sonra bu SharePoint DPM tarafından korunur.

### <a name="to-protect-a-sharepoint-farm"></a>Bir SharePoint grubunu korumak için
1. Gelen **koruma** sekmesini tıklatın, DPM Yönetici Konsolu'nda **yeni**.
    ![Yeni koruma sekmesi](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. Üzerinde **koruma grubu türünü seçin** sayfasında **yeni koruma grubu oluşturma** seçin **sunucuları**ve ardından **sonraki**.
   
    ![Koruma grubu seç türü](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. Üzerinde **grup üyelerini seçin** ekranında, önce korumak istediğiniz SharePoint server için onay kutusunu işaretleyin **sonraki**.
   
    ![Grup üyelerini seçin](./media/backup-azure-backup-sharepoint/select-group-members2.png)
   
   > [!NOTE]
   > DPM aracı yüklü olan sunucunun Sihirbazı'nda görebilirsiniz. DPM, aynı zamanda yapısını gösterir. ConfigureSharePoint.exe çalıştırdığınız için DPM SharePoint VSS Yazıcı hizmeti ve karşılık gelen SQL Server veritabanlarını ile iletişim kurar ve SharePoint grup yapısı, ilişkili içerik veritabanları ve karşılık gelen tüm öğeleri tanır.
   > 
   > 
4. Üzerinde **veri koruma yöntemini seçin** sayfasında, adını girin **koruma grubu**ve tercih ettiğiniz seçin *koruma yöntemleri*. **İleri**’ye tıklayın.
   
    ![Veri koruma yöntemini seçin](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)
   
   > [!NOTE]
   > Disk koruması yöntemi kısa kurtarma zamanı hedeflerine yardımcı olur. Azure için bantları karşılaştırıldığında ekonomik, uzun vadeli koruma hedefidir. Daha fazla bilgi için bkz: [bant altyapınızın yerini alması için Azure Yedekleme'yi](https://azure.microsoft.com/documentation/articles/backup-azure-backup-cloud-as-tape/)
   > 
   > 
5. Üzerinde **kısa vadeli hedefleri belirtin** sayfasında, tercih ettiğiniz **bekletme aralığı** ve gerçekleşmesini istediğinizde belirleyin.
   
    ![Kısa vadeli hedefleri belirtin](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)
   
   > [!NOTE]
   > Kurtarma genellikle beş günden eski olan verileri için gerekli olduğundan, biz diskteki beş günlük bir bekletme aralığı seçili ve yedekleme Bu örnek için üretim dışı saatlerde olur güvence altına.
   > 
   > 
6. Depolama havuzuna disk alanı koruma grubu için ayrılmış ve ardından gözden geçirme **sonraki**.
7. Her koruma grubu için DPM depolamak ve çoğaltmaları yönetmek için disk alanı ayırır. Bu noktada, DPM seçili verilerin bir kopyasını oluşturmanız gerekir. Nasıl ve ne zaman, oluşturulan çoğaltma istediğiniz'ı seçin ve **sonraki**.
   
    ![Çoğaltma oluşturma yöntemini seçin](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)
   
   > [!NOTE]
   > Ağ trafiği değil parametreden emin olmak için üretim saatleri dışında bir saat seçin.
   > 
   > 
8. DPM, çoğaltma üzerinde tutarlılık denetimleri gerçekleştirerek veri bütünlüğü sağlar. Kullanılabilir iki seçenek vardır. Tutarlılık denetimleri çalıştırmak için bir zamanlama tanımlayın veya tutarsız hale her DPM otomatik olarak çoğaltma üzerinde tutarlılık denetimleri çalıştırabilirsiniz. Tercih ettiğiniz seçeneği seçin ve ardından **sonraki**.
   
    ![Tutarlılık denetimi](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. Üzerinde **çevrimiçi koruma verilerini belirtin** sayfasında, koruma ve ardından istediğiniz SharePoint grubu seçin **sonraki**.
   
    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, tercih ettiğiniz zamanlamayı seçin ve ardından **sonraki**.
    
    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)
    
    > [!NOTE]
    > DPM en fazla iki günlük yedeklemeler için Azure farklı zamanlarda sağlar. Azure yedekleme de en yüksek ve saatlere yedeklemelerin kullanarak kullanılabilir WAN bant genişliği miktarını kontrol [Azure yedekleme ağ azaltma](https://azure.microsoft.com/en-in/documentation/articles/backup-configure-vault/#enable-network-throttling).
    > 
    > 
11. Üzerinde seçili yedekleme zamanlaması bağlı olarak **çevrimiçi bekletme ilkesini belirtin** sayfasında, günlük, haftalık, aylık ve yıllık yedekleme noktası bekletme ilkesi seçin.
    
    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)
    
    > [!NOTE]
    > DPM, farklı yedekleme noktaları için farklı bir bekletme ilkesi seçilebilir bir üst öğe son bekletme düzeni kullanır.
    > 
    > 
12. Disk benzeyen, bir ilk başvuru noktası çoğaltmasını Azure içinde oluşturulması gerekir. Azure bir ilk yedek kopyasını oluşturun ve ardından tercih ettiğiniz seçeneği seçin **sonraki**.
    
    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Seçtiğiniz ayarları gözden **Özet** sayfasında ve ardından **Grup Oluştur**. Koruma grubu oluşturulduktan sonra bir başarı iletisi görürsünüz.
    
    ![Özet](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-dpm"></a>DPM kullanarak bir SharePoint öğesi diskten geri yükleme
Aşağıdaki örnekte, *SharePoint kurtarma öğesi* yanlışlıkla silinmişse ve geri yüklenmesi gerekiyor.
![DPM SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Açık **DPM Yönetici Konsolu**. DPM tarafından korunan tüm SharePoint grupları gösterilen **koruma** sekmesi.
   
    ![DPM SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. Öğeyi kurtarmak başlamak için seçin **kurtarma** sekmesi.
   
    ![DPM SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. SharePoint için arama yapabilirsiniz *SharePoint kurtarma öğesi* kurtarma içinde bir joker karakter tabanlı arama kullanarak aralığı gösterin.
   
    ![DPM SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Arama sonuçlarından uygun Kurtarma noktasını seçin, öğeyi sağ tıklatın ve ardından **kurtarmak**.
5. Ayrıca, çeşitli kurtarma noktalarına göz ve bir veritabanı veya kurtarmak için öğeyi seçin. Seçin **tarihi > kurtarma süresini**ve ardından doğru seçin **veritabanı > SharePoint grubu > kurtarma noktası > öğesi**.
   
    ![DPM SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
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
9. Veritabanı geçici olarak kurtarmak için hazırlama bir SQL Server örneğinin konumu sağlayın ve DPM sunucusu ve öğeyi kurtarmak için SharePoint çalıştıran sunucu hazırlama bir dosya paylaşımı sağlayın.
   
    ![Hazırlama Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)
   
    DPM geçici SQL Server örneği için SharePoint öğe barındırma içerik veritabanını ekler. İçerik veritabanından DPM sunucusu öğesi kurtarır ve DPM sunucusundaki hazırlama dosya konumuna yerleştirir. DPM sunucusunun hazırlama konumuna sunulmuştur Kurtarılan bir öğeye SharePoint grubu üzerinde hazırlama konumu için aktarılması gerekir.
   
    ![Hazırlama Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Seçin **kurtarma seçeneklerini belirtin**, SharePoint grubu için güvenlik ayarlarını Uygula ve kurtarma noktası güvenlik ayarlarını uygula. **İleri**’ye tıklayın.
    
    ![Kurtarma Seçenekleri](./media/backup-azure-backup-sharepoint/recovery-options.png)
    
    > [!NOTE]
    > Ağ bant genişliği kullanımını azaltma seçebilirsiniz. Bu, üretim saatleri içinde üretim sunucusu üzerinde etkisi azaltır.
    > 
    > 
11. Özet bilgilerini inceleyin ve ardından **kurtarmak** dosyasının kurtarmayı başlatmak için.
    
    ![Kurtarma özeti](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Şimdi seçin **izleme** sekmesinde **DPM Yönetici Konsolu'nu** görüntülemek için **durum** kurtarma.
    
    ![Kurtarma durumu](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)
    
    > [!NOTE]
    > Dosya artık geri yüklenir. Geri yüklenen dosya denetlemek için SharePoint sitesi yenileyebilirsiniz.
    > 
    > 

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>DPM kullanarak bir SharePoint veritabanı Azure'dan geri yükleme
1. Bir SharePoint içerik veritabanını kurtarmak için (daha önce gösterildiği gibi) çeşitli kurtarma noktalarına göz ve geri yüklemek istediğiniz kurtarma noktasını seçin.
   
    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. SharePoint kurtarma noktası kullanılabilir SharePoint katalog bilgileri görüntülemek için çift tıklayın.
   
   > [!NOTE]
   > SharePoint grubu Azure uzun vadeli bekletme için korumalı olduğundan, hiçbir Kataloğu bilgi (meta veriler) DPM sunucusunda kullanılabilir. Sonuç olarak, bir zaman içinde nokta SharePoint içerik veritabanı kurtarılması gereken her SharePoint grubu yeniden katalog gerekir.
   > 
   > 
3. Tıklatın **yeniden kataloglama**.
   
    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)
   
    **Bulut yeniden katalogla** durum penceresi açar.
   
    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)
   
    Katalog tamamlandıktan sonra durum değişikliklerini *başarı*. **Kapat**’a tıklayın.
   
    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. DPM gösterilen SharePoint nesnesine tıklayın **kurtarma** içerik veritabanı yapısı almak için sekmesini. Öğeyi sağ tıklatın ve ardından **kurtarmak**.
   
    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Bu noktada, izleyin [kurtarma adımları bu makalenin önceki bölümlerinde](#restore-a-sharepoint-item-from-disk-using-dpm) diskten bir SharePoint içerik veritabanını kurtarmak için.

## <a name="faqs"></a>SSS
S: hangi sürümlerinin DPM, SQL Server 2014 ve SQL 2012 (SP2) destekliyor?<br>
Y: DPM 2012 R2 güncelleştirme paketi 4 ile hem de destekler.

S: özgün konuma SharePoint öğesi, SharePoint SQL AlwaysOn (disk koruması) kullanılarak yapılandırılmışsa kurtarma?<br>
A: Evet öğenin özgün SharePoint sitesine kurtarılabilir.

S: bir SharePoint veritabanını özgün konumuna, SharePoint SQL AlwaysOn kullanarak yapılandırılmışsa kurtarma?<br>
A: SharePoint veritabanlarını SQL AlwaysOn yapılandırıldığından, kullanılabilirlik grubu kaldırılmadığı sürece bunlar değiştirilemez. Sonuç olarak, DPM bir veritabanını özgün konumuna geri yükleyemezsiniz. SQL Server veritabanını başka bir SQL Server örneğine kurtarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* DPM SharePoint koruması hakkında daha fazla bilgi - bkz [Video serisi - SharePoint DPM koruma](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
* Gözden geçirme [System Center 2012 - Data Protection Manager için sürüm notları](https://technet.microsoft.com/library/jj860415.aspx)
* Gözden geçirme [System Center 2012 SP1 Data Protection Manager için sürüm notları](https://technet.microsoft.com/library/jj860394.aspx)

