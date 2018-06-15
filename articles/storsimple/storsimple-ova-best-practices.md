---
title: En iyi uygulamalar için StorSimple sanal dizisi | Microsoft Docs
description: Dağıtma ve StorSimple sanal dizinin yönetmeye yönelik en iyi uygulamaları açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 57ac6eeb-c47c-442d-a5f4-b360d81a76a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/16/2018
ms.author: alkohli
ms.openlocfilehash: 46fd818d8ca15515c91bb6e65e99b0a3bc1f1fa4
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
ms.locfileid: "29972849"
---
# <a name="storsimple-virtual-array-best-practices"></a>StorSimple sanal dizinin en iyi uygulamalar
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple sanal dizinin bir hiper yönetici ve Microsoft Azure bulut depolama çalıştıran bir şirket içi sanal aygıt arasında depolama görevleri yöneten bir tümleşik depolama çözümüdür. StorSimple sanal dizinin 8000 serisi fiziksel dizisine verimli ve ekonomik bir alternatiftir. Sanal dizinin mevcut hiper yönetici altyapınızda çalıştırabilirsiniz, iSCSI ve SMB protokollerini destekler ve uzak office/şube ofisi senaryolarında için de çok uygundur. StorSimple çözümleri hakkında daha fazla bilgi için Git [Microsoft Azure StorSimple genel bakış](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Bu makalede, ilk Kurulum, dağıtımı ve Yönetimi StorSimple sanal dizinin sırasında uygulanan en iyi yöntemleri kapsar. Bu en iyi uygulamaları için Kurulum ve Yönetim sanal dizinin doğrulanmış yönergeleri sağlar. Bu makalede, dağıtma ve yönetme, veri merkezlerini sanal dizilerde BT yöneticileri doğru yöneliktir.

Kurulum veya işlem akışına değişiklik yapıldığında, cihazınız hala uyumlu olup sağlamaya yardımcı olmak için en iyi uygulamaları periyodik olarak İnceleme öneririz. Karşılaştığınız sorunları sanal dizinizi üzerinde uygulama çalışırken bu en iyi yöntemler [Microsoft Support başvurun](storsimple-virtual-array-log-support-ticket.md) Yardım için.

## <a name="configuration-best-practices"></a>Yapılandırma en iyi uygulamalar
Bu en iyi uygulamaları sanal diziler dağıtımını ve ilk kurulum sırasında izlenmesi gereken yönergeleri içerir. Boyutlandırma, ağı kurma, depolama hesaplarını yapılandırma ve paylaşımları ve birimler sanal dizini oluşturma Grup İlkesi ayarları, sanal makine sağlamak için ilgili bu en iyi uygulamaları içerir. 

### <a name="provisioning"></a>Sağlama
StorSimple sanal (Hyper-V veya VMware) hiper yöneticide sağlanan sanal makine (VM), ana bilgisayar sunucusu dizidir. Sanal makine sağlanırken, ana bilgisayarınız yeterli kaynak ayırması mümkün olduğundan emin olun. Daha fazla bilgi için Git [düşük kaynak gereksinimlerini](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) bir dizi sağlayacak.

Sanal dizinin sağlamada aşağıdaki en iyi yöntemleri uygulayın:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Sanal makine türü** |**2. nesil** Windows Server 2012 veya daha sonra kullanmak için VM ve *.vhdx* görüntü. <br></br> **1. nesil** VM için bir Windows Server 2008 veya sonraki sürümünü kullanın ve *.vhd* görüntü. |Kullanım sanal makine sürüm kullanırken 8 *.vmdk* görüntü. |
| **Bellek türü** |Olarak yapılandırma **statik bellek**. <br></br> Kullanmayın **dinamik bellek** seçeneği. | |
| **Veri disk türü** |Olarak sağlamak **dinamik olarak genişletilen**.<br></br> **Boyutu sabit** uzun sürüyor. <br></br> Kullanmayın **fark kayıt** seçeneği. |Kullanım **ince provizyon** seçeneği. |
| **Veri diski değişikliği** |Genişletme veya küçültme izin verilmiyor. Bunu yapmak için girişiminde cihazdaki tüm yerel verilerin kaybedilmesine sonuçlanır. |Genişletme veya küçültme izin verilmiyor. Bunu yapmak için girişiminde cihazdaki tüm yerel verilerin kaybedilmesine sonuçlanır. |

### <a name="sizing"></a>Boyutlandırma
StorSimple sanal dizinizi boyutlandırma yaparken, aşağıdaki etmenleri dikkate alın:

* Birimler veya paylaşımlar için yerel ayırma. Alanı yaklaşık % 12 yerel katman her sağlanan katmanlı birim veya paylaşım için ayrılmıştır. Kabaca alanın % 10 da yerel olarak sabitlenmiş bir birim dosya sistemi için ayrılmış.
* Ek yükü anlık görüntüsünü alın. Kabaca % 15 alanı yerel katman anlık görüntüler için ayrılmıştır.
* Geri yüklemeler için gerekli. Geri yükleme yeni bir işlem olarak yapılması boyutlandırma geri yüklemek için gerekli alan için hesap. Geri yükleme, bir paylaşımı veya birimi aynı boyutta yapılır.
* Bazı arabellek herhangi beklenmeyen büyüme için ayrılmalıdır.

Önceki etkenlere bağlı olarak, boyutlandırma gereksinimleri deneylerin gösterilebilir:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

Aşağıdaki örnekler, gereksinimlerinize göre sanal bir dizi nasıl boyutunu gösterir.

#### <a name="example-1"></a>Örnek 1:
Sanal dizinizi üzerinde için kullanabilmek ister

* 2 TB sağlamak katmanlı birim veya paylaşım.
* 1 TB sağlamak katmanlı birim veya paylaşım.
* yerel olarak sabitlenmiş birimin 300 GB sağlamak ya da paylaşabilirsiniz.

Bize önceki birimler veya paylaşımlar için yerel katman alanı gereksinimlerini hesaplamak.

İlk olarak, her katmanlı birim/paylaşımı için yerel ayırma birimi/paylaşım boyutu % 12 yüzdesi eşit olacaktır. Yerel olarak sabitlenmiş birim/paylaşımı için yerel ayırma (ek olarak sağlanan boyut) yerel olarak sabitlenmiş birim/paylaşma boyutunun % 10'dur. Bu örnekte, gerekir

* 240 GB yerel ayırma (için 2 TB katmanlı birim/paylaşım)
* 120 GB yerel ayırma (1 TB katmanlı birim/paylaşma)
* Yerel olarak sabitlenmiş bir birim veya paylaşım (yerel ayırma % 10 sağlanan 300 GB boyutunu ekleme) için 330 GB

Yerel katmanında kadarki gereken toplam alan: 240 GB + 120 GB + 330 GB = 690 GB.

İkincisi, en az olarak kadar alan yerel katman en büyük tek ayırma ihtiyacımız var. Bir bulut anlık görüntüden geri yüklemeniz durumunda bu ek miktar kullanılır. Bu örnekte, en büyük yerel ayırma 330 (dosya sistemi için ayırma dahil), GB'dir, eklediğiniz şekilde 690 GB: 690 GB + 330 GB = 1020 GB.
Biz sonraki ek geri yükleme gerçekleştirdiyseniz, biz her zaman önceki geri yükleme işlemi yer boşaltabilirsiniz.

Bunu yalnızca % 85 kullanılabilir olmasını sağlamak üçüncü, o ana kadarki yerel anlık görüntüleri saklamak için toplam yerel alanınızı % 15 ihtiyacımız var. Bu örnekte, olacaktır geçici 1020 GB = 0.85&ast;sağlanan veri TB disk. Bu nedenle, sağlanan veri diski olacaktır (1020&ast;(1/0.85)) 1200 GB = 1.20 TB = ~ 1,25 (en yakın DÖRTTEBİRLİK yuvarlama) TB

Beklenmeyen büyüme ve yeni geri yüklemeler Finansman, geçici bir yerel disk sağlamanız 1,25 - 1, 5 TB.

> [!NOTE]
> Yerel disk ölçülü kaynak öneririz. Bu öneri, beş günden eski olan verileri geri yüklemek istediğinizde geri yükleme alanı yalnızca gerekli olmasıdır. Öğe düzeyinde kurtarma verileri geri yüklemek için ek boşluk gerektirmeden son beş gün boyunca geri yüklemenize olanak sağlar.


#### <a name="example-2"></a>Örnek 2:
Sanal dizinizi üzerinde için kullanabilmek ister

* 2 TB sağlamak katmanlı birim
* yerel olarak sabitlenmiş 300 GB birimi sağlamak

İhtiyacımız katmanlı birimlerin/paylaşımları için yerel alanı ayırma ve yerel olarak sabitlenmiş birimleri/paylaşımları için % %10 12 bağlı olarak,

* 240 GB yerel ayırma (2 TB katmanlı birim/paylaşım)
* Yerel olarak sabitlenmiş bir birim veya paylaşım (yerel ayırma % 10 sağlanan 300 GB alanı ekleme) için 330 GB

Yerel katmanında gerekli toplam alan: 240 GB + 330 GB = 570 GB

Geri yükleme için gereken en düşük yerel alan 330 GB'dir.

Toplam disk % 15, böylece yalnızca 0.85 kullanılabilir anlık görüntülerini depolamak için kullanılır. Bu nedenle, disk boyutudur (900&ast;(1/0.85)) 1.06 TB = ~ 1,25 (en yakın DÖRTTEBİRLİK yuvarlama) TB

Beklenmeyen bir artışa Finansman, yerel bir 1,25-1,5 TB disk sağlayabilirsiniz.

### <a name="group-policy"></a>Grup ilkesi
Grup İlkesi kullanıcılar ve bilgisayarlar için belirli yapılandırmaları uygulamak izin veren bir altyapıdır. Grup İlkesi ayarları, aşağıdaki Active Directory etki alanı Hizmetleri (AD DS) kapsayıcılara bağlı Grup İlkesi nesneleri (GPO'lar) içerdiği: siteleri, etki alanları veya kuruluş birimlerini (OU). 

Sanal dizinizi etki alanına katılmış ise, GPO'ları uygulanabilir. Bu GPO'ları StorSimple sanal dizinin işlemi olumsuz yönde etkileyebilir bir virüsten koruma yazılımı gibi uygulamaları yükleyebilirsiniz.

Bu nedenle, öneririz:

* Sanal dizinizi Active Directory için kendi kuruluş biriminde (OU) olduğundan emin olun.
* Hiçbir Grup İlkesi nesneleri (GPO'lar) sanal dizinizi uygulanan emin olun. Sanal dizinin (alt düğümü) otomatik olarak herhangi bir GPO üst devralmadığından emin olmak için devralmayı engelleyebilirsiniz. Daha fazla bilgi için Git [engelleme devralma](https://technet.microsoft.com/library/cc731076.aspx).

### <a name="networking"></a>Ağ
Sanal dizinizi için ağ yapılandırmasının yerel web kullanıcı Arabirimi gerçekleştirilir. Bir sanal ağ arabirimi sanal dizinin sağlandı hiper yönetici etkinleştirilir. Kullanım [ağ ayarlarını](storsimple-virtual-array-deploy3-fs-setup.md) sayfasını sanal ağ arabirimi IP adresi, alt ağ ve ağ geçidi yapılandırmak için.  Cihazınız için birincil ve ikincil DNS sunucusu, saat ayarlarını ve isteğe bağlı proxy ayarlarını da yapılandırabilirsiniz. Çoğu ağ yapılandırması, bir kerelik Kurulum olur. Gözden geçirme [ağ gereksinimleri StorSimple](storsimple-ova-system-requirements.md#networking-requirements) sanal dizinin dağıtmadan önce.

Sanal dizinizi dağıtırken bu en iyi uygulamaları izlemeniz önerilir:

* Sanal dizinin her zaman dağıtılan ağ 5 MB/sn Internet bant genişliği (veya daha fazla) ayrılması kapasiteye sahip olduğundan emin olun.
  
  * Internet bant genişliği gereksinimi, iş yükü özellikleri ve veri değişim oranı bağlı olarak değişir.
  * İşlenebilir veri değişikliği için Internet bant orantılıdır. Bir yedekleme duruma getirirken örnek olarak, 5 MB/sn bant genişliği 18 GB yaklaşık 8 saat içindeki bir veri değişikliği barındırabilir. Dört kez daha fazla bant genişliği ile (20 MB/sn), dört kat daha fazla veri değişikliği (72 GB) işleyebilir.
* Internet bağlantısı her zaman kullanılabilir olduğundan emin olun. Aralıklı ya da güvenilir olmayan Internet bağlantıları aygıtlara bulutta verilere erişim kaybına neden olabilir ve desteklenmeyen bir yapılandırmada neden olabilir.
* Cihazınızı bir iSCSI sunucusu dağıtmayı planlıyorsanız:
  
  * Devre dışı bırakmanızı öneririz **IP adresini otomatik olarak almak** seçeneği (DHCP).
  * Statik IP adreslerini yapılandırın. Birincil ve ikincil bir DNS sunucusu yapılandırmanız gerekir.
  * Birden çok ağ arabirimi, sanal üzerinde tanımlanıyorsa dizi, yalnızca ilk ağ arabirimi (varsayılan olarak, bu arabirimidir **Ethernet**) bulut ulaşabilirsiniz. Trafik türlerini denetlemek için birden çok sanal ağ arabirimi (iSCSI sunucusu olarak yapılandırılmış) sanal dizinizi oluşturmak ve bu arabirimleri farklı alt ağlara bağlanmak.
* Yalnızca bulut bant genişliğini azaltmak için (kullanılan sanal dizisi tarafından), yönlendirici veya güvenlik duvarı azaltma yapılandırın. Hiper yöneticide azaltma tanımlarsanız, iSCSI ve SMB yalnızca bulut bant genişliği yerine dahil olmak üzere tüm protokoller kısıtlama.
* Hiper yöneticilerin etkin o zaman eşitleme emin olun. Hyper-V Yöneticisi'nde sanal dizinizi Hyper-V kullanarak seçeneğini belirlerseniz, Git **ayarları &gt; Integration Services**ve emin **zaman eşitleme** denetlenir.

### <a name="storage-accounts"></a>Depolama hesapları
StorSimple sanal dizinin tek bir depolama hesabıyla ilişkili olabilir. Bu depolama hesabının otomatik olarak oluşturulan depolama hesabı, hesap hizmeti ile aynı abonelikte olabilir veya başka bir abonelik için bir depolama hesabıyla ilgili. Daha fazla bilgi için bkz: nasıl yapılır [sanal dizinizi depolama hesaplarını yönetme](storsimple-virtual-array-manage-storage-accounts.md).

Sanal dizinizi ile ilişkili depolama hesapları için aşağıdaki önerileri kullanın.

* Sanal bir dizi ve bir depolama hesabı için maksimum boyut (500 TB) için en fazla kapasite (64 TB) tek bir depolama hesabına birden çok sanal dizilerle bağlanırken öğeli. Bu, yaklaşık 7, depolama hesabıyla ilişkilendirilebilir tam boyutlu sanal diziler sayısını sınırlar.
* Yeni bir depolama hesabı oluştururken
  
  * Bu bölgede en yakın gecikmelerini en aza indirmek için StorSimple sanal dizinizi dağıtıldığı uzak office/şube ofisi oluşturmanızı öneririz.
  * Farklı bölgelerdeki depolama hesabı taşıyamazsınız aklınızda size aittir. Ayrıca bir hizmet abonelikler arasında taşınamaz.
  * Veri merkezleri arasında artıklık uygulayan bir depolama hesabı kullanın. Coğrafi olarak yedekli depolama (GRS), bölge olarak yedekli depolama (ZRS) ve yerel olarak yedekli depolama (LRS) tüm sanal dizinizi ile kullanım için desteklenir. Farklı türlerde depolama hesapları hakkında daha fazla bilgi için Git [Azure storage çoğaltma](../storage/common/storage-redundancy.md).

### <a name="shares-and-volumes"></a>Paylaşımları ve birimler
StorSimple sanal dizinizi için bir dosya sunucusu ve bir iSCSI sunucusu olarak yapılandırıldığında birimleri olarak yapılandırıldığında paylaşımları sağlayabilirsiniz. Paylaşımları ve birimler oluşturmak için en iyi uygulamalar, boyutu ve yapılandırılmış türü ilişkilendirilir.

#### <a name="volumeshare-size"></a>Birim/paylaşımı
Bir dosya sunucusu ve bir iSCSI sunucusu olarak yapılandırıldığında birimleri olarak yapılandırıldığında, sanal dizinizi üzerinde paylaşımları sağlayabilirsiniz. Paylaşımları ve birimler oluşturmak için en iyi uygulamalar, boyutu ve yapılandırılmış türü ilişkilendirebilirsiniz. 

Aşağıdaki en iyi yöntemleri paylaşımları veya birimler, sanal Cihazınızda sağlamada unutmayın.

* Katmanlı paylaşımının sağlanan boyutuna göre dosya boyutları katmanlama performansı etkileyebilir. Büyük dosyaları ile çalışma yavaş katmanı genişletmek neden olabilir. Büyük dosyaları ile çalışırken en büyük dosya paylaşımı boyutunu %3 küçüktür öneririz.
* En fazla 16 birimleri/paylaşımları Sanal dizi oluşturulabilir. Yerel olarak sabitlenmiş ve katmanlı birimlerin/paylaşımları boyutu sınırları için her zaman başvurmak [StorSimple sanal dizinin sınırlar](storsimple-ova-limits.md).
* Bir birim oluştururken, beklenen veri tüketim yanı sıra gelecekteki büyüme faktörü. Birim daha sonra genişletilemiyor.
* Birim oluşturulduktan sonra StorSimple biriminin boyutunu küçültülemez.
* GÇ birim verilerini (göreli birimi için ayrılan yerel alan) belirli bir eşiğe ulaştığında bir katmanlı birim StorSimple üzerinde yazılırken kısıtlanır. Bu birime yazmaya devam GÇ önemli ölçüde yavaşlatır. Katmanlı birim kapasitesinin (biz etkin olarak sağlanan kapasite ötesinde yazma kullanıcıdan durdurmaz) sağlanan yazabilirler olsa, talep etkili olması için bir uyarı bildirimine bakın. Uyarıyı gördüğünüzde, bu birimdeki verileri silme gibi düzeltici önlemleri almasını zorunludur (Birim genişletme şu anda desteklenmiyor).
* Olağanüstü durum kurtarma kullanım durumları için izin verilen paylaşımları/birim sayısı 16'dır ve en fazla sayıda paralel olarak işlenebilir paylaşımları/birime ayrıca 16, paylaşımları/birimlerin sayısı, şifrelemeyle RPO ve Rto'lara yok.

#### <a name="volumeshare-type"></a>Birim/paylaşım türü
StorSimple kullanımına bağlı iki birim/paylaşma türlerini destekler: yerel olarak sabitlenmiş ve katmanlı. Katmanlı birimlerin/paylaşımları ölçülü kaynak ancak yerel olarak sabitlenmiş birimleri/paylaşımları sıkı sağlanır. Yerel olarak sabitlenmiş bir birim/paylaşımı dönüştürülemiyor çok katmanlı veya *tersi* oluşturulduktan sonra.

StorSimple birimlerini/paylaşımları yapılandırırken aşağıdaki en iyi yöntemleri uygulayın öneririz:

* Bir birim oluşturmadan önce dağıtmayı planladığınız iş yükleri üzerinde temel birim türünü tanımlayın. Yerel olarak sabitlenmiş birimlerin (hatta sırasında bulut kesinti) veri yerel GARANTİLERİN gerektiren ve düşük bulut gecikme süreleri gerektiren iş yükleri için kullanın. Bir birim üzerinde sanal dizinizi oluşturduktan sonra birim türünü değiştiremezsiniz gelen yerel olarak çok katmanlı sabitlenmiş veya *tam tersini*. Örnek olarak, yerel olarak sabitlenmiş birimlerin SQL iş yükleri veya iş yükleri barındıran sanal makineleri (VM'ler); dağıtırken oluşturma Katmanlı birimler, dosya paylaşımı iş yükleri için kullanın.
* Daha az sık kullanılan arşiv verileri için seçeneği büyük dosya boyutlarına sahip ilgilenirken denetleyin. Bulut için veri aktarımını hızlandırmak için bu seçeneği etkin olduğunda daha büyük bir yinelenenleri kaldırma öbek boyutu 512 k kullanılır.

#### <a name="volume-format"></a>Birim biçimi
StorSimple birimlerini iSCSI sunucunuzda oluşturduktan sonra başlatma, bağlama ve birimleri biçimlendirme gerekir. Bu işlem, StorSimple Cihazınızı bağlı ana bilgisayar üzerinde gerçekleştirilir. Aşağıdaki en iyi uygulamaları oluşturma ve StorSimple konaktaki birimleri biçimlendirme önerilir.

* Hızlı biçimlendirme tüm StorSimple birimlerde gerçekleştirin.
* Bir StorSimple biriminin biçimlendirme sırasında 64 KB ayırma birimi boyutu (Avustralya) kullanın (varsayılan değer 4 KB). Avustralya 64 KB ortak StorSimple iş yükleri ve diğer iş yükleri için şirket içinde yapılır testlerine dayanır.
* StorSimple sanal bir iSCSI sunucusu olarak yapılandırılmış dizi kullanırken, Dağıtılmış birimler veya dinamik diskleri bu birimleri kullanmayın veya diskler StorSimple tarafından desteklenmez.

#### <a name="share-access"></a>Paylaşım erişimi
Sanal dizinin dosya sunucunuzda paylaşımları oluştururken, aşağıdaki yönergeleri izleyin:

* Bir paylaşımı oluştururken, tek bir kullanıcı yerine bir paylaşım yönetici olarak bir kullanıcı grubuna atayın.
* Paylaşım paylaşımları Windows Gezgini üzerinden düzenleyerek oluşturulduktan sonra NTFS izinlerini yönetebilir.

#### <a name="volume-access"></a>Birim erişimi
İSCSI birimleri, StorSimple sanal dizisinde yapılandırırken, gerekli olan her yerde erişimi denetlemek önemlidir. Hangi ana bilgisayar sunucuları birimlere erişebilirsiniz belirlemek için oluşturabilir ve erişim denetimi kayıtları (ACRs) StorSimple birimleri ile ilişkilendirebilirsiniz.

ACRs StorSimple birimlerini yapılandırırken aşağıdaki en iyi yöntemleri kullanın:

* Her zaman en az bir ACR bir birimle ilişkilendirin.

* Birden fazla ACR bir birime atarken birim Burada, aynı anda birden fazla kümelenmemiş ana bilgisayar tarafından erişilebilir bir biçimde gösterilmez emin olun. Bir birim için birden çok ACRs atanmışsa bir uyarı iletisi, yapılandırmanızı gözden geçirmeniz için açılır.

### <a name="data-security-and-encryption"></a>Veri güvenliği ve şifreleme
StorSimple sanal dizinizi gizlilik ve veri bütünlüğünü sağlamak veri güvenlik ve şifreleme özelliklere sahiptir. Bu özellikler kullanırken, bu en iyi uygulamaları izlemeniz önerilir: 

* AES 256 şifrelemesi verileri buluta sanal diziden gönderilmeden önce oluşturmak için bir bulut depolama şifreleme anahtarı tanımlayın. Verilerinizi başından itibaren şifrelenir, bu anahtar gerekli değildir. Anahtar oluşturulur ve anahtar yönetimi sistemi kullanarak güvenli kalmasını [Azure anahtar kasası](../key-vault/key-vault-whatis.md).
* StorSimple Yöneticisi hizmeti üzerinden depolama hesabı yapılandırırken, StorSimple cihazınız ve bulut arasındaki ağ iletişimi için güvenli bir kanal oluşturmak SSL modu etkinleştirdiğinizden emin olun.
* Depolama hesaplarınızı tuşları (Azure Storage hizmetine erişerek) düzenli aralıklarla hesabına erişmek değişiklikler Yöneticiler değiştirilen listesine göre yeniden oluşturun.
* Sanal dizinizi verileri sıkıştırılır ve Azure'a gönderilmeden önce yinelenenleri kaldırılmış. Yinelenen verileri kaldırma rol hizmetini Windows Server ana bilgisayarında kullanmanızı öneririz yok.

## <a name="operational-best-practices"></a>İşletimsel en iyi uygulamalar
İşletimsel en iyi yöntemler günlük yönetimi ve sanal dizinin işlemi sırasında izlenmesi gereken yönergelerdir. Bu yöntemler bir yük devri gerçekleştirmek, bir yedekleme kümesinden geri yedek almak gibi belirli yönetim görevlerini kapsar, devre dışı bırakılması ve sistem kullanımı ve sistem durumu izleme ve virüs çalışan dizinin silinmesi sanal dizinizi tarar.

### <a name="backups"></a>Yedeklemeler
Sanal dizinizi verileri iki yolla buluta yedeklendiğinden, varsayılan Otomatik günlük yedekleme 22:30 veya el ile talep üzerine yedekleme ile başlayan tüm cihaz. Varsayılan olarak, cihaz üzerinde bulunan tüm verilerin günlük bulut anlık görüntüleri otomatik olarak oluşturur. Daha fazla bilgi için Git [StorSimple sanal dizinizi yedekleme](storsimple-virtual-array-backup.md).

Varsayılan yedekleme ile ilişkili bekletme ve sıklığı değiştirilemez, ancak her gün başlatılan günlük yedekleri zaman yapılandırabilirsiniz. Otomatik yedeklemeler için başlangıç saatini yapılandırırken öneririz:

* Yedeklemeleriniz için yoğun olmayan saatler zamanlayın. Yedekleme başlangıç zamanı çok sayıda konak g/ç ile çakıştığı değil.
* El ile talep üzerine yedekleme aygıtı yük devretme veya sanal dizinizi üzerindeki verileri korumak için bakım penceresi öncesinde gerçekleştirmek planlama yaparken başlatır.

### <a name="restore"></a>Geri Yükleme
İki şekilde ayarlanmış bir yedekten geri yükleyebilirsiniz: başka bir birime veya paylaşıma geri yükleyin veya (yalnızca dosya sunucusu olarak yapılandırılmış bir sanal dizisinde kullanılabilir) bir öğe düzeyinde kurtarma gerçekleştirin. Öğe düzeyinde kurtarma StorSimple cihazında tüm paylaşımlar, bir bulut yedeğinden dosya ve klasörlerin ayrıntılı kurtarma yapmanıza izin verir. Daha fazla bilgi için Git [bir yedekten geri yükleme](storsimple-virtual-array-clone.md).

Bir geri yükleme gerçekleştirirken, aşağıdaki yönergeleri göz önünde bulundurun:

* StorSimple sanal dizinizi yerinde geri yükleme işlemlerini desteklemiyor. Ancak bu yedeğe olabilir iki adımlı bir işlem tarafından elde: dizi ve başka bir birim/paylaşımına geri yükle alanı sanal olun.
* Yerel bir birimden geri tutmak göz önünde geri yükleme uzun süren bir işlem olacaktır. Birim hızlı bir şekilde çevrimiçi gelebilir de, veri arka planda hydrated devam eder.
* Birim türü geri yükleme işlemi sırasında aynı kalır. Katmanlı birim için başka bir katmanlı birim geri ve yerel olarak sabitlenmiş bir birim yerel olarak başka bir birim sabitlenir.
* Bir birim veya bir paylaşım bir yedekten geri yüklemeyi denerken ayarlayın, geri yükleme işi başarısız olursa, bir hedef birim veya paylaşım hala portalda oluşturulabilir. Bu kullanılmayan hedef birimde silin veya bu öğesinden doğan gelecekteki sorunları en aza indirmek için portalda paylaşmak önemlidir.

### <a name="failover-and-disaster-recovery"></a>Yük devretme ve olağanüstü durum kurtarma
Cihaz yük devretme, verilerinizden geçirmenize olanak sağlar bir *kaynak* başka bir veri merkezindeki cihaz *hedef* aygıt bulunan aynı veya farklı bir coğrafi konum. Cihaz yük devretme için tüm aygıttır. Yük devretme sırasında bulut veri kaynağı cihaz için sahipliği, hedef aygıt değiştirir.

StorSimple sanal dizinizi için, yalnızca aynı StorSimple Yöneticisi hizmeti tarafından yönetilen başka bir sanal dizinin yük. Bir yük devretme 8000 serisi aygıt ya da (kaynak aygıtın olandan) farklı bir StorSimple Yöneticisi hizmet yöneten bir dizi izin verilmiyor. Yük devretme konuları hakkında daha fazla bilgi edinmek için şu adrese gidin [aygıt yük devretme için Önkoşullar](storsimple-virtual-array-failover-dr.md).

Bir başarısız üzerinden sanal dizinizi gerçekleştirirken, aşağıdakileri göz önünde bulundurun:

* Planlanmış bir yük devretme için tüm birimleri/yük devretmeyi başlatmadan önce paylaşımlar çevrimdışı yapılacak bir önerilen en iyi uygulamadır. Konakta birimler/paylaşımları çevrimdışı ilk alabilir ve ardından bu sanal Cihazınızda çevrimdışına için işletim sistemine özgü yönergeleri izleyin.
* Bir dosya sunucusu olağanüstü durum kurtarma için (DR), böylece paylaşım izinleri otomatik olarak çözülür hedef aygıt kaynak ile aynı etki alanına katılacak öneririz. Yük devretme aynı etki alanında bir hedef cihaz için yalnızca bu sürümde desteklenmiyor.
* DR başarıyla tamamlandığında, kaynak cihaz otomatik olarak silinir. Cihaz artık kullanılabilir olsa da, ana bilgisayar sisteminde sağlanan sanal makine hala kaynakları tüketilmesine neden olabilir. Ana bilgisayar sisteminizden herhangi bir ücret tahakkuk gelen önlemek için bu sanal makineyi Sil öneririz.
* Yük devretme başarısız olsa bile Not **verileri her zaman bulutta güvenlidir**. Yük devretme, bir başarıyla tamamlanmadı aşağıdaki üç senaryoları göz önünde bulundurun:
  
  * DR ön denetimleri zaman gerçekleştirilir gibi yük devretme ilk aşamaları içinde bir hata oluştu. Bu durumda, hedef Cihazınızı hala kullanılabilir. Yük devretme aynı hedef aygıttaki yeniden deneyebilirsiniz.
  * Gerçek yük devretme işlemi sırasında bir hata oluştu. Bu durumda, hedef aygıta kullanılamaz olarak işaretlenmiş. Sağlamak ve başka bir hedef sanal dizi yapılandırmak ve yük devretme için kullanmanız gerekir.
  * Yük devretme, kaynak aygıt silindi aşağıdaki tam, ancak hedef aygıt sorunları vardır ve herhangi bir veri erişemez. Verileri bulutta hala güvendedir ve kolayca başka bir sanal dizinin oluşturarak ve ardından DR için bir hedef cihaz olarak kullanılarak alınabilir.

### <a name="deactivate"></a>Devre dışı bırak
StorSimple sanal dizinin devre dışı bıraktığınızda, aygıt ve karşılık gelen StorSimple Yöneticisi hizmeti arasındaki bağlantı sever. Devre dışı bırakma olan bir **kalıcı** işlemi ve geri alınamaz. Devre dışı bırakılan bir cihazı ile StorSimple Yöneticisi hizmeti yeniden kaydedilemedi. Daha fazla bilgi için Git [devre dışı bırakın ve StorSimple sanal dizinizi silme](storsimple-virtual-array-deactivate-and-delete-device.md).

Aşağıdaki en iyi yöntemleri sanal dizinizi devre dışı bırakırken göz önünde bulundurun:

* Bir bulut anlık görüntü sanal cihazı devre dışı bırakma önce tüm veri alın. Sanal bir dizi devre dışı bıraktığınızda, tüm yerel cihaz verileri kaybolur. Bir bulut anlık daha sonraki bir aşamada verileri kurtarmak olanak sağlar.
* StorSimple sanal dizinin devre dışı bırakmadan önce istemciler ve bu cihazda bağımlı konakları silin veya durdurmak emin olun.
* Böylece ücretler tahakkuk değil artık kullanıyorsanız devre dışı bırakılmış bir aygıtı silme.

### <a name="monitoring"></a>İzleme
StorSimple sanal dizinizi sürekli iyi durumda olduğundan emin olmak için dizi izlemek ve Uyarıları da dahil olmak üzere sistem bilgi aldığından emin olmak gerekir. Sanal dizinin genel durumunu izlemek için aşağıdaki en iyi yöntemleri uygulayın:

* İşletim sistemi diski yanı sıra sanal dizinin veri diski disk kullanımı izlemek için izlemeyi yapılandırın. Hyper-V çalıştıran, sanallaştırma ana bilgisayarlarını izlemek için System Center Virtual Machine Manager (SCVMM) ve System Center Operations Manager (SCOM) bir birleşimini kullanabilirsiniz.
* E-posta bildirimleri sanal dizinizi belirli kullanım düzeylerinde uyarıları göndermek üzere yapılandırın.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Dizin araması ve virüs uygulamaları tarama
StorSimple sanal dizinin otomatik olarak Microsoft Azure bulut yerel katmanından veri katmanı. Bir uygulama dizin araması veya bir virüs taraması gibi StorSimple üzerinde depolanan verileri taramak için kullanıldığında, bulut verilerini değil erişilen ve yerel katmanına geri çekilen emin ilgilenebilmek gerekir.

Dizin arama veya virüs taraması sanal dizinizi yapılandırırken aşağıdaki en iyi yöntemleri uygulayın öneririz:

* Otomatik olarak yapılandırılan tam tarama işlemleri devre dışı bırakın.
* Katmanlı birimler için artımlı bir tarama gerçekleştirmek için dizin arama veya virüs tarama uygulamayı yapılandırın. Bu, yalnızca yeni veri büyük olasılıkla yerel katmanda bulunan tarama. Buluta katmanlı veri artımlı bir işlem sırasında erişilebilir değil.
* Doğru arama filtreleri ve ayarları yapılandırılmış dosya yalnızca hedeflenen türlerini taranan emin olun. Örneğin, resim dosyaları (JPEG, GIF ve TIFF) ve çizimler mühendislik artımlı veya tam dizini yeniden oluşturma sırasında taranmalıdır değil.

Dizin oluşturma işlemi Windows kullanıyorsanız, aşağıdaki yönergeleri izleyin:

* Dizin sık sık yeniden oluşturulması gerekiyorsa buluttan büyük miktarlarda verinin (TBs) çeker gibi Windows Dizin Oluşturucu katmanlı birimler için kullanmayın. Dizini yeniden derlemeyi içeriklerini dizin için tüm dosya türlerini alır.
* Bu yalnızca (bulut verilerini değil erişileceği) dizin oluşturmak için yerel katmanlarda verilere erişir gibi dizin oluşturma işlemi yerel olarak sabitlenmiş birimler için Windows kullanın.

### <a name="byte-range-locking"></a>Bayt aralığı kilitleme
Uygulamaların içindeki dosyalar belirtilen bayt aralığı kilitleyebilirsiniz. Bayt aralığı kilitleme, StorSimple yazma uygulamaları etkinse, ardından katmanlama sanal dizinizi çalışmaz. Çalışmak için sunabilmek için tüm alanları erişilen dosyaların kilidi olması gerekir. Sanal dizinizi katmanlı birimlerin bayt aralığı kilitleme desteklenmiyor.

Bayt aralığı kilitleme hafifletmek için önerilen ölçüleri içerir:

* Uygulama mantığınızın kilitleme bayt aralığı kapatın.
* Kullanım yerel olarak sabitlenmiş birimler (yerine katmanlı) bu uygulamayla ilişkili verileri için. Yerel olarak sabitlenmiş birimlerin bulutunu katmanı değil.
* Kullanarak yerel olarak birimler bayt aralığı kilitleme etkin sabitlenmiş, geri yükleme tamamlanmadan önce birim çevrimiçi gelebilir. Bu durumlarda, tam olarak geri yüklemek için beklemeniz gerekir.

## <a name="multiple-arrays"></a>Birden çok diziler
Birden çok sanal diziler dolayısıyla aygıt performansını etkileyen bulut sığdırmaya veri büyüyen bir çalışma kümesi için hesap dağıtılması gerekebilir. Bu gibi durumlarda çalışma kümesi büyüdükçe ölçek aygıtlar için en iyisidir. Bu, şirket içi veri merkezinde eklenecek bir veya daha fazla aygıtları gerektirir. Aygıtlar eklerken, olabilir:

* Geçerli veri kümesi bölün.
* Yeni iş yüklerine yeni aygıtlara dağıtın.
* Birden çok sanal dizileri dağıtma, yük dengeleyici gelen öneririz açısından, dizi farklı hiper yönetici ana bilgisayarlara dağıtın.
* (Dosya sunucusu veya bir iSCSI sunucu olarak yapılandırıldığında) birden çok sanal diziler bir dağıtılmış dosya sistemi Namespace içinde dağıtılabilir. Ayrıntılı adımlar için Git [dağıtılmış dosya sistemi Namespace çözümüyle karma bulut depolama Dağıtım Kılavuzu](https://www.microsoft.com/download/details.aspx?id=45507). Dağıtılmış dosya sistemi çoğaltma, şu anda sanal dizinin ile kullanım için önerilmez. 

## <a name="see-also"></a>Ayrıca bkz.
Bilgi edinmek için nasıl [StorSimple sanal dizinizi yönetmek](storsimple-virtual-array-manager-service-administration.md) StorSimple Yöneticisi hizmeti üzerinden.

