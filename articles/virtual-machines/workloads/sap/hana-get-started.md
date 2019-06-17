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
ms.openlocfilehash: 5091932989849943f00cb71f72378dd17af23a4a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60205050"
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-virtual-machines"></a>Hızlı Başlangıç: Tek örnek SAP hana Azure sanal Makineler'de el ile yükleme
## <a name="introduction"></a>Giriş
Bu kılavuzda bir Azure sanal Makineler'de tek örnek SAP HANA, SAP NetWeaver 7.5 ve SAP HANA 1.0 SP12 el ile yüklediğinizde ayarlamanıza yardımcı olur. Bu kılavuzun odak noktası, Azure üzerinde SAP HANA dağıtma açıktır. Bu, SAP belgelerindeki yerini almaz. 

> [!NOTE]
> Bu kılavuzda, Azure Vm'lerinde SAP hana dağıtımlarını açıklanmaktadır. SAP HANA HANA büyük örnekleri dağıtma hakkında daha fazla bilgi için bkz: [kullanım SAP Azure sanal Makineler'de](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Önkoşullar
Bu kılavuz, böyle bir altyapı bir hizmet (Iaas) temel olarak aşina olduğunuzu varsayar:
 * Sanal makine (VM) ya da sanal ağlar Azure portal veya PowerShell aracılığıyla dağıtma
 * Azure platformlar arası komut satırı JavaScript nesne gösterimi (JSON) şablonları kullanma seçeneğini içeren arabirimi (CLI).

Bu kılavuz Ayrıca, alışık olduğunuz varsayılır:
* SAP HANA ve SAP NetWeaver ve bunları şirket içinde yükleme.
* Nasıl yükleyin ve SAP HANA ve SAP uygulama örnekleri, Azure üzerinde çalışır.
* Aşağıdaki kavramlar ve yordamlar:
   * Azure sanal ağ planlama ve Azure depolama alanı kullanımı içeren Azure üzerinde SAP dağıtımını planlama. Bkz: [Azure sanal makineler üzerinde - SAP NetWeaver planlama ve Uygulama Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Dağıtım ilkeleri ve azure'da VM'ler dağıtmak için yol. Bkz: [SAP için Azure sanal makineler dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Yüksek kullanılabilirlik için azure'da kuyruğa çoğaltma sunucusuna (Ağıranlar) SAP NetWeaver ABAP SAP merkezi Hizmetleri (ascs) gelir ve SAP Central Hizmetleri (SCS). Bkz: [Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * ASCS/SCS azure'da çoklu SID yüklenmesini verimliliği geliştirmeye ilişkin ayrıntıları. Bkz: [SAP NetWeaver çoklu SID yapılandırmasını oluşturun](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Azure'da Linux tabanlı sanal makineler üzerinde çalışan SAP NetWeaver ilkeler temel. Bkz: [Microsoft Azure SUSE Linux Vm'lerde SAP NetWeaver çalıştırma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Bu kılavuz, Azure sanal makinelerinde Linux belirli ayarlarını sağlar. Ayrıca, düzgün bir şekilde Linux VM'ler için Azure depolama diskleri ekleme hakkında bilgi sağlar.

Üretim senaryoları için kullanılabilecek Azure VM türleri listelenen [IAAS için SAP belgelerindeki](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Üretim dışı senaryolar için çok çeşitli yerel Azure VM türleri kullanılabilir.
VM yapılandırması ve işlemleri hakkında daha fazla bilgi için bkz. [SAP HANA altyapısı yapılandırmaları ve işlemleri Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
SAP HANA yüksek kullanılabilirlik için bkz: [SAP HANA yüksek kullanılabilirlik için Azure sanal makineler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

SAP HANA örneği veya hızlı bir şekilde dağıtılan S/4HANA veya BW/4hana'yı sistem almak istiyorsanız, kullanmayı [SAP Cloud Appliance Library](https://cal.sap.com). Örneğin, Azure üzerinde SAP Cloud Appliance Library aracılığıyla S/4hana'yı sistem içinde dağıtma hakkında belgeleri bulabilirsiniz [bu kılavuzda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Tek ihtiyacınız olan bir Azure aboneliği ve SAP Cloud Appliance Library ile kayıtlı bir SAP kullanıcısı.

## <a name="additional-resources"></a>Ek kaynaklar
### <a name="sap-hana-backup"></a>SAP HANA yedeklemesi
Azure Vm'leri üzerinde SAP HANA veritabanlarını yedeklemek nasıl daha fazla bilgi için bkz:
* [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide).
* [SAP HANA dosya düzeyi Azure yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level).
* [SAP HANA yedeklemesi depolama anlık görüntülerine dayalı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots).

### <a name="sap-cloud-appliance-library"></a>SAP Cloud Appliance Library'de
S/4HANA veya BW/4hana'yı dağıtmak için SAP Cloud Appliance Library kullanma hakkında daha fazla bilgi için bkz: [dağıtma SAP S/4HANA veya BW/4hana'yı Microsoft Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA tarafından desteklenen işletim sistemleri
SAP HANA desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz: [SAP notu 2235581 - SAP HANA: Desteklenen işletim sistemleri](https://launchpad.support.sap.com/#/notes/2235581/E). Azure sanal makineler yalnızca bir alt kümesini bu işletim sistemlerini destekler. Azure'da SAP HANA dağıtmak için aşağıdaki işletim sistemleri desteklenir: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

SAP HANA ve farklı Linux işletim sistemleri hakkında ek SAP belgeleri için bkz:

* [SAP notu 171356: Linux'ta SAP yazılım: Genel bilgiler](https://launchpad.support.sap.com/#/notes/1984787).
* [SAP notu 1944799: SLES işletim sistemi yüklemesi için SAP HANA yönergeleri](https://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html).
* [SAP notu 2205917: SAP HANA veritabanı önerilen SAP uygulamaları için işletim sistemi ayarlarını SLES 12](https://launchpad.support.sap.com/#/notes/2205917/E).
* [SAP notu 1391070: Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070).
* [SAP notu 2009879: Red Hat Enterprise Linux (RHEL) işletim sistemi için SAP HANA yönergeleri](https://launchpad.support.sap.com/#/notes/2009879).
* [SAP notu 2292690: SAP HANA VERİTABANI: İşletim sistemi ayarları RHEL 7 için önerilen](https://launchpad.support.sap.com/#/notes/2292690/E).

### <a name="sap-monitoring-in-azure"></a>SAP Azure'da izleme
Azure'da SAP izleme hakkında daha fazla bilgi için:

* [SAP notu 2191498](https://launchpad.support.sap.com/#/notes/2191498/E) azure'da Linux VM'ler ile İzleme Gelişmiş SAP açıklanır. 
* [SAP notu 1102124](https://launchpad.support.sap.com/#/notes/1102124/E) Linux SAPOSCOL hakkındaki bilgiler ele alınmaktadır. 
* [SAP notu 2178632](https://launchpad.support.sap.com/#/notes/2178632/E) ana izleme ölçümleri, Microsoft Azure'da SAP için açıklanır.

### <a name="azure-vm-types"></a>Azure VM türleri
Azure VM türleri ve SAP HANA ile kullanılan SAP tarafından desteklenen iş yükü senaryoları bölümünde belgelenmiştir [SAP sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

SAP NetWeaver veya S/4hana'yı uygulama katmanı tarafından SAP sertifikalı azure VM türleri belgelenir [SAP notu 1928533: Azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533/E).

> [!NOTE]
> SAP Linux Azure tümleştirmesi yalnızca Azure Resource Manager ve klasik dağıtım modeli için desteklenir. 

## <a name="manual-installation-of-sap-hana"></a>SAP hana el ile yükleme

> [!IMPORTANT]
> Seçtiğiniz işletim sistemi kullandığınız belirli VM türleri üzerinde SAP HANA için sertifikalıdır SAP olduğundan emin olun. VM türleri ve işletim sistemi sürümleri için de bu VM türleri aranabilir listesi, SAP HANA sertifikalı [SAP HANA sertifikalı ve Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Belirli bir sanal makine türü için SAP HANA tarafından desteklenen işletim sistemi sürümleri tam listesini almak için listelenen VM türü detayına tıkladığınızdan emin olun. Bu belge örnek için M serisi VM'ler üzerinde SAP HANA için SAP tarafından desteklenmeyen bir SUSE Linux Enterprise Server (SLES) işletim sistemi sürüm kullandık.
>

Bu kılavuz, el ile Azure Vm'leri üzerinde SAP HANA iki farklı şekilde yükleme açıklanmaktadır:

* SAP yazılım sağlama Yöneticisi (SWPM), "yükleme veritabanı örneği" adımda dağıtılmış NetWeaver yüklemesinin parçası olarak kullanın.
* SAP HANA veritabanı yaşam döngüsü (HDBLCM) manager aracını kullanın ve NetWeaver'ı yükleyin.

SWPM, tek bir VM ile SAP HANA, SAP uygulama sunucusu ve ASCS örneği gibi tüm bileşenlerini yüklemek için de kullanabilirsiniz. Adımlar, bu konuda açıklanan [SAP HANA blog duyurusuna](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Bu seçenek, bu hızlı başlangıç kılavuzunda açıklanan değildir, ancak dikkate almanız gereken sorunlar aynıdır.

Yükleme başlamadan önce okumanızı öneririz "hazırlama Azure Vm'lerde SAP hana el ile yükleme" bölümünde bu kılavuzun devamında. Bunun yapılması, yalnızca bir varsayılan Azure VM yapılandırması kullanırken oluşabilecek çeşitli temel hataları önlemeye yardımcı olabilir.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>SAP SWPM kullandığınızda SAP HANA yüklemesi için temel adımlar
Bu bölümde, bir dağıtılmış SAP NetWeaver 7.5 yüklemeyi gerçekleştirmek için SAP SWPM kullandığınızda el ile tek örnek SAP HANA yüklemesi için temel adımları listelenir. Her bir adım ekran görüntüleri bu kılavuzun devamında daha ayrıntılı açıklanmıştır.

1. İki test sanal makineleri içeren bir Azure sanal ağı oluşturun.
2. Azure Resource Manager modeline göre işletim sistemleriyle iki Azure sanal makine dağıtın. Bu örnek, SUSE Linux Enterprise Server ve SLES SAP uygulamaları 12 SP1 için kullanır. 
3. İki Azure standart veya premium depolama diskleri, örneğin, 75 GB veya 500 GB disk, uygulama sunucusu VM'sine ekleyin.
4. Premium depolama diski HANA veritabanı sunucusu VM'sine. Daha fazla bilgi için bu kılavuzun ilerleyen bölümlerindeki "Disk" Kurulum"bölümüne bakın.
5. Boyutu veya aktarım gereksinimlerine bağlı olarak, birden fazla disk ekleyin. Ardından Bölüştürülmüş birimler oluşturun. VM içindeki işletim sistemi düzeyinde mantıksal birim yönetimi (LVM) ya da cihazları birden çok (mdadm) Yönetim Aracı'nı kullanın.
6. XFS dosya sistemleri, bağlı diskleri veya mantıksal birimler oluşturun.
7. İşletim sistemi düzeyinde yeni XFS dosya sistemlerine bağlayın. Bir dosya sistemi tüm SAP yazılımı kullanın. Bir dosya sistemi /sapmnt dizin ve yedeklemeler için örneğin kullanın. SAP HANA veritabanı sunucusunda XFS dosya sistemleri /hana ve /usr/sap olarak premium depolama disklerindeki bağlayın. Bu işlem, kök dosya sistemine dolmaya önlemek gereklidir. Kök dosya sistemine Linux Azure VM üzerinde büyük değil. 
8. / Etc/Hosts dosyasında test Vm'leri yerel IP adreslerini girin.
9. Girin **nofail**  /etc/fstab dosyasında parametre.
10. Kullandığınız Linux işletim sistemi sürüm göre Linux çekirdek parametrelerini ayarlayın. Daha fazla bilgi için SAP notları HANA ve bu kılavuzdaki "Çekirdek parametreleri" bölümüne bakın.
11. Takas alanı ekleyin.
12. İsteğe bağlı olarak, bir grafik Masaüstü Vm'leri testinden yükleyin. Aksi takdirde, bir uzak SAPinst yüklemesini kullanın.
13. SAP yazılımı SAP Service Marketplace ' indirin.
14. SAP ASCS örnek uygulama sunucusuna VM yükleyin.
15. Test sanal makineleri arasında /sapmnt dizin NFS kullanarak paylaşın. VM uygulama sunucusunun NFS sunucusudur.
16. DB sunucusunda VM SWPM'ı kullanarak, HANA içeren veritabanı örneğini yükleyin.
17. Birincil uygulama sunucusunda (PA'lar), VM uygulama sunucusuna yükleyin.
18. SAP Yönetim Konsolu'nu (SAP MC) başlatın. Örneğin, SAP GUI veya HANA Studio ile bağlanın.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>HDBLCM kullandığınızda SAP HANA yüklemesi için temel adımlar
Bu bölümde, bir dağıtılmış SAP NetWeaver 7.5 yüklemeyi gerçekleştirmek için SAP HDBLCM kullandığınızda el ile tek örnek SAP HANA yüklemesi için temel adımları listelenir. Her bir adım ekran görüntüleri bu kılavuz boyunca daha ayrıntılı açıklanmıştır.

1. İki test sanal makineleri içeren bir Azure sanal ağı oluşturun.
2. Azure Resource Manager modeline göre işletim sistemleriyle iki Azure sanal makine dağıtın. Bu örnek, SLES ve SLES SAP uygulamaları 12 SP1 için kullanır.
3. İki Azure standart veya premium depolama diskleri, örneğin, 75 GB veya 500 GB disk, uygulama sunucusu VM'sine ekleyin.
4. Premium depolama diski HANA veritabanı sunucusu VM'sine. Daha fazla bilgi için bu kılavuzun ilerleyen bölümlerindeki "Disk" Kurulum"bölümüne bakın.
5. Boyutu veya aktarım gereksinimlerine bağlı olarak, birden fazla disk ekleyin. Şeritli birimler, VM içindeki işletim sistemi düzeyinde mantıksal birim yönetimi veya mdadm aracı kullanarak oluşturursunuz.
6. XFS dosya sistemleri, bağlı diskleri veya mantıksal birimler oluşturun.
7. İşletim sistemi düzeyinde yeni XFS dosya sistemlerine bağlayın. Bir dosya sistemi tüm SAP yazılımı kullanın. Bir dosya sistemi /sapmnt dizin ve yedeklemeler için örneğin kullanın. SAP HANA veritabanı sunucusunda XFS dosya sistemleri /hana ve /usr/sap olarak premium depolama disklerindeki bağlayın. Bu işlem, kök dosya sistemine dolmaya engellemek yardımcı olmak gereklidir. Kök dosya sistemine Linux Azure VM üzerinde büyük değil.
8. / Etc/Hosts dosyasında test Vm'leri yerel IP adreslerini girin.
9. Girin **nofail**  /etc/fstab dosyasında parametre.
10. Kullandığınız Linux işletim sistemi sürüm göre çekirdek parametrelerini ayarlayın. Daha fazla bilgi için SAP notları HANA ve bu kılavuzdaki "Çekirdek parametreleri" bölümüne bakın.
11. Takas alanı ekleyin.
12. İsteğe bağlı olarak, bir grafik Masaüstü Vm'leri testinden yükleyin. Aksi takdirde, bir uzak SAPinst yüklemesini kullanın.
13. SAP yazılımı SAP Service Marketplace ' indirin.
14. HANA veritabanı sunucusunda VM grubu kimliği 1001 sapsys, bir grup oluşturun.
15. SAP HANA, HANA veritabanı yaşam döngüsü Yöneticisi'ni kullanarak VM DB sunucuya yükleyin.
16. SAP ASCS örnek uygulama sunucusuna VM yükleyin.
17. Test sanal makineleri arasında /sapmnt dizin NFS kullanarak paylaşın. VM uygulama sunucusunun NFS sunucusudur.
18. HANA veritabanı sunucusunda VM SWPM'ı kullanarak, HANA içeren veritabanı örneğini yükleyin.
19. Birincil uygulama sunucusunda VM uygulama sunucusunda yükleyin.
20. SAP MC başlatın. SAP GUI veya HANA Studio bağlanın.

## <a name="prepare-azure-vms-for-a-manual-installation-of-sap-hana"></a>Azure sanal makineleri el ile bir SAP HANA yüklemesi için hazırlama
Bu bölümde aşağıdaki konuları içerir:

* İşletim sistemi güncelleştirmeleri
* Disk Kurulumu
* Çekirdek parametreleri
* Dosya sistemleri
* / Etc/hosts dosyası
* / Etc/fstab dosyası

### <a name="os-updates"></a>İşletim sistemi güncelleştirmeleri
Ek yazılım yüklemeden önce Linux işletim sistemi güncelleştirmeleri ve düzeltmeleri denetleyin. Bir düzeltme eki yükleyerek destek masasına bir çağrı almayabilirsiniz.

Kullandığınızdan emin olun:
* SUSE Linux Enterprise Server SAP uygulamaları için.
* Red Hat Enterprise Linux için SAP uygulamaları veya SAP HANA için Red Hat Enterprise Linux. 

Henüz yapmadıysanız, işletim sistemi dağıtımı Linux satıcının Linux aboneliğinizi kaydedin. SUSE, zaten hizmetleri içeren ve otomatik olarak kayıtlı SAP uygulamaları için işletim sistemi görüntülerine sahiptir.

İşte bir örnek için kullanılabilecek bir düzeltme ekleri için SUSE Linux kullanarak denetlemek nasıl **zypper** komutu:

 `sudo zypper list-patches`

Sorun türünü bağlı olarak, düzeltme ekleri, kategori ve önem derecesine göre sınıflandırılır. Kategori için yaygın olarak kullanılan değerler şunlardır: 
- Güvenlik
- Önerilen
- İsteğe bağlı
- Özellik
- Belge
- yast

Önem derecesi için yaygın olarak kullanılan değerler şunlardır:

- Kritik
- Önemli
- Orta
- Düşük
- Belirtilmemiş

**Zypper** komutu yalnızca yüklü paketlerinizdeki gerektiren güncelleştirmeler için arar. Örneğin, bu komutu kullanabilirsiniz:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Parametre ekleyebilirsiniz `--dry-run` sistem güncelleştirmeden güncelleştirme test etmek için.


### <a name="disk-setup"></a>Disk Kurulumu
Azure'da bir Linux VM kök dosya sisteminde bir boyut sınırlaması vardır. Bu nedenle, bir Azure VM'ye SAP çalıştırmak için ek disk alanı eklemek gerekir. SAP uygulama sunucusu için Azure sanal makinelerini, Azure standart depolama disklerinin yeterli olabilir. SAP HANA DBMS Azure Vm'leri için üretim ve üretim uygulamaları için Azure premium depolama disklerini kullanılması zorunludur.

Temel [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), aşağıdaki Azure premium depolama yapılandırması önerilir: 

| VM SKU | RAM |  / hana/veri ve/hana/günlük <br /> LVM'yi veya mdadm Şerit | / hana/paylaşılan | / root birimi | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

Önerilen disk yapılandırması, HANA veri hacmi ve günlük birimi LVM veya mdadm şeritli Azure premium depolama diskleri aynı kümesine yerleştirilir. Azure premium depolama disk artıklık için üç görüntü tuttuğundan, herhangi bir RAID yedeklilik düzeyi tanımlamak gerekli değildir. 

Yeterli depolama alanı yapılandırma emin olmak için bkz: [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) ve [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Ayrıca farklı Azure premium depolama diskleri farklı sanal sabit disk (VHD) aktarım hızı birimlerini açıklandığı gibi göz önünde bulundurun [yüksek performanslı premium depolama ve VM'ler için yönetilen diskler](../../windows/disks-types.md). 

Daha fazla premium depolama disklerini veritabanı veya işlem günlüğü yedeklemeleri depolamak için HANA DBMS VM'ler ekleyebilirsiniz.

Şeritleme yapılandırmak için kullanılan iki ana araçları hakkında daha fazla bilgi için bkz:

* [Linux'ta yazılım RAID yapılandırma](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure konuk işletim sistemi olarak Linux çalıştıran Vm'leri diskleri ekleme hakkında daha fazla bilgi için bkz. [bir Linux VM'ye disk ekleme](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure premium SSD ile diski önbelleğe alma modu tanımlayabilirsiniz. /Hana/data ve /hana/log tutan şeritli kümesi için diski önbelleğe alma işlemi devre dışı bırakın. Diğer birimleri için diğer bir deyişle, diskleri kümesine önbelleğe alma modu **salt okunur**.

Sanal makineler oluşturmak için kullanılacak örnek JSON şablonları bulmak için bkz: [Azure hızlı başlangıç şablonları](https://github.com/Azure/azure-quickstart-templates).
Bir temel şablon vm basit sles şablonudur. Bu, bir yazılım bölümünde listelenmişse ek 100 GB veri diski içerir. Temel olarak bu şablonu kullanın. Şablon, belirli bir yapılandırma için uyarlayabilirsiniz.

> [!NOTE]
> Açıklandığı gibi bir UUID'ye kullanarak Azure depolama diski eklemek önemlidir [Microsoft Azure SUSE Linux vm'lerde SAP NetWeaver'ı çalıştırmak](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Test ortamında aşağıdaki ekran görüntüsünde gösterildiği gibi SAP uygulama sunucusu VM'sine, iki Azure standart depolama diskleri eklenir. Bir disk tüm SAP gibi yazılımları NetWeaver 7.5, GUI SAP ve SAP HANA yüklemesi için depolar. İkinci bir disk, yeterli boş alana ek gereksinimler için kullanılabilir olmasını sağlar. Örneğin, aynı SAP ortamı için ait tüm VM'ler arasında paylaşılması yedekleme ve test verileri ve /sapmnt dizini, diğer bir deyişle, SAP profilleri gerekir.

![SAP HANA uygulama sunucu diskleri penceresinde iki veri diski ve boyutlarının görüntüleme](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Çekirdek parametreleri
SAP HANA belirli Linux çekirdek ayarlarını gerektirir. Bu çekirdek ayarları standart Azure galeri görüntüleri dahil değildir ve el ile ayarlamanız gerekir. SUSE veya Red Hat kullanmadığınıza bağlı olarak, parametrelerin farklı olabilir. Listelenen SAP notları, daha önce bu parametreler hakkında bilgi verin. Ekran görüntülerinde gösterilen SUSE Linux 12 SP1 kullanıldı. 

SLES SAP uygulamaları 12 genel kullanılabilirlik ve SLES SAP uygulamaları 12 SP1 için yeni bir aracı yüklü **ayarlanmış adm**, bu eski yerini **sapconf** aracı. Özel bir SAP HANA profili için kullanılabilir **ayarlanmış adm**. SAP HANA için sistem ayarlamak için aşağıdaki profili kök kullanıcı olarak girin:

   `tuned-adm profile sap-hana`

Hakkında daha fazla bilgi için **ayarlanmış adm**, bkz: [ayarlanmış adm SUSE belgelerine](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

Aşağıdaki ekran görüntüsünde görebilirsiniz nasıl **ayarlanmış adm** değiştirilen `transparent_hugepage` ve `numa_balancing` değerler, gerekli SAP HANA ayarlara göre:

![Gerekli SAP HANA ayarlara göre değerler ayarlanmış adm aracı değiştirir](./media/hana-get-started/image005.jpg)

SAP HANA çekirdek ayarlarını kalıcı yapma, **grub2** SLES 12 üzerinde. Hakkında daha fazla bilgi için **grub2**, bkz: [yapılandırma dosya yapısı](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) SUSE belgelerin bölümü.

Aşağıdaki ekran görüntüsüne nasıl çekirdek ayarlarını yapılandırma dosyasında değiştirildi ve ardından kullanılarak derlenmiş gösterir **grub2 mkconfig**:

![Yapılandırma dosyasında değiştirilebilir ve grub2 mkconfig kullanılarak derlenmiş çekirdek ayarları](./media/hana-get-started/image006.jpg)

YaST kullanarak ayarları değiştirmek için başka bir seçenektir ve **önyükleyici** > **çekirdek parametreleri** ayarları:

![Çekirdek parametreleri ayarlar sekmesinde YaST önyükleme yükleyicisi](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Dosya sistemleri
Aşağıdaki ekran görüntüsünde, iki bağlı Azure standart depolama diskleri üzerinde SAP uygulama sunucusu VM üzerinde oluşturulan iki dosya sistemleri gösterir. Her iki dosya sistemleri XFS türünü ve /sapdata ve /sapsoftware bağlanır.

Bu şekilde, dosya sistemleri yapısı için zorunlu değildir. Diğer seçenekler için disk alanı nasıl var. Kök dosya sistemi boş alan çalışmasını önlemek için en önemli husustur bakın.

![SAP uygulama sunucusu VM üzerinde oluşturulmuş iki dosya sistemleri](./media/hana-get-started/image008.jpg)

SAP HANA DB VM SAPinst SWPM ile kullandığınızda bir veritabanı yüklemesi sırasında ve **tipik** yükleme seçeneği, her şeyin /hana ve /usr/sap altında yüklendiğinden. Altında /usr/sap SAP HANA günlük yedekleme için varsayılan konumdur. Yeniden kök dosya sistemi depolama alanınızın bitmesi önlemek önemlidir. SAP HANA SWPM kullanarak yüklemeden önce /hana ve /usr/sap altında yeterli boş alan olduğundan emin olun.

SAP hana standart dosya sistemi Düzen açıklaması için bkz: [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![SAP uygulama sunucusu VM üzerinde oluşturulan ek dosya sistemleri](./media/hana-get-started/image009.jpg)

Standart bir SLES/SLES SAP uygulamaları 12 Azure galeri görüntüsü için SAP NetWeaver'ı yüklediğinizde, aşağıdaki ekran görüntüsünde gösterildiği gibi hiçbir takas alanı belirten bir ileti görüntüler. Bu iletiyi kapatmak için el ile takas dosyası kullanarak ekleyebilirsiniz **GG**, **mkswap**, ve **swapon**. Bilgi edinmek için nasıl arama "dosyayı el ile ekleme" [YaST bölümleyici kullanarak](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) SUSE belgelerin bölümü.

Linux VM Aracısı'nı kullanarak takas alanı yapılandırma başka bir seçenektir. Daha fazla bilgi için bkz. [Azure Linux Aracısı kullanıcı kılavuzu](../../extensions/agent-linux.md).

![Yetersiz takas alanı olduğunu bildiren bir açılır ileti](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>/ Etc/hosts dosyası
SAP yüklemeye başlamadan önce ana bilgisayar adları ve IP adresleri SAP sanal makinelerin/Etc/Hosts dosyasında eklediğinizden emin olun. Bir Azure sanal ağ içindeki tüm SAP sanal makineleri dağıtın. İç IP adreslerini, ardından burada gösterildiği gibi kullanın:

![Ana bilgisayar adları ve IP adresleri/Etc/Hosts dosyasında listelenen SAP VM](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>/ Etc/fstab dosyası

Eklemek yararlıdır **nofail** fstab dosyasını parametresi. Bir diskle sorun yaşanırsa bu şekilde, VM'yi önyükleme işleminin yanıt vermeyi durdurur değil. Ancak ek disk alanı kullanılabilir olmayabilir ve kök dosya sistemi işlemleri dolgu unutmayın. /Hana eksikse, SAP HANA başlatılamıyor.

![Fstab dosyaya nofail parametre ekleyin](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>SLES 12/SLES SAP uygulamaları 12 için grafik GNOME Masaüstü
Bu bölümde açıklanmaktadır nasıl yapılır:

* GNOME Masaüstü ve xrdp SLES 12/SLES üzerinde SAP uygulamaları 12 yükleyin.
* Firefox SLES 12/SLES üzerinde SAP uygulamaları 12 kullanarak Java tabanlı SAP MC çalıştırın.

Bu kılavuzda açıklanan olmayan Xterminal veya VNC, gibi alternatifleri de kullanabilirsiniz.

### <a name="install-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>GNOME Masaüstü ve xrdp SLES 12/SLES üzerinde SAP uygulamaları 12 yükleyin.
Windows arka plan varsa, Firefox, SAPinst, GUI SAP, SAP MC veya HANA Studio çalıştırmak için bir grafik Desktop'ta doğrudan SAP sanal makineleri kolayca kullanabilirsiniz. Ardından, sanal makineye Uzak Masaüstü Protokolü (RDP) aracılığıyla bir Windows bilgisayardan bağlanabilirsiniz. Üretim ve üretim dışı Linux tabanlı sistemler GUI'ler ekleme hakkında daha fazla şirket ilkelerinize bağlı olarak, sunucunuzda GNOME yüklemek isteyebilirsiniz. GNOME Masaüstü SAP uygulamaları 12 VM için bir Azure SLES 12/SLES yüklemek için aşağıdaki adımları izleyin.

1. GNOME Masaüstü PuTTY penceresinde aşağıdaki komutu, örneğin, girerek yükleyin:

   `zypper in -t pattern gnome-basic`

2. Bir VM ile RDP bağlantılarına izin vermek için xrdp yükleyin:

   `zypper in xrdp`

3. /Etc/sysconfig/windowmanager düzenleyin ve GNOME için varsayılan Pencere Yöneticisi'ni ayarlayın:

   `DEFAULT_WM="gnome"`

4. Çalıştırma **chkconfig** bu xrdp yeniden başlatmanın ardından otomatik olarak başlar emin olmak için:

   `chkconfig -level 3 xrdp on`

5. RDP bağlantısıyla ilgili bir sorun varsa, örneğin, bir PuTTY penceresinden yeniden deneyin:

   `/etc/xrdp/xrdp.sh restart`

6. Önceki adımda bahsedilen bir xrdp yeniden işe yaramazsa, için .pid dosyasını denetleyin:

   `check /var/run` 

   Aranacak `xrdp.pid`. Bulursanız, kaldırın ve yeniden deneyin.

### <a name="start-sap-mc"></a>SAP MC Başlat
GNOME Masaüstü yükledikten sonra grafik Java tabanlı SAP MC Firefox ' başlatın. SAP uygulamalarını 12 VM için bir listelenen içinde Azure SLES 12/SLES çalışırsa bir hata görüntülenebilir. Eksik Java tarayıcı nedeniyle eklenti hata oluşur.

SAP MC başlatmak için URL `<server>:5<instance_number>13`.

Daha fazla bilgi için [SAP Yönetim web tabanlı konsolunda başlangıç](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

Aşağıdaki ekran görüntüsünde, Java-tarayıcı eklentisini eksik olduğunda görüntülenen hata iletisini gösterir:

![Tarayıcı eklentisi eksik Java grafiği belirten hata iletisi](./media/hana-get-started/image013.jpg)

Sorunu çözmenin bir yolu, YaST, kullanarak aşağıdaki ekran görüntüsünde gösterildiği gibi eksik yüklemektir:

![Eksik'ı yüklemek için YaST kullanma](./media/hana-get-started/image014.jpg)

SAP Yönetim Konsolu URL'si girin, eklentiyi etkinleştirmeniz istenir:

![Eklenti etkinleştirme isteyen bir iletişim kutusu](./media/hana-get-started/image015.jpg)

Ayrıca, eksik bir dosya ile ilgili bir hata iletisi alabilirsiniz javafx.properties. Oracle Java 1.8 gereksinimini SAP GUI 7.4 ilgilidir. Daha fazla bilgi için [SAP notu 2059429](https://launchpad.support.sap.com/#/notes/2059424).
IBM Java sürümü ve SLES/SLES SAP uygulamaları 12 için teslim edilen openjdk paketi gerekli javafx.properties dosyayı içermez. Java SE 8 Oracle'dan indirip çözümüdür.

Tartışma openSUSE üzerinde openjdk ile benzer bir sorun hakkında ek bilgi için bkz. [Sapguı 7.4 Java openSUSE 42.1 için Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>SAP HANA el ile yükleme: SWPM
Bu bölümdeki ekran görüntüleri bir dizi SWPM SAPinst ile kullandığınızda, SAP NetWeaver 7.5 ve SAP HANA SP12 yükleme için temel adımları gösterir. NetWeaver 7.5 yüklemesinin bir parçası olarak, SWPM HANA veritabanı tek bir örnek olarak da yükleyebilirsiniz.

Örnek test ortamında bir Gelişmiş iş uygulaması programlama (ABAP) uygulama sunucusu yüklediğimiz. Aşağıdaki ekran görüntüsünde gösterildiği gibi kullandık **dağıtılmış sistemi** ASCS ve birincil uygulama sunucu örnekleri, bir Azure sanal Makinesinde yüklemek için seçeneği. SAP HANA veritabanı sistem başka bir Azure VM olarak kullandık.

![ASCS ve Dağıtılmış Sistem seçeneği kullanılarak yüklenen birincil uygulama sunucu örnekleri](./media/hana-get-started/image012.jpg)

ASCS örnek uygulama sunucusunda VM yüklendikten sonra SAP yönetim konsolunda yeşil bir simge tarafından tanımlanır. SAP HANA veritabanı sunucusuyla VM SAP profili dizin içeren dizin /sapmnt paylaşılması gerekir. DB yükleme adımı, bu bilgilere erişmesi gerekir. Erişim sağlamak için en iyi yolu YaST kullanarak yapılandırılabilecek NFS kullanmaktır.

![SAP yönetim yeşil bir simge kullanarak VM uygulama sunucusunda yüklü ASCS örneği gösteren konsol](./media/hana-get-started/image016.jpg)

Uygulama sunucusunda VM kullanarak /sapmnt dizini NFS paylaşılan **rw** ve **no_root_squash** seçenekleri. Varsayılanlar **ro** ve **root_squash**, hangi neden olabilir sorunları veritabanı örneğini yüklediğinizde.

![Rw ve no_root_squash seçenekleri kullanarak NFS aracılığıyla /sapmnt dizine paylaşma](./media/hana-get-started/image017b.jpg)

Sonraki ekran görüntüsünde gösterildiği gibi uygulama sunucusu VM'SİNDEN /sapmnt paylaşımı SAP HANA veritabanı sunucusunda VM kullanarak yapılandırılmalıdır **NFS İstemcisi** ve YaST:

![NFS İstemcisi kullanılarak yapılandırılan /sapmnt paylaşımı](./media/hana-get-started/image018b.jpg)

Bir dağıtılmış NetWeaver 7.5, diğer bir deyişle, yüklemesi için bir **veritabanı örneği**, SAP HANA veritabanı sunucusu VM'sine oturum açın ve SWPM başlatın:

![SAP HANA veritabanı sunucusu VM'sine oturum açma ve başlangıç SWPM bir veritabanı örneğini yükleme](./media/hana-get-started/image019.jpg)

Seçtikten sonra **tipik** Kurulum ve yükleme medyasını yolunu DB SID, ana bilgisayar adı, örnek numarasını ve DB sistem yöneticisi parolasını girin:

![SAP HANA veritabanı sistem yönetici oturum açma sayfası](./media/hana-get-started/image035b.jpg)

DBACOCKPIT şema için parolayı girin:

![Parola kutusu DBACOCKPIT şeması](./media/hana-get-started/image036b.jpg)

Bir soru SAPABAP1 şema parolasını girin:

![Bir soru SAPABAP1 şema parolasını girin](./media/hana-get-started/image037b.jpg)

Her görev tamamlandıktan sonra DB yükleme işleminin her aşamada yanında yeşil bir onay işareti görüntülenir. ' % S'iletisi ", yürütme... Veritabanı örneği tamamlandı"görüntülenir.

![Görev tamamlandı penceresi onay iletisi](./media/hana-get-started/image023.jpg)

Başarılı yüklemeden sonra SAP Yönetim Konsolu yeşil bir simge ile DB örnek ayrıca gösterir. SAP HANA süreçlerinize hdbindexserver ve hdbcompileserver tam listesini görüntüler.

![SAP HANA işlemlerin listesi ile SAP Yönetim Konsolu penceresi](./media/hana-get-started/image024.jpg)

Aşağıdaki ekran görüntüsünde, parçaları SWPM HANA yükleme işlemi sırasında oluşturulan /hana/shared dizin altında dosya yapısı gösterilmektedir. Farklı bir yol belirtmek için seçeneği olduğundan, SAP HANA yüklemeden önce /hana dizini altında ek disk alanı SWPM kullanarak bağlamak önemlidir. Bu adım kök dosya sistemi boş alan çalışmasını engeller.

![HANA yükleme işlemi sırasında oluşturulan /hana/shared dizin dosya yapısı](./media/hana-get-started/image025.jpg)

Bu ekran /usr/sap dizine dosya yapısını gösterir:

![/ Usr/sap dizin dosya yapısı](./media/hana-get-started/image026.jpg)

Birincil uygulama sunucusu örneği yüklemek için Dağıtılmış ABAP yüklemesinin son adımı verilmiştir:

![Son adım olarak ABAP yükleme gösteren birincil uygulama sunucusu örneği](./media/hana-get-started/image027b.jpg)

Birincil uygulama sunucusu örneği ve SAP GUI yüklendikten sonra kullanılacak **DBA Cockpit** SAP HANA yüklemesi doğru bir şekilde tamamlandığını onaylamak için işlem:

![DBA Cockpit penceresi başarılı yüklemesini onaylamak](./media/hana-get-started/image028b.jpg)

Son adım olarak, ilk yüklemede HANA Studio SAP app server VM isteyebilirsiniz. Ardından DB sunucusu VM üzerinde çalışan SAP HANA örneği bağlanın.

![SAP HANA Studio SAP app server VM yükleme](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>SAP HANA el ile yükleme: HDBLCM
SAP HANA SWPM kullanarak dağıtılmış yüklemesinin bir parçası yüklemenin yanı sıra, HANA tek başına ilk olarak, HDBLCM kullanarak yükleyebilirsiniz. SAP NetWeaver 7.5, örneğin daha sonra yükleyebilirsiniz. Bu bölümdeki ekran görüntüleri, bu işlemin nasıl çalıştığını gösterir.

HANA HDBLCM aracı hakkında daha fazla bilgi için bkz:

* [Göreviniz için doğru SAP HANA HDBLCM seçin](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm).
* [SAP HANA yaşam döngüsü Yönetim Araçları](https://www.tutorialspoint.com/sap_hana_administration/sap_hana_administration_lifecycle_management.htm).
* [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](https://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf).

İçin varsayılan grup kimliği ayarı sorunlarını önlemek istediğiniz `\<HANA SID\>adm user`, HDBLCM araç tarafından oluşturuldu. SAP HANA HDBLCM aracılığıyla yüklemeden önce adlı yeni bir grup tanımlayın `sapsys` grup kimliği kullanarak `1001`:

!["Yeni Grup sapsys kullanılarak tanımlanmış" Grup Kimliği 1001](./media/hana-get-started/image030.jpg)

HDBLCM ilk kez başlattığınızda, basit bir Başlat menüsünde görüntüler. 1 öğe seç **yeni sisteme yüklemek**:

!["Yeni sistemi yükle" seçeneği HDBLCM başlangıç penceresi](./media/hana-get-started/image031.jpg)

Aşağıdaki ekran görüntüsünde, daha önce seçtiğiniz anahtar seçenekleri görüntüler.

> [!IMPORTANT]
> Bu örnek ve /usr/sap /hana/shared olan yükleme yolu HANA günlük ve veri birimleri için adlı dizinleri kök dosya sisteminin bir parçası olmalıdır. Bu dizinler VM'ye bağlı Azure veri diskleri ait. Daha fazla bilgi için "Disk" Kurulum"bölümüne bakın. 

Bu yaklaşım kök dosya sistemi alanı yetersiz çalışmasını engeller. HANA sistem yöneticisinin kullanıcı kimliği olduğunu göreceksiniz `1005` parçası `sapsys` Grup Kimliğine sahip `1001`, yüklemeden önce tanımlandı.

![Daha önce seçtiğiniz tüm anahtar SAP HANA bileşenlerin listesi](./media/hana-get-started/image032.jpg)

Denetleme  `\<HANA SID\>adm user` /etc/parola dizin ayrıntıları. Aranacak `azdadm`aşağıdaki ekran görüntüsünde gösterildiği gibi:

![HANA \<HANA SID\>listelenen/etc/parola dizinde adm kullanıcı ayrıntıları](./media/hana-get-started/image033.jpg)

SAP HANA HDBLCM kullanarak yükledikten sonra aşağıdaki ekran görüntüsünde gösterildiği gibi SAP HANA Studio dosya yapısı görebilirsiniz. Tüm SAP NetWeaver tabloları içeren SAPABAP1 şeması henüz kullanılamaz.

![SAP HANA Studio SAP HANA dosya yapısı](./media/hana-get-started/image034.jpg)

SAP HANA yükledikten sonra SAP NetWeaver çıktıklarını yükleyebilirsiniz. Aşağıdaki ekran görüntüsünde gösterildiği gibi yükleme SWPM kullanarak dağıtılmış bir yükleme olarak gerçekleştirildi. Bu işlem, önceki bölümde açıklanmıştır. Veritabanı örneği SWPM kullanarak yüklediğinizde, aynı verileri HDBLCM kullanarak girin. Örneğin, ana bilgisayar adı, HANA SID ve örnek numarasını girin. SWPM mevcut HANA yüklemesi kullanır ve başka şemalar ekler.

![SWPM kullanılarak gerçekleştirilen bir dağıtılmış yükleme](./media/hana-get-started/image035b.jpg)

Aşağıdaki ekran görüntüsünde SWPM yükleme adımı DBACOCKPIT şeması hakkında daha fazla veri gireceğiniz gösterilmektedir:

![DBACOCKPIT şema veri girildiği SWPM yükleme adımı](./media/hana-get-started/image036b.jpg)

SAPABAP1 şeması hakkında daha fazla veri girin:

![SAPABAP1 şeması hakkında daha fazla veri girme](./media/hana-get-started/image037b.jpg)

SWPM veritabanı örneği yüklemesi tamamlandıktan sonra SAP HANA Studio SAPABAP1 şemada görebilirsiniz:

![SAP HANA Studio SAPABAP1 şeması](./media/hana-get-started/image038b.jpg)

SAP uygulama sunucusu ve SAP GUI yükleme tamamlandıktan sonra son olarak, HANA veritabanı örneği kullanarak doğrulamak **DBA Cockpit** işlem:

![DBA Cockpit işlemle doğrulandı HANA veritabanı örneği](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>SAP yazılım indirme işlemleri
Aşağıdaki ekran görüntülerinde gösterildiği gibi SAP Service Marketplace ' yazılım indirebilirsiniz.

NetWeaver 7.5, Linux/HANA yükleyin:

 ![NetWeaver 7.5 karşıdan yüklemek için SAP service yükleme ve yükseltme penceresi](./media/hana-get-started/image001.jpg)

HANA SP12 Platform Edition'ı yükleyin:

 ![HANA SP12 Platform sürümü karşıdan yüklemek için SAP service yükleme ve yükseltme penceresi](./media/hana-get-started/image002.jpg)

