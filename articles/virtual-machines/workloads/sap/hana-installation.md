---
title: "SAP HANA SAP HANA azure'da (büyük örnekler) yükleyin. | Microsoft Docs"
description: "SAP HANA SAP HANA azure'da (büyük örneği) yüklemek nasıl."
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 280001f9057825b9dcd98c5180340a54e2e239cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırma

Aşağıda, bu kılavuzu okumadan önce bilmeniz bazı önemli tanımları verilmiştir. İçinde [SAP HANA (büyük örnekler) genel bakış ve Azure ile ilgili mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) ile HANA büyük örneği birimlerin farklı iki sınıf gösterdiğimizi:

- S72, S72m, S144, S144m, S192 ve adlandırdığımız 'ı sınıf türü olarak' S192m SKU.
- S384, S384m, S384xm, S576, S768 ve adlandırdığımız 'Type II sınıfı' SKU'ları S960.

Sınıfı tanımlayıcısı HANA büyük örnek belge boyunca sonunda farklı özellikleri ve gereksinimleri HANA büyük örneği SKU'larında dayalı başvurmak için kullanılacak.

Sık kullandığınız diğer tanımların şunlardır:
- **Büyük örneği damga:** SAP HANA TDI olan bir donanım altyapı yığını sertifikalı ve Azure içindeki SAP HANA örnekleri çalıştırmak için ayrılmış.
- **SAP HANA azure'da (büyük örnekler):** SAP HANA büyük örneği Damgalar farklı Azure bölgelerinde dağıtılmış donanım sertifikalı TDI üzerinde HANA çalıştırmak Azure teklifin resmi adı örnekler içinde. İlgili dönem **HANA büyük örneği** azure'da (büyük örnekler) kısaltması SAP HANA ve yaygın olarak kullanılan bu teknik dağıtım kılavuzu.


SAP HANA yükleme sizin sorumluluğunuzdadır ve Azure (büyük örnekler) sunucuda yeni bir SAP HANA Aktarmadan sonra etkinlik başlatabilirsiniz. Ve Azure VNet(s) ve HANA arasındaki bağlantıyı sonra büyük örneği birimlerinin kurulmuş. 

> [!Note]
> SAP ilke SAP HANA yüklemesini SAP HANA yüklemelerini gerçekleştirmek için sertifikalı bir kişi tarafından gerçekleştirilmesi gerekir. Geçmiş sertifikalı SAP teknolojisi ilişkilendirmek incelemesi, SAP HANA yükleme sertifika incelemesi, bir kişi, veya bir SAP Sertifikalı Sistem Tümleştirici (SI).

