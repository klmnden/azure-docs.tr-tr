---
title: 'Hızlı Başlangıç: Tek Örnekli SAP HANA Azure sanal makineler üzerinde el ile yükleme | Microsoft Docs'
description: Tek Örnekli SAP HANA el ile yüklenmesi için Azure sanal makineler üzerinde Hızlı Başlangıç Kılavuzu
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
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 14cdb2d3e433da38913ffa29b3b150bdb264278b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658715"
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Hızlı Başlangıç: Azure vm'lerinde Tek Örnekli SAP HANA el ile yükleme
## <a name="introduction"></a>Giriş
Bu kılavuzda, tek örnek SAP HANA Azure sanal makinelerde (VM'ler) SAP NetWeaver 7.5 ve SAP HANA 1.0 SP12 el ile yüklediğinizde kurmanıza yardımcı olur. SAP HANA Azure üzerinde dağıtma bu kılavuzun odak noktasıdır. SAP belgelerine değiştirmez. 

>[!Note]
>Bu kılavuzda, SAP HANA dağıtımları Azure VM'ler içine açıklanmaktadır. SAP HANA HANA büyük örneği dağıtma hakkında daha fazla bilgi için bkz: [kullanarak SAP Azure sanal makinelerde (VM'ler)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Önkoşullar
Bu kılavuz, böyle bir altyapı (ıaas) temel olarak aşina olduğunuzu varsayar:
 * Nasıl sanal makine ya da sanal ağlar Azure portal veya PowerShell aracılığıyla dağıtılır.
 * Azure platformlar arası komut satırı JavaScript nesne gösterimi (JSON) şablonları kullanma seçeneğiniz de dahil olmak üzere arabirimi (CLI).

