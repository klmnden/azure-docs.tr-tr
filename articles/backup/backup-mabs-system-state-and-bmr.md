---
title: Azure yedekleme sunucusu sistem durumu korur ve çıplak metal geri yükler
description: Sistem durumunu yedeklemek ve tam kurtarma (BMR) koruma sağlamak için Azure yedekleme sunucusu kullanın.
services: backup
author: markgalioto
manager: carmonm
keywords: ''
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: markgal
ms.openlocfilehash: d35f8667cb1ca9a0b3abd08450ebc647d6d12276
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607217"
---
# <a name="back-up-system-state-and-restore-to-bare-metal-with-azure-backup-server"></a>Sistem durumu yedeklemesi ve tam Azure yedekleme sunucusu ile geri yükleme

Azure yedekleme sunucusu sistem durumu yedeklemesi yedekler ve tam kurtarma (BMR) koruma sağlar.

*   **Sistem durumu yedeklemesi**: bilgisayar başlatıldığında, ancak sistem dosyalarını ve kayıt defteri kayboluyor kurtarmak için işletim sistemi dosyalarını yedekler. Bir sistem durumu yedeği içerir:
    * Etki alanı üyesi: önyükleme dosyaları, COM + sınıf kaydı veritabanı, kayıt defteri
    * Etki alanı denetleyicisi: Windows Server Active Directory (NTDS), önyükleme dosyaları, COM + sınıf kaydı veritabanı, kayıt defteri, sistem birimi (SYSVOL)
    * Küme hizmetlerini çalıştıran bilgisayar: küme sunucusu meta verilerini
    * Sertifika Hizmetleri çalıştıran bilgisayar: sertifika verileri
* **Tam yedekleme**: (kullanıcı veriler hariç) kritik birimlerdeki işletim sistemi dosyalarını ve tüm verileri yedekler. Tanımına göre bir BMR yedeklemesi bir sistem durumu yedeği içerir. Bir bilgisayar başlatılamıyor ve her şeyi kurtarmak koruma sağlar.

Aşağıdaki tabloda, yedekleme ve kurtarma özetler. BMR ve sistem durumu ile korunan uygulama sürümleri hakkında ayrıntılı bilgi için bkz: [Azure yedekleme sunucusu yaptığı yedekleme?](backup-mabs-protection-matrix.md).

