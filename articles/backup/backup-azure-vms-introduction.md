---
title: Azure VM yedeklemesi hakkında
description: Azure VM yedeklemesi hakkında bilgi edinin ve en iyi yöntemlerden bazıları dikkat edin.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: raynew
ms.openlocfilehash: f4a2fe4c9307f7e59ca94e47683356143546d090
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57310752"
---
# <a name="about-azure-vm-backup"></a>Azure VM yedeklemesi hakkında

Bu makalede nasıl [Azure Backup hizmeti](backup-introduction-to-azure-backup.md) Azure Vm'lerini yedekler.

## <a name="backup-process"></a>Yedekleme işlemi
Nasıl Azure Backup bir Azure Vm'leri için yedeklemeden aşağıda verilmiştir.

1. Yedekleme için seçilen Azure Vm'leri için Azure Backup hizmeti, belirttiğiniz yedekleme zamanlamasına uygun olarak bir yedekleme işi başlatır.
2. Çalışıyorsa ilk yedekleme sırasında VM üzerinde bir yedekleme uzantısı yüklenir.

    - Windows Vm'leri için _VMSnapshot_ uzantısı yüklü.
    - Linux VM'ler için _VMSnapshotLinux_ uzantısı yüklü.
3. Windows çalıştıran VM'ler için yedekleme, sanal makinenin uygulama ile tutarlı bir anlık görüntüsünü almak için VSS ile düzenler.

    - Varsayılan olarak, VSS yedeklemeleri tam yedeklemesini alır.
    - Yedekleme, uygulamayla tutarlı bir anlık görüntüsünü almak silemiyor (VM durduğunda herhangi bir uygulama yazma işlemi gerçekleşmediği için) sonra temel alınan depolama alanı dosya tutarlı anlık görüntüsünü alır.
4. Linux sanal makineleri yedekleme için dosya tutarlı yedeklemesini alır. Uygulamayla tutarlı anlık görüntüler için el ile ön/son betik özelleştirmeniz gerekir.
5. Anlık görüntünün alınma sonra veriler kasaya aktarılır.

    - Yedekleme, paralel her VM disk yedekleyerek en iyi duruma getirilmiştir.
    - Yedeklenmekte olan her bir disk için Azure Backup, diskteki blokları okur ve tanımlar ve (delta) önceki yedeklemeden itibaren değişmiş olan veri blokları aktarır.
    - Anlık görüntünün alınma sonra veriler kasaya aktarılır.
    - Anlık görüntü verileri hemen kasaya kopyalanmaması. Bu, yoğun saatlerde bazı saat sürebilir. Bir VM için toplam yedek süresi 24 saatten daha kısa bir süre için günlük yedekleme ilkelerini olacaktır.

6. Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

