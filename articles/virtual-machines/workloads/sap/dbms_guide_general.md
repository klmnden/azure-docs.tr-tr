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
ms.openlocfilehash: 5e514f35567f4be0932c7bcc591cbd0f05cd9814
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53606767"
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

Bu kılavuz belgeleri Microsoft Azure üzerinde SAP yazılım dağıtma ve uygulama'nın bir parçasıdır. Bu kılavuzu okumadan önce okuma [planlama ve Uygulama Kılavuzu][planning-guide]. Bu belgede, Microsoft Azure Virtual Machines'de (VM'ler) SAP ilgili DBMS sistemleri bir hizmet (Iaas) özellikleri Azure altyapısı kullanılarak genel dağıtım yönlerini kapsar.

Kağıt yüklemeleri ve SAP yazılım dağıtımları için birincil kaynakları temsil eden platformları üzerinde SAP notları ve SAP yükleme belgelerine tamamlar.

Bu belgede, Azure Vm'lerinde SAP ilgili DBMS sistemleri çalıştırma konuları kullanıma sunulmuştur. Bu bölümde belirli DBMS sistemleri için birkaç başvurular var. Bunun yerine belirli DBMS sistemleri içinde bu yazıda, sonra bu belgede ele alınır.

## <a name="definitions-upfront"></a>Ön maliyet tanımları
Belgede aşağıdaki terimler kullanılır:

* Iaas: Hizmet olarak altyapı.
* PaaS: Bir hizmet olarak Platform.
* SaaS: Bir hizmet olarak yazılım.
* SAP bileşeni: ECC, BW, çözüm Yöneticisi veya EP'deki gibi tek bir SAP uygulama SAP bileşenleri geleneksel ABAP veya Java teknolojileri ya da bir olmayan-NetWeaver tabanlı uygulama iş nesneleri gibi temel alabilir.
* SAP ortamı: bir veya daha fazla SAP bileşenleri geliştirme, QAS, eğitim, DR veya üretim gibi bir iş işlevi gerçekleştirmek için mantıksal olarak gruplandırılır.
* SAP ortamı: Bu terim, bir müşterinin tamamı SAP varlıkları başvurduğu BT yatay. SAP ortamı, tüm üretim ve üretim dışı ortamlar içerir.
* SAP sistemi: DBMS katmanı ve uygulama katmanı, örneğin, bir SAP ERP geliştirme sisteminin, SAP BW test sistemi, SAP CRM üretim sistemine vb. birleşimi. Azure dağıtımında, bu iki katmanı şirket içi ile Azure arasında bölmek için desteklenmiyor. Sonuç olarak, bir SAP sistemiyle dağıtılan şirket içinde olduğu veya Azure'da dağıtılır. Ancak, Azure'da veya şirket içi bir SAP ortamının farklı sistemleri dağıtabilirsiniz. Örneğin, SAP CRM geliştirme dağıtma ve Azure ancak SAP CRM üretim sistemi şirket içi sistemleri test edin.
* Şirket içi: Siteden siteye, çok siteli veya ExpressRoute bağlantısı şirket içi datacenter(s) ve Azure arasında olan bir Azure aboneliğine VM'ler dağıtıldığı bir senaryo açıklanır. Azure ortak belgeler, bu tür dağıtımlar şirketler arası senaryoları açıklanmıştır. Bağlantı için şirket içi etki alanları, şirket içi Active Directory ve şirket içi DNS Azure'a genişletmek için nedenidir. Şirket içi yatay aboneliğin Azure varlıkları için genişletilir. Bu uzantı, sanal makineleri şirket içi etki alanının parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları sunucularına erişebilir ve Hizmetleri (gibi hizmetleri DBMS) bu sanal makineler üzerinde çalıştırabilir. Sanal makineler arasında iletişim ve ad çözümlemesi, şirket içi dağıtılan ve Azure'da dağıtılan Vm'leri mümkündür. Bu senaryo, Azure üzerinde SAP varlıklarını dağıtmak için en yaygın bir senaryodur. Daha fazla bilgi için [planlama ve tasarım VPN ağ geçidi için](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> Şirket içi dağıtımlarını SAP sistemlerini SAP sistemlerini çalıştıran Azure sanal makineleri şirket içi etki alanının üyesi olduğu, üretim SAP sistemlerini için desteklenir. Şirketler arası yapılandırmalar bölümleri dağıtmak için desteklenen veya SAP ortamlarını Azure'a tamamlayın. Bile tam SAP ortamı Azure'da çalışan şirket içi etki alanı ile AD/LDAP parçası olan bu VM'nin bulunması gerekir. Belgelerinin önceki sürümleri, hibrit BT senaryolarını, burada bahsedilen terimi *karma* bir şirket içi ile Azure arasında şirketler arası bağlantı olduğunu aslında kökü belirtilmemiş. Bu durumda *karma* aynı zamanda Azure sanal makineleri şirket içi Active Directory'nin parçası olduğunu anlamına gelir.
>
>

Bazı Microsoft belge içi ve dışı karışık senaryo özellikle DBMS yüksek kullanılabilirlik yapılandırmaları için biraz farklı açıklar. Bir siteden siteye veya özel sorun için içi ve dışı karışık senaryo SAP ile ilgili belgeler söz konusu olduğunda, boils [ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantı ve olgu SAP ortamının şirket içi ile Azure arasında dağıtılır.

## <a name="resources"></a>Kaynaklar
Azure'da SAP iş yükü çeşitli makalelerde yayımlanan yok. Başlamak için önerilir [Azure - Get Started SAP iş yüküne](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) ve ilgi'ı seçin

Aşağıdaki SAP notları bu belgede ele alınan alanıyla ilgili Azure üzerinde SAP ilgilidir:

| Not numarası | Unvan |
| --- | --- |
| [1928533] |Azure'da SAP uygulamaları: Desteklenen Ürünler ve Azure VM türleri |
| [2015553] |Microsoft Azure üzerinde SAP: Destek önkoşulları |
| [1999351] |SAP için Gelişmiş Azure izleme sorunlarını giderme |
| [2178632] |Microsoft Azure üzerinde SAP için ölçümleri izleme anahtarı |
| [1409604] |Sanallaştırma Windows üzerinde: Gelişmiş izleme |
| [2191498] |Azure ile Linux üzerinde SAP: Gelişmiş izleme |
| [2039619] |Oracle veritabanı'nı kullanarak Microsoft Azure üzerinde SAP uygulamaları: Desteklenen ürünleri ve sürümleri |
| [2233094] |DB6: Linux, UNIX ve Windows - ek bilgi için IBM DB2 kullanarak azure'da SAP uygulamaları |
| [2243692] |Linux üzerinde Microsoft Azure (Iaas) sanal makine: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Yükleme notları |
| [2002167] |Red Hat Enterprise Linux 7.x: Yükleme ve yükseltme |
| [2069760] |Oracle Linux 7.x SAP yükleme ve yükseltme |
| [1597355] |Linux için takas alanı önerisi |
| [2171857] |Oracle Database 12c - Linux üzerinde dosya sistemi desteği |
| [1114181] |Oracle veritabanı 11g - Linux üzerinde dosya sistemi desteği |


Ayrıca okuma [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , Linux için tüm SAP notları içerir.

Microsoft Azure Mimarisi ve Microsoft Azure sanal makineleri nasıl dağıtılan ve çalıştırılan hakkında bilgiye sahip olmalıdır. Daha fazla bilgi bulabilirsiniz [Azure belgeleri](https://docs.microsoft.com/azure/).

Iaas görüştükten rağmen genel olarak Windows, Linux ve DBMS yükleme ve yapılandırma temelde herhangi bir sanal makine ya da şirket içi yüklenir çıplak metal makine aynıdır. Ancak, Azure Iaas kullanırken farklı uygulama kararlar bazı mimarisi ve sistem yönetim vardır. Bu belgenin amacı, özel mimari ve sizin için Azure Iaas kullanırken hazırlanmalıdır sistem yönetimi farkları açıklamaktadır sağlamaktır.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>RDBMS dağıtımlar için bir VM depolama yapısı
Bu bölümde takip etmek için hangi sunulup sunulmadığı anlamak için gerekli olduğu [bu] [ deployment-guide-3] bölüm [Dağıtım Kılavuzu][deployment-guide]. Farklı VM serisi ve farkları ve Azure Standard'ın farklar hakkında daha fazla bilgi ve [Premium depolama](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) bu bölümü okumadan önce bilinen ve anlaşılması gerekir.

Azure sanal makineler için Azure depolama açısından makalelerle bilgi sahibi olmanız gerekir:

- [Azure Windows Vm'leri için diskleri depolama hakkında](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds)
- [Azure Linux Vm'leri için diskleri depolama hakkında](https://docs.microsoft.com/azure/virtual-machines/linux/about-disks-and-vhds)

Temel bir yapılandırmada, genellikle bir işletim sistemi, DBMS ve nihai SAP ikili dosyaları veritabanı dosyalarından ayrı olduğu dağıtım yapısı öneririz. Bu nedenle, SAP sistemlerini içinde Azure yüklü işletim sistemi, veritabanı yönetim sistemi yürütülebilir dosyalarının ve SAP yürütülebilir dosyaları temel VHD (veya disk) için çalışan sanal makineler için önerilir. DBMS veri ve günlük dosyalarını ayrı disklerin (standart veya Premium depolama) Azure Depolama'da depolanan ve özgün Azure işletim sistemi görüntüsüne VM bağlı mantıksal diskleri. Özellikle Linux dağıtımlarında, belgelenen farklı önerileri olabilir. Özellikle SAP HANA geçici bir çözüm.

Disk düzeninizi planlarken, aşağıdaki öğeleri arasında en iyi dengeyi bulmanız gerekir:

* Veri dosyalarının sayısını.
* Dosyaları içeren disk sayısı.
* Tek bir diskin IOPS kotalar.
* Disk başına veri aktarım hızı.
* VM boyutu olası ek veri diskleri sayısı.
* VM toplam depolama aktarım hızı sağlar.
* Gecikme süresi farklı Azure depolama türleri sağlar.
* Sanal makine SLA'ları

Azure, bir veri disk başına IOPS kotası zorlar. Bu kotalar, Azure standart depolama ve Premium depolama üzerinde barındırılan diskleri için farklıdır. G/ç gecikme süresi, ayrıca iki depolama türleri arasında farklılık gösterir. Premium depolama sayesinde, daha iyi g/ç gecikme Etkenler teslim etme. Her biri farklı VM türleri, sınırlı sayıda iliştirebilir veri diskleri sahiptir. VM türleri yalnızca belirli Azure Premium depolama yararlanabilir başka bir kısıtlaması yoktur. Sonuç olarak, belirli bir VM türüne karar yalnızca CPU ve bellek gereksinimleri de IOPS tarafından tarafından disk sayısını veya Premium depolama diski türü ile genellikle Ölçeklendirildi gecikme süresi ve disk aktarım hızı gereksinimleri yönlendirebilir değil. Özellikle Premium depolama ile bir disk boyutu da IOPS ve her disk tarafından elde gereken aktarım hızı sayısı belirlenmesine.

> [!NOTE]
> DBMS dağıtımları için Premium depolama kullanımı için herhangi bir veri, işlem günlüğü veya Yinele dosyaları önemle tavsiye edilir. Üretim veya üretim dışı sistemlere dağıtmak isteyip istemediğinizi böylece bir önemi yoktur.

> [!NOTE]
> Azure'dan yararlanmak için benzersiz [tek bir sanal makine SLA'sı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) bağlı tüm diskleri temel VHD dahil olmak üzere, Azure Premium depolama türünde olması gerekir.
>

Veritabanı dosyalarını ve günlük/Yinele dosyaları ve Azure kullanılan depolamanın türü yerleşimini IOPS, gecikme ve üretilen iş gereksinimlerine göre tanımlanmalıdır. Yeterli IOPS sahip olmak için birden çok diskler'den yararlanmaya ya da daha büyük bir Premium depolama disk kullanmak üzere zorlanabilirsiniz. Birden çok disk kullanarak olması durumunda, veri dosyalarını içeren veya günlük/dosyaları Yinele disklerde yazılım stripe oluşturmayı tercih. Böyle durumlarda, IOPS ve disk aktarım hızı SLA'ları temel alınan Premium depolama diskleri veya ulaşılabilir maksimum IOPS, Azure standart depolama diskleri için sonuç kümesi biriktirici.

IOPS ihtiyacınıza ne tek bir VHD sağlayabilir, aşarsa önceden belirtildiği gibi veritabanı dosyaları için gerekli VHD sayısı arasında IOPS sayısını dengelemeniz gerekir. Disklerde IOPS yükünü dağıtmak için en kolay yolu, farklı diskler üzerinde yazılım stripe oluşturmaktır. Ardından bir dizi veri dosyaları SAP DBMS gerekmez dışında yazılım stripe LUN'ları yerleştirin. stripe, disk sayısını taleplerini IOPS talepleri, disk aktarım hızı taleplerini ve birim tarafından yönetilir.


- - -
> ![Windows][Logo_Windows] Windows
>
> Bu tür eşlikli birden çok Azure VHD'leri arasında oluşturmak için Windows depolama alanları kullanmanızı öneririz. En az kullanmak için önerilen Windows Server 2012 R2 veya Windows Server 2016.
>
> ![Linux][Logo_Linux] Linux
>
> Yalnızca MDADM ve LVM (mantıksal birim Yöneticisi) bir Linux'ta yazılım RAID oluşturmak için desteklenir. Daha fazla bilgi için bu makaleleri okuyun:
>
> - [Linux'ta yazılım RAID yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) MDADM kullanma
> - [Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) LVM kullanma
>
>

- - -

> [!NOTE]
> Azure depolama, VHD üç görüntüleri tutmak olduğundan, onu bölümlenerek ne zaman bir yedekleme yapılandırmak için mantıklı değildir. G/ç üzerinde farklı VHD dağıtılmasını, bunu bölümlenerek yapılandırmak için yeterlidir.
>

### <a name="managed-or-non-managed-disks"></a>Yönetilen veya yönetilmeyen diskler
Bir Azure depolama hesabı yalnızca bir yönetim yapısı, aynı zamanda bir konu sınırlamalar ' dir. Sınırlamalar, Azure standart depolama hesabı ve Azure Premium depolama hesapları arasında farklılık gösterir. Tam özelliklerini ve sınırlamalarını makalesinde listelenen [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)

Azure standart depolama olduğunu bir sınır üzerindeki depolama hesabı başına IOPS çağırmak önemli olduğunu (satır içeren **toplam istek oranı** makaledeki [Azure depolama ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)). Ayrıca, ilk bir Azure aboneliği başına depolama hesaplarının sayısı sınırı yoktur. Bu nedenle, VHD daha büyük SAP ortamı için bu depolama hesabı sınırları ulaşmaktan kaçınmak için farklı bir depolama hesabı arasında dengelemeniz gerekir. Birden fazla bin VHD'ler ile birkaç yüz sanal makineler hakkında konuşurken yorucu bir iş.

Başvurular ve Azure standart depolama önerileri SAP iş yüküyle birlikte DBMS dağıtımları için Azure standart depolama kullanmak için önerilmez olduğundan, bu kısa sınırlı [makale](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)

Planlama ve farklı Azure depolama hesabında VHD dağıtma yönetim işlemlerini önlemek için ne çağrılır sunulan [yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) 2017'de. Yönetilen diskler, Azure Premium depolama yanı sıra Azure standart depolama için kullanılabilir. Yönetilen diskler gibi yönetilmeyen diskler listesi ile karşılaştırıldığında önemli avantajları:

- Yönetilen diskler için Azure farklı VHD farklı bir depolama hesabı arasında dağıtım sırasında otomatik olarak dağıtan ve böylece bir Azure depolama hesabını veri hacmi, g/ç aktarım hızı ve IOPS açısından sınırlarını ulaşma önler.
- Yönetilen Diskler'i kullanarak, Azure depolama Azure kullanılabilirlik kümeleri kavramlarını uygularken ve VM, bir Azure kullanılabilirlik kümesinin parçasıysa ile bir sanal makinenin bağlı disk ve taban VHD farklı hata ve güncelleme etki alanlarına dağıtır.


> [!IMPORTANT]
> Azure yönetilen diskler avantajları göz önünde bulundurulduğunda, genel DBMS dağıtımları ve SAP dağıtımları için Azure yönetilen diskleri kullanmak için önerilir.
>

Yönetilmeyen disklerden yönetilen disklere dönüştürme için makaleleri inceleyin:

- [Windows sanal makinesini yönetilmeyen disklerden yönetilen disklere dönüştürme](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)
- [Linux sanal makinesi yönetilmeyen disklerden yönetilen disklere dönüştürme](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks)


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>VM'ler ve veri diskleri için önbelleğe alma
VM'ler için diskler bağladığınızda, g/ç trafiğinin VM ve Azure depolamada bulunan bu diskleri arasında olup olmadığını önbelleğe seçebilirsiniz. Azure standart ve Premium depolama, iki farklı teknoloji önbellek bu tür için kullanın.

Aşağıdaki öneriler bu g/ç özelliklerini için standart DBMS varsayılarak:

- Çoğunlukla iş yükünün bir veritabanı veri dosyası okunur. Bu okuma performansı DBMS sistemi için kritik olan
- Veri dosyalarını karşı yazma, kontrol noktaları veya sabit bir akışa dayanan artışları yaşanır. Bununla birlikte, gün içinde ortalama, yazma okuma küçük. Veri dosyalarını okuma ters yönde bu yazma işlemleri zaman uyumsuzdur ve tüm kullanıcı işlemleri tutan değil.
- Günlük veya yineleme dosyası gereken herhangi bir işlem okumalardan vardır. Özel durumu, işlem günlüğü yedeklemeleri gerçekleştirirken büyük g/ç vardır.
- Ana işlem veya yineleme günlük dosyalarını karşı yazma yüktür. G/ç olabilir iş yükü doğasına bağlı, 4 KB olarak veya diğer durumlarda g/ç boyutu 1 MB veya daha fazla küçük.
- Tüm yazma işlemlerini güvenilir bir biçimde diskte kalıcı gerekir

Azure standart depolama için olası önbellek türleri şunlardır:

* None
* Okuma
* Okuma/Yazma

Tutarlı ve belirleyici performansı elde etmek için Azure standart depolama alanında içeren tüm diskler için önbelleğe almayı ayarlamalısınız **DBMS ile ilgili veri dosyaları, günlük/Yinele dosyaları ve tablo alanı için 'NONE'**. VHD tabanı önbelleğe almayı varsayılan kalabilir.

Azure Premium depolama için aşağıdaki önbelleğe alma seçenekleri mevcuttur:

* None
* Okuma
* Okuma/yazma
* Yok + yazma Hızlandırıcısı (yalnızca Azure M serisi VM'ler için)
* Okuma + yazma Hızlandırıcısı (yalnızca Azure M serisi sanal makineler için)

Azure Premium depolama önerilir yararlanmak için **veri dosyaları için önbelleğe alma okuma** SAP veritabanı ve seçtiğiniz **günlük dosyalarını şuraya diskler için önbelleğe alma**.

M serisi dağıtımları için Azure yazma Hızlandırıcı DBMS dağıtım için kullanılacak önemle tavsiye edilir. Ayrıntılar için kısıtlamalar ve Azure yazma Hızlandırıcı dağıtımını belge başvurun [yazma hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


### <a name="azure-non-persistent-disks"></a>Azure kalıcı olmayan diskler
VM dağıtıldıktan sonra azure sanal makineleri kalıcı olmayan diskler sunar. VM'yi yeniden başlatma durumunda, bu sürücülerde tüm içeriği temizlenir. Bu nedenle, olan bir biçimde hiçbir koşulda veri dosyaları ve veritabanı günlük/Yinele dosyaları bu kalıcı olmayan sürücüler bulunmalıdır. Burada bu kalıcı olmayan sürücüler tempdb ve geçici açabilmek için uygun olabilir veritabanlarından bazıları için özel durumlar olabilir. Ancak, bu kalıcı olmayan sürücüler aktarım hızının, VM ailesi ile sınırlı olduğundan, A serisi VM'ler için bu sürücüleri kullanmaktan kaçının. Daha fazla ayrıntı için makaleyi okuyun [azure'da Windows VM'ler geçici sürücüyü anlama](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

- - -
> ![Windows][Logo_Windows] Windows
>
> Azure VM'de D:\ Azure işlem düğümü üzerinde bazı yerel disk ile desteklenir, kalıcı olmayan sürücü sürücüdür. Kalıcı olmayan olduğundan, bu D:\ sürücüsüne içerikte yapılan tüm değişiklikler kayıp VM yeniden başlatıldığında anlamına gelir. "Herhangi bir değişiklik" tutulan dosyaları gibi oluşturulan dizinleri yüklü uygulamalar tarafından vb.
>
> ![Linux][Logo_Linux] Linux
>
> Azure sanal makineleri otomatik olarak bir sürücü üzerinde Azure işlem düğümünün yerel disk ile desteklenir ve kalıcı olmayan bir sürücü /mnt/resource bağlayın. Kalıcı olmayan olduğundan, bu sanal makine yeniden başlatıldığında /mnt/resource içerikte yapılan tüm değişiklikler kaybedilir anlamına gelir. Depolanan, dosyaları Dizinler oluşturuldu, yüklü uygulamalar gibi herhangi bir değişiklikten vb.
>
>

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure depolama dayanıklılık
Microsoft Azure depolama, en az üç farklı depolama düğümlerinde (OS ile) temel VHD ve bağlı diskleri veya BLOB'ları depolar. Bu durum, yerel olarak yedekli depolama (LRS) olarak adlandırılır. Azure depolama tüm türleri için varsayılan değer lrs'dir.

Tüm makalesinde açıklanan birkaç daha fazla yedeklilik yöntemleri vardır [Azure depolama çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>Azure Premium depolama DBMS VM'ler ve veritabanı ve günlük/Yinele dosyaları depolama diskleri için önerilen türü olan depolama itibariyle, yalnızca kullanılabilir yöntem lrs'dir. Sonuç olarak, SQL Server Always On, Oracle Data Guard veya gibi HANA sistem çoğaltması veritabanı başka bir Azure bölgesi veya başka bir Azure kullanılabilirlik alanı veri çoğaltmayı etkinleştirmek için veritabanı yöntemlerini yapılandırmak gerekir.


> [!NOTE]
> Ciddi performans etkisi ve yazma sırası arasında bir VM'ye bağlı farklı VHD dikkate almaz olduğundan DBMS dağıtımları için Azure standart depolama ile kullanılabilir olarak coğrafi olarak yedekli depolama kullanımı önerilmez. Yazma sırasını arasında farklı VHD uygularken değil, olgu yüksek olası bir veritabanı ve günlük/Yinele dosyaları (çoğu durumda gibi) birden çok VHD VM yan kaynak arasında yayılır, tutarsız veritabanlarını çoğaltma hedef tarafta düştüğünden taşıyan.



## <a name="vm-node-resiliency"></a>VM düğüm dayanıklılık
Azure platformu, VM'ler için birkaç farklı SLA'lar sunar. En son sürümünde hakkında tam Ayrıntılar bulunabilir [sanal makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). DBMS katman genellikle bir SAP sistemiyle, bir kullanılabilirlik kritik parçası olduğundan, kendiniz kullanılabilirlik kümeleri, kullanılabilirlik ve Bakım olayları kavramlarla ilgili bilgi sahibi olmak gerekir. Tüm bu kavramları açıklayan makaleleri olan [azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ve [Azure Linux sanal makinelerinin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

En düşük ile SAP iş yükü üretim DBMS senaryoları için önerilir:

- Bir ayrı kullanılabilirlik kümesindeki aynı Azure bölgesindeki iki sanal makine dağıtın.
- Bu iki VM aynı Azure Vnet'te çalıştırılır ve NIC dışında aynı alt ağda'e bağlı olması.
- İkinci VM ile etkin bir bekleme tutmak veritabanı yöntemlerini kullanın. Yöntemler, SQL Server Always On, Oracle Data Guard veya HANA sistem çoğaltması olabilir.

Ayrıca, üçüncü bir VM içinde başka bir Azure bölgesine dağıtın ve başka bir Azure bölgesinde bir zaman uyumsuz çoğaltması sağlamak için aynı veritabanı yöntemleri kullanın.

Azure kullanılabilirlik kümeleri yolu, bu konuda gösterilmiştir [öğretici](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).



## <a name="azure-network-considerations"></a>Azure ağ konuları
Şema, kullanmakta olduğunuz SAP dağıtımlarda büyük ölçekli, öneri olan [Azure sanal veri merkezi](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) VNet yapılandırma, izinleri ve rol atamalarını ve kuruluşun farklı bölümlerine için.

Müşteri dağıtımları yüzlerce dışında sonuçlandı birkaç en iyi uygulamalar şunlardır:

- SAP uygulama Expressroute'lara içine dağıtılır, internet erişimi yok.
- ' % S'veritabanı Vm'leri aynı sanal ağda uygulama katmanı olarak çalışıyor.
- Sanal ağ içindeki sanal makineler özel IP adresini statik ayırma vardır. Makaleye göz atın [IP adresi türleri ve ayırma yöntemleri azure'da](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) başvuru olarak.
- Yönlendirme kısıtlamalar DBMS VM'lerin **değil** yerel DBMS Vm'lerde yüklü güvenlik duvarları ile ayarlayın. Bunun yerine trafik yönlendirme ile tanımlanan [Azure ağ güvenlik grupları (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview)
- Ayrılması ve DBMS VM trafiğini yalıtmak amacıyla VM farklı Nıc'lere atayın. Burada her NIC, farklı bir IP adresi vardır ve her NIC farklı bir sanal ağ alt ağı için bir atanan farklı NSG kuralları yeniden sahip. Ağ trafiği ayrımı ve yalıtım yönlendirme için yalnızca bir ölçüdür ve ağ aktarım hızı için kotaları ayarlama izin vermiyor aklınızda bulundurun.

> [!NOTE]
> Azure yol aracılığıyla statik IP adresleri atamasını bireysel Vnıc'ler için. Konuk işletim sistemi içinde statik IP adresleri için bir Vnıc atamanız gerekir değil. Olgu üzerinde bazı Azure Hizmetleri gibi Azure Backup hizmeti kullanan, en azından birincil Vnıc DHCP ve statik IP adresleri için ayarlanır. Ayrıca bkz [sanal makine yedekleme sorunlarını giderme Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Bir VM'ye birden fazla statik IP adresleri atamak gerekiyorsa, bir VM'ye çoklu Vnıcs atamanız gerekir.
>


> [!IMPORTANT]
> İşlevsellik, ancak daha fazla dışında önemli performans nedeniyle dışında yapılandırmak için desteklenmez [Azure ağ sanal Gereçleri](https://azure.microsoft.com/solutions/network-appliances/) DBMS katmanı bir SAP NetWeaver SAP uygulama arasındaki iletişim yolunun içinde Hybris veya S/4HANA, SAP sistemine bağlı. SAP uygulama katmanı ve DBMS katman arasındaki iletişim, doğrudan bir tane olması gerekiyor. Kısıtlama içermemesi [Azure ASG ve NSG kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview) ASG ve NSG kuralları bir doğrudan iletişime izin vermek sürece. Burada nva'ları desteklenmez başka senaryolar şunlardır açıklandığı gibi Linux Pacemaker küme düğümlerini ve SBD cihazları temsil eden bir Azure VM'ler arasında iletişimi yollarda [SUSE Linux Enterprise Server üzerindeki Azure vm'lerinde SAP NetWeaver için yüksek kullanılabilirlik SAP uygulamaları için](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse). Veya iletişim yollarını arasında Azure VM ve Windows Server SOFS açıklandığı kadar ayarlamak [SAP ASCS/SCS örneği ile Azure dosya paylaşımı kullanarak bir Windows Yük devretme kümesinde Küme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). İletişim yolları can nva'larını kolayca ağ gecikme süresi iki iletişim iş ortakları arasında çift, aktarım hızı SAP uygulama katmanı ve DBMS katman arasında kritik yollarda kısıtlayabilirsiniz. Müşterilerle gözlemlenen bazı senaryolarda, nva'ları Pacemaker Linux kümeleri SBD cihazını bir NVA aracılığıyla iletişim kurmak için Linux Pacemaker düğümler arasındaki iletişimler gerektiğinde başarısız olmasına neden olabilir.
>

> [!IMPORTANT]
> Olan başka bir tasarım **değil** desteklenir SAP uygulama katmanı ve DBMS katman ayrımı olmayan farklı Azure sanal ağlarına [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) birbiriyle. SAP uygulama katmanında ve farklı Azure sanal ağları kullanarak yerine bir Azure sanal ağ içindeki alt ağlara DBMS katman ayırmanız önerilir. İki sanal ağ olmayan öneriye ve bunun yerine iki katmanı farklı bir sanal ağa ayırmak karar verirseniz, olması gereken [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Unutmayın, ağ trafiğini iki [eşlenmiş](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağları olan aktarım maliyetlerinin konu. SAP uygulama katmanında ve DBMS katman arasında alınıp verilen terabayta kadar büyük veri hacmi ile önemli maliyetleri, toplanabilir SAP uygulama katmanında ve DBMS katman iki eşlenen Azure sanal ağları arasında ayrılmış.

DBMS dağıtım yanı sıra SAP uygulama katmanı ve iki DBMS VM yönetimi ve işlemleri trafiği için ayrı yönlendirme Azure kullanılabilirlik kümesi içinde üretim için iki VM kullanarak kaba diyagramı aşağıdaki gibidir:

![İki alt ağ içindeki iki sanal makine diyagramı](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="azure-load-balancer-for-redirecting-traffic"></a>Azure yük dengeleyici trafiği yönlendirme
SQL Server Always On veya HANA sistem çoğaltma gibi işlevler kullanılan özel sanal IP adresleri kullanımı Azure Load Balancer yapılandırılmasını gerektirir. Azure Load Balancer, etkin DBMS düğüm belirlemek ve özel olarak, etkin veritabanı düğümü trafiği yönlendirmek için yoklama bağlantı noktası aracılığıyla kuramıyor. Veritabanı düğümü bir yük devretmesi durumunda, yeniden yapılandırmak SAP uygulamasına gerek yoktur. Bunun yerine en sık karşılaşılan SAP uygulamaları mimarileri karşı özel sanal IP adresi yeniden bağlantı kurar. Bu sırada Azure load balancer ikinci düğüme karşı özel sanal IP adresi trafiği yönlendirerek düğüm yük devretme tepki verilmiş.

Azure'un sunduğu iki farklı [yük dengeleyici SKU'ları](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Temel SKU ve standart SKU. Azure kullanılabilirlik alanları genelinde dağıtmak istediğiniz sürece, temel Azure load balancer'a SKU ince ayar yapar.

Her zaman her zaman Azure yük dengeleyici ile yönlendirildiğinden SAP uygulama katmanı DBMS VM'ler arasındaki trafiği mi? Yanıt, yük dengeleyicinin nasıl yapılandırdığınıza bağlıdır. Bu anda, her zaman DBMS VM'ye gelen trafiği Azure yük dengeleyici üzerinden yönlendirilir. Giden trafik yol DBMS VM'den VM Azure load balancer yapılandırmasına bağlıdır. uygulama katmanı. Yük Dengeleyici noktalarının DirectServerReturn bir seçeneğini sunar. Bu seçeneği yapılandırıldıysa, SAP uygulama katmanına DBMS VM'den yönlendirilen trafik olacak **değil** Azure yük dengeleyici üzerinden yönlendirilir. Bunun yerine uygulama katmanına doğrudan geçer. Noktalarının DirectServerReturn yapılandırılmamışsa, dönüş trafiği SAP uygulama katmanı için Azure yük dengeleyici üzerinden yönlendirilir

SAP uygulama katmanı ve iki katman arasındaki ağ gecikme süresini azaltmak için DBMS katmanı arasında konumlandırılmış Azure load balancer'ları ile birlikte noktalarının DirectServerReturn yapılandırılması önerilir.

Bu tür bir yapılandırma ayarlama örneği SQL server her zaman üzerinde geçici bir çözüm yayımlandıktan [bu makalede](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener).

Azure'da SAP altyapısını dağıtımlarınız için başvuru olarak yayımlanan GitHub JSON şablonları kullanmayı seçerseniz bu üzerinde çalışmanız [bir SAP 3 katmanlı sistemi için şablonu](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md). Bu şablonda doğru ayarların Azure load balancer'ın İnceleme.

### <a name="azure-accelerated-networking"></a>Azure hızlandırılmış ağ iletişimi
Daha fazla Azure sanal makineler arasındaki ağ gecikmesini azaltmak amacıyla seçeneğini seçmek için önerilir [Azure hızlandırılmış ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) Azure Vm'lerinde SAP iş yükü için dağıtırken. Özellikle SAP uygulama katmanı ve SAP DBMS'yi katmanı için.

> [!NOTE]
> Tüm VM türleri, hızlandırılmış ağ destek dosyalarıdır. Makale, hızlandırılmış ağ destekleyen bir VM türleri listesi olduğu.
>

- - -
> ![Windows][Logo_Windows] Windows
>
> Windows makalesine başvurun için [hızlandırılmış ağ ile Windows sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) kavramları ve hızlandırılmış ağ ile Vm'leri dağıtmak için bu şekilde nasıl anlamak için
>
> ![Linux][Logo_Linux] Linux
>
> Linux makaleyi okumak için [hızlandırılmış ağ ile bir Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) ayrıntıları için Linux dağıtımını almak için.
>
>

- - -

> [!NOTE]
> SUSE, Red Hat ve Oracle Linux söz konusu olduğunda, hızlandırılmış ağ ile son sürümlerde desteklenir. SLES 12 SP2 veya RHEL 7.2 gibi eski sürümleri Azure hızlandırılmış ağ desteklemiyor
>


## <a name="deployment-of-host-monitoring"></a>İzleme ana bilgisayarı dağıtımı
Azure sanal Makineler'de SAP uygulamaları üretim kullanımı için SAP, Azure sanal makineleri çalıştıran fiziksel ana bilgisayarlardan verilerin izleme ana bilgisayarı alma olanağı gerektirir. Belirli bir SAP konak Aracısı düzeltme eki düzeyi gereklidir, SAPOSCOL ve SAP konak Aracısı bu yeteneği sağlar. Tam düzeltme eki düzeyi SAP Not belgelenen [1409604].

Ana verileri SAPOSCOL ve SAP konak Aracısı teslim bileşenlerin dağıtımına ilişkin ve bu bileşenlerden birini yaşam döngüsü yönetimi ile ilgili ayrıntılar için başvurmak [Dağıtım Kılavuzu][deployment-guide]


## <a name="next-steps"></a>Sonraki Adımlar
Belirli DBMS hakkında daha fazla bilgi için şu makalelere bakın:

- [SAP iş yükü için SQL Server Azure Sanal Makineler DBMS dağıtımı](dbms_guide_sqlserver.md)
- [SAP iş yükü için Oracle Azure Sanal Makineler DBMS dağıtımı](dbms_guide_oracle.md)
- [SAP iş yükü için IBM DB2 Azure Sanal Makineler DBMS dağıtımı](dbms_guide_ibm.md)
- [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](dbms_guide_sapase.md)
- [SAP maxDB, dinamik önbellek ve azure'da içerik sunucusu dağıtımı](dbms_guide_maxdb.md)
- [Azure işlemlerinde SAP HANA kılavuzu](hana-vm-operations.md)
- [Azure sanal makineler için SAP HANA yüksek kullanılabilirlik](sap-hana-availability-overview.md)
- [SAP HANA için yedekleme Kılavuzu Azure sanal Makineler'de](sap-hana-backup-guide.md)

