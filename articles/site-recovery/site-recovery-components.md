<properties
    pageTitle="Site Recovery nasıl çalışır? | Microsoft Azure"
    description="Bu makalede Site Recovery mimarisine genel bir bakış sağlanır."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/13/2016"
    ms.author="raynew"/>


# <a name="how-does-azure-site-recovery-work?"></a>Azure Site Recovery nasıl çalışır?

Azure Site Recovery hizmetinin mimarisini ve bu hizmetin çalışmasını sağlayan bileşenleri anlamak için bu makaleyi okuyun.

Tüm yorumlarınızı ve sorularınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.


## <a name="overview"></a>Genel Bakış

Uygulamaları, iş yüklerini ve verileri planlanmış ve planlanmamış kesinti süreleri içinde çalışır ve kullanılabilir durumda tutan ve mümkün olan en kısa zamanda normal çalışma koşullarına dönmelerini sağlayan bir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ihtiyaç duyarsınız. 

Site Recovery, BCDR stratejinize katkıda bulunan bir Azure hizmetidir. Bu hizmet, şirket içi fiziksel sunucuların ve sanal makinelerin buluta (Azure) veya ikincil bir veri merkezine çoğaltılmasını düzenler. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışma koşullarına dönüldüğünde de yükü birincil konuma geri alabilirsiniz. [Site Recovery nedir?](site-recovery-overview.md) bölümünde bir genel bakış bulabilirsiniz