|Backup|Sorun|Azure yedekleme sunucusu yedeklemeden Kurtar|Sistem durumu yedeklemesinden kurtarma|BMR|
|----------|---------|---------------------------|------------------------------------|-------|
|**Dosya verileri**<br /><br />Normal veri yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp dosya verileri|E|N|N|
|**Dosya verileri**<br /><br />Dosya verileri Azure yedekleme sunucusu yedeği<br /><br />BMR/sistem durumu yedeklemesi|Kayıp veya bozuk işletim sistemi|N|E|E|
|**Dosya verileri**<br /><br />Dosya verileri Azure yedekleme sunucusu yedeği<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veri birimleri bozulmamış)|N|N|E|
|**Dosya verileri**<br /><br />Dosya verileri Azure yedekleme sunucusu yedeği<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veri birimleri kayıp)|E|Hayır|Evet (yedeklenen dosya verilerinin normal kurtarması BMR'den)|
|**SharePoint verilerini**:<br /><br />Grup verilerini Azure yedekleme sunucusu yedeği<br /><br />BMR/sistem durumu yedeklemesi|Kayıp site, listeler, liste öğelerini, belgeleri|E|N|N|
|**SharePoint verilerini**:<br /><br />Grup verilerini Azure yedekleme sunucusu yedeği<br /><br />BMR/sistem durumu yedeklemesi|Kayıp veya bozuk işletim sistemi|N|E|E|
|**SharePoint verilerini**:<br /><br />Grup verilerini Azure yedekleme sunucusu yedeği<br /><br />BMR/sistem durumu yedeklemesi|Olağanüstü durum kurtarma|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuk Azure yedekleme sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi konağının|Kayıp VM|E|N|N|
|Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuk Azure yedekleme sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi konağının|Kayıp veya bozuk işletim sistemi|N|E|E|
|Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuk Azure yedekleme sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi konağının|Kayıp Hyper-V Konağı (VM'ler bozulmamış)|N|N|E|
|Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuk Azure yedekleme sunucusu yedekleme<br /><br />BMR/sistem durumu yedeklemesi konağının|Kayıp Hyper-V Konağı (VM'ler kayıp)|N|N|E<br /><br />Normal Azure yedekleme sunucusu kurtarma BMR'den|
|SQL Server/Exchange<br /><br />Azure yedekleme sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp uygulama verileri|E|N|N|
|SQL Server/Exchange<br /><br />Azure yedekleme sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp veya bozuk işletim sistemi|N|Y|E|
|SQL Server/Exchange<br /><br />Azure yedekleme sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veritabanı/işlem günlüklerini bozulmamış)|N|N|E|
|SQL Server/Exchange<br /><br />Azure yedekleme sunucusu uygulama yedekleme<br /><br />BMR/sistem durumu yedeklemesi|Kayıp sunucu (veritabanı/işlem günlüklerini kayıp)|N|N|E<br /><br />Normal Azure yedekleme sunucusu kurtarma tarafından izlenen BMR kurtarma|

## <a name="how-system-state-backup-works"></a>Sistem Durumu yedeğini nasıl çalışır

Bir sistem durumu yedeklemesi çalıştığında, yedekleme sunucusu Windows Server sunucunun sistem durumunun bir yedeğini istemek için yedekleme ile iletişim kurar. Varsayılan olarak, en fazla kullanılabilir boş alana sahip sürücü yedekleme sunucusu ve Windows Server Yedekleme'yi kullanın. Bu sürücü hakkında bilgi için PSDataSourceConfig.xml dosyasını kaydedilir. Bu yedeklemeler için Windows Server Yedekleme kullanan sürücüdür.

Yedek sunucu sistem durumu yedeklemesi için kullandığı sürücüyü özelleştirebilirsiniz. Korumalı sunucuda C:\Program Files\Microsoft Data Protection Manager\MABS\Datasources gidin. Düzenlemek için PSDataSourceConfig.xml dosyasını açın. Değişiklik \<Korunacakdosyalar\> değerini sürücü harfi için. Dosyayı kaydedin ve kapatın. Bilgisayarın sistem durumunu korumak için bir koruma grubu kümesine ise, bir tutarlılık denetimi çalıştırın. Bir uyarı üretilir, seçin **koruma grubunu değiştir** uyarısında ve ardından Sihirbazı tamamlayın. Ardından, başka bir tutarlılık denetimi çalıştırın.

Koruma sunucusu bir kümede ise, en çok boş alana sahip olan sürücü olarak bir küme sürücüsünün seçilmiş olması mümkündür olduğunu unutmayın. Sürücü sahipliğinin başka bir düğüme ve sistem durumu yedekleme çalıştırılana geçti, sürücü kullanılamıyor ve yedekleme başarısız olur. Bu senaryoda, bir yerel sürücüye işaret edecek şekilde PSDataSourceConfig.xml değiştirin.

Ardından, Windows Server Yedekleme, geri yükleme klasörü kök dizininde WindowsImageBackup adında bir klasör oluşturur. Windows Server Yedekleme yedekleme oluşturduğu gibi tüm veriler bu klasöre yerleştirilir. Yedekleme tamamlandığında dosya yedekleme sunucusu bilgisayara aktarılır. Aşağıdaki bilgileri not edin:

* Yedekleme veya aktarımı tamamlandığında, bu klasörü ve içeriği temizlenmesini değil. Bunu düşünmenin en iyi bir yedekleme tamamlandıktan sonraki açışlarında alan ayrılmış olduğunu yoludur.
* Bir yedekleme yapılan her zaman klasör oluşturulur. Saat ve tarih damgası, son sistem durumu yedeklemesi zaman yansıtır.

## <a name="bmr-backup"></a>BMR yedekleme

(Bir sistem durumu yedeklemesi dahil) BMR için yedekleme işi doğrudan bir paylaşıma yedekleme sunucusu bilgisayarına kaydedilir. Korumalı sunucuda bir klasöre kaydedilmez.

Yedek sunucu Windows Server Yedekleme çağırır ve bu BMR yedekleme için çoğaltma birimini paylaşır. Bu durumda, sürücü en fazla boş alanı ile kullanmak için Windows Server Yedekleme'yi bildirmez. Bunun yerine, iş için oluşturulan paylaşımı kullanır.

Yedekleme tamamlandığında dosya yedekleme sunucusu bilgisayara aktarılır. Günlükler, C:\Windows\Logs\WindowsServerBackup dizininde depolanır.

## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

-   BMR, Windows Server 2003 çalıştıran bilgisayarlar için veya bir istemci işletim sistemi çalıştıran bilgisayarlar için desteklenmiyor.

-   Farklı koruma gruplarındaki aynı bilgisayar için BMR'yi ve sistem durumunu koruyamazsınız.

-   Bir yedekleme sunucusu bilgisayar kendisini BMR için korunamıyor.

-   Banda (diskin teyp veya D2T) kısa vadeli koruma için BMR desteklenmiyor. Uzun vadeli depolama için banda (disk-disk-için-banda veya D2D2T) desteklenir.

-   BMR koruma için Windows Server Yedekleme korumalı bilgisayara yüklenmesi gerekir.

-   BMR koruma için farklı sistem durumu koruması için yedekleme sunucusu herhangi bir alan gereksinimi korumalı bilgisayarda yok. Windows Server Yedekleme, yedekleme sunucusu bilgisayara doğrudan yedeklemeleri aktarır. Yedekleme aktarım işi yedek sunucuda görünmüyor **işleri** görünümü.

-   Yedekleme sunucusu, BMR için 30 GB çoğaltma biriminde alan ayırır. Bu değiştirebilirsiniz **Disk ayırmayı** sayfasında koruma grubunu Değiştirme Sihirbazı'nı veya Get-DatasourceDiskAllocation ve Set-DatasourceDiskAllocation PowerShell cmdlet'lerini kullanarak. Kurtarma noktası birimde BMR koruması, beş gün bekletme için yaklaşık 6 GB gerektirir.
    * Çoğaltma birimi boyutunun 15 GB'tan daha az olamayacağını unutmayın.
    * Yedekleme sunucusu, BMR veri kaynağının boyutunu hesaplamaz. Tüm sunucular için 30 GB olduğunu varsayar. Ortamınızda beklenen BMR yedeklemelerinin boyutuna göre değeri değiştirin. Bir BMR yedeklemesinin boyutu kabaca tüm kritik birimlerde kullanılan alanın toplamı olarak hesaplanabilir. Kritik birimler = önyükleme birimi + sistem birimi + Active Directory gibi sistem durumu verilerini barındıran birim.

-   Sistem durumu korumasından BMR korumasına değiştirirseniz, BMR koruması üzerinde daha az alan gerektirir *kurtarma noktası birimi*. Ancak birimdeki ek alan geri alınmaz. Üzerinde birim boyutunu el ile küçültebilirsiniz **Disk ayırmayı Değiştir** sayfasında koruma grubunu Değiştirme Sihirbazı'nın veya Get-DatasourceDiskAllocation ve Set-DatasourceDiskAllocation PowerShell cmdlet'lerini kullanarak.

    Sistem durumu korumasından BMR korumasına değiştirirseniz, BMR koruması hakkında daha fazla alan gerektirir *çoğaltma birimi*. Birim otomatik olarak genişletilir. Varsayılan alan ayırmalarını değiştirmek istiyorsanız, Modify-DiskAllocation PowerShell cmdlet'ini kullanın.

-   BMR korumasından sistem durumu korumasına değiştirirseniz, kurtarma noktası biriminde daha fazla alan gerekir. Yedek sunucu otomatik olarak birim artırmak deneyebilirsiniz. Depolama havuzunda yeterli alan yoksa hata oluşur.

    BMR korumasından sistem durumu korumasına değiştirirseniz, korunan bilgisayarda alan gerekir. Sistem durumu koruması ilk çoğaltma yerel bilgisayara yazar ve yedekleme sunucu bilgisayarına aktarır olmasıdır.

## <a name="before-you-begin"></a>Başlamadan önce

1.  **Azure Backup sunucusu dağıtmak**. Yedekleme sunucusu doğru şekilde dağıtıldığını doğrulayın. Daha fazla bilgi için bkz.
    * [Azure yedekleme sunucusu için sistem gereksinimleri](http://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Yedek sunucu koruma Matrisi](backup-mabs-protection-matrix.md)

2.  **Depolama ayarlama**. Diskte, bantta ve Azure ile bulutta yedekleme verilerini depolayabilirsiniz. Daha fazla bilgi için bkz: [veri depolama alanı hazırlama](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage).

3.  **Koruma aracısını ayarlama**. Yedeklemek istediğiniz bilgisayara koruma aracısını yükleyin. Daha fazla bilgi için bkz: [DPM koruma aracısını dağıtın](http://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent).

## <a name="back-up-system-state-and-bare-metal"></a>Sistem durumu ve tam yedekleme
Bölümünde açıklandığı gibi bir koruma grubu ayarlayın [koruma gruplarını dağıtmak](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups). Farklı gruplarındaki aynı bilgisayar için BMR'yi ve sistem durumunu koruyamazsınız unutmayın. Ayrıca, BMR'yi seçtiğinizde sistem durumu otomatik olarak etkinleştirilir.


1.  Yedek sunucu Yönetici konsolunda yeni koruma grubu oluşturma Sihirbazı'nı açmak için seçin **koruma** > **Eylemler** > **koruma grubu oluşturma**.

2.  Üzerinde **koruma grubu türünü seçin** sayfasında **sunucuları**ve ardından **sonraki**.

3.  Üzerinde **grup üyelerini seçin** sayfasında, bilgisayarı genişletin ve ardından ya da **BMR** veya **sistem durumu**.

    Farklı gruplarındaki aynı bilgisayar için BMR'yi ve sistem durumunu koruyamazsınız unutmayın. Ayrıca, BMR'yi seçtiğinizde sistem durumu otomatik olarak etkinleştirilir. Daha fazla bilgi için bkz: [koruma gruplarını dağıtmak](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups).

4.  Üzerinde **veri koruma yöntemini seçin** sayfasında, kısa vadeli ve uzun vadeli yedekleme işlemek istediğiniz şekli seçin. Kısa vadeli yedekleme her zaman olan ilk disk, Azure Backup (kısa veya uzun süreli) kullanarak diskten Azure'a yedekleme seçeneği bulut. Bulut için uzun vadeli yedekleme için yedekleme sunucusuna bağlı bir tek başına bant aygıt veya bant kitaplığı için uzun vadeli yedekleme ayarlamak için alternatiftir.

5.  Üzerinde **kısa vadeli hedefleri seçin** sayfasında, kısa vadeli depolama için disk üzerinde yedeklemek istediğiniz nasıl seçin:
    1. İçin **bekletme aralığı**, ne kadar disk üzerinde verileri tutmak istediğinizi seçin. 
    2. İçin **eşitleme sıklığı**, ne sıklıkta diske bir artımlı yedekleme çalıştırmak istediğinizi seçin. Yedekleme aralığını ayarlamak istemiyorsanız kontrol edebilirsiniz **bir kurtarma noktasından hemen önce** seçeneği. Yalnızca her kurtarma noktası zamanlanmadan önce yedekleme sunucusu express, tam bir yedekleme çalıştırın.

6.  Banttaki verilerin uzun vadeli depolama için depolamak istiyorsanız **uzun vadeli hedefleri belirtin** sayfasında, bu bant verilerinin (1-99 yıl) korumak istediğiniz ne kadar süreyle seçin. 
    1. İçin **yedekleme sıklığı**, ne sıklıkta banda yedekleme Çalıştır'ı seçin. Sıklık seçtiğiniz bekletme aralığına dayalıdır:
        * Bekletme aralığı 1-99 yıl arası olduğunda, yedeklemelerin günlük, haftalık, iki haftalık, aylık, üç aylık, altı aylık veya yıllık olarak gerçekleşmesini seçebilirsiniz.
        * Bekletme aralığı 1-11 ay arası olduğunda, yedeklemelerin günlük, haftalık, iki haftalık, gerçekleşmesi için ya da aylık seçebilirsiniz.
        * Bekletme aralığı 1-4 hafta arası olduğunda, yedeklemelerin günlük veya haftalık olarak gerçekleşmesini seçebilirsiniz.

    2. Üzerinde **bant ve kitaplık ayrıntılarını seçin** sayfasında, kullanılacak kitaplık ve bant seçin ve olup veri sıkıştırılmış ve gerekir şifrelenmiş.

7.  Üzerinde **Disk ayırmayı gözden** sayfasında, koruma grubu için ayrılmış depolama havuzu disk alanını gözden geçirin.

    1. **Toplam veri boyutu** yedeklemek istediğiniz verileri boyutudur.
    2. **Disk üzerinde Azure yedekleme sunucusu sağlanacak alan** koruma grubu için yedekleme sunucusu önerir alanıdır. Yedekleme sunucusu ayarlarınızı temel alan ideal yedekleme birimi seçer. Ancak, yedekleme birimi seçimleri düzenleyebilirsiniz **Disk ayırması ayrıntıları**. 
    3. İş yükleri için açılır menüde, tercih edilen depolama seçin. Düzenlemeleriniz değerlerini değiştirmek **toplam depolama** ve **boş depolama** içinde **kullanılabilir Disk Depolama** bölmesi. Yedekleme sunucusu kesintisiz yedeklemeleri emin olmak için birime eklediğiniz öneren depolama miktarını underprovisioned alanıdır.

8.  Üzerinde **çoğaltma oluşturma yöntemini seçin** sayfasında, ilk tam veri çoğaltmayı düzenlemek istediğiniz şekli seçin. Ağ üzerinden çoğaltılmasını seçerseniz yoğun olmayan bir zamanı seçmeniz önerilir. Büyük miktarlarda veri veya en iyi durumda olmayan ağ koşulları için Çıkarılabilir medya kullanarak çevrimdışı veri çoğaltmayı düşünebilirsiniz.

9. Üzerinde **tutarlılık denetimi seçenekleri seçin** sayfasında, tutarlılık denetimleri otomatik hale getirmek istediğiniz şekli seçin. Yalnızca çoğaltma verilerini olduğunda bir onay tutarsız olarak veya bir zamanlamaya göre çalıştırmayı seçebilirsiniz. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, el ile denetim herhangi bir zamanda çalıştırabilirsiniz. El ile denetim çalıştırmak için **koruma** yedekleme sunucu Yönetici Konsolu'nun alanı koruma grubuna sağ tıklayın ve ardından **tutarlılık denetimi gerçekleştir**.

10. Azure Backup kullanarak buluta yedekleme seçtiyseniz **çevrimiçi koruma verilerini belirtin** sayfasında, Azure'a yedeklemek istediğiniz iş yüklerini seçtiğinizden emin olun.

11. Üzerinde **çevrimiçi yedekleme zamanlamasını belirtin** sayfa, Azure select ne sıklıkta artımlı yedeklemeler meydana gelir. Her gün, hafta, ay ve yıl çalıştırmak ve tarih ve saat çalışacakları seçmek için yedeklemeler zamanlayabilirsiniz. Yedekleme günde en fazla iki kez ortaya çıkabilir. Bir yedekleme her çalıştığında, yedekleme sunucusu diskte depolanan yedekleme verilerini kopyasından Azure'da bir veri kurtarma noktası oluşturulur.

12. Üzerinde **çevrimiçi bekletme ilkesini belirtin** sayfasında, günlük, haftalık, aylık ve yıllık yedeklemelerden oluşturulan kurtarma noktaları Azure'da nasıl korunur.

13. Üzerinde **seçin çevrimiçi çoğaltma** sayfasında, ilk tam veri çoğaltmasının nasıl gerçekleştiğini seçin. Ağ üzerinden çoğaltmasına veya çevrimdışı yapmak (çevrimdışı dengeli) yedekleme. Çevrimdışı Yedekleme Azure içeri aktarma özelliğini kullanır. Daha fazla bilgi için bkz: [Azure backup'ta Çevrimdışı Yedekleme iş akışı](backup-azure-backup-import-export.md).

14. Üzerinde **Özet** sayfasında, ayarlarınızı gözden geçirin. Siz seçtikten sonra **Grup Oluştur**, ilk veri çoğaltma işlemi gerçekleşir. Veri çoğaltma tamamlandığında, üzerinde **durum** sayfasıdır, koruma grubunun durumu **Tamam**. Yedekleme sonra her koruma grubu ayarları gerçekleşir.

## <a name="recover-system-state-or-bmr"></a>Sistem durumu veya BMR kurtarma
Bir ağ konumuna BMR veya sistem durumunu kurtarabilirsiniz. BMR'yi yedeklediyseniz, sisteminizi başlatmak ve ağa bağlanmak için Windows Kurtarma Ortamı'nı (WinRE) kullanın. Ardından, ağ konumundan kurtarmak için Windows Server Yedekleme'yi kullanın. Yalnızca sistem durumunu yedeklediyseniz ağ konumundan kurtarmak için Windows Server Yedekleme'yi kullanın.

### <a name="restore-bmr"></a>BMR'yi geri yükleme
Kurtarma, yedekleme sunucusu bilgisayarda çalıştırın:

1.  İçinde **kurtarma** bölmesinde, kurtarmak istediğiniz Bul bilgisayar ve ardından **tam kurtarma**.

2.  Kullanılabilir kurtarma noktaları gösterilir Takvim üzerinde kalın. Tarih ve saat için kullanmak istediğiniz kurtarma noktası seçin.

3.  Üzerinde **kurtarma türünü seçin** sayfasında **bir ağ klasörüne kopyala.**

4.  Üzerinde **hedef belirtin** verileri kopyalamak istediğiniz sayfasında, seçin. Seçilen hedefin yeterli alana sahip olması gerektiğini unutmayın. Yeni bir klasör oluşturmanızı öneririz.

5.  Üzerinde **kurtarma seçeneklerini belirtin** sayfasında, uygulamak için güvenlik ayarlarını seçin. Depolama alanı ağı (SAN) kullanmak mı istediğinizi seçin, ardından-daha hızlı kurtarma için donanım anlık görüntülerini tabanlı. (Yalnızca bu işlevsellik kullanılabilir bir SAN ve özelliği oluşturmak ve bölme için yazılabilir yapmak için varsa bir seçenek budur. Buna ek olarak korumalı bilgisayar ve yedekleme sunucusu bilgisayar aynı ağa bağlanmalıdır.)

6.  Bildirim seçeneklerini ayarlayın. Üzerinde **onay** sayfasında, **kurtarmak**.

Paylaşım konumunu ayarlayın:

1.  Geri yükleme konumunda, yedekleme içeren klasöre gidin.

2.  Böylece paylaşılan klasörün kökünde WindowsImageBackup klasörü WindowsImageBackup yukarıda bir düzey klasörü paylaşın. Bunu yapmazsanız, geri yükleme yedeği bulmaz. Windows Kurtarma Ortamı'nı (WinRE) kullanarak bağlanmak için doğru IP adresi ve kimlik bilgileri ile oluşturulması erişebileceği bir paylaşım gerekir.

Sistem Geri yükleme:

1.  Geri yüklediğiniz sisteme için Windows DVD'sini kullanarak resmi geri yüklemek istediğiniz bilgisayarı başlatın.

2.  İlk sayfasında, dil ve yerel ayarları doğrulayın. Üzerinde **yükleme** sayfasında, **Bilgisayarınızı onarın**.

3.  Üzerinde **Sistem Kurtarma Seçenekleri** sayfasında, **daha önce oluşturduğunuz sistem görüntüsünü kullanarak bilgisayarınızı geri**.

4.  Üzerinde **sistem yansıması yedeği Seç** sayfasında **Sistem Görüntüsü Seç** > **Gelişmiş** > **ağda sistem görüntüsü Ara**. Bir uyarı görünürse seçin **Evet**. Paylaşım yolu için kimlik bilgilerini girin ve ardından Kurtarma noktasını seçin gidin. Bu, Kurtarma noktasında kullanılabilir belirli yedeklemelerinize tarar. Kullanmak istediğiniz kurtarma noktasını seçin.

5.  Üzerinde **yedeklemeyi geri yükleme şeklini seçin** sayfasında, **diskleri Biçimlendir ve yeniden bölümle**. Sonraki sayfada ayarlarını doğrulayın. 

6.  Geri yüklemeyi başlatmak için seçin **son**. Yeniden başlatma gereklidir.

### <a name="restore-system-state"></a>Sistem durumunu geri yükle

Kurtarma, yedekleme sunucusunda çalıştırın:

1.  İçinde **kurtarma** bölmesinde, kurtarmak istediğiniz bilgisayar Bul ve ardından **tam kurtarma**.

2.  Kullanılabilir kurtarma noktaları gösterilir Takvim üzerinde kalın. Tarih ve saat için kullanmak istediğiniz kurtarma noktası seçin.

3.  Üzerinde **kurtarma türünü seçin** sayfasında **bir ağ klasörüne Kopyala**.

4.  Üzerinde **hedef belirtin** verileri kopyalamak istediğiniz sayfasında, seçin. Seçilen hedefin yeterli alan gerektiğini unutmayın. Yeni bir klasör oluşturmanızı öneririz.

5.  Üzerinde **kurtarma seçeneklerini belirtin** sayfasında, uygulamak için güvenlik ayarlarını seçin. Ardından, daha hızlı kurtarma için SAN tabanlı donanım anlık görüntülerini kullanmak mı istediğinizi seçin. (Yalnızca oluşturma ve kopyayı yazılabilir yapmak için bölme bu işlevselliği ve özelliği ile bir SAN varsa bu bir seçenektir. Ayrıca, yedekleme sunucusu ve korumalı bilgisayar aynı ağa bağlı olması gerekir.)

6.  Bildirim seçeneklerini ayarlayın. Üzerinde **onay** sayfasında, **kurtarmak**.

Windows Server Yedekleme çalıştırın:

1.  Seçin **Eylemler** > **kurtarmak** > **bu sunucu** > **sonraki**.

2.  Seçin **başka bir sunucuya**seçin **konum türünü belirtin** sayfasında ve ardından **uzaktan paylaşılan klasörünü**. Kurtarma noktasını içeren klasörün yolunu girin.

3.  Üzerinde **kurtarma türünü seçin** sayfasında **sistem durumu**. 

4. Üzerinde **sistem durumu kurtarma için konum seçin** sayfasında **özgün konumuna**.

5.  Üzerinde **onay** sayfasında, **kurtarmak**. Geri yüklemeden sonra sunucuyu yeniden başlatın.

6.  Bu gibi durumlarda, sistem durumu geri ayrıca bir komut isteminde çalıştırabilirsiniz. Bunu yapmak için kurtarmak istediğiniz bilgisayarda Windows Server Yedekleme başlatın. Bir komut isteminde sürüm tanımlayıcı almak için şunu girin: ```wbadmin get versions -backuptarget \<servername\sharename\>```

    Sistem durumu geri yüklemesi başlatmak için sürüm tanıtıcısını kullanın. Komut isteminde girin: ```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

    Kurtarma başlatmak istediğinizi onaylayın. Komut İstemi penceresinde işlemi görebilirsiniz. Geri yükleme günlüğü oluşturulur. Geri yüklemeden sonra sunucuyu yeniden başlatın.

