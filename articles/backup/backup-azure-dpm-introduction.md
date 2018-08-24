---
title: İş yüklerini Azure'a yedeklemek için DPM'yi kullanma
description: DPM verileri bir Azure kurtarma Hizmetleri kasasına yedekleme giriş.
services: backup
author: adigan
manager: nkolli
keywords: System Center Data Protection Manager, data protection manager, dpm yedekleme
ms.service: backup
ms.topic: conceptual
ms.date: 8/22/2018
ms.author: adigan
ms.openlocfilehash: 873e7066bcf51b32c3a7a54e845ffd5a744f407f
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745444"
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>DPM ile Azure’a iş yüklerini yedeklemeye hazırlama
> [!div class="op_single_selector"]
> * [Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Bu makalede, System Center Data Protection Manager (DPM) verilerini Azure'a nasıl yedekleyeceğiniz açıklanmaktadır; şunları içerir:

* DPM sunucusu verileri Azure'a yedeklemek nasıl
* Kesintisiz bir yedekleme deneyimi elde etmek için Önkoşullar
* Tipik hatalarla karşılaşıldı ve bunlarla başa çıkma
* Desteklenen senaryolar

> [!NOTE]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Resource Manager modeli kullanılarak dağıtılan Vm'leri geri yüklemek için bilgi ve yordamları verilmektedir.
>
>

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) dosya ve uygulama verileri yedekler. Desteklenen iş yükleri hakkında daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix). DPM'e yedeklenen verileri diskte, bantta depolanan veya Microsoft Azure yedekleme ile azure'a yedeklenen. DPM, Azure Backup ile aşağıdaki gibi etkileşim kurar:

* **Fiziksel sunucu veya şirket içi sanal makine olarak dağıtılan DPM** — DPM'yi fiziksel sunucu dağıtılan veya bir şirket içi Hyper-V sanal makine disk ve bant ek olarak bir kurtarma Hizmetleri kasası için verileri yedekler yedekleme.
* **Azure sanal makinesi olarak dağıtılan DPM** — güncelleştirme 3 ile System Center 2012 R2'den DPM bir Azure sanal makinesi üzerinde dağıtabilirsiniz. DPM Azure sanal makinesi olarak dağıtılırsa, VM'ye bağlı Azure disklerine veri yedekleyebilir veya kurtarma Hizmetleri kasasına yedekleme tarafından veri depolama yük boşaltma.

## <a name="why-back-up-dpm-to-azure"></a>Neden DPM'den Azure'a yedekleme?
DPM sunucularını Azure'a yedekleme iş avantajları şunlardır:

* Banda uzun vadeli dağıtım alternatif olarak, Azure şirket içi DPM dağıtımı için kullanın.
* DPM azure'da sanal makinesi üzerinde dağıtmak için Azure diskine depolama yük boşaltma. Kurtarma Hizmetleri Kasası'nda eski verileri depolamak, işletmenizin diske yeni verileri depolayarak ölçekleme olanak tanır.

## <a name="prerequisites"></a>Önkoşullar
Azure Backup'ın DPM verilerini aşağıdaki gibi yedekleme hazırlayın:

1. **Bir kurtarma Hizmetleri kasası oluşturma** — Azure portalında bir kasa oluşturun.
2. **Kasa kimlik bilgilerini indirme** — DPM sunucusunu kurtarma Hizmetleri kasası ile kaydetmek için kullandığınız kimlik bilgilerini indirin.
3. **Azure Backup Aracısı yükleme** — aracı, her DPM sunucusuna yükleyin.
4. **Sunucuyu kaydetmek** — DPM sunucusunu kurtarma Hizmetleri kasasına kaydedin.

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="key-definitions"></a>Temel tanımları
DPM için azure'a yedekleme için bazı temel tanımları şunlardır:

1. **Kasa kimlik bilgileri** — kasa kimlik bilgileri, yedekleme verilerini Azure Backup hizmetindeki tanımlanmış bir kasaya göndermek üzere makinenin kimliğini doğrulamak için gereklidir. Kasadan indirilebilir ve 48 saat boyunca geçerlidir.
2. **Parola** — bulut yedeklemeleri şifrelemek için kullanılan parola. Bir kurtarma işlemi sırasında gerekli olduğu gibi dosyayı güvenli bir konuma kaydedin.
3. **Güvenlik PIN'i** — etkinleştirdiyseniz [güvenlik ayarları](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) kasa kritik yedekleme işlemleri gerçekleştirmek için güvenlik PIN'i gerekiyor. Bu çok faktörlü kimlik doğrulaması, bir güvenlik katmanı ekler. 
4. **Kurtarma klasörü** — bulut yedeklemeleri geçici olarak bulut Kurtarma sırasında yüklenen terimdir. Boyutu kabaca, paralel olarak kurtarmak istediğiniz yedekleme öğeleri boyutuna eşit olmalıdır.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