## <a name="site-recovery-in-the-azure-portal"></a>Azure portalında Site Recovery

Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki [dağıtım modeli](../resource-manager-deployment-model.md) kullanır: Azure Resource Manager modeli ve klasik hizmet yönetimi modeli. Ayrıca Azure iki portala sahiptir: Klasik dağıtım modelini destekleyen [klasik Azure portalı](https://manage.windowsazure.com/) ve her iki dağıtım modeline de destek sağlayan [Azure portalı](https://portal.azure.com).

- Site Recovery hem klasik portalda hem de Azure portalında kullanılabilir.
- Klasik Azure portalında, klasik hizmet yönetim modeliyle Site Recovery’yi destekleyebilirsiniz.
- Azure portalında klasik modeli veya Kaynak Modeli dağıtımlarını destekleyebilirsiniz. 

Bu makaledeki bilgiler hem klasik hem de Azure portalı dağıtımları için geçerlidir. Tabloda tüm farklılıklar özetlenmiştir.

**Çoğaltma** | **Azure Portal** | **Klasik portal**
--- | --- | ---
**Azure’a VMware VM çoğaltma** | Basitleştirilmiş dağıtım işlemi.<br/><br/> VM’lerin yükünü klasik veya Resource Manager tabanlı depolamaya devretme.<br/><br/> Klasik veya Resource Manager tabanlı depolamaya çoğaltma.<br/><br/> Yük devretme sonrasında Azure VM’lerini bağlamak için klasik ağı veya Resource Manager ağlarını kullanın.<br/><br/> LRS veya GRS depolama kullanın. | Yalnızca klasik depolamaya yük devretme.<br/><br/> Yük devretme sonrasında VM’leri bağlamak için yalnızca klasik ağları kullanın.<br/><br/> GRS depolama kullanın.
**Azure’a Hyper-V VM çoğaltması (VMM olmadan)** | Basitleştirilmiş dağıtım işlemi.<br/><br/> VM’lerin yükünü klasik veya Resource Manager tabanlı depolamaya devretme.<br/><br/> Klasik veya Resource Manager tabanlı depolamaya çoğaltma.<br/><br/> Yük devretme sonrasında Azure VM’lerini bağlamak için klasik ağı veya Resource Manager ağlarını kullanın.
**Azure’a Hyper-V VM çoğaltması (VMM ile)** | Basitleştirilmiş dağıtım işlemi.<br/><br/> VM’lerin yükünü klasik veya Resource Manager tabanlı depolamaya devretme.<br/><br/> Klasik veya Resource Manager tabanlı depolamaya çoğaltma.<br/><br/> Yük devretme sonrasında Azure VM’lerini bağlamak için klasik ağı veya Resource Manager ağlarını kullanın.<br/><br/> Ağ eşlemesi ayarlanmalıdır | Yalnızca klasik depolamaya yük devretme.<br/><br/> Yük devretme sonrasında VM’leri bağlamak için yalnızca klasik ağları kullanın.
**İkincil siteye Hyper-V VM çoğaltması (VMM ile)** | Basitleştirilmiş dağıtım işlemi.<br/><br/> VM’lerin yükünü klasik veya Resource Manager tabanlı depolamaya devretme.<br/><br/> Klasik veya Resource Manager tabanlı depolamaya çoğaltma.<br/><br/> Yük devretme sonrasında Azure VM’lerini bağlamak için klasik ağı veya Resource Manager ağlarını kullanın.<br/><br/> Ağ eşlemesi ayarlanmalıdır | Yalnızca klasik depolamaya yük devretme.<br/><br/> Yük devretme sonrasında VM’leri bağlamak için yalnızca klasik ağları kullanın.<br/><br/> Depolama eşlemesini ayarlayabilirsiniz. <br/><br/> SAN çoğaltması desteklenmez.



## <a name="deployment-scenarios"></a>Dağıtım senaryoları

Site Recovery, birçok senaryoda çoğaltmayı düzenlemek için dağıtılabilir:

- **VMware VM’lerini çoğaltma**: Şirket içi VMware sanal makinelerini Azure depolama alanına veya ikincil veri merkezine çoğaltabilirsiniz.
- **Fiziksel makineleri çoğaltma**: Windows veya Linux çalıştıran fiziksel makineleri Azure depolama alanına veya ikincil veri merkezine çoğaltabilirsiniz. Fiziksel makineleri çoğaltma işlemi VMware VM’lerini çoğaltma işlemiyle neredeyse aynıdır.
- **Hyper-V VM’lerini çoğaltma**: Hyper-V konaklarınız System Center VMM bulutlarında yönetiliyorsa, VM’leri Azure’a veya ikincil bir veri merkezine çoğaltabilirsiniz. Konaklar VMM tarafından yönetilmiyorsa yalnızca Azure’a çoğaltabilirsiniz. VMM tarafından yönetilmeyen Hyper-V VM'lerini Azure depolama alanına çoğaltabilirsiniz.
- **VM'leri geçirme**: [Azure IaaS VM'leri](site-recovery-migrate-azure-to-azure.md) bölgeler arasında geçirmek veya [AWS Windows örneklerini](site-recovery-migrate-aws-to-azure.md) Azure IaaS VM'lerine geçirmek için Site Recovery'yi kullanabilirsiniz. Tam çoğaltma şu anda desteklenmemektedir. Geçiş (yük devretme) için çoğaltma yapabilirsiniz, ancak önceki duruma geri dönemezsiniz. 

Site Recovery, bu VM'lerde ve fiziksel sunucularda çalışan çoğu uygulamayı çoğaltabilir. [Azure Site Recovery hangi iş yüklerini koruyabilir?](site-recovery-workload.md) bölümünden desteklenen uygulamaların tam özetine ulaşabilirsiniz.


## <a name="replicate-vmware-virtual-machines-to-azure"></a>VMware sanal makinelerini Azure’a çoğaltma

