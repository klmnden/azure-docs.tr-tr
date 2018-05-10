---
title: Azure Backup - DPM için Çevrimdışı Yedekleme ve Azure yedekleme sunucusu | Microsoft Docs
description: Azure Backup nasıl Azure içeri/dışarı aktarma hizmetini kullanarak ağ verileri göndermenize olanak sağlayan bilgi edinin. Bu makalede, çevrimdışı ilk yedek verilerini Azure içe aktarma dışarı aktarma hizmetini kullanarak dengeli dağıtımı açıklanmaktadır.
services: backup
documentationcenter: ''
author: saurabhsensharma
manager: shivamg
editor: ''
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/8/2018
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: e3f7ae187bee8680fbff7e5c78c666a0bda7e48f
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server"></a>DPM ve Azure yedekleme sunucusu için Çevrimdışı Yedekleme iş akışı
Azure yedekleme verileri azure'a ilk tam yedeklemesi sırasında ağ ve depolama maliyet tasarrufu birkaç yerleşik verimliliği sahiptir. İlk tam yedekleme tipik olarak büyük miktarlarda veri aktarır ve yalnızca farkları/incrementals aktarım sonraki yedeklemeleri karşılaştırıldığında daha fazla ağ bant genişliği gerektirir. Azure yedekleme ilk yedekleme sıkıştırır. Çevrimdışı dengeli sürecinde, Azure yedekleme disklerini sıkıştırılmış ilk yedek verileri çevrimdışı Azure'a yüklemek için kullanabilirsiniz.

Azure Backup dengeli çevrimdışı işlemi ile tümleşiktir [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) diskleri kullanarak Azure'a veri aktarımı olanak sağlar. Yüksek gecikmeli ve düşük bant genişlikli ağ üzerinden aktarılması gereken ilk yedek verileri terabayt (TBs) varsa, ilk yedek kopyayı Azure veri merkezi için bir veya daha fazla sabit sürücülerde sevk etmek dengeli çevrimdışı iş akışı kullanabilirsiniz. Bu makalede, genel bir bakış ve daha fazla System Center DPM ve Azure yedekleme sunucusu için bu iş akışını tamamlamak ayrıntıları adımları sağlar.

> [!NOTE]
> Microsoft Azure kurtarma Hizmetleri (MARS) aracısı için Çevrimdışı Yedekleme işlemini, System Center DPM ve Azure yedekleme sunucusu farklıdır. Çevrimdışı Yedekleme MARS Aracısı ile kullanma hakkında daha fazla bilgi için bkz: [bu makalede](backup-azure-backup-import-export.md). Çevrimdışı Yedekleme Azure Backup Aracısı kullanılarak gerçekleştirilen sistem durumu yedeklemeleri için desteklenmiyor. 
>

## <a name="overview"></a>Genel Bakış
Azure Backup ve Azure içeri/dışarı aktarma dengeli çevrimdışı özelliğiyle, verileri çevrimdışı diskleri kullanarak Azure'a yükleyin basittir. Çevrimdışı Yedekleme işlemi aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Ağ üzerinden gönderilen yerine yedekleme verilerini yazılan bir *hazırlama*
> * Verileri *hazırlama konumuna* sonra kullanarak bir veya daha fazla SATA diskleri için yazılmış *AzureOfflineBackupDiskPrep* yardımcı programı
> * Bir Azure alma işi yardımcı programı tarafından otomatik olarak oluşturulur
> * SATA sürücülerini daha sonra en yakın Azure veri merkezine gönderilir
> * Azure yedekleme veri yükleme tamamlandıktan sonra Azure Backup yedekleme verileri yedekleme Kasası'na kopyalar ve artımlı yedeklemeler zamanlanır.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar 
Çevrimdışı Yedekleme Azure, şirket dışı yedekleme verileri Microsoft Cloud şirket içi yedekleme, tüm dağıtım modelleri için desteklenir. Bu içerir

> [!div class="checklist"]
> * Dosya ve klasörlerin Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nı veya Azure Backup Aracısı ile yedekleme. 
> * Yedekleme, tüm iş yükleri ve dosyalar ile System Center Data Protection Manager (SC DPM) 
> * Microsoft Azure yedekleme sunucusu ile tüm iş yükleri ve dosyalarını yedekleme <br/>

