---
title: "Site Recovery nasıl çalışır? | Microsoft Belgeleri"
description: "Bu makalede Site Recovery mimarisine genel bir bakış sağlanır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: bd8082c46ee36c70e372208d1bd15337acc558a1
ms.openlocfilehash: eb97f66901efa336942dee56d9a8a62ade1f6842


---
# <a name="how-does-azure-site-recovery-work"></a>Azure Site Recovery nasıl çalışır?

Azure Site Recovery hizmetinin temel mimarisini ve bu hizmetin çalışmasını sağlayan bileşenleri anlamak için bu makaleyi okuyun.

Kuruluşlar, planlı ve planlanmayan kesinti süreleri boyunca uygulamaların, iş yüklerinin ve verilerin çalışır durumda, kullanılabilir olmasına yönelik izlenecek yolu belirleyen ve mümkün olan en kısa sürede normal çalışma koşullarına dönmeyi sağlayan BCDR stratejisine gereksinim duyar. BCDR stratejinizin işletme verilerini güvende tutması, kurtarılabilir şekilde saklaması ve bir olağanüstü durum sırasında iş yüklerinin sürekli olarak kullanılabilir kalmasını sağlaması gerekir.

Site Recovery, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure) veya ikincil bir veri merkezine çoğaltılmasını düzenleyerek BCDR stratejinize katkı sağlayan bir Azure hizmetidir. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz. [Site Recovery nedir?](site-recovery-overview.md) bölümünden daha fazla bilgi edinebilirsiniz.

