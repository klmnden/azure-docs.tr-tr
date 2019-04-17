---
title: "Hızlı Başlangıç: Tek örnek SAP hana Azure sanal Makineler'de el ile yükleme | Microsoft Docs"
description: Azure sanal Makineler'de Hızlı Başlangıç Kılavuzu tek örnek SAP hana el ile yükleme
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/06/2018
ms.author: hermannd
ms.openlocfilehash: 7d46e2047debe5546c6d36f245ae076cec6f73a3
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59618133"
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Hızlı Başlangıç: Tek örnek SAP hana Azure vm'lerde el ile yükleme
## <a name="introduction"></a>Giriş
Bu kılavuz, tek örnek SAP HANA Azure sanal makinelerinde (VM'ler) SAP NetWeaver 7.5 ve SAP HANA 1.0 SP12 el ile yüklediğinizde ayarlamanıza yardımcı olur. Bu kılavuzun odak noktası, Azure üzerinde SAP HANA dağıtma hakkında ' dir. SAP belgelerindeki değiştirmez. 

>[!Note]
>Bu kılavuzda, Azure Vm'lerinde SAP hana dağıtımlarını açıklanmaktadır. SAP HANA HANA büyük örnekleri dağıtma hakkında daha fazla bilgi için bkz: [Azure sanal makinelerinde (VM'ler) SAP kullanma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Önkoşullar
Bu kılavuz, böyle bir altyapı bir hizmet (Iaas) temel olarak aşina olduğunuzu varsayar:
 * Sanal makine veya sanal ağlar Azure portal veya PowerShell aracılığıyla dağıtma
 * Azure platformlar arası komut satırı seçeneği JavaScript nesne gösterimi (JSON) şablonlarını kullanma dahil olmak üzere arabirimi (CLI).

