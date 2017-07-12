---
title: "VMware'den Azure'a çoğaltma işlemi Azure Site Recovery'de nasıl çalışır? | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hizmeti aracılığıyla şirket içi VMware VM'leri ve fiziksel sunucuları Azure'a çoğaltırken kullanılan bileşenlere ve mimariye yönelik genel bir bakış sağlanmaktadır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-architecture
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 81f02c1277ae8a2c377ca0d6db67ec4211e9aa5e
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017

---



<a id="how-does-vmware-replication-to-azure-work-in-site-recovery" class="xliff"></a>

# VMware'den Azure'a çoğaltma işlemi Site Recovery'de nasıl çalışır?

Bu makalede şirket içi VMware sanal makineleri ve Windows/Linux fiziksel sunucuları [Azure Site Recovery](site-recovery-overview.md) hizmeti kullanılarak Azure'a çoğaltılırken kullanılan bileşenler ve işlemler açıklanmaktadır.

Fiziksel şirket içi sunucuları Azure'a çoğaltma işleminde VMware VM çoğaltmasıyla aynı bileşenler ve işlemler kullanılır ancak şu farklılıklara dikkat etmeniz gerekir:

- Yapılandırma sunucusu için VMware VM yerine fiziksel bir sunucu kullanabilirsiniz.
- Yeniden çalışma için bir yerinde VMware altyapısı gerekir. Bir fiziksel sunucuda yeniden çalışma gerçekleştiremezsiniz.

Tüm yorumlarınızı bu makalenin alt kısmında paylaşabilir veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda soru sorabilirsiniz.


<a id="architectural-components" class="xliff"></a>

## Mimari bileşenler

VMware VM'leri ve fiziksel sunucuları Azure'a çoğaltırken kullanılan çeşitli bileşenler vardır.

**Bileşen** | **Konum** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure’da bir Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır. | Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu** | Yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu da dahil olmak üzere dağıtım için gerekli tüm şirket içi bileşenleri çalıştıran tek bir şirket içi yönetim sunucusu (VMWare VM) | Yapılandırma sunucusu bileşeni, şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
 **İşlem sunucusu**:  | Varsayılan olarak yapılandırma sunucusuna yüklenir. | Çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir.<br/><br/> İşlem sunucusu ayrıca Mobility hizmetinin korunan makinelere göndermeli yüklemesini ele alır ve VMware VM’lerinin otomatik olarak bulunmasını gerçekleştirir.<br/><br/> Dağıtımınız büyüdükçe artan çoğaltma trafiği hacimlerini idare etmek için ilave olarak ayrı ayrı adanmış işlem sunucuları ekleyebilirsiniz.
 **Ana hedef sunucu** | Varsayılan olarak şirket içi yapılandırma sunucusuna yüklenir. | Azure’dan yeniden çalışma sırasında çoğaltma verilerini işler.<br/><br/> Yeniden çalışma trafiğinin hacmi yüksekse, yeniden çalışma için ayrı bir ana hedef sunucu dağıtabilirsiniz.
**VMware sunucuları** | VMware VM’leri vSphere ESXi sunucularında barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu kullanılması önerilir. | VMware sunucularını Kurtarma Hizmetleri kasanıza eklersiniz.
**Çoğaltılan makineler** | Mobility hizmeti, çoğaltmak istediğiniz her VMware VM’ine yüklenir. Her makineye el ile yüklenebileceği gibi, işlem sunucusundan göndererek yükleme yoluyla da yüklenebilir.

[Destek matrisinde](site-recovery-support-matrix-to-azure.md), bu bileşenlerden her birine ilişkin dağıtım önkoşulları ve gereksinimler hakkında bilgi edinin.

**Şekil 1: VMware’den Azure bileşenlerine**

![Bileşenler](./media/site-recovery-components/arch-enhanced.png)

<a id="replication-process" class="xliff"></a>

## Çoğaltma işlemi

