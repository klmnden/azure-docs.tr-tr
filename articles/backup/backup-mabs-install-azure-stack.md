---
title: Azure Stack üzerinde Azure Backup Sunucusu'nu Yükle | Microsoft Docs
description: Dosya korumak veya Azure Stack'te iş yüklerini yedeklemek için Azure Backup sunucusu kullanma.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: d3a2ffdedda7f541fb1a3f37a8b40bc7af3dcb57
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996518"
---
# <a name="install-azure-backup-server-on-azure-stack"></a>Azure Stack üzerinde Azure Backup Sunucusu'nu yükleme

Bu makalede, Azure Stack'te Azure Backup Sunucusu'nu yüklemeye açıklanmaktadır. Azure Backup sunucusu ile altyapı olarak Azure Stack'te çalışan sanal makineler gibi bir hizmet (Iaas) iş yüklerini koruyabilir. Azure Backup sunucusu iş yüklerinizi korumak için kullanmanın bir avantajı, tüm iş yükü koruması tek bir konsoldan yönetebilir ' dir.

> [!NOTE]
> Güvenlik özellikleri hakkında bilgi edinmek için bkz [Azure Backup belgeleri özellikleri](backup-azure-security-feature.md).
>

## <a name="azure-backup-server-protection-matrix"></a>Azure Backup Sunucusu koruma matrisi
Azure Backup Sunucusu'nu aşağıdaki Azure Stack sanal makine iş yüklerini korur.

| Korumalı veri kaynağı | Koruma ve kurtarma |
| --------------------- | ----------------------- |
| Windows Server yarı yıllık kanal - Datacenter/Enterprise/standart | Birimler, dosyalar, klasörler |
| Windows Server 2016 - Datacenter/Enterprise/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 R2 - Datacenter/Enterprise/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 - Datacenter/Enterprise/standart | Birimler, dosyalar, klasörler |
| Windows Server 2008 R2 - Datacenter/Enterprise/standart | Birimler, dosyalar, klasörler |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2016 | Grup, veritabanı, ön uç, web sunucusu |
| SharePoint 2013 | Grup, veritabanı, ön uç, web sunucusu |
| SharePoint 2010 | Grup, veritabanı, ön uç, web sunucusu |

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Azure Backup sunucusu ortamı için Önkoşullar

Azure Backup sunucusu, Azure Stack ortamınıza yüklerken bu bölümdeki önerileri göz önünde bulundurun. Azure Backup sunucusu yükleyici, gerekli önkoşulları ortamınız var, ancak yüklemeden önce hazırlayarak zamandan tasarruf denetler.

