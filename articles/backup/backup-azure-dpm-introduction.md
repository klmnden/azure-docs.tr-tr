---
title: Azure portalına iş yüklerini yedeklemek için DPM kullanın
description: Azure Yedekleme hizmetini kullanarak DPM sunucularını yedekleme için bir giriş
services: backup
author: adigan
manager: nkolli
keywords: System Center Data Protection Manager, veri koruma Yöneticisi, dpm yedekleme
ms.service: backup
ms.topic: conceptual
ms.date: 08/15/2017
ms.author: adigan
ms.openlocfilehash: ffb3a5a5729e97e1a9b00072624d7e51842f61f8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605058"
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>DPM ile Azure’a iş yüklerini yedeklemeye hazırlama
> [!div class="op_single_selector"]
> * [Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Bu makalede, System Center Data Protection Manager (DPM) sunucuları ve iş yüklerini korumak için Microsoft Azure Yedekleme kullanılarak giriş bilgileri sağlar. Bunu okuyarak, anlamanız:

* Azure DPM sunucusu yedek nasıl çalışır
* Kesintisiz bir yedekleme deneyimi elde etmek için Önkoşullar
* Tipik hatalarla karşılaşıldı ve bunlarla ilgili Yapılacaklar
* Desteklenen senaryolar

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki dağıtım modeline sahiptir: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Resource Manager modelini kullanarak dağıtılan VM'ler geri yüklemek için bilgi ve yordamlar sağlar.
>
>

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) dosya ve uygulama verileri yedekler. Desteklenen iş yükleri hakkında daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix). DPM için yedeklenen verileri diskte, bantta depolanan veya Microsoft Azure yedekleme ile azure'a yedeklenebilir. DPM Azure Backup ile aşağıdaki gibi etkileşim kurar:

* **Fiziksel sunucu veya şirket içi sanal makine olarak dağıtılan DPM** — varsa DPM fiziksel sunucu olarak veya geri verileri kurtarma Hizmetleri kasasına disk ve bant yanı sıra bir şirket içi Hyper-V sanal makinesi olarak dağıtılan yedekleme.
* **Azure sanal makinesi olarak dağıtılan DPM** — güncelleştirme 3 ' System Center 2012 R2'den DPM Azure sanal makinesi üzerinde dağıtabilirsiniz. DPM Azure sanal makinesi olarak dağıtılırsa, VM'ye bağlı Azure disklerine yedekleme veya kurtarma Hizmetleri kasasına yedekleme tarafından veri depolama boşaltma.

## <a name="why-back-up-dpm-to-azure"></a>Neden DPM'yi Azure'a yedekleme?
DPM sunucularını Azure'a yedekleme iş avantajları şunlardır:

* Şirket içi DPM dağıtımı için Azure banda uzun vadeli dağıtım alternatif olarak kullanın.
* Azure VM'de DPM dağıtmak için Azure diski depodan yük boşaltma. Kurtarma Hizmetleri kasasına eski verilerin depolanması, iş disk yeni verileri depolayarak ölçekleme izin verir.

## <a name="prerequisites"></a>Önkoşullar
Azure yedekleme DPM verileri yedeklemek gibi için hazırlayın:

1. **Kurtarma Hizmetleri kasası oluşturma** — Azure portalında bir kasa oluşturun.
2. **Kasa kimlik bilgilerini indirme** — DPM sunucusunu kurtarma Hizmetleri kasası ile kaydetmek için kullandığınız kimlik bilgilerini indirin.
3. **Azure yedekleme Aracısı'nı yüklemek** — aracı her DPM sunucusuna yükleyin.
4. **Sunucuyu kaydetmek** — DPM sunucusunu kurtarma Hizmetleri kasası ile kaydedin.

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="key-definitions"></a>Temel tanımları
DPM için Azure yedekleme için bazı temel tanımları şunlardır:

1. **Kimlik bilgileri kasası** — kasa kimlik bilgileri, yedekleme verilerini Azure Backup hizmetindeki tanımlanmış bir kasaya göndermek üzere makinenin kimliğini doğrulamak için gereklidir. Kasadan indirilebilir ve 48 saat için geçerlidir.
2. **Parola** — parola buluta yedeklemeleri şifrelemek için kullanılır. Bir kurtarma işlemi sırasında gerekli olduğu gibi dosyayı güvenli bir konuma kaydedin.
3. **Güvenlik PIN** — etkinleştirdiyseniz [güvenlik ayarları](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) kasası güvenlik PIN kritik yedekleme işlemleri gerçekleştirmek için gereklidir. Bu çok faktörlü kimlik doğrulaması, bir güvenlik katmanı ekler. 
4. **Kurtarma klasörünü** — bulut yedeklemelerden geçici olarak bulut Kurtarma sırasında yüklenen terimdir. Boyutu kabaca, paralel olarak kurtarmak istediğiniz yedekleme öğelerin boyutu eşit olmalıdır.


