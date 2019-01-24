---
title: Azure VM yedeklemesi hakkında
description: Azure VM yedeklemesi hakkında bilgi edinin ve en iyi yöntemlerden bazıları dikkat edin.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: 57d52412648cbe8a0791aa306075018a2092bf51
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827338"
---
# <a name="about-azure-vm-backup"></a>Azure VM yedeklemesi hakkında

Bu makalede nasıl [Azure Backup hizmeti](backup-introduction-to-azure-backup.md) Azure Vm'lerini yedekler.

## <a name="backup-process"></a>Yedekleme işlemi

Nasıl Azure Backup bir Azure Vm'leri için yedeklemeden aşağıda verilmiştir.

1. Yedekleme için seçilen Azure Vm'leri için Azure Backup hizmeti, belirttiğiniz yedekleme zamanlamasına uygun olarak bir yedekleme işi başlatır.
2. Hizmeti yedekleme uzantısını tetikler.
    - Windows Vm'leri kullanım _VMSnapshot_ uzantısı.
    - Linux Vm'leri kullanım _VMSnapshotLinux_ uzantısı.
    - Uzantının ilk VM yedeklemesi sırasında yüklenir.
    - Uzantıyı yüklemek için VM çalıştırılması gerekir.
    - VM çalışmıyorsa Backup hizmeti, temel alınan depolamanın anlık görüntüsünü alır (VM durduğunda herhangi bir uygulama yazma işlemi gerçekleşmediği için).
4. Backup uzantısı, depolama düzeyinde kilitlenme tutarlı/dosya tutarlı anlık görüntü alır.
5. Anlık görüntünün alınma sonra veriler kasaya aktarılır. Verimliliği en üst düzeye çıkarmak için hizmet tanımlar ve yalnızca (delta) önceki yedeklemeden itibaren değişmiş olan veri blokları aktarır.
5. Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

