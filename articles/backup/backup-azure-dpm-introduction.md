---
title: DPM sunucusu iş yüklerini Azure'a Yedeklemeye hazırlanma
description: DPM verileri bir Azure kurtarma Hizmetleri kasasına yedekleme giriş.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: kasinh
ms.openlocfilehash: 82e4278a130bb67a1af61ead981259d7bb4e1aa7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427428"
---
# <a name="prepare-to-back-up-workloads-to-azure-with-system-center-dpm"></a>System Center DPM ile azure'a iş yüklerini yedeklemek hazırlama

Bu makalede, System Center Data Protection Manager (DPM) yedekleri Azure Yedekleme hizmetini kullanarak azure'a hazırlamak açıklanmaktadır.

Makaleyi sağlar:

- Azure yedekleme ile DPM dağıtma genel bakış.
- Önkoşullar ve sınırlamalar Azure yedekleme ile DPM'yi kullanma.
- Azure, bir kurtarma Hizmetleri yedekleme kasası ayarlama ve isteğe bağlı olarak Azure depolama kasası türünü değiştirme dahil olmak üzere hazırlama adımları.
- Yükleme dahil olmak üzere DPM sunucusu, hazırlama adımları, Azure Backup aracısını yükleyerek ve DPM sunucusunu kasaya kaydetmeyi kimlik bilgileri kasası.
- Sık karşılaşılan hatalar için sorun giderme ipuçları.


## <a name="why-back-up-dpm-to-azure"></a>Neden DPM'den Azure'a yedekleme?

[System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) dosya ve uygulama verileri yedekler. DPM, Azure Backup ile aşağıdaki gibi etkileşim kurar:

* **Bir fiziksel sunucu veya şirket içi VM üzerinde çalışan DPM** — veriler, azure'daki bir yedekleme kasasına ayrıca diske ve banta yedekleyebilirsiniz yedekleme.
* **Bir Azure sanal makinesinde çalışan DPM** — System Center 2012 R2 Update 3 veya daha sonra DPM Azure sanal makinesinde dağıtabilirsiniz. VM'ye bağlı Azure disklerine veri yedekleyebilir veya verileri bir Backup kasasına yedeklemek için Azure Backup'ı kullanın.

DPM sunucularını Azure'a yedekleme iş avantajları şunlardır:

* Şirket içi DPM için Azure Backup banda uzun vadeli dağıtım bir alternatif sunar.
* Bir Azure sanal makinesinde çalışan DPM için Azure Backup, depolama alanı Azure diskine boşaltabilirsiniz olanak tanır. Bir yedekleme Kasası'nda eski verileri depolamak, işletmenizin diske yeni verileri depolayarak ölçekleme olanak tanır.

## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