1. Azure bileşenleri ve bir Kurtarma Hizmetleri kasası dahil olmak üzere dağıtım işlemini siz ayarlarsınız. Kasada çoğaltma kaynağını ve hedefini belirtir, yapılandırma sunucusunu ayarlar, VMware sunucuları ekler, bir çoğaltma ilkesi oluşturur, Mobility hizmetini dağıtır, çoğaltmayı etkinleştirir ve bir yük devretme testi çalıştırırsınız.
2.  Makineler çoğaltma ilkesine uygun olarak çoğaltılmaya başlanır ve verilerin ilk kopyası Azure depolamaya çoğaltılır.
4. Delta değişikliklerinin Azure’a çoğaltılması ilk çoğaltma işlemi tamamlandıktan sonra başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
    - Çoğaltılan makineler çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasındaki yapılandırma sunucusuyla iletişim kurar.
    - Çoğaltılan makineler çoğaltma verilerini HTTPS 9443 gelen bağlantı noktasındaki (yapılandırılabilir) işlem sunucusuna gönderir.
    - Yapılandırma sunucusu, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma yönetimini düzenler.
    - İşlem sunucusu kaynak makinelerden gelen verileri alır, iyileştirip şifreler ve 443 giden bağlantı noktası üzerinden Azure depolamaya gönderir.
    - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar. Çoklu VM, birden çok makineyi yük devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşan çoğaltma grupları halinde gruplandırdığınızda kullanılır. Bu özellik, makinelerin aynı iş yükünü çalıştırdığı ve tutarlı olmasının gerektiği durumlarda kullanışlıdır.
5. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute [genel eşliğini](../expressroute/expressroute-circuit-peerings.md#public-peering) kullanabilirsiniz. Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.

**Şekil 2: VMware’den Azure’a çoğaltma**

![Gelişmiş](./media/site-recovery-components/v2a-architecture-henry.png)

<a id="failover-and-failback-process" class="xliff"></a>

## Yük devretme ve yeniden çalışma işlemi

1. Yük devretme testinin beklenen şekilde çalıştığını doğruladıktan sonra, gerektiğinde Azure’a planlanmamış yük devretmeler çalıştırabilirsiniz. Planlanan yük devretme desteklenmez.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok VM’in devredilmesini düzenleyebilirsiniz.
3. Yük devretme işlemini çalıştırdığınızda Azure’da çoğaltma sanal makineleri oluşturulur. Kopya Azure VM’sindeki iş yüküne erişmeye başlamak için bir yük devretme yürütürsünüz.
4. Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz. Bir yeniden çalışma altyapısı ayarlarsınız, makineyi ikincil siteden birincil siteye çoğaltmaya başlar ve ikincil siteden planlanmamış bir yük devretme gerçekleştirirsiniz. Bu yük devretme yürütüldükten sonra, veriler şirket içine döner ve Azure’a çoğaltmayı yeniden etkinleştirmeniz gerekir. [Daha fazla bilgi](site-recovery-failback-azure-to-vmware.md)

Birkaç yeniden çalışma gereksinimi vardır:


- **Azure'da geçici işlem sunucusu**: Yük devretmeden sonra Azure'da yeniden çalıştırma işlemini gerçekleştirmek isterseniz Azure'dan çoğaltmayı düzenlemek için işlem sunucusu olarak yapılandırılan bir Azure VM ayarlamanız gerekir. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
- **VPN bağlantısı**: Yeniden çalışma için, Azure ağından şirket içi siteye olacak şekilde ayarlanmış bir VPN bağlantısına (veya Azure ExpressRoute) sahip olmanız gerekir.
- **Ayrı şirket içi ana hedef sunucusu**: Şirket içi ana hedef sunucusu yeniden çalışma işlemini yürütür. Ana hedef sunucusu varsayılan olarak yönetim sunucusunda yüklüdür. Ancak daha fazla hacimli trafikte yeniden çalıştırma işlemini gerçekleştiriyorsanız bu işlem için ayrı bir şirket içi ana hedef sunucusu ayarlamanız gerekir.
- **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Bu ilke çoğaltma ilkenizi oluşturduğunuzda otomatik olarak oluşturulur.
- **VMware altyapısı**: Bir şirket içi VMware VM'ye geri dönmeniz gerekir. Bu, şirket içi fiziksel sunucuları Azure'a çoğaltıyor olsanız bile şirket içi VMware altyapısına ihtiyacınız olduğu anlamına gelir.

**Şekil 3: VMware/fiziksel yeniden çalışma**

![Yeniden çalışma](./media/site-recovery-components/enhanced-failback.png)


<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

[Destek matrisini](site-recovery-support-matrix-to-azure.md) inceleyin

