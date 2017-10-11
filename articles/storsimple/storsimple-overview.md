---
title: "StorSimple 8000 serisi çözümüne genel bakış | Microsoft Docs"
description: "StorSimple katmanlama, cihaz, sanal cihaz, hizmetleri ve depolama yönetimi açıklar ve StorSimple içinde kullanılan anahtar terimleri tanıtır."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: v-sharos@microsoft.com
ms.openlocfilehash: 86b8300553caa0741e8aca3c0e7621ec80cc5b21
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000 serisi: karma bulut depolama çözümü
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple, Depolama görevlerini şirket içi cihazlar ve Microsoft Azure bulut depolama arasında yöneten bir tümleşik depolama çözümü Hoş Geldiniz. StorSimple sorunlar ve Kurumsal Depolama ve veri koruma ile ilişkili giderleri birçoğunu ortadan kaldıran bir verimli, uygun maliyetli ve kolayca yönetilebilir depolama alanı ağı (SAN) çözümüdür. Özel StorSimple 8000 serisi aygıt kullanır, bulut hizmetleriyle tümleştirilen ve sorunsuz bir bulut depolama da dahil olmak üzere tüm Kurumsal Depolama görünümü için bir dizi yönetim araçları sağlar. (Microsoft Azure Web sitesinde yayınlanan StorSimple dağıtım bilgileri yalnızca StorSimple 8000 serisi cihazlar için geçerlidir. Bir StorSimple 5000/7000 Serisi cihaz kullanıyorsanız, Git [StorSimple Yardım](http://onlinehelp.storsimple.com/).)

StorSimple kullanan [depolama katmanlama](#automatic-storage-tiering) çeşitli depolama medyaya depolanan verileri yönetmek için. Şirket içi depolanan katı hal sürücüleri (SSD) üzerinde geçerli çalışma kümesi olduğundan, daha az sık kullanılan veri sabit disk sürücülerinin (HDD'ler) depolanır ve Arşiv verileri buluta gönderilir. Ayrıca, verileri kullanır depolama miktarını azaltmak için yinelenenleri kaldırma ve sıkıştırma StorSimple kullanır. Daha fazla bilgi için Git [yinelenenleri kaldırma ve sıkıştırma](#deduplication-and-compression). Diğer anahtar terimleri ve kavramları StorSimple 8000 serisi belgelerde kullanılan tanımları Git [StorSimple terminolojisi](#storsimple-terminology) bu makalenin sonunda.

Depolama Yönetimi ek olarak, StorSimple veri koruma özelliklerine isteğe bağlı oluşturmanızı sağlar ve zamanlanmış yedeklemeler ve bunları yerel olarak veya bulutta depolayın. Yedeklemeler, bunlar oluşturulabilir ve hızlı bir şekilde geri, yani biçiminde artımlı anlık görüntü alınır. İkincil depolama sistemleri (örneğin, bant yedekleme) değiştirin ve veri merkeziniz için veya diğer sitelere gerekirse geri yüklemenize olanak tanımak için bulut anlık görüntüleri olağanüstü durum kurtarma senaryolarında kritik düzeyde önemli olabilir.

![video simgesi](./media/storsimple-overview/video_icon.png) Microsoft Azure StorSimple hızlı bir giriş videoyu izleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>StorSimple neden kullanılır?
Aşağıdaki tabloda Microsoft Azure StorSimple sağlayan en önemli avantajlardan bazıları açıklanmaktadır.

| Özellik | Avantaj |
| --- | --- |
| Saydam tümleştirme |İSCSI protokolü görünmez Veri Depolama tesisleri bağlamak için kullanır. Bu, veri merkezindeki, bulutta depolanan bu verileri sağlar veya tek bir konumda depolanması için uzak sunucularda görünür. |
| Azaltılmış depolama maliyetleri |Geçerli taleplerini karşılamak üzere yeterli yerel veya Bulut depolama ayırır ve bulut depolama yalnızca gerekli olduğunda genişletir. Bu daha fazla depolama alanı gereksinimleri ve gider aynı verileri (kaldırmayı) yedek sürümlerini ortadan kaldırır ve sıkıştırma kullanarak azaltır. |
| Basitleştirilmiş Depolama Yönetimi |Sistem yapılandırmak ve depolanan verileri şirket içi, uzak bir sunucudaki ve bulutta yönetmek için yönetim araçları sağlar. Ayrıca, yedekleme yönetmek ve bir Microsoft Yönetim Konsolu (MMC) ek bileşeninden işlevleri geri yükleyebilirsiniz.|
| Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk |Genişletilmiş kurtarma süresi gerektirmez. Bunun yerine, gerektiğinde verileri yükler. Başka bir deyişle, normal işlemleri en az kesintiyi ile devam edebilirsiniz. Ayrıca, yedekleme zamanlamaları ve veri bekletme ilkelerini yapılandırabilirsiniz. |
| Veri mobility |Microsoft Azure bulut hizmetlerine yüklenen veri kurtarma ve geçiş amaçları için diğer sitelerdeki erişilebilir. Ayrıca, Microsoft Azure'da çalışan sanal makineler (VM'ler) StorSimple bulut cihazları yapılandırmak için StorSimple kullanabilirsiniz. Sanal makineleri sonra test veya kurtarma amacıyla depolanan verilere erişmek için sanal cihazları'nı kullanabilirsiniz. |
| İş sürekliliği |StorSimple, 5000-7000 Serisi kullanıcıların bir StorSimple 8000 serisi cihaza verilerini geçirmek sağlar. |
| Azure kamu portalında kullanılabilirliği |StorSimple Azure kamu Portalı'nda mevcuttur. Daha fazla bilgi için bkz: [kamu portalında şirket içi StorSimple Cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-gov-u2.md). |
| Veri koruma ve kullanılabilirlik |StorSimple 8000 serisi bölge olarak yedekli depolama (ZRS), yerel olarak yedekli depolama (LRS) ve coğrafi olarak yedekli depolama (GRS) ek olarak destekler. Başvurmak [bu makalede Azure Storage artıklık seçenekleri](https://azure.microsoft.com/documentation/articles/storage-redundancy/) ZRS Ayrıntılar için. |
| Kritik uygulamalar için destek |StorSimple yerel olarak sabitlenmiş uygun birimlerini tanımlayın sağlayan sırayla Kritik uygulamalar tarafından gerekli verileri buluta katmanlı değil olanak sağlar. Yerel olarak sabitlenmiş birimler bulut gecikmeleri veya bağlantı sorunları tabi değildir. Yerel olarak sabitlenmiş birimleri hakkında daha fazla bilgi için bkz: [birimleri yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manage-volumes-u2.md). |
| Düşük gecikme süreli ve yüksek performans |Yüksek performans, düşük gecikme süresi Azure premium storage özelliklerini yararlanmak bulut uygulamaları oluşturabilirsiniz. StorSimple premium bulut uygulamaları hakkında daha fazla bilgi için bkz: [dağıtma ve azure'da bir StorSimple bulut uygulaması yönetmek](storsimple-8000-cloud-appliance-u2.md). |


## <a name="storsimple-components"></a>StorSimple bileşenleri
Microsoft Azure StorSimple çözümünüzle aşağıdaki bileşenleri içerir:

* **Microsoft Azure StorSimple cihaz** – SSD ve HDD, yedek denetleyicileri ve otomatik yük devretme yetenekleri ile birlikte içeren bir şirket içi karma depolama dizisi. Denetleyicileri katmanlama, şu anda kullanılan (veya sık kullanılan) veri yerel depolamada (aygıt ya da şirket içi sunucuları), buluta daha az sık kullanılan veri taşırken yerleştirme depolama yönetin.
* **StorSimple bulut uygulaması** – olarak da bilinen StorSimple sanal gereç mimarisi çoğaltır StorSimple cihazı yazılımı sürümü ve fiziksel karma depolama aygıtı çoğu özelliklerini budur. Bir Azure sanal makinesinde tek bir düğümde StorSimple bulut uygulaması çalıştırır. Azure premium Storage yararlanmak, premium sanal aygıt ve sonrasında güncelleştirme 2'de kullanılabilir.
* **StorSimple cihaz Yöneticisi hizmeti** – bir StorSimple cihazı veya StorSimple bulut uygulaması bir tek web arabiriminden yönetmenizi sağlayan Azure portalı uzantısı bir. StorSimple cihaz Yöneticisi hizmeti oluşturun ve hizmetleri yönetmek, görüntülemek ve cihazları yönetmek, uyarıları görüntülemek, birimleri yönetin ve görüntüleyin ve yedekleme ilkeleri ve yedekleme kataloğu yönetmek için kullanabilirsiniz.
* **StorSimple için Windows PowerShell** – StorSimple cihazı yönetmek için kullanabileceğiniz bir komut satırı arabirimi. StorSimple için Windows PowerShell, StorSimple Cihazınızı kaydetmek, ağ arabiriminin aygıtınızda yapılandırın, belirli türden güncelleştirmeler yüklemeniz, destek oturum erişerek cihazınızın sorunlarını giderirken olanak sağlar ve aygıtın değiştirme özelliğe sahiptir durumu. StorSimple için Windows PowerShell seri konsoluna bağlanmak veya Windows PowerShell uzaktan iletişimini kullanarak erişebilirsiniz.
* **Azure PowerShell StorSimple cmdlet'lerini** – komut satırından hizmet düzeyi ve geçiş görevlerinizi otomatikleştirmenizi sağlayan Windows PowerShell cmdlet'leri koleksiyonu. StorSimple için Azure PowerShell cmdlet'leri hakkında daha fazla bilgi için Git [cmdlet başvurusu](/powershell/module/azure/?view=azuresmps-3.7.0#azure).
* **StorSimple Snapshot Manager** – uygulamayla tutarlı yedeklemeler oluşturmak için birim grupları ve Windows birim gölge kopyası hizmeti kullanan bir MMC ek bileşenini. Ayrıca, StorSimple Snapshot Manager yedekleme zamanlamaları ve kopya oluşturmak veya birimleri geri yüklemek için kullanabilirsiniz.
* **SharePoint için StorSimple bağdaştırıcısı** – saydam SharePoint Server için Microsoft Azure StorSimple depolama ve veri koruması genişleten bir aracı grupları, SharePoint Merkezi StorSimple depolama görüntülenebilir ve yönetilebilir yaparken Yönetim Portalı.

Aşağıdaki diyagramda Microsoft Azure StorSimple mimarisi ve bileşenleri üst düzey bir görünümünü sağlar.

![StorSimple mimarisi](./media/storsimple-overview/overview-big-picture.png)

Aşağıdaki bölümlerde daha ayrıntılı bu bileşenlerin her birini açıklayan ve nasıl çözüm verileri düzenler, depolama ayırır ve Depolama Yönetimi ve veri koruması kolaylaştıran açıklar. Son bölümde bazı önemli terimler ve StorSimple bileşenleri ve bunların yönetimi ile ilgili kavramları için tanımları sağlar.

## <a name="storsimple-device"></a>StorSimple cihazı
Microsoft Azure StorSimple cihazını birincil depolama ve depolanan verilere iSCSI erişim sağlayan bir şirket içi karma depolama dizisidir. Bulut depolama ile iletişim yönetir ve Microsoft Azure StorSimple çözüm üzerinde depolanan tüm verileri gizliliğini ve güvenlik sağlamaya yardımcı olur.

StorSimple cihazı SSD ve sabit disk sürücülerinin HDD yanı sıra, kümeleme ve otomatik yük devretme için destek içerir. Paylaşılan bir işlemci, paylaşılan depolama alanı ve iki yansıtılmış denetleyicisi içerir. Her denetleyici aşağıdakileri sağlar:

* Bir ana bilgisayar bağlantı
* Yerel ağa (LAN) bağlanmak için en fazla altı ağ bağlantı noktaları
* Donanım izleme
* Güç kesildi olmasa bile, bilgileri saklar, geçici olmayan rasgele erişim belleği (NVRAM)
* Küme duyarlı güncelleştirme en az böylece yük devretme kümesindeki sunucularda yazılım güncelleştirmelerini yönetmek için güncelleştirme veya hizmet kullanılabilirliği üzerinde hiçbir etkisi
* Küme hizmeti, hangi işlevleri gibi yüksek düzeyde kullanılabilirlik sağladığınızdan ve HDD veya SSD hata verirse veya oluşabilecek olumsuz etkileri en aza bir arka uç küme çevrimdışına

Yalnızca bir denetleyici zamanda herhangi bir noktada etkindir. Etkin denetleyicisi başarısız olursa, ikinci denetleyicisi otomatik olarak etkinleşir.

Daha fazla bilgi için Git [StorSimple donanım bileşenleri ve durum](storsimple-8000-monitor-hardware-status.md).

## <a name="storsimple-cloud-appliance"></a>StorSimple Cloud Appliance
StorSimple fiziksel karma depolama aygıtı özelliklerini ve mimarisini çoğaltan bir bulut uygulaması oluşturmak için kullanabilirsiniz. Bir Azure sanal makinesinde tek bir düğümde StorSimple bulut uygulaması (olarak da bilinen StorSimple sanal gereç) çalışır. (Bulut uygulaması, yalnızca bir Azure sanal makinesi üzerinde oluşturulabilir. Bir StorSimple cihazı veya bir şirket içi sunucuda oluşturamazsınız.)

Bulut uygulaması aşağıdaki özelliklere sahiptir:

* Fiziksel bir Gereci gibi davranır ve buluttaki sanal makineler için iSCSI arabirimi sunabilir.
* Bulut cihazları sınırsız sayıda bulutta oluşturmak ve bunları açma ve kapatma gerektiği şekilde açın.
* Olağanüstü durum kurtarma, geliştirme ve test senaryoları şirket içi ortamlarını benzetmek yardımcı olabilir ve öğe düzeyinde alma yedeklerden ile yardımcı olabilir.

StorSimple bulut uygulaması iki modellerinde kullanılabilir: (önceden 1100 model olarak biliniyordu) 8010 cihaz ve 8020 cihaz. 8010 cihaz 30 TB maksimum kapasitesine sahiptir. Azure premium Storage yararlanır, 8020 cihaz 64 TB maksimum kapasitesine sahiptir. (Standart depolama Hdd'lerde veri depoladığı yerel katmanlarda, Azure premium depolama verileri Ssd'de depolar.) Premium depolama kullanmak için bir Azure premium depolama hesabı olması gerektiğini unutmayın. Premium depolama hakkında daha fazla bilgi için Git [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../storage/common/storage-premium-storage.md).

StorSimple bulut uygulaması hakkında daha fazla bilgi için Git [dağıtma ve azure'da bir StorSimple bulut uygulaması yönetmek](storsimple-8000-cloud-appliance-u2.md).

## <a name="storsimple-device-manager-service"></a>StorSimple cihaz Yöneticisi hizmeti
Microsoft Azure StorSimple ve merkezi olarak yönetebilmenizi veri merkezi depolama bulut olanak sağlayan bir web tabanlı kullanıcı arabirimi (StorSimple cihaz Yöneticisi hizmeti) sağlar. StorSimple cihaz Yöneticisi hizmeti, şu görevleri gerçekleştirmek için kullanabilirsiniz:

* StorSimple cihazlar için sistem ayarlarını yapılandırın.
* Yapılandırmak ve StorSimple cihazlar için güvenlik ayarlarını yönetin.
* Bulut kimlik bilgileri ve özelliklerini yapılandırın.
* Yapılandırın ve bir sunucu üzerindeki birimleri yönetin.
* Birim grupları yapılandırın.
* Yedekleme ve veri geri yükleme.
* Performans İzleyici.
* Sistem ayarlarını gözden geçirin ve olası sorunları tanımlar.

StorSimple cihaz Yöneticisi hizmeti, ilk kurulum ve güncelleştirmelerin yüklenmesi gibi kesinti sistem gerektiren olanlar dışında tüm yönetim görevlerini gerçekleştirmek için kullanabilirsiniz.

Daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell
StorSimple için Windows PowerShell oluşturmak ve Microsoft Azure StorSimple hizmeti yönetme ve ayarlama ve StorSimple cihazları izlemek için kullanabileceğiniz bir komut satırı arabirimi sağlar. Bu bir Windows PowerShell tabanlı komut satırı StorSimple Cihazınızı yönetmek için ayrılmış cmdlet'ler içeren arabirimidir. StorSimple için Windows PowerShell izin özelliklere sahiptir:

* Bir cihaz kaydetme.
* Ağ arabirimi bir aygıtta yapılandırın.
* Belirli türde bir güncelleştirmeleri yükleyin.
* Cihazınızı destek oturum erişerek sorunlarını giderin.
* Cihaz durumunu değiştirin.

(Aygıtına doğrudan bağlı bir ana bilgisayara) bir seri konsoldan veya uzaktan StorSimple için Windows PowerShell erişebilirsiniz Windows PowerShell uzaktan iletişimini kullanarak. İlk cihaz kaydı gibi StorSimple görevleri için bazı Windows PowerShell yalnızca seri konsolunuzdaki yapılması unutmayın.

Daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek için Windows PowerShell'i kullanın](storsimple-8000-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple cmdlet'leri
Azure PowerShell StorSimple cmdlet'leri komut satırından hizmet düzeyi ve geçiş görevlerinizi otomatikleştirmenizi sağlayan Windows PowerShell cmdlet'lerinin koleksiyonudur. StorSimple için Azure PowerShell cmdlet'leri hakkında daha fazla bilgi için Git [cmdlet başvurusu](/powershell/module/azure/?view=azuresmps-3.7.0).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
StorSimple Snapshot Manager olan tutarlı, oluşturmak için kullanabileceğiniz bir Microsoft Yönetim Konsolu (MMC) ek bileşenini zaman içinde nokta yedek kopyalarını yerel ve bulut veri. Ek bileşenini Windows Server tabanlı bir ana bilgisayarda çalışır. StorSimple Snapshot Manager kullanabilirsiniz:

* Yapılandırma, yedekleme ve birimleri silin.
* Sağlamak için birim grupları yapılandırmak, yedeklenen verileri uygulamayla tutarlı olan.
* Böylece verileri belirlenmiş bir zamanlamayla yedeklenebilir ve (yerel ve bulut) atanmış bir konumda depolanan yedekleme ilkelerini yönetin.
* Birimleri ve dosyaları tek tek geri yükleyin.

Yedeklemeleri Son anlık görüntü alındıktan sonra yalnızca değişiklikleri kaydetmek ve tam yedeklemeler daha çok daha az depolama alanı gerektiren anlık görüntü olarak yakalanır. Yedekleme zamanlamaları oluşturabilir veya gerektiğinde hemen yedek alabilir. Ayrıca, StorSimple Snapshot Manager bekletme ilkeleri oluşturmak için kaç anlık görüntü kaydedilecek denetleyen kullanabilirsiniz. Bundan sonra bir yedekleme, StorSimple Snapshot Manager sağlar verileri geri yüklemeniz gerekiyorsa yerel katalog veya Bulut anlık görüntüleri seçin. 

Gerektiğinde olağanüstü bir durum oluşursa veya başka bir nedenle veri geri yüklemeniz gerekiyorsa, StorSimple Snapshot Manager, artımlı olarak geri yükler. Verileri geri yükleme, bir dosyayı geri yüklemek, donanım değiştirin ya da işlemleri başka bir siteye taşımak tüm sistemi Kapat gerektirmez.

Daha fazla bilgi için Git [StorSimple anlık görüntü Yöneticisi nedir?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Bağdaştırıcısı
Microsoft Azure StorSimple, SharePoint, SharePoint server grupları StorSimple depolama ve veri koruma özelliklerine saydam genişleten isteğe bağlı bir bileşen için StorSimple bağdaştırıcısı içerir. Bağdaştırıcı BLOB'ları Microsoft Azure StorSimple sistemi tarafından yedeklenen bir sunucuya taşımak olanak tanıyan bir Uzak Blob Depolama (KKY) sağlayıcı ve SQL Server KKY özelliği ile çalışır. Microsoft Azure StorSimple sonra BLOB verilerini yerel olarak veya bulutta kullanıma dayalı depolar.

SharePoint için StorSimple bağdaştırıcısı SharePoint Merkezi Yönetim Portalı'ndan yönetilir. Sonuç olarak, SharePoint Yönetim Merkezi kalır ve tüm depolama SharePoint grubunda gibi görünüyor.

Daha fazla bilgi için Git [SharePoint için StorSimple bağdaştırıcısı](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Depolama yönetimi teknolojileri
Ayrılmış StorSimple cihazı, sanal cihaz ve diğer bileşenleri ek olarak, Microsoft Azure StorSimple verileri hızlı erişim sağlar ve depolama tüketimini azaltmak için aşağıdaki yazılım teknolojileri kullanır:

* [Otomatik depolama katmanlama](#automatic-storage-tiering) 
* [Ölçülü kaynak sağlama](#thin-provisioning) 
* [Yinelenenleri kaldırma ve sıkıştırma](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Otomatik depolama katmanlama
Microsoft Azure StorSimple otomatik olarak veri mantıksal katmanlarında geçerli kullanımı, geçerlilik süresi ve diğer verilere ilişkisine göre düzenler. Daha az etkin ve etkin olmayan verileri otomatik olarak buluta geçiş sırasında en etkin olan verileri yerel olarak depolanır. Aşağıdaki diyagram bu depolama yaklaşımı gösterir.

![StorSimple depolama katmanları](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Hızlı erişim etkinleştirmek için StorSimple çok etkin verileri (etkin) StorSimple cihazı Ssd'de depolar. Bazen kullanılan verilerini depolayan (normal veri) HDD cihazı veya veri merkezindeki sunucular üzerinde. Etkin olmayan verileri, yedekleme verilerini taşır ve veriler korunur için arşivleme veya buluta uyumluluk amaçlar. 

> [!NOTE]
> Güncelleştirme 2 veya sonraki sürümlerde, yerel olarak sabitlenmiş bir birim belirtebilirsiniz, bu durumda veriler yerel cihazda kalır ve değil buluta katmanlı. 


StorSimple ayarlar ve verileri yeniden düzenler ve kullanım düzenlerini olarak depolama atamalarını değiştirin. Örneğin, bazı bilgiler zaman içinde daha az etkin hale gelebilir. Aşamalı olarak daha az etkin hale geldiğinde, SSD HDD ve ardından bulut geçirilir. Bu aynı verileri tekrar etkin hale gelirse, depolama aygıtına geçirilir.

Depolama katmanlama işlemi aşağıdaki gibidir:

1. Bir Sistem Yöneticisi bir Microsoft Azure bulut depolama hesabı ayarlar.
2. Yönetici seri konsolu ve (Azure Portalı'nda çalışan) StorSimple cihaz Yöneticisi Hizmeti aygıt ile dosya sunucusu yapılandırmak için birimler ve veri koruma ilkeleri oluşturma kullanır. Şirket içi makineler (örneğin, dosya sunucuları) Internet küçük bilgisayar sistemi arabirimi (iSCSI) StorSimple cihazı erişmek için kullanın.
3. Başlangıçta, StorSimple cihazı hızlı SSD katmanı üzerinde verileri depolar.
4. SSD katmanı kapasitesine yaklaştığında StorSimple deduplicates en eski veri blokları sıkıştırır ve HDD katmanına taşır.
5. HDD katmanı yaklaşımlar kapasitesi olarak StorSimple en eski veri bloklarını şifreler ve bunları güvenli bir şekilde Microsoft Azure depolama hesabı HTTPS üzerinden gönderir.
6. Microsoft Azure olağanüstü bir durum oluşursa verilerin kurtarılabilmesini sağlamak, veri merkezinde ve uzak bir veri merkezinde verilerin birden çok çoğaltma oluşturur.
7. Dosya sunucusu bulutta depolanan veriler istediğinde, StorSimple sorunsuz bir şekilde döndürür ve StorSimple cihazı SSD katmanı üzerinde bir kopyasını depolar.

#### <a name="how-storsimple-manages-cloud-data"></a>StorSimple bulut verilerini nasıl yönetir

StorSimple müşteri verileri tüm anlık görüntüleri ve birincil verilerini (ana bilgisayar tarafından yazılan) üzerinden deduplicates. Yinelenenleri kaldırma depolama verimliliği için harika olsa da, "bulutta karmaşık nedir" Soru kolaylaştırır. Katmanlı birincil veri ve anlık görüntü verilerini birbirleri ile çakışıyor. Buluttaki verilerin tek bir öbek katmanlı birincil veri olarak kullanılabilir ve ayrıca birkaç anlık görüntüleri tarafından başvuruda. Bu anlık görüntü silinene kadar tüm zaman içinde nokta verilerin bir kopyasını bulutunu kilitli her bulut anlık görüntüsü sağlar.

Bu verileri başvuru olduğunda veriler yalnızca buluttan silinir. Örneğin, biz StorSimple cihazı ve bazı birincil veri silmek bulut anlık tüm verileri alma, biz görmek _birincil veri_ hemen bırakın. _Bulut verilerini_ katmanlı veri ve yedeklemeler içerir, kalır aynı. Hala bulut verilerini başvuran bir anlık görüntü olduğundan bu değildir. Sonra bulut anlık görüntüsü silinir (ve aynı veri başvurulan diğer tüm anlık görüntü) tüketim düşme bulut. Biz bulut verilerini kaldırmadan önce anlık görüntü yok hala bu verilere başvuruda denetleyin. Bu işlem çağrılırken _çöp toplama_ ve cihazda çalıştıran bir arka plan hizmeti. Bulut verilerin kaldırılması hemen değil atık toplama hizmetini silme işlemini önce verilerin başka bir başvuru olup olmadığını denetler. Çöp toplama hızına anlık görüntüler ve toplam veri toplam sayısına bağlıdır. Genellikle, bulut verilerini değerinden bir hafta içinde temizlenir.


### <a name="thin-provisioning"></a>Ölçülü kaynak sağlama
Ölçülü kaynak sağlama fiziksel kaynakları aşan kullanılabilir depolama alanı görünür bir sanallaştırma teknolojisidir. Önceden yeterli depolama alanı ayırma yerine, StorSimple ölçülü kaynak sağlama geçerli gereksinimlerini karşılamak için yeterli alan ayırmak için kullanır. StorSimple artırabilir veya değişen taleplerini karşılamak üzere bulut depolama azaltmak için bu yaklaşım bulut depolama esnek yapısını kolaylaştırır.

> [!NOTE]
> Yerel olarak sabitlenmiş birimlerin ölçülü kaynak sağlanan değil. Birim oluşturulduğunda, yalnızca yerel bir birimi için ayrılan depolama alanını tamamının sağlanır.


### <a name="deduplication-and-compression"></a>Yinelenenleri kaldırma ve sıkıştırma
Microsoft Azure StorSimple daha fazla depolama gereksinimlerini azaltmak için yinelenenleri kaldırma ve verileri sıkıştırmayı kullanır.

Yinelenenleri kaldırma genel depolanan veri kümesi içinde artıklık ortadan kaldırarak depolanan veri miktarını azaltır. Bilgi değiştikçe StorSimple değişmeyen verilerin yoksayar ve yalnızca değişiklikleri yakalar. Ayrıca, StorSimple tanımlama ve gereksiz bilgileri kaldırma depolanan veri miktarını azaltır. 

> [!NOTE]
> Yerel olarak sabitlenmiş birimlerdeki veriler yinelenenleri kaldırılmış veya sıkıştırılmış değil. Ancak, yerel olarak sabitlenmiş birimlerin yedekleri, yinelenenleri kaldırılmış sıkıştırılmış ve.


## <a name="storsimple-workload-summary"></a>StorSimple iş yükü özeti
Desteklenen StorSimple iş yükleri özetini aşağıdaki tabloda verilmiştir.

| Senaryo | İş yükü | Destekleniyor | Kısıtlamaları | Sürüm |
| --- | --- | --- | --- | --- |
| İş Birliği |Dosya Paylaşımı |Evet | |Tüm sürümler |
| İş Birliği |Dağıtılmış dosya paylaşımı |Evet | |Tüm sürümler |
| İş Birliği |SharePoint |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Güncelleştirme 2 ve üstü |
| Arşivleme |Basit dosya arşivleme |Evet | |Tüm sürümler |
| Sanallaştırma |Sanal makineler |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Güncelleştirme 2 ve üstü |
| Database |SQL |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Güncelleştirme 2 ve üstü |
| Kameralı |Kameralı |Evet* |StorSimple cihazı yalnızca bu iş yüküne adanıp olduğunda desteklenir |Güncelleştirme 2 ve üstü |
| Backup |Birincil hedef yedekleme |Evet* |StorSimple cihazı yalnızca bu iş yüküne adanıp olduğunda desteklenir |Güncelleştirme 3 ve üzeri |
| Backup |İkincil hedef yedekleme |Evet* |StorSimple cihazı yalnızca bu iş yüküne adanıp olduğunda desteklenir |Güncelleştirme 3 ve üzeri |

*Evet &#42; -Çözüm yönergeleri ve kısıtlamaları uygulanmalıdır.*

Aşağıdaki iş yüklerini StorSimple 8000 serisi cihazlar tarafından desteklenmez. StorSimple üzerinde dağıttıysanız, bu iş yükleri desteklenmeyen bir yapılandırmada neden olur.

* Tıbbi görüntüleme
* Exchange
* VDI
* Oracle
* SAP
* Büyük veriler
* İçerik dağıtımı
* SCSI önyükleme

Desteklenen StorSimple altyapı bileşenlerin bir listesi aşağıda verilmiştir.

| Senaryo | İş yükü | Destekleniyor | Kısıtlamaları | Sürüm |
| --- | --- | --- | --- | --- |
| Genel |Express Route |Evet | |Tüm sürümler |
| Genel |DataCore FC |Evet* |DataCore SANsymphony ile desteklenen |Tüm sürümler |
| Genel |DFSR |Evet* |Yalnızca yerel olarak sabitlenmiş birimleri ile desteklenen |Tüm sürümler |
| Genel |Dizinleme |Evet* |Katmanlı birimler için yalnızca meta veri dizinini desteklenir (verileri değil).<br>Yerel olarak sabitlenmiş birimler için dizin oluşturma tamamlandı desteklenir. |Tüm sürümler |
| Genel |Virüsten koruma |Evet* |Katmanlı birimler için yalnızca tarama açık ve Kapat desteklenir.<br> Yerel olarak sabitlenmiş birimler için tam tarama desteklenir. |Tüm sürümler |

*Evet &#42; -Çözüm yönergeleri ve kısıtlamaları uygulanmalıdır.*

StorSimple ile çözümleri oluşturmak için kullanılan diğer yazılımların listesi verilmiştir.

| İş yükü türü | StorSimple ile kullanılan yazılım | Desteklenen sürümler|Çözüm Kılavuzu'na bağlantı| 
| --- | --- | --- | --- |
| Yedekleme hedefi |Veeam |Veeam v 9 ve sonraki sürümler |[Yedekleme hedefi olarak StorSimple Veaam ile](storsimple-configure-backup-target-veeam.md)|
| Yedekleme hedefi |Veritas Backup Exec |Yedekleme Exec 16 ve üzeri |[Yedekleme hedefi olarak StorSimple yedekleme Exec ile](storsimple-configure-backup-target-using-backup-exec.md)|
| Yedekleme hedefi |VERITAS NetBackup |NetBackup 7.7.x ve sonraki sürümler  |[Yedekleme hedefi olarak StorSimple NetBackup ile](storsimple-configure-backuptarget-netbackup.md)|
| Genel dosya paylaşımı <br></br> İş Birliği |Talon  |[StorSimple Talon ile](https://www.talonstorage.com/products/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>StorSimple terminolojisi
Microsoft Azure StorSimple çözümünüzün dağıtmadan önce aşağıdaki terimleri ve tanımları gözden geçirmenizi öneririz.

### <a name="key-terms-and-definitions"></a>Anahtar terimleri ve tanımları
| Terim (kısaltma veya kısaltması) | Açıklama |
| --- | --- |
| erişim denetimi kaydı (ACR) |Hangi ana bilgisayarların bağlanabileceği belirler, Microsoft Azure StorSimple cihaz üzerindeki bir birimi ile ilişkili bir kaydı. İSCSI belirlenmesi temel tam adını (IQN) StorSimple Cihazınızı bağlanan (ACR içinde yer alan) ana bilgisayar. |
| AES 256 |Bulut gelen ve giden hareket ederken verileri şifrelemek için bir 256 bit Gelişmiş Şifreleme Standardı (AES) algoritması. |
| ayırma birimi boyutu (Avustralya) |En küçük bir dosya, Windows'da tutmak için ayrılan disk alanı miktarını dosya sistemleri. Dosyanın (kadar sonraki birden çok küme boyutu) tutmak için bir dosya boyutu bir küme boyutunu değil, ek boşluk kullanılmalıdır kayıp alanı ve sabit disk parçalanması. <br>Azure StorSimple birimler için önerilen Avustralya 64 KB nedeni, yinelenenleri kaldırma algoritmalarıyla iyi çalışır. |
| Otomatik depolama katmanlama |Otomatik olarak daha az etkin veri HDD ve ardından bir katman bulutta SSD taşıma ve tüm depolama biriminden bir merkezi kullanıcı arabirimi yönetimini etkinleştirme. |
| Yedekleme kataloğu |Yedeklemeler, genellikle kullanılan uygulama türüne göre ilgili koleksiyonu. Bu koleksiyon, StorSimple cihaz Yöneticisi hizmeti UI yedekleme kataloğu dikey penceresinde görüntülenir. |
| Yedekleme kataloğu dosyası |Kullanılabilir anlık görüntüleri yedekleme veritabanında StorSimple anlık görüntü Yöneticisi'nin şu anda depolanan listesini içeren bir dosya. |
| Yedekleme İlkesi |Birimler, yedekleme türü ve önceden tanımlanmış bir zamanlamaya göre yedeklemeler oluşturmanıza olanak tanıyan bir zaman çizelgesi seçimi. |
| ikili büyük nesneler (BLOB) |Bir veritabanı yönetim sisteminin tek bir varlık olarak saklanan ikili verileri koleksiyonu. Bazen ikili yürütülebilir kod BLOB olarak depolanan BLOB'ları genellikle görüntüler, ses ve diğer multimedya nesneler, ancak. |
| Karşılıklı Kimlik Doğrulama Protokolü (CHAP) |Bir parola veya gizli paylaşımı eş tabanlı bir bağlantı eş kimlik doğrulaması için kullanılan protokol. Tek yönlü veya karşılıklı CHAP olabilir. Tek yönlü CHAP ile hedef Başlatıcı kimliğini doğrular. Karşılıklı CHAP hedefi Başlatıcı kimliğini doğrulamak ve Başlatıcı Hedef doğrulayacağını gerektirir. |
| kopya |Bir birim yinelenen bir kopyası. |
| Bulut Katmanı (CaaT) olarak |Depolama mimarisi katmandadır olarak tüm depolama bir kurumsal depolama ağı parçası olarak görünmesi tümleşik depolama bulut. |
| Bulut hizmeti sağlayıcısı (CSP) |Bulut Hizmetleri Sağlayıcısı. |
| Bulut anlık görüntüsü |Bulutta depolanan birim verilerini zaman içinde nokta kopyası. Bir bulut anlık görüntüsü, farklı, şirket dışı depolama sisteminizde çoğaltılan bir anlık görüntü eşdeğerdir. Bulut anlık görüntüleri olağanüstü durum kurtarma senaryolarında özellikle kullanışlıdır. |
| Bulut depolama şifreleme anahtarı |Bir parola veya buluta cihazınız tarafından gönderilen şifrelenmiş verilere erişmek için StorSimple cihazınız tarafından kullanılan bir anahtar. |
| Küme durumunu algılayan güncelleştirme |Güncelleştirmelerin en düşük böylece yük devretme kümesindeki sunucularda yazılım güncelleştirmelerini yönetme veya servis kullanılabilirliğini etkilemez. |
| DataPath |Arası bağlı veri işleme işlemleri işlevsel birimlerini koleksiyonu. |
| Devre dışı bırakma |StorSimple cihazı ile ilişkili bulut hizmeti arasındaki bağlantıyı keser kalıcı bir eylem. Cihaz bulut anlık görüntüleri sonra bu işlemi kalmasını ve kopyalanması veya olağanüstü durum kurtarma için kullanılan. |
| Disk yansıtma |Mantıksal disk birimi ayrı sabit çoğaltmasını sürekli kullanılabilirliğini sağlamak için gerçek zamanlı olarak sürücüleri. |
| dinamik disk yansıtma |Dinamik diskler mantıksal disk birimi çoğaltma. |
| dinamik diskler |Veri depolamak ve birden çok fiziksel disklerde yönetmek için Mantıksal Disk Yöneticisi (LDM) kullanan bir disk birimi biçimi. Dinamik diskler, daha fazla boş alan sağlanacak büyütülebilir. |
| Genişletilmiş disk demet (EBOD) kasası |Ek depolama alanı için ek sabit diskleri içeren Microsoft Azure StorSimple Cihazınızı, ikincil bir kutu. |
| FAT sağlama |Bir geleneksel depolama hangi depolama alanına ayrılmış alanı göre sağlama gereksinimlerini beklenen (ve genellikle geçerli gerekiyor). Ayrıca bkz. *ölçülü kaynak sağlama*. |
| sabit disk sürücüsü (HDD) |Verileri depolamak için döndürme plaka kullanan bir sürücüye. |
| karma bulut depolama |Bulut depolama da dahil olmak üzere, yerel ve şirket dışı kaynak kullanan bir depolama mimarisi. |
| Internet küçük bilgisayar sistemi arabirimi (iSCSI) |Veri depolama donanımı veya tesis bağlama için bir Internet Protokolü IP tabanlı depolama ağ standardı. |
| iSCSI başlatıcısı |Bir dış iSCSI tabanlı depolama ağına bağlanmak için Windows çalıştıran bir konak bilgisayar sağlayan bir yazılım bileşeni. |
| iSCSI tam adını (IQN) |Bir iSCSI hedefi veya Başlatıcı tanımlayan benzersiz bir ad. |
| iSCSI hedefi |Merkezi iSCSI disk alt sistemleri depolama alanı ağlarında sağlayan bir yazılım bileşeni. |
| Arşivleme Canlı |Arşiv verileri (Site dışındaki bantta, örneğin depolandıktan değil) her zaman erişilebilir olduğu depolama yaklaşımı. Microsoft Azure StorSimple Canlı arşivleme kullanır. |
| yerel olarak sabitlenmiş birim |cihazda bulunan ve hiçbir zaman buluta katmanlı birim. |
| Yerel anlık görüntü |Zaman içinde nokta verilerin bir kopyasını Microsoft Azure StorSimple cihazında depolanmış birim. |
| Microsoft Azure StorSimple |Bir veri merkezi depolama Gereci ve BT kuruluşların dizinindeymiş gibi veri merkezi depolama biriminde bulut depolama yararlanmak yazılım oluşan güçlü bir çözümdür. StorSimple, maliyetleri azaltırken veri koruma ve veri yönetimini basitleştirir. Çözüm birincil depolama, Arşiv, yedekleme ve olağanüstü durum kurtarma (DR bulut ile sorunsuz tümleştirme yoluyla) birleştirir. SAN depolama ve bulut veri yönetimi bir kurumsal sınıf platformunda birleştirerek, StorSimple cihazları hızı, Basitlik ve güvenilirliği tüm depolama ile ilgili gereksinimler için etkinleştirin. |
| Güç ve soğutma Modülü (PCM) |StorSimple Cihazınızı güç kaynakları ve soğutma fanı, bu nedenle Power adı oluşan ve modül soğutma donanım bileşenleri. EBOD muhafazası iki 580W PCMs sahipken aygıtın birincil muhafaza iki 764W PCMs sahiptir. |
| Birincil kasası |Ana muhafaza StorSimple cihazınızın uygulama platformu denetleyicileri içeriyor. |
| Kurtarma süresi hedefi (RTO) |Bir iş sürecini veya sistem önce expended en uzun süreyi tam olarak bir olağanüstü durum sonra geri yüklendi. |
| Seri Bağlı SCSI (SAS) |Sabit disk sürücüsü (HDD) türü. |
| Hizmet verileri şifreleme anahtarı |StorSimple cihaz Yöneticisi hizmetine kaydeder herhangi yeni bir StorSimple aygıt için kullanıma sunulan bir anahtar. StorSimple cihaz Yöneticisi hizmeti ve aygıt arasında aktarılan yapılandırma verileri bir ortak anahtar kullanılarak şifrelenmiş ve ardından yalnızca özel bir anahtar kullanarak cihazda şifresi çözülebilir. Hizmet verileri şifreleme anahtarı şifre çözme için bu özel anahtarı edinmek hizmet sağlar. |
| Hizmet kayıt anahtarı |Bir anahtar, daha fazla yönetim işlemleri için Azure Portalı'nda görünmesi StorSimple cihaz StorSimple cihaz Yöneticisi Hizmeti'ne kaydedin yardımcı olur. |
| Küçük bilgisayar sistemi arabirimi (SCSI) |Fiziksel bilgisayarları birbirine bağlamak ve bunlar arasında veri geçirme için standartları kümesidir. |
| katı hal sürücüsü (SSD) |Hiçbir taşıma bölümleri içeren bir diski; Örneğin, bir flash sürücüye. |
| Depolama hesabı |Verilen bulut hizmeti sağlayıcı için depolama hesabınıza bağlı erişim kimlik bilgileri kümesi. |
| SharePoint için StorSimple Bağdaştırıcısı |SharePoint server grupları StorSimple depolama ve veri koruma şeffaf bir şekilde genişletir Microsoft Azure StorSimple bileşeni. |
| StorSimple cihaz Yöneticisi hizmeti |Azure StorSimple şirket içi ve sanal cihazları yönetmenize olanak sağlayan Azure portalı uzantısı. |
| StorSimple Snapshot Manager |Bir Microsoft Yönetim Konsolu (MMC) ek Microsoft Azure StorSimple yedekleme ve geri yükleme işlemleri yönetmek için bileşeni. |
| yedek alın |Bir birim etkileşimli yedekleyin olanak tanır. bir özellik. El ile yedekleme tanımlanmış bir ilke aracılığıyla otomatik bir yedekleme yapmayı aksine bir birimin almaya alternatif bir yöntemdir. |
| Ölçülü kaynak sağlama |Depolama sistemlerinde kullanılan kullanılabilir depolama alanı ile verimliliği en iyi duruma getirme yöntemi. Ölçülü kaynak sağlama depolama herhangi bir anda her kullanıcı tarafından gerekli en düşük alan göre birden çok kullanıcı arasında tahsis edilir. Ayrıca bkz. *fat sağlama*. |
| Katmanlama |Geçerli kullanımı, geçerlilik süresi ve diğer verilere ilişkisine göre mantıksal gruplandırmaları verileri düzenleme. StorSimple otomatik olarak katmanları verileri düzenler. |
| Birim |Sürücüleri biçiminde sunulan mantıksal depolama alanları. StorSimple birimlerini iSCSI ve StorSimple cihazını kullanımı ile keşfedilen dahil olmak üzere ana bilgisayar tarafından bağlanan birimler karşılık gelir. |
| Birim kapsayıcısı |Birimler ve kendileri için geçerli ayarları gruplandırması. StorSimple Cihazınızı tüm birimlerin birim kapsayıcıları gruplandırılır. Birim kapsayıcısı ayarları depolama hesapları, ilişkili şifreleme anahtarları ile bulut için gönderilen veri ve bulut içeren işlemleri için kullanılan bant genişliği için şifreleme ayarları içerir. |
| birim grubu |StorSimple anlık görüntü Yöneticisi'nde, bir birim grubu birimleri yedekleme işlemi kolaylaştırmak için yapılandırılmış bir koleksiyonudur. |
| Birim Gölge Kopyası Hizmeti (VSS) |Artımlı anlık görüntü oluşturmaya koordine etmek için VSS uyumlu uygulamalarla iletişim kurarak uygulama tutarlılığı kolaylaştıran bir Windows Server işletim sistemi hizmetidir. VSS anlık görüntülerinin alınma uygulamaları geçici olarak devre dışı olmasını sağlar. |
| StorSimple için Windows PowerShell |Bir Windows PowerShell tabanlı komut satırı çalıştırma ve StorSimple Cihazınızı yönetmek için kullanılan arabirim. Bazı Windows PowerShell temel özelliklerini korurken, bu arabirimi StorSimple cihazı yönetme doğrultusunda sağlamıştır ek adanmış cmdlet'leri vardır. |

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [StorSimple güvenlik](storsimple-8000-security.md).