**Ayar** | **Gereksinim**
--- | ---
Bir Azure VM'de DPM | System Center 2012 R2 DPM 2012 R2 güncelleştirme paketi 3 veya sonraki bir sürümü.
DPM fiziksel sunucu üzerinde | System Center 2012 SP1 veya üzeri; System Center 2012 R2.
DPM bir Hyper-V sanal makinesi üzerinde | System Center 2012 SP1 veya üzeri; System Center 2012 R2.
Bir VMware VM'de DPM | System Center 2012 R2 Güncelleştirme Paketi 5 veya üzeri.
Bileşenler | DPM sunucusu, Windows PowerShell ve .NET Framework 4.5 yüklü olmalıdır.
Desteklenen uygulamalar | [Bilgi](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix) ne DPM yedekleme yapabilirsiniz.
Desteklenen dosya türleri | Bu dosya türlerini, Azure Backup ile yedeklenebilir: (Yalnızca tam yedeklemeler) şifrelenir (Artımlı yedeklemeler desteklenir) sıkıştırılır; Aralıklı (artımlı yedeklemeler desteklenir); Sıkıştırılmış ve aralıklı (seyrek olarak kabul edilir).
Desteklenmeyen dosya türleri | Büyük küçük harfe duyarlı dosya sistemlerindeki sunucular; sabit bağlantılar (atlanır); yeniden ayrıştırma noktaları (atlanır); şifrelenmiş ve sıkıştırılmış (atlanır); şifrelenmiş ve aralıklı (atlanır); Sıkıştırılmış akış; akışı ayrıştıramadı.
Yerel depolama | Yedeklemek istediğiniz her makine yedeklenmekte verilerin boyutunu % 5'en az yerel boş depolama alanı olması gerekir. Örneğin, 100 GB veri yedekleme, en az 5 GB boş alan karalama konumu gerektirir.
Depolama kasası | Bir Azure Backup vault'a Yedekleyebileceğiniz veri miktarı için bir sınır yoktur ancak (örneğin bir sanal makine veya veritabanı) bir veri kaynağının boyutunu 54400 GB uzun olmaması gerekir.
Azure ExpressRoute | Azure ExpressRoute özel veya Microsoft eşlemesi ile yapılandırılmışsa, verileri Azure'a yedeklemek için kullanılamaz.<br/><br/> Azure ExpressRoute genel eşlemesi ile yapılandırılmışsa, verileri Azure'a yedeklemek için kullanılabilir.<br/><br/> **Not:** Genel eşdüzey hizmet sağlama, yeni bağlantı hatları için kullanım dışı bırakılmıştır.
Azure Backup aracısı | DPM, System Center 2012 SP1'de çalışıyorsa, paketi 2 veya üzeri için DPM SP1 yükleyin. Bu, aracı yüklemesi için gereklidir.<br/><br/> Bu makalede, Azure Backup Aracısı, Microsoft Azure kurtarma hizmeti (MARS) aracısı olarak da bilinen en son sürümünü dağıtmayı açıklar. Dağıtılan bir önceki sürümüne sahipseniz, bu yedekleme beklendiği gibi çalıştığından emin olmak için en son sürüme güncelleştirin.

Başlamadan önce Azure yedekleme özelliği etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Hakkında bilgi edinin [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-settings"></a>Depolama ayarlarını değiştirme

Coğrafi olarak yedekli ve yerel olarak yedekli depolama arasında seçim yapabilirsiniz.

- Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir.
- Kasa, birincil yedeklemenizse seçeneği coğrafi olarak yedekli depolama şeklinde bırakın. Kalıcı olarak oldukça olmayan ucuz bir seçenek istiyorsanız yerel olarak yedekli depolamayı yapılandırmak için aşağıdaki yordamı kullanın.
- Hakkında bilgi edinin [Azure depolama](../storage/common/storage-redundancy.md)ve [coğrafi olarak yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak yedekli](../storage/common/storage-redundancy-lrs.md) Depolama Seçenekleri.
- İlk yedeklemeyi önce depolama ayarlarını değiştirin. Bir öğeyi zaten yedeklediyseniz, depolama ayarları değiştirmeden önce kasaya yedekleme durdurun.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunda açın.

2. İçinde **Yönet**, tıklayın **Yedekleme Altyapısı**.

3. İçinde **yedekleme yapılandırması** bir depolama seçeneği için kasa menüsünden seçin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-dpm-introduction/choose-storage-configuration-rs-vault.png)

## <a name="download-vault-credentials"></a>Kasa kimlik bilgilerini indirme

DPM sunucusunu kasaya kaydettiğinizde kasa kimlik bilgilerini kullanın.

- Kasa kimlik bilgileri dosyası, her bir yedekleme kasası için portal tarafından oluşturulan bir sertifikadır.
- Portal daha sonra ortak anahtarı Access Control Service'e (ACS) yükler.
- Makine kayıt iş akışı sırasında sertifikanın özel anahtarı makinenin kimliğini doğrular kullanıcıya kullanılabilir hale getirilir.
- Azure Backup hizmeti kimlik doğrulamasına göre tanımlanan kasaya verileri gönderir.

