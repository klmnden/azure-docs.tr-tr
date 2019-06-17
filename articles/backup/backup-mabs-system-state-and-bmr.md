---
title: Azure Backup sunucusu sistem durumu korur ve çıplak için geri yükler
description: Azure Backup sunucusu, sistem durumunu yedekleme ve tam kurtarma (BMR) koruma sağlamak için kullanın.
services: backup
author: rayne-wiselman
manager: carmonm
keywords: ''
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: raynew
ms.openlocfilehash: 35ab150670cdc27efcedca233928e0c2184aeca6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62116184"
---
# <a name="back-up-system-state-and-restore-to-bare-metal-with-azure-backup-server"></a>Azure Backup sunucusu ile çıplak bilgisayardan geri yüklemek ve sistem durumunu yedekleme

Azure Backup sunucusu, sistem durumunu yedeklediğinde ve tam kurtarmayı (BMR) koruma sağlar.

*   **Sistem durumu yedeklemesi**: İşletim sistemi dosyalarını yedekler ve böylece bir bilgisayar başlatıldığında ancak sistem dosyalarını ve kayıt defteri kaybolur kurtarabilirsiniz. Bir sistem durumu yedeklemesi içerir:
    * Etki alanı üyesi: Önyükleme dosyaları, COM + sınıf kaydı veritabanı, kayıt defteri
    * Etki alanı denetleyicisi: Windows Server Active Directory (NTDS), önyükleme dosyaları, COM + sınıf kaydı veritabanı, kayıt defteri, sistem birimi (SYSVOL)
    * Küme hizmetlerini çalıştıran bilgisayar: Küme sunucusu meta verileri
    * Sertifika Hizmetleri çalıştıran bilgisayarda: Sertifika verileri
* **Tam yedekleme**: Kritik birimler (dışındaki kullanıcı verileri) üzerinde işletim sistemi dosyalarını ve tüm verileri yedekler. Tanımına göre bir BMR yedeklemesi, sistem durumu yedeklemesi içerir. Bir bilgisayar başlatılamıyor ve her şeyi koruma sağlar.

Aşağıdaki tabloda, yedekleme ve kurtarma özetlenmektedir. Sistem durumu ve BMR ile korunabilecek uygulama sürümleri hakkında ayrıntılı bilgi için bkz: [yaptığı Azure Backup sunucusu yedekleme?](backup-mabs-protection-matrix.md).