Bu kılavuz, ayrıca aşina olduğunuzu varsayar:
* SAP HANA ve SAP NetWeaver ve bunları şirket içinde yükleme.
* Yükleme ve SAP HANA ve Azure üzerinde SAP uygulama örnekleri.
* Aşağıdaki kavramlar ve yordamlar:
   * Azure sanal ağ planlama ve Azure depolama alanı kullanımı dahil olmak üzere, Azure üzerinde SAP dağıtımını planlama. Bkz: [Azure Virtual Machines'de (VM'ler) - SAP NetWeaver planlama ve Uygulama Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Dağıtım ilkeleri ve azure'da VM'ler dağıtmak için yol. Bkz: [SAP için Azure sanal makineler dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Yüksek kullanılabilirlik için azure'da Ağıranlar (kuyruğa çoğaltma sunucusu) SAP NetWeaver ASCS (ABAP SAP Central Services'in) ve SCS (SAP Central Services'in). Bkz: [Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * ASCS/SCS azure'da çoklu SID yüklenmesini yararlanarak, verimliliği geliştirmeye ilişkin ayrıntıları. Bkz: [SAP NetWeaver çoklu SID yapılandırmasını oluşturun](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Azure'da Linux tabanlı sanal makineler üzerinde çalışan SAP NetWeaver ilkeler temel. Bkz: [Microsoft Azure SUSE Linux Vm'lerde SAP NetWeaver'ı çalıştıran](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Bu kılavuz, düzgün bir şekilde Linux VM'ler için Azure depolama diskleri ekleme konusunda Linux Azure sanal makinelerini ve Ayrıntılar için belirli ayarlarını sağlar.

Üretim senaryoları için kullanılabilecek Azure VM türleri listelenen [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Üretim dışı senaryolar için çok çeşitli yerel Azure VM türleri kullanılabilir.
VM üzerinde daha fazla ayrıntı için yapılandırma ve işlemleri belge başvurun [SAP HANA altyapısı yapılandırmaları ve işlemleri Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
SAP HANA yüksek kullanılabilirlik için bkz: [Azure sanal makineler için SAP HANA yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

Bir SAP HANA örneği veya S/4HANA veya BW/4hana'yı sistem çok hızlı bir sürede dağıtılan almak arıyorsanız kullanımını dikkate almanız gereken [SAP Cloud Appliance Library](https://cal.sap.com). Örneğin, bir S/4hana'yı sistemi içinde Azure üzerinde SAP CAL aracılığıyla dağıtma hakkında belgeler bulabilirsiniz [bu kılavuzda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Tek ihtiyacınız olan bir Azure aboneliği ve SAP Cloud Appliance Library ile kayıtlı bir SAP kullanıcısı.

## <a name="additional-resources"></a>Ek kaynaklar
### <a name="sap-hana-backup"></a>SAP HANA yedeklemesi
Azure Vm'leri üzerinde SAP HANA veritabanlarını yedekleme hakkında daha fazla bilgi için bkz:
* [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [SAP HANA dosya düzeyi Azure yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [Depolama anlık görüntülerine dayalı SAP HANA yedeklemesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

### <a name="sap-cloud-appliance-library"></a>SAP Cloud Appliance Library'de
S/4HANA veya BW/4hana'yı dağıtmak için SAP Cloud Appliance Library kullanma hakkında daha fazla bilgi için bkz: [dağıtma SAP S/4HANA veya BW/4hana'yı Microsoft Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA tarafından desteklenen işletim sistemleri
SAP HANA desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz: [SAP destek Not #2235581 - SAP HANA: Desteklenen işletim sistemleri](https://launchpad.support.sap.com/#/notes/2235581/E). Azure sanal makineler yalnızca bir alt kümesini bu işletim sistemlerini destekler. Azure'da SAP HANA dağıtmak için aşağıdaki işletim sistemleri desteklenir: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

SAP HANA ve farklı Linux işletim sistemleri hakkında ek SAP belgeleri için bkz:

* [Destek Not #171356 - SAP yazılım Linux üzerinde SAP:  Genel bilgiler](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP destek Not #1944799 - SLES işletim sistemi yüklemesi için SAP HANA Kılavuzu](https://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [SAP destek Not #2205917 - SAP HANA veritabanı işletim sistemi için SLES 12 SAP uygulamaları için önerilen ayarları](https://launchpad.support.sap.com/#/notes/2205917/E)
* [SAP destek Not #1984787 - SUSE Linux Enterprise Server 12:  Yükleme notları](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP destek Not #1391070 - Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070)
* [SAP destek Not #2009879 - Red Hat Enterprise Linux (RHEL) işletim sistemi için SAP HANA Kılavuzu](https://launchpad.support.sap.com/#/notes/2009879)
* [2292690 - SAP HANA VERİTABANI: RHEL 7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/2292690/E)

### <a name="sap-monitoring-in-azure"></a>SAP Azure'da izleme
SAP Azure'da izleme hakkında daha fazla bilgi için bkz:

* [SAP notu 2191498](https://launchpad.support.sap.com/#/notes/2191498/E). Bu Not SAP "Gelişmiş izleme" ile Linux Vm'leri Azure'da ele alınmaktadır. 
* [SAP notu 1102124](https://launchpad.support.sap.com/#/notes/1102124/E). Bu Not, Linux SAPOSCOL hakkındaki bilgiler ele alınmaktadır. 
* [SAP notu 2178632](https://launchpad.support.sap.com/#/notes/2178632/E). Bu Not, Microsoft Azure üzerinde SAP için ana izleme ölçümleri açıklanır.

### <a name="azure-vm-types"></a>Azure VM türleri
Azure VM türleri ve SAP HANA ile kullanılan SAP tarafından desteklenen iş yükü senaryoları bölümünde belgelenmiştir [SAP sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

SAP NetWeaver veya S/4hana'yı uygulama katmanı tarafından SAP sertifikalı azure VM türleri belgelenir [SAP notu 1928533 - azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533/E).

>[!Note]
>SAP Linux Azure tümleştirmesi yalnızca Azure Resource Manager ve klasik dağıtım modeli için desteklenir. 

## <a name="manual-installation-of-sap-hana"></a>SAP hana el ile yükleme

> [!IMPORTANT]
> Seçtiğiniz işletim sistemi kullanmakta olduğunuz belirli VM türleri üzerinde SAP HANA için sertifikalıdır SAP olduğundan emin olun. Bu, aranabilir için VM türleri ve işletim sistemi sürümleri listesi, SAP HANA sertifikalı [SAP HANA sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). SAP HANA tam listesini almak için listelenen VM türü ayrıntılarına tıkladığınızdan emin olun, işletim sistemi sürümleri için belirli bir sanal makine türü desteklenmiyor. Bu belgedeki örnekte biz M serisi VM'ler üzerinde SAP HANA için SAP tarafından desteklenmeyen bir SLES işletim sistemi sürümü kullandığınız farkında olun.
>

Bu kılavuz, el ile Azure Vm'leri üzerinde SAP HANA iki farklı şekilde yükleme açıklanmaktadır:

* "Yükleme veritabanı örneği" adımda dağıtılmış NetWeaver yüklemesinin parçası olarak, SAP yazılım sağlama Yöneticisi (SWPM) kullanarak
* SAP HANA'yı kullanarak veritabanı lifecycle manager aracı, HDBLCM ve NetWeaver'ı yükleme

Ayrıca SWPM (SAP HANA, SAP uygulama sunucusu ve örneği ASCS) tüm bileşenleri tek bir VM ile yüklemek için bu konuda açıklandığı gibi kullanabileceğiniz [SAP HANA blog duyurusuna](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Bu seçenek, bu hızlı başlangıç kılavuzunda açıklanan değildir, ancak dikkate almalısınız sorunları aynıdır.

Yükleme başlamadan önce okumanızı öneririz "hazırlama Azure Vm'leri için SAP hana el ile yükleme" bölümünde bu kılavuzun devamında. Bunun yapılması, yalnızca bir varsayılan Azure VM yapılandırması kullanırken oluşabilecek çeşitli temel hataları önlemeye yardımcı olabilir.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>SAP SWPM kullandığınızda SAP HANA yüklemesi için temel adımlar
Bu bölümde, bir dağıtılmış SAP NetWeaver 7.5 yüklemeyi gerçekleştirmek için SAP SWPM kullandığınızda el ile tek örnek SAP HANA yüklemesi için temel adımları listelenir. Her bir adım ekran görüntüleri bu kılavuzun devamında daha ayrıntılı açıklanmıştır.

1. İki test sanal makineleri içeren bir Azure sanal ağı oluşturun.
2. Azure Resource Manager modeline göre (örneğimizde, SUSE Linux Enterprise Server (SLES) ve SLES SAP uygulamaları 12 SP1), işletim sistemleri ile iki Azure Vm'leri dağıtın.
3. İki Azure standart veya premium depolama disklerini (örneğin, 75 GB ya da 500 GB disk), uygulama sunucusu VM'sine ekleyin.
4. Premium depolama diski HANA veritabanı sunucusu VM'sine. Ayrıntılar için bu kılavuzun ilerleyen bölümlerindeki "Disk" Kurulum"bölümüne bakın.
5. Boyutu veya aktarım gereksinimlerine bağlı olarak birden çok disk ve VM içindeki işletim sistemi düzeyinde mantıksal birim yönetimi ya da cihazları birden çok yönetim aracını (MDADM) kullanarak şeritli birimler oluşturun.
6. XFS dosya sistemleri, bağlı diskleri veya mantıksal birimler oluşturun.
7. İşletim sistemi düzeyinde yeni XFS dosya sistemlerine bağlayın. Bir dosya sistemi tüm SAP yazılımı kullanın. Bir dosya sistemi /sapmnt dizin ve yedeklemeler için örneğin kullanın. SAP HANA veritabanı sunucusunda XFS dosya sistemleri /hana ve /usr/sap olarak premium depolama disklerindeki bağlayın. Bu işlem, Linux Azure VM üzerinde büyük değilse, kök dosya sistemi dolmaya önlemek gereklidir.
8. / Etc/Hosts dosyasında test Vm'leri yerel IP adreslerini girin.
9. Girin **nofail**  /etc/fstab dosyasında parametre.
10. Kullanmakta olduğunuz Linux işletim sistemi sürüm göre Linux çekirdek parametrelerini ayarlayın. Daha fazla bilgi için uygun SAP notları HANA ve bu kılavuzdaki "Çekirdek parametreleri" bölümüne bakın.
11. Takas alanı ekleyin.
12. İsteğe bağlı olarak, bir grafik Masaüstü Vm'leri testinden yükleyin. Aksi takdirde, bir uzak SAPinst yüklemesini kullanın.
13. SAP yazılımı SAP Service Marketplace ' indirin.
14. SAP ASCS örnek uygulama sunucusuna VM yükleyin.
15. Test sanal makineleri arasında /sapmnt dizin NFS kullanarak paylaşın. VM uygulama sunucusunun NFS sunucusudur.
16. HANA veritabanı sunucusunda VM SWPM kullanarak dahil olmak üzere, bir veritabanı örneğini yükleyin.
17. Birincil uygulama sunucusunda (PA'lar), VM uygulama sunucusuna yükleyin.
18. SAP Yönetim Konsolu'nu (SAP MC) başlatın. Örneğin, SAP GUI veya HANA Studio ile bağlanın.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>HDBLCM kullandığınızda SAP HANA yüklemesi için temel adımlar
Bu bölümde, bir dağıtılmış SAP NetWeaver 7.5 yüklemeyi gerçekleştirmek için SAP HDBLCM kullandığınızda el ile tek örnek SAP HANA yüklemesi için temel adımları listelenir. Her bir adım ekran görüntüleri bu kılavuz boyunca daha ayrıntılı açıklanmıştır.

1. İki test sanal makineleri içeren bir Azure sanal ağı oluşturun.
2. Azure Resource Manager modeline göre işletim sistemleri (örneğimizde, SLES ve SLES SAP uygulamaları 12 SP1) ile iki Azure sanal makine dağıtın.
3. İki Azure standart veya premium depolama disklerini (örneğin, 75 GB ya da 500 GB disk), uygulama sunucusu VM'sine ekleyin.
4. Premium depolama diski HANA veritabanı sunucusu VM'sine. Ayrıntılar için bu kılavuzun ilerleyen bölümlerindeki "Disk" Kurulum"bölümüne bakın.
5. Boyutu veya aktarım gereksinimlerine bağlı olarak birden çok disk ve VM içindeki işletim sistemi düzeyinde mantıksal birim yönetimi ya da cihazları birden çok yönetim aracını (MDADM) kullanarak Bölüştürülmüş birimler oluşturun.
6. XFS dosya sistemleri, bağlı diskleri veya mantıksal birimler oluşturun.
7. İşletim sistemi düzeyinde yeni XFS dosya sistemlerine bağlayın. Bir dosya sistemi tüm SAP yazılımı kullanın ve örneğin /sapmnt dizin ve yedeklemeler için bir tane kullanın. SAP HANA veritabanı sunucusunda XFS dosya sistemleri /hana ve /usr/sap olarak premium depolama disklerindeki bağlayın. Bu işlem, Linux Azure VM üzerinde büyük değilse, kök dosya sistemi dolmaya önlemek amacıyla gereklidir.
8. / Etc/Hosts dosyasında test Vm'leri yerel IP adreslerini girin.
9. Girin **nofail**  /etc/fstab dosyasında parametre.
10. Kullanmakta olduğunuz Linux işletim sistemi sürüm göre çekirdek parametrelerini ayarlayın. Daha fazla bilgi için uygun SAP notları HANA ve bu kılavuzdaki "Çekirdek parametreleri" bölümüne bakın.
11. Takas alanı ekleyin.
12. İsteğe bağlı olarak, bir grafik Masaüstü Vm'leri testinden yükleyin. Aksi takdirde, bir uzak SAPinst yüklemesini kullanın.
13. SAP yazılımı SAP Service Marketplace ' indirin.
14. HANA veritabanı sunucusunda VM grubu kimliği 1001 sapsys, bir grup oluşturun.
15. SAP HANA veritabanı sunucusunda VM, HANA veritabanı Lifecycle Manager'ı (HDBLCM) kullanarak yükleyin.
16. SAP ASCS örnek uygulama sunucusuna VM yükleyin.
17. Test sanal makineleri arasında /sapmnt dizin NFS kullanarak paylaşın. VM uygulama sunucusunun NFS sunucusudur.
18. HANA, HANA veritabanı sunucusunda VM SWPM kullanarak dahil olmak üzere, bir veritabanı örneğini yükleyin.
19. Birincil uygulama sunucusunda (PA'lar), VM uygulama sunucusuna yükleyin.
20. SAP MC başlatın. SAP GUI veya HANA Studio bağlanın.

## <a name="preparing-azure-vms-for-a-manual-installation-of-sap-hana"></a>Azure sanal makineleri el ile bir SAP HANA yüklemesi için hazırlama
Bu bölümde aşağıdaki konuları içerir:

* İşletim sistemi güncelleştirmeleri
* Disk Kurulumu
* Çekirdek parametreleri
* Dosya sistemleri
* / Etc/hosts dosyası
* / Etc/fstab dosyası

### <a name="os-updates"></a>İşletim sistemi güncelleştirmeleri
Linux işletim sistemi güncelleştirmeleri ve düzeltmeleri ek yazılım yüklemeden önce denetleyin. Bir düzeltme eki yükleyerek destek masasına bir çağrı önlemek mümkün olabilir.

Kullandığınızdan emin olun:
* SUSE Linux Enterprise Server SAP uygulamaları için.
* Red Hat Enterprise Linux için SAP uygulamaları veya SAP HANA için Red Hat Enterprise Linux. 

Henüz yapmadıysanız, işletim sistemi dağıtımı Linux satıcının Linux aboneliğinizi kaydedin. SUSE zaten hizmetleri içeren ve otomatik olarak kayıtlı SAP uygulamaları için işletim sistemi görüntüleri olduğuna dikkat edin.

İşte bir örnek kullanarak için SUSE Linux için kullanılabilecek bir düzeltme ekleri denetleme **zypper** komutu:

 `sudo zypper list-patches`

Sorun türünü bağlı olarak, düzeltme ekleri, kategori ve önem derecesine göre sınıflandırılır. Kategori için yaygın olarak kullanılan değerler: **güvenlik**, **önerilen**, **isteğe bağlı**, **özellik**, **belge**, veya **yast**.
Önem derecesi için yaygın olarak kullanılan değerler: **kritik**, **önemli**, **orta**, **düşük**, veya **belirtilmemiş**.

**Zypper** komutu yalnızca yüklü paketlerinizdeki gerektiren güncelleştirmeler için arar. Örneğin, bu komutu kullanabilirsiniz:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Parametre ekleyebilirsiniz `--dry-run` sistem güncelleştirmeden güncelleştirme test etmek için.


### <a name="disk-setup"></a>Disk Kurulumu
Azure'da bir Linux VM kök dosya sisteminde bir boyut sınırlaması vardır. Bu nedenle, SAP çalıştırmak için bir Azure sanal makinesi için ek disk alanı eklemek gereklidir. SAP uygulama sunucusu için Azure sanal makinelerini, Azure standart depolama disklerinin yeterli olabilir. Bununla birlikte, SAP HANA DBMS Azure Vm'leri için üretim ve üretim dışı uygulamaları için Azure Premium depolama diskleri kullanılması zorunludur.

Temel [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), aşağıdaki Azure Premium depolama yapılandırması önerilir: 

| VM SKU | RAM |  / hana/veri ve/hana/günlük <br /> LVM'yi veya MDADM Şerit | / hana/paylaşılan | / root birimi | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

Önerilen disk yapılandırması, HANA veri hacmi ve günlük birimi LVM veya MDADM şeritli Azure premium depolama diskleri aynı kümesine yerleştirilir. Azure Premium depolama disk artıklık için üç görüntü tuttuğundan, herhangi bir RAID yedeklilik düzeyi tanımlamak gerekli değildir. Yeterli depolama alanı yapılandırma emin olmak için başvurun [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) ve [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Ayrıca farklı Azure premium depolama diskleri farklı sanal sabit disk (VHD) aktarım hızı birimlerini açıklandığı gibi göz önünde bulundurun [yüksek performanslı Premium depolama ve VM'ler için yönetilen diskler](../../windows/disks-types.md). 

Veritabanı veya işlem günlüğü yedeklemeleri depolamak için HANA DBMS VM'ler için daha fazla premium depolama diski ekleyebilirsiniz.

Şeritleme yapılandırmak için kullanılan iki ana araçları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Linux'ta yazılım RAID yapılandırma](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure konuk işletim sistemi olarak Linux çalıştıran Vm'leri için diskleri ekleme ile ilgili daha fazla bilgi için bkz: [bir Linux VM'ye disk ekleme](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure premium SSD disk modu önbellekleme tanımlamanıza olanak sağlar. /Hana/Data ve /hana/log şeritli kümesi için diski önbelleğe alma işlemi devre dışı bırakılmalıdır. Diğer birimleri için (diskler), önbelleğe alma modu ayarlanmalıdır **salt okunur**.

VM oluşturmak için örnek JSON şablonları bulmak için Git [Azure hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates).
Bir temel şablon vm basit sles şablonudur. Bu, bir yazılım bölümünde listelenmişse ek 100 GB veri diski içerir. Bu şablon, temel olarak kullanılabilir. Şablon, belirli bir yapılandırma için uyarlayabilirsiniz.

>[!Note]
>Açıklandığı gibi bir UUID'ye kullanarak Azure depolama diski eklemek önemlidir [Microsoft Azure SUSE Linux vm'lerde SAP NetWeaver'ı çalıştıran](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Test ortamında aşağıdaki ekran görüntüsünde gösterildiği gibi SAP uygulama sunucusu VM'sine, iki Azure standart depolama disklerinin eklenmedi. Bir disk (NetWeaver 7.5, GUI SAP ve SAP HANA gibi) tüm SAP yazılım yüklemesi için depolanır. İkinci bir disk güvence altına yeterli boş alan için aynı SAP ortamı ait tüm VM'ler arasında paylaşılacak /sapmnt dizini (diğer bir deyişle, SAP profilleri) ve ek gereksinimler (örneğin, yedekleme ve test verileri) için kullanılabilir hale gelir.

![SAP HANA uygulama sunucu diskleri penceresinde iki veri diski ve boyutlarının görüntüleme](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Çekirdek parametreleri
SAP HANA, standart Azure galeri görüntüleri parçası değildir ve el ile ayarlamanız gerekir belirli Linux çekirdek ayarlarını gerektirir. SUSE veya Red Hat kullanmadığınıza bağlı olarak, parametrelerin farklı olabilir. Daha önce listelenen SAP notları bu parametreler hakkında bilgi verin. Ekran görüntülerinde gösterilen SUSE Linux 12 SP1 kullanıldı. 

SLES SAP uygulamaları 12 genel kullanım için ve SLES SAP uygulamaları 12 SP1 için yeni bir aracı yüklü **ayarlanmış adm**, bu eski yerini **sapconf** aracı. Özel bir SAP HANA profili için kullanılabilir **ayarlanmış adm**. SAP HANA için sistem ayarlamak için bir kök kullanıcı olarak aşağıdakileri girin:

   `tuned-adm profile sap-hana`

Hakkında daha fazla bilgi için **ayarlanmış adm**, bkz: [ayarlanmış adm SUSE belgelerine](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

Aşağıdaki ekran görüntüsünde görebilirsiniz nasıl **ayarlanmış adm** değiştirilen `transparent_hugepage` ve `numa_balancing` gerekli SAP HANA ayarlara göre bir değer.

![Gerekli SAP HANA ayarlara göre değerler ayarlanmış adm aracı değiştirir](./media/hana-get-started/image005.jpg)

SAP HANA çekirdek ayarlarını kalıcı yapma, **grub2** SLES 12 üzerinde. Hakkında daha fazla bilgi için **grub2**Git [yapılandırma dosya yapısı](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) SUSE belgelerin bölümü.

Aşağıdaki ekran görüntüsüne nasıl çekirdek ayarlarını yapılandırma dosyasında değiştirildi ve ardından kullanılarak derlenmiş gösterir **grub2 mkconfig**:

![Yapılandırma dosyasında değiştirilebilir ve grub2 mkconfig kullanılarak derlenmiş çekirdek ayarları](./media/hana-get-started/image006.jpg)

YaST kullanarak ayarları değiştirmek için başka bir seçenektir ve **önyükleyici** > **çekirdek parametreleri** ayarları:

![Çekirdek parametreleri ayarlar sekmesinde YaST önyükleme yükleyicisi](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Dosya sistemleri
Aşağıdaki ekran görüntüsünde, iki bağlı Azure standart depolama diskleri üzerinde SAP uygulama sunucusu VM üzerinde oluşturulan iki dosya sistemleri gösterir. Her iki dosya sistemleri XFS türüdür ve /sapdata ve /sapsoftware bağlanır.

Bu şekilde, dosya sistemleri yapısı için zorunlu değildir. Disk alanı yapılandırma için diğer seçeneğiniz vardır. Kök dosya sistemi boş alan çalışmasını önlemek için en önemli husustur bakın.

![SAP uygulama sunucusu VM üzerinde oluşturulmuş iki dosya sistemleri](./media/hana-get-started/image008.jpg)

SAPinst (SWPM) kullandığınızda, SAP HANA DB VM veritabanı yüklemesi sırasında ilgili ve **tipik** yükleme seçeneği, her şeyi /hana ve /usr/sap altında yüklenir. Altında /usr/sap SAP HANA günlük yedekleme için varsayılan konumdur. Yeniden emin olun, kök dosya sistemi depolama alanınızın bitmesi önlemek önemlidir çünkü yeterli boş alan olduğundan /hana ve /usr/sap altında SWPM kullanarak SAP HANA yüklemeden önce.

SAP hana standart dosya sistemi Düzen açıklaması için bkz: [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![SAP uygulama sunucusu VM üzerinde oluşturulan ek dosya sistemleri](./media/hana-get-started/image009.jpg)

Standart bir SLES/SLES SAP uygulamaları 12 Azure galeri görüntüsü için SAP NetWeaver'ı yüklediğinizde, aşağıdaki ekran görüntüsünde gösterildiği gibi hiçbir takas alanı belirten bir ileti görüntülenir. Bu iletiyi kapatmak için el ile takas dosyası kullanarak ekleyebilirsiniz **GG**, **mkswap**, ve **swapon**. Bilgi edinmek için nasıl arama "dosyayı el ile ekleme" [YaST Bölümleyici kullanarak](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) SUSE belgelerin bölümü.

Linux VM Aracısı'nı kullanarak takas alanı yapılandırma başka bir seçenektir. Daha fazla bilgi için [Azure Linux Aracısı Kullanım Kılavuzu](../../extensions/agent-linux.md).

![Yetersiz takas alanı olduğunu bildiren bir açılır ileti](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>/ Etc/hosts dosyası
SAP yüklemeye başlamadan önce ana bilgisayar adları ve IP adresleri SAP sanal makinelerin/Etc/Hosts dosyasında eklediğinizden emin olun. Bir Azure sanal ağ içindeki tüm SAP sanal makineleri dağıtmak ve iç IP adresleri, burada gösterildiği gibi kullanın:

![Ana bilgisayar adları ve IP adresleri/Etc/Hosts dosyasında listelenen SAP VM](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>/ Etc/fstab dosyası

Eklemek yararlıdır **nofail** fstab dosyasını parametresi. Bir diskle sorun yaşanırsa bu şekilde, VM'yi önyükleme işleminin yanıt durdurmaz. Ancak ek disk alanı kullanılabilir olmayabilir ve kök dosya sistemi işlemleri dolgu unutmayın. /Hana eksikse, SAP HANA başlatılamıyor.

![Fstab dosyaya nofail parametre ekleyin](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>SLES 12/SLES SAP uygulamaları 12 için grafik GNOME Masaüstü
Bu bölümde aşağıdaki konuları içerir:

* GNOME Masaüstü ve xrdp SLES 12/SLES üzerinde SAP uygulamaları 12 için yükleme
* Firefox SLES 12/SLES üzerinde SAP uygulamaları 12 kullanarak Java tabanlı SAP MC çalıştırma

Xterminal veya VNC (Bu kılavuzda açıklanan değil) gibi alternatifleri de kullanabilirsiniz.

### <a name="installing-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>GNOME Masaüstü ve xrdp SLES 12/SLES üzerinde SAP uygulamaları 12 için yükleme
Windows arka plan varsa, Firefox, SAPinst, GUI SAP, SAP MC veya HANA Studio çalıştırın ve bir Windows bilgisayarından Uzak Masaüstü Protokolü (RDP) üzerinden VM bağlanmak için bir grafik Desktop'ta doğrudan SAP sanal makineleri kolayca kullanabilirsiniz. Üretim ve üretim dışı Linux için grafik kullanıcı arabirimleri ekleme hakkında daha fazla şirket ilkelerinizi bağlıdır tabanlı sistemler, sunucunuzda GNOME yüklemek isteyebilirsiniz. GNOME Masaüstü SAP uygulamaları 12 VM için bir Azure SLES 12/SLES yüklemek için:

1. GNOME Masaüstü (örneğin, bir pencerede PuTTY) aşağıdaki komutu girerek yükleyin:

   `zypper in -t pattern gnome-basic`

2. Bir VM ile RDP bağlantılarına izin vermek için xrdp yükleyin:

   `zypper in xrdp`

3. /Etc/sysconfig/windowmanager düzenleyin ve GNOME için varsayılan Pencere Yöneticisi'ni ayarlayın:

   `DEFAULT_WM="gnome"`

4. Çalıştırma **chkconfig** bu xrdp yeniden başlatmanın ardından otomatik olarak başlar emin olmak için:

   `chkconfig -level 3 xrdp on`

5. RDP bağlantısıyla ilgili bir sorun varsa, yeniden başlatmayı deneyin (penceresinden PuTTY, örneğin):

   `/etc/xrdp/xrdp.sh restart`

6. Önceki adımda bahsedilen bir xrdp yeniden işe yaramazsa, için .pid dosyasını denetleyin:

   `check /var/run` 

   Aranacak `xrdp.pid`. Bulursanız, kaldırın ve yeniden deneyin.

### <a name="starting-sap-mc"></a>SAP MC başlatılıyor
GNOME Masaüstü yükledikten sonra bir listelenen içinde Azure SLES 12/SLES SAP uygulamaları 12 VM için çalıştırılırken grafik Java tabanlı SAP MC Firefox başlayarak eksik Java tarayıcı nedeniyle eklenti hata görüntülenebilir.

SAP MC başlatmak için URL `<server>:5<instance_number>13`.

Daha fazla bilgi için [SAP Yönetim Web tabanlı konsolunda başlangıç](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

Java-tarayıcı eklentisini eksik olduğundan görüntülenen hata iletisi aşağıdaki ekran gösterilir:

![Tarayıcı eklentisi eksik Java grafiği belirten hata iletisi](./media/hana-get-started/image013.jpg)

Sorunu çözmenin bir yolu, YaST, kullanarak aşağıdaki ekran görüntüsünde gösterildiği gibi eksik yüklemektir:

![Eksik'ı yüklemek için YaST kullanma](./media/hana-get-started/image014.jpg)

SAP Yönetim Konsolu URL'si yeniden girdiğinizde, eklenti etkinleştirmek isteyen bir ileti görüntülenir:

![Eklenti etkinleştirme isteyen bir iletişim kutusu](./media/hana-get-started/image015.jpg)

Ayrıca, eksik bir dosya ile ilgili bir hata iletisi alabilirsiniz javafx.properties. Bu Oracle Java 1.8 SAP GUI 7.4 yönelik gereksinimi ilişkilidir. (Bkz ['lu SAP notuna 2059429](https://launchpad.support.sap.com/#/notes/2059424).) IBM Java sürümü ne SLES/SLES SAP uygulamaları 12 için teslim openjdk paketi gerekli javafx.properties dosyasını içerir. Java SE 8 Oracle'dan indirip çözümüdür.

Tartışma openSUSE üzerinde openjdk ile benzer bir sorun hakkında ek bilgi için bkz. [Sapguı 7.4 Java openSUSE 42.1 için Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>SAP HANA el ile yükleme: SWPM
Bu bölümdeki ekran görüntüleri bir dizi SWPM (SAPinst) kullandığınızda, SAP NetWeaver 7.5 ve SAP HANA SP12 yükleme için temel adımları gösterir. NetWeaver 7.5 yüklemesinin bir parçası olarak, SWPM HANA veritabanı tek bir örnek olarak da yükleyebilirsiniz.

Örnek test ortamında, biz yalnızca bir Gelişmiş iş uygulaması programlama (ABAP) uygulama sunucusu yüklü. Aşağıdaki ekran görüntüsünde gösterildiği gibi kullandık **dağıtılmış sistemi** ASCS ve birincil uygulama sunucu örneğinin veritabanı sistem başka bir Azure VM olarak bir Azure VM ve SAP HANA yüklemek için seçeneği.

![ASCS ve Dağıtılmış Sistem seçeneği kullanılarak yüklenen birincil uygulama sunucu örnekleri](./media/hana-get-started/image012.jpg)

ASCS örneği VM uygulama sunucusunda yüklü olduğundan ve SAP yönetim konsolundaki "green" kümesine (aşağıdaki ekran görüntüsünde gösterilen) sonra /sapmnt dizini (SAP profil dizini dahil) SAP HANA veritabanı sunucusu VM ile paylaşılması gerekir. DB yükleme adımı, bu bilgilere erişmesi gerekir. Erişim sağlamak için en iyi yolu YaST kullanarak yapılandırılabilecek NFS kullanmaktır.

![SAP yönetim ASCS örneği gösteren konsol uygulama sunucusuna VM ve "green" için ayarlayın](./media/hana-get-started/image016.jpg)

Uygulama sunucusunda VM /sapmnt dizini NFS kullanarak paylaşılması **rw** ve **no_root_squash** seçenekleri. Varsayılanlar **ro** ve **root_squash**, hangi neden olabilir sorunları veritabanı örneğini yüklediğinizde.

![Rw ve no_root_squash seçenekleri kullanarak NFS aracılığıyla /sapmnt dizine paylaşma](./media/hana-get-started/image017b.jpg)

Sonraki ekran görüntüsünde gösterildiği gibi uygulama sunucusu VM'SİNDEN /sapmnt paylaşımı SAP HANA veritabanı sunucusunda VM kullanarak yapılandırılmalıdır **NFS İstemcisi** (ve YaST).

![NFS İstemcisi kullanılarak yapılandırılan /sapmnt paylaşımı](./media/hana-get-started/image018b.jpg)

Dağıtılmış bir NetWeaver 7.5 yüklemeyi gerçekleştirmek için (**veritabanı örneği**), aşağıdaki ekran görüntüsünde olarak gösterilen, SAP HANA veritabanı sunucusu VM'sine oturum açın ve SWPM başlatın.

![SAP HANA veritabanı sunucusu VM'sine oturum açma ve başlangıç SWPM bir veritabanı örneğini yükleme](./media/hana-get-started/image019.jpg)

Seçtikten sonra **tipik** Kurulum ve yükleme medyasını yolunu DB SID, ana bilgisayar adı, örnek numarasını ve DB sistem yöneticisi parolasını girin.

![SAP HANA veritabanı sistem yönetici oturum açma sayfası](./media/hana-get-started/image035b.jpg)

DBACOCKPIT şema için parolayı girin:

![Parola kutusu DBACOCKPIT şeması](./media/hana-get-started/image036b.jpg)

Bir soru SAPABAP1 şema parolasını girin:

![Bir soru SAPABAP1 şema parolasını girin](./media/hana-get-started/image037b.jpg)

Her görev tamamlandıktan sonra DB yükleme işleminin her aşamada yanında yeşil bir onay işareti görüntülenir. ' % S'iletisi ", yürütme... Veritabanı örneği tamamlandı"görüntülenir.

![Görev tamamlandı penceresi onay iletisi](./media/hana-get-started/image023.jpg)

Başarılı yüklemeden sonra SAP Yönetim Konsolu ayrıca veritabanı örneğiniz "green" olarak gösterebilir ve SAP HANA işlemleri (hdbindexserver, hdbcompileserver ve diğerleri) tam listesini görüntüleyin.

![SAP HANA işlemlerin listesi ile SAP Yönetim Konsolu penceresi](./media/hana-get-started/image024.jpg)

Aşağıdaki ekran görüntüsünde, parçaları SWPM HANA yükleme işlemi sırasında oluşturulan /hana/shared dizin altında dosya yapısı gösterilmektedir. Farklı bir yol belirtmek için seçeneği olduğundan, SAP HANA yüklemeden önce /hana dizini altında ek disk alanı SWPM kullanarak bağlamak önemlidir. Bu kök dosya sistemi boş alanınızın bitmesi engeller.

![HANA yükleme işlemi sırasında oluşturulan /hana/shared dizin dosya yapısı](./media/hana-get-started/image025.jpg)

Bu ekran /usr/sap dizine dosya yapısını gösterir:

![/ Usr/sap dizin dosya yapısı](./media/hana-get-started/image026.jpg)

Birincil uygulama sunucusu örneği yüklemek için Dağıtılmış ABAP yüklemesinin son adımı verilmiştir:

![Son adım olarak ABAP yükleme gösteren birincil uygulama sunucusu örneği](./media/hana-get-started/image027b.jpg)

Birincil uygulama sunucusu örneği ve SAP GUI yüklendikten sonra kullanılacak **DBA Cockpit** SAP HANA yüklemesi doğru bir şekilde tamamlandığını onaylamak için işlem:

![DBA Cockpit penceresi başarılı yüklemesini onaylamak](./media/hana-get-started/image028b.jpg)

Son adım olarak, ilk yüklemede HANA Studio SAP app server VM istediğiniz ve DB sunucusu VM üzerinde çalışan SAP HANA örneği bağlanın:

![SAP HANA Studio SAP app server VM yükleme](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>SAP HANA el ile yükleme: HDBLCM
SAP HANA SWPM kullanarak dağıtılmış yüklemesinin bir parçası yüklemenin yanı sıra, HANA tek başına ilk olarak, HDBLCM kullanarak yükleyebilirsiniz. SAP NetWeaver 7.5, örneğin daha sonra yükleyebilirsiniz. Bu bölümdeki ekran görüntüleri, bu işlemin nasıl çalıştığını gösterir.

HANA HDBLCM aracı hakkında daha fazla bilgi için bkz:

* [Göreviniz için doğru SAP HANA HDBLCM seçme](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [SAP HANA yaşam döngüsü Yönetim Araçları](https://www.tutorialspoint.com/sap_hana_administration/sap_hana_administration_lifecycle_management.htm)
* [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

İçin varsayılan grup kimliği ayarı sorunlarını önlemek için `\<HANA SID\>adm user` (HDBLCM araç tarafından oluşturulan) adlı yeni bir grup tanımlayın `sapsys` grup kimliği kullanarak `1001` SAP HANA HDBLCM aracılığıyla yüklemeden önce:

!["Yeni Grup sapsys kullanılarak tanımlanmış" Grup Kimliği 1001](./media/hana-get-started/image030.jpg)

HDBLCM ilk kez başlattığınızda, bir basit başlangıç menüsünde görüntülenir. 1 öğe seç **yeni sisteme yüklemek**aşağıdaki ekran görüntüsünde gösterildiği gibi:

!["Yeni sistemi yükle" seçeneği HDBLCM başlangıç penceresi](./media/hana-get-started/image031.jpg)

Aşağıdaki ekran görüntüsünde, daha önce seçtiğiniz anahtar seçenekleri görüntüler.

> [!IMPORTANT]
> HANA günlük ve veri birimlerini yanı sıra yükleme yolu (hana Bu örnekte, paylaşılan /) ve /usr/sap, için adlı dizin kök dosya sisteminin parçası olmamalıdır. Bu dizinler ("Disk Kurulumu" bölümünde açıklanan) sanal makineye bağlı Azure veri diskleri ait. Bu yaklaşım kök dosya sistemi alanı yetersiz çalışmasını engeller. Aşağıdaki ekran görüntüsünde, HANA sistem yöneticisinin kullanıcı kimliği olduğunu görebilir `1005` parçası `sapsys` grubu (kimliği `1001`) yüklemeden önce tanımlandı.

![Daha önce seçtiğiniz tüm anahtar SAP HANA bileşenlerin listesi](./media/hana-get-started/image032.jpg)

Denetleyebilirsiniz `\<HANA SID\>adm user` (`azdadm` aşağıdaki ekran görüntüsünde) / etc/parola dizin ayrıntıları:

![HANA \<HANA SID\>listelenen/etc/parola dizinde adm kullanıcı ayrıntıları](./media/hana-get-started/image033.jpg)

SAP HANA HDBLCM kullanarak yükledikten sonra aşağıdaki ekran görüntüsünde gösterildiği gibi SAP HANA Studio dosya yapısı görebilirsiniz. Tüm SAP NetWeaver tabloları içeren SAPABAP1 şeması henüz kullanılamaz.

![SAP HANA Studio SAP HANA dosya yapısı](./media/hana-get-started/image034.jpg)

SAP HANA yükledikten sonra SAP NetWeaver çıktıklarını yükleyebilirsiniz. Aşağıdaki ekran görüntüsünde gösterildiği gibi yükleme (önceki bölümde açıklandığı gibi) SWPM kullanarak dağıtılmış bir yükleme olarak gerçekleştirildi. SWPM kullanarak veritabanı örneğini yüklediğinizde, aynı verileri (örneğin, ana bilgisayar adı, HANA SID ve örnek numarası) HDBLCM kullanarak girin. SWPM mevcut HANA yüklemesi kullanır ve başka şemalar ekler.

![SWPM kullanılarak gerçekleştirilen bir dağıtılmış yükleme](./media/hana-get-started/image035b.jpg)

Aşağıdaki ekran görüntüsünde SWPM yükleme adımı DBACOCKPIT şeması hakkında daha fazla veri gireceğiniz gösterilmektedir:

![DBACOCKPIT şema veri girildiği SWPM yükleme adımı](./media/hana-get-started/image036b.jpg)

SAPABAP1 şeması hakkında daha fazla veri girin:

![SAPABAP1 şeması hakkında daha fazla veri girme](./media/hana-get-started/image037b.jpg)

SWPM veritabanı örneği yüklemesi tamamlandıktan sonra SAP HANA Studio SAPABAP1 şemada görebilirsiniz:

![SAP HANA Studio SAPABAP1 şeması](./media/hana-get-started/image038b.jpg)

SAP uygulama sunucusu ve SAP GUI yükleme tamamlandıktan sonra son olarak, HANA veritabanı örneği kullanarak doğrulayabilirsiniz **DBA Cockpit** işlem:

![DBA Cockpit işlemle doğrulandı HANA veritabanı örneği](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>SAP yazılım indirme işlemleri
Aşağıdaki ekran görüntülerinde gösterildiği gibi SAP Service Marketplace ' yazılım indirebilirsiniz.

NetWeaver 7.5, Linux/HANA yükleyin:

 ![NetWeaver 7.5 indirme için SAP Service yükleme ve yükseltme penceresi](./media/hana-get-started/image001.jpg)

HANA SP12 Platform Edition'ı yükleyin:

 ![HANA SP12 Platform sürümü karşıdan yüklemek için SAP Service yükleme ve yükseltme penceresi](./media/hana-get-started/image002.jpg)