### <a name="1-create-a-recovery-services-vault"></a>1. Kurtarma hizmetleri kasası oluşturma
Kurtarma hizmetleri kasası oluşturmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Hub menüsünde **Gözat**'a tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda liste, girdinize göre filtrelenir. **Kurtarma Hizmetleri kasası** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    Kurtarma Hizmetleri kasalarının listesi görüntülenir.
3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası menüsü açılır sağlamak isteyip istemediğinizi soran bir **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    ![Kurtarma Hizmetleri kasası oluşturma 5. adım](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)
4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.
5. Kullanılabilir abonelik listesini görmek için **Abonelik** seçeneğine tıklayın. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Ancak kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olacaktır.
6. Kullanılabilir Kaynak grubu listesini görmek için **Kaynak grubu** seçeneğine, yeni bir Kaynak grubu oluşturmak için de **Yeni** seçeneğine tıklayın. Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md)
7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın.
8. **Oluştur**’a tıklayın. Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalda sağ üst alandaki durum bildirimlerini izleyin.
   Kasanız oluşturulduktan sonra portalda açılır.

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama
Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanıza olanak sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Bu, birincil yedeklemenizse seçeneği coğrafi olarak yedekli depolamaya ayarlanmış şekilde bırakın. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu ve Ayarlar menüsünü açmak için kasanızı seçin. Varsa **ayarları** değil menüsünü açın, **tüm ayarları** kasa panosunda.
2. Üzerinde **ayarları** menüsünde tıklatın **Yedekleme Altyapısı** > **yedekleme yapılandırması** açmak için **yedekleme yapılandırması**menüsü. Üzerinde **yedekleme yapılandırması** menüsünde, kasanız için depolama çoğaltma seçeneğini belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Kasanız için depolama seçeneğini belirledikten sonra, VM'yi kasa ile ilişkilendirmek için hazır duruma gelirsiniz. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

### <a name="2-download-vault-credentials"></a>2. Kasa kimlik bilgilerini indirme
Kasa kimlik bilgileri dosyası, her bir yedekleme kasası için portal tarafından oluşturulan bir sertifikadır. Portal daha sonra ortak anahtarı Access Control Service'e (ACS) yükler. Kullanıcıya, makine kayıt iş akışı içinde bir giriş olarak verilen iş akışının parçası olarak sertifikanın özel anahtarı kullanılabilir hale getirilir. Bu, yedekleme verilerini Azure Backup hizmetindeki tanımlanmış bir kasaya göndermek üzere makinenin kimliğini doğrular.

Kasa kimlik bilgileri yalnızca kayıt iş akışı sırasında kullanılır. Kasa kimlik bilgileri dosyası tehlikeye olun kullanıcının sorumluluğundadır. Yetkisiz bir kullanıcının eline geçmesi durumunda, söz konusu kasaya diğer makinelerin kaydedilebilmesi için kasa kimlik bilgileri dosyası kullanılabilir. Yedekleme verilerini müşteriye ait olan bir parola kullanılarak şifrelenir gibi ancak var olan yedekleme verilerinin gizliliği tehlikeye giremez. Bu endişenin ortadan kaldırılabilmesi için kasa kimlik bilgileri 48hrs içinde süresi dolacak şekilde ayarlanmıştır. Bir kurtarma Hizmetleri kasa kimlik bilgilerini kez – herhangi bir sayıda yükleyebilirsiniz, ancak kayıt iş akışı sırasında yalnızca en son kasa kimlik bilgilerini geçerlidir.

Kasa kimlik bilgilerini Azure portalından güvenli bir kanal üzerinden indirilir. Azure Backup hizmeti sertifikanın özel anahtarı farkında değildir ve özel anahtarı portalı veya hizmetinde kalıcı yapılmaz. Kasa kimlik bilgilerini yerel makineye indirmek için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. DPM makine kaydetmek istediğiniz kurtarma Hizmetleri kasası açın.
3. Ayarlar menüsünde varsayılan olarak açılır. Kapalıysa, tıklayın **ayarları** ayarlar menüsünü açmak için kasa Panosu üzerinde. Ayarlar menüsünde tıklatın **özellikleri**.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
4. Özellikler sayfasında, tıklatın **karşıdan** altında **yedekleme kimlik**. Portal indirme için kullanılabilir hale kasa kimlik bilgilerini oluşturur.

    ![İndirme](./media/backup-azure-dpm-introduction/vault-credentials.png)