![Azure sanal makine yedekleme mimarisi](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="backing-up-encrypted-azure-vms"></a>Şifrelenmiş Azure Vm'lerini yedekleme
Azure sanal makinelerini Azure Backup ile yedekleme, sanal makineleri bekleyen depolama hizmeti şifrelemesi (SSE) ile şifrelenir. Ayrıca, Azure Backup, Azure sanal makinelerini, Azure Disk şifrelemesi (ADE) kullanılarak şifrelenmesini yedekleyebilirsiniz.

**Şifreleme** | **Ayrıntılar** | **Destek**
--- | --- | ---
**ADE** | ADE Azure Vm'leri için hem işletim sistemi ve veri disklerini şifreler.<br/><br/> ADE anahtar kasasında gizli dizi olarak korunması BitLocker şifreleme anahtarları (BEK) veya Azure anahtar kasası anahtar şifreleme anahtarı (KEK) ile tümleşir. | Azure Backup, yönetilen ve yönetilmeyen Azure yalnızca BEK veya BEK birlikte KEK ile şifrelenmiş VM'ler yedeklemesini destekler.<br/><br/> Her iki BEK ve yedeklenen ve şifrelenir.<br/><br/> KEK ve BEK izinlerine sahip olan kullanıcıların gerekirse yedeklediğiniz beri anahtarları ve gizli anahtarları geri anahtar Kasası'na geri yükleyebilir ve şifrelenmiş sanal Makineyi kurtarma.<br/><br/> Yetkisiz kullanıcılar tarafından veya Azure tarafından şifrelenmiş anahtarları ve gizli anahtarları okunamıyor.
**SSE** | SSE, Azure depolama otomatik olarak depolamadan önce verileri şifreleyerek bekleyen şifreleme sağlar ve alma önce çözer. | Azure Backup için SSE bekleyen şifreleme Azure vm'leri kullanır.

- BitLocker şifreleme anahtarı (BEK) yalnızca ve anahtar şifreleme Key(KEK) birlikte BEK ile şifrelenmiş sanal makinelerin yedeklenmesi, yönetilen ve yönetilmeyen Azure Vm'leri için desteklenir.
- Yedeklenen (gizli anahtarlar) BEK ve KEK (anahtarlar) şifrelenir. Bunlar okunabilir ve yalnızca yetkili kullanıcılar tarafından geri anahtar Kasası'na geri kullanılır. Yetkisiz kullanıcılar ya da Azure okuyabilir veya anahtarlar veya parolalar ' kullanımı desteklenir.
- BEK kaybolursa BEK ayrıca yedeklendiğinden olduğundan, yetkili kullanıcıların BEK geri yüklemek için anahtar kasası ve şifrelenmiş sanal Makineyi kurtarma.
- Yalnızca doğru izin düzeylerine sahip kullanıcılar, yedekleme ve şifrelenmiş Vm'leri yanı sıra anahtarları ve gizli anahtarları geri yükleyebilirsiniz.


## <a name="taking-snapshots"></a>Anlık görüntü alma
Azure yedekleme anlık görüntüleri yedekleme zamanlamasına uygun olarak.

- **Windows Vm'leri**: Windows Vm'leri için yedekleme hizmeti Birim Gölge Kopyası Hizmeti (sanal makine diskleri uygulamayla tutarlı bir anlık görüntüsünü almak için VSS ile) düzenler.
    - Varsayılan olarak, Azure Backup, VSS tam yedeklemeler alır. [Daha fazla bilgi edinin](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
    - Azure yedekleme VSS kopyalama yedeklemeleri olabilmesi ayarı değiştirmek istiyorsanız, bir komut isteminden aşağıdaki kayıt defteri anahtarını ayarlayın: **REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d TRUE /f**.
- **Linux Vm'leri**: Uygulamayla tutarlı Linux VM anlık isterseniz Linux ön betik kullanma ve tutarlılığını sağlamak için kendi komut dosyalarınızı yazmak için framework sonrası komut dosyası.
    -  Azure yedekleme, yalnızca sizin tarafınızdan yazılan ön/son betiklerini çağırır.
    - Betik öncesi ve sonrası Komut başarıyla çalışırsa, Azure Backup kurtarma noktası uygulamayla tutarlı olarak işaretler. Ancak, özel betikler kullanarak uygulama tutarlılığı için sorumlu olursunuz.
    - [Daha fazla bilgi edinin](backup-azure-linux-app-consistent.md) betiklerini yapılandırma hakkında daha fazla.


### <a name="snapshot-consistency"></a>Anlık görüntü tutarlılık

Aşağıdaki tabloda, tutarlılık farklı türleri açıklanmaktadır.

**Anlık görüntü** | **Ayrıntılar** | **Kurtarma** | **Önemli noktalar**
--- | --- | --- | ---
**Uygulamayla tutarlı** | Uygulamayla tutarlı yedeklemeler bellek içeriği ve bekleyen g/ç işlemlerini yakalayın. Uygulamayla tutarlı anlık görüntüleri VSS Yazıcı (veya Linux için ön/son betik) yedekleme gerçekleşmeden önce uygulama veri tutarlılığı kullanın. | Uygulamayla tutarlı bir anlık görüntü ile kurtarırken, VM yedekleme önyüklenir. Veri bozulmasına veya kaybına yoktur. Uygulamaları tutarlı bir durumda başlatın. | Windows: Tüm VSS yazıcılarının başarılı oldu<br/><br/> Linux: Ön/son betik başarılı oldu ve yapılandırılır
**Dosya sistemiyle tutarlı** | Dosya tutarlı yedekler, aynı anda tüm dosyaları anlık görüntüsünü alarak disk dosyalarının tutarlı yedeklemeler sağlar.<br/><br/> | Bir dosya tutarlı anlık görüntü ile kurtarırken, VM yedekleme önyüklenir. Veri bozulmasına veya kaybına yoktur. Geri yüklenen verilerin tutarlı olduğundan emin olmak için kendi "yukarı düzeltme" mekanizması uygulamak uygulamaları gerekir. | Windows: Bazı VSS yazıcılarının başarısız oldu <br/><br/> Linux: (Ön/son betik yapılandırılmış başarısız oldu veya varsa) varsayılan
**Kilitlenmeyle tutarlı** | Kilitlenme tutarlılığı, genellikle bir Azure VM yedekleme sırasında kapatıldığında oluşur.  Zaten diskte yedekleme sırasında var olan veriler yakalanır ve yedeklendi.<br/><br/> Kilitlenmeyle tutarlı kurtarma noktası işletim sistemi veya uygulama için veri tutarlılığı garanti etmez. | Hiçbir garanti eder, ancak genellikle VM önyüklemeleri vardır ve bozulma hatalarını düzeltmek için bir disk ile aşağıdaki gibi denetleyin. Herhangi bir bellek içi verileri veya transfer olmayan yazma disk kaybolur. Uygulama kendi veri doğrulaması uygular. Örneğin, işlem günlüğü veritabanında bulunmayan girişleri varsa veri tutarlı olana kadar için bir veritabanı uygulaması, veritabanı yazılımına yapar. | VM kapatma durumdadır


## <a name="backup-considerations"></a>Yedekleme konuları

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Disk** | Her sanal makinenin disk paralel yedekleme tarafından yedekleme işlemi en iyi duruma getirir. Örneğin, bir VM dört disk varsa, hizmeti tüm dört disklerini paralel yedekleme çalışır. Yedeklenmekte olan her bir disk için Azure Backup disk üzerindeki blokları okur ve yalnızca değiştirilen verileri (artımlı yedeklemeyi) depolar.
**Zamanlama** |  Yedekleme trafiğini azaltmak için günün olmadan farklı zamanlarda farklı Vm'leri yedekleme ile çakışıyor. Aynı anda VM'lerin yedeklenmesi trafik sıkışıklıklarının neden olur.
**Yedekleri hazırlanıyor** | Yükleme veya yedekleme uzantısı güncelleştiriliyor ve anlık görüntü yedekleme zamanlamasına uygun olarak tetikleyen içeren yedekleme hazırlık süresini göz önünde bulundurun.
**Veri aktarımı** | Yedekleme hizmeti, artımlı değişiklikler önceki yedeklemeden hesaplamak gereken zamanı.<br/><br/> Artımlı bir yedeklemede bulunan blok sağlama toplamı değişiklikleri hizmeti bilgisayarı belirleyin. Bir blok, tanımlanmış bir kasaya göndermek için değiştirilirse. Hizmet, daha fazla veri aktarmak için en aza indirmek denemek için tanımlanan bloklarda ayrıntılı açıklanmıştır. Tüm değerlendirmesi engellenen değiştikten sonra değişiklikleri kasaya aktarılır.<br/><br/> Anlık görüntü alma ve kasaya kopyalayarak arasında bir gecikme olabilir.<br/><br/> Yoğun saatlerde uygulamanın işlem bekletilecek yedeklemeleri sekiz saate kadar sürebilir. Bir VM için yedekleme zamanı günlük yedekleme için 24 saatten az olacaktır.
**İlk yedekleme** | Toplam yedekleme zamanını 24 saatten az, artımlı yedeklemeler için geçerli olsa da, ilk yedekleme için olmayabilir. Zaman Yedekleme alındığında ve verilerin boyutuna bağlı olacaktır.
**. Sıra bekleme süresi** | Yedekleme hizmeti işlemleri işleri beri birden çok müşteri depolama hesabından aynı zamanda, anlık görüntü verileri hemen kurtarma Hizmetleri kasasına kopyalanmaması. Yoğun yük zamanlarda, yedeklemeleri işlenmeden önce en fazla sekiz saat sürebilir. Ancak, toplam VM yedekleme zamanını 24 saatten daha kısa bir süre için günlük yedekleme ilkelerini olur.


### <a name="backup-performance"></a>Yedekleme performansı
Bu senaryoyu yedekleme zamanı etkileyebilir:

- Korunan bir Azure sanal makine için yeni bir disk ekleyin: Yedekleme, VM artımlı yedekleme yapılıyor ve yeni disk eklendiğinde, 24 saatten fazla yeni disk, mevcut disk değişim çoğaltması ile birlikte ilk çoğaltması nedeniyle son.
- Parçalanmış diskler: Disk değişiklikleri birlikte, yedekleme işlemleri daha hızlıdır. Değişiklikleri dağılmış ve bir disk arasında parçalanmış, yedekleme daha yavaş olur.
- Disk değişim sıklığı: Yedekleme, 200 GB'tan fazla bir günlük değişim sıklığı artımlı yedekleme aşamasında korunan disk varsa, tamamlanması uzun zaman (sekiz saatten fazla) sürebilir.
- Sağlama toplamı (CC) karşılaştırma modu: CC modu, anlık geri yükleme tarafından kullanılan en iyi duruma getirilmiş modu daha yavaştır. Anında geri yükleme zaten kullanıyorsunuz ve anlık görüntüleri sildikten, yedekleme 24 saat (veya başarısız) yedekleme işlemi neden CC moduna geçer.

## <a name="restore-considerations"></a>Geri yükleme konuları

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Geri Yükleme sırası** | Azure yedekleme işleri birden fazla depolama hesaplarından aynı anda geri yükleme işlemleri ve geri yükleme isteği sıraya yerleştirilir.
**Kopyalama geri yükleme** | Geri yükleme işlemi sırasında verileri kasadan depolama hesabına kopyalanır.<br/><br/> Geri yükleme süresi, IOPS ve aktarım hızı depolama hesabının bağlıdır.<br/><br/> Kopyalama süreyi azaltmak üzere diğer uygulama yazar ve okur ile yüklü olmayan bir depolama hesabı seçin.


## <a name="best-practices"></a>En iyi uygulamalar
VM yedeklemeleri yapılandırılırken bu yöntemler aşağıdaki öneririz:

- İlke kümesi varsayılan zamanlama süresi değiştirmeyi düşünün. Örneğin, varsayılan saat 12: 00'da ilkedir ise, kaynakları en iyi şekilde kullanılır, böylece da dakika artırmadan düşünün.
- Premium VM için yedekleme anlık RP olmayan özelliğini ~ % 50'hesap toplam depolama alanı ayırır. Yedekleme hizmeti Kasası'na aktarılması ve aynı depolama hesabını anlık görüntüyü kopyalamak için bu alanı gerektirir.
- Vm'leri tek bir kasadan geri yüklemek için farklı kullanmak için önerilir [v2 depolama hesaplarının](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) hedef depolama hesabı olmayan kısıtlanan emin olmak için. Örneğin, her VM (ise 10 Vm'leri geri sonra 10 farklı depolama hesabı kullanmayı düşünün) farklı bir depolama hesabı olmalıdır.
- (Aynı depolama hesabı olduğundan) 1. katman depolama katmanından (snapshot) geri yükleme işlemleri saat sürebilir, Katman-2 depolama katmanı karşı (kasa) birkaç dakika içinde tamamlanır. Kullanmanızı öneririz [anında geri yükleme](backup-instant-restore-capability.md) veri kullanılabildiği Katman-1'de (veri saat sürer ardından kasadan geri yüklenmesi gerekiyorsa) çalışması için daha hızlı geri yüklemeler için özellik.
- Depolama hesabı başına disk sayısı sınırına göre yükün diskleri Iaas VM üzerinde çalışan uygulamalar tarafından erişildiği ' dir. Tek bir depolama hesabında birden fazla disk barındırılıyorsa, doğrulayın. 5-10 diskleri veya daha fazla tek bir depolama hesabında mevcut olması durumunda, genel bir yöntem olarak, depolama hesabı ayırmak için bazı diskler taşıyarak Yük Dengelemesi yapın.


## <a name="backup-costs"></a>Yedekleme maliyetleri
Azure Vm'leri Azure Backup ile yedeklenmekte olan konusu [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).

- Faturalandırma, ilk başarılı yedekleme tamamlanana kadar başlamıyor. Bu noktada, hem depolama hem de korumalı VM'ler için faturalandırma başlar.
- Tüm yedekleme verileri VM için bir kasada depolanmış var olduğu sürece, faturalandırma devam eder. Bir VM için korumayı durdurun, ancak VM için yedekleme verileri bir kasada var ise, faturalandırma devam eder.
- Belirtilen bir VM için fatura bilgilerini yalnızca koruma durduruldu ve tüm yedekleme verileri silinir durdurur.
- Koruma durduğunda ve yedekleme etkin iş yok, en son başarılı VM yedekleme boyutu için aylık fatura kullanılan korumalı örnek boyutu haline gelir.
- Korumalı örnekler hesaplaması dayanır *gerçek* VM'deki geçici depolama hariç tüm verilerin toplam VM boyutu.
- Fiyatlandırma veri diski depolanan gerçek verileri temel alır.
- Vm'leri yedeklemeye yönelik fiyatlandırma VM'ye bağlı her veri diski için desteklenen en büyük boyut temel almaz.
- Benzer şekilde, Azure Yedekleme'de her kurtarma noktası gerçek verileri toplamı olduğu depolanan veri miktarı yedekleme alanı faturada dayanır.

Örneğin, iki ek veri diskleri en fazla 4 TB olan bir boyutta standart A2 VM yararlanın. Aşağıdaki tabloda bu disklerin her biri üzerinde depolanan gerçek veri sunar:

**Disk** | **En büyük boyutu** | **Gerçek veri yok**
--- | --- | ---
İşletim sistemi diski | 4095 GB | 17 GB
Yerel/geçici disk | 135 GB | 5 GB (yedekleme için dahil değil)
Veri diski 1 | 4095 GB | 30 GB
Veri diski 2 | 4095 GB | 0 GB

- Gerçek VM boyutuna, 17 GB + 30 GB + 0 GB = 47 GB bu durumda olur.
- Bu korumalı örnek boyutu (GB 47) başına aylık fatura olur.
- Sanal makine içindeki veri miktarı büyüdükçe, korumalı örnek boyutu için faturalandırma değişiklikleri uygun şekilde kullanılır.


## <a name="next-steps"></a>Sonraki adımlar

Şimdi, [hazırlama](backup-azure-arm-vms-prepare.md) Azure VM yedeklemesi için.
