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
ms.date: 02/21/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 22b50dd6242e8c10241b0626b48f8ef842b6b0fd
ms.openlocfilehash: c33ca9e5292096a0fd96d98da3e89d721463e903
ms.lasthandoff: 03/02/2017


---
# <a name="how-does-azure-site-recovery-work"></a>Azure Site Recovery nasıl çalışır?

[Azure Site Recovery](site-recovery-overview.md) hizmetinin temel mimarisini ve bu hizmetin çalışmasını sağlayan bileşenleri anlamak için bu makaleyi okuyun.

Tüm yorumlarınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.


## <a name="replication-to-azure"></a>Azure’a çoğaltma

Azure’a aşağıdakileri çoğaltabilirsiniz:
- **VMware**: [Desteklenen bir ana bilgisayarda](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) çalışan yerinde VMware sanal makineleri. [Desteklenen işletim sistemlerini](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) çalıştıran VMware sanal makinelerini çoğaltabilirsiniz
- **Hyper-V**: [Desteklenen bir ana bilgisayarda](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) çalışan yerinde Hyper-V sanal makineleri.
- **Fiziksel makineler**: [Desteklenen işletim sistemlerinde](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) Windows veya Linux çalıştıran yerinde fiziksel sunucular. [Hyper-V ve Azure tarafından desteklenen](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) herhangi bir konuk işletim sistemini çalıştıran Hyper-V sanal makinelerini çoğaltabilirsiniz.

## <a name="vmware-replication-to-azure"></a>Azure’a VMware çoğaltma

Alan | Bileşen | Ayrıntılar
--- | --- | ---
**Azure** | Azure’da bir Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır. | Depolama ve ağ, Kaynak Yöneticisi hesapları veya klasik hesaplar olabilir.<br/><br/>  Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur. Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Yapılandırma sunucusu** | Tek bir yönetim sunucusu (VMWare VM), tüm yerinde bileşenlerde (yapılandırma sunucusu, işlem sunucusu, ana hedef sunucusu) çalışır | Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
 **İşlem sunucusu**:  | Varsayılan olarak yapılandırma sunucusuna yüklenir. | Çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir.<br/><br/> İşlem sunucusu ayrıca Mobility hizmetinin korunan makinelere göndermeli yüklemesini ele alır ve VMware VM’lerinin otomatik olarak bulunmasını gerçekleştirir.<br/><br/> Dağıtımınız büyüdükçe artan çoğaltma trafiği hacimlerini idare etmek için ilave olarak ayrı ayrı adanmış işlem sunucuları ekleyebilirsiniz.
 **Ana hedef sunucu** | Varsayılan olarak şirket içi yapılandırma sunucusuna yüklenir. | Azure’dan yeniden çalışma sırasında çoğaltma verilerini işler.<br/><br/> Yeniden çalışma trafiğinin hacmi yüksekse, yeniden çalışma için ayrı bir ana hedef sunucu dağıtabilirsiniz.
**VMware sunucuları** | VMware VM’leri vSphere ESXi sunucularında barındırılır. Ana bilgisayarları yönetmek için bir vCenter sunucusu kullanılması önerilir. | VMware sunucularını Kurtarma Hizmetleri kasanıza eklersiniz.<br/><br/> I
**Çoğaltılan makineler** | Mobility hizmeti, çoğaltmak istediğiniz her VMware VM’ine yüklenir. Her makineye el ile yüklenebileceği gibi, işlem sunucusundan göndererek yükleme yoluyla da yüklenebilir.

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

### <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

1. Yük devretme testinin beklenen şekilde çalıştığını doğruladıktan sonra, gerektiğinde Azure’a planlanmamış yük devretmeler çalıştırabilirsiniz. Planlanan yük devretme desteklenmez.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok VM’in devredilmesini düzenleyebilirsiniz.
3. Yük devretme işlemini çalıştırdığınızda Azure’da çoğaltma sanal makineleri oluşturulur. Kopya Azure VM’sindeki iş yüküne erişmeye başlamak için bir yük devretme yürütürsünüz.
4. Birincil şirket içi siteniz yeniden kullanılabilir olduğunda siteyi yeniden çalıştırabilirsiniz. Bir yeniden çalışma altyapısı ayarlarsınız, makineyi ikincil siteden birincil siteye çoğaltmaya başlar ve ikincil siteden planlanmamış bir yük devretme gerçekleştirirsiniz. Bu yük devretme yürütüldükten sonra, veriler şirket içine döner ve Azure’a çoğaltmayı yeniden etkinleştirmeniz gerekir. [Daha fazla bilgi](site-recovery-failback-azure-to-vmware.md)

