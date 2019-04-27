---
title: En iyi uygulamalar için StorSimple sanal dizisi | Microsoft Docs
description: StorSimple sanal dizisi dağıtıp için en iyi yöntemler açıklanmıştır.
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
ms.date: 11/08/2018
ms.author: alkohli
ms.openlocfilehash: b8e9f12a549f71971c2da3b9865f6a74dad58f61
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60630147"
---
# <a name="storsimple-virtual-array-best-practices"></a>StorSimple sanal dizisi en iyi uygulamalar
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple Virtual Array bir hiper yönetici ve Microsoft Azure bulut depolama çalışan bir şirket içi sanal aygıt arasındaki Depolama görevlerini yöneten tümleşik depolama çözümüdür. StorSimple sanal dizisi 8000 serisi fiziksel dizisine verimli ve ekonomik bir alternatiftir. Sanal dizi mevcut hiper yönetici altyapınızda çalıştırabilirsiniz, hem iSCSI ve SMB protokolleri destekler ve Uzaktan ofis/şube ofisi senaryolarında için çok uygundur. StorSimple çözümleri hakkında daha fazla bilgi için Git [Microsoft Azure StorSimple genel bakış](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Bu makalede, ilk Kurulum, dağıtım ve yönetimini StorSimple sanal dizisi sırasında uygulanan en iyi uygulamaları kapsar. Bu en iyi uygulamaları, Kurulum ve Yönetim sanal diziniz için doğrulanmış yönergeleri sağlar. Bu makalede, dağıtma ve yönetme veri merkezlerini sanal dizilerde BT yöneticileri doğrultusunda yöneliktir.

Kurulum veya işlem akışı için değişiklik yapıldığında, cihazınızın hala uyumlu olduğundan emin olmak için en iyi uygulamaları periyodik olarak İnceleme öneririz. Karşılaştığınız sorunları sanal diziniz üzerinde bu en iyi uygulama çalışırken [Microsoft Support başvurun](storsimple-virtual-array-log-support-ticket.md) Yardım almak için.

## <a name="configuration-best-practices"></a>Yapılandırma en iyi uygulamalar
Sanal diziler dağıtılması ve ilk kurulum sırasında izlenmesi gereken yönergeler bu en iyi uygulamaları kapsar. Grup İlkesi ayarları, boyutlandırma, ağı kurma, depolama hesaplarını yapılandırma ve paylaşımları ve birimler sanal dizi oluşturma sanal makinenin sağlama için ilgili bu en iyi yöntemleri içerir. 

### <a name="provisioning"></a>Sağlama
StorSimple sanal dizisi bir sanal (Hyper-V veya VMware) hiper yöneticide sağlanan makine (VM) ana bilgisayar sunucu bulunuyor. Sanal makine sağlanırken konağınız yeterli kaynak ayırması mümkün olduğundan emin olun. Daha fazla bilgi için Git [en düşük kaynak gereksinimlerini](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) bir diziyi sağlamak için.

Sanal dizi sağlanırken aşağıdaki en iyi yöntemleri uygulayın:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Sanal makine türü** |**2. nesil** Windows Server 2012 veya daha sonra kullanmak için VM ve *.vhdx* görüntü. <br></br> **1. kuşak** bir Windows Server 2008 veya daha sonra kullanmak için VM ve *.vhd* görüntü. |Sanal makine sürümünü kullanın kullanırken 8 *.vmdk* görüntü. |
| **Bellek türü** |Olarak yapılandırma **statik bellek**. <br></br> Kullanmayın **dinamik bellek** seçeneği. | |
| **Veri disk türü** |Olarak sağlama **dinamik olarak genişleyen**.<br></br> **Boyutu sabit** uzun sürüyor. <br></br> Kullanmayın **fark kayıt** seçeneği. |Kullanım **ölçülü kaynak sağlama** seçeneği. |
| **Veri disk değiştirme** |Genişletme veya küçültme izin verilmiyor. Bunu yapma girişimi, cihazdaki tüm yerel veri kaybı sonuçlanır. |Genişletme veya küçültme izin verilmiyor. Bunu yapma girişimi, cihazdaki tüm yerel veri kaybı sonuçlanır. |

### <a name="sizing"></a>Boyutlandırma
StorSimple Virtual Array'iniz için boyutlandırma yaparken aşağıdaki etmenleri dikkate alın:

* Birimler veya paylaşımlar için yerel ayırma. % Alanı yaklaşık 12 her sağlanan katmanlı birim veya paylaşım için yerel katmanında ayrılmıştır. Yaklaşık %10 alan da yerel olarak sabitlenmiş bir birim dosya sistemi için ayrılmıştır.
* Ek yükü anlık görüntüsünü alın. Yaklaşık % 15 alan yerel katmanındaki anlık görüntüler için ayrılmış.
* Geri yüklemeler için gerekli. Yeni bir işlem olarak geri yükleme yaparsanız, boyutlandırma geri yüklemek için ihtiyaç duyulan alanı hesabı. Geri yükleme, bir paylaşımı veya birimi aynı boyutta için gerçekleştirilir.
* Bazı arabellek için beklenmeyen büyümesine ayrılmalıdır.

Önceki etkenlere bağlı olarak, boyutlandırma gereksinimlerini deneylerin gösterilebilir:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

Aşağıdaki örnekler, gereksinimlerinize göre bir sanal diziye nasıl boyutunu gösterir.

#### <a name="example-1"></a>Örnek 1:
Sanal diziniz için kullanabilmek ister.

* 2 TB katmanlı birim veya paylaşım sağlayın.
* 1 TB katmanlı birim veya paylaşım sağlayın.
* 300 GB yerel olarak sabitlenmiş birim veya paylaşım sağlayın.

Bize önceki birimler veya paylaşımlar için yerel katman alanı gereksinimleri hesaplayın.

İlk olarak, her katmanlı birim/paylaşım için yerel ayırma birim/paylaşım boyutu % 12 oranında eşit olacaktır. Yerel olarak sabitlenmiş birim/paylaşım için yerel ayırma (ek olarak sağlanan boyut) yerel olarak sabitlenmiş birim/paylaşım boyutunun % 10'dur. Bu örnekte, gereksinim

* 240 GB için ayrılmış yerel alanlarınız (2 TB katmanlı birim/paylaşım)
* 120 GB için ayrılmış yerel alanlarınız (1 TB katmanlı birim/paylaşım)
* 330 GB yerel olarak sabitlenmiş birim veya paylaşım (ayrılmış yerel alanlarınız % 10 sağlanan 300 GB boyutunu ekleme)

Yerel katmanında şimdiye gereken toplam alanı verilmiştir: 240 GB + 120 GB + 330 GB = 690 GB.

İkinci olarak, yerel katmanında en az bir alan en büyük tek ayırma ihtiyacımız var. Bu ek miktar, bulut anlık görüntüden geri yüklemeniz gereken durumlarda kullanılır. Bu örnekte, en büyük ayrılmış yerel alanlarınız 330 (dosya sistemi ayırma dahil), GB'dir bu nedenle, 690 GB ile eklemeniz: 690 GB + 330 GB = 1020 GB.
Sonraki ek geri yüklemeler gerçekleştirdiğimiz, size her zaman önceki geri yükleme işlemi yer boşaltabilirsiniz.

Böylece yalnızca % 85, kullanılabilir üçüncü şimdiye yerel anlık görüntüleri depolamak için toplam yerel alanınızın % 15 ihtiyacımız var. Bu örnekte, olurdu etrafında 1020 GB = 0.85&ast;sağlanan verilerin TB disk. Bu nedenle, sağlanan veri diski olacaktır (1020&ast;(1/0.85)) 1200 GB = 1.20 TB = ~ 1,25 (en yakın dörtte için yuvarlama) TB

Beklenmeyen büyüme ve yeni geri yüklemeler hesaba katacak şekilde, geçici olarak yerel bir disk sağlamanız gerekir 1,25-1,5 TB.

> [!NOTE]
> Ayrıca yerel disk ölçülü öneririz. Bu öneri, geri yükleme alanı yalnızca beş günden eski olan verileri geri yüklemek istediğinizde gerekli olmasıdır. Öğe düzeyinde kurtarma verileri, ek alan geri yüklemek için gerek kalmadan son beş gün boyunca geri yüklemenize olanak sağlar.


#### <a name="example-2"></a>Örnek 2:
Sanal diziniz için kullanabilmek ister.

* 2 TB katmanlı birim sağlayın
* 300 GB yerel olarak sabitlenmiş bir birim

İhtiyacımız katmanlı birimleri/paylaşımları için yerel alanı ayırma ve yerel olarak sabitlenmiş birimleri/paylaşımları için % %10 12 bağlı olarak,

* 240 GB yerel ayırma (birim/paylaşım için 2 TB katmanlı)
* Yerel olarak sabitlenmiş birim veya paylaşım (10 yerel ayırma yüzdesi 300 GB sağlanan alanına eklemek) için 330 GB

Yerel katmanında gereken toplam alan şöyledir: 240 GB + 330 GB = 570 GB

Geri yükleme için gereken en düşük yerel alan 330 GB'dir.

Toplam diskinizin % 15, böylece yalnızca 0.85 kullanılabilir anlık görüntülerini depolamak için kullanılır. Bu nedenle, disk boyutu olan (900&ast;(1/0.85)) 1.06 TB = ~ 1,25 (en yakın dörtte için yuvarlama) TB

Beklenmeyen büyümesine hesaba katacak şekilde, yerel bir 1,25-1,5 TB disk sağlayabilirsiniz.

### <a name="group-policy"></a>Grup ilkesi
Grup İlkesi, kullanıcılar ve bilgisayarlar için belirli yapılandırmaları uygulamak izin veren bir altyapıdır. Grup İlkesi ayarları, Grup İlkesi nesneleri (GPO'lar), Active Directory etki alanı Hizmetleri (AD DS) kapsayıcıların bağlanılan içindedir: sitelere, etki alanları veya kuruluş birimlerine (OU). 

Sanal diziniz etki alanına katılmış ise, GPO'ları uygulanabilir. StorSimple sanal dizisi işlemi olumsuz yönde etkileyebilir bir virüsten koruma yazılımı gibi uygulamalar bu GPO'larını yükleyebilirsiniz.

Bu nedenle, öneririz:

* Sanal diziniz için Active Directory kendi kuruluş birimi (OU) olduğundan emin olun.
* Sanal diziniz hiçbir Grup İlkesi nesneleri (GPO'lar) uygulandığından emin olun. Sanal dizi (alt düğümü) otomatik olarak herhangi bir GPO üstten devralmaz emin olmak için devralmayı engelleyebilirsiniz. Daha fazla bilgi için Git [block devralma](https://technet.microsoft.com/library/cc731076.aspx).

### <a name="networking"></a>Ağ
Sanal diziniz için ağ yapılandırması yerel web UI gerçekleştirilir. Bir sanal ağ arabirimi, hiper yönetici sanal dizinin sağlandığı etkinleştirilir. Kullanım [ağ ayarlarını](storsimple-virtual-array-deploy3-fs-setup.md) sayfasında sanal ağ arabirimi IP adresi, alt ağ ve ağ geçidi yapılandırmak için.  Cihazınız için birincil ve ikincil DNS sunucusu, saat ayarlarını ve isteğe bağlı proxy ayarlarını da yapılandırabilirsiniz. Çoğu ağ yapılandırması, bir kerelik Kurulum olur. Gözden geçirme [gereksinimlerinde StorSimple](storsimple-ova-system-requirements.md#networking-requirements) sanal diziyi dağıtmadan önce.

Sanal diziniz dağıtırken, bu en iyi uygulamaları izlemenizi öneririz:

* Sanal dizide her zaman dağıtılan ağ 5 MB/sn Internet bant genişliği (veya daha fazlası) ayrılması kapasitesi olduğundan emin olun.
  
  * Internet bant genişliği gereksinimi, iş yükü özellikleriniz ve veri değişikliği hızına bağlı olarak değişir.
  * İşlenebilir veri değişikliği için Internet bant genişliğiniz orantılıdır. Bir yedekleme alırken bir örnek olarak, 5 MB/sn bant genişliği 18 GB yaklaşık 8 saat içindeki bir veri değişikliği barındırabilir. Dört kat daha fazla bant genişliği ile (20 MB/sn), dört kat daha fazla veri değişikliği (72 GB) işleyebilir.
* İnternet'e her zaman kullanılabilir olduğundan emin olun. Internet bağlantısı ara sıra ya da güvenilir olmayan cihazlara bulutta verilere erişim kaybına neden olabilir ve desteklenmeyen bir yapılandırma neden olabilir.
* Cihazınız bir iSCSI sunucusu olarak dağıtmayı planlıyorsanız:
  
  * Devre dışı bırakmanızı öneririz **IP adresini otomatik olarak almak** seçeneği (DHCP).
  * Statik IP adreslerini yapılandırın. Birincil ve ikincil DNS sunucusu yapılandırmanız gerekir.
  * Birden çok ağ arabirimi sanal üzerinde tanımlanıyorsa dizisi, yalnızca ilk ağ arabirimini (varsayılan olarak, bu arabirimidir **Ethernet**) bulut ulaşabilirsiniz. Trafik türü denetlemek için (bir iSCSI sunucusu olarak yapılandırılmış) sanal diziniz birden çok sanal ağ arabirimi oluşturun ve bu arabirimler farklı alt ağlara bağlanmak.
* Yalnızca bulut bant genişliğini azaltma (kullanılan sanal dizi tarafından), yönlendirici veya güvenlik duvarı azaltma yapılandırın. Azaltma, Hiper yöneticide tanımlarsanız, iSCSI ve SMB yalnızca bulut bant genişliğini yerine dahil olmak üzere tüm protokoller kısıtlama.
* Hiper etkinleştirilmişse, o zaman eşitleme emin olun. Hyper-V Yöneticisi'nde sanal diziniz Hyper-V kullanarak seçeneğini belirlerseniz, Git **ayarları &gt; tümleştirme hizmetleri**, olduğundan emin olun **zaman eşitleme** denetlenir.

### <a name="storage-accounts"></a>Depolama hesapları
StorSimple sanal dizisi, tek bir depolama hesabı ile ilişkili olabilir. Veya başka bir abonelik için bir depolama hesabıyla ilgili bu depolama hesabına otomatik olarak oluşturulan depolama hesabı, hizmet ile aynı abonelikte hesap olabilir. Daha fazla bilgi için bkz. nasıl [sanal diziniz için depolama hesaplarını yönetme](storsimple-virtual-array-manage-storage-accounts.md).

Sanal diziniz ile ilişkili depolama hesapları için aşağıdaki önerileri kullanın.

* Maksimum Kapasite (64 TB) bir sanal dizin ve bir depolama hesabı için maksimum boyut (500 TB) için birden çok sanal diziler ile tek bir depolama hesabına bağlanırken faktörü. Bu, söz konusu depolama hesabına yaklaşık 7 ile ilişkilendirilebilir tam boyutlu sanal diziler sayısını sınırlar.
* Yeni bir depolama hesabı oluştururken
  
  * Bu bölgede en yakın gecikme süreleri en aza indirmek için StorSimple Virtual Array'iniz dağıtıldığı uzaktan ofis/şube ofisi oluşturmanızı öneririz.
  * Unutmayın, bir depolama hesabı farklı bölgeler arasında taşıyamazsınız size aittir. Ayrıca, bir hizmet abonelikler arasında taşıyamazsınız.
  * Veri merkezleri arasında yedeklilik uygulayan bir depolama hesabını kullanırsınız. Coğrafi olarak yedekli depolama (GRS), bölgesel olarak yedekli depolama (ZRS) ve yerel olarak yedekli depolama (LRS) tüm sanal diziniz ile kullanım için desteklenir. Farklı türlerde depolama hesapları hakkında daha fazla bilgi için Git [Azure depolama çoğaltma](../storage/common/storage-redundancy.md).

### <a name="shares-and-volumes"></a>Paylaşımları ve birimler
StorSimple Virtual Array'iniz için bir dosya sunucusu ve bir iSCSI sunucusu olarak yapılandırıldığında birimleri olarak yapılandırıldığında, paylaşımları sağlayabilirsiniz. Paylaşımları ve birimler oluşturmak için en iyi uygulamalar, boyutu ve yapılandırılan türü ilgilidir.

#### <a name="volumeshare-size"></a>Birim/paylaşım boyutu
Sanal diziniz üzerinde bir dosya sunucusu ve bir iSCSI sunucusu olarak yapılandırıldığında birimleri olarak yapılandırıldığında, paylaşımları sağlayabilirsiniz. Paylaşımları ve birimler oluşturmak için en iyi boyut ve yapılandırılan türü ilişkilendirebilirsiniz. 

Aşağıdaki en iyi paylaşımlarından veya birimlerinden sanal Cihazınızda sağlanırken unutmayın.

* Katmanlı bir paylaşım boyutunun sağlanan göreli dosya boyutları katmanlama performansını etkileyebilir. Büyük dosyaları ile çalışma, yavaş bir katmanın ölçeğini sonuçlanabilir. Büyük dosyalarla çalışırken, en büyük dosya paylaşım boyutunun %3 küçükse öneririz.
* Sanal dizide en fazla 16 birimleri/paylaşımları oluşturulabilir. Yerel olarak sabitlenmiş hem de katmanlı birimleri/paylaşımları için boyut sınırları, her zaman başvurmak [StorSimple sanal dizi sınırları](storsimple-ova-limits.md).
* Bir birim oluştururken, beklenen veri tüketimi yanı sıra gelecekteki büyüme faktörü. Birim daha sonra genişletilemez.
* Birim oluşturulduktan sonra StorSimple birimde boyutunu küçültülemez.
* Birimdeki verileri (ayrılmış birim için yerel alan göre) belirli bir eşiğe ulaştığında bir katmanlı birim için StorSimple yazarken GÇ kısıtlanır. Bu birime yazmaya devam GÇ önemli ölçüde uzuyor. Katmanlı birim (biz etkin olarak sağlanan kapasiteyi aşan yazma kullanıcıdan durdurmaz), sağlanan kapasiteyi aşan yazabileceğiniz rağmen zamanlayıcınızın, etkili olması için bir uyarı bildirimine bakın. Uyarıyı gördüğünüzde, kesinlik temelli birimdeki verileri silme gibi düzeltici önlemleri alın (Birim genişletme şu anda desteklenmiyor).
* Olağanüstü durum kurtarma için kullanım örnekleri, izin verilen paylaşımları/birim sayısı 16'dır ve paralel olarak işlenebilir paylaşımları/birim sayısı da 16, paylaşımları/birim sayısı bir boşluğunu RPO ve RTO yok.

#### <a name="volumeshare-type"></a>Birim/paylaşım türü
StorSimple, kullanıma bağlı iki birime/paylaşıma tür destekler: yerel olarak sabitlenmiş hem de katmanlı. Yerel olarak sabitlenmiş birimleri/paylaşımları, katmanlı birimleri/paylaşımları ölçülü ise bol sağlanır. Yerel olarak sabitlenmiş bir birim/paylaşım dönüştürülemiyor çok katmanlı veya *tersi* oluşturulduktan sonra.

StorSimple birimleri/paylaşımları yapılandırırken aşağıdaki en iyi uygulama öneririz:

* Bir birim oluşturmadan önce dağıtmak istediğiniz iş yüklerini temel birim türünü belirleyin. Yerel olarak sabitlenmiş birimler (hatta bir bulut kesinti sırasında) yerel GARANTİLERİN veri gerektiren ve bulut düşük gecikme süreleri gerektiren iş yükleri için kullanın. Bir birim üzerinde sanal diziniz oluşturduktan sonra birim türünü değiştiremezsiniz öğesinden yerel olarak çok katmanlı sabitlenmiş veya *tersi*. SQL iş yükleri veya sanal makineleri (VM'ler); iş yüklerini dağıtırken örnek olarak, yerel olarak sabitlenmiş birim oluştur Katmanlı birimler, dosya paylaşımı iş yükleri için kullanın.


#### <a name="volume-format"></a>Ses biçimi
StorSimple birimlerini iSCSI sunucunuzda oluşturduktan sonra Başlat, bağlamak ve birimleri biçimlendirmek gerekir. Bu işlem, StorSimple cihazınıza bağlı konak üzerinde gerçekleştirilir. Bağlama ve StorSimple konaktaki birimleri biçimlendirme, aşağıdaki en iyi yöntemleri kullanmanız önerilir.

* Tüm StorSimple birimlerde hızlı biçimlendirme gerçekleştirin.
* Bir StorSimple biriminin biçimlendirilirken bir 64 KB ayırma birimi boyutu (AU) kullanın (varsayılan değer 4 KB). 64 KB AU, şirket içinde ortak StorSimple iş yükleri ve diğer iş yükleri için yapılan testlere temel alır.
* StorSimple Virtual Array'ı bir iSCSI sunucusu olarak yapılandırılmış kullanırken, bu birimler dağıtılmış birimler veya dinamik diskleri kullanmayın veya diskler StorSimple tarafından desteklenmez.

#### <a name="share-access"></a>Paylaşım erişimi
Sanal dizi dosya sunucunuzda paylaşımları oluştururken, aşağıdaki yönergeleri izleyin:

* Bir paylaşımı oluştururken, tek bir kullanıcı yerine bir paylaşım yönetici olarak bir kullanıcı grubuna atayın.
* Windows Gezgini aracılığıyla paylaşımları düzenleyerek paylaşım oluşturulduktan sonra NTFS izinlerini yönetebilir.

#### <a name="volume-access"></a>Birim erişimi
İSCSI birimler, StorSimple sanal dizisi yapılandırırken, gerekli olan her yerde erişimi denetlemek önemlidir. Hangi ana bilgisayar sunucuları birimlere erişebilirsiniz belirlemek için oluşturma ve erişim denetimi kayıtları (Acr'leri) StorSimple birimlerini ile ilişkilendirebilirsiniz.

Acr'leri için StorSimple birimlerini yapılandırırken aşağıdaki en iyi yöntemleri kullanın:

* Her zaman en az bir ACR toplu ile ilişkilendirin.

* Birden fazla ACR bir birime atarken, birim, burada, aynı anda birden fazla kümelenmemiş ana bilgisayar tarafından erişilebilen bir şekilde gösterilmez emin olun. Bir birim için birden çok Acr'leri atadıysanız, bir uyarı iletisi yapılandırmanızı gözden geçirmek için açılır.

### <a name="data-security-and-encryption"></a>Veri güvenliği ve şifreleme
StorSimple sanal dizisi gizlilik ve veri bütünlüğünü sağlamak veri güvenliği ve şifreleme özellikleri de vardır. Bu özellikler kullanılırken, bu en iyi uygulamaları izlemeniz önerilir: 

* Veriler sanal diziniz buluta gönderilmeden önce AES-256 şifreleme oluşturmak için bir bulut depolama şifreleme anahtarı tanımlayın. Verileriniz ile başlaması şifrelenir, bu anahtar gerekli değildir. Anahtar oluşturulur ve anahtar yönetimi sistemi kullanarak nedensiz [Azure anahtar kasası](../key-vault/key-vault-whatis.md).
* StorSimple Yöneticisi hizmeti aracılığıyla depolama hesabı yapılandırırken, StorSimple cihazınız ve bulut arasında ağ iletişimi için güvenli bir kanal oluşturmak SSL modu etkinleştirdiğinizden emin olun.
* Depolama hesaplarınız için anahtarları (Azure depolama hizmetine erişerek) düzenli aralıklarla hesabına erişmek herhangi bir değişiklik için Yöneticiler değiştirilen listesine göre yeniden oluşturun.
* Veriler sanal diziniz üzerinde sıkıştırılır ve Azure'a gönderilmeden önce kaldırılmış. Yinelenen verileri kaldırma rol hizmetini Windows Server konağınızda kullanımı önerilmemektedir.

## <a name="operational-best-practices"></a>İşletimsel en iyi uygulamalar
İşletimsel en iyi uygulamalar günlük yönetim ve sanal dizinin işlemi sırasında izlenmesi gereken yönergelerdir. Bu yöntemler bir yük devri gerçekleştirmek, bir yedekleme kümesinden geri yedekler almak gibi belirli yönetim görevlerini kapsar, devre dışı bırakılması ve sistem kullanımı ve sistem durumu izleme ve koruma çalıştırma dizinin silinmesi sanal diziniz tarar.

### <a name="backups"></a>Yedeklemeler
Sanal diziniz verileri iki yolla buluta yedeklendiğinden, varsayılan Otomatik günlük yedekleme 22:30 veya el ile talep üzerine yedekleme ile başlayan tüm cihaz. Varsayılan olarak, cihaz üzerinde bulunan tüm verilerin günlük bulut anlık görüntüleri otomatik olarak oluşturur. Daha fazla bilgi için Git [StorSimple Virtual Array'iniz yedekleme](storsimple-virtual-array-backup.md).

Varsayılan yedekleme ile ilişkili bekletme ve sıklığı değiştirilemez, ancak her gün başlatılan günlük yedekleri zaman yapılandırabilirsiniz. Otomatik yedekleri için başlangıç saatini yapılandırırken öneririz:

* Yedeklemeleriniz için yoğun olmayan saatler zamanlayın. Yedekleme başlangıç saati çok sayıda konak g/ç ile çakıştığı değil.
* Sanal diziniz üzerinde verileri korumak için bakım penceresi için önceki ya da cihaz yük devretme gerçekleştirmek planlama yaparken, el ile isteğe bağlı yedekleme başlatın.

### <a name="restore"></a>Geri Yükleme
İki şekilde ayarlanmış bir yedekten geri yükleyebilirsiniz: başka bir birime veya paylaşıma geri yükleme ya da (yalnızca dosya sunucusu olarak yapılandırılmış bir sanal dizin kullanılabilir) bir öğe düzeyinde kurtarma gerçekleştirin. Öğe düzeyinde kurtarma StorSimple cihazında tüm paylaşımlar bulut yedeğinden dosya ve klasörlerin parçalı kurtarma gerçekleştirmenize izin verir. Daha fazla bilgi için Git [bir yedekten geri yükleme](storsimple-virtual-array-clone.md).

Bir geri yükleme gerçekleştirilirken aşağıdaki yönergeleri göz önünde bulundurun:

* StorSimple Virtual Array'iniz yerinde geri yüklemeyi desteklemez. Ancak bu yedeğe olabilir iki adımlı bir işlem tarafından gerçekleştirilen: sanal dizi ve daha sonra başka bir birime/paylaşıma geri alan.
* Yerel bir birimden geri koruduğunuzda göz önünde uzun süre çalışan bir işlemin geri yükleme olacaktır. Birim hızlı bir şekilde çevrimiçi gelebilir ancak veriler arka planda hydrated devam eder.
* Birim türünü geri yükleme işlemi sırasında aynı kalır. Katmanlı birim başka bir katmanlı birime geri yüklenir ve yerel olarak sabitlenmiş bir birim için başka bir yerel olarak sabitlenmiş birim.
* Bir birimi veya paylaşımı bir yedekten geri yüklemeyi denerken ayarlayın, geri yükleme işi başarısız olursa, bir hedef birim veya paylaşım Portalı'nda yine de oluşturulabilir. Bu kullanılmayan hedef birimi silin veya bu öğeden doğan gelecekteki sorunları en aza indirmek için portaldaki paylaşıma önemlidir.

### <a name="failover-and-disaster-recovery"></a>Yük devretme ve olağanüstü durum kurtarma
Cihaz yük devretme geçirmenizi verilerinizden sağlayan bir *kaynak* cihaz başka bir veri merkezinde *hedef* cihaz, aynı veya farklı bir coğrafi konumda bulunan. Cihaz yük devretme için tüm cihazdır. Yük devretme sırasında kaynak cihazdaki bulut verilerini sahipliği, hedef cihazın değiştirir.

StorSimple Virtual Array'iniz için size yalnızca aynı StorSimple Yöneticisi hizmeti tarafından yönetilen başka bir sanal diziye devredebilir. 8000 serisi bir cihaza ya da dizi (kaynak cihaz için olandan) farklı bir StorSimple Yöneticisi hizmeti tarafından yönetilen bir yük devretme izin verilmez. Yük devretme dikkat edilecek noktalar hakkında daha fazla bilgi için Git [cihaz yük devretme için Önkoşullar](storsimple-virtual-array-failover-dr.md).

Sanal diziniz için yük devretme üzerinden gerçekleştirildiği sırada aşağıdakileri göz önünde bulundurun:

* Planlanmış bir yük devretme için tüm birimleri/yük devretmeyi başlatmadan önce paylaşımlar çevrimdışına almak için önerilen en iyi uygulama olan. Birimleri/paylaşımları çevrimdışı konakta ilk alıp ardından bu sanal Cihazınızda çevrimdışı duruma işletim sistemine özgü yönergeleri izleyin.
* Bir dosya sunucusu olağanüstü durum kurtarma için (DR), böylece paylaşım izinleri otomatik olarak çözümlenir hedef cihaz kaynak ile aynı etki alanına katılma öneririz. Bu sürümde, yalnızca aynı etki alanında bir hedef cihaza yük devretme desteklenir.
* DR başarıyla tamamlandıktan sonra kaynak cihaz otomatik olarak silinir. Cihaz artık kullanılabilir olsa da, konak sisteminde sağlanan sanal makine kaynakları hala tüketiyor. Ana bilgisayar sisteminizden herhangi bir ücret tahakkuk gelen önlemek için bu sanal makine silmenizi öneririz.
* Yük devretme başarısız olsa bile Not **verileri her zaman bulutta güvenlidir**. Yük devretme, bir başarıyla tamamlanmadı üç aşağıdaki senaryoları göz önünde bulundurun:
  
  * Yük devretme zaman DR öncesi denetimler gerçekleştiriliyor gibi ilk aşamalarında bir hata oluştu. Bu durumda, hedef cihazınız hala kullanılabilir. Aynı hedef cihazda yük devretmeyi yeniden deneyin.
  * Gerçek bir yük devretme işlemi sırasında bir hata oluştu. Bu durumda, hedef cihaza kullanılamaz olarak işaretlenmiş. Sağlama ve başka bir hedef sanal dizi yapılandırmak ve, yük devretme için kullanmanız gerekir.
  * Yük devretme, kaynak cihaz silindikten sonra aşağıdaki tamamlandı, ancak hedef cihaz sorunları vardır ve herhangi bir veri erişemez. Veriler bulutta hala güvenlidir ve başka bir sanal diziye oluşturarak ve ardından DR için bir hedef cihaz kullanarak kolayca alınabilir.

### <a name="deactivate"></a>Devre dışı bırak
StorSimple sanal dizisi devre dışı bıraktığınızda, karşılık gelen bir StorSimple Yöneticisi hizmeti ile cihaz arasındaki bağlantı sever. Devre dışı bırakma olan bir **kalıcı** işlemi ve geri alınamaz. Devre dışı bırakılmış bir cihaz StorSimple Yöneticisi hizmetine yeniden kaydedilemez. Daha fazla bilgi için Git [devre dışı bırakma ve silme StorSimple Virtual Array'iniz](storsimple-virtual-array-deactivate-and-delete-device.md).

Aşağıdaki en iyi sanal diziniz devre dışı bırakılırken göz önünde bulundurun:

* Sanal cihazı devre dışı bırakma önce tüm verileri bir bulut anlık görüntüsünü alın. Bir sanal dizin devre dışı bıraktığınızda, tüm yerel cihaz verileri kaybolur. Bir bulut anlık görüntü alma, verileri daha sonraki bir aşamada kurtarmak izin verir.
* StorSimple sanal dizisi devre dışı bırakmadan önce durdurmak veya istemciler ve o cihazda ana bilgisayarları Sil emin olun.
* Böylece ücretler tahakkuk etmez artık kullanıyorsanız, devre dışı bırakılmış bir cihaz silin.

### <a name="monitoring"></a>İzleme
StorSimple Virtual Array'iniz sürekli iyi durumda olduğundan emin olmak için dizi izleme ve uyarılar dahil olmak üzere sistem bilgileri aldığından emin olmak gerekir. Sanal diziyi genel durumunu izlemek için aşağıdaki en iyi yöntemleri uygulayın:

* Sanal dizi veri diskinizi yanı sıra işletim sistemi diskinin disk kullanımını izlemek için izleme yapılandırın. Hyper-V çalıştırıyorsanız, sanallaştırma konaklarını izlemek için System Center Virtual Machine Manager (SCVMM) ve System Center Operations Manager'ı kullanabilirsiniz.
* E-posta bildirimleri sanal diziniz belirli kullanım düzeylerinde uyarıları göndermek üzere yapılandırın.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Dizin arama ve virüs uygulamaları tarama
StorSimple Virtual Array, otomatik olarak Microsoft Azure bulutuna yerel katmanından veri katmanı. Bir uygulama gibi bir dizin araması veya bir virüs taraması StorSimple üzerinde depolanan verileri taramak için kullanıldığında, bulut verilerini değil erişilen alıp yerel katmana geri çekilen, halletmeniz gerekir.

Dizin arama veya virüs taraması sanal diziniz yapılandırırken aşağıdaki en iyi uygulama öneririz:

* Herhangi bir otomatik olarak yapılandırılan bir tam tarama işlemi devre dışı bırakın.
* Artımlı bir tarama gerçekleştirmek için dizin arama veya virüs taraması uygulama katmanlı birimler için yapılandırın. Bu, yalnızca yeni veriler büyük olasılıkla yerel katmanında bulunan taraması. Artımlı bir işlemi sırasında buluta katmanlanmış verileri erişilebilir değil.
* Doğru arama filtreleri ve ayarları yapılandırılmış dosya yalnızca hedeflenen türlerini taranan emin olun. Örneğin, görüntü dosyaları (JPEG, GIF ve TIFF) ve mühendislik çizimler artımlı veya tam dizinin yeniden oluşturulması sırasında taranmalıdır değil.

Dizin oluşturma işlemi Windows kullanıyorsanız, aşağıdaki yönergeleri izleyin:

* Dizin sık sık yeniden oluşturulması gerekiyorsa bulut üzerinden büyük miktarlarda veri (TB'a) çeker gibi Windows Dizin Oluşturucu için katmanlı birim kullanmayın. Dizinini yeniden oluşturmayı dizin içeriklerinin tüm dosya türlerini alır.
* Bu, yalnızca (bulut veri erişilemeyecektir) dizin oluşturmak için yerel katmanlardaki verilere erişir gibi dizin oluşturma işlemi yerel olarak sabitlenmiş birimler için Windows kullanın.

### <a name="byte-range-locking"></a>Bayt aralığı kilitleme
Uygulamalar, belirtilen bayt aralığı içindeki dosyaları kilitleyebilirsiniz. Bayt aralığı kilitleme için StorSimple'ınızı yazma uygulamaları etkinse, sonra katmanlama sanal diziniz çalışmaz. Çalışmak için katman ayarlama için tüm alanları erişilen dosyaların kilitli olmalıdır. Sanal diziniz katmanlı birimlerde bayt aralığı kilitleme desteklenmez.

Bayt aralığı kilitleme hafifletmek için önerilen ölçüleri içerir:

* Bayt aralığı uygulama mantığınızın kilitleme devre dışı bırakın.
* Kullanım yerel olarak sabitlenmiş birimler (yerine katmanlı) bu uygulamayla ilişkili verileri. Yerel olarak sabitlenmiş birimler buluta katmanı değil.
* Kullanarak yerel olarak bayt aralığı kilitleme etkin sabitlenmiş birimler, geri yükleme tamamlanmadan önce birim çevrimiçi gelebilir. Bu gibi durumlarda, tam olarak geri yüklemek için beklemeniz gerekir.

## <a name="multiple-arrays"></a>Birden çok dizisi
Birden çok sanal diziler, böylece cihaz performansını buluta sığdırmaya veri büyüyen bir çalışma kümesi hesabına dağıtılması gerekebilir. Bu gibi durumlarda, çalışma kümesi arttıkça ölçeği cihazlara en iyisidir. Bu, şirket içi veri merkezinde eklenecek bir veya daha fazla cihaz gerektirir. Cihaz ekleme, şunları yapabilirsiniz:

* Geçerli veri kümesi bölün.
* Yeni iş yüklerini yeni aygıtlara dağıtın.
* Birden çok sanal diziler dağıtımı-yük dengelemeden öneririz açısından, bir dizi farklı hiper yönetici ana bilgisayarları üzerinde dağıtın.
* (Dosya sunucusu veya bir iSCSI sunucusu olarak yapılandırıldığında) birden çok sanal dizisi, bir dağıtılmış dosya sistemi Namespace içinde dağıtılabilir. Ayrıntılı adımlar için Git [dağıtılmış dosya sistemi Namespace çözümüyle karma bulut depolama Dağıtım Kılavuzu](https://www.microsoft.com/download/details.aspx?id=45507). Dağıtılmış dosya sistemi çoğaltma, şu anda sanal diziyi ile kullanım için önerilmez. 

## <a name="see-also"></a>Ayrıca bkz.
Bilgi edinmek için nasıl [StorSimple Virtual Array'iniz yönetmek](storsimple-virtual-array-manager-service-administration.md) StorSimple Yöneticisi hizmeti aracılığıyla.