## <a name="edit-storage-replication"></a>Depolama çoğaltma Düzenle

Depolama çoğaltması, coğrafi olarak yedekli depolama ile yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Kasa, birincil yedeklemenizse seçeneği coğrafi olarak yedekli depolama şeklinde bırakın. Kalıcı olarak oldukça olmayan ucuz bir seçenek istiyorsanız yerel olarak yedekli depolamayı yapılandırmak için aşağıdaki yordamı kullanın. [Coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage çoğaltmaya genel bakış](../storage/common/storage-redundancy.md) bölümünde edinebilirsiniz.

Depolama çoğaltma ayarını düzenlemek için:

> [!NOTE] 
> Depolama çoğaltması, ilk yedekleme tetiklemeden önce yapılandırın. Depolama çoğaltma yapılandırması korumalı öğenin yedekleme sonra değiştirmeye karar verirseniz, depolama yapılandırması geçmeden önce kasa üzerinde korumayı durdurmanız gerekir.
>  

1. Kasanızı seçin ve kasa panosunu açın.

2. İçinde **Yönet** bölümünde **Yedekleme Altyapısı**.

3. Üzerinde **yedekleme yapılandırması** menüsünde, kasanız için depolama çoğaltma seçeneğini belirleyin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-dpm-introduction/choose-storage-configuration-rs-vault.png)

    Kasanız için depolama seçeneğini belirledikten sonra, VM'yi kasa ile ilişkilendirmek için hazır duruma gelirsiniz. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

## <a name="download-vault-credentials"></a>Kasa kimlik bilgilerini indirme
Kasa kimlik bilgileri dosyası, her bir yedekleme kasası için portal tarafından oluşturulan bir sertifikadır. Portal daha sonra ortak anahtarı Access Control Service'e (ACS) yükler. Makine kayıt iş akışı sırasında sertifikanın özel anahtarı makinenin kimliğini doğrular kullanıcıya kullanılabilir hale getirilir. Azure Backup hizmeti kimlik doğrulamasına göre tanımlanan kasaya verileri gönderir.

Kasa kimlik bilgileri yalnızca kayıt iş akışı sırasında kullanılır. Kasa kimlik bilgileri dosyasının gizliliğinin tehlikeye girmemesini sağlamak kullanıcının sorumluluğundadır. Kimlik bilgilerinin denetiminin kaybolursa, kasaya diğer makinelerin kaydetmek için kasa kimlik bilgileri kullanılabilir. Ancak, var olan yedekleme verilerinin gizliliği tehlikeye giremez şekilde müşteriye ait bir parolayı kullanarak yedekleme verileri şifrelenir. Bu endişenin ortadan kaldırılabilmesi için kasa kimlik bilgileri 48 sonra sona saat. Gerektiği birçok yeni kasa kimlik bilgilerini indirin. Ancak kayıt iş akışı sırasında yalnızca en son kasa kimlik bilgileri dosyası kullanılabilir.

Kasa kimlik bilgilerini, Azure portalından güvenli bir kanal aracılığıyla yüklenir. Azure Backup hizmeti sertifikanın özel anahtarı farkında değildir ve özel anahtar, portal veya hizmet kullanılamıyor. Kasa kimlik bilgilerini bir yerel makineye indirmek için aşağıdaki adımları kullanın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Bir DPM sunucusuna kaydetmek istediğiniz kurtarma Hizmetleri kasasını açın.

3. Varsayılan ayarlar menüsü açılır. Kapalıysa, tıklayın **ayarları** menüsünü açmak için kasa panosunda. İçinde **ayarları** menüsünde tıklatın **özellikleri**.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. Özellikler sayfasında altında **yedekleme kimlik bilgilerini** tıklayın **indirme**. Portalı yükleme için kullanılabilir hale getirileceğini kasa kimlik bilgileri dosyası oluşturur.

    ![İndirme](./media/backup-azure-dpm-introduction/vault-credentials.png)