### <a name="best-practices-for-vault-credentials"></a>Kasa kimlik bilgileri için en iyi uygulamalar

Kimlik bilgilerini almak için kasa kimlik bilgilerini güvenli bir kanal aracılığıyla Azure portalından indirin:

- Kasa kimlik bilgileri yalnızca kayıt iş akışı sırasında kullanılır.
- Bu kasa kimlik bilgileri dosyası, güvenli ve güvenliği aşılan değil olduğundan emin olmak için sizin sorumluluğunuzdur.
    - Kimlik bilgilerinin denetiminin kaybolursa, kasaya diğer makinelerin kaydetmek için kasa kimlik bilgileri kullanılabilir.
    - Ancak, var olan yedekleme verilerinin gizliliği tehlikeye giremez şekilde müşteriye ait bir parolayı kullanarak yedekleme verileri şifrelenir.
- Bu dosya, DPM sunucusundan erişilebilen bir konuma kaydedildiğinden emin olun. Bir dosya paylaşımı/SMB depolandığı için erişim izinlerini denetleyin.
- Kasa kimlik bilgilerinin süresi 48 sonra saat. Yeni kasa kimlik bilgileri gerektiği birçok indirebilirsiniz. Ancak kayıt iş akışı sırasında yalnızca en son kasa kimlik bilgileri dosyası kullanılabilir.
- Azure Backup hizmeti sertifikanın özel anahtarı farkında değildir ve özel anahtar, portal veya hizmet kullanılamıyor.

Kasa kimlik bilgileri dosyası gibi bir yerel makineye indirme:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. DPM sunucusunu kaydetmek istediğiniz kasaya açın.
3. İçinde **ayarları**, tıklayın **özellikleri**.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. İçinde **özellikleri** > **yedekleme kimlik bilgilerini**, tıklayın **indirme**. Portal, geçerli tarih ve kasa adını bir birleşimini kullanarak kasa kimlik bilgilerini oluşturur ve indirme için kullanılabilir hale getirir.

    ![İndirme](./media/backup-azure-dpm-introduction/vault-credentials.png)

5. Tıklayın **Kaydet** klasörüne kasa kimlik bilgilerini indirmek için veya **Kaydet** ve konumu belirtin. Oluşturulacak dosya için bir dakika sürer.


## <a name="install-the-backup-agent"></a>Yedekleme aracısını yükleme

Azure Backup tarafından yedeklenen her makineye yedekleme aracı (Microsoft Azure kurtarma hizmeti (MARS) aracısı olarak da bilinir) üzerinde yüklü olmalıdır. Aracıyı DPM sunucusuna aşağıdaki gibi yükleyin:

1. DPM sunucusunu kaydetmek istediğiniz kasaya açın.
2. İçinde **ayarları**, tıklayın **özellikleri**.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Üzerinde **özellikleri** sayfa, Azure Backup Aracısı'nı indirin.

    ![İndirme](./media/backup-azure-dpm-introduction/azure-backup-agent.png)


4. İndirdikten sonra MARSAgentInstaller.exe çalıştırın. DPM makinesinde aracıyı yüklemek için.
5. Aracı için bir yükleme klasörü ve önbellek klasörü seçin. Yedekleme verilerinin en az % 5'si önbellek konumu boş alan olması gerekir.
6. İnternet'e bağlanmak üzere bir proxy sunucusu kullanıyorsanız **Proxy Yapılandırması** ekranında, proxy sunucusu ayrıntıları girin. Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola ayrıntıları bu ekranda girin.
7. (Bunlar yüklü değilse) .NET Framework 4.5 ve Windows PowerShell Azure Backup aracısını yükler ve yüklemeyi tamamlamak için.
8. Aracı yüklendikten sonra **Kapat** penceresi.

    ![Kapat](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)

