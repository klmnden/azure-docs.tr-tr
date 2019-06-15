---
title: Azure Backup - DPM için Çevrimdışı Yedekleme ve Azure Backup sunucusu
description: Azure Backup, Azure içeri/dışarı aktarma hizmetini kullanarak ağ kapalı veri göndermek nasıl olanak tanıdığını öğrenin. Bu makalede Azure içeri dışarı aktarma hizmetini kullanarak üretme, çevrimdışı ilk yedekleme verilerini açıklanmaktadır.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 5/8/2018
ms.author: saurse
ms.openlocfilehash: 18f84062bcaf2766ee0abd5248f876c3d8acef3f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66304029"
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server"></a>DPM ve Azure Backup sunucusu için Çevrimdışı Yedekleme iş akışı
Azure Backup, azure'a veri ilk tam yedekleme sırasında ağ ve depolama maliyetlerinden tasarruf birkaç yerleşik verimliliği sahiptir. Genellikle ilk tam yedeklemeler büyük miktarlarda veri aktarmanıza ve yalnızca deltaları/incrementals aktarmak sonraki yedeklemelerle karşılaştırıldığında daha fazla ağ bant genişliği gerektirecektir. Azure yedekleme, ilk yedekleme sıkıştırır. Çevrimdışı dengeli dağıtım işlemi boyunca, Azure Backup diskleri Azure'a sıkıştırılmış ilk yedek verileri çevrimdışı yükleme için kullanabilirsiniz.

