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
ms.openlocfilehash: 93be913182db56941c346ef0cad47f70c0d614c9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706841"
---
# <a name="about-azure-vm-backup"></a>Azure VM yedeklemesi hakkında

Bu makalede nasıl [Azure Backup hizmeti](backup-introduction-to-azure-backup.md) Azure sanal makinelerini (VM) yedekler.

## <a name="backup-process"></a>Yedekleme işlemi

Nasıl Azure Backup bir Azure Vm'leri için yedeklemeden aşağıda verilmiştir:

1. Yedekleme için seçilen Azure Vm'leri için Azure Backup, belirttiğiniz yedekleme zamanlamaya göre bir yedekleme işi başlatır.
1. Sanal makine çalışıyorsa ilk yedekleme sırasında sanal makinede yedekleme uzantısı yüklenir.
    - Windows Vm'leri için _VMSnapshot_ uzantısı yüklü.
    - Linux VM'ler için _VMSnapshotLinux_ uzantısı yüklü.
1. Windows çalıştıran VM'ler için yedekleme, Windows Birim Gölge Kopyası Hizmeti (sanal makinenin uygulama ile tutarlı bir anlık görüntüsünü almak için VSS ile) düzenler.
    - Varsayılan olarak, VSS yedeklemeleri tam yedekleme gerçekleştirir.
    - Yedekleme, uygulamayla tutarlı bir anlık görüntü alınamıyor (VM durduğunda herhangi bir uygulama yazma gerçekleşmesi için) sonra temel alınan depolama alanı dosya tutarlı anlık görüntüsünü alır.
1. Linux VM'ler için yedekleme dosyayla tutarlı yedekleme gerçekleştirir. Uygulamayla tutarlı anlık görüntüler için el ile ön/son betik özelleştirmeniz gerekir.
1. Yedek anlık görüntüyü aldıktan sonra veriler kasaya aktarır.
    - Yedekleme, paralel her VM disk yedekleyerek en iyi duruma getirilmiştir.
    - Yedeklenen her bir disk için Azure Backup disk üzerindeki blokları okur ve tanımlar ve (delta) önceki yedeklemeden itibaren değişmiş veri blokları aktarır.
    - Anlık görüntü verileri hemen kasaya kopyalanmaması. Bu, yoğun saatlerde bazı saat sürebilir. Bir VM için toplam yedek süresi 24 saatten daha kısa bir süre için günlük yedekleme ilkelerini olacaktır.
 1. Azure Backup etkinleştirildikten sonra bir Windows VM için yapılan değişiklikler şunlardır:
    -   Microsoft Visual C++ 2013 Redistributable(x64) - 12.0.40660 VM'de yüklü
    -   Başlangıç türünü otomatik olarak el ile değiştirilen Birim Gölge Kopyası Hizmeti (VSS)
    -   IaaSVmProvider Windows hizmeti eklendi

1. Veri aktarımı tamamlandığında anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.

