---
title: SAP HANA (büyük örnekler) Azure üzerinde SAP HANA yükleyin | Microsoft Docs
description: Nasıl bir SAP HANA (büyük örnekler) Azure üzerinde SAP HANA yükleyin.
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
ms.date: 03/05/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 96acb2e7af797f2777cc751417f50eb21faa46da
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60202964"
---
# <a name="how-to-install-and-configure-sap-hana-large-instances-on-azure"></a>Yükleme ve Azure üzerinde SAP HANA (büyük örnekler) yapılandırın

Bu makalede okumadan önce hakkında bilgi edinin [HANA büyük örnekleri genel koşulları](hana-know-terms.md) ve [HANA büyük örnekleri SKU'ları](hana-available-skus.md).

SAP HANA yüklemesi sizin sorumluluğunuzdur. Azure sanal ağlarınıza ve HANA büyük örneği birimi arasında bağlantı kurduktan sonra yeni bir SAP HANA (büyük örnekler) Azure sunucusuna yüklüyorsanız başlayabilirsiniz. 

> [!Note]
> Her ilke SAP, SAP HANA yüklemesi kişi kim geçti sınavı teknoloji ilişkilendirmek SAP sertifikalı, SAP HANA yüklemesi sertifika sınavı ya da bir SAP Sertifikalı Sistem entegratörü (sı) kim tarafından gerçekleştirilmesi gerekir.

HANA 2.0 yükleme planlıyorsanız, bkz. [SAP destek Not #2235581 - SAP HANA: Desteklenen işletim sistemleri](https://launchpad.support.sap.com/#/notes/2235581/E) ile işletim Sisteminin desteklendiğinden emin olmak için SAP HANA sürüm, yüklemekte olduğunuz. Desteklenen işletim sistemi HANA 2.0 için desteklenen işletim sistemi HANA 1.0 için daha kısıtlayıcı. 

> [!IMPORTANT] 
> SLES 12 SP2 işletim sistemi sürümü desteklenmiyor Type II birimleri için şu anda. 

HANA yüklemeye başlamadan önce aşağıdakileri doğrulamalıdır:
- [HLI birimi](#validate-the-hana-large-instance-units)
- [İşletim sistemi yapılandırması](#operating-system)
- [Ağ yapılandırması](#networking)
- [Depolama yapılandırması](#storage)


## <a name="validate-the-hana-large-instance-units"></a>HANA büyük örneği birim doğrula

Microsoft'un sunduğu HANA büyük örneği birim aldıktan sonra aşağıdaki ayarları doğrulayın ve gerektiği gibi ayarlayın.

**İlk adım** örneğinin işletim sistemi, işletim sistemi sağlayıcısı ile kaydolmak üzere, HANA büyük örneği alır ve erişimi ve örneklerine bağlantı kurmak sonra olur. Bu adım, azure'da bir sanal makinede dağıtılan SUSE SMT örneği, SUSE Linux işletim sistemi kaydediliyor içerir. 

HANA büyük örneği birim bu SMT örneğe bağlanabilir. (Daha fazla bilgi için [SMT sunucusu kurmak için SUSE Linux nasıl](hana-setup-smt.md)). Alternatif olarak, Red Hat işletim sisteminizi Red Hat abonelik bağlanmak için gereken Yöneticisi ile kayıtlı olması gerekir. Daha fazla bilgi için konusundaki yorumlara bakın [SAP HANA (büyük örnekler) azure'da nedir?](https://docs.microsoft.com/azure/virtual-machines/linux/sap-hana-overview-architecture?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Bu adım, müşterinin sorumluluğundadır işletim sistemi düzeltme eki uygulama için gereklidir. SUSE için yükleme ve yapılandırma bu sayfada hakkında SMT belgelere [SMT yükleme](https://www.suse.com/documentation/sles-12/book_smt/data/smt_installation.html).

**İkinci adım** yeni yamaları ve düzeltmeler belirli işletim sistemi sürümünün sürüm/denetlemek için. HANA büyük örneği düzeltme eki düzeyini en son durumunda olduğundan emin olun. En son düzeltme eklerinin burada eklenmeyen durumlar olabilir. Bir HANA büyük örneği birimi üzerinde aldıktan sonra düzeltme eki uygulanması gerekli olup olmadığını denetlemek için zorunludur.

**Üçüncü adım** yükleme ve SAP HANA belirli işletim sistemi sürüm/sürümünün yapılandırma ilgili SAP notları kullanıma sağlamaktır. Öneriler veya değişiklik notları veya tek tek yükleme senaryoları bağımlı yapılandırmaları SAP değiştirme nedeniyle Microsoft her zaman bir HANA büyük örneği birimi mükemmel bir şekilde yapılandırmak mümkün olmayacaktır. 

Bu nedenle, bu SAP notları okumak için bir müşteri için SAP HANA tam Linux sürümünüz için ilgili olarak sizin için zorunludur. Ayrıca işletim sistemi sürümünün sürüm/yapılandırmalarını kontrol edin ve henüz yapmadıysanız, yapılandırma ayarlarını uygulayabilirsiniz.

Özellikle, aşağıdaki parametreleri denetleyin ve sonunda ayarla:

- net.core.rmem_max = 16777216
- net.core.wmem_max = 16777216
- net.core.rmem_default = 16777216
- NET.Core.wmem_default 16777216 =
- net.core.optmem_max = 16777216
- net.ipv4.tcp_rmem = 65536 16777216 16777216
- NET.ipv4.tcp_wmem 65536 16777216 16777216 =

RHEL 7.2 SLES12 SP1 ile başlayarak, bu parametreleri /etc/sysctl.d dizinindeki bir yapılandırma dosyasında ayarlanmalıdır. Örneğin, bir yapılandırma dosyası 91-NetApp-HANA.conf adı ile oluşturulmalıdır. Eski SLES ve RHEL sürümleri için bu parametreler kümesi in/etc/sysctl.conf olması gerekir.

RHEL 6.3 ile başlayan tüm RHEL yayınlar için aşağıdakileri göz önünde bulundurun: 
- Sunrpc.tcp_slot_table_entries = 128 in/etc/modprobe.d/sunrpc-local.conf parametresini ayarlayın. Dosya mevcut değilse, önce aşağıdaki giriş ekleyerek oluşturmanız gerekir: 
    - options sunrpc tcp_max_slot_table_entries=128

**Dördüncü adım** HANA büyük örneği birim sistem saati denetlemektir. Örnek, bir sistem saat dilimi ile dağıtılır. Bu saat dilimini, HANA büyük örneği damgasında bulunduğu Azure bölgesinin konumunu temsil eder. Sistem saati veya size ait örnekler saat dilimini değiştirebilirsiniz. 

Daha fazla örnek kiracınızda oturum sipariş, yeni teslim edilen örnekler saat dilimini uyarlamak gerekir. Microsoft, sonra devreden MultiPath örnekleriyle ayarladığınız hiçbir öngörü sistem saat dilimi sahiptir. Bu nedenle, yeni dağıtılan örnekleri için değiştirilmiş bir aynı saat diliminde ayarlanmamış olabilir. Sahip olarak gerekirse, üzerinden gönderilen örnek saat dilimini uyarlamak için müşteri sizin sorumluluğunuzdadır. 

**Beşinci adım** etc/hosts denetlemektir. Dikey pencereleri devredildiği gibi farklı amaçlar için atanmış farklı IP adreslerini sahiptirler. Etc/hosts dosyasını kontrol edin. Birimler mevcut bir kiracınız eklendiğinde, vb./konak sistemlerini, daha önce sunulan IP adresleriyle doğru tutulan yeni dağıtılan sistemler, sahip olmayı beklediğiniz yok. Yeni dağıtılan örnek etkileşim ve kiracınıza daha önce dağıttığınız birimlerinin adlarını çözmek için müşteri xenapp'i gibi sizin sorumluluğunuzdur. 


## <a name="operating-system"></a>İşletim sistemi

> [!IMPORTANT] 
> Type II birimleri için yalnızca SLES 12 SP2 işletim sistemi sürümü şu anda desteklenmiyor. 

Teslim edilen işletim sistemi görüntüsünün takas alanı 2 GB göre ayarlanır [SAP destek Not #1999997 - SSS: SAP HANA bellek](https://launchpad.support.sap.com/#/notes/1999997/E). Farklı ayar istiyorsanız, bir müşteri olarak, kendiniz ayarlamanız gerekir.

[SUSE Linux Enterprise Server 12 SP1 SAP uygulamaları için](https://www.suse.com/products/sles-for-sap/download/) SAP hana (büyük örnekler) Azure üzerinde yüklü bir Linux dağıtımıdır. Bu belirli dağıtım SAP özgü özellikleri sağlayan "kullanıma hazır (SAP SLES üzerinde etkili bir şekilde çalıştırmak için önceden ayarlanmış parametreleri dahil)".

Bkz: [Kaynak Kitaplığı/tanıtım yazıları](https://www.suse.com/products/sles-for-sap/resource-library#white-papers) SUSE Web sitesinde ve [SUSE üzerinde SAP](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE) SAP topluluk ağ (SCN) üzerinde SAP HANA (yüksek Kurulum dahil olmak üzere SLES dağıtımıyla ilgili çeşitli yararlı kaynaklar için kullanılabilirlik, SAP işlemler ve daha fazla bilgi için belirli güvenlik sağlamlaştırma).

Ek ve kullanışlı SAP SUSE ilgili bağlantıları aşağıda verilmiştir:

- [SUSE Linux site üzerinde SAP HANA](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+SUSE)
- [SAP için en iyi uygulamalar: Sıraya alma çoğaltma – SAP NetWeaver'ı SUSE Linux Enterprise 12](https://www.suse.com/docrepcontent/container.jsp?containerId=9113)
- [ClamSAP – SAP için SLES virüs koruması](https://scn.sap.com/community/linux/blog/2014/04/14/clamsap--suse-linux-enterprise-server-integrates-virus-protection-for-sap) (SAP uygulamaları için SLES 12 dahil)

SAP HANA SLES 12 uygulama için geçerli olan SAP destek notları şunlardır:

- [SAP destek Not #1944799 – SLES işletim sistemi yüklemesi için SAP HANA Kılavuzu](https://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
- [İşletim sistemi için SLES 12 SAP uygulamaları için önerilen ayarları SAP destek Not #2205917 – SAP HANA veritabanı](https://launchpad.support.sap.com/#/notes/2205917/E)
- [SAP destek Not #1984787 – SUSE Linux Enterprise Server 12: yükleme notları](https://launchpad.support.sap.com/#/notes/1984787)
- [Destek Not #171356 – SAP yazılım Linux üzerinde SAP:  Genel bilgiler](https://launchpad.support.sap.com/#/notes/1984787)
- [SAP destek Not #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070)

[SAP HANA için Red Hat Enterprise Linux](https://www.redhat.com/en/resources/red-hat-enterprise-linux-sap-hana) HANA büyük örnekler üzerinde SAP HANA çalıştırmayı için başka bir tekliftir. RHEL 6.7 ve 7.2 sürümlerinde kullanılabilir. HANA büyük örnekleri RHEL 6.7 de destekler, burada yalnızca RHEL 7.2 ve daha yeni sürümlerde desteklenir, yerel Azure Vm'leri ters yönde dikkat edin. Ancak, bir RHEL 7.x sürümü kullanmanızı öneririz.

Red Hat ilgili bağlantılar üzerinde ek yararlı SAP aşağıda verilmiştir:
- [Red Hat Linux site üzerinde SAP HANA](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+on+Red+Hat).

Red Hat üzerinde SAP HANA uygulama için geçerli olan SAP destek notları aşağıda verilmiştir:

- [SAP destek Not #2009879 - Red Hat Enterprise Linux (RHEL) işletim sistemi için SAP HANA Kılavuzu](https://launchpad.support.sap.com/#/notes/2009879/E)
- [SAP destek Not #2292690 - SAP HANA veritabanı: RHEL 7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/2292690)
- [SAP destek Not #2247020 - SAP HANA veritabanı: RHEL 6.7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/2247020)
- [SAP destek Not #1391070 – Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070)
- [SAP destek Not #2228351 - Linux: SAP HANA veritabanı SPS 11 düzeltme 110 (veya üzeri) RHEL 6 veya SLES 11](https://launchpad.support.sap.com/#/notes/2228351)
- [SAP destek Not #2397039 - SSS: RHEL üzerinde SAP](https://launchpad.support.sap.com/#/notes/2397039)
- [SAP destek Not #1496410 - Red Hat Enterprise Linux 6.x: Yükleme ve yükseltme](https://launchpad.support.sap.com/#/notes/1496410)
- [SAP destek Not #2002167 - Red Hat Enterprise Linux 7.x: Yükleme ve yükseltme](https://launchpad.support.sap.com/#/notes/2002167)

### <a name="time-synchronization"></a>Zaman eşitleme

SAP NetWeaver mimarisine oluşturulan SAP uygulamalarını, SAP sistemi oluşturan çeşitli bileşenler için saat farklılıklarını duyarlıdır. SAP ABAP kısa dökümleri ZDATE hata başlığı ile\_büyük\_zaman\_fark biliyor. Bu kısa dökümleri farklı sunucular veya VM'ler sistem saati çok fazla ara drifting atandıklarında olmasıdır.

(Büyük örnekler) Azure üzerinde SAP HANA için Azure'da yapmış zaman eşitleme büyük örnek Damgalar işlem birimleri için geçerli değildir. Azure, sistem saati doğru eşitlendiğinden sağlar çünkü bu eşitleme yerel Azure Vm'lerde SAP uygulamaları çalıştırmak için geçerli değildir. 

Sonuç olarak, kullanılabilir ayrı saat sunucusu HANA büyük örnekler üzerinde çalışan SAP HANA veritabanı örnekleri ve Azure Vm'lerinde çalışan SAP uygulama sunucuları tarafından ayarlamanız gerekir. Bir depolama altyapısı büyük örnek damgaları NTP sunucuları ile saatin eşitlenmiş.


## <a name="networking"></a>Ağ
Biz, Azure sanal ağlarınıza tasarlama ve HANA büyük örnekler için bu sanal ağları bağlama önerileri aşağıdaki belgelerde açıklandığı uyguladığınız varsayılmaktadır:

- [SAP HANA (büyük örnek) genel bakış ve azure'da mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- [SAP HANA (büyük örnekler) altyapısı ve Azure bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Bazı ayrıntılar tek birimlerinin ağı hakkında bahseden değer vardır. Her HANA büyük örneği birim, iki veya üç NIC bağlantı noktası için atanmış olan iki veya üç IP adresi ile birlikte gelir. Üç IP adresini, HANA genişleme yapılandırmaları ve HANA sistem çoğaltma senaryosu kullanılır. Açıklanan IP havuzu birimi NIC için atanan IP adreslerini biri olan sunucu dışında [SAP HANA (büyük örnekler) genel bakışı ve mimarisi azure'da](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).

Mimarinizi için Ethernet ayrıntıları hakkında daha fazla bilgi için bkz. [HLI desteklenen senaryoları](hana-supported-scenario.md).

## <a name="storage"></a>Depolama

Depolama alanı düzenini (büyük örnekler) Azure üzerinde SAP HANA için SAP HANA tarafından önerilen yönergeleri SAP aracılığıyla Azure Hizmet Yönetimi yapılandırılır. Bu yönergeleri bölümünde belgelendirilen [SAP HANA depolama gereksinimlerini](https://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemesi. 

Kaba boyutları farklı HANA büyük örnekleri SKU'ları ile farklı birimler belirtilmiştir [SAP HANA (büyük örnekler) genel bakışı ve mimarisi azure'da](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Depolama birimleri adlandırma kurallarını aşağıdaki tabloda listelenmiştir:

| Depolama kullanımı | Takma adı | Birim adı | 
| --- | --- | ---|
| HANA data | /hana/Data/SID/mnt0000\<m > | Depolama IP: / hana_data_SID_mnt00001_tenant_vol |
| HANA günlüğü | /hana/log/SID/mnt0000\<m > | Depolama IP: / hana_log_SID_mnt00001_tenant_vol |
| HANA günlük yedekleme | /hana/log/Backups | Depolama IP: / hana_log_backups_SID_mnt00001_tenant_vol |
| Paylaşılan HANA | /hana/Shared/SID | Storage IP:/hana_shared_SID_mnt00001_tenant_vol/shared |
| usr/sap | /usr/SAP/SID | Depolama IP: / hana_shared_SID_mnt00001_tenant_vol/usr_sap |

*SID* HANA örneği sistem kimliği 

*Kiracı* bir iç işlem bir kiracı dağıtırken numaralandırmadır.

Usr/sap HANA aynı birimde paylaşın. Bağlama terminolojiyi bağlama numarası yanı sıra, HANA örnekleri sistem Kimliğini içerir. Ölçek büyütme dağıtımlarda, mnt00001 gibi yalnızca bir bağlama yoktur. Ölçek genişletme dağıtımlarda, diğer taraftan, çalışan ve ana düğümleri sahip olduğunuz sayıda takar bakın. 

Ölçek genişletme ortamları, veri, günlük ve günlük yedekleme birimleri paylaşılan ve genişletme yapılandırma her düğüme eklenmiş. Birden çok SAP örnekleri yapılandırmalarını birimleri farklı bir dizi oluşturulur ve HANA büyük örneği birimine bağlı. Senaryonuz için depolama düzeni ayrıntıları için bkz. [HLI desteklenen senaryoları](hana-supported-scenario.md).

Bir HANA büyük örneği birimi baktığınızda, birimleri HANA/veri cömert disk birimi ile gelir ve birim HANA/log/yedekleme olduğunu unutmayın. HANA/veri çok büyük yaptık nedeni, bir müşteri olarak aynı kullanmakta olduğunuz teklif depolama anlık görüntüleri Birim disk olmasıdır. Daha fazla depolama anlık görüntüleri gerçekleştirdiğiniz, daha fazla alan atanan depolama birimlerinizi anlık görüntü tarafından kullanılır. 

HANA/log/yedekleme birimi veritabanı yedeklemeleri için birimin olması beklenmiyor. HANA işlem günlüğü yedeklemeleri için yedekleme birimi olarak kullanılacak boyutlandırılır. Daha fazla bilgi için [SAP HANA (büyük örnekler) azure'da yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Sağlanan depolama alanına ek 1 TB'lik artışlarla ek depolama kapasitesi satın alabilirsiniz. Bu ek depolama alanı yeni birimleri, HANA büyük örneği için eklenebilir.

Azure hizmet yönetimi üzerinde SAP HANA ile ekleme sırasında kullanıcı kimliği (UID) ve Grup Kimliği (GID) sidadm kullanıcı ve sapsys grubu için müşteri belirtir (örneğin: 1000,500). SAP HANA sistem yüklemesi sırasında aynı değerler kullanmanız gerekir. Bir birim birden çok HANA örneklerine dağıtmak istediğiniz çünkü birden fazla birimler (her örneği için bir küme) alın. Sonuç olarak, dağıtım sırasında aşağıdakileri tanımlamanız gerekir:

- (Sidadm ondan türetilir) farklı HANA örnekleri SID'si.
- Bellek boyutları farklı HANA örnekleri. Örnek başına bellek boyutu, her bir tek birim kümesi birimlerin boyutunu tanımlar.

Depolama sağlayıcısı önerilerine göre aşağıdaki bağlama seçeneklerini tüm bağlı birimleri için yapılandırılmış (önyükleme LUN hariç):

- NFS rw vers = 4, sabit, timeo 600 rsize = 1048576, wsize = 1048576, giriş, noatime, kilit 0 0

Bu bağlama noktaları aşağıdaki grafiklerde gösterildiği /etc/fstab içinde yapılandırılır:

![bağlanan birimlerin HANA büyük örneği birimindeki fstab](./media/hana-installation/image1_fstab.PNG)

Komut SD -h S72m HANA büyük örneği birim üzerinde çıkışını şu şekilde görünür:

![bağlanan birimlerin HANA büyük örneği birimindeki fstab](./media/hana-installation/image2_df_output.PNG)


Depolama denetleyicisi ve düğümler büyük örnek damgaları NTP sunucuları eşitlenir. SAP HANA (büyük örnekler) Azure birimler ve Azure Vm'leri bir NTP sunucusuna karşı eşitlediğinizde, hiçbir önemli zaman kayması altyapısı ve Azure ya da büyük örnek Damgalar işlem birimleri arasında olmalıdır.

SAP HANA altında kullanılan depolama alanı için en iyi duruma getirme, aşağıdaki SAP HANA yapılandırma parametrelerini ayarlayın:

- max_parallel_io_requests 128
- async_read_submit on
- üzerinde async_write_submit_active
- Tüm async_write_submit_blocks
 
SPS12 kadar SAP HANA 1.0 sürümleri için bu parametreleri SAP HANA veritabanı yüklemesi sırasında açıklanan şekilde ayarlanabilir ['lu SAP notuna #2267798 - SAP HANA veritabanı yapılandırmasını](https://launchpad.support.sap.com/#/notes/2267798).

Parametreleri SAP HANA veritabanı yükleme sonrasında hdbparam çerçevesi kullanılarak da yapılandırabilirsiniz. 

HANA büyük örnekleri kullanılan depolama alanı dosya boyutu sınırlaması vardır. [Boyut sınırlaması 16 TB'tır](https://docs.netapp.com/ontap-9/index.jsp?topic=%2Fcom.netapp.doc.dot-cm-vsmg%2FGUID-AA1419CF-50AB-41FF-A73C-C401741C847C.html) dosya başına. Farklı dosya boyutu sınırlamaları EXT3 dosya sistemleri, HANA örtük olarak HANA büyük örnekleri depolaması tarafından zorlanan depolama sınırlama farkında değil. Sonuç olarak 16 TB'lık dosya boyutu sınırını ulaşıldığında HANA'ya yeni bir veri dosyası otomatik olarak oluşturmaz. HANA 16 TB ötesinde dosya büyütme girişiminde HANA sonunda, hataları ve dizin sunucusu çöker rapor eder.

> [!IMPORTANT]
> Veri büyür ve HANA büyük Örnek Depolama 16 TB dosya boyutu sınırını aşan çalışılırken HANA önlemek için aşağıdaki parametreleri SAP HANA global.ini yapılandırma dosyasında ayarlamanız gerekir
> 
> - datavolume_striping = true
> - datavolume_striping_size_gb 15000 =
> - Ayrıca SAP bkz Not [#2400005](https://launchpad.support.sap.com/#/notes/2400005)
> - SAP Not unutmayın [#2631285](https://launchpad.support.sap.com/#/notes/2631285)


SAP HANA 2.0 ile hdbparam framework kullanım dışıdır. Sonuç olarak, SQL komutlarını kullanarak parametreler ayarlanmalıdır. Daha fazla bilgi için [#2399079 SAP notuna göz atın: HANA 2 hdbparam saydamlığından](https://launchpad.support.sap.com/#/notes/2399079).

Başvurmak [HLI desteklenen senaryoları](hana-supported-scenario.md) Mimarinizi depolama düzeni hakkında daha fazla bilgi edinmek için.


**Sonraki adımlar**

- Başvurmak [HLI HANA yüklemesi](hana-example-installation.md)










































 







 




