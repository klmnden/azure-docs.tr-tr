---
title: Azure Backup - Çevrimdışı Yedekleme veya Azure içeri/dışarı aktarma hizmetini kullanarak ilk dengeli dağıtım
description: Azure Backup, Azure içeri/dışarı aktarma hizmetini kullanarak ağ kapalı veri göndermek nasıl olanak tanıdığını öğrenin. Bu makalede Azure içeri dışarı aktarma hizmetini kullanarak üretme, çevrimdışı ilk yedekleme verilerini açıklanmaktadır.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: saurse
ms.openlocfilehash: b6f0ce1939b2a78ca191d2feb0140506d130b9b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60648423"
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Azure Backup’ta çevrimdışı yedekleme iş akışı
Azure Backup, azure'a veri ilk tam yedekleme sırasında ağ ve depolama maliyetlerinden tasarruf birkaç yerleşik verimliliği sahiptir. Genellikle ilk tam yedeklemeler büyük miktarlarda veri aktarmanıza ve yalnızca deltaları/incrementals aktarmak sonraki yedeklemelerle karşılaştırıldığında daha fazla ağ bant genişliği gerektirecektir. Çevrimdışı dengeli dağıtım işlemi boyunca, Azure Backup diskler Çevrimdışı Yedekleme verileri Azure'a karşıya yüklemek için kullanabilirsiniz.

Azure çevrimdışı dengeli dağıtım işlemi ile sıkı bir şekilde tümleşmiş yedekleme [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) diskleri kullanarak ilk yedek verileri Azure'a aktarmak etkinleştirir. Yüksek gecikme ve düşük bant genişliğine sahip ağ üzerinden aktarılması gereken ilk yedek verileri (TB'a) terabaytlarca varsa, bir veya daha fazla sabit sürücülerde, Azure veri merkezi için ilk yedek kopyanın göndermeye çevrimdışı dengeli dağıtım iş akışı kullanabilirsiniz. Aşağıdaki görüntüde, iş akışındaki adımlar genel bir bakış sağlar.

  ![Çevrimdışı içeri aktarma iş akışı işlemine genel bakış](./media/backup-azure-backup-import-export/offlinebackupworkflowoverview.png)

Çevrimdışı Yedekleme işlemi aşağıdaki adımları içerir:

1. Yedekleme verileri ağ üzerinden göndermek yerine, yedekleme verileri için bir hazırlama konumu yazın.
2. Verileri bir veya daha fazla SATA diskleri hazırlama konumuna yazılacak AzureOfflineBackupDiskPrep yardımcı programını kullanın.
3. Hazırlık iş işleminin bir parçası olarak AzureOfflineBackupDiskPrep yardımcı programı, Azure içeri aktarma işi oluşturur. SATA sürücülerini en yakın Azure veri merkezine gönderin ve içeri aktarma işi etkinliklerini bağlama başvuru.
4. Azure veri merkezinde, Azure depolama hesabınız için verileri diske kopyalanır.
5. Azure yedekleme yedekleme verileri depolama hesabından kurtarma Hizmetleri Kasası'na kopyalar ve artımlı yedeklemeler zamanlanır.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar 
Aşağıdaki Azure Backup özellikleri veya iş yükleriniz Çevrimdışı Yedekleme kullanımını destekler.

