---
title: Microsoft Azure StorSimple sanal Dizisi'nin genel bakış | Microsoft Docs
description: StorSimple sanal dizisi, bir şirket içi sanal dizi ve Microsoft Azure bulut depolama arasındaki Depolama görevlerini yöneten tümleşik bir çözüm açıklanmaktadır.
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: e5713af737a6d9d190814b4155a8e772deea06bc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60630368"
---
# <a name="introduction-to-the-storsimple-virtual-array"></a>StorSimple sanal dizisi giriş

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple Virtual Array bir hiper yönetici ve Microsoft Azure bulut depolama çalışan bir şirket içi sanal dizi arasındaki Depolama görevlerini yöneten tümleşik depolama çözümüdür. Sanal diziyi bir verimli, Hesaplı ve kolayca yönetilen dosya sunucusu veya birçok sorunu ve Kurumsal Depolama ve veri korumasıyla ilişkili giderlerini ortadan kaldıran iSCSI sunucusu çözümü ' dir. Sanal dizi özellikle iyi seyrek erişilen arşiv verileri depolama alanı için uygundur.

Bu makalede sanal diziyi genel bir bakış sağlar - diğer bazı kaynaklar aşağıda verilmiştir:

* En iyi yöntemler için bkz: [StorSimple Virtual Array en iyi](storsimple-ova-best-practices.md).
* StorSimple 8000 serisi cihazlar genel bakış için Git [StorSimple 8000 serisi: bir karma bulut çözümü](storsimple-overview.md).
* StorSimple 5000/7000 Serisi cihazlar hakkında daha fazla bilgi için Git [StorSimple çevrimiçi Yardım](http://onlinehelp.storsimple.com/).

Sanal dizi, iSCSI veya sunucu ileti bloğu (SMB) protokolünü destekler. Var olan hiper yönetici altyapınızın çalışır ve bulut, bulut yedekleme, hızlı geri yükleme, öğe düzeyinde kurtarma ve olağanüstü durum kurtarma özellikleri için katman ayarlama sağlar.

Aşağıdaki tabloda StorSimple sanal dizisi önemli özellikleri özetlenmektedir.

| Özellik | StorSimple Sanal Dizisi |
| --- | --- |
| Yükleme gereksinimleri |Sanallaştırma altyapısı (Hyper-V veya VMware) kullanır. |
| Kullanılabilirlik |Tek düğüm |
| Toplam Kapasite (bulut dahil) |Sanal dizi başına en fazla 64 TB kullanılabilir kapasite |
| Yerel kapasite |Sanal dizi (500 GB ile 8 TB disk alanı sağlamak için gereklidir) başına 6.4 TB kullanılabilir kapasite 390 GB |
| Yerel protokolleri |iSCSI veya SMB |
| Kurtarma süresi hedefi (RTO) |iSCSI: 2 dakikadan kısa bir süre boyutundan bağımsız olarak |
| Kurtarma noktası hedefi (RPO) |Günlük yedekleme ve isteğe bağlı yedekleme |
| Depolama katmanlamayı |Eşleme, hangi verilerin içe veya dışa katmanlı belirlemek için kullandığı ısı |
| Destek |Sağlayıcı tarafından desteklenen sanallaştırma altyapısı |
| Performans |Altyapının bağlı olarak değişir. |
| Veri taşınabilirliği |Aynı cihaza geri yükleyebilir veya öğe (dosya sunucusu) düzeyinde kurtarma |
| Depolama katmanları |Yerel hiper yönetici depolama alanı ve bulut |
| Paylaşım boyutu |Katmanlı: 20 TB'a varan; yerel olarak sabitlenmiş: en fazla 2 TB |
| Birim boyutu |Katmanlı: 500 GB ila 5 TB '; yerel olarak sabitlenmiş: 50 GB ile 200 GB <br> Katmanlı birim için maksimum yerel ayırma 200 GB olur. |
| Anlık Görüntüler |Kilitlenmeyle tutarlı |
| Öğe düzeyinde kurtarma |Evet; paylaşımlarından kullanıcıları geri yükleyebilir |

## <a name="why-use-storsimple"></a>StorSimple neden kullanmalısınız?

StorSimple kullanıcı ve sunucu, herhangi bir uygulama değişiklik olmadan dakikalar içinde Azure depolama alanına bağlanır.

Aşağıdaki tabloda StorSimple sanal dizisi sağlıyor önemli avantajlardan bazıları açıklanmaktadır.

| Özellik | Avantaj |
| --- | --- |
| Saydam tümleştirme |Sanal dizi, iSCSI veya SMB protokolünü destekler. Yerel katmanı ve bulut katmanı arasında veri taşıma, sorunsuz ve kullanıcıya saydam. |
| Daha düşük depolama maliyetleri |StorSimple ile en çok kullanılan sık erişimli veriler için geçerli taleplerini karşılamak için yeterli yerel depolama sağlayın. Depolama Büyümesi gerektiğinde gibi StorSimple katmanları Durgun verileri düşük maliyetli bulut depolama. Veriler yinelenen verileri kaldırma işlemi ve depolama gereksinimlerini ve para harcamazsınız daha da azaltmak için buluta göndermeden önce sıkıştırılmış. |
| Basitleştirilmiş Depolama Yönetimi |StorSimple, birden çok cihazı yönetmek için StorSimple cihaz Yöneticisi'ni kullanarak bulutta merkezi yönetim sağlar. |
| Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk |StorSimple daha hızlı bir olağanüstü durum kurtarma meta veriler hemen geri yükleme ve gerektiğinde veri geri yükleme işlemini kolaylaştırır. Başka bir deyişle, normal işlemleri en az kesinti ile devam edebilirsiniz. |
| Veri taşınabilirliği |Buluta katmanlanmış verileri kurtarma ve geçiş amaçları doğrultusunda diğer sitelerden de erişilebilir. Not yalnızca özgün sanal dizi verileri geri yükleyebilirsiniz. Ancak, tüm sanal dizi, başka bir sanal diziye geri yüklemek için olağanüstü durum kurtarma özelliklerini kullanın. |

## <a name="storsimple-workload-summary"></a>StorSimple iş yükü özeti

Desteklenen StorSimple iş yüklerinin özetini aşağıdaki tabloda verilmiştir.

|Senaryo     |İş yükü     |Desteklenen      |Kısıtlamalar               | Geçerli sürümler|
|-------------|-------------|---------------|---------------------------|--------------------|
|Uzaktan ofis/şube Office (ROBO)  |Dosya paylaşımı     |Evet      |Bkz: [dosya sunucusu için sınırlarını](storsimple-ova-limits.md).<br></br>Bkz: [desteklenen SMB sürümleri için sistem gereksinimleri](storsimple-ova-system-requirements.md).| Tüm sürümler     |
|Arşivleme bulut  |Arşiv dosya paylaşımı     |Evet      |Bkz: [dosya sunucusu için sınırlarını](storsimple-ova-limits.md).<br></br>Bkz: [desteklenen SMB sürümleri için sistem gereksinimleri](storsimple-ova-system-requirements.md).| Tüm sürümler     |

StorSimple sanal dizisi nadiren erişilen veriler için idealdir. Sanal dizinin yerel bir önbelleğin performansını artırmak üzere olsa da Kullanıcılar Cihaz en düşük katmanlı depolama (bulut) dosyaları Hizmetleri varsaymanız gerekir. Her bir sanal diziye, yazma ve Azure Depolama'ya yaklaşık 100 MB/sn hızında okuma. Bu bağlantı, cihazında gelen tüm istekler arasında paylaşılan ve aşağıdaki diyagramda gösterildiği gibi bir performans sorunu haline gelebilir.

![Arşivleme bulut](./media/storsimple-ova-overview/cloud-archiving.png)

Birden çok eş zamanlı kullanıcı sanal diziyi eriştiğinizde, tüm bağlantı daha düşük bir performans azure'a paylaşırlar. Kullanıcı başına garantili hiçbir performans yoktur ve geldikçe cihaz tek tek istekleri işler.

StorSimple sanal dizisi, yüksek kullanılabilirlik gerektiren iş yükleri için uygun değil. Sanal dizi, yazılım güncelleştirmeleri yüklenirken kapalı kalma süresi deneyimleri tek düğümlü bir cihaz ' dir. Yöneticiler yıllık 3 - 4 kez 30 dakikalık bir bakım penceresi planlamanız gerekir.

## <a name="workflows"></a>İş akışları

StorSimple sanal dizisi, özellikle aşağıdaki iş akışları için uygundur:

* [Bulut tabanlı depolama yönetimi](#cloud-based-storage-management)
* [Konumdan bağımsız yedekleme](#location-independent-backup)
* [Veri koruma ve olağanüstü durum kurtarma](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Bulut tabanlı depolama yönetimi
Azure portalında çalışan StorSimple cihaz Yöneticisi hizmeti birden fazla konumda birden fazla cihazda depolanan verileri yönetmek için kullanabilirsiniz. Bu dağıtılmış dal senaryolarda özellikle yararlıdır. Ayrı örneklerini sanal diziler ve fiziksel StorSimple cihazları yönetmek için StorSimple cihaz Yöneticisi hizmeti oluşturmanız gerektiğini unutmayın. Ayrıca sanal diziyi yeni Azure portalında Klasik Azure portalı yerine artık kullandığına dikkat edin.

![Bulut tabanlı depolama yönetimi](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Konumdan bağımsız yedekleme
Sanal dizi, bulut anlık görüntüleri bir birimi veya paylaşım konumdan bağımsız, zaman içinde nokta kopyasını sağlayın. Bulut anlık görüntüleri, varsayılan olarak etkinleştirilir ve devre dışı bırakılamaz. Tüm birim ve paylaşımların aynı anda tek bir günlük yedekleme İlkesi aracılığıyla yedekleme ve gerektiğinde ek geçici yedeklemeler alabilir.

### <a name="data-protection-and-disaster-recovery"></a>Veri koruma ve olağanüstü durum kurtarma
Sanal dizi, aşağıdaki veri koruma ve olağanüstü durum kurtarma senaryolarını destekler:

* **Birimi veya paylaşımı geri yükleme** – birimi veya paylaşımı, kurtarılır, yeni iş akışı olarak geri yüklemesini kullanın. Birimin tamamını veya paylaşım kurtarmak için bu yaklaşımı kullanın.
* **Öğe düzeyinde kurtarma** – paylaşımları, son yedeklemelerin yönelik Basitleştirilmiş erişim izin verin. Tek bir dosyayı özel bir kolayca kurtarabilirsiniz *.backup* bulutta klasör. Bu geri yükleme özelliği, kullanıcı odaklı ve yönetici müdahalesi gerekli değildir.
* **Olağanüstü durum kurtarma** – tüm birimler veya paylaşımlar yeni bir sanal diziye kurtarmak için yük devretme özelliğini kullanın. Yeni bir sanal diziye oluşturun ve StorSimple cihaz Yöneticisi hizmetiyle kaydedilmesi sonra özgün sanal dizi başarısız. Yeni bir sanal diziye ardından sağlanan kaynakları varsayar.

## <a name="storsimple-virtual-array-components"></a>StorSimple sanal dizisi bileşenleri

Sanal dizi, aşağıdaki bileşenleri içerir:

* [Sanal dizi](#virtual-array) – sanallaştırılmış bir ortam veya hiper Yöneticisi sağlanan bir sanal makine temel bir karma bulut depolama cihazı.
* [StorSimple cihaz Yöneticisi hizmeti](#storsimple-device-manager-service) bir uzantısı olan bir veya daha fazla StorSimple cihazlar farklı coğrafi konumlardan erişebildiği tek bir web arabiriminden yönetmenizi sağlayan Azure portalı. StorSimple cihaz Yöneticisi hizmeti oluşturma ve hizmetleri yönetmek, görüntülemek ve aygıtları ve Uyarıları yönetebilir ve birimler, paylaşımlar ve varolan anlık görüntülerini yönetmek için kullanabilirsiniz.
* [Yerel web kullanıcı arabirimi](#local-web-user-interface) – cihaz yerel ağa bağlayın ve sonra cihaz StorSimple cihaz Yöneticisi hizmetiyle kaydedin şekilde yapılandırmak için kullanılan bir web tabanlı bir kullanıcı Arabirimi. 
* [Komut satırı arabirimi](#command-line-interface) – sanal dizide destek oturumu başlatmak için kullanabileceğiniz bir Windows PowerShell arabirimi.
  Aşağıdaki bölümlerde, bu bileşenlerin daha ayrıntılı açıklanmaktadır ve nasıl çözüm veri düzenler, depolama alanı ayırır ve Depolama Yönetimi ve veri korumasını kolaylaştırır açıklar.

### <a name="virtual-array"></a>Sanal dizi

Sanal dizi, birincil depolama sağlayan, bulut depolama ile iletişimi yönetir ve cihazda depolanan tüm verileri gizliliğini ve güvenlik sağlamaya yardımcı olur tek düğümlü depolama çözümüdür.

Sanal dizi indirme için kullanılabilir bir modeli kullanıma sunulmuştur. Sanal dizi bulut depolama maksimum kapasiteye 6.4 TB cihazla (bir temel alınan depolama gereksinimi 8 TB) ve 64 TB dahil olmak üzere sahiptir.

Sanal dizi aşağıdaki özelliklere sahiptir:

* Uygun maliyetli olduğundan. Bunu var olan sanallaştırma altyapınızın kullanın ve mevcut Hyper-V veya VMware hiper yöneticide dağıtılabilir.
* Bu, veri merkezinde yer alır ve bir iSCSI sunucusu veya dosya sunucusu yapılandırılabilir.
* Bulutla tümleştirilmiş.
* Yedekleme, olağanüstü durum kurtarmayı kolaylaştıran ve öğe düzeyinde kurtarmayı (ILR) basitleştirmek bulutta depolanır.
* Yalnızca bunları fiziksel cihaza uygulanacak şekilde güncelleştirmelerini sanal diziye uygulayabilirsiniz.

> [!NOTE]
> Sanal dizi genişletilemez. Bu nedenle, sanal diziyi oluşturduğunuzda yeterli depolama sağlamak önemlidir.

### <a name="storsimple-device-manager-service"></a>StorSimple Device Manager hizmeti

Microsoft Azure StorSimple, StorSimple depolama merkezi olarak yönetmenize olanak sağlayan StorSimple cihaz Yöneticisi hizmeti bir web tabanlı kullanıcı arabirimi sağlar. StorSimple cihaz Yöneticisi hizmeti, aşağıdaki görevleri gerçekleştirmek için kullanabilirsiniz:

* Birden çok StorSimple sanal dizisi, tek bir hizmetten yönetin.
* Yapılandırmak ve StorSimple sanal dizilerine yönelik güvenlik ayarlarını yönetin. (Şifreleme bulutta Microsoft Azure API'leri bağlıdır.)
* Depolama hesabı kimlik bilgilerini ve özelliklerini yapılandırın.
* Yapılandırma ve birimler veya paylaşımlar yönetin.
* Yedekleme ve veri birimler veya paylaşımlar üzerinde geri yükleme.
* Performansını izleme.
* Sistem ayarları gözden geçirin ve olası sorunları tanımlayın.

Sanal diziniz günlük yönetimi gerçekleştirmek için StorSimple cihaz Yöneticisi hizmetini kullanın.

Daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Yerel web kullanıcı arabirimi

Sanal dizi tek seferlik yapılandırma ve StorSimple cihaz Yöneticisi hizmeti ile cihaz kaydı için kullanılan bir web tabanlı bir kullanıcı Arabirimi içerir. Kapat ve sanal diziyi yeniden tanılama sınamaları çalıştırın yazılım güncelleştirme cihaz Yöneticisi parolasını değiştirme sistem günlüklerini görüntülemek ve bir hizmet isteği için Microsoft Support başvurun için kullanabilirsiniz.

Web tabanlı kullanıcı Arabirimi kullanma hakkında daha fazla bilgi için Git [StorSimple Virtual Array'iniz yönetmek için web tabanlı kullanıcı arabirimini kullanın](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Komut satırı arabirimi

Dahil edilen Windows PowerShell arabirimini, sorun giderme ve sanal diziniz karşılaşabileceğiniz sorunları çözmenize yardımcı olabilir, böylece Microsoft Support destek oturumu başlatmak sağlar.

## <a name="storage-management-technologies"></a>Depolama yönetimi teknolojileri

Sanal diziyi ve diğer bileşenlere ek olarak StorSimple çözümü önemli verilere hızlı erişim sağlayan, depolama kullanımını azaltma ve sanal diziniz üzerinde depolanan verileri korumak için aşağıdaki yazılım teknolojileri kullanır:

* [Otomatik depolama katmanlamayı](#automatic-storage-tiering) 
* [Yerel olarak sabitlenmiş paylaşımları ve birimler](#locally-pinned-shares-and-volumes)
* Yinelenenleri kaldırma ve sıkıştırma verileri için katmanlı veya buluta yedeklenen 
* [Zamanlanmış ve isteğe bağlı yedeklemeler](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Otomatik depolama katmanlamayı
Sanal dizi, sanal diziyi ve bulutta depolanan verileri yönetmek için yeni bir katmanlama mekanizması kullanır. Yalnızca iki katmanı vardır: yerel sanal dizi ve Azure bulut depolama. StorSimple sanal dizisi, veri otomatik olarak geçerli kullanım, yaş ve ilişkiler için diğer verileri izler bir ısı Haritası temel katmanlar halinde düzenler. Daha az etkin ve etkin olmayan verileri otomatik olarak buluta geçiş sırasında (en düşük çoğaltılma olasılığına) yerel olarak depolanan en etkin olan veriler. (Tüm yedeklemeler bulutta depolanır.) StorSimple, ayarlar ve verileri yeniden düzenler ve kullanım biçimlerini depolama atamalar değiştirin. Örneğin, bazı bilgiler zaman içinde daha az etkin hale gelebilir. Aşamalı olarak daha az etkin hale gelir, out buluta katmanlı. Bu aynı verileri tekrar etkin hale gelirse, bu, Depolama dizisinde katmanlı.

Belirli katmanlı bir paylaşım veya birim için verileri kendi yerel katmanı alanı (yaklaşık % 10 o paylaşımı veya birimi için toplam sağlanan alanının) garanti edilir. Bu sanal diziden o paylaşımı veya birimi için kullanılabilir depolama alanı azaltır, ancak bir paylaşımı veya birimi katmanlama katmanlama ihtiyaçlarını diğer paylaşımları veya birimleri tarafından etkilenmez, sağlar. Bu nedenle bir paylaşımı veya birimi üzerindeki çok meşgul bir iş yükünü buluta diğer iş yükleri tutamaz.

İSCSI için oluşturulan katmanlı birimlerin 200 GB birimin boyutundan bağımsız olarak bir maksimum yerel ayırma vardır.

![Otomatik depolama katmanlamayı](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Yerel olarak sabitlenmiş bir birim belirtebilirsiniz, bu durumda, veriler sanal dizide kalır ve hiçbir zaman buluta katmanlı. Daha fazla bilgi için Git [yerel olarak sabitlenmiş paylaşımları ve birimler](#locally-pinned-shares-and-volumes).


### <a name="locally-pinned-shares-and-volumes"></a>Yerel olarak sabitlenmiş paylaşımları ve birimler

Uygun paylaşımları ve yerel olarak sabitlenmiş birimler oluşturabilirsiniz. Bu özellik, kritik uygulamalar tarafından gerektirilen veri sanal dizide kalır ve hiçbir zaman buluta katmanlanmış sağlar. Yerel olarak sabitlenmiş paylaşımları ve birimler aşağıdaki özelliklere sahiptir:

* Bunlar bulut gecikmeleri veya bağlantı sorunları tabi değildir.
* Bunlar, StorSimple bulut yedekleme ve olağanüstü durum kurtarma özellikleri yine de yararlanabilir.

Yerel olarak sabitlenmiş bir paylaşım veya birim katmanlı olarak veya katmanlı bir paylaşım geri yükleyebilirsiniz veya toplu olarak yerel olarak sabitlenmiş. 

Yerel olarak sabitlenmiş birimler hakkında daha fazla bilgi için Git [birimleri yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Yinelenenleri kaldırma ve sıkıştırma verileri için katmanlı veya buluta yedeklenen

StorSimple, yinelenenleri kaldırma ve veri sıkıştırma daha da bulut depolama gereksinimlerini azaltmak için kullanır. Yinelenenleri kaldırma genel yedeklilik depolanan veri kümesindeki ortadan kaldırarak depolanan veri miktarını azaltır. Bilgi değiştikçe StorSimple değiştirilmemiş verilerini yoksayar ve yalnızca değişiklikleri yakalar. Ayrıca, StorSimple tanımlama ve yinelenen bilgileri kaldırılıyor depolanan verilerin miktarını azaltır.

> [!NOTE]
> Sanal dizide depolanan verileri kaldırılmış veya sıkıştırılmış desteklenmez. Tüm yinelenenleri kaldırma ve sıkıştırma yalnızca verileri buluta gönderilmeden önce gerçekleşir.

### <a name="scheduled-and-on-demand-backups"></a>Zamanlanmış ve isteğe bağlı yedeklemeler

StorSimple veri koruma özellikleri isteğe bağlı yedeklemeleri oluşturmanıza olanak sağlar. Ayrıca, varsayılan bir yedekleme zamanlaması günlük yedek yedeklenen verileri sağlar. Yedeklemeler, bulutta depolanan artımlı anlık görüntüleri biçiminde alınır. Son yedeklemeden bu yana değişiklikler yalnızca kaydetme, anlık görüntüler oluşturulabilir ve hızlı bir şekilde geri. İkincil depolama sistemleri (örneğin, bant yedekleme) değiştirin ve veri Merkezinize veya diğer sitelere gerekirse geri yüklemenize olanak tanımak için bu anlık görüntüler olağanüstü durum kurtarma senaryolarında kritik düzeyde önemli olabilir.

## <a name="managing-personal-information"></a>Kişisel bilgilerini yönetme

Sanal serisi için StorSimple cihaz Yöneticisi iki anahtar örneklerinde kişisel bilgilerinizi toplar:
 - Kullanıcıların e-posta adreslerini yapılandırıldığı kullanıcı ayarları uyarır. Bu bilgiler, yönetici tarafından silinebilir. 
 - Paylaşımlarında bulunan veriler erişebilen kullanıcılar. Paylaşımı verileri erişebilen kullanıcıları içeren bir liste görüntülenir ve dışarı aktarılabilir. Bu liste de silinir paylaşımları silindiğinde.

Daha fazla bilgi için gözden [Güven Merkezi'nde Microsoft Privacy İlkesi](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [sanal dizi portalı hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).
