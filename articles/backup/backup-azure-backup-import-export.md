---
title: "Azure Backup - Çevrimdışı Yedekleme ya da Azure içeri/dışarı aktarma hizmetini kullanarak ilk dengeli dağıtımı | Microsoft Docs"
description: "Azure Backup nasıl Azure içeri/dışarı aktarma hizmetini kullanarak ağ verileri göndermenize olanak sağlayan bilgi edinin. Bu makalede, çevrimdışı ilk yedek verilerini Azure içe aktarma dışarı aktarma hizmetini kullanarak dengeli dağıtımı açıklanmaktadır."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/18/2017
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: c58aafda21e02e12984e09ef605f7ea13200e381
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Azure Backup’ta çevrimdışı yedekleme iş akışı
Azure yedekleme verileri azure'a ilk tam yedeklemesi sırasında ağ ve depolama maliyet tasarrufu birkaç yerleşik verimliliği sahiptir. İlk tam yedekleme tipik olarak büyük miktarlarda veri aktarır ve yalnızca farkları/incrementals aktarım sonraki yedeklemeleri karşılaştırıldığında daha fazla ağ bant genişliği gerektirir. Azure yedekleme ilk yedekleme sıkıştırır. Çevrimdışı dengeli sürecinde, Azure yedekleme disklerini sıkıştırılmış ilk yedek verileri çevrimdışı Azure'a yüklemek için kullanabilirsiniz.  

Azure Backup dengeli çevrimdışı işlemi ile tümleşiktir [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) diskleri kullanarak Azure'a veri aktarımı olanak sağlar. Yüksek gecikmeli ve düşük bant genişlikli ağ üzerinden aktarılması gereken ilk yedek verileri terabayt (TBs) varsa, ilk yedek kopyayı Azure veri merkezi için bir veya daha fazla sabit sürücülerde sevk etmek dengeli çevrimdışı iş akışı kullanabilirsiniz. Bu makalede, bu iş akışını tamamlama adımları genel bir bakış sağlar.

## <a name="overview"></a>Genel Bakış
Azure Backup ve Azure içeri/dışarı aktarma dengeli çevrimdışı özelliğiyle, verileri çevrimdışı diskleri kullanarak Azure'a yükleyin basittir. İlk tam kopyayı ağ üzerinden aktarılması yerine yedekleme verilerini yazılan bir *hazırlama konumuna*. Azure içeri/dışarı aktarma aracını kullanarak hazırlama konumu için kopyalama tamamlandıktan sonra bu verileri bir veya daha fazla SATA sürücülerini, verilerin miktarına bağlı olarak yazılır. Bu sürücüler sonunda en yakın Azure veri merkezine gönderilir.

