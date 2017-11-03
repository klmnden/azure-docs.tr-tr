---
title: "İş yüklerini Azure'a yedeklemek için Azure yedekleme sunucusu kullanın | Microsoft Docs"
description: "Azure yedekleme sunucusu korumak veya Azure portalına iş yüklerini yedeklemek için kullanın."
services: backup
documentationcenter: 
author: PVRK
manager: shivamg
editor: 
keywords: "Azure backup sunucusu; iş yüklerini korumak; iş yüklerini yedeklemeye"
ms.assetid: e7fb1907-9dc1-4ca1-8c61-50423d86540c
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: c54468d71e0b383916e49847576a98303d659d38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="preparing-to-back-up-workloads-using-azure-backup-server"></a>Azure Backup Sunucusu kullanarak iş yüklerini yedeklemeye hazırlama
> [!div class="op_single_selector"]
> * [Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Bu makalede Azure yedekleme sunucusu kullanarak iş yüklerini yedeklemeye ortamınız için nasıl hazırlanılacağını açıklamaktadır. Azure yedekleme Sunucusu'nu kullanarak, Hyper-V sanal makineleri, Microsoft SQL Server, SharePoint Server, Microsoft Exchange ve Windows istemcileri gibi uygulama iş yükleri tek bir konsoldan koruyabilirsiniz.

> [!NOTE]
> Azure yedekleme sunucusu artık VMware Vm'leri koruyabilir ve Gelişmiş güvenlik özellikleri sağlar. Aşağıdaki bölümlerde açıklandığı gibi ürün yükleyin; Güncelleştirme 1 ve en son Azure Yedekleme aracısı uygulanır. Azure yedekleme sunucusu VMware sunucularıyla yedekleme hakkında daha fazla bilgi için bkz [VMware sunucusunu yedeklemek üzere kullanma Azure yedekleme sunucusu](backup-azure-backup-server-vmware.md). Güvenlik özellikleri hakkında bilgi edinmek için bkz [Azure yedekleme güvenlik özellikleri belgelerine](backup-azure-security-feature.md).
>
>

Azure'da VM gibi bir hizmet (Iaas) iş yükleri olarak altyapısı da koruyabilir.

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki dağıtım modeline sahiptir: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Resource Manager modelini kullanarak dağıtılan VM'ler geri yüklemek için bilgi ve yordamlar sağlar.
>
>

Azure Backup sunucusu iş yükü yedekleme işlevlerinin çoğunu Data Protection Manager (DPM) devralır. Bu makale bağlantılar DPM belgelerine paylaşılan işlevselliği bazıları açıklanmaktadır. Ancak Azure yedekleme sunucusu ile aynı işlevselliği DPM çoğunu paylaşır. Azure yedekleme sunucusu başlamıyor banda yedeklemek veya System Center ile tümleşik çalışıyor.

## <a name="1-choose-an-installation-platform"></a>1. Bir yükleme platformu seçin
Azure yedekleme sunucusu Başlarken hazırlarken ve çalışırken doğrultusunda ilk adım, bir Windows Server ayarlamaktır. Sunucunuz, Azure veya şirket içi olabilir.

### <a name="using-a-server-in-azure"></a>Azure üzerinde bir sunucu kullanarak
Azure yedekleme sunucusu çalıştıran bir sunucu seçerken, bir Windows Server 2012 R2 Datacenter galeri görüntüsüyle Başlat önerilir. Makale [ilk Windows sanal makinenizi Azure Portalı'nda oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), hiçbir zaman Azure önce kullanmış olduğunuz olsa bile, azure'da önerilen sanal makine ile çalışmaya başlama için bir öğretici sağlar. Sunucu sanal makine (VM) için önerilen en düşük gereksinimler olmalıdır: A2 standart iki çekirdek ve 3.5 GB RAM ile.

Azure yedekleme sunucusu ile iş yüklerini koruma pek çok küçük farklar vardır. Makale [bir Azure sanal makinesi olarak DPM yükleme](https://technet.microsoft.com/library/jj852163.aspx), yardımcı olur, bu küçük farklar açıklanmaktadır. Makine dağıtmadan önce bu makalenin tümüyle okuyun.

### <a name="using-an-on-premises-server"></a>Bir şirket içi sunucu kullanma
Temel sunucu Azure'da çalışması istemiyorsanız, bir Hyper-V VM, bir VMware sanal veya fiziksel bir ana bilgisayar sunucu çalıştırabilirsiniz. Sunucu donanımı için önerilen en düşük gereksinimler iki çekirdek ve 4 GB RAM verilmiştir. Desteklenen işletim sistemleri aşağıdaki tabloda listelenmiştir:

| İşletim Sistemi | Platform | SKU |
|:--- | --- |:--- |
| Windows Server 2012 R2 ve en son SP'ler |64 bit |Standard, Datacenter, Foundation |
| Windows Server 2012 ve en son SP'ler |64 bit |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 ve en son SP'ler |64 bit |Standard, Workgroup |
| Windows Storage Server 2012 ve en son SP'ler |64 bit |Standard, Workgroup |

Windows Server yinelenenleri kaldırma kullanılarak DPM depolama alanında yinelenenleri kaldırma. Hakkında daha fazla bilgi [DPM ve yinelenenleri kaldırma](https://technet.microsoft.com/library/dn891438.aspx) Hyper-V VM dağıtıldığında birlikte çalışır.

> [!NOTE]
> Azure yedekleme sunucusu ayrılmış, tek amaçlı bir sunucuda çalışmak üzere tasarlanmıştır. Azure yedekleme sunucusu üzerinde yüklenemiyor:
> - Bir etki alanı denetleyicisi olarak çalıştıran bir bilgisayar
> - Uygulama Sunucusu rolünün yüklü olduğu bir bilgisayar
> - System Center Operations Manager yönetim sunucusu olan bir bilgisayar
> - Exchange Server çalıştıran bir bilgisayar
> - Kümenin bir düğümü olan bir bilgisayar

Her zaman Azure yedekleme sunucusu bir etki alanına katılın. Sunucunun farklı bir etki alanına taşımayı planlıyorsanız, sunucunun Azure yedekleme sunucusu yüklemeden önce yeni etki alanına önerilir. Dağıtım tamamlandıktan sonra var olan bir Azure yedekleme sunucusu makine yeni bir etki alanına taşıma *desteklenmiyor*.

## <a name="2-recovery-services-vault"></a>2. Kurtarma Hizmetleri kasası
Azure'a yedekleme verileri göndermek ya da yerel olarak tutmak isteyip yazılım Azure'a bağlı olması gerekir. Daha fazla olması belirli, Azure yedekleme sunucusu makine kurtarma Hizmetleri kasası ile kayıtlı olması gerekir.

Kurtarma hizmetleri kasası oluşturmak için:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasası** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png) <br/>

    Kurtarma Hizmetleri kasalarının listesi görüntülenir.
3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-microsoft-azure-backup/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası dikey penceresi açılır ve sizden bir **Ad**, **Abonelik**, **Kaynak Grubu** ve **Konum** sağlamanızı ister.

    ![Kurtarma Hizmetleri kasası oluşturma 5. adım](./media/backup-azure-microsoft-azure-backup/rs-vault-attributes.png)
4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.
5. Kullanılabilir abonelik listesini görmek için **Abonelik** seçeneğine tıklayın. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Yalnızca kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olur.
6. Kullanılabilir Kaynak grubu listesini görmek için **Kaynak grubu** seçeneğine, yeni bir Kaynak grubu oluşturmak için de **Yeni** seçeneğine tıklayın. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md)
7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın.
8. **Oluştur**'a tıklayın. Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalda sağ üst alandaki durum bildirimlerini izleyin.
   Kasanız oluşturulduktan sonra portalda açılır.

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama
Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanıza olanak sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Bu kasaya birincil kasanız coğrafi olarak yedekli depolamaya ayarlanmış depolama seçeneği bırakın. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy.md#geo-redundant-storage) ve [yerel olarak yedekli](../storage/common/storage-redundancy.md#locally-redundant-storage) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu ve Ayarlar dikey penceresini açmak için kasanızı seçin. **Ayarlar** dikey penceresi açılmazsa kasa panosunda **Tüm ayarlar** seçeneğine tıklayın.
2. **Ayarlar** dikey penceresinde, **Yedekleme Yapılandırması** dikey penceresini açmak için **Yedekleme Altyapısı** > **Yedekleme Yapılandırması**'na tıklayın. **Yedekleme Yapılandırması** dikey penceresinde, kasanıza yönelik depolama çoğaltma seçeneğini belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Kasanız için depolama seçeneğini belirledikten sonra, VM'yi kasa ile ilişkilendirmek için hazır duruma gelirsiniz. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

## <a name="3-software-package"></a>3. Yazılım paketi
### <a name="downloading-the-software-package"></a>Yazılım paketini indirme
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Açık kurtarma Hizmetleri kasanız zaten varsa, 3. adıma geçin. Açık Kurtarma Hizmetleri kasanız yoksa ancak Azure portaldaysanız Hub menüsünde **Gözat**'a tıklayın.

   * Kaynak listesinde **Kurtarma Hizmetleri** yazın.
   * Yazmaya başladığınızda liste, girdinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

     ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     Kurtarma Hizmetleri kasalarının listesi görünür.
   * Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

     Seçilen kasa panosu açılır.

     ![Kasa dikey penceresini açma](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. **Ayarları** dikey varsayılan olarak açılır. Kapalıysa, tıklayın **ayarları** ayarları dikey penceresini açın.

    ![Kasa dikey penceresini açma](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Tıklatın **yedekleme** Başlarken Sihirbazı'nı açın.

    ![Başlarken yedekleme](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    İçinde **yedekleme ile çalışmaya başlama** açar, dikey **yedekleme hedefleri** otomatik olarak seçilir.

    ![Yedekleme hedefleri-varsayılan-açılan](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. İçinde **yedekleme hedefi** dikey penceresinde, gelen **, iş yükünü çalıştırdığı** menüsünde, select **şirket içi**.

    ![Şirket içi ve hedefleri olarak iş yükleri](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    Gelen **neleri yedeklemek istiyorsunuz?** açılır menüsünde, Azure yedekleme sunucusu kullanarak korumak istediğiniz iş yüklerini seçin ve ardından **Tamam**.

    **Yedekleme ile çalışmaya başlama** Sihirbazı anahtarları **altyapıyı hazırlama** iş yüklerini Azure'a yedeklemek için seçeneği.

   > [!NOTE]
   > Azure Backup aracısını kullanan ve aşağıdaki makalede yer alan yönergeleri dosyaları ve klasörleri yedeklemek istiyorsanız, öneririz [ilk bakış: dosyaları ve klasörleri yedekleyin](backup-try-azure-backup-in-10-mins.md). Birden çok dosya ve klasörleri korumak için kalacaklarını ya da koruma gereksinimlerini gelecekte genişletin planlıyorsanız, bu iş yüklerini seçin.
   >
   >

    ![Başlarken Sihirbazı'nı değişiklik alma](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. İçinde **altyapıyı hazırlama** 'ye tıklayın dikey **karşıdan** bağlantılarını Azure yedekleme sunucusu yükleme ve indirme kasa kimlik bilgileri. Azure yedekleme sunucusu kaydı kurtarma Hizmetleri kasasına sırasında kasa kimlik bilgilerini kullanın. Bağlantılar yazılım paketini indirdiğiniz indirme Merkezi'ne yönlendirir.

    ![Azure yedekleme sunucusu için altyapıyı hazırlama](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Tüm dosyaları seçin ve tıklatın **sonraki**. Microsoft Azure yedekleme indirme sayfasından gelen tüm dosyaları indirin ve tüm dosyaları aynı klasöre yerleştirin.

    ![İndirme Merkezinden 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    İndirme boyutu tüm dosyaların birlikte > 3 G olduğundan, yüklemeyi tamamlamak 60 dakika kadar sürebilir bağlantı 10 MB/sn üzerinde indirin.

### <a name="extracting-the-software-package"></a>Yazılım paketi ayıklanıyor
Tüm dosyaları indirdikten sonra tıklayın **MicrosoftAzureBackupInstaller.exe**. Bu başlatır **Microsoft Azure yedekleme Kurulum Sihirbazı'nı** Kurulum dosyaları sizin belirttiğiniz bir konuma ayıklayın. Sihirbaza devam edin ve tıklayın **ayıklamak** ayıklama işlemine başlamak için düğmesi.

> [!WARNING]
> Kurulum dosyalarını ayıklamak için en az 4GB boş disk alanı gereklidir.
>
>

![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Ayıklama işlemi tamamlandı, istemcinin ayıklanan başlatmak için kutusunu işaretlediğinizde *setup.exe* Microsoft Azure yedekleme sunucusu yüklemeye başlamak için **son** düğmesi.

### <a name="installing-the-software-package"></a>Yazılım paketi yükleniyor
1. Tıklatın **Microsoft Azure yedekleme** Kurulum Sihirbazı'nı başlatmak için.

    ![Microsoft Azure yedekleme Kurulum Sihirbazı](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Hoş Geldiniz ekranında tıklatın **sonraki** düğmesi. Bu, olanak sürer *önkoşul denetler* bölümü. Bu ekranda tıklatın **denetleyin** Azure yedekleme sunucusu için donanım ve yazılım önkoşullarının karşılanıp karşılanmadığını varsa belirlemek için. Tüm Önkoşullar başarıyla karşılanıyorsa makine gereksinimlerini karşıladığını belirten bir ileti görürsünüz. Tıklayın **sonraki** düğmesi.

    ![Azure yedekleme sunucusu - Hoş Geldiniz ve önkoşulları denetleyin](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure yedekleme sunucusu SQL Server Standard gerektirir ve uygun SQL Server ikili gereken Azure yedekleme sunucusu yükleme paketi ile birlikte gelen sunar. Yeni bir Azure yedekleme sunucusu yüklemesine başlatırken seçeneğini seçmeniz gerekir **yeni SQL Server örneği bu kurulum ile yükleme** tıklatıp **denetle ve Yükle** düğmesi. Önkoşullar başarıyla yüklendikten sonra tıklayın **sonraki**.

    ![Azure yedekleme sunucusu - SQL denetimi](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Makineyi yeniden başlatmak için bir öneri bir arıza durumunda bunu ve tıklayın **yeniden denetle**.

   > [!NOTE]
   > Azure yedekleme sunucusu uzak bir SQL Server örneği ile çalışmaz. Azure yedekleme sunucusu tarafından kullanılan örnek yerel olması gerekir.
   >
   >
4. Microsoft Azure yedekleme sunucusu dosyaların yüklenmesi için bir konum belirtin ve tıklatın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Azure yedekleme bir zorunluluk karalama konumdur. Karalama konumun buluta yedeklenmesi için planlanan veri % 5'en az olduğundan emin olun. Disk koruması için ayrı diskin yükleme işlemi tamamlandıktan sonra yapılandırılması gerekir. Depolama havuzları hakkında daha fazla bilgi için bkz: [depolama havuzlarını yapılandırın ve disk depolama](https://technet.microsoft.com/library/hh758075.aspx).
5. Kısıtlı yerel kullanıcı hesapları için güçlü bir parola sağlayın ve tıklatın **sonraki**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Kullanmak mı istediğinizi seçin *Microsoft Update* güncelleştirmeleri denetlemek ve tıklayın **sonraki**.

   > [!NOTE]
   > Windows Update Microsoft Update, Windows ve Microsoft Azure yedekleme sunucusu gibi diğer ürünleri için güvenlik ve önemli güncelleştirmeler sunar yönlendirmek olması önerilir.
   >
   >

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Gözden geçirme *ayarların özeti* tıklatıp **yükleme**.

    ![Microsoft Azure yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. Yükleme aşamada gerçekleşir. İlk aşamada Microsoft Azure kurtarma Hizmetleri Aracısı sunucusuna yüklenir. Sihirbaz aynı zamanda Internet bağlantısını denetler. Internet bağlantısı olup olmadığını yüklemeye devam edebilirsiniz, aksi durumda, Internet'e bağlanmak için proxy ayrıntılarını sağlamanız gerekir.

    Sonraki adım, Microsoft Azure kurtarma Hizmetleri Aracısı yapılandırmaktır. Yapılandırmasının parçası olarak, Kurtarma Hizmetleri kasası makineye kaydetmek için kasa kimlik bilgilerini sağlamanız gerekir. Ayrıca, şirket içi ve Azure arasında gönderilen verilerin şifreleme/şifre çözme için bir parola sağlar. Otomatik olarak bir parola oluşturmak veya kendi en az 16 karakter parola girin. Aracı yapılandırılana kadar sihirbaza devam edin.

    ![Azure yedekleme Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Microsoft Azure yedekleme sunucusu kaydı başarıyla tamamlandığında, Genel Kurulum Sihirbazı'nı yükleme ve yapılandırma SQL Server ve Azure yedekleme sunucusu bileşenleri devam eder. SQL Server bileşen yüklemesi tamamlandıktan sonra Azure yedekleme sunucusu bileşenleri yüklenir.

    ![Azure Backup Sunucusu](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Yükleme adım tamamlandığında, ürünün masaüstü simgelerini de oluşturulmuş. Yalnızca ürün başlatmak için simgesini çift tıklatın.

### <a name="add-backup-storage"></a>Yedekleme depolama ekleme
İlk yedek kopyayı Azure yedekleme sunucusu makineye bağlı depolama üzerinde tutulur. Diskleri ekleme hakkında daha fazla bilgi için bkz: [depolama havuzlarını yapılandırın ve disk depolama](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Azure'a veri göndermeyi planladığınız olsa bile yedekleme depolama alanı eklemeniz gerekir. Azure yedekleme sunucusu geçerli mimarisinde Azure yedekleme kasası tutan *ikinci* yerel depolama sırasında verilerin kopyasını ilk (ve zorunlu) yedek kopyasını tutar.
>
>

## <a name="4-network-connectivity"></a>4. Ağ bağlantısı
Azure yedekleme sunucusu Azure yedekleme hizmetine başarılı olarak çalışması ürün için bağlantısı gerektirir. Makine Azure bağlantısı olup olmadığını doğrulamak için kullanın ```Get-DPMCloudConnection``` Azure yedekleme sunucusu PowerShell konsolundaki cmdlet'i. Bağlantı olduğundan, cmdlet'in çıktısı, TRUE ise başka bağlantısı yok.

Aynı anda Azure aboneliği sağlıklı bir durumda olması gerekir. Aboneliğinizin durumunu bulmak ve yönetmek için oturum [abonelik portal](https://account.windowsazure.com/Subscriptions).

Azure bağlantı ve Azure abonelik durumu öğrendikten sonra sunulan yedekleme/geri yükleme işlevselliği üzerindeki etkisini öğrenmek için aşağıdaki tabloyu kullanabilirsiniz.

| Bağlantı durumu | Azure Aboneliği | Azure'a yedekleme | Diske yedekleme | Azure'dan geri yükleme | Disk, geri yükleme |
| --- | --- | --- | --- | --- | --- |
| bağlı |Etkin |İzin verilen |İzin verilen |İzin verilen |İzin verilen |
| bağlı |Süresi dolmuş |Durduruldu |Durduruldu |İzin verilen |İzin verilen |
| bağlı |Sağlaması kaldırılıyor. sağlaması |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |
| Kayıp bağlantısı > 15 gün |Etkin |Durduruldu |Durduruldu |İzin verilen |İzin verilen |
| Kayıp bağlantısı > 15 gün |Süresi dolmuş |Durduruldu |Durduruldu |İzin verilen |İzin verilen |
| Kayıp bağlantısı > 15 gün |Sağlaması kaldırılıyor. sağlaması |Durduruldu |Durduruldu |Silinen durduruldu ve Azure kurtarma noktaları |Durduruldu |

### <a name="recovering-from-loss-of-connectivity"></a>Bağlantı kaybına karşı kurtarma
Bir güvenlik duvarı veya Azure'a erişimi engelleyen bir proxy varsa, güvenlik duvarı/proxy profilinde aşağıdaki etki alanı adreslerini beyaz liste ile gerekir:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Azure bağlantı Azure yedekleme sunucusu makineye geri yüklendikten sonra gerçekleştirilebilir işlemleri Azure aboneliği durumuna göre belirlenir. Yukarıdaki tabloda makineye bağlı"sonra" izin verilen işlemleri hakkında ayrıntılar bulunur.

### <a name="handling-subscription-states"></a>Abonelik durumları işleme
Azure aboneliğinden olması mümkündür bir *süresi doldu* veya *Deprovisioned* durumunu *etkin* durumu. Ancak durumu ancak bu bazı ürün davranışı şifrelemelerini *etkin*:

* A *Deprovisioned* abonelik bu sağlaması kaldırılıyor. sağlaması dönem için işlevselliğini kaybeder. Dönüş üzerinde *etkin*, yedekleme/geri yükleme ürün işlevselliğini hale getirilir. Yerel disk üzerindeki yedekleme verileri de yeterli büyüklükte bekletme süresi ile bıraktıysanız alınabilir. Abonelik girdikten sonra ancak yedekleme verilerini Azure irretrievably kaybolur *Deprovisioned* durumu.
* Bir *süresi doldu* abonelik hale getirdiği kadar işlevselliği için yalnızca kaybederse *etkin* yeniden. Abonelik olan dönem için zamanlanan yedeklemeleri *süresi doldu* çalışmaz.

## <a name="troubleshooting"></a>Sorun giderme
Microsoft Azure Yedekleme Sunucusu Kurulum aşaması (veya yedekleme veya geri yükleme sırasında) hataları ile başarısız olursa, bu başvuru [hata kodları belge](https://support.microsoft.com/kb/3041338) daha fazla bilgi için.
Ayrıca başvurabilirsiniz [Azure Backup ile ilgili sık sorulan sorular](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı bilgi almak [DPM için ortamınızı hazırlama](https://technet.microsoft.com/library/hh758176.aspx) Microsoft TechNet sitesindeki. Ayrıca, üzerinde Azure yedekleme sunucusu dağıtılan ve kullanılabilecek desteklenen yapılandırmalar hakkında bilgi içerir.

Bu makaleler, Microsoft Azure yedekleme sunucusu kullanarak iş yükü koruması daha derin bir anlayış kazanmak için kullanabilirsiniz.

* [SQL Server Yedekleme](backup-azure-backup-sql.md)
* [SharePoint server yedekleme](backup-azure-backup-sharepoint.md)
* [Diğer sunucu yedekleme](backup-azure-alternate-dpm-server.md)
