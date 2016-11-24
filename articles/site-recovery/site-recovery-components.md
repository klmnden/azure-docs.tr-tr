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
ms.date: 10/05/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 5614c39d914d5ae6fde2de9c0d9941e7b93fc10f
ms.openlocfilehash: ef9a7da86e7528d3052f89dbe1eaac6fbb90527c


---
# <a name="how-does-azure-site-recovery-work"></a>Azure Site Recovery nasıl çalışır?
Azure Site Recovery hizmetinin temel mimarisini ve bu hizmetin çalışmasını sağlayan bileşenleri anlamak için bu makaleyi okuyun.

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

## <a name="overview"></a>Genel Bakış
Kuruluşlar, planlı ve planlanmayan kesinti süreleri boyunca uygulamaların, iş yüklerinin ve verilerin çalışır durumda, kullanılabilir olmasına yönelik izlenecek yolu belirleyen ve mümkün olan en kısa sürede normal çalışma koşullarına dönmeyi sağlayan BCDR stratejisine gereksinim duyar. BCDR stratejinizin işletme verilerini güvende tutması, kurtarılabilir şekilde saklaması ve bir olağanüstü durum sırasında iş yüklerinin sürekli olarak kullanılabilir kalmasını sağlaması gerekir.

Site Recovery, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure) veya ikincil bir veri merkezine çoğaltılmasını düzenleyerek BCDR stratejinize katkı sağlayan bir Azure hizmetidir. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz. [Site Recovery nedir?](site-recovery-overview.md) bölümünden daha fazla bilgi edinebilirsiniz.