Birkaç yeniden çalışma gereksinimi vardır:


- **Azure'da geçici işlem sunucusu**: Yük devretmeden sonra Azure'da yeniden çalıştırma işlemini gerçekleştirmek isterseniz Azure'dan çoğaltmayı düzenlemek için işlem sunucusu olarak yapılandırılan bir Azure VM ayarlamanız gerekir. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
- **VPN bağlantısı**: Yeniden çalışma için, Azure ağından şirket içi siteye olacak şekilde ayarlanmış bir VPN bağlantısına (veya Azure ExpressRoute) sahip olmanız gerekir.
- **Ayrı şirket içi ana hedef sunucusu**: Şirket içi ana hedef sunucusu yeniden çalışma işlemini yürütür. Ana hedef sunucusu varsayılan olarak yönetim sunucusunda yüklüdür. Ancak daha fazla hacimli trafikte yeniden çalıştırma işlemini gerçekleştiriyorsanız bu işlem için ayrı bir şirket içi ana hedef sunucusu ayarlamanız gerekir.
- **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Bu ilke çoğaltma ilkenizi oluşturduğunuzda otomatik olarak oluşturulur.

**Şekil 3: VMware/fiziksel yeniden çalışma**

![Yeniden çalışma](./media/site-recovery-components/enhanced-failback.png)

## <a name="physical-server-replication-to-azure"></a>Azure’a fiziksel sunucu çoğaltması