![Azure sanal makine yedekleme mimarisi](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="data-encryption"></a>Veri şifrelemesi

Azure Backup, veri yedekleme işleminin bir parçası olarak şifrelemez. Azure Backup, Azure Disk şifrelemesi kullanılarak şifrelenmiş bir Azure VM yedekleme desteklemiyor.

- Sanal makinelerin yedeklenmesi, BitLocker şifreleme Key(BEK) ile yalnızca şifrelenir ve anahtar şifreleme Key(KEK) birlikte BEK desteklenir, yönetilen ve yönetilmeyen Azure Vm'leri için.
- Bu nedenle bunlar okunabilir ve kullanılması için yalnızca yetkili kullanıcılar tarafından geri anahtar Kasası'na geri BEK(secrets) ve yedeklenen KEK(keys) şifrelenir.
- BEK ayrıca BEK kayıp, olduğu senaryolarda yedeklendiğinden beri yetkili kullanıcılar için KeyVault BEK geri yükleyebilir ve şifrelenmiş sanal Makineyi kurtarmak. Anahtarları ve gizli anahtarları şifrelenmiş VM'lerin şifrelenmiş biçimde, yetkisiz kullanıcıların ne Azure okuyabilir ya da kullanım yedeklenen anahtarları ve gizli anahtarları yedeklenir. Yalnızca doğru izin düzeylerine sahip kullanıcılar, yedekleme ve şifrelenmiş Vm'leri yanı sıra anahtarları ve gizli anahtarları geri yükleyebilirsiniz.

## <a name="snapshot-consistency"></a>Anlık görüntü tutarlılık

Uygulamaları çalıştırırken anlık görüntülerini almak için Azure Backup uygulamayla tutarlı anlık görüntüleri alır.

- **Windows Vm'leri**: Windows Vm'leri için yedekleme hizmeti Birim Gölge Kopyası Hizmeti (sanal makine disklerinin tutarlı bir anlık görüntü almak için VSS ile) düzenler.
    - Varsayılan olarak, Azure Backup, VSS tam yedeklemeler alır. [Daha fazla bilgi edinin](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
    - Azure yedekleme VSS kopyalama yedeklemeleri olabilmesi ayarı değiştirmek istiyorsanız, aşağıdaki kayıt defteri anahtarını ayarlayın:
        ```
        [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
        ""USEVSSCOPYBACKUP"="TRUE"
        ```
        - Çalıştırma aşağıdaki yukarıdaki kayıt defteri anahtarını ayarlayın (yönetici olarak) yükseltilmiş komut isteminden komutu:
          ```
          REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d TRUE /f
          ```
- **Linux Vm'leri**: Linux Vm'lerinizi Azure Backup, anlık görüntüsünü alır, uygulamayla tutarlı olmasını sağlamak için Linux betik öncesi ve betik sonrası çerçeve kullanabilirsiniz. VM anlık görüntüsünü alırken tutarlılığını sağlamak için kendi özel komut dosyaları yazabilirsiniz.
    -  Azure Backup yalnızca çağırır öncesi ve sonrası betikler, tarafından yazılmış.
    - Betik öncesi ve sonrası Komut başarıyla çalışırsa, Azure Backup kurtarma noktası uygulamayla tutarlı olarak işaretler. Ancak, özel betikler kullanarak uygulama tutarlılığı için sorumlu olursunuz.
    - [Daha fazla bilgi edinin](backup-azure-linux-app-consistent.md) betiklerini yapılandırma hakkında daha fazla.


#### <a name="consistency-types"></a>Tutarlılık türleri

Aşağıdaki tabloda, tutarlılık farklı türleri açıklanmaktadır.

**Anlık görüntü** | **Ayrıntılar** | **Kurtarma** | **Önemli noktalar**
--- | --- | --- | ---
**Uygulamayla tutarlı** | Uygulamayla tutarlı yedeklemeler bellek içeriği ve bekleyen g/ç işlemlerini yakalayın. Uygulamayla tutarlı anlık görüntüleri VSS Yazıcı (veya Linux için ön/son betik) yedekleme gerçekleşmeden önce uygulama veri tutarlılığı kullanın. | Uygulamayla tutarlı bir anlık görüntü ile kurtarırken, VM yedekleme önyüklenir. Veri bozulmasına veya kaybına yoktur. Uygulamaları tutarlı bir durumda başlatın. | Windows: Tüm VSS yazıcılarının başarılı oldu<br/><br/> Linux: Ön/son betik başarılı oldu ve yapılandırılır
**Dosya sistemiyle tutarlı** | Dosya tutarlı yedekler, aynı anda tüm dosyaları anlık görüntüsünü alarak disk dosyalarının tutarlı yedeklemeler sağlar.<br/><br/> | Bir dosya tutarlı anlık görüntü ile kurtarırken, VM yedekleme önyüklenir. Veri bozulmasına veya kaybına yoktur. Geri yüklenen verilerin tutarlı olduğundan emin olmak için kendi "yukarı düzeltme" mekanizması uygulamak uygulamaları gerekir. | Windows: Bazı VSS yazıcılarının başarısız oldu <br/><br/> Linux: (Ön/son betik yapılandırılmış başarısız oldu veya varsa) varsayılan
**Kilitlenmeyle tutarlı** | Kilitlenme tutarlılığı, genellikle bir Azure VM yedekleme sırasında kapatıldığında oluşur.  Zaten diskte yedekleme sırasında var olan veriler yakalanır ve yedeklendi.<br/><br/> Kilitlenmeyle tutarlı kurtarma noktası işletim sistemi veya uygulama için veri tutarlılığı garanti etmez. | Hiçbir garanti eder, ancak genellikle VM önyüklemeleri vardır ve bozulma hatalarını düzeltmek için bir disk ile aşağıdaki gibi denetleyin. Herhangi bir bellek içi verileri veya transfer olmayan yazma disk kaybolur. Uygulama kendi veri doğrulaması uygular. Örneğin, işlem günlüğü veritabanında bulunmayan girişleri varsa veri tutarlı olana kadar için bir veritabanı uygulaması, veritabanı yazılımına yapar. | VM kapatma durumdadır


## <a name="service-and-subscription-limits"></a>Hizmet ve abonelik limitleri

Azure Backup, bir dizi sınırları abonelikleri ve Kasaları sahiptir.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Yedekleme performansı

### <a name="disk-considerations"></a>Disk konuları

Her sanal makinenin disk paralel yedekleme tarafından yedekleme işlemi en iyi duruma getirir. Örneğin, bir VM dört disk varsa, hizmeti tüm dört disklerini paralel yedekleme çalışır. Yedeklenmekte olan her bir disk için Azure Backup disk üzerindeki blokları okur ve yalnızca değiştirilen verileri (artımlı yedeklemeyi) depolar.


### <a name="scheduling-considerations"></a>Planlama konuları

Yedekleme Zamanlama performansını etkiler.

- Tüm VM'lerin aynı anda yedeklenir şekilde ilkeler yapılandırırsanız, tüm disklerde paralel yedeklemek yedekleme işlemi denediğinde bir trafik Başınızı zamanladınız.
- Yedekleme trafiğini azaltmak için farklı zaman örtüşme ile günün farklı sanal makinelerini yedekleme.


### <a name="time-considerations"></a>Zaman konuları

Okuma ve veri kopyalama en yedekleme zaman harcanmadığından sırasında diğer işlemleri bir sanal Makineyi yedeklemek için gereken toplam süreyi ekleyin:

- **Backup uzantısı yükleme**: Yükleme veya güncelleştirme yedekleme uzantısı için gereken süre.
- **Tetikleyici anlık görüntü**: Anlık görüntü tetiklemek için geçen süre. Anlık görüntüleri zamanlanmış yedekleme zamanını yakın tetiklenir.
- **Sıra bekleme süresi**: Yedekleme hizmeti işlemleri işleri beri birden çok müşteri depolama hesabından aynı zamanda, anlık görüntü verileri hemen kurtarma Hizmetleri kasasına kopyalanmaması. Yoğun yük zamanlarda, yedeklemeleri işlenmeden önce en fazla sekiz saat sürebilir. Ancak, toplam VM yedekleme zamanını 24 saatten daha kısa bir süre için günlük yedekleme ilkelerini olur.
- **İlk yedekleme**: Toplam yedekleme zamanını 24 saatten az, artımlı yedeklemeler için geçerli olsa da, ilk yedekleme için olmayabilir. Zaman Yedekleme alındığında ve verilerin boyutuna bağlı olacaktır.
- **Veri aktarımı zaman**: Depolama kasası için artımlı değişiklikler önceki yedeklemeden işlem ve bu değişiklikleri aktarmak yedekleme hizmeti için gereken süre.

### <a name="factors-affecting-backup-time"></a>Yedekleme süresini etkileyen faktörler

Yedekleme anlık görüntüleri alma ve anlık görüntüleri kasasına aktarma iki aşamaları oluşur. Backup hizmeti, depolama için iyileştirir.

- Anlık görüntü verileri bir kasaya aktarırken hizmeti, yalnızca artımlı değişiklikler önceki anlık görüntüden aktarır.
- Artımlı değişiklikleri belirlemek için hizmet bloğu sağlama toplamını hesaplar.
    - Bir blok değiştirilirse, blok kasaya gönderilecek bir blok olarak tanımlanır.
    - Hizmet tatbikatları daha fazla her tanımlanan blokları, veri aktarmak için en aza indirmek fırsatlar arar.
    - Hizmet, tüm değiştirilen blokları değerlendirme sonra değişiklikleri birleştirir ve bunları kasaya gönderir.

Yedekleme süresini etkileyen durumlar şunlardır:

- **Yeni eklenen diski zaten korumalı bir VM için ilk yedekleme**: Ardından mevcut disk değişim çoğaltması ile birlikte ilk çoğaltma yapmak yeni eklenen diski olduğundan VM artımlı yedekleme yapılıyor ve bu VM için yeni bir disk eklenmiş, yedekleme süresi 24 saat gidebilirsiniz.
- **Parçalanma**: Yedekleme ürününde iki yedekleme işlemleri arasındaki artımlı değişiklikler tarar. Yedekleme işlemleri daha hızlı değişiklikler karşılaştırıldığında değişiklikleri disk üzerinde ne zaman birlikte olan üzerinden yayılan sonra disk. 
- **Değişim**: 200 GB'tan büyük disk başına günlük değişim sıklığı (için artımlı çoğaltma), işlemi tamamlamak için ~ 8 saatten büyük alabilir. VM birden fazla disk içeriyorsa ve söz konusu disklerden yedekleme uzun sürüyorsa, ardından bunu genel yedekleme işlemi etkileyebilir (veya yüklenememesine neden olabilir). 
- **Sağlama toplamı (CC) karşılaştırma modunu**: CC modu, anında RP tarafından kullanılan en iyi duruma getirilmiş modu daha yavaştır. Anlık RP zaten kullanıyorsunuz ve 1. Katman anlık görüntüleri sildikten, yedekleme 24 saat (veya başarısız) yedekleme işlemi neden CC moduna geçer.

## <a name="restore-considerations"></a>Geri yükleme konuları

Geri yükleme işlemi iki ana görevden oluşur: seçilen depolama hesabına geri kasadan veri kopyalama ve sanal makine oluşturuluyor. Verileri kasadan kopyalama için gereken süre, yedekleri Azure ve depolama hesabı konumu depolandığı üzerinde bağlıdır. Verileri kopyalamak için harcanan süre bağlıdır:

- **Sıra bekleme süresi**: Hizmet işlemleri birden çok depolama hesaplarından aynı anda geri yükleme işlerini olduğundan geri yükleme isteği sıraya yerleştirilir.
- **Veri kopyalama zaman**: Verileri kasadan depolama hesabına kopyalanır. Geri yükleme süresi, IOPS ve aktarım hızı Azure Backup hizmetinin kullandığı seçilen depolama hesabına bağlıdır. Geri yükleme işlemi sırasında kopyalama süresini azaltmak için diğer uygulama yazar ve okur ile yüklenmemiş bir depolama hesabı seçin.

## <a name="best-practices"></a>En iyi uygulamalar

VM yedeklemeleri yapılandırılırken bu yöntemler aşağıdaki öneririz:

- Varsayılan ilke zaman değiştirmeyi düşünün (için örnek. Varsayılan ilke sürenizi 12: 00'da göz önünde bulundurun dakika artan ise), veri anlık görüntüleri kaynakları en iyi şekilde kullanılan emin olmak için.
- Premium VM için yedekleme anlık RP olmayan özelliğini ~ % 50'hesap toplam depolama alanı ayırır. Yedekleme hizmeti Kasası'na aktarılması ve aynı depolama hesabını anlık görüntüyü kopyalamak için bu alanı gerektirir.
- Vm'leri tek bir kasadan geri yüklemek için farklı kullanmak için önerilir [v2 depolama hesaplarının](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) hedef depolama hesabı olmayan kısıtlanan emin olmak için. Örneğin, her VM (ise 10 Vm'leri geri sonra 10 farklı depolama hesabı kullanmayı düşünün) farklı bir depolama hesabı olmalıdır.
- (Aynı depolama hesabı olduğundan) 1. katman depolama katmanından (snapshot) geri yükleme işlemleri saat sürebilir, Katman-2 depolama katmanı karşı (kasa) birkaç dakika içinde tamamlanır. Kullanmanızı öneririz [anında geri yükleme](backup-instant-restore-capability.md) veri kullanılabildiği Katman-1'de (veri saat sürer ardından kasadan geri yüklenmesi gerekiyorsa) çalışması için daha hızlı geri yüklemeler için özellik.
- Depolama hesabı başına disk sayısı sınırına göre yükün diskleri Iaas VM üzerinde çalışan uygulamalar tarafından erişildiği ' dir. Tek bir depolama hesabında birden fazla disk barındırılıyorsa, doğrulayın. 5-10 diskleri veya daha fazla tek bir depolama hesabında mevcut olması durumunda, genel bir yöntem olarak, depolama hesabı ayırmak için bazı diskler taşıyarak Yük Dengelemesi yapın.

## <a name="backup-costs"></a>Yedekleme maliyetleri

Azure Vm'leri Azure Backup ile yedeklenmekte olan konusu [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).

- Faturalandırma, ilk başarılı yedekleme tamamlanana kadar başlatılmaz. Bu noktada, hem depolama hem de korunan örnekler için faturalandırma başlar.
- Sanal makine için bir kasada depolanan yedekleme tüm veriler var olduğu sürece, faturalandırma devam eder. Sanal makinede korumayı durdurun, ancak sanal makine yedekleme verilerini bir kasada var ise, faturalandırma devam eder.
- Belirtilen bir VM için fatura bilgilerini yalnızca koruma durduruldu ve tüm yedekleme verileri silinir durdurur.
- Koruma durduğunda ve yedekleme etkin iş yok, en son başarılı VM yedekleme boyutu için aylık fatura kullanılan korumalı örnek boyutu haline gelir.
- Korunan örnekler hesaplaması dayanır *gerçek* geçici depolama hariç sanal makinenin tüm verilerin toplamıdır sanal makine boyutu.
- Fiyatlandırma veri diski depolanan gerçek verileri temel alır.
- VM'lerin yedeklenmesi için fiyatlandırma sanal makineye bağlı her veri diski için desteklenen en büyük boyut temel almaz.
- Benzer şekilde, Azure Yedekleme'de her kurtarma noktası gerçek verileri toplamı olduğu depolanan veri miktarı yedekleme alanı faturada dayanır.

Örneğin, bir standart A2 boyutu en fazla 4 TB'lık iki ek veri diskleri olan sanal makine yararlanın. Aşağıdaki tabloda bu disklerin her biri üzerinde depolanan gerçek veri sunar:

| Disk türü | Maksimum boyut | Gerçek veri yok |
| --------- | -------- | ----------- |
| İşletim sistemi diski |4095 GB |17 GB |
| Yerel disk / geçici disk |135 GB |5 GB (yedekleme için dahil değil) |
| Veri diski 1 |4095 GB |30 GB |
| Veri diski 2 |4095 GB |0 GB |

- VM sanal makinenin gerçek boyut, 17 GB + 30 GB + 0 GB = 47 GB bu durumda olur.
- Bu korumalı örnek boyutu (GB 47) başına aylık fatura olur.
- Sanal makinede veri miktarı büyüdükçe, korumalı örnek boyutu için faturalandırma değişiklikleri uygun şekilde kullanılır.


## <a name="next-steps"></a>Sonraki adımlar

Yedekleme işlemi ve performans konuları gözden geçirdikten sonra aşağıdakileri yapın:

- [Hakkında bilgi edinin](../virtual-machines/windows/premium-storage-performance.md) uygulamaları için Azure depolama ile en iyi performans ayarlama. Premium depolama, makale odaklanmak ancak aynı zamanda standart depolama diskleri için uygulanabilir.
- [Başlama](backup-azure-arm-vms-prepare.md) VM desteği ve sınırlamalar inceleyerek daha fazla yedekleme ile kasa oluşturma ve sanal makineleri yedekleme için hazırlanıyor.
