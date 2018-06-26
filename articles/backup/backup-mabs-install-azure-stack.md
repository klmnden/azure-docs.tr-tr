---
title: Azure yığında Azure yedekleme Sunucusu'nu Yükle | Microsoft Docs
description: Azure yedekleme sunucusu korumak veya Azure yığın iş yüklerini yedeklemek için kullanın.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: Azure backup sunucusu; iş yüklerini korumak; iş yüklerini yedeklemeye
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/5/2018
ms.author: markgal
ms.openlocfilehash: c9dd6a1818b0afeb5e577724568a8254a70c8228
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753363"
---
# <a name="install-azure-backup-server-on-azure-stack"></a>Azure Stack üzerinde Azure Backup Sunucusu'nu yükleme

Bu makalede, Azure yığında Azure yedekleme Sunucusu'nu yükleme açıklanmaktadır. Azure yedekleme Sunucusu'nu kullanarak, Azure yığınında çalışan sanal makineler gibi hizmet (Iaas) iş yükleri olarak altyapı koruyabilirsiniz. İş yüklerini korumak için Azure yedekleme sunucusu kullanmanın bir avantajı, tüm iş yükü koruması tek bir konsoldan yönetebileceğiniz ' dir.

> [!NOTE]
> Güvenlik özellikleri hakkında bilgi edinmek için bkz [Azure Backup güvenlik özellikleri belgelerine](backup-azure-security-feature.md).
>

## <a name="azure-backup-server-protection-matrix"></a>Azure Backup Sunucusu koruma matrisi
Azure yedekleme sunucusu aşağıdaki Azure yığın sanal makine iş yüklerini korur.

| Korunan veri kaynağı | Koruma ve kurtarma |
| --------------------- | ----------------------- |
| Windows Server yarı yıllık kanalı - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2016 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 R2 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 - Entprise/veri merkezi/standart | Birimler, dosyalar, klasörler |
| Windows Server 2008 R2 - kuruluş/veri merkezi/standart | Birimler, dosyalar, klasörler |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2016 | Grup, veritabanı, ön uç, web sunucusu |
| SharePoint 2013 | Grup, veritabanı, ön uç, web sunucusu |
| SharePoint 2010 | Grup, veritabanı, ön uç, web sunucusu |

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Azure yedekleme sunucusu ortamı için Önkoşullar

Bu bölümdeki öneriler Azure yedekleme sunucusu Azure yığın ortamınızda yüklerken göz önünde bulundurun. Azure yedekleme sunucusu yükleyici ortamınızın gerekli önkoşullara sahip, ancak yüklemeden önce hazırlama zaman kazanırsınız denetler.