Yeniden, özellikle HANA 2.0 yüklemek planlama yaparken denetlemek [SAP destek Not #2235581 - SAP HANA: desteklenen işletim sistemleri](https://launchpad.support.sap.com/#/notes/2235581/E) ile işletim Sisteminin desteklendiğinden emin olmak için SAP HANA sürüm, yüklemeye karar. Desteklenen işletim Sistemleri için HANA 2.0 HANA 1.0 için desteklenen işletim sistemi daha kısıtlı olduğunu unutmayın. 

## <a name="first-steps-after-receiving-the-hana-large-instance-units"></a>HANA büyük örneği birimlerinin aldıktan sonra ilk adımlar

**İlk adım** HANA büyük örnek alma ve kurulan erişim ve bağlantı örneklerine sahip sonra işletim sistemi örneği OS sağlayıcınız ile kaydetmek için sağlanır. Bu adım, Azure VM'deki dağıtmış olan gerek SUSE SMT örneğindeki SUSE Linux OS kaydetme dahildir. HANA büyük örneği birim bu SMT örneği (Bu belgede daha sonra bakın) bağlanabilir. Veya, RedHat OS Red Hat abonelik bağlanmak için gereken Yöneticisi ile kayıtlı olması gerekir. Bkz: de açıklamalar bu [belge](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bu adım ayrıca OS düzeltme yapabilmek gereklidir. Sorumluluğundadır müşteri bir görev. SUSE için yükleme ve yapılandırma SMT belgeleri bulmak [burada](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**İkinci adım** yeni düzeltme ekleri ve belirli işletim sistemi sürüm/sürümü düzeltmelerini denetlemek için. Düzeltme eki düzeyine HANA büyük örneğinin son durumuna olup olmadığını denetleyin. İşletim sistemi düzeltme eki/sürümleri ve Microsoft dağıtabilirsiniz görüntü değişiklikleri zamanlamaya bağlı olarak, burada en son düzeltme eklerini dahil edilmemiş olabilir durumlar olabilir. Bu nedenle, zorunlu düzeltme ekleri güvenlik, işlevselliği, kullanılabilirlik ve performans için ilgili bu arada belirli Linux satıcı tarafından yayımlanan olup olmadığını denetleyin ve uygulanması gerekir bir HANA büyük örneği birim üzerinde aldıktan sonra bir adımdır.

**Üçüncü adım** ilgili SAP Notları'nı yükleme ve belirli işletim sistemi sürüm/sürümü SAP HANA yapılandırma kullanıma sağlamaktır. Öneriler ya da değişiklikleri değiştirme nedeniyle SAP notları veya tek tek yükleme senaryoları bağımlı yapılandırmaları için Microsoft her zaman kusursuz şekilde yapılandırılmış bir HANA büyük örneği birimi olması mümkün olmaz. Bu nedenle onu sizin için SAP notları SAP HANA tam Linux sürümünüzün ilgili okumak için bir müşteri olarak zorunludur. Ayrıca, işletim sistemi/sürümü gerekli yapılandırmalarını denetleyin ve yapılandırma ayarlarının uygulanacağı zaten yapılmadı burada.

Özel, aşağıdaki parametreleri denetleyin ve sonunda göre ayarlanmış:

- NET.Core.rmem_max 16777216 =
- NET.Core.wmem_max 16777216 =
- NET.Core.rmem_default 16777216 =
- NET.Core.wmem_default 16777216 =
- NET.Core.optmem_max 16777216 =
- NET.ipv4.tcp_rmem 65536 16777216 16777216 =
- NET.ipv4.tcp_wmem 65536 16777216 16777216 =

RHEL 7.2 SLES12 SP1 ile başlayarak, bu parametreler /etc/sysctl.d dizindeki yapılandırma dosyasında ayarlanmalıdır. Örneğin, bir yapılandırma dosyası 91-NetApp-HANA.conf adı ile oluşturulması gerekir. Eski SLES ve RHEL sürümler için bu parametre kümesi in/etc/sysctl.conf olması gerekir.

İçin tüm RHEL serbest bırakır ve SLES12 ile başlatılıyor 
- sunrpc.tcp_slot_table_entries = 128

parametre in/etc/modprobe.d/sunrpc-local.conf ayarlamanız gerekir. Dosya mevcut değilse, önce aşağıdaki girişini ekleyerek oluşturulmalıdır: 
- Seçenekler sunrpc tcp_max_slot_table_entries = 128

**Dördüncü adım** HANA büyük örneği biriminiz sistem saati denetlenmesidir. Örnekleri HANA büyük örneği damgası bulunan Azure bölgesi konumunu temsil eden bir sistem saat dilimi ile dağıtılır. Sistem saati veya sahip olduğunuz örnekleri saat dilimini değiştirmek boş. Bunun yapılması ve Kiracı daha fazla örneği sıralama, yeni teslim edilen örnekler saat dilimini uyum gerek hazırlıklı olun. Microsoft operations sistem saat dilimi örnekleriyle sonra handover ayarladığınız hiçbir Öngörüler vardır. Bu nedenle yeni dağıtılan örnekleri için değiştirilmiş bir aynı saat diliminde ayarlanmamış olabilir. Sonuç olarak, denetlemek ve gerekirse karmalayan örnekler saat dilimini uyarlamak için müşteri olarak sizin sorumluluğunuzdadır olur. 

**Beşinci adım** etc/hosts denetlenmesidir. Kanatlar karmalayan gibi farklı amaçlar için (sonraki bölüme bakın) atanmış farklı IP adreslerini sahiptirler. Etc/hosts dosyasını denetleyin. Birimler mevcut bir kiracı burada eklenen durumlarda, vb./ana bilgisayarları doğru daha önce teslim sistemleri IP adresleriyle yönetilmesini yeni dağıtılan sistemler sahip olmayı beklediğiniz yok. Bu nedenle, doğru ayarlarını denetlemek için müşteri olarak olduğu bunu, yeni dağıtılan bir örneği etkileşim ve daha önce Dağıtılmış birimler kiracınızda adlarını çözmek. 

## <a name="networking"></a>Ağ
Azure Vnet'ler tasarlanması ve bu belgelerinde açıklandığı gibi bu sanal ağlar HANA büyük örneklerine bağlanma önerileri uyguladığınız varsayılmaktadır:

- [SAP HANA (büyük örneği) genel bakış ve Azure üzerinde mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP HANA (büyük örnekler) altyapısı ve Azure ile ilgili bağlantı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Tek birimleri ağ hakkında belirtmeyi bazı ayrıntılar değer vardır. Her HANA büyük örneği birim iki veya üç NIC bağlantı noktası birimi için atanmış olan iki veya üç IP adresleri ile birlikte gelir. Üç IP adresi HANA genişleme yapılandırmaları ve HANA sistem çoğaltma senaryosuna kullanılır. Birim NIC'ye atanan IP adreslerini biri içinde açıklanan sunucu IP havuzu dışında [SAP HANA (büyük örneği) genel bakış ve Azure ile ilgili mimari](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

İki IP adresi atanmış birimleriyle dağıtımı gibi görünmelidir:

- eth0.xx Microsoft'a gönderilen sunucu IP havuzu adres aralığı dışında atanmış bir IP adresi olmalıdır. Bu IP adresi/etc/hosts işletim sisteminin içinde sürdürmek için kullanılır.
- eth1.xx NFS iletişimi için kullanılan bir IP adresi olmalıdır. Bu nedenle, bu adresleri yapmak **değil** etc/hosts örneği, örnek trafiği Kiracı içinde izin vermek üzere saklanması gerekir.

Dağıtım durumlarda HANA sistem çoğaltma veya HANA genişleme, dikey yapılandırması atanan iki IP adreslerine sahip uygun değil. Yalnızca atanan iki IP adreslerine sahip olmasına ve isteyen bu tür bir yapılandırma dağıtmanız, SAP HANA üçüncü üçüncü bir IP adresi almak için Azure Hizmet Yönetimi başvurun VLAN atanmışsa. Üç NIC noktalarına atanan üç IP adreslerine sahip olmasına HANA büyük örneği birimleri için aşağıdaki kullanım kurallar geçerlidir:

- eth0.xx Microsoft'a gönderilen sunucu IP havuzu adres aralığı dışında atanmış bir IP adresi olmalıdır. Bu nedenle bu IP adresi/etc/hosts işletim sisteminin içinde sürdürmek için kullanılacak işaretçi yok.
- eth1.xx NFS depolama iletişimi için kullanılan bir IP adresi olmalıdır. Bu nedenle bu tür adresleri etc/hosts saklanması gereken değil.
- eth2.xx etc/hosts farklı örnekleri arasında iletişim için sürdürülebilmesi için özel olarak kullanılmalıdır. Bu adresler ayrıca genişleme HANA yapılandırmalarında HANA düğümler arası yapılandırmasını kullanan IP adresleri olarak güncelleştirilmesi gereken IP adresleri olacaktır.



## <a name="storage"></a>Depolama

SAP HANA azure'da (büyük örnekler) için depolama düzeni SAP HANA kılavuz çizgileri açıklandığı gibi önerilen SAP aracılığıyla Azure Hizmet Yönetimi tarafından yapılandırılan [SAP HANA depolama gereksinimleri](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemesi. Farklı HANA büyük örnekleri SKU'ları ile farklı birimler kaba boyutlarını kısmında belgelenen [SAP HANA (büyük örneği) genel bakış ve Azure ile ilgili mimari](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Adlandırma kuralları depolama birimleri aşağıdaki tabloda listelenmiştir:

| Depolama kullanımı | Takma adı | Birim adı | 
| --- | --- | ---|
| HANA veri | /hana/Data/SID/mnt0000<m> | Depolama IP: / hana_data_SID_mnt00001_tenant_vol |
| HANA günlük | /hana/log/SID/mnt0000<m> | Depolama IP: / hana_log_SID_mnt00001_tenant_vol |
| HANA günlük yedeği | /hana/log/Backups | Depolama IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| Paylaşılan HANA | /hana/Shared/SID | Depolama IP: / hana_shared_SID_mnt00001_tenant_vol/paylaşılan |
| usr/sap | /usr/SAP/SID | Depolama IP: / hana_shared_SID_mnt00001_tenant_vol/usr_sap |

Burada SID HANA örneğinin sistem kimliği = 

Ve Kiracı işlemlerinin iç numaralandırması Kiracı dağıtırken =.

Gördüğünüz gibi paylaşılan HANA ve usr/sap aynı birimi paylaşan. Bağlama terminolojisi bağlama numarası yanı sıra HANA örneklerinin sistem Kimliğini içerir. Büyütme dağıtımlarında yalnızca var. mnt00001 gibi bir bağlama Genişleme dağıtımı'nda farklı sayıda başlatmalar görür ancak çalışan ve ana düğümlerin vardır. Genişleme ortam, veri, günlük için günlük yedekleme birimleri paylaşılan ve genişletme yapılandırma her düğüme bağlı. Birden çok SAP örneği çalıştıran yapılandırmaları için farklı bir birim kümesi oluşturulur ve HAN büyük örneği birimine bağlı.

Kağıt okuyun ve HANA büyük örneği birim arayın gibi birimleri HANA/verileri yerine geniştir disk birimi ile gelir ve toplu günlük/HANA/yedekleme sahibiz unutmayın. Neden biz HANA/veri çok büyük boyutta bir müşteri olarak sunuyoruz sizin için depolama anlık görüntüler aynı disk birimi kullanıyorsanız nedenidir. Daha fazla depolama alanı anlamına gelir gerçekleştirdiğiniz anlık görüntüler, daha fazla alan atanan depolama birimlerinizi anlık görüntülerini tarafından kullanılır. Günlük/HANA/yedekleme birim, veritabanı yedeklemeleri yerleştirmek için birimi olmasını zorlayıcı değil. HANA işlem günlüğü yedeklemeleri için yedekleme birimi olarak kullanılacak boyutlandırılır. Gelecekte depolama sürümleri self service, biz daha sık anlık görüntüleri sağlamak için bu özel birim hedeflediğini anlık görüntü. Ve seçeneği HANA büyük örneği altyapısı tarafından sağlanan olağanüstü durum kurtarma işlevi için üzerinde isterse ile daha fazla çoğaltma olağanüstü durum kurtarma sitesine sık. Ayrıntıları görmek [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma Azure ile ilgili](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 

Sağlanan depolama alanına ek olarak, 1 TB artışlarla ek depolama kapasitesi satın alabilirsiniz. Bu ek depolama alanı yeni birimleri HANA büyük örneklerine eklenebilir.

SAP HANA Azure Hizmet Yönetimi onboarding işlemi sırasında müşterinin bir kullanıcı kimliği (UID) ve Grup Kimliği (GID) için sidadm kullanıcı ve sapsys grubunu belirtir (örn: 1000,500) SAP HANA sistemi yüklemesi sırasında aynı bu değerler kullanılır gereklidir. Bir birim birden çok HANA örneklerinde dağıtmak istediğiniz gibi birden çok kümesi birimlerin (her örneği için bir küme) alın. Sonuç olarak, dağıtım sırasında tanımlamanız gerekir:

- (Sidadm dışında türetilmiş) farklı HANA örnekleri SID'si.
- Bellek boyutları farklı HANA örneği. Örnek başına bellek boyutu her tek tek birim kümesi birimlerin boyutunu tanımladığından.

Aşağıdaki bağlama seçenekleri tüm takılı birimleri için yapılandırılmış olan depolama sağlayıcısı önerilerine göre (önyükleme LUN hariç):

- NFS rw sürücüleri = 4, sabit, timeo 600, rsize = 1048576, wsize = 1048576, giriş, noatime, kilitleme 0 0

Bu bağlama noktaları aşağıdaki grafiklerde gösterilen/etc/fstab gibi yapılandırılmış:

![bağlanan birimlerin HANA büyük örneği biriminde fstab](./media/hana-installation/image1_fstab.PNG)

Komut df -h S72m HANA büyük örneği biriminde çıktısı gibi görünür:

![bağlanan birimlerin HANA büyük örneği biriminde fstab](./media/hana-installation/image2_df_output.PNG)


Depolama denetleyicisi ve büyük örneği Damgalar düğümler NTP sunuculara eşitlenir. Sizinle Azure (büyük örnekler) birimlerdeki SAP HANA ve Azure Vm'leri NTP sunucusu karşı eşitleme, olmamalıdır hiçbir önemli zaman kayması oluşmasını altyapısı ve Azure veya büyük örnek Damgalar işlem birimleri arasında.

SAP HANA altında kullanılan depolama iyileştirmek için aşağıdaki SAP HANA yapılandırma parametreleri ayarlamanız gerekir:

- max_parallel_io_requests 128
- üzerinde async_read_submit
- üzerinde async_write_submit_active
- Tüm async_write_submit_blocks
 
SPS12 kadar SAP HANA 1.0 sürümleri için bu parametreler SAP HANA veritabanına yüklemesi sırasında açıklandığı gibi ayarlanabilir [SAP Not #2267798 - SAP HANA veritabanı yapılandırması](https://launchpad.support.sap.com/#/notes/2267798)

Parametreleri SAP HANA veritabanına yüklendikten sonra hdbparam framework kullanarak da yapılandırabilirsiniz. 

SAP HANA 2.0 ile hdbparam framework kullanım dışıdır. Sonuç olarak parametreleri SQL komutlarını kullanarak ayarlamalısınız. Ayrıntılar için bkz [SAP Not #2399079: HANA 2'deki hdbparam ortadan kaldırılması](https://launchpad.support.sap.com/#/notes/2399079).


## <a name="operating-system"></a>İşletim sistemi

Teslim edilen işletim sistemi görüntüsünün takas alanı 2 GB göre ayarlanmış [SAP destek Not #1999997 - SSS: SAP HANA bellek](https://launchpad.support.sap.com/#/notes/1999997/E). İstenen tüm farklı ayarı sizin tarafınızdan bir müşteri olarak ayarlanması gerekir.

[SUSE Linux Enterprise Server 12 SP1 SAP uygulamaları için](https://www.suse.com/products/sles-for-sap/hana) SAP HANA Azure (büyük örnekler) üzerinde yüklü Linux dağıtımı. Bu belirli dağıtım SAP özgü özellikleri sağlayan &quot;kutunun dışında&quot; (SAP SLES üzerinde etkili bir şekilde çalıştırmak için önceden ayarlanmış parametreleri de dahil olmak üzere).

Bkz: [Kaynak Kitaplığı/tanıtım yazıları](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) SUSE Web sitesinde ve [SUSE üzerinde SAP](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) SAP HANA (yüksek Kurulum dahil olmak üzere SLES üzerinde dağıtımıyla ilgili çeşitli kullanışlı kaynaklar SAP topluluk Ağ'üzerinde (SCN) Kullanılabilirliği, güvenliği sağlamlaştırma SAP işlemlerine özgü ve daha fazla).

SUSE ilgili bağlantıların üzerinde ek ve kullanışlı SAP:

- [SAP HANA SUSE Linux sitesinde](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [En iyi yöntemi için SAP: Enqueue çoğaltma – SUSE Linux Enterprise 12 üzerinde SAP NetWeaver](https://www.suse.com/docrepcontent/container.jsp?containerId=9113).
- [ClamSAP – SAP SLES virüs koruması](http://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (SLES 12 SAP uygulamaları dahil).

SAP destek Notlar SAP HANA SLES 12 uygulama için geçerlidir:

- [SAP destek Not #1944799 – SLES işletim sistemi yüklemesi için SAP HANA yönergeleri](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
- [SAP destek Not #2205917 – SAP HANA DB önerilen SLES 12 SAP uygulamaları için işletim sistemi ayarlarını](https://launchpad.support.sap.com/#/notes/2205917/E).
- [SAP destek Not #1984787 – SUSE Linux Enterprise Server 12: Yükleme notları](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP destek Not #171356 – SAP yazılım Linux'ta: genel bilgiler](https://launchpad.support.sap.com/#/notes/1984787).
- [SAP destek Not #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070).

[Red Hat Enterprise Linux SAP HANA için](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) HANA büyük örneklerinde SAP HANA çalıştırmak için başka bir teklifidir. RHEL 6.7 ve 7.2 sürümlerinde kullanılabilir. 

Red Hat üzerinde ek ve kullanışlı SAP bağlantıları ile ilgili:
- [SAP HANA üzerinde Red Hat Linux Site](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

SAP destek Notlar SAP HANA Red Hat üzerinde uygulama için geçerlidir:

- [SAP destek Not #2009879 - SAP HANA yönergeleri Red Hat Enterprise Linux (RHEL) işletim sistemi için](https://launchpad.support.sap.com/#/notes/2009879/E).
- [SAP destek Not #2292690 - SAP HANA DB: RHEL 7 için önerilen işletim sistemi ayarlarını](https://launchpad.support.sap.com/#/notes/2292690).
- [SAP destek Not #2247020 - SAP HANA DB: Önerilen işletim sistemi ayarlarını RHEL 6.7](https://launchpad.support.sap.com/#/notes/2247020).
- [SAP destek Not #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070).
- [SAP destek Not #2228351 - Linux: SAP HANA veritabanına SP'ler 11 düzeltme 110 (veya üstü) RHEL 6 veya SLES 11](https://launchpad.support.sap.com/#/notes/2228351).
- [SAP destek Not #2397039 - SSS: RHEL üzerinde SAP](https://launchpad.support.sap.com/#/notes/2397039).
- [SAP destek Not #1496410 - Red Hat Enterprise Linux 6.x: yükleme ve yükseltme](https://launchpad.support.sap.com/#/notes/1496410).
- [SAP destek Not #2002167 - Red Hat Enterprise Linux 7.x: yükleme ve yükseltme](https://launchpad.support.sap.com/#/notes/2002167).

## <a name="time-synchronization"></a>Zaman eşitleme

SAP NetWeaver mimarisi üzerine kurulu SAP uygulamaları saat farklılıklarını SAP sistem oluşturan çeşitli bileşenler için duyarlıdır. SAP ABAP kısa dökümleri ZDATE hata başlıkla\_büyük\_zaman\_fark olan büyük olasılıkla alışılmış, farklı sunucular veya VM'ler sistem saati çok fazla ara drifting olduğunda bu kısa dökümleri görüldüğü şekilde.

SAP HANA azure'da (büyük örnekler), Azure içermiyor &#39; bitti zaman eşitleme için t büyük örneği Damgalar işlem birimleri için geçerlidir. Bir sistem &#39;Azure sağlar gibi bu eşitleme yerel Azure Vm'lerde SAP uygulamaları çalıştırmak için geçerli değildir; s zaman düzgün eşitlenir. Sonuç olarak, SAP tarafından kullanılabilen sunucu ayarlanmalıdır ayrı birer Azure Vm'leri ve SAP HANA çalıştıran uygulama sunucuları HANA büyük örneklerinde çalıştıran veritabanı örnekleri. Büyük örneği Damgalar depolama altyapısında NTP sunucuları ile eşitlenen saattir.

## <a name="setting-up-smt-server-for-suse-linux"></a>SUSE Linux için SMT sunucusu kurma
SAP HANA büyük örnekleri doğrudan Internet bağlantısı yok. Bu nedenle, bu tür bir birimi OS sağlayıcı ile kaydetmek için karşıdan yükle ve yamalar uygulama için kolay bir işlem değil. SUSE Linux söz konusu olduğunda, bir Azure VM SMT Server'da ayarlamak için bir çözüm olabilir. Azure VM HANA büyük örneğine bağlı bir Azure VNet içinde barındırılan gerekiyor ancak. Bu tür bir SMT sunucusuyla, HANA büyük örneği birim kaydetmek ve düzeltme eklerini indirmek. 

SUSE sağlar içeren büyük bir kılavuzun kendi [SLES 12 SP2 için Abonelik Yönetim Aracı](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Görev için HANA büyük örneği yerine getiren bir SMT sunucusu yüklemesi için daha fazla önkoşulu olarak ihtiyacınız:

- HANA büyük örneği ER devresini bağlı bir Azure VNet.
- Bir kuruluşla ilişkili bir SUSE hesabı. Kuruluş geçerli bazı SUSE aboneliğiniz gerek ise.

### <a name="installation-of-smt-server-on-azure-vm"></a>Azure VM SMT sunucusu yükleme

Bu adımda, Azure VM'deki SMT sunucusu yükleyin. Oturum açmak için ilk ölçüdür [SUSE Müşteri merkezi](https://scc.suse.com/)

Oturum açtığınız gibi kuruluşun Git kuruluş kimlik bilgilerini-->. Bu bölümde SMT sunucusu kurmak gerekli kimlik bilgilerini bulmanız gerekir.

Üçüncü adım, Azure VNet içinde bir SUSE Linux VM yüklemektir. VM dağıtmak için bir Azure SLES 12 SP2 galeri görüntüsü alın. Dağıtım işlemi, bir DNS adı tanımlayın yoktur ve bu ekran görüntüsü görüldüğü gibi statik IP adresleri kullanmayın

![VM dağıtımı için SMT sunucu](./media/hana-installation/image3_vm_deployment.png)

Dağıtılan VM daha küçük bir VM olan ve iç IP adresi 10.34.1.4 Azure VNet içinde aldı. VM adını smtserver oluştu. Yükleme sonrasında HANA büyük örneği birimlerinin bağlantıyı denetlendi. Ad çözümlemesi nasıl organize bağımlı, HANA büyük örneği birimlerinin çözümlemesi vb./konakların Azure VM yapılandırmanız gerekebilir. Bir ek disk düzeltme eklerini tutmak için kullanılacak VM ekleyin. Önyükleme diski çok küçük olabilir. Gösterilen durumda diskin aşağıdaki ekran görüntüsünde gösterildiği gibi /srv/www/htdocs bağlı. 100 GB disk yeterli olacaktır.

![VM dağıtımı için SMT sunucu](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

HANA büyük örneği birimlerinin oturum açma, / etc/hosts korumak ve ağ üzerinden SMT sunucunun çalıştırmaması gerektiği Azure VM ulaşmak olup olmadığını denetleyin.

Bu denetimi başarıyla tamamlandıktan sonra SMT sunucunun çalışması gereken Azure VM'de oturum açmak gerekir. VM oturum açmak için putty kullanıyorsanız, bu komutları dizisini bash pencerenizde yürütme gerekir:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Bu komutlar yürüttükten sonra ayarları etkinleştirmek için bash yeniden başlatın. Daha sonra YAST başlatın.

Yazılım bakımı ve smt araması YAST içinde gidin. Aşağıda gösterildiği gibi yast2 smt için otomatik olarak geçer smt seçin

![Yast SMT](./media/hana-installation/image5_smt_in_yast.PNG)


Smtserver yüklemesinde seçimini kabul edin. Bir kez yüklendikten sonra SMT Sunucu Yapılandırması'na gidin ve SUSE müşteri, daha önce aldığınız Merkezi'nden kuruluş kimlik bilgilerini girin. Ayrıca, Azure VM ana bilgisayar adı SMT sunucu URL'sini girin. Bu örnekte, https://smtserver sonraki grafikleri görüntülenen kaydedildi.

![SMT sunucusunun yapılandırması](./media/hana-installation/image6_configuration_of_smtserver1.png)

Sonraki adım olarak, bağlantı SUSE Müşteri merkezi çalışır olup olmadığını test etmeniz gerekir. Tanıtım durumda, aşağıdaki grafik gördüğünüz gibi çalışmamış.

![Test SUSE müşteri Merkezi'ne bağlanma](./media/hana-installation/image7_test_connect.png)

Bir kez SMT Kurulumu başlar, veritabanı parolasını sağlamanız gerekir. Yeni bir yükleme olduğuna göre sonraki grafikleri gösterildiği gibi bu parolayı tanımlamanız gerekir.

![Veritabanının parolası tanımlayın](./media/hana-installation/image8_define_db_passwd.PNG)

Sahip olduğunuz sonraki etkileşim bir sertifika oluşturulduğunu durumdur. Sonraki gösterildiği gibi iletişim kutusundan gidin ve. adıma geçin.

![SMT sunucu için sertifika oluşturun.](./media/hana-installation/image9_certificate_creation.PNG)

Yapılandırma sonunda 'Çalıştır eşitleme onay' adımda harcanan bazı dakika olabilir. Yükleme ve yapılandırma SMT sunucusunun sonra bağlama noktası /srv/www/htdocs altında dizin depodaki bulmalısınız/depodaki altında bazı alt dizinleri artı. 

SMT sunucunun ve bu komutları ile ilgili hizmetlerini yeniden başlatın.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

### <a name="download-of-packages-onto-smt-server"></a>Paketlerin SMT sunucuya indirme

Tüm hizmetleri yeniden başlatıldıktan sonra SMT Yast kullanarak Yönetimi'nde uygun paketlerini seçin. Paket seçimi HANA büyük örnek sunucusunun işletim sistemi görüntüsü ve SLES sürüm veya SMT server çalıştıran VM sürümü üzerinde değil bağlıdır. Seçim ekranına örneği aşağıda verilmiştir.

![Paketleri seçin](./media/hana-installation/image10_select_packages.PNG)

Paket seçimi bittiğinde, ayarladığınız SMT sunucuya select paketleri ilk kopyasını başlatmanız gerekir. Bu kopya komut smt-aşağıda gösterildiği gibi yansıtma kullanarak Kabuğu'nda tetiklenir


![Paketleri SMT sunucuya indirin](./media/hana-installation/image11_download_packages.PNG)

Yukarıda gördüğünüz gibi paketleri bağlama noktası /srv/www/htdocs altında oluşturulan dizinleri kopyalanır. Bu işlem biraz zaman alabilir. Seçtiğiniz kaç paketin bağımlı, onu bir saat veya daha fazla sürebilir.
Bu işlem bitirdiğinde, SMT istemci kurulum taşımanız gerekir. 

### <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>HANA büyük örneği birimleri SMT istemcide ayarlama

İstemci bu durumda HANA büyük örneği birimleridir. SMT sunucu kurulum komut dosyası clientSetup4SMT.sh Azure VM'de oturum kopyalanır. Üzerinden HANA büyük örneği birimine, bir komut dosyası kopyalama SMT sunucunuza bağlanmak istediğiniz. -H seçeneğiyle komut dosyasını başlatmak ve parametre olarak SMT sunucunuzun adını verin. Bu örnek smtserver.

![SMT istemcisini yapılandırma](./media/hana-installation/image12_configure_client.PNG)

Burada istemci tarafından sunucusundan sertifikayı yükleme başarılı oldu, ancak aşağıda gösterildiği gibi kaydı başarısız oldu bir senaryo olabilir.

![İstemci kaydı başarısız](./media/hana-installation/image13_registration_failed.PNG)

Kayıt başarısız olursa bu okuma [SUSE destek belge](https://www.suse.com/de-de/support/kb/doc/?id=7006024) ve orada açıklanan adımları yürütün.

> [!IMPORTANT] 
> Sunucu adı olarak tam etki alanı adı olmadan bu servis talebi smtserver VM adı sağlamanız gerekir. Yalnızca VM adı çalışır. 

Bu adımları yürütülen sonra HANA büyük örneği biriminde aşağıdaki komutu yürütün gerekir

```
SUSEConnect –cleanup
```

> [!Note] 
> Bizim testlerinde biz her zaman bu adımından sonra birkaç dakika bekleyin gerekiyordu. Hemen bir yürütme clientSetup4SMT.sh sertifika henüz geçerli olmaz iletilerle SUSE makalesinde açıklanan düzeltme ölçüleri sonra sona erdi. Genellikle 5-10 dakika bekleyen ve clientSetup4SMT.sh yürütme başarılı istemci yapılandırmasında sona erdi.

SUSE makalenin adımları temel düzeltme gerekiyordu sorunu içine çalıştırdıysanız, clientSetup4SMT.sh HANA büyük örneği biriminde yeniden yeniden başlatmanız gerekir. Şimdi, aşağıda gösterildiği gibi başarıyla tamamlanmalıdır.

![İstemci kaydı başarılı](./media/hana-installation/image14_finish_client_config.PNG)

Bu adımda, Azure VM'de yüklü SMT sunucusuna bağlanmak için HANA büyük örneği biriminin SMT istemci yapılandırılır. İşletim sistemi yamalarını HANA büyük örneklerine yüklemek veya ek paketleri yüklemek için 'zypper Yukarı' veya 'ın zypper' şimdi alabilir. SMT sunucuda önce indirilen eklerini yalnızca alabilir anlaşılır.


## <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>SAP HANA yüklemenin HANA büyük örneklerinde örneği
Bu bölümde nasıl HANA büyük örneği biriminde SAP HANA yükleneceğini gösterir. Sahibiz başlangıç durumu şuna benzer:

- SAP HANA büyük örneğini dağıtmak için tüm verileri Microsoft sağlanan.
- SAP HANA büyük örneği Microsoft'tan aldı.
- Şirket içi ağınıza bağlı bir Azure sanal oluşturuldu.
- ExpressRotue hattı HANA büyük örneklerinin aynı Azure sanal ağa bağlı.
- Bir atlama kutu olarak HANA büyük örnekleri için kullandığınız bir Azure VM yüklü.
- HANA büyük örneği biriminiz ve tersi yönde atlama kutusundan bağlanabilir emin olun.
- Tüm gerekli paketleri ve düzeltme ekleri yüklü olup olmadığını kontrol.
- SAP notlar ve HANA yükleme kullanıyorsanız ve seçim HANA sürümü işletim sisteminde desteklendiğinden emin yapılan işletim sistemi ile ilgili belgeleri okuyun sürüm.

Sonraki sıralarında gösterilen atlama kutusuna bu durumda bir Windows işletim sistemlerinde, paketleri HANA büyük örneği birimine kopyasını ve Kurulum dizisini çalıştıran VM HANA yükleme paketleri indirilmesini ' dir.

### <a name="download-of-the-sap-hana-installation-bits"></a>SAP HANA yükleme bit indirme
HANA büyük örneği birimleri doğrudan internet bağlantısı yoksa bu yana HANA büyük örneği VM'ye SAP yükleme paketleri doğrudan indiremez. Eksik doğrudan internet bağlantısı üstesinden atlama kutusunu gerekir. Atlama kutunun VM paketlerini yükleyin.

HANA yükleme paketlerini indirmek için bir SAP S-kullanıcı veya SAP Market erişmenize olanak sağlayan diğer kullanıcı gerekir. Oturum açtıktan sonra bu ekranlar sırası gidin:

Gidin [SAP hizmet Market](https://support.sap.com/en/index.html) > tıklatın indirme yazılım > yüklemeleri ve yükseltme > alfabetik dizine göre > H altında – SAP HANA Platform Edition > SAP HANA Platform sürüm 2.0 > yükleme > indirin Aşağıdaki dosyaları

![HANA yükleme indirin](./media/hana-installation/image16_download_hana.PNG)

Tanıtım durumda SAP HANA 2.0 yükleme paketleri indirilir. Azure atlama kutusunda VM, kendi kendine ayıklanan arşivler dizine aşağıda gösterildiği gibi genişletin.

![HANA yükleme Ayıkla](./media/hana-installation/image17_extract_hana.PNG)

Arşivler ayıklanır gibi ayıklama, durumda 51052030, yukarıda oluşturduğunuz bir dizine /hana/shared birim içine HANA büyük örneği birimine tarafından oluşturulan dizin kopyalayın.

> [!Important]
> Alanı sınırlıdır ve diğer işlemler tarafından kullanılmalıdır bu yana yükleme paketleri kök ya da önyükleme LUN kopyalamayın.


### <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>SAP HANA HANA büyük örneği biriminde yükleyin
SAP HANA yüklemek için kullanıcının kök oturum açmak gerekir. Yalnızca kök SAP HANA yüklemek için yeterli izinlere sahip.
Yapmanız gereken ilk şey üzerinden/hana paylaşılan / kopyaladığınız dizini izinleri ayarlamaktır. İzinler gibi ayarlamanız gerekir

```
chmod –R 744 <Installation bits folder>
```

SAP HANA grafik Kurulum kullanılarak yüklemek istiyorsanız, gtk2 paket HANA büyük örneklerinde yüklü olması gerekir. Komutuyla yüklü olup olmadığını denetleyin

```
rpm –qa | grep gtk2
```

Daha fazla adımlarda size grafik kullanıcı arabirimi ile SAP HANA Kurulum gösteren. Sonraki adım olarak yükleme dizinine gidin ve HDB_LCM_LINUX_X86_64 alt dizinine gidin. Başlatma

```
./hdblcmgui 
```
Bu dizine dışında. Şimdi, ekranlar bir dizi burada veri yükleme için sağlamanız gerekir rehberlik sağlanır. Gösterilen durumda biz SAP HANA veritabanı sunucusu ve SAP HANA istemci bileşenlerini yüklüyorsunuz. Bu nedenle bizim 'SAP HANA veritabanına' aşağıda gösterildiği gibi seçimdir

![Yüklemede HANA seçin](./media/hana-installation/image18_hana_selection.PNG)

Sonraki ekranda, 'Yeni sistemini Yükle' seçeneğini seçin

![Yeni yükleme HANA seçin](./media/hana-installation/image19_select_new.PNG)

Bu adımdan sonra SAP HANA veritabanı sunucusuna ek olarak yüklenebilen birkaç ek bileşenler arasındaki seçmeniz gerekir.

![Ek HANA bileşenleri seçin](./media/hana-installation/image20_select_components.PNG)

Bu belgede SAP HANA istemci ve SAP HANA Studio seçtik. Biz de büyütme örneğinin yüklü. Bu nedenle sonraki ekranda, 'Tek konak System' seçmeniz gerekir 

![Büyütme yükleme seçin](./media/hana-installation/image21_single_host.PNG)

Sonraki ekranda, bazı veriler sağlamanız gerekir.

![SAP HANA SID sağlayın](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> HANA sistem kimliği (SID) olarak aynı SID, size HANA büyük örnek dağıtım sıralı zaman Microsoft sağlanan sağlamanız gerekir. Farklı bir SID seçme farklı birimlerde erişim izin sorunları nedeniyle başarısız yüklemeyi yapar

Yükleme olarak dizin paylaşılan dizin/hana/kullanın. Sonraki adımda HANA veri dosyaları ve HANA günlük dosyaları için konumları sağlamanız gerekir.


![HANA günlük konumunu belirtin](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Veri tanımlama ve günlük dosyalarını zaten bu ekran önce ekran seçimdeki seçtiğiniz SID içeren bağlama noktaları ile birlikte gelen birimleri gerekir. SID yazdığınız bir uyuşmazlık varsa, önce ekranında geri dönün ve SID bağlama noktalarında sahip değerine ayarlayın.

Sonraki adımda, ana bilgisayar adını gözden geçirin ve sonunda düzeltin. 

![Gözden geçirme ana bilgisayar adı](./media/hana-installation/image24_review_host_name.PNG)

Sonraki adımda, ayrıca HANA büyük örnek dağıtım sıralı zaman Microsoft'a verdiğiniz verileri almak gerekir. 

![Sistem kullanıcısı sağlamak UID ve GID](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Microsoft, sırada birim dağıtım sağlanan yazarken aynı sistem kullanıcı kimliği ve kullanıcı grubu kimliği sağlamanız gerekir. Çok aynı kimlikleri vermek başarısız olursa, SAP HANA HANA büyük örneği biriminde yüklenmesi başarısız olur.

Biz, bu belgede, sistem kullanıcı SAP HANA veritabanına sapadm kullanıcı için parola ve parola sağlamanız gerekir görünmüyorsa, sonraki iki ekranda içinde SAP HANA datab bir parçası olarak yüklenen SAP konak Aracısı kullanıldığı Ana örneği.

Parola tanımladıktan sonra bir onay ekranı başlanan bir uyarıdır. tüm veriler listelenen ve yükleme işlemine devam denetleyin. Yükleme ilerleme durumu gibi aşağıdaki belgeler ilerleme ekranı ulaşmak

![Yükleme ilerleme durumunu denetleme](./media/hana-installation/image27_show_progress.PNG)

Yükleme bitirdiğinde, aşağıdakine benzer bir resim gerekir

![Yükleme tamamlandı](./media/hana-installation/image28_install_finished.PNG)

Bu noktada, SAP HANA örnek kullanım için hazır ve çalışır ve hazır olmalıdır. SAP HANA Studio'dan bağlanmak olması gerekir. SAP HANA en son düzeltme eki için denetleyin ve bu yamalar uygulama olduğundan emin olun.
























































 







 




