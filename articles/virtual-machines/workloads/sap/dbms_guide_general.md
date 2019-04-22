---
title: Azure sanal makineleri DBMS dağıtım SAP iş yükü için dikkate alınacak noktalar | Microsoft Docs
description: SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b19c0fd8af2792a4ffb877e5c6a7fc6b3f94511
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699262"
---
# <a name="considerations-for-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>SAP iş yükü Azure sanal makineleri DBMS dağıtım konuları
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[deployment-guide]:deployment-guide.md
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[planning-guide]:planning-guide.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu kılavuzu belgelerine uygulamak ve Microsoft Azure üzerinde SAP yazılım dağıtmak nasıl bir parçasıdır. Bu kılavuzu okumadan önce okuma [planlama ve Uygulama Kılavuzu][planning-guide]. Bu belge, bir hizmet (Iaas) özellikleri Azure altyapısı kullanarak Microsoft Azure sanal makinelerinde (VM'ler) SAP ilgili DBMS sistemleri genel dağıtım yönlerini kapsar.

Kağıt yüklemeleri ve SAP yazılım dağıtımları için birincil kaynakları temsil eden platformları üzerinde SAP notları ve SAP yükleme belgelerine tamamlar.

Bu belgede, Azure Vm'lerinde SAP ilgili DBMS sistemleri çalıştırma konuları kullanıma sunulmuştur. Bu bölümde belirli DBMS sistemleri için birkaç başvurular var. Bunun yerine, belirli DBMS sistemleri içinde bu yazıda, sonra bu belgede ele alınır.

## <a name="definitions"></a>Tanımlar
Belgede aşağıdaki terimler kullanılır:

* **Iaas**: Hizmet olarak altyapı.
* **PaaS**: Bir hizmet olarak Platform.
* **SaaS**: Bir hizmet olarak yazılım.
* **SAP bileşen**: Tek bir SAP uygulama ERP merkezi bileşeni (ECC), Business Warehouse (BW), çözüm Yöneticisi ya da Enterprise Portal (EP) gibi. Bileşenleri SAP NetWeaver tabanlı olmayan bir uygulama gibi iş nesneleri veya Geleneksel ABAP veya Java teknolojileri üzerinde temel alabilir.
* **SAP ortamı**: Bir veya daha fazla SAP bileşenleri geliştirme, kalite güvencesi, eğitim, olağanüstü durum kurtarma veya üretim gibi bir iş işlevi gerçekleştirmek için mantıksal olarak gruplandırılır.
* **SAP ortamı**: Bu terim, bir müşterinin tamamı SAP varlıkları başvurduğu BT yatay. SAP ortamının tüm üretim ve üretim dışı ortamlar içerir.
* **SAP sistemine**: DBMS katmanı ve bir uygulama katmanı, örneğin, bir SAP ERP geliştirme sistemi birleşimi bir SAP Business Warehouse sistem veya bir SAP CRM üretim sistemini test. Azure dağıtımında, bu iki şirket içi ile Azure arasında bölünen desteklenmez. Sonuç olarak, bir SAP şirket içinde dağıtılabilir veya onun Azure'da dağıtılan sistemidir. Azure'da veya şirket içi bir SAP ortamının farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtma ve Azure'da sistemleri test ancak SAP CRM üretim sistemi şirket içinde dağıtın.
* **Şirketler arası**: Burada VM'ler siteden siteye ve çok siteli sahip bir Azure aboneliğine dağıtılır veya Azure ExpressRoute bağlantısı arasında şirket içi veri merkezlerine senaryo ve Azure açıklar. Azure ortak belgeler, bu tür dağıtımlar şirketler arası senaryoları açıklanmıştır. 

    Bağlantı için şirket içi etki alanları, şirket içi Active Directory ve şirket içi DNS Azure'a genişletmek için nedenidir. Şirket içi yatay aboneliğin Azure varlıkları için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları, sunuculara erişmek ve bu vm'lerdeki DBMS Hizmetleri gibi hizmetleri çalıştırın. Sanal makineler arasında iletişim ve ad çözümlemesi, şirket içi dağıtılan ve Azure'da dağıtılan Vm'leri mümkündür. Bu senaryo, Azure üzerinde SAP varlıklarını dağıtmak için kullanılan en yaygın senaryodur. Daha fazla bilgi için [planlama ve tasarım VPN ağ geçidi için](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> Şirket içi dağıtımlarını SAP sistemlerini, SAP sistemlerini çalıştıran Azure sanal makineleri şirket içi etki alanının üyesi olduğu olan ve üretim SAP sistemlerini için desteklenir. Şirketler arası yapılandırmalar bölümleri dağıtmak için desteklenen veya SAP ortamlarını Azure'a tamamlayın. Hatta tam SAP ortamı Azure'da çalışan bir şirket içi etki alanının parçası olması için bu sanal makineler ve Active Directory/LDAP gerektirir. 
>
> Belgelerinin önceki sürümleri, hibrit BT senaryolarını bahsedilmiştir. Terim *karma* bir şirket içi ile Azure arasında şirketler arası bağlantı olduğunu aslında kökü belirtilmemiş. Bu durumda, karma de Azure sanal makineleri şirket içi Active Directory'nin parçası olduğunu belirtir.
>
>

Bazı Microsoft belge içi ve dışı karışık senaryo özellikle DBMS yüksek kullanılabilirlik yapılandırmaları için biraz farklı tanımlar. SAP ile ilgili belgeler söz konusu olduğunda, içi ve dışı karışık senaryo için siteden siteye veya özel boils [ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantısını ve şirket içi ile Azure arasında dağıtılan bir SAP ortamının.

## <a name="resources"></a>Kaynaklar
Kullanılabilir diğer makaleler üzerinde SAP iş yükü Azure üzerinde. İle başlayan [azure'da SAP iş yükü: Başlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) ilgilendiğiniz alanı seçin.

Aşağıdaki SAP notları bu belgede ele alan in regard to azure'da SAP ilgilidir.

| Not numarası | Unvan |
| --- | --- |
| [1928533] |Azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |Gelişmiş Azure için SAP izleme sorunlarını giderme |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1409604] |Sanallaştırma Windows üzerinde: Gelişmiş izleme |
| [2191498] |Azure ile Linux üzerinde SAP: Gelişmiş izleme |
| [2039619] |Oracle veritabanı'nı kullanarak Microsoft Azure üzerinde SAP uygulamaları: Desteklenen ürünleri ve sürümleri |
| [2233094] |DB6: Linux, UNIX ve Windows için IBM DB2 kullanarak azure'da SAP uygulamaları: Ek bilgiler |
| [2243692] |Linux üzerinde Microsoft Azure (Iaas) sanal makine: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Yükleme notları |
| [2002167] |Red Hat Enterprise Linux 7.x: Yükleme ve yükseltme |
| [2069760] |Oracle Linux 7.x SAP yükleme ve yükseltme |
| [1597355] |Linux için takas alanı önerisi |
| [2171857] |Oracle Database 12c: Linux üzerinde dosya sistemi desteği |
| [1114181] |Oracle veritabanı 11g: Linux üzerinde dosya sistemi desteği |


Linux için tüm SAP notları hakkında bilgi için bkz: [SAP topluluk wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes).

Microsoft Azure Mimarisi ve Microsoft Azure sanal makinelerini nasıl dağıtılan ve çalıştırılan bilgisine ihtiyacınız vardır. Daha fazla bilgi için [Azure belgeleri](https://docs.microsoft.com/azure/).

Genel olarak, Windows, Linux ve DBMS yükleme ve yapılandırma temel olarak herhangi bir sanal makine veya şirket içinde yüklediğiniz çıplak metal makine aynıdır. Azure Iaas kullandığınızda, farklı olan bazı mimarisi ve sistem yönetimi uygulaması karar vardır. Bu belgede, özel mimari ve Azure Iaas kullandığınızda için hazırlıklı olmak için sistem yönetimi farklar açıklanmaktadır.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>RDBMS dağıtımlar için bir VM depolama yapısı
Bu bölümde takip etmek için okuma ve bölümünde verilen bilgileri anlamak [Bu bölümde] [ deployment-guide-3] , [Dağıtım Kılavuzu][deployment-guide]. Anlama ve farklı VM serisi ve bu bölümü okumadan önce standart ve premium depolama arasındaki farkları bilmeniz gerekir. 

Azure Vm'leri için Azure depolama hakkında bilgi edinmek için bkz:

- [Azure Windows Vm'leri için yönetilen disklere giriş](../../windows/managed-disks-overview.md).
- [Azure Linux VM'ler için yönetilen disklere giriş](../../linux/managed-disks-overview.md).

Temel bir yapılandırmada, genellikle işletim sistemi, DBMS ve nihai SAP ikili dosyaları burada veritabanı dosyalarından ayrı bir dağıtım yapısı öneririz. Azure sanal makineler'de çalışan SAP sistemlerini temel VHD veya disk, işletim sistemi, veritabanı yönetim sistemi yürütülebilir dosyalarının ve SAP yürütülebilir dosyaları yüklü olmasını öneririz. 

DBMS veri ve günlük dosyaları, standart depolama veya premium depolama alanında depolanır. Bunlar ayrı disklerin depolanan ve özgün Azure işletim sistemi görüntüsüne VM bağlı mantıksal diskleri. Linux dağıtımları için farklı öneriler, özellikle SAP HANA için belirtilmiştir.

Disk düzeninizi planlarken, bu öğeler arasında en iyi dengeyi bulun:

* Veri dosyalarının sayısını.
* Dosyaları içeren disk sayısı.
* Tek bir diskin IOPS kotalar.
* Disk başına veri aktarım hızı.
* VM boyutu olası ek veri diskleri sayısı.
* VM toplam depolama aktarım hızı sağlar.
* Gecikme süresi farklı Azure depolama türleri sağlar.
* Sanal makine SLA'ları.

Azure, bir veri disk başına IOPS kotası zorlar. Bu kotalar, standart depolama ve premium depolama üzerinde barındırılan diskleri için farklıdır. G/ç gecikme süresi, ayrıca iki depolama türleri arasında farklılık gösterir. Premium depolama, daha iyi g/ç gecikme süresi sunar. 

Her biri farklı VM türleri, sınırlı sayıda ekleyebileceğiniz diskleri sahiptir. Premium depolama yalnızca belirli VM türleri kullanabileceğiniz başka bir kısıtlaması yoktur. Genellikle, CPU ve bellek gereksinimlerine göre belirli bir VM'nin tür kullanmaya karar. Ayrıca, IOPS, gecikme süresi ve genellikle disk sayısını veya premium depolama disklerini türü ile ölçeklenir disk aktarım hızı gereksinimleri de düşünebilirsiniz. Her disk tarafından elde IOPS ve aktarım hızı sayısı, disk boyutu, özellikle premium depolama ile belirleyebilir.

> [!NOTE]
> DBMS dağıtımları için tüm veriler, işlem günlüğü için premium depolama kullanılması önerilir veya dosyaları Yinele. Üretim ve üretim dışı sistemlerini dağıtmak isteyip istemediğinizi önemi yoktur.

> [!NOTE]
> Azure'dan yararlanmak için benzersiz [tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), bağlı olan tüm diskler temel VHD içeren premium depolama türü olmalıdır.

> [!NOTE]
> Ana veritabanı dosyalarını barındıran, veri ve günlük dosyaları gibi Azure veri merkezlerine bitişik birlikte bulunan üçüncü taraf veri merkezlerinde bulunan depolama donanımı üzerinde SAP veritabanlarının desteklenmez. SAP iş yükleri için yerel bir Azure hizmeti olarak temsil edilen yalnızca depolama, veri ve işlem günlük dosyaları SAP veritabanları için desteklenir.

Veritabanı dosyalarını ve günlük ve Yinele dosyaları ve kullandığınız Azure depolama türünü, IOPS, gecikme süresi ve aktarım hızı gereksinimleri tarafından tanımlanır. Yeterli IOPS için birden çok disk kullanın veya daha büyük bir premium depolama diski kullanmak için zorlanabilirsiniz. Birden çok disk kullanırsanız, bir yazılım stripe disklerde veri dosyaları ya da günlük içerir ve dosyaları yeniden oluşturun. Böyle durumlarda, IOPS ve disk aktarım hızı SLA'ları temel premium depolama diskleri veya standart depolama disklerinin ulaşılabilir maksimum IOPS için sonuç kümesi biriktirici.

Önceden belirtildiği gibi tek bir VHD neler sağlar, IOPS gereksinim aşarsa, VHD sayısı arasında veritabanı dosyaları için gereken IOPS sayısını dengeleyin. Disklerde IOPS yükünü dağıtmak için en kolay yolu, bir yazılım Şerit üzerinde farklı diskler oluşturmaktır. Ardından bir dizi veri dosyaları SAP DBMS gerekmez dışında yazılım stripe LUN'ları yerleştirin. stripe, disk sayısını taleplerini IOPS talepleri, disk aktarım hızı taleplerini ve birim tarafından yönetilir.


- - -
> ![Windows][Logo_Windows] Windows
>
> Eşlikli birden çok Azure VHD'leri arasında oluşturmak için Windows depolama alanları kullanmanızı öneririz. En düşük Windows Server 2012 R2 veya Windows Server 2016.
>
> ![Linux][Logo_Linux] Linux
>
> Yalnızca MDADM ve mantıksal birim Yöneticisi (LVM) bir Linux'ta yazılım RAID oluşturmak için desteklenir. Daha fazla bilgi için bkz.
>
> - [Linux'ta yazılım RAID yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) MDADM kullanma
> - [Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) LVM kullanma
>
>

- - -

> [!NOTE]
> Azure depolama üç görüntü VHD tuttuğundan, bu, stripe ne zaman bir yedekleme yapılandırmak için anlam ifade etmez. Yalnızca şeritleme g/ç farklı VHD dağıtılan şekilde yapılandırmanız gerekir.
>

### <a name="managed-or-nonmanaged-disks"></a>Yönetilen veya yönetilmeyen diskler
Bir Azure depolama hesabı, bir yönetim yapısı ve ayrıca sınırlamaları konusunun ' dir. Sınırlamalar, standart depolama hesapları ve premium depolama hesapları arasında farklılık gösterir. Özellikler ve sınırlamalar hakkında daha fazla bilgi için bkz: [Azure depolama ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

Standart depolama için olduğunu bir sınır üzerindeki depolama hesabı başına IOPS unutmayın. İçeren satırı bkz **toplam istek oranı** makaledeki [Azure depolama ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets). Azure aboneliği başına depolama hesabı sayısına ilk bir sınır yoktur. Bakiye bu depolama hesabı sınırları ulaşmaktan kaçınmak için VHD'ler farklı depolama hesapları arasında daha büyük SAP ortamı. Birden fazla binlerce VHD'ler ile birkaç yüz sanal makineler hakkında konuşurken yorucu bir süreç iş budur.

Başvurular ve öneriler için standart depolama bir SAP iş yüküyle birlikte DBMS dağıtımları için standart depolama kullanarak önerilmez çünkü bu kısa sınırlı [makale](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)

Planlama ve farklı Azure depolama hesabında VHD dağıtma yönetim işlemlerini önlemek için Microsoft gelen [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) 2017'de. Yönetilen diskler, standart depolama ve premium depolama için kullanılabilir. Yönetilen disklerin yönetilmeyen disklere kıyasla önemli avantajları şunlardır:

- Yönetilen diskler için Azure farklı VHD farklı bir depolama hesabı arasında otomatik olarak dağıtım sırasında dağıtır. Bu şekilde, depolama hesabı için veri hacmi, g/ç aktarım hızı, sınırlar ve IOPS isabet değildir.
- Yönetilen diskleri kullanarak Azure depolama, Azure kullanılabilirlik kümeleri kavramlarını geliştirir. VM bir Azure kullanılabilirlik kümesinin parçasıysa, sanal makinenin bağlı disk ve taban VHD farklı hata ve güncelleme etki alanlarına dağıtılır.


> [!IMPORTANT]
> Azure yönetilen diskler avantajları göz önünde bulundurulduğunda, Azure yönetilen diskler DBMS dağıtımları ve SAP dağıtımları için genel kullanmanızı öneririz.
>

Yönetilmeyen disklerden yönetilen disklere dönüştürme için bkz:

- [Bir Windows sanal makine yönetilmeyen disklerden yönetilen disklere dönüştürme](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks).
- [Linux sanal makinesi yönetilmeyen disklerden yönetilen disklere dönüştürme](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks).


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>VM'ler ve veri diskleri için önbelleğe alma
VM'ler için diskler bağladığınızda, Azure depolamada bulunan bu diskleri VM arasındaki g/ç trafiğinin önbelleğe alınmış seçebilirsiniz. Standart ve premium depolama, önbellek bu tür için iki farklı teknolojileri kullanın.

Aşağıdaki öneriler bu g/ç özelliklerini için standart DBMS varsayın:

- Bu çoğunlukla bir salt okunur bir veritabanının veri dosyalarını karşı iş yüküdür. Bu okuma performansı DBMS sistemi için kritik olan.
- Veri dosyalarını karşı yazma artışları kontrol noktaları veya sabit bir akışa göre gerçekleşir. Bir gün ortalaması alınan, daha az yazma okuma daha vardır. Bu yazma, veri dosyalarını okuma, ters yönde uyumsuzdur ve tüm kullanıcı işlemleri tutmayın.
- Günlük veya yineleme dosyası gereken herhangi bir işlem okumalardan vardır. İşlem günlüğü yedeklemeleri gerçekleştirirken büyük g/ç durumlardır.
- Ana işlem veya yineleme günlük dosyalarını karşı yazma yüktür. G/ç olabilir bağımlı iş yükünü yapısını, 4 KB olarak veya diğer durumlarda, g/ç boyutu 1 MB veya daha fazla küçük.
- Tüm yazma işlemlerini güvenilir bir biçimde diskte kalıcı gerekir.

Standart depolama için olası önbellek türleri şunlardır:

* None
* Okuma
* Okuma/Yazma

Tutarlı ve belirleyici performansı elde etmek için DBMS ile ilgili veri dosyalarını içeren tüm diskleri, oturum ve dosya ve tablo alanı için yineleme için standart depolama alanında önbellek kümesi **NONE**. VHD tabanı önbelleğe almayı varsayılan kalabilir.

Premium depolama için aşağıdaki önbelleğe alma seçenekleri mevcuttur:

* None
* Okuma
* Okuma/yazma
* Yok + yazma Hızlandırıcı, yalnızca Azure M serisi VM'ler için
* Okuma + yazma Hızlandırıcı, yalnızca Azure M serisi VM'ler için

Premium depolama için biz kullanmanızı öneririz. **veri dosyaları için önbelleğe alma okuma** SAP veritabanı ve seçin **günlük dosyalarını şuraya diskler için önbelleğe alma**.

M serisi dağıtımları için Azure yazma Hızlandırıcı DBMS dağıtımınız için kullanmanızı öneririz. Ayrıntılar, kısıtlamalar ve Azure yazma Hızlandırıcı, dağıtım için bkz. [yazma hızlandırıcıyı etkinleştirme](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


### <a name="azure-nonpersistent-disks"></a>Azure kalıcı olmayan diskler
VM dağıtıldıktan sonra azure sanal makineleri kalıcı olmayan diskler sunar. VM'yi yeniden başlatma söz konusu olduğunda, bu sürücülerde tüm içeriği silindikten. Bu bir biçimde hiçbir koşulda veri dosyaları ve veritabanı günlük ve Yinele dosyaları bu kalıcı olmayan sürücüler bulunmalıdır. Burada bu kalıcı olmayan sürücüler tempdb ve geçici açabilmek için uygun olabilir bazı veritabanları için özel durumlar olabilir. Bu kalıcı sürücülerini aktarım hızının, VM ailesi ile sınırlı olduğundan, A serisi VM'ler için bu sürücüler kullanmaktan kaçının. 

Daha fazla bilgi için [azure'da Windows VM'ler geçici sürücüyü anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).

- - -
> ![Windows][Logo_Windows] Windows
>
> Azure VM'de D sürücüsü Azure işlem düğümü üzerinde bazı yerel disk ile desteklenir kalıcı bir sürücüdür. Olduğundan kalıcı, VM yeniden başlatıldığında D sürücüsündeki içeriği yapılan tüm değişiklikler kaybolur. Depolanan dosyaları, oluşturulan dizinleri ve yüklenen uygulamaların değişiklikler içerir.
>
> ![Linux][Logo_Linux] Linux
>
> Azure sanal makineleri otomatik olarak bir sürücü üzerinde Azure işlem düğümünün yerel disk ile desteklenir, kalıcı bir sürücü /mnt/resource bağlayın. Olduğundan kalıcı, VM yeniden başlatıldığında/mnt/kaynak içerikte yapılan tüm değişiklikler kaybolur. Depolanan dosyaları, oluşturulan dizinleri ve yüklenen uygulamaların değişiklikler içerir.
>
>

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure depolama dayanıklılık
Microsoft Azure depolama, en az üç farklı depolama düğümlerinde işletim sistemi ve bağlı diskleri veya BLOB'ları ile temel VHD depolar. Bu depolama türü, yerel olarak yedekli depolama (LRS) olarak adlandırılır. Azure depolama tüm türleri için varsayılan seçenek LRS'dir.

Diğer yedeklilik yöntemleri vardır. Daha fazla bilgi için [Azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>Premium depolama için depolama DBMS Vm'leri ve diskleri, veritabanı ve günlük ve dosyaları yeniden önerilen türüdür. Premium depolama için yalnızca yedeklilik yöntemi lrs'dir. Sonuç olarak, başka bir Azure bölgesi veya kullanılabilirlik bölgesi içinde veritabanı veri çoğaltmayı etkinleştirmek için veritabanı yöntemlerini yapılandırmak gerekir. SQL Server Always On, Oracle Data Guard ve HANA sistem çoğaltması veritabanı yöntemleri içerir.


> [!NOTE]
> DBMS dağıtımları için standart depolama için coğrafi olarak yedekli depolama (GRS) kullanılması önerilmez. GRS ciddi bir şekilde performansı etkiler ve yazma sırası arasında bir VM'ye bağlı farklı VHD dikkate değil. Yazma sırasını arasında farklı VHD potansiyel olarak uygularken değil çoğaltma hedef tarafta tutarsız veritabanlarını yol açar. Genellikle, kaynak VM yan olduğu gibi veritabanı ve günlük ve Yinele dosyaları birden çok VHD arasında yayılır, bu durum meydana gelir.



## <a name="vm-node-resiliency"></a>VM düğüm dayanıklılık
Azure Vm'leri için birkaç farklı SLA'lar sunar. Daha fazla bilgi için bkz. en son sürümü [sanal makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). DBMS katman uçtaki bir SAP sistemiyle kullanılabilirlik genellikle önemli olduğundan, kullanılabilirlik kümeleri, kullanılabilirlik ve Bakım olayları anlamanız gerekir. Bu kavramlarla ilgili daha fazla bilgi için bkz. [azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ve [Azure Linux sanal makinelerinin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

En az bir SAP iş yükü ile üretim DBMS senaryoları için önerilir:

- Ayrı birer kullanılabilirlik kümesi aynı Azure bölgesinde iki sanal makine dağıtın.
- Bu iki VM aynı Azure sanal ağı içinde çalıştırın ve NIC dışında aynı alt ağda eklenmiş.
- İkinci VM ile etkin bir bekleme tutmak veritabanı yöntemlerini kullanın. Yöntemler, SQL Server Always On, Oracle Data Guard veya HANA sistem çoğaltması olabilir.

Ayrıca üçüncü bir Azure bölgesindeki başka bir VM dağıtma ve başka bir Azure bölgesinde bir zaman uyumsuz çoğaltması sağlamak için aynı veritabanı yöntemlerini kullanabilirsiniz.

Azure kullanılabilirlik kümeleri konusunda daha fazla bilgi için bkz: [Bu öğreticide](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).



## <a name="azure-network-considerations"></a>Azure ağ konuları
Şema, büyük ölçekli SAP dağıtımlarda kullanın [Azure sanal veri merkezi](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter). Sanal ağ yapılandırma, izinleri ve rol atamaları kuruluşunuzun farklı bölümleri için kullanın.

Bu en iyi müşteri dağıtımları yüzlerce sonucudur:

- SAP uygulama içine dağıtılır sanal ağlar, internet erişiminiz yok.
- ' % S'veritabanı Vm'leri uygulama katmanı aynı sanal ağda çalıştırılır.
- Sanal ağ içindeki sanal makineler özel IP adresini statik ayırma vardır. Daha fazla bilgi için [IP adresi türleri ve ayırma yöntemleri azure'da](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm).
- Yönlendirme kısıtlamalar DBMS VM'lerin *değil* yerel DBMS Vm'lerde yüklü güvenlik duvarları ile ayarlayın. Bunun yerine, trafik yönlendirme ile tanımlanan [ağ güvenlik grupları (Nsg'ler)](https://docs.microsoft.com/azure/virtual-network/security-overview).
- Ayırın ve DBMS VM trafiğini yalıtmak için VM farklı Nıc'lere atayın. Her NIC, farklı bir IP adresi alır ve her NIC, farklı bir sanal ağ alt ağa atanır. Her alt ağ, farklı NSG kuralları vardır. Ağ trafiği ayrımı ve yalıtım yönlendirme bir ölçüdür. Ağ aktarım hızı için kota ayarlamak için kullanılmaz.

> [!NOTE]
> Azure aracılığıyla statik IP adresleri atayarak bunları tek tek sanal Nıc'lere atayın anlamına gelir. Sanal bir NIC'ye konuk işletim sistemi içinde statik IP adresleri atamayın Olgu üzerinde bazı Azure Hizmetleri gibi Azure Backup kullanan, en azından birincil sanal NIC'yi DHCP ve statik IP adresleri için ayarlanır. Daha fazla bilgi için [sanal makine yedekleme sorunlarını giderme Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Bir VM'ye birden fazla statik IP adresleri atamak için birden çok sanal NIC bir VM'ye atayın.
>


> [!IMPORTANT]
> Yapılandırma [ağ sanal Gereçleri](https://azure.microsoft.com/solutions/network-appliances/) SAP uygulama, bir SAP NetWeaver - DBMS katman arasındaki iletişim yolunun, Hybris veya SAP S/4HANA tabanlı sistem için desteklenmiyor. Bu kısıtlama, işlevi ve Performans nedeniyle olur. Doğrudan bir SAP uygulama katmanı ve DBMS katman arasındaki iletişim yolunun olmalıdır. Kısıtlama içermez [uygulama güvenlik grubu (ASG) ve NSG kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview) doğrudan iletişim yolunun ASG ve NSG kuralları izin veriyorsa. 
>
> İçindeki ağ sanal Gereçleri desteklendiği olmayan diğer senaryolar şunlardır:
>
> * Linux Pacemaker temsil eden bir Azure Vm'leri arasında iletişim yolları, düğümleri ve SBD cihazları açıklandığı gibi küme [SUSE Linux Enterprise Server SAP uygulamaları için Azure vm'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse).
> * Azure sanal makinelerini ve Windows Server genişleme dosya sunucusu (SOFS) arasındaki iletişim yolları ayarlayın açıklandığı kadar [SAP ASCS/SCS örneği ile Azure dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). 
>
> Ağ sanal Gereçleri iletişim yolları, iki iletişim iş ortakları arasında kolayca çift ağ gecikmesi olabilir. Bunlar, SAP uygulama katmanı ve DBMS katmanı arasında kritik yollarda aktarım hızı da kısıtlayabilirsiniz. Bazı müşteri senaryolarda, ağ sanal Gereçleri Pacemaker Linux kümeleri başarısız olmasına neden olabilir. Linux Pacemaker düğümler arasındaki iletişimler SBD cihazını ağ sanal Gereci iletişim kurduğu durumlarda bunlar.
>

> [!IMPORTANT]
> Başka bir tasarım 's *değil* desteklenir SAP uygulama katmanı ve DBMS katman ayrımı olmayan farklı Azure sanal ağlarına [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) birbiriyle. Farklı Azure sanal ağları kullanarak yerine bir Azure sanal ağ içindeki alt ağlar SAP uygulama katmanında ve DBMS katman ayırmak önerilir. 
>
> Değil öneriye ve bunun yerine iki katmanı farklı sanal ağlara ayırmak karar verirseniz, iki sanal ağ olmalıdır [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). 
>
> Unutmayın, ağ trafiğini iki [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağları olan aktarım maliyetleri tabidir. Birçok terabaytlarca oluşan büyük veri hacmi DBMS katman ve SAP uygulama katmanı arasında paylaşılmaz. SAP uygulama katmanında ve DBMS katmanı iki eşlenen Azure sanal ağları arasında arkadaşlarından, önemli maliyetleri birikebilir.

DBMS dağıtım Azure kullanılabilirlik içinde üretim için iki VM kullanımı ayarlayın. Ayrıca, SAP uygulama katmanı ve iki DBMS VM yönetimi ve işlemleri trafiği için ayrı ayrı yönlendirme kullanın. Aşağıdaki resme bakın:

![İki alt ağ içindeki iki sanal makine diyagramı](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="use-azure-load-balancer-to-redirect-traffic"></a>Trafiği yönlendirmek için Azure Load Balancer'ı kullanın
SQL Server Always On veya HANA sistem çoğaltması gibi işlevler kullanılan özel sanal IP adresleri Azure yük dengeleyici yapılandırmasını gerektirir. Yük Dengeleyici etkin DBMS düğüm belirlemek ve özel olarak, etkin veritabanı düğümü trafiği yönlendirmek için araştırma bağlantı noktalarını kullanır. 

Bir yük devretme veritabanı düğümü ise, yeniden yapılandırmak SAP uygulamasına gerek yoktur. Bunun yerine, en yaygın SAP uygulama mimarileri karşı özel sanal IP adresine bağlanın. Bu arada, yük dengeleyicinin trafiği özel sanal IP adresi karşı ikinci düğüme yönlendirerek düğüm yük devretme tepki verir.

Azure'un sunduğu iki farklı [yük dengeleyici SKU'ları](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview): temel SKU ve standart SKU. Azure kullanılabilirlik alanları genelinde dağıtmak istediğiniz sürece, temel Azure load balancer'a SKU ince ayar yapar.

Her zaman her zaman yük dengeleyici ile yönlendirildiğinden SAP uygulama katmanı DBMS VM'ler arasındaki trafiği mi? Yanıt, yük dengeleyicinin nasıl yapılandırdığınıza bağlıdır. 

Şu anda DBMS VM'ye gelen trafik her zaman yük dengeleyici üzerinden yönlendirilir. Giden trafik yol DBMS VM'den VM yük dengeleyici yapılandırmasına bağlıdır. uygulama katmanı. 

Yük Dengeleyici noktalarının DirectServerReturn bir seçeneğini sunar. Bu seçeneği yapılandırıldıysa, SAP uygulama katmanına DBMS VM'den yönlendirilmiş trafiğidir *değil* yük dengeleyici üzerinden yönlendirilir. Bunun yerine, doğrudan uygulama katmanına gider. Noktalarının DirectServerReturn yapılandırılmamışsa, dönüş trafiği SAP uygulama katmanı için yük dengeleyici üzerinden yönlendirilir.

SAP uygulama katmanı ve DBMS katmanı arasında konumlandırılmış yük Dengeleyiciler ile birlikte noktalarının DirectServerReturn yapılandırmanızı öneririz. Bu yapılandırma, iki katman arasındaki ağ gecikmesini azaltır.

Bu yapılandırma ile SQL Server Always On ayarlamak nasıl bir örnek için bkz [Azure'da AlwaysOn Kullanılabilirlik grupları için ILB dinleyicisi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener).

Azure'da SAP altyapısını dağıtımlarınız için bir başvuru olarak yayımlanan GitHub JSON şablonları kullanıyorsanız, bu İnceleme [bir SAP 3 katmanlı sistemi için şablonu](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md). Bu şablonda doğru ayarların yük dengeleyici için de görebilirsiniz.

### <a name="azure-accelerated-networking"></a>Azure hızlandırılmış ağ iletişimi
Daha fazla Azure sanal makineler arasındaki ağ gecikmesini azaltmak için seçtiğiniz öneririz [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Bir SAP iş yükü için SAP uygulama katmanı ve SAP DBMS'yi katmanı için özellikle Azure Vm'leri dağıttığınızda, bunu kullanın.

> [!NOTE]
> Tüm VM türleri, hızlandırılmış ağ destekler. Önceki makalede hızlandırılmış ağ destekleyen bir VM türleri listeler.
>

- - -
> ![Windows][Logo_Windows] Windows
>
> Ağ Windows için hızlandırılmış ile Vm'leri dağıtma konusunda bilgi için bkz [hızlandırılmış ağ ile Windows sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell).
>
> ![Linux][Logo_Linux] Linux
>
> Linux dağıtım hakkında daha fazla bilgi için bkz. [hızlandırılmış ağ ile bir Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).
>
>

- - -

> [!NOTE]
> SUSE, Red Hat ve Oracle Linux söz konusu olduğunda, hızlandırılmış ağ ile son sürümlerde desteklenir. SLES 12 SP2 veya RHEL 7.2 gibi eski sürümleri, Azure hızlandırılmış ağ desteklemez.
>


## <a name="deployment-of-host-monitoring"></a>İzleme ana bilgisayarı dağıtımı
Azure sanal makineler'de SAP uygulamaları üretim sırasında kullanım için SAP, Azure sanal makinelerini çalıştıran fiziksel ana bilgisayarlardan verilerin izleme ana bilgisayarı alma olanağı gerektirir. Belirli bir SAP konak Aracısı düzeltme eki düzeyi gereklidir, SAPOSCOL ve SAP konak Aracısı bu yeteneği sağlar. Tam düzeltme eki düzeyi SAP Not belgelenen [1409604].

Bileşenlerin dağıtımına ilişkin ana verileri SAPOSCOL ve SAP konak Aracısı sunan ve bu bileşenlerden birini yaşam döngüsü yönetimi ile ilgili daha fazla bilgi için bkz. [Dağıtım Kılavuzu][deployment-guide].


## <a name="next-steps"></a>Sonraki adımlar
Belirli bir DBMS hakkında daha fazla bilgi için bkz:

- [SAP iş yükü için SQL Server Azure Sanal Makineler DBMS dağıtımı](dbms_guide_sqlserver.md)
- [SAP iş yükü için Oracle Azure Sanal Makineler DBMS dağıtımı](dbms_guide_oracle.md)
- [SAP iş yükü için IBM DB2 Azure Sanal Makineler DBMS dağıtımı](dbms_guide_ibm.md)
- [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](dbms_guide_sapase.md)
- [SAP maxDB, dinamik önbellek ve azure'da içerik sunucusu dağıtımı](dbms_guide_maxdb.md)
- [Azure işlemlerinde SAP HANA kılavuzu](hana-vm-operations.md)
- [Azure sanal makineler için SAP HANA yüksek kullanılabilirlik](sap-hana-availability-overview.md)
- [Azure sanal makineler'de SAP HANA için yedekleme Kılavuzu](sap-hana-backup-guide.md)