## <a name="prerequisites"></a>Önkoşullar
Çevrimdışı Yedekleme iş akışı başlatmadan önce aşağıdaki önkoşulların karşılandığından emin olun
* A [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) oluşturuldu. ' Ndaki adımları başvurmak oluşturmak için [bu makalede](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Azure Backup aracısı veya Azure yedekleme sunucusu veya SC DPM yüklü olduğu geçerli ya da Windows Server/Windows istemci ve bilgisayarı kurtarma Hizmetleri kasasına kayıtlı. Yalnızca olun [Azure Backup en son sürümünü](https://go.microsoft.com/fwlink/?linkid=229525) kullanılır. 
* [Azure yayımlama ayarları dosyasını indirme](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) içinden planladığınız geri verilerinizi bilgisayarda. Yayımlama ayarları dosyasını indirme abonelik kurtarma Hizmetleri kasası içeren abonelikten farklı olabilir. Aboneliğiniz Azure Bulutları sovereign üzerinde ise, aşağıdaki bağlantıları Azure yayımlama ayarları dosyasını karşıdan yüklemek uygun şekilde kullanın.

    | Sovereign bulut bölge | Azure yayımlama ayarları dosyası bağlantısı |
    | --- | --- |
    | Amerika Birleşik Devletleri | [Bağlantı](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | Çin | [Bağlantı](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Bir Azure depolama hesabıyla *Klasik* dağıtım modeli, indirdiğiniz yayımlama ayarları dosyası aşağıda gösterildiği gibi abonelik oluşturuldu: 

 ![Klasik depolama hesabı oluşturma](./media/backup-azure-backup-import-export/storageaccountclassiccreate.png)

* Bir ağ paylaşımına veya bilgisayarda, iç veya dış, ek herhangi sürücü ilk kopyanızı tutmak için yeterli disk alanına sahip olabilen bir hazırlama konumu oluşturulur. Örneğin, 500 GB dosya sunucusunu yedeklemek çalışıyorsanız, hazırlama alanı en az 500 GB olduğundan emin olun. (Daha küçük miktarda sıkıştırma nedeniyle kullanılır.)
* Azure'a gönderilen diskleri göre yalnızca 2,5 inç SSD veya 2,5 inç veya 3,5 inç SATA II/dahili sabit sürücüler kullanılan III emin olun. Sabit disk sürücüleri kullanan 10 TB'a kadar. Denetleme [Azure içeri/dışarı aktarma hizmeti belgeleri](../storage/common/storage-import-export-service.md#hard-disk-drives) hizmetini destekleyen sürücüler son kümesi için.
* SATA sürücülerini bir bilgisayara bağlı olması gerekir (olarak adlandırılan bir *kopya bilgisayar*) nerede gelen yedekleme verilerinin kopyasını *hazırlama konumuna* SATA sürücülerini yapılır. BitLocker'ı üzerinde etkinleştirildiğinden emin olun *kopya bilgisayar* 

## <a name="workflow"></a>İş akışı
Bu bölümdeki bilgiler, verilerinizi Azure veri merkezi için teslim ve Azure depolama alanına karşıya Çevrimdışı Yedekleme iş akışı tamamlamanıza yardımcı olur. İçeri aktarma hizmeti veya herhangi bir değişiklik işlemi, hakkında sorularınız varsa bkz [alma hizmetine genel bakış](../storage/common/storage-import-export-service.md) daha önce başvurulan belgeleri.

### <a name="initiate-offline-backup"></a>Çevrimdışı Yedekleme başlatmak
1. Bir yedekleme zamanlarsanız aşağıdaki ekranında (Windows Server, Windows istemci veya System Center Data Protection Manager) bakın.

    ![İçeri aktarma ekran](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    System Center Data Protection Manager içinde karşılık gelen ekran şöyledir: <br/>
    ![SC DPM ve Azure yedekleme sunucusu ekran Al](./media/backup-azure-backup-import-export/dpmoffline.png)

    Girdi açıklaması aşağıdaki gibidir:

    * **Hazırlama konumu**: ilk yedek kopyanın yazılır geçici depolama konumu. Hazırlama konumu, bir ağ paylaşımına veya yerel bilgisayarda olabilir. Kaynak bilgisayar ve kopya bilgisayar farklıysa, hazırlama konumu tam ağ yolunu belirtin önerilir.
    * **Azure içeri aktarma iş adı**: hangi Azure alma tarafından hizmet ve Azure yedekleme izlemek gönderilen veri aktarımını disklerde Azure için benzersiz bir ad.
    * **Azure yayımlama ayarları**: yayımlama ayarları dosyası yerel yolunu girin.
    * **Azure abonelik kimliği**: Azure abonelik kimliği için Azure yayımlama ayarları dosyasını indirdiğiniz gelen abonelik. 
    * **Azure depolama hesabı**: Azure yayımlama ayarları dosyası ile ilişkili Azure Abonelikteki depolama hesabının adı.
    * **Azure depolama kapsayıcısının**: yedekleme verileri nerede alınır Azure depolama hesabındaki hedef depolama blob adı.

     Kaydet *hazırlama konumuna* ve *Azure içeri aktarma iş adı* diskler hazırlamak için gerektiği şekilde sağladığınız.  
     
2. İş akışını tamamlamak ve çevrimdışı yedekleme kopyasını başlatmak için tıklatın **Şimdi Yedekle** Azure Yedekleme aracısı Yönetim Konsolu'nda. İlk yedekleme için bu adımı bir parçası olarak hazırlama alanına yazılır.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/backupnow.png)

    System Center Data Protection Manager veya Azure yedekleme sunucusu karşılık gelen iş akışında tamamlamak için sağ **koruma grubu**ve ardından **kurtarma noktası oluşturma** seçeneği. Daha sonra seçtiğiniz **çevrimiçi koruma** seçeneği.

    ![SC DPM ve Azure Backup sunucusu Şimdi Yedekle](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    İşlemi tamamlandıktan sonra hazırlama konumu disk hazırlığı için kullanılmak üzere hazırdır.

    ![Yedekleme ilerleme durumu](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA sürücülerini hazırlamak ve Azure'a sevk
*AzureOfflineBackupDiskPrep* yardımcı programı, en yakın Azure veri merkezine gönderilir SATA sürücülerini hazırlamak için kullanılır. Bu yardımcı program yolunda kurtarma Hizmetleri Aracısı'nın yükleme dizininde bulunur:

   *\Microsoft* *azure* *kurtarma* *Hizmetleri* * Agent\Utils\*

1. Dizin ve kopyalama gidin **AzureOfflineBackupDiskPrep** hazırlıklı olmak için SATA sürücülerini bağlı bir kopya bilgisayarı için dizin. Aşağıdaki kopya bilgisayar yapılmadığından emin olun:

    * Kopya bilgisayar çevrimdışı dengeli dağıtımı iş akışı için hazırlama konumu olarak sağlanan aynı ağ yolunu kullanarak erişebilirsiniz **başlatmak Çevrimdışı Yedekleme** iş akışı.
    * BitLocker kopyalama bilgisayarda etkin.
    * Kopya bilgisayar Azure portalına erişebilir.

    Gerekirse, kopya bilgisayar kaynak bilgisayarla aynı olabilir. 
    
    > [!IMPORTANT] 
    > Kaynak bilgisayarda bir sanal makine ise, ardından onu farklı bir fiziksel sunucu veya istemci makine kopya bilgisayar olarak kullanmak üzere zorunludur.
    
    
2. Kopya bilgisayarda yükseltilmiş bir komut istemi açın *AzureOfflineBackupDiskPrep* yardımcı programı dizini olarak geçerli dizinin ve aşağıdaki komutu çalıştırın:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*hazırlama konumu yolu*&gt; |Girdiğiniz hazırlama konumu yolunu sağlamak için kullanılan Zorunlu giriş **başlatmak Çevrimdışı Yedekleme** iş akışı. |
    | p:&lt;*PublishSettingsFile yolu*&gt; |Yolunu sağlamak için kullanılan isteğe bağlı giriş **Azure yayımlama ayarları** girdiğiniz dosya **başlatmak Çevrimdışı Yedekleme** iş akışı. |

    > [!NOTE]
    > &lt;AzurePublishSettingFile yoluna&gt; değeri, kaynak bilgisayar ve kopya bilgisayar farklı olduğunda zorunludur.
    >
    >

    Komutu çalıştırdığınızda, yardımcı program seçimi hazırlanması gerekir sürücülere karşılık gelen Azure Alma işinin ister. Yalnızca bir tek alma işi sağlanan hazırlama konumu ile ilişkili olan, izleyen bir gibi bir ekran görürsünüz.

    ![Azure Disk Hazırlık Aracı giriş](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
    
3. Transfer Azure için hazırlamak istediğiniz bağlı diski için sürücü harfini izleyen iki nokta üst üste olmadan girin. İstendiğinde sürücüsünü biçimlendirmek için onay sağlayın.

    Aracı, disk ve yedekleme verileri kopyalayarak hazırlamak sonra başlar. Sağlanan disk yedekleme verileri için yeterli alanı yok durumunda aracı tarafından istendiğinde ilave diskler eklemeniz gerekebilir. <br/>

    Aracının başarılı yürütme sonunda, sağladığınız bir veya daha fazla disk Azure dağıtımına için hazırlanır. Ayrıca, ada sahip bir alma işi sırasında sağladığınız **başlatmak Çevrimdışı Yedekleme** iş akışı, Azure içinde oluşturulur. Son olarak, aracı diskleri burada sevk gerek Azure veri merkezi için teslimat adresi görüntüler.

    ![Azure disk hazırlığı tamamlandı](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>
    
4. Komut yürütme sonunda da aşağıda gösterildiği gibi sevkiyat bilgilerini güncelleştirme seçeneğini bakın:

    ![Güncelleştirme bilgilerini seçeneği aktarma](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

5. Ayrıntıları hemen girebilirsiniz. Aracı bir dizi girişleri içeren işleminde size rehberlik eder. Ancak izleme numarası veya dağıtımına ilgili diğer ayrıntılar gibi bilgiler yoksa, oturumu sonlandırabilir. Bu makalede daha sonra güncelleştirme Sevkiyat ayrıntıları için adımları sağlanır. 

6. Aracı sağlanan adres disklere sevk ve gelecekte başvurmak için izleme numarası tutun.

   > [!IMPORTANT] 
   > İki Azure içeri aktarma işi aynı izleme numarasına sahip olabilir. Tek bir Azure içe aktarma işi altında yardımcı programı tarafından hazırlanan sürücüleri tek bir paket birlikte gönderilir ve paketi için tek benzersiz izleme numarası olduğundan emin olun. Bir parçası olarak hazırlanan sürücüleri birleştirmeyin **farklı** Azure içe aktarma işlerinin tek bir pakette. 

5. Sayı bilgileri, alma işinin tamamlanması bekleniyor kaynak bilgisayara gidin ve aşağıdaki komutu yükseltilmiş bir komut istemi ile çalıştırın izleme olduğunda *AzureOfflineBackupDiskPrep* yardımcı programı dizini olarak Geçerli dizin: 

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   İsteğe bağlı olarak aşağıdaki komutu farklı bir bilgisayardan aşağıdaki gibi çalıştırabilirsiniz *kopya bilgisayar*, ile *AzureOfflineBackupDiskPrep* yardımcı programı dizini geçerli dizin olarak:
   
   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parametre | Açıklama |
    | --- | --- |
    | U: | Bir Azure alma işi sevkiyat ayrıntılarını güncelleştirmek için kullanılan Zorunlu giriş |
    | s:&lt;*hazırlama konumu yolu*&gt; | Komutu kaynak bilgisayarda çalıştırdığınızda değil, Zorunlu giriş. Girdiğiniz hazırlama konumu yolunu sağlamak için kullanılan **başlatmak Çevrimdışı Yedekleme** iş akışı. |
    | p:&lt;*PublishSettingsFile yolu*&gt; | Komutu kaynak bilgisayarda çalıştırdığınızda değil, Zorunlu giriş. Yolunu sağlamak için kullanılan **Azure yayımlama ayarları** girdiğiniz dosya **başlatmak Çevrimdışı Yedekleme** iş akışı. |
    
    Yardımcı programı, kaynak bilgisayar bekliyor veya farklı bir bilgisayarda komutu çalıştırdığınızda, içeri aktarma işi hazırlama konumu ile ilişkili alma işi otomatik olarak algılar. Ardından, aşağıda gösterildiği gibi bir dizi girişleri üzerinden sevkiyat bilgilerini güncelleştirme seçeneği sağlar: 
    
    ![Sevkiyat bilgileri girme](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

6. Tüm girişleri sağlanan sonra ayrıntıları dikkatle gözden geçirin ve sağlanan yazarak Sevkiyat bilgisi kaydedilmesi *Evet*. 

    ![Sevkiyat bilgileri gözden geçirin](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

7. Sevkiyat bilgisi başarıyla güncelleştirme üzerinde yardımcı programı tarafından girdiğiniz sevkiyat Ayrıntılar aşağıda gösterildiği gibi depolandığı yerel bir konum sağlar 

    ![Bilgi Kargo depolanması](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT] 
   > Sürücüleri Sevkiyat bilgileri kullanarak sağlama iki hafta içinde Azure veri merkezi ulaştığından emin olmak *AzureOfflineBackupDiskPrep* yardımcı programı. Bunun Sağlanamaması işlenmeyen sürücüleri neden olabilir.  

Yukarıdaki adımları tamamladıktan sonra Azure veri merkezi sürücüleri almak ve bunları oluşturduğunuz Klasik türü Azure depolama hesabı sürücülerden yedekleme verilerini aktarmak için daha fazla işlemek hazırdır. 

### <a name="time-to-process-the-drives"></a>Sürücüleri işlemek için süre 
Gereken bir Azure alma işi değişir Sevkiyat Zamanı gibi farklı etkenlere bağlı olarak işlenecek süreyi iş türü, türü ve kopyalanan verilerin boyutunu ve sağlanan disk boyutu. Azure içeri/dışarı aktarma hizmeti bir SLA yok ancak diskleri alındıktan sonra hizmet 7-10 gün içinde Azure storage hesabınıza yedek veri kopyalama tamamlamak için çalışır. Sonraki bölümde, Azure Alma işinin durumunu nasıl izleyeceğiniz ayrıntıları. 

### <a name="monitoring-azure-import-job-status"></a>Azure Alma işinin durumunu izleme
Sürücülerinizin aktarımda veya depolama hesabına kopyalanacak Azure veri merkezindeki olsa da, Azure Backup aracısını veya SC DPM veya Azure Yedekleme Sunucusu konsolu kaynak bilgisayarda zamanlanmış yedeklemeler için aşağıdaki iş durumunu gösterir. 

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

İçeri aktarma işi durumunu denetlemek için aşağıdaki adımları izleyin. 
1. Kaynak bilgisayarda yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:
    
     `AzureOfflineBackupDiskPrep.exe u:`
    
2.  Çıkış, aşağıda gösterildiği gibi Alma işinin geçerli durumunu gösterir: 

    ![Alma işi durumunu denetleme](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Azure Alma işinin çeşitli durumları hakkında daha fazla bilgi için bkz: [bu makalede](../storage/common/storage-import-export-service.md#how-does-the-azure-importexport-service-work)

### <a name="complete-the-workflow"></a>İş akışını tamamla
İçe aktarma işi tamamlandıktan sonra ilk yedek verileri depolama hesabınızdaki kullanılabilir. Sonraki zamanlanmış yedekleme zaman Azure yedekleme verileri içeriğini depolama hesabından kurtarma Hizmetleri Kasası'na aşağıda gösterildiği gibi kopyalar: 

   ![Veri kurtarma Hizmetleri Kasası'na kopyalama](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Sonraki zamanlanmış yedekleme zaman Azure yedekleme artımlı yedekleme ilk yedek kopyayı gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure içeri/dışarı aktarma iş akışı üzerinde herhangi bir sorunuz için başvurmak [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanma](../storage/common/storage-import-export-service.md).
* Çevrimdışı Yedekleme bölümüne Azure yedekleme bakın [SSS](backup-azure-backup-faq.md) iş akışı hakkında herhangi bir sorunuz için.
