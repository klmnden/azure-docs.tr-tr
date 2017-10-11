---
title: "Microsoft Azure StorSimple sanal dizinin genel bakış | Microsoft Docs"
description: "StorSimple sanal dizinin bir şirket içi sanal dizinin ve Microsoft Azure bulut depolama arasında depolama görevleri yöneten bir tümleşik depolama çözümü açıklanmaktadır."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/09/2016
ms.author: alkohli
ms.openlocfilehash: 100eed4694d2017333ef25eca86034d17cce78d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-the-storsimple-virtual-array"></a>StorSimple sanal dizi giriş
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple sanal dizinin bir hiper yönetici ve Microsoft Azure bulut depolama çalıştıran bir şirket içi sanal dizinin arasında depolama görevleri yöneten bir tümleşik depolama çözümüdür. Sanal bir verimli, uygun maliyetli ve kolayca yönetilen dosya sunucusu veya Kurumsal Depolama ve veri koruma ile ilişkili giderleri ve sorunların çoğunu ortadan kaldıran iSCSI sunucu çözümü dizidir. Sanal dizinin uzak office/şube ofisi senaryolarında için özellikle de çok uygundur.

Bu konu, sanal dizinin genel bir bakış sağlar - diğer bazı kaynaklar aşağıda verilmiştir:

* En iyi yöntemler için bkz: [StorSimple sanal dizinin en iyi yöntemler](storsimple-ova-best-practices.md).
* StorSimple 8000 serisi cihazlar genel bakış için Git [StorSimple 8000 serisi: karma bulut çözümünün](storsimple-overview.md). 
* StorSimple 5000/7000 Serisi cihazlar hakkında daha fazla bilgi için Git [StorSimple çevrimiçi Yardım](http://onlinehelp.storsimple.com/).

Sanal dizinin iSCSI veya sunucu ileti bloğu (SMB) protokolünü destekler. Varolan hiper yönetici altyapınızda çalışır ve bulut, bulut yedekleme, hızlı geri yükleme, öğe düzeyinde kurtarma ve olağanüstü durum kurtarma özellikleri için katmanlama sağlar.

Aşağıdaki tabloda StorSimple sanal dizinin önemli özellikler özetlenmektedir.

| Özellik | StorSimple Sanal Dizisi |
| --- | --- |
| Yükleme gereksinimleri |Sanallaştırma (Hyper-V veya VMware) altyapısını kullanır |
| Kullanılabilirlik |Tek düğüm |
| Toplam Kapasite (bulut dahil) |Sanal dizinin başına en fazla 64 TB kullanılabilir kapasite |
| Yerel kapasite |Sanal dizinin (8 TB 500 GB disk alanı sağlamak için gereklidir) başına 6.4 TB kullanılabilir kapasitesi 390 GB |
| Yerel protokolleri |iSCSI veya SMB |
| Kurtarma süresi hedefi (RTO) |iSCSI: 2 dakikadan daha kısa bir süre boyutu ne olursa olsun |
| Kurtarma noktası hedefi (RPO) |Günlük yedeklemeler ve isteğe bağlı yedeklemeler |
| Depolama katmanlamayı |Hangi verilerin, giriş veya çıkış katmanlı belirlemek için eşleme kullanır ısı |
| Destek |Sağlayıcı tarafından desteklenen sanallaştırma altyapısı |
| Performans |Temel alınan altyapınıza bağlı olarak değişir |
| Veri mobility |Aynı cihaza geri yükleyebilirsiniz veya (dosya sunucusu) düzeyinde |
| Depolama katmanları |Yerel hiper yönetici depolama ve bulut |
| Paylaşım boyutu |Katmanlı: en fazla 20 TB'ye; yerel olarak sabitlenmiş: 2 TB |
| Birim boyutu |Katmanlı: 5 TB 500 GB; yerel olarak sabitlenmiş: 50 GB 500 GB |
| Birim boyutu |Katmanlı: en fazla 5 TB; yerel olarak sabitlenmiş: 500 GB'a kadar |
| Anlık görüntüler |Kilitlenme tutarlı |
| Öğe düzeyinde kurtarma |Evet; Kullanıcılar paylaşımlarından geri yükleyebilir |

## <a name="why-use-storsimple"></a>StorSimple neden kullanılır?
StorSimple kullanıcı ve sunucu, herhangi bir uygulama değişiklik olmadan dakika cinsinden Azure depolama alanına bağlanır.

Aşağıdaki tabloda StorSimple sanal dizisi çözümü sağlar en önemli avantajlardan bazıları açıklanmaktadır.

| Özellik | Avantaj |
| --- | --- |
| Saydam tümleştirme |Sanal dizinin iSCSI veya SMB protokolünü destekler. Yerel katmanı ve bulut katmanı arasındaki veri taşıma sorunsuz ve kullanıcıya şeffaf ' dir. |
| Azaltılmış depolama maliyetleri |StorSimple ile en fazla kullanılan dinamik veri için geçerli taleplerini karşılamak üzere yeterli yerel depolama birimi sağlayın. Büyüme depolama gereksinimlerine göre uygun maliyetli uygulamasına StorSimple katmanları soğuk veri depolama bulut. Veri yinelenenleri kaldırılmış ve daha fazla depolama alanı gereksinimleri ve gider azaltmak için buluta göndermeden önce sıkıştırılır. |
| Basitleştirilmiş Depolama Yönetimi |StorSimple birden çok cihazları yönetmek için StorSimple cihaz Yöneticisi'ni kullanarak bulutta merkezi yönetim sağlar. |
| Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk |StorSimple meta veriler hemen geri yükleme ve gerektiğinde geri yüklenmesi daha hızlı olağanüstü durum kurtarma işlemini kolaylaştırır. Başka bir deyişle, normal işlemleri en az kesintiyi ile devam edebilirsiniz. |
| Veri mobility |Veri Katmanlı buluta kurtarma ve geçiş amaçları için diğer sitelere erişilebilir. Verileri yalnızca özgün sanal dizinin geri olduğunu unutmayın. Ancak, tüm sanal dizinin sanal başka diziye geri yüklemek için olağanüstü durum kurtarma özellikleri kullanın. |

## <a name="storsimple-workload-summary"></a>StorSimple iş yükü özeti

Desteklenen StorSimple iş yükleri özetini aşağıdaki tabloda verilmiştir.

|Senaryo     |İş yükü     |Destekleniyor      |Kısıtlamaları               |
|-------------|-------------|---------------|---------------------------|
|ROBO işbirliği |Dosya Paylaşımı     |Evet      |Bkz: [dosya sunucusu için en fazla sınırları](storsimple-ova-limits.md).<br></br>Bkz: [desteklenen SMB sürümleri için sistem gereksinimleri](storsimple-ova-system-requirements.md).| Tüm sürümler     |

## <a name="workflows"></a>İş akışları
StorSimple sanal dizinin özellikle aşağıdaki iş akışları için uygundur:

* [Bulut tabanlı depolama yönetimi](#cloud-based-storage-management)
* [Konumdan bağımsız yedekleme](#location-independent-backup)
* [Veri koruma ve olağanüstü durum kurtarma](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>bulut tabanlı depolama yönetimi
Azure portalında çalışan StorSimple cihaz Yöneticisi hizmeti birden çok aygıta ve birden fazla konumda depolanan verileri yönetmek için kullanabilirsiniz. Bu dağıtılmış şube senaryolarında özellikle kullanışlıdır. Ayrı ayrı örnekleri sanal dizileri ve fiziksel StorSimple cihazları yönetmek için StorSimple cihaz Yöneticisi hizmeti oluşturmanız gerektiğini unutmayın. Ayrıca sanal dizinin yerine Klasik Azure portalı artık yeni Azure portalına kullandığını unutmayın.

![bulut tabanlı depolama yönetimi](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Konumdan bağımsız yedekleme
Sanal dizinin ile bulut anlık görüntüleri bir birim veya paylaşım konumu-bağımsız, zaman içinde nokta kopyasını sağlar. Bulut anlık görüntüleri, varsayılan olarak etkinleştirilir ve devre dışı bırakılamaz. Tüm birimler ve paylaşımlar aynı anda tek bir günlük yedekleme ilke aracılığıyla Yedekleme'yi ayarlama ve ek geçici yedeklemeler gerektiğinde alabilir.

### <a name="data-protection-and-disaster-recovery"></a>Veri koruma ve olağanüstü durum kurtarma
Sanal dizinin aşağıdaki veri koruma ve olağanüstü durum kurtarma senaryoları destekler:

* **Birimi veya paylaşımı geri yükleme** – bir birim veya paylaşım kurtarmak için yeni iş akışı olarak geri yüklemeyi kullanın. Tüm birim veya paylaşım kurtarmak için bu yaklaşımı kullanın.
* **Öğe düzeyinde kurtarma** – paylaşımları son yedeklemeler için Basitleştirilmiş erişim izni. Bir özel tek bir dosyayı kolayca kurtarabilirsiniz *.backup* bulutta klasör. Bu geri yükleme yeteneği kullanıcı odaklı ve hiçbir yönetim müdahalesi gerekli değildir.
* **Olağanüstü durum kurtarma** – tüm birimler veya paylaşımlar için yeni bir sanal dizi kurtarmak için yük devretme özelliğini kullanın. Yeni sanal dizinin oluşturun ve StorSimple cihaz Yöneticisi Hizmeti'ne kaydedin sonra özgün sanal dizinin başarısız. Yeni sanal dizinin ardından sağlanan kaynakları varsayar. 

## <a name="storsimple-virtual-array-components"></a>StorSimple sanal dizinin bileşenleri
Sanal dizinin aşağıdaki bileşenleri içerir:

* [Sanal dizinin](#virtual-array) – sanallaştırılmış ortamı ya da hiper yönetici sağlanan bir sanal makine temel bir karma bulut depolama cihazı.  
* [StorSimple cihaz Yöneticisi hizmeti](#storsimple-device-manager-service) – bir veya daha fazla StorSimple cihazlar farklı coğrafi konumlardan erişebilmeniz için tek bir web arabiriminden yönetmenizi sağlayan Azure portalı uzantısı bir. StorSimple cihaz Yöneticisi hizmeti oluşturmak ve hizmetleri yönetmek, görüntülemek ve aygıtları ve Uyarıları yönetebilir ve birimler, paylaşımlar ve varolan anlık görüntülerini yönetmek için kullanabilirsiniz.
* [Yerel web kullanıcı arabirimi](#local-web-user-interface) – cihaz yerel ağa bağlanın ve sonra StorSimple cihaz Yöneticisi hizmeti ile kaydedilecek şekilde yapılandırmak için kullanılan bir web tabanlı bir kullanıcı Arabirimi. 
* [Komut satırı arabirimi](#command-line-interface) – sanal dizi destek oturumu başlatmak için kullanabileceğiniz bir Windows PowerShell arabirimi.
  Aşağıdaki bölümlerde daha ayrıntılı bu bileşenlerin her birini açıklayan ve nasıl çözüm verileri düzenler, depolama ayırır ve Depolama Yönetimi ve veri koruması kolaylaştıran açıklar.

### <a name="virtual-array"></a>Sanal dizi
Sanal dizinin birincil depolama sağlayan, bulut depolama ile iletişim yönetir ve cihazda depolanan tüm verileri gizliliğini ve güvenlik sağlamaya yardımcı olur bir tek düğümlü depolama çözümüdür.

Sanal dizinin indirme için kullanılabilir bir modeli mevcuttur. Sanal dizinin depolama bulut maksimum kapasitesi 6.4 TB'lık aygıtla (bir temel alınan depolama gereksinimi 8 TB) ve 64 TB dahil olmak üzere sahiptir. 

Sanal dizinin aşağıdaki özelliklere sahiptir:

* Uygun maliyetli olması. Bu bağdaştırıcılar var olan sanallaştırma altyapınızın kullanır ve var olan Hyper-V veya VMware hiper yöneticide dağıtılabilir.
* Veri merkezinde yer alır ve bir iSCSI sunucusu veya bir dosya sunucusu yapılandırılabilir. 
* Bulut ile tümleşiktir.
* Yedeklemeler, olağanüstü durum kurtarma kolaylaştırmak ve öğe düzeyinde Kurtarma (ILR) basitleştirmek bulutta depolanır. 
* Yalnızca bunları fiziksel cihaza uygulanacak şekilde sanal dizisine güncelleştirmelerini uygulayabilirsiniz.

> [!NOTE]
> Sanal bir dizi genişletilemiyor. Bu nedenle, sanal dizinin oluşturduğunuzda yeterli depolama alanı hazırlamak önemlidir. 
> 
> 

### <a name="storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmeti
Microsoft Azure StorSimple StorSimple depolama merkezi olarak yönetmenizi sağlayan StorSimple cihaz Yöneticisi hizmeti bir web tabanlı kullanıcı arabirimi sağlar. StorSimple cihaz Yöneticisi hizmeti, şu görevleri gerçekleştirmek için kullanabilirsiniz:

* Birden çok StorSimple sanal dizisi tek bir hizmetten yönetin. 
* Yapılandırmak ve StorSimple sanal diziler için güvenlik ayarlarını yönetin. (Şifreleme bulutta Microsoft Azure API bağlıdır.)
* Depolama hesabının kimlik bilgilerini ve özelliklerini yapılandırın.
* Yapılandırın ve birimler veya paylaşımlar yönetin.
* Yedekleme ve birimler veya paylaşımlar verilerini geri yükleyin.
* Performans İzleyici.
* Sistem ayarlarını gözden geçirin ve olası sorunları tanımlar.

StorSimple cihaz Yöneticisi hizmeti, günlük yönetim sanal dizinin gerçekleştirmek için kullanın.

Daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-virtual-array-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Yerel web kullanıcı arabirimi
Sanal dizinin tek seferlik yapılandırma ve StorSimple Aygıt Yöneticisi'ni hizmetiyle aygıt kaydı için kullanılan bir web tabanlı bir kullanıcı Arabirimi içerir. Kapatmak ve sanal dizinin yeniden tanılama testleri çalıştırmak yazılım güncelleştirme cihaz Yöneticisi parolasını değiştirme sistem günlüklerini görüntülemek ve bir hizmet isteği dosya için Microsoft Support başvurun için kullanabilirsiniz. 

Web tabanlı kullanıcı arabirimini kullanma hakkında daha fazla bilgi için Git [StorSimple sanal dizinizi yönetmek için web tabanlı kullanıcı arabirimini kullanma](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Komut satırı arabirimi
Dahil Windows PowerShell arabirimi, ve sanal dizinizi karşılaşabileceğiniz sorunları gidermek Yardım Microsoft Support destek oturumla başlatmasını sağlar.

## <a name="storage-management-technologies"></a>Depolama yönetimi teknolojileri
Sanal dizinin ve diğer bileşenleri ek olarak, StorSimple çözüm önemli verileri hızlı erişim sağlayan, depolama tüketimini azaltabilir ve sanal dizinizi depolanmış verileri korumak için aşağıdaki yazılım teknolojilerini kullanır:

* [Otomatik depolama katmanlama](#automatic-storage-tiering) 
* [Yerel olarak sabitlenmiş paylaşımları ve birimler](#locally-pinned-shares-and-volumes)
* [Yinelenenleri kaldırma ve veri sıkıştırma katmanlı veya buluta yedeklendi](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
* [Zamanlanmış ve isteğe bağlı yedeklemeler](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Otomatik depolama katmanlama
Sanal dizinin sanal dizinin ve bulut arasında depolanan verileri yönetmek için yeni bir katmanlama mekanizması kullanır. Yalnızca iki katmanı vardır: yerel sanal dizinin ve Azure bulut depolama. StorSimple sanal dizinin veri geçerli kullanımı, geçerlilik süresi ve ilişkileri diğer veri izleyen bir ısı Haritası göre katmanları içine otomatik olarak düzenler. Daha az etkin ve etkin olmayan verileri otomatik olarak buluta geçiş sırasında (yeni) yerel olarak depolanan en etkin olan verileri. (Tüm yedeklemeler bulutta depolanır.) StorSimple ayarlar ve verileri yeniden düzenler ve kullanım düzenlerini olarak depolama atamalarını değiştirin. Örneğin, bazı bilgiler zaman içinde daha az etkin hale gelebilir. Aşamalı olarak daha az etkin hale geldiğinde, çıkışı buluta katmanlı. Bu aynı verileri tekrar etkin hale gelirse, bu, Depolama dizisinde katmanlı.

Belirli katmanlı paylaşım veya birim için verileri kendi yerel katmanı alanı garanti edilmez. (yaklaşık alanın % 10 toplam sağlanan o paylaşımı veya birimi için). Bu paylaşım veya birim için sanal dizisindeki kullanılabilir depolama alanı azaltır, ancak bir paylaşımı veya birimi katmanlama katmanlama ihtiyaçlarını diğer paylaşımları veya birimler tarafından etkilenmez, sağlar. Bu nedenle bir paylaşımı veya birimi çok meşgul bir iş yükünü buluta diğer iş yüklerini zorlayamaz. 

![Otomatik depolama katmanlama](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Yerel olarak sabitlenmiş bir birim belirtebilirsiniz, bu durumda veriler sanal dizi kalır ve hiçbir zaman buluta katmanlı. Daha fazla bilgi için Git [yerel olarak sabitlenmiş paylaşımları ve birimler](#locally-pinned-shares-and-volumes).
> 
> 

### <a name="locally-pinned-shares-and-volumes"></a>Yerel olarak sabitlenmiş paylaşımları ve birimler
Uygun paylaşımları ve yerel olarak sabitlenmiş birimler oluşturabilirsiniz. Bu özellik, kritik uygulamalar tarafından gerekli veri sanal dizisinde kalmasını ve hiçbir zaman buluta katmanlı sağlar. Yerel olarak sabitlenmiş paylaşımları ve birimler aşağıdaki özelliklere sahiptir: 

* Bunlar bulut gecikmeleri veya bağlantı sorunları tabi değildir.
* Bunlar, yedekleme ve olağanüstü durum kurtarma özelliklerinden StorSimple bulut hala yararlanır.

Yerel olarak sabitlenmiş bir paylaşımı veya katmanlı gibi birim veya bir katmanlı paylaşımı geri yükleyebilirsiniz veya toplu olarak yerel olarak sabitlenmiş. 

Yerel olarak sabitlenmiş birimleri hakkında daha fazla bilgi için Git [birimleri yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-virtual-array-manage-volumes.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Yinelenenleri kaldırma ve veri sıkıştırma katmanlı veya buluta yedeklendi
StorSimple daha fazla bulut depolama gereksinimlerini azaltmak için yinelenenleri kaldırma ve verileri sıkıştırmayı kullanır. Yinelenenleri kaldırma genel depolanan veri kümesi içinde artıklık ortadan kaldırarak depolanan veri miktarını azaltır. Bilgi değiştikçe StorSimple değişmeyen verilerin yoksayar ve yalnızca değişiklikleri yakalar. Ayrıca, StorSimple tanımlama ve yinelenen bilgileri kaldırma depolanan veri miktarını azaltır. 

> [!NOTE]
> Sanal dizi depolanan verileri yinelenenleri kaldırılmış veya sıkıştırılmış değil. Tüm yinelenenleri kaldırma ve sıkıştırma verileri buluta yalnızca gönderilmeden önce gerçekleşir.
> 
> 

### <a name="scheduled-and-on-demand-backups"></a>Zamanlanmış ve isteğe bağlı yedeklemeler
StorSimple veri koruma özelliklerine isteğe bağlı yedeklemeler oluşturmanızı sağlar. Ayrıca, bir varsayılan yedekleme zamanlaması günlük yedek yedeklenen verileri sağlar. Yedeklemeler, bulutta depolanan artımlı anlık görüntüleri biçiminde alınır. Son yedeklemeden sonra yalnızca değişiklikleri kaydedin, anlık görüntü oluşturulur ve hızlı bir şekilde geri. Bu anlık görüntüler, çünkü bunlar ikincil depolama sistemleri (örneğin, bant yedekleme) değiştirin ve, gerekirse, veri merkeziniz veya diğer siteler için geri yüklemenize izin olağanüstü durum kurtarma senaryolarında kritik düzeyde önemli olabilir.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [sanal dizinin portal hazırlama](storsimple-virtual-array-deploy1-portal-prep.md).