Bu makale, [Azure portalında](https://portal.azure.com) dağıtımı açıklamaktadır. Var olan Site Recovery kasalarının bakımını yapmak için [klasik Azure portalı](https://manage.windowsazure.com/) kullanılabilir, ancak buradan yeni kasa oluşturamazsınız.

Yorumlarınızı bu makalenin altında paylaşabilirsiniz. Teknik sorular için [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nu kullanın.


## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Site Recovery, birçok senaryoda çoğaltmayı düzenlemek için dağıtılabilir:

- **VMware sanal makinelerini çoğaltma**: Şirket içi VMware sanal makinelerini Azure'a veya ikincil veri merkezine çoğaltabilirsiniz.
- **Fiziksel makineleri çoğaltma**: Fiziksel makineleri (Windows veya Linux) Azure’a veya ikincil veri merkezine çoğaltabilirsiniz. Fiziksel makineleri çoğaltma işlemi VMware VM’lerini çoğaltma işlemiyle neredeyse aynıdır.
- **Hyper-V VM’lerini çoğaltma**: Hyper-V VM’lerini Azure’a veya ikincil bir VMM sitesine çoğaltabilirsiniz. VM’leri ikincil bir siteye çoğaltmak istiyorsanız VM’lerin System Center Virtual Machine Manager (VMM) bulutlarında yönetilmesi gerekir.
- **VM’leri geçirme**: Şirket içi VM’leri ve fiziksel sunucuları (çoğaltma, yük devretme, yeniden başlatma) Azure’a çoğaltmaya ek olarak Azure VM’lerine de geçirebilirsiniz (çoğaltma, yük devretme, yeniden başlatma). Geçiş için şunları yapmanız gerekir:
    - Şirket içi Hyper-V VM’leri, VMware VM’leri ve fiziksel sunucular üzerinde çalışan iş yüklerinin Azure VM’leri üzerinde çalışacak şekilde geçişini yapabilirsiniz.
    - Bir Azure bölgesinden diğerine [Azure IaaS VM’lerini](site-recovery-migrate-azure-to-azure.md) geçirebilirsiniz. Şu anda bu senaryoda yalnızca geçiş desteklenmektedir ve yeniden çalışma desteği yoktur.
    - [AWS Windows örneklerini](site-recovery-migrate-aws-to-azure.md) Azure IaaS VM’lerine geçirebilirsiniz. Şu anda bu senaryoda yalnızca geçiş desteklenmektedir ve yeniden çalışma desteği yoktur.

Site Recovery, desteklenen VM’lerde ve fiziksel sunucularda çalışan uygulamaları çoğaltır. [Azure Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) bölümünden desteklenen uygulamaların tam özetine ulaşabilirsiniz.

## <a name="replicate-vmware-vmsphysical-servers-to-azure"></a>VMware VM’lerini/fiziksel sunucuları Azure’a çoğaltma

### <a name="components"></a>Bileşenler

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | Azure’da bir Microsoft Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır.<br/><br/> Depolama ve ağ, Kaynak Yöneticisi hesapları veya klasik hesaplar olabilir.<br/><br/>  Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu** | Şirket içi siteyle Azure arasındaki iletişimleri düzenlemek ve veri çoğaltmayı yönetmek için bir şirket içi yapılandırma sunucusu ayarlarsınız.
**İşlem sunucusu** | Varsayılan olarak şirket içi yapılandırma sunucusuna yüklenir.<br/><br/> Çoğaltma ağ geçidi olarak davranır. Korumalı kaynak makinelerden çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir.<br/><br/> Mobility hizmetinden korunan makinelere göndererek yükleme işlemini yapar ve VMware VM’lerinin otomatik olarak bulunmasını gerçekleştirir.<br/><br/> Dağıtımınız büyüdükçe artan çoğaltma trafiği hacimlerini idare etmek için ilave olarak ayrı ayrı adanmış işlem sunucuları ekleyebilirsiniz.
**Ana hedef sunucu** | Varsayılan olarak şirket içi yapılandırma sunucusuna yüklenir.<br/><br/> Azure’dan yeniden çalışma sırasında çoğaltma verilerini işler. Yeniden çalışma trafiğinin hacmi yüksekse, yeniden çalışma için ayrı bir ana hedef sunucu dağıtabilirsiniz.
**VMware sunucuları** | VMware VM’lerini çoğaltmak için Kurtarma Hizmetleri kasanıza vCenter ve vSphere sunucuları eklersiniz.<br/><br/> Fiziksel sunucuları çoğaltıyorsanız, yeniden çalışma için bir şirket içi VMware altyapısı gerekir. Bir fiziksel sunucuda yeniden çalışma gerçekleştiremezsiniz.
**Çoğaltılan makineler** | Çoğaltmak istediğiniz her makinede Mobility hizmeti yüklü olmalıdır. Her makineye el ile yüklenebileceği gibi, işlem sunucusundan göndererek yükleme yoluyla da yüklenebilir.

**Şekil 1: VMware’den Azure bileşenlerine**

![Bileşenler](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Çoğaltma işlemi

1. Azure bileşenleri ve bir Kurtarma Hizmetleri kasası dahil olmak üzere dağıtım işlemini siz ayarlarsınız. Kasada çoğaltma kaynağını ve hedefini belirtir, yapılandırma sunucusunu ayarlar, VMware sunucuları ekler, bir çoğaltma ilkesi oluşturur, Mobility hizmetini dağıtır, çoğaltmayı etkinleştirir ve bir yük devretme testi çalıştırırsınız.
2.  Makineler çoğaltma ilkesine uygun olarak çoğaltılmaya başlanır ve verilerin ilk kopyası Azure depolamaya çoğaltılır.
4. Delta değişikliklerinin Azure’a çoğaltılması ilk çoğaltma işlemi tamamlandıktan sonra başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
    - Çoğaltılan makineler çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasındaki yapılandırma sunucusuyla iletişim kurar.
    - Çoğaltılan makineler çoğaltma verilerini HTTPS 9443 gelen bağlantı noktasındaki (yapılandırılabilir) işlem sunucusuna gönderir.
    - Yapılandırma sunucusu, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltma yönetimini düzenler.
    - İşlem sunucusu kaynak makinelerden gelen verileri alır, iyileştirip şifreler ve 443 giden bağlantı noktası üzerinden Azure depolamaya gönderir.
    - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 bağlantı noktası üzerinden iletişim kurar. Çoklu VM, birden çok makineyi yük devrettikleri zaman kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaşan çoğaltma grupları halinde gruplandırdığınızda kullanılır. Bu özellik, makinelerin aynı iş yükünü çalıştırdığı ve tutarlı olmasının gerektiği durumlarda kullanışlıdır.
5. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute [genel eşliğini](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) kullanabilirsiniz. Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.

**Şekil 2: VMware’den Azure’a çoğaltma**

![Gelişmiş](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi VMware VM’lerinden ve fiziksel sunuculardan Azure’a planlanmamış yük devretme işlemleri çalıştırırsınız. Planlanan yük devretme desteklenmez.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok makinenin devredilmesini düzenleyebilirsiniz.
3. Yük devretme işlemini çalıştırdığınızda Azure’da kopya VM’ler oluşturulur. Kopya Azure VM’sindeki iş yüküne erişmeye başlamak için bir yük devretme yürütürsünüz.
4. Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz. Bir yeniden çalışma altyapısı ayarlarsınız, makineyi ikincil siteden birincil siteye çoğaltmaya başlar ve ikincil siteden planlanmamış bir yük devretme gerçekleştirirsiniz. Bu yük devretme yürütüldükten sonra, veriler şirket içine döner ve Azure’a çoğaltmayı yeniden etkinleştirmeniz gerekir. [Daha fazla bilgi](site-recovery-failback-azure-to-vmware.md)

Birkaç yeniden çalışma gereksinimi vardır:

- **Fiziksel-fiziksel yeniden çalışma desteklenmez**: Fiziksel sunucuların yükünü Azure’a devredip sonra yeniden çalışmak isterseniz bir VMware sanal makinesinde yeniden çalışmanız gerekir. Bir fiziksel sunucuda yeniden çalışamazsınız. Yeniden çalışmak için bir Azure sanal makinesi gereklidir ve yapılandırma sunucusunu bir VMware sanal makinesi olarak dağıtmadıysanız VMware sanal makinesi olarak ayrı bir ana hedef sunucu ayarlamanız gerekir. Ana hedef sunucu diskleri bir VMware sanal makinesine geri yüklemek için VMware depolama alanı ile etkileşim kurar ve bağlanır.
- **Azure'da geçici işlem sunucusu**: Yük devretmeden sonra Azure'da yeniden çalıştırma işlemini gerçekleştirmek isterseniz Azure'dan çoğaltmayı düzenlemek için işlem sunucusu olarak yapılandırılan bir Azure VM ayarlamanız gerekir. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
- **VPN bağlantısı**: Yeniden çalışma için, Azure ağından şirket içi siteye olacak şekilde ayarlanmış bir VPN bağlantısına (veya Azure ExpressRoute) sahip olmanız gerekir.
- **Ayrı şirket içi ana hedef sunucusu**: Şirket içi ana hedef sunucusu yeniden çalışma işlemini yürütür. Ana hedef sunucusu varsayılan olarak yönetim sunucusunda yüklüdür. Ancak daha fazla hacimli trafikte yeniden çalıştırma işlemini gerçekleştiriyorsanız bu işlem için ayrı bir şirket içi ana hedef sunucusu ayarlamanız gerekir.
- **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Bu ilke çoğaltma ilkenizi oluşturduğunuzda otomatik olarak oluşturulur.

**Şekil 3: VMware/fiziksel yeniden çalışma**

![Yeniden çalışma](./media/site-recovery-components/enhanced-failback.png)


## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>VMware VM’lerini/Fiziksel sunucuları ikincil bir siteye çoğaltma

### <a name="components"></a>Bileşenler

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | Bu senaryoyu, InMage Scout kullanarak dağıtırsınız. Bunu elde etmek için bir Azure aboneliğine sahip olmanız gerekir.<br/><br/> Bir Kurtarma Hizmetleri kasası oluşturduktan sonra InMage Scout hizmetini indirip dağıtımı ayarlamak üzere en son güncelleştirmeleri yüklersiniz.
**İşlem sunucusu** | Önbelleğe alma, sıkıştırma ve veri iyileştirme işlemlerini yürütmek için birincil sitenizde işlem sunucusu bileşenini dağıtırsınız.<br/><br/> Ayrıca bu sunucu, Birleşik Aracı'nın korumak istediğiniz makinelere göndermeli yükleme işlemini yürütür.
**VMware ESX/ESXi ve vCenter sunucusu** |  VMware VM’lerini çoğaltmak için bir VMware altyapınız olmalıdır.
**VM’ler/fiziksel sunucular** |  Çoğaltmak istediğiniz VMware VM’leri veya Windows/Linux fiziksel sunucularına Birleşik Aracı’yı yüklersiniz.<br/><br/> Aracı,tüm bileşenler arasındaki bir iletişim sağlayıcısı gibi davranır.
**Yapılandırma sunucusu** | Yapılandırma sunucusu, yönetim Web sitesini veya vContinuum konsolunu kullanarak dağıtımınızı yönetme, yapılandırma ve izleme işlemleri için ikincil siteye yüklenir.
**vContinuum sunucusu** | Yapılandırma sunucusuyla aynı konuma yüklenir.<br/><br/> Korunan ortamınızı yönetmeye ve izlemeye yönelik bir konsol sağlar.
**Ana hedef sunucu (ikincil site)** | Ana hedef sunucu çoğaltılan verileri tutar. İşlem sunucusundan verileri alır, ikincil sitede çoğaltılan bir makine oluşturur ve veri bekletme noktalarını tutar.<br/><br/> İhtiyacınız olan ana hedef sunucusu sayısı koruduğunuz makine sayısına bağlıdır.<br/><br/> Birincil sitede yeniden çalıştırmak isterseniz burada da bir ana hedef sunucusuna sahip olmanız gerekir. Birleşik Aracı bu sunucuya yüklenir.

### <a name="replication-process"></a>Çoğaltma işlemi

1. Her sitede bileşen sunucularını (yapılandırma, işlem, ana hedef) ayarlayıp çoğaltmak istediğiniz makinelere Birleşik Aracı'yı yükleyin.
2. İlk çoğaltmanın ardından makinelerdeki aracılar çoğaltma değişimleri işlem sunucusuna gönderir.
3. İşlem sunucusu verileri iyileştirir ve ikincil sitedeki ana hedef sunucusuna aktarır. Yapılandırma sunucusu çoğaltma sürecini yönetir.

**Şekil 4: VMware’den VMware’e çoğaltma**

![VMware'den VMware'e](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="replicate-hyper-v-vms-to-azure"></a>Hyper-V VM'lerini Azure'a çoğaltma


### <a name="components"></a>Bileşenler

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | Azure’da bir Microsoft Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır.<br/><br/> Depolama ve ağ, Kaynak Yöneticisi tabanlı veya klasik hesaplar olabilir.<br/><br/> Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | Hyper-V konaklarınız VMM bulutlarında yer alıyorsa [ağ eşlemesi](site-recovery-network-mapping.md) yapılandırmak üzere ayar yapmak için mantıksal ve VM ağlarına ihtiyacınız olur. VM ağının da bulutla ilişkilendirilen bir mantıksal ağ ile bağlantılı olması gerekir.
**Hyper-V konağı** | Bir veya daha fazla Hyper-V konak sunucunuz olmalıdır.
**Hyper-V VM’leri** | Hyper-V konak sunucusunda bir veya daha fazla VM’niz olmalıdır. Hyper-V konağında çalışan Sağlayıcı, İnternet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Aracı, veri çoğaltma işlemine ait verileri HTTPS 443 üzerinden işler. Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.


## <a name="replication-process"></a>Çoğaltma işlemi

1. Azure bileşenlerini ayarlarsınız. Site Recovery dağıtımına başlamadan önce depolama ve ağ hesapları ayarlamanızı öneririz.
2. Site Recovery kurtarma için bir Kurtarma Hizmetleri kasası ayarlarsınız ve aşağıdakileri de içeren kasa ayarlarını yapılandırırsınız:
    - Hyper-V konaklarını bir VMM bulutunda yönetmiyorsanız, bir Hyper-V site kapsayıcısı oluşturur ve Hyper-V konaklarını buna eklersiniz.
    - Çoğaltma kaynağı ve hedefi. Hyper-V konaklarınız VMM’de yönetiliyorsa kaynak VMM bulutudur. Aksi takdirde, kaynak Hyper-V sitesidir.
    - Azure Site Recovery Sağlayıcısı ve Microsoft Azure Kurtarma Hizmetleri aracısının yüklenmesi. VMM’niz varsa Sağlayıcı VMM’ye, aracı ise her bir Hyper-V konağına yüklenir. VMM’niz yoksa her bir konağa hem Sağlayıcı hem de aracı yüklenir.
    - Hyper-V sitesi veya VMM bulutu için bir çoğaltma ilkesi oluşturursunuz. İlke, site veya buluttaki tüm konaklarda yer alan tüm VM’lere uygulanır.
    - Hyper-V VM’leri için çoğaltmayı etkinleştirirsiniz. İlk çoğaltma, çoğaltma ilkesi ayarlarına uygun şekilde gerçekleştirilir.
4. Veri değişiklikleri izlenir ve delta değişikliklerinin Azure’a çoğaltılması ilk çoğaltma işlemi tamamlandıktan sonra başlar. Bir öğe için izlenen değişiklikler bir .hrl dosyasında saklanır.
5. Her şeyin çalıştığından emin olmak için bir Yük devretme testi çalıştırırsınız.

### <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi Hyper-V VM’lerinden Azure’a planlanmış veya planlanmamış bir [yük devretme](site-recovery-failover.md) gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok makinenin devredilmesini düzenleyebilirsiniz.
4. Yük devretmeyi çalıştırdıktan sonra, oluşturulan kopya VM’leri Azure’da görebiliyor olmanız gerekir. Gerekli olursa VM’ye genel bir IP adresi atayabilirsiniz.
5. Daha sonra, kopya Azure VM’sindeki iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
6. Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz. Azure’dan birincil siteye planlanmış yük devretme işlemi başlatırsınız. Planlanmış bir yük devretme gerçekleştirmek için aynı VM’de ya da alternatif bir konumda yeniden çalışmayı seçebilir ve Azure ile şirket içi arasında değişiklikleri eşitleyerek veri kaybı olmamasını sağlayabilirsiniz. VM’ler şirket içinde oluşturulduğunda yük devretmeyi yürütürsünüz.

**Şekil 5: Hyper-V sitesinden Azure’a çoğaltma**

![Bileşenler](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Şekil 6: VMM bulutlarındaki Hyper-V’den Azure’a çoğaltma**

![Bileşenler](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)



## <a name="replicate-hyper-v-vms-to-a-secondary-site"></a>Hyper-V VM'lerini ikincil bir siteye çoğaltma

### <a name="components"></a>Bileşenler

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure hesabı** | Bir Microsoft Azure hesabınızın olması gerekir.
**VMM sunucusu** | Bir tane birincil sitede, bir tane de İnternet’e bağlı ikincil sitede VMM sunucusu olması önerilir.<br/><br/> Her sunucunun Hyper-V yetenek profili kümesi içeren en az bir VMM özel bulutu olmalıdır.<br/><br/> Azure Site Recovery Sağlayıcısını VMM sunucusuna yüklemeniz gerekir. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Sağlayıcı ile Azure arasındaki iletişimler güvenli ve şifrelidir.
**Hyper-V sunucusu** |  Birincil ve ikincil VMM bulutlarında bir veya daha fazla Hyper-V konak sunucusuna sahip olmanız gerekir. Sunucular İnternet’e bağlı olmalıdır.<br/><br/> Verilerin, Kerberos veya sertifika kimlik doğrulaması kullanılarak, LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılması gerekir.  
**Kaynak makineler** | Kaynak Hyper-V konak sunucusunda en az bir tane çoğaltmak istediğiniz VM olması gerekir.

## <a name="replication-process"></a>Çoğaltma işlemi

1. Azure hesabını ayarlarsınız.
2. Site Recovery kurtarma için bir Kurtarma Hizmetleri kasası ayarlarsınız ve aşağıdakileri de içeren kasa ayarlarını yapılandırırsınız:

    - Çoğaltma kaynağı ve hedefi (birincil ve ikincil siteler).
    - Azure Site Recovery Sağlayıcısı ve Microsoft Azure Kurtarma Hizmetleri aracısının yüklenmesi. Sağlayıcı VMM sunucularına, aracı ise her bir Hyper-V konağına yüklenir.
    - Kaynak VMM bulutu için bir çoğaltma ilkesi oluşturursunuz. İlke, buluttaki tüm konaklarda yer alan tüm VM’lere uygulanır.
    - Hyper-V VM’leri için çoğaltmayı etkinleştirirsiniz. İlk çoğaltma, çoğaltma ilkesi ayarlarına uygun şekilde gerçekleştirilir.
4. Veri değişiklikleri izlenir ve delta değişikliklerinin çoğaltılması ilk çoğaltma işlemi tamamlandıktan sonra başlar. Bir öğe için izlenen değişiklikler bir .hrl dosyasında saklanır.
5. Her şeyin çalıştığından emin olmak için bir Yük devretme testi çalıştırırsınız.

**Şekil 7: VMM’den VMM’ye çoğaltma**

![Şirket içinden şirket içine](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi siteler arasında planlanmış veya planlanmamış bir [yük devretme](site-recovery-failover.md) gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok makinenin devredilmesini düzenleyebilirsiniz.
4. İkincil bir siteye yönelik planlanmamış bir yük devretme gerçekleştirirseniz, işlem tamamlandıktan sonra ikincil konumdaki makineler koruma veya çoğaltma için etkinleştirilmez. Planlı bir yük devretme gerçekleştirdiyseniz, işlemden sonra ikincil konumdaki makineler korunur.
5. Daha sonra, kopya VM’deki iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
6. Birincil sitenizi yeniden kullanılabilir duruma geldiğinde ters çoğaltma başlatarak ikincil siteden birincil siteye çoğaltma gerçekleştirirsiniz. Ters çoğaltma sanal makineleri korumalı bir duruma getirir, ancak ikincil veri merkezi hala etkin konumdur.
7. Birincil siteyi yeniden etkin konum durumuna getirmek için ikincil siteden birincil siteye planlı yük devretme başlatır ve arkasından başka bir ters çoğaltma gerçekleştirirsiniz.


### <a name="hyper-v-replication-workflow"></a>Hyper-V çoğaltma iş akışı

**İş akışı aşaması** | **Eylem**
--- | ---
1. **Korumayı etkinleştir** | Bir Hyper-V VM için korumayı etkinleştirdiğinizde, makinenin önkoşullarla uyumlu olup olmadığının denetlenmesi için **Korumayı Etkinleştir** işi başlatılır. İş tarafından iki yöntem çağırılır:<br/><br/> Yapılandırdığınız ayarlarla çoğaltmanın ayarlanması için [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx).<br/><br/> Tam VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx).
2. **İlk çoğaltma** |  Bir sanal makine anlık görüntüsü alınır ve sanal sabit diskler tamamı ikincil konuma kopyalanana kadar tek tek çoğaltılır.<br/><br/> Bu işlemi tamamlamak için gereken süre VM boyutuna, ağ bant genişliğine ve iş çoğaltma yöntemine bağlıdır.<br/><br/> İlk çoğaltma sırasında disk değişimi meydana gelirse Hyper-V Çoğaltma'nın Çoğaltma İzleyicisi bu değişiklikleri, disklerle aynı klasörde yer alan Hyper-V Çoğaltma Günlükleri (.hrl) olarak izler.<br/><br/> Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır.<br/><br/> İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır. İlk çoğaltma tamamlandığında VM anlık görüntüsü silinir, günlükteki değişim diski değişiklikleri eşitlenir ve birleştirilir.
3. **Korumayı sonlandırma** | İlk çoğaltma sonlandırıldıktan sonra **Korumayı sonlandır** işi, ağ ayarlarını ve diğer çoğaltma sonrası ayarları yapılandırır ve sanal makine korunur.<br/><br/> Azure'da çoğaltma yapıyorsanız, sanal makinede ince ayarlar yaparak sanal makineyi yük devretme için hazır hale getirmeniz gerekebilir.<br/><br/> Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.
4. **Çoğaltma** | İlk çoğaltma sonrasında, çoğaltma ayarlarına uygun olarak değişim eşitlemesi başlar.<br/><br/> **Çoğaltma hatası**: Değişim çoğaltması başarısız olursa ve tam çoğaltma bant genişliği ve zaman konusunda fazla kaynak tüketimine yol açarsa yeniden eşitleme meydana gelir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir. Yeniden eşitleme, kaynak ve hedef sanal makinelerin sağlama toplamlarını hesaplar ve yalnızca değişim verilerini gönderir. Böylece gönderilen veri miktarını azaltır. Yeniden eşitleme bittikten sonra değişim çoğaltması devam eder. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz.<br/><br/> **Çoğaltma hatası**: Bir çoğaltma hatası meydana gelmesi durumunda yerleşik olarak yeniden deneme işlevi bulunur. Kimlik doğrulama veya yetkilendirme ya da çoğaltılan makinenin geçersiz durumda olması gibi kurtarılamaz bir hata olursa yeniden deneme işlevi uygulanmaz. Ağ hatası veya düşük disk alanı/belleği gibi kurtarılabilir bir hata oluşursa artan aralıklarla yeniden denemeler meydana gelir (her 1, 2, 4, 8, 10 ve ardından 30 dakikada bir şeklinde).
5. **Planlanan/planlanmamış yük devretme** | Gerektiğinde planlanan veya planlanmamış yük devretme işlemleri çalıştırabilirsiniz.<br/><br/> Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.<br/><br/> Oluşturulan çoğaltma VM’leri, yürütme bekleniyor durumuna geçirilir. Yük devretmeyi tamamlamak için işlemleri yürütmeniz gerekir.<br/><br/> Birincil site çalışır duruma geldikten sonra, site kullanılabilir hale geldiğinde buraya yeniden çalışma gerçekleştirebilirsiniz.


**Şekil 8: Hyper-V iş akışı**

![iş akışı](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Sonraki adımlar

[Dağıtım için hazırlanma](site-recovery-best-practices.md)



<!--HONumber=Jan17_HO1-->