### <a name="determining-size-of-virtual-machine"></a>Sanal makinenin boyutunu belirleme
Azure Backup sunucusu, bir Azure Stack sanal makinesinde çalıştırmak için A2 boyutu kullanın veya daha büyük. Bir sanal makine boyutu seçerken Yardım almak için indirme [Azure Stack sanal makine boyutu hesaplayıcı](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Azure Stack sanal makinelere sanal ağlar
Bir Azure Stack iş yükü içinde kullanılan tüm sanal makineler aynı Azure sanal ağ ve Azure aboneliğine ait olmalıdır.

### <a name="azure-backup-server-vm-performance"></a>Azure Backup sunucusu VM performansı
Diğer sanal makinelerle paylaşılırsa, depolama alanı boyut ve IOPS sınırları etkisi Azure Backup sunucusu VM performansı hesap. Bu nedenle, Azure Backup sunucusu sanal makine için ayrı bir depolama hesabı kullanmanız gerekir. Azure Backup sunucusu üzerinde çalışan Azure yedekleme aracısının, geçici depolama gerekir:
- kendi kullanımı (bir önbellek konumu)
- (yerel hazırlama alanı) buluttan geri yüklenen veriler

### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Backup geçici disk depolama yapılandırma
Her Azure Stack sanal makine toplu olarak kullanıcı kullanılabilir geçici disk depolama alanı ile birlikte gelen `D:\`. Azure Yedekleme'nin ihtiyaç duyduğu yerel hazırlama alanı içinde bulunacak şekilde yapılandırılabilir `D:\`, ve önbellek konumu üzerinde yerleştirilebilir `C:\`. Bu şekilde, hiçbir depolama Azure Backup sunucusu sanal makinesine bağlı veri diskleri uzağa gerekmez gerekir.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Yerel diskte ve azure'da yedek verilerinin depolanması
Azure Backup sunucusu yedekleme verileri, operasyonel kurtarma için sanal makineye bağlı Azure disklerine üzerine veri depolar. Azure Backup Sunucusu'na depolama diskleri ve depolama alanı sanal makineye bağlandıktan sonra sizin yerinize yönetir. Yedekleme verileri depolama alanından sayısına ve her birine bağlı disklerin boyutuna bağlıdır [Azure Stack sanal makine](/azure-stack/user/azure-stack-storage-overview). Her Azure Stack VM boyutu en fazla sanal makineye bağlı diskler vardır. Örneğin, dört diskleri A2 olur. A3 sekiz diskler ' dir. A4 16 disk var. Yeniden disk sayısı ve boyutu, toplam yedekleme depolama havuzunu belirler.

> [!IMPORTANT]
> Yapmanız gerekenler **değil** beş günden fazla bir süre için Azure Backup sunucusu bağlı disklerde operasyonel Kurtarma (Yedekleme) verileri korur.
>

Azure'da yedek verilerinin depolanması, Azure Stack yedekleme altyapısında azaltır. Veri beş günden daha eski ise, Azure'da depolanması gerekir.

Azure'a yedekleme verilerini depolamak için oluşturabilir veya bir kurtarma Hizmetleri kasası kullanabilirsiniz. Azure Backup sunucusu iş yükü ' üzere hazırlanırken, [kurtarma Hizmetleri kasasını yapılandırarak](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault). Bir yedekleme işinin çalışma, her zaman yapılandırıldıktan kasaya bir kurtarma noktası oluşturulur. Her bir kurtarma Hizmetleri kasası için en çok 9999 kurtarma noktalarını tutar. Oluşturulan kurtarma noktalarının ve ne kadar saklanır sayısına bağlı olarak, yıllardır yedekleme verileri koruyabilirsiniz. Örneğin, aylık kurtarma noktaları oluşturmak ve bunları beş yıl boyunca Beklet.
 
### <a name="scaling-deployment"></a>Dağıtım ölçeklendirme
Dağıtımınız ölçeklendirmek istiyorsanız, aşağıdaki seçenekleriniz:
  - Ölçeği artırma - D serisi serisinden Azure Backup sunucusu sanal makinenin boyutunu artırın ve yerel depolama alanını artırmak [Azure Stack sanal makine yönergelerine göre](/azure-stack/user/azure-stack-manage-vm-disks).
  - Veri boşaltma - eski verileri Azure'a göndermek ve Azure Backup Sunucusu'na bağlı depolama üzerinde yalnızca en yeni verileri korur.
  - -Daha fazla Azure Backup iş yüklerini korumak için sunucu eklemek ölçeği.

### <a name="net-framework"></a>.NET Framework

.NET framework 3.5 SP1 veya üzeri sanal makinede yüklü olmalıdır.

### <a name="joining-a-domain"></a>Bir etki alanına katılma

Azure Backup sunucusu sanal makine bir etki alanına katılması gerekir. Yönetici ayrıcalıklarına sahip bir etki alanı kullanıcısı Azure Backup sunucusu, sanal makineye yüklemeniz gerekir.

## <a name="using-an-iaas-vm-in-azure-stack"></a>Azure Stack'te bir Iaas VM'si kullanma

Azure Backup sunucusu için bir sunucu seçerken, bir Windows Server 2012 R2 Datacenter veya Windows Server 2016 Datacenter galeri görüntüsü ile başlayın. Makale [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), önerilen sanal makine ile Başlarken bir öğretici sağlar. Sanal makinede (VM) sunucusu için önerilen en düşük gereksinimleri olmalıdır: Standart a2 iki çekirdek ve 3,5 GB RAM.

Azure Backup sunucusu iş yüklerini koruma birçok küçük farklar vardır. Makale [bir Azure sanal makinesi olarak DPM yükleme](https://technet.microsoft.com/library/jj852163.aspx), yardımcı olur, bu küçük farklar açıklanmaktadır. Makine dağıtmadan önce tamamen bu makaleyi okuyun.

> [!NOTE]
> Azure Backup sunucusu, ayrılmış, tek amaçlı bir sanal makinede çalışacak şekilde tasarlanmıştır. Azure Backup sunucusu üzerinde yüklenemiyor:
> - Etki alanı denetleyicisi olarak çalıştırılan bir bilgisayar
> - Uygulama Sunucusu rolünün yüklü olduğu bir bilgisayar
> - Exchange Server’ın çalıştırıldığı bir bilgisayar
> - Küme düğümü olan bir bilgisayar

Her zaman Azure Backup sunucusu bir etki alanına katılın. Azure Backup sunucusu farklı bir etki alanına taşımanız gerekiyorsa, ilk Azure Backup sunucusu yükleyin ve yeni etki alanına katılın. Azure Backup sunucusu dağıttığınızda, yeni bir etki alanı taşınamıyor.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama

Kurtarma Hizmetleri kasası depolama çoğaltma seçeneğini coğrafi olarak yedekli depolama ile yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Varsayılan olarak, Kurtarma Hizmetleri kasaları, coğrafi olarak yedekli depolama kullanın. Bu kasa birincil kasanız varsa depolama seçeneği coğrafi olarak yedekli depolama şeklinde bırakın. Daha az dayanıklı ucuz bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu ve Ayarlar menüsünü açmak için kasanızı seçin. Varsa **ayarları** değil menüsünü açın, **tüm ayarlar** kasa panosunda.
2. Üzerinde **ayarları** menüsünde tıklatın **Yedekleme Altyapısı** > **yedekleme yapılandırması** açmak için **yedekleme yapılandırması**menüsü. Üzerinde **yedekleme yapılandırması** menüsünde, kasanız için depolama çoğaltma seçeneğini belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

## <a name="download-azure-backup-server-installer"></a>Azure Backup sunucusu yükleyicisini indirin

Azure Backup sunucusu yükleyiciyi indirmek için iki yolu vardır. Azure Backup sunucusu Yükleyicisi'nden indirebileceğiniz [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55269). Kurtarma Hizmetleri kasası yapılandırmakta olduğunuz gibi Azure Backup sunucusu yükleyici de indirebilirsiniz. Aşağıdaki adımlarda bir kurtarma Hizmetleri kasası yapılandırılırken Azure portalından yükleyici indirirken size yol.

1. Azure Stack sanal makinenizden [Azure portalında Azure aboneliğinizde oturum açın](https://portal.azure.com/).
2. Sol taraftaki menüde **tüm hizmetleri**.

    ![Ana menüde tüm hizmetleri seçeneğini belirleyin](./media/backup-mabs-install-azure-stack/click-all-services.png)

3. İçinde **tüm hizmetleri** iletişim kutusunda, türü *kurtarma Hizmetleri*. Yazmaya başladığınızda, giriş kaynakların listesini filtrelenir. Gördüğünüzde, seçmek **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-mabs-install-azure-stack/all-services.png)

    Abonelikte kurtarma Hizmetleri kasalarının listesi görünür.

4. Kurtarma Hizmetleri kasalarının listesinden panosunu açmak için kasanızı seçin.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-mabs-install-azure-stack/rs-vault-dashboard.png)

5. Kasanın Başlarken menüde **yedekleme** Başlarken Sihirbazı'nı açın.

    ![Yedeklemeyi kullanmaya başlama](./media/backup-mabs-install-azure-stack/getting-started-backup.png)

    Yedekleme menüsü açılır.

    ![Yedekleme hedefleri-varsayılan-açılan](./media/backup-mabs-install-azure-stack/getting-started-menu.png)

6. Yedekleme menüsündeki gelen **iş yükünüz çalıştığı** menüsünde **şirket içi**. Gelen **neleri yedeklemek istiyorsunuz?** istediğiniz iş yüklerini korumak Azure Backup sunucusu kullanarak açılan menüsünde. Seçmek için hangi iş yüklerini emin değilseniz, seçin **Hyper-V sanal makinelerini** ve ardından **altyapıyı hazırlama**.

    ![Şirket içi ve hedefleri olarak iş yükleri](./media/backup-mabs-install-azure-stack/getting-started-menu-onprem-hyperv.png)

    **Altyapıyı hazırlama** menüsü açılır.

7. İçinde **altyapıyı hazırlama** menüsünde tıklatın **indirin** Azure Backup sunucusu yükleme dosyalarını indirmek için bir web sayfasını açın.

    ![Başlarken Sihirbazı'nı değişiklik alma](./media/backup-mabs-install-azure-stack/prepare-infrastructure.png)

    Azure Backup sunucusu için indirilebilir dosyalarını barındıran Microsoft web sayfası açılır.

8. Microsoft Azure Backup sunucusu indirme sayfasında, bir dil seçin ve tıklayın **indirme**.

    ![İndirme Merkezi'ni açar](./media/backup-mabs-install-azure-stack/mabs-download-center-page.png)

9. Azure Backup sunucusu yükleyici, sekiz dosya - bir yükleyici ve yedi .bin dosyaları oluşur. Denetleme **dosya adı** gerekli tüm dosyaları seçin ve **sonraki**. Tüm dosyaları aynı klasöre indirin.

    ![İndirme Merkezinden 1](./media/backup-mabs-install-azure-stack/download-center-selected-files.png)

    Tüm yükleme dosyalarının indirme boyutu 3 GB'tan büyüktür. 10 MB/sn yüklenebilir bağlantı, tüm yükleme dosyaları indiriliyor 60 dakikaya kadar sürebilir. Dosyaları, belirtilen indirme konumuna indirin.

## <a name="extract-azure-backup-server-install-files"></a>Azure Backup sunucusu yükleme dosyalarını ayıklayın

Azure Stack sanal makinenize tüm dosyaları indirdikten sonra indirme konumuna gidin. İlk aşamada Azure Backup sunucusu yükleme dosyaları ayıklamaktır.

![İndirme Merkezinden 1](./media/backup-mabs-install-azure-stack/download-mabs-installer.png)

1. İndirilen dosyaları listesinden, yüklemeyi başlatmak için tıklatın **MicrosoftAzureBackupserverInstaller.exe**.

    > [!WARNING]
    > Kurulum dosyalarını ayıklamak için en az 4GB boş alan gerekir.
    >

2. Azure Backup Sunucusu Sihirbazı ' **sonraki** devam etmek için.

    ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wiz-1.png)

3. Azure Backup sunucusu dosyalarının yolunu seçin ve tıklayın **sonraki**.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-select-destination-1.png)

4. Ayıklama konumunu doğrulayın ve tıklayın **ayıklamak**.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-extract-2.png)

5. Sihirbaz dosyaları ayıklar ve yükleme işlemi onu hazırlar.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-install-3.png)

6. Ayıklama işlemi tamamlandıktan sonra tıklayın **son**. Varsayılan olarak, **setup.exe yürütme** seçilir. Tıkladığınızda **son**, Setup.exe, Microsoft Azure Backup sunucusu belirtilen konuma yükler.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-finish-4.png)

## <a name="install-the-software-package"></a>Yazılım paketini yükle

Önceki adımda, tıkladığınız **son** ayıklama aşaması çıkmak ve Azure Backup Sunucusu Kurulum Sihirbazı'nı başlatın.

![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5.png)

Azure Backup sunucusu, veri koruma Yöneticisi ile kod paylaşır. Data Protection Manager'ı ve DPM'yi Azure Backup sunucusu Yükleyicisi'nde başvurular görürsünüz. Azure Backup sunucusu ve Data Protection Manager ayrı bir ürün olsa da, bu ürünleri yakından ilişkilidir.

1. Kurulum Sihirbazı'nı başlatmak için tıklayın **Microsoft Azure Backup sunucusu**.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5b.png)

2. Üzerinde **Hoş Geldiniz** ekranında **sonraki**.

    ![Azure Backup sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-setup-6.png)

3. Üzerinde **önkoşul denetimleri** ekranında **denetleyin** Azure Backup sunucusu için donanım ve yazılım önkoşullarının karşılanıp karşılanmadığını olmadığını belirlemek için.

    ![Azure Backup sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-7.png)

    Ortamınızda gerekli önkoşullar varsa, makinenin bu gereksinimleri karşıladığını belirten bir ileti görürsünüz. **İleri**’ye tıklayın.  

    ![Azure Backup sunucusu - Önkoşullar denetimi başarılı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-passed-8.png)

    Ortamınızı gerekli önkoşulları karşılamıyorsa sorunlar belirtilir. Önkoşullar karşılanmadı DpmSetup.log içinde de listelenir. Önkoşul hatalarını çözümlemeyi ve ardından çalıştırın **tekrar kontrol edin**. Tüm önkoşulların sağlandığından kadar kurulum devam edemiyor.

    ![Azure Backup sunucusu - yükleme önkoşulları sağlanmadı](./media/backup-mabs-install-azure-stack/installation-errors.png)

4. Microsoft Azure Backup sunucusu, SQL Server gerektirir. Azure Backup sunucusu yükleme paketi uygun SQL Server ikili dosyaları ile sunulur. Kendi SQL yüklemesi kullanmak istiyorsanız, bunu yapabilirsiniz. Ancak, SQL Server yeni bir örneğini ekleme kurucusunun önerilen seçenek olur. Seçtiğiniz ortamınız ile çalıştığından emin olmak için tıklayın **denetle ve Yükle**.

   > [!NOTE]
   > Azure Backup sunucusu, uzak bir SQL Server örneği ile çalışmaz. Azure Backup sunucusu tarafından kullanılan örnek yerel olması gerekir.
   >

    ![Azure Backup sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-install-9.png)

    Sanal makinenin Azure Backup sunucusu yüklemek için gerekli önkoşullar varsa, denetledikten sonra tıklatın **sonraki**.

    ![Azure Backup sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-ready-10.png)

    Makineyi yeniden başlatmak için bir öneri ile bir hata oluşursa, daha sonra makineyi yeniden başlatın. Yükleyici, makine yeniden başlatıldıktan sonra yeniden başlatın ve ulaştığınızda **SQL ayarları** ekranında **tekrar kontrol edin**.

5. İçinde **yükleme ayarları**, Microsoft Azure Backup sunucusu dosyaları yüklenmesi için bir konum sağlayın ve tıklayın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-settings-11.png)

    Karalama konumu, Azure'a yedeklemek için gereklidir. Karalama konumu boyutu en az %5 verilerinin Azure'a yedeklenmesi için planlanan eşdeğer olduğundan emin olun. Disk koruması için ayrı disklere yükleme tamamlandıktan sonra yapılandırılması gerekir. Depolama havuzları hakkında daha fazla bilgi için bkz. [depolama havuzlarını yapılandırın ve disk depolama](https://technet.microsoft.com/library/hh758075.aspx).

6. Üzerinde **güvenlik ayarları** ekranında, kısıtlı yerel kullanıcı hesapları için güçlü bir parola girin ve tıklatın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-security-12.png)

7. Üzerinde **Microsoft Update katılımı** ekranında, kullanmak isteyip istemediğinizi seçin *Microsoft Update* güncelleştirmeleri denetlemek ve **sonraki**.

   > [!NOTE]
   > Windows güncelleştirme Microsoft Update, Windows ve Microsoft Azure Backup sunucusu gibi diğer ürünleri için güvenlik ve önemli güncelleştirmeler sunar. yeniden yönlendirme olması önerilir.
   >

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-update-13.png)

8. Gözden geçirme *ayarların özeti* tıklatıp **yükleme**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-summary-14.png)

    Azure Backup sunucusu yüklemesini tamamladıktan sonra yükleyici hemen Microsoft Azure kurtarma Hizmetleri Aracısı yükleyici başlatır.

9. Microsoft Azure kurtarma Hizmetleri Aracısı yükleyicisi açılır ve Internet bağlantısını denetler. Internet bağlantısı varsa, yükleme işlemine devam edin. Bağlantı yoksa, Internet'e bağlanmak için proxy ayrıntıları sağlayın. Proxy ayarlarınız belirttikten sonra tıklayın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-proxy-15.png)

10. Microsoft Azure kurtarma Hizmetleri Aracısı'nı yüklemek için tıklayın **yükleme**.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-mars-agent-16.png)

    Microsoft Azure kurtarma Hizmetleri Aracısı Azure Backup aracısı olarak da bilinir, Kurtarma Hizmetleri kasasına Azure Backup sunucusu yapılandırır. Yapılandırıldıktan sonra Azure Backup sunucusu her zaman aynı kurtarma Hizmetleri kasası için verileri yedekleme.

11. Microsoft Azure kurtarma Hizmetleri Aracısı'nın yükleme işlemini tamamladıktan sonra tıklayın **sonraki** sonraki aşamaya başlatmak için: Azure Backup sunucusu, Kurtarma Hizmetleri kasası ile kaydediliyor.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-complete-16.png)

    Yükleyici başlatır **Kaydetme Sihirbazı'nı**.

12. Azure aboneliğinizi ve kurtarma Hizmetleri kasanız için geçiş yapın. İçinde **altyapıyı hazırlama** menüsünde tıklatın **indirme** kasa kimlik bilgilerini indirmek için. Varsa **indirme** 2. Adım'daki düğmesi etkin, select değil **indirdiğiniz ya da en son Azure Backup sunucusu yüklemesi kullanarak zaten** düğmesini etkinleştirmek için. Kasa kimlik bilgileri indirmeler depoladığınız bir konuma indirin. Sonraki adım için gerekeceği için bu konumu unutmayın.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/download-mars-credentials-17.png)

13. İçinde **kasa kimlik** menüsünde, tıklayın **Gözat** kurtarma Hizmetleri kasa kimlik bilgileri bulunamıyor.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-vault-id-18.png)

    İçinde **kasa kimlik bilgilerini seçin** iletişim kutusunda, indirme konumuna, kasa kimlik bilgilerini seçin ve tıklayın Git **açık**.

    Yol kimlik bilgileri kasası kimliği menüsünde görünür. Tıklayın **sonraki** şifreleme ayarı ilerlemek için.

14. İçinde **şifreleme ayarı** iletişim kutusunda, yedek şifreleme ve parola depolama ve konumu için bir parola belirleyin **sonraki**.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-encryption-19.png)

    Kendi parola girin veya sizin için bir tane oluşturmak için parola Oluşturucu kullanın. Parola sizindir ve Microsoft kaydedin veya bu parolayı yönetin. Bir olağanüstü durum için hazırlamak için parolanız erişilebilen bir konuma kaydedin.

    ' A tıkladığınızda **sonraki**, Azure Backup sunucusu, Kurtarma Hizmetleri kasasına kayıtlı. Yükleyici, SQL Server ve Azure Backup sunucusu yükleme devam eder.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-still-installing-20.png)

15. Yükleyici tamamlandığında, tüm yazılım başarıyla yüklendi durumu gösterilir.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-done-22.png)

    Yükleme tamamlandığında, Azure Backup Sunucusu konsolu ve Azure Backup sunucusu PowerShell simgeleri sunucu masaüstünde oluşturulur.

## <a name="add-backup-storage"></a>Yedekleme depolama alanı ekleme

İlk yedek kopyanın Azure Backup sunucusu makinesine bağlı depolama tutulur. Disk ekleme hakkında daha fazla bilgi için bkz. [ekleme Modern yedekleme depolama alanı](https://docs.microsoft.com/system-center/dpm/add-storage?view=sc-dpm-1801).

> [!NOTE]
> Verileri Azure'a göndermek planlama olsa bile, yedekleme depolama alanı ekleme gerekir. Azure Backup sunucusu mimaride, Kurtarma Hizmetleri kasası ayrı tutma *ikinci* ilk (ve zorunlu) yedek kopyayı yerel depolama sırasında verilerin kopyasını tutar.
>
>

## <a name="network-connectivity"></a>Ağ bağlantısı

Azure Backup sunucusu başarıyla çalışmak ürün için Azure Backup hizmeti bağlantısı gerektirir. Makineyi azure'a bağlantısı olup olmadığını doğrulamak için kullanın ```Get-DPMCloudConnection``` Azure Backup sunucusu PowerShell konsolundaki cmdlet'i. Bu cmdlet'in çıktısı true'dur sonra bağlantı yoksa, başka hiçbir bağlantısı yoktur.

Aynı zamanda, Azure aboneliğiniz iyi durumda olması gerekir. Aboneliğinizin durumunu bulmak ve yönetmek için oturum [abonelik portalı](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

Azure bağlantı ve Azure aboneliğinin durumu öğrendikten sonra sunulan yedekleme/geri yükleme işlevselliği üzerindeki etkisini öğrenmek için aşağıdaki tabloyu kullanabilirsiniz.

| Bağlantı durumu | Azure Aboneliği | Azure'a yedekleme | Diske yedekleme | Azure'dan geri yükleme | Diskten geri yükleme |
| --- | --- | --- | --- | --- | --- |
| Bağlanıldı |Etkin |İzin Verilen |İzin Verilen |İzin Verilen |İzin Verilen |
| Bağlanıldı |Süresi dolmuş |Durduruldu |Durduruldu |İzin Verilen |İzin Verilen |
| Bağlanıldı |Yetki Kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |
| Kayıp bağlantı > 15 gün |Etkin |Durduruldu |Durduruldu |İzin Verilen |İzin Verilen |
| Kayıp bağlantı > 15 gün |Süresi dolmuş |Durduruldu |Durduruldu |İzin Verilen |İzin Verilen |
| Kayıp bağlantı > 15 gün |Yetki Kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |

### <a name="recovering-from-loss-of-connectivity"></a>Bağlantı kaybından kurtarma

Azure'a erişimi engelliyorsa bir güvenlik duvarı veya proxy beyaz liste aşağıdaki etki alanı güvenlik duvarı/proxy profilinde ele alır:

- `http://www.msftncsi.com/ncsi.txt`
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

Azure Backup sunucusu için Azure bağlantı geri yüklendikten sonra Azure abonelik durumu gerçekleştirebileceği işlemleri belirler. Sunucu sonra **bağlı**, tablodaki kullanın [ağ bağlantısı](backup-mabs-install-azure-stack.md#network-connectivity) kullanılabilir tüm işlemleri görmek için.

### <a name="handling-subscription-states"></a>Abonelik durumları işleme

Bir Azure aboneliğine ait değiştirmek mümkündür *süresi dolan* veya *yetki kaldırıldı* durumunu *etkin* durumu. Abonelik durumu değilken *etkin*:

- Bir aboneliği *yetki kaldırıldı*, işlevselliğini kaybeder. Abonelik geri *etkin*, yedekleme/geri yükleme işlevlerini revives. Yedekleme verileri yerel diskteki bir yeterli büyüklükte saklama süresi ile tutulmaktadır, bu yedekleme verileri alınabilir. Ancak, abonelik girdikten sonra azure'da yedekleme verilerini irretrievably kayıp *yetki kaldırıldı* durumu.
- Bir aboneliği *süresi dolan*, işlevselliğini kaybeder. Bir aboneliği zamanlanmış yedeklemeler çalıştırmayın *süresi dolan*.

## <a name="troubleshooting"></a>Sorun giderme

Microsoft Azure Backup sunucusu kurulumu aşamasında (veya yedekleme veya geri yükleme sırasında) hatalarla başarısız olursa bkz [hata kodları belge](https://support.microsoft.com/kb/3041338).
Ayrıca başvurabilirsiniz [Azure Backup ilgili sık sorulan sorular](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

Makale [ortamınızı DPM'ye hazırlama](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1801), desteklenen Azure Backup sunucusu yapılandırmaları hakkında bilgi içerir.

Microsoft Azure Backup sunucusu kullanarak iş yükü koruması daha derin bir anlayış kazanmak için aşağıdaki makalelere kullanabilirsiniz.

- [SQL Server Yedekleme](https://docs.microsoft.com/azure/backup/backup-mabs-sql-azure-stack)
- [SharePoint server yedekleme](https://docs.microsoft.com/azure/backup/backup-mabs-sharepoint-azure-stack)
- [Alternatif Sunucu yedekleme](backup-azure-alternate-dpm-server.md)