![Gelişmiş](./media/site-recovery-components/arch-enhanced.png)

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | **Hesap**: Bir Azure hesabınız olmalıdır.<br/><br/> **Depolama**: Çoğaltılan verileri depolamak için bir Azure depolama hesabınızın olması gerekir. Bir klasik hesap ya da Resource Manager depolama hesabı kullanabilirsiniz. Azure portalında dağıtım yaptığınızda hesap LRS veya GRS olabilir. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.<br/><br/> **Ağ**: Yük devretme sırasında oluşturulduğunda Azure VM’lerinin bağlandığı bir Azure sanal ağınızın olması gerekir. Azure portalında ağlar klasik hizmet yöneticisi modelinde ya da Resource Manager modelinde oluşturulabilir.
**Şirket içi yapılandırma sunucusu** |  Yapılandırma sunucusunu ve diğer Site Recovery bileşenlerini çalıştıran bir şirket içi Windows Server 2012 R2 makinenizin olması gerekir.<br/><br/> Bu makine yüksek oranda kullanılabilir bir VMware VM olmalıdır.<br/><br/> Sunucu üzerindeki bileşenler şunlardır:<br/><br/> **Yapılandırma sunucusu**: Şirket içi ortamınız ile Azure arasındaki iletişimi düzenler ve veri çoğaltma ile kurtarma işlemlerini yönetir.<br/><br/> **İşlem sunucusu**: Çoğaltma ağ geçidi olarak davranır. Korumalı kaynak makinelerden çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. Ayrıca Mobility hizmetinin korunan makinelere göndermeli yüklemesini ele alır ve VMware VM'lerinin otomatik olarak bulunmasını gerçekleştirir. Dağıtımınız büyüdükçe artan çoğaltma trafiği hacimlerini idare etmek için ilave olarak ayrı ayrı adanmış işlem sunucuları ekleyebilirsiniz.<br/><br/> **Ana hedef sunucusu**: Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.
**Şirket içi sanallaştırma sunucuları** | Bir veya daha fazla vSphere konağı. Ayrıca konakları yönetmek için bir vCenter sunucusunun kullanılması önerilir.
**Çoğaltılacak VM’ler** | Azure'a çoğaltmak istediğiniz her VMware VM’sinde Mobility hizmeti bileşeninin yüklü olması gerekir. Bu hizmet verileri yakalar, makinede yazar ve işlem sunucusuna gönderir. Bu bileşen elle yüklenebilir veya bir makine için çoğaltma işlemini etkinleştirdiğinizde işlem sunucusu tarafından otomatik olarak gönderilip yüklenebilir.

- Azure portalı dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmware-to-azure.md#azure-prerequisites).
- Azure portalında yeniden çalışma hakkında [daha fazla bilgi edinin](site-recovery-failback-azure-to-vmware.md).


## <a name="replicate-physical-servers-to-azure"></a>Fiziksel sunucuları Azure'a çoğaltma