[Azure Yedekleme'nin (ve daha sonra) güncelleştirme Ağustos 2016](http://go.microsoft.com/fwlink/?LinkID=229525) içeren *Azure Disk Hazırlama aracı*, AzureOfflineBackupDiskPrep, adlı:

* Azure içeri/dışarı aktarma aracını kullanarak sürücülerinizin Azure alma için hazırlanmanıza yardımcı olur.
* Otomatik olarak Azure içeri/dışarı aktarma hizmeti için bir Azure içe aktarma işi oluşturur [Azure portal](https://ms.portal.azure.com).

Azure yedekleme veri yükleme tamamlandıktan sonra Azure Backup yedekleme verileri yedekleme Kasası'na kopyalar ve artımlı yedeklemeler zamanlanır.

> [!NOTE]
> Azure Disk hazırlığı aracını kullanmak için Ağustos 2016 güncelleştirmenin Azure Yedekleme'nin (veya üstü) yüklü olduğu ve onunla iş akışının tüm adımları gerçekleştirin emin olun. Azure Backup eski bir sürümünü kullanıyorsanız, Azure içeri/dışarı aktarma aracı ayrıntılı, bu makalenin sonraki bölümlerinde kullanarak SATA sürücü hazırlayabilirsiniz.
>
>

## <a name="prerequisites"></a>Önkoşullar
* [Azure içeri/dışarı aktarma iş akışı ile öğrenmeniz](../storage/common/storage-import-export-service.md).
* İş akışı başlatmadan önce aşağıdakilerden emin olun:
  * Bir Azure yedekleme kasası oluşturuldu.
  * İndirilen kasa kimlik bilgileri.
  * Azure Backup aracısını Windows Server/Windows istemci veya System Center Data Protection Manager sunucu üzerinde yüklü ve bilgisayar Azure yedekleme kasasına kayıtlı.
* [Dosya Azure yayımlama ayarları indirme](https://manage.windowsazure.com/publishsettings) içinden planladığınız geri verilerinizi bilgisayarda.
* Bir ağ paylaşımına veya bilgisayarda ek sürücüye olabilir bir hazırlama konumu hazırlayın. Hazırlama konumu geçici depolama ve geçici olarak bu iş akışı sırasında kullanılır. Hazırlama konumuna ilk kopyanızı tutmak için yeterli disk alanı olduğundan emin olun. Örneğin, 500 GB dosya sunucusunu yedeklemek çalışıyorsanız, hazırlama alanı en az 500 GB olduğundan emin olun. (Daha küçük miktarda sıkıştırma nedeniyle kullanılır.)
* Desteklenen bir sürücü kullandığınızdan emin olun. Yalnızca 2,5 inç SSD veya 2.5 veya 3,5 inç SATA II/III dahili sabit sürücüler içeri/dışarı aktarma hizmeti ile kullanım için desteklenir. Sabit disk sürücüleri kullanan 10 TB'a kadar. Denetleme [Azure içeri/dışarı aktarma hizmeti belgeleri](../storage/common/storage-import-export-service.md#hard-disk-drives) hizmetini destekleyen sürücüler son kümesi için.
* BitLocker SATA sürücü yazan bağlandığı bilgisayarda etkinleştirin.
* [Azure içeri/dışarı aktarma Aracı'nı indirme](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) SATA sürücü yazan bağlı bilgisayara. İndirilen ve Ağustos 2016 güncelleştirme Azure Yedekleme'nin (veya üstü) yüklü, bu adım gerekli değildir.

## <a name="workflow"></a>İş akışı
Bu bölümdeki bilgiler, verilerinizi Azure veri merkezi için teslim ve Azure depolama alanına karşıya Çevrimdışı Yedekleme iş akışı tamamlamanıza yardımcı olur. İçeri aktarma hizmeti veya herhangi bir değişiklik işlemi, hakkında sorularınız varsa bkz [alma hizmetine genel bakış](../storage/common/storage-import-export-service.md) daha önce başvurulan belgeleri.

### <a name="initiate-offline-backup"></a>Çevrimdışı Yedekleme başlatmak
1. Bir yedekleme zamanlarsanız aşağıdaki ekranında (Windows Server, Windows istemci veya System Center Data Protection Manager) bakın.

    ![İçeri aktarma ekran](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    System Center Data Protection Manager içinde karşılık gelen ekran şöyledir: <br/>
    ![DPM içeri aktarma ekran](./media/backup-azure-backup-import-export/dpmoffline.png)

    Girdi açıklaması aşağıdaki gibidir:

    * **Hazırlama konumu**: ilk yedek kopyanın yazılır geçici depolama konumu. Bu, bir ağ paylaşımına veya yerel bilgisayarda olabilir. Kaynak bilgisayar ve kopya bilgisayar farklıysa, hazırlama konumu tam ağ yolunu belirtin önerilir.
    * **Azure içeri aktarma iş adı**: hangi Azure alma tarafından hizmet ve Azure yedekleme izlemek gönderilen veri aktarımını disklerde Azure için benzersiz bir ad.
    * **Azure yayımlama ayarları**: Abonelik profilinizi hakkında bilgi içeren bir XML dosyası. Ayrıca, aboneliğinizle ilişkili güvenli kimlik bilgileri içerir. Yapabilecekleriniz [dosya indirme](https://manage.windowsazure.com/publishsettings). Yayımlama ayarları dosyası yerel yolu sağlar.
    * **Azure abonelik kimliği**: Azure Alma işinin başlatmak planladığınız abonelik için Azure abonelik kimliği. Birden çok Azure aboneliğiniz varsa, içe aktarma işi ile ilişkilendirmek istediğiniz abonelik Kimliğini kullanın.
    * **Azure depolama hesabı**: Azure içe aktarma işi ile ilişkili Azure abonelik depolama hesabında.
    * **Azure depolama kapsayıcısının**: Bu işin veriler alınırken burada Azure depolama hesabındaki hedef depolama blob adı.

    > [!NOTE]
    > Sunucunuz için bir Azure kurtarma Hizmetleri Kasası'nı kaydettiğiniz varsa [Azure portal](https://portal.azure.com) değil ve yedeklemeler için bir bulut çözümü sağlayıcısı (CSP) abonelik depolama hesabı Azure portalından oluşturabilirsiniz ve Çevrimdışı Yedekleme iş akışı için kullanın.
    >
    >

     Aşağıdaki adımlarda yeniden girmeniz gerekir çünkü bu bilgileri kaydedin. Yalnızca *hazırlama konumuna* diskleri hazırlamak için Azure Disk hazırlığı aracını kullandıysanız gereklidir.    
2. İş akışı tamamlayın ve ardından **Şimdi Yedekle** çevrimdışı yedekleme kopyasını başlatmak için Azure Yedekleme Yönetimi konsolunda. İlk yedekleme için bu adımı bir parçası olarak hazırlama alanına yazılır.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/backupnow.png)

    System Center Data Protection Manager karşılık gelen iş akışını tamamlamak için sağ **koruma grubu**ve ardından **kurtarma noktası oluşturma** seçeneği. Daha sonra seçtiğiniz **çevrimiçi koruma** seçeneği.

    ![Şimdi DPM yedekleme](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    İşlemi tamamlandıktan sonra hazırlama konumu disk hazırlığı için kullanılmak üzere hazırdır.

    ![Yedekleme ilerleme durumu](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-the-azure-disk-preparation-tool"></a>SATA sürücü hazırlamak ve Azure Disk hazırlığı aracını kullanarak bir Azure alma işi oluşturma
Azure Disk Hazırlık Aracı kurtarma Hizmetleri Aracısı'nı yükleme dizininde kullanıma hazır (Ağustos 2016 güncelleştirin ve daha sonra) aşağıdaki yolundaki.

   *\Microsoft* *azure* *kurtarma* *Hizmetleri* * Agent\Utils\*

1. Dizin ve kopyalama gidin **AzureOfflineBackupDiskPrep** üzerinde hazırlıklı olmak için bağlı sürücüler kopyalama bilgisayara dizin. Kopya bilgisayar açısından için aşağıdakilerden emin olun:

    * Kopya bilgisayar çevrimdışı dengeli dağıtımı iş akışı için hazırlama konumu olarak sağlanan aynı ağ yolunu kullanarak erişebilirsiniz **başlatmak Çevrimdışı Yedekleme** iş akışı.
    * BitLocker'ı bilgisayarda etkin.
    * Bilgisayar Azure portalına erişebilir.

    Gerekirse, kopya bilgisayar kaynak bilgisayarla aynı olabilir.
2. Azure Disk Hazırlık Aracı dizini geçerli dizin olarak kopyalama bilgisayarda yükseltilmiş bir komut istemi açın ve aşağıdaki komutu çalıştırın:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to PublishSettingsFile*>]`

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*hazırlama konumu yolu*&gt; |Girdiğiniz hazırlama konumu yolunu sağlamak için kullanılan Zorunlu giriş **başlatmak Çevrimdışı Yedekleme** iş akışı. |
    | p:&lt;*PublishSettingsFile yolu*&gt; |Yolunu sağlamak için kullanılan isteğe bağlı giriş **Azure yayımlama ayarları** girdiğiniz dosya **başlatmak Çevrimdışı Yedekleme** iş akışı. |

    > [!NOTE]
    > &lt;PublishSettingFile yoluna&gt; değeri, kaynak bilgisayar ve kopya bilgisayar farklı olduğunda zorunludur.
    >
    >

    Komutu çalıştırdığınızda, araç hazırlanması gerekir sürücülere karşılık gelen Azure Alma işinin seçimi ister. Yalnızca bir tek alma işi sağlanan hazırlama konumu ile ilişkili olan, izleyen bir gibi bir ekran görürsünüz.

    ![Azure Disk Hazırlık Aracı giriş](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Transfer Azure için hazırlamak istediğiniz bağlı diski için sürücü harfini izleyen iki nokta üst üste olmadan girin. İstendiğinde sürücüsünü biçimlendirmek için onay sağlayın.

    Yedekleme verilerini diskle hazırlamak aracı sonra başlar. Sağlanan disk yedekleme verileri için yeterli alanı yok durumunda aracı tarafından istendiğinde ilave diskler eklemeniz gerekebilir. <br/>

    Aracının başarılı yürütme sonunda, sağladığınız bir veya daha fazla disk Azure dağıtımına için hazırlanır. Ayrıca, ada sahip bir alma işi sırasında sağladığınız **başlatmak Çevrimdışı Yedekleme** iş akışı, Azure portalında oluşturulur. Son olarak, aracı diskleri burada sevk gerek Azure veri merkezi için teslimat adresi ve içe aktarma işi Azure Portalı'nda bulmak için bağlantı görüntüler.

    ![Azure disk hazırlığı tamamlandı](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Aracı sağlanan adres disklere sevk ve gelecekte başvurmak için izleme numarası tutun.<br/>

5. Aracı görüntülenen bağlantısını olduğunuzda, belirttiğiniz Azure depolama hesabı bkz **başlatmak Çevrimdışı Yedekleme** iş akışı. Burada, yeni oluşturulan alma işi görebilirsiniz **İÇERİ/dışarı aktarma** depolama hesabının sekmesi.

    ![Oluşturulan alma işi](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>

6. Tıklatın **Sevkiyat bilgisi** aşağıdaki ekran görüntüsünde gösterildiği gibi iletişim bilgilerinizi güncelleştirmek için sayfanın altındaki. Microsoft bu bilgileri alma işi tamamlandıktan sonra geri için disklerinizi sevk etmek için kullanır.

    ![İletişim bilgileri](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>

7. Sonraki ekranda sevkiyat ayrıntılarını girin. Sağlamak **teslim taşıyıcı** ve **izleme numarası** Azure veri merkezine sevk diskleri karşılık ayrıntıları.

    ![Sevkiyat bilgileri](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-the-workflow"></a>İş akışını tamamla
İçe aktarma işi tamamlandıktan sonra ilk yedek verileri depolama hesabınızdaki kullanılabilir. Kurtarma Hizmetleri aracısını ardından kopya yedekleme kasası ya da kurtarma Hizmetleri bu hesabı verilerden içeriğini kasa, bunlardan hangisi söz konusuysa. Sonraki zamanlanmış yedekleme süre içinde Azure Yedekleme aracısı ilk yedek kopyayı artımlı yedekleme gerçekleştirir.

> [!NOTE]
> Aşağıdaki bölümlerde Azure Disk Hazırlama aracı erişimi olmayan önceki sürümlerinden Azure Yedekleme'nin kullanıcılar için geçerlidir.
>
>

### <a name="prepare-a-sata-drive"></a>SATA sürücüyü hazırlama
1. Karşıdan [Microsoft Azure içeri/dışarı aktarma aracı](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) kopya bilgisayarı için. Hazırlama konumu sonraki komut kümesini çalıştırmayı planladığınız bilgisayardan erişilebilir olduğundan emin olun. Gerekirse, kopya bilgisayar kaynak bilgisayarla aynı olabilir.

2. WAImportExport.zip dosyanın sıkıştırmasını açın. SATA sürücü biçimleri, yedekleme verilerini SATA sürücüye yazar ve bunu şifreler WAImportExport aracını çalıştırın. Aşağıdaki komutu çalıştırmadan önce BitLocker'ı bilgisayarda etkinleştirildiğinden emin olun. <br/>

    `*.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*`

    > [!NOTE]
    > Ağustos 2016 güncelleştirme Azure Yedekleme'nin (veya üstü) yüklü değilse, girdiğiniz hazırlama konumu aynı üzerinde olduğundan emin olun **Şimdi Yedekle** ekranında ve AIB ve temel Blob dosyalarını içerir.
    >
    >

| Parametre | Açıklama |
| --- | --- |
| /j: <*JournalFile*> |Günlük dosyası yolu. Her bir sürücü tam olarak bir günlük dosyası olmalıdır. Günlük dosyası hedef sürücüde olmaması gerekir. Günlük dosyası uzantısının .jrn ve bu komutu çalıştıran bir parçası olarak oluşturulur. |
| /ID: <*SessionID*> |Oturum kimliği kopyalama oturumu tanımlar. Kesintiye uğramış kopyalama oturumu doğru kurtarma sağlamak için kullanılır. Bir kopyalama oturumda kopyalanır dosyaları hedef sürücüde oturum kimliği sonra adlı bir dizinde depolanır. |
| /SK: <*StorageAccountKey*> |Verilerin içe aktarıldığı depolama hesabı için hesap anahtarı. Anahtarı yedekleme İlkesi/koruma grubu oluşturma sırasında girilen aynı olması gerekir. |
| / BlobType |Blob türü. Yalnızca bu iş akışı başarılı **PageBlob** belirtilir. Bu varsayılan seçeneği değildir ve bu komutta belirtilen. |
| / t: <*TargetDriveLetter*> |Sürücü harfini izleyen iki nokta geçerli kopyalama oturumu için hedef sabit disk olmadan. |
| Format |Sürücüyü biçimlendirmek için seçeneği. Sürücünün biçimlendirilmesi gerektiğinde bu parametreyi belirtin; Aksi takdirde yok sayın. Aracı sürücü biçimleri önce konsoldan onay ister. Onay gizlemek için /silentmode parametresini belirtin. |
| / şifrele |Sürücü Şifreleme seçeneği. Sürücü henüz BitLocker ile şifrelenmiş değil ve aracı tarafından şifrelenmiş gerektiğinde bu parametreyi belirtin. Sürücünün BitLocker ile şifrelenmiş, bu parametreyi, /bk parametresini belirtin ve varolan BitLocker anahtarını girin. De belirtmeniz gerekir Format parametresini belirtirseniz, / parametre şifreleyin. |
| /srcdir: <*SourceDirectory*> |Hedef sürücüye kopyalanması için dosyaları içeren kaynak dizini. Belirtilen dizin adı bir tam yerine göreli yol olduğundan emin olun. |
| /dstdir: <*DestinationBlobVirtualDirectory*> |Azure depolama hesabınızdaki hedef sanal dizin yolu. Hedef sanal dizinleri ve blobları belirttiğinizde geçerli kapsayıcı adları kullandığınızdan emin olun. Kapsayıcı adları küçük harfli olması gerektiğini unutmayın.  Bu kapsayıcı adı yedekleme İlkesi/koruma grubu oluşturma sırasında girilen biri olması gerekir. |

> [!NOTE]
> Bir günlük dosyası, iş akışının tamamı bilgilerini yakalar WAImportExport klasöründe oluşturulur. Azure portalında bir alma işi oluşturduğunuzda bu dosya gerekir.
>
>

  ![PowerShell çıkışı](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-the-azure-portal"></a>Azure portalında bir içeri aktarma işi oluşturma
1. Depolama hesabınıza gidin [Azure portal](https://ms.portal.azure.com/), tıklatın **içeri/dışarı aktarma**ve ardından **alma işi oluştur** görev bölmesinde.

    ![Azure portalında içeri/dışarı aktarma sekmesi](./media/backup-azure-backup-import-export/azureportal.png)

2. 1. adımda Sihirbazı'nın sürücünüze hazırladıktan ve sürücü günlük dosyasının kullanılabilir olduğunu gösterir.

3. 2. adımda sihirbazın, bu içeri aktarma işi için sorumlu kişi için kişi bilgilerini sağlayın.

4. 3. adımda, önceki bölümde edindiğiniz sürücü günlük dosyalarını karşıya yükleyin.

5. 4. adımda, yedekleme İlkesi/koruma grubu oluşturma sırasında girilen içe aktarma işi için açıklayıcı bir ad girin. Girdiğiniz ad içerebilir yalnızca küçük harfler, sayılar, tire ve alt çizgi, bir harf ile başlamalı ve boşluk içermemelidir. Seçtiğiniz ad ilerleme ve tamamlandıktan sonra çalışırken işlerinizi izlemek için kullanılır.

6. Ardından, veri merkezi bölgenizi listeden seçin. Veri Merkezi bölge datacenter ve paketinizi hazırlamalısınız adresini gösterir.

    ![Veri Merkezi bölgeyi seçin](./media/backup-azure-backup-import-export/dc.png)

7. 5. adım, dönüş taşıyıcı listeden seçin ve taşıyıcı hesap numaranızı girin. Microsoft, içe aktarma işi tamamlandıktan sonra sürücülerinizin sizin sevk etmek için bu hesabı kullanır.

8. Disk sevk ve sevkiyat durumunu izlemek için izleme numarası girin. Disk veri merkezinde ulaştıktan sonra depolama hesabına kopyalanır ve durumu güncelleştirilir.

    ![Tamamlanma durumu](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-the-workflow"></a>İş akışını tamamla
İlk yedek verileri depolama hesabınızdaki kullanılabilir olduktan sonra Microsoft Azure kurtarma Hizmetleri Aracısı'nı veri içeriğini bu hesaptan yedekleme Kasası'na kopyalar veya kurtarma Hizmetleri kasası, bunlardan hangisi söz konusuysa. Sonraki yedekleme zaman içinde Azure Yedekleme aracısı ilk yedek kopyayı artımlı yedekleme gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure içeri/dışarı aktarma iş akışı üzerinde herhangi bir sorunuz için başvurmak [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanma](../storage/common/storage-import-export-service.md).
* Çevrimdışı Yedekleme bölümüne Azure yedekleme bakın [SSS](backup-azure-backup-faq.md) iş akışı hakkında herhangi bir sorunuz için.