### <a name="determining-size-of-virtual-machine"></a>Sanal makine boyutunu belirleme
Azure yedekleme sunucusu bir Azure yığın sanal makine üzerinde çalıştırılacak boyutunu A2 kullanın veya daha büyük. Bir sanal makine boyutu seçerken Yardım almak için indirin [Azure yığın VM boyutu hesaplayıcı](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Azure yığın sanal makinelerde sanal ağlar
Bir Azure yığın iş yükü kullanılan tüm sanal makineler aynı Azure sanal ağı ve Azure aboneliğine ait olmalıdır.

### <a name="azure-backup-server-vm-performance"></a>Azure yedekleme sunucusu VM performans
Diğer sanal makinelerle paylaşılan, depolama boyutu ve IOPS sınırları etkisi Azure yedekleme sunucusu VM performans hesap. Bu nedenle, Azure yedekleme sunucusu sanal makine için ayrı bir depolama hesabı kullanmanız gerekir. Azure yedekleme sunucusu üzerinde çalışan Azure Yedekleme aracısı için geçici depolama gerekir:
- kendi kullanımı (bir önbellek konumu)
- (yerel hazırlama alanı) buluttan geri yüklenen veriler

### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Backup geçici disk depolamayı yapılandırma
Her Azure yığın sanal makine toplu olarak kullanıcı için kullanılabilir geçici disk depolama ile birlikte gelen `D:\`. Azure Yedekleme'nin ihtiyaç duyduğu yerel hazırlama alanı bulunması için yapılandırılabilir `D:\`, ve önbellek konumu üzerinde yerleştirilebilir `C:\`. Bu şekilde, depolama Azure yedekleme sunucusu sanal makinesine bağlı veri disklerinden çıktığınızda yontulmuş gerekiyor.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Yerel disk ve Azure yedekleme veri depolama
Azure Backup sunucusu işletimsel kurtarma için sanal makineye bağlı Azure disklerde yedek verileri depolar. Diskler ve depolama alanı sanal makineye bağlı sonra Azure yedekleme sunucusu depolama yönetir. Yedek veri depolama alanı miktarı sayısını ve her birine bağlı disklerin boyutunu bağlıdır [Azure yığın sanal makine](../azure-stack/user/azure-stack-storage-overview.md). Her Azure yığın VM boyutu en fazla sayıda sanal makineye bağlı diskler vardır. Örneğin, A2 dört diskler ' dir. A3 sekiz diskler ' dir. A4 16 disk ' dir. Yeniden disk sayısı ve boyutu, toplam yedekleme depolama havuzunu belirler.

> [!IMPORTANT]
> Yapmanız gerekenler **değil** beş günden fazla bir süre için Azure yedekleme sunucusuna bağlı disklerde işletimsel Kurtarma (Yedekleme) verileri korur.
>

Yedek veri depolama, yedekleme altyapısı Azure yığında azaltır. Veri beş günden daha eski ise, Azure'da depolanması gerekir.

Azure'da yedek verileri depolamak için oluşturun veya bir kurtarma Hizmetleri kasası kullanın. Azure yedekleme sunucusu iş yükü çalışma yedeklemek hazırlarken, [kurtarma Hizmetleri kasası yapılandırma](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault). Bir kere yapılandırıldığında, yedekleme işi her çalıştığında, kasaya bir kurtarma noktası oluşturulur. Her kurtarma Hizmetleri kasası kadar 9999 kurtarma noktalarını tutar. Oluşturulan kurtarma noktaları ve ne kadar süreyle saklanacağını sayısına bağlı olarak, birçok yıldır yedekleme verileri koruyabilirsiniz. Örneğin, aylık kurtarma noktaları oluşturmak ve bunları beş yıl boyunca bekletmek.
 
### <a name="scaling-deployment"></a>Dağıtım ölçeklendirme
Dağıtımınız ölçeklendirmek istiyorsanız aşağıdaki seçenekleriniz vardır:
  - Ölçeği artırma - D serisinin serisinden Azure yedekleme sunucusu sanal makineye boyutunu artırır ve yerel depolama alanını büyütmek [Azure yığın sanal makine yönergeleri başına](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Veri Boşalt - eski verileri Azure'a gönderin ve yalnızca en yeni verileri Azure yedekleme sunucusuna bağlı depolama korur.
  - -Daha fazla Azure yedekleme iş yüklerini korumak için sunucu eklemek ölçeğini.

### <a name="net-framework"></a>.NET Framework

.NET framework 3.5 SP1 veya üstünü sanal makinede yüklü olmalıdır.

### <a name="joining-a-domain"></a>Bir etki alanına katılma

Azure yedekleme sunucusu sanal makine bir etki alanına katılması gerekir. Yönetici ayrıcalıklarına sahip bir etki alanı kullanıcı sanal makineye Azure yedekleme sunucusu yüklemeniz gerekir.

## <a name="using-an-iaas-vm-in-azure-stack"></a>Bir Iaas VM Azure yığınında kullanma

Bir sunucu için Azure yedekleme sunucusu seçerken, bir Windows Server 2012 R2 Datacenter veya Windows Server 2016 Datacenter galeri görüntüsü ile başlatın. Makale [ilk Windows sanal makinenizi Azure Portalı'nda oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), önerilen sanal makine ile çalışmaya başlama için bir öğretici sağlar. Sunucu sanal makine (VM) için önerilen en düşük gereksinimler olmalıdır: A2 standart iki çekirdek ve 3.5 GB RAM.

Azure yedekleme sunucusu ile iş yüklerini koruma pek çok küçük farklar vardır. Makale [bir Azure sanal makinesi olarak DPM yükleme](https://technet.microsoft.com/library/jj852163.aspx), yardımcı olur, bu küçük farklar açıklanmaktadır. Makine dağıtmadan önce bu makalenin tümüyle okuyun.

> [!NOTE]
> Azure yedekleme sunucusu ayrılmış, tek amaçlı bir sanal makinede çalışmak üzere tasarlanmıştır. Azure yedekleme sunucusu üzerinde yüklenemiyor:
> - Etki alanı denetleyicisi olarak çalıştırılan bir bilgisayar
> - Uygulama Sunucusu rolünün yüklü olduğu bir bilgisayar
> - Exchange Server’ın çalıştırıldığı bir bilgisayar
> - Küme düğümü olan bir bilgisayar

Her zaman Azure yedekleme sunucusu bir etki alanına katılın. Azure yedekleme sunucusu farklı bir etki alanına taşımanız gerekiyorsa, ilk Azure yedekleme sunucusu yükleyin ve ardından yeni etki alanına katılın. Azure yedekleme sunucusu dağıttığınızda, yeni bir etki alanına taşıyamazsınız.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama

Kurtarma Hizmetleri kasası depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Varsayılan olarak, Kurtarma Hizmetleri kasalarının coğrafi olarak yedekli depolama kullanın. Bu kasaya birincil kasanız coğrafi olarak yedekli depolamaya ayarlanmış depolama seçeneği bırakın. Daha az dayanıklı ucuz bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu ve Ayarlar menüsünü açmak için kasanızı seçin. Varsa **ayarları** değil menüsünü açın, **tüm ayarları** kasa panosunda.
2. Üzerinde **ayarları** menüsünde tıklatın **Yedekleme Altyapısı** > **yedekleme yapılandırması** açmak için **yedekleme yapılandırması**menüsü. Üzerinde **yedekleme yapılandırması** menüsünde, kasanız için depolama çoğaltma seçeneğini belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

## <a name="download-azure-backup-server-installer"></a>Azure yedekleme sunucusu yükleyici indirin

Azure yedekleme sunucusu yükleyici indirmek için iki yolu vardır. Azure yedekleme sunucusu Yükleyicisi'nden indirebilirsiniz [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55269). Kurtarma Hizmetleri kasası yapılandırdığınız gibi Azure yedekleme sunucusu yükleyici de indirebilirsiniz. Aşağıdaki adımlar bir kurtarma Hizmetleri kasası yapılandırılırken Azure portalından yükleyici indirme aracılığıyla yol.

1. Azure yığın sanal makinenizden [Azure portalında Azure aboneliğinizde oturum açmak](https://portal.azure.com/).
2. Sol menüde seçin **tüm hizmetleri**.

    ![Ana menüde tüm hizmetleri seçeneği seçin](./media/backup-mabs-install-azure-stack/click-all-services.png)

3. İçinde **tüm hizmetleri** iletişim, türü *kurtarma Hizmetleri*. Yazmaya başladığınızda, girişinizi kaynakların listesini filtreler. Gördüğünüzde, seçeneğini **kurtarma Hizmetleri kasaları**.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-mabs-install-azure-stack/all-services.png)

    Abonelikteki kurtarma Hizmetleri kasalarının listesi görüntülenir.

4. Kurtarma Hizmetleri kasalarının listesinden kendi panosunu açmak için kasanızı seçin.

    ![Kurtarma Hizmetleri tüm hizmetleri iletişim kutusunda yazın](./media/backup-mabs-install-azure-stack/rs-vault-dashboard.png)

5. Kasası'nın Başlarken menüsünde tıklatın **yedekleme** Başlarken Sihirbazı'nı açın.

    ![Başlarken yedekleme](./media/backup-mabs-install-azure-stack/getting-started-backup.png)

    Yedekleme menüsü açılır.

    ![Yedekleme hedefleri-varsayılan-açılan](./media/backup-mabs-install-azure-stack/getting-started-menu.png)

6. Yedekleme menüde gelen **, iş yükünü çalıştırdığı** menüsünde, select **şirket içi**. Gelen **neleri yedeklemek istiyorsunuz?** istediğiniz iş yüklerini korumak Azure yedekleme sunucusu kullanarak açılan menüsünde seçin. Seçmek için hangi iş yüklerini emin değilseniz, seçin **Hyper-V sanal makineleri** ve ardından **altyapıyı hazırlama**.

    ![Şirket içi ve hedefleri olarak iş yükleri](./media/backup-mabs-install-azure-stack/getting-started-menu-onprem-hyperv.png)

    **Altyapıyı hazırlama** menüsü açılır.

7. İçinde **altyapıyı hazırlama** menüsünde tıklatın **karşıdan** Azure yedekleme sunucusu yükleme dosyalarını indirmek üzere bir web sayfasını açmak için.

    ![Başlarken Sihirbazı'nı değişiklik alma](./media/backup-mabs-install-azure-stack/prepare-infrastructure.png)

    Azure yedekleme sunucusu için indirilebilir dosyalarını barındıran Microsoft web sayfasını açar.

8. Microsoft Azure yedekleme sunucusu indirme sayfasında bir dil seçin ve'ı tıklatın **karşıdan**.

    ![Merkezi açılır indirin](./media/backup-mabs-install-azure-stack/mabs-download-center-page.png)

9. Azure yedekleme sunucusu yükleyici, sekiz dosya - bir yükleyici ve yedi .bin dosyaları oluşur. Denetleme **dosya adı** gerekli tüm dosyaları seçin ve **sonraki**. Tüm dosyalar aynı klasöre yükleyin.

    ![İndirme Merkezinden 1](./media/backup-mabs-install-azure-stack/download-center-selected-files.png)

    Tüm yükleme dosyalarını indirme boyutu 3 GB'den büyük. 10 MB/sn indirme işlemini tüm yükleme dosyalarını indirme bağlantısı, 60 dakika kadar sürebilir. Dosyaları, belirtilen indirme konuma indirin.

## <a name="extract-azure-backup-server-install-files"></a>Azure yedekleme sunucusu yükleme dosyalarını ayıklayın

Azure yığın sanal makinenize tüm dosyaları indirdikten sonra yükleme konumuna gidin. Azure yedekleme sunucusu yükleme ilk dosyalarını ayıklamak için aşamasıdır.

![İndirme Merkezinden 1](./media/backup-mabs-install-azure-stack/download-mabs-installer.png)

1. Karşıdan yüklenen dosyalar listesinden yüklemeyi başlatmak için tıklatın **MicrosoftAzureBackupserverInstaller.exe**.

    > [!WARNING]
    > Kurulum dosyalarını ayıklamak için en az 4GB boş disk alanı gereklidir.
    >

2. Azure Yedekleme Sunucusu Sihirbazı'nda tıklatın **sonraki** devam etmek için.

    ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wiz-1.png)

3. Azure yedekleme sunucusu dosyaların yolunu seçin ve tıklatın **sonraki**.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-select-destination-1.png)

4. Ayıklama konumunu doğrulayın ve tıklayın **ayıklamak**.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-extract-2.png)

5. Sihirbaz dosyaları ayıklar ve yükleme işlemi onu hazırlar.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-install-3.png)

6. Ayıklama işlemi tamamlandıktan sonra tıklayın **son**. Varsayılan olarak, **setup.exe yürütme** seçilir. Tıkladığınızda **son**, Setup.exe belirtilen konuma Microsoft Azure yedekleme sunucusu yükler.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-finish-4.png)

## <a name="install-the-software-package"></a>Yazılım paketi yükleme

Önceki adımda tıkladığınız **son** ayıklama aşaması çıkın ve Azure Yedekleme Sunucusu Kurulum Sihirbazı'nı başlatın.

![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5.png)

Azure yedekleme sunucusu kodu Data Protection Manager ile paylaşır. Data Protection Manager ve DPM başvuruları Azure yedekleme sunucusu yükleyicisinde görürsünüz. Azure yedekleme sunucusu ve Data Protection Manager ayrı ürünler olmakla birlikte, bu ürünler yakından ilişkilidir.

1. Kurulum Sihirbazı'nı başlatmak için tıklatın **Microsoft Azure yedekleme sunucusu**.

   ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5b.png)

2. Üzerinde **Hoş Geldiniz** ekranında **sonraki**.

    ![Azure yedekleme sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-setup-6.png)

3. Üzerinde **önkoşul denetler** ekranında **denetleyin** Azure yedekleme sunucusu için donanım ve yazılım önkoşullarının karşılanıp karşılanmadığını varsa belirlemek için.

    ![Azure yedekleme sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-7.png)

    Ortamınızda gerekli önkoşullar varsa, makine gereksinimlerini karşıladığını belirten bir ileti görürsünüz. **İleri**’ye tıklayın.  

    ![Azure yedekleme sunucusu - Önkoşullar denetimden geçti](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-passed-8.png)

    Ortamınızı gerekli Önkoşullar karşılamıyorsa sorunları belirtilir. Karşılanmamış Önkoşullar da DpmSetup.log listelenir. Önkoşul hataları giderin ve ardından çalıştırın **yeniden denetle**. Tüm ön koşulların karşılanıp karşılanmadığını kadar kurulum devam edemiyor.

    ![Azure yedekleme sunucusu - karşılanmayan yükleme önkoşulları](./media/backup-mabs-install-azure-stack/installation-errors.png)

4. Microsoft Azure yedekleme sunucusu SQL Server gerektirir. Azure yedekleme sunucusu yükleme paketi ile uygun SQL Server ikili dosyalarının beraberinde gelir. Kendi SQL yükleme kullanmak istiyorsanız, şunları yapabilirsiniz. Ancak, önerilen SQL Server'ın yeni bir örneğini ekleme yükleyici izin seçimdir. Tercih ettiğiniz ortamınız ile çalıştığından emin olmak için tıklatın **denetle ve Yükle**.

   > [!NOTE]
   > Azure yedekleme sunucusu uzak bir SQL Server örneği ile çalışmaz. Azure yedekleme sunucusu tarafından kullanılan örnek yerel olması gerekir.
   >

    ![Azure yedekleme sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-install-9.png)

    Sanal makineyi Azure yedekleme sunucusu yüklemek için gerekli önkoşullar varsa, denetledikten sonra tıklatın **sonraki**.

    ![Azure yedekleme sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-ready-10.png)

    Makineyi yeniden başlatmak için bir öneri ile bir hata meydana gelirse, makineyi yeniden başlatın. Yükleyici makine yeniden başlatıldıktan sonra yeniden başlatın ve ulaştığınızda **SQL ayarlarını** ekranında **yeniden denetle**.

5. İçinde **yükleme ayarları**, Microsoft Azure yedekleme sunucusu dosyaların yüklenmesi için bir konum belirtin ve tıklatın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-settings-11.png)

    Karalama konumu Azure'a yedeklemek için gereklidir. Karalama konumunun boyutu en az %5 Azure'a yedeklenmesi için planlanan veri eşit olduğundan emin olun. Disk koruması için ayrı diskin yükleme işlemi tamamlandıktan sonra yapılandırılması gerekir. Depolama havuzları hakkında daha fazla bilgi için bkz: [depolama havuzlarını yapılandırın ve disk depolama](https://technet.microsoft.com/library/hh758075.aspx).

6. Üzerinde **güvenlik ayarları** ekranında, kısıtlı yerel kullanıcı hesapları için güçlü bir parola sağlayın ve tıklatın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-security-12.png)

7. Üzerinde **Microsoft Update katılımı** ekranında, kullanmak mı istediğinizi seçin *Microsoft Update* güncelleştirmeleri denetlemek ve tıklayın **sonraki**.

   > [!NOTE]
   > Windows Update Microsoft Update, Windows ve Microsoft Azure yedekleme sunucusu gibi diğer ürünleri için güvenlik ve önemli güncelleştirmeler sunar yönlendirmek olması önerilir.
   >

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-update-13.png)

8. Gözden geçirme *ayarların özeti* tıklatıp **yükleme**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-summary-14.png)

    Azure yedekleme sunucusu yükleme tamamlandığında, yükleyici Microsoft Azure kurtarma Hizmetleri Aracısı yükleyicisi hemen başlatır.

9. Microsoft Azure kurtarma Hizmetleri Aracısı yükleyici açar ve Internet bağlantısı olup olmadığını denetler. Internet bağlantısı varsa, yükleme işlemine devam edin. Hiçbir bağlantı varsa, Internet'e bağlanmak için proxy ayrıntılarını girin. Proxy ayarlarınız belirlediğiniz sonra tıklayın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-proxy-15.png)

10. Microsoft Azure kurtarma Hizmetleri aracısı yüklemek için tıklatın **yükleme**.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-mars-agent-16.png)

    Azure Backup aracısı olarak da bilinir Microsoft Azure kurtarma Hizmetleri Aracısı, Kurtarma Hizmetleri kasasına Azure yedekleme sunucusu yapılandırır. Yapılandırdıktan sonra Azure yedekleme sunucusu her zaman aynı kurtarma Hizmetleri kasasına verileri yedekleyin.

11. Microsoft Azure kurtarma Hizmetleri Aracısı'nı yüklemeyi tamamladıktan sonra tıklayın **sonraki** sıradaki aşama başlatmak için: Azure yedekleme sunucusu kurtarma Hizmetleri kasası ile kaydediliyor.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-complete-16.png)

    Yükleyici başlatır **sunucuyu Kaydetme Sihirbazı**.

12. Azure aboneliğinizi ve kurtarma Hizmetleri kasanız için geçiş yapar. İçinde **altyapıyı hazırlama** menüsünde tıklatın **karşıdan** kasa kimlik bilgilerini indirmek için. Varsa **karşıdan** 2. adımda düğmesi etkin, select değil **indirilebilir veya en son Azure yedekleme sunucusu yükleme kullanarak zaten** düğmesini etkinleştirmek için. Kasa kimlik bilgileri indirmeleri depoladığınız bir konuma indirin. Sonraki adımda gerekir çünkü bu konumu unutmayın.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/download-mars-credentials-17.png)

13. İçinde **kasa kimlik** menüsünde tıklatın **Gözat** kurtarma Hizmetleri kasa kimlik bilgilerini bulmak için.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-vault-id-18.png)

    İçinde **kasa kimlik bilgilerini seçin** iletişim kutusunda, indirme konumuna, kasa kimlik bilgilerini seçin ve'i tıklatın **açık**.

    Kimlik bilgileri yoluna kasa kimlik menüsünde görüntülenir. Tıklatın **sonraki** şifreleme ayarı ilerlemek için.

14. İçinde **şifreleme ayarı** iletişim kutusunda, yedek şifreleme ve parola depolama ve'konumu için bir parola girin **sonraki**.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-encryption-19.png)

    Kendi parola girin veya sizin için bir tane oluşturmak için parola oluşturucuyu kullanın. Parola sizindir ve Microsoft kaydedin veya bu parolayı yönetin. Bir olağanüstü durum için hazırlamak için parolayı erişilebilen bir konuma kaydedin.

    Tıkladığınızda **sonraki**, Azure yedekleme sunucusu kurtarma Hizmetleri kasasına kayıtlı. Yükleyici, SQL Server ve Azure yedekleme Sunucusu'nu yükleme devam eder.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-still-installing-20.png)

15. Yükleyici tamamladığında, tüm yazılım başarıyla yüklendi durumunu gösterir.

    ![Azure Backup sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-done-22.png)

    Yükleme tamamlandığında, Azure yedekleme sunucusu konsol ve Azure yedekleme sunucusu PowerShell simgeleri sunucu masaüstünde oluşturulur.

## <a name="add-backup-storage"></a>Yedekleme depolama ekleme

İlk yedek kopyayı Azure yedekleme sunucusu makineye bağlı depolama üzerinde tutulur. Diskleri ekleme hakkında daha fazla bilgi için bkz: [eklemek Modern yedekleme depolama](https://docs.microsoft.com/en-us/system-center/dpm/add-storage?view=sc-dpm-1801).

> [!NOTE]
> Azure'a veri göndermeyi planladığınız olsa bile yedekleme depolama alanı eklemeniz gerekir. Azure yedekleme sunucusu mimarisinde, Kurtarma Hizmetleri kasası ayrı tutma *ikinci* yerel depolama sırasında verilerin kopyasını ilk (ve zorunlu) yedek kopyasını tutar.
>
>

## <a name="network-connectivity"></a>Ağ bağlantısı

Azure yedekleme sunucusu Azure yedekleme hizmetine başarılı olarak çalışması ürün için bağlantısı gerektirir. Makine Azure bağlantısı olup olmadığını doğrulamak için kullanın ```Get-DPMCloudConnection``` Azure yedekleme sunucusu PowerShell konsolundaki cmdlet'i. Cmdlet'in çıktısı, true'dur sonra bağlantı yoksa, başka bağlantısı yok.

Aynı anda Azure aboneliği sağlıklı bir durumda olması gerekir. Aboneliğinizin durumunu bulmak ve yönetmek için oturum [abonelik portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

Azure bağlantı ve Azure abonelik durumu öğrendikten sonra sunulan yedekleme/geri yükleme işlevselliği üzerindeki etkisini öğrenmek için aşağıdaki tabloyu kullanabilirsiniz.

| Bağlantı durumu | Azure Aboneliği | Azure'a yedekleme | Diske yedekleme | Azure'dan geri yükleme | Disk, geri yükleme |
| --- | --- | --- | --- | --- | --- |
| Bağlanıldı |Etkin |İzin Verilen |İzin Verilen |İzin Verilen |İzin Verilen |
| Bağlanıldı |Süresi dolmuş |Durduruldu |Durduruldu |İzin Verilen |İzin Verilen |
| Bağlanıldı |Yetki Kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |
| Kayıp bağlantısı > 15 gün |Etkin |Durduruldu |Durduruldu |İzin Verilen |İzin Verilen |
| Kayıp bağlantısı > 15 gün |Süresi dolmuş |Durduruldu |Durduruldu |İzin Verilen |İzin Verilen |
| Kayıp bağlantısı > 15 gün |Yetki Kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |

### <a name="recovering-from-loss-of-connectivity"></a>Bağlantı kaybına karşı kurtarma

Bir güvenlik duvarı veya proxy erişimi için Azure engelliyorsa, beyaz liste aşağıdaki etki alanı güvenlik duvarı/proxy profilinde adresleri:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

Azure bağlantısı için Azure yedekleme sunucusu geri yüklendikten sonra Azure abonelik durumu gerçekleştirilebilir işlemleri belirler. Sunucu olduğunda **bağlı**, tablodaki kullanın [ağ bağlantınızı](backup-mabs-install-azure-stack.md#network-connectivity) kullanılabilir tüm işlemleri görmek için.

### <a name="handling-subscription-states"></a>Abonelik durumları işleme

Azure aboneliğinden değiştirmek mümkündür *süresi doldu* veya *Deprovisioned* durumunu *etkin* durumu. Abonelik durumu olmamasına karşın *etkin*:

- Bir abonelik olsa *Deprovisioned*, işlevselliğini kaybeder. Abonelik geri *etkin*, yedekleme/geri yükleme işlevselliği revives. Yedekleme verilerini yerel diskte yeterli büyüklükte bekletme süresi ile korunur, bu yedekleme verileri alınabilir. Abonelik girdikten sonra ancak Azure yedekleme verilerinde irretrievably kaybolur *Deprovisioned* durumu.
- Bir abonelik olsa *süresi doldu*, işlevselliğini kaybeder. Bir abonelik olsa da zamanlanmış yedeklemeler çalıştırmayın *süresi doldu*.

## <a name="troubleshooting"></a>Sorun giderme

Microsoft Azure Yedekleme Sunucusu Kurulum aşaması (veya yedekleme veya geri yükleme sırasında) hataları ile başarısız olursa bkz [hata kodları belge](https://support.microsoft.com/kb/3041338).
Ayrıca başvurabilirsiniz [Azure Backup ile ilgili sık sorulan sorular](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Sonraki adımlar

Makale [DPM için ortamınızı hazırlama](https://docs.microsoft.com/en-us/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1801), desteklenen Azure yedekleme sunucusu yapılandırmaları hakkında bilgi içerir.

Microsoft Azure yedekleme sunucusu kullanarak iş yükü koruması daha derin bir anlayış edinmek için aşağıdaki makalelere kullanabilirsiniz.

- [SQL Server Yedekleme](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sql-azure-stack)
- [SharePoint server yedekleme](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sharepoint-azure-stack)
- [Diğer sunucu yedekleme](backup-azure-alternate-dpm-server.md)
