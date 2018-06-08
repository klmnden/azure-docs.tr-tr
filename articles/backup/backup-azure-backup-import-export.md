---
title: Azure Backup - Çevrimdışı Yedekleme veya dengeli ilk Azure içeri/dışarı aktarma hizmeti kullanma
description: Azure Backup nasıl Azure içeri/dışarı aktarma hizmetini kullanarak ağ verileri göndermenize olanak sağlayan bilgi edinin. Bu makalede, çevrimdışı ilk yedek verilerini Azure içe aktarma dışarı aktarma hizmetini kullanarak dengeli dağıtımı açıklanmaktadır.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: saurse
ms.openlocfilehash: 5ef44ccf87bc5e40b57dc7fc997c9a827c93484b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831472"
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Azure Backup’ta çevrimdışı yedekleme iş akışı
Azure yedekleme verileri azure'a ilk tam yedeklemesi sırasında ağ ve depolama maliyet tasarrufu birkaç yerleşik verimliliği sahiptir. İlk tam yedekleme tipik olarak büyük miktarlarda veri aktarır ve yalnızca farkları/incrementals aktarım sonraki yedeklemeleri karşılaştırıldığında daha fazla ağ bant genişliği gerektirir. Çevrimdışı dengeli sürecinde, Azure yedekleme diskleri Çevrimdışı Yedekleme verileri Azure'a yüklemek için kullanabilirsiniz.

Azure işlem dengeli çevrimdışı ile tümleşik sıkı bir şekilde yedekleme [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) diskleri kullanarak Azure'a ilk yedek verileri aktarın olanak sağlar. Yüksek gecikmeli ve düşük bant genişlikli ağ üzerinden aktarılması gereken ilk yedek verileri terabayt (TBs) varsa, bir veya daha fazla sabit sürücülerde, Azure veri merkezi için ilk yedek kopyayı dağıtmayı dengeli çevrimdışı iş akışı kullanabilirsiniz. Aşağıdaki görüntü iş akışı'ndaki adımları genel bir bakış sağlar.

  ![Çevrimdışı içeri aktarma iş akışı işlemine genel bakış](./media/backup-azure-backup-import-export/offlinebackupworkflowoverview.png)

Çevrimdışı Yedekleme işlemi aşağıdaki adımları içerir:

1. Yedekleme verilerini ağ üzerinden göndermek yerine, yedekleme verilerinin hazırlama konumuna yazma.
2. Bir veya daha fazla SATA diskleri için hazırlama konumda veri yazmak için AzureOfflineBackupDiskPrep yardımcı programını kullanın.
3. Hazırlık çalışması bir parçası olarak, AzureOfflineBackupDiskPrep yardımcı programı bir Azure içe aktarma işi oluşturur. SATA sürücülerini en yakın Azure veri merkezine gönderin ve etkinlikleri bağlanmak için içeri aktarma işi başvuru.
4. Azure veri merkezi yer alan disklerdeki verileri bir Azure depolama hesabına kopyalanır.
5. Azure yedekleme yedekleme verilerini depolama hesabından kurtarma Hizmetleri Kasası'na kopyalar ve artımlı yedeklemeler zamanlanır.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar 
Aşağıdaki Azure yedekleme özellikleri veya iş yükleri Çevrimdışı Yedekleme kullanımını destekler.