Bu kılavuz, ayrıca aşina olduğunuzu varsayar:
* SAP HANA ve SAP NetWeaver ve şirket içi yükleme.
* Yükleme ve işletim SAP HANA ve Azure üzerinde SAP uygulama örnekleri.
* Aşağıdaki kavramlar ve yordamlar:
   * Azure sanal ağ planlaması ve Azure depolama alanı kullanımı dahil olmak üzere Azure üzerinde SAP dağıtımını planlama. Bkz: [SAP NetWeaver Azure Virtual Machines'de (VM'ler) - planlama ve Uygulama Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Dağıtım ilkeleri ve VM'ler için Azure'da dağıtmanın yolu. Bkz: [SAP için Azure sanal makineler dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Yüksek kullanılabilirlik için SAP NetWeaver ASCS (ABAP SAP merkezi Hizmetleri), SCS (SAP merkezi hizmetlerinin) ve Azure üzerinde ERS (alındı kapatma hesaplanan). Bkz: [yüksek kullanılabilirlik için SAP NetWeaver Azure vm'lerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Azure üzerinde ASCS/SCS çoklu SID yüklemesini yararlanarak, verimliliği artırmak hakkında ayrıntılar. Bkz: [SAP NetWeaver çoklu SID yapılandırması oluşturma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Linux tabanlı sanal makineleri Azure üzerinde SAP NetWeaver çalışan kurallara göre. Bkz: [SAP NetWeaver Microsoft Azure SUSE Linux VM'ler üzerinde çalıştırılan](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Bu kılavuz, Azure depolama diskleri Linux VM'ler için doğru ekleme konusunda Linux Azure Vm'leri ve Ayrıntılar için belirli ayarları sağlar.

Şu anda, Azure sanal makineleri yalnızca SAP HANA büyütme yapılandırmaları SAP tarafından onaylandı. SAP HANA iş yükleri ile genişleme yapılandırmaları henüz desteklenmiyor. SAP HANA yüksek kullanılabilirlik için büyütme yapılandırmalarının durumlarda bkz [SAP HANA yüksek kullanılabilirlik Azure sanal makinelerde (VM'ler)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

SAP HANA örneği veya S/4HANA veya çok hızlı zamanında dağıtılan BW/4HANA sistem almak arıyorsanız kullanımını düşünmelisiniz [SAP bulut Gereci Kitaplığı](http://cal.sap.com). Örneğin, Azure üzerinde SAP CAL S/4HANA sisteminde dağıtma hakkında bulabilirsiniz [bu kılavuzda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Tüm sağlamak için gereken bir Azure aboneliği ve SAP bulut Gereci kitaplıkla kayıtlı bir SAP kullanıcı budur.

## <a name="additional-resources"></a>Ek kaynaklar
### <a name="sap-hana-backup"></a>SAP HANA yedekleme
Azure Vm'lerinde SAP HANA veritabanlarını yedekleme hakkında daha fazla bilgi için bkz:
* [SAP HANA için yedekleme Kılavuzu Azure sanal makineler üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [Dosya düzeyinde bir SAP HANA Azure yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [SAP HANA yedekleme depolama anlık görüntü tabanlı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

### <a name="sap-cloud-appliance-library"></a>SAP bulut Gereci kitaplığı
S/4HANA veya BW/4HANA dağıtmak için SAP bulut Gereci kitaplığını kullanma hakkında daha fazla bilgi için bkz: [dağıtmak SAP S/4HANA veya BW/4HANA Microsoft Azure üzerinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA desteklenen işletim sistemleri
SAP HANA desteklenen işletim sistemleri hakkında daha fazla bilgi için bkz: [SAP destek Not #2235581 - SAP HANA: desteklenen işletim sistemleri](https://launchpad.support.sap.com/#/notes/2235581/E). Azure VM'ler, yalnızca bir alt kümesini bu işletim sistemlerini destekler. SAP HANA azure'da dağıtmak için aşağıdaki işletim sistemlerinde desteklenir: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

SAP HANA ve farklı Linux işletim sistemleri hakkında ek SAP belgeler için bkz:

* [SAP destek Not #171356 - Linux SAP yazılımı: genel bilgiler](https://launchpad.support.sap.com/#/notes/1984787)
* [Destek Not #1944799 - SLES işletim sistemi yüklemesi için SAP HANA yönergeleri SAP](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [SAP destek Not #2205917 - SAP HANA DB işletim sistemi için SLES 12 SAP uygulamaları için önerilen ayarları](https://launchpad.support.sap.com/#/notes/2205917/E)
* [SAP destek Not #1984787 - SUSE Linux Enterprise Server 12: Yükleme notları](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP destek Not #1391070 - Linux UUID çözümleri](https://launchpad.support.sap.com/#/notes/1391070)
* [Destek Not #2009879 - SAP HANA yönergeleri Red Hat Enterprise Linux (RHEL) işletim sistemi için SAP](https://launchpad.support.sap.com/#/notes/2009879)
* [2292690 - SAP HANA DB: RHEL 7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/2292690/E)

### <a name="sap-monitoring-in-azure"></a>SAP Azure'da izleme
SAP Azure'da izleme hakkında daha fazla bilgi için bkz:

* [SAP Not 2191498](https://launchpad.support.sap.com/#/notes/2191498/E). Bu Not SAP "Gelişmiş izleme" ile Linux sanal makineleri Azure üzerinde açıklanır. 
* [SAP Not 1102124](https://launchpad.support.sap.com/#/notes/1102124/E). Bu Not Linux SAPOSCOL hakkındaki bilgiler açıklanmaktadır. 
* [SAP Not 2178632](https://launchpad.support.sap.com/#/notes/2178632/E). Bu Not Microsoft Azure üzerinde için SAP anahtar izleme ölçümleri açıklanır.

### <a name="azure-vm-types"></a>Azure VM türleri
Azure VM türleri ve iş yükü SAP desteklenen senaryolar SAP HANA ile kullanılan konusunda belgelenir [SAP sertifikalı Iaas platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

SAP tarafından SAP NetWeaver veya S/4HANA uygulama katmanı için sertifikalı azure VM türleri konusunda belgelenir [SAP Not 1928533 - Azure SAP uygulamaları: desteklenen ürünler ve Azure VM türleri](https://launchpad.support.sap.com/#/notes/1928533/E).

>[!Note]
>SAP Linux Azure tümleştirme yalnızca Azure Resource Manager ve klasik dağıtım modeli desteklenir. 

## <a name="manual-installation-of-sap-hana"></a>SAP HANA el ile yükleme
Bu kılavuz el ile SAP HANA Azure Vm'lerinde iki farklı şekilde nasıl yükleneceğini açıklar:

* "Yükleme veritabanı örneğine" adımda dağıtılmış NetWeaver yüklemesinin parçası olarak SAP yazılım sağlama Yöneticisi (SWPM) kullanarak
* SAP HANA kullanarak veritabanı lifecycle manager aracı, HDBLCM ve NetWeaver yükleme

Ayrıca SWPM (SAP HANA, SAP uygulama sunucusu ve ASCS örnek) tüm bileşenleri tek tek bir VM'de, yüklemek için bu konuda açıklandığı gibi kullanabileceğiniz [SAP HANA blog duyuru](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Bu seçenek Bu Hızlı Başlangıç Kılavuzu'nda açıklanan değil, ancak göz önünde bulundurmalıdır sorunları aynıdır.

Yükleme başlamadan önce okumanızı öneririz "hazırlama Azure VM'ler SAP HANA el ile yüklenmesi için" bölümünde bu kılavuzda daha sonra. Bunun yapılması, yalnızca bir varsayılan Azure VM yapılandırması kullandığınızda oluşabilecek çeşitli temel hataları önlenmesine yardımcı olabilir.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>SAP SWPM kullandığınızda SAP HANA yükleme için anahtar adımları
Dağıtılmış bir SAP NetWeaver 7.5 yükleme gerçekleştirmek için SAP SWPM kullandığınızda bu bölümde el ile tek örnekli bir SAP HANA yükleme için anahtar adımları listelenir. Tek tek adımları bu kılavuzun ilerleyen bölümlerindeki ekran görüntüleri daha ayrıntılı açıklanmıştır.

1. İki test sanal makineleri içeren Azure sanal ağı oluşturun.
2. İki Azure sanal makinelerini işletim sistemleriyle (örneğimizde, SUSE Linux Enterprise Server (SLES) ve SLES SAP uygulamaları 12 SP1), Azure Resource Manager modeli göre dağıtın.
3. İki Azure standart veya premium depolama diskler (örneğin, 75 GB ya da 500 GB disk) uygulama sunucusu VM ekleyin.
4. Premium depolama diskleri VM HANA DB sunucusu ekleyin. Ayrıntılar için bu kılavuzun ilerleyen bölümlerindeki "Disk Kurulum" bölümüne bakın.
5. Boyutu veya üretilen iş gereksinimlerine bağlı olarak birden çok disk ekleyin ve ardından VM içindeki işletim sistemi düzeyinde mantıksal birim yönetimi ya da cihazları birden çok yönetim aracı (MDADM) kullanarak şeritli birimler oluşturun.
6. XFS dosya sistemleri bağlı disklerde ya da mantıksal birimler oluşturun.
7. Yeni XFS dosya sistemleri işletim sistemi düzeyinde bağlayın. Bir dosya sistemi için tüm SAP yazılımı kullanın. Bir dosya sistemi /sapmnt dizin ve yedeklemesi için örneğin kullanın. SAP HANA DB sunucuda XFS dosya sistemleri /hana ve /usr/sap olarak premium depolama disklerde bağlayın. Bu işlem doldurma Linux Azure Vm'lerinde büyük değil, kök dosya sistemi engellemek gereklidir.
8. / Etc/Hosts dosyasında VM'ler test yerel IP adreslerini girin.
9. Girin **nofail**  /etc/fstab dosyasında parametresi.
10. Kullanmakta olduğunuz Linux işletim sistemi sürüm göre Linux çekirdek parametrelerini ayarlayın. Daha fazla bilgi için HANA ve bu kılavuzda "Çekirdek parametreleri" bölümünde ele alan uygun SAP notlara bakın.
11. Takas alanı ekleyin.
12. İsteğe bağlı olarak, bir grafik Masaüstü VM'ler testinden yükleyin. Aksi takdirde uzaktan SAPinst yükleme kullanın.
13. SAP yazılım SAP hizmet marketten indirin.
14. SAP ASCS örneği VM uygulama sunucuya yükleyin.
15. NFS kullanarak /sapmnt directory test VM'ler arasında paylaşın. Uygulama sunucusu VM NFS sunucudur.
16. DB sunucusu VM SWPM kullanarak HANA, de dahil olmak üzere veritabanı örneğini yükleyin.
17. Birincil uygulama sunucusu (PA'lar), VM uygulama sunucuya yükleyin.
18. SAP Yönetim Konsolu'nu (SAP MC) başlatın. SAP GUI veya HANA Studio, örneğin bağlayın.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>HDBLCM kullandığınızda SAP HANA yükleme için anahtar adımları
Dağıtılmış bir SAP NetWeaver 7.5 yükleme gerçekleştirmek için SAP HDBLCM kullandığınızda bu bölümde el ile tek örnekli bir SAP HANA yükleme için anahtar adımları listelenir. Her bir adım ekran görüntüleri bu kılavuzda daha ayrıntılı açıklanmıştır.

1. İki test sanal makineleri içeren Azure sanal ağı oluşturun.
2. İki Azure VM işletim sistemleriyle (örneğimizde, SLES ve SLES SAP uygulamaları 12 SP1 için) Azure Resource Manager modeli göre dağıtın.
3. İki Azure standart veya premium depolama diskler (örneğin, 75 GB ya da 500 GB disk) VM uygulama sunucusunun ekleyin.
4. Premium depolama diskleri VM HANA DB sunucusu ekleyin. Ayrıntılar için bu kılavuzun ilerleyen bölümlerindeki "Disk Kurulum" bölümüne bakın.
5. Boyutu veya üretilen iş gereksinimlerine bağlı olarak birden çok disk ekleme ve VM içindeki işletim sistemi düzeyinde mantıksal birim yönetimi ya da cihazları birden çok yönetim aracı (MDADM) kullanarak şeritli birimler oluşturabilirsiniz.
6. XFS dosya sistemleri bağlı disklerde ya da mantıksal birimler oluşturun.
7. Yeni XFS dosya sistemleri işletim sistemi düzeyinde bağlayın. Tüm SAP yazılım için bir dosya sistemini kullanmak ve başka biri /sapmnt dizin ve yedeklemeler, örneğin kullanın. SAP HANA DB sunucuda XFS dosya sistemleri /hana ve /usr/sap olarak premium depolama disklerde bağlayın. Bu işlem doldurmasını Linux Azure Vm'lerinde büyük değil, kök dosya sistemi önlemeye yardımcı olmak gereklidir.
8. / Etc/Hosts dosyasında VM'ler test yerel IP adreslerini girin.
9. Girin **nofail**  /etc/fstab dosyasında parametresi.
10. Kullanmakta olduğunuz Linux işletim sistemi sürüm göre çekirdek parametrelerini ayarlayın. Daha fazla bilgi için HANA ve bu kılavuzda "Çekirdek parametreleri" bölümünde ele alan uygun SAP notlara bakın.
11. Takas alanı ekleyin.
12. İsteğe bağlı olarak, bir grafik Masaüstü VM'ler testinden yükleyin. Aksi takdirde uzaktan SAPinst yükleme kullanın.
13. SAP yazılım SAP hizmet marketten indirin.
14. Grup Kimliği 1001 HANA DB sunucusu VM ile sapsys, bir grup oluşturun.
15. SAP HANA HANA veritabanına Lifecycle Manager (HDBLCM) kullanarak VM DB sunucusunda yükleyin.
16. SAP ASCS örneği VM uygulama sunucuya yükleyin.
17. NFS kullanarak /sapmnt directory test VM'ler arasında paylaşın. Uygulama sunucusu VM NFS sunucudur.
18. HANA, HANA DB sunucusu VM SWPM kullanarak dahil olmak üzere veritabanı örneğini yükleyin.
19. Birincil uygulama sunucusu (PA'lar), VM uygulama sunucuya yükleyin.
20. SAP MC başlatın. SAP GUI veya HANA Studio bağlayın.

## <a name="preparing-azure-vms-for-a-manual-installation-of-sap-hana"></a>Azure VM'ler el ile bir SAP HANA yükleme için hazırlama
Bu bölüm aşağıdaki konuları içerir:

* İşletim sistemi güncelleştirmeleri
* Disk Kurulumu
* Çekirdek parametreleri
* Dosya sistemleri
* / Etc/hosts dosyasını
* / Etc/fstab dosyası

### <a name="os-updates"></a>İşletim sistemi güncelleştirmeleri
Linux işletim sistemi güncelleştirmelerini ve ek yazılım yüklemeden önce düzeltmeleri denetleyin. Bir düzeltme eki yükleyerek destek masasına bir çağrı kaçınabilirsiniz olabilir.

Kullanmakta olduğunuz emin olun:
* SUSE Linux Enterprise Server SAP uygulamaları için.
* Red Hat Enterprise Linux SAP uygulamaları veya Red Hat Enterprise Linux SAP HANA için. 

Henüz yapmadıysanız, işletim sistemi dağıtımı Linux satıcıdan Linux aboneliğinizle kaydedin. SUSE zaten hizmetleri içeren ve otomatik olarak kayıtlı SAP uygulamaları için işletim sistemi görüntüleri olduğuna dikkat edin.

İşte bir örnek için kullanılabilir düzeltme ekleri kullanarak SUSE Linux için denetleme **zypper** komutu:

 `sudo zypper list-patches`

Sorun türüne bağlı olarak, düzeltme ekleri kategorisi ve önem derecesini tarafından sınıflandırılır. Kategori için yaygın olarak kullanılan değerler şunlardır: **güvenlik**, **önerilen**, **isteğe bağlı**, **özelliği**, **belge**, veya **yast**.
Önem derecesi için yaygın olarak kullanılan değerler şunlardır: **kritik**, **önemli**, **orta**, **düşük**, veya **belirtilmemiş**.

**Zypper** komutu yalnızca yüklü paketlerinizi gerektiren güncelleştirmeler için arar. Örneğin, bu komutu kullanabilirsiniz:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

Parametre ekleme `--dry-run` sistem güncelleştirmeden güncelleştirme test etmek için.


### <a name="disk-setup"></a>Disk Kurulumu
Azure üzerinde bir Linux VM kök dosya sisteminde bir boyutu kısıtlaması vardır. Bu nedenle, SAP çalıştırmak için bir Azure VM için ek disk alanı eklemek gereklidir. SAP uygulama sunucusu Azure VM'ler için Azure standard storage disklerin yeterli olabilir. Ancak, üretim ve üretim dışı uygulamaları için Azure Premium Storage diskleri kullanımını SAP HANA DBMS Azure VM'ler için zorunludur.

Temel [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), aşağıdaki Azure Premium Storage yapılandırması önerilir: 

| VM SKU | RAM |  / hana/veri ve/hana/günlük <br /> LVM veya MDADM şeritli | / hana/paylaşılan | / root birim | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

Önerilen disk yapılandırmasını HANA veri hacmi ve günlük birimi LVM veya MDADM şeritli Azure premium depolama diskleri aynı kümesini yerleştirilir. Azure Premium Depolama'ya artıklık için diskleri üç görüntülerini tuttuğu için herhangi bir RAID artıklık düzeyi tanımlamak gerekli değildir. Yeterli depolama alanı yapılandırma emin olmak için başvurun [SAP HANA TDI depolama gereksinimlerini](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) ve [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Ayrıca farklı Azure premium depolama disklerinin farklı sanal sabit disk (VHD) üretilen iş birimleri açıklandığı gibi göz önünde bulundurun [yüksek performanslı Premium depolama ve VM'ler için yönetilen diskleri](https://docs.microsoft.com/azure/storage/storage-premium-storage). 

Veritabanı veya işlem günlüğü yedeklemeleri depolamak için HANA DBMS VM'ler daha fazla premium depolama diskleri ekleyebilirsiniz.

Şeritleme yapılandırmak için kullanılan iki ana araçları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Yazılım RAID Linux üzerinde yapılandırma](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure'da bir Linux VM LVM yapılandırın](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure konuk işletim sistemi Linux çalıştıran Vm'leri diskleri ekleme ile ilgili daha fazla bilgi için bkz: [bir Linux VM için bir disk eklemek](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure Premium Storage modları önbelleğe alma disk tanımlamanızı sağlar. /Hana/Data ve /hana/log bulunduran şeritli küme için disk önbelleği devre dışı bırakılması gerekir. Diğer birimler için (diskler) önbelleğe alma modu ayarlanmalı **salt okunur**.

Daha fazla bilgi için bkz: [Premium Storage: Azure sanal makine iş yükleri için yüksek performanslı depolama](../../windows/premium-storage.md).

Sanal makineleri oluşturmak için örnek JSON şablonları bulmak için Git [Azure hızlı başlangıç şablonlarını](https://github.com/Azure/azure-quickstart-templates).
Vm basit sles şablonu temel bir şablondur. Ek 100 GB veri diski ile bir depolama bölüm içerir. Bu şablon, temel olarak kullanılabilir. Şablon belirli yapılandırmanızı uyarlayabilirsiniz.

>[!Note]
>Azure depolama diski açıklandığı gibi bir UUID kullanarak eklemek önemlidir [Microsoft Azure SUSE Linux sanal makineleri üzerinde çalışan SAP NetWeaver](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Test ortamında, aşağıdaki ekran görüntüsünde gösterildiği gibi SAP uygulama sunucusuna VM, iki Azure standart depolama diskleri eklenmedi. Bir disk (NetWeaver 7.5, SAP GUI ve SAP HANA dahil) tüm SAP yazılım yüklemesi için depolanır. İkinci diski güvence altına yeterli boş alana ek gereksinimleri (örneğin, yedekleme ve test veri) için ve aynı SAP yatay ait tüm sanal makineler arasında paylaşılacak /sapmnt dizin (diğer bir deyişle, SAP profilleri) için kullanılabilir olacaktır.

![SAP HANA uygulama sunucusu diskleri penceresine iki veri diskleri ve boyutlarının görüntüleme](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Çekirdek parametreleri
SAP HANA standart Azure galeri görüntüleri parçası olmayan ve el ile ayarlamanız gerekir belirli Linux çekirdek ayarları gerektirir. SUSE veya Red Hat kullanmadığınıza bağlı olarak, Parametreler farklı olabilir. Daha önce listelenen SAP notları bu parametreleri hakkında bilgi verin. SUSE Linux 12 SP1 gösterildiği ekran görüntülerinde kullanıldı. 

SAP uygulamaları 12 GA SLES ve SLES SAP uygulamaları 12 SP1 için sahip yeni bir aracı **ayarlanmış adm**, bu eski yerine **sapconf** aracı. Özel SAP HANA profil kullanılabilir **ayarlanmış adm**. SAP HANA için sistem ayarlamak için bir kök kullanıcı olarak aşağıdakileri girin:

   `tuned-adm profile sap-hana`

Hakkında daha fazla bilgi için **ayarlanmış adm**, bkz: [ayarlanmış adm SUSE belgelerine](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

Aşağıdaki ekran görüntüsünde, gördüğünüz nasıl **ayarlanmış adm** değiştirilen `transparent_hugepage` ve `numa_balancing` gerekli SAP HANA ayarlara göre değerler.

![Olarak ayarlanmış adm aracı gerekli SAP HANA ayarlara göre değerlerini değiştirir](./media/hana-get-started/image005.jpg)

SAP HANA çekirdek ayarlarını kalıcı hale getirmek için kullanmak **grub2** SLES 12 üzerinde. Hakkında daha fazla bilgi için **grub2**gidin [yapılandırma dosyası yapısı](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) SUSE belgelerine bölümü.

Aşağıdaki ekran görüntüsü nasıl çekirdek ayarlarını yapılandırma dosyasında değişti ve kullanarak derlenmiş gösterir **grub2 mkconfig**:

![Çekirdek ayarlarını yapılandırma dosyasında değişti ve grub2 mkconfig kullanarak derlenmiş](./media/hana-get-started/image006.jpg)

YaST kullanarak ayarlarını değiştirmek için başka bir seçenektir ve **önyükleyici** > **çekirdek parametreleri** ayarları:

![Çekirdek parametreler ayarları sekmesinde YaST önyükleme yükleyicisi](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Dosya sistemleri
Aşağıdaki ekran görüntüsü, SAP uygulama sunucusundaki VM iki bağlı Azure standart depolama diskleri en üstünde oluşturulan iki dosya sistemleri gösterir. Her iki dosya sistemleri türü XFS ve /sapdata ve /sapsoftware bağlanır.

Bu şekilde, dosya sistemleri yapısı zorunlu değildir. Disk alanı yapılandırılması için diğer seçeneğiniz vardır. Kök dosya sistemi boş alan çalışmasını engellemek için en önemli noktadır bakın.

![SAP uygulama sunucusunda VM oluşturulan iki dosya sistemleri](./media/hana-get-started/image008.jpg)

SAP HANA DB VM, SAPinst (SWPM) kullandığınızda, bir veritabanı yüklemesi sırasında ile ilgili ve **tipik** yükleme seçeneği her şeyi /hana ve /usr/sap altında yüklenir. SAP HANA günlük yedekleme için varsayılan konum /usr/sap altında değil. Yeniden kök dosya sistemi depolama alanının tükenmesinden engellemek, emin olmak önemlidir çünkü yeterli boş alan olduğundan /hana ve /usr/sap altında SWPM kullanarak SAP HANA yüklemeden önce.

SAP HANA standart dosya sistemi düzenini açıklaması için bkz: [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![SAP uygulama sunucusunda VM oluşturulan ek dosya sistemleri](./media/hana-get-started/image009.jpg)

SAP NetWeaver SAP uygulamaları 12 Azure galeri görüntüsü için standart bir SLES/SLES yüklediğinizde aşağıdaki ekran görüntüsünde gösterildiği gibi hiçbir takas alanı bildiren bir ileti görüntülenir. Bu iletiyi kapatmak için el ile bir takas dosyası kullanarak ekleyebilirsiniz **GG**, **mkswap**, ve **swapon**. Bilgi edinmek için nasıl arama "takas dosyası el ile ekleme" [YaST Bölümleyici kullanarak](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) SUSE belgelerine bölümü.

Linux VM Aracısı'nı kullanarak takas alanı yapılandırma başka bir seçenektir. Daha fazla bilgi için bkz: [Azure Linux Aracısı Kullanıcı Kılavuzu](../../extensions/agent-linux.md).

![Yetersiz takas alanı olduğunu bildiren bir açılır ileti](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>/ Etc/hosts dosyasını
SAP yüklemeye başlamadan önce ana bilgisayar adları ve IP adreslerini SAP VM'ler/Etc/Hosts dosyasında eklediğinizden emin olun. Bir Azure sanal ağı içindeki tüm SAP VM'ler dağıtabilir ve ardından aşağıda gösterildiği gibi iç IP adreslerini kullan:

![Ana bilgisayar adları ve IP adreslerini SAP/Etc/Hosts dosyasında listelenen VM'ler](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>/ Etc/fstab dosyası

Eklemek yararlıdır **nofail** fstab dosyasına parametresi. Bir disklerle sorun yaşanırsa bu şekilde, VM önyükleme işleminde askıda değil. Ancak ek disk alanı kullanılamayabilir ve işlemler kök dosya sistemi doldurun unutmayın. /Hana eksikse, SAP HANA başlatılamıyor.

![Nofail parametresi fstab dosyasına Ekle](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>SLES 12/SLES SAP uygulamaları 12 için grafik GNOME masaüstünde
Bu bölüm aşağıdaki konuları içerir:

* Xrdp ve GNOME Masaüstü için SAP uygulamaları 12 SLES 12/SLES üzerinde yükleme
* Firefox SLES 12/SLES üzerinde için SAP uygulamaları 12 kullanarak Java tabanlı SAP MC çalıştırma

Xterminal veya VNC (Bu kılavuzda açıklanan değil) gibi alternatifleri de kullanabilirsiniz.

### <a name="installing-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>Xrdp ve GNOME Masaüstü için SAP uygulamaları 12 SLES 12/SLES üzerinde yükleme
Bir Windows arka plan varsa, Firefox, SAPinst, SAP GUI, SAP MC veya HANA Studio çalıştırmak ve Uzak Masaüstü Protokolü (RDP) üzerinden VM için bir Windows bilgisayardan bağlanmak için bir grafik Masaüstü doğrudan SAP Linux VM'ler içinde kolayca kullanabilirsiniz. Üretim ve üretim dışı Linux grafik kullanıcı arabirimleri ekleme hakkında şirket ilkelerinizi bağımlı tabanlı sistemlerde, sunucunuzda GNOME yüklemek isteyebilirsiniz. SAP uygulamaları 12 VM için bir Azure SLES 12/SLES GNOME Masaüstü yüklemek için:

1. Aşağıdaki komutu (örneğin, bir pencerede PuTTY) girerek GNOME Masaüstü yükleyin:

   `zypper in -t pattern gnome-basic`

2. RDP aracılığıyla VM için bir bağlantıya izin vermek için xrdp yükleyin:

   `zypper in xrdp`

3. /Etc/sysconfig/windowmanager düzenleyin ve GNOME için varsayılan Pencere Yöneticisi ayarlayın:

   `DEFAULT_WM="gnome"`

4. Çalıştırma **chkconfig** bu xrdp bir yeniden başlatmadan sonra otomatik olarak başlar emin olmak için:

   `chkconfig -level 3 xrdp on`

5. RDP bağlantısı ile ilgili bir sorun varsa, yeniden başlatmayı deneyin (penceresinden PuTTY, örneğin):

   `/etc/xrdp/xrdp.sh restart`

6. Önceki adımda belirtilen bir xrdp yeniden işe yaramazsa için .pid dosyasını denetleyin:

   `check /var/run` 

   Ara `xrdp.pid`. Bulursanız, bunu kaldırın ve yeniden başlatmayı deneyin.

### <a name="starting-sap-mc"></a>SAP MC başlatılıyor
SAP uygulamaları 12 VM için bir listelenen içinde Azure SLES 12/SLES çalıştırılırken grafik Java tabanlı SAP MC Firefox başlayarak GNOME Masaüstü yükledikten sonra bir hata eksik Java tarayıcı nedeniyle eklenti görüntülenebilir.

SAP MC başlatmak için URL `<server>:5<instance_number>13`.

Daha fazla bilgi için bkz: [Web tabanlı SAP Yönetim Konsolu'nu başlatmadan](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

Aşağıdaki ekran görüntüsünde Java-tarayıcı eklentisi eksik olduğunda görüntülenen hata iletisi gösterir:

![Tarayıcı eklentisi eksik Java grafiği belirten hata iletisi](./media/hana-get-started/image013.jpg)

Sorunu çözmenin bir yolu, YaST, kullanarak aşağıdaki ekran görüntüsünde gösterildiği gibi eksik eklenti yüklemektir:

![Eksik eklenti yüklemek için YaST kullanma](./media/hana-get-started/image014.jpg)

SAP Yönetim Konsolu URL'si yeniden girdiğinizde, eklenti etkinleştirme isteyen bir ileti görüntülenir:

![Eklenti etkinleştirme isteyen iletişim kutusu](./media/hana-get-started/image015.jpg)

Eksik dosya ile ilgili bir hata iletisi de alabilirsiniz javafx.properties. Bu SAP GUI 7.4 Oracle Java 1.8 gereksinimini ilgilidir. (Bkz [SAP Not 2059429](https://launchpad.support.sap.com/#/notes/2059424).) IBM Java Sürüm ne için SAP uygulamaları 12 SLES/SLES ile teslim openjdk paket gerekli javafx.properties dosyası içerir. Karşıdan yükleyip Java SE 8 Oracle'dan çözümüdür.

Tartışma iş parçacığı openSUSE üzerinde openjdk ile benzer bir sorun hakkında ek bilgi için bkz [SAPGui 7.4 Java openSUSE 42.1 için Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>SAP HANA el ile yükleme: SWPM
Bu bölümdeki ekran görüntüleri bir dizi SWPM (SAPinst) kullandığınızda, SAP NetWeaver 7.5 ve SAP HANA SP12 yüklemek için önemli adımlar gösterilmektedir. NetWeaver 7.5 yüklemesinin bir parçası olarak SWPM HANA veritabanına tek bir örneği olarak da yükleyebilirsiniz.

Bir örnek test ortamında tek bir Gelişmiş iş uygulama programlama (ABAP) uygulama sunucusu yüklü. Aşağıdaki ekran görüntüsünde gösterildiği gibi kullandık **dağıtılmış sistemi** ASCS ve birincil uygulama sunucu örnekleri başka bir Azure VM veritabanı sistemi olarak bir Azure VM ve SAP HANA yüklemek için seçeneği.

![ASCS ve dağıtılmış sistemi seçeneği kullanılarak yüklenen birincil uygulama sunucu örnekleri](./media/hana-get-started/image012.jpg)

ASCS örnek uygulama sunucusunda VM yüklendikten sonra (SAP yönetim konsolundaki "Yeşil" olarak ayarlandığında, aşağıdaki ekran görüntüsünde gösterilir), VM SAP HANA DB sunucuyla paylaşılan (SAP profil dizini dahil) /sapmnt dizin gerekir. DB yükleme adımı bu bilgilere erişim verilmesi gerekir. Erişim sağlamak için en iyi yolu YaST kullanarak yapılandırılabilir NFS kullanmaktır.

![SAP yönetim ASCS örneği gösteren konsol VM uygulama sunucuda yüklü ve "Yeşil" ayarlayın](./media/hana-get-started/image016.jpg)

Uygulama sunucusunda VM /sapmnt NFS kullanarak paylaşılan dizin **rw** ve **no_root_squash** seçenekleri. Varsayılanlar **ro** ve **root_squash**, hangi sağlama ile ilgili sorunlara veritabanı örneğini yüklediğinizde.

![Rw ve no_root_squash seçeneklerini kullanarak NFS aracılığıyla /sapmnt dizin paylaşımı](./media/hana-get-started/image017b.jpg)

Sonraki ekran görüntüsü gösterildiği gibi VM uygulama sunucusunun /sapmnt paylaşımından SAP HANA DB sunucusu üzerinde VM kullanarak yapılandırılmalıdır **NFS İstemcisi** (ve YaST).

![NFS İstemcisi kullanılarak yapılandırılan /sapmnt paylaşımı](./media/hana-get-started/image018b.jpg)

Dağıtılmış bir NetWeaver 7.5 yüklemeyi gerçekleştirmek için (**veritabanı örneği**), aşağıdaki ekran görüntüsü olarak gösterilen, SAP HANA DB sunucusu VM için oturum açın ve SWPM başlatın.

![SAP HANA DB sunucusu VM için oturum açma ve SWPM başlayarak bir veritabanı örneğini yükleme](./media/hana-get-started/image019.jpg)

Siz seçtikten sonra **tipik** Kurulum ve yükleme medyasını yoluna bir DB SID, ana bilgisayar adını, örnek numarasını ve DB sistem yöneticisi parolasını girin.

![SAP HANA veritabanına sistem yönetici oturum açma sayfası](./media/hana-get-started/image035b.jpg)

DBACOCKPIT şema için parolayı girin:

![DBACOCKPIT şeması için parola giriş kutusu](./media/hana-get-started/image036b.jpg)

SAPABAP1 şema parola için bir soru girin:

![Bir soru SAPABAP1 şema parolasını girin](./media/hana-get-started/image037b.jpg)

Her görev tamamlandıktan sonra DB yükleme işlemi her aşaması yanında yeşil bir onay işareti görüntülenir. İletinin ", yürütme... Veritabanı örneği tamamlandı"görüntülenir.

![Onay iletisi penceresiyle görev tamamlandı](./media/hana-get-started/image023.jpg)

Başarılı yüklemeden sonra SAP Yönetim Konsolu ayrıca DB örneği "Yeşil" olarak göstermek ve SAP HANA işlemler (hdbindexserver, hdbcompileserver ve benzeri) tam listesini görüntüleyebilirsiniz.

![SAP HANA işlemlerin listesini SAP Yönetim Konsolu penceresi](./media/hana-get-started/image024.jpg)

Aşağıdaki ekran görüntüsünde SWPM HANA yükleme işlemi sırasında oluşturulan /hana/shared dizini altındaki dosya yapısı parçaları gösterilmektedir. Farklı bir yol belirtme seçeneği olduğundan SWPM kullanarak SAP HANA yüklemeden önce /hana dizini altında ek disk alanı bağlamak önemlidir. Bu kök dosya sistemi boş alanı tükenmesinden engeller.

![HANA yükleme işlemi sırasında oluşturulan /hana/shared directory dosya yapısı](./media/hana-get-started/image025.jpg)

Bu ekran /usr/sap directory dosya yapısını gösterir:

![/ Usr/sap directory dosya yapısı](./media/hana-get-started/image026.jpg)

Birincil uygulama sunucusu örneği yüklemek için son adımı dağıtılmış ABAP yüklemesinin verilmiştir:

![Son adım olarak ABAP yükleme gösteren birincil uygulama sunucusu örneği](./media/hana-get-started/image027b.jpg)

Birincil uygulama sunucusu örneği ve SAP GUI yüklendikten sonra kullanılacak **DBA Pilot** hareketin SAP HANA yükleme doğru şekilde tamamlandığını doğrulamak için:

![DBA Pilot penceresi başarılı yüklemesini onaylamak](./media/hana-get-started/image028b.jpg)

Son adım olarak, ilk yükleme HANA Studio SAP uygulama sunucusu VM için istediğiniz ve VM DB sunucusu üzerinde çalışan SAP HANA örneğine bağlanın:

![SAP HANA Studio SAP uygulama server VM yükleme](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>SAP HANA el ile yükleme: HDBLCM
SAP HANA SWPM kullanarak dağıtılmış yüklemesinin bir parçası yüklemenin yanı sıra, HANA tek başına ilk olarak, HDBLCM kullanarak yükleyebilirsiniz. SAP NetWeaver 7.5, örneğin daha sonra yükleyebilirsiniz. Bu bölümdeki ekran görüntüleri bu işlemi nasıl çalıştığını gösterir.

HANA HDBLCM aracı hakkında daha fazla bilgi için bkz:

* [Göreviniz için doğru SAP HANA HDBLCM seçme](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [SAP HANA yaşam döngüsü yönetimi araçları](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)
* [SAP HANA sunucusu yükleme ve güncelleştirme Kılavuzu](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

İçin varsayılan grup kimliği ayarını sorunlarını önlemek için `\<HANA SID\>adm user` (HDBLCM aracı tarafından oluşturulan), adlı yeni bir grup tanımlayın `sapsys` grup kimliği kullanarak `1001` SAP HANA HDBLCM aracılığıyla yüklemeden önce:

!["Yeni Grup sapsys kullanılarak tanımlanmış" kimliği 1001 Grup](./media/hana-get-started/image030.jpg)

HDBLCM ilk kez başlattığınızda, basit Başlat menüsü görüntülenir. Öğe Seç 1, **yükleme yeni sistem**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

!["Yeni sistem Yükle" seçeneği HDBLCM başlangıç penceresinde](./media/hana-get-started/image031.jpg)

Aşağıdaki ekran görüntüsünde, daha önce seçtiğiniz tüm anahtar seçeneklerini görüntüler.

> [!IMPORTANT]
> HANA günlük ve veri birimleri, aynı zamanda yükleme yolu (Bu örnekte paylaşılan / hana /) ve /usr/sap, için adlı dizin kök dosya sisteminin parçası olmamalıdır. Bu dizinleri ("Disk Kurulumu" bölümünde açıklanan) VM eklendiği Azure veri diski ait olmalıdır. Bu yaklaşım kök dosya sistemi alanı yetersiz çalışmasını engeller. Aşağıdaki ekran görüntüsünde HANA sistem yöneticisinin kullanıcı kimliği olduğunu görebilirsiniz `1005` ve parçası olan `sapsys` grubu (kimliği `1001`) yüklemeden önce tanımlanmıştı.

![Daha önce seçtiğiniz tüm anahtar SAP HANA bileşenlerin listesi](./media/hana-get-started/image032.jpg)

Kontrol edebilirsiniz `\<HANA SID\>adm user` (`azdadm` aşağıdaki ekran görüntüsü) / etc/parola directory ayrıntıları:

![HANA \<HANA SID \> /etc/parola dizininde listelenen adm kullanıcı ayrıntıları](./media/hana-get-started/image033.jpg)

SAP HANA HDBLCM kullanarak yükledikten sonra aşağıdaki ekran görüntüsünde gösterildiği gibi SAP HANA Studio'da dosya yapısı görebilirsiniz. Tüm SAP NetWeaver tabloları içerir, SAPABAP1 şema henüz kullanılamıyor.

![SAP HANA Studio'da SAP HANA dosya yapısı](./media/hana-get-started/image034.jpg)

SAP HANA yükledikten sonra SAP NetWeaver üzerine yükleyebilirsiniz. Aşağıdaki ekran görüntüsünde gösterildiği gibi yükleme (önceki bölümde açıklandığı gibi) SWPM kullanarak dağıtılmış bir yükleme olarak gerçekleştirildi. Veritabanı örneği SWPM kullanarak yüklediğinizde, aynı veri HDBLCM (örneğin, ana bilgisayar adı, HANA SID ve örnek numarasını) kullanarak girin. SWPM varolan HANA yükleme kullanır ve başka şemalar ekler.

![SWPM kullanılarak gerçekleştirilen bir dağıtılmış yükleme](./media/hana-get-started/image035b.jpg)

Aşağıdaki ekran görüntüsünde DBACOCKPIT şeması hakkında veri girdiğiniz SWPM yükleme adım gösterir:

![DBACOCKPIT şema verileri girildiği SWPM yükleme adımı](./media/hana-get-started/image036b.jpg)

Verileri SAPABAP1 şeması hakkında girin:

![SAPABAP1 şeması hakkında veri girme](./media/hana-get-started/image037b.jpg)

SWPM veritabanı örneği yükleme tamamlandıktan sonra SAP HANA Studio'da SAPABAP1 şema görebilirsiniz:

![SAP HANA Studio'da SAPABAP1 şeması](./media/hana-get-started/image038b.jpg)

SAP uygulama sunucusu ve SAP GUI yükleme tamamlandıktan sonra son olarak, HANA DB örneği kullanarak doğrulayabilirsiniz **DBA Pilot** işlem:

![DBA Pilot işlemle doğrulandı HANA DB örneği](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>SAP yazılım yüklemeleri
Aşağıdaki ekran görüntülerinde gösterildiği gibi yazılım SAP hizmet marketten yükleyebilirsiniz.

NetWeaver 7.5 Linux/HANA yükleyin:

 ![NetWeaver 7.5 indirme SAP hizmeti yükleme ve yükseltme penceresi](./media/hana-get-started/image001.jpg)

HANA SP12 Platform Edition'ı yükleyin:

 ![SAP hizmeti yükleme ve yükseltme penceresi HANA SP12 Platform sürümü karşıdan yüklemek için](./media/hana-get-started/image002.jpg)