## <a name="register-the-dpm-server-in-the-vault"></a>DPM sunucusunu kasaya kaydedin.

1. DPM yönetici Konsolu'ndaki > **Yönetim**, tıklayın **çevrimiçi**. **Kaydol**’u seçin. Bu sunucu Kaydetme Sihirbazı'nı açılır.
2. İçinde **Proxy Yapılandırması**, proxy ayarlarını gereken şekilde belirtin.

    ![Proxy yapılandırması](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. İçinde **Backup kasası**göz atın ve indirdiğiniz kasa kimlik bilgileri dosyasını seçin.

    ![Kasa kimlik bilgileri](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

10. İçinde **daraltma ayarları**, isteğe bağlı olarak bant genişliği azaltma yedeklemeler için etkinleştirebilirsiniz. Hız sınırlarını ayarlayabilirsiniz için çalışma saatleri ve günleri belirtin.

    ![Daraltma ayarları](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)

11. İçinde **kurtarma klasörü ayarını**, veri kurtarma sırasında kullanılan bir konum belirtin.

    - Azure yedekleme, kurtarılan veriler için bir geçici bir bekleme sahası bu konumu kullanır.
    - Sonlandırma veri kurtarma işleminden sonra Azure Backup, bu alandaki verileri temizler.
    - Konum kurtarma paralel olarak tahmin öğeleri tutmak için yeterli alan olmalıdır.

    ![Kurtarma klasörü ayarını](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)

12. İçinde **şifreleme ayarı** oluşturmak veya bir parola girin.

    - Parola, buluta yedeklemeleri şifrelemek için kullanılır.
    - En az 16 karakter belirtin.
    - Güvenli bir konumda dosyayı kaydederseniz, kurtarma için gereklidir.

    ![Şifreleme](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Şifreleme parolası sahibi ve Microsoft bunu görünürlüğe sahip olmayacağı.
    > Parola kaybolur veya unutulursa; Microsoft, yedekleme verileri kurtarmada yardımcı olamaz.

13. Tıklayın **kaydetme** DPM sunucusunu kasaya kaydetmek için.

Sunucu başarıyla kaydedildikten sonra kasa ve Microsoft Azure yedekleme başlatmak hazırsınız.

## <a name="troubleshoot-vault-credentials"></a>Kasa kimlik bilgileri sorunlarını giderme

### <a name="expiration-error"></a>Zaman aşımı hatası

Kasa kimlik bilgileri dosyası yalnızca 48 için geçerlidir (portaldan yüklendikten sonra) saat. Bu ekranı ("sağlanan dosyasının süresi dolmuş Örneğin, kasa kimlik bilgileri"), Azure portalı ve kasa kimlik bilgilerini yeniden dosya indirme için oturum açma herhangi bir hata ile karşılaşırsanız.

### <a name="access-error"></a>Erişim hatası

Kasa kimlik bilgileri dosyası Kurulum uygulama tarafından erişilebilen bir konumda kullanılabilir olduğundan emin olun. Karşılaşırsanız ilgili hatalar erişimi, kasa kimlik bilgileri dosyası bu makinedeki geçici bir konuma kopyalayın ve işlemi yeniden deneyin.

### <a name="invalid-credentials-error"></a>Geçersiz kimlik bilgileri hatası

Geçersiz kasa kimlik bilgisi hatayla karşılaşırsanız (örneğin, "geçersiz kasa sağlanan kimlik bilgileri") dosyası bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili.

- Portaldan yeni bir kasa kimlik bilgileri dosyasını indirdikten sonra işlemi yeniden deneyin.
- Bu hata genellikle üzerinde tıkladığınızda görülür **indirme kasa kimlik bilgileri** seçeneği Azure portalında, iki kez sayfayı hızlı bir şekilde. Bu durumda, yalnızca ikinci kasa kimlik bilgileri dosyası geçerli değil.