> [!div class="checklist"]
> * Dosya ve klasörleri Azure Backup aracısı olarak da bilinir, Microsoft Azure kurtarma Hizmetleri (MARS) aracısı ile yedekleme. 
> * Yedekleme, tüm iş yükleri ve dosyaları ile System Center Data Protection Manager (DPM SC) 
> * Microsoft Azure Backup sunucusu ile tüm iş yükleri ve dosyalarını yedekleme <br/>

   > [!NOTE]
   > Çevrimdışı Yedekleme Azure yedekleme ajanını kullanarak yapılan sistem durumu yedeklemeleri için desteklenmiyor. 

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Önkoşullar

  > [!NOTE]
  > Yalnızca dosya ve klasörlerin kullanarak çevrimdışı yedekleme için aşağıdaki önkoşulları ve iş akışı uygulama [en yeni MARS Aracısı](https://aka.ms/azurebackup_agent). Çevrimdışı Yedekleme iş yükleri için System Center DPM veya Azure Backup sunucusu kullanarak gerçekleştirmek için başvurmak [bu makalede](backup-azure-backup-server-import-export-.md). 

Çevrimdışı Yedekleme iş akışı başlatmadan önce aşağıdaki önkoşulları tamamlayın: 
* Oluşturma bir [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md). Bir kasa oluşturmak için adımları bakın [bu makalede](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Emin olun, sadece [Azure yedekleme aracısının en son sürümünü](https://aka.ms/azurebackup_agent) uygunsa Windows Server/Windows istemcide yüklü olduğundan ve bilgisayarı kurtarma Hizmetleri kasasına kayıtlı.
* Azure PowerShell 3.7.0 Azure Backup Aracısı'nı çalıştıran bilgisayarda gereklidir. İçin indirmeniz önerilir ve [Azure PowerShell sürümünü yükleyin 3.7.0](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017).
* Azure Backup Aracısı'nı çalıştıran bilgisayarda Microsoft Edge veya Internet Explorer 11 yüklü ve JavaScript'in etkin olduğundan emin olun. 
* Bir Azure depolama hesabı aynı abonelikte kurtarma Hizmetleri kasası oluşturun. 
* Olduğundan emin olun [gerekli izinleri](../active-directory/develop/howto-create-service-principal-portal.md) Azure Active Directory uygulaması oluşturma. Çevrimdışı Yedekleme iş akışı Azure depolama hesabıyla ilişkili aboneliği bir Azure Active Directory uygulaması oluşturur. Uygulama Çevrimdışı Yedekleme iş akışı için gereken Azure içeri aktarma hizmeti, Azure Backup ile güvenli ve kapsamlı erişim sağlamak için hedefidir. 
* Azure depolama hesabını içeren aboneliği ile Microsoft.ImportExport kaynak sağlayıcısını kaydedin. Kaynak sağlayıcısını kaydetmek için:
    1. Ana menüde **abonelikleri**.
    2. Birden çok abone olunan, Çevrimdışı Yedekleme için kullanmakta olduğunuz aboneliği seçin. Yalnızca bir abonelik kullanın, ardından aboneliğinizi görünür.
    3. Abonelik menüden **kaynak sağlayıcıları** sağlayıcıları listesini görüntülemek için.
    4. Microsoft.ImportExport kaydırın sağlayıcıları listesi. Durum NotRegistered ise, tıklayın **kaydetme**.
    ![Kaynak sağlayıcısını kaydetme](./media/backup-azure-backup-import-export/registerimportexport.png)
* Bir ağ paylaşımına veya herhangi başka bir sürücü bilgisayarında, iç veya dış, ilk kopyanızı tutmak için yeterli disk alanına sahip olabilecek bir hazırlama konumu oluşturulur. Örneğin, 500 GB'lık dosya sunucusunu yedeklemek çalışıyorsanız, hazırlama alanına en az 500 GB olduğundan emin olun. (Daha küçük bir miktarı, sıkıştırma nedeniyle kullanılır.)
* Diskleri Azure'a gönderirken, yalnızca SSD 2,5 inç veya 2.5 inç veya 3,5 inçlik SATA II/III iç sabit sürücüsü kullanın. Sabit sürücüleri kullanabileceğiniz 10 TB'a kadar artırın. Denetleme [Azure içeri/dışarı aktarma hizmeti belgeleri](../storage/common/storage-import-export-requirements.md#supported-hardware) hizmetinin desteklediği sürücüleri son kümesi.
* SATA sürücülerini bir bilgisayara bağlanmalıdır (olarak adlandırılan bir *kopya bilgisayar*) nereden gelen yedekleme verilerin kopyasını *hazırlama konumuna* için SATA sürücülerini gerçekleştirilir. Üzerinde BitLocker'ın etkin olduğundan emin olun *kopya bilgisayar*.

## <a name="workflow"></a>İş akışı
Bu bölümde, böylece verilerinizi Azure veri merkezi için sunulan ve Azure depolama alanına yüklenir Çevrimdışı Yedekleme iş akışı açıklanır. İçeri aktarma hizmeti veya işlemin herhangi bir özelliği ile ilgili sorularınız varsa bkz [içeri aktarma hizmeti genel bakış belgeleri](../storage/common/storage-import-export-service.md).

## <a name="initiate-offline-backup"></a>Çevrimdışı Yedekleme başlatmak
1. MARS aracısı üzerinde bir yedekleme işini zamanlamak için aşağıdaki ekranı görürsünüz.

    ![İçeri aktarma ekran](./media/backup-azure-backup-import-export/offlinebackup_inputs.png)

   Girişleri açıklaması aşağıdaki gibidir:

    * **Hazırlama konumu**: İlk yedek kopyanın yazıldığı geçici depolama konumu. Hazırlama konumu, bir ağ paylaşımına veya yerel bilgisayar üzerinde olabilir. Kopya bilgisayar ve kaynak bilgisayara farklıysa, hazırlama konumunun tam ağ yolu belirtmeniz önerilir.
    * **Azure Resource Manager depolama hesabı**: Herhangi bir Azure aboneliğinde depolama hesabı Resource Manager türü adı.
    * **Azure depolama kapsayıcısı**: Kurtarma Hizmetleri kasasına kopyalanmadan önce yedekleme verileri burada alınır Azure depolama hesabında hedef depolama blob adı.
    * **Azure abonelik kimliği**: Azure depolama hesabının oluşturulduğu Azure abonelik kimliği.
    * **Azure içeri aktarma işinin adını**: Tarafından hangi Azure içeri aktarma hizmeti ve Azure Backup gönderilen veri aktarımını disklerde Azure'a izlemek benzersiz adı. 
  
   ' A tıklayın ve giriş ekranındaki **sonraki**. Sağlanan kaydetme *hazırlama konumu* ve *Azure içeri aktarma işinin adını*gibi diskleri hazırlamak için bu bilgiler gereklidir.

2. İstendiğinde, Azure aboneliğinizde oturum açın. Azure Backup Azure Active Directory uygulaması oluşturma ve Azure içeri aktarma hizmeti erişmek için gerekli izinleri sağlayan oturum açmalısınız.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/azurelogin.png)

3. İş akışını tamamlayın ve Azure Backup Aracısı konsolunda **Şimdi Yedekle**.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/backupnow.png)

4. Sihirbazın onay sayfasında tıklayın **yedekleme**. İlk yedeklemeyi hazırlama alanı kurulumun bir parçası olarak yazılır.

   ![Artık yedekleme hazır olduğunuzdan emin olun](./media/backup-azure-backup-import-export/backupnow-confirmation.png)

    İşlem tamamlandıktan sonra hazırlama konumu disk hazırlığı için kullanılmak üzere hazırdır.

   ![Şimdi Yedekle](./media/backup-azure-backup-import-export/opbackupnow.png)

## <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA sürücülerini hazırlayıp Azure'a
*AzureOfflineBackupDiskPrep* yardımcı programı, en yakın Azure veri merkezine gönderilir SATA sürücülerini hazırlar. Bu yardımcı program (aşağıdaki yolda) Azure Backup Aracısı yükleme dizininde bulunur:

   *\Microsoft azure kurtarma Hizmetleri Agent\Utils\\*

1. Dizin ve kopya Git **AzureOfflineBackupDiskPrep** SATA disklerin bağlı olduğu başka bir bilgisayara dizin. Bağlı SATA sürücüsü olan bir bilgisayarda emin olun:

   * Kopya bilgisayar hazırlama konumu çevrimdışı dengeli dağıtım iş akışı için sunulan aynı ağ yolunu kullanarak erişebilirsiniz **çevrimdışı yedekleme başlatmak** iş akışı.
   * Kopyalama bilgisayarda BitLocker etkin.
   * Azure PowerShell 3.7.0 yüklenir.
   * En son uyumlu tarayıcıları (Microsoft Edge veya Internet Explorer 11) yüklü ve JavaScript'in etkinleştirilmiş. 
   * Kopya bilgisayar Azure portala erişebilirsiniz. Gerekirse, kopya bilgisayar kaynak bilgisayarla aynı olabilir.
    
     > [!IMPORTANT] 
     > Kaynak bilgisayar bir sanal makine ise, kopya bilgisayar farklı bir fiziksel sunucu veya istemci makinesinden kaynak bilgisayar olmalıdır.

2. Kopyalama bilgisayarda yükseltilmiş bir komut istemi açın *AzureOfflineBackupDiskPrep* yardımcı programı dizini olarak geçerli dizin ve şu komutu çalıştırın:

    ```.\AzureOfflineBackupDiskPrep.exe s:<Staging Location Path>```

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*hazırlama konumu yolu*&gt; |Girdiğiniz hazırlama konumunun yolunu sağlamak için kullanılan Zorunlu giriş **çevrimdışı yedekleme başlatmak** iş akışı. |
    | p:&lt;*PublishSettingsFile yolu*&gt; |Yolunu sağlamak için kullanılan isteğe bağlı bir giriş **Azure yayımlama ayarları** girdiğiniz dosya **çevrimdışı yedekleme başlatmak** iş akışı. |

    Komutu çalıştırdığınızda, yardımcı program hazırlanması gereken sürücülere karşılık gelen Azure Alma işinin seçimi ister. Yalnızca bir tek içeri aktarma işi sağlanan bir hazırlama konumu ile ilişkili olan izleyen benzer bir ekran görürsünüz.

    ![Azure Disk Hazırlık Aracı giriş](./media/backup-azure-backup-import-export/diskprepconsole0_1.png) <br/>

3. Azure'a aktarım için hazırlama istediğiniz bağlı diski için sürücü harfini izleyen iki nokta üst üste olmadan girin. 
4. İstendiğinde sürücüsünü biçimlendirmek için onay sağlayın.
5. Azure aboneliğinizde oturum açmanız istenir. Kimlik bilgilerinizi sağlayın.

    ![Azure Disk Hazırlık Aracı giriş](./media/backup-azure-backup-import-export/signindiskprep.png) <br/>

    Araç, disk ve yedekleme veri kopyalama hazırlamak sonra başlar. Sağlanan disk yedekleme verileri için yeterli alan yok durumunda araç tarafından istendiğinde ek diskler ekleme gerekebilir. <br/>

    Aracı başarılı yürütme sonunda, komut istemini üç parça bilgi sağlar:
   1. Sağladığınız bir veya daha fazla diski azure'a sevk için hazırlanır. 
   2. Onay, içeri aktarma işi oluşturulduktan alırsınız. İçeri aktarma işi, sağlanan adını kullanır.
   3. Araç, Azure veri merkezi için teslimat adresini görüntüler.

      ![Azure disk hazırlığı tamamlandı](./media/backup-azure-backup-import-export/console2.png)<br/>

6. Komut yürütme sonunda gönderim bilgilerinizi güncelleştirebilirsiniz.

7. Diskleri aracı sağlanan adresine gönderin ve gelecekte başvurmak için takip numarasını tutun.

   > [!IMPORTANT] 
   > İki Azure içeri aktarma iş aynı izleme numarası olabilir. Tek bir Azure alma işi altında yardımcı programı tarafından hazırlanan sürücüleri tek bir pakette birlikte gönderilir ve paketi için tek benzersiz izleme numarası olduğundan emin olun. Tek bir pakette ayrı Azure içeri aktarma işlerinde bir parçası olarak hazırlanan sürücüleri birleştirmeyin.



## <a name="update-shipping-details-on-the-azure-import-job"></a>Azure içeri aktarma işinin sevkiyat ayrıntılarını güncelleştirme

Aşağıdaki yordam, Azure içeri aktarma işi Sevkiyat ayrıntıları güncelleştirir. Bu bilgiler, ilgili ayrıntıları içerir:
* disklerini Azure'a gönderir taşıyıcı adı
* dönüş diskleriniz için Sevkiyat ayrıntıları

1. Azure aboneliğinizde oturum açın.
2. Ana menüde **tüm hizmetleri** ve tüm hizmetleri iletişim kutusunda, içeri aktarma yazın. Gördüğünüzde **içeri/dışarı aktarma işleri**, buna tıklayın.
    ![Sevkiyat bilgilerini girme](./media/backup-azure-backup-import-export/search-import-job.png)<br/>

    Listesini **içeri/dışarı aktarma işleri** menüsü açılır ve seçili Abonelikteki tüm içeri/dışarı aktarma işlerinin listesi görüntülenir. 

3. Birden fazla aboneliğiniz varsa, yedekleme verilerini aktarmak için kullanılan aboneliği seçin emin olun. Ardından ayrıntılarını açmak için yeni oluşturulan içeri aktarma işi seçin.

    ![Sevkiyat bilgileri gözden geçirin](./media/backup-azure-backup-import-export/import-job-found.png)<br/>

4. İçeri aktarma işi için ayarları menüsünde **yönetme iade sevkiyatı bilgileri** iade sevkiyatı ayrıntılarını girin.

    ![Sevkiyat bilgilerini depolama](./media/backup-azure-backup-import-export/shipping-info.png)<br/>

5. Sevkiyat operatörünüz izleme numarası varsa, Azure içeri aktarma işi genel bakış sayfasında başlığa tıklayın ve aşağıdaki ayrıntıları girin:

   > [!IMPORTANT] 
   > Azure içeri aktarma işini oluşturduktan itibaren iki hafta içinde taşıyıcı bilgilerini ve takip numarasını güncelleştirmeyi unutmayın. İki hafta içinde bu bilgileri doğrulamak için başarısız işi siliniyor ve sürücüleri işlenmeyen neden olabilir.

   ![Sevkiyat bilgilerini depolama](./media/backup-azure-backup-import-export/joboverview.png)<br/>

   ![Sevkiyat bilgilerini depolama](./media/backup-azure-backup-import-export/tracking-info.png)

### <a name="time-to-process-the-drives"></a>Sürücüleri işlem süresi 
Azure içeri aktarma işine değişir sevkiyat zamanını farklı etkene bağlı olarak işleme süresini miktarı iş türü, türü ve kopyalanan verilerin boyutlarına ve sağlanan disklerin boyutu. Azure içeri/dışarı aktarma hizmeti, bir SLA yoktur ancak diskleri alındıktan sonra hizmeti Azure depolama hesabınız için yedek veri kopyalama 7 ila 10 gün içinde tamamlanması üstlenmeye çalışır. 

### <a name="monitoring-azure-import-job-status"></a>Azure Alma işinin durumunu izleme
Giderek Azure portalında, içeri aktarma işinin durumunu izleyebilirsiniz **içeri/dışarı aktarma işleri** sayfası ve işinizi seçme. İçeri aktarma işlerinde durumu hakkında daha fazla bilgi için bkz. [depolama içeri dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) makalesi.

### <a name="complete-the-workflow"></a>İş akışını tamamlayın
İlk yedek verileri içeri aktarma işi başarıyla tamamlandıktan sonra depolama hesabınızdaki kullanılabilir. Sonraki zamanlanmış yedekleme sırasında Azure Backup içeriğini veri depolama hesabından aşağıda gösterildiği gibi kurtarma Hizmetleri kasasına kopyalar: 

   ![Kurtarma Hizmetleri Kasası'na veri kopyalama](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Sonraki zamanlanmış yedekleme zaman, artımlı yedekleme Azure yedekleme gerçekleştirir.

### <a name="cleaning-up-resources"></a>Kaynakları temizleme
İlk Yedekleme tamamlandıktan sonra Azure depolama kapsayıcısı ve yedekleme verileri hazırlama konumu, aktarılan veriler güvenli bir şekilde silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Azure içeri/dışarı aktarma iş akışındaki herhangi bir sorunuz için başvurmak [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmetini kullanmak](../storage/common/storage-import-export-service.md).
* Azure Backup'ın Çevrimdışı Yedekleme bölümüne başvurun [SSS](backup-azure-backup-faq.md) iş akışı hakkında tüm sorularınız için.