Portal kasa adını ve geçerli tarih birleşimini kullanarak bir kasa kimlik bilgisi oluşturur. Tıklatın **kaydetmek** kasa kimlik bilgilerini Yerel hesabın indirmeler klasörüne indirin veya kasa kimlik bilgileri için bir konum belirtmek için Kaydet menüsünden Farklı Kaydet'i seçin. Bunu oluşturulacak dosyası için bir dakika sürer.

### <a name="note"></a>Not
* Kasa kimlik bilgileri dosyası makinenizden erişilebilen bir konuma kaydedildiğinden emin olun. Bir dosya paylaşımı/SMB depolanıyorsa erişim izinlerini denetleyin.
* Kasa kimlik bilgileri dosyası yalnızca kayıt iş akışı sırasında kullanılır.
* Kasa kimlik bilgileri dosyası 48hrs sonra süresi dolar ve portalından indirilebilir.

### <a name="3-install-backup-agent"></a>3. Yedekleme aracısını yükleme
Azure yedekleme kasası oluşturduktan sonra bir aracı her veri ve Azure uygulamalarının yedekleme sağlayan Windows makinelerinizin (Windows Server, Windows istemci, System Center Data Protection Manager sunucusu veya Azure yedekleme sunucusu makine) yüklü olmalıdır .

