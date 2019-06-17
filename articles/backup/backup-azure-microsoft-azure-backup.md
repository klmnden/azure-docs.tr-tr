---
title: İş yüklerini Azure'a yedeklemek için Azure Backup sunucusu kullanma
description: Dosya korumak veya Azure portalında iş yüklerini yedeklemek için Azure Backup sunucusu kullanma.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: kasinh
ms.openlocfilehash: 26f25a0dcbeef0d5b7456d42caaca392c3ca6a1a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62098871"
---
# <a name="install-and-upgrade-azure-backup-server"></a>Yükleme ve Azure Backup sunucusu yükseltme
> [!div class="op_single_selector"]
> * [Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Bu makalede, Microsoft Azure Backup sunucusu (MABS) kullanarak iş yüklerini yedeklemek için ortamınızı hazırlama açıklanmaktadır. Azure Backup sunucusu ile Hyper-V Vm'leri, Microsoft SQL Server, SharePoint Server, Microsoft Exchange ve Windows istemcileri gibi uygulama iş yükleri tek bir konsoldan koruyabilirsiniz.

> [!NOTE]
> Azure Backup sunucusu artık VMware Vm'leri koruyabilir ve Gelişmiş güvenlik özellikleri sağlar. Ürün, aşağıdaki bölümlere ve en son Azure Backup Aracısı açıklandığı gibi yükleyin. Azure Backup sunucusu ile VMware sunucularını yedekleme hakkında daha fazla bilgi için bkz [bir VMware sunucusunu yedeklemek için Azure Backup sunucusu kullanma](backup-azure-backup-server-vmware.md). Güvenlik özellikleri hakkında bilgi edinmek için bkz [Azure yedekleme güvenlik özelliklerinin belgeleri](backup-azure-security-feature.md).
>
>

Bir Azure sanal Makinesinde dağıtılan MABS sanal makinenin azure'a yedekleyebilirsiniz, ancak yedekleme işlemi etkinleştirmek için aynı etki alanında olması gerekir. Bir Azure VM yedekleme işlemi, şirket içi VM'lerin yedeklenmesi olarak aynı kalır, ancak azure'da MABS dağıtma bazı sınırlamaları vardır. Sınırlama hakkında daha fazla bilgi için bkz [bir Azure sanal makinesi olarak DPM](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-1807#setup-prerequisites)

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Resource Manager modeli kullanılarak dağıtılan Vm'leri geri yüklemek için bilgi ve yordamları verilmektedir.
>
>

Azure Backup sunucusu iş yükü yedekleme işlevselliğinin Data Protection Manager (DPM) devralır. DPM belgesinin bazı paylaşılan işlevlerini açıklamak için bu makalede bağlar. Azure Backup sunucusu DPM ile aynı işlevlere çoğunu paylaşır, ancak Azure Backup sunucusu desteklemez banda yedekleyin ya da System Center ile tümleştirilir.

## <a name="choose-an-installation-platform"></a>Bir yükleme platformu seçin
Azure Backup sunucusu kullanmaya başlamak ve çalıştırmak doğrultusunda ilk adımı bir Windows Server'ı ayarlamaktır. Sunucunuz, Azure'da veya şirket içinde olabilir.

### <a name="using-a-server-in-azure"></a>Azure'da bir sunucu kullanarak
Azure Backup sunucusu çalıştıran bir sunucu seçerken, bir galeri görüntüsü ile Windows Server 2012 R2 Datacenter, Windows Server 2016 Datacenter veya Windows Server 2019 Datacenter Başlat önerilir. Makale [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), hiçbir zaman Azure önce kullanmış olsanız için önerilen sanal makine azure'da kullanmaya başlama bir öğretici sağlar. Sanal makinede (VM) sunucusu için önerilen en düşük gereksinimleri olmalıdır: Standart a2 iki çekirdek ve 3,5 GB RAM.

Azure Backup sunucusu iş yüklerini koruma birçok küçük farklar vardır. Makale [bir Azure sanal makinesi olarak DPM yükleme](https://technet.microsoft.com/library/jj852163.aspx), yardımcı olur, bu küçük farklar açıklanmaktadır. Makine dağıtmadan önce tamamen bu makaleyi okuyun.

### <a name="using-an-on-premises-server"></a>Bir şirket içi sunucusu kullanma
Azure'da taban sunucusu çalıştırmak istemiyorsanız, Hyper-V VM, VMware VM veya fiziksel bir konak sunucunun çalıştırabilirsiniz. Sunucu donanımı için önerilen en düşük gereksinimler, iki çekirdek ve 4 GB RAM verilmiştir. Aşağıdaki tabloda listelenen desteklenen işletim sistemleri:

| İşletim sistemi | Platform | SKU |
|:--- | --- |:--- |
| Windows Server 2019 |64 bit |Standard, Datacenter, Essentials (MABS V3 ve üzeri) |
| Windows Server 2016 ve en son Sp'ler |64 bit |Standard, Datacenter, Essentials (MABS V2 ve üzeri) |
| Windows Server 2012 R2 ve en son SP'ler |64 bit |Standard, Datacenter, Foundation |
| Windows Server 2012 ve en son SP'ler |64 bit |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 ve en son SP'ler |64 bit |Standard, Workgroup |
| Windows Storage Server 2012 ve en son SP'ler |64 bit |Standard, Workgroup |

Windows Server yinelenenleri kaldırma özelliği kullanılarak DPM depolama alanında yinelenenleri kaldırma. Hakkında daha fazla bilgi [DPM ve yinelenenleri kaldırma](https://technet.microsoft.com/library/dn891438.aspx) Hyper-V Vm'lerini dağıtıldığında birlikte çalışır.

> [!NOTE]
> Azure Backup sunucusu, ayrılmış, tek amaçlı bir sunucuda çalışacak şekilde tasarlanmıştır. Azure Backup sunucusu üzerinde yüklenemiyor:
> - Etki alanı denetleyicisi olarak çalıştırılan bir bilgisayar
> - Uygulama Sunucusu rolünün yüklü olduğu bir bilgisayar
> - System Center Operations Manager yönetim grubu olan bir bilgisayar
> - Exchange Server’ın çalıştırıldığı bir bilgisayar
> - Küme düğümü olan bir bilgisayar

Her zaman Azure Backup sunucusu bir etki alanına katılın. Sunucunun farklı bir etki alanına taşımayı planlıyorsanız, önce Azure Backup sunucusu yükledikten sonra sunucuyu yeni etki alanına katılın. Dağıtım tamamlandıktan sonra var olan bir Azure Backup sunucusu makine yeni bir etki alanına taşıma *desteklenmiyor*.

Yedekleme verileri Azure'a göndermek ya da yerel olarak koru olsun, Azure Backup sunucusu bir kurtarma Hizmetleri kasası ile kayıtlı olması gerekir.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama
Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanıza olanak sağlar. Varsayılan olarak, Kurtarma Hizmetleri kasaları, coğrafi olarak yedekli depolama kullanın. Bu kasa birincil kasanız varsa depolama seçeneği coğrafi olarak yedekli depolama şeklinde bırakın. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu ve Ayarlar menüsünü açmak için kasanızı seçin. Varsa **ayarları** değil menüsünü açın, **tüm ayarlar** kasa panosunda.
2. Üzerinde **ayarları** menüsünde tıklatın **Yedekleme Altyapısı** > **yedekleme yapılandırması** açmak için **yedekleme yapılandırması**dikey penceresi. Üzerinde **yedekleme yapılandırması** menüsünde, kasanız için depolama çoğaltma seçeneğini belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Kasanız için depolama seçeneğini belirledikten sonra, VM'yi kasa ile ilişkilendirmek için hazır duruma gelirsiniz. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

## <a name="software-package"></a>Yazılım paketi
### <a name="downloading-the-software-package"></a>Yazılım paketini indirme
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Açık kurtarma Hizmetleri kasanız zaten varsa, 3. adıma geçin. Açık bir kurtarma Hizmetleri kasanız yoksa, ancak Azure portalında, ana menüsündeki olan tıklatmak **Gözat**.

   * Kaynak listesinde **Kurtarma Hizmetleri** yazın.
   * Yazmaya başladığınızda liste, girdinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

     ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     Kurtarma Hizmetleri kasalarının listesi görünür.
   * Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

     Seçilen kasa panosu açılır.

     ![Kasa dikey penceresini açma](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. **Ayarları** dikey penceresi varsayılan olarak açılır. Kapalıysa, tıklayarak **ayarları** ayarları dikey penceresini açın.

    ![Kasa dikey penceresini açma](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Tıklayın **yedekleme** Başlarken Sihirbazı'nı açın.

    ![Yedeklemeyi kullanmaya başlama](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    İçinde **yedeklemeye başlama** açılan dikey pencerede **yedekleme hedefini** otomatik olarak seçilir.

    ![Yedekleme hedefleri-varsayılan-açılan](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. İçinde **yedekleme hedefi** dikey penceresinde, gelen **iş yükünüz çalıştığı** menüsünde **şirket içi**.

    ![Şirket içi ve hedefleri olarak iş yükleri](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    Gelen **neleri yedeklemek istiyorsunuz?** açılan menüsünde, Azure Backup sunucusu kullanarak korumak istediğiniz iş yükleri seçin ve ardından **Tamam**.

    **Yedeklemeye başlama** Sihirbazı anahtarları **altyapıyı hazırlama** iş yüklerini Azure'a yedeklemek için seçeneği.

   > [!NOTE]
   > Makaledeki yönergeleri izleyerek ve Azure Backup aracısı kullanarak dosya ve klasörleri yedeklemek istiyorsanız, öneririz [ilk bakış: dosyaları ve klasörleri](backup-try-azure-backup-in-10-mins.md). Birden fazla dosya ve klasörleri korumak için oluşturacağınız veya koruma gereksinimlerini gelecekte genişletin planlıyorsanız, bu iş yüklerini seçin.
   >
   >

    ![Başlarken Sihirbazı'nı değişiklik alma](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. İçinde **altyapıyı hazırlama** 'ye tıklayın dikey **indirme** Azure Backup Sunucusu'nu yükleme ve indirme kasa kimlik bilgileri için bağlantılar. Kurtarma Hizmetleri kasasına Azure Backup sunucusu kaydı sırasında kasa kimlik bilgilerini kullanın. Bağlantılar yazılım paketini indirdiğiniz indirme merkezine yönlendirir.

    ![Azure Backup sunucusu için altyapıyı hazırlama](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Tüm dosyaları seçin ve tıklayın **sonraki**. Microsoft Azure Backup indirme sayfasından gelen tüm dosyaları indirmek ve tüm dosyaları aynı klasöre yerleştirin.

    ![İndirme Merkezinden 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    İndirme boyutu tüm dosyaların birlikte > 3 G olduğundan, 10 MB/sn üzerinde indirme bağlantısı İndirmenin 60 dakikaya kadar sürebilir.

### <a name="extracting-the-software-package"></a>Yazılım paketi ayıklanıyor
Tüm dosyaları indirdikten sonra tıklayın **MicrosoftAzureBackupInstaller.exe**. Bu başlatır **Microsoft Azure Backup Kurulum Sihirbazı'nı** Kurulum dosyaları sizin belirttiğiniz bir konuma ayıklayın. Sihirbaza devam edin ve tıklayarak **ayıklamak** ayıklama işlemi başlatmak için düğme.

> [!WARNING]
> Kurulum dosyalarını ayıklamak için en az 4GB boş alan gerekir.
>
>

![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Ayıklama işlemi tamamlandı, yeni ayıklanan başlatmak için bu kutuyu işaretlediğinizde *setup.exe* tıklayın ve Microsoft Azure Backup Sunucusu'nu yüklemeye başlamak için **son** düğmesi.

### <a name="installing-the-software-package"></a>Yazılım paketini yükleme
1. Tıklayın **Microsoft Azure Backup** ve Kurulum sihirbazını başlatmak için.

    ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Hoş Geldiniz ekranında tıklayın **sonraki** düğmesi. Sayfasına yönlendirileceksiniz *önkoşul denetimleri* bölümü. Bu ekranda tıklayın **denetleyin** Azure Backup sunucusu için donanım ve yazılım önkoşullarının karşılanıp karşılanmadığını olmadığını belirlemek için. Tüm Önkoşullar başarıyla karşılandıysa makinenin bu gereksinimleri karşıladığını belirten bir ileti görürsünüz. Tıklayarak **sonraki** düğmesi.

    ![Azure Backup sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup sunucusu, SQL Server Enterprise gerektirir. Ayrıca, Azure Backup sunucusu yükleme paketini kendi SQL kullanmak istemiyorsanız gereken uygun SQL Server ikili dosyaları ile sunulur. Yeni bir Azure Backup sunucusu yüklemesi ile başlatılırken seçeneğini belirlemelisiniz **yeni SQL Server örneği bu kurulum ile yükleme** tıklatıp **denetle ve Yükle** düğmesi. Önkoşullar başarıyla yüklendikten sonra tıklayın **sonraki**.

    ![Azure Backup sunucusu - SQL denetimi](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Makineyi yeniden başlatmak için bir öneri ile bir hata oluşması durumunda bunu ve tıklayın **tekrar kontrol edin**. Herhangi bir SQL yapılandırma sorunu durumunda SQL SQL yönergelerine göre yeniden yapılandırmanız ve mevcut SQL örneğini kullanarak MABS yüklemesi/yükseltmesi için yeniden deneyin.

   > [!NOTE]
   > Azure Backup sunucusu, uzak bir SQL Server örneği ile çalışmaz. Azure Backup sunucusu tarafından kullanılan örnek yerel olması gerekir. MABS Kurulum, MABS için mevcut bir SQL server kullandığınız durumda yalnızca kullanımını destekler. *örnekleri adlı* SQL Server.

   **El ile yapılandırma**

   Kendi SQL örneğini kullandığınızda, ana veritabanı sysadmin rolüne BUILTIN\Administrators eklediğiniz emin olun.

    **SQL 2017 ile SSRS yapılandırma**

    SQL 2017 kendi örneğini kullanırken, SSRS el ile yapılandırmanız gerekiyor. SSRS yapılandırma sonrasında emin *Isınitialized* özelliği SSRS *True*. True olarak ayarlandığında, MABS SSRS zaten yapılandırıldı ve SSRS yapılandırma atlanacak varsayar.

    SSRS yapılandırma için aşağıdaki değerleri kullanın:

        - Service Account: ‘Use built-in account’ should be Network Service
        - Web Service URL: ‘Virtual Directory’ should be ReportServer_<SQLInstanceName>
        - Database: DatabaseName should be ReportServer$<SQLInstanceName>
        - Web Portal URL: ‘Virtual Directory’ should be Reports_<SQLInstanceName>

    [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode?view=sql-server-2017) SSRS yapılandırma hakkında.

4. Microsoft Azure Backup sunucusu dosyaları yüklenmesi için bir konum girin ve tıklayın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Karalama konumu, Azure yedekleme için bir gereksinimdir. Karalama konumu buluta yedeklenmesini planlı verilerin en az %5 olduğundan emin olun. Disk koruması için ayrı disklere yükleme tamamlandıktan sonra yapılandırılması gerekir. Depolama havuzları hakkında daha fazla bilgi için bkz. [depolama havuzlarını yapılandırın ve disk depolama](https://technet.microsoft.com/library/hh758075.aspx).
5. Kısıtlı yerel kullanıcı hesapları için güçlü bir parola sağlayın ve tıklayın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Kullanmak isteyip istemediğinizi seçin *Microsoft Update* güncelleştirmeleri denetlemek ve **sonraki**.

   > [!NOTE]
   > Windows güncelleştirme Microsoft Update, Windows ve Microsoft Azure Backup sunucusu gibi diğer ürünleri için güvenlik ve önemli güncelleştirmeler sunar. yeniden yönlendirme olması önerilir.
   >
   >

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Gözden geçirme *ayarların özeti* tıklatıp **yükleme**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. Yükleme aşamada gerçekleşir. İlk aşamada, Microsoft Azure kurtarma Hizmetleri Aracısı sunucusuna yüklenir. Sihirbaz aynı zamanda Internet bağlantısı için denetler. Internet bağlantısı olup olmadığını yükleme işlemine devam etmek için Aksi durumda, Internet'e bağlanmak için proxy ayrıntıları sağlamanız gerekir.

    Sonraki adım, Microsoft Azure kurtarma Hizmetleri Aracısı yapılandırmaktır. Yapılandırmanın parçası, Kurtarma Hizmetleri kasası için makine kaydedilemiyor, kasa kimlik bilgilerini sağlamanız gerekir. Ayrıca, şirket içi ve Azure arasında gönderilen verilerin şifreleme/şifre çözme için bir parola sağlar. Otomatik olarak bir parola oluşturabilir veya kendi en az 16 karakter parolayı girin. Aracıyı yapılandırılana kadar sihirbazıyla devam edin.

    ![Azure yedekleme Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Microsoft Azure Backup sunucusunun kaydı başarıyla tamamlandıktan sonra Genel Kurulum Sihirbazı'nı SQL Server ve Azure Backup sunucusu bileşenlerinin yapılandırma ve yükleme devam eder. Azure Backup sunucusu bileşenleri SQL Server bileşeni yüklemesi tamamlandıktan sonra yüklenir.

    ![Azure Backup Sunucusu](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Yükleme adım tamamlandığında, ürünün masaüstü simgelerini de oluşturulmuş. Yalnızca ürün başlatmak için simgesini çift tıklatın.

### <a name="add-backup-storage"></a>Yedekleme depolama alanı ekleme
İlk yedek kopyanın Azure Backup sunucusu makinesine bağlı depolama tutulur. Disk ekleme hakkında daha fazla bilgi için bkz. [depolama havuzlarını yapılandırın ve disk depolama](https://docs.microsoft.com/azure/backup/backup-mabs-add-storage).

> [!NOTE]
> Verileri Azure'a göndermek planlama olsa bile, yedekleme depolama alanı ekleme gerekir. Azure Backup sunucusu geçerli mimaride Azure yedekleme kasası tutan *ikinci* ilk (ve zorunlu) yedek kopyayı yerel depolama sırasında verilerin kopyasını tutar.
>
>

### <a name="install-and-update-the-data-protection-manager-protection-agent"></a>Yükleme ve Data Protection Manager koruma Aracısı güncelleştirme

System Center Data Protection Manager koruma Aracısı, MABS kullanır. [Adımlar şunlardır](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-1807) koruma sunucularınız üzerinde koruma aracısı yüklemek için.

Aşağıdaki bölümlerde, istemci bilgisayarlar için koruma aracılarını güncelleştirme konusunda açıklanmaktadır.

1. Yedek sunucu yöneticisi Konsolu'ndaki seçin **Yönetim** > **aracıları**.

2. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz bilgisayarları seçin.

   > [!NOTE]
   > **Aracı güncelleştirmeleri** sütunu her korumalı bilgisayar için bir koruma Aracısı güncelleştirmesi çıktığında gösterir. İçinde **eylemleri** bölmesinde **güncelleştirme** eylemi, yalnızca korumalı bir bilgisayarın seçili olduğundan ve güncelleştirmeleri kullanılabilir.
   >
   >

3. Seçili bilgisayarlara güncelleştirilmiş koruma aracıları yüklemek için **eylemleri** bölmesinde **güncelleştirme**.

4. Bilgisayar ağa bağlanana kadar ağa bağlı olmayan bir istemci bilgisayar için **aracı durumu** sütun bir durumu gösterir **güncelleştirme Beklemede**.

   Bir istemci bilgisayar ağa bağlandıktan sonra **aracı güncelleştirmeleri** sütununda istemci bilgisayar durumunu gösteren **güncelleştirme**.

## <a name="move-mabs-to-a-new-server"></a>Yeni bir sunucuya MABS Taşı

MABS, depolama korurken yeni bir sunucuya taşımanız gerekiyorsa adımlar şunlardır. Yalnızca tüm veriler ise Modern yedekleme depolama alanı üzerinde bu yapılabilir.


  > [!IMPORTANT]
  > - Yeni sunucu adını, özgün Azure Backup sunucusu örneğinin adıyla aynı olmalıdır. Önceki depolama havuzu ve Data Protection Manager veritabanı, Kurtarma noktalarını tutmak isteyip kullanmak istiyorsanız, yeni Azure Backup sunucusu örneğinin adını değiştiremezsiniz.
  > - Data Protection Manager veritabanının bir yedeğini olması gerekir. Veritabanını geri yüklemek gerekir.

1. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz bilgisayarları seçin.
2. Kapatma özgün Azure backup sunucusu veya kablo devre dışı duruma.
3. Makine hesabı active Directory'de sıfırlayın.
4. Server 2016 yeni makineye yüklemek ve aynı makine adı özgün Azure Backup sunucusu olarak adlandırın.
5. Etki alanına katılma
6. Azure Backup Sunucusu'nu V2 veya üzeri (taşıma DPM depolama havuzu diskleri eski sunucu ve içeri aktarma) yükleyin
7. 1\. adımda alınan DPMDB geri yükleyin.
8. Depolama, yedekleme özgün sunucudan yeni sunucuya ekleyin.
9. DPMDB'yi SQL geri yükleme
10. Microsoft Azure Backup'a yeni server CD'sinde yönetici komut satırından konumu ve bin klasörüne yükleyin

    Örnek yol: C:\Windows\System32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\" 

11. Azure yedekleme, çalıştırma DPMSYNC-SYNC

    Eskileri taşıma yerine DPM depolama havuzuna yeni diskler eklediyseniz, ardından DPMSYNC - reallocatereplica öğesini çalıştırın

## <a name="network-connectivity"></a>Ağ bağlantısı
Azure Backup sunucusu başarıyla çalışmak ürün için Azure Backup hizmeti bağlantısı gerektirir. Makineyi azure'a bağlantısı olup olmadığını doğrulamak için kullanın ```Get-DPMCloudConnection``` Azure Backup sunucusu PowerShell konsolundaki cmdlet'i. Bu cmdlet'in çıktısı TRUE ise bağlantı var. daha sonra başka hiçbir bağlantısı yoktur.

Aynı zamanda, Azure aboneliğiniz iyi durumda olması gerekir. Aboneliğinizin durumunu bulmak ve yönetmek için oturum [abonelik portalı](https://account.windowsazure.com/Subscriptions).

Azure bağlantı ve Azure aboneliğinin durumu öğrendikten sonra sunulan yedekleme/geri yükleme işlevselliği üzerindeki etkisini öğrenmek için aşağıdaki tabloyu kullanabilirsiniz.

| Bağlantı durumu | Azure Aboneliği | Azure'a yedekleme | Diske yedekleme | Azure'dan geri yükleme | Diskten geri yükleme |
| --- | --- | --- | --- | --- | --- |
| Bağlı |Etkin |İzin Verildi |İzin Verildi |İzin Verildi |İzin Verildi |
| Bağlı |Süresi dolmuş |Durduruldu |Durduruldu |İzin Verildi |İzin Verildi |
| Bağlı |Sağlaması kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |
| Kayıp bağlantı > 15 gün |Etkin |Durduruldu |Durduruldu |İzin Verildi |İzin Verildi |
| Kayıp bağlantı > 15 gün |Süresi dolmuş |Durduruldu |Durduruldu |İzin Verildi |İzin Verildi |
| Kayıp bağlantı > 15 gün |Sağlaması kaldırıldı |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |

### <a name="recovering-from-loss-of-connectivity"></a>Bağlantı kaybından kurtarma
Bir güvenlik duvarı veya Azure'a erişimi engelleyen bir proxy varsa, güvenlik duvarı/proxy profilinde aşağıdaki etki alanı adreslerini beyaz listeye gerekir:

* `http://www.msftncsi.com/ncsi.txt`
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*. microsoftonline.com
* \*. windows.net

Bağlantının Azure Azure Backup sunucusu makineye geri sonra gerçekleştirilebilecek işlemleri Azure abonelik durumuna göre belirlenir. Yukarıdaki tabloda, makine bağlı"sonra" izin verilen işlemleri hakkında ayrıntılar bulunur.

### <a name="handling-subscription-states"></a>Abonelik durumları işleme
Bir Azure aboneliğine ait olması mümkündür bir *süresi dolan* veya *yetki kaldırıldı* durumunu *etkin* durumu. Durumu değil ancak bunun bazı ürün davranışı şifrelemelerini *etkin*:

* A *yetki kaldırıldı* abonelik sağlaması dönemin işlevselliğini kaybeder. Durumun dönüş üzerinde *etkin*, yedekleme/geri yükleme ürün işlevselliğini hale getirilir. Yerel disk yedekleme verilerine, ayrıca ile yeterince uzun bir saklama dönemi bıraktıysanız alınabilir. Ancak, abonelik girdikten sonra Azure yedekleme verilerinde irretrievably kayıp *yetki kaldırıldı* durumu.
* Bir *süresi dolan* abonelik hale getirdiği kadar işlevselliği için yalnızca kaybeder *etkin* yeniden. Aboneliği olan bir dönem için zamanlanan tüm yedeklemeler *süresi dolan* çalışmaz.

## <a name="upgrade-mabs"></a>MABS yükseltme
MABS yükseltmek için aşağıdaki yordamları kullanın.

### <a name="upgrade-from-mabs-v2-to-v3"></a>V3 için MABS V2'den yükseltme

> [!NOTE]
> 
> MABS V2 MABS V3 yüklemek için bir önkoşul olmamasına. Ancak, MABS V3 yalnızca MABS V2'den yükseltme yapabilirsiniz.

MABS yükseltmek için aşağıdaki adımları kullanın:

1. MABS V3 MABS V2'den yükseltme için işletim sisteminizi Windows Server 2016 veya Windows Server 2019 gerekirse yükseltin.

2. Sunucunuzu yükseltin. Adımları benzer [yükleme](#install-and-upgrade-azure-backup-server). Ancak, SQL ayarları için SQL örneğinizin SQL 2017'ye yükseltmek veya kendi SQL server 2017 kullanmak için bir seçenek alırsınız.

   > [!NOTE]
   > 
   > SQL örneğinizin yükseltiliyor, mevcut SQL Raporlama örneği kaldırılır ve bu nedenle MABS yeniden yükseltme girişimi başarısız olur ancak çıkmayın.

   Dikkat edilecek önemli noktalar:

   > [!IMPORTANT]
   > 
   >  SQL 2017 yükseltmesinin bir parçası olarak SQL şifreleme anahtarlarını yedeklemeniz ve Raporlama Hizmetleri kaldırın. SQL server yükseltmesi sonrasında raporlama service(14.0.6827.4788) yüklü & şifreleme anahtarı geri yüklenir.
   > 
   > SQL 2017 elle yapılandırırken başvurmak *SQL 2017 ile SSRS yapılandırma* bölümü altında yükleme yönergeleri.

3. Korumalı sunuculardaki koruma aracılarını güncelleştirin.
4. Yedeklemeler, üretim sunucuları yeniden başlatma gerek kalmadan devam etmelidir.
5. Artık verilerinizi korumaya başlayabilir. Korurken, Modern yedekleme depolama alanı için yükseltme yapıyorsanız, yedeklemeleri depolamak ve denetlemek için sağlanan alanı altında istediğiniz birimleri de seçebilirsiniz. [Daha fazla bilgi edinin](backup-mabs-add-storage.md).

> [!NOTE]
> 
> MABS V1'den V2'ye yükseltiyorsanız, işletim sisteminizi Windows Server 2016 veya Windows Server 2012 R2 olduğundan emin olun. System Center 2016 veri koruma Yöneticisi'ni Modern yedekleme depolama alanı gibi yeni özelliklerden yararlanmak için Windows Server 2016'da yedekleme sunucusu V2 yüklemeniz gerekir. Yükseltme veya yedekleme sunucusu V2'ı yüklemeden önce okumanız [yükleme önkoşulları](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-1807#setup-prerequisites) MABS için geçerlidir.

## <a name="troubleshooting"></a>Sorun giderme
Microsoft Azure Backup sunucusu kurulumu aşamasında (veya yedekleme veya geri yükleme sırasında) hatalarla başarısız olursa, şuna başvurun [hata kodları belge](https://support.microsoft.com/kb/3041338) daha fazla bilgi için.
Ayrıca başvurabilirsiniz [Azure Backup ilgili sık sorulan sorular](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı bilgi edinebilirsiniz [ortamınızı DPM'ye hazırlama](https://technet.microsoft.com/library/hh758176.aspx) Microsoft TechNet sitesindeki. Ayrıca, Azure Backup sunucusu dağıtılan kaldırılabilir ve desteklenen yapılandırmalar hakkında bilgi içerir. Kullanabileceğiniz bir dizi [PowerShell cmdlet'i](https://docs.microsoft.com/powershell/module/dataprotectionmanager/?view=systemcenter-ps-2016) çeşitli işlemleri gerçekleştirmek için.

Bu makale, Microsoft Azure Backup sunucusu kullanarak iş yükü koruması daha derin bir anlayış kazanmak için kullanabilirsiniz.

* [SQL Server Yedekleme](backup-azure-backup-sql.md)
* [SharePoint server yedekleme](backup-azure-backup-sharepoint.md)
* [Alternatif Sunucu yedekleme](backup-azure-alternate-dpm-server.md)