> [!div class="checklist"]
> * Yedekleme dosyaları ve klasörleri Microsoft Azure kurtarma Hizmetleri (MARS) aracısıyla Azure Yedekleme aracısı olarak da bilinir. 
> * Yedekleme, tüm iş yükleri ve dosyalar ile System Center Data Protection Manager (SC DPM) 
> * Microsoft Azure yedekleme sunucusu ile tüm iş yükleri ve dosyalarını yedekleme <br/>

   > [!NOTE]
   > Çevrimdışı Yedekleme Azure Backup Aracısı kullanılarak gerçekleştirilen sistem durumu yedeklemeleri için desteklenmiyor. 

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Önkoşullar

  > [!NOTE]
  > Aşağıdaki önkoşulları ve iş akışı yalnızca çevrimdışı yedeğini kullanarak dosya ve klasörleri için geçerli [MARS Aracısı en son sürümünü](https://aka.ms/azurebackup_agent). Çevrimdışı Yedekleme iş yükleri için System Center DPM veya Azure yedekleme sunucusu kullanarak gerçekleştirmek için başvurmak [bu makalede](backup-azure-backup-server-import-export-.md). 

Çevrimdışı Yedekleme iş akışı başlatmadan önce aşağıdaki önkoşulların tamamlayın: 
* Oluşturma bir [kurtarma Hizmetleri kasası](backup-azure-recovery-services-vault-overview.md). Bir kasa oluşturmak için adımlarda bakın [bu makalede](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Emin olun, yalnızca [Azure Yedekleme aracısı en son sürümünü](https://aka.ms/azurebackup_agent) Windows Server/Windows istemcisinde, geçerli olarak yüklü ve bilgisayarı kurtarma Hizmetleri kasasına kayıtlı.
* Azure PowerShell 3.7.0 Azure Backup aracısını çalıştıran bilgisayar üzerindeki gereklidir. Karşıdan yüklediğiniz önerilir ve [Azure PowerShell sürümünü yüklemeye 3.7.0](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017).
* Azure Backup aracısını çalıştıran bilgisayarda Microsoft Edge veya Internet Explorer 11 yüklü ve JavaScript etkinleştirildiğinden emin olun. 
* Kurtarma Hizmetleri kasası ile aynı abonelikte bir Azure depolama hesabı oluşturun. 
* Olduğundan emin olun [gerekli izinleri](../azure-resource-manager/resource-group-create-service-principal-portal.md) Azure Active Directory uygulaması oluşturmak için. Çevrimdışı Yedekleme iş akışı, Azure depolama hesabıyla ilişkili Abonelikteki bir Azure Active Directory uygulaması oluşturur. Uygulama Azure Backup Çevrimdışı Yedekleme iş akışı için gereken Azure içeri aktarma hizmeti ile güvenli ve kapsamlı erişim sağlamak için hedefidir. 
* Microsoft.ImportExport kaynak sağlayıcısı Azure depolama hesabı içeren aboneliğiniz ile kaydeder. Kaynak sağlayıcısını kaydetmek için:
    1. Ana menüye tıklayın **abonelikleri**.
    2. Birden çok abone olunan Çevrimdışı Yedekleme için kullanmakta olduğunuz abonelik seçin. Yalnızca bir abonelik kullanırsanız, aboneliğinizin görüntülenir.
    3. Abonelik menüye tıklayın **kaynak sağlayıcıları** sağlayıcıları listesini görüntülemek için.
    4. Microsoft.ImportExport sağlayıcıları kaydırın listesinde. NotRegistered durumundaysa tıklatın **kaydetmek**.
    ![Kaynak sağlayıcısı kaydediliyor](./media/backup-azure-backup-import-export/registerimportexport.png)
* Bir ağ paylaşımına veya bilgisayarda, iç veya dış, ek herhangi sürücü ilk kopyanızı tutmak için yeterli disk alanına sahip olabilen bir hazırlama konumu oluşturulur. Örneğin, 500 GB dosya sunucusunu yedeklemek çalışıyorsanız, hazırlama alanı en az 500 GB olduğundan emin olun. (Daha küçük miktarda sıkıştırma nedeniyle kullanılır.)
* Diskler için Azure gönderirken, yalnızca 2,5 inç SSD veya 2,5 inç veya 3,5 inç SATA II/III dahili sabit sürücüler kullanın. Sabit disk sürücüleri kullanan 10 TB'a kadar. Denetleme [Azure içeri/dışarı aktarma hizmeti belgeleri](../storage/common/storage-import-export-requirements.md#supported-hardware) hizmetini destekleyen sürücüler son kümesi için.
* SATA sürücülerini bir bilgisayara bağlanmalıdır (olarak adlandırılan bir *kopya bilgisayar*) nerede gelen yedekleme verilerinin kopyasını *hazırlama konumuna* SATA sürücülerini yapılır. BitLocker'ı üzerinde etkinleştirildiğinden emin olun *kopya bilgisayar*.

## <a name="workflow"></a>İş akışı
Bu bölümde, verilerinizi Azure veri merkezi için teslim ve Azure depolama alanına karşıya Çevrimdışı Yedekleme iş akışı açıklanır. İçeri aktarma hizmeti veya herhangi bir değişiklik işlemi, hakkında sorularınız varsa bkz [alma hizmeti genel bakış belgeleri](../storage/common/storage-import-export-service.md).

## <a name="initiate-offline-backup"></a>Çevrimdışı Yedekleme başlatmak
1. MARS aracısı üzerinde bir yedekleme işini zamanlamak için aşağıdaki ekranı görürsünüz.

    ![İçeri aktarma ekran](./media/backup-azure-backup-import-export/offlinebackup_inputs.png)

  Girdi açıklaması aşağıdaki gibidir:

    * **Hazırlama konumu**: ilk yedek kopyanın yazılır geçici depolama konumu. Hazırlama konumu, bir ağ paylaşımına veya yerel bilgisayarda olabilir. Kaynak bilgisayar ve kopya bilgisayar farklıysa, hazırlama konumu tam ağ yolunu belirtin önerilir.
    * **Azure Resource Manager depolama hesabı**: Resource Manager depolama hesabı türü herhangi bir Azure aboneliği içindeki adı.
    * **Azure depolama kapsayıcısının**: Burada yedekleme verileri alınır kurtarma Hizmetleri Kasası'na kopyalanmadan önce Azure depolama hesabındaki hedef depolama blob adı.
    * **Azure abonelik kimliği**: Azure depolama hesabının oluşturulduğu Azure abonelik kimliği.
    * **Azure içeri aktarma iş adı**: hangi Azure alma tarafından hizmet ve Azure yedekleme izlemek gönderilen veri aktarımını disklerde Azure için benzersiz bir ad. 
  
  Giriş ekranındaki ve tıklayın **sonraki**. Sağlanan kaydetme *hazırlama konumu* ve *Azure içeri aktarma iş adı*gibi diskler hazırlamak için bu bilgiler gereklidir.

2. İstendiğinde, Azure aboneliğinizde oturum açın. Böylece Azure Backup Azure Active Directory uygulaması oluşturma ve Azure içe aktarma hizmete erişmek için gerekli izinleri sağlayan oturum gerekir.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/azurelogin.png)

3. İş akışını tamamlamak ve Azure Yedekleme aracısı konsolunda **Şimdi Yedekle**.

    ![Şimdi Yedekle](./media/backup-azure-backup-import-export/backupnow.png)

4. Sihirbaz onay sayfasına tıklayın **yedekleme**. İlk yedekleme hazırlama alanı kurulumunun bir parçası olarak yazılır.

   ![Artık yedekleme hazırsınız olduğunu onaylayın](./media/backup-azure-backup-import-export/backupnow-confirmation.png)

    İşlemi tamamlandıktan sonra hazırlama konumu disk hazırlığı için kullanılmak üzere hazırdır.

   ![Şimdi Yedekle](./media/backup-azure-backup-import-export/opbackupnow.png)

## <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA sürücülerini hazırlamak ve Azure'a sevk
*AzureOfflineBackupDiskPrep* yardımcı programı en yakın Azure veri merkezine gönderilir SATA sürücülerini hazırlar. Bu yardımcı program (aşağıdaki yolundaki) Azure Yedekleme aracısı yükleme dizininde bulunur:

   *\Microsoft azure kurtarma Hizmetleri Agent\Utils\\*

1. Kopyalama ve dizin gidin **AzureOfflineBackupDiskPrep** SATA sürücülerini bağlandığı başka bir bilgisayara dizin. Bağlı SATA sürücülerini içeren bilgisayarda emin olun:

    * Kopya bilgisayar çevrimdışı dengeli dağıtımı iş akışı için hazırlama konumu olarak sağlanan aynı ağ yolunu kullanarak erişebilirsiniz **başlatmak Çevrimdışı Yedekleme** iş akışı.
    * BitLocker kopyalama bilgisayarda etkin.
    * Azure PowerShell 3.7.0 yüklenir.
    * En son uyumlu tarayıcılar (kenar veya Internet Explorer 11) yüklü ve JavaScript etkindir. 
    * Kopya bilgisayar Azure portalına erişebilir. Gerekirse, kopya bilgisayar kaynak bilgisayarla aynı olabilir.
    
    > [!IMPORTANT] 
    > Kaynak bilgisayar bir sanal makine ise, kopya bilgisayar farklı bir fiziksel sunucu veya istemci makine kaynak bilgisayardan olmalıdır.

2. Kopya bilgisayarda yükseltilmiş bir komut istemi açın *AzureOfflineBackupDiskPrep* yardımcı programı dizini olarak geçerli dizinin ve aşağıdaki komutu çalıştırın:

    ```.\AzureOfflineBackupDiskPrep.exe s:<Staging Location Path>```

    | Parametre | Açıklama |
    | --- | --- |
    | s:&lt;*hazırlama konumu yolu*&gt; |Zorunlu giriş girdiğiniz hazırlama konumu yolunu sağlamak için kullanılan **başlatmak Çevrimdışı Yedekleme** iş akışı. |
    | p:&lt;*PublishSettingsFile yolu*&gt; |Yolunu sağlamak için kullanılan isteğe bağlı giriş **Azure yayımlama ayarları** girdiğiniz dosya **başlatmak Çevrimdışı Yedekleme** iş akışı. |

    Komutu çalıştırdığınızda, yardımcı program seçimi hazırlanması gerekir sürücülere karşılık gelen Azure Alma işinin ister. Yalnızca bir tek alma işi sağlanan hazırlama konumu ile ilişkili olan, izleyen bir gibi bir ekran görürsünüz.

    ![Azure Disk Hazırlık Aracı giriş](./media/backup-azure-backup-import-export/diskprepconsole0_1.png) <br/>

3. Transfer Azure için hazırlamak istediğiniz bağlı diski için sürücü harfini izleyen iki nokta üst üste olmadan girin. 
4. İstendiğinde sürücüsünü biçimlendirmek için onay sağlayın.
5. Azure aboneliğinizde oturum açmanız istenir. Kimlik bilgilerinizi sağlayın.

    ![Azure Disk Hazırlık Aracı giriş](./media/backup-azure-backup-import-export/signindiskprep.png) <br/>

    Aracı, disk ve yedekleme verileri kopyalayarak hazırlamak sonra başlar. Sağlanan disk yedekleme verileri için yeterli alanı yok durumunda aracı tarafından istendiğinde ilave diskler eklemeniz gerekebilir. <br/>

    Aracının başarılı yürütme sonunda, komut istemini üç parça bilgi sağlar:
    1. Sağladığınız bir veya daha fazla disk Azure dağıtımına için hazırlanır. 
    2. İçe aktarma işi oluşturuldu onaylamanız istenir. İçe aktarma işi belirttiğiniz ad kullanır.
    3. Aracı Azure veri merkezi için teslimat adresi görüntüler.

    ![Azure disk hazırlığı tamamlandı](./media/backup-azure-backup-import-export/console2.png)<br/>

6. Komut yürütme sonunda sevkiyat bilgilerini güncelleştirebilirsiniz.

7. Aracı sağlanan adres disklere sevk ve gelecekte başvurmak için izleme numarası tutun.

   > [!IMPORTANT] 
   > İki Azure içeri aktarma işi aynı izleme numarasına sahip olabilir. Tek bir Azure alma işi altında yardımcı programı tarafından hazırlanan sürücüleri tek bir paket birlikte gönderilir ve paketi için tek benzersiz izleme numarası olduğundan emin olun. Tek bir pakette ayrı Azure içe aktarma işlerinin parçası olarak hazırlanan sürücüleri birleştirmeyin.



## <a name="update-shipping-details-on-the-azure-import-job"></a>Azure Alma işinin sevkiyat ayrıntılarını güncelleştir

Aşağıdaki yordamda Azure içe aktarma işi sevkiyat ayrıntılarını güncelleştirir. Bu bilgiler hakkında ayrıntılar içerir:
* Azure diskleri sunar taşıyıcı adı
* Diskleriniz için sevkiyat ayrıntılarını döndürür

1. Azure aboneliğinizde oturum açın.
2. Ana menüye tıklayın **tüm hizmetleri** ve tüm hizmetleri iletişim kutusunda, içeri aktarma yazın. Gördüğünüzde **içeri/dışarı aktarma işleri**, tıklatın.
    ![Sevkiyat bilgileri girme](./media/backup-azure-backup-import-export/search-import-job.png)<br/>

    Listesini **içeri/dışarı aktarma işleri** menüsünü açar ve seçili Abonelikteki tüm içeri/dışarı aktarma işlerinin listesi görüntülenir. 

3. Birden çok aboneliğiniz varsa, yedekleme verilerini almak için kullanılan abonelik seçtiğinizden emin olun. Ardından ayrıntılarını açmak için yeni oluşturulan alma işi seçin.

    ![Sevkiyat bilgileri gözden geçirin](./media/backup-azure-backup-import-export/import-job-found.png)<br/>

4. İçe aktarma işi için ayarlar menüsünde **yönetmek Sevkiyat bilgisi** ve dönüş sevkiyat ayrıntılarını girin.

    ![Bilgi Kargo depolanması](./media/backup-azure-backup-import-export/shipping-info.png)<br/>

5. Sevkiyat, taşıyıcı izleme numarası varsa, Azure alma işi genel bakış sayfasında başlığını tıklatın ve aşağıdaki ayrıntıları girin:

   > [!IMPORTANT] 
   > Azure alma işi oluşturmanın iki hafta içinde Taşıyıcı bilgileri ve izleme numarası güncelleştirildiğinden emin olun. İki hafta içinde bu bilgileri doğrulamak için hata silindiğinden iş ve işlenmeyen sürücüleri neden olabilir.

   ![Bilgi Kargo depolanması](./media/backup-azure-backup-import-export/joboverview.png)<br/>

   ![Bilgi Kargo depolanması](./media/backup-azure-backup-import-export/tracking-info.png)

### <a name="time-to-process-the-drives"></a>Sürücüleri işlemek için süre 
Gereken bir Azure alma işi değişir Sevkiyat Zamanı gibi farklı etkenlere bağlı olarak işlenecek süreyi iş türü, türü ve kopyalanan verilerin boyutunu ve sağlanan disk boyutu. Azure içeri/dışarı aktarma hizmeti bir SLA yok ancak diskleri alındıktan sonra hizmet 7-10 gün içinde Azure storage hesabınıza yedek veri kopyalama tamamlamak için çalışır. 

### <a name="monitoring-azure-import-job-status"></a>Azure Alma işinin durumunu izleme
Giderek Azure portalından, içeri aktarma işinin durumunu izleyebilirsiniz **içeri/dışarı aktarma işleri** sayfası ve işinizi seçme. İçe aktarma işlerinin durumu hakkında daha fazla bilgi için bkz: [depolama alma dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) makalesi.

### <a name="complete-the-workflow"></a>İş akışını tamamla
Alma işlemi başarıyla tamamlandıktan sonra ilk yedek verileri depolama hesabınızdaki kullanılabilir. Sonraki zamanlanmış yedekleme zaman Azure yedekleme içeriğini veri depolama hesabından aşağıda gösterildiği gibi kurtarma Hizmetleri kasasına kopyalar: 

   ![Veri kurtarma Hizmetleri Kasası'na kopyalama](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Sonraki zamanlanmış yedekleme zaman Azure yedekleme artımlı yedekleme gerçekleştirir.

### <a name="cleaning-up-resources"></a>Kaynakları temizleme
İlk Yedekleme tamamlandıktan sonra Azure depolama kapsayıcısı ve hazırlama konumu yedekleme verilerinde alınan verileri güvenle silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Azure içeri/dışarı aktarma iş akışı üzerinde herhangi bir sorunuz için başvurmak [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanma](../storage/common/storage-import-export-service.md).
* Çevrimdışı Yedekleme bölümüne Azure yedekleme bakın [SSS](backup-azure-backup-faq.md) iş akışı hakkında herhangi bir sorunuz için.