Portal, kasa adını ve geçerli tarihi bir birleşimini kullanarak bir kasa kimlik bilgisi oluşturur. Tıklayın **Kaydet** kasa kimlik bilgilerini Yerel hesabın indirmeler klasörünüze indirilir veya kasa kimlik bilgileri için bir konum belirtmek için Kaydet menüsünden Farklı Kaydet'i seçin. Oluşturulacak dosya için bir dakika sürer.

### <a name="note"></a>Not
* Kasa kimlik bilgileri dosyası makinenizden erişilebilen bir konuma kaydedildiğinden emin olun. Bir dosya paylaşımı/SMB depolandığı için erişim izinlerini denetleyin.
* Kasa kimlik bilgileri dosyası, yalnızca kayıt iş akışı sırasında kullanılır.
* Kasa kimlik bilgileri dosyasının süresi dolduktan sonra 48hrs ve Portalı'ndan indirilebilir.

## <a name="install-backup-agent"></a>Yedekleme aracısını yükleme
Azure Backup kasasını oluşturduktan sonra bir aracı her bir veri ve uygulamaları azure'a yedekleyin sağlar, Windows makine (Windows Server, Windows istemci, System Center Data Protection Manager sunucusu veya Azure Backup sunucusu makine) yüklü olması gerekir .