![Azure sanal makine yedekleme mimarisi](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="encryption-of-azure-vm-backups"></a>Azure VM yedekleme şifreleme

Azure sanal makinelerini Azure Backup ile yedekleme, sanal makineleri bekleyen depolama hizmeti şifrelemesi (SSE) ile şifrelenir. Azure Backup ayrıca Azure Disk şifrelemesi kullanılarak şifrelenmiş Azure Vm'leri yedekleyebilir.

**Şifreleme** | **Ayrıntılar** | **Destek**
--- | --- | ---
**Azure Disk şifrelemesi** | Azure Disk şifrelemesi, Azure Vm'leri için hem işletim sistemi ve veri disklerini şifreler.<br/><br/> Azure Disk şifrelemesi olan bir anahtar kasasında gizli dizi olarak korunur BitLocker şifreleme anahtarları (BEKs) ile tümleşir. Azure Disk şifrelemesi, ayrıca Azure Key Vault anahtar şifreleme anahtarları (Dünyaları) ile tümleşir. | Azure Backup, yönetilen ve yönetilmeyen Azure BEKs yalnızca veya BEKs Dünyaları birlikte ile şifrelenmiş VM'ler yedeklemesini destekler.<br/><br/> BEKs hem Dünyaları yedeklenmesini ve şifrelenir.<br/><br/> Dünyaları ve BEKs yedeklenir gerekli izinlere sahip kullanıcılar anahtarları geri yükleyebilirsiniz ve gerekirse anahtar kasasına gizli anahtarları yedekleyin. Bu kullanıcılar da şifrelenmiş sanal Makineyi kurtarabilirsiniz.<br/><br/> Şifreli anahtarları ve gizli anahtarları Azure veya yetkisiz kullanıcılar tarafından okunamaz.
**SSE** | SSE ile Azure depolama, otomatik olarak depolamadan önce verileri şifreleyerek bekleyen şifreleme sağlar. Azure depolama ayrıca almadan önce verilerin şifresini çözer. | Azure Backup SSE, bekleyen şifreleme Azure vm'leri için kullanır.

Yönetilen ve yönetilmeyen Azure Vm'leri için yalnızca BEKs ile şifrelenmiş VM'ler veya BEKs Dünyaları birlikte ile şifrelenmiş VM'ler hem yedeklemeyi destekler.

Yedeklenen BEKs (gizli anahtarlar) ve (anahtarlar) Dünyaları şifrelenir. Bunlar okunabilir ve yalnızca bunlar geri anahtar Kasası'na yetkili kullanıcılar tarafından geri kullanılır. Yetkisiz kullanıcılar ya da Azure okuyup okuyamayacağını veya yedeklenen anahtarlar veya parolalar kullanın.

BEKs ayrıca yedeklenir. Bu nedenle, BEKs kaybolması durumunda, yetkili kullanıcıların BEKs anahtar Kasası'na geri yükleyebilir ve şifrelenmiş Vm'leri kurtarma. Yalnızca gerekli düzeyde izinlere sahip kullanıcılar, yedekleme ve şifrelenmiş VM'ler veya anahtarları ve gizli anahtarları geri yükleyin.

## <a name="snapshot-creation"></a>Anlık görüntü oluşturma

Azure yedekleme anlık görüntüleri yedekleme zamanlamasına göre gerçekleştirir.

- **Windows Vm'leri için:** Windows Vm'leri için yedekleme hizmeti, VSS ile VM disklerini uygulamayla tutarlı bir anlık görüntüsünü almak için düzenler.

  - Varsayılan olarak, Azure Backup, VSS tam yedeklemeler alır. [Daha fazla bilgi edinin](https://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
  - Azure yedekleme VSS kopyalama yedeklemeleri olabilmesi ayarını değiştirmek için bir komut isteminden aşağıdaki kayıt defteri anahtarını ayarlayın:

    **REG "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d TRUE /f Ekle**

- **Linux Vm'leri için:** Linux Vm'leri uygulamayla tutarlı anlık için Linux ön betik kullanma ve tutarlılığını sağlamak için kendi komut dosyalarınızı yazmak için framework sonrası komut dosyası.

  - Azure Backup yalnızca sizin tarafınızdan yazılan ön/son betiklerini çağırır.
  - Öncesi ve sonrası Komut başarıyla çalışırsa, Azure Backup kurtarma noktası uygulamayla tutarlı olarak işaretler. Özel komut dosyaları kullanırken, ancak, uygulama tutarlılığı için sorumlu olursunuz.
  - [Daha fazla bilgi edinin](backup-azure-linux-app-consistent.md) betiklerini yapılandırma hakkında.

### <a name="snapshot-consistency"></a>Anlık görüntü tutarlılık

Aşağıdaki tabloda, anlık görüntü tutarlılık farklı türleri açıklanmaktadır:

**Anlık görüntü** | **Ayrıntılar** | **Kurtarma** | **Önemli noktalar**
--- | --- | --- | ---
**Uygulamayla tutarlı** | Uygulamayla tutarlı yedeklemeler bellek içeriği ve bekleyen g/ç işlemlerini yakalayın. Uygulamayla tutarlı anlık görüntüleri yedekleme gerçekleşmeden önce uygulama veri tutarlılığını sağlamak için VSS Yazıcı'ı (veya Linux için ön/son betikler) kullanın. | Uygulamayla tutarlı bir anlık görüntü ile bir VM kurtarma gerçekleştiriyorsanız, VM yedekleme önyüklenir. Veri bozulmasına veya kaybına yoktur. Uygulamaları tutarlı bir durumda başlatın. | Windows: Tüm VSS yazıcılarının başarılı oldu<br/><br/> Linux: Ön/son betik başarılı oldu ve yapılandırılır
**Dosya sistemiyle tutarlı** | Dosya sistemi tutarlı yedekler, aynı anda tüm dosyaları anlık görüntüsünü alarak tutarlılık sağlar.<br/><br/> | Dosya sistemi ile tutarlı bir anlık görüntü ile bir VM kurtarma gerçekleştiriyorsanız, VM yedekleme önyüklenir. Veri bozulmasına veya kaybına yoktur. Geri yüklenen verilerin tutarlı olduğundan emin olmak için kendi "yukarı düzeltme" mekanizması uygulamak uygulamaları gerekir. | Windows: Bazı VSS yazıcılarının başarısız oldu <br/><br/> Linux: (Ön/son betik yapılandırılmış başarısız oldu veya yararsız) varsayılan
**Kilitlenmeyle tutarlı** | Kilitlenme ile tutarlı anlık görüntüler, genellikle bir Azure VM yedekleme sırasında kapanırsa oluşur. Zaten diskte yedekleme sırasında var olan veriler yakalanır ve yedeklendi.<br/><br/> Kilitlenmeyle tutarlı kurtarma noktası işletim sistemi veya uygulama için veri tutarlılığı garanti etmez. | Garanti olsa da, VM genellikle önyükler ve ardından bozulma hatalarını düzeltmek için bir disk denetimi başlatır. Herhangi bir bellek içi verileri veya transfer olmayan yazma işlemleri kayıp kilitlenme önce diske. Uygulama kendi veri doğrulaması uygular. Örneğin, bir veritabanı uygulama, işlem günlüğü doğrulama için kullanabilirsiniz. İşlem günlüğü veritabanında olmayan girişler varsa, veritabanı yazılımına verileri yeniden tutarlı olana kadar işlem yapar. | VM kapatma durumdadır

## <a name="backup-and-restore-considerations"></a>Yedekleme ve geri yükleme konuları

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Disk** | VM diskleri yedeğini paraleldir. Örneğin, VM dört disk varsa, tüm dört diskleri paralel yedeklemek Backup hizmeti çalışır. (Verilerin değiştirilmesi yalnızca) artımlı yedeklemedir.
**Zamanlama** |  Yedekleme trafiğini azaltmak için günün farklı zamanlarında farklı Vm'lerini yedekleme ve saatleri çakışmadığından emin olun. Aynı anda VM'lerin yedeklenmesi trafik sıkışıklıklarının neden olur.
**Yedekleri hazırlanıyor** | Yedekleme hazırlamak için gereken süreyi göz önünde bulundurun. Hazırlık süresi, yükleme veya güncelleştirme yedekleme uzantısı ve anlık görüntü yedekleme zamanlamaya göre tetikleme içerir.
**Veri aktarımı** | Önceki yedeklemeden artımlı değişiklikleri belirlemek Azure yedekleme için gereken süreyi göz önünde bulundurun.<br/><br/> Artımlı bir yedeklemede bulunan Azure Backup değişiklikleri blok sağlama toplamı hesaplama tarafından belirler. Bir blok değiştirilirse, kasaya aktarımı için işaretlenir. Hizmet, daha fazla veri aktarmak için en aza denemek için tanımlanan blokları analiz eder. Değiştirilen blokları değerlendirdikten sonra Azure Backup vault'a değişiklikleri aktarır.<br/><br/> Anlık görüntü alma ve kasaya kopyalayarak arasında bir gecikme olabilir.<br/><br/> Yoğun saatlerde uygulamanın işlenecek yedeklemeler için sekiz saate kadar sürebilir. Bir VM için yedekleme zamanı günlük yedekleme için 24 saatten az olacaktır.
**İlk yedekleme** | Artımlı yedeklemeler için toplam yedekleme zamanını 24 saatten az olsa da, bu ilk yedekleme için durum geçerli olmayabilir. İlk yedekleme için gereken süre, verilerin ve yedekleme işlendiğinde boyutuna bağlıdır.
**Geri Yükleme sırası** | Azure yedekleme işleri birden fazla depolama hesaplarından aynı anda geri yükleme işlemleri ve geri yükleme isteği sıraya koyar.
**Kopyalama geri yükleme** | Geri yükleme işlemi sırasında verileri kasadan depolama hesabına kopyalanır.<br/><br/> G/ç işlemi (IOPS) ve aktarım hızı depolama hesabının toplam geri yükleme saatine bağlıdır.<br/><br/> Kopyalama süreyi azaltmak üzere diğer uygulama yazar ve okur ile yüklü olmayan bir depolama hesabı seçin.

### <a name="backup-performance"></a>Yedekleme performansı

Bu senaryoyu toplam yedekleme süresini etkileyebilir:

- **Korunan bir Azure sanal makine için yeni bir disk ekleme:** Bir VM artımlı yedekleme yapılıyor ve yeni disk eklendiğinde, yedekleme süresini artırır. Toplam yedekleme zamanını 24 saatten fazla yeni disk, mevcut disk değişim çoğaltması ile birlikte ilk çoğaltması nedeniyle son.
- **Parçalanmış diskler:** Yedekleme işlemleri disk değişiklikleri bitişik olduğunda daha hızlıdır. Değişiklikleri dağılmış ve bir disk arasında parçalanmış, yedekleme daha yavaş olur.
- **Disk değişim sıklığı:** Artımlı yedekleme aşamasında diskleri korumalı bir günlük değişim sıklığı 200 GB'tan fazla olması, yedekleme tamamlanması uzun zaman (sekiz saatten fazla) sürebilir.
- **Yedekleme sürümleri:** En son sürümünü (anlık geri sürüm olarak bilinir) yedekleme değişiklikleri tanımlamak için sağlama toplamı karşılaştırma daha fazla en iyi duruma getirilmiş bir işlem kullanır. Ancak, anlık geri kullanıyorsanız ve bir yedek anlık görüntüsü silinmiş durumunda yedekleme sağlama toplamı ile karşılaştırma geçer. Bu durumda, yedekleme işlemi 24 saat (veya başarısız).

## <a name="best-practices"></a>En iyi uygulamalar

VM yedeklemeleri yapılandırırken bu yöntemler öneririz:

- İlke kümesi varsayılan zamanlama sürelerinin değiştirin. İlkede varsayılan saat 12: 00'da ise, kaynakları en iyi şekilde kullanılır, böylece Örneğin, birkaç dakika zamanlama artırın.
- Premium depolama kullanan bir VM yedeklemesi için Azure Backup'ın en son sürümünü çalıştıran olan öneririz ([anında geri yükleme](backup-instant-restore-capability.md)). En son sürümünü çalıştırmıyorsanız, yedekleme yaklaşık yüzde 50'sini toplam depolama alanı ayırır. Backup hizmeti anlık görüntüyü aynı depolama hesabını ve Kasası'na aktarılması kopyalamak için bu alanı gerektirir.
- Tek bir kasadan Vm'leri geri, farklı kullanmanızı öneririz [genel amaçlı v2 depolama hesaplarının](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) hedef depolama hesabı kısıtlanmazsınız değil emin olmak için. Örneğin, her sanal makine farklı bir depolama hesabı olmalıdır. Örneğin, 10 VM geri yüklediyseniz, 10 farklı depolama hesabı kullanın.
- Anlık görüntü aynı depolama hesabında olduğundan bir genel amaçlı v1 depolama katmanından (snapshot) geri yükleme birkaç dakika içinde tamamlanır. Genel amaçlı v2 depolama katmanından (kasa) geri yüklemeler saat sürebilir. Kullanmanızı tavsiye ederiz verileri nerede genel amaçlı v1 depolama alanında kullanılabilir durumda [anında geri yükleme](backup-instant-restore-capability.md) daha hızlı geri yüklemeler için özellik. (Bir kasadan veri geri yüklenmesi gerekiyorsa, ardından daha uzun sürer.)
- Depolama hesabı başına disk sayısı ne kadar yoğun olarak diskler üzerinde bir altyapı (Iaas) sanal makine hizmet olarak çalışan uygulamaları tarafından erişildiği göre sınırlıdır. 5-10 diskleri veya daha fazla tek bir depolama hesabında mevcut olması durumunda, genel bir yöntem olarak, depolama hesabı ayırmak için bazı diskler taşıyarak Yük Dengelemesi yapın.

## <a name="backup-costs"></a>Yedekleme maliyetleri

Azure Vm'leri Azure Backup ile yedeklenmekte olan konusu [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).

Faturalandırma, ilk başarılı yedekleme tamamlanana kadar başlamıyor. Bu noktada, hem depolama hem de korumalı VM'ler için faturalandırma başlar. Tüm yedekleme verileri VM için bir kasada depolanır olduğu sürece, faturalandırma devam eder. Bir VM için korumayı durdurun, ancak VM için yedekleme verileri bir kasada var ise, faturalandırma devam eder.

Belirtilen bir VM için fatura bilgilerini yalnızca koruma durduruldu ve tüm yedekleme verileri silinir durdurur. Koruma durduğunda ve yedekleme etkin iş yok, en son başarılı VM yedekleme boyutu için aylık fatura kullanılan korumalı örnek boyutu haline gelir.

Korumalı örnek boyutu hesaplaması dayanır *gerçek* VM boyutu. Sanal makinenin boyutu VM'deki geçici depolama hariç tüm verilerin toplamıdır. Fiyatlandırma veri disklerinde depolanan, üst sınırı üzerinde değil VM'ye bağlı her veri diski boyutu desteklenen gerçek verileri temel alır.

Benzer şekilde, Azure Yedekleme'de her kurtarma noktası gerçek verileri toplamı olduğu depolanan veri miktarı yedekleme alanı faturada dayanır.

Örneğin, iki ek veri diskleri en fazla 4 TB olan A2-standart boyutlu bir VM yararlanın. Aşağıdaki tabloda bu disklerin her biri üzerinde depolanan gerçek veriler gösterilmektedir:

**Disk** | **En büyük boyutu** | **Gerçek veri yok**
--- | --- | ---
İşletim sistemi diski | 4095 GB | 17 GB
Yerel/geçici disk | 135 GB | 5 GB (yedekleme için dahil değil)
Veri diski 1 | 4095 GB | 30 GB
Veri diski 2 | 4095 GB | 0 GB

Gerçek VM boyutuna, 17 GB + 30 GB + 0 GB = 47 GB bu durumda olur. Bu korumalı örnek boyutu (GB 47) başına aylık fatura olur. Sanal makine içindeki veri miktarı büyüdükçe, korumalı örnek boyutu için faturalandırma değişiklikleri eşleştirmek için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, [Azure VM yedeklemesi için hazırlama](backup-azure-arm-vms-prepare.md).