1. DPM makine kaydetmek istediğiniz kurtarma Hizmetleri kasası açın.
2. Ayarlar menüsünde varsayılan olarak açılır. Kapalıysa, tıklayın **ayarları** ayarlar menüsünü açın. Ayarlar menüsünde tıklatın **özellikleri**.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Ayarlar sayfasında, tıklatın **karşıdan** altında **Azure Yedekleme aracısı**.

    ![İndirme](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Aracı yüklendikten sonra Azure Backup agent kurulumunu başlatmak için MARSAgentInstaller.exe çalıştırın. Yükleme klasörü ve aracı için gereken geçici klasörü seçin. Belirtilen önbellek konumunu yedekleme verilerini % 5'en az boş alan olması gerekir.
4. İçinde internet'e bağlanmak için bir proxy sunucu kullanıyorsanız **Proxy Yapılandırması** ekranında, proxy sunucusu ayrıntılarını girin. Doğrulanmış bir proxy kullanıyorsanız, bu ekranda kullanıcı adı ve parola bilgilerinizi girin.
5. (Bu zaten mevcut değilse) Azure Backup aracısını yüklemeyi tamamlamak için Windows PowerShell ve .NET Framework 4.5 yükler.
6. Aracı yüklendikten sonra **Kapat** penceresi.

   ![Kapat](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)
7. İçin **DPM sunucusunu kaydettirin** kasasına içinde **Yönetim** sekmesi tıklatın üzerinde **çevrimiçi**. Ardından, seçin **kaydetmek**. Kurulum Kaydetme Sihirbazı'nı açar.
8. İçinde internet'e bağlanmak için bir proxy sunucu kullanıyorsanız **Proxy Yapılandırması** ekranında, proxy sunucusu ayrıntılarını girin. Doğrulanmış bir proxy kullanıyorsanız, bu ekranda kullanıcı adı ve parola bilgilerinizi girin.

    ![Proxy yapılandırması](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. Kasa kimlik bilgileri ekranında göz atın ve daha önce indirilen kasa kimlik bilgileri dosyası seçin.

    ![Kasa kimlik bilgileri](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    Kasa kimlik bilgileri dosyası yalnızca 48 için geçerlidir (portaldan yüklendikten sonra) saat. Bu ekran ("sağlanan dosyasının süresi doldu Örneğin, kasa kimlik bilgileri"), kasa kimlik bilgileri dosyasını yeniden indirin ve Azure portalında oturum açma herhangi bir hatayla karşılaşırsanız.

    Kasa kimlik bilgileri dosyası Kurulum uygulama tarafından erişilebilen bir konumda kullanılabilir olduğundan emin olun. Karşılaşırsanız ilgili hatalar erişim, kasa kimlik bilgileri dosyası bu makinede geçici bir konuma kopyalayın ve işlemi yeniden deneyin.

    Geçersiz kasa kimlik bilgileri hatayla karşılaşırsanız (örneğin, "geçersiz kasa sağlanan kimlik bilgileri") dosya bozuk veya mu sahip en yeni kimlik ilişkilendirilmemiş kurtarma hizmeti ile. Yeni bir kasa kimlik bilgilerini portaldan indirme işlemi yeniden deneyin. Bu hata genellikle kullanıcı üzerinde tıklarsa görülür **indirme kasa kimlik bilgileri** Azure portalında hızlı art arda seçeneği. Bu durumda, yalnızca ikinci kasa kimlik bilgilerini geçerli değil.
10. İçinde iş ve iş dışı saatler sırasında ağ bant genişliği kullanımını denetlemek için **azaltma ayarı** ekranı, bant genişliği kullanım sınırlarını ayarlama ve iş ve iş dışı saatler tanımlayın.

    ![Azaltma ayarı](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)
11. İçinde **kurtarma klasör ayarı** ekranında, dosyaların Azure kaynağından indirdiğiniz klasörü geçici olarak hazırlanmış için göz atın.

    ![Kurtarma klasör ayarı](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)
12. İçinde **şifreleme ayarı** ekran, bir parola oluşturabilir veya bir parola (en fazla 16 karakter) girin. Parolayı güvenli bir konumda kaydetmeyi unutmayın.

    ![Şifreleme](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Parola kaybolur veya unutulursa; Microsoft, yedekleme verilerini kurtarmak yardımcı olamaz. Son kullanıcı şifreleme parolası sahibi ve Microsoft son kullanıcı tarafından kullanılan parola görünürlüğe sahip değil. Lütfen bir kurtarma işlemi sırasında gerekli olduğu gibi dosyayı güvenli bir konuma kaydedin.
    >
    >
13. Tıkladığınızda **kaydetmek** düğmesi makine başarıyla kasaya kayıtlı ve Microsoft Azure yedekleme başlatmak artık hazırsınız.
14. Data Protection Manager kullanırken tıklayarak kayıt iş akışı sırasında belirtilen ayarları değiştirebileceğiniz **yapılandırma** seçerek seçeneği **çevrimiçi** altında **Yönetimi**  Sekmesi.

## <a name="requirements-and-limitations"></a>Gereksinimleri (ve sınırlamaları)
* Bir fiziksel sunucuda veya System Center 2012 SP1 veya System Center 2012 R2'de yüklü bir Hyper-V sanal makinesi olarak DPM çalıştırıyor olabilir. System Center 2012 R2 ile en az çalışan Azure sanal makinesi olarak da çalışabilir DPM 2012 R2 güncelleştirme paketi 3 veya System Center 2012 R2 ile en az çalışan vmware'deki Windows sanal makine Güncelleştirme Paketi 5.
* System Center 2012 SP1 ile DPM çalıştırıyorsanız, güncelleştirme dökümü 2'yi System Center Data Protection Manager SP1 için yüklemeniz gerekir. Azure yedekleme Aracısı'nı yükleyebilmek için önce bu gereklidir.
* DPM sunucusunda Windows PowerShell ve .net olması Framework 4.5 yüklü.
* DPM çoğu iş yüklerini Azure Yedekleme'de yedekleyebilir. Bkz: desteklenen sahip bir tam listesi için Azure Backup aşağıdaki öğeleri destekler.
* Azure Yedekleme'de depolanan verileri "banda Kopyala" seçeneğiyle kurtarılamıyor.
* Azure Backup özelliği etkinleştirilmiş bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Hakkında bilgi edinin [Azure yedekleme fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/).
* Azure Yedekleme'yi kullanarak Azure Backup aracısının yedeklemek istediğiniz sunucularda yüklü olması gerekir. Her sunucunun yerel boş depolama olarak kullanılabilir yedeklendiğinden verilerin boyutunu % 5'en az olmalıdır. Örneğin, 100 GB veri yedekleme en az 5 GB boş alan karalama konumda gerektirir.
* Veriler Azure kasası depolama alanında depolanır. Bir Azure yedekleme kadar kasa veri miktarı için bir sınır yoktur ancak (örneğin sanal makine ya da veritabanı) veri kaynağının boyutunu 54400 GB aşan döndürmemelidir.

Bu dosya türlerini Azure'a yedekleme için desteklenir:

* Şifrelenmiş (yalnızca tam yedeklemeler)
* Sıkıştırılmış (artımlı yedeklemeler desteklenir)
* Aralıklı (artımlı yedeklemeler desteklenir)
* Sıkıştırılmış ve aralıklı (aralıklı olarak kabul edilir)

Ve bu desteklenmez:

* Büyük küçük harfe duyarlı dosya sistemlerindeki sunucular desteklenmez.
* Sabit bağlantılar (atlanır)
* Yeniden ayrıştırma noktaları (atlanır)
* Şifrelenmiş ve sıkıştırılmış (atlanır)
* Şifrelenmiş ve aralıklı (atlanır)
* Sıkıştırılmış akış
* Aralıklı akış

> [!NOTE]
> Gelen System Center 2012 DPM SP1 ile başlayarak, Microsoft Azure Yedekleme kullanılarak Azure DPM tarafından korunan iş yüklerini yedekleme.
>
>