1. DPM makinesinde kaydetmek istediğiniz kurtarma Hizmetleri kasasını açın.
2. Varsayılan ayarlar menüsü açılır. Kapalıysa, tıklayarak **ayarları** ayarlar menüsünü açın. Ayarlar menüsünde tıklayarak **özellikleri**.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Ayarlar sayfasında, tıklayın **indirme** altında **Azure Backup Aracısı**.

    ![İndirme](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Aracısı'nı indirdikten sonra Azure Backup Aracısı yüklemesini başlatmak için MARSAgentInstaller.exe çalıştırın. Yükleme klasörü ve aracı için gerekli karalama klasörünü seçin. Belirtilen önbellek konumunu, yedekleme verilerinin en az %5 boş alan olması gerekir.

4. İnternet'e bağlanmak üzere bir proxy sunucusu kullanıyorsanız **Proxy Yapılandırması** ekranında, proxy sunucusu ayrıntıları girin. Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola ayrıntıları bu ekranda girin.

5. (Bunu zaten mevcut değilse) Azure Backup aracısını yüklemeyi tamamlamak için .NET Framework 4.5 ve Windows PowerShell yükler.

6. Aracı yüklendikten sonra **Kapat** penceresi.

   ![Kapat](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)

7. İçin **DPM sunucusu kayıt** kasasına içinde **Yönetim** sekmesine tıklatın üzerinde **çevrimiçi**. Ardından, **kaydetme**. Bu kayıt Kurulum Sihirbazı açılır.

8. İnternet'e bağlanmak üzere bir proxy sunucusu kullanıyorsanız **Proxy Yapılandırması** ekranında, proxy sunucusu ayrıntıları girin. Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola ayrıntıları bu ekranda girin.

    ![Proxy yapılandırması](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. Kasa kimlik bilgileri ekranını göz atın ve daha önce indirilen kasa kimlik bilgileri dosyası seçin.

    ![Kasa kimlik bilgileri](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    Kasa kimlik bilgileri dosyası yalnızca 48 için geçerlidir (portaldan yüklendikten sonra) saat. Bu ekranı ("sağlanan dosyasının süresi dolmuş Örneğin, kasa kimlik bilgileri"), Azure portalı ve kasa kimlik bilgilerini yeniden dosya indirme için oturum açma herhangi bir hata ile karşılaşırsanız.

    Kasa kimlik bilgileri dosyası Kurulum uygulama tarafından erişilebilen bir konumda kullanılabilir olduğundan emin olun. Karşılaşırsanız ilgili hatalar erişimi, kasa kimlik bilgileri dosyası bu makinedeki geçici bir konuma kopyalayın ve işlemi yeniden deneyin.

    Geçersiz kasa kimlik bilgisi hatayla karşılaşırsanız (örneğin, "geçersiz kasa sağlanan kimlik bilgileri") dosyası bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili. Portaldan yeni bir kasa kimlik bilgileri dosyasını indirdikten sonra işlemi yeniden deneyin. Bu hata genellikle kullanıcı tıklatırsa görülen **indirme kasa kimlik bilgileri** seçeneği Azure portalında, sayfayı hızlı bir şekilde. Bu durumda, yalnızca ikinci kasa kimlik bilgileri dosyası geçerli değil.

10. İçinde iş ve iş dışı saatler sırasında ağ bant genişliği kullanımını denetlemek için **daraltma ayarları** ekran, bant genişliği kullanım sınırları ayarlayabilir ve iş saatleri ve çalışma dışı saatler tanımlayın.

    ![Daraltma ayarları](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)

11. İçinde **kurtarma klasörü ayarını** ekran, dosyalar, Azure'dan indirdiğiniz klasörü geçici olarak hazırlanan için göz atın.

    ![Kurtarma klasörü ayarını](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)

12. İçinde **şifreleme ayarı** ekranında, bir parola oluşturmak veya (en az 16 karakter) bir parola girin. Parolayı güvenli bir konumda kaydetmeyi unutmayın.

    ![Şifreleme](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Parola kaybolur veya unutulursa; Microsoft, yedekleme verileri kurtarmada yardımcı olamaz. Şifreleme parolası son kullanıcı sahibi ve Microsoft son kullanıcı tarafından kullanılan parolayı görünürlük yok. Lütfen bir kurtarma işlemi sırasında gerekli olduğu gibi bu dosyayı güvenli bir konuma kaydedin.
    >
    >

13. ' A tıkladığınızda **kaydetme** düğmesi makine başarıyla kasaya kaydedilen ve Microsoft Azure yedekleme başlatmak artık hazırsınız.

14. Data Protection Manager kullanırken tıklayarak kayıt iş akışı sırasında belirtilen ayarları değiştirebilirsiniz **yapılandırma** seçeneği seçerek **çevrimiçi** altında **Yönetimi**  Sekmesi.

## <a name="requirements-and-limitations"></a>Gereksinimleri (ve sınırlamaları)
* DPM fiziksel sunucu veya System Center 2012 SP1 veya System Center 2012 R2 üzerinde yüklü bir Hyper-V sanal makine olarak çalışabilir. Ayrıca en az System Center 2012 R2 üzerinde çalışan Azure sanal makinesi olarak çalışabilir DPM 2012 R2 güncelleştirme paketi 3 veya en az System Center 2012 R2 üzerinde çalışan vmware'deki Windows sanal makine Güncelleştirme Paketi 5 '.
* System Center 2012 SP1 ile DPM çalıştırıyorsanız, güncelleştirme paketi 2 ' için System Center Data Protection Manager SP1 yüklemeniz gerekir. Azure Backup aracısını yüklemeden önce bu gereklidir.
* DPM sunucusunda Windows PowerShell ve .net olması Framework 4.5 yüklü.
* DPM, çoğu iş yüklerini Azure Yedekleme'de yedekleyebilir. Azure Backup, bkz: desteklenen sahip tam listesi için aşağıdaki öğeleri destekler.
* "Banda Kopyala" seçeneği ile Azure Yedekleme'de depolanan veriler kurtarılamaz.
* Azure yedekleme özelliği etkin bir Azure hesabı gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Hakkında bilgi edinin [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).
* Azure Backup'ı kullanarak Azure Backup aracısının yedeklemek istediğiniz sunucularda yüklü olması gerekir. Her sunucuda en az % 5, yerel boş depolama alanı olarak kullanılabilir yedeklenmiyor veri boyutu olmalıdır. Örneğin, 100 GB veri yedekleme, en az 5 GB boş alan karalama konumu gerektirir.
* Veriler Azure kasası depolama alanında depolanır. Bir Azure Backup kadar kasa veri miktarı için bir sınır yoktur ancak (örneğin bir sanal makine veya veritabanı) bir veri kaynağının boyutunu 54400 GB uzun olmaması gerekir.

Bu dosya türleri Azure'a yedekleme için desteklenir:

* Şifrelenmiş (yalnızca tam yedeklemeler)
* Sıkıştırılmış (artımlı yedeklemeler desteklenir)
* Aralıklı (artımlı yedeklemeler desteklenir)
* Sıkıştırılmış ve aralıklı (aralıklı olarak kabul edilir)

Ve desteklenmeyen şunlardır:

* Büyük küçük harfe duyarlı dosya sistemlerindeki sunucular desteklenmez.
* Sabit bağlantılar (atlanır)
* Yeniden ayrıştırma noktaları (atlanır)
* Şifrelenmiş ve sıkıştırılmış (atlanır)
* Şifrelenmiş ve aralıklı (atlanır)
* Sıkıştırılmış akış
* Aralıklı akış

> [!NOTE]
> System Center 2012 DPM SP1 ve üzeri, korumalı iş yüklerini Azure'a yedekleyebilirsiniz.
>
>