## <a name="site-recovery-in-the-azure-portal"></a>Azure portalında Site Recovery
Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki [dağıtım modeli](../resource-manager-deployment-model.md) kullanır: Azure Resource Manager modeli ve klasik hizmet yönetimi modeli. Ayrıca Azure iki portala sahiptir: Klasik dağıtım modelini destekleyen [klasik Azure portalı](https://manage.windowsazure.com/) ve her iki dağıtım modeline de destek sağlayan [Azure portalı](https://portal.azure.com).

Site Recovery hem klasik portalda hem de Azure portalında kullanılabilir. Klasik Azure portalında Site Recovery hizmetini klasik hizmet yönetimi modeliyle destekleyebilirsiniz. Azure portalında klasik modeli veya Kaynak Modeli dağıtımlarını destekleyebilirsiniz. Azure portalı ile dağıtma hakkında [daha fazla bilgi edinin](site-recovery-overview.md#site-recovery-in-the-azure-portal).

Bu makaledeki bilgiler hem klasik hem de Azure portalı dağıtımları için geçerlidir. Geçerli olduğu yerlerde farklılıklar belirtilmiştir.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları
Site Recovery, birçok senaryoda çoğaltmayı düzenlemek için dağıtılabilir:

* **VMware sanal makinelerini çoğaltma**: Şirket içi VMware sanal makinelerini Azure'a veya ikincil veri merkezine çoğaltabilirsiniz.
* * **Fiziksel makineleri çoğaltma**: Windows veya Linux çalıştıran fiziksel makineleri Azure'a veya ikincil veri merkezine çoğaltabilirsiniz. Fiziksel makineleri çoğaltma işlemi VMware sanal makinelerini çoğaltma işlemiyle neredeyse aynıdır
* **Hyper-V VM'lerini (VMM olmadan) çoğaltma**: VMM tarafından yönetilmeyen Hyper-V VM'lerini Azure'a çoğaltabilirsiniz.
* **System Center VMM bulutlarında yönetilen Hyper-V VM'lerini çoğaltma**: VMM bulutlarındaki Hyper-V konak sunucularında çalışan şirket içi Hyper-V sanal makinelerini Azure'a veya ikincil veri merkezine çoğaltabilirsiniz. Standart Hyper-V Çoğaltmayı veya SAN çoğaltmayı kullanarak çoğaltabilirsiniz.
* **VM'leri geçirme**: [Azure IaaS VM'leri](site-recovery-migrate-azure-to-azure.md) bölgeler arasında geçirmek veya [AWS Windows örneklerini](site-recovery-migrate-aws-to-azure.md) Azure IaaS VM'lerine geçirmek için Site Recovery'yi kullanabilirsiniz. Şu anda yalnızca bu VM'lere yük aktarımı işlevini gerçekleştiren ancak yeniden çalıştırılmayan geçirme işlemleri desteklenir.

Site Recovery, bu VM'lerde ve fiziksel sunucularda çalışan çoğu uygulamayı çoğaltabilir. [Azure Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) bölümünden desteklenen uygulamaların tam özetine ulaşabilirsiniz.

## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Azure’a çoğaltma: VMware sanal makineleri veya fiziksel Windows/Linux sunucuları
VMware sanal makinelerini Site Recovery ile çoğaltmanın birkaç yolu vardır.

* **Azure portalını kullanarak**-Azure portalında Site Recovery dağıttığınızda sanal makineleri klasik hizmet yöneticisi depolama alanına veya Resource Manager’a devredebilirsiniz. VMware sanal makinelerinin Azure portalında çoğaltılması, Azure’da klasik veya Resource Manager depolama alanına çoğaltabilme imkanı dahil birkaç avantaj getirir. [Daha fazla bilgi edinin](site-recovery-vmware-to-azure.md).
* **Klasik portalı kullanarak**-Site Recovery’i gelişmiş bir deneyim kullanarak klasik portalda dağıtabilirsiniz. Klasik portaldaki tüm yeni dağıtımlar için bu seçenek kullanılmalıdır. Bu dağıtımda yalnızca sanal makineleri Azure’daki klasik depolama alanına devredebilirsiniz ve Resource Manager depolama alanına devredemezsiniz. [Daha fazla bilgi edinin](site-recovery-vmware-to-azure-classic.md). Klasik portalda VMware çoğaltmasını ayarlamaya yönelik [eski bir deneyim](site-recovery-vmware-to-azure-classic-legacy.md) de mevcuttur. Bu seçenek yeni dağıtımlar için kullanılmamalıdır.  Eski deneyimi kullanarak daha önce dağıttıysanız gelişmiş dağıtıma [geçiş hakkında bilgi edinin](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

Site Recovery’nin Azure portalında veya klasik Azure portalında (gelişmiş) VMware sanal makinelerini/fiziksel sunucularını çoğaltmak için dağıtılmasına yönelik mimari gereksinimleri birkaç farklılıkla birlikte benzerdir:

* Azure portalında dağıtım yaparsanız Resource Manager tabanlı depolama alanına çoğaltabilir ve yük devretme sonrasında Azure sanal makinelerini bağlamak üzere Resource Manager ağlarını kullanabilirsiniz.
* Azure portalında dağıtım yaptığınızda hem LRS hem de GRS depolama desteklenir. Klasik portalda GRS zorunludur.
* Azure portalında dağıtım işlemi basitleştirilmiştir ve kullanımı daha kolaydır.

İşte gerekenler:

* **Azure hesabı**: Bir Microsoft Azure hesabınızın olması gerekir.
* **Azure depolama alanı**: Çoğaltılan verileri depolamak için bir Azure depolama alanı hesabınızın olması gerekir. Bir klasik hesap ya da Resource Manager depolama hesabı kullanabilirsiniz. Azure portalında dağıtım yaptığınızda hesap LRS veya GRS olabilir. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.
* **Azure ağı**: Yük devretme sırasında oluşturulduğunda Azure sanal makinelerinin bağlanacağı bir Azure sanal ağınızın olması gerekir. Azure portalında bunlar klasik hizmet yöneticisi modelinde ya da Resource Manager modelinde oluşturulmuş ağlar olabilir.
* **Şirket içi yapılandırma sunucusu**: Yapılandırma sunucusunu ve diğer Site Recovery bileşenlerini çalıştıran bir şirket içi Windows Server 2012 R2 makinenizin olması gerekir. VMware sanal makinelerini çoğaltıyorsanız yüksek oranda kullanılabilir bir VMware sanal makinesi olmalıdır. Fiziksel sunucuları çoğaltmak istiyorsanız makine fiziksel olabilir. Makineye aşağıdaki Site Recovery bileşenleri yüklenecektir:
  * **Yapılandırma sunucusu**: Şirket içi ortamınız ile Azure arasındaki iletişimi düzenler ve veri çoğaltma ile kurtarma işlemlerini yönetir.
  * **İşlem sunucusu**: Çoğaltma ağ geçidi olarak davranır. Korumalı kaynak makinelerden çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. Ayrıca Mobility hizmetinin korunan makinelere göndermeli yüklemesini ele alır ve VMware VM'lerinin otomatik olarak bulunmasını gerçekleştirir. Dağıtımınız büyüdükçe artan çoğaltma trafiği hacimlerini idare etmek için ilave olarak ayrı ayrı adanmış işlem sunucuları ekleyebilirsiniz.
  * **Ana hedef sunucusu**: Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.
* **Çoğaltılacak VMware sanal makineleri veya fiziksel sunucular**: Azure'a çoğaltmak istediğiniz her makinede Mobility hizmeti bileşeninin yüklü olması gerekir. Bu hizmet verileri yakalar, makinede yazar ve işlem sunucusuna gönderir. Bu bileşen elle yüklenebilir veya bir makine için çoğaltma işlemini etkinleştirdiğinizde işlem sunucusu tarafından otomatik olarak gönderilip yüklenebilir.
* **vSPhere ana bilgisayarları/vCenter sunucusu**: VMware sanal makinelerini çalıştıran bir veya daha fazla vSphere ana bilgisayar sunucunuz olmalıdır. Bu ana bilgisayarları yönetmek için bir vCenter sunucusu dağıtmanızı öneririz.
* **Yeniden çalışma**: Şunlar gereklidir:
  * **Fiziksel-fiziksel yeniden çalışma desteklenmez**: Fiziksel sunucuların yükünü Azure’a devredip sonra yeniden çalışmak isterseniz bir VMware sanal makinesinde yeniden çalışmanız gerekir. Bir fiziksel sunucuda yeniden çalışamazsınız. Yeniden çalışmak için bir Azure sanal makinesi gereklidir ve yapılandırma sunucusunu bir VMware sanal makinesi olarak dağıtmadıysanız VMware sanal makinesi olarak ayrı bir ana hedef sunucu ayarlamanız gerekir. Ana hedef sunucu diskleri bir VMware sanal makinesine geri yüklemek için VMware depolama alanı ile etkileşim kurar ve bağlanır.
  * * **Azure'da geçici işlem sunucusu**: Yük devretmeden sonra Azure'da yeniden çalıştırma işlemini gerçekleştirmek isterseniz Azure'dan çoğaltmayı düzenlemek için işlem sunucusu olarak yapılandırılan bir Azure VM ayarlamanız gerekir. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
  * **VPN bağlantısı**: Yeniden çalışma için, Azure ağından şirket içi siteye olacak şekilde ayarlanmış bir VPN bağlantısına (veya Azure ExpressRoute) sahip olmanız gerekir.
  * **Ayrı şirket içi ana hedef sunucusu**: Şirket içi ana hedef sunucusu yeniden çalışma işlemini yürütür. Ana hedef sunucusu varsayılan olarak yönetim sunucusunda yüklüdür. Ancak daha fazla hacimli trafikte yeniden çalıştırma işlemini gerçekleştiriyorsanız bu işlem için ayrı bir şirket içi ana hedef sunucusu ayarlamanız gerekir.

**Genel mimari**

![Gelişmiş](./media/site-recovery-components/arch-enhanced.png)

**Dağıtım bileşenleri**

![Gelişmiş](./media/site-recovery-components/arch-enhanced2.png)

**İlk duruma döndürme**

![Gelişmiş yeniden çalışma](./media/site-recovery-components/enhanced-failback.png)

* Azure portalı dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmware-to-azure.md#azure-prerequisites).
* Klasik portalda gelişmiş dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
* Azure portalında yeniden çalışma hakkında [daha fazla bilgi edinin](site-recovery-failback-azure-to-vmware.md).
* Klasik portalda yeniden çalışma hakkında [daha fazla bilgi edinin](site-recovery-failback-azure-to-vmware-classic.md).

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Azure’a çoğaltma: VMM tarafından yönetilmeyen Hyper-V sanal makineleri
System Center VMM tarafından yönetilmeyen Hyper-V sanal makinelerini Site Recovery ile Azure’a aşağıdaki gibi çoğaltabilirsiniz:

* **Azure portalını kullanarak**-Azure portalında Site Recovery dağıttığınızda sanal makineleri klasik depolama alanına veya Resource Manager’a devredebilirsiniz. [Daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure.md).
* **Klasik portalı kullanarak**-Site Recovery’i klasik portalda dağıtabilirsiniz. Bu dağıtımda yalnızca sanal makineleri Azure’daki klasik depolama alanına devredebilirsiniz ve Resource Manager depolama alanına devredemezsiniz. [Daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure-classic.md).

Her iki dağıtım mimarisi de aşağıdakiler dışında benzerdir:

* Azure portalında dağıtım yaparsanız Resource Manager depolama alanına çoğaltabilir ve yük devretme sonrasında Azure sanal makinelerini bağlamak üzere Resource Manager ağlarını kullanabilirsiniz.
* Azure portalında dağıtım işlemi basitleştirilmiştir ve kullanımı daha kolaydır.

İşte gerekenler:

* **Azure hesabı**: Bir Microsoft Azure hesabınızın olması gerekir.
* **Azure depolama alanı**: Çoğaltılan verileri depolamak için bir Azure depolama alanı hesabınızın olması gerekir. Azure portalında bir klasik hesap ya da Resource Manager depolama hesabı kullanabilirsiniz. Klasik portalda yalnızca klasik bir hesap kullanabilirsiniz. Çoğaltılan veriler Azure depolama alanında saklanır ve Azure VM'leri yük devretme gerçekleştiğinde oluşturulur.
* **Azure ağı**: Yük devretme sonrasında oluşturulduğunda Azure makinelerinin bağlanacağı bir Azure ağınızın olması gerekir.
* **Hyper-v ana bilgisayarı**: Bir veya daha fazla Windows Server 2012 R2 Hyper-V ana bilgisayar sunucusu gereklidir. Site Recovery dağıtımı sırasında, konak üzerinde Azure Site Recovery Sağlayıcısı ve Microsoft Azure Kurtarma Hizmetleri aracısını yüklemeniz gerekir.
* **Hyper-V sanal makineleri**: Hyper-V ana bilgisayar sunucusunda bir veya daha fazla sanal makine gereklidir. Site Recovery dağıtımı sırasında Hyper-V ana bilgisayarı üzerinde Azure Site Recovery Sağlayıcısı ve Azure Kurtarma Hizmetleri aracısı. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Aracı, veri çoğaltma işlemine ait verileri HTTPS 443 üzerinden işler. Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.

**Genel mimari**

![Azure'da Hyper-V sitesi](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

* Azure portalı dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites).
* Klasik portal dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites).

## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Azure’a çoğaltma: VMM tarafından yönetilen Hyper-V sanal makineleri
VMM bulutlarındaki Hyper-V sanal makinelerini Site Recovery ile Azure’a aşağıdaki gibi çoğaltabilirsiniz:

* **Azure portalını kullanarak**-Azure portalında Site Recovery dağıttığınızda sanal makineleri klasik depolama alanına veya Resource Manager’a devredebilirsiniz. [Daha fazla bilgi edinin](site-recovery-vmm-to-azure.md).
* **Klasik portalı kullanarak**-Site Recovery’i klasik portalda dağıtabilirsiniz. Bu dağıtımda yalnızca sanal makineleri Azure’daki klasik depolama alanına devredebilirsiniz ve Resource Manager depolama alanına devredemezsiniz. [Daha fazla bilgi edinin](site-recovery-vmm-to-azure-classic.md).

Her iki dağıtım mimarisi de aşağıdakiler dışında benzerdir:

* Azure portalında dağıtım yaparsanız Resource Manager tabanlı depolama alanına çoğaltabilir ve yük devretme sonrasında Azure sanal makinelerini bağlamak üzere Resource Manager ağlarını kullanabilirsiniz.
* Azure portalında dağıtım işlemi basitleştirilmiştir ve kullanımı daha kolaydır.

İşte gerekenler:

* **Azure hesabı**: Bir Microsoft Azure hesabınızın olması gerekir.
* **Azure depolama alanı**: Çoğaltılan verileri depolamak için bir Azure depolama alanı hesabınızın olması gerekir. Azure portalında bir klasik hesap ya da Resource Manager depolama hesabı kullanabilirsiniz. Klasik portalda yalnızca klasik bir hesap kullanabilirsiniz. Çoğaltılan veriler Azure depolama alanında saklanır ve Azure VM'leri yük devretme gerçekleştiğinde oluşturulur.
* **Azure ağı**: Yük devretmenin ardından oluşturulduklarında Azure sanal makinelerinin uygun ağlara bağlanması için ağ eşlemesi gereklidir.
* **VMM sunucusu**: System Center 2012 R2 üzerinde çalışan ve bir veya daha fazla özel bulut ile ayarlanmış bir veya daha fazla şirket içi VMM sunucusu gereklidir. Azure portalında dağıtıyorsanız ağ eşlemesini yapılandırabilmek için mantıksal ve sanal makine ağlarının ayarlanması gerekir. Klasik portalda bu seçenek isteğe bağlıdır.  VM ağının da bulutla ilişkilendirilen bir mantıksal ağ ile bağlantılı olması gerekir.
* **Hyper-v ana bilgisayarı**: VMM bulutunda bir veya daha fazla Windows Server 2012 R2 Hyper-V ana bilgisayar sunucusu gereklidir.
* **Hyper-V sanal makineleri**: Hyper-V ana bilgisayar sunucusunda bir veya daha fazla sanal makine gereklidir.

**Genel mimari**

![VMM'den Azure'a](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

* Azure portalı dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-azure.md#azure-prerequisites).
* Klasik portal dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-azure-classic.md).

## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>İkincil siteye çoğaltma: VMware sanal makineleri veya fiziksel sunucular
VMware sanal makinelerini veya fiziksel sunucuları ikincil bir siteye çoğaltmak için Azure Site Recovery aboneliğine dahil olan InMage Scout hizmetini indirirsiniz. Bu hizmet Azure portalından veya klasik Azure portalından indirilebilir.

Her sitede bileşen sunucularını (yapılandırma, işlem, ana hedef) ayarlayıp çoğaltmak istediğiniz makinelere Birleşik Aracı'yı yükleyin. İlk çoğaltmanın ardından makinelerdeki aracılar çoğaltma değişimleri işlem sunucusuna gönderir. İşlem sunucusu verileri iyileştirir ve ikincil sitedeki ana hedef sunucusuna aktarır. Yapılandırma sunucusu çoğaltma sürecini yönetir.

İşte gerekenler:

**Azure hesabı**: Bu senaryoyu, InMage Scout kullanarak dağıtırsınız. Bunu elde etmek için bir Azure aboneliğine sahip olmanız gerekir. Bir Site Recovery kasası oluşturduktan sonra InMage Scout hizmetini indirip dağıtımı ayarlamak üzere en son güncelleştirmeleri yüklersiniz.
**İşlem sunucusu (birincil site)**: Önbelleğe alma, sıkıştırma ve veri iyileştirme işlemlerini yürütmek için birincil sitenizde işlem sunucusu bileşenini ayarlayın. Ayrıca bu sunucu, Birleşik Aracı'nın korumak istediğiniz makinelere göndermeli yükleme işlemini yürütür.
**VMware ESX/ESXi ve vCenter sunucusu (birincil site)**: VMware VM'lerini koruyorsanız hiper yöneticileri yönetmek için bir VMware EXS/ESXi hiper yöneticisine ve isteğe bağlı olarak VMware vCenter sunucusuna sahip olmanız gerekir.

* **VM'ler/fiziksel sunucular (birincil site)**: Korumak istediğiniz VMware VM'leri veya Windows/Linux fiziksel sunucuları, Birleşik Aracı'nın yüklü olmasını gerektirir. Ayrıca, makinelere yüklenen Birleşik Aracı ana hedef sunucusu gibi davranır. Aracı,tüm bileşenler arasındaki bir iletişim sağlayıcısı gibi davranır.
* * **Yapılandırma sunucusu (ikincil site)**: Yapılandırma sunucusu yüklediğiniz ilk bileşendir ve yönetim Web sitesini veya vContinuum konsolunu kullanarak dağıtımınızı yönetme, yapılandırma ve izleme işlemleri için ikincil siteye yüklenir. Bir dağıtım işleminde yalnızca bir yapılandırma sunucusu vardır ve bu sunucunun Windows Server 2012 R2 çalıştıran bir makineye yüklenmesi gerekir.
* **vContinuum sunucusu (ikincil site)**: Yapılandırma sunucusuyla aynı konuma (ikincil site) yüklenir. Korunan ortamınızı yönetmeye ve izlemeye yönelik bir konsol sağlar. Varsayılan yüklemede, vContinuum ilk ana hedef sunucusudur ve Birleşik Aracı bu sunucuda yüklüdür.
* **Ana hedef sunucusu (ikincil site)**: Ana hedef sunucusu çoğaltılan verileri tutar. İşlem sunucusundan verileri alır, ikincil sitede çoğaltılan bir makine oluşturur ve veri bekletme noktalarını tutar. İhtiyacınız olan ana hedef sunucusu sayısı koruduğunuz makine sayısına bağlıdır. Birincil sitede yeniden çalıştırmak isterseniz burada da bir ana hedef sunucusuna sahip olmanız gerekir.

**Genel mimari**

![VMware'den VMware'e](./media/site-recovery-components/vmware-to-vmware.png)

## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>İkincil siteye çoğaltma: VMM tarafından yönetilen Hyper-V sanal makineleri
System Center VMM tarafından yönetilen Hyper-V sanal makinelerini Site Recovery ile ikincil veri merkezine aşağıdaki gibi çoğaltabilirsiniz:

* **Azure portalını kullanarak**-Site Recovery’i Azure portalında dağıttığınızda. [Daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure.md).
* **Klasik portalı kullanarak**-Site Recovery’i klasik portalda dağıtabilirsiniz. [Daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure-classic.md).

Her iki dağıtım mimarisi de aşağıdakiler dışında benzerdir:

* Azure portalında dağıtırsanız ağ eşlemeyi ayarlamanız gerekir. Klasik portalda bu seçenek isteğe bağlıdır.
* Azure portalında dağıtım işlemi basitleştirilmiştir ve kullanımı daha kolaydır.
* * Klasik Azure portalında dağıtırsanız [depolama eşlemesi](site-recovery-storage-mapping.md) kullanılabilir.

İşte gerekenler:

* **Azure hesabı**: Bir Microsoft Azure hesabınızın olması gerekir.
* **VMM sunucusu**: En az bir VMM özel bulutu içeren bir VMM sunucusunun hem birincil sitede hem de ikincil sitede bir tane olacak şekilde bulundurulmasını öneririz. Sunucunun, en son güncelleştirmeleri içeren System Center 2012 SP1 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir. Bulutların Hyper-V işlev profili kümesine sahip olması gerekir. VMM sunucusunda Azure Site Recovery Sağlayıcısı'nı yükleyin. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Sağlayıcı ile Azure arasındaki iletişimler güvenli ve şifrelidir.
* **Hyper-V sunucusu**: Hyper-V ana bilgisayar sunucularının birincil ve ikincil VMM bulutlarında yer alması gerekir. Ana bilgisayar sunucularının, en son güncelleştirmeleri içeren Windows Server 2012 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir. Veriler, Kerberos veya sertifika kimlik doğrulaması kullanan LAN ya da VPN üzerinden birincil ve ikincil Hyper-V ana bilgisayar sunucuları arasında çoğaltılır.  
* **Korunan makineler**: Kaynak Hyper-V ana bilgisayar sunucusunun korumak istediğiniz en az bir VM'ye sahip olması gerekir.

**Genel mimari**

![Şirket içinden şirket içine](./media/site-recovery-components/arch-onprem-onprem.png)

* Azure portalındaki dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-vmm.md#azure-prerequisites).
* * Klasik Azure portalındaki dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-vmm-classic.md#before-you-start).

## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>SAN çoğaltması ile ikincil siteye çoğaltma: VMM tarafından yönetilen Hyper-V sanal makineleri
VMM bulutlarında yönetilen Hyper-V sanal makinelerini SAN çoğaltması ile klasik Azure portalını kullanarak çoğaltabilirsiniz. Bu senaryo yeni Azure portalında şu anda desteklenmemektedir.

Bu senaryoda, Site Recovery dağıtımı sırasında VMM sunucularına Azure Site Recovery Sağlayıcısı'nı yüklersiniz. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Veriler, zaman uyumlu SAN çoğaltması kullanılarak birincil ve ikincil depolama dizileri arasında çoğaltılır.

İşte gerekenler:

**Azure hesabı**: Bir Azure aboneliğine sahip olmanız gerekir

* **SAN dizisi**: Birincil VMM sunucusu tarafından yönetilen [ desteklenen SAN dizisi](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx). SAN, ikincil sitede başka bir SAN dizisiyle bir ağ altyapısı paylaşır.
* **VMM sunucusu**: En az bir VMM özel bulutu içeren bir VMM sunucusunun hem birincil sitede hem de ikincil sitede bir tane olacak şekilde bulundurulmasını öneririz. Sunucunun, en son güncelleştirmeleri içeren System Center 2012 SP1 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir. Bulutların Hyper-V işlev profili kümesine sahip olması gerekir.
* **Hyper-V sunucusu**: Birincil ve ikincil VMM bulutlarında yer alan Hyper-V ana bilgisayar sunucuları. Ana bilgisayar sunucularının, en son güncelleştirmeleri içeren Windows Server 2012 sürümünde veya sonraki bir sürümde çalıştırılması ve internete bağlı olması gerekir.
* **Korunan makineler**: Kaynak Hyper-V ana bilgisayar sunucusunun korumak istediğiniz en az bir VM'ye sahip olması gerekir.

**SAN çoğaltması mimarisi**

![SAN çoğaltması](./media/site-recovery-components/arch-onprem-onprem-san.png)

Dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-san.md#before-you-start).

### <a name="on-premises"></a>Şirket içi
## <a name="hyper-v-protection-lifecycle"></a>Hyper-V koruması yaşam döngüsü
Bu iş akışı, Hyper-V sanal makineleri üzerinde gerçekleşen koruma, çoğaltma ve yük devretme işlemlerinin süreçlerini gösterir.

1. **Korumayı etkinleştirme**: Site Recovery kasasını oluşturup, VMM bulutu veya Hyper-V sitesi için çoğaltma ayarlarını yapılandırırsınız ve VM'ler için korumayı etkinleştirirsiniz. **Korumayı Etkinleştir** olarak adlandırılan bir iş başlatılır ve **İşler** sekmesinden izlenebilir. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler ve ardından daha önce yapılandırdığınız ayarlarla Azure'a çoğaltma işlemini ayarlayan [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) yöntemini çağırır. Ayrıca, **Korumayı Etkinleştir** işi tam bir VM çoğaltmasını başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini de çağırır.
2. **İlk çoğaltma**: Bir sanal makine anlık görüntüsü alınır ve tamamı Azure'a veya ikincil bir veri merkezine kopyalanana dek sanal sabit diskler çoğaltılır. Bu işlemi tamamlamak için gereken süre VM boyutuna, ağ bant genişliğine ve iş çoğaltma yöntemine bağlıdır. İlk çoğaltma sırasında disk değişimi meydana gelirse Hyper-V Çoğaltma'nın Çoğaltma İzleyicisi bu değişiklikleri, disklerle aynı klasörde yer alan Hyper-V Çoğaltma Günlükleri (.hrl) olarak izler. Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyalarının disk kaynaklarını kullandığını unutmayın. İlk çoğaltma tamamlandığında VM anlık görüntüsü silinir, günlükteki değişim diski değişiklikleri eşitlenir ve birleştirilir.
3. **Korumayı sonlandırma**: İlk çoğaltma sonlandırıldıktan sonra **Korumayı sonlandır** işi, ağ ve diğer çoğaltma sonrası ayarlarını yapılandırır ve sanal makine korunur. Azure'da çoğaltma yapıyorsanız sanal makinede ince ayar yapmanız gerekebilir. Böylece sanal makine yük devretme için hazır hale gelir. Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.
4. **Çoğaltma**: İlk çoğaltma sonrasında çoğaltma ayarlarına uygun şekilde değişim eşitlemesi başlar.
   * **Çoğaltma hatası**: Değişim çoğaltması başarısız olursa ve tam çoğaltma bant genişliği ve zaman konusunda fazla kaynak tüketimine yol açarsa yeniden eşitleme meydana gelir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM yeniden eşitleme için işaretlenir. Yeniden eşitleme, kaynak ve hedef sanal makinelerin işlem sağlama toplamları tarafından gönderilen verilerin miktarını azaltır ve yalnızca değişim verilerini gönderir. Yeniden eşitleme bittikten sonra değişim çoğaltması devam eder. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz.
   * **Çoğaltma hatası**: Bir çoğaltma hatası meydana gelmesi durumunda yerleşik olarak yeniden deneme işlevi bulunur. Kimlik doğrulama veya yetkilendirme ya da çoğaltılan makinenin geçersiz durumda olması gibi kurtarılamaz bir hata olursa yeniden deneme işlevi uygulanmaz. Ağ hatası veya düşük disk alanı/belleği gibi kurtarılabilir bir hata oluşursa artan aralıklarla yeniden denemeler meydana gelir (her 1, 2, 4, 8, 10 ve ardından 30 dakikada bir şeklinde).
5. **Planlanan/planlanmamış yük devretme işlemleri**: Gerektiğinde planlanan veya planlanmamış yük devretmeler çalıştırabilirsiniz. Planlanan bir yük devretme çalıştırırsanız kaynak VM'ler veri kaybı olmamasını sağlamak için kapatılır. Çoğaltılan VM'ler oluşturulduktan sonra yürütme bekleme durumuna yerleştirilir. Yürütmenin otomatik olduğu SAN ile çoğaltma işlemini gerçekleştirmiyorsanız yük devretmeyi tamamlamak için VM'leri yürütmeniz gerekir. Birincil site açılıp çalıştığında yeniden çalışma meydana gelir. Çoğaltma Azure'a yapılıyorsa ters çoğaltma otomatiktir. Aksi halde, ters çoğaltmayı elle tetiklersiniz.

![iş akışı](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Sonraki adımlar
[Dağıtım için hazırlanma](site-recovery-best-practices.md)



<!--HONumber=Nov16_HO2-->