|Backup|Sorun|Azure Backup Sunucusu'na yedeklemeden Kurtar|Sistem durumu yedeklemesinden kurtarma|BMR|
|----------|---------|---------------------------|------------------------------------|-------|
|**Dosya verileri**<br /><br />Normal veri yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp dosya verileri|E|N|N|
|**Dosya verileri**<br /><br />Dosya verilerinin Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp veya hasarlı işletim sistemi|N|E|E|
|**Dosya verileri**<br /><br />Dosya verilerinin Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veri birimleri bozulmamış)|N|N|E|
|**Dosya verileri**<br /><br />Dosya verilerinin Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veri birimleri kayıp)|E|Hayır|Evet (BMR'den, yedeklenen dosya verilerinin normal kurtarılması sonra)|
|**SharePoint verilerini**:<br /><br />Grup verilerinin Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp site, listeler, liste öğelerini, belgeleri|E|N|N|
|**SharePoint verilerini**:<br /><br />Grup verilerinin Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp veya hasarlı işletim sistemi|N|E|E|
|**SharePoint verilerini**:<br /><br />Grup verilerinin Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Olağanüstü durum kurtarma|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Hyper-V konak veya konuk Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeğini konak|Kayıp VM|E|N|N|
|Hyper-V<br /><br />Hyper-V konak veya konuk Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeğini konak|Kayıp veya hasarlı işletim sistemi|N|E|E|
|Hyper-V<br /><br />Hyper-V konak veya konuk Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeğini konak|Kayıp Hyper-V Konağı (VM'ler bozulmamış)|N|N|E|
|Hyper-V<br /><br />Hyper-V konak veya konuk Azure Backup sunucusu yedekleme<br /><br />BMR/sistem durumu yedeğini konak|Kayıp Hyper-V Konağı (VM'ler kayıp)|N|N|E<br /><br />BMR, normal Azure Backup sunucusu ardından|
|SQL Server/Exchange<br /><br />Azure Backup sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp uygulama verileri|E|N|N|
|SQL Server/Exchange<br /><br />Azure Backup sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp veya hasarlı işletim sistemi|N|Y|E|
|SQL Server/Exchange<br /><br />Azure Backup sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veritabanı/işlem günlüğü dosyaları bozulmamış)|N|N|E|
|SQL Server/Exchange<br /><br />Azure Backup sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veritabanı/işlem günlüğü kayıp)|N|N|E<br /><br />BMR kurtarma ardından normal Azure Backup sunucusu kurtarma|

## <a name="how-system-state-backup-works"></a>Sistem durumu yedeklemesi nasıl çalışır?

Bir sistem durumu yedeklemesi çalıştığında, yedekleme sunucusu Windows Server sunucunun sistem durumunun bir yedeğini ister Backup ile iletişim kurar. Varsayılan olarak, en fazla kullanılabilir boş alana sahip bir sürücü Backup sunucusu ve Windows Server Yedekleme'yi kullanın. Bu sürücü hakkındaki bilgiler PSDataSourceConfig.xml dosyasına kaydedilir. Bu yedeklemeler için Windows Server Yedekleme kullanan sürücüdür.

Yedek sunucu sistem durumu yedeklemesi için kullandığı sürücüyü özelleştirebilirsiniz. Korumalı sunucuda C:\Program Files\Microsoft Data Protection Manager\MABS\Datasources gidin. Düzenlemek için PSDataSourceConfig.xml dosyasını açın. Değişiklik \<FilesToProtect\> değerini sürücü harfi için. Dosyayı kaydedin ve kapatın. Bilgisayarın sistem durumunu korumak için bir koruma grubu kümesine varsa bir tutarlılık denetimi çalıştırın. Bir uyarının oluşturulması durumunda seçin **koruma grubunu değiştir** uyarısında ve ardından Sihirbazı tamamlayın. Ardından, başka bir tutarlılık denetimi çalıştırın.

Koruma sunucusu bir kümenin içindeyse en fazla boş alana sahip olarak bir küme sürücüsü seçilmiş olması olası unutmayın. Sürücü sahipliğinin başka bir düğüme ve sistem durumu yedekleme çalıştırılana geçirildi, sürücü kullanılamaz ve yedekleme başarısız olur. Bu senaryoda, bir yerel sürücüye Psdatasourceconfig.XML'yi değiştirerek.

Ardından, Windows Server Yedekleme geri yükleme klasörünün kök dizininde WindowsImageBackup adında bir klasör oluşturur. Windows Server Yedekleme yedekleme oluşturduğu gibi tüm veriler bu klasöre yerleştirilir. Yedekleme tamamlandığında dosya Backup sunucusu bilgisayara aktarılır. Aşağıdaki bilgileri not edin:

* Yedekleme veya Aktarım tamamlandığında bu klasörü ve içeriği temizlenir değil. Bunu düşünmenin en iyi yolu, alan bir yedekleme tamamlandıktan sonraki seferde ayrılan ' dir.
* Bir yedekleme yapılan tüm değişiklikler klasör oluşturulur. Saat ve tarih damgası, son sistem durumu yedeklemenizin zamanını yansıtır.

## <a name="bmr-backup"></a>BMR yedekleme

(Bir sistem durumu yedeklemesi içerir), BMR için yedekleme işi, doğrudan bir paylaşıma yedekleme sunucusu bilgisayarında kaydedilir. Korunan sunucuda bir klasöre kaydedilmez.

Backup sunucusu, Windows Server Yedekleme çağırır ve bu BMR yedekleme için çoğaltma birimini paylaşır. Bu durumda, bu en fazla boş alana sahip sürücüyü kullanmak için Windows Server Yedekleme sunmayacaktır. Bunun yerine, iş için oluşturulan paylaşımı kullanır.

Yedekleme tamamlandığında dosya Backup sunucusu bilgisayara aktarılır. Günlükler, C:\Windows\Logs\WindowsServerBackup dizininde depolanır.

## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

-   BMR, Windows Server 2003 çalıştıran bilgisayarlar için veya bir istemci işletim sistemi çalıştıran bilgisayarlar için desteklenmiyor.

-   Farklı koruma gruplarındaki aynı bilgisayar için BMR'yi ve sistem durumunu koruyamazsınız.

-   Bir yedekleme sunucusu bilgisayar, BMR için kendisini koruyamaz.

-   BMR için banda (diske ve banda veya D2T) kısa vadeli koruma desteklenmez. Uzun vadeli depolama için banda (disk-disk-için-banda veya D2D2T) desteklenir.

-   BMR koruma için Windows Server Yedekleme korumalı bilgisayara yüklenmesi gerekir.

-   BMR koruma için farklı sistem durumu koruması için yedekleme sunucusu herhangi bir alan gereksinimi korumalı bilgisayarda yok. Windows Server Yedekleme, yedeklemeleri doğrudan yedekleme sunucu bilgisayarına aktarır. Yedek sunucu yedekleme aktarım işi görünmez **işleri** görünümü.

-   Backup sunucusu, BMR için 30 GB alan çoğaltma biriminde tutar. Bu değiştirebilirsiniz **Disk ayırmayı** sayfasında koruma grubunu Değiştirme Sihirbazı'nı veya Get-DatasourceDiskAllocation ve Set-DatasourceDiskAllocation PowerShell cmdlet'lerini kullanarak. Kurtarma noktası birimde BMR koruması, beş gün bekletme için yaklaşık 6 GB gerektirir.
    * Çoğaltma birimi boyutunun 15 GB'tan daha az olamayacağını unutmayın.
    * Backup sunucusu, BMR veri kaynağının boyutunu hesaplamaz. Tüm sunucular için 30 GB olduğunu varsayar. Ortamınızda beklenen BMR yedeklemelerinin boyutuna göre değeri değiştirin. Bir BMR yedeklemesinin boyutu kabaca tüm kritik birimlerde kullanılan alanın toplamı olarak hesaplanabilir. Kritik birimler = önyükleme birimi + sistem birimi + Active Directory gibi sistem durumu verilerini barındıran birim.

-   Sistem durumu korumasından BMR korumasına değiştirirseniz, BMR koruması üzerinde daha az alan gerektirir *kurtarma noktası birimi*. Ancak birimdeki ek alan geri alınmaz. Üzerinde birim boyutunu el ile küçültebilirsiniz **Disk ayırmayı Değiştir** sayfasında koruma grubunu Değiştirme Sihirbazı'nın veya Get-DatasourceDiskAllocation ve Set-DatasourceDiskAllocation PowerShell cmdlet'lerini kullanarak.

    Sistem durumu korumasından BMR korumasına değiştirirseniz, BMR koruması hakkında daha fazla alan gerektirir *çoğaltma birimi*. Birim otomatik olarak genişletilir. Varsayılan alan ayırmalarını değiştirmek istiyorsanız Modify-diskallocation öğesini PowerShell cmdlet'ini kullanın.

-   BMR korumasından sistem durumu korumasına değiştirirseniz, kurtarma noktası biriminde daha fazla alana ihtiyacınız olur. Backup sunucusu, birimi otomatik olarak artırmak deneyebilir. Depolama havuzunda yeterli alan yoksa, bir hata oluşur.

    BMR korumasından sistem durumu korumasına değiştirirseniz, korumalı bilgisayar üzerindeki alan gerekir. Sistem durumu koruması ilk kopyayı yerel bilgisayara yazar ve ardından yedekleme sunucu bilgisayarına aktarır olmasıdır.

## <a name="before-you-begin"></a>Başlamadan önce

1.  **Azure Backup sunucusu dağıtma**. Backup sunucusu doğru şekilde dağıtıldığını doğrulayın. Daha fazla bilgi için bkz.
    * [Azure Backup sunucusu için sistem gereksinimleri](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Backup sunucusu koruma Matrisi](backup-mabs-protection-matrix.md)

2.  **Depolamayı ayarlama**. Yedekleme verileri, diskte, bantta ve Azure ile bulutta depolayabilirsiniz. Daha fazla bilgi için [veri depolamayı hazırlama](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage).

3.  **Set up the protection agent**. Koruma aracısını yedeklemek istediğiniz bir bilgisayara yükleyin. Daha fazla bilgi için [DPM koruma aracısını dağıtma](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent).

## <a name="back-up-system-state-and-bare-metal"></a>Sistem durumu ve tam yedekleme
Bölümünde anlatıldığı gibi bir koruma grubu ayarlayın [koruma gruplarını dağıtma](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups). Not farklı gruplardaki aynı bilgisayar için BMR'yi ve sistem durumunu koruyamazsınız. Ayrıca, BMR'yi seçtiğinizde sistem durumunun otomatik olarak etkinleştirilir.


1.  Yedek sunucu yöneticisi Konsolu'ndaki yeni koruma grubu oluşturma Sihirbazı'nı açmak için seçin **koruma** > **eylemleri** > **koruma grubu oluşturma** .

2.  Üzerinde **koruma grubu türünü seçin** sayfasında **sunucuları**ve ardından **sonraki**.

3.  Üzerinde **grup üyelerini seçin** sayfasında, bilgisayarı genişletin ve ardından ya da **BMR** veya **sistem durumu**.

    Hem BMR ve sistem durumunu farklı gruplardaki aynı bilgisayar için koruyamayacağınızı unutmayın. Ayrıca, BMR'yi seçtiğinizde sistem durumunun otomatik olarak etkinleştirilir. Daha fazla bilgi için [koruma gruplarını dağıtma](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups).

4.  Üzerinde **veri koruma yöntemini seçin** sayfasında, kısa vadeli ve uzun süreli yedekleme işlemini nasıl istediğinizi seçin. Kısa vadeli yedekleme her zaman olduğu için ilk olarak diske, diskten Azure'a yedekleme seçeneği ile Azure Backup (kısa veya uzun süreli) kullanarak bulut. Buluta uzun süreli yedekleme için yedekleme sunucusuna bağlı bir tek başına bant cihaz veya bant kitaplığına uzun süreli yedekleme ayarlamak için alternatiftir.

5.  Üzerinde **kısa vadeli hedefleri seçin** sayfasında, nasıl diskte kısa süreli depolama için yedeklemek istediğinizi seçin:
    1. İçin **bekletme aralığı**, ne kadar süreyle verileri disk üzerinde tutmak istediğinizi seçin. 
    2. İçin **eşitleme sıklığı**, diske artımlı yedekleme işlemi yapılmasını istediğiniz sıklığı seçin. Bir yedekleme aralığı ayarlamak istemiyorsanız, denetleyebilirsiniz **bir kurtarma noktasından hemen önce** seçeneği. Backup sunucusu, yalnızca her kurtarma noktası zamanlanmadan önce hızlı, tam bir yedekleme çalışır.

6.  Banttaki verilerin uzun vadeli depolama için depolamak istiyorsanız **uzun vadeli hedefleri belirtin** sayfasında, ne kadar süreyle (1-99 yıl) bant verilerini saklamak istediğinizi seçin. 
    1. İçin **yedekleme sıklığı**, ne sıklıkta banda yedekleme Çalıştır'ı seçin. Sıklık, seçtiğiniz bekletme aralığına dayalıdır:
        * Bekletme aralığı 1-99 yıl arası olduğunda, yedeklemelerin günlük, haftalık, iki haftalık, aylık, üç aylık, altı aylık veya yıllık olarak gerçekleşmesini için seçebilirsiniz.
        * Bekletme aralığı 1-11 ay arası olduğunda, yedeklemelerin günlük, haftalık, iki haftada, oluşmasına veya aylık seçebilirsiniz.
        * Bekletme aralığı 1-4 hafta arası olduğunda, yedeklemelerin günlük veya haftalık olarak gerçekleşmesini seçebilirsiniz.

    2. Üzerinde **bant ve kitaplık ayrıntılarını seçin** sayfasında kullanılacak bant ve kitaplığı seçin ve olup veri sıkıştırılır ve şifrelenir.

7.  Üzerinde **Disk ayırmayı gözden** sayfasında, koruma grubu için ayrılmış depolama havuzu disk alanını inceleyin.

    1. **Toplam veri boyutu** yedeklemek istediğiniz veri boyutu.
    2. **Azure Backup sunucusu üzerinde sağlanacak alan disk** Backup sunucusu koruma grubu için önerdiği alandır. Backup sunucusu ayarları temel alarak ideal yedekleme birimini seçer. Ancak, yedekleme birimi seçeneklerini düzenleyebilirsiniz **Disk ayırma ayrıntıları**. 
    3. İş yükleri için açılan menüden tercih edilen depolamayı seçin. Düzenlemeleriniz değerlerini değiştirmek **toplam depolama alanı** ve **boş depolama alanı** içinde **kullanılabilir Disk depolama alanı** bölmesi. Yetersiz sağlanmış alan Backup sunucusu kesintisiz yedeklemeleri emin olmak için birime eklediğiniz önerdiği depolama miktarıdır.

8.  Üzerinde **çoğaltma oluşturma yöntemini seçin** sayfasında, ilk tam veri çoğaltmasını istediğiniz şekli seçin. Ağ üzerinden çoğaltılmasını seçerseniz yoğun olmayan bir saat seçmenizi öneririz. Büyük miktarlarda veri veya en iyi durumda olmayan ağ koşulları, çıkarılabilir medya kullanarak çevrimdışı veri çoğaltmayı göz önünde bulundurun.

9. Üzerinde **tutarlılık denetimi seçenekleri seçin** sayfasında, tutarlılık denetimlerinin otomatikleştirmek istediğiniz şekli seçin. Çoğaltma verilerini olduğunda onay tutarsız olarak veya bir zamanlamaya göre çalıştırmayı seçebilirsiniz. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, istediğiniz zaman bir el ile denetim gerçekleştirebilirsiniz. El ile denetim çalıştırmak için **koruma** alan yedekleme Sunucu Yöneticisi konsolunun koruma grubuna sağ tıklayın ve ardından **tutarlılık denetimi gerçekleştir**.

10. Azure Backup kullanarak buluta yedekleme seçtiyseniz **çevrimiçi koruma verilerini belirtin** sayfasında, iş yüklerini Azure'a yedeklemek istediğiniz seçtiğinizden emin olun.

11. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, seçme ne sıklıkta artımlı yedeklemeleri azure'a meydana gelir. Her gün, hafta, ay ve yıl çalıştırmak ve hangi çalışacakları saat ve tarihi seçin için yedeklemeler zamanlayabilirsiniz. Yedeklemeler günde ortaya çıkabilir. Bir yedekleme her çalıştığında, yedekleme sunucusuna diskte depolanan yedekleme verileri kopyasından Azure'da bir kurtarma noktası oluşturulur.

12. Üzerinde **çevrimiçi saklama ilkesini belirtin** sayfasında, günlük, haftalık, aylık ve yıllık yedeklerden oluşturulan kurtarma noktalarının Azure'da nasıl bekletileceğini seçin.

13. Üzerinde **çevrimiçi çoğaltma seçin** sayfasında, ilk tam veri çoğaltmanın nasıl gerçekleşir. Ağ üzerinden çoğaltma veya Çevrimdışı Yedekleme (çevrimdışı dengeli dağıtım). Çevrimdışı Yedekleme Azure içe aktarma özelliğini kullanır. Daha fazla bilgi için [Azure backup'ta Çevrimdışı Yedekleme iş akışı](backup-azure-backup-import-export.md).

14. Üzerinde **özeti** sayfasında, ayarlarınızı gözden geçirin. Seçtikten sonra **Grup Oluştur**, ilk veri çoğaltma işlemi gerçekleşir. Veri kopyalama tamamlandığında, üzerinde **durumu** sayfasıdır, koruma grubunun durumu **Tamam**. Yedekleme sonra her koruma grubu ayarlarını gerçekleşir.

## <a name="recover-system-state-or-bmr"></a>Sistem durumu veya BMR kurtarma
BMR veya sistem durumunu bir ağ konumuna kurtarabilir. BMR'yi yedeklediyseniz, sisteminizi başlatmak ve ağa bağlanmak için Windows Kurtarma Ortamı'nı (WinRE) kullanın. Ardından ağ konumundan kurtarmak için Windows Server Yedekleme'yi kullanın. Yalnızca sistem durumunu yedeklediyseniz ağ konumundan kurtarmak için Windows Server Yedekleme'yi kullanın.

### <a name="restore-bmr"></a>BMR'yi geri yükleme
Kurtarma, yedekleme sunucusu bilgisayarında çalıştırabilirsiniz:

1.  İçinde **kurtarma** bölmesinde, kurtarmak istediğiniz bulma bilgisayar ve ardından **tam kurtarma**.

2.  Kullanılabilir kurtarma noktaları gösterilir Takvim üzerinde kalın. Tarih ve saat için kullanmak istediğiniz kurtarma noktasını seçin.

3.  Üzerinde **kurtarma türünü seçin** sayfasında **bir ağ klasörüne kopyala.**

4.  Üzerinde **hedefi belirtin** verileri kopyalamak istediğiniz sayfasında, seçin. Seçilen hedefin, yeterli alana sahip olması gerektiğini unutmayın. Yeni bir klasör oluşturmanızı öneririz.

5.  Üzerinde **kurtarma seçeneklerini belirtin** sayfasında, uygulanacak güvenlik ayarlarını seçin. Ardından, depolama alanı ağı (SAN) kullanmak isteyip istemediğinizi seçin-daha hızlı kurtarma için donanım anlık görüntülerini alarak. (Yalnızca bir SAN ile sağlanan bu işlevsellik ve özelliği oluşturma ve kopyayı bölme yazılabilir yapmak için varsa, bu bir seçenektir. Ayrıca, korumalı bilgisayar ve Backup sunucusu bilgisayar aynı ağa bağlanması gerekir.)

6.  Bildirim seçeneklerini ayarlayın. Üzerinde **onay** sayfasında **kurtarmak**.

Paylaşım konumunu ayarlayın:

1.  Geri yükleme konumunda yedeklemeyi içeren klasöre gidin.

2.  Böylece paylaşılan klasörün kökünün WindowsImageBackup klasörü, bir düzey WindowsImageBackup üzerindeki klasörü paylaşın. Bunu yapmazsanız, geri yükleme yedeği bulamaz. Windows Kurtarma Ortamı'nı (WinRE) kullanarak bağlanmak için doğru IP adresini ve kimlik bilgileri ile WinRE erişebileceğiniz bir paylaşım gerekir.

Sistem Geri yükleme:

1.  Geri yüklediğiniz sisteme için Windows DVD'SİNİ'ı kullanarak resmi geri yüklemek istediğiniz hedef bilgisayarı başlatın.

2.  İlk sayfasında, dil ve yerel ayarları doğrulayın. Üzerinde **yükleme** sayfasında **Bilgisayarınızı onarın**.

3.  Üzerinde **Sistem Kurtarma Seçenekleri** sayfasında **önceden oluşturduğunuz sistem görüntüsünü kullanarak bilgisayarınızı geri yükleyin**.

4.  Üzerinde **bir sistem resmi yedeği seçin** sayfasında **Sistem Görüntüsü Seç** > **Gelişmiş** > **sistem görüntüsü Ara ağ üzerinde**. Bir uyarı görünürse seçin **Evet**. Gidin paylaşım yolu için kimlik bilgilerini girin ve ardından Kurtarma noktasını seçin. Bu, bu kurtarma noktasında kullanılabilir belirli yedeklemelerinize yönelik tarar. Kullanmak istediğiniz kurtarma noktasını seçin.

5.  Üzerinde **yedeklemeyi geri yükleme şeklini seçin** sayfasında **diskleri Biçimlendir ve yeniden bölümle**. Sonraki sayfada, ayarları doğrulayın. 

6.  Geri yüklemeyi başlatmak için seçin **son**. Yeniden başlatma gerekiyor.

### <a name="restore-system-state"></a>Sistem durumu geri yükleme

Kurtarma, yedekleme sunucusu çalıştırın:

1.  İçinde **kurtarma** bölmesinde, kurtarmak istediğiniz bilgisayar bulma ve ardından **tam kurtarma**.

2.  Kullanılabilir kurtarma noktaları gösterilir Takvim üzerinde kalın. Tarih ve saat için kullanmak istediğiniz kurtarma noktasını seçin.

3.  Üzerinde **kurtarma türünü seçin** sayfasında **bir ağ klasörüne Kopyala**.

4.  Üzerinde **hedefi belirtin** , verileri kopyalamak istediğiniz sayfasında, seçin. Seçilen hedefte yeterli alan olması gerektiğini unutmayın. Yeni bir klasör oluşturmanızı öneririz.

5.  Üzerinde **kurtarma seçeneklerini belirtin** sayfasında, uygulanacak güvenlik ayarlarını seçin. Ardından, daha hızlı kurtarma için SAN tabanlı donanım anlık görüntüleri kullanmak isteyip istemediğinizi seçin. (Yalnızca oluşturma ve kopyayı yazılabilir yapmak için bölme bu işlevselliği ile özelliği bir SAN varsa bir seçenek budur. Ayrıca, yedekleme sunucusu ve korumalı bilgisayar aynı ağa bağlanması gerekir.)

6.  Bildirim seçeneklerini ayarlayın. Üzerinde **onay** sayfasında **kurtarmak**.

Windows Server Yedekleme çalıştırın:

1.  Seçin **eylemleri** > **kurtarmak** > **bu sunucu** > **sonraki**.

2.  Seçin **başka bir sunucuya**seçin **konum türünü belirtin** sayfasında ve ardından **paylaşılan uzak klasör**. Kurtarma noktasını içeren klasörün yolunu girin.

3.  Üzerinde **kurtarma türünü seçin** sayfasında **sistem durumu**. 

4. Üzerinde **sistem durumu kurtarma için konum seçin** sayfasında **özgün konumuna**.

5.  Üzerinde **onay** sayfasında **kurtarmak**. Geri yüklemeden sonra sunucuyu yeniden başlatın.

6.  Bir komut isteminde sistem durumu geri yüklemesi de çalıştırabilirsiniz. Bunu yapmak için kurtarmak istediğiniz bilgisayarda Windows Server Yedekleme başlatın. Bir komut isteminde sürüm tanımlayıcısını almak için şunu girin: ```wbadmin get versions -backuptarget \<servername\sharename\>```

    Sistem durumu geri yüklemesi başlatmak için sürüm tanıtıcısını kullanın. Komut isteminde girin: ```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

    Kurtarma işlemini başlatmak istediğinizi onaylayın. Komut İstemi penceresinde işlemi görebilirsiniz. Geri yükleme günlüğü oluşturulur. Geri yüklemeden sonra sunucuyu yeniden başlatın.