![Gelişmiş](./media/site-recovery-components/arch-enhanced.png)

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | **Hesap**: Bir Azure hesabınız olmalıdır.<br/><br/> **Depolama**: Çoğaltılan verileri depolamak için bir Azure depolama hesabınızın olması gerekir. Bir klasik hesap ya da Resource Manager depolama hesabı kullanabilirsiniz. Azure portalında dağıtım yaptığınızda hesap LRS veya GRS olabilir. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.<br/><br/> **Ağ**: Yük devretme sırasında oluşturulduğunda Azure VM’lerinin bağlanacağı bir Azure sanal ağınızın olması gerekir. Azure portalında ağlar klasik hizmet yöneticisi modelinde ya da Resource Manager modelinde oluşturulabilir.
**Şirket içi yapılandırma sunucusu** |  Yapılandırma sunucusunu ve diğer Site Recovery bileşenlerini çalıştıran bir şirket içi Windows Server 2012 R2 makinenizin olması gerekir.<br/><br/> Sunucu sanal veya fiziksel olabilir.<br/><br/> Sunucu üzerindeki bileşenler şunlardır:<br/><br/> **Yapılandırma sunucusu**: Şirket içi ortamınız ile Azure arasındaki iletişimi düzenler ve veri çoğaltma ile kurtarma işlemlerini yönetir.<br/><br/> **İşlem sunucusu**: Çoğaltma ağ geçidi olarak davranır. Korumalı kaynak makinelerden çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. Ayrıca Mobility hizmetinin korunan makinelere göndermeli yüklemesini ele alır ve VMware VM'lerinin otomatik olarak bulunmasını gerçekleştirir. Dağıtımınız büyüdükçe artan çoğaltma trafiği hacimlerini idare etmek için ilave olarak ayrı ayrı adanmış işlem sunucuları ekleyebilirsiniz.<br/><br/> **Ana hedef sunucusu**: Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.
**Çoğaltılacak makineler** | Azure’a çoğaltmak istediğiniz her fiziksel Windows/Linux üzerinde Mobility hizmet bileşeninin yüklü olması gerekir. Bu hizmet verileri yakalar, makinede yazar ve işlem sunucusuna gönderir. Bu bileşen elle yüklenebilir veya bir makine için çoğaltma işlemini etkinleştirdiğinizde işlem sunucusu tarafından otomatik olarak gönderilip yüklenebilir.
**İlk duruma döndürme** | Fizikselden fiziksele geri dönme desteklenmez. Fiziksel sunucuların yükünü Azure’a devredip, sonra ilk duruma döndürmek isterseniz bir VMware VM’sine geri döndürmeniz gerekir. Bir fiziksel sunucuda yeniden çalışamazsınız. Geri döndürmek için bir Azure VM’si gereklidir ve yapılandırma sunucusunu bir VMware VM’si olarak dağıtmadıysanız VMware VM’si olarak ayrı bir ana hedef sunucu ayarlamanız gerekir. Ana hedef sunucu diskleri bir VMware sanal makinesine geri yüklemek için VMware depolama alanı ile etkileşim kurar ve bağlanır.


- Azure portalı dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmware-to-azure.md#azure-prerequisites).
- Azure portalında yeniden çalışma hakkında [daha fazla bilgi edinin](site-recovery-failback-azure-to-vmware.md).


## <a name="replicate-hyper-v-vms-not-managed-by-vmm-to-azure"></a>VMM tarafından yönetilmeyen Hyper-V VM’lerini Azure’a çoğaltma

![Azure'da Hyper-V sitesi](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | **Hesap**: Bir Azure hesabınız olmalıdır.<br/><br/> **Depolama**: Çoğaltılan verileri depolamak için bir Azure depolama hesabınızın olması gerekir. Bir klasik hesap ya da Resource Manager depolama hesabı kullanabilirsiniz. Hesap GRS olmalıdır. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.<br/><br/> **Ağ**: Yük devretme sırasında oluşturulduğunda Azure VM’lerinin bağlanacağı bir Azure sanal ağınızın olması gerekir.
**Hyper-V konakları/VM’ler** | Bir ya da daha fazla VM çalıştıran bir veya daha fazla Hyper-V konağı.<br/><br/> Site Recovery Sağlayıcısı ve Kurtarma Hizmetleri aracısı, dağıtım sırasında her bir konağa yüklenir.

- Azure portalı dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites).
- Klasik portal dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites).



## <a name="replicate-hyper-v-vms-managed-by-vmm-to-azure"></a>VMM tarafından yönetilen Hyper-V VM’lerini Azure’a çoğaltma


