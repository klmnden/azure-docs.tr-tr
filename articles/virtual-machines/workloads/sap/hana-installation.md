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
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dad088138fea2dd4fadc0cc9eed71245c32a8e0b
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162688"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırın

Bu makalede okumadan önce hakkında bilgi edinin [HANA büyük örnekleri genel koşulları](hana-know-terms.md) ve [HANA büyük örnekleri SKU'ları](hana-available-skus.md).

SAP HANA yüklemesi sizin sorumluluğunuzdur ve yeni bir SAP hana (büyük örnekler) Azure sunucuya aktarmadan sonra etkinlik başlayabilirsiniz. Ve sonra Azure Vnet'lerinizdeki ve HANA büyük örneği birimi arasında bağlantı. 

> [!Note]
> Her ilke SAP, SAP HANA yüklemesi SAP HANA yüklemelerini gerçekleştirmek için sertifikalı bir kişi tarafından gerçekleştirilmesi gerekir. Geçti sınavı teknoloji ilişkilendirmek SAP sertifikalı, SAP HANA yüklemesi sertifika sınavı, bir kişi, veya bir SAP Sertifikalı Sistem entegratörü (sı).

Özellikle HANA 2. 0'da, yükleme planlarken yeniden denetle [SAP destek Not #2235581 - SAP HANA: desteklenen işletim sistemleri](https://launchpad.support.sap.com/#/notes/2235581/E) ile işletim Sisteminin desteklendiğinden emin olmak için SAP HANA sürüm, yüklemeye karar vermiştir. Desteklenen işletim sistemi HANA 2.0 için HANA 1.0 için desteklenen işletim sistemi daha sınırlı olduğunu unutmayın. 

> [!IMPORTANT] 
> Bu noktada Type II birimleri yalnızca SLES 12 SP2 işletim sistemi sürümü desteklenmiyor. 

HANA yüklemeye başlamadan önce aşağıdakileri doğrulamalıdır:
- [HLI birimi doğrula](#validate-the-hana-large-instance-units)
- [İşletim sistemi yapılandırması](#operating-system)
- [Ağ yapılandırması](#networking)
- [Depolama yapılandırması](#storage)


## <a name="validate-the-hana-large-instance-units"></a>HANA büyük örnek birimi doğrula

Microsoft'un sunduğu HANA büyük örneği birim aldıktan sonra aşağıdaki ayarları doğrulayın ve gerektiği gibi ayarlayın.

**İlk adım** örneğinin işletim sistemi, işletim sistemi sağlayıcısı ile kaydolmak üzere HANA büyük örnek alma sonra kurulan erişimi ve bağlantı örneklerine sahip olduğu. Bu adım, SUSE Linux işletim sistemi SUSE SMT, azure'da bir sanal makinede dağıtılması gereken bir örneğini kaydetme dahildir. Bu SMT örneğine HANA büyük örneği birim bağlayabilirsiniz (bkz [SUSE Linux için Kurulum SMT sunucuya nasıl](hana-setup-smt.md)). Ya da Red Hat işletim sisteminizi Red Hat abonelik bağlanmanız gereken Yöneticisi ile kayıtlı olması gerekir. Bkz: Ayrıca açıklamalar bu [belge](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu adım Ayrıca işletim sistemi düzeltme eki gereklidir. Müşterinin sorumluluğundadır içinde bir görev. SUSE için yükleme ve yapılandırma SMT belgelere [burada](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

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

### <a name="time-synchronization"></a>Zaman eşitleme

SAP NetWeaver bir mimari temelinde SAP uygulamaları SAP sistemi oluşturan çeşitli bileşenler için saat farklılıklarını duyarlıdır. SAP ABAP kısa dökümleri ZDATE hata başlığı ile\_büyük\_zaman\_fark olan büyük olasılıkla alışılmış, farklı sunucular veya VM'ler sistem saati çok fazla ara drifting bu kısa dökümleri görünür olarak.

(Büyük örnekler) azure'da SAP HANA için Azure eklenmemişse bitti eşitleme zaman&#39;büyük örnek Damgalar işlem biriminde t uygulanır. Azure, bir sistem sağlar gibi bu eşitleme yerel Azure Vm'lerde SAP uygulamaları çalıştırmak için geçerli değildir&#39;s zaman düzgün bir şekilde eşitlenir. Sonuç olarak, SAP tarafından kullanılabilen sunucu ayarlanmalıdır ayrı birer HANA büyük örnekler üzerinde çalışan örneklerinin Azure sanal makineleri ve SAP HANA üzerinde çalışan uygulama sunucuları veritabanı. Bir depolama altyapısı büyük örnek damgaları NTP sunucuları ile eşitlenmiş saattir.


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

İncelemeyi okuyun ve HANA büyük örneği birim ara birimleri HANA/veri yerine cömert disk birimi ile gelir ve birim HANA/log/yedekleme sahibiz farkında olun. Neden biz HANA/veri çok büyük boyutta bir müşteri olarak sunuyoruz, depolama anlık görüntüleri aynı disk birimi kullandığınızı nedenidir. Daha fazla depolama alanı geldiğini gerçekleştirdiğiniz anlık görüntüleri, daha fazla alan atanan depolama birimlerinizi anlık görüntü tarafından kullanılır. HANA/log/yedekleme birimi, veritabanı yedeklemeleri yerleştirmek için toplu olarak düşünülebilir değil. HANA işlem günlüğü yedeklemeleri için yedekleme birimi olarak kullanılacak boyutlandırılır. Ayrıntılara bakın [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve azure'da olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

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


**Sonraki adımlar**

- Başvuru [HLI HANA yüklemesi](hana-example-installation.md)










































 







 