Azure Backup'ın çevrimdışı dengeli dağıtım işlemi ile tümleşiktir [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) diskleri kullanarak verilerinizi Azure'a aktarmanıza imkan sağlayan. Yüksek gecikme ve düşük bant genişliğine sahip ağ üzerinden aktarılması gereken ilk yedek verileri (TB'a) terabaytlarca varsa, bir Azure veri merkezine sabit sürücülerde bir veya daha fazla ilk yedek kopyanın göndermeye çevrimdışı dengeli dağıtım iş akışı kullanabilirsiniz. Bu makalede, bir genel bakış ve daha fazla System Center DPM ve Azure Backup sunucusu için bu iş akışını tamamlamak ayrıntıları adımları sağlar.

> [!NOTE]
> Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı çevrimdışı yedekleme işlemi, System Center DPM ve Azure Backup sunucusu farklıdır. Çevrimdışı Yedekleme MARS Aracısı ile kullanma hakkında daha fazla bilgi için bkz: [bu makalede](backup-azure-backup-import-export.md). Çevrimdışı Yedekleme Azure yedekleme ajanını kullanarak yapılan sistem durumu yedeklemeleri için desteklenmiyor.
>

## <a name="overview"></a>Genel Bakış
Azure Backup ve Azure içeri/dışarı aktarma çevrimdışı dengeli dağıtım özelliğiyle, çevrimdışı veri diskleri kullanarak Azure'a yükleme basittir. Çevrimdışı Yedekleme işlemi, aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Ağ üzerinden gönderilen yerine yedekleme verilerini yazılan bir *hazırlama konumuna*
> * Veriler üzerinde *hazırlama konumuna* ardından kullanarak bir veya daha fazla SATA diskleri için yazılan *AzureOfflineBackupDiskPrep* yardımcı programı
> * Azure içeri aktarma işine yardımcı programı tarafından otomatik olarak oluşturulur
> * SATA sürücülerini daha sonra en yakın Azure veri merkezine gönderilir
> * Azure yedekleme verileri karşıya yükleme tamamlandıktan sonra Azure Backup yedekleme verileri yedekleme Kasası'na kopyalar ve artımlı yedeklemeler zamanlandıktan.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar
Çevrimdışı Yedekleme Azure, şirket dışı yedekleme verileri şirket için Microsoft Cloud yedekleme, tüm dağıtım modelleri için desteklenir. Bu içerir

> [!div class="checklist"]
> * Microsoft Azure kurtarma Hizmetleri (MARS) aracısı veya Azure Backup Aracısı ile dosya ve klasörleri yedeğini.
> * Yedekleme, tüm iş yükleri ve dosyaları ile System Center Data Protection Manager (DPM SC)
> * Microsoft Azure Backup sunucusu ile tüm iş yükleri ve dosyalarını yedekleme <br/>

## <a name="prerequisites"></a>Önkoşullar
Çevrimdışı Yedekleme iş akışı başlatmadan önce aşağıdaki önkoşulların karşılandığından emin olun
* A [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md) oluşturuldu. Oluşturmak için adımları bakın [bu makalede](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Geçerli olduğu ya da Windows Server/Windows istemcisi üzerinde Azure Backup aracısı veya Azure Backup sunucusu veya SC DPM yüklendi ve bilgisayar kurtarma Hizmetleri kasasına kayıtlı. Yalnızca [Azure Backup'ın en son sürümü](https://go.microsoft.com/fwlink/?linkid=229525) kullanılır.
* [Azure yayımlama ayarları dosyasını indirin](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) içinden planladığınız yedekleme verilerinizi bilgisayarda. Yayımlama ayarları dosyasını indirdiğiniz abonelik içeren kurtarma Hizmetleri kasası abonelikten farklı olabilir. Aboneliğiniz Azure Bulutları bağımsız ise, aşağıdaki bağlantıları Azure yayımlama ayarları dosyasını indirmek uygun şekilde kullanın.

    | Bağımsız bulut bölgesi | Azure yayımlama ayarları dosyası bağlantısı |
    | --- | --- |
    | Amerika Birleşik Devletleri | [Bağlantı](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | Çin | [Bağlantı](https://portal.azure.cn/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Bir Azure depolama hesabıyla *Klasik* dağıtım modeli, indirdiğiniz yayımlama ayarları dosyası aşağıda gösterildiği gibi abonelik oluşturuldu:

  ![Klasik depolama hesabı oluşturma](./media/backup-azure-backup-import-export/storageaccountclassiccreate.png)

* Bir ağ paylaşımına veya herhangi başka bir sürücü bilgisayarında, iç veya dış, ilk kopyanızı tutmak için yeterli disk alanına sahip olabilecek bir hazırlama konumu oluşturulur. Örneğin, 500 GB'lık dosya sunucusunu yedeklemek çalışıyorsanız, hazırlama alanına en az 500 GB olduğundan emin olun. (Daha küçük bir miktarı, sıkıştırma nedeniyle kullanılır.)
* Azure'a gönderilen diskleri bakımından, yalnızca 2,5 inç SSD veya 2.5 inç veya 3,5 inçlik SATA II/iç sabit sürücüler kullanılan III emin olun. Sabit sürücüleri kullanabileceğiniz 10 TB'a kadar artırın. Denetleme [Azure içeri/dışarı aktarma hizmeti belgeleri](../storage/common/storage-import-export-requirements.md#supported-hardware) hizmetinin desteklediği sürücüleri son kümesi.
* SATA sürücülerini bir bilgisayara bağlı olması gerekir (olarak adlandırılan bir *kopya bilgisayar*) nereden gelen yedekleme verilerin kopyasını *hazırlama konumuna* için SATA sürücülerini gerçekleştirilir. Üzerinde BitLocker'ın etkin olduğundan emin olun *kopya bilgisayar*

## <a name="workflow"></a>İş akışı
Bu bölümdeki bilgiler, böylece verilerinizi Azure veri merkezi için sunulan ve Azure depolama alanına yüklenir, Çevrimdışı Yedekleme iş akışı tamamlamanıza yardımcı olur. İçeri aktarma hizmeti veya işlemin herhangi bir özelliği ile ilgili sorularınız varsa bkz [içeri aktarma hizmetine genel bakış](../storage/common/storage-import-export-service.md) daha önce başvuru belgeleri.

### <a name="initiate-offline-backup"></a>Çevrimdışı Yedekleme başlatmak
1. Bir yedekleme işini zamanlamak için aşağıdaki ekranda (Windows Server, Windows istemci veya System Center Data Protection Manager) görürsünüz.

    ![İçeri aktarma ekran](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    System Center Data Protection Manager karşılık gelen ekranın şöyledir: <br/>
    ![SC DPM ve Azure Backup sunucusu ekran içeri aktarma](./media/backup-azure-backup-import-export/dpmoffline.png)

    Girişleri açıklaması aşağıdaki gibidir:

   * **Hazırlama konumu**: İlk yedek kopyanın yazıldığı geçici depolama konumu. Hazırlama konumu, bir ağ paylaşımına veya yerel bilgisayar üzerinde olabilir. Kopya bilgisayar ve kaynak bilgisayara farklıysa, hazırlama konumunun tam ağ yolu belirtmeniz önerilir.
   * **Azure içeri aktarma işinin adını**: Tarafından hangi Azure içeri aktarma hizmeti ve Azure Backup gönderilen veri aktarımını disklerde Azure'a izlemek benzersiz adı.
   * **Azure yayımlama ayarları**: Yayımlama ayarları dosyasına yerel yolunu belirtin.
   * **Azure abonelik kimliği**: Abonelik'ın Azure yayımlama ayarları dosyasını indirdiğiniz Azure abonelik kimliği.
   * **Azure depolama hesabı**: Azure yayımlama ayarları dosyası ile ilişkili Azure aboneliğinde depolama hesabı adı.
   * **Azure depolama kapsayıcısı**: Hedef depolama blobunda yedekleme verileri burada alınır Azure depolama hesabı adı.

     Kaydet *hazırlama konumuna* ve *Azure içeri aktarma işinin adını* diskleri hazırlamak için gerekli olduğundan, sağlanan.  

2. İş akışını tamamlamak ve çevrimdışı yedekleme kopyasını başlatmak için tıklatın **Şimdi Yedekle** Azure Yedekleme aracısı yönetim konsolundaki. İlk yedeklemeyi, hazırlama alanına bu adım bir parçası olarak yazılır.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/backupnow.png)

    System Center Data Protection Manager veya Azure Backup sunucusu karşılık gelen iş akışında tamamlamak için sağ **koruma grubu**ve ardından **kurtarma noktası oluştur** seçeneği. Ardından seçtiğiniz **çevrimiçi koruma** seçeneği.

    ![SC DPM ve Azure Backup sunucusu Şimdi Yedekle](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    İşlem tamamlandıktan sonra hazırlama konumu disk hazırlığı için kullanılmak üzere hazırdır.

    ![Yedekleme ilerleme durumu](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA sürücülerini hazırlayıp Azure'a
*AzureOfflineBackupDiskPrep* yardımcı programı, en yakın Azure veri merkezine gönderilir SATA sürücüsü hazırlamak için kullanılır. Bu yardımcı program, aşağıdaki yolda kurtarma Hizmetleri Aracısı'nın yükleme dizininde bulunur:

*\\Microsoft Azure kurtarma Hizmetleri Aracısı\\Utils\\*

1. Dizin ve kopya Git **AzureOfflineBackupDiskPrep** hazırlanacak SATA sürücülerini bağlandığınızdan kopyalama bilgisayara dizin. Kopya bilgisayar bakımından aşağıdakilerden emin olun:

   * Kopya bilgisayar hazırlama konumu çevrimdışı dengeli dağıtım iş akışı için sunulan aynı ağ yolunu kullanarak erişebilirsiniz **çevrimdışı yedekleme başlatmak** iş akışı.
   * Kopyalama bilgisayarda BitLocker etkin.
   * Kopya bilgisayar Azure portala erişebilirsiniz.

     Gerekirse, kopya bilgisayar kaynak bilgisayarla aynı olabilir.

     > [!IMPORTANT]
     > Kaynak bilgisayar bir sanal makine ise, ardından onu farklı bir fiziksel sunucu veya istemci makinenin kopya bilgisayarı olarak kullanılacak zorunludur.


2. Kopyalama bilgisayarda yükseltilmiş bir komut istemi açın *AzureOfflineBackupDiskPrep* yardımcı programı dizini olarak geçerli dizin ve şu komutu çalıştırın:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*hazırlama konumu yolu*&gt; |Girdiğiniz hazırlama konumunun yolunu sağlamak için kullanılan Zorunlu giriş **çevrimdışı yedekleme başlatmak** iş akışı. |
    | p:&lt;*PublishSettingsFile yolu*&gt; |Yolunu sağlamak için kullanılan isteğe bağlı bir giriş **Azure yayımlama ayarları** girdiğiniz dosya **çevrimdışı yedekleme başlatmak** iş akışı. |

    > [!NOTE]
    > &lt;AzurePublishSettingFile yoluna&gt; değeri, kopya bilgisayar ve kaynak bilgisayara farklı olduğunda zorunludur.
    >
    >

    Komutu çalıştırdığınızda, yardımcı program hazırlanması gereken sürücülere karşılık gelen Azure Alma işinin seçimi ister. Yalnızca bir tek içeri aktarma işi sağlanan bir hazırlama konumu ile ilişkili olan izleyen benzer bir ekran görürsünüz.

    ![Azure Disk Hazırlık Aracı giriş](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>

3. Azure'a aktarım için hazırlama istediğiniz bağlı diski için sürücü harfini izleyen iki nokta üst üste olmadan girin. İstendiğinde sürücüsünü biçimlendirmek için onay sağlayın.

    Araç, disk ve yedekleme veri kopyalama hazırlamak sonra başlar. Sağlanan disk yedekleme verileri için yeterli alan yok durumunda araç tarafından istendiğinde ek diskler ekleme gerekebilir. <br/>

    Aracı başarılı yürütme sonunda, sağladığınız bir veya daha fazla disk sevkiyat azure'a hazırlanır. Ayrıca, ad ile içeri aktarma işi sırasında sağladığınız **çevrimdışı yedekleme başlatmak** iş akışı, Azure'da oluşturulur. Son olarak, araç diskleri sevk edilmesi için gereken yere Azure veri merkezi teslimat adresini görüntüler.

    ![Azure disk hazırlığı tamamlandı](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Komut yürütme sonunda seçeneğini aşağıda gösterildiği gibi gönderim bilgileri güncelleştirmek için Ayrıca bkz:

    ![Sevkiyat bilgilerini seçeneği güncelleştir](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

5. Hemen ayrıntıları girebilirsiniz. Aracı'nı içeren bir dizi girişleri işleminde size yol gösterir. Ancak sayı veya dağıtımına ilgili diğer ayrıntıları izleme gibi bilgiler yoksa, oturum sona erdirebilirsiniz. Bu makaledeki adımları daha sonra güncelleştirme Sevkiyat ayrıntıları sağlanır.

6. Diskleri aracı sağlanan adresine gönderin ve gelecekte başvurmak için takip numarasını tutun.

   > [!IMPORTANT]
   > İki Azure içeri aktarma iş aynı izleme numarası olabilir. Tek bir Azure içeri aktarma işi altında yardımcı programı tarafından hazırlanan sürücüleri tek bir pakette birlikte gönderilir ve paketi için tek benzersiz izleme numarası olduğundan emin olun. Hazırlanmış bir parçası olarak sürücüleri birleştirmeyin **farklı** Azure içeri aktarma işlerinde tek bir pakette.

5. İzleme bilgileri numarası, içeri aktarma işi tamamlanmasını bekleme kaynak bilgisayara gidin ve ile yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırın olduğunda *AzureOfflineBackupDiskPrep* yardımcı programı dizini olarak Geçerli dizin:

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   İsteğe bağlı olarak aşağıdaki komutu farklı bir bilgisayardan gibi çalıştırabileceğiniz *kopya bilgisayar*, ile *AzureOfflineBackupDiskPrep* yardımcı programı dizin olarak geçerli dizin:

   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parametre | Açıklama |
    | --- | --- |
    | %u | Azure içeri aktarma işine sevkiyat ayrıntılarını güncelleştirmek için kullanılan Zorunlu giriş |
    | s:&lt;*hazırlama konumu yolu*&gt; | Kaynak bilgisayarda komutu çalıştırdığınızda değil, Zorunlu giriş. Girdiğiniz hazırlama konumunun yolunu sağlamak için kullanılan **çevrimdışı yedekleme başlatmak** iş akışı. |
    | p:&lt;*PublishSettingsFile yolu*&gt; | Kaynak bilgisayarda komutu çalıştırdığınızda değil, Zorunlu giriş. Yolunu sağlamak için kullanılan **Azure yayımlama ayarları** girdiğiniz dosya **çevrimdışı yedekleme başlatmak** iş akışı. |

    Yardımcı programı, kaynak bilgisayar bekliyor ya da farklı bir bilgisayarda komutu çalıştırdığınızda, içeri aktarma işlerinde hazırlama konumu ile ilişkili içeri aktarma işi otomatik olarak algılar. Ardından, aşağıda gösterildiği gibi bir dizi girişleri sevkiyat bilgilerini güncelleştirmek için seçeneği sağlar:

    ![Sevkiyat bilgilerini girme](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

6. Tüm girişleri sağlandıktan sonra ayrıntılarını dikkatle gözden geçirin ve size sağlanan yazarak sevkiyat bilgilerini işleme *Evet*.

    ![Sevkiyat bilgileri gözden geçirin](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

7. Sevkiyat bilgilerini başarıyla güncelleştirme özelliğini yardımcı programı tarafından girdiğiniz Sevkiyat ayrıntıları aşağıda gösterildiği gibi depolandığı yerel bir konum sağlar

    ![Sevkiyat bilgilerini depolama](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT]
   > Sürücüleri Sevkiyat bilgileri kullanarak sağlama iki hafta içinde Azure veri merkezi ulaştığından emin olmak *AzureOfflineBackupDiskPrep* yardımcı programı. Bunun yapılmaması, işlenmeyen sürücüleri neden olabilir.  

Yukarıdaki adımları tamamladıktan sonra Azure veri merkezi sürücülerinizi alıp bunları oluşturduğunuz Azure Klasik türündeki depolama hesabını sürücüleri yedekleme verilerini aktarmak daha fazla işlem hazır olur.

### <a name="time-to-process-the-drives"></a>Sürücüleri işlem süresi
Azure içeri aktarma işine değişir sevkiyat zamanını farklı etkene bağlı olarak işleme süresini miktarı iş türü, türü ve kopyalanan verilerin boyutlarına ve sağlanan disklerin boyutu. Azure içeri/dışarı aktarma hizmeti, bir SLA yoktur ancak diskleri alındıktan sonra hizmeti Azure depolama hesabınız için yedek veri kopyalama 7 ila 10 gün içinde tamamlanması üstlenmeye çalışır. Sonraki bölümde, Azure içeri aktarma işinin durumunu nasıl izleyeceğiniz açıklanmaktadır.

### <a name="monitoring-azure-import-job-status"></a>Azure Alma işinin durumunu izleme
Sürücülerinizi Aktarımdaki veya depolama hesabına kopyalamak için Azure veri merkezinde olsa da, Azure Backup aracısı veya SC DPM veya Azure Backup Sunucusu konsolunu kaynak bilgisayarda zamanlanmış yedeklemeleriniz için aşağıdaki iş durumunu gösterir.

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

İçeri aktarma işi durumu denetlemek için aşağıdaki adımları izleyin.
1. Kaynak bilgisayarda yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

     `AzureOfflineBackupDiskPrep.exe u:`

2.  Çıktı, aşağıda gösterildiği gibi içeri aktarma işinin geçerli durumunu gösterir:

    ![İçeri aktarma işi durumunu denetleme](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Azure Alma işinin çeşitli durumları hakkında daha fazla bilgi için bkz. [bu makalede](../storage/common/storage-import-export-view-drive-status.md)

### <a name="complete-the-workflow"></a>İş akışını tamamlayın
İlk yedek verileri içeri aktarma işi tamamlandıktan sonra depolama hesabınızdaki kullanılabilir. Sonraki zamanlanmış yedekleme sırasında Azure backup içeriğini veri depolama hesabından kurtarma Hizmetleri kasasına aşağıda gösterildiği gibi kopyalar:

   ![Kurtarma Hizmetleri Kasası'na veri kopyalama](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Sonraki zamanlanmış yedekleme sırasında Azure Backup artımlı yedekleme ilk yedek kopyanın gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure içeri/dışarı aktarma iş akışındaki herhangi bir sorunuz için başvurmak [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmetini kullanmak](../storage/common/storage-import-export-service.md).
* Azure Backup'ın Çevrimdışı Yedekleme bölümüne başvurun [SSS](backup-azure-backup-faq.md) iş akışı hakkında tüm sorularınız için.