![VMM'den Azure'a](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | **Hesap**: Bir Azure hesabınız olmalıdır.<br/><br/> **Depolama**: Çoğaltılan verileri depolamak için bir Azure depolama hesabınızın olması gerekir. Bir klasik hesap ya da Resource Manager depolama hesabı kullanabilirsiniz. Hesap GRS olmalıdır. Çoğaltılan veriler Azure depolama alanında depolanır ve yük devretme durumunda Azure VM'leri çalışmaya başlar.<br/><br/> **Ağ**: Yük devretme sırasında oluşturulduğunda Azure VM’lerinin bağlanacağı bir Azure sanal ağınızın olması gerekir.
**VMM sunucusu** | Bir veya daha fazla özel bulutla birlikte bir ya da daha fazla şirket içi VMM sunucusu gereklidir. Site Recovery Sağlayıcısı, dağıtım sırasında her bir sunucuya yüklenir.
**Hyper-V konakları/VM’ler** | Bir ya da daha fazla VM çalıştıran bir veya daha fazla Hyper-V konağı.<br/><br/> Kurtarma Hizmetleri aracısı, dağıtım sırasında her bir konağa yüklenir.


- Azure portalı dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-azure.md#azure-requirements).
- Klasik portal dağıtımının gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-azure-classic.md#before-you-start).




## <a name="replicate-vmware-virtual-machines-or-physical-server-to-a-secondary-site"></a>VMware sanal makineleri veya fiziksel sunucuları ikincil bir siteye çoğaltma

![VMware'den VMware'e](./media/site-recovery-components/vmware-to-vmware.png)


**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | **Hesap**: Bu senaryoyu, InMage Scout kullanarak dağıtırsınız. Bunu elde etmek için bir Azure hesabına sahip olmanız gerekir.<br/><br/> Bir Site Recovery kasası oluşturduktan sonra InMage Scout hizmetini indirip dağıtımı ayarlamak üzere en son güncelleştirmeleri yüklersiniz.
**Birincil site** | **İşlem sunucusu**: Önbelleğe alma, sıkıştırma ve veri iyileştirme işlemlerini yürütmek için birincil sitenizde işlem sunucusu bileşenini ayarlayın. Ayrıca bu sunucu, Birleşik Aracı'nın korumak istediğiniz makinelere göndermeli yükleme işlemini yürütür.<br/><br/> **VMware ESX/ESXi ve vCenter**: Bir VMware EXS/ESXi hiper yöneticisine ve isteğe bağlı olarak bir VMware vCenter sunucusuna sahip olmanız gerekir.<br/><br/> **VM’ler** | Dağıtım sırasında, korumak istediğiniz makinelere Birleşik Aracı yüklenir. Aracı,tüm bileşenler arasındaki bir iletişim sağlayıcısı gibi davranır.
**İkincil site** | **Yapılandırma sunucusu**: Yapılandırma sunucusu yüklediğiniz ilk bileşendir ve yönetim Web sitesini veya vContinuum konsolunu kullanarak dağıtımınızı yönetme, yapılandırma ve izleme işlemleri için ikincil siteye yüklenir. Bir dağıtım işleminde yalnızca bir yapılandırma sunucusu vardır ve bu sunucunun Windows Server 2012 R2 çalıştıran bir makineye yüklenmesi gerekir.<br/><br/> **vContinuum sunucusu (ikincil site)**: Yapılandırma sunucusuyla aynı konuma yüklenir. Korunan ortamınızı yönetmeye ve izlemeye yönelik bir konsol sağlar. Varsayılan yüklemede, vContinuum ilk ana hedef sunucusudur ve Birleşik Aracı bu sunucuda yüklüdür.<br/><br/> **Ana hedef sunucusu**: Ana hedef sunucusu çoğaltılan verileri tutar. İşlem sunucusundan verileri alır, ikincil sitede çoğaltılan bir makine oluşturur ve veri bekletme noktalarını tutar. İhtiyacınız olan ana hedef sunucusu sayısı koruduğunuz makine sayısına bağlıdır. Birincil sitede yeniden çalıştırmak isterseniz burada da bir ana hedef sunucusuna sahip olmanız gerekir.


VMware VM’lerini veya fiziksel sunucuları ikincil bir siteye çoğaltmak için Azure Site Recovery aboneliğine dahil olan InMage Scout hizmetini indirirsiniz. Bu hizmet Azure portalından veya klasik Azure portalından indirilebilir.

Her sitede bileşen sunucularını (yapılandırma, işlem, ana hedef) ayarlayıp çoğaltmak istediğiniz makinelere Birleşik Aracı'yı yükleyin. İlk çoğaltmanın ardından makinelerdeki aracılar çoğaltma değişimleri işlem sunucusuna gönderir. İşlem sunucusu verileri iyileştirir ve ikincil sitedeki ana hedef sunucusuna aktarır. Yapılandırma sunucusu çoğaltma sürecini yönetir.


## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site"></a>VMM tarafından yönetilen Hyper-V VM’lerini ikincil siteye çoğaltma

![Şirket içinden şirket içine](./media/site-recovery-components/arch-onprem-onprem.png)

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | **Hesap**: Bir Azure hesabınız olmalıdır.
**VMM sunucusu** | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir. Her sunucuda bir veya daha fazla özel bulut olmalıdır.<br/><br/> Dağıtım sırasında VMM sunucusuna Azure Site Recovery Sağlayıcısı'nı yükleyin.
**Hyper-V konakları/VM’ler** | Birincil ve ikincil sitedeki VMM bulutlarında çalışan bir veya daha fazla Hyper-V konağı<br/><br/> Her konakta çoğaltmak için bir veya daha fazla VM olması gerekir.<br/><br/>. Kurtarma Hizmetleri aracısı, dağıtım sırasında her bir konağa yüklenir.

- Azure portalındaki dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-vmm.md#azure-prerequisites).
- Klasik Azure portalındaki dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-vmm-classic.md#before-you-start).


## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site-using-san-replication"></a>VMM tarafından yönetilen Hyper-V VM’lerini SAN çoğaltması kullanarak ikincil siteye çoğaltma

![SAN çoğaltması](./media/site-recovery-components/arch-onprem-onprem-san.png)

VMM bulutlarında yönetilen Hyper-V VM’lerini SAN çoğaltması ile klasik portalı kullanarak çoğaltabilirsiniz. Bu senaryo Azure portalında şu anda desteklenmemektedir.

**Bileşen** | **Ayrıntılar**
--- | ---
**Azure** | **Hesap**: Bir Azure hesabınız olmalıdır.
**VMM sunucusu** | Bir tane birincil sitede, bir tane de ikincil sitede VMM sunucusu olması önerilir. Her sunucuda bir veya daha fazla özel bulut olmalıdır.<br/><br/> Dağıtım sırasında VMM sunucusuna Azure Site Recovery Sağlayıcısı'nı yükleyin.
**SAN** | Birincil VMM sunucusu tarafından yönetilen, desteklenen bir SAN dizisi.<br/><br/> SAN, ikincil sitede başka bir SAN dizisiyle bir ağ altyapısını paylaşmalıdır.
**Hyper-V konakları/VM’ler** | Birincil ve ikincil sitedeki VMM bulutlarında çalışan bir veya daha fazla Hyper-V konağı<br/><br/> Her konakta çoğaltmak için bir veya daha fazla VM olması gerekir.<br/><br/>. Kurtarma Hizmetleri aracısı, dağıtım sırasında her bir konağa yüklenir.

Bu senaryoda, Site Recovery dağıtımı sırasında VMM sunucularına Azure Site Recovery Sağlayıcısı'nı yüklersiniz. Sağlayıcı, internet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Veriler, zaman uyumlu SAN çoğaltması kullanılarak birincil ve ikincil depolama dizileri arasında çoğaltılır.

Dağıtım gereksinimleri hakkında [daha fazla bilgi edinin](site-recovery-vmm-san.md#before-you-start).

## <a name="hyper-v-protection-lifecycle"></a>Hyper-V koruması yaşam döngüsü

Bu iş akışı, Hyper-V sanal makineleri üzerinde gerçekleşen koruma, çoğaltma ve yük devretme işlemlerinin süreçlerini gösterir.

1. **Korumayı etkinleştirme**: Site Recovery kasasını oluşturup, VMM bulutu veya Hyper-V sitesi için çoğaltma ayarlarını yapılandırırsınız ve VM'ler için korumayı etkinleştirirsiniz. **Korumayı Etkinleştir** olarak adlandırılan bir iş başlatılır ve **İşler** sekmesinden izlenebilir. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler ve ardından daha önce yapılandırdığınız ayarlarla Azure'a çoğaltma işlemini ayarlayan [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) yöntemini çağırır. Ayrıca, **Korumayı Etkinleştir** işi tam bir VM çoğaltmasını başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini de çağırır.
2. **İlk çoğaltma**: Bir sanal makine anlık görüntüsü alınır ve tamamı Azure'a veya ikincil bir veri merkezine kopyalanana dek sanal sabit diskler çoğaltılır. Bu işlemi tamamlamak için gereken süre VM boyutuna, ağ bant genişliğine ve iş çoğaltma yöntemine bağlıdır. İlk çoğaltma devam ederken disk değişiklikleri meydana gelirse Hyper-V Çoğaltma'nın çoğaltma izleyicisi bu değişiklikleri, disklerle aynı klasörde yer alan Hyper-V çoğaltma günlükleri (.hrl) olarak izler. Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyalarının disk kaynaklarını kullandığını unutmayın. İlk çoğaltma tamamlandığında VM anlık görüntüsü silinir, günlükteki değişim diski değişiklikleri eşitlenir ve birleştirilir.
3. **Korumayı sonlandırma**: İlk çoğaltma sonlandırıldıktan sonra **Korumayı sonlandır** işi, ağ ve diğer çoğaltma sonrası ayarlarını yapılandırır ve sanal makine korunur. Azure'da çoğaltma yapıyorsanız sanal makinede ince ayar yapmanız gerekebilir. Böylece sanal makine yük devretme için hazır hale gelir. Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.
4. **Çoğaltma**: İlk çoğaltma sonrasında çoğaltma ayarlarına uygun şekilde değişim eşitlemesi başlar.
    - **Çoğaltma hatası**: Değişim çoğaltması başarısız olursa ve tam çoğaltma bant genişliği ve zaman konusunda fazla kaynak tüketimine yol açarsa yeniden eşitleme meydana gelir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM yeniden eşitleme için işaretlenir. Yeniden eşitleme, kaynak ve hedef sanal makinelerin işlem sağlama toplamları tarafından gönderilen verilerin miktarını azaltır ve yalnızca değişim verilerini gönderir. Yeniden eşitleme bittikten sonra değişim çoğaltması devam eder. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz.
    - **Çoğaltma hatası**: Bir çoğaltma hatası meydana gelmesi durumunda yerleşik olarak yeniden deneme işlevi bulunur. Kimlik doğrulama veya yetkilendirme ya da çoğaltılan makinenin geçersiz durumda olması gibi kurtarılamaz bir hata olursa yeniden deneme işlevi uygulanmaz. Ağ hatası veya düşük disk alanı/belleği gibi kurtarılabilir bir hata oluşursa artan aralıklarla yeniden denemeler meydana gelir (her 1, 2, 4, 8, 10 ve ardından 30 dakikada bir şeklinde).
4. **Planlanan/planlanmamış yük devretme işlemleri**: Gerektiğinde planlanan veya planlanmamış yük devretmeler çalıştırabilirsiniz. Planlanan bir yük devretme çalıştırırsanız kaynak VM'ler veri kaybı olmamasını sağlamak için kapatılır. Çoğaltılan VM'ler oluşturulduktan sonra yürütme bekleme durumuna yerleştirilir. Yürütmenin otomatik olduğu SAN ile çoğaltma işlemini gerçekleştirmiyorsanız yük devretmeyi tamamlamak için VM'leri yürütmeniz gerekir. Birincil site açılıp çalıştığında yeniden çalışma meydana gelir. Çoğaltma Azure'a yapılıyorsa ters çoğaltma otomatiktir. Aksi halde, ters çoğaltmayı elle tetiklersiniz.


![iş akışı](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Sonraki adımlar

[Dağıtım için hazırlanma](site-recovery-best-practices.md)



<!--HONumber=Oct16_HO3-->


