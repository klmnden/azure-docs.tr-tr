---
title: StorSimple 8000 series çözümüne genel bakış | Microsoft Docs
description: StorSimple katmanlama, cihaz, sanal cihaz, hizmetleri ve depolama yönetimi açıklar ve StorSimple içinde kullanılan anahtar terimlerin kullanıma sunar.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: v-sharos@microsoft.com
ms.openlocfilehash: 63906e65acb8e8aa836e6e59714bddca24ea21eb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60630210"
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000 serisi: bir hibrit bulut depolaması çözümü
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple, şirket içi cihazlar ve Microsoft Azure bulut depolama arasındaki Depolama görevlerini yöneten tümleşik bir çözüm Hoş Geldiniz. StorSimple, birçok sorunu ve Kurumsal Depolama ve veri korumasıyla ilişkili giderlerini ortadan kaldıran bir verimli, Hesaplı ve kolayca yönetilebilir depolama alanı ağı (SAN) çözümüdür. Özel StorSimple 8000 serisi cihaz, bulut hizmetleriyle tümleştirilen ve bir dizi yönetim aracı için bulut depolama da dahil olmak üzere tüm kurumsal depolamanın sorunsuz bir görünümünü sağlar. (Microsoft Azure Web sitesinde yayınlanan StorSimple dağıtım bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. Bir StorSimple 5000/7000 Serisi cihaz kullanıyorsanız, Git [StorSimple Yardım](http://onlinehelp.storsimple.com/).)

StorSimple kullanan [depolama katmanlamayı](#automatic-storage-tiering) depolanan veriler üzerinde çeşitli depolama ortamı yönetmek için. Geçerli çalışma kümesi şirket içinde depolanan katı hal sürücülerine (SSD) üzerinde olduğundan, daha az sıklıkta kullanılan verileri sabit disk sürücülerinin (HDD'ler) depolanır ve Arşiv verileri buluta nasıl gönderilir. Ayrıca, verileri kullanan depolama miktarını azaltmak için yinelenen verileri kaldırma ve sıkıştırma StorSimple kullanır. Daha fazla bilgi için Git [yinelenenleri kaldırma ve sıkıştırma](#deduplication-and-compression). Diğer önemli terimler ve kavramlar StorSimple 8000 serisi belgelerinde kullanılan tanımları için Git [StorSimple terminolojisi](#storsimple-terminology) bu makalenin sonunda.

Depolama Yönetimi ek olarak StorSimple veri koruma özellikleri, isteğe bağlı oluşturmanıza olanak sağlar ve zamanlanmış yedeklemeler ve bunları yerel olarak veya bulutta depolayın. Yedeklemeleri bunlar oluşturulabilir ve hızlı bir şekilde geri, yani biçiminde artımlı anlık görüntü alınır. İkincil depolama sistemleri (örneğin, bant yedekleme) değiştirin ve veri Merkezinize veya diğer sitelere gerekirse geri yüklemenize olanak tanımak için bulut anlık görüntüleri olağanüstü durum kurtarma senaryolarında kritik düzeyde önemli olabilir.

![video simgesi](./media/storsimple-overview/video_icon.png) Microsoft Azure StorSimple hızlı bir giriş için videoyu izleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>StorSimple neden kullanmalısınız?
Aşağıdaki tabloda, Microsoft Azure StorSimple sağlayan başlıca avantajlarından bazıları açıklanmaktadır.

| Özellik | Avantaj |
| --- | --- |
| Saydam tümleştirme |İSCSI protokolü, veri depolama olanakları görünmez bağlamak için kullanır. Bu, veri merkezi, bulutta depolanan verilerin sağlar veya tek bir konumda depolanması için uzak sunucularda görünür. |
| Daha düşük depolama maliyetleri |Geçerli taleplerini karşılamak için yeterli yerel veya Bulut depolama alanı ayırır ve bulut depolama yalnızca gerekli olduğunda genişletir. Bu ek depolama gereksinimleri ve harcama yedekli sürümleri aynı verileri (kaldırmayı) ortadan kaldırır ve sıkıştırma kullanarak azaltır. |
| Basitleştirilmiş Depolama Yönetimi |Sistem, yapılandırmak ve depolanan verileri şirket içinde bir uzak sunucuda ve bulutta yönetmek için yönetim araçları sağlar. Ayrıca, yedeklemeyi yönetme ve bir Microsoft Yönetim Konsolu (MMC) ek bileşeninden işlevleri geri yükleyebilirsiniz.|
| Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk |Genişletilmiş kurtarma süresi gerektirmez. Bunun yerine, gerektiğinde veri geri yükler. Başka bir deyişle, normal işlemleri en az kesinti ile devam edebilirsiniz. Ayrıca, yedekleme zamanlamaları ve veri saklama ilkeleri yapılandırabilirsiniz. |
| Veri taşınabilirliği |Microsoft Azure bulut hizmetlerine yüklenen veri kurtarma ve geçiş amaçları doğrultusunda diğer sitelerden de erişilebilir. Ayrıca, StorSimple, StorSimple Cloud Appliance'lar Microsoft Azure'da çalışan sanal makineler (VM'ler) yapılandırmak için kullanabilirsiniz. VM'ler, sonra test veya kurtarma amacıyla depolanan verilere erişmek için sanal cihazları kullanabilirsiniz. |
| İş sürekliliği |StorSimple, StorSimple 8000 serisi bir cihaza için kendi verilerini geçirmek 5000-7000 Serisi kullanıcıların sağlar. |
| Azure kamu Portalı'nda kullanılabilirlik |StorSimple, Azure kamu Portalı'nda kullanılabilir. Daha fazla bilgi için [kamu Portalı'nda şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-gov-u2.md). |
| Veri koruma ve kullanılabilirlik |StorSimple 8000 serisi, bölgesel olarak yedekli depolama (ZRS), yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS) ek olarak destekler. Başvurmak [bu makalede Azure depolama yedekliliği seçenekleri](https://azure.microsoft.com/documentation/articles/storage-redundancy/) ZRS Ayrıntılar için. |
| Kritik uygulamalar için destek |StorSimple, yerel olarak sabitlenmiş uygun birimlerini tanımlayın sırayla Kritik uygulamalar tarafından gerekli verileri buluta katmanlanmış değil sağlayan sağlar. Yerel olarak sabitlenmiş birimler bulut gecikmeleri veya bağlantı sorunları tabi değildir. Yerel olarak sabitlenmiş birimleri hakkında daha fazla bilgi için bkz. [birimleri yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manage-volumes-u2.md). |
| Düşük gecikme süresi ve yüksek performans |Yüksek performanslı Azure premium depolama, düşük gecikme süresi özellikleri yararlanarak bulut Gereçleri oluşturabilirsiniz. StorSimple premium bulut Gereçleri hakkında daha fazla bilgi için bkz: [Dağıt ve azure'da bir StorSimple Cloud Appliance'ı yönetme](storsimple-8000-cloud-appliance-u2.md). |


## <a name="storsimple-components"></a>StorSimple bileşenleri
Microsoft Azure StorSimple çözümü, aşağıdaki bileşenleri içerir:

* **Microsoft Azure StorSimple cihaz** – SSD ve HDD, yedekli denetleyicileri ve otomatik yük devretme özellikleri ile birlikte içeren bir şirket içi karma depolama dizisi. Depolama katmanlamayı, şu anda kullanılan (veya sık erişimli) veri (aygıt ya da şirket içi sunucular için), yerel depolama bulut için daha az sık kullanılan veri taşırken yerleştirerek denetleyicilerini yönetin.
* **StorSimple Cloud Appliance** – olarak da bilinir, StorSimple sanal Gereci mimarisi çoğaltır StorSimple cihaz yazılımı sürümü ve fiziksel karma depolama cihazı çoğu yeteneklerini budur. StorSimple Cloud Appliance, bir Azure sanal makinesinde tek bir düğümde çalışır. Azure premium depolama avantajlarından yararlanın, premium sanal cihazları ve sonrasında güncelleştirme 2'de kullanılabilir.
* **StorSimple cihaz Yöneticisi hizmeti** bir uzantısı olan StorSimple cihazı veya StorSimple Cloud Appliance tek bir web arabiriminden yönetmenizi sağlayan Azure portalı. StorSimple cihaz Yöneticisi hizmeti oluşturma ve hizmetleri yönetmek, görüntülemek ve cihazları yönetmek, uyarıları görüntülemek, işlemlerle birimleri yönetin ve görüntülemek ve yedekleme ilkelerini ve yedekleme kataloğunu yönetmek için kullanabilirsiniz.
* **StorSimple için Windows PowerShell** – StorSimple cihazı yönetmek için kullanabileceğiniz bir komut satırı arabirimi. StorSimple için Windows PowerShell, StorSimple Cihazınızı kaydetmek, Cihazınızda ağ arabirimini yapılandırın, belirli türden güncelleştirmeler yüklemeniz, destek oturumu erişerek cihazınızın sorunlarını olanak sağlar ve cihazı değiştirmek özelliklere sahiptir durumu. Seri konsola bağlanmak veya Windows PowerShell uzaktan iletişimini kullanarak StorSimple için Windows PowerShell erişebilirsiniz.
* **Azure PowerShell, StorSimple cmdlet'leri** – komut satırından hizmet düzeyi ve geçiş görevlerinizi otomatikleştirmenizi sağlayan Windows PowerShell cmdlet'leri koleksiyonu. StorSimple için Azure PowerShell cmdlet'leri hakkında daha fazla bilgi için Git [cmdlet başvurusu](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0#azure).
* **StorSimple Snapshot Manager** – bir MMC ek bileşeni, uygulamayla tutarlı yedeklemeler oluşturmak için birim gruplarını ve Windows birim gölge kopyası hizmeti kullanır. Ayrıca, StorSimple Snapshot Manager yedekleme zamanlamaları ve kopya oluşturmak veya birimleri geri yüklemek için kullanabilirsiniz.
* **SharePoint için StorSimple bağdaştırıcısı** – Microsoft Azure StorSimple depolama ve veri korumasını SharePoint sunucusuna şeffaf bir şekilde genişleten bir aracı grupları, SharePoint Merkezi StorSimple depolama görüntülenebilen ve yönetilebilen yaparken Yönetim Portalı.

Aşağıdaki diyagramda Microsoft Azure StorSimple mimarisini ve bileşenlerini üst düzey bir görünümünü sağlar.

![StorSimple mimarisi](./media/storsimple-overview/overview-big-picture.png)

Aşağıdaki bölümlerde, bu bileşenlerin daha ayrıntılı açıklanmaktadır ve nasıl çözüm veri düzenler, depolama alanı ayırır ve Depolama Yönetimi ve veri korumasını kolaylaştırır açıklar. Son bölümde bazı önemli terimler ve StorSimple bileşenleri ve bunların yönetimi ile ilgili kavramları için tanımları sağlar.

## <a name="storsimple-device"></a>StorSimple cihaz
Microsoft Azure StorSimple, birincil depolama ve iSCSI depolanan verilere erişim sağlayan bir şirket içi karma depolama dizisini cihazdır. Bulut depolama ile iletişimi yönetir ve Microsoft Azure StorSimple çözümünde depolanan tüm verileri gizliliğini ve güvenlik sağlamaya yardımcı olur.

StorSimple cihazı SSD ve sabit disk sürücüleri HDD'ler yanı sıra, kümeleme ve otomatik yük devretme için destek içerir. Bu, paylaşılan bir işlemci, paylaşılan depolamaya ve iki yansıtılmış denetleyicileri içerir. Her denetleyici aşağıdakileri sağlar:

* Bir konak bilgisayar için bağlantı
* Yerel ağa (LAN) bağlanmak için en fazla altı ağ bağlantı noktaları
* Donanım izleme
* Güç kesintiye olmasa bile, bilgileri saklar, geçici olmayan rasgele erişim belleği (NVRAM)
* Küme duyarlı güncelleştirmelerin en düşük olması, yazılım güncelleştirmelerini bir yük devretme kümesindeki sunucularda yönetmek için güncelleştiriliyor veya hizmet kullanılabilirliği üzerinde hiçbir etkisi
* Küme hizmeti gibi bir arka uç, yüksek düzeyde kullanılabilirlik sağladığınızdan ve bir HDD veya SSD hata verirse veya oluşabilecek tüm olumsuz etkileri en aza küme, işlev, çevrimdışı duruma

Yalnızca bir denetleyici zaman içinde herhangi bir noktada etkindir. İkinci denetleyici, etkin denetleyiciyi başarısız olursa, otomatik olarak etkinleşir.

Daha fazla bilgi için Git [StorSimple donanım bileşenleri ve durum](storsimple-8000-monitor-hardware-status.md).

## <a name="storsimple-cloud-appliance"></a>StorSimple Cloud Appliance
StorSimple fiziksel karma depolama cihazın özelliklerini ve mimarisini çoğaltan bir bulut Gereci oluşturmak için kullanabilirsiniz. StorSimple Cloud Appliance'ı (diğer adıyla StorSimple sanal Gereci), bir Azure sanal makinesinde tek bir düğümde çalışır. (Bir bulut Gereci, yalnızca bir Azure sanal makinesi üzerinde oluşturulabilir. Bir StorSimple cihazı veya bir şirket içi sunucuda oluşturamazsınız.)

Bulut Gereci aşağıdaki özelliklere sahiptir:

* Bu, bir fiziksel gereç gibi davranır ve buluttaki sanal makineler için bir iSCSI arabirimi sunabilir.
* Bulut Gereçleri, sınırsız sayıda bulutta oluşturun ve bunları açıp gerektiği şekilde açın.
* Şirket içi ortamlarda olağanüstü durum kurtarma, geliştirme ve test senaryoları benzetimini yardımcı olabilir ve yedeklemelerden öğe düzeyinde alma yardımcı olabilir.

StorSimple Cloud Appliance iki modellerinde kullanılabilir: (önceden 1100 model olarak biliniyordu) 8010 cihaz ve 8020 cihaz. 8010 cihaz 30 TB kapasiteye sahiptir. Azure premium depolama yararlanır, 8020 cihaz 64 TB'lık maksimum kapasiteye sahiptir. (Standart depolama Hdd'lerde veri depoladığı yerel katmanlarda, Azure premium depolama veri Ssd'de depolar.) Premium depolama kullanmak için bir Azure premium depolama hesabına sahip olmaları gerektiğini unutmayın.

StorSimple Cloud Appliance hakkında daha fazla bilgi için Git [Dağıt ve azure'da bir StorSimple Cloud Appliance'ı yönetme](storsimple-8000-cloud-appliance-u2.md).

## <a name="storsimple-device-manager-service"></a>StorSimple Device Manager hizmeti
Microsoft Azure StorSimple, merkezi olarak veri merkezinin yönetimi ve bulut depolama sağlayan bir web tabanlı kullanıcı arabirimi (StorSimple cihaz Yöneticisi hizmeti) sağlar. StorSimple cihaz Yöneticisi hizmeti, aşağıdaki görevleri gerçekleştirmek için kullanabilirsiniz:

* StorSimple cihazlar için sistem ayarları yapılandırın.
* Yapılandırmak ve StorSimple cihazlar için güvenlik ayarlarını yönetin.
* Bulut kimlik bilgilerini ve özelliklerini yapılandırın.
* Yapılandırma ve bir sunucuda birimleri yönetin.
* Birim gruplarını yapılandırın.
* Yedekleme ve veri geri yükleme.
* Performansını izleme.
* Sistem ayarları gözden geçirin ve olası sorunları tanımlayın.

StorSimple cihaz Yöneticisi hizmeti, sistem kesinti gibi ilk kurulum ve güncelleştirmelerin yüklenmesini gerektiren dışındaki tüm yönetim görevlerini gerçekleştirmek için kullanabilirsiniz.

Daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell
StorSimple için Windows PowerShell, oluşturma ve Microsoft Azure StorSimple hizmetindeki yönetmek ve ayarlamak ve StorSimple cihazları izlemek için kullanabileceğiniz bir komut satırı arabirimi sağlar. Bu bir Windows PowerShell tabanlı komut satırı StorSimple Cihazınızı yönetmek için adanmış cmdlet'leri içeren arabirimidir. StorSimple için Windows PowerShell olanak tanıyan özellikler vardır:

* Bir cihazı kaydedin.
* Bir cihaz üzerinde ağ arabirimini yapılandırın.
* Belirli bir türdeki güncelleştirmeleri yükleyin.
* Cihazınızı destek oturumu erişerek sorunlarını giderin.
* Cihaz durumunu değiştirin.

(Cihaza doğrudan bağlı bir ana bilgisayara) bir seri konsoldan veya uzaktan StorSimple için Windows PowerShell erişebilir Windows PowerShell uzaktan iletişimini kullanarak. İlk cihaz kaydı gibi StorSimple görevleri için bazı Windows PowerShell üzerinde seri konsol yalnızca yapılabilir unutmayın.

Daha fazla bilgi için Git [Cihazınızı yönetmek StorSimple için Windows PowerShell kullanarak](storsimple-8000-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell, StorSimple cmdlet'leri
Azure PowerShell StorSimple cmdlet'leri, komut satırından hizmet düzeyi ve geçiş görevlerinizi otomatikleştirmenizi sağlayan Windows PowerShell cmdlet'leri koleksiyonudur. StorSimple için Azure PowerShell cmdlet'leri hakkında daha fazla bilgi için Git [cmdlet başvurusu](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
StorSimple Snapshot Manager'ı olan tutarlı, oluşturmak için kullanabileceğiniz bir Microsoft Yönetim Konsolu (MMC) ek bileşenini yerel yedek kopyalarını zaman içinde nokta ve bulut veri. Ek bileşenini Windows Server tabanlı konakta çalışır. StorSimple Snapshot Manager için kullanabilirsiniz:

* Yapılandırma, yedekleme ve birimleri silin.
* Birim gruplarını emin olmak için yapılandırma uygulamayla tutarlı yedeklenen verileri.
* Verileri önceden belirlenmiş bir zamanlamayla yedeklenen ve (yerel olarak veya bulutta) belirlenen bir konumda depolanan yedekleme ilkelerini yönetme.
* Birimler ve tek tek dosyaları geri yükleyin.

Yedekleri tam yedeklerden daha az depolama alanı gerektirir ve son anlık görüntü alındıktan sonra yalnızca değişiklikleri kaydedin. anlık görüntü olarak yakalanır. Yedekleme zamanlamaları oluşturabilir veya gerektiğinde anında yedek alabilir. Ayrıca, StorSimple Snapshot Manager bekletme ilkeleri oluşturmak için kaç anlık görüntüleri kaydedilecek denetleyen kullanabilirsiniz. Sonradan bir yedekleme, StorSimple Snapshot Manager sağlar verileri geri yüklemeniz gerekirse katalog yerel veya Bulut anlık görüntüleri seçin. 

Gerektiğinde bir olağanüstü durum oluşursa veya başka bir nedenle verileri geri yüklemeniz gerekiyorsa, StorSimple Snapshot Manager, artımlı olarak geri yükler. Bir dosyayı geri yükleme, ekipman değiştirin ya da başka bir siteye taşıma işlemlerini tüm sistemin çökmesine Kapat veri geri yükleme gerektirmez.

Daha fazla bilgi için Git [StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Bağdaştırıcısı
Microsoft Azure StorSimple, SharePoint, SharePoint sunucu grupları için StorSimple depolama ve veri koruma özelliklerini şeffaf bir şekilde genişleten isteğe bağlı bir bileşen için StorSimple bağdaştırıcısını içerir. Bağdaştırıcı, BLOB'ları Microsoft Azure StorSimple sistemi tarafından yedeklenen bir sunucuya taşımanızı sağlayan Uzak Blob Depolama (KKY) sağlayıcı ve SQL Server KKY özelliği ile çalışır. Microsoft Azure StorSimple sonra BLOB verilerini yerel olarak veya bulutta kullanıma göre depolar.

SharePoint için StorSimple bağdaştırıcısını SharePoint Yönetim Merkezi portalında yönetilir. Sonuç olarak, SharePoint Yönetim Merkezi kalır ve tüm depolama SharePoint çiftliğinde gibi görünüyor.

Daha fazla bilgi için Git [SharePoint için StorSimple bağdaştırıcısı](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Depolama yönetimi teknolojileri
Adanmış StorSimple cihazı, sanal cihaz ve diğer bileşenleri ek olarak, Microsoft Azure StorSimple veri hızlı erişim sağlar ve depolama tüketimini azaltmak için aşağıdaki yazılım teknolojilerini kullanır:

* [Otomatik depolama katmanlamayı](#automatic-storage-tiering) 
* [ölçülü kaynak sağlama](#thin-provisioning) 
* [Yinelenenleri kaldırma ve sıkıştırma](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Otomatik depolama katmanlamayı
Microsoft Azure StorSimple, otomatik olarak geçerli kullanım, geçerlilik süresi ve diğer veriler ilişkisini göre mantıksal katmanları verileri düzenler. Daha az etkin ve etkin olmayan verileri otomatik olarak buluta geçiş sırasında en etkin olan verileri yerel olarak depolanır. Aşağıdaki diyagramda, bu depolama yaklaşımı gösterir.

![StorSimple depolama katmanları](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Hızlı erişimi etkinleştirmek üzere StorSimple çok etkin verilerin (sık erişimli veriler) StorSimple cihaz Ssd'de depolar. Nadiren kullanılan verileri depolar (normal veri) cihazı veya veri merkezinde sunucularda HDD'ler üzerinde. Etkin olmayan verileri, yedekleme verilerini taşır ve korunan veriler için arşiv veya uyumluluk amacıyla buluta. 

> [!NOTE]
> Güncelleştirme 2 veya sonraki sürümlerde, yerel olarak sabitlenmiş bir birim belirtebilirsiniz, bu durumda, veriler yerel cihazda kalır ve değil buluta katmanlı. 


StorSimple, ayarlar ve verileri yeniden düzenler ve kullanım biçimlerini depolama atamalar değiştirin. Örneğin, bazı bilgiler zaman içinde daha az etkin hale gelebilir. Aşamalı olarak daha az etkin hale gelir, SSD HDD'ler ve ardından buluta geçirilir. Bu aynı verileri tekrar etkin hale gelirse, depolama cihazına geçirilir.

Depolama katmanlama işlemi aşağıdaki gibidir:

1. Bir Sistem Yöneticisi bir Microsoft Azure bulut depolama hesabı ayarlar.
2. Yönetici seri konsol ve StorSimple cihaz Yöneticisi hizmetini (Azure Portalı'nda çalışan) cihaz ve dosya sunucusunu yapılandırmak için birimleri ve veri koruma ilkeleri oluşturma kullanır. Şirket içi makineleri (örneğin, dosya sunucuları), StorSimple cihazınıza erişim hakkı Internet küçük bilgisayar sistemi Arabirimi'ni (iSCSI) kullanın.
3. Başlangıçta, StorSimple cihaz hızlı SSD katmanı üzerinde verileri depolar.
4. SSD katmanı kapasitesine yaklaştığında StorSimple deduplicates en eski veri blokları sıkıştırır ve bunları HDD katmanına taşır.
5. HDD katmanı yaklaşımları kapasitesini olarak StorSimple en eski veri bloklarını şifreler ve Microsoft Azure depolama hesabına HTTPS üzerinden güvenli bir şekilde gönderir.
6. Microsoft Azure bir olağanüstü durum oluşması durumunda verilerin kurtarılabilmesini sağlamak, merkezinde ve uzak bir veri merkezinde verilerin birden çok kopyasını oluşturur.
7. Dosya sunucusu, bulutta depolanan veriler istediğinde, StorSimple sorunsuz bir şekilde döndürür ve StorSimple cihaz SSD katmanına bağlı bir kopyayı depolar.

#### <a name="how-storsimple-manages-cloud-data"></a>StorSimple bulut verileri nasıl yönetir?

StorSimple, tüm anlık görüntüleri ve birincil veri (ana bilgisayar tarafından yazılan veriler) müşteri verilerini deduplicates. Yinelenenleri kaldırma için depolama verimliliği harika olsa da, "bulutta karmaşık nedir" Soru kolaylaştırır. Birincil katmanlı verileri ve anlık görüntü verilerini birbiriyle çakışıyor. Tek bir öbek bulutta veri katmanlı birincil veri kullanılabilir ve birkaç anlık görüntüler görüntülenerek de başvuru. Her bulut anlık görüntüsü, bu anlık görüntü silinene kadar tüm zaman içinde nokta verilerin bir kopyasını buluta kilitli sağlar.

Başvuru verileri için olduğunda, veriler yalnızca buluttan silinir. Biz StorSimple cihazı ve sonra birincil veri silebilir bir bulut anlık görüntüsü tüm verilerin alırsa, biz gibi görür _birincil veri_ hemen bırakın. _Bulut veri_ katmanlı veriler ve yedeklemeler içeren aynı kalır. Bulut veri yine de başvuran bir anlık görüntü olduğundan budur. Sonra bulut anlık görüntüsü silinir (ve aynı verilere başvurduğu diğer tüm anlık görüntü), bulut tüketimi bırak. Biz bulut veri kaldırmadan önce anlık görüntü yok yine de bu verilere başvuruda denetleyin. Bu işlem çağrılırken _çöp toplama_ ve cihazda çalıştıran bir arka plan hizmeti. Bulut verilerini hemen değil silmeden önce bu verileri diğer başvurular atık toplama hizmetinin denetler. Çöp toplama hızı, anlık görüntüler ve toplam veri toplam sayısına bağlıdır. Genellikle, bulut verilerini bir hafta içinde temizlenir.


### <a name="thin-provisioning"></a>ölçülü kaynak sağlama
Ölçülü kaynak sağlama fiziksel kaynakları aşmayı kullanılabilir depolama alanı görünür bir sanallaştırma teknolojisidir. Önceden yeterli depolama alanı ayırmak yerine, StorSimple ölçülü kaynak sağlama geçerli gereksinimlerini karşılamak için yeterli alan ayırmak için kullanır. StorSimple artırabilir veya azaltabilirsiniz değişen taleplerini karşılamak üzere bulut depolama çünkü bu yaklaşım bulut depolama esnek yapısı kolaylaştırır.

> [!NOTE]
> Yerel olarak sabitlenmiş birimlerin ölçülü kaynak sağlanan değil. Birim oluşturulduğunda, yalnızca yerel bir birime ayrılmış depolama sunabilen sağlanır.


### <a name="deduplication-and-compression"></a>Yinelenenleri kaldırma ve sıkıştırma
Microsoft Azure StorSimple, yinelenenleri kaldırma ve veri sıkıştırma daha fazla depolama gereksinimlerini azaltmak için kullanır.

Yinelenenleri kaldırma genel yedeklilik depolanan veri kümesindeki ortadan kaldırarak depolanan veri miktarını azaltır. Bilgi değiştikçe StorSimple değiştirilmemiş verilerini yoksayar ve yalnızca değişiklikleri yakalar. Ayrıca, StorSimple tanımlama ve gereksiz bilgileri kaldırılıyor depolanan verilerin miktarını azaltır. 

> [!NOTE]
> Verileri yerel olarak sabitlenmiş birim üzerinde yinelenenleri kaldırılmış veya sıkıştırılmış desteklenmez. Ancak, yerel olarak sabitlenmiş birimlerin yedekleri yinelenenler sıkıştırılmış ve.


## <a name="storsimple-workload-summary"></a>StorSimple iş yükü özeti
Desteklenen StorSimple iş yüklerinin bir özeti aşağıda tabloda verilmiştir.

| Senaryo | İş yükü | Desteklenen | Kısıtlamalar | Version |
| --- | --- | --- | --- | --- |
| İş Birliği |Dosya Paylaşımı |Evet | |Tüm sürümler |
| İş Birliği |Dağıtılmış bir dosya paylaşımı |Evet | |Tüm sürümler |
| İş Birliği |SharePoint |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Güncelleştirme 2 ve üzeri |
| Arşivleme |Basit dosya arşivleme |Evet | |Tüm sürümler |
| Sanallaştırma |Sanal makineler |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Güncelleştirme 2 ve üzeri |
| Database |SQL |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Güncelleştirme 2 ve üzeri |
| Kameralı |Kameralı |Evet* |StorSimple cihaz, yalnızca bu iş yüküne adanıp olduğunda desteklenen |Güncelleştirme 2 ve üzeri |
| Backup |Birincil hedef yedekleme |Evet* |StorSimple cihaz, yalnızca bu iş yüküne adanıp olduğunda desteklenen |Güncelleştirme 3 ve üzeri |
| Backup |İkincil hedef yedekleme |Evet* |StorSimple cihaz, yalnızca bu iş yüküne adanıp olduğunda desteklenen |Güncelleştirme 3 ve üzeri |

*Evet&#42; -çözüm kılavuzları ve sınırlamaları uygulanabilir.*

Aşağıdaki iş yükleri, StorSimple 8000 serisi cihazlar tarafından desteklenmez. Bu iş yükleri, StorSimple dağıttıysanız, desteklenmeyen bir yapılandırma neden olur.

* Tıbbi görüntüleme
* Exchange
* VDI
* Oracle
* SAP
* Big Data
* İçerik dağıtımı
* SCSI önyükleme

Aşağıdaki desteklenen StorSimple altyapı bileşenlerini bir listesidir.

| Senaryo | İş yükü | Desteklenen | Kısıtlamalar | Version |
| --- | --- | --- | --- | --- |
| Genel |Express Route |Evet | |Tüm sürümler |
| Genel |DataCore FC |Evet* |DataCore SANsymphony ile desteklenen |Tüm sürümler |
| Genel |DFSR |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Tüm sürümler |
| Genel |Dizinleme |Evet* |Katmanlı birimler için yalnızca meta veri dizinleme desteklenir (veri).<br>Yerel olarak sabitlenmiş birimler için dizin oluşturma tamamlandı desteklenir. |Tüm sürümler |
| Genel |Virüsten koruma |Evet* |Katmanlı birimler için yalnızca tarama açık ve Kapat desteklenir.<br> Yerel olarak sabitlenmiş birimler için tam tarama desteklenir. |Tüm sürümler |

*Evet&#42; -çözüm kılavuzları ve sınırlamaları uygulanabilir.*

StorSimple ile çözümler oluşturmak için kullanılan diğer yazılımların listesi aşağıda verilmiştir.

| İş yükü türü | StorSimple ile kullanılan yazılım | Desteklenen sürümler|Çözüm Kılavuzu bağlama| 
| --- | --- | --- | --- |
| Yedekleme hedefi |Veeam |Veeam v 9 ve sonraki sürümler |[Yedekleme hedefi olarak StorSimple Veaam ile](storsimple-configure-backup-target-veeam.md)|
| Yedekleme hedefi |Veritas Backup Exec |Yedekleme Exec 16 ve üzeri |[Backup Exec ile yedekleme hedefi olarak StorSimple](storsimple-configure-backup-target-using-backup-exec.md)|
| Yedekleme hedefi |Veritas NetBackup |NetBackup 7.7.x ve üzeri  |[Yedekleme hedefi olarak StorSimple NetBackup ile](storsimple-configure-backuptarget-netbackup.md)|
| Genel dosya paylaşımı <br></br> İş Birliği |Talon  |[Talon ile StorSimple](https://www.talonstorage.com/products/archive/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>StorSimple terminolojisi
Microsoft Azure StorSimple çözümünüzle dağıtmadan önce aşağıdaki terimleri ve tanımları gözden geçirmenizi öneririz.

### <a name="key-terms-and-definitions"></a>Önemli terimleri ve tanımları
| Dönemi (kısaltması veya kısaltması) | Açıklama |
| --- | --- |
| erişim denetimi kaydı (ACR) |Hangi konakların kendisine bağlanıp bağlanamayacağını belirler, Microsoft Azure StorSimple cihaz üzerindeki bir birimi ile ilişkilendirilmiş bir kaydı. Belirlenmesi, iSCSI tabanlı tam adı (IQN) StorSimple cihazınıza bağlanma konakların (ACR içinde yer alan). |
| AES-256 |Verilerin şifrelenmesi için 256 bit Gelişmiş Şifreleme Standardı (AES) algoritması, için ve buluttan taşır. |
| ayırma birimi boyutu (AU) |En küçük bir dosya içinde Windows tutmak için ayrılan disk alanı miktarını dosya sistemleri. Dosyayı (kadar sonraki birden çok küme boyutu) saklamak için bir dosya boyutu, küme boyutunun bir katı değil, fazladan boşluk kullanılmalıdır kayıp alanı ve sabit disk parçalanma. <br>Yinelenenleri kaldırma algoritmalarıyla ile çalıştığı için önerilen AU Azure StorSimple birimler için 64 KB'tır. |
| Otomatik depolama katmanlamayı |HDD'ler ve ardından bulutta bir katman için daha az etkin verileri SSD otomatik olarak taşıma ve ardından merkezi kullanıcı arabiriminden tüm depolama yönetimini etkinleştirme. |
| Yedekleme kataloğu |Yedeklemeler, genellikle kullanılan uygulama türü ile ilgili bir koleksiyonu. Bu koleksiyon, StorSimple cihaz Yöneticisi hizmeti UI'si yedekleme kataloğu dikey penceresinde görüntülenir. |
| Yedekleme kataloğu dosyası |Kullanılabilir anlık görüntü şu anda StorSimple Snapshot Manager'ın Yedekleme veritabanında depolanan bir listesini içeren dosya. |
| Yedekleme İlkesi |Birimleri, yedekleme türünü ve önceden tanımlanmış bir zamanlamaya göre yedekleme oluşturmaya olanak tanıyan bir zaman çizelgesi seçimi. |
| ikili büyük nesne (BLOB) |Bir veritabanı yönetim sisteminin tek bir varlık olarak depolanan ikili veri koleksiyonu. Bazen ikili yürütülebilir kod, bir BLOB depolanır ancak genellikle görüntüleri, ses veya diğer multimedya nesneleri blobudur. |
| Karşılıklı Kimlik Doğrulama Protokolü (CHAP) |Eş bir parola veya parola paylaşımı eş göre bir bağlantının kimliğini doğrulamak için kullanılan protokol. Tek yönlü veya karşılıklı CHAP olabilir. Tek yönlü CHAP ile hedef Başlatıcı kimliğini doğrular. Karşılıklı CHAP hedefi Başlatıcı kimlik doğrulaması ve Başlatıcı Hedef kimlik doğrulaması gerektirir. |
| Kopya |Bir birime yinelenen bir kopyası. |
| Bir katmana (CaaT) olarak bulut |Bulut depolama mimarisi içinde bir katman olarak tüm depolama görünmesi bir kurumsal depolama ağının parçası olması için tümleşik depolama. |
| Bulut hizmeti sağlayıcısı (CSP) |Bulut bilgi işlem Hizmetleri Sağlayıcısı. |
| Bulut anlık görüntüsü |Bulutta depolanan verilerin toplu zaman içinde nokta kopyası. Bir bulut anlık görüntüsünü, farklı ve şirket dışı depolama sisteminde çoğaltılan bir anlık görüntü eşdeğerdir. Bulut anlık görüntüleri, olağanüstü durum kurtarma senaryolarda özellikle yararlıdır. |
| Bulut depolama şifreleme anahtarı |Bir parola veya buluta cihazınız tarafından gönderilen şifrelenmiş verilere erişmek için StorSimple cihazınız tarafından kullanılan anahtar. |
| Küme durumunu algılayan güncelleştirme |Güncelleştirmelerin en düşük böylece yük devretme kümesindeki sunucularda yazılım güncelleştirmelerini yönetme veya hizmet kullanılabilirliğini etkilemez. |
| DataPath |Bir koleksiyonu arası bağlı veri işleme işlemleri işlevsel birimi. |
| Devre dışı bırak |StorSimple cihazı ile ilişkili bir bulut hizmeti arasındaki bağlantıyı keser kalıcı bir eylem. Bulut anlık görüntüleri cihazın bu işlem kalır ve kopyalanmış veya olağanüstü durum kurtarma için kullanılan. |
| Disk yansıtma |Mantıksal disk birimi ayrı sabit çoğaltılmasını sürekli kullanılabilirlik sağlamak için gerçek zamanlı olarak beraberinde getirir. |
| dinamik disk yansıtma |Dinamik diskler mantıksal disk birimi çoğaltma. |
| dinamik diskler |Birden çok fiziksel disklere verileri yönetmek için Mantıksal Disk Yöneticisi (LDM) kullanır. bir disk birimi biçimi. Daha fazla boş alan sağlamak için dinamik diskler büyütülebilir. |
| Genişletilmiş disk Bunch (EBOD) kasası |Ek depolama alanı için ek sabit disk diskleri içeren Microsoft Azure StorSimple cihazınızın ikincil bir kutu. |
| FAT sağlama |Bir geleneksel depolama hangi depolama alanı ayrılır dayalı sağlama gereksinimlerini beklenen (ve genellikle geçerli ihtiyacı,). Ayrıca bkz: *ölçülü kaynak sağlama*. |
| sabit disk sürücüsü (HDD) |Verileri depolamak için döndürme plakalarının kullanan bir sürücüye. |
| hibrit bulut depolaması |Bulut depolama da dahil olmak üzere, yerel ve uzak kaynakları kullanan bir depolama mimarisi. |
| Internet küçük bilgisayar sistemi Arabirimi'ni (iSCSI) |Veri depolama donanımı veya tesis bağlama için bir Internet Protokolü IP tabanlı depolama ağ standardı. |
| iSCSI başlatıcısı |Bir dış iSCSI tabanlı depolama ağına bağlanmak için Windows çalıştıran bir konak bilgisayar sağlayan bir yazılım bileşeni. |
| iSCSI tam adı (IQN) |Bir iSCSI hedefi veya Başlatıcı tanımlayan benzersiz bir ad. |
| iSCSI hedefi |Merkezi iSCSI disk alt sistemleri depolama alanı ağlarında sağlayan bir yazılım bileşeni. |
| Arşivleme Canlı |Arşiv verileri (yedeklemelerle şirket dışındaki bant üzerinde örneğin depolandığını değil) her zaman erişilebilir olduğu bir depolama yaklaşım. Microsoft Azure StorSimple, Canlı arşivleme kullanır. |
| yerel olarak sabitlenmiş birim |buluta, cihazda bulunan ve hiçbir zaman bir birim katmanlı. |
| Yerel anlık görüntü |Microsoft Azure StorSimple cihazında depolanmış birim verilerini zaman içinde nokta kopyası. |
| Microsoft Azure StorSimple |Bir depolama gerecini veri merkezi ve bulut depolama işlevmiş gibi veri merkezi depolama yararlanmak BT kuruluşların yazılım oluşan güçlü bir çözümdür. StorSimple, maliyetleri azaltırken veri koruma ve veri yönetimini basitleştirir. Çözüm, birincil depolama, arşivleme, yedekleme ve olağanüstü durum kurtarma (DR bulut ile sorunsuz tümleştirme) birleştirir. StorSimple cihazları, kurumsal sınıf platform SAN depolama alanı ve bulut veri yönetimi birleştirerek hızı, Basitlik ve tüm depolama ile ilgili ihtiyaçları için güvenilirlik etkinleştirin. |
| Güç ve soğutma Modülü (PCM) |Donanım bileşenleri StorSimple cihazınızın güç kaynakları ve soğutma fan, bu nedenle Power adı oluşan ve soğutma modülü. EBOD muhafazası iki 580W PCMs sahipken cihazın birincil muhafaza iki 764W PCMs sahiptir. |
| Birincil |Ana kutu StorSimple cihazınızın uygulama platformu denetleyicileri içerir. |
| Kurtarma süresi hedefi (RTO) |Bir iş sürecini veya sistem önce minimuma en uzun süreyi olağanüstü bir durumla karşılaştığınızda tam olarak geri yüklenir. |
| Seri Bağlı SCSI (SAS) |Sabit disk sürücüsü (HDD) türü. |
| Hizmet veri şifreleme anahtarı |Yeni bir StorSimple cihaz için kullanılabilir hale bir anahtar, StorSimple cihaz Yöneticisi hizmetine kaydeder. StorSimple cihaz Yöneticisi hizmeti ile cihaz arasında aktarılan yapılandırma verileri, bir ortak anahtar kullanılarak şifrelenir ve yalnızca özel bir anahtar kullanarak cihaz üzerinde ardından şifresi çözülebilir. Hizmet veri şifreleme anahtarı, bu özel şifre çözme anahtarı almak hizmet sağlar. |
| Hizmet kayıt anahtarı |Bir anahtar, daha fazla yönetim eylemleri için Azure portalında görünmesi StorSimple cihazı StorSimple cihaz Yöneticisi hizmetiyle kaydedin yardımcı olur. |
| Küçük bilgisayar sistemi arabirimi (SCSI) |Fiziksel bilgisayarları birbirine bağlamak ve aralarında veri geçirerek standartları kümesidir. |
| katı hal sürücüsü (SSD) |Hiçbir hareketli parçadan içeren bir diski; Örneğin, bir flash sürücü. |
| depolama hesabı |Erişim kimlik bilgileri kümesi için belirli bir bulut hizmeti sağlayıcısı depolama hesabınıza bağlı. |
| SharePoint için StorSimple Bağdaştırıcısı |SharePoint sunucu grupları StorSimple depolaması ve veri koruma şeffaf bir şekilde genişleten bir Microsoft Azure StorSimple bileşeni. |
| StorSimple Device Manager hizmeti |Azure StorSimple şirket içi ve sanal cihazları yönetmenize olanak sağlar Azure portalının bir uzantısıdır. |
| StorSimple Snapshot Manager |Bir Microsoft Yönetim Konsolu (MMC) ek Microsoft Azure StorSimple yedekleme ve geri yükleme işlemleri yönetmek için bileşeni. |
| yedek Al |Kullanıcının bir birim etkileşimli bir yedeğini almak bir özelliği. El ile yedekleme tanımlanmış bir ilke aracılığıyla otomatik bir yedekleme sürüyor aksine, bir birimin alınmasına alternatif bir yoludur. |
| ölçülü kaynak sağlama |Depolama sistemlerinde kullanılan kullanılabilir depolama alanı ile verimliliğini en iyi duruma getirme yöntemi. Ölçülü kaynak sağlama, depolama, her kullanıcı tarafından belirli bir zamanda gereken en düşük alanı göre birden çok kullanıcı arasında ayrılır. Ayrıca bkz: *fat sağlama*. |
| Katman ayarlama |Geçerli kullanım, geçerlilik süresi ve diğer veriler ilişkisini göre mantıksal gruplandırmaları verileri düzenleme. StorSimple, veri katmanlarını otomatik olarak düzenler. |
| birim |Mantıksal depolama alanları sürücüleri biçiminde gösterilir. StorSimple birimlerini, iSCSI ve StorSimple cihazını kullanarak bulunan dahil olmak üzere, ana bilgisayar tarafından bağlanan birimler karşılık gelir. |
| Birim kapsayıcısı |Birimler ve kendilerine uygulanan ayarları gruplandırmasıdır. StorSimple cihazınızdaki tüm birimler birim kapsayıcıları gruplandırılır. Birim kapsayıcısı ayarları depolama hesapları, ilişkili şifreleme anahtarları ile buluta gönderilen verileri ve bulut ilgili işlemler için kullanılan bant genişliği için şifreleme ayarlarını içerir. |
| birim grubu |StorSimple Snapshot Manager'da bir birim grubu birimlerin yedekleme işlemini kolaylaştırmak için yapılandırılmış bir koleksiyondur. |
| Birim Gölge Kopyası Hizmeti (VSS) |Artımlı anlık oluşturulmasını koordine etmek için VSS kullanabilen uygulamalarla iletişim kurarak uygulama tutarlılığı kolaylaştıran bir Windows Server işletim sistemi hizmetidir. VSS anlık görüntüler alındığında uygulamaların geçici olarak devre dışı olmasını sağlar. |
| StorSimple için Windows PowerShell |Bir Windows PowerShell tabanlı komut satırı çalıştırma ve StorSimple Cihazınızı yönetmek için kullanılan arabirim. Windows PowerShell temel özelliklerden bazıları koruyarak bu arabirimi bir StorSimple cihazı yönetmeye yönelik olarak ek adanmış cmdlet'leri vardır. |

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [StorSimple güvenlik](storsimple-8000-security.md).

