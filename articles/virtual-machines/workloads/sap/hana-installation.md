---
title: SAP HANA (büyük örnekler) Azure üzerinde SAP HANA yükleyin | Microsoft Docs
description: Nasıl bir SAP HANA (büyük örnek) Azure üzerinde SAP HANA yükleyin.
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/27/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1d335e135551b7b6faed8ee566acb14b46fd6c81
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43107520"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırın

Bu kılavuzu okumadan önce bilmeniz gereken bazı önemli hususlar verilmiştir. İçinde [SAP HANA (büyük örnekler) genel bakışı ve mimarisi azure'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) HANA büyük örneği birimleri ile iki farklı sınıflardaki tanıtımını yaptık:

- S72, S72m, S144, S144m, S192, S192m ve 'ı sınıf türü olarak' diyoruz S192xm, SKU.
- S384, S384m S384xm, S384xxm, S576m, S576xm, S768m, S768xm ve form veya S960m 'Type II sınıfının' SKU'ları diyoruz.

Sınıfı tanımlayıcısı, HANA büyük örneği belgelerinde farklı özellikleri ve gereksinimleri HANA büyük örneği SKU'larında göre sonuçta başvurmak için kullanılacak geçiyor.

Sık kullandığımız diğer tanımları şunlardır:
- **Büyük örneği damgasında:** SAP HANA TDI olan bir donanım altyapısı yığını sertifikalı ve azure'da SAP HANA örnekleri çalıştırmak için ayrılmış.
- **SAP HANA (büyük örnekler) azure'da:** üzerinde SAP HANA TDI sertifikalı büyük örnek Damgalar farklı Azure bölgelerindeki dağıtılmış donanım HANA çalıştırmak Azure teklifi için resmi ad örnekler. İlgili dönem **HANA büyük örneği** için SAP HANA (büyük örnekler) azure'da ve yaygın olarak kullanılan bu teknik dağıtım kılavuzu.


SAP HANA yüklemesi sizin sorumluluğunuzdur ve yeni bir SAP hana (büyük örnekler) Azure sunucuya aktarmadan sonra etkinlik başlayabilirsiniz. Ve sonra Azure Vnet'lerinizdeki ve HANA büyük örneği birimi arasında bağlantı. 

> [!Note]
> Her ilke SAP, SAP HANA yüklemesi SAP HANA yüklemelerini gerçekleştirmek için sertifikalı bir kişi tarafından gerçekleştirilmesi gerekir. Geçti sınavı teknoloji ilişkilendirmek SAP sertifikalı, SAP HANA yüklemesi sertifika sınavı, bir kişi, veya bir SAP Sertifikalı Sistem entegratörü (sı).

Özellikle HANA 2. 0'da, yükleme planlarken yeniden denetle [SAP destek Not #2235581 - SAP HANA: desteklenen işletim sistemleri](https://launchpad.support.sap.com/#/notes/2235581/E) ile işletim Sisteminin desteklendiğinden emin olmak için SAP HANA sürüm, yüklemeye karar vermiştir. Desteklenen işletim sistemi HANA 2.0 için HANA 1.0 için desteklenen işletim sistemi daha sınırlı olduğunu unutmayın. 

> [!IMPORTANT] 
> Bu noktada Type II birimleri yalnızca SLES 12 SP2 işletim sistemi sürümü desteklenmiyor. 

## <a name="first-steps-after-receiving-the-hana-large-instance-units"></a>HANA büyük örneği birim aldıktan sonra ilk adımlar

**İlk adım** örneğinin işletim sistemi, işletim sistemi sağlayıcısı ile kaydolmak üzere HANA büyük örnek alma sonra kurulan erişimi ve bağlantı örneklerine sahip olduğu. Bu adım, SUSE Linux işletim sistemi SUSE SMT, azure'da bir sanal makinede dağıtılması gereken bir örneğini kaydetme dahildir. HANA büyük örneği birim bu SMT örneğine (Bu belgede daha sonra bakın) bağlanabilirsiniz. Ya da Red Hat işletim sisteminizi Red Hat abonelik bağlanmanız gereken Yöneticisi ile kayıtlı olması gerekir. Bkz: Ayrıca açıklamalar bu [belge](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu adım Ayrıca işletim sistemi düzeltme eki gereklidir. Müşterinin sorumluluğundadır içinde bir görev. SUSE için yükleme ve yapılandırma SMT belgelere [burada](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**İkinci adım** yeni yamaları ve düzeltmeler belirli işletim sistemi sürümünün sürüm/denetlemek için. HANA büyük örneği düzeltme eki düzeyini en son durumda olup olmadığını denetleyin. İşletim sistemi düzeltme eki/sürümleri ve Microsoft dağıtabilirsiniz görüntü değişiklikleri zamanlamaya bağlı olarak, burada en son düzeltme eklerinin eklenmeyebilir durumlar olabilir. Bu nedenle, zorunlu bir uygulanması gerekir ve güvenlik, İşlevler, kullanılabilirlik ve performans için ilgili düzeltme ekleri bu arada belirli Linux satıcı tarafından yayımlanan olup olmadığını denetlemek için bir HANA büyük örneği birim üzerinde aldıktan sonra adımdır.

**Üçüncü adım** yükleme ve SAP HANA belirli işletim sistemi sürüm/sürümünün yapılandırma ilgili SAP notları kullanıma sağlamaktır. Öneriler veya değişiklikleri nedeniyle SAP notları veya tek tek yükleme senaryoları bağımlı olan yapılandırmalar için Microsoft her zaman bir HANA büyük örneği birim mükemmel bir şekilde yapılandırılmış olması mümkün olmayacaktır. Bu nedenle, sizin için SAP notları tam Linux sürümünüzü üzerinde SAP HANA için ilgili okumak için bir müşteri olarak zorunludur. Ayrıca, işletim sistemi sürümünün sürüm/gerekli yapılandırmalarını kontrol edin ve uygulama yapılandırma ayarlarını nerede zaten yapılmadı.

Özel, aşağıdaki parametreleri denetleyin ve sonunda şekilde ayarlanır:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- NET.Core.rmem_default 16777216 =
- NET.Core.wmem_default 16777216 =
- net.core.optmem_max = 16777216
- NET.ipv4.tcp_rmem 65536 16777216 16777216 =
- NET.ipv4.tcp_wmem 65536 16777216 16777216 =

RHEL 7.2 SLES12 SP1 ile başlayarak, bu parametreleri /etc/sysctl.d dizinindeki bir yapılandırma dosyasında ayarlanmalıdır. Örneğin, bir yapılandırma dosyası 91-NetApp-HANA.conf adı ile oluşturulmalıdır. Eski SLES ve RHEL sürümleri için bu parametreler kümesi in/etc/sysctl.conf olması gerekir.

İçin tüm RHEL serbest bırakır ve SLES12 ile başlatılıyor 
- sunrpc.tcp_slot_table_entries 128 =

parametreyi in/etc/modprobe.d/sunrpc-local.conf ayarlamanız gerekir. Dosya mevcut değilse, önce aşağıdaki giriş ekleyerek oluşturulmalıdır: 
- seçenekleri sunrpc tcp_max_slot_table_entries 128 =

**Dördüncü adım** HANA büyük örneği birim sistem saati denetlemektir. Örnekler, HANA büyük örneği damgasında bulunan Azure bölgesinin konumunu temsil eden bir sistem saat dilimi ile dağıtılır. Sistem saati veya size ait örnekler saat dilimini değiştirmek ücretsizdir. Bunun yapılması ve kiracınız ile daha fazla örnek sıralama, yeni teslim edilen örnekler saat dilimini uyum sağlamak ihtiyacınız hazırlıklı olmalıdır. Microsoft operations örnekleriyle devreden MultiPath sonra ayarladığınız hiçbir Öngörüler sistem saat dilimi vardır. Bu nedenle yeni dağıtılan örnekleri için değiştirilmiş bir aynı saat diliminde ayarlanmamış olabilir. Sonuç olarak, bu, denetlemek ve gerekirse devredildiği örneklerdeki saat dilimini uyarlamak için müşteri olarak sorumluluğundadır. 

**Beşinci adım** etc/hosts denetlemektir. Dikey pencereleri devredildiği gibi farklı amaçlar için (sonraki bölüme bakın) atanmış farklı IP adreslerini sahiptirler. Etc/hosts dosyasını kontrol edin. Birimler mevcut bir kiracınız burada eklenir durumlarda, daha önce teslim edilen sistemleri IP adresleriyle doğru tutulan yeni dağıtılan sistemler vb./ana sahip olmayı beklediğiniz yok. Bu nedenle, müşteri olarak doğru ayarlarını denetlemek için olduğu kadar yeni dağıtılan örnek etkileşim ve daha önce dağıtılan birimleri kiracınızdaki adlarını çözümlemek. 

## <a name="networking"></a>Ağ
Biz, öneriler, Azure sanal ağları tasarlama ve bu belgelerdeki açıklandığı HANA büyük örnekleri için bu sanal ağları bağlanma uyguladığınız varsayılmaktadır:

- [SAP HANA (büyük örnek) genel bakış ve azure'da mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Tek birimlerinin ağı hakkında bahsetmek için bazı ayrıntılar değer vardır. Her HANA büyük örneği birim, iki veya üç NIC bağlantı noktası birimi için atanmış olan iki veya üç IP adresi ile birlikte gelir. Üç IP adresini, HANA genişleme yapılandırmaları ve HANA sistem çoğaltması senaryo kullanılır. Birimin NIC için atanan IP adresleri biri öğesinde tanımlanan sunucu IP havuzu dışında [SAP HANA (büyük örnek) genel bakışı ve mimarisi azure'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

Başvuru [HLI desteklenen senaryoları](hana-supported-scenario.md) Mimarinizi ethernet ayrıntılarını öğrenin.

## <a name="storage"></a>Depolama

Kılavuz çizgileri açıklandığı gibi önerilen SAP aracılığıyla Azure hizmet yönetimi üzerinde SAP HANA (büyük örnekler) Azure üzerinde SAP HANA depolama düzenini yapılandıran [SAP HANA depolama gereksinimlerini](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemesi. Kaba boyutları farklı HANA büyük örnekleri SKU'ları ile farklı birimler belirtilmiştir [SAP HANA (büyük örnek) genel bakışı ve mimarisi azure'da](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Depolama birimleri adlandırma kurallarını aşağıdaki tabloda listelenmiştir:

| Depolama kullanımı | Takma adı | Birim adı | 
| --- | --- | ---|
| HANA verileri | /hana/Data/SID/mnt0000<m> | Depolama IP: / hana_data_SID_mnt00001_tenant_vol |
| HANA günlüğü | /hana/log/SID/mnt0000<m> | Depolama IP: / hana_log_SID_mnt00001_tenant_vol |
| HANA günlük yedekleme | /hana/log/Backups | Depolama IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| Paylaşılan HANA | /hana/Shared/SID | Depolama IP: / hana_shared_SID_mnt00001_tenant_vol/paylaşılan |
| usr/sap | /usr/SAP/SID | Depolama IP: / hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Burada SID HANA örneği sistem kimliği = 

Ve Kiracı işlemleri iç numaralandırması bir kiracı dağıtırken =.

Gördüğünüz gibi paylaşılan HANA ve usr/sap aynı birimde paylaşıyor. Bağlama terminolojiyi sistem kimliği bağlama numarası yanı sıra, HANA örnekleri içerir. Ölçek büyütme dağıtımlarda yalnızca var. mnt00001 gibi bir bağlama Genişleme dağıtımı gibi birçok başlatmalar, görür ancak çalışan ve ana düğümünüz vardır. Ölçek genişletme ortam, veri, günlük için günlük yedekleme birimleri paylaşılan ve genişletme yapılandırma her düğüme eklenmiş. SAP birden fazla çalışan yapılandırmaları için birimleri farklı bir dizi oluşturulur ve en büyük örnek HAN birimine bağlı. Başvuru [HLI desteklenen senaryoları](hana-supported-scenario.md) senaryonuz için depolama düzeni Ayrıntılar için.

İncelemeyi okuyun ve HANA büyük örneği birim ara birimleri HANA/veri yerine cömert disk birimi ile gelir ve birim HANA/log/yedekleme sahibiz farkında olun. Neden biz HANA/veri çok büyük boyutta bir müşteri olarak sunuyoruz, depolama anlık görüntüleri aynı disk birimi kullandığınızı nedenidir. Daha fazla depolama alanı geldiğini gerçekleştirdiğiniz anlık görüntüleri, daha fazla alan atanan depolama birimlerinizi anlık görüntü tarafından kullanılır. HANA/log/yedekleme birimi, veritabanı yedeklemeleri yerleştirmek için toplu olarak düşünülebilir değil. HANA işlem günlüğü yedeklemeleri için yedekleme birimi olarak kullanılacak boyutlandırılır. Gelecek sürümleri depolama alanının self servis, bu belirli birimi daha sık anlık görüntü için hedef anlık görüntü. Ve seçeneği HANA büyük örneği altyapısı tarafından sağlanan olağanüstü durum kurtarma işlevleri için açmak isterse ile daha fazla çoğaltma, olağanüstü durum kurtarma sitesine sık. Ayrıntılara bakın [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve azure'da olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Sağlanan depolama alanına ek olarak, 1 TB'lik artışlarla ek depolama kapasitesi satın alabilirsiniz. Bu ek depolama alanı yeni birimleri, HANA büyük örnekler için eklenebilir.

Azure hizmet yönetimi üzerinde SAP HANA ile ekleme sırasında müşterinin bir kullanıcı kimliği (UID) ve Grup Kimliği (GID) için sidadm kullanıcı ve sapsys grubunu belirtir (örn: 1000,500) SAP HANA sistem yüklemesi sırasında aynı değerler kullanılır gereklidir. Bir birim birden çok HANA örneklerine dağıtmak istediğiniz gibi birden fazla birimler (her örneği için bir küme) alın. Sonuç olarak, dağıtım sırasında tanımlamanız gerekir:

- (Sidadm dışında türetilir) farklı HANA örnekleri SID'si.
- Bellek boyutları farklı HANA örnekleri. Örnek başına bellek boyutu her ayrı ayrı toplu kümede birimlerin boyutunu tanımladığından.

Aşağıdaki bağlama seçeneklerini tüm bağlı birimleri için yapılandırılmış olan depolama sağlayıcısı önerileri göre (önyükleme LUN hariç):

- NFS rw vers = 4, sabit, timeo 600 rsize = 1048576, wsize = 1048576, giriş, noatime, kilit 0 0

Bu bağlama noktaları yapılandırılmış/etc/fstab gibi aşağıdaki grafiklerde gösterilmektedir:

![bağlanan birimlerin HANA büyük örneği birimindeki fstab](./media/hana-installation/image1_fstab.PNG)

Komut SD -h S72m HANA büyük örneği birim üzerinde çıkışını gibi görünür:

![bağlanan birimlerin HANA büyük örneği birimindeki fstab](./media/hana-installation/image2_df_output.PNG)


Depolama denetleyicisi ve düğümler büyük örnek damgaları NTP sunucuları eşitlenir. Sizinle birimleri (büyük örnekler) Azure üzerinde SAP HANA ve Azure Vm'leri bir NTP sunucusuna karşı eşitleme olması gerekir altyapısı ve Azure ya da büyük örnek Damgalar işlem birimleri arasında hiçbir önemli zaman kayması oluşuyor.

SAP HANA altında kullanılan depolama alanı için iyileştirmek için aşağıdaki SAP HANA yapılandırma parametrelerini ayarlamanız gerekir:

- max_parallel_io_requests 128
- üzerinde async_read_submit
- üzerinde async_write_submit_active
- Tüm async_write_submit_blocks
 
SPS12 kadar SAP HANA 1.0 sürümleri için bu parametreleri SAP HANA veritabanı yüklemesi sırasında açıklanan şekilde ayarlanabilir [SAP notu #2267798 - SAP HANA veritabanı yapılandırması](https://launchpad.support.sap.com/#/notes/2267798)

Parametreleri SAP HANA veritabanı yükleme sonrasında hdbparam çerçevesi kullanılarak da yapılandırabilirsiniz. 

SAP HANA 2.0 ile hdbparam framework kullanım dışıdır. Sonuç olarak parametreleri SQL komutlarını kullanarak ayarlamalısınız. Ayrıntılar için bkz [SAP notu #2399079: hdbparam HANA 2 Saydamlığından](https://launchpad.support.sap.com/#/notes/2399079).

Başvuru [HLI desteklenen senaryoları](hana-supported-scenario.md) Mimarinizi depolama düzenini öğrenin.

## <a name="operating-system"></a>İşletim sistemi

> [!IMPORTANT] 
> Bu noktada Type II birimleri yalnızca SLES 12 SP2 işletim sistemi sürümü desteklenmiyor. 

Teslim edilen işletim sistemi görüntüsünün takas alanı 2 GB göre ayarlanmış [SAP destek Not #1999997 - SSS: SAP HANA bellek](https://launchpad.support.sap.com/#/notes/1999997/E). İstenen tüm farklı ayarı sizin tarafınızdan bir müşteri olarak ayarlanması gerekir.

[SUSE Linux Enterprise Server 12 SP1 SAP uygulamaları için](https://www.suse.com/products/sles-for-sap/hana) SAP hana (büyük örnekler) Azure üzerinde yüklü bir Linux dağıtımıdır. Bu belirli dağıtım SAP özgü özellikleri sağlayan &quot;hazır&quot; (SAP SLES üzerinde etkili bir şekilde çalıştırmak için önceden ayarlanmış parametreleri de dahil olmak üzere).

Bkz: [Kaynak Kitaplığı/tanıtım yazıları](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) SUSE Web sitesinde ve [SUSE üzerinde SAP](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) SAP topluluk ağ (SCN) üzerinde SAP HANA (yüksek Kurulum dahil olmak üzere SLES dağıtımıyla ilgili çeşitli yararlı kaynaklar için Kullanılabilirlik, güvenliğin sağlamlaştırılmasını SAP işlemlerine özgü vb.).

Ek ve kullanışlı SAP SUSE ilgili bağlantıları:

- [SUSE Linux Site üzerinde SAP HANA](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [En iyi yöntem için SAP: kuyruğa çoğaltma – SAP NetWeaver'ı SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – SAP için SLES virüs koruması](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (SAP uygulamaları için SLES 12 dahil).

SAP destek Notlar SLES 12 üzerinde SAP HANA uygulamak için uygulanabilir:

- [SAP destek Not #1944799 – SLES işletim sistemi yüklemesi için SAP HANA yönergeleri](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [SAP destek Not #2205917 – SAP HANA veritabanı SLES 12 işletim sistemi ayarlarını SAP uygulamaları için önerilen](https://launchpad.support.sap.com/#/notes/2205917/E).
- [SAP destek Not #1984787 – SUSE Linux Enterprise Server 12: Yükleme notları](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP destek Not #171356 – Linux'ta SAP yazılımı: genel bilgiler](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP destek Not #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070).

[SAP HANA için Red Hat Enterprise Linux](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) HANA büyük örnekler üzerinde SAP HANA çalıştırmayı için başka bir tekliftir. RHEL 6.7 ve 7.2 sürümlerinde kullanılabilir. Lütfen yerel Azure burada yalnızca RHEL 7.2 ve daha yeni sürümlerde desteklenir, Vm'leri HANA büyük örnekleri ters yönde Not destek RHEL 6.7 de. Ancak bir RHEL 7.x sürümü kullanmanızı öneririz.

Red Hat ilgili bağlantılar üzerinde SAP ek ve kullanışlıdır:
- [SAP HANA Red Hat Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

SAP destek notları Red Hat üzerinde SAP HANA uygulamak için uygulanabilir:

- [SAP destek Not #2009879 - Red Hat Enterprise Linux (RHEL) işletim sistemi için SAP HANA yönergeleri](https://launchpad.support.sap.com/#/notes/2009879/E).
- [SAP destek Not #2292690 - SAP HANA veritabanı: RHEL 7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/2292690).
- [SAP destek Not #2247020 - SAP HANA veritabanı: Önerilen işletim sistemi ayarlarını RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [SAP destek Not #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070).
- [SAP destek Not #2228351 - Linux: SAP HANA veritabanı SPS 11 düzeltme 110 (veya üzeri) RHEL 6 veya SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [SAP destek Not #2397039 - SSS: RHEL üzerinde SAP](https://launchpad.support.sap.com/#/notes/2397039).
- [SAP destek Not #1496410 - Red Hat Enterprise Linux 6.x: yükleme ve yükseltme](https://launchpad.support.sap.com/#/notes/1496410).
- [SAP destek Not #2002167 - Red Hat Enterprise Linux 7.x: yükleme ve yükseltme](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Zaman eşitleme

SAP NetWeaver bir mimari temelinde SAP uygulamaları SAP sistemi oluşturan çeşitli bileşenler için saat farklılıklarını duyarlıdır. SAP ABAP kısa dökümleri ZDATE hata başlığı ile\_büyük\_zaman\_fark olan büyük olasılıkla alışılmış, farklı sunucular veya VM'ler sistem saati çok fazla ara drifting bu kısa dökümleri görünür olarak.

(Büyük örnekler) azure'da SAP HANA için Azure eklenmemişse bitti eşitleme zaman&#39;büyük örnek Damgalar işlem biriminde t uygulanır. Azure, bir sistem sağlar gibi bu eşitleme yerel Azure Vm'lerde SAP uygulamaları çalıştırmak için geçerli değildir&#39;s zaman düzgün bir şekilde eşitlenir. Sonuç olarak, SAP tarafından kullanılabilen sunucu ayarlanmalıdır ayrı birer HANA büyük örnekler üzerinde çalışan örneklerinin Azure sanal makineleri ve SAP HANA üzerinde çalışan uygulama sunucuları veritabanı. Bir depolama altyapısı büyük örnek damgaları NTP sunucuları ile eşitlenmiş saattir.

## <a name="setting-up-smt-server-for-suse-linux"></a>SUSE Linux için SMT sunucusu ayarlama
SAP HANA büyük örnekleri, doğrudan Internet bağlantısı yok. Bu nedenle, böyle bir birim OS sağlayıcıya Kaydol indirin ve düzeltme ekleri uygulama için basit bir işlem değil. SUSE Linux söz konusu olduğunda, Azure VM'deki bir SMT sunucusunu ayarlamak için tek bir çözüm olabilir. Azure VM, HANA büyük örneği için bağlı olan bir Azure VNet, barındırılması gerekiyor ancak. Böyle bir SMT sunucusu ile HANA büyük örneği birim kaydedin ve düzeltme eklerini indirmek. 

SUSE üzerinde daha büyük bir kılavuz sağlar, [SLES 12 SP2 için abonelik yönetimini](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

HANA büyük örneği için görev karşılayan bir SMT sunucusunun yüklenmesi için önkoşul ihtiyacınız:

- Bir Azure HANA büyük örneği ER bağlantı hattına bağlı sanal ağ.
- Bir kuruluşla ilişkili olan bir SUSE hesabı. Kuruluş bazı geçerli SUSE aboneliğinizin olması gerekir ancak.

### <a name="installation-of-smt-server-on-azure-vm"></a>Azure sanal makinesinde SMT server yüklemesi

Bu adımda, Azure VM'deki SMT sunucusu yükleyin. Oturum açmak için ilk ölçüdür [SUSE Müşteri merkezi](https://scc.suse.com/)

Oturum açtığınız gibi kuruluş Git kuruluş kimlik-->. Bu bölümde SMT sunucusu kurmak gerekli kimlik bilgilerini bulmanız gerekir.

Üçüncü adım SUSE Linux VM Azure Vnet'te yüklemektir. VM dağıtmak için Azure SLES 12 SP2 galeri görüntüsü alın. Dağıtım işlemi, bir DNS adı tanımlayın yok ve bu ekran görüntüsünde görüldüğü gibi statik IP adresleri kullanmayın

![SMT sunucusu için VM dağıtımı](./media/hana-installation/image3_vm_deployment.png)

Dağıtılan VM, daha küçük bir VM olan ve iç IP adresi 10.34.1.4 Azure Vnet'te aldı. VM'nin adı smtserver oluştu. Yükleme sonrasında, HANA büyük örneği birim bağlantısını denetlendi. Ad çözümlemesinin nasıl düzenlediğiniz bağımlı, HANA büyük örneği birimlerinin çözümlemesi vb./konakları Azure VM'nin yapılandırmanız gerekebilir. Düzeltme ekleri tutmak için kullanılacak giden VM'ye ek bir disk ekleyin. Önyükleme diski çok küçük olabilir. Gösterilen durumda da, aşağıdaki ekran görüntüsünde gösterildiği gibi /srv/www/htdocs disk takılı. 100 GB disk yeterli olacaktır.

![SMT sunucusu için VM dağıtımı](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

HANA büyük örneği birimi için oturum açın, / etc/hosts korumak ve SMT sunucunun ağ üzerinde çalıştırmak için gereken Azure VM ulaşıp ulaşamadığını denetleyin.

Bu denetimi başarıyla tamamlandıktan sonra SMT sunucunun çalışması gereken Azure VM ile oturum açmanız gerekir. VM'de oturum açmak için putty kullanıyorsanız, bu komutları dizisini bash pencerenizde yürütülecek gerekir:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Bu komutlar yürütüldükten sonra ayarları etkinleştirmek için bash yeniden başlatın. Daha sonra YAST başlatın.

Yazılım bakımı ve smt araması YAST içinde gidin. Aşağıda gösterildiği gibi yast2 smt için otomatik olarak geçer smt, seçin

![SMT yast içinde](./media/hana-installation/image5_smt_in_yast.PNG)


Smtserver yüklemesinde seçimini kabul edin. Yüklendikten sonra SMT Sunucu Yapılandırması'na gidin ve daha önce aldığınız SUSE müşteri Merkezi'nden Kurumsal kimlik bilgilerinizi girin. Ayrıca, Azure VM konak adı SMT sunucu URL'sini girin. Bu örnekte olduğu https://smtserver sonraki grafikleri görüntülendiği gibi.

![SMT sunucusunun yapılandırması](./media/hana-installation/image6_configuration_of_smtserver1.png)

Sonraki adım olarak, SUSE müşteri Merkezi'ne bağlantı çalışıp çalışmadığını test etmelisiniz. Tanıtım durumda, aşağıdaki grafiklerde gördüğünüz gibi çalıştı.

![Test SUSE müşteri Merkezi'ne bağlanma](./media/hana-installation/image7_test_connect.png)

Bir kez SMT Kurulum başladığında, veritabanı parolasını sağlamanız gerekir. Yeni bir yüklemesi olduğundan, bu parolayı sonraki grafikleri gösterildiği tanımlamanız gerekir.

![Veritabanı parolasını tanımlayın](./media/hana-installation/image8_define_db_passwd.PNG)

Bir sertifika oluşturulur sonraki etkileşim sahip olduğunuz durumdur. Aşağıda gösterildiği iletişim kutusundan gidin ve adım devam etmelisiniz.

![SMT sunucusu için sertifika oluşturma](./media/hana-installation/image9_certificate_creation.PNG)

Yapılandırmanın sonunda 'Eşitleme denetimi Çalıştır' adımı harcanan bazı dakika olabilir. Yükleme ve yapılandırma SMT sunucusunun sonra bağlama noktası /srv/www/htdocs altında dizin deposu bulmalısınız/artı bazı alt dizinler depo altında. 

SMT sunucunun ve ilgili hizmetleri şu komutlarla yeniden başlatın.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>SMT sunucuya paketleri indirin

Tüm hizmetleri yeniden başlatıldıktan sonra SMT Yast kullanarak Yönetimi'nde uygun paketleri seçin. Paket seçimi, HANA büyük örneği sunucu işletim sistemi görüntüsü ve SLES sürüm veya sürüm SMT server çalıştıran sanal makinenin üzerinde bağlıdır. Seçimi ekran örneği aşağıda gösterilmiştir.

![Paketleri seçin](./media/hana-installation/image10_select_packages.PNG)

Paket seçimi tamamladıktan sonra ayarladığınız SMT sunucuya paket seçmek ilk kopyasını başlatmanız gerekir. Bu kopya komut smt-aşağıda gösterildiği gibi yansıtma kullanarak Kabuğu'nda tetiklendi


![SMT sunucuya paketleri indirin](./media/hana-installation/image11_download_packages.PNG)

Yukarıda gördüğünüz gibi bağlama noktası /srv/www/htdocs altında oluşturulan dizinleri halinde paketler kopyalanmasını. Bu işlem biraz sürebilir. Seçtiğiniz kaç paketin bağımlı, bunu bir saat veya daha fazla sürebilir.
Bu işlem bittikçe SMT istemci kurulum taşımanız gerekir. 

### <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>HANA büyük örneği birimleri SMT istemcide ayarlama

İstemci bu durumda HANA büyük örneği birimleridir. SMT Sunucusu Kurulumu betik clientSetup4SMT.sh Azure VM'de oturum kopyalanır. Üzerinde HANA büyük örneği birimi komut dosyası kopyalama SMT sunucunuza bağlanmak istediğiniz. -H seçeneğiyle komut dosyasını başlatmak ve parametre olarak SMT sunucunuzun adını verin. Bu örnek smtserver.

![SMT istemciyi Yapılandırma](./media/hana-installation/image12_configure_client.PNG)

Bir senaryo burada sunucu tarafından istemci sertifikası yükünü başarılı oldu, ancak aşağıda gösterildiği gibi kayıt başarısız olabilir.

![İstemci kaydı başarısız](./media/hana-installation/image13_registration_failed.PNG)

Kayıt başarısız olursa bu okuma [SUSE belge desteği](https://www.suse.com/de-de/support/kb/doc/?id=7006024) ve burada açıklanan adımları yürütün.

> [!IMPORTANT] 
> Sunucu adı olarak tam etki alanı adı olmadan bu büyük/küçük harf smtserver, VM'nin adı sağlamanız gerekir. Yalnızca VM adı çalışır. 

Adımları yürütülen sonra aşağıdaki komutu yürütün HANA büyük örneği biriminde gerekir

```
SUSEConnect –cleanup
```

> [!Note] 
> Bizim testleri her zaman bu adımdan sonra birkaç dakika beklemeniz vardı. Hemen yürütme clientSetup4SMT.sh SUSE makalesinde açıklanan düzeltici önlemleri sonra sertifika henüz geçerli olmaz iletileri ile sona erdi. Başarılı istemci yapılandırmasında, genellikle 5-10 dakika bekleyen ve clientSetup4SMT.sh yürütme sona erdi.

Sorunu düzeltmek için SUSE makalenin adımlara göre gerekli olan çalıştırdıysanız, HANA büyük örneği birimi clientSetup4SMT.sh yeniden yeniden başlatmanız gerekir. Şimdi, aşağıda gösterildiği gibi başarıyla tamamlanmalıdır.

![İstemci kaydı başarılı](./media/hana-installation/image14_finish_client_config.PNG)

Bu adımda, Azure sanal Makinesinde yüklü SMT sunucusuna karşı bağlanmak için HANA büyük örneği biriminin SMT istemci yapılandırılmış. Şimdi, HANA büyük örnekler için işletim sistemi düzeltme ekleri yüklemek veya ek paketler yüklemek için 'kurmak zypper' veya 'ı zypper içinde' alabilir. SMT sunucuda önceden indirdiğiniz eklerini yalnızca alabilir anlaşılır.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>HANA büyük örnekler üzerinde SAP HANA yükleme örneği
Bu bölümde, bir HANA büyük örneği birim üzerinde SAP HANA yükleneceği gösterilmektedir. Sahip olduğumuz başlangıç durumu gibi görünür:

- Microsoft tüm verileri bir SAP HANA büyük örneği dağıtmak için sağladığınız.
- SAP HANA büyük örneği Microsoft'tan aldığınız.
- Şirket içi ağınıza bağlı olan Azure sanal ağı oluşturduğunuz.
- Aynı Azure sanal ağı için HANA büyük örnekler için ExpressRotue devre bağlı.
- Azure HANA büyük örnekleri için bir Sıçrama kutusu kullandığınız sanal yüklediğiniz.
- HANA büyük örneği birim ve tersi Sıçrama kutusundan bağlanabildiğinden emin yaptığınız.
- Tüm gerekli paketleri ve düzeltme eklerinin yüklü olup olmadığını kontrol.
- SAP notları ve belgeler HANA yüklemesi kullanıyorsanız ve HANA yayın tercih ettiğiniz işletim sisteminde desteklendiğinden emin yapılan işletim sistemi ile ilgili makaleyi yayın.

Sonraki sıralarında gösterilene atlama kutusuna bu durumda bir Windows işletim sisteminde, HANA büyük örneği birim paketler kopyasını ve Kurulum dizisini çalıştıran VM, HANA yükleme paketlerinin indirilir.

### <a name="download-of-the-sap-hana-installation-bits"></a>SAP HANA yükleme indirme
HANA büyük örneği birimleri doğrudan internet bağlantısı yoksa HANA büyük örneği VM SAP'den yükleme paketleri doğrudan indiremez. Eksik doğrudan internet bağlantısı üstesinden gelmek için atlama kutusunu gerekir. Atlama kutusuna VM paketleri karşıdan yükleyin.

HANA yükleme paketleri indirmek için bir SAP S-kullanıcı veya SAP Market erişmenize olanak sağlayan diğer kullanıcı gerekir. Oturum açtıktan sonra bu ekranları sırasıyla gidin:

Git [SAP Service Marketplace](https://support.sap.com/en/index.html) > karşıdan yükleme yazılımı tıklayın > yükleme ve yükseltme > alfabetik dizin tarafından > – H altında SAP HANA Platform sürümü > SAP HANA Platform sürümü 2.0 > yükleme > indirin Aşağıdaki dosyaları

![HANA yüklemesi indirin](./media/hana-installation/image16_download_hana.PNG)

SAP HANA 2.0 yükleme paketleri indirilen tanıtım durumda. Azure atlama çubuğundaki VM, kendi kendine ayıklanan arşivleri dizine aşağıda gösterildiği gibi genişletin.

![HANA yüklemesi ayıklayın](./media/hana-installation/image17_extract_hana.PNG)

Arşivleri ayıklanan gibi çıkarma durumda 51052030, yukarıda oluşturduğunuz bir dizine /hana/shared birim içine HANA büyük örneği birimine tarafından oluşturulan dizine kopyalayın.

> [!Important]
> Alanı sınırlıdır ve başka işlemler tarafından kullanılması gereken yükleme paketleri kök ya da önyükleme LUN kopyalamayın.


### <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA HANA büyük örneği biriminde yükleyin
SAP HANA yüklemek için kullanıcı kök olarak oturum açmanız gerekir. Yalnızca kök SAP HANA yüklemek için yeterli izinlere sahiptir.
Üzerinden/hana paylaşılan / kopyaladığınız dizin izinlerini ayarlamak için yapmanız gereken ilk şey var. İzinleri ayarlamanız gerekir

```
chmod –R 744 <Installation bits folder>
```

Grafik Kurulum kullanılarak SAP HANA'ya yüklemek istiyorsanız, gtk2 paket HANA büyük örnekler üzerinde yüklü olması gerekir. Komutu ile yüklü olup olmadığını denetleyin

```
rpm –qa | grep gtk2
```

Daha fazla adımlarda size grafik kullanıcı arabirimi ile SAP HANA Kurulum gösteren. Sonraki adım olarak yükleme dizinine gidin ve HDB_LCM_LINUX_X86_64 alt dizinine gidin. Başlatma

```
./hdblcmgui 
```
Bu dizini. Artık, bir dizi ekranlar arasında veri sağlayacağınız için gerek duyduğunuz senaryolara gerçekleşir. Gösterilen durumda biz SAP HANA veritabanı sunucusu ve SAP HANA istemci bileşenlerini yüklüyorsunuz. Bu nedenle bizim 'SAP HANA veritabanı' aşağıda gösterildiği gibi seçimdir

![HANA yüklemede seçin](./media/hana-installation/image18_hana_selection.PNG)

Sonraki ekranda, yeni sistemi yükleyin' seçeneğini seçin

![Yeni yükleme HANA seçin](./media/hana-installation/image19_select_new.PNG)

Bu adımdan sonra ayrıca SAP HANA veritabanı sunucusuna yüklenebilir çeşitli ek bileşenler arasındaki seçmeniz gerekir.

![Ek HANA bileşenleri seçin](./media/hana-installation/image20_select_components.PNG)

Bu belgede, SAP HANA Client ve SAP HANA Studio seçtik. Ayrıca bir ölçek büyütme örneği yüklediğimiz. Bu nedenle 'Tek konak System' seçim yapması sonraki ekranda, 

![Ölçek büyütme yüklemesi seçin](./media/hana-installation/image21_single_host.PNG)

Sonraki ekranda, bazı veri sağlamanız gerekir.

![SAP HANA SID sağlayın](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> HANA sistemi kimliği (SID) olarak aynı SID, HANA büyük örneği dağıtım sıralı olduğunda Microsoft sağladığınız sağlamanız gerekir. Farklı bir SID seçerek farklı birimlerde erişim izin sorunları nedeniyle başarısız yükleme yapar

Yükleme olarak paylaşılan dizin directory/hana/kullanırsınız. Sonraki adımda, HANA veri dosyaları ve HANA günlük dosyaları için konumlar sağlamanız gerekir.


![HANA günlük konumu girin](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Verileri olarak tanımlayın ve günlük dosyaları zaten bu ekranı önce ekran seçimdeki seçtiğiniz SID içeren bağlama noktalarını birlikte gelen birimler gerekir. SID yazdığınız bir uyuşmazlık, ekranda önce geri dönüp SID bağlama noktalarına sahip değerine ayarlayın.

Sonraki adımda, ana bilgisayar adını gözden geçirin ve sonunda düzeltin. 

![Gözden geçirme ana bilgisayar adı](./media/hana-installation/image24_review_host_name.PNG)

Sonraki adımda, ayrıca, HANA büyük örneği dağıtım sıralı Microsoft'a verdiğiniz veri gerekir. 

![Sistem kullanıcısı sağlamak fazla](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Sipariş birim dağıtım Microsoft sağladığınız olarak aynı sistem kullanıcı kimliği ve kullanıcı grubu kimliği'nı sağlamanız gerekir. Çok aynı kimlikleri vermek başarısız olursa, HANA büyük örneği birim üzerinde SAP HANA yüklemesi başarısız olur.

Biz, bu belgede, SAP HANA veritabanı sapadm kullanıcının parolasını ve Sistem kullanıcısı için parola sağlamanız gereken görünmüyorsa, sonraki iki ekrana içinde SAP HANA datab bir parçası olarak yüklenen SAP konak aracısı için kullanılır ase örneği.

Parola tanımladıktan sonra bir onay ekranı başlanan bir uyarıdır. tüm veriler listelenir ve yükleme işlemine devam denetleyin. Yükleme ilerleme durumu gibi bir aşağıdaki belgeler bir ilerleme durumu ekranı ulaşın

![Yükleme ilerleme durumunu denetleme](./media/hana-installation/image27_show_progress.PNG)

Yükleme tamamlandıktan gibi aşağıdakine benzer bir resmi gerekir

![Yükleme tamamlandı](./media/hana-installation/image28_install_finished.PNG)

Bu noktada, SAP HANA örneği kullanım için hazır ve çalışır ve hazır olmalıdır. SAP HANA Studio'dan bağlanabilir olması gerekir. Ayrıca SAP HANA'ın en son düzeltme eklerinin denetleyin ve bu düzeltme ekleri uygulama emin olun.
























































 







 