Bu çoğaltma senaryosu da [VMware’den Azure’a](#vmware-replication-to-azure) ile aynı bileşenleri ve işlemi kullanır ancak aşağıdaki farklılıkları göz önünde bulundurmanız gerekir:

- Yapılandırma sunucusu için bir VMware VM’i yerine fiziksel bir sunucu kullanabilirsiniz
- Yeniden çalışma için bir yerinde VMware altyapısı gerekir. Bir fiziksel sunucuda yeniden çalışma gerçekleştiremezsiniz.

## <a name="hyper-v-replication-to-azure"></a>Azure'a Hyper-V çoğaltması

**Alan** | **Bileşen** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure’da bir Microsoft Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır. | Depolama ve ağ, Kaynak Yöneticisi tabanlı veya klasik hesaplar olabilir.<br/><br/> Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | VMM bulutlarında bulunan Hyper-V ana bilgisayarları | Hyper-V ana bilgisayarları VMM bulutlarında yönetiliyorsa, VMM sunucusunu Kurtarma Hizmetleri kasasına kaydedin.<br/><br/> VMM sunucusuna, Azure ile çoğaltmayı düzenlemek için Site Recovery Sağlayıcısı’nı yükleyin.<br/><br/> Ağ eşlemeyi yapılandırmak için, mantıksal ağlar ve VM ağları kurulumu gerekir. VM ağının da bulutla ilişkilendirilen bir mantıksal ağ ile bağlantılı olması gerekir.
**Hyper-V konağı** | Hyper-V sunucuları VMM sunucusu ile veya sunucu olmadan dağıtılabilir. | VMM sunucusu yoksa, İnternet’ten Site Recovery ile çoğaltmayı düzenlemek için ana bilgisayara Site Recovery Sağlayıcısı yüklenir. Bir VMM sunucusu varsa, Sağlayıcı ana bilgisayara değil bu sunucuya yüklenir.<br/><br/> Kurtarma Hizmetleri aracısı, veri çoğaltmayı işlemek için ana bilgisayara yüklenir.<br/><br/> Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.
**Hyper-V VM’leri** | Hyper-V konak sunucusunda bir veya daha fazla VM’niz olmalıdır. | Sanal makinelere açıkça bir şey yüklenmesi gerekmez


### <a name="replication-process"></a>Çoğaltma işlemi

1. Azure bileşenlerini ayarlarsınız. Site Recovery dağıtımına başlamadan önce depolama ve ağ hesapları ayarlamanızı öneririz.
2. Site Recovery kurtarma için bir Kurtarma Hizmetleri kasası ayarlarsınız ve aşağıdakileri de içeren kasa ayarlarını yapılandırırsınız:
    - Kaynak ve hedef ayarları. Hyper-V konaklarını bir VMM bulutunda yönetmiyorsanız, hedef için bir Hyper-V site kapsayıcısı oluşturur ve Hyper-V konaklarını buna eklersiniz. Hyper-V konakları VMM’de yönetiliyorsa kaynak VMM bulutudur. Hedef, Azure’dur.
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
6. Birincil yerinde siteniz yeniden kullanılabilir olduğunda siteyi [yeniden çalıştırabilirsiniz](site-recovery-failback-from-azure-to-hyper-v.md). Azure’dan birincil siteye planlanmış yük devretme işlemi başlatırsınız. Planlanmış bir yük devretme gerçekleştirmek için aynı VM’de ya da alternatif bir konumda yeniden çalışmayı seçebilir ve Azure ile şirket içi arasında değişiklikleri eşitleyerek veri kaybı olmamasını sağlayabilirsiniz. VM’ler şirket içinde oluşturulduğunda yük devretmeyi yürütürsünüz.

**Şekil 4: Hyper-V sitesinden Azure’a çoğaltma**

![Bileşenler](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Şekil 5: VMM bulutlarındaki Hyper-V’den Azure’a çoğaltma**

![Bileşenler](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-to-a-secondary-site"></a>İkincil bir siteye çoğaltma

İkincil sitenize aşağıdakileri çoğaltabilirsiniz:

- **VMware**: [Desteklenen bir ana bilgisayarda](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) çalışan yerinde VMware sanal makineleri. [Desteklenen işletim sistemlerini](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions) çalıştıran VMware sanal makinelerini çoğaltabilirsiniz
- **Fiziksel makineler**: [Desteklenen işletim sistemlerinde](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions) Windows veya Linux çalıştıran yerinde fiziksel sunucular.
- **Hyper-V**: VMM bulutlarında yönetilen [desteklenen Hyper-V ana bilgisayarlarında](site-recovery-support-matrix-to-sec-site.md#on-premises-servers) çalışan yerinde Hyper-V sanal makineleri. [desteklenen ana bilgisayarlar](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers). [Hyper-V ve Azure tarafından desteklenen](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) herhangi bir konuk işletim sistemini çalıştıran Hyper-V sanal makinelerini çoğaltabilirsiniz.


## <a name="vmware-vmphysical-server-replication-to-a-secondary-site"></a>VMware VM’lerini/Fiziksel sunucuları ikincil bir siteye çoğaltma

### <a name="components"></a>Bileşenler

**Alan** | **Bileşen** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bu senaryoyu, InMage Scout kullanarak dağıtırsınız. | InMage Scout elde etmek için bir Azure aboneliğine sahip olmanız gerekir.<br/><br/> Bir Kurtarma Hizmetleri kasası oluşturduktan sonra InMage Scout hizmetini indirip dağıtımı ayarlamak üzere en son güncelleştirmeleri yüklersiniz.
**İşlem sunucusu** | Birincil sitede bulunur | Önbelleğe alma, sıkıştırma ve veri iyileştirme işlemlerini yürütmek için işlem sunucusunu dağıtırsınız.<br/><br/> Ayrıca bu sunucu, Birleşik Aracı'nın korumak istediğiniz makinelere göndermeli yükleme işlemini yürütür.
**Yapılandırma sunucusu** | İkincil sitede bulunur | Yapılandırma sunucusu, yönetim Web sitesini veya vContinuum konsolunu kullanarak dağıtımınızı yönetir, yapılandırır ve izler.
**vContinuum sunucusu** | İsteğe bağlı. Yapılandırma sunucusuyla aynı konuma yüklenir. | Korunan ortamınızı yönetmeye ve izlemeye yönelik bir konsol sağlar.
**Ana hedef sunucu** | İkincil sitede bulunur | Ana hedef sunucu çoğaltılan verileri tutar. İşlem sunucusundan verileri alır, ikincil sitede çoğaltılan bir makine oluşturur ve veri bekletme noktalarını tutar.<br/><br/> İhtiyacınız olan ana hedef sunucusu sayısı koruduğunuz makine sayısına bağlıdır.<br/><br/> Birincil sitede yeniden çalıştırmak isterseniz burada da bir ana hedef sunucusuna sahip olmanız gerekir. Birleşik Aracı bu sunucuya yüklenir.
**VMware ESX/ESXi ve vCenter sunucusu** |  VM’ler ESX/ESXi ana bilgisayarlarında barındırılır. Ana bilgisayarlar bir vCenter sunucusu ile yönetilir | VMware VM’lerini çoğaltmak için bir VMware altyapınız olmalıdır.
**VM’ler/fiziksel sunucular** |  Çoğaltmak istediğiniz VMware VM’leri veya fiziksel sunucularda yüklü Birleşik Aracı. | Aracı,tüm bileşenler arasındaki bir iletişim sağlayıcısı gibi davranır.


### <a name="replication-process"></a>Çoğaltma işlemi

1. Her sitede bileşen sunucularını (yapılandırma, işlem, ana hedef) ayarlayıp çoğaltmak istediğiniz makinelere Birleşik Aracı'yı yükleyin.
2. İlk çoğaltmanın ardından makinelerdeki aracılar çoğaltma değişimleri işlem sunucusuna gönderir.
3. İşlem sunucusu verileri iyileştirir ve ikincil sitedeki ana hedef sunucusuna aktarır. Yapılandırma sunucusu çoğaltma sürecini yönetir.

**Şekil 6: VMware’den VMware’e çoğaltma**

![VMware'den VMware'e](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="hyper-v-vm-replication-to-a-secondary-site"></a>İkincil bir siteye Hyper-V VM çoğaltması


**Alan** | **Bileşen** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Microsoft Azure hesabınızın olması gerekir. |
**VMM sunucusu** | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir | Her VMM sunucusu İnternet’e bağlı olmalıdır.<br/><br/> Her sunucunun Hyper-V yetenek profili kümesi içeren en az bir VMM özel bulutu olmalıdır.<br/><br/> Azure Site Recovery Sağlayıcısını VMM sunucusuna yüklemeniz gerekir. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Sağlayıcı ile Azure arasındaki iletişimler güvenli ve şifrelidir.
**Hyper-V sunucusu** |  Birincil ve ikincil VMM bulutlarında bir veya daha fazla Hyper-V konak sunucusu.<br/><br/> Sunucular İnternet’e bağlı olmalıdır.<br/><br/> Verilerin, Kerberos veya sertifika kimlik doğrulaması kullanılarak, LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılması gerekir.  
**Hyper-V VM’leri** | Kaynak Hyper-V ana bilgisayar sunucusunda bulunur. | Kaynak ana bilgisayar sunucusunda çoğaltmak istediğiniz en az bir VM olması gerekir.

### <a name="replication-process"></a>Çoğaltma işlemi

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

### <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

1. Şirket içi siteler arasında planlanmış veya planlanmamış bir [yük devretme](site-recovery-failover.md) gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok makinenin devredilmesini düzenleyebilirsiniz.
4. İkincil bir siteye yönelik planlanmamış bir yük devretme gerçekleştirirseniz, işlem tamamlandıktan sonra ikincil konumdaki makineler koruma veya çoğaltma için etkinleştirilmez. Planlı bir yük devretme gerçekleştirdiyseniz, işlemden sonra ikincil konumdaki makineler korunur.
5. Daha sonra, kopya VM’deki iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
6. Birincil sitenizi yeniden kullanılabilir duruma geldiğinde ters çoğaltma başlatarak ikincil siteden birincil siteye çoğaltma gerçekleştirirsiniz. Ters çoğaltma sanal makineleri korumalı bir duruma getirir, ancak ikincil veri merkezi hala etkin konumdur.
7. Birincil siteyi yeniden etkin konum durumuna getirmek için ikincil siteden birincil siteye planlı yük devretme başlatır ve arkasından başka bir ters çoğaltma gerçekleştirirsiniz.


## <a name="hyper-v-replication-workflow"></a>Hyper-V çoğaltma iş akışı

**İş akışı aşaması** | **Eylem**
--- | ---
1. **Korumayı etkinleştir** | Bir Hyper-V VM için korumayı etkinleştirdiğinizde, makinenin önkoşullarla uyumlu olup olmadığının denetlenmesi için **Korumayı Etkinleştir** işi başlatılır. İş tarafından iki yöntem çağırılır:<br/><br/> Yapılandırdığınız ayarlarla çoğaltmanın ayarlanması için [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx).<br/><br/> Tam VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx).
2. **İlk çoğaltma** |  Bir sanal makine anlık görüntüsü alınır ve sanal sabit diskler tamamı ikincil konuma kopyalanana kadar tek tek çoğaltılır.<br/><br/> Bu işlemi tamamlamak için gereken süre VM boyutuna, ağ bant genişliğine ve iş çoğaltma yöntemine bağlıdır.<br/><br/> İlk çoğaltma sırasında disk değişimi meydana gelirse Hyper-V Çoğaltma'nın Çoğaltma İzleyicisi bu değişiklikleri, disklerle aynı klasörde yer alan Hyper-V Çoğaltma Günlükleri (.hrl) olarak izler.<br/><br/> Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır.<br/><br/> İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır. İlk çoğaltma tamamlandığında VM anlık görüntüsü silinir, günlükteki değişim diski değişiklikleri eşitlenir ve birleştirilir.
3. **Korumayı sonlandırma** | İlk çoğaltma sonlandırıldıktan sonra **Korumayı sonlandır** işi, ağ ayarlarını ve diğer çoğaltma sonrası ayarları yapılandırır ve sanal makine korunur.<br/><br/> Azure'da çoğaltma yapıyorsanız sanal makinede ince ayar yapmanız gerekebilir. Böylece sanal makine yük devretme için hazır hale gelir.<br/><br/> Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.
4. **Çoğaltma** | İlk çoğaltma sonrasında, çoğaltma ayarlarına uygun olarak değişim eşitlemesi başlar.<br/><br/> **Çoğaltma hatası**: Değişim çoğaltması başarısız olursa ve tam çoğaltma bant genişliği ve zaman konusunda fazla kaynak tüketimine yol açarsa yeniden eşitleme meydana gelir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir. Yeniden eşitleme, kaynak ve hedef sanal makinelerin sağlama toplamlarını hesaplar ve yalnızca değişim verilerini gönderir. Böylece gönderilen veri miktarını azaltır. Yeniden eşitleme bittikten sonra değişim çoğaltması devam eder. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz.<br/><br/> **Çoğaltma hatası**: Bir çoğaltma hatası meydana gelmesi durumunda yerleşik olarak yeniden deneme işlevi bulunur. Kimlik doğrulama veya yetkilendirme ya da çoğaltılan makinenin geçersiz durumda olması gibi kurtarılamaz bir hata olursa yeniden deneme işlevi uygulanmaz. Ağ hatası veya düşük disk alanı/belleği gibi kurtarılabilir bir hata oluşursa artan aralıklarla yeniden denemeler meydana gelir (her 1, 2, 4, 8, 10 ve ardından 30 dakikada bir şeklinde).
5. **Planlanan/planlanmamış yük devretme** | Gerektiğinde planlanan veya planlanmamış yük devretme işlemleri çalıştırabilirsiniz.<br/><br/> Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.<br/><br/> Oluşturulan çoğaltma VM’leri, yürütme bekleniyor durumuna geçirilir. Yük devretmeyi tamamlamak için işlemleri yürütmeniz gerekir.<br/><br/> Birincil site çalışır duruma geldikten sonra, site kullanılabilir hale geldiğinde buraya yeniden çalışma gerçekleştirebilirsiniz.


**Şekil 8: Hyper-V iş akışı**

![iş akışı](./media/site-recovery-components/arch-hyperv-azure-workflow.png)




## <a name="next-steps"></a>Sonraki adımlar

[Önkoşulları denetleme](site-recovery-prereq.md)

